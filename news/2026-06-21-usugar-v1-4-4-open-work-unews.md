---
type: docs
project: uSugar
series: usugar
title: Карта незавершённых задач и uNewsLog workflow
version: 1.4.4
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-21-usugar-v1-4-4-open-work-unews.png
---

uSugar 1.4.4 фиксирует карту незавершённых задач и делает публикацию патчноутов через uNewsLog обязательной частью закрытия релизов.

Что сделано:
— создан `OPEN_WORK.md` со списком открытых задач по OCR, family mode, daily flow, food, exports и future;
— создан `RELEASE_PLAN.md` с порядком следующих релизов от `1.5.0` до `1.8.0`;
— обновлены правила публикации через uNews: патчноут должен лежать в `news/`, иметь `project: uSugar`, `series: usugar`, безопасную картинку и проходить dry-run/check;
— зафиксировано правило: если check зелёный и нет safety-блокеров, патчноут публикуется в `@uNewsLog`.

Что не менялось:
— медицинская логика;
— OCR-алгоритмы;
— Telegram token;
— база данных;
— runtime-поведение бота.

Статус:
документационный релиз подготовлен как основа для следующего этапа разработки.

Проверки:
— py_compile: OK;
— unit tests: 73 OK;
— uNews dry-run: OK, sendPhoto;
— Telegram smoke: подтверждён вручную после перезапуска runtime на v1.4.4.

Короткий текст для Telegram:
uSugar 1.4.4 фиксирует карту незавершённых задач и вводит обязательную публикацию патчноутов через uNewsLog после закрытия релизов. Медицинская логика, OCR и token не менялись.
