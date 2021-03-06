<properties 
	pageTitle="Отслеживание приложений логики в службе приложений Azure | Microsoft Azure" 
	description="Просмотр результатов действий приложений логики" 
	authors="jeffhollan" 
	manager="erikre" 
	editor="" 
	services="logic-apps" 
	documentationCenter=""/>

<tags
	ms.service="logic-apps"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/22/2016"
	ms.author="jehollan"/>

# Мониторинг приложений логики

После [создания приложения логики](app-service-logic-create-a-logic-app.md) вы можете просмотреть полный журнал его выполнения на портале Azure. Вы также можете настроить службы, например систему диагностики Azure и службу оповещений Azure, которые будут отслеживать события в режиме реального времени и предупреждать вас о критических событиях, например если в течение часа более 5 запусков завершаются сбоем.

## Мониторинг на портале Azure

Чтобы просмотреть журнал, выберите **Обзор**, а затем — **Приложения логики**. Отобразится список приложений логики в вашей подписке. Выберите приложение логики, которое требуется отслеживать. Отобразится список всех действий и триггеров, выполненных для этого приложения логики.

![Обзор](./media/app-service-logic-monitor-your-logic-apps/overview.png)

В этой колонке есть несколько полезных разделов:

- **Сводка** — здесь приведены сведения о **всех выполнениях**, а также **журнал триггера**.
	- **Все запуски** — в этой области перечислены последние выполнения приложения логики. Щелкните любую строку, чтобы просмотреть сведения о выполнении, или плитку, чтобы вывести больше выполнений.
	- **Журнал триггера** — в этой области перечислены все действия триггера этого приложения логики. Для действий триггера могут быть указаны такие состояния: "Пропущено" — пропущена проверка на наличие новых данных (например, проверка добавления нового файла к FTP), "Успешно" — данные возвращены для запуска приложения логики, "Сбой" — ошибка в конфигурации.
- **Диагностика** — в этом разделе можно просматривать сведения о среде выполнения и события, а также подписаться на получение [оповещений Azure](#adding-azure-alerts).

### Просмотр сведений о выполнении

В списке выполнений представлены сведения о **состоянии**, **времени начала** и **длительности** конкретного выполнения. Щелкните любую строку, чтобы увидеть сведения об этом конкретном запуске.

В представлении мониторинга показан каждый этап выполнения, входные и выходные данные, а также сообщения о произошедших ошибках.

![Запуск и действия](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Если вам нужны дополнительные сведения, например **идентификатор корреляции** выполнения (который может использоваться для REST API), нажмите кнопку **Подробности о запуске**. Эти подробности включают в себя сведения обо всех шагах, состоянии, а также входных и выходных данных выполнения.

## Система диагностики и оповещения Azure

Помимо упомянутых выше сведений, предоставленных на портале Azure и в REST API, вы можете настроить использование в приложении логики системы диагностики Azure для получения более подробных сведений и выполнения отладки.

1. Щелкните раздел **Диагностика** в колонке приложения логики.
1. Щелкните **Параметры диагностики**, чтобы настроить их.
1. Настройте отправку данных в концентратор событий или учетную запись хранения.

	![Параметры системы диагностики Azure](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### Добавление оповещений Azure

После настройки диагностики вы можете добавить оповещения Azure, которые будут отправляться при превышении определенных пороговых значений. В колонке **Диагностика** выберите плитку **Оповещения**, а затем — команду **Добавить оповещение**. Далее вам будут представлены пошаговые указания по настройке оповещения на основе ряда пороговых значений и данных метрики.

![Метрики оповещений Azure](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

Вы можете настроить необходимые **условие**, **пороговое значение** и **период**. Наконец, вы можете настроить электронный адрес, куда будут отправляться уведомления, или объект webhook. Кроме того, в приложении логики можно использовать [триггер запроса](../connectors/connectors-native-reqres.md), который будет запускаться при оповещении (например, чтобы [опубликовать в Slack](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [отправить текст](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app) или [добавить сообщение в очередь](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)).

### Параметры системы диагностики Azure

Каждое из этих событий содержит сведения о приложении логики и событии, например о состоянии. Ниже приведен пример события *ActionCompleted*:

```javascript
{
			"time": "2016-07-09T17:09:54.4773148Z",
			"workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
			"resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
			"category": "WorkflowRuntime",
			"level": "Information",
			"operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
			"properties": {
				"$schema": "2016-06-01",
				"startTime": "2016-07-09T17:09:53.4336305Z",
				"endTime": "2016-07-09T17:09:53.5430281Z",
				"status": "Succeeded",
				"code": "OK",
				"resource": {
					"subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
					"resourceGroupName": "MyResourceGroup",
					"workflowId": "cff00d5458f944d5a766f2f9ad142553",
					"workflowName": "MyLogicApp",
					"runId": "08587361146922712057",
					"location": "eastus",
					"actionName": "Http"
				},
				"correlation": {
					"actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
					"clientTrackingId": "my-custom-tracking-id"
				},
				"trackedProperties": {
					"myProperty": "<value>"
				}
			}
		}
```

Самые важные свойства для отслеживания и мониторинга — *clientTrackingId* и *trackedProperties*.

#### Идентификатор отслеживания клиента

Идентификатор отслеживания клиента — это значение, по которому будут сопоставляться события в выполнении приложения логики, включая вложенные рабочие процессы, вызываемые как часть этого приложения. Если этот идентификатор не указан, он будет создан автоматически. Вы можете вручную указать его из триггера. Для этого передайте заголовок `x-ms-client-tracking-id` со значением идентификатора в запрос триггера (триггер запроса, HTTP или webhook).

#### Отслеживаемые свойства

Отслеживаемые свойства можно добавить в действия в определении рабочего процесса для отслеживания входных или выходных данных в данных диагностики. Это можно сделать, если в данных телеметрии нужно отслеживать такие данные, как "Идентификатор заказа". Чтобы добавить отслеживаемое свойство, включите свойство `trackedProperties` в действие. Отслеживаемые свойства могут отслеживать входные и выходные данные только одного действия. Однако можно использовать свойства `correlation` событий, чтобы сопоставлять действия в выполнении.

```javascript
{
	"myAction": {
		"type": "http",
		"inputs": {
			"uri": "http://uri",
			"headers": {
				"Content-Type": "application/json"
			},
			"body": "@triggerBody()"
		},
		"trackedProperties":{
			"myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
			"myActionHTTPValue": "@action()['outputs']['body']['foo']",
			"transactionId": "@action()['inputs']['body']['bar']"
		}
	}
}
```

### Расширение решений

Вы можете использовать эти данные телеметрии из концентратора событий или хранилища в других службах, например [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite), [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) и [Power BI](https://powerbi.com), чтобы отслеживать рабочие процессы интеграции в режиме реального времени.

## Дальнейшие действия
- [Распространенные примеры и сценарии использования приложений логики](app-service-logic-examples-and-scenarios.md)
- [Создание шаблона развертывания приложения логики](app-service-logic-create-deploy-template.md)
- [Возможности интеграции Enterprise](app-service-logic-enterprise-integration-overview.md)

<!---HONumber=AcomDC_0803_2016-->