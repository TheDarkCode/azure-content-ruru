<properties
	pageTitle="Реконструирование и выбор признаков в машинном обучении Azure | Microsoft Azure"
	description="Объясняются цели выбора и реконструирования признаков и приводятся примеры их роли в совершенствовании данных в процессе машинного обучения."
	services="machine-learning"
	documentationCenter=""
	authors="bradsev"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/31/2016"
	ms.author="zhangya;bradsev" />


# Реконструирование и выбор признаков в машинном обучении Azure

В этом разделе объясняются цели реконструирования и выбора признаков в процессе совершенствования данных в машинном обучении. На примерах, предоставляемых Студией машинного обучения Azure, показаны составляющие этих процессов.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Обучающие данные, используемые в машинном обучении, можно улучшить путем выбора или извлечения признаков из собранных необработанных данных. Пример реконструирования признака в контексте обучения для классификации изображений рукописных символов содержит битовую карту распределения плотности, построенную на основе необработанных данных распределения битов. Эта карта помогает более эффективно находить края символов, чем в случае необработанных данных о распределении.

Реконструированные и выбранные признаки повышают эффективность процесса обучения, который пытается извлечь ключевые сведения, содержащиеся в данных. Они также дают возможность повысить возможности этих моделей, чтобы точнее классифицировать входные данные и более надежно предсказывать нужные результаты. Реконструирование и выбор признаков можно также объединять, чтобы сделать процесс обучения более алгоритмизируемым. Этого можно достичь путем расширения и сокращения числа признаков, необходимых для калибровки или обучения модели. С точки зрения математики выбранные для обучения модели признаки являются минимальным набором независимых переменных, которые определяют структуры в данных и затем успешно прогнозируют результаты.

Реконструирование и выбор признаков является частью большего процесса, который обычно состоит из четырех этапов:

* сбора данных;
* совершенствования представления данных;
* построения модели;
* постобработки.

Реконструирование и выбор признаков относятся к этапу **совершенствования представления данных**. Для наших целей можно выделить три следующих аспекта этого процесса.

* **Предварительная обработки данных**: этот процесс должен гарантировать, что собраны чистые и согласованные данные. Он предусматривает объединение наборов данных, обработку отсутствующих данных, обработку несогласованных данных и преобразование типов данных.
* **Реконструирование признаков**: этот процесс направлен на создание дополнительных признаков на основе соответствующих существующих необработанных признаков и повышение эффективности прогнозирования алгоритма обучения.
* **Выбор признаков**: в этом процессе выбирается ключевое подмножество исходных признаков с целью сокращения размерности задачи обучения.

В этом разделе описываются только аспекты реконструирования признаков и выбора признаков в процессе совершенствования данных. Дополнительную информацию об этапе предварительной обработки данных см. в видеоролике [Предварительная обработка данных в Студии машинного обучения Microsoft Azure](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).


## Создание признаков из данных — реконструирование признаков

Обучающие данные образуют матрицу из примеров (записей или наблюдений, хранимых в строках), каждый из которых имеет набор признаков (переменных или полей, хранящихся в столбцах). Признаки, указанные в схеме эксперимента должны характеризовать закономерности в данных. Несмотря на то, что многие поля необработанных данных можно напрямую включить в набор выбранных признаков, используемых для обучения модели, часто бывает так, что для формирования набора усовершенствованных обучающих данных дополнительные (реконструированные) признаки требуется создавать из признаков в необработанных данных.

Какие признаки нужно создавать для расширения набора данных при обучении модели? Реконструированные признаки, совершенствующие обучение, содержат сведения, которые лучше выделяют закономерности в данных. Новые признаки должны предоставлять дополнительные сведения, нечетко зарегистрированные или не очевидные в исходном или существующем наборе. Это довольно творческий процесс. Обоснованные и эффективные решения часто требуют определенного знания предметной области.

При запуске машинного обучения Azure проще всего понять этот процесс с помощью примеров, которые поставляются в комплекте со Студией. Здесь представлены два примера.

* Пример регрессии [Прогнозирование количества прокатов велосипедов](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) в контролируемом эксперименте с известными целевыми значениями
* Пример классификации интеллектуального анализа текста с использованием [хэширования признаков][feature-hashing]

### Пример 1. Добавление временных признаков для регрессионной модели ###

