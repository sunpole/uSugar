# AI_CONTEXT - uSugar Documentation Navigator

## Current Update For Agents - 2026-06-22

Current runtime/documentation version: `1.6.4`.

uSugar `1.6.4` is a stop-the-line command-routing stability hotfix after the Settings WebApp fix. It documents the runtime/handler audit in `BOT_STABILITY_AUDIT_1_6_4.md` and fixes emoji menu aliases so `⚙️ Настройки`, `📊 Статус`, `📒 Журнал`, `📷 OCR`, `⏰ Напоминания`, `📐 Формула`, and `❓ Помощь` resolve before the random/friendly fallback. No Food Log, OCR tuning, DeepSeek, BJU/product database, A4 diary, token change, or medical formula change was added.

uSugar `1.6.3` is a Settings WebApp hotfix after Trusted Family Thread Mode. The main “Настройки” reply-keyboard button no longer embeds a stale static WebApp URL; it routes through the same `/settings` handler as the command. `/settings` sends a fresh Telegram Mini App button with current protocol prefill, and the bot confirms trusted-contact and family-group settings after save. IDs are masked in chat confirmations but stored fully in the local protocol JSON.

uSugar `1.6.2` adds Trusted Family Thread Mode after the 1.6.1 group hotfix. A configured `family_group` chat/topic can accept daily family records for one patient profile, while ordinary groups remain blocked. `trusted_contact_*` is still the alert destination; `family_group_*` is the allowed source for family entries. OCR still requires confirmation, and Food Log, OCR algorithms, DeepSeek, BJU/product database, and medical formulas did not change.

uSugar `1.6.0` is a safe family-group launch and alias-fix release. It moves Russian navigation aliases into an early router so `настройки`, `журнал`, and other navigation words are handled before friendly fallback replies; `/ocr` now only opens the OCR status screen unless the user explicitly types a mode word; group `/start`, `/help`, `/whoami`, and `/trustedtest` are safe while medical entries and OCR saves stay private-only. No Food Log, DeepSeek, BJU/product database, A4 diary, medical formula change, OCR autosave, token change, or local Telegram publication was added.

uSugar `1.5.9` is a small Telegram UX release after the legacy daily buttons were removed. It adds Russian text aliases for common navigation (`помощь`, `команды`, `настройки`, `статус`, `журнал`, `оцр`, `напоминания`, `формула`, `версия`, `здоровье`, `кто я`, `бэкап`, `история`), keeps `/help` short for daily use, moves the full technical/legacy list to `/commands` and `/devhelp`, and shows the current OCR mode next to the bot version in key surfaces. No medical formula, OCR algorithm, database schema, food_log, DeepSeek, or trusted-contact policy changed.

uSugar `1.5.8` is a small menu cleanup release after the Settings/Menu UX pass: the visible main keyboard is navigation-only, legacy Name/Sugar/Food/Insulin daily buttons are not exposed, and `/help` points users to smart text input (`8.4 сахар`, `3 укол`, `50 40 еда`, OCR mode words, and `отмена`). The commands `/sugar`, `/food`, `/insulin`, and legacy `/setname` remain available for compatibility. Food Log is still not implemented and should be planned as a later feature release.

uSugar `1.5.6` is a runtime hotfix after `1.5.5`: it fixes a Python 3.9 startup crash in the insulin text fallback. The fix replaces a Python 3.10-only type hint with a Python 3.9-compatible `Optional[str]`. No medical formula, OCR algorithm, database schema, Telegram token, or production setting changed.

uSugar `1.5.5` adds a per-user OCR mode selector (`auto`, `libre2_old`, `libre2_new`, `glucometer`) and smart text input for explicit daily phrases such as `8.4 сахар`, `сахр 8.4`, `50 40 еда`, `3 4 5 20 70 80 еда`, `3 укол`, and `укло 3`. The mode is stored in the existing protocol JSON as `ocr_mode`, including Settings WebApp prefill/save. OCR still never saves glucose without explicit confirmation. No medical formula, database schema, DeepSeek, trusted-contact, food-log, or production deployment work was added in this release.

