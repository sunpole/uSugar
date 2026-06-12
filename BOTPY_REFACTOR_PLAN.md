# BOTPY_REFACTOR_PLAN - uSugar

Дата анализа: 2026-06-12
Состояние проекта: `1.2.8`

Этот план описывает безопасное разделение `bot.py` без изменения поведения, без переноса функций прямо сейчас и без коммита. Цель - подготовить маршрут, который можно выполнять маленькими шагами и не сломать рабочий runtime.

## 1. Текущая структура `bot.py`

Текущий `bot.py` после девятого безопасного шага `1.2.8`:

- длина до шага: `1,276` строк;
- длина после `1.2.0`: `1,122` строки;
- длина после `1.2.1`: `1,140` физических строк;
- длина после `1.2.2`: `1,107` физических строк;
- длина после `1.2.3`: `912` физических строк;
- длина после `1.2.4`: `817` физических строк;
- длина после `1.2.5`: `714` физических строк;
- длина после `1.2.6`: `433` физические строки;
- длина после `1.2.7`: `256` физических строк;
- длина после `1.2.8`: `143` физических строки;
- вынесено за шаг `1.2.8`: `112` физических строк из `bot.py`;
- функций и классов в `bot.py`: `3`;
- системных функций/регистраторов в `handlers/system.py`: `10`;
- профильных функций/регистраторов в `handlers/profile.py`: `6`;
- информационных функций/регистраторов в `handlers/info.py`: `4`;
- glucose функций/регистраторов в `handlers/glucose.py`: `10`;
- settings функций/регистраторов в `handlers/settings.py`: `5`;
- logs функций/регистраторов в `handlers/logs.py`: `4`;
- therapy функций/регистраторов в `handlers/therapy.py`: `12`;
- OCR функций/регистраторов в `handlers/ocr.py`: `5`;
- reminder runtime функций/регистраторов в `runtime/reminders.py`: `4`;
- зарегистрированных обработчиков Telegram в `bot.py`: `1` маркер `@dp.message(...)`, `@dp.callback_query(...)` или `dp.*.register(...)`;
- системные, профильные, информационные, glucose-команды/text flow, settings/WebApp handlers, journal handlers, therapy handlers, OCR flow и reminder runtime теперь регистрируются явно через `register_system_handlers(dp)`, `register_info_handlers(dp)`, `register_profile_handlers(dp, UserState)`, `register_glucose_handlers(dp, UserState)`, `register_settings_handlers(dp, UserState)`, `register_logs_handlers(dp, UserState)`, `register_therapy_handlers(dp, UserState)`, `register_ocr_handlers(dp, UserState)` и `register_reminder_handlers(dp)`.

### Основные зоны внутри файла

