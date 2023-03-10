# Тестовое задание MRS
## Структура файла

В файле имеется 2 листа: на листе «Данные – Мерчендайзеры» отражен результат ручного заполнения ответов на вопросы мерчендайзерами, на листе «Данные – Аудиторы» отражен результат заполнения ответов на вопросы аудиторами.

- Дата Посещения – дата посещения торговой точки мерчендайзером и аудитором (мерчандайзер может посещать точку несколько раз в неделю, а член команды аудиторов посещает точку в большинстве случаев 1 раз в месяц)
- Код Торговой Точки – уникальный код торговой точки, зашитый в систему
- Тип Торговой Точки – классификация торговых точек на типы, по большей части строится на базе метража торговых точек
- Вопрос – непосредственно тот вопрос, ответ на который собирается в формате ручного заполнения мерчендайзерами и аудиторами
- Ответ – Мерчендайзеры – ответ на вопрос

### 1. Cопоставить ответы на вопросы мерчендайзеров и аудиторов
Использовать функцию ВПР

### 2. Оценить количество несовпадений в ответах
Использовать функцию IF и условное форматирование

### 3. Оценить долю расхождений в общем массиве ответов
Использовать сводную таблицу

## Вывод
- Количество ответов на вопросы = 88, количество вопросов = 8, количество типов торговых точек = 5
- В 28 случаях ответ мерчендайзера и ответ аудитора не совпадает: это 31.81% (практически треть всех ответов). Это довольно большая доля, с которой необходимо работать и улучшать качество данных
- Больше всего процент несовпадающих ответов в «Супермаркетах B» = 60%
- Больше всего процент несовпадающих ответов на вопрос «Категория товаров для животных примыкает к промо аллее?» = 75%