# PROJECT_STORY_AUDIT

Дата аудита: 2026-06-11

Цель: превратить историю uSugar в полноценную презентацию развития продукта, не создавая фейковые скриншоты и не меняя код, Telegram-бота, OCR или базу данных.

Изученные источники:

- `docs/history/SCREENSHOT_STORY.md`
- `CHANGELOG.md`
- `PROJECT_STATUS.md`
- `ROADMAP.md`
- `docs/AI_CONTEXT.md`

## Краткий вывод

История проекта уже имеет хороший живой материал: начало в локальной папке, очистка, первые Telegram-доказательства, OCR, напоминания, WebApp-настройки, trusted contact, follow-up reminders, mobile settings и Story Landing.

Главный пробел: версия `1.1.0` пока не имеет исторического скриншота, хотя это первый стабильный daily-use release. Также документационные аудиты (`PROJECT_AUDIT`, `CODE_AUDIT`, `AI_CONTEXT`, `FUTURE_BACKLOG`) важны для зрелости проекта, но почти не представлены визуально.

Не нужно делать скриншот на каждую патч-версию. Для презентации лучше собрать 20 сильных кадров, которые показывают продуктовую эволюцию и работу Codex: от хаоса к управляемой системе.

## Покрытие версий из CHANGELOG

| Версия | Историческое событие | Есть запись в истории | Есть скриншот | Описание | Историческая ценность | Рекомендация |
| --- | --- | --- | --- | --- | --- | --- |
| 1.1.0 | Stability Release: OCR flag, `/undo`, settings prefill, backlog | Нет | Нет | Нет | Высокая | Нужен новый финальный скриншот релиза |
| 1.0.39 | Story Landing | Да | Да | Хорошее | Высокая | Оставить, добавить рядом 1.1.0 |
| 1.0.38 | Mobile settings compact | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.37 | Follow-up reminders | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.36 | Stable archive workflow | Нет | Нет | Нет | Средняя | Нужен только если показывать rollback-культуру |
| 1.0.35 | Trusted Contact | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.34 | WebApp reminder settings | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.33 | Basal reminders | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.32 | Command/help/docs sync | Нет | Нет | Нет | Средняя | Можно объединить с 1.1.0 stability |
| 1.0.31 | Background reminders | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.30 | Reminder engine | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.29 | First bot brain | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.28 | Libre2 trend metadata | Нет | Нет | Нет | Средняя | Можно показать через OCR composite, не отдельным кадром |
| 1.0.27 | Inline OCR confirmation | Нет | Нет | Нет | Средняя | Нужен скрин только если есть живой Telegram кадр с inline buttons |
| 1.0.26 | Telegram network retry | Нет | Нет | Нет | Средняя | Визуально трудно, достаточно changelog |
| 1.0.25 | EasyOCR disabled by default | Нет | Нет | Нет | Средняя | Покрыть в OCR architecture/process кадре |
| 1.0.24 | Libre2 CV fallback recognizes 8.2 | Нет | Нет | Нет | Высокая | Нужен хороший OCR result screenshot или composite |
| 1.0.23 | OCR pipeline implementation | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.22 | OCR photo confirmation workflow | Нет | Нет | Нет | Высокая | Нужен Telegram screenshot с confirmation buttons |
| 1.0.21 | OCR metadata in backup | Нет | Нет | Нет | Средняя | Можно объединить с backup/OCR audit |
| 1.0.20 | `/ocrlog` | Нет | Нет | Нет | Средняя | Второстепенно |
| 1.0.19 | `ocr_attempts` table | Нет | Нет | Нет | Средняя | Архитектурный момент, не обязателен для презентации |
| 1.0.18 | OCR aggregation rules | Нет | Нет | Нет | Высокая | Нужен diagram-like screenshot позже, но не фейковый |
| 1.0.17 | Safe OCR intake | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.16 | Unknown command fallback | Нет | Нет | Нет | Низкая | Не нужен отдельный кадр |
| 1.0.15 | Version footer in responses | Нет | Нет | Нет | Средняя | Уже видно в Telegram screenshots |
| 1.0.14 | FSM command priority fix | Нет | Нет | Нет | Средняя | Не нужен отдельный кадр |
| 1.0.13 | Formula builder extracted | Нет | Нет | Нет | Средняя | Покрыто `/formula` screenshot |
| 1.0.12 | Telegram backup | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.11 | Telegram health | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.10 | First managed bot version proof | Да | Да | Хорошее | Высокая | Оставить |
| 1.0.9 | Version file/runtime scripts/testable logic | Нет | Частично через process screenshots | Частичное | Высокая | Нужен или оставить process_git/runtime кадры |
| 1.0.8 | FSM priority/name/filter/multi-number | Нет | Нет | Нет | Средняя | Не нужен отдельный кадр |
| 1.0.7 | Global HI/LOW/name field | Нет | Нет | Нет | Средняя | Не нужен отдельный кадр |
| 1.0.6 | Insulin/HI LOW/smart reaction | Нет | Нет | Нет | Средняя | Можно покрыть будущим Telegram happy-path demo |
| 1.0.5 | Formula reference improvements | Частично | Да, `/formula` | Хорошее | Средняя | Оставить `/formula` как объединенный кадр |
| 1.0.4 | Default config/anonymity/whoami/webapp env | Нет | Нет | Нет | Средняя | Покрыть future `/whoami` or settings screenshot if needed |
| 1.0.3 | Pen step/rounding/formula abbreviations | Частично | Да, `/formula` | Хорошее | Средняя | Не нужен отдельный кадр |
| 1.0.2 | Separate tables/log/CSV/export/duplicate commands | Частично | Через backup/log screenshots | Частичное | Средняя | Не нужен отдельный кадр |
| 1.0.1 | First SQLite database version | Нет | Начальная папка косвенно | Нет | Высокая как origin | Нужен текстовый story block, не обязательно скрин |

