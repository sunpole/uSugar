# CODE_AUDIT - uSugar

Дата аудита кода: 2026-06-11
Версия проекта по `VERSION.json`: `1.0.39`

Этот аудит создан после чтения `docs/AI_CONTEXT.md`, `PROJECT_STATUS.md` и `PROJECT_AUDIT.md`, а затем проверки локального кода. Код проекта не изменялся.

Проверка тестов:

```text
venv\Scripts\python.exe -m unittest discover -s tests -v
Ran 64 tests in 1.635s
OK
```

## 1. Какие функции реально работают по коду

### Telegram runtime

Реально реализовано в `bot.py`:

- запуск через aiogram polling;
- retry polling после `TelegramNetworkError`;
- фоновый reminder loop при `USUGAR_REMINDERS_ENABLED=true`;
- команда `/version`;
- команда `/health` без вывода токена, полного DB path и ngrok URL;
- команда `/backup`;
- команда `/story`;
- команда `/start`;
- команда `/help`;
- команда `/formula`;
- команда `/ocr`;
- команда `/ocrlog`;
- команда `/setname`;
- команда `/whoami`;
- команда `/sugar`;
- команда `/status`;
- команда `/settings`;
- команда `/insulin`;
- команда `/food`;
- команда `/log`;
- команда `/reminders`;
- обработка неизвестных команд;
- обработка обычных чисел;
- обработка нескольких чисел как возможных углеводов;
- простые random replies в private chat.

### Пользовательские данные

Реально реализовано в `db.py`:

- SQLite инициализация;
- таблица `users`;
- таблица `glucose_log`;
- таблица `insulin_log`;
- таблица `ocr_attempts`;
- таблица `reminder_events`;
- таблица `followup_reminders`;
- сохранение имени;
- сохранение протокола;
- сохранение сахара;
- сохранение инсулина;
- чтение последних значений;
- чтение истории сахара и инсулина;
- чтение всех записей сахара/инсулина для backup;
- сохранение и обновление OCR attempts;
- защита от повторной отправки reminder events;
- lifecycle follow-up reminders.

### Формулы и протокол

Реально реализовано:

- `usugar_logic.py`: ISF, ICR, meal bonus, rounding, name filter, number extraction, health report, formula reference, food dose calculation;
- `usugar_protocol.py`: сбор протокола из Telegram WebApp data, включая reminder settings;
- `config.py`: default protocol и чтение `.env` для основных runtime-флагов.

### WebApp

Реально реализовано:

- `settings.html` как статическая Telegram WebApp форма;
- отправка JSON через `Telegram.WebApp.sendData`;
- compact/mobile layout;
- поля протокола, ISF/ICR, meal bonus, basal, reminders, trusted contact, follow-up;
- `bot.py` принимает `web_app_data`, строит protocol и сохраняет его.

Ограничение: WebApp не загружает текущий протокол пользователя обратно в форму.

### OCR и фото

Реально реализовано:

- обработчик Telegram photo;
- в private chat бот отвечает на любое фото;
- в group/supergroup бот отвечает на фото только при релевантной подписи;
- скачивание изображения в `.runtime/ocr_images`;
- Libre2 CV распознавание крупного значения;
- Tesseract variants, если `tesseract.exe` найден;
- EasyOCR subprocess, но только если включен флагом;
- aggregation OCR results;
- inline confirmation buttons;
- manual OCR value flow;
- сохранение OCR attempts в SQLite;
- `/ocrlog` без показа Telegram `file_id`;
- извлечение trend arrow и базового graph trend из Libre2 screenshot.

Важное наблюдение: `USUGAR_OCR_ENABLED` используется в тексте `/ocr`, но текущий `photo_intake` все равно запускает `usugar_ocr.recognize_glucose(...)`. То есть флаг сейчас не является жестким выключателем OCR-обработки.

### Экспорт

Реально реализовано:

- `/backup` создает in-memory ZIP через `usugar_export.py`;
- ZIP содержит `manifest.json`, `protocol.json`, `glucose_log.csv`, `insulin_log.csv`, `ocr_attempts.csv`;
- секреты и Telegram `file_id` не включаются в backup;
- `/log` дополнительно может отправлять CSV-файл по выбранному типу журнала и периоду.

### Reminders и bot brain

Реально реализовано:

- `usugar_brain.py`: реакции на in-range, low, high glucose;
- расчет примерной high correction по протоколу;
- basal check около времени длинного инсулина;
- `usugar_reminders.py`: measurement reminder state;
- first/second measurement reminders;
- trusted alert state;
- basal before/overdue reminders;
- follow-up after short insulin;
- delivery keys против повторной отправки;
- background delivery из `bot.py`.

### История проекта

Реально реализовано:

- `story.html`;
- `docs/history/story_data.js`;
- `scripts/generate_story_data.py`;
- `scripts/sync_version.py` обновляет story data;
- `/story` выводит ссылку на `story.html`.

