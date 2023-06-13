# Анализ базы резюме из HeadHunter

## Цель проекта
Подготовка [базы резюме](https://drive.google.com/file/d/1i3hAJx1jWZFO3TPiHVi9A4yhE8TaJwlP/view?usp=sharing "Link to database file on GoogleDrive") компании HeadHunter к анализу и построению модели прогнозирования уровня заработной платы соискателей, исходя из информации, которую они указывают о себе.

## Задачи проекта
Реализация поставленной цели предусматривает решение следующих задач:

    1) исследование структуры данных
    2) преобразование данных
    3) выявление зависимостей в данных
    4) очистка данных


## Реализация проекта

### Исследование структуры данных
Исходный DataFrame имеет структуру (44744, 12).
Все столбцы имеют тип данных 'object'. 
В трех признаках присутствуют пропуски: 

    - 'Последняя/нынешняя должность'
    - 'Последнее/нынешнее место работы'
    - 'Опыт работы'.

### Преобразование данных
На этом этапе из комплексных строк, содержащих информацию о соискателях были выделены и добавлены в основной dataframe следующие признаки:

    * 'Образование'
    * 'Пол'
    * 'Возраст'
    * 'Опыт работы (месяц)'
    * 'Город'
    * 'Готовность к командировкам'
    * 'Готовность к переезду'
    * 'ЗП (RUB)' (перерасчет в RUB на [дату](https://drive.google.com/file/d/1QhnbyOEYOYfRrdMhoau0D9KeBEnBpj3A/view?usp=sharing "Data file with exchange rates") обновления резюме)
    * признаки-индикаторы для типов занятости и графика работы

В результате преобразовний основной dataframe имеет следующую структуру:

    + RangeIndex: 44744 entries, 0 to 44743
    + Data columns (total 23 columns)
    + dtypes: bool(12), datetime64[ns](1), float64(2), int64(1), object(7)
    + memory usage: 4.3+ MB

### Выяление зависимости в данных
Для разведывательного анализа использовался метод визуализации средствами Seaborn, Plotly. Исследованы/выявлены зависимости следующих признаков:

    * распределение признака «Возраст»
    * распределение признака «Опыт работы (месяц)»
    * распределение признака «ЗП (руб)» 
    * зависимость медианной желаемой заработной платы («ЗП (руб)») от уровня образования («Образование»)
    * распределение желаемой заработной платы («ЗП (руб)») в зависимости от города («Город»)
    * зависимость медианной заработной платы («ЗП (руб)») от признаков «Готовность к переезду» и «Готовность к командировкам»
    * зависимость медианной желаемой заработной платы от возраста («Возраст») и образования («Образование»)
    зависимость опыта работы («Опыт работы (месяц)») от возраста («Возраст»)

### Очистка данных
        Очистка данных - поиск и удаление выбросов (аномалий) -  наблюдений, существенно выбивающихся из рассматриваемого распределения и являющихся не типичными для выборки.
        Основные статистических методы очистки данных:
            * Метод межквартиального размаха 
            * Метод сигм
На начальном этапе в исследуемом массиве выявлен и удален 161 дубликат.
Три признака содержали пропуски; все пропуски были удалены в столбцах «Последнее/нынешнее место работы», «Последняя/нынешняя должность», тогда как в столбце «Опыт работы (месяц)» пропуски были заполнены медианным значением.
Далее очистка данных осуществлялась в 2 этапа: вруную удалены 89 выбросов по критерию 1e6 < ЗП (RUB) <1000, а также 7 выбросов с аномальным опытом, превышающим возраст соискателей; на втором этапе для очистки данных использовался метод сигм. 

Установлено, что в пределы классического интервала +/- 3 сигмы попадает 99.75% выборки, что практически соответствует ожидаемому значению для нормального распределния. Однако, учитывая ассиметричность графика распределения, отметкой +3 сигмы отсекается большее количество выбросов, чем левой границей (-3 сигмы: 2 из 112). 
При расширении верхней границы до +4 сигмы количество выбросов снижается до трех, в пределах интервала -3..+4 сигмы попадает 99.99% выборки. 

## Использованные инструменты и библиотеки
* numpy 1.24.3
* pandas 2.0.1
* matplotlib 3.7.1
* seaborn 0.12.2
* plotly 5.14.1

## Дополнительные источники:
* [Нормальное распределение](https://ru.wikipedia.org/wiki/Нормальное_распределение)
* [Метод межквартильного размаха](https://recture.ru/common/chto-takoe-pravilo-mezhkvartilnogo-razmaha/)
* [Правило трех сигм](https://wiki.loginom.ru/articles/3-sigma-rule.html)








