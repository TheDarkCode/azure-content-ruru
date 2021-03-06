<properties
   pageTitle="Устаревание данных в DocumentDB с использованием времени жизни | Microsoft Azure"
   description="Функция TTL в Microsoft Azure DocumentDB позволяет автоматически удалять документы из системы по прошествии определенного периода времени."
   services="documentdb"
   documentationCenter=""
   keywords="Срок жизни"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# Автоматическое устаревание данных в коллекциях DocumentDB с использованием срока жизни

Приложения могут создавать и хранить большие объемы данных. Некоторые из этих данных, например созданные компьютером данные о событиях, журналы и пользовательские сеансы, имеют ценность только в течение ограниченного периода времени. Когда данных становится больше, чем нужно приложению, их можно безопасно удалять, чтобы снизить потребности приложения в ресурсах хранения.

Свойство TTL (время жизни) в Microsoft Azure DocumentDB позволяет автоматически удалять документы из базы данных по прошествии определенного периода времени. Время жизни можно указать на уровне коллекции, и тогда оно будет использоваться по умолчанию, либо отдельно переопределить для каждого документа. Когда свойство TTL определено (будь то время жизни по умолчанию на уровне коллекции или для конкретного документа), DocumentDB автоматически удаляет документы, время жизни которых с момента их последнего изменения превышает заданное значение.

Срок жизни в DocumentDB отсчитывается в секундах от последнего изменения документа. Для этого используется поле \_ts, которое существует в каждом документе. В поле \_ts отмечается дата и время в формате UNIX-времени. Поле \_ts обновляется при каждом изменении документа.

## Поведение TTL

Функция TTL управляется свойствами TTL на двух уровнях — коллекции и документа. Значения задаются в секундах и отсчитываются от значения \_ts, то есть от момента последнего изменения документа.

 1.  DefaultTTL (TTL по умолчанию) на уровне коллекции:
  * Если свойство отсутствует (или имеет значение NULL), документы не удаляются автоматически.
  
  * Если свойство присутствует и имеет значение -1, документы по умолчанию имеют неограниченный срок жизни.
  
  * Если свойство присутствует и его значением является некоторое число (n), документы удаляются по истечении n секунд после последнего изменения.

 2.  TTL на уровне документов:
  * Свойство применяется только в том случае, если для родительской коллекции установлено значение DefaultTTL.
  
  * Переопределяет значение DefaultTTL, установленное для родительской коллекции.

Когда срок жизни документа истекает (ttl + \_ts >= текущее время сервера), документ отмечается как устаревший. После этого не допускаются никакие действия с этим документом, и он исключается из результатов любых запросов. Позднее документ физически удаляется из системы в фоновом режиме. Это действие не использует [единицы запроса](documentdb-request-units.md) из бюджета коллекции.

Описанную выше логику можно представить в виде следующей таблицы.

| | Свойство DefaultTTL на уровне коллекции отсутствует или не определено | Свойство DefaultTTL на уровне коллекции имеет значение -1 | Свойство DefaultTTL на уровне коллекции имеет значение n|
| ------------- |:-------------|:-------------|:-------------|
| Свойство TTL на уровне документа отсутствует| Переопределение на уровне документа не происходит, так как свойство TTL не применяется ни для коллекции, ни для документа. | Документы в этой коллекции не устаревают. | Документы в этой коллекции устаревают по истечении n секунд. |
| Свойство TTL на уровне документа имеет значение -1 | Переопределение на уровне документа не происходит, так как для коллекции не определено свойство DefaultTTL. Система не учитывает свойство TTL, определенное для документа. | Документы в этой коллекции не устаревают. | Документ с TTL=-1 в этой коллекции никогда не устаревает. Все прочие документы устаревают по истечении n секунд. |
| Свойство TTL на уровне документа имеет значение n | Переопределение на уровне документа не происходит. Система не учитывает свойство TTL, определенное для документа. | Документ с TTL=n устаревает через n секунд. Другие документы наследуют значение интервала -1 и никогда не устаревают. | Документ с TTL=n устаревает через n секунд. Другие документы наследуют интервал n из коллекции. |


## Настройка TTL

По умолчанию срок жизни отключен для всех коллекций DocumentDB и для всех документов.

## Активация TTL

Чтобы включить TTL на уровне коллекции или документов, необходимо определить для коллекции свойство DefaultTTL и задать в качестве его значения ненулевое положительное число или -1. Значение DefaultTTL, равное -1, означает, что по умолчанию все документы будут сохраняться в коллекции в течение неограниченного времени, но служба DocumentDB будет отслеживать наличие в этой коллекции документов, для которых значение по умолчанию переопределено.

## Настройка TTL по умолчанию на уровне коллекции

Вы можете настроить срок жизни по умолчанию для коллекции.

Для этого необходимо ввести ненулевое положительное число, которое будет означать время в секундах, по истечении которого с момента последнего изменения документа (отметка времени \_ts) любой документ в этой коллекции будет считаться устаревшим.

Также можно задать значение по умолчанию -1, что будет означать неограниченное время жизни по умолчанию для всех документов в этой коллекции.

## Настройка TTL на уровне документа