Дата аудита документации: 2026-06-21
Текущая версия проекта по активной документации: `1.5.4`

Этот файл является навигатором по уже существующей документации uSugar. Он не заменяет `README.MD`, `PROJECT_STATUS.md`, `RUNBOOK.md` или `ROADMAP.md`, а помогает человеку или ИИ-агенту понять, какие документы читать первыми и каким документам доверять как актуальным.

## 1. Краткое описание проекта

uSugar - локальный Telegram-бот для семейной поддержки при диабете. Проект помогает записывать сахар, инсулин, еду, смотреть журнал, рассчитывать дозу по настроенному протоколу, принимать Libre2-скриншоты через OCR, отправлять напоминания и хранить историю разработки.

Проект сейчас работает как локальная Windows-разработка:

- Telegram-бот запускается из `bot.py`;
- данные хранятся в локальной SQLite-базе `usugar.db`;
- настройки протокола открываются через `settings.html`;
- интерактивная история проекта открывается через `story.html`;
- локальный сервер uSugar использует порт `8001`, чтобы не конфликтовать с uChurch на `3000` и `5173`;
- ngrok используется для тестирования Telegram WebApp.

## 2. Текущее состояние проекта

Актуальное состояние лучше всего описано в:

- `PROJECT_STATUS.md`;
- `PROJECT_AUDIT.md`;
- `README.MD`;
- `RUNBOOK.md`;
- `CHANGELOG.md`.

На момент аудита проект находится в рабочей MVP-стадии. Локальная версия богаче старой GitHub-документации: есть Telegram runtime, SQLite, WebApp настройки, OCR Libre2, reminders, backup, тесты, story landing, история скриншотов, UX cleanup релиз `1.1.1`, полный 1.2.x путь разделения `bot.py` на system/profile/info/glucose/settings/logs/therapy/OCR/reminders модули, релиз `1.3.0`, который вынес startup version sync в `runtime/startup.py` и оставил `bot.py` composition root, `1.3.1` large-files audit для планирования следующих мини-модулей, `1.3.2` live runtime/OCR verification pass, `1.3.3` product requirements capture, `1.4.0` Settings WebApp + Smart Food Flow, `1.4.1` UX Safety Commands, `1.4.2` Telegram Family Mode + Bot Identity, `1.4.3` Current State + Patch Bot Integration, `1.4.4` Open Work Inventory + uNewsLog Auto Patch Workflow, `1.5.0` OCR New Sources, `1.5.1` OCR Safety Hotfix после живого smoke на публичных примерах, `1.5.2` OCR Reality + uNews Publishing Audit, `1.5.3` New Libre2 Narrow OCR Tuning, а также `1.5.4` Glucometer Photo OCR Tuning.

Важно: публичная GitHub-документация в `docs/github_original/` полезна как исходная концепция, но не является точным описанием текущей локальной реализации.

## 3. Реализованные функции

По активным документам реализованы:

