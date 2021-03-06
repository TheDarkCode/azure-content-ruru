<properties
	pageTitle="Все статьи о службе фабрики данных Azure | Microsoft Azure"
	description="Таблица всех статей о службе фабрики данных Azure, расположенных на сайте http://azure.microsoft.com/documentation/articles/, с заголовками и описаниями."
	services="data-factory"
	documentationCenter=""
	authors="spelluru"
	manager="paulettm"
	editor=""/>

<tags
	ms.service="data-factory"
	ms.workload="data-factory"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/21/2016"
	ms.author="spelluru"/>


# Все статьи о службе фабрики данных Azure

Ниже перечислены все статьи, относящиеся к службе **фабрики данных Azure**. На этой веб-странице с помощью клавиш **Ctrl+F** можно искать интересующие вас разделы по ключевым словам.




## Создать

| &nbsp; | Название | Описание |
| --: | :-- | :-- |
| 1 | [Создание первой фабрики данных Azure с помощью REST API фабрики данных](data-factory-build-your-first-pipeline-using-rest-api.md) | В этом руководстве вы создадите образец конвейера фабрики данных Azure с помощью REST API фабрики данных. |
| 2 | [Руководство. Создание конвейера с действием копирования с помощью REST API](data-factory-copy-activity-tutorial-using-rest-api.md) | С помощью этого руководства вы, используя REST API, создадите конвейер фабрики данных Azure с действием копирования. |
| 3 | [Мастер копирования фабрики данных](data-factory-copy-wizard.md) | Из этой статьи вы узнаете, как с помощью мастера копирования фабрики данных копировать данные из поддерживаемых источников данных в приемники. |
| 4\. | [Шлюз управления данными](data-factory-data-management-gateway.md) | Настройка шлюза данных для перемещения данных между локальными узлами и облаком. Используйте шлюз управления данными в фабрике данных Azure для перемещения данных. |
| 5 | [Перемещение данных из локальной базы данных Cassandra с помощью фабрики данных Azure](data-factory-onprem-cassandra-connector.md) | Узнайте, как перемещать данные из локальной базы данных Cassandra с помощью фабрики данных Azure. |
| 6 | [Перемещение данных из MongoDB с помощью фабрики данных Azure](data-factory-on-premises-mongodb-connector.md) | Узнайте, как перемещать данные из базы данных MongoDB с использованием фабрики данных Azure. |
| 7 | [Перемещение данных из Salesforce с помощью фабрики данных Azure](data-factory-salesforce-connector.md) | Узнайте, как перемещать данные из Salesforce с использованием фабрики данных Azure. |


## Обновленные статьи

В этом разделе перечислены статьи, в которые недавно были внесены важные изменения. Для каждой из этих статей приводится грубо отформатированный фрагмент добавленного текста. Изменения в статьи были внесены с **26.07.2016** по **21.08.2016**.