## Исторически важные события

Эти события должны остаться в будущей интерактивной презентации как основные главы.

1. Начальная папка проекта.
   - Скриншот: есть `2026-06-04_initial_state.png`.
   - Ценность: показывает реальный старт, а не отполированную витрину.
   - Текст: "uSugar начался не с идеальной архитектуры, а с живой рабочей папки, где уже были код, заметки, база и следы предыдущих попыток."

2. Документация и secret cleanup.
   - Скриншоты: есть before/after cleanup.
   - Ценность: показывает переход от риска и разрозненности к управляемой работе.
   - Текст: "Первый серьёзный шаг был не в новых функциях, а в безопасности: отделить секреты, сохранить историю и собрать документы в одном месте."

3. Первый управляемый runtime и health check.
   - Скриншоты: есть health check, ngrok readiness, Telegram `/health`.
   - Ценность: доказывает, что проект можно запускать рядом с uChurch.
   - Текст: "uSugar получает собственный безопасный runtime: отдельный порт, проверка окружения и Telegram-команда без раскрытия секретов."

4. Первый живой Telegram bot.
   - Скриншот: есть `2026-06-05_runtime_telegram_bot_version_public.png`.
   - Ценность: первый момент, когда проект стал не файлами, а работающим ботом.
   - Текст: "Бот оживает в Telegram и начинает отвечать текущей версией."

5. Formula reference.
   - Скриншот: есть `2026-06-05_runtime_telegram_formula_reference_public.png`.
   - Ценность: объясняет сердце продукта: протокол и расчет.
   - Текст: "uSugar не прячет формулу: пользователь видит, какие параметры участвуют в расчёте."

6. Backup.
   - Скриншот: есть backup public.
   - Ценность: первая пользовательская защита данных.
   - Текст: "Появляется первый пользовательский экспорт: протокол и журналы можно забрать ZIP-файлом без секретов."

7. Safe OCR intake.
   - Скриншот: есть `v1_0_17`.
   - Ценность: важный safety-first подход: сначала принять фото, потом распознавать.
   - Текст: "OCR начинается безопасно: бот принимает скриншот, но не делает медицинских действий без подтверждения."

