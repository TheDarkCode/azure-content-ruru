<properties
   pageTitle="Часто задаваемые вопросы о каталоге данных Azure | Microsoft Azure"
   description="Часто задаваемые вопросы о каталоге данных Azure, включая возможности для обнаружения источников данных, создания заметок и управления."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="07/12/2016"
   ms.author="maroche"/>

# Часто задаваемые вопросы о каталоге данных Azure

В этой статье приведены ответы на часто задаваемые вопросы, связанные со службой **каталога данных Microsoft Azure**.

## Вопрос. Что такое каталог данных Azure?

Ответ. Каталог данных Microsoft Azure — это полностью управляемая служба, размещенная в облаке Microsoft Azure, которая выступает в качестве системы регистрации и обнаружения корпоративных источников данных. Возможности каталога данных Azure позволяют любому пользователю — от аналитиков до специалистов по анализу данных и разработчиков — регистрировать, обнаруживать, анализировать и использовать источники данных.

## Вопрос. Какие задачи клиента решает каталог данных Azure?

Каталог данных Azure решает проблему обнаружения источника данных и «темных данных», позволяя пользователям находить и анализировать корпоративные источники данных.

## Вопрос. Кто является целевой аудиторией каталога данных Azure?

Каталог данных в Azure предоставляет возможности как для технических, так и для других специалистов, к которым относятся следующие.

- Разработчики данных и бизнес-аналитики: они отвечают за создание данных и анализ данных, используемых другими пользователями.
-	Менеджеры данных: те, кто имеет набор знаний о данных, о том, что они означают и для какой цели будут использоваться.
- Потребители данных: те, кому необходимо легко обнаруживать, понимать и подключаться к данным, необходимым для работы, с помощью выбранных инструментов.
- Центральный IT-департамент: те, кому необходимо делать сотни источников данных доступными бизнес-пользователям и следить за тем, как и кем используются данные.

## Вопрос. В каких регионах доступен каталог данных Azure?

Службы каталога данных Azure в настоящее время доступны в следующих центрах обработки данных.

- Запад США
- Восток США
- Западная Европа
- Северная Европа
- Восточная часть Австралии
- Юго-Восточная Азия

## Вопрос. Каковы ограничения на количество ресурсов данных в каталоге данных Azure?

Бесплатная версия каталога данных Azure ограничена 5000 зарегистрированных ресурсов данных.

Стандартная версия каталога данных Azure поддерживает до 100 000 зарегистрированных ресурсов данных.

## Вопрос. Какие типы источников данных и ресурсов данных поддерживаются?

Список источников данных, поддерживаемых в настоящее время, см. в разделе [Источники данных, поддерживаемые каталогом данных Azure](data-catalog-dsr.md).

## Вопрос. Как запросить поддержку для другого источника данных?