- Telegram-команды `/start`, `/help`, `/version`, `/health`, `/backup`, `/setname`, `/whoami`, `/sugar`, `/status`, `/settings`, `/story`, `/undo`, `/insulin`, `/food`, `/log`, `/formula`, `/ocr`, `/ocrlog`, `/reminders`, `/trustedtest`;
- ручной ввод сахара;
- запись короткого и длинного инсулина;
- расчет еды и дозы по пользовательскому протоколу;
- журнал сахара и инсулина;
- SQLite-хранение пользователей, протокола, журналов, OCR-попыток и напоминаний;
- Telegram WebApp настройки через `settings.html`;
- prefill WebApp настроек из текущего сохранённого протокола через `/settings`;
- редактирование имени через Settings WebApp как предпочтительный путь, с `/setname` как legacy fallback;
- быстрый расчёт еды по суммам вроде `50+40` и `50 40` без сохранения записи в журнал;
- протокольные параметры `glucose_fresh_minutes` и `dose_reduction_percent` для расчёта еды;
- текстовые safety fallback-команды для `/undo`, OCR confirmation и smart-food confirmation, если Telegram-кнопки скрыты;
- Telegram family mode: в группах безопасными считаются `/version`, `/help`, `/health`, `/whoami`, `/reminders`, а медицинский ввод по умолчанию уводится в личный чат;
- `/whoami` показывает `user_id`, `chat_id`, `chat_type` и `message_thread_id` для семейной настройки и доверенного контакта;
- `trusted_contact_thread_id` хранится в protocol/settings JSON и используется для отправки trusted-сообщений в конкретную ветку/топик группы;
- `family_group` хранится в protocol/settings JSON и разрешает принимать записи только из явно настроенного group `chat_id` и optional `message_thread_id`;
- записи из доверенной семейной ветки должны использовать patient profile owner, найденный по `family_group`, а не Telegram `from_user.id` автора сообщения;
- `/trustedtest` проверяет доставку доверенному контакту без медицинских данных;
- `TELEGRAM_BOT_SETUP.md` описывает BotFather identity, команды, privacy mode, канал и политику смены token;
- `CURRENT_STATE.md` даёт короткую карту текущего состояния после паузы;
- `MANUAL_TELEGRAM_TEST_PLAN.md` задаёт ручной Telegram smoke, когда Codex плохо попадает в Telegram Web;
- `PATCH_NOTIFICATION_RULES.md` описывает интеграцию uSugar с локальным uNews workflow в `C:\!CODE_CLUB\new 2026\004_uNews`, каналом `@uNewsLog`, ботом `@uNewsDev_bot` и GitHub auto-discovery через папки `news/` в публичных репозиториях;
- `OPEN_WORK.md` описывает незавершённые задачи по блокам, статусам, приоритетам и целевым релизам;
- `RELEASE_PLAN.md` задаёт порядок следующих релизов от `1.5.0` OCR new sources до `1.8.0` DeepSeek;
- `PATCH_NOTIFICATION_RULES.md` теперь требует patch note и uNewsLog-публикацию после каждого закрытого релиза, если dry-run/check и safety checks зелёные;
- ZIP backup через `/backup`;
- локальный health check;
- локальный source-aware OCR/CV-путь с подтверждением результата: `libre2_cv_old`, `libre2_narrow_updated`, `glucometer_photo`; в `1.5.1` старый Libre2 CV получает приоритет при конфликте, Libre OCR не запускается на фото глюкометров, а сомнительные glucometer-цифры остаются ручной проверкой; в `1.5.2` добавлены `OCR_REALITY_REPORT.md` и `scripts/ocr_smoke.py`, которые честно показывают partial/mostly failed/low-confidence состояние реальных примеров; в `1.5.3` `libre2_narrow_updated` получил landscape ROI для реальных `1280x576` скринов из `img/simple_new`; в `1.5.4` `glucometer_photo` получил fallback-поиск display ROI для реальных фото из `img/simple_gluk`, но все кандидаты остаются manual-only;
- локально проверенное включение OCR через `.env` (`USUGAR_OCR_ENABLED=true`) в версии `1.3.2`;
- optional Tesseract OCR, если установлен `tesseract.exe`;
- отключенный по умолчанию EasyOCR из-за проблем стабильности/скорости на Windows;
- первые реакции "мозгов" бота на хороший, низкий и высокий сахар;
- напоминания о замерах;
- напоминания о базальном инсулине;
- trusted-contact alert по Telegram ID;
- follow-up после короткого инсулина;
- удаление последней ошибочной записи сахара или инсулина через `/undo` с подтверждением;
- сгруппированный `/help`, более понятные `/health`/`/status`/`/reminders`, безопаснее описанный `/undo` и короткий OCR low-confidence flow в версии `1.1.1`;
- стабильные локальные архивы;
- интерактивный исторический лендинг `story.html`;
- генерация `docs/history/story_data.js` из `docs/history/SCREENSHOT_STORY.md`;
- handler-модуль `handlers/system.py` для `/start`, `/help`, `/version`, `/health`, `/story`, `/backup` и unknown slash commands;
- handler-модуль `handlers/profile.py` для `/setname`, `/whoami` и FSM обработки имени;
- handler-модуль `handlers/info.py` для `/formula`, `/ocr` и `/ocrlog`;
- handler-модуль `handlers/glucose.py` для `/sugar`, `/status`, ручного ввода сахара, HI/LOW/help shortcuts, multi-number flow и number-context choice;
- handler-модуль `handlers/settings.py` для `/settings`, `web_app_data`, settings URL и summary текущего протокола;
- handler-модуль `handlers/logs.py` для `/log`, journal FSM и CSV export при длинных журналах;
- handler-модуль `handlers/therapy.py` для `/food`, `/insulin`, `/undo`, food/insulin FSM, undo confirmation и short-insulin follow-up scheduling;
- handler-модуль `handlers/ocr.py` для photo intake, OCR callbacks, OCR confirmation flow и manual OCR value flow;
- runtime-модуль `runtime/reminders.py` для `/reminders`, due-reminder delivery и background reminder loop;
- runtime-модуль `runtime/startup.py` для startup-only синхронизации версии в `settings.html`;
- общие helper-модули `common/text.py`, `common/fsm.py` и `common/chat.py`;
- набор unit tests в `tests/`.