Воспользуемся экспериментом «Прогнозирование спроса на велосипеды» в студии машинного обучения Azure, чтобы продемонстрировать реконструкцию признаков для задачи регрессии. Цель этого эксперимента — прогноз спроса на велосипеды, то есть количество сдаваемых напрокат велосипедов в конкретный день/месяц/час. «Набор данных по прокату велосипедов UCI» используется в качестве необработанных входных данных. Этот набор данных основывается на реальных данных компании Capital Bikeshare, которая содержит сеть проката велосипедов в Вашингтоне (ОК), США. Набор данных представляет количество сдаваемых напрокат велосипедов в определенный час дня в 2011–2012 гг. и содержит 17379 строки и 17 столбцов. Набор необработанных признаков содержит погодные условия (температура, влажность и скорость ветра) и тип дня (праздник/будний день). Поле для прогнозирования «cnt» — количество сдаваемых напрокат велосипедов в конкретный час, которое меняется в диапазоне от 1 до 977.

Для создания эффективных признаков в обучающих данных строятся четыре регрессионные модели с использованием одного и того же алгоритма, но с четырьмя разными обучающими наборами данных. Четыре набора данных представляют те же необработанные входные данные, но с увеличиваемым номером набора признаков. Эти признаки сгруппированы в четыре категории:

1. А = «погода» + «праздник» + «рабочий день» + «выходной день» для прогнозируемого дня
2. Б = количество велосипедов, которые сданы напрокат в каждый из предыдущих 12 часов
3. В = количество велосипедов, которые были сданы напрокат в каждый из предыдущих 12 дней в течение одного и того же часа
4. Г = количество велосипедов, которые были сданы напрокат в каждую из предыдущих 12 недель в течение одного и того же часа и дня

Помимо набора признаков А, который уже существует в исходных необработанных данных, другие три набора признаков создаются в процессе реконструирования признаков. Набор признаков Б охватывает новейший спрос на велосипеды. Набор признак В спрос на велосипеды в конкретный час. Набор признаков Г охватывает спрос на велосипеды в определенный час и определенный день недели. Четыре набора обучающих данных содержат наборы признаков А, А + Б, А + Б + В и A + Б + В + Г, соответственно.

В эксперименте машинного обучения Azure эти четыре наборы обучающих данных формируются через четыре ветви предварительно обработанного входного набора данных. За исключением крайней слева ветви, каждая из этих ветвей содержит модуль [Выполнить сценарий R][execute-r-script], в котором набор производных признаков (набор признаков Б, В и Г) соответственно реконструируется и добавляется в импортируемый набор данных. На следующем рисунке показан сценарий R, используемый для создания набора признаков Б во второй слева ветви.

