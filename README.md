# ITMO_data_collect_and_mark

# Лабораторная работа 2 "Качество и чистка данных"
См. файл Lab2FillEmpty.ipynb

## Выводы по работе
Были поставлены задачи:
* Проанализируйте пропуски из датасета ЛР1: определите их процент и расположение (случайны ли пропуски или есть закономерность?)
* Сформулируйте гипотезу о возможных причинах появления пропусков. Проанализируйте датасет на выбросы удобным вам методом
* Примените несколько разных методов работы с пропусками, выбросами.
* Приведите данные в единый вид, поработайте над категориальными признаками.
* Оцените, как каждый из методов повлиял на распределение данных и результаты простого анализа (например, расчёт среднего или простая регрессия).
* Выберите и обоснуйте самый эффективный метод для вашего набора данных.
* Сформулируйте краткую рекомендацию по оптимальному подходу к обработке пропущенных значений в вашем датасете.

**Реализовано:**
Данные проанализированы на наличие пропусков. Они встретились в полях `discount`, `fat_content`, `is_sliced`, `is_bzmj`, `is_creamy`, `discount`, `rating`.
Выявлены причины появления пропусков:
- по признакам с префиксом 'is_', формируемым по наличию ключевых слов в поле name, пропуски связаны с тем, что в наименовании не указано ничего по этому признаку. Пример: наименование `Сыр Российский молодой 45% 200г`. Признак `is_bzmj` на этапе заполнения (в ЛР1) заполняется null, поскольку нет ключевого слова `БЗМЖ`, `is_sliced` - аналогично из-за отсутствия нужных подстрок типа `нарез`, `is_creamy` - нет подстрок `плавлен`. По умолчанию, если это не указано, я не выставляю сразу false в данных, поскольку было замечено, что некоторые продукты в разных магазинах имеют различия в наименовании и то, что не указано БЗМЖ, не означает обратного.
- `discount` и `rating`: до 29.03.2024 эти данные с сайта не собирались. Далее, `discount` не заполняется, если нет скидки на товар (при сборе данных некорректно, если скидка не указана, сохраняется null вместо логически подходящего 0). `rating` может быть не указан, если товар новый и на него нет отзывов.

Выбросы:
- по аналитическим графикам из ЛР1 видно, что выбросов нет. Это связано с небольшим размером датасета и с тем, что данные обновляются в реальном времени, ошибки редко могут возникнуть.

Заполнение пропусков:
- для заполнения пропусков преимущественно был применен способ мэтчинга товаров из разных магазинов для выявления признаков, не пустых в одной строке (товар А из Пятерочки) и пустых в другой (товар Б из Магнита), условно. В случае, когда данных не найдено, данные заполняются средним или медианой

# Лабораторная работа 1 "Введение в сбор данных"
См. файл Lab1StoreScraping.ipynb

## Выводы по работе
Были поставлены задачи:
* Сбор данных из двух разных источников (открытый датасет / веб-скрейпинг или API).
* Агрегация в единый датасет.
* Разведывательный анализ данных (EDA).
* Базовые визуализации для основных признаков с учетом разметки данных.
* Возможные применения этих данных в контексте машинного обучения.
* Описание датасета

**Реализована идея:**

Получить данные о продуктах и ценах на них в разных магазинах, для возможности поиска наиболее выгодных предложений, с извлечением дополнительных признаков из описания товаров и поддержкой гибкого добавления новых источников (магазинов).

**Реализация:**

Был произведен веб-скрейпинг данных с сайтов https://magnit.ru/ (Магнит) и https://5ka.ru/ (Пятерочка). В данной работе производится сбор данных о ценах на товары категории "Сыр", за продолжительное время (в идеале, за каждый день). Данные обработаны, агрегированы и записаны в БД. Получены результаты аналитики и визуализации изменений по дням цен и скидок. Идея для дальнейшей разметки - метка "Бренд" ввиду сложности разработки алгоритма точного выделения этого признака из наименования.

**Применение для машинного обучения:**

Предсказание цены и размера скидки на товар как для временного ряда.
