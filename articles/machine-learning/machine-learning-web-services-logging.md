<properties 
	pageTitle="Ведение журналов для веб-служб машинного обучения | Microsoft Azure" 
	description="Узнайте, как включить функцию ведения журналов для веб-служб машинного обучения. Функция ведения журналов предоставляет дополнительные сведения по устранению неполадок API-интерфейсов." 
	services="machine-learning" 
	documentationCenter="" 
	authors="raymondlaghaeian" 
	manager="paulettm" 
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="big-data" 
	ms.date="08/09/2016"
	ms.author="raymondl;garye"/>

#Включение функции ведения журналов для веб-служб машинного обучения  

В этом документе представлена информация о возможности ведения журналов веб-служб Azure ML. Включение ведения журналов для веб-служб позволяет получать дополнительную информацию, которая поможет при устранении неисправностей в API гораздо более эффективно, чем при простом выводе номера ошибки и сообщения о ней.

-	Как включить ведение журнала веб-службы
	-	Выполните вход на [классический портал Azure](https://manage.windowsazure.com/).
	-	Щелкните "Машинное обучение" и вашу рабочую область, а затем выберите пункт меню "Веб-служба".
	-	В списке веб-служб щелкните имя веб-службы.
	-	В списке конечных точек щелкните имя конечной точки.
	-	Выберите пункт меню "Настройка".
	-	Установите уровень трассировки диагностики "Ошибка" или "Все", а затем нажмите кнопку "Сохранить" в нижней строке меню.
-	Что происходит при включении ведения журналов
	-	Когда функция ведения журналов включена, все сведения диагностики и сведения об ошибках, которые поступают в конечную точку по умолчанию, помещаются в журнал учетной записи хранения Azure, которая привязана к рабочему пространству пользователя. Эту учетную запись хранения можно видеть в представлении панели мониторинга классического портала Azure (в нижней части раздела быстрого просмотра) в своем рабочем пространстве.
-	Как просматривать журналы
	-	Эти журналы можно просматривать при помощи любого из доступных инструментов, позволяющих просматривать элементы в учетной записи хранения Azure. Самый простой способ заключается в переходе в учетную запись хранения на классическом портале Azure с последующим открытием вкладки "КОНТЕЙНЕРЫ". Должен отобразиться контейнер с именем **ml-diagnostics**. В этом контейнере содержатся все диагностические сведения обо всех конечных точках веб-службы для всех рабочих пространств, связанных с этой учетной записью хранения.
-	Сведения большого двоичного объекта журнала
	-	Каждый большой двоичный объект в контейнере содержит диагностические данные по одной из следующих операций:
		-	Осуществление метода пакетного выполнения
		-	Выполнение метода запроса-ответа
		-	Инициализация контейнера "запрос-ответ"
	-	Имя каждого большого двоичного объекта имеет префикс следующего вида: {ИД рабочего пространства}-{ИД веб-службы}-{ИД конечной точки}/{Тип журнала}
-	Доступны следующие типы журнала:
	- batch
	- score/requests
	- score/init

 

<!---HONumber=AcomDC_0810_2016-->