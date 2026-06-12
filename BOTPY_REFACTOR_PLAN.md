# BOTPY_REFACTOR_PLAN - uSugar

Дата анализа: 2026-06-12
Состояние проекта: `1.2.3`

Этот план описывает безопасное разделение `bot.py` без изменения поведения, без переноса функций прямо сейчас и без коммита. Цель - подготовить маршрут, который можно выполнять маленькими шагами и не сломать рабочий runtime.

## 1. Текущая структура `bot.py`

Текущий `bot.py` после четвёртого безопасного шага `1.2.3`:

- длина до шага: `1,276` строк;
- длина после `1.2.0`: `1,122` строки;
- длина после `1.2.1`: `1,140` физических строк;
- длина после `1.2.2`: `1,107` физических строк;
- длина после `1.2.3`: `912` физических строк;
- вынесено за шаг `1.2.3`: `195` физических строк из `bot.py`;
- функций и классов в `bot.py`: `33`;
- системных функций/регистраторов в `handlers/system.py`: `10`;
- профильных функций/регистраторов в `handlers/profile.py`: `6`;
- информационных функций/регистраторов в `handlers/info.py`: `4`;
- glucose функций/регистраторов в `handlers/glucose.py`: `10`;
- зарегистрированных обработчиков Telegram в `bot.py`: `24` маркера `@dp.message(...)`, `@dp.callback_query(...)` или `dp.*.register(...)`;
- системные, профильные, информационные и glucose-команды/text flow теперь регистрируются явно через `register_system_handlers(dp)`, `register_info_handlers(dp)`, `register_profile_handlers(dp, UserState)` и `register_glucose_handlers(dp, UserState)`.

### Основные зоны внутри файла

| Зона | Примерные строки | Что лежит внутри |
| --- | --- | --- |
| Импорты и runtime globals | 1-40 | `aiogram`, `config`, `db`, `usugar_*`, `Dispatcher`, `MemoryStorage`, `logging` |
| Вспомогательные helper-функции | 42-110 | URL builders, protocol summary, timestamp formatting, follow-up text |
| FSM и общие расчётные wrapper-helpers | 161-196 | `UserState`, `get_isf`, `get_ic_ratio`, `get_meal_bonus`, `round_to_step` |
| Глобальные перехваты и обработка "просто текста" | `handlers/glucose.py` | HI/LOW/HELP shortcuts, number parsing, multi-number flow, number confirmation |
| Системные команды | `handlers/system.py` | `/version`, `/health`, `/backup`, `/story`, `/start`, `/help`, unknown slash commands |
| Profile / info commands | `handlers/profile.py`, `handlers/info.py` | `/setname`, `/whoami`, `/formula`, `/ocr`, `/ocrlog`; OCR photo intake/callbacks не тронуты |
| Sugar / status / settings / webapp / undo | 180-510 | `/sugar`, `/status`, `process_sugar`, HI/LOW/help, number parsing and multi-number flow уже в `handlers/glucose.py`; `/settings`, `web_app_data`, `/undo` остаются в `bot.py` |
| Insulin / food / logs | 766-1003 | `/insulin`, `/food`, `/log`, журналные FSM-переходы |
| OCR intake and confirmation | 1002-1177 | `photo_intake`, OCR callback, save/manual confirmation |
| Reminders and reminder loop | 1175-1283 | `/reminders`, `send_due_reminders`, `reminder_loop` |
| Unknown commands and random replies | 1283-1301 | unknown command handler, random friendly replies |
| Startup and polling loop | 1317-1355 | `update_version_in_html`, `main`, `asyncio.run(main())` |

### Что уже вынесено из `bot.py`

`bot.py` уже опирается на отдельные модули, значит разделение лучше делать по уже существующим швам, а не переписывать архитектуру с нуля:

- `db.py` - SQLite-доступ и CRUD;
- `usugar_logic.py` - расчёты и health/formula helpers;
- `usugar_protocol.py` - сбор протокола из WebApp data;
- `usugar_reminders.py` - reminder state machine and texts;
- `usugar_brain.py` - sugar feedback and simple reaction logic;
- `usugar_ocr.py` - OCR engines and aggregation;
- `usugar_vision.py` - photo intake, OCR captions, runtime image paths;
- `usugar_export.py` - ZIP backup;
- `keyboards.py` - reply/inline keyboards.
- `handlers/system.py` - system command handlers and unknown slash-command handler registration.
- `handlers/profile.py` - `/setname`, `/whoami`, `process_name`, `filter_name`.
- `handlers/info.py` - `/formula`, `/ocr`, `/ocrlog`.
- `handlers/glucose.py` - `/sugar`, `/status`, manual sugar input, HI/LOW/help shortcuts, multi-number flow, number-context choice, and saved sugar feedback text.
- `common/text.py` - shared `with_version()` and `build_story_url()`.
- `common/fsm.py` - shared `clear_command_state()`.