![создание признаков](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

Сравнение результатов производительности четырех моделей приведены в таблице ниже. Наилучшие результаты даются набором признаков А + Б + В. Обратите внимание, что частота ошибок уменьшается, когда в обучающие данные включается дополнительный набор признаков. Он подтверждает наше предположение, что набор признаков Б + В предоставляет дополнительные данные для задачи регрессии. Однако добавление набора признаков Г не дает дополнительного сокращения частоты ошибок.

![сравнение результатов](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a> Пример 2. Создание признаков в интеллектуальном анализе текста  

Реконструирование признаков широко применяется в задачах, связанных с интеллектуальным анализом текста, например классификации документов и анализе тональностей. Например, если мы хотим классифицировать документы по нескольким категориям, типичными предположениями являются слова и фразы, которые вероятнее всего есть в одной категории документов, и которые с меньшей вероятностью есть в другой категории. Другими словами, частота распределения слов или фраз может характеризовать разные категории документов. В приложениях интеллектуального анализа текста отдельные части текстового содержимого обычно служат в качестве входных данных, поэтому для создания признаков, связанных с частотой вхождения слова или фразы, необходим процесс реконструирования признаков.

Для выполнения этой задачи вызывается метод **хэширования признаков**, чтобы эффективно превратить произвольные признаки текста в индексы. Вместо того, чтобы сопоставлять каждый признак текста (слова или фразы) для определенного индекса, этот метод работает путем применения хэш-функции к признакам и непосредственного использования их хэш-значений как индексов.

В машинном обучении Azure есть модуль [хэширования признаков][feature-hashing], с помощью которого удобнее создавать признаки этих слов и фраз. На рисунке ниже показан пример использования данного модуля. Входной набор данных содержит два столбца: рейтинг книги от 1 до 5 и содержимое фактической рецензии. Задача этого модуля [хэширования признаков][feature-hashing] состоит в извлечении множества новых признаков, чтобы показать частоту вхождения соответствующих слов в рецензии на определенную книгу. Чтобы использовать этот модуль, необходимо выполнить такие действия.

* Сначала выберите столбец, содержащий входной текст («Col2» в этом примере).
* Затем для Hashing bitsize установите значение 8, означающее, что будет создано 2^8 = 256 признаков. Слова и фразы всего текста будут хэшированы на 256 индексов. Параметр Hashing bitsize меняется в диапазоне от 1 до 31. Слова и фразы с меньшей вероятностью хэшируются с тем же индексом при росте значения параметра.
* После этого задайте для параметра N-grams значение 2. Этот параметр возвращает частоту вхождения униграмм (признаков для каждого отдельного слова) и биграмм (признаков для каждой пары смежных слов) во входном тексте. Параметр N-grams меняется от 0 до 10, определяя максимальное количество последовательно идущих слов, которые включены в признак.  

![Модуль «Хэширование признаков»](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

На следующем рисунке показано, на что похожи эти новые признаки.

![Пример «Хэширование признаков»](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## Фильтрация признаков в данных. Выбор признаков  ##

Выбор признаков — это процесс, который часто применяется для создания обучающих наборов данных для задач прогнозирующего моделирования, например задач классификации или регрессии. Целью этого является выбор подмножества признаков из исходного набора данных, уменьшение его размеров с помощью минимального набора признаков для представления максимального отклонения в данных. Это подмножество признаков является именно тем подмножеством, которое должно быть включено в обучение модели. Выбор признаков служит двум основным целям.

* Во-первых, выбор признаков часто повышает точность классификации, исключая несоответствующие, избыточные или сильно коррелирующие признаки.
* Во-вторых, он сокращает число признаков, что повышает эффективность процесса обучения модели. Это особенно важно для ученик, затратных в плане обучения, таких как вспомогательные векторные машины.

Несмотря на то, что выбор признаков нацелен на сокращение числа признаков в наборе данных, используемых для обучения модели, он зачастую не связан с термином «сокращение размерности». Методы выбора признаков извлекают поднабор из исходных признаков в данных без каких-либо изменений. Методы сокращения размерности используют реконструированные признаки, которые могут преобразовывать исходные признаки и соответственно изменять их. Примеры методов сокращения размерности: анализ главных компонентов, анализ канонических корреляций и сингулярная декомпозиция.

В частности одна из распространенных категорий методов выбора признаков в контролируемом контексте называется «Выбор признаков на основе фильтра». Эти методы применяют статистическую меру для назначения рейтинга каждому признаку путем вычисления корреляции между каждым признаком и целевым атрибутом. Затем признаки ранжируются по рейтингу, который может использоваться, чтобы задать пороговое значение для сохранения или исключения конкретных признаков. Примеры статистических показателей, используемых в этих методах: корреляция Пирсона, взаимная информация и критерий хи-квадрат.

В Студии машинного обучения Azure предусмотрены модули для выбора признаков. Как показано на следующем рисунке, это модули [Выбор признаков на основе фильтра][filter-based-feature-selection] и [Линейный дискриминант Фишера][fisher-linear-discriminant-analysis].

![Пример выбора признаков](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)


Рассмотрим, например, использование модуля [Выбор признаков на основе фильтра][filter-based-feature-selection]. Для удобства мы будем продолжать использовать описанный выше пример интеллектуального анализа текста. Допустим, что мы хотим построить регрессионную модель после создания набор из 256 признаков с помощью модуля [Хэширование признаков][feature-hashing] и что выходная переменная находится в столбце Col1 и представляет собой рейтинги рецензий на книги в диапазоне от 1 до 5. Установив для параметра «Метод количественной оценки» значение «Корреляция Пирсона», параметра «Целевой столбец» — «Col1» и для параметра «Число необходимых признаков» — значение 50. Затем модуль [Выбор признаков на основе фильтра][filter-based-feature-selection] создаст набор данных, содержащий 50 признаков с целевым атрибутом «Col1». На следующем рисунке показан только что описанный процесс этого эксперимента и входные параметры.

![Пример выбора признаков](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

На следующем рисунке показаны результирующие наборы данных. Каждый признак оценивается на основе корреляции Пирсона между ним и целевым атрибутом «Col1». Сохраняются признаки с наибольшей оценкой.

![Пример выбора признаков](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

На следующем рисунке показаны соответствующие оценки для выбранных признаков.

![Пример выбора признаков](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Применяя этот модуль [Выбор признаков на основе фильтра][filter-based-feature-selection], будут выбраны 50 из 256 признаков, поскольку эти признаки максимально коррелируют с целевой переменной «Col1» на основе метода оценки «Корреляция Пирсона».

## Заключение
Реконструирование признаков и выбор признаков — два часто выполняемые действия по подготовке обучающих данных при построении модели машинного обучения. Как правило, реконструирование признаков сначала применяется для создания дополнительных признаков, а затем выполняется на этапе выбора признаков, чтобы исключить несоответствующие, избыточные или сильно коррелирующие признаки.

Обратите внимание, что он не всегда обязателен для выполнения реконструирования или выбора признаков. Его необходимость зависит от собираемых данных, используемого алгоритма и цели эксперимента.

<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

<!---HONumber=AcomDC_0601_2016-->