| Зона | Примерные строки | Что лежит внутри |
| --- | --- | --- |
| Импорты и runtime globals | 1-40 | `aiogram`, `config`, `db`, `usugar_*`, `Dispatcher`, `MemoryStorage`, `logging` |
| Вспомогательные helper-функции | handlers modules | timestamp formatting, follow-up text, URL helpers, glucose feedback helpers |
| FSM | 50-68 | `UserState` definitions only; calculation wrappers moved out of `bot.py` |
| Глобальные перехваты и обработка "просто текста" | `handlers/glucose.py` | HI/LOW/HELP shortcuts, number parsing, multi-number flow, number confirmation |
| Системные команды | `handlers/system.py` | `/version`, `/health`, `/backup`, `/story`, `/start`, `/help`, unknown slash commands |
| Profile / info commands | `handlers/profile.py`, `handlers/info.py` | `/setname`, `/whoami`, `/formula`, `/ocr`, `/ocrlog` |
| Sugar / status / settings / webapp / undo | `handlers/glucose.py`, `handlers/settings.py`, `handlers/therapy.py` | `/sugar`, `/status`, glucose text flow, `/settings`, `web_app_data`, `/undo` |
| Insulin / food / logs | `handlers/therapy.py`, `handlers/logs.py` | `/insulin`, `/food`, `/log`, journal FSM, therapy FSM |
| OCR intake and confirmation | `handlers/ocr.py` | `photo_intake`, OCR callback, save/manual confirmation |
| Reminders and reminder loop | `runtime/reminders.py` | `/reminders`, `send_due_reminders`, `reminder_loop` |
| Unknown commands and random replies | 80-101 | unknown command handler registration and random friendly replies |
| Startup and polling loop | 103-143 | `update_version_in_html`, `main`, `asyncio.run(main())` |

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
- `handlers/settings.py` - `/settings`, `web_app_data`, `build_settings_url`, `build_protocol_summary`.
- `handlers/logs.py` - `/log`, journal FSM, and CSV export used by long journal responses.
- `handlers/therapy.py` - `/food`, `/insulin`, `/undo`, food/insulin FSM, undo confirmation, and short-insulin follow-up scheduling.
- `handlers/ocr.py` - photo intake, OCR callback handlers, OCR confirmation, and manual OCR value flow.
- `runtime/reminders.py` - `/reminders`, due reminder delivery, and background reminder loop.
- `common/text.py` - shared `with_version()` and `build_story_url()`.
- `common/fsm.py` - shared `clear_command_state()`.

## 2. Карта зависимостей

### Верхний уровень зависимостей

`bot.py` напрямую зависит от:

- `config` - bot token, version, random-reply synonym lists;
- `db` - startup database initialization;
- handler/runtime modules - explicit registration and `reminder_loop`;
- `aiogram` - `Bot`, `Dispatcher`, `MemoryStorage`, FSM state class, polling.

### Наиболее связные цепочки

| Зона | Главные зависимости | Комментарий |
| --- | --- | --- |
| `/help`, `/version`, `/health`, `/story` | `handlers/system.py`, `config`, `db`, `usugar_logic`, `usugar_export` | вынесено, почти статичный surface |
| `/setname`, `/whoami`, `/sugar`, `/status` | `handlers/profile.py`, `handlers/glucose.py`, `db`, `usugar_logic`, `usugar_brain` | вынесено, пользовательские записи и feedback |
| `/settings`, `web_app_data` | `handlers/settings.py`, `db`, `usugar_protocol`, `keyboards` | вынесено, формирование и сохранение протокола |
| `/insulin`, `/food`, `/undo`, `/log` | `handlers/therapy.py`, `handlers/logs.py`, `db`, `usugar_logic`, `usugar_reminders`, `csv`, `io`, `FSInputFile` | вынесено, много текста и несколько FSM-состояний |
| OCR photo intake | `handlers/ocr.py`, `usugar_vision`, `usugar_ocr`, `db`, `keyboards`, `bot.download_file` | вынесено, самый тяжелый пользовательский handler-flow |
| Reminders loop | `runtime/reminders.py`, `db`, `usugar_reminders`, `usugar_protocol`, `TelegramAPIError` | вынесено, фоновые уведомления и дедупликация |
| Startup | `db.init_db()`, `update_version_in_html()`, `dp.start_polling()` | composition root и side effects |

### Дублирующиеся или уже готовые к выносу швы

В `bot.py` уже есть логические прокладки, которые удобно вынести первыми:

- `with_version()` - общий text wrapper;
- `build_story_url()` и `build_settings_url()` - URL helpers;
- `build_protocol_summary()` - чистая текстовая сборка;
- `build_saved_sugar_text()` - одно место для feedback after save;
- `schedule_insulin_followup_text()` - стык `db` + `usugar_reminders`;
- `update_version_in_html()` - отдельный startup side effect;
- `send_due_reminders()` - вынесен в `runtime/reminders.py`;
- `photo_intake()` - вынесен в `handlers/ocr.py`;
- `process_ocr_callback()`, `process_ocr_confirmation()`, `process_ocr_manual_value()` - вынесены как единый OCR flow.

## 3. Предлагаемая структура модулей

