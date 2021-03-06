<properties
   pageTitle="Использование средства проверки XML BizTalk в приложениях логики в службе приложений Azure | Microsoft Azure"
   description="Проверка схем с помощью средства проверки XML BizTalk в приложении логики"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajram"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="rajram"/>

# Средство проверки XML BizTalk

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Используйте соединитель средства проверки XML BizTalk в приложении для проверки данных XML на соответствие предопределенным схемам XML. Пользователи могут применить существующие схемы или создавать схемы на основе экземпляра неструктурированного файла, экземпляра JSON или существующих соединителей.

## Использование средства проверки XML BizTalk
Чтобы использовать средство проверки XML BizTalk, сначала необходимо создать соответствующий экземпляр приложения API средства проверки XML BizTalk. Это можно сделать либо в процессе, при создании приложения логики, либо выбрав приложение API средства проверки XML BizTalk на сайте Azure Marketplace.

### Настройка средства проверки XML BizTalk
Средство проверки XML BizTalk принимает схемы в составе своей конфигурации. Пользователи могут запустить колонку настройки приложения API, запустив приложение API непосредственно с портала Azure или дважды щелкнув приложение API в рабочей области конструктора.

![Настройка средства проверки XML BizTalk][1]

В колонке "Приложение API" можно настроить схему, выбрав *Схемы*.

![Часть схемы средства проверки XML BizTalk][2]

Пользователи могут загрузить схемы с диска или создать их на основе экземпляра неструктурированного файла или экземпляра JSON.

![Схемы средства проверки XML BizTalk][3]


### Использование кодировщика неструктурированных файлов BizTalk в рабочей области конструирования
После настройки пользователи могут щелкнуть значок *->* и выбрать действие в списке.

![Список действий средства проверки XML BizTalk][4]

#### Проверка XML

Действие проверки XML проверяет входные XML-данные на соответствие предопределенным схемам.

![Действие "Проверить XML" средства проверки XML BizTalk][5]

Параметр|Тип|Описание параметра
---|---|---
Входной XML-файл|string|Входные XML-данные для проверки

Действие возвращает выходные данные в виде объекта. Выходные данные содержат модель, представляющую ответ средства проверки XML. Она состоит из результата, имени схемы, корневого узла и описания ошибки.


<!-- References -->
[1]: ./media/app-service-logic-xml-validator/XmlValidator.ClickToConfigure.PNG
[2]: ./media/app-service-logic-xml-validator/XmlValidator.SchemasPart.PNG
[3]: ./media/app-service-logic-xml-validator/XmlValidator.SchemaUpload.PNG
[4]: ./media/app-service-logic-xml-validator/XmlValidator.ListOfActions.PNG
[5]: ./media/app-service-logic-xml-validator/XmlValidator.ValidateXml.PNG

<!---HONumber=AcomDC_0803_2016-->