Запросы на функции и другие отзывы можно отправлять на [форум каталога данных Azure](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## Вопрос. Как начать работу с каталогом данных Azure?

Лучше всего начать со статьи [Начало работы с каталогом данных](data-catalog-get-started.md). Эта статья содержит полный обзор возможностей службы.

## Вопрос. Как зарегистрировать мои данные?

Для регистрации данных в каталоге данных Azure запустите средство регистрации каталога данных Azure в области «Публикация» портала каталога данных Azure. Войдите в приложение публикации каталога данных Azure, используя те же учетные данные, которые использовались для доступа к порталу каталога данных Azure, и выберите источник данных и конкретные ресурсы, которые хотите зарегистрировать.

## Вопрос. Какие свойства извлекаются из регистрируемых ресурсов данных?

Конкретные свойства отличаются для разных источников данных, но в целом служба публикации каталога данных Azure извлекает следующую информацию.

- Имя ресурса
- Тип ресурса
- Описание ресурса
- Имена атрибутов или столбцов
- Типы данных атрибутов или столбцов
- Описание атрибутов или столбцов

> [AZURE.IMPORTANT] При регистрации ресурсов-контейнеров данных в каталоге данных Azure ваши данные не перемещаются и не копируются в облако. При регистрации ресурсов из источника данных метаданные ресурсов копируются в Azure, но данные остаются на прежнем месте источника данных. Единственное исключение из этого правила возникает, если пользователь выбирает передачу записей предварительной версии или профиля данных при регистрации ресурсов-контейнеров. При добавлении записей предварительной версии из каждого ресурса-контейнера копируется до 20 записей, которые сохраняются в виде моментального снимка в каталоге данных Azure. Если добавляется профиль данных, общая информация (например, размер таблиц, процентных значений NULL в столбце и минимальное, максимальное и среднее значения для столбцов) рассчитывается и включается в метаданные, хранящиеся в каталоге.

<br/>

> [AZURE.NOTE] Приложение публикации каталога данных Azure извлечет значение свойства первого класса **Description**, которое есть у таких источников данных, как службы SQL Server Analysis Services. У реляционных баз данных SQL Server нет свойства первого класса **Description**. Поэтому приложение для публикаций каталога данных Azure извлечет значение из расширенного свойства ms\_description для объектов и столбцов. Дополнительные сведения см. в статье TechNet [Использование расширенных свойств объектов базы данных](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).

## Вопрос. Как скоро вновь зарегистрированные ресурсы появляются в каталоге данных Azure?

После регистрации ресурсов-контейнеров в каталоге данных Azure может потребоваться 5–10 секунд, прежде чем они появятся на портале каталога данных Azure.

## Вопрос. Как аннотировать и дополнять метаданные для зарегистрированных ресурсов данных?

Самый простой способ предоставить метаданные зарегистрированным ресурсам-контейнерам — это выбрать ресурс-контейнер на портале каталога данных Azure и ввести значения метаданных для выбранного объекта в области свойств или схем.

Некоторые метаданные, такие как эксперты и теги, также можно указать в процессе регистрации. Значения, указанные в службе публикации каталога данных Azure, будут применены ко всем регистрируемым в этот момент ресурсам-контейнерам. Чтобы добавить заметки к недавно зарегистрированным на портале объектам, нажмите кнопку **Просмотреть портал** на последнем экране приложения публикации каталога данных Azure.

## Вопрос. Как удалить мои зарегистрированные объекты данных?

Чтобы удалить объект из каталога данных Azure, выберите на портале этот объект и нажмите кнопку **Удалить**. Несмотря на то, что метаданные объекта будут удалены из каталога данных Azure, базовый источник данных сохранится.

## Вопрос. Кто такой эксперт?

Эксперт — это лицо, у которого есть обоснованная точка зрения об объекте данных. У объекта может быть несколько экспертов. Эксперт необязательно является «владельцем» объекта; эксперт — это просто опытный пользователь, который знает, как должны использоваться данные.

## Вопрос. Как поделиться информацией с разработчиками каталога данных Azure при возникновении проблем?

Сообщить о проблемах, поделиться информацией или задать вопросы можно на форуме каталога данных Azure. Форум доступен по ссылке http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409.

##Вопрос. Поддерживает ли каталог данных Azure другие источники данных, которые меня интересуют?
Мы ведем активную работу по добавлению дополнительных источников данных в каталог данных Azure. Если вы хотите включить поддержку какого-либо источника данных, предложите свой вариант (или проголосуйте за него, если он уже указан) на [форуме каталога данных Azure](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## Вопрос. Какое отношение каталог данных Azure имеет к каталогу данных в Power BI для Office 365?

Каталог данных Azure можно рассматривать как следующий шаг на пути развития каталога данных. Каталог данных Azure предоставляет похожие возможности для публикации и обнаружения источников данных. Кроме того, в нем доступны более широкие сценарии и отсутствует привязка к Office 365. Вскоре после того, как каталог данных Azure станет общедоступным, оба каталога будут объединены в единую службу.

## Вопрос. Какие разрешения необходимы пользователю для регистрации ресурсов в каталоге данных Azure?

Пользователю, который запускает инструмент регистрации каталога данных Azure, требуется разрешение на чтение метаданных из источника данных. Если пользователь также включает предварительный просмотр, у него также должно быть разрешение на чтение данных из регистрируемых объектов.

## Вопрос. Будет ли каталог данных Azure также доступен для локального развертывания?

Каталог данных Azure — это облачная служба, которая может работать как с облачными, так и с локальными источниками данных. Это гибридное решение для обнаружения источников данных. В настоящее время мы не планируем выпускать локальную версию службы каталога данных Azure.

##Вопрос. Можно ли извлечь дополнительные или более полные метаданные из регистрируемых источников данных?

Мы активно работаем над расширением возможностей каталога данных Azure. Если вы хотите, чтобы была реализована поддержка какого-либо дополнительного типа метаданных, предложите свой вариант (или проголосуйте за него, если он уже есть) на [форуме каталога данных Azure](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). В будущем мы планируем разрешить добавление новых типов источников данных третьим лицам через API расширения.

## Вопрос. Как ограничить видимость зарегистрированных ресурсов данных, чтобы они были доступны для обнаружения только определенным пользователям?

Ответ. Выберите ресурсы данных в каталоге данных Azure и нажмите кнопку «Стать владельцем». Владельцы ресурсов данных в каталоге данных Azure могут изменять параметры видимости, чтобы сделать свои ресурсы доступными всем пользователям каталога или конкретным пользователям.

## Вопрос. Как обновить регистрацию для ресурса данных, так чтобы изменения в источнике данных отражались в каталоге?

Ответ. Для обновления метаданных ресурсов данных, которые уже зарегистрированы в каталоге, просто повторно зарегистрируйте источник данных, содержащий ресурсы. Любые изменения в источнике данных, например добавление и удаление столбцов из таблиц или представлений, будут автоматически отражаться в каталоге, но любые аннотации, сделанные пользователями, будут сохранены.

## Вопрос. Я не нашел ответа на свой вопрос. Что делать?

Перейдите на [форум каталога данных Azure](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Ответы на вопросы, заданные на форуме, будут опубликованы здесь.

<!---HONumber=AcomDC_0713_2016-->