| &nbsp; | Статья | Фрагмент обновленного текста |
| --: | :-- | :-- |
| 8 | [Создание, отслеживание фабрик данных Azure и управление ими с помощью пакета .NET SDK фабрики данных](data-factory-create-data-factories-programmatically.md) | **Вход без всплывающего диалогового окна** Приведенный выше образец кода запускает диалоговое окно для ввода учетных данных Azure. Если требуется выполнить вход программным образом без применения диалогового окна, см. раздел "Проверка подлинности субъекта-службы в Azure Resource Manager" (resource-group-authenticate-service-principal.md authenticate-service-principal-with-certificate---powershell). **Пример** Создайте метод GetAuthorizationHeaderNoPopup, как показано ниже. public static string GetAuthorizationHeaderNoPopup() { var authority = new Uri(new Uri("https://login.windows.net"), ConfigurationManager.AppSettings "ActiveDirectoryTenantId" ); var context = new AuthenticationContext(authority.AbsoluteUri); var credential = new ClientCredential(ConfigurationManager.AppSettings "AdfClientId" , ConfigurationManager.AppSettings "AdfClientSecret" ); AuthenticationResult result = context.AcquireTokenAsync(ConfigurationManager.AppSettings "WindowsManagementUri" , credential).Result; |
| 9 | [Перемещение данных с помощью действия копирования](data-factory-data-movement-activities.md) | **Поддерживаемые форматы файлов** Действие копирования может копировать файлы "как есть" из одного файлового хранилища данных в другое (это такие хранилища, как большой двоичный объект Azure, файловая система и распределенная файловая система Hadoop (HDFS)). Для этого нужно пропустить раздел формата (data-factory-create-datasets.md) в определениях наборов входных и выходных данных. Это позволяет эффективно копировать данные без какой-либо сериализации или десериализации. Кроме того, действие копирования позволяет читать файлы таких форматов (и делать в них записи): текстовый формат, AVRO, ORC и JSON. Вот некоторые примеры действий копирования, доступных для вас: / копирование данных в текстовом формате (CSV) из большого двоичного объекта Azure и запись в SQL Azure; / копирование файлов в текстовом формате (CSV) из локальной файловой системы и запись в большой двоичный объект Azure в формате AVRO; / копирование данных в базе данных SQL Azure и запись в локальную систему HDFS в формате ORC, **.a name="global"../a.Глобально доступное перемещение данных** Хотя фабрика данных Azure доступна только в западной и восточной части США, а также в Северной Европе, служба, на базе которой реализовано действие копирования, доступна в указанных ниже географических регионах по всему миру.|
| 10 | [Перемещение данных из источника OData с помощью фабрики данных Azure](data-factory-odata-connector.md) | **Использование проверки подлинности Windows при получении доступа к локальному источнику OData**{ "name": "inputLinkedService", "properties": { "type": "OData", "typeProperties": { "url": ".endpoint of on-premises OData source e.g. Dynamics CRM.", "authenticationType": "Windows", "username": "domain\\user", "password": "password", "gatewayName": "mygateway" } } } |





## Учебники

| &nbsp; | Название | Description (Описание) |
| --: | :-- | :-- |
| 11 | [Учебник. Создание первого конвейера для обработки данных с помощью кластера Hadoop](data-factory-build-your-first-pipeline.md) | В этом руководстве по фабрике данных Azure объясняется, как создать и запланировать фабрику данных, которая обрабатывает данные с помощью сценария Hive в кластере Hadoop. |
| 12 | [Tutorial: Build your first Azure data factory using Azure Resource Manager template](data-factory-build-your-first-pipeline-using-arm.md) | Работая с этим руководством, вы создадите образец конвейера фабрики данных Azure с помощью шаблона диспетчера ресурсов Azure. |
| 13\. | [Создание первой фабрики данных Azure с помощью портала Azure и редактора фабрики данных](data-factory-build-your-first-pipeline-using-editor.md) | В этом руководстве вы создадите образец конвейера фабрики данных Azure с помощью редактора фабрики данных на портале Azure. |
| 14 | [Начало работы с фабрикой данных Azure с помощью Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md) | В этом руководстве вы создадите образец конвейера фабрики данных Azure с помощью Azure PowerShell. |
| 15 | [Начало работы с фабрикой данных Azure с помощью Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) | В этом руководстве вы создадите образец конвейера фабрики данных Azure с помощью Visual Studio. |
| 16 | [Учебник. Создание конвейера с действием копирования с помощью редактора фабрики данных](data-factory-copy-activity-tutorial-using-azure-portal.md) | В этом руководстве вы создадите конвейер фабрики данных Azure с действием копирования при помощи редактора фабрики данных на портале Azure. |
| 17 | [Учебник. Создание конвейера с действием копирования с помощью Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) | В этом руководстве вы создадите конвейер фабрики данных Azure с действием копирования с помощью Azure PowerShell. |
| 18 | [Учебник. Создание конвейера с действием копирования с помощью Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) | С помощью этого руководства вы, используя Visual Studio, создадите конвейер фабрики данных Azure с действием копирования. |
| 19 | [Копирование данных из хранилища BLOB-объектов Azure в базу данных SQL с помощью фабрики данных](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | В этом учебнике рассказывается, как использовать действие копирования в конвейере фабрики данных Azure для копирования данных из хранилища BLOB-объектов в базу данных SQL. |
| 20 | [Руководство. Создание конвейера с действием копирования с помощью мастера копирования фабрики данных](data-factory-copy-data-wizard-tutorial.md) | С помощью этого учебника вы создадите конвейер фабрики данных Azure с действием копирования при помощи мастера копирования, поддерживаемого фабрикой данных. |



## Перемещение данных

| &nbsp; | Название | Описание |
| --: | :-- | :-- |
| 21 | [Перемещение данных в большой двоичный объект Azure и из него с помощью фабрики данных Azure](data-factory-azure-blob-connector.md) | Как копировать данные BLOB-объектов в фабрике данных Azure. Используйте наш пример: как копировать данные в базу данных SQL Azure и хранилище BLOB-объектов Azure и обратно. |
| 22 | [Перемещение данных в озеро данных Azure и обратно с помощью фабрики данных Azure](data-factory-azure-datalake-connector.md) | Узнайте, как с помощью фабрики данных Azure перемещать данные в озеро данных Azure и обратно |
| 23 | [Перемещение данных в хранилище данных DocumentDB и из него с помощью фабрики данных Azure](data-factory-azure-documentdb-connector.md) | Узнайте, как переместить данные в коллекцию Azure DocumentDB и из нее с помощью фабрики данных Azure. |
| 24 | [Перемещение данных в базу данных SQL Azure и из нее с помощью фабрики данных Azure](data-factory-azure-sql-connector.md) | Узнайте, как переместить данные в базу данных SQL Azure и из нее с помощью фабрики данных Azure. |
| 25 | [Перемещение данных в хранилище данных Azure SQL и из него с помощью фабрики данных Azure](data-factory-azure-sql-data-warehouse-connector.md) | Узнайте, как переместить данные в хранилище данных SQL Azure и из него с помощью фабрики данных Azure. |
| 26 | [Перемещение данных в таблицу SQL Azure и из нее с помощью фабрики данных Azure](data-factory-azure-table-connector.md) | Узнайте, как переместить данные в таблицу SQL Azure и из нее с помощью фабрики данных Azure. |
| 27 | [Руководство по настройке производительности действия копирования](data-factory-copy-activity-performance.md) | Узнайте о ключевых факторах, которые влияют на производительность перемещения данных в фабрике данных Azure с использованием действия копирования. |
| 28 | [Перемещение данных с помощью действия копирования](data-factory-data-movement-activities.md) | Дополнительные сведения о перемещении данных в конвейерах фабрики данных: перенос данных между облачными хранилищами, а также между локальным и облачным хранилищами. Использование действия копирования. |
| 29 | [Заметки о выпуске шлюза управления данными](data-factory-gateway-release-notes.md) | Заметки о выпуске шлюза управления данными |
| 30 | [Перемещение данных из локальной системы HDFS с помощью фабрики данных Azure](data-factory-hdfs-connector.md) | Узнайте, как перемещать данные из локальной системы HDFS с помощью фабрики данных Azure. |
| 31 | [Мониторинг конвейеров фабрики данных Azure и управление ими с помощью нового приложения по мониторингу и управлению](data-factory-monitor-manage-app.md) | Узнайте, как отслеживать фабрики данных и конвейеры Azure и управлять ими с помощью приложения по мониторингу и управлению. |
| 32 | [Перемещение данных между локальными источниками и облаком с помощью шлюза управления данными](data-factory-move-data-between-onprem-and-cloud.md) | Настройка шлюза данных для перемещения данных между локальными узлами и облаком. Используйте шлюз управления данными в фабрике данных Azure для перемещения данных. |
| 33 | [Перемещение данных из источника OData с помощью фабрики данных Azure](data-factory-odata-connector.md) | Узнайте, как перемещать данные из источников OData с помощью фабрики данных Azure. |
| 34 | [Перемещение данных из хранилищ данных ODBC с помощью фабрики данных Azure](data-factory-odbc-connector.md) | Узнайте, как перемещать данные из хранилищ данных ODBC с помощью фабрики данных Azure. |
| 35 | [Перемещение данных из DB2 с помощью фабрики данных Azure](data-factory-onprem-db2-connector.md) | Узнайте, как перемещать данные из базы данных DB2 с использованием фабрики данных Azure |
| 36 | [Перемещение данных в локальную файловую систему или из нее с помощью фабрики данных Azure](data-factory-onprem-file-system-connector.md) | Узнайте, как переместить данные в локальную базу данных или из нее c помощью фабрики данных Azure. |
| 37 | [Перемещение данных из MySQL с помощью фабрики данных Azure](data-factory-onprem-mysql-connector.md) | Узнайте, как перемещать данные из базы данных MySQL с использованием фабрики данных Azure. |
| 38 | [Перемещение данных в локальную базу данных Oracle и обратно с помощью фабрики данных Azure](data-factory-onprem-oracle-connector.md) | Узнайте, как переместить данные в локальную базу данных Oracle или из нее с помощью фабрики данных Azure. |
| 11,9 | [Перемещение данных из PostgreSQL с помощью фабрики данных Azure](data-factory-onprem-postgresql-connector.md) | Узнайте, как перемещать данные из базы данных PostgreSQL с использованием фабрики данных Azure |
| 40 | [Перемещение данных из Sybase с помощью фабрики данных Azure](data-factory-onprem-sybase-connector.md) | Узнайте, как перемещать данные из базы данных Sybase с использованием фабрики данных Azure. |
| 41 | [Перемещение данных из Teradata с помощью фабрики данных Azure](data-factory-onprem-teradata-connector.md) | Сведения о соединителе Teradata для службы фабрики данных, с помощью которого можно перемещать данные из базы данных Teradata. |
| 42 | [Перемещение данных в базу данных SQL Server и обратно на локальных компьютерах и виртуальных машинах Azure IaaS с помощью фабрики данных Azure](data-factory-sqlserver-connector.md) | Узнайте, как с помощью фабрики данных Azure перемещать данные в базу данных SQL Server и обратно на локальных компьютерах и виртуальных машинах Azure. |
| 43 | [Перемещение данных из источника веб-таблицы с помощью фабрики данных Azure](data-factory-web-table-connector.md) | Узнайте, как перемещать данные из локальной таблицы на веб-странице с помощью фабрики данных Azure. |



## Преобразование данных

| &nbsp; | Название | Description (Описание) |
| --: | :-- | :-- |
| 44 | [Создание прогнозных конвейеров с помощью действий машинного обучения Azure](data-factory-azure-ml-batch-execution-activity.md) | Описывается, как создавать прогнозирующие конвейеры с помощью фабрики данных Azure и машинного обучения Azure. |
| 45 | [Связанные службы вычислений](data-factory-compute-linked-services.md) | Сведения о средах вычислений, которые можно использовать в конвейерах фабрики данных Azure для преобразования и обработки данных. |
| 46 | [Обработка больших наборов данных с помощью фабрики данных и пакетной службы](data-factory-data-processing-using-batch.md) | Содержит описание процесса обработки больших объемов данных в конвейере фабрики данных Azure с использованием возможности параллельной обработки пакетной службы Azure. |
| 47 | [Сведения о преобразовании и анализе данных в фабрике данных Azure](data-factory-data-transformation-activities.md) | Сведения о преобразовании данных в фабрике данных Azure. Преобразование и обработка данных в кластере Azure HDInsight или в пакетной службе Azure. |
| 48 | [Потоковая активность Hadoop](data-factory-hadoop-streaming-activity.md) | Узнайте, как с помощью действия потоковой передачи Hadoop в фабрике данных Azure выполнять программы потоковой передачи Hadoop в собственном кластере HDInsight или кластере по требованию. |
| 49 | [Действие Hive](data-factory-hive-activity.md) | Узнайте, как с помощью действия Hive в фабрике данных Azure выполнять запросы Hive к кластеру HDInsight по требованию или собственному кластеру HDInsight. |
| 50 | [Вызов программы MapReduce из фабрики данных](data-factory-map-reduce.md) | Узнайте, как обрабатывать данные путем выполнения программ MapReduce в кластере Azure HDInsight из фабрики данных Azure. |
| 51 | [Действие Pig](data-factory-pig-activity.md) | Узнайте, как с помощью действия Pig в фабрике данных Azure выполнять запросы Pig к собственному кластеру HDInsight или к кластеру HDInsight по требованию. |
| 52 | [Вызов программ Spark из фабрики данных](data-factory-spark.md) | Узнайте, как вызывать программы Spark из фабрики данных Azure с помощью действия MapReduce. |
| 53 | [Действие "Хранимая процедура SQL Server"](data-factory-stored-proc-activity.md) | Узнайте, как с помощью действия хранимой процедуры SQL Server можно вызвать хранимую процедуру в Базе данных SQL Azure или хранилище данных SQL Azure из конвейера фабрики данных. |
| 54 | [Использование настраиваемых действий в конвейере фабрики данных Azure](data-factory-use-custom-activities.md) | Узнайте, как создавать пользовательские действия и использовать их в конвейере фабрики данных Azure. |
| 55 | [Запуск скрипта U-SQL в аналитике озера данных Azure из фабрики данных Azure](data-factory-usql-activity.md) | Узнайте, как обрабатывать данные с помощью скриптов U-SQL в службе вычислений аналитики озера данных Azure. |



## Примеры

| &nbsp; | Название | Description (Описание) |
| --: | :-- | :-- |
| 56 | [Фабрика данных Azure — примеры](data-factory-samples.md) | Подробное описание примеров, поставляемых со службой фабрики данных Azure. |



## Варианты использования

| &nbsp; | Название | Описание |
| --: | :-- | :-- |
| 57 | [Примеры реальных клиентов](data-factory-customer-case-studies.md) | Узнайте о том, как некоторые из наших клиентов используют фабрику данных Azure. |
| 58 | [Вариант использования: профилирование клиентов](data-factory-customer-profiling-usecase.md) | Узнайте, как с помощью фабрики данных Azure создать управляемый данными рабочий процесс (конвейер) для профилирования игроков. |
| 59 | [Вариант использования: система рекомендации товаров](data-factory-product-reco-usecase.md) | Сведения о решении, в котором используется фабрика данных Azure и другие службы. |



## Отслеживание и управление

| &nbsp; | Название | Описание |
| --: | :-- | :-- |
| 60 | [Мониторинг конвейеров фабрики данных Azure и управление ими](data-factory-monitor-manage-pipelines.md) | Узнайте, как с помощью портала Azure и Azure PowerShell отслеживать состояние созданных конвейеров и фабрик данных Azure и управлять ими. |



## SDK

| &nbsp; | Название | Описание |
| --: | :-- | :-- |
| 61 | [Фабрика данных Azure — журнал изменений в .NET API](data-factory-api-change-log.md) | Описываются критические изменения, добавления функций, исправления ошибок и т. д. — для определенной версии API для .NET для фабрики данных Azure. |
| 62 | [Создание, отслеживание фабрик данных Azure и управление ими с помощью пакета .NET SDK фабрики данных](data-factory-create-data-factories-programmatically.md) | Узнайте, как программным способом создавать, отслеживать и контролировать фабрики данных Azure с помощью пакета SDK фабрики данных. |
| 63 | [Справочник разработчика фабрики данных Azure](data-factory-sdks.md) | Узнайте о разных способах создания, отслеживания фабрик данных Azure и управления ими. |



## Разное

| &nbsp; | Название | Description (Описание) |
| --: | :-- | :-- |
| 64 | [Фабрика данных Azure — часто задаваемые вопросы](data-factory-faq.md) | Часто задаваемые вопросы о фабрике данных Azure. |
| 65 | [Фабрика данных Azure — функции и системные переменные](data-factory-functions-variables.md) | Содержит список функций и системных переменных фабрики данных Azure. |
| 66 | [Фабрика данных Azure — правила именования](data-factory-naming-rules.md) | Описывает правила именования для сущностей фабрик данных. |
| 67 | [Устранение неполадок фабрики данных](data-factory-troubleshoot.md) | Узнайте, как устранять неполадки при использовании фабрики данных Azure. |

<!---HONumber=AcomDC_0907_2016-->