8. OCR pipeline.
   - Скриншот: есть `v1_0_23`.
   - Ценность: переход от идеи OCR к реальному pipeline.
   - Текст: "Скриншот Libre2 становится входом в систему: варианты изображения, движки и подтверждение собираются в один поток."

9. Libre2 CV 8.2 recognition.
   - Скриншот: нет отдельного сильного кадра для `1.0.24`.
   - Нужен новый скриншот: Telegram chat showing uploaded Libre2 test screenshot and bot response with candidate `8.2`.
   - Как сделать: использовать публично безопасный тестовый Libre2 screenshot, скрыть личные чаты, оставить версию бота и OCR variants visible.
   - Текст: "Первый уверенный локальный OCR-результат: Libre2 CV читает 8.2 без внешнего сервиса."

10. Bot brain.
   - Скриншот: есть `v1_0_29`.
   - Ценность: бот становится помощником, а не только журналом.
   - Текст: "uSugar начинает отвечать по смыслу: хороший, низкий и высокий сахар получают разные реакции."

11. Reminder engine.
   - Скриншоты: есть `v1_0_30`, `v1_0_31`, `v1_0_33`.
   - Ценность: главный переход к семейному ежедневному ассистенту.
   - Текст: "Проект начинает думать во времени: когда был последний замер, когда ждать следующий и когда напомнить."

12. WebApp reminder settings.
   - Скриншот: есть `v1_0_34`.
   - Ценность: настройки уходят из кода в пользовательский протокол.
   - Текст: "Расписание напоминаний становится настраиваемым: окна, задержки и длинный инсулин переходят в WebApp."

13. Trusted Contact.
   - Скриншот: есть `v1_0_35`.
   - Ценность: семейный смысл продукта.
   - Текст: "uSugar делает первый шаг к семейной безопасности: если замеров долго нет, можно уведомить доверенное лицо."

14. Follow-up reminders.
   - Скриншот: есть `v1_0_37`.
   - Ценность: бот начинает помнить действия пользователя.
   - Текст: "После короткого инсулина бот может напомнить о контрольном замере."

15. Mobile settings.
   - Скриншот: есть `v1_0_38`.
   - Ценность: важный UX шаг.
   - Текст: "Настройка протокола становится пригодной для телефона, а не только для большого экрана."

16. Project audits.
   - Скриншотов: нет для `PROJECT_AUDIT.md` и `CODE_AUDIT.md`.
   - Нужен новый скриншот: side-by-side Codex/docs view or file preview showing audit headings.
   - Как сделать: открыть `PROJECT_AUDIT.md` or `CODE_AUDIT.md` in editor, рядом терминал с git/log or tests if useful; без приватных данных.
   - Текст: "Проект получает техническую память: не только что сделано, но и какие долги видны."

17. AI_CONTEXT.
   - Скриншота: нет.
   - Нужен новый скриншот: editor preview of `docs/AI_CONTEXT.md` with sections "Source of Truth" and reading order.
   - Как сделать: открыть файл в редакторе, оставить видимой структуру документа.
   - Текст: "AI_CONTEXT становится входной дверью для будущего ИИ-агента: где правда, где архив, с чего начинать."

18. Story Landing `1.0.39`.
   - Скриншот: есть.
   - Ценность: мета-уровень, история разработки стала продуктовой страницей.
   - Текст: "История разработки превращается в интерактивный лендинг, собранный из настоящих скриншотов."

19. Stability Release `1.1.0`.
   - Скриншота: нет.
   - Нужен новый скриншот: side-by-side Codex/terminal and Telegram/WebApp showing `uSugarBot v1.1.0`, `/undo` or `/settings` prefill, and tests `66 OK`.
   - Как сделать: открыть Telegram Web with bot response `/version` or `/settings`, рядом Codex/terminal with test result or changelog; скрыть личные чаты.
   - Текст: "uSugar 1.1.0 фиксирует первый стабильный daily-use release: OCR-флаг, undo, prefilled settings и отдельный backlog будущих задач."

