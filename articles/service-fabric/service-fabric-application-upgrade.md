<properties
   pageTitle="Обновление приложения Service Fabric | Microsoft Azure"
   description="Эта статья содержит вводные сведения об обновлении приложения Service Fabric, включая выбор режимов обновления и выполнение проверок работоспособности."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/18/2016"
   ms.author="subramar"/>


# Обновление приложения Service Fabric

Приложение Azure Service Fabric представляет собой коллекцию служб. Во время обновления структура служб сравнивает новый [манифест приложения](service-fabric-application-model.md#describe-an-application) с предыдущей версией и определяет, каким службам в приложении требуется обновление. Service Fabric делает это путем сравнения номеров версий в манифестах служб с номерами версий для предыдущей версии. Если служба не была изменена, эта служба не обновляется.

## Обзор последовательных обновлений

При последовательном обновлении приложения обновление выполняется поэтапно. На каждом этапе обновление применяется к определенному подмножеству узлов в кластере, который называется доменом обновления. В результате этого приложение остается доступным в течение всего времени обновления. Во время обновления в кластере может содержаться сочетание старых и новых версий приложения.

По этой причине обе версии должны обладать прямой и обратной совместимостью. Если такая совместимость отсутствует, администратор приложения несет ответственность за поэтапное многофазное обновление для обеспечения доступности. Администратор делает это перед обновлением до финальной версии путем обновления с использованием промежуточной версии приложения, которая будет совместима с предыдущей версией.

Домены обновления указываются в манифесте кластера при его настройке. Не следует полагать, что домены обновления будут получать обновления в определенном порядке. Домен обновления представляет собой логическую единицу, в пределах которой развертывается то или иное приложение. Домены обновления позволяют сохранять высокий уровень доступности служб во время обновления.

Непоследовательные обновления возможны в том случае, если обновление применяется ко всем узлам кластера, что и происходит, если у приложения имеется только один домен обновления. Это не рекомендуется, поскольку служба будет отключена и недоступна в течение всего срока выполнения обновления. Кроме того, Azure не предоставляет никаких гарантий в случаях, когда кластер настраивается только с одним доменом обновления.

## Проверки работоспособности во время обновления

Для обновления необходимо настроить политики работоспособности (могут также использоваться значения по умолчанию). Обновление считается успешным, когда все домены обновления обновлены в пределах заданного времени ожидания, а все домены обновления считаются работоспособными. Работоспособное обновление домена означает, что обновленный домен прошел все проверки работоспособности, указанные в политике работоспособности. Например, политика работоспособности может подразумевать обязательную *работоспособность* всех служб в экземпляре приложения, так как работоспособность определяется Service Fabric.

Политики работоспособности и проверки во время обновлений средствами Service Fabric не опираются на службы и приложения. Это означает, что проверок, зависящих от конкретных служб, не выполняется. Например, для вашей службы может быть задано минимальное требование по пропускной способности. Однако Service Fabric не обладает сведениями, необходимыми, чтобы это проверить, поэтому проверка пропускной способности не будет выполняться в соответствии с заданными в приложениях параметрами. Сведения о выполняемых проверках см. в [статьях о работоспособности](service-fabric-health-introduction.md). Проверки, которые происходят во время обновления, включают проверки того, был ли пакет приложения скопирован правильно, был ли экземпляр запущен и т. д.

Работоспособность приложения фактически представляет собой коллекцию дочерних сущностей приложения. Одним словом, Service Fabric выполняет оценку работоспособности приложения по данным о работоспособности, которые передает само приложение. Она также проверяет работоспособность всех служб приложения таким образом. Service Fabric выполняет дальнейшую проверку работоспособности служб приложения путем статистической обработки данных о работоспособности их дочерних элементов, таких как реплики служб. После того как политика работоспособности приложения удовлетворена, обновление продолжается. При нарушении политики работоспособности обновление приложения считается неудачным.

## Режимы обновления

Мы рекомендуем использовать для обновлений отслеживаемый режим. Он используется наиболее часто. В отслеживаемом режиме обновление выполняется на одном домене обновления. При успешном разрешении всех проверок безопасности (в соответствии с заданной политикой) осуществляется автоматический переход к следующему домену обновления. Если проверки работоспособности завершаются с ошибкой или превышены значения времени ожидания, то выполняется либо откат обновления для домена обновления, либо изменение режима на неотслеживаемый ручной (если во время обновления выбран соответствующий параметр).

При использовании неотслеживаемого ручного режима после каждого обновления домена обновления потребуется вручную запустить обновление на следующем домене обновления. Проверок работоспособности Service Fabric не выполняется. Администратор проверяет работоспособность или состояние перед запуском обновления в следующем домене обновления.

## Блок-схема обновления приложения

Следующая диаграмма помогает понять процесс обновления приложения Service Fabric. В частности, она описывает, как параметры времени ожидания, в том числе *HealthCheckStableDuration*, *HealthCheckRetryTimeout* и *UpgradeHealthCheckInterval*, помогают управлять тем, при каких условиях обновление домена обновления считается успешным или неудачным.

![Процесс обновления приложения структуры служб][image]


## Дальнейшие действия

Пошаговое руководство [Обновление приложения с помощью Visual Studio](service-fabric-application-upgrade-tutorial.md) поможет вам выполнить обновление приложения с помощью Visual Studio.

Пошаговое руководство [Обновление приложения с помощью PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) поможет вам выполнить обновление приложения с помощью PowerShell.

Управление обновлениями приложения осуществляется с помощью [параметров обновления](service-fabric-application-upgrade-parameters.md).

Узнайте, как использовать [сериализацию данных](service-fabric-application-upgrade-data-serialization.md), чтобы обеспечить совместимость обновлений приложения.

[Дополнительные разделы](service-fabric-application-upgrade-advanced.md) содержат сведения о работе с расширенными функциями при обновлении приложения.

Сведения об устранении распространенных проблем при обновлении приложений см. в разделе [Устранение неполадок обновления приложения](service-fabric-application-upgrade-troubleshooting.md).
 


[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png

<!---HONumber=AcomDC_0525_2016-->