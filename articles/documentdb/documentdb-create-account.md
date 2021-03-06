<properties
	pageTitle="Как создать учетную запись DocumentDB | Microsoft Azure"
	description="Создайте базу данных NoSQL в Azure DocumentDB. Следуя приведенным в статье инструкциям, вы создадите учетную запись DocumentDB и начнете создавать собственную высокопроизводительную глобально масштабируемую базу данных NoSQL." 
	keywords="создание базы данных"
	services="documentdb"
	documentationCenter=""
	authors="mimig1"
	manager="jhubbard"
	editor="monicar"/>

<tags
	ms.service="documentdb"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="09/12/2016"
	ms.author="mimig"/>

# Как создать учетную запись DocumentDB в базе данных NoSQL с помощью портала Azure

> [AZURE.SELECTOR]
- [Портал Azure](documntdb-create-account.md)
- [Azure CLI и Azure Resource Manager](documentdb-automation-resource-manager-cli.md)

Чтобы создать базу данных в Microsoft Azure DocumentDB, вам потребуются:

- Учетная запись Azure. Если у вас ее нет, ее можно [создать бесплатно](https://azure.microsoft.com/free).
- Учетная запись DocumentDB.

Учетную запись DocumentDB можно создать с помощью портала Azure, шаблонов Azure Resource Manager или интерфейса командной строки Azure (Azure CLI). Из этой статьи вы узнаете, как создать учетную запись DocumentDB с помощью портала Azure. Сведения о том, как создать учетную запись с помощью Azure Resource Manager или Azure CLI, см. в статье [Автоматизация создания учетной записи DocumentDB с помощью шаблонов Azure Resource Manager и интерфейса командной строки Azure](documentdb-automation-resource-manager-cli.md).

Не знакомы с DocumentDB? Просмотрите это четырехминутное [видео](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/), в котором Скотт Хансельман (Scott Hanselman) рассказывает, как выполнять наиболее распространенные задачи на веб-портале.

1.	Войдите на [портал Azure](https://portal.azure.com/).
2.	На навигационной панели щелкните **Создать**, выберите **Данные + хранилище**, а затем щелкните **DocumentDB (NoSQL)**.

	![Снимок экрана с порталом Azure, на котором выделены дополнительные службы и DocumentDB (NoSQL)](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-1.png)

3. В колонке **Новая учетная запись** укажите желаемую конфигурацию учетной записи DocumentDB.

	![Снимок экрана с колонкой "Создать DocumentDB"](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-2.png)

	- В поле **Идентификатор** введите имя для идентификации учетной записи DocumentDB. После проверки **идентификатора** в поле **идентификатор** отображается зеленая галочка. Значение **идентификатора** становится именем узла в URI. В **идентификаторе** могут использоваться только строчные буквы, цифры и символ "-". Длина идентификатора должна быть от 3 до 50 символов. Обратите внимание, что к выбранному имени конечной точки добавляется *documents.azure.com*. Итоговое полное имя будет использоваться в качестве имени конечной точки вашей учетной записи DocumentDB.

    - В разделе **API NoSQL** выберите нужную модель программирования.
        - **DocumentDB**. API для DocumentDB предоставляет программный доступ ко всем функциональным возможностям DocumentDB. Этот API доступен в пакетах [SDK](documentdb-sdk-dotnet.md) для .NET, Java, Node.js, Python, JavaScript, а также через HTTP [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx).
       
        - **MongoDB**. В DocumentDB также предлагается поддержка интерфейсов API для **MongoDB** [на уровне протокола](documentdb-protocol-mongodb.md). Выбрав API для MongoDB, вы сможете организовать взаимодействие с DocumentDB с помощью существующих пакетов SDK и [средств](documentdb-mongodb-mongochef.md) для MongoDB. Вы сможете [импортировать](documentdb-import-data.md) существующие приложения MongoDB в базу данных DocumentDB, [не внося изменения в код](documentdb-connect-mongodb-account.md). В этом случае вы получите полностью управляемую базу данных как службу, неограниченные возможности масштабирования, глобальную репликацию и другие преимущества.

	- В поле **Подписка** выберите подписку Azure, которую требуется использовать для учетной записи DocumentDB. Если ваша учетная запись включает только одну подписку, эта учетная запись будет выбрана по умолчанию.

	- В разделе **Группа ресурсов** выберите или создайте группу ресурсов для своей учетной записи DocumentDB. По умолчанию предлагается создать новую группу ресурсов. Дополнительные сведения см. в статье [Управление ресурсами Azure через портал](../articles/azure-portal/resource-group-portal.md).

	- В поле **Расположение** укажите географическое расположение, где будет размещена учетная запись DocumentDB.

4.	Настроив параметры учетной записи DocumentDB, нажмите кнопку **Создать**. Чтобы узнать о состоянии развертывания, просмотрите информацию в концентраторе уведомлений.

	![Быстрое создание баз данных. Снимок экрана с концентратором уведомлений, который сообщает, что создается учетная запись DocumentDB](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-4.png)

	![Снимок экрана с концентратором уведомлений, который сообщает, что учетная запись DocumentDB успешно создана и развернута в группе ресурсов. Уведомление для создателя базы данных в Интернете](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-5.png)

5.	Когда учетная запись DocumentDB будет создана, ее можно будет использовать с параметрами по умолчанию. Согласованность учетной записи DocumentDB по умолчанию настроена на уровне **сеанса**. Чтобы изменить уровень согласованности по умолчанию, выберите в меню ресурсов пункт **Согласованность по умолчанию**. Дополнительные сведения об уровнях согласованности в DocumentDB см. в статье [Уровни согласованности в DocumentDB](documentdb-consistency-levels.md).

    ![Снимок экрана с колонкой "Группа ресурсов". Начало разработки приложения](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-6.png)

    ![Снимок экрана с колонкой "Уровень согласованности". Выбрана согласованность на уровне сеанса](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-7.png)

[How to: Create a DocumentDB account]: #Howto
[Next steps]: #NextSteps
[documentdb-manage]: ../articles/documentdb/documentdb-manage.md


## Дальнейшие действия

Следующий шаг после создания учетной записи DocumentDB — создание базы данных DocumentDB.

Базу данных можно создать одним из нескольких способов.

- С помощью портала Azure. См. статью [Создание базы данных для DocumentDB на портале Azure](documentdb-create-database.md).
- С помощью подробных руководств с примерами данных: [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md), [Python](documentdb-python-application.md).
- С помощью примера кода для [.NET](documentdb-dotnet-samples.md#database-examples), [Node.js](documentdb-nodejs-samples.md#database-examples) или [Python](documentdb-python-samples.md#database-examples) с сайта GitHub.
- С помощью пакетов SDK для [.NET](documentdb-sdk-dotnet.md), [Node.js](documentdb-sdk-node.md), [Java](documentdb-sdk-java.md), [Python](documentdb-sdk-python.md) и [REST](https://msdn.microsoft.com/library/azure/mt489072.aspx).

Создав базу данных, в нее нужно [добавить хотя бы одну коллекцию](documentdb-create-collection.md), а затем в коллекцию [добавить документы](documentdb-view-json-document-explorer.md).

Когда в коллекции появятся документы, к ним можно [отправлять запросы](documentdb-sql-query.md#executing-queries), созданные с помощью [DocumentDB SQL](documentdb-sql-query.md). Запросы можно отправлять через [обозреватель запросов](documentdb-query-collections-query-explorer.md) на портале, с помощью [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) или одного из пакетов [SDK](documentdb-sdk-dotnet.md).

### Подробнее

Дополнительные сведения о DocumentDB можно получить из следующих ресурсов:

-	[Схема обучения для DocumentDB.](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
-	[Иерархическая модель ресурсов и понятия DocumentDB.](documentdb-resources.md)

<!---HONumber=AcomDC_0914_2016-->