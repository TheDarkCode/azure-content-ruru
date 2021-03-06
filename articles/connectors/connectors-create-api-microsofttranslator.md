<properties
    pageTitle="Добавление API Microsoft Translator в PowerApps Enterprise или приложения логики| Microsoft Azure"
    description="Обзор соединителя Microsoft Translator с параметрами интерфейса API REST"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# Начало работы с соединителем Microsoft Translator
Подключитесь к Microsoft Translator, чтобы переводить тексты, определять языки и выполнять многие другие действия. С помощью Microsoft Translator вы можете:

- формировать бизнес-процессы на основе данных, получаемых из Microsoft Translator;
- использовать действия для перевода текста, определения языка и т. д. Эти действия получают ответ и делают выходные данные доступными для использования другими действиями. Например, когда в Dropbox создается файл, с помощью Microsoft Translator можно перевести его текст на другой язык.

Сведения о добавлении операции в приложения логики см. в статье о [создании приложения логики](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Триггеры и действия
Microsoft Translator позволяет выполнять следующие действия. Триггеры отсутствуют.

триггеры; | Действия
--- | ---
None | <ul><li>Определение языка</li><li>Преобразование текста в речь</li><li>Перевод текста</li><li>Получение языков</li><li>Получение языков для синтеза речи</li></ul>

Все соединители поддерживают данные в форматах JSON и XML.


## Создание подключения к Microsoft Translator

>[AZURE.INCLUDE [Шаги по созданию подключения к Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## Справочник по REST API Swagger
Относится к версии 1.0.

### Определение языка    
Определяет исходный язык заданного текста. ```GET: /Detect```

| Имя| Тип данных|Обязательно|Местонахождение|Значение по умолчанию|Description (Описание)|
| ---|---|---|---|---|---|
|запрос|string|Да|запрос|Нет |Текст, язык которого будет определен|

#### Ответ
|Имя|Описание|
|---|---|
|200|ОК|
|по умолчанию|Операция завершилась ошибкой.|


### Преобразование текста в речь    
Преобразует заданный текст в речь в виде звукового потока в формате WAVE. ```GET: /Speak```

| Name (Имя)| Тип данных|Обязательно|Местонахождение|Значение по умолчанию|Описание|
| ---|---|---|---|---|---|
|запрос|string|Да|запрос|Нет |Преобразуемый текст|
|язык|string|Да|запрос|Нет |Код языка для создания речи (например "ru-RU")|

#### Ответ
|Name (Имя)|Описание|
|---|---|
|200|ОК|
|по умолчанию|Операция завершилась ошибкой.|


### Перевод текста    
Переводит текст на указанный язык с помощью Microsoft Translator. ```GET: /Translate```

| Имя| Тип данных|Обязательно|Местонахождение|Значение по умолчанию|Description (Описание)|
| ---|---|---|---|---|---|
|запрос|string|Да|запрос|Нет |Текст для перевода|
|languageTo|string|Да|запрос| Нет|Код целевого языка (например "f")|
|languageFrom|string|Нет|запрос|Нет |Исходный язык. Если не указан, Microsoft Translator попытается определить язык автоматически (например en).|
|category|string|Нет|запрос|общие |Категория перевода (по умолчанию — "Общие")|

#### Ответ
|Имя|Описание|
|---|---|
|200|ОК|
|по умолчанию|Операция завершилась ошибкой.|


### Получение языков    
Извлекает все языки, поддерживаемые Microsoft Translator. ```GET: /TranslatableLanguages```

Для этого вызова параметры отсутствуют.

#### Ответ
|Name (Имя)|Описание|
|---|---|
|200|ОК|
|по умолчанию|Операция завершилась ошибкой.|


### Получение языков для синтеза речи    
Извлекает языки, доступные для синтеза речи. ```GET: /SpeakLanguages```

Для этого вызова параметры отсутствуют.

#### Ответ
|Имя|Описание|
|---|---|
|200|ОК|
|по умолчанию|Операция завершилась ошибкой.|

## Определения объектов

#### Language: языковая модель для переводимых языков Microsoft Translator

|Имя свойства | Тип данных | Обязательно|
|---|---|---|
|Код|string|Нет|
|Имя|string|Нет|


## Дальнейшие действия

[Создайте приложение логики](../app-service-logic/app-service-logic-create-a-logic-app.md).

Вы можете вернуться к [списку интерфейсов API](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png

<!---HONumber=AcomDC_0824_2016-->