# AI_CONTEXT - uSugar Documentation Navigator

Дата аудита документации: 2026-06-11
Текущая версия проекта по активной документации: `1.0.39`

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

На момент аудита проект находится в рабочей MVP-стадии. Локальная версия богаче старой GitHub-документации: есть Telegram runtime, SQLite, WebApp настройки, OCR Libre2, reminders, backup, тесты, story landing и история скриншотов.

Важно: публичная GitHub-документация в `docs/github_original/` полезна как исходная концепция, но не является точным описанием текущей локальной реализации.

## 3. Реализованные функции

По активным документам реализованы:

- Telegram-команды `/start`, `/help`, `/version`, `/health`, `/backup`, `/setname`, `/whoami`, `/sugar`, `/status`, `/settings`, `/story`, `/insulin`, `/food`, `/log`, `/formula`, `/ocr`, `/ocrlog`, `/reminders`;
- ручной ввод сахара;
- запись короткого и длинного инсулина;
- расчет еды и дозы по пользовательскому протоколу;
- журнал сахара и инсулина;
- SQLite-хранение пользователей, протокола, журналов, OCR-попыток и напоминаний;
- Telegram WebApp настройки через `settings.html`;
- ZIP backup через `/backup`;
- локальный health check;
- локальный Libre2 OCR/CV-путь с подтверждением результата;
- optional Tesseract OCR, если установлен `tesseract.exe`;
- отключенный по умолчанию EasyOCR из-за проблем стабильности/скорости на Windows;
- первые реакции "мозгов" бота на хороший, низкий и высокий сахар;
- напоминания о замерах;
- напоминания о базальном инсулине;
- trusted-contact alert по Telegram ID;
- follow-up после короткого инсулина;
- стабильные локальные архивы;
- интерактивный исторический лендинг `story.html`;
- генерация `docs/history/story_data.js` из `docs/history/SCREENSHOT_STORY.md`;
- набор unit tests в `tests/`.

## 4. Незавершенные функции

По документам и аудиту незавершены:

- полноценные три надежные OCR-движка;
- надежное распознавание времени телефона/замера со скриншота;
- полноценный анализ графика Libre2 как временного ряда;
- автоматическое сохранение OCR только после политики доверия;
- восстановление данных из backup;
- импорт CSV/JSON обратно в базу;
- полноценный поиск по журналам;
- расширенная фильтрация журналов;
- редактирование и удаление ошибочных записей;
- загрузка текущего протокола обратно в WebApp перед редактированием;
- админ-панель и роли;
- полноценный consent-flow для доверенного лица;
- DeepSeek/LLM слой;
- безопасная база знаний `usugar_knowledge.py`;
- миграции SQLite;
- CI/CD;
- production deployment;
- разбиение крупного `bot.py` на handlers/services.

## 5. Основные документы

Эти документы являются основной рабочей документацией:

- `README.MD` - краткий обзор проекта, запуск, команды, текущий состав.
- `PROJECT_STATUS.md` - актуальный статус проекта и что уже сделано.
- `PROJECT_AUDIT.md` - подробный аудит структуры, функций, долгов и архитектуры.
- `RUNBOOK.md` - практические инструкции запуска, проверки, ngrok, OCR, backup.
- `ROADMAP.md` - план развития по фазам.
- `CHANGELOG.md` - история версий и изменений.
- `DOCS_INDEX.md` - краткая карта документации.
- `BOT_BRAIN.md` - поведение бота, реакции и roadmap напоминаний.
- `DATA_SOURCES.md` - внешние источники данных и границы безопасности.
- `SECRETS.md` - правила секретов и `.env`.
- `VERSIONING.md` - правила версионирования.
- `STABLE_ARCHIVES.md` - правила локальных стабильных архивов.
- `TELEGRAM_SMOKE_TEST.md` - ручной чек-лист Telegram-проверки.
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
6. `ROADMAP.md` - увидеть планы развития.
7. `CHANGELOG.md` - понять историю версий.
8. `BOT_BRAIN.md` - понять поведение бота и напоминания.
9. `DATA_SOURCES.md` - понять границы внешних данных и LLM.
10. `docs/history/SCREENSHOT_STORY.md` - понять историю разработки и landing.
11. `SECRETS.md`, `VERSIONING.md`, `STABLE_ARCHIVES.md` - прочитать перед реальной работой с секретами, версиями и архивами.

## 10. Рекомендуемый порядок чтения для ИИ-агента

1. `docs/AI_CONTEXT.md` - текущий навигатор и границы доверия к документации.
2. `PROJECT_STATUS.md` - актуальное состояние.
3. `PROJECT_AUDIT.md` - подробный аудит проекта.
4. `README.MD` - краткая рабочая картина.
5. `RUNBOOK.md` - запуск, ngrok, OCR, backup, runtime.
6. `ROADMAP.md` - план работ.
7. `BOT_BRAIN.md` - поведение, напоминания, UX-правила.
8. `DATA_SOURCES.md` - LLM/data safety.
9. `TELEGRAM_SMOKE_TEST.md` - ручная проверка через пользователя.
10. `docs/history/SCREENSHOT_STORY.md` - как обновлять историю и story page.
11. `CHANGELOG.md`, `VERSIONING.md`, `STABLE_ARCHIVES.md` - версии, архивы, история изменений.

ИИ-агенту не рекомендуется начинать с `docs/github_original/` или `docs/legacy_local/`, потому что там много старых целей, не равных текущему коду.

## 11. Где находятся ключевые темы

### Архитектура

- Актуально: `PROJECT_AUDIT.md`, `README.MD`, `PROJECT_STATUS.md`.
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
- `NEXT_STEPS.md`;
- `BOT_BRAIN.md`;
- `DATA_SOURCES.md`;
- `PROJECT_AUDIT.md`.

### Статус проекта

- `PROJECT_STATUS.md`;
- `PROJECT_AUDIT.md`;
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
- `RUNBOOK.md` - если изменились запуск, переменные `.env`, ngrok, backup, OCR, reminders;
- `README.MD` - если изменились ключевые возможности или команды;
- `TELEGRAM_SMOKE_TEST.md` - если изменилась ручная проверка в Telegram;
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
- `RUNBOOK.md` - как запускать и проверять.
- `ROADMAP.md` - куда развивать.
- `CHANGELOG.md` - что было сделано по версиям.
- `BOT_BRAIN.md` - правила поведения бота и напоминаний.
- `DATA_SOURCES.md` - внешние данные, LLM и safety boundaries.
- `SECRETS.md` - секреты и `.env`.
- `TELEGRAM_SMOKE_TEST.md` - ручной Telegram smoke test.
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
