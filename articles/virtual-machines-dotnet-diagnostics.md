<properties
	pageTitle="Как использовать систему диагностики Azure в виртуальных машинах | Microsoft Azure"
	description="Сбор данных виртуальных машин Azure с помощью системы диагностики Azure для отладки, оценки производительности, мониторинга, анализа трафика и многого другого."
	services="virtual-machines"
	documentationCenter=".net"
	authors="rboucher"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="virtual-machines"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="02/20/2016"
	ms.author="robb"/>



# Включение диагностики на виртуальных машинах Azure

Основные сведения о системе диагностики Azure см. в [обзоре системы диагностики Azure](azure-diagnostics.md).

## Как включить диагностику в виртуальной машине

Мы рассмотрим, как удаленно установить систему диагностики на виртуальной машине Azure с компьютера разработчика. Также вы узнаете, как реализовать приложение, которое выполняется на виртуальной машине Azure и отправляет данные телеметрии через класс .NET [EventSource][]. Система диагностики Azure используется для сбора телеметрии и хранения ее в учетной записи хранения Azure.

### Предварительные требования
В этом пошаговом учебнике предполагается, что у вас есть подписка Azure и вы используете Visual Studio 2013 с пакетом SDK для Azure. Если у вас нет подписки Azure, можно зарегистрироваться для получения [бесплатной пробной версии][]. Следует обязательно [установить и настроить Azure PowerShell версии 0.8.7 или более поздней][].

### Шаг 1. Создание виртуальной машины
1.	На компьютере разработчика запустите Visual Studio 2013.
2.	В **обозревателе сервера** Visual Studio разверните **Azure**, щелкните правой кнопкой мыши элемент **Виртуальные машины** и выберите команду **Создать виртуальную машину**.
3.	Выберите подписку Azure в диалоговом окне **Выберите подписку** и щелкните **Далее**.
4.	Выберите **Windows Server 2012 R2 Datacenter, November 2014** в диалоговом окне **Выбор образа виртуальной машины** и нажмите кнопку **Далее**.
5.	В разделе **базовых параметров виртуальной машины** укажите для виртуальной машины имя wadexample. Укажите имя пользователя и пароль администратора и щелкните **Далее**.
6.	В диалоговом окне **Параметры облачной службы** создайте новую облачную службу с именем wadexampleVM. Создайте новую учетную запись хранения с именем wadexample и нажмите кнопку **Далее**.
7.	Щелкните **Создать**.

### Шаг 2. Создание приложения
1.	На компьютере разработчика запустите Visual Studio 2013.
2.	Создайте новое приложение консоли Visual C#, предназначенное для платформы .NET Framework 4.5. Назовите проект WadExampleVM.![CloudServices\_diag\_new\_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.	Замените содержимое файла Program.cs на код, приведенный ниже. Класс **SampleEventSourceWriter** реализует четыре метода ведения журнала: **SendEnums**, **MessageMethod**, **SetOther** и **HighFreq**. Первый параметр для метода WriteEvent определяет идентификатор соответствующего события. Метод Run реализует бесконечный цикл, который вызывает каждый из методов ведения журнала, реализованных в классе **SampleEventSourceWriter**, каждые 10 секунд.

		using System;
		using System.Diagnostics;
		using System.Diagnostics.Tracing;
		using System.Threading;

		namespace WadExampleVM
		{
    	sealed class SampleEventSourceWriter : EventSource
    	{
        	public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        	public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        	public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        	public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        	public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    	}

    	enum MyColor
    	{
        	Red,
        	Blue,
        	Green
    	}

    	[Flags]
    	enum MyFlags
    	{
        	Flag1 = 1,
        	Flag2 = 2,
        	Flag3 = 4
    	}

    	class Program
    	{
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }

        }
    	}
		}


4.	Сохраните файл и в меню **Сборка** выберите **Собрать решение**, чтобы выполнить сборку кода.


### Шаг 3. Развертывание приложения
1.	В **обозревателе решений** щелкните правой кнопкой мыши проект **WadExampleVM** и выберите **Открыть папку в проводнике**.
2.	Перейдите к папке *bin\\Debug* и скопируйте все файлы (WadExampleVM.*)
3.	В **обозревателе серверов** щелкните правой кнопкой мыши виртуальную машину и выберите **Подключиться с помощью удаленного рабочего стола**.
4.	После подключения к виртуальной машине создайте папку с именем WadExampleVM и вставьте в нее свои файлы приложения.
5.	Запустите приложение WadExampleVM.exe. Должно отобразиться пустое окно консоли.