## 4. Незавершенные функции

По документам и аудиту незавершены:

- полноценные три надежные OCR-движка;
- полностью автоматизированный Telegram Web smoke без участия пользователя или риска попадания фокуса в reply-кнопки;
- дальнейшее расширение smart dialog за пределы carb-sum сценария;
- production-grade OCR для реальных обновленных Libre screenshots и фото ручного глюкометра за пределами синтетических/первичных CV-проверок;
- полный trusted-contact consent/verification/quiet-hours flow;
- надежное распознавание времени телефона/замера со скриншота;
- полноценный анализ графика Libre2 как временного ряда;
- автоматическое сохранение OCR только после политики доверия;
- восстановление данных из backup;
- импорт CSV/JSON обратно в базу;
- полноценный поиск по журналам;
- расширенная фильтрация журналов;
- редактирование старых записей и выборочное удаление не только последней записи;
- админ-панель и роли;
- полноценный consent-flow для доверенного лица;
- DeepSeek/LLM слой;
- безопасная база знаний `usugar_knowledge.py`;
- миграции SQLite;
- CI/CD;
- production deployment;
- дальнейшая production-подготовка без расширения `bot.py` новыми пользовательскими flows.

## 5. Основные документы

Эти документы являются основной рабочей документацией:

- `README.MD` - краткий обзор проекта, запуск, команды, текущий состав.
- `PROJECT_STATUS.md` - актуальный статус проекта и что уже сделано.
- `PROJECT_AUDIT.md` - подробный аудит структуры, функций, долгов и архитектуры.
- `PRODUCT_REQUIREMENTS.md` - актуальные продуктовые требования перед следующим этапом реализации; это не список уже реализованных функций.
- `LARGE_FILES_AUDIT.md` - план дальнейшего разделения больших файлов после завершения `bot.py` refactor.
- `RUNBOOK.md` - практические инструкции запуска, проверки, ngrok, OCR, backup.
- `ROADMAP.md` - план развития по фазам.
- `CHANGELOG.md` - история версий и изменений.
- `DOCS_INDEX.md` - краткая карта документации.
- `BOT_BRAIN.md` - поведение бота, реакции и roadmap напоминаний.
- `DATA_SOURCES.md` - внешние источники данных и границы безопасности.
- `FUTURE_BACKLOG.md` - осознанно отложенные задачи после версии `1.1.0`.
- `PRODUCT_REQUIREMENTS.md` - продуктовые требования, которые нельзя считать уже реализованными без проверки кода.
- `SECRETS.md` - правила секретов и `.env`.
- `VERSIONING.md` - правила версионирования.
- `STABLE_ARCHIVES.md` - правила локальных стабильных архивов.
- `TELEGRAM_SMOKE_TEST.md` - ручной чек-лист Telegram-проверки.
- `TELEGRAM_BOT_SETUP.md` - BotFather identity, privacy mode, family group/channel policy, trusted-contact test and token timing.
- `CURRENT_STATE.md` - короткое текущее состояние проекта.
- `MANUAL_TELEGRAM_TEST_PLAN.md` - как проводить ручной Telegram smoke через пользователя.
- `PATCH_NOTIFICATION_RULES.md` - как готовить uSugar patch notes для uNews.
- `OPEN_WORK.md` - текущая карта незавершённых задач.
- `RELEASE_PLAN.md` - целевой порядок следующих релизов.
- `OCR_REALITY_REPORT.md` - фактическое состояние OCR по папкам `img/simple`, `img/simple_new`, `img/simple_gluk`.
- `TEXT_INTEGRITY_REPORT.md` - результат проверки текущих текстов на битую кодировку и кракозябры.
- `UNEWS_PUBLISHING_AUDIT.md` - разбор, почему uSugar 1.5.0/1.5.1 не появились в `@uNewsLog`.
- `docs/history/SCREENSHOT_STORY.md` - источник истории скриншотов для landing/story.