## 2. Заявлено в документации, но отсутствует или не завершено в коде

- Полноценные три надежные OCR-движка отсутствуют. Есть Libre2 CV, optional Tesseract и disabled-by-default EasyOCR; PaddleOCR нет.
- `USUGAR_OCR_ENABLED=false` не останавливает фактическую OCR-обработку фото в `photo_intake`.
- DeepSeek/LLM отсутствует как runtime-функция. Есть только `.env.example` и документация.
- `DEEPSEEK_API_KEY`, `DATABASE_URL`, `DEBUG` есть в `.env.example`, но код их не использует.
- `usugar_knowledge.py` отсутствует.
- Импорт/восстановление из `/backup` отсутствует.
- Импорт CSV/JSON в базу отсутствует.
- Редактирование и удаление записей отсутствует.
- Полноценный поиск по журналам отсутствует.
- Расширенная фильтрация журналов отсутствует; есть только тип журнала и период `сегодня/неделя/месяц`.
- WebApp prefill текущего протокола отсутствует.
- Админ-панель, роли и управление пользователями отсутствуют.
- Consent-flow для trusted contact отсутствует; доверенный Telegram ID вводится вручную.
- APScheduler/job store отсутствует; reminders работают через собственный async loop.
- PostgreSQL/production database отсутствует; `DATABASE_URL` не подключен.
- CI/CD отсутствует.
- Production deployment отсутствует.
- Надежное распознавание времени телефона/замера со скриншота отсутствует: в metadata явно стоит `reading_time=None`.
- Полноценный анализ Libre2 графика как временного ряда отсутствует; есть только базовая эвристика trend.
- Групповой сценарий частично есть для photo relevance, но нет полноценной системы group admin/roles.
- Команда `/setup` из старой GitHub-документации отсутствует; текущая настройка идет через `/settings`.

## 3. Есть в коде, но слабо описано в документации

- `/log` умеет отправлять CSV-файлы за выбранный период, а не только показывать текстовый журнал.
- `bot.py` при запуске вызывает `update_version_in_html()` и меняет версию в `settings.html`. Это runtime-side effect, который не очень явно выделен в документации.
- `photo_intake` запускает OCR независимо от `USUGAR_OCR_ENABLED`; документация говорит о флаге как о контроле OCR, но код сейчас работает иначе.
- В `db.py` есть мини-миграция только для колонок `ocr_attempts`; при этом полноценной миграционной системы нет.
- Есть retries для Telegram network outages в `main()`, это полезная runtime-устойчивость.
- Есть `get_users_for_reminders()`, который включает users и glucose_log, значит reminders могут касаться пользователей без полного профиля.
- Есть `followup_reminders` с `UNIQUE(user_id, source_key)`, защищающий от дублей по одному insulin event.
- Есть `should_reply_to_photo()` для групповых фото: в группах бот не отвечает на каждую картинку.
- В `usugar_ocr.py` есть распознавание Libre2 trend arrow и graph summary, хотя это пока эвристика.
- В `usugar_export.py` backup явно очищает OCR CSV от Telegram `file_id`.

## 4. Что первым выделять из `bot.py`

`bot.py` сейчас около 1202 строк и содержит сразу handlers, FSM, OCR flow, reminders, runtime startup и небольшие wrapper-функции. Самые безопасные первые выделения:

1. `handlers/system.py`

   Вынести `/version`, `/health`, `/backup`, `/story`, `/start`, `/help`, unknown command. Это низкий риск: логика почти не зависит от FSM.

2. `handlers/profile.py`

   Вынести `/setname`, `waiting_for_name`, `/whoami`, базовые user-data flows.

3. `handlers/glucose.py`

   Вынести `/sugar`, `waiting_for_sugar`, сохранение сахара, `build_saved_sugar_text`, high/low/help global handler.

4. `handlers/food_insulin.py`

   Вынести `/food`, `/insulin`, meal flow, dose calculation call, short-insulin follow-up scheduling.

5. `handlers/logs.py`

   Вынести `/log`, period flow, CSV export from journal. Там отдельная ответственность и есть временные CSV-файлы.

6. `handlers/ocr.py`

   Вынести photo intake, OCR callbacks, manual OCR states. Это большой, но логически самодостаточный блок.

7. `handlers/settings.py`

   Вынести `/settings` и WebApp data parser/response.

8. `runtime/reminder_worker.py`

   Вынести `send_due_reminders()` и `reminder_loop()`.

9. `runtime/startup.py`

   Вынести `update_version_in_html()` и `main()` или хотя бы version sync side effect.

Дополнительно стоит убрать из `bot.py` wrapper-дубликаты `get_isf`, `get_ic_ratio`, `get_meal_bonus`, `round_to_step`, `filter_name`, `extract_numbers`, потому что они уже делегируют в `usugar_logic.py`.

## 5. Наиболее опасные технические долги