20. FUTURE_BACKLOG.
   - Скриншота: нет.
   - Нужен новый скриншот: `FUTURE_BACKLOG.md` в редакторе, видны категории OCR, Architecture, Database, LLM, Deployment.
   - Как сделать: открыть файл, рядом можно оставить changelog `1.1.0`; без терминала с секретами.
   - Текст: "Идеи не потеряны, но отделены от стабильного релиза: будущая работа получает своё место."

## Полезные, но второстепенные события

- `1.0.16` unknown command fallback.
- `1.0.20` `/ocrlog`.
- `1.0.21` OCR metadata in backup.
- `1.0.26` Telegram polling retry.
- `1.0.27` inline OCR buttons, если нет хорошего живого кадра.
- `1.0.28` trend metadata, пока график не стал полноценной функцией.
- `1.0.32` command/help/docs sync.
- `1.0.36` stable archive workflow.

Эти события стоит оставить в changelog и, возможно, в фильтрах story landing, но не делать главными карточками презентации.

## Лишние или дублирующие события

- Несколько cleanup/root/docs tree кадров можно объединить в одну главу "Организация проекта".
- Backup public и backup/help showcase частично дублируют друг друга. Для короткой презентации оставить showcase, для подробной истории оставить оба.
- Reminder engine, background reminders and basal reminders важны, но в публичной презентации их стоит показывать как одну главу с 2-3 слайдами, а не как три равнозначных центральных события.
- Версии `1.0.1`-`1.0.8` лучше описывать текстом "ранняя внутренняя история" без попытки задним числом создавать скриншоты.

## Особые зоны внимания

### 1.0.39 Story Landing

Состояние: покрыто хорошо.

Улучшение: нужен второй screenshot later после добавления `1.1.0` в историю, чтобы показать, что landing обновляется и растёт.

Рекомендуемый текст: "История uSugar больше не живёт только в заметках: она собирается в интерактивную страницу из настоящих этапов работы."

### 1.1.0 Stability Release

Состояние: не покрыто.

Нужен скриншот: Telegram `/settings` or `/version` with `uSugarBot v1.1.0`, plus visible Codex/terminal line `Ran 66 tests OK`, or a clean editor view of changelog `1.1.0`.

Как сделать:

- открыть Telegram Web рядом с Codex;
- отправить `/version` and `/settings`;
- скрыть чаты и личные данные;
- оставить visible version `1.1.0`;
- сделать public crop.

Текст под скриншотом: "Первый стабильный daily-use release: uSugar 1.1.0 закрепляет безопасный OCR-флаг, удаление ошибочных записей, prefilled settings и отдельный backlog будущего развития."

### OCR Milestones

Покрыты: `1.0.17`, `1.0.23`.

Недостаёт: отдельный сильный кадр `1.0.24` with Libre2 CV `8.2`, and possibly `1.0.27` inline confirmation.

Главная story line: "Сначала безопасность, потом распознавание, потом подтверждение, потом метаданные."

### Reminder Milestones

Покрыты: `1.0.30`, `1.0.31`, `1.0.33`, `1.0.34`, `1.0.37`.

Недостаёт: concise screenshot showing `/reminders` status in Telegram after `1.1.0` Russian message cleanup.

Главная story line: "uSugar переходит от реактивных ответов к расписанию и семейной поддержке."

### Trusted Contact

Покрыт: `1.0.35`.

Нужно: в будущем добавить screenshot consent-flow, когда он появится. Сейчас не создавать фейк.

Текст: "Trusted contact показывает семейную природу проекта: бот нужен не только для записи, но и для спокойствия близких."

### Follow-up Reminders

Покрыто: `1.0.37`.

Нужно: позже добавить live Telegram screenshot фактического follow-up message, если он будет публично безопасен.

### AI_CONTEXT

Покрытие: нет визуального скриншота.

Нужен screenshot: editor view with Source of Truth / reading order.

Текст: "После аудитов проект получил карту памяти для человека и ИИ-агента."

### PROJECT_AUDIT

Покрытие: changelog mentions in `1.0.38`, but no screenshot.