### Шаг 4. Создание конфигурации системы диагностики и установка расширения
1.	Скачайте общедоступное определение схемы файла конфигурации на свой компьютер для разработки, выполнив следующую команду PowerShell:

		(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.	Откройте новый XML-файл в Visual Studio либо в уже открытом проекте, либо в экземпляре Visual Studio без открытых проектов. В Visual Studio выберите **Добавить** -> **Новый элемент…** -> **Элементы Visual C#** -> **Данные** -> **XML-файл**. Назовите файл «WadExample.xml».
3.	Свяжите файл WadConfig.xsd с файлом конфигурации. Убедитесь, что окно редактора WadExample.xml активно. Нажмите клавишу **F4**, чтобы открыть окно **Свойства**. Щелкните свойство **Schemas** в окне **Свойства**. Щелкните **…** в свойстве **Schemas**. Нажмите кнопку **Добавить…**, перейдите в расположение, где сохранен XSD-файл, и выберите файл WadConfig.xsd. Нажмите кнопку **ОК**.
4.	Замените содержимое файла настройки WadExample.xml приведенным кодом XML и сохраните файл. Этот файл конфигурации определяет пару счетчиков производительности: один — для использование ЦП, и один — для использования памяти. Затем конфигурация определяет четыре события, соответствующие методам в классе SampleEventSourceWriter.

```
		<?xml version="1.0" encoding="utf-8"?>
		<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  			<WadCfg>
    			<DiagnosticMonitorConfiguration overallQuotaInMB="25000">
      			<PerformanceCounters scheduledTransferPeriod="PT1M">
        			<PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
        			<PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
      				</PerformanceCounters>
      				<EtwProviders>
        				<EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
          					<Event id="1" eventDestination="EnumsTable"/>
          					<Event id="2" eventDestination="MessageTable"/>
          					<Event id="3" eventDestination="SetOtherTable"/>
          					<Event id="4" eventDestination="HighFreqTable"/>
          					<DefaultEvents eventDestination="DefaultTable" />
        				</EtwEventSourceProviderConfiguration>
      				</EtwProviders>
    			</DiagnosticMonitorConfiguration>
  			</WadCfg>
		</PublicConfig>
```

### Шаг 5. Удаленная установка системы диагностики на виртуальной машине Azure
Командлеты PowerShell для управления диагностикой на виртуальной машине: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension и Remove-AzureVMDiagnosticsExtension.

1.	На компьютере разработчика откройте Azure PowerShell.
2.	Выполните сценарий для удаленной установки системы диагностики в своей виртуальной машине (замените *StorageAccountKey* ключом для учетной записи хранения wadexamplevm):

		$storage_name = "wadexamplevm"
		$key = "<StorageAccountKey>"
		$config_path="c:\users<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
		$service_name="wadexamplevm"
		$vm_name="WadExample"
		$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
		$VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
		$VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
		$VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### Шаг 6. Просмотр данных телеметрии
В **обозревателе решений** Visual Studio перейдите к учетной записи хранения wadexample. После того, как виртуальная машина проработала около пяти минут, следует посмотреть таблицы **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** и **WADSetOtherTable**. Дважды щелкните одну из таблиц, чтобы просмотреть собранную телеметрию.

![CloudServices\_diag\_wadexamplevm\_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## Схема файла конфигурации

Файл конфигурации системы диагностики определяет значения, которые используются для инициализации параметров конфигурации диагностики, когда запускается агент диагностики. Допустимые значения и примеры см. в [последнем справочнике по схеме](https://msdn.microsoft.com/library/azure/mt634524.aspx).

## Устранение неполадок

Дополнительные сведения см. в разделе [Устранение неполадок системы диагностики Azure](azure-diagnostics-troubleshooting.md).


## Дальнейшие действия
[См. перечень статей о системе диагностики Azure, связанных с виртуальными машинами](azure-diagnostics.md#virtual-machines-using-azure-diagnostics), чтобы изменить данные, которые собираются, устранить неполадки или больше узнать о диагностике в целом.


[EventSource]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[бесплатной пробной версии]: http://azure.microsoft.com/pricing/free-trial/
[установить и настроить Azure PowerShell версии 0.8.7 или более поздней]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

<!---HONumber=AcomDC_0302_2016-->