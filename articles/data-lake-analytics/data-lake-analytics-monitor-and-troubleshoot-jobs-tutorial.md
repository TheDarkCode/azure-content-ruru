<properties 
   pageTitle="Устранение неполадок с заданиями аналитики озера данных Azure с помощью портала Azure | Azure" 
   description="Узнайте, как использовать портал Azure для устранения неполадок с заданиями аналитики озера данных." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# Устранение неполадок с заданиями аналитики озера данных Azure с помощью портала Azure

Узнайте, как использовать портал Azure для устранения неполадок с заданиями аналитики озера данных.

При прохождении этого учебника вы смоделируете проблему с отсутствующим исходным файлом и устраните ее с помощью портала Azure.

**Предварительные требования**

Перед началом работы с этим учебником необходимо иметь следующее:

- **Базовые знания о процедуре выполнения заданий аналитики озера данных**. См. раздел [Начало работы с аналитическим модулем озера данных Azure при помощи портала Azure](data-lake-analytics-get-started-portal.md).
- **Учетная запись аналитики озера данных**. См. раздел [Начало работы с аналитическим модулем озера данных Azure при помощи портала Azure](data-lake-analytics-get-started-portal.md#create-adl-analytics-account).
- **Копия образца данных в учетной записи хранения озера данных по умолчанию**. См. статью [Подготовка образца данных](data-lake-analytics-get-started-portal.md#prepare-source-data)

##Отправка задания аналитики озера данных

Теперь вы создадите задание U-SQL с неправильным именем исходного файла.

**Отправка задания**

1. На портале Azure щелкните **Microsoft Azure** в левом верхнем углу.
2. Щелкните элемент с именем вашей учетной записи аналитики озера данных. Она была закреплена здесь при создании учетной записи. Если учетная запись здесь не закреплена, ознакомьтесь с инструкциями из статьи [Открытие учетной записи аналитики из портала](data-lake-analytics-manage-use-portal.md#access-adla-account).
3. Выберите команду **Создать задание** в верхнем меню.
4. Введите имя задания и следующий скрипт U-SQL:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv1"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/output/SearchLog-from-adls.csv"
        USING Outputters.Csv();

    В скрипте определен исходный файл **/Samples/Data/SearchLog.tsv1**, а должен быть **/Samples/Data/SearchLog.tsv**.
     
5. Щелкните **Отправить задание** наверху. Откроется новая панель сведений о задании. В строке заголовка будет отображено состояние задания. На завершение задания может потребоваться несколько минут. Щелкните **Обновить**, чтобы вывести актуальное состояние.
6. Подождите, пока состояние задания не изменится на **Ошибка**. Задание может иметь состояние **Успешно**, если вы не удалили папку /Samples. См. раздел **Предварительные требования** в начале этого учебника.

Может возникнуть вопрос: почему небольшое задание выполняется так долго. Вспомните — аналитика озера данных предназначена для обработки больших данных. Она отлично справляется с обработкой большого объема данных благодаря своей распределенной архитектуре.

Предположим, вы отправили задание и закрыли портал. В следующем разделе вы научитесь устранять неполадки задания.


## Устранение неполадок задания

В предыдущем разделе вы отправили задание, и оно завершилось сбоем.

**Просмотр всех заданий**

1. На портале Azure щелкните **Microsoft Azure** в левом верхнем углу.
2. Щелкните элемент с именем вашей учетной записи аналитики озера данных. Сводные данные задания отображаются на элементе **Управление заданиями**.

    ![Аналитика озера данных Azure: управление заданиями](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)
    
    Элемент управления заданиями позволяет взглянуть на состояние заданий. Обратите внимание, что здесь имеется задание, завершившееся сбоем.
   
3. Щелкните элемент **Управление заданиями** для просмотра заданий. Задания делятся на три категории: **Выполняется**, **В очереди** и **Завершено**. Задание, завершившееся сбоем, будет отображено в разделе **Завершено**. Оно должно быть первым в списке. При наличии большого числа заданий можно щелкнуть **Фильтр**, чтобы упростить их поиск.

    ![Аналитика озера данных Azure: фильтрация заданий](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)

4. Щелкните невыполненное задание из списка, чтобы открыть сведения о задании в новой колонке:

    ![Аналитика озера данных Azure: невыполненное задание](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)
    
    Обратите внимание на кнопку **Отправить повторно**. После устранения проблемы можно отправить задание повторно.

5. Щелкните выделенную часть на предыдущем снимке экрана, чтобы открыть сведения об ошибке. Отобразится примерно следующий текст:

    ![Аналитика озера данных Azure: сведения о невыполненном задании](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    В нем сообщается, что исходная папка не найдена.
    
6. Щелкните **Дублировать скрипт**.
7. Измените путь **FROM** на:

    /Samples/Data/SearchLog.tsv

8. Щелкните **Отправить задание**.


##См. также

- [Обзор аналитики озера данных Azure](data-lake-analytics-overview.md)
- [Начало работы с аналитикой озера данных Azure с помощью Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Начало работы с аналитикой озера данных Azure и U-SQL с помощью Visual Studio](data-lake-analytics-u-sql-get-started.md)
- [Управление аналитикой озера данных Azure с помощью портала Azure](data-lake-analytics-manage-use-portal.md)

<!---HONumber=AcomDC_0615_2016-->