1. Кодировка/mojibake в пользовательских строках.

   Это самый видимый пользовательский риск. В коде и документах есть строки вида `Рџ...`, `вњ...`. Часть Telegram-текстов может отображаться испорченно.

2. `bot.py` как монолит.

   Любая новая функция повышает риск сломать существующий handler order, FSM state или callback flow.

3. Нет настоящих DB migrations.

   `init_db()` создает таблицы и содержит частичную миграцию `ocr_attempts`, но нет версии схемы, rollback, idempotent migrations для всех будущих изменений.

4. Несоответствие `USUGAR_OCR_ENABLED` фактическому поведению.

   Документация говорит, что OCR controlled/disabled by default, но код запускает recognition в `photo_intake` независимо от флага. Это может удивить при тестировании и на слабом компьютере.

5. OCR accuracy and safety.

   Libre2 CV работает как быстрый путь, но это эвристика. Tesseract зависит от внешней установки, EasyOCR тяжелый и выключен. Автоматического сохранения нет, что хорошо, но доверие к распознаванию пока ограничено.

6. WebApp без prefill и backend API.

   Пользователь может открыть настройки, не видя текущего сохраненного протокола, и случайно перезаписать значения.

7. Нет edit/delete для записей.

   Для журнала сахара/инсулина это опасно на практике: ошибочная запись влияет на статус, reminders и будущую аналитику.

8. Reminder loop без scheduler и durable job model.

   Сейчас это приемлемо для локального MVP, но после рестарта/долгого отключения нет полноценной политики восстановления пропущенных событий.

9. Trusted contact без consent-flow.

   Telegram ID можно указать вручную, но нет явного приглашения, подтверждения и управления доверенным контактом.

10. Локальная и GitHub-история расходятся.

   Для публикаций приходится использовать временный worktree. Это не ломает код, но усложняет сопровождение и повышает риск ошибочного push.

## 6. Задачи до версии 1.1.0

Рекомендуемый фокус для `1.1.0`: не новые большие фичи, а превращение MVP в устойчивую основу.

1. Исправить кодировку пользовательских сообщений и активной документации.

2. Сделать `USUGAR_OCR_ENABLED` настоящим feature flag:

   - если `false`, не запускать heavy OCR;
   - можно оставить caption/manual intake;
   - `/ocr` должен честно совпадать с поведением photo handler.

3. Добавить edit/delete последних записей:

   - минимум удалить последнюю запись сахара;
   - минимум удалить последний insulin log;
   - показать подтверждение перед удалением.

4. Добавить WebApp prefill или отдельную команду просмотра текущих настроек:

   - чтобы пользователь видел текущий protocol перед изменением;
   - не обязательно делать полноценный backend сразу, но нужно убрать риск слепой перезаписи.

5. Ввести первую версию DB schema migrations:

   - таблица `schema_migrations`;
   - idempotent migration runner;
   - зафиксировать текущую схему как baseline.

6. Разделить `bot.py` хотя бы на system/profile/glucose/OCR/reminders handlers.

7. Уточнить OCR v1.1 boundary:

   - Libre2 CV как основной быстрый путь;
   - Tesseract как documented optional;
   - EasyOCR либо убрать из обычного runtime, либо оставить только debug flag;
   - не обещать PaddleOCR до отдельной проверки.

8. Добавить restore/import design, даже если не полный runtime:

   - формат backup уже есть;
   - нужно описать и затем реализовать безопасное восстановление.

9. Добавить минимальный user-facing search/filter:

   - поиск/фильтр журнала по диапазону дат;
   - хотя бы команды или расширенный `/log`.

10. Добавить trusted contact confirmation:

    - trusted user отправляет `/whoami`;
    - основной пользователь вводит ID;
    - бот отправляет trusted user сообщение-подтверждение;
    - alert включается только после подтверждения.

11. Зафиксировать локальную публикационную стратегию GitHub.

    До объединения историй продолжать использовать worktree/PR-подход и не делать force push.

12. Не подключать DeepSeek/LLM до privacy boundary:

    - какие данные можно отправлять;
    - какие нельзя;
    - какие ответы LLM не имеет права давать;
    - где хранится `DEEPSEEK_API_KEY`;
    - как отключить LLM полностью.

## 7. Итог

Код подтверждает большую часть активной документации: бот уже не просто черновик, а рабочий локальный MVP с Telegram-командами, SQLite, WebApp, OCR intake, reminders, backup, story landing и 64 проходящими тестами.

Главные расхождения между документацией и кодом:

- старые GitHub-документы описывают более амбициозную архитектуру, чем реализована;
- OCR feature flag ведет себя не так строго, как описано;
- LLM, import/restore, edit/delete, search, admin/roles и полноценные migrations пока отсутствуют;
- часть функций есть в коде, но документация недостаточно явно говорит об их ограничениях.

Главный следующий инженерный шаг: стабилизировать основу перед `1.1.0`, особенно кодировку, feature flags, DB migrations, edit/delete записей и разбиение `bot.py`.