## 6. Вспомогательные документы

Вспомогательные, но полезные документы:

- `NEXT_STEPS.md` - старый ближайший план; читать после актуального `ROADMAP.md`.
- `NGROK_AND_UCHURCH.md` - отдельная справка о совместном запуске uSugar и uChurch.
- `GITHUB_PUBLISH_PLAN.md` - план безопасной публикации с учетом разной истории локального и GitHub-репозитория.
- `docs/history/*.txt` - текстовые снимки этапов работы.
- `docs/history/*.png` - скриншоты истории разработки.
- `docs/history/story_data.js` - сгенерированные данные для `story.html`.
- `docs/github_original/img/simple/*` - оригинальные изображения из GitHub-документации.
- `docs/github_original/LICENSE` - лицензия из GitHub-снимка.

## 7. Устаревшие документы

Эти документы не стоит читать как описание текущего кода:

- `docs/github_original/README.md`;
- `docs/github_original/README_1.0.0.MD`;
- `docs/github_original/NOTE_1.0.0.MD`;
- `docs/github_original/NOTE_1.0.1.MD`;
- `docs/github_original/NOTE_1.0.2.MD`;
- `docs/github_original/MVP_1.0.0.MD`;
- `docs/github_original/STRUCTURE.MD`;
- `docs/github_original/REQUIREMENTS.MD`;
- `docs/github_original/START_INFO_DOC_MD`;
- `docs/legacy_local/README_GITHUB.md`;
- `docs/legacy_local/README_1.0.0.MD`;
- `docs/legacy_local/NOTE_1.0.0.MD`;
- `docs/legacy_local/NOTE_1.0.1.MD`;
- `docs/legacy_local/NOTE_1.0.2.MD`;
- `docs/legacy_local/MVP_1.0.0.MD`;
- `docs/legacy_local/STRUCTURE.MD`;
- `docs/legacy_local/REQUIREMENTS.MD`;
- `docs/legacy_local/START_INFO_DOC_MD`.

Причина: эти файлы описывают раннюю концепцию, плановую архитектуру, PaddleOCR/EasyOCR/Tesseract как целевую схему, `/setup`, будущую модульную структуру и сценарии, которые не полностью совпадают с текущей локальной реализацией.

## 8. Документы, которые дублируют друг друга