Нужен screenshot: `PROJECT_AUDIT.md` headings and maybe file tree.

Текст: "Перед ростом функций проект остановился и описал своё реальное состояние."

### CODE_AUDIT

Покрытие: no screenshot and not central in changelog.

Нужен screenshot: `CODE_AUDIT.md` with sections about working functions, absent functions, and `bot.py` modularization.

Текст: "Code audit separated real behavior from documented wishes and gave the next refactor direction."

### FUTURE_BACKLOG

Покрытие: included in `1.1.0`, no screenshot.

Нужен screenshot: `FUTURE_BACKLOG.md` categories.

Текст: "Future backlog keeps ambition without letting it destabilize the first daily-use release."

## TOP_20_MOST_IMPORTANT_SCREENSHOTS

1. `2026-06-11_stability_release_v1_1_0_working.png` - new screenshot needed. Show v1.1.0, tests OK, `/settings` or `/undo`.
2. `2026-06-11_story_landing_v1_0_39_working.png` - existing. Story Landing as presentation engine.
3. `2026-06-04_initial_state.png` - existing. Origin of the real local project.
4. `2026-06-04_after_docs_and_secret_cleanup.png` - existing. Safety and documentation consolidation.
5. `2026-06-05_runtime_telegram_bot_version_public.png` - existing. First live Telegram proof.
6. `2026-06-05_runtime_telegram_health_public.png` - existing. Safe runtime diagnostics.
7. `2026-06-05_runtime_telegram_formula_reference_public.png` - existing. Formula transparency.
8. `2026-06-05_runtime_telegram_backup_public.png` - existing. User-controlled export.
9. `2026-06-05_process_codex_telegram_backup_side_by_side_public.png` - existing. Real-time AI-assisted development and testing.
10. `2026-06-05_process_codex_telegram_ocr_intake_side_by_side_public.png` - existing. Safe OCR intake begins.
11. `2026-06-05_ocr_pipeline_v1_0_23_running.png` - existing. OCR pipeline implementation.
12. `2026-06-05_libre2_cv_8_2_v1_0_24_public.png` - new screenshot needed. Libre2 CV recognizes a test value.
13. `2026-06-05_bot_brain_v1_0_29_working.png` - existing. First assistant-like reactions.
14. `2026-06-05_reminder_engine_v1_0_30_working.png` - existing. Reminder state engine.
15. `2026-06-05_background_reminders_v1_0_31_working.png` - existing. Background reminder delivery.
16. `2026-06-05_basal_reminders_v1_0_33_working.png` - existing. Basal schedule awareness.
17. `2026-06-05_webapp_reminder_settings_section_v1_0_34_working.png` - existing. User-configurable reminders.
18. `2026-06-06_trusted_contact_settings_v1_0_35_working.png` - existing. Trusted contact.
19. `2026-06-06_followup_settings_v1_0_37_working.png` - existing. Follow-up reminders.
20. `2026-06-11_future_backlog_and_ai_context_v1_1_0_working.png` - new screenshot needed. Show `AI_CONTEXT` and `FUTURE_BACKLOG` as maturity artifacts.

## Recommended Presentation Structure

1. Origin and cleanup.
2. Safe local runtime beside uChurch.
3. First live Telegram bot.
4. Transparent formula and backup.
5. OCR: safe intake to local recognition.
6. Assistant behavior and reminders.
7. Family safety: trusted contact and follow-up.
8. Mobile WebApp and configurable protocol.
9. Documentation maturity: audits, AI context, backlog.
10. Story Landing and 1.1.0 stability release.

## Next Actions

1. Capture a real `1.1.0` stability screenshot.
2. Capture a real `FUTURE_BACKLOG` / `AI_CONTEXT` documentation maturity screenshot.
3. If possible, capture a clean OCR `8.2` recognition screenshot for the `1.0.24` gap.
4. Add these screenshots to `docs/history/SCREENSHOT_STORY.md` later, then regenerate `docs/history/story_data.js`.
5. Do not invent screenshots for early versions `1.0.1`-`1.0.8`; describe them as internal pre-history.
