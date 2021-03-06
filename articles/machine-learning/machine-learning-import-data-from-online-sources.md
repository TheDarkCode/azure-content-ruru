<properties
	pageTitle="Импорт данных в Студию машинного обучения из сетевых источников данных | Microsoft Azure"
	description="Импорт обучающих данных в Студию машинного обучения Azure из разных сетевых источников."
	keywords="импорт данных, формат данных, типы данных, источники данных, обучающие данные"
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
	ms.date="06/17/2016"
	ms.author="bradsev;garye;gopitk" />


# Импорт данных в Студию машинного обучения Azure из разных сетевых источников данных с помощью модуля "Импорт данных"

В этой статье описывается поддержка импорта сетевых данных из разных источников и приводятся сведения, необходимые для переноса данных из этих источников в эксперимент машинного обучения Azure.

> [AZURE.NOTE] Эта статья содержит общие сведения о модуле [Импорт данных][import-data]. Дополнительные сведения о типах данных, к которым можно получить доступ, форматах, параметрах, а также ответы на часто задаваемые вопросы см. в разделе справки по модулю [Импорт данных][import-data].

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## Введение

В ходе эксперимента с использованием модуля [Импорт данных][import-data] вы можете получить доступ к данным одного из нескольких сетевых источников данных из Студии машинного обучения Azure.

- URL-адрес с использованием HTTP;
- Hadoop с использованием HiveQL
- Хранилище blob-объектов Azure
- таблицу Azure;
- базу данных SQL Azure или сервер SQL Server на виртуальной машине Azure;
- поставщик веб-канала данных (в настоящее время OData).
 
Рабочий процесс, определяющий проведение экспериментов в Студии машинного обучения Azure, представляет собой перетаскивание компонентов на холст. Чтобы получить доступ к сетевым источникам данных, добавьте в эксперимент модуль [Импорт данных][import-data], выберите **источник данных**, а затем укажите параметры, необходимые для доступа к данным. Поддерживаемые сетевые источники данных описаны в таблице ниже. Кроме того, в этой таблице перечислены поддерживаемые форматы файлов и параметры, используемые для доступа к данным.

Обратите внимание, что так как доступ к этим данным для обучения осуществляется во время эксперимента, они доступны только в рамках этого эксперимента. Для сравнения: данные, хранящиеся в модуле набора данных, доступны для любого эксперимента в рабочей области.

> [AZURE.IMPORTANT] В настоящее время модули [Импорт данных][import-data] и [Экспорт данных][export-data] могут читать и записывать только данные в службе хранилища Azure, созданной с помощью классической модели развертывания. Другими словами, новый тип учетной записи хранилища BLOB-объектов Azure, предоставляющий "горячий" или "холодный" уровень доступа к хранилищу, не еще поддерживается.

> Как правило, это не повлияет на учетные записи хранения Azure, созданные до появления данного уровня служб. Если необходимо создать новую учетную запись, выберите **классическую** модель развертывания или используйте Resource Manager и в качестве **типа учетной записи** выберите **Общее назначение**, а не **Хранилище BLOB-объектов**.

> Дополнительные сведения см. в разделе [Хранилище BLOB-объектов Azure: "горячий" и "холодный" уровни хранилища](../storage/storage-blob-storage-tiers.md).



## Поддерживаемые сетевые источники данных
Модуль **Импорт данных** Машинного обучения Azure поддерживает следующие источники данных.