Обнаружены точные или почти точные дубли между `docs/github_original/` и `docs/legacy_local/`:

- `docs/github_original/MVP_1.0.0.MD` и `docs/legacy_local/MVP_1.0.0.MD`;
- `docs/github_original/NOTE_1.0.0.MD` и `docs/legacy_local/NOTE_1.0.0.MD`;
- `docs/github_original/NOTE_1.0.1.MD` и `docs/legacy_local/NOTE_1.0.1.MD`;
- `docs/github_original/NOTE_1.0.2.MD` и `docs/legacy_local/NOTE_1.0.2.MD`;
- `docs/github_original/NOTE_1.0.2.MD.png` и `docs/legacy_local/NOTE_1.0.2.MD.png`;
- `docs/github_original/README.md` и `docs/legacy_local/README_GITHUB.md`;
- `docs/github_original/START_INFO_DOC_MD` и `docs/legacy_local/START_INFO_DOC_MD`;
- `docs/github_original/STORY.MD` и `docs/legacy_local/STORY.MD`;
- `docs/github_original/STRUCTURE.MD` и `docs/legacy_local/STRUCTURE.MD`.

Есть смысл считать `docs/github_original/` исходным снимком GitHub, а `docs/legacy_local/` - локальным историческим архивом. Ни один из этих каталогов не должен спорить с активными документами в корне.

## 9. Рекомендуемый порядок чтения для нового разработчика

1. `README.MD` - понять, что это за проект и как он запускается.
2. `PROJECT_STATUS.md` - понять текущее состояние.
3. `PROJECT_AUDIT.md` - увидеть структуру, реализованное, незавершенное и долги.
4. `RUNBOOK.md` - научиться запускать и проверять проект локально.
5. `TELEGRAM_SMOKE_TEST.md` - понять ручной Telegram-тест.
6. `CURRENT_STATE.md` - быстро понять, что реально работает прямо сейчас.
7. `TELEGRAM_BOT_SETUP.md` - понять официальную настройку бота, группы, канал и token policy.
8. `ROADMAP.md` - увидеть планы развития.
9. `CHANGELOG.md` - понять историю версий.
10. `BOT_BRAIN.md` - понять поведение бота и напоминания.
11. `DATA_SOURCES.md` - понять границы внешних данных и LLM.
12. `docs/history/SCREENSHOT_STORY.md` - понять историю разработки и landing.
13. `SECRETS.md`, `VERSIONING.md`, `STABLE_ARCHIVES.md` - прочитать перед реальной работой с секретами, версиями и архивами.

## 10. Рекомендуемый порядок чтения для ИИ-агента

1. `docs/AI_CONTEXT.md` - текущий навигатор и границы доверия к документации.
2. `PROJECT_STATUS.md` - актуальное состояние.
3. `PROJECT_AUDIT.md` - подробный аудит проекта.
4. `LARGE_FILES_AUDIT.md` - следующий архитектурный ориентир после `bot.py`, если задача касается распила файлов.
5. `README.MD` - краткая рабочая картина.
6. `RUNBOOK.md` - запуск, ngrok, OCR, backup, runtime.
7. `ROADMAP.md` - план работ.
8. `BOT_BRAIN.md` - поведение, напоминания, UX-правила.
9. `DATA_SOURCES.md` - LLM/data safety.
10. `TELEGRAM_SMOKE_TEST.md` - ручная проверка через пользователя.
11. `CURRENT_STATE.md` - быстрый operational snapshot.
12. `MANUAL_TELEGRAM_TEST_PLAN.md` - правила ручного smoke через пользователя.
13. `OPEN_WORK.md` - текущие незавершённые задачи по областям.
14. `RELEASE_PLAN.md` - порядок следующих релизов.
15. `PATCH_NOTIFICATION_RULES.md` - uNews patch-note workflow.
16. `TELEGRAM_BOT_SETUP.md` - BotFather, family mode, channel policy and token timing.
17. `docs/history/SCREENSHOT_STORY.md` - как обновлять историю и story page.
18. `CHANGELOG.md`, `VERSIONING.md`, `STABLE_ARCHIVES.md` - версии, архивы, история изменений.

