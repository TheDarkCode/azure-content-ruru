<properties
   pageTitle="Создание, запуск и удаление шлюза приложений | Microsoft Azure"
   description="На этой странице приводятся инструкции по созданию, настройке, запуску и удалению шлюза приложений Azure."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="gwallace"/>

# Создание, запуск или удаление шлюза приложений

Шлюз приложений — это балансировщик нагрузки уровня 7. Он отвечает за отработку отказов и эффективную маршрутизацию HTTP-запросов между разными серверами (облачными и локальными). Шлюз приложений выполняет такие функции доставки приложений: балансировка HTTP-нагрузки, определение сходства сеансов по файлам cookie, разгрузка SSL.

> [AZURE.SELECTOR]
- [Портал Azure](application-gateway-create-gateway-portal.md)
- [PowerShell и диспетчер ресурсов Azure](application-gateway-create-gateway-arm.md)
- [Классическая модель — Azure PowerShell](application-gateway-create-gateway.md)
- [Шаблон диспетчера ресурсов Azure](application-gateway-create-gateway-arm-template.md)
- [Интерфейс командной строки Azure](application-gateway-create-gateway-cli.md)

В этой статье рассказывается, как создавать, настраивать, запускать и удалять шлюз приложений.

## Перед началом работы

1. Установите последнюю версию командлетов Azure PowerShell, используя установщик веб-платформы. Вы можете скачать и установить последнюю версию из раздела **Windows PowerShell** на [странице загрузок](https://azure.microsoft.com/downloads/).
2. Если у вас есть виртуальная сеть, выберите в ней пустую подсеть, которая не используется, или создайте другую подсеть специально для использования шлюзом приложений. Шлюз приложений можно развернуть только в той виртуальной сети, где находятся ресурсы, которые нужно развернуть на базе шлюза приложений.
3. Убедитесь, что у вас есть рабочая виртуальная сеть с действительной подсетью. Убедитесь, что подсеть не используется виртуальной машиной или облачным развертыванием. Сам шлюз приложений должен находиться в подсети виртуальной сети.
3. Для использования шлюза приложений настраиваются существующие серверы или серверы, для которых в виртуальной сети созданы конечные точки либо же назначен общедоступный или виртуальный IP-адрес.

## Что необходимо для создания шлюза приложений?

Если вы создаете шлюз приложений с помощью команды **New-AzureApplicationGateway**, на этом этапе не будет задана конфигурация. Созданный ресурс необходимо настроить с помощью XML-файла или объекта конфигурации.

Доступны следующие значения.

- **Back-end server pool** (Внутренний пул серверов). Список IP-адресов внутренних серверов. Указанные IP-адреса должны относиться к подсети виртуальной сети либо представлять собой общедоступные или виртуальные IP-адреса.
- **Back-end server pool settings** (Параметры внутреннего пула серверов). Каждый пул имеет такие параметры, как порт, протокол и сходство на основе файлов cookie. Эти параметры привязываются к пулу и применяются ко всем серверам в этом пуле.
- **Внешний порт**. Общедоступный порт, открытый в шлюзе приложений. Трафик поступает на этот порт, а затем перенаправляется на один из тыловых серверов.
- **Прослушиватель**. У прослушивателя есть внешний порт, протокол (Http или Https — с учетом регистра) и имя SSL-сертификата (в случае настройки разгрузки SSL).
- **Правило**. Правило связывает прослушиватель и внутренний пул серверов, а также определяет, в какой внутренний пул серверов следует направлять трафик, поступающий на определенный прослушиватель.

## Создание шлюза приложений

Создание шлюза приложений:

1. Создайте ресурс шлюза приложений.
2. Создайте XML-файл конфигурации или объект конфигурации.
3. Примените конфигурацию к созданному ресурсу шлюза приложений.

>[AZURE.NOTE] Если вам нужно настроить пользовательскую проверку для шлюза приложений, см. статью [Создание пользовательской проверки для шлюза приложений (классического) Azure с помощью PowerShell](application-gateway-create-probe-classic-ps.md). Дополнительные сведения см. в [обзоре мониторинга работоспособности шлюза приложений](application-gateway-probe-overview.md).

### Создание ресурса шлюза приложений

Для создания шлюза используйте командлет **New-AzureApplicationGateway**, подставив в него свои значения. Выставление счетов для шлюза начинается не на данном этапе, а позднее, после успешного запуска шлюза.

В следующем примере создается шлюз приложений с использованием виртуальной сети testvnet1 и подсети subnet-1.


	New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

	VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
	VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Description*, *InstanceCount* и *GatewaySize* — необязательные параметры.

Чтобы проверить, создан ли шлюз, используйте командлет **Get-AzureApplicationGateway**.

	Get-AzureApplicationGateway AppGwTest
	Name          : AppGwTest
	Description   :
	VnetName      : testvnet1
	Subnets       : {Subnet-1}
	InstanceCount : 2
	GatewaySize   : Medium
	State         : Stopped
	VirtualIPs    : {}
	DnsName       :

>[AZURE.NOTE]  Значение параметра *InstanceCount* по умолчанию — 2 (максимальное значение — 10). По умолчанию для параметра *GatewaySize* используется значение Medium. Можно выбрать размер Small (Малый), Medium (Средний) или Large (Большой).

Параметры *VirtualIPs* и *DnsName* отображаются без значений, так как шлюз еще не запущен. Эти значения будут заданы после его запуска.

## Настройка шлюза приложений

Шлюз приложений можно настроить с помощью XML-файла или объекта конфигурации.

## Настройка шлюза приложений с помощью XML-файла

В следующем примере все параметры шлюза приложений настраиваются и применяются к ресурсу шлюза приложений при помощи XML-файла.

### Шаг 1  

Скопируйте следующий текст в Блокнот.

	<?xml version="1.0" encoding="utf-8"?>
	<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
	    <FrontendPorts>
	        <FrontendPort>
	            <Name>(name-of-your-frontend-port)</Name>
	            <Port>(port number)</Port>
	        </FrontendPort>
	    </FrontendPorts>
	    <BackendAddressPools>
	        <BackendAddressPool>
	            <Name>(name-of-your-backend-pool)</Name>
	            <IPAddresses>
	                <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
	                <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
	            </IPAddresses>
	        </BackendAddressPool>
	    </BackendAddressPools>
	    <BackendHttpSettingsList>
	        <BackendHttpSettings>
	            <Name>(backend-setting-name-to-configure-rule)</Name>
	            <Port>80</Port>
	            <Protocol>[Http|Https]</Protocol>
	            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
	        </BackendHttpSettings>
	    </BackendHttpSettingsList>
	    <HttpListeners>
	        <HttpListener>
	            <Name>(name-of-the-listener)</Name>
	            <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
	            <Protocol>[Http|Https]</Protocol>
	        </HttpListener>
	    </HttpListeners>
	    <HttpLoadBalancingRules>
	        <HttpLoadBalancingRule>
	            <Name>(name-of-load-balancing-rule)</Name>
	            <Type>basic</Type>
	            <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
	            <Listener>(name-of-the-listener)</Listener>
	            <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
	        </HttpLoadBalancingRule>
	    </HttpLoadBalancingRules>
	</ApplicationGatewayConfiguration>

Измените значения в скобках для элементов конфигурации. Сохраните файл с расширением XML.

>[AZURE.IMPORTANT] В элементе протокола HTTP или HTTPS учитывается регистр.

В следующем примере показано, как настроить шлюз приложений с помощью файла конфигурации. Пример нагрузки распределяет трафик HTTP на открытый порт 80 и отправляет сетевой трафик на серверный порт 80 между двумя IP-адресами.

	<?xml version="1.0" encoding="utf-8"?>
	<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
	    <FrontendPorts>
	        <FrontendPort>
	            <Name>FrontendPort1</Name>
	            <Port>80</Port>
	        </FrontendPort>
	    </FrontendPorts>
	    <BackendAddressPools>
	        <BackendAddressPool>
	            <Name>BackendPool1</Name>
	            <IPAddresses>
	                <IPAddress>10.0.0.1</IPAddress>
	                <IPAddress>10.0.0.2</IPAddress>
	            </IPAddresses>
	        </BackendAddressPool>
	    </BackendAddressPools>
	    <BackendHttpSettingsList>
	        <BackendHttpSettings>
	            <Name>BackendSetting1</Name>
	            <Port>80</Port>
	            <Protocol>Http</Protocol>
	            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
	        </BackendHttpSettings>
	    </BackendHttpSettingsList>
	    <HttpListeners>
	        <HttpListener>
	            <Name>HTTPListener1</Name>
	            <FrontendPort>FrontendPort1</FrontendPort>
	            <Protocol>Http</Protocol>
	        </HttpListener>
	    </HttpListeners>
	    <HttpLoadBalancingRules>
	        <HttpLoadBalancingRule>
	            <Name>HttpLBRule1</Name>
	            <Type>basic</Type>
	            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
	            <Listener>HTTPListener1</Listener>
	            <BackendAddressPool>BackendPool1</BackendAddressPool>
	        </HttpLoadBalancingRule>
	    </HttpLoadBalancingRules>
	</ApplicationGatewayConfiguration>


### Шаг 2

Теперь необходимо настроить шлюз приложений. Используйте командлет **Set-AzureApplicationGatewayConfig** и XML-файл конфигурации.


	Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

	VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
	VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## Настройка шлюза приложений с помощью объекта конфигурации

В следующем примере показано, как настроить шлюз приложений с помощью объектов конфигурации. Все элементы конфигурации нужно настроить отдельно, а затем добавить в объект конфигурации шлюза приложений. Создав объект конфигурации, используйте команду **Set-AzureApplicationGateway**, чтобы применить конфигурацию к созданному ранее ресурсу шлюза приложений.

>[AZURE.NOTE] Прежде чем присваивать значения каждому объекту конфигурации, необходимо определить, в каком типе объекта PowerShell он будет храниться. Первая строка для создания отдельных элементов определяет, какое имя объекта Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(имя объекта) используется.

### Шаг 1

Создайте все отдельные элементы конфигурации.

Создайте внешний IP-адрес, как показано в следующем примере.

	$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
	$fip.Name = "fip1"
	$fip.Type = "Private"
	$fip.StaticIPAddress = "10.0.0.5"

Создайте внешний порт, как показано в следующем примере.

	$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
	$fep.Name = "fep1"
	$fep.Port = 80

Создайте пул тыловых серверов.

 Определите IP-адреса, которые добавляются в пул тыловых серверов, как показано в следующем примере.


	$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
	$servers.Add("10.0.0.1")
	$servers.Add("10.0.0.2")

 С помощью объекта $server добавьте значения для объекта пула тыловых серверов ($pool).

	$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
	$pool.BackendServers = $servers
	$pool.Name = "pool1"

Создайте параметр пула тыловых серверов.

	$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
	$setting.Name = "setting1"
	$setting.CookieBasedAffinity = "enabled"
	$setting.Port = 80
	$setting.Protocol = "http"

Создайте прослушиватель.

	$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
	$listener.Name = "listener1"
	$listener.FrontendPort = "fep1"
	$listener.FrontendIP = "fip1"
	$listener.Protocol = "http"
	$listener.SslCert = ""

Создайте правило.

	$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
	$rule.Name = "rule1"
	$rule.Type = "basic"
	$rule.BackendHttpSettings = "setting1"
	$rule.Listener = "listener1"
	$rule.BackendAddressPool = "pool1"

### Шаг 2

Назначьте все отдельные элементы конфигурации объекту конфигурации шлюза приложений ($appgwconfig).

Добавьте в конфигурацию внешний IP-адрес.

	$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
	$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
	$appgwconfig.FrontendIPConfigurations.Add($fip)

Добавьте в конфигурацию внешний порт.

	$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
	$appgwconfig.FrontendPorts.Add($fep)

Добавьте в конфигурацию пул тыловых серверов.

	$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
	$appgwconfig.BackendAddressPools.Add($pool)  

Добавьте в конфигурацию параметр пула тыловых серверов.

	$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
	$appgwconfig.BackendHttpSettingsList.Add($setting)

Добавьте в конфигурацию прослушиватель.

	$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
	$appgwconfig.HttpListeners.Add($listener)

Добавьте в конфигурацию правило.

	$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
	$appgwconfig.HttpLoadBalancingRules.Add($rule)

### Шаг 3.

Примените объект конфигурации к ресурсу шлюза приложений, используя командлет **Set-AzureApplicationGatewayConfig**.

	Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## Запуск шлюза

Настроенный шлюз можно запустить с помощью командлета **Start-AzureApplicationGateway**. Выставление счетов для шлюза приложений начинается после запуска шлюза.

> [AZURE.NOTE] Выполнение командлета **Start-AzureApplicationGateway** может занять 15–20 минут.

	Start-AzureApplicationGateway AppGwTest

	VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
	VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## Проверка состояния шлюза

Чтобы проверить состояние шлюза, используйте командлет **Get-AzureApplicationGateway**. Если командлет **Start-AzureApplicationGateway** на предыдущем этапе выполнен успешно, у параметра *State* должно быть значение "Работает", а у параметров *Vip* и *DnsName* — допустимые значения.

В следующем примере показан рабочий шлюз приложений, готовый к приему трафика, отправляемого по адресу `http://<generated-dns-name>.cloudapp.net`.

	Get-AzureApplicationGateway AppGwTest

	VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
	VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
	Name          : AppGwTest
	Description   :
	VnetName      : testvnet1
	Subnets       : {Subnet-1}
	InstanceCount : 2
	GatewaySize   : Medium
	State         : Running
	Vip           : 138.91.170.26
	DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## Удаление шлюза приложений

Удалить шлюз приложений можно так.

1. Чтобы остановить работу шлюза, используйте командлет **Stop-AzureApplicationGateway**.
2. Чтобы удалить шлюз, используйте командлет **Remove-AzureApplicationGateway**.
3. Убедитесь, что шлюз удален, с помощью командлета **Get-AzureApplicationGateway**.

В следующем примере командлет **Stop-AzureApplicationGateway** приведен в первой строке, а за ним следуют выходные данные.

	Stop-AzureApplicationGateway AppGwTest

	VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
	VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Когда работа шлюза приложений будет остановлена, удалите службу с помощью командлета **Remove-AzureApplicationGateway**.


	Remove-AzureApplicationGateway AppGwTest

	VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
	VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Убедитесь, что служба удалена, с помощью командлета **Get-AzureApplicationGateway**. Этот шаг не является обязательным.


	Get-AzureApplicationGateway AppGwTest

	VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

	Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
	.....

## Дальнейшие действия

Сведения о настройке разгрузки SSL см. в статье [Настройка шлюза приложений для разгрузки SSL](application-gateway-ssl.md).

Указания по настройке шлюза приложений для использования с внутренним балансировщиком нагрузки см. в статье [Создание шлюза приложений с внутренней подсистемой балансировщика нагрузки (ILB)](application-gateway-ilb.md).

Дополнительные сведения о параметрах балансировки нагрузки в целом см. в статьях:

- [Подсистема балансировщика нагрузки Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!---HONumber=AcomDC_0907_2016-->