## 2. Карта зависимостей

### Верхний уровень зависимостей

`bot.py` напрямую зависит от:

- `config` - версии, feature flags, default protocol, reminder constants;
- `db` - все записи, чтение, удаление, reminder events;
- `usugar_logic` - формулы, extraction, health report, dose calculation;
- `usugar_protocol` - WebApp parsing and reminder settings;
- `usugar_vision` - OCR intake UX, local runtime paths, OCR captions;
- `usugar_ocr` - recognition engines, aggregation summary;
- `usugar_brain` - glucose feedback and user-facing response text;
- `usugar_reminders` - reminder state, keys, texts, due calculations;
- `usugar_export` - backup ZIP;
- `keyboards` - all reply and inline keyboards.

### Наиболее связные цепочки

| Зона | Главные зависимости | Комментарий |
| --- | --- | --- |
| `/help`, `/version`, `/health`, `/story` | `config`, `db`, `usugar_logic`, `usugar_export` | почти статичный surface, минимальный риск |
| `/setname`, `/whoami`, `/sugar`, `/status` | `db`, `usugar_logic`, `usugar_brain` | пользовательские записи и feedback |
| `/settings`, `web_app_data` | `db`, `usugar_protocol`, `keyboards` | формирование и сохранение протокола |
| `/insulin`, `/food`, `/log` | `db`, `usugar_logic`, `usugar_reminders`, `csv`, `io`, `FSInputFile` | много текста и несколько FSM-состояний |
| OCR photo intake | `usugar_vision`, `usugar_ocr`, `db`, `keyboards`, `bot.download_file` | самый тяжелый и самый хрупкий поток |
| Reminders loop | `db`, `usugar_reminders`, `usugar_protocol`, `TelegramAPIError` | фоновые уведомления и дедупликация |
| Startup | `db.init_db()`, `update_version_in_html()`, `dp.start_polling()` | composition root и side effects |

### Дублирующиеся или уже готовые к выносу швы

В `bot.py` уже есть логические прокладки, которые удобно вынести первыми:

- `with_version()` - общий text wrapper;
- `build_story_url()` и `build_settings_url()` - URL helpers;
- `build_protocol_summary()` - чистая текстовая сборка;
- `build_saved_sugar_text()` - одно место для feedback after save;
- `schedule_insulin_followup_text()` - стык `db` + `usugar_reminders`;
- `update_version_in_html()` - отдельный startup side effect;
- `send_due_reminders()` - уже почти готовый runtime service;
- `photo_intake()` - кандидат на `handlers/ocr.py` почти целиком;
- `process_ocr_callback()`, `process_ocr_confirmation()`, `process_ocr_manual_value()` - единый OCR flow.

## 3. Предлагаемая структура модулей

Минимально безопасное разбиение без изменения поведения:

```text
bot.py                  # composition root, dispatcher wiring, main()
handlers/
  system.py             # /version /health /backup /story /start /help /unknown
  profile.py            # /setname /whoami
  glucose.py            # /sugar /status /helpful synonyms / saved sugar feedback
  food_insulin.py       # /insulin /food /undo flows related to meal & insulin
  logs.py               # /log and CSV export
  ocr.py                # photo intake, callback, confirmation, manual OCR flow
  settings.py           # /settings and web_app_data parsing
runtime/
  reminders.py          # send_due_reminders(), reminder_loop()
  startup.py            # update_version_in_html(), async main() side effects
common/
  text.py               # shared with_version(), protocol summary, helpers
  fsm.py                # shared clear state helpers and common FSM utilities
```

Важно: `common/` можно не создавать сразу. Если проект хочет двигаться совсем осторожно, эти helper-функции можно оставить в `bot.py` до второго или третьего шага. Главное - сначала вынести routing, а уже потом общие куски.

## 4. Порядок рефакторинга по шагам