Помимо значения TTL по умолчанию для коллекции, вы можете задать значение TTL на уровне документа. Это значение переопределит значение по умолчанию, установленное для коллекции.

Чтобы задать TTL для документа, необходимо ввести ненулевое положительное число, которое будет означать время в секундах, по истечении которого с момента последнего изменения документа (отметка времени \_ts) этот документ будет считаться устаревшим.

Чтобы задать такой период до устаревания документа, задайте поле TTL для документа.

Если в документе нет поля TTL, для коллекции будет применяться значение по умолчанию.

Если свойство TTL отключено на уровне коллекции, поле TTL в документе будет игнорироваться, пока свойство TTL не будет включено для этой коллекции.


## Продление TTL для существующего документа

Отсчет периода TTL для документа можно сбросить, выполнив для него любую операцию записи. При этом в поле \_ts будет записано текущее время, и отсчет устаревания документа, определенный свойством TTL, начнется заново.

Если вы хотите изменить срок жизни на уровне документа, вы можете обновить поле TTL, как и любое другое редактируемое поле.


## Удаление TTL из документа

Если для документа определено свойство TTL, но необходимость в устаревании документа отпала, вы можете извлечь этот документ, удалить поле TTL и сохранить документ на сервере.

Когда поле TTL удаляется из документа, для него применяется значение по умолчанию, установленное для коллекции.

Чтобы документ не устаревал и к нему не применялось значение TTL для коллекции, следует установить значение TTL, равное -1.


## Отключение TTL

Чтобы полностью отключить функцию TTL для коллекции и остановить фоновый поиск устаревших документов, следует удалить свойство DefaultTTL из коллекции.

Удаление этого свойства и выбор значения -1 имеют разный эффект. Выбор значения -1 означает, что новые документы, добавляемые в коллекцию, будут сохраняться в течение неограниченного времени, но это поведение можно переопределить для конкретных документов в коллекции.

Удаление свойства из коллекции означает, что документы не будут устаревать, даже если для них значение по умолчанию было ранее переопределено.


## Часто задаваемые вопросы

**Сколько стоит использование функции TTL?**

Использование TTL на уровне документа не влечет никаких дополнительных затрат.

**Сколько времени займет удаление документа после истечения срока жизни?**

Когда срок жизни документа истекает (ttl + \_ts >= текущее время сервера), документ отмечается как устаревший. После этого не допускаются никакие действия с этим документом, и он исключается из результатов любых запросов. Документы физически удаляются из системы в фоновом режиме. На это не расходуются единицы запросов из бюджета коллекции.

**Если удаление документов занимает некоторое время, отразится ли это на моей квоте (и счете)?**

Нет, с момента устаревания документов их хранение не оплачивается, а размер таких документов не учитывается при использовании квоты на ресурсы хранилища для коллекции.

**Предусматривает ли использование функции TTL на уровне документа оплату за использование единиц запросов?**

Нет, операции с любым документом в DocumentDB не предусматривают оплату за использование единиц запросов.

**Влияет ли удаление документов на пропускную способность, выделенную для коллекции?**

Нет, обслуживание запросов к коллекции имеет приоритет перед фоновым удалением документов. Включение TTL на уровне документа не влияет на это поведение.

**Как долго будет храниться в коллекции документ после истечения срока жизни?**

Сразу после истечения срока жизни документ становится недоступным. Точное время хранения документа в коллекции до фактического удаления нельзя предусмотреть. Оно зависит от того, когда фоновый процесс получит возможность удалить документ.

**Устаревшие документы удаляются на всех узлах, или согласованность обеспечивается только в долгосрочной перспективе?**

Документ становится недоступным одновременно на всех узлах и во всех регионах.

**Расходуются ли единицы запросов на задачи фонового мониторинга TTL?**

Нет, единицы запросов не расходуются на эти задачи.

**Как часто проверяется истечение срока жизни TTL?**

Для проверки истечения TTL не предусмотрен отдельный фоновый процесс. Серверная служба проверяет TTL при основной обработке запроса, исключая из ответа все устаревшие документы. Удаление физического документа — единственный процесс, который выполняется асинхронно в фоновом режиме. Частота его выполнения определяется наличием единиц запросов, доступных для коллекции.

**Функция TTL применяется только к всему документу, или я могу установить срок жизни для отдельных свойств документа?**

TTL применяется ко всему документу. Если вы хотите, чтобы устаревала только часть документа, мы рекомендуем извлекать эту часть из основного документа в отдельный связанный документ, для которого и применять TTL.

**Есть ли особые требования к индексированию при использовании функции TTL?**

Да. Для коллекции должна быть установлена [политика индексирования](documentdb-indexing-policies.md) со значением Consistent или Lazy. Попытка определить DefaultTTL на уровне коллекции с политикой индексирования None приведет к ошибке. Также ошибкой завершится попытка отключить индексирование коллекции, для которой уже задано свойство DefaultTTL.


## Дальнейшие действия

Дополнительные сведения об Azure DocumentDB см. на странице [*документации*](https://azure.microsoft.com/documentation/services/documentdb/) по этой службе.

<!---HONumber=AcomDC_0720_2016-->