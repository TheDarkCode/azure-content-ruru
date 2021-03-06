<properties 
	pageTitle="Как создать базу данных в DocumentDB | Microsoft Azure" 
	description="Узнайте, как с помощью портала веб-служб для Azure DocumentDB создать базу данных — молниеносно быструю глобальную базу данных NoSQL." 
	keywords="Как создать базу данных" 
	services="documentdb" 
	authors="mimig1" 
	manager="jhubbard" 
	editor="monicar" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/25/2016" 
	ms.author="mimig"/>

# Создание базы данных для DocumentDB на портале Azure

Чтобы использовать Microsoft Azure DocumentDB, необходимы [учетная запись DocumentDB](documentdb-create-account.md), база данных, коллекция и документы. В этом разделе описывается создание базы данных для DocumentDB на портале Microsoft Azure. Сведения о том, как создать базу данных с помощью одного из пакетов SDK см. в разделе [Другие способы создания базы данных DocumentDB](#other-ways-to-create-a-documentdb-database).

1.  На навигационной панели [портала Azure](https://portal.azure.com/) щелкните **DocumentDB (NoSQL)**. Если пункт **DocumentDB (NoSQL)** не отображается, то нажмите кнопку **Другие службы**, а затем выберите **DocumentDB (NoSQL)**.


    ![Снимок экрана, на котором показано, как создать базу данных, и отображены элемент "Учетные записи DocumentDB" в колонке "Обзор" и учетная запись DocumentDB в колонке "Учетные записи DocumentDB".](./media/documentdb-create-database/docdb-database-creation-1-2.png)

2.  В колонке **DocumentDB (NoSQL)** выберите учетную запись, в которую следует добавить базу данных NoSQL DocumentDB. При отсутствии учетных записей необходимо [создать учетную запись DocumentDB](documentdb-create-account.md).

3. В колонке **Учетная запись DocumentDB** щелкните **Добавить базу данных**.

    ![Снимок экрана, на котором показано, как создать базу данных, и отображены кнопка "Добавить базу данных", поле "Идентификатор" и кнопка "OK".](./media/documentdb-create-database/docdb-database-creation-3-5.png)

4. В колонке **Добавление базы данных** введите идентификатор новой базы данных. После проверки имени в поле **идентификатора** отображается зеленая галочка. Нажмите кнопку **ОК**.

    ![Снимок экрана, на котором показано, как создать базу данных, и отображены кнопка "Добавить базу данных", поле "Идентификатор" и кнопка "OK".](./media/documentdb-create-database/docdb-database-creation-4.png)

5. Теперь новая база данных появится в разделе **Базы данных** колонки **Учетная запись DocumentDB**.
 
	![Снимок экрана: новая база данных в колонке "Учетная запись DocumentDB"](./media/documentdb-create-database/docdb-database-creation-6.png)

## Другие способы создания базы данных DocumentDB

Базы данных необязательно создавать на портале: для этого также можно использовать [пакеты SDK для DocumentDB](documentdb-sdk-dotnet.md) или [REST API](https://msdn.microsoft.com/library/mt489072.aspx). Сведения о работе с базами данных с помощью пакета SDK для .NET см. в [примерах базы данных .NET](documentdb-dotnet-samples.md#database-examples). Сведения о работе с базами данных с помощью пакета SDK для Node.js см. в [примерах базы данных Node.js](documentdb-nodejs-samples.md#database-examples).

## Дальнейшие действия

Теперь, когда вы знаете, как создать базу данных для DocumentDB, пора [создать коллекцию](documentdb-create-collection.md).

После создания коллекции можно [добавить документы JSON](documentdb-view-json-document-explorer.md) с помощью обозревателя документов на портале, [импортировать документы](documentdb-import-data.md) в коллекцию с помощью средства миграции данных DocumentDB или использовать один из [пакетов SDK для DocumentDB](documentdb-sdk-dotnet.md) для выполнения операций CRUD. DocumentDB содержит пакеты SDK для .NET, Java, Python, Node.js и JavaScript API. Примеры кода .NET, в которых показано, как создавать, удалять и обновлять документы, см. в [примерах документов .NET](documentdb-dotnet-samples.md#document-examples). Сведения о работе с документами с помощью пакета SDK для Node.js см. в [примерах документов Node.js](documentdb-nodejs-samples.md#document-examples).

После добавления документов в коллекции вы можете использовать [DocumentDB SQL](documentdb-sql-query.md) для [выполнения запросов](documentdb-sql-query.md#executing-sql-queries) для документов с помощью [обозревателя запросов](documentdb-query-collections-query-explorer.md) на портале, интерфейса [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) или одного из [пакетов SDK](documentdb-sdk-dotnet.md).

<!---HONumber=AcomDC_0831_2016-->