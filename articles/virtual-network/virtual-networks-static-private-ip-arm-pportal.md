<properties 
   pageTitle="Как задать статический частный IP-адрес в режиме ARM с помощью портала Azure | Microsoft Azure"
   description="Основные сведения о частных IP-адресах (DIP) и управлении ими в режиме ARM с помощью портала Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# Как задать статический частный IP-адрес на портале Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] В этой статье описывается модель развертывания с использованием менеджера ресурсов. Кроме того, вы можете [управлять статическим частным IP-адресом в классической модели развертывания](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Для выполнения приведенных ниже примеров действий требуется созданная простая среда. Чтобы выполнять действия в соответствии с указаниями, представленными в этом документе, сначала постройте тестовую среду, как описано в статье [Создание виртуальной сети](virtual-networks-create-vnet-arm-pportal.md).

## Как создать виртуальную машину для тестирования статических частных IP-адресов

Невозможно задать статический частный IP-адрес во время создания виртуальной машины в режиме развертывания диспетчера ресурсов с помощью портала Azure. Сначала необходимо создать виртуальную машину, а затем указать, что ее статический IP-адрес является частным.

Чтобы создать виртуальную машину с именем *DNS01* в подсети *FrontEnd* виртуальной сети с именем *TestVNet*, выполните следующие действия.

1. В браузере откройте страницу http://portal.azure.com и при необходимости войдите в свою учетную запись Azure.
2. Щелкните **СОЗДАТЬ** > **Среда выполнения приложений** > **Windows Server 2012 R2 Datacenter**, обратите внимание на то, что в списке **Выберите модель развертывания** уже указан параметр **Диспетчер ресурсов**, и нажмите кнопку **Создать**, как показано ниже.

	![Создание виртуальной машины на портале Azure](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. В колонке **Основные** введите имя создаваемой виртуальной машины (в нашем случае это *DNS01*), учетную запись локального администратора и пароль, как показано на рисунке ниже.

	![Колонка «Основные»](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Убедитесь, что выбрано **расположение** *Центральная часть США*, щелкните **Выбрать существующую** в разделе **Группы ресурсов**, затем еще раз щелкните **Группа ресурсов**, щелкните *TestRG* и нажмите кнопку **ОК**.

	![Колонка «Основные»](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. В колонке **Выбор размера** выберите **A1 Standard**, затем щелкните **Выбрать**.

	![Колонка «Выбор размера»](./media/virtual-networks-static-ip-arm-pportal/figure04.png)

6. Убедитесь, что в колонке **Параметры** заданы значения свойств, указанные ниже, и нажмите кнопку **ОК**.

	-**Учетная запись**: *vnetstorage*
	- **Сеть**: *TestVNet*
	- **Подсеть**: *FrontEnd*

	.![Колонка «Выбор размера»](./media/virtual-networks-static-ip-arm-pportal/figure05.png)

7. В колонке **Сводка** нажмите кнопку **ОК**. Ниже обратите внимание на элемент, отображенный в панели мониторинга.

	.![Создание виртуальной машины на портале Azure](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## Как получить информацию о статическом частном IP-адресе виртуальной машины

Чтобы просмотреть информацию о статическом частном IP-адресе виртуальной машины, созданной с помощью описанные выше действий, выполните следующее.

1. На портале Azure щелкните элементы **ПРОСМОТРЕТЬ ВСЕ** > **Виртуальные машины** > **DNS01** > **Все параметры** > **Сетевые интерфейсы**, затем щелкните единственный сетевой интерфейс в списке.

	.![Элемент «Развертывание виртуальной машины»](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. В колонке **Сетевой интерфейс** щелкните **Все параметры** > **IP-адреса** и обратите внимание на значения **Назначение** и **IP-адрес**.

	![Элемент «Развертывание виртуальной машины»](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## Как добавить статический частный IP-адрес для существующей виртуальной машины
Чтобы добавить статический частный IP-адрес для виртуальной машины, созданной с помощью действий, описанных выше, выполните следующее:

1. В колонке **IP-адреса**, показанной выше, в разделе **Назначение** щелкните **Статический**.
2. Введите *192.168.1.101* в поле **IP-адрес**, а затем нажмите кнопку **Сохранить**.

	.![Создание виртуальной машины на портале Azure](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Если после нажатия кнопки **Сохранить** вы заметите, что назначение по-прежнему остается **динамическим**, это означает, что введенный IP-адрес уже используется. Попробуйте другой IP-адрес.

## Как удалить статический частный IP-адрес виртуальной машины
Чтобы удалить статический частный IP-адрес виртуальной машины, созданной ранее, выполните следующее.
	
1. В колонке **IP-адреса**, показанной выше, в разделе **Назначение** щелкните **Динамический** и нажмите кнопку **Сохранить**.

## Дальнейшие действия

- Ознакомьтесь с информацией о [зарезервированных общедоступных IP-адресах](virtual-networks-reserved-public-ip.md).
- Узнайте об [общедоступных IP-адресах уровня экземпляра (ILPIP)](virtual-networks-instance-level-public-ip.md).
- Ознакомьтесь с [REST API зарезервированных IP-адресов](https://msdn.microsoft.com/library/azure/dn722420.aspx).

<!---HONumber=AcomDC_0810_2016-->