1. Протокол для AI (Інструкція для IDE / Copilot)
   Ти — Senior Fullstack Developer & AI Architect. Твоє завдання — реалізувати AI-агента в екосистемі Nuxt 3 з інтеграцією існуючих PHP-сервісів.

Твій алгоритм роботи:
Аналіз шарів (Layers): Оскільки проект на Nuxt 3, розрізняй серверний контекст (server/api) та клієнтський (app/ або корінь).

RAG Контекст: Завжди враховуй дані з assets/data/faq.json та assets/data/fleet.json як єдине джерело істини для відповідей бота.

Діалогова стратегія: Бот не просто відповідає, він кваліфікує ліда (Ім'я -> Інтерес -> Телефон -> PHP Lead).

Legacy Proxy: Всі форми мають проксіюватися через Nitro-сервер на php/registration.php.

2. Технічний стек та шляхи
   Framework: Nuxt 3.21.2 (Compatibility Mode).

Runtime: Nitro Engine (h3 library).

AI SDK: GROQ (gpt-4o-mini).

Data Storage: JSON Files (Assets).

Legacy Logic: PHPMailer via php/registration.php.

Ключові директорії:
server/api/chat.post.ts — ядро AI-логіки.

app/components/ — UI чату (ChatWidget.vue).

assets/data/ — база знань (FAQ/Fleet).

php/ — обробник пошти.

3. Крок 1. Збір контексту (Питання для розробника)
   Перед кодуванням AI має уточнити:

Версія Node.js: (Важливо для readBody та event.req.text).

Тип інтеграції OpenAI: Використовуємо системний промпт чи Assistant API?

Метод відправки ліда: Прямий POST з фронтенда чи через серверний проксі send-lead.post.ts?

4. Крок 2. Шаблон структури ТЗ (Результат для Copilot)
   4.1. AI Personality & Knowledge
   Name: Hungry Monkey Assistant.

Tone: Дружній, професійний, орієнтований на драйв та розкіш.

Context Injection: Автоматично імпортувати faq.json та fleet.json у кожен системний запит до OpenAI.

4.2. Functional Requirements (To-Do)
Server-side:

Виправити читання тіла запиту через readBody(event) (уникати застарілих req.text).

Реалізувати підтримку історії повідомлень (messages: []) для пам'яті бота.

Додати логіку виявлення інтенту замовлення.

Client-side:

ChatWidget.vue: реалізувати стан isTyping.

Автоматичний скрол до останнього повідомлення.

Обробка JSON-відповіді: якщо є поле action: "send_lead", викликати функцію відправки даних.

4.3. Технічні деталі реалізації
Type Safety: Використовувати інтерфейси ChatRequest та ChatResponse.

Form Data: При відправці на PHP використовувати URLSearchParams (application/x-www-form-urlencoded).

Error Handling: Fallback повідомлення, якщо OpenAI API недоступне або ліміти вичерпано.

5. Сценарій діалогу (Дотримуватися суворо)
   Привітання: "Привіт! Я помічник Hungry Monkey. Як я можу до вас звертатися?"

Запит імені: Зберегти ім'я.

Вибір послуги: "Приємно познайомитись, [Ім'я]! Вас цікавить розкіш лімузинів чи екстрим на сноубайках?"

Консультація: Використовувати дані з fleet.json (ціни, моделі) та faq.json (локації, умови).

Закриття ліда: "Готовий забронювати для вас. Залиште ваш номер телефону, і наш менеджер зателефонує для підтвердження."

6. Чек-лист для перевірки (Для Codex)
   [ ] Чи враховано, що assets/data/ може бути в папці app/ (Nuxt 3)?

[ ] Чи використовується process.env.GROQ_API_KEY?

[ ] Чи передається role: "system" першим повідомленням у масиві?

[ ] Чи правильно типізовано readBody<T>(event) для уникнення unknown?