### Шаг 1. Системные команды и статичный текст

Статус: выполнено в `1.2.0`.

Вынесено:

- `/version`
- `/health`
- `/backup`
- `/story`
- `/start`
- `/help`
- `unknown_command`
- `with_version`
- `build_story_url`

Почему это безопасно:

- мало FSM;
- почти нет сложных переходов;
- легко проверить `test_command_surface.py` и `test_version_info.py`.

Риск:

- можно случайно поломать текст команд или порядок кнопок;
- можно забыть `with_version()` на одной из веток.

### Шаг 2. Профиль и базовые user-data flow

Статус: выполнено в `1.2.1`.

Вынесено:

- `/setname`
- `/whoami`
- `filter_name`
- `process_name`

Также в `1.2.1` вынесены информационные команды:

- `/formula`
- `/ocr`
- `/ocrlog`

Важно: OCR photo intake, OCR callbacks and manual OCR flow остались в `bot.py` и не менялись.

Почему:

- это маленький и изолированный кусок;
- зависит в основном от `db` и `usugar_logic`.

Риск:

- срыв состояния FSM;
- изменение текста, которое увидят пользователи и тесты.

### Шаг 3. Глюкоза и простые реакции

Статус: выполнено в `1.2.3`.

В `1.2.2` вынесено:

- `/sugar`
- `/status`

В `1.2.3` дополнительно вынесено:

- `global_hilo_help`
- `number_or_text_handler`
- `process_multi_numbers`
- `process_number_choice`
- `process_sugar`
- `build_saved_sugar_text`

Оставлено в `bot.py` без изменений:

- OCR photo intake/callbacks/manual OCR flow;
- settings/WebApp;
- food/insulin/undo;
- logs;
- reminders;
- database schema;
- startup/main/polling.

Почему:

- это центральный пользовательский сценарий, но он уже опирается на готовые pure helpers;
- db-save остался внутри glucose handler module, без изменения пользовательского поведения.

Риск:

- поломка определения чисел и multi-number flow;
- сдвиг текста high/low/help;
- тесты `test_usugar_logic.py` и `test_command_surface.py`.

### Шаг 4. `/settings`, `/log`

Выносить:

- `/settings`
- `handle_webapp_data`
- `/log`
- `build_protocol_summary`

Почему:

- эти ветки читают данные, форматируют ответ и мало влияют на другие потоки;
- хорошо отделяются от OCR/reminders.

Риск:

- prefill и `web_app_data` можно сломать незаметно;
- `test_command_surface.py`, `test_usugar_protocol.py`, `test_db.py`.

### Шаг 5. `/insulin`, `/food`, `/undo`

Выносить:

- `/undo`
- `/insulin`
- `/food`
- `schedule_insulin_followup_text`

Почему:

- это уже отдельный продуктовый контур: еда, инсулин, корректировка ошибок;
- после вынесения модули будут читаться лучше.

Риск:

- логика подтверждений и отмен;
- связь с `db.delete_*`;
- связь с follow-up reminders;
- тесты `test_db.py`, `test_usugar_logic.py`, `test_usugar_reminders.py`.

### Шаг 6. OCR поток

Выносить:

- `photo_intake`
- `process_ocr_callback`
- `process_ocr_confirmation`
- `process_ocr_manual_value`

Почему последним из пользовательских потоков:

- много I/O;
- много состояний FSM;
- зависит от `bot.download_file`, `db.save_ocr_attempt`, `usugar_vision`, `usugar_ocr`.

Риск:

- самый высокий из handler-слоёв;
- можно случайно изменить UX при сохранении/отмене;
- могут пострадать `test_usugar_ocr.py`, `test_usugar_vision.py`, `test_command_surface.py`.

### Шаг 7. Reminders runtime

Выносить:

- `reminders_status`
- `send_due_reminders`
- `reminder_loop`

Почему последним:

- это фоновые уведомления, которые живут вокруг всего проекта;
- здесь важны дедупликация и стабильность доставки.

Риск:

- ложные/дублирующиеся уведомления;
- поломка trusted-contact logic;
- поломка follow-up reminders;
- тесты `test_usugar_reminders.py`, `test_db.py`, `test_usugar_brain.py`.

### Шаг 8. Startup / composition root

В самом конце:

- `update_version_in_html()`
- `main()`
- `asyncio.run(main())`

Оставить `bot.py` как composition root:

- собрать `Bot`, `Dispatcher`, регистрацию handlers и запуск polling;
- не держать в нём бизнес-логику.

Риск:

- очень легко сломать запуск и фоновые задачи;
- именно здесь лучше не экспериментировать до финальной стабилизации.

## 5. Риски каждого шага

| Шаг | Риск | На что смотреть |
| --- | --- | --- |
| 1 | низкий | тексты, команды, help surface |
| 2 | низкий-средний | FSM state transitions, name filtering |
| 3 | средний | sugar parsing, multi-number handling, feedback wording |
| 4 | средний | WebApp prefill, status formatting, CSV export path |
| 5 | средний-высокий | undo confirmation, insulin follow-up scheduling |
| 6 | высокий | OCR state machine, media download, confirmation/cancel flow |
| 7 | высокий | duplicate reminders, trust alerts, long-insulin timing |
| 8 | очень высокий | polling startup, version sync side effect, runtime crash recovery |

## 6. Какие тесты могут пострадать

| Тест | Почему чувствителен |
| --- | --- |
| `tests/test_command_surface.py` | проверяет команды, тексты кнопок, `settings.html` и presence of `story.html` assets |
| `tests/test_version_info.py` | связан с версией, `settings.html` и историческими комментариями |
| `tests/test_story_generation.py` | зависит от формата `SCREENSHOT_STORY.md` и генератора story data |
| `tests/test_db.py` | затронут любым изменением undo, reminder events, follow-up reminders |
| `tests/test_usugar_logic.py` | пострадает, если поменяются helper wrappers или расчётные тексты |
| `tests/test_usugar_vision.py` | чувствителен к OCR text helpers, photo intake helpers и result aggregation |
| `tests/test_usugar_ocr.py` | чувствителен к OCR engine pipeline и disabled/default behavior |
| `tests/test_usugar_protocol.py` | легко ломается при изменении WebApp parsing |
| `tests/test_usugar_reminders.py` | самый чувствительный к reminder timing и text builders |
| `tests/test_usugar_brain.py` | зависит от glucose feedback wording и basal checks |

## 7. Оценка сложности

### Низкая

- system commands;
- version/story/help wrappers;
- profile handlers.

### Средняя

- glucose flows;
- settings/webapp;
- logs and CSV export.

### Высокая

- undo;
- insulin/food;
- OCR flow;
- reminders runtime.

### Очень высокая

- startup/polling composition changes;
- любые попытки одновременно менять `bot.py`, OCR и reminder logic.

## 8. Что делать после версии 1.2.3

После `1.2.3` стоит двигаться к такому состоянию маленькими проверяемыми шагами:

- `bot.py` постепенно становится composition root и в основном собирает приложение;
- settings/logs переезжают в отдельные handler modules;
- OCR flow выносится отдельно, но поведение не меняется;
- reminder loop выносится отдельно и продолжает использовать тот же `db` и `usugar_reminders`;
- все 66 тестов остаются зелёными;
- текстовые поверхности не меняются неожиданно;
- никакого нового функционала в процессе распила не добавляется.

## 9. Что отложить после 1.2.3

Лучше не лезть до стабильного разделения:

- переписывание OCR engines;
- введение новой DB migration system;
- переделка reminder policy;
- смена `sqlite` на другую БД;
- production deployment;
- webhook migration;
- LLM/DeepSeek integration;
- глубокий редизайн `settings.html`;
- расширение `bot.py` новыми фичами до завершения раскроя.

## 10. Что лучше не трогать, пока проект развивается

Эти части сейчас лучше оставить стабильными:

- `db.py` schema and CRUD;
- `usugar_ocr.py` recognition pipeline;
- `usugar_reminders.py` reminder policy;
- `usugar_protocol.py` WebApp parsing;
- `usugar_brain.py` response text rules;
- `scripts/generate_story_data.py` and `story.html` until the history pipeline is stable;
- `update_version_in_html()` behavior, кроме случаев, когда реально меняется version sync flow.

## 11. Краткий вывод

`bot.py` уже не "всё в одном" по смыслу, но всё ещё "всё в одном" по поверхности. Самый безопасный путь - разрезать его на handler modules вокруг уже существующих сервисов, оставить `bot.py` как композиционный корень и не трогать OCR/reminder/db internals до тех пор, пока command surface и tests полностью не подтвердят стабильность.
