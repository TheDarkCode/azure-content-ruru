<properties 
	pageTitle="Известные сети | Microsoft Azure" 
	description="Настроив известные сети, вы избавитесь от ситуации, при которой IP-адреса вашей организации попадают в отчеты ";Операции входа из нескольких географических регионов"; и ";Операции входа с IP-адресов с подозрительными действиями";." 
	services="active-directory" 
	documentationCenter="" 
	authors="markusvi" 
	manager="femila"  
	editor=""/>

<tags 
	ms.service="active-directory" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/20/2016" 
	ms.author="markvi"/>

# Известные сети


Вы можете использовать отчеты об использовании и получении доступа Azure Active Directory, чтобы получить информацию о целостности и безопасности каталога вашей организации. C помощью этой информации администратор каталога может более точно определить источники возможных угроз безопасности, что позволяет правильно спланировать их устранение.

Возможна ситуация, при которой в отчеты "*Операции входа из нескольких географических регионов*" и "*Операции входа с IP-адресов с подозрительными действиями*" могут ошибочно попасть IP-адреса, фактически принадлежащие вашей организации.

Например, это может произойти, когда:

- пользователь в офисе в Бостоне удаленно подключился к центру обработки данных в Сан-Франциско, что привело к появлению отчета "Операции входа из нескольких географических регионов";

- пользователь вашей организации пытается войти несколько раз, указывая неправильный пароль, что приводит к появлению отчета "Операции входа с IP-адресов с подозрительными действиями".

Чтобы в таких случаях не создавались ложные отчеты системы безопасности, следует добавить известные диапазоны IP-адресов в список общедоступных IP-адресов вашей организации.


###Чтобы добавить диапазоны IP-адресов в список общедоступных IP-адресов вашей организации, выполните следующие действия. 

1.	Войдите в [портал управления Azure](https://manage.windowsazure.com).

2.	В левой панели щелкните **Active Directory**. <br><br>![Как работает Cloud App Discovery](./media/active-directory-known-networks/known-netwoks-01.png)

3.	На вкладке **Каталог** выберите требуемый каталог.

4.	В верхнем меню щелкните **Настроить**. <br><br>![Как работает Cloud App Discovery](./media/active-directory-known-networks/known-netwoks-02.png)

5.	На вкладке "Настройка" перейдите в **диапазоны общедоступных IP-адресов вашей организации** <br><br>![Как работает Cloud App Discovery](./media/active-directory-known-networks/known-netwoks-03.png)

6.	Щелкните **Добавьте известные диапазоны IP-адресов**.

7.	Добавьте диапазоны адресов в появившемся диалоговом окне и нажмите кнопку "Проверить". <br><br>![Как работает Cloud App Discovery](./media/active-directory-known-networks/known-netwoks-04.png)


**Дополнительные ресурсы**


* [Просмотр отчетов о доступе и использовании](active-directory-view-access-usage-reports.md)
* [Операции входа с IP-адресов с подозрительными действиями](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Операции входа из нескольких географических регионов](active-directory-reporting-sign-ins-from-multiple-geographies.md)

<!---HONumber=AcomDC_0727_2016-->