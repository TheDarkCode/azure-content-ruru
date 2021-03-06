<properties
	pageTitle="Что такое хранилище ключей Azure? | Microsoft Azure"
	description="Хранилище ключей Azure помогает защитить криптографические ключи и секреты, используемые облачными приложениями и службами. Хранилище ключей позволяет заказчикам шифровать ключи и секреты (например, ключи аутентификации, ключи учетных записей хранения, ключи шифрования данных, PFX-файлы и пароли), используя ключи, защищенные аппаратными модулями безопасности."
	services="key-vault"
	documentationCenter=""
	authors="cabailey"
	manager="mbaldwin"
	tags="azure-resource-manager"/>

<tags
	ms.service="key-vault"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="07/15/2016"
	ms.author="cabailey"/>



# Что такое хранилище ключей Azure?

Хранилище ключей Azure доступно в большинстве регионов. Дополнительные сведения см. на странице [Цены на хранилище ключей](https://azure.microsoft.com/pricing/details/key-vault/).

## Введение

Хранилище ключей Azure помогает защитить криптографические ключи и секреты, используемые облачными приложениями и службами. С помощью хранилища ключей можно шифровать ключи и секреты (например, ключи аутентификации, ключи учетных записей хранения, ключи шифрования данных, PFX-файлы и пароли), используя ключи, защищенные аппаратными модулями безопасности. Чтобы обеспечить более высокий уровень защиты, ключи можно импортировать или создать в аппаратных модулях безопасности. Если вы выберете этот вариант, корпорация Майкрософт будет обрабатывать ключи в аппаратных модулях безопасности (оборудование и микропрограммы), сертифицированных по стандарту FIPS 140-2 уровня 2.

Хранилище ключей упрощает управление ключами и позволяет поддерживать контроль над ключами, которые предоставляют доступ к вашим данным и шифруют их. Разработчики могут за считанные минуты создавать ключи для разработки и тестирования, а затем с легкостью использовать их в рабочей среде. По необходимости администраторы безопасности могут предоставлять (и отзывать) разрешения на использование ключей.

Используйте таблицу ниже, чтобы лучше понять, как хранилище ключей может помочь в удовлетворении требований разработчиков и администраторов безопасности.





| Роль | Проблема | Решение с помощью хранилища ключей Azure |
| ------------- |-------------|-----|
| Разработчик приложения Azure | «Я хочу написать приложение для Azure, которое будет использовать ключи для подписывания и шифрования, но нужно, чтобы это происходило вне приложения, дабы решение подходило для географически удаленного приложения. <br/><br/>Кроме того, необходимо, чтобы эти ключи и секреты были защищены и не нужно было писать код самостоятельно, а также чтобы их можно было с легкостью и оптимальной производительностью использовать в моем приложении». | √ Ключи хранятся в хранилище, и при необходимости их можно вызывать с помощью URI.<br/><br/> √ Azure защищает ключи с помощью стандартных отраслевых алгоритмов, управления длиной ключей и аппаратных модулей безопасности.<br/><br/> √ Ключи находятся в аппаратных модулях безопасности в центрах обработки данных Azure, что обеспечивает более высокую надежность и сокращает задержку в сравнении с ключами, расположенными в различных местах, например, локально.|
| Разработчик программного обеспечения как услуги (SaaS) |«Я не хочу нести ответственность или связывать себя потенциальными обязательствами, связанными с ключами и секретами клиентов моих заказчиков. <br/><br/>Мне нужно, чтобы заказчики владели и управляли своими ключами, а мне можно было полностью сосредоточиться на своей основной работе, а именно предоставлении базовых функций программного обеспечения». | √ Клиенты могут импортировать свои ключи в Azure и управлять ими. Если приложению SaaS необходимо выполнять криптографические операции, используя ключи клиентов, то хранилище ключей делает это от имени приложения. Приложение не видит ключи клиентов.|
| Руководитель службы безопасности | «Мне необходимо знать, что аппаратные модули безопасности, используемые в наших приложениях, соответствуют стандарту FIPS 140-2 уровня 2, обеспечивающему безопасное управление ключами. <br/><br/>Я также хочу убедиться, что моя организация может управлять жизненным циклом ключей и отслеживать их использование. <br/><br/>Кроме того, хотя мы используем множество служб и ресурсов Azure, необходимо, чтобы ключами можно было управлять из одного расположения в Azure». |√ Аппаратные модули безопасности прошли проверку по стандарту FIPS 140-2 уровня 2.<br/><br/>√ Хранилище ключей разработано таким образом, чтобы сотрудники Майкрософт не могли видеть или извлекать ваши ключи.<br/><br/>√ Журнал использования ключей ведется практически в режиме реального времени.<br/><br/>√ Хранилище предоставляет единый интерфейс, независимо от количества хранилищ в Azure, поддерживаемых регионов и использующих их приложений. |


Хранилища ключей может создавать и использовать любой пользователь, у которого есть подписка Azure. Хотя хранилища ключей предоставляют преимущества для разработчиков и администраторов безопасности, администратор организации, который управляет в ней другими службами Azure, может также реализовать хранилища и управлять ими. Например, этот администратор может войти в систему, используя подписку Azure, и создать хранилище, в котором будут храниться ключи, для организации. Затем он будет отвечать за выполнение таких операционных задач, как:

+ создание или импорт ключа или секрета;
+ отзыв или удаление ключа или секрета;
+ авторизация пользователей или приложений для доступа к хранилищу ключей с целью управления ключами и секретами или их использования;
+ настройка использования ключей (например, подписи и шифрования);
+ мониторинг использования ключей.

Затем этот администратор будет предоставлять разработчикам URI для вызова ключей из приложения, а их администраторам безопасности — информацию журналов использования ключей.

   ![Обзор хранилища ключей Azure][1]

Разработчики также могут управлять ключами напрямую с помощью API. Подробные сведения см. в [руководстве разработчика хранилища ключей](key-vault-developers-guide.md).

## Дальнейшие действия

Учебник по началу работы для администраторов см. в статье [Приступая к работе с хранилищем ключей Azure](key-vault-get-started.md).

Дополнительные сведения о ведении журнала использования для хранилища ключей см. в статье [Azure Key Vault Logging](key-vault-logging.md) (Ведение журнала хранилища ключей Azure).

Дополнительную информацию об использовании ключей и секретов с помощью хранилища ключей Azure см. в разделе [О ключах и секретах](https://msdn.microsoft.com/library/azure/dn903623.aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png

<!---HONumber=AcomDC_0720_2016-->