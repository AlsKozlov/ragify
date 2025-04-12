# Бизнес-анализ: Платформа для создания вопросно-ответных систем на базе RAG  

---

## 1. Организационная структура

**Стейкхолдеры со стороны заказчика:**
- Представители комплаенс-службы (банки, промышленные предприятия)
- Руководители юридических департаментов
- Руководители цифровых направлений / ИТ-директора

**Целевая аудитория проекта:**
- Специалисты по внутреннему контролю и KYC/AML
- Юристы, работающие с нормативными и регуляторными актами
- Операционные сотрудники, которые взаимодействуют с регламентами
- Интеграторы и системные аналитики

**Контакты и вовлечение:**
- Уже проведены предварительные встречи с представителями крупных банков и производственных компаний
- Получен живой интерес к решению со стороны комплаенс-офицеров и руководства

**Ключевая особенность:**
- Платформа ориентирована на **on-premise развёртывание**: заказчики (особенно из банковского и промышленного секторов) требуют полного контроля над инфраструктурой и данными
- Поддержка локальных как локальных LLM (Mistral, LLaMA, ExLlama) и закрытого периметра, так и работа по API
- Возможность кастомизации и расширения под внутренние политики (ВНД, приказы, регламенты)

---

## 2. Бизнес-цель проекта

- Повысить скорость и качество интерпретации сложных регуляторных документов.
- Снизить количество ошибок при применении норм (особенно в сценариях быстрого принятия решений — блокировка подозрительных операций, KYC).
- Сократить нагрузку на юридическую службу, предоставив сотрудникам first-line tool на базе ИИ.
- Повысить уровень автоматизации бизнес-процессов комплаенс-проверок.

---

## 3. Существующие решения

На рынке присутствуют решения от крупных международных компаний (OpenAI, Google, Microsoft), а также от отдельных open-source сообществ. Однако у этих решений есть существенные ограничения:

- **Облачные Big Tech-сервисы** (например, Azure OpenAI, Google Vertex AI) не подходят для многих компаний из-за ограничений по безопасности и политике обработки данных. Эти решения предполагают отправку запросов во внешнее облако, что недопустимо для банков, госкомпаний и промышленности.
- **RAGFlow** — одно из немногих open-source решений с визуальным интерфейсом, но:
  - Плохо справляется с **русским языком**
  - Не может корректно обрабатывать **юридически сложные и формализованные документы** (например, 115-ФЗ, положения ЦБ РФ)
  - Нет качественной поддержки инструкций и цепочек размышлений (Chain-of-Thought), необходимых для юридического домена
- Внутренние справочные системы (например, корпоративные порталы, базы знаний) не обладают семантическим поиском и не способны интерпретировать формулировки нормативных документов


---

## 1.1 Текущая ситуация

**Ресурсы:**
- Есть доступ к GPU fine-tuning и запуска inference
- Доступ к LLM через open-source модели (например, Mistral, Llama), а также локальный deployment

**Данные:**
- Тексты нормативных документов в PDF/DOCX и структурированных HTML-формах
- Возможность получать документы из правовых систем (например, Консультант+ через API)
- Возможно расширение за счёт внутренних политик, приказов, регламентов

**Поддержка:**
- Поддержки не планируется

**Риски:**
| Риск | Митигирующее действие |
|------|------------------------|
| Недостаточное качество данных | Использовать OCR, структурировать документы, использовать semantic chunking |
| Отсутствие явно выраженных закономерностей | Тестировать на заранее отобранных вопросах, использовать human-in-the-loop |
| Проблемы с воспроизводимостью ответов | Использовать проверяемые источники и цепочку RAG с прозрачным log-потоком |

---

## 1.2 Задачи с точки зрения аналитики (Data Mining Goals)

- Построение RAG-модуля с поддержкой:
  - Семантического поиска по chunk-репрезентациям документа (BM25 + Dense Retrieval)
  - Генерации ответа на базе LLM (instruction-tuned)
  - Проверки обоснованности ответа (source highlighting)

**Метрики:**
- Semantic Answer Similarity (например, BLEU, ROUGE, BERTScore)
- точность нахождения релевантных chunk'ов
- User Satisfaction Score (опрос пользователей)
- Precision/Recall по релевантности retrieved документа

**Критерии успешности:**
- **Top-1 Precision** ≥ 0.75
- **Answer Relevance Score** ≥ 4/5 по оценке пользователей
- Среднее время ответа ≤ 2 секунды

---

## 1.3 План проекта (Project Plan)

| Этап | Содержание | Длительность |
|------|------------|--------------|
| 1. Business Understanding | Бизнес-анализ, сбор требований, интервью с юристами | 1 неделя |
| 2. Data Understanding | Исследование доступных документов, аннотирование chunk'ов | 1 неделя |
| 3. Data Preparation | Очистка, парсинг, chunking, генерация семантических embedding'ов | 2 недели |
| 4. Modeling | Обучение retriever, настройка LLM, настройка chain-of-thought и т.д | 2–3 недели |
| 5. Evaluation | Оценка метрик, ручная валидация, UX-тестирование | 1 неделя |
| 6. Deployment | Интеграция с внутренними системами, CI/CD, мониторинг | 1–2 недели |

**Итого:** MVP — за 6–7 недель.  
---