Источник данных | Описание | Параметры |
---|---|---|
URL-адрес с использованием протокола HTTP |Считывает данные в файлах с разделителями-запятыми (CSV), файлах с разделителями-табуляциями (TSV), а также в файлах в формате ARFF и SVM-light из любого URL-адреса, использующего протокол HTTP. | <b>URL-адрес</b>. Задает полное имя файла, включая URL-адрес сайта и имя файла с любым расширением. <br/><br/><b>Формат данных</b>. Задает один из поддерживаемых форматов данных: CSV, TSV, ARFF или SVM-light. Если данные содержат строку заголовков, она используется для назначения имен столбцов.|
Hadoop/HDFS|Считывает данные из распределенного хранилища в Hadoop. Необходимые вам данные можно указать с помощью HiveQL, языка запросов на основе SQL. Используя HiveQL, вы также можете выполнить статистическую обработку и фильтрацию данных перед их добавлением в Студию машинного обучения.|<b>Запрос к базе данных Hive</b>. Задает запрос Hive, используемый для создания данных.<br/><br/><b>Универсальный код ресурса (URI) сервера HCatalog </b>. Указывает имя кластера в формате *&lt;имя\_кластера&gt;.azurehdinsight.net.*<br/><br/><b>Имя учетной записи пользователя Hadoop</b>. Задает имя учетной записи пользователя Hadoop, используемое для подготовки кластера.<br/><br/><b>Пароль учетной записи пользователя Hadoop</b>. Задает учетные данные, используемые при подготовке кластера. Дополнительные сведения см. в статье [Создание кластеров Hadoop в HDInsight](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Расположение выходных данных</b>. Указывает, где хранятся данные: в распределенной файловой системе Hadoop (HDFS) или в Azure. <br/><ul>Если выходные данные хранятся в HDFS, укажите универсальный код ресурса (URI) сервера HDFS. (не забудьте указать имя кластера HDInsight без префикса HTTPS://). <br/><br/>Если выходные данные хранятся в Azure, необходимо указать имя учетной записи хранения Azure, ключ доступа к хранилищу и имя контейнера хранилища.</ul>|
База данных SQL |Считывает данные, хранящиеся в базе данных SQL Azure или в базе данных SQL Server, запущенной на виртуальной машине Azure. | <b>Имя сервера базы данных</b>. Указывает имя сервера, на котором запущена база данных.<br/><ul>Если это База данных SQL Azure, введите имя создаваемого сервера. Обычно оно указывается в таком формате: *&lt;созданный\_идентификатор&gt;.database.windows.net.* <br/><br/>Если это SQL Server, размещенный на виртуальной машине Azure, введите *tcp:&lt;DNS-имя\_виртуальной\_машины&gt;, 1433*</ul><br/><b>Имя базы данных</b>. Задает имя базы данных на сервере. <br/><br/><b>Имя учетной записи пользователя сервера</b>. Задает имя пользователя учетной записи с разрешением на доступ к базе данных. <br/><br/><b>Пароль учетной записи пользователя сервера</b>. Задает пароль для учетной записи пользователя.<br/><br/><b>Принимать любые сертификаты сервера</b>. Установите этот флажок, чтобы пропускать проверку сертификата сайта перед считыванием данных (не рекомендуется).<br/><br/><b>Запрос к базе данных</b>. Введите инструкцию SQL, описывающую данные для чтения.|
Таблица Azure|Считывает данные из службы таблиц в хранилище Azure.<br/><br/>Если вам нечасто требуется считывание больших объемов данных, используйте службу таблиц Azure. Это недорогое и гибкое нереляционное (NoSQL) решение хранилища с высокой степенью масштабируемости и доступности.| Изменение параметров в модуле **Импорт данных** зависит от того, обращаетесь ли вы к общедоступной информации или к закрытой учетной записи хранения, для входа в которую нужны учетные данные. Это определяется параметром <b>Тип проверки подлинности</b>, который может иметь значение PublicOrSAS или Account. Каждое из этих значений имеет собственный набор параметров. <br/><br/><b>Открытый универсальный код ресурса (URI) или URI подписанного URL-адреса (SAS) </b>. Параметры:<br/><br/><ul><b>URI таблицы</b>. Задает открытый URI или URI SAS для таблицы.<br/><br/><b>Указывает строки для поиска имен свойств</b>. Значения: <i>TopN</i> — для просмотра указанного числа строк, <i>ScanAll</i> — для просмотра всех строк в таблице. <br/><br/>В случае с однородными и прогнозируемыми данными рекомендуется выбрать значение *TopN* и ввести число для N. При работе с большими таблицами это может ускорить считывание данных.<br/><br/>Если данные структурированы с использованием наборов свойств, которые зависят от глубины и положения таблицы, выберите вариант *ScanAll*, чтобы просматривать все строки. Это гарантирует целостность полученного свойства и преобразования метаданных.<br/><br/></ul><b>Закрытая учетная запись хранения</b>. Параметры: <br/><br/><ul><b>Имя учетной записи</b>. Задает имя учетной записи, которая содержит таблицу для чтения.<br/><br/><b>Ключ учетной записи</b>. Задает ключ к хранилищу данных, связанный с учетной записью.<br/><br/><b>Имя таблицы</b>. Задает имя таблицы, содержащей данные для чтения.<br/><br/><b>Строки для поиска имен свойств</b>. Значения: <i>TopN</i> — для просмотра указанного числа строк, <i>ScanAll</i> — для просмотра всех строк в таблице.<br/><br/>В случае с однородными и прогнозируемыми данными рекомендуется выбрать значение *TopN* и ввести число для N. При работе с большими таблицами это может ускорить считывание данных.<br/><br/>Если данные структурированы с использованием наборов свойств, которые зависят от глубины и положения таблицы, выберите вариант *ScanAll*, чтобы просматривать все строки. Это гарантирует целостность полученного свойства и преобразования метаданных.<br/><br/>|
Хранилище больших двоичных объектов Azure | Считывает данные, хранящиеся в службе больших двоичных объектов в хранилище Azure, включая изображения, неструктурированные текстовые данные и двоичные данные.<br/><br/>Службу BLOB-объектов можно использовать для предоставления общего доступа к данным или для закрытого хранения данных приложения. Доступ к данным можно получить из любого места, подключившись через протокол HTTP или HTTPS. | Изменение параметров в модуле **Импорт данных** зависит от того, обращаетесь ли вы к общедоступной информации или к закрытой учетной записи хранения, для входа в которую нужны учетные данные. Это определяется параметром <b>Тип проверки подлинности</b>, который может иметь значение PublicOrSAS или Account.<br/><br/><b>Открытый универсальный код ресурса (URI) или URI подписанного URL-адреса (SAS)</b>. Параметры:<br/><br/><ul><b>URI</b>. Задает открытый URI или URI SAS для большого двоичного объекта хранилища.<br/><br/><b>Формат файлов</b>. Задает формат данных в службе BLOB-объектов. Поддерживаемые форматы: CSV, TSV и ARFF.<br/><br/></ul><b>Закрытая учетная запись хранения</b>. Параметры: <br/><br/><ul><b>Имя учетной записи</b>. Задает имя учетной записи, которая содержит большой двоичный объект для чтения.<br/><br/><b>Ключ учетной записи</b>. Задает ключ к хранилищу данных, связанный с учетной записью.<br/><br/><b>Путь к контейнеру, каталогу или большому двоичному объекту</b>. Задает имя большого двоичного объекта, содержащего данные для чтения.<br/><br/><b>Формат файла большого двоичного объекта</b>. Задает формат данных в службе BLOB-объектов. Поддерживаемые форматы данных: CSV, TSV, ARFF, CSV с заданной кодировкой и Excel. <br/><br/><ul>Если используется формат CSV или TSV, обязательно укажите, содержит ли файл строку заголовков.<br/><br/>Формат Excel можно использовать для чтения данных из книг Excel. В параметре <i>Формат данных Excel</i> укажите, где находятся данные: в диапазоне листа Excel или в таблице Excel. В параметре <i>Лист Excel или внедренная таблица</i> укажите имя листа или таблицы для считывания данных.</ul><br/>|
Поставщик веб-канала данных | Считывает данные, получаемые от поддерживаемого поставщика веб-канала. В настоящее время поддерживается только формат Open Data Protocol (OData). | <b>Тип содержимого данных</b>. Задает формат OData.<br/><br/><b>Исходный URL-адрес</b>. Задает полный URL-адрес для веб-канала данных. <br/>Например, следующий URL-адрес считывает данные из образца базы данных Northwind: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/

<!---HONumber=AcomDC_0622_2016-->