Минимально безопасное разбиение без изменения поведения:

```text
bot.py                  # composition root, dispatcher wiring, main()
handlers/
  system.py             # /version /health /backup /story /start /help /unknown
  profile.py            # /setname /whoami
  glucose.py            # /sugar /status /helpful synonyms / saved sugar feedback
  therapy.py            # /insulin /food /undo flows related to meal & insulin
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

Статус: выполнено в `1.2.5`.

Вынесено:

- `/settings`
- `handle_webapp_data`
- `build_settings_url`
- `build_protocol_summary`
- `/log`
- journal FSM
- CSV export path

Почему:

- эти ветки читают данные, форматируют ответ и мало влияют на другие потоки;
- хорошо отделяются от OCR/reminders.

Риск:

- prefill и `web_app_data` можно сломать незаметно;
- `test_command_surface.py`, `test_usugar_protocol.py`, `test_db.py`.

### Шаг 5. `/insulin`, `/food`, `/undo`

Статус: выполнено в `1.2.6`.

Вынесено:

- `/undo`
- `/insulin`
- `/food`
- `schedule_insulin_followup_text`
- food/insulin FSM handlers
- undo confirmation

Почему:

- это уже отдельный продуктовый контур: еда, инсулин, корректировка ошибок;
- после вынесения модули будут читаться лучше.

Риск:

- логика подтверждений и отмен;
- связь с `db.delete_*`;
- связь с follow-up reminders;
- тесты `test_db.py`, `test_usugar_logic.py`, `test_usugar_reminders.py`.

### Шаг 6. OCR поток

Статус: выполнено в `1.2.7`.

Вынесено:

- `photo_intake`
- `process_ocr_callback`
- `process_ocr_confirmation`
- `process_ocr_manual_value`
- OCR-only registration wrappers

Почему это был последний большой пользовательский handler-flow:

- много I/O;
- много состояний FSM;
- зависит от `bot.download_file`, `db.save_ocr_attempt`, `usugar_vision`, `usugar_ocr`.

Оставлено в `bot.py` без изменений:

- reminders status/loop;
- database schema;
- startup/main/polling.

Риск:

- самый высокий из handler-слоёв;
- можно случайно изменить UX при сохранении/отмене;
- могут пострадать `test_usugar_ocr.py`, `test_usugar_vision.py`, `test_command_surface.py`.

### Шаг 7. Reminders runtime

Статус: выполнено в `1.2.8`.

Вынесено:

- `reminders_status`
- `send_due_reminders`
- `reminder_loop`
- `register_reminder_handlers`

Почему это был последний крупный runtime-блок:

- это фоновые уведомления, которые живут вокруг всего проекта;
- здесь важны дедупликация и стабильность доставки.

Оставлено в `bot.py` без изменений:

- Bot/Dispatcher/MemoryStorage;
- handler registration order;
- `main()`, polling, retry loop, and reminder task startup;
- database schema and startup side effects.

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
| 4 | средний | WebApp prefill, protocol summary, journal FSM, CSV export path |
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

## 8. Что делать после версии 1.2.8

После `1.2.8` проект уже близок к такому состоянию:

- `bot.py` стал composition root и в основном собирает приложение;
- reminder status/loop вынесен отдельно и продолжает использовать тот же `db` и `usugar_reminders`;
- все 66 тестов остаются зелёными;
- текстовые поверхности не меняются неожиданно;
- никакого нового функционала в процессе распила не добавляется.

Следующие шаги должны быть осторожнее обычного: если выносить startup, делать это только отдельным маленьким патчем и не менять polling/retry semantics.

## 9. Что отложить после 1.2.8

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

`bot.py` уже стал composition root: основные пользовательские command/FSM/OCR flows живут в handler modules, а reminder command/background runtime живёт в `runtime/reminders.py`. Самый безопасный следующий шаг - стабилизировать это состояние и не трогать database schema, OCR engines or startup side effects до полного подтверждения стабильности.
