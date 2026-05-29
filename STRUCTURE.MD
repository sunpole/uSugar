usugar/
├── bot.py                # пустой запуск
├── config.py             # загрузка .env
├── handlers/
│   ├── photo.py
│   ├── commands.py
│   ├── food.py
│   └── fsm.py
├── services/
│   ├── ocr_service.py    # сюда ИИ напишет тройной OCR
│   ├── formula_service.py
│   ├── journal_service.py
│   └── reminder_service.py
├── db/
│   ├── models.py
│   └── database.py
├── states/
│   └── user_states.py
├── utils/
│   ├── logger.py
│   └── helpers.py
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md