ИИ-агенту не рекомендуется начинать с `docs/github_original/` или `docs/legacy_local/`, потому что там много старых целей, не равных текущему коду.

## 11. Где находятся ключевые темы

### Архитектура

- Актуально: `PROJECT_AUDIT.md`, `LARGE_FILES_AUDIT.md`, `README.MD`, `PROJECT_STATUS.md`.
- Практический запуск: `RUNBOOK.md`.
- Историческая/плановая архитектура: `docs/github_original/NOTE_1.0.2.MD`, `docs/legacy_local/NOTE_1.0.2.MD`.

### История проекта

- `docs/history/SCREENSHOT_STORY.md`;
- `docs/history/*.txt`;
- `docs/history/*.png`;
- `CHANGELOG.md`;
- `PROJECT_STATUS.md`;
- `story.html`.

### Планы развития

- `ROADMAP.md`;
- `OPEN_WORK.md`;
- `RELEASE_PLAN.md`;
- `NEXT_STEPS.md`;
- `BOT_BRAIN.md`;
- `DATA_SOURCES.md`;
- `PROJECT_AUDIT.md`.

### Статус проекта

- `PROJECT_STATUS.md`;
- `PROJECT_AUDIT.md`;
- `FUTURE_BACKLOG.md`.
- `CHANGELOG.md`;
- `README.MD`.

### Источники данных

- `DATA_SOURCES.md`.

### Описание OCR

- `RUNBOOK.md` раздел `OCR / Photo Intake`;
- `ROADMAP.md` раздел `OCR And Medical Safety`;
- `PROJECT_AUDIT.md` разделы про OCR и изображения;
- `TELEGRAM_SMOKE_TEST.md` OCR-шаги;
- `DATA_SOURCES.md` для целевой идеи трех OCR-движков;
- исторически: `docs/github_original/NOTE_1.0.2.MD`.

### Описание LLM

- `DATA_SOURCES.md` - границы DeepSeek/LLM;
- `ROADMAP.md` - будущий DeepSeek helper layer;
- `SECRETS.md` - `DEEPSEEK_API_KEY` только в `.env`, без вывода в логи, backups, screenshots или GitHub.

На момент аудита LLM не реализован как рабочая функция бота.

### Описание исторического лендинга

- `RUNBOOK.md` раздел `Project Story Page`;
- `README.MD` раздел документации и story;
- `PROJECT_AUDIT.md` раздел исторического лендинга;
- `docs/history/SCREENSHOT_STORY.md`;
- `story.html`.

### Описание генератора историй

- `RUNBOOK.md` указывает команду `venv\Scripts\python.exe scripts\generate_story_data.py`;
- `DOCS_INDEX.md` объясняет связку `SCREENSHOT_STORY.md` -> `story_data.js` -> `story.html`;
- `docs/history/SCREENSHOT_STORY.md` задает исходный контент;
- `scripts/generate_story_data.py` является фактической реализацией генератора.

## 12. Документы, которые нужно поддерживать после каждой крупной задачи

После каждой крупной задачи обновлять при необходимости:

- `CHANGELOG.md` - что изменилось и в какой версии;
- `PROJECT_STATUS.md` - текущее состояние проекта;
- `ROADMAP.md` - что стало done, что осталось future;
- `FUTURE_BACKLOG.md` - если задача сознательно отложена за пределы стабильной версии;
- `RUNBOOK.md` - если изменились запуск, переменные `.env`, ngrok, backup, OCR, reminders;
- `README.MD` - если изменились ключевые возможности или команды;
- `TELEGRAM_SMOKE_TEST.md` - если изменилась ручная проверка в Telegram;
- `OPEN_WORK.md` - если задача закрывает или добавляет крупный блок работы;
- `RELEASE_PLAN.md` - если меняется целевой порядок релизов;
- `PATCH_NOTIFICATION_RULES.md` - если меняется uNews/uNewsLog release workflow;
- `BOT_BRAIN.md` - если изменились реакции, reminders, trusted alerts, follow-ups;
- `DATA_SOURCES.md` - если добавлены источники данных, LLM или знания;
- `docs/history/SCREENSHOT_STORY.md` - если добавлены исторические скриншоты;
- `docs/history/story_data.js` - регенерировать после изменения `SCREENSHOT_STORY.md`;
- `PROJECT_AUDIT.md` - после больших архитектурных изменений или нового аудита;
- `docs/AI_CONTEXT.md` - если изменилась карта документации или source-of-truth документы.

## 13. Source of Truth

Основные источники истины на текущем этапе:

- `VERSION.json` и `version_info.py` - текущая версия runtime.
- `README.MD` - общий рабочий обзор.
- `PROJECT_STATUS.md` - актуальное состояние.
- `PROJECT_AUDIT.md` - подробная карта структуры и долгов.
- `LARGE_FILES_AUDIT.md` - карта следующих безопасных мини-рефакторингов после `bot.py`.
- `RUNBOOK.md` - как запускать и проверять.
- `ROADMAP.md` - куда развивать.
- `CHANGELOG.md` - что было сделано по версиям.
- `BOT_BRAIN.md` - правила поведения бота и напоминаний.
- `DATA_SOURCES.md` - внешние данные, LLM и safety boundaries.
- `FUTURE_BACKLOG.md` - отложенные задачи, которые не должны смешиваться с текущим стабильным scope.
- `SECRETS.md` - секреты и `.env`.
- `TELEGRAM_SMOKE_TEST.md` - ручной Telegram smoke test.
- `TELEGRAM_BOT_SETUP.md` - официальная Telegram-настройка бота, семейные группы, канал, privacy mode и token policy.
- `CURRENT_STATE.md` - короткая карта того, что работает, что частично работает и что ещё не реализовано.
- `MANUAL_TELEGRAM_TEST_PLAN.md` - ручной Telegram smoke plan.
- `PATCH_NOTIFICATION_RULES.md` - правила patch notes для uNews.
- `OPEN_WORK.md` - source of truth для незавершённой работы.
- `RELEASE_PLAN.md` - source of truth для следующей последовательности релизов.
- `docs/history/SCREENSHOT_STORY.md` - source of truth для исторического лендинга.
- `DOCS_INDEX.md` и `docs/AI_CONTEXT.md` - навигация по документации.

## 14. Не использовать как источник актуальной информации

Не использовать как актуальную правду о текущем коде:

- `docs/github_original/*`;
- `docs/legacy_local/*`;
- `docs/history/*.txt`;
- `docs/history/*.png`;
- `docs/history/story_data.js`;
- `NEXT_STEPS.md`, если он противоречит `ROADMAP.md`, `PROJECT_STATUS.md` или `PROJECT_AUDIT.md`;
- `GITHUB_PUBLISH_PLAN.md`, если вопрос не про публикацию;
- старые Mermaid/PNG/SVG диаграммы из `NOTE_1.0.2`.

Эти файлы полезны для истории, идеи продукта, презентации и сравнения ранней задумки с текущей реализацией, но при конфликте нужно доверять активным корневым документам и фактическому коду.

## 15. Короткое правило для будущей работы

Если нужно понять проект быстро:

1. читать `docs/AI_CONTEXT.md`;
2. проверять `PROJECT_STATUS.md`;
3. сверяться с `PROJECT_AUDIT.md`;
4. запускать по `RUNBOOK.md`;
5. менять план в `ROADMAP.md`;
6. фиксировать изменения в `CHANGELOG.md`;
7. не брать `docs/github_original/` и `docs/legacy_local/` как актуальное описание реализации.
