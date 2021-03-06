<properties
	pageTitle="Моделирование устройства с помощью шлюза пакета SDK | Microsoft Azure"
	description="Пошаговое руководство по работе с пакетом SDK для шлюза IoT Azure под управлением Windows показывает, как отправлять данные телеметрии с виртуального устройства, используя пакет SDK для шлюза IoT Azure."
	services="iot-hub"
	documentationCenter=""
	authors="chipalost"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="04/20/2016"
     ms.author="cstreet"/>


# IoT Gateway SDK (beta) – send device-to-cloud messages with a simulated device using Windows (Пакет SDK для шлюза IoT (бета-версия): отправка сообщений с устройства в облако через виртуальное устройство с помощью Windows)

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## Сборка и запуск примера

Перед началом работы необходимо выполнить следующие действия:

- [Настроить среду разработки][lnk-setupdevbox] для работы с пакетом SDK для Windows.
- [Создать центр IoT][lnk-create-hub] в подписке Azure (для работы с данным руководством необходимо знать имя центра). Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись][lnk-free-trial].
- Добавьте в центр IoT два устройства и запишите их идентификаторы и ключи устройств. Для добавления устройств в центр IoT, созданный при выполнении предыдущего шага, и получения их ключей можно использовать [обозреватель устройств или средство iothub-explorer][lnk-explorer-tools].

Сборка примера

1. Откройте **командную строку разработчика для VS2015**.
2. Перейдите в корневую папку в локальной копии репозитория **azure-iot-gateway-sdk**.
3. Запустите сценарий **tools\\build.cmd**. Этот сценарий создает файл решения Visual Studio, собирает решение и проводит тесты. Решение Visual Studio находится в папке **build** локальной копии репозитория **azure-iot-gateway-sdk**.

Запуск примера

В текстовом редакторе откройте файл **samples\\simulated\_device\_cloud\_upload\\src\\simulated\_device\_cloud\_upload\_win.json** в локальной копии репозитория **azure-iot-gateway-sdk**. Этот файл настраивает модули в примере шлюза:

- Модуль **IoTHub** подключается к центру IoT. Его необходимо настроить для отправки данных в центр IoT. В частности, укажите в качестве значения **IoTHubName** имя своего центра IoT, а в качестве значения **IoTHubSuffix** — **azure devices.net**.
- Модуль **mapping** сопоставляет MAC-адреса виртуальных устройств с идентификаторами устройств центра IoT. Убедитесь в том, что значения **deviceId** совпадают с идентификаторами двух устройств, добавленных в центр IoT, а значения **deviceKey** содержат ключи эти двух устройств.
- Модули **BLE1** и **BLE2** — это виртуальные устройства. Обратите внимание на то, что их MAC-адреса совпадают с адресами в модуле **mapping**.
- Модуль **Logger** записывает активность вашего шлюза в файл.
- Указанные ниже значения **module path** предполагают, что вы клонировали репозиторий пакета SDK для шлюза в корневую папку **C:**. Если вы загрузили его в другое место, измените соответственно значения **module path**.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\modules\\iothubhttp\\Debug\\iothubhttp_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\modules\\identitymap\\Debug\\identitymap_hl.dll",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "GW-ble1-demo",
                    "deviceKey"  : "{Device key}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "GW-ble2-demo",
                    "deviceKey"  : "{Device key}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "C:\\azure-iot-gateway-sdk\\modules\\ble_fake\\Debug\\ble_fake_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\modules\\ble_fake\\Debug\\ble_fake_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
            }
        }
    ]
}
```

Сохраните все изменения, внесенные в файл конфигурации.

Запуск примера

1. В командной строке перейдите в корневую папку в локальной копии репозитория **azure-iot-gateway-sdk**.
2. Выполните следующую команду:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. Для мониторинга сообщений, полученных центром IoT из шлюза, можно использовать [обозреватель устройств или средство iothub-explorer][lnk-explorer-tools].


## Дальнейшие действия

Если вы хотите подробнее изучить возможности пакета SDK для шлюза и поэкспериментировать с примерами кода, см. следующие руководства и ресурсы для разработчиков.

- [Пакет SDK для шлюза IoT (бета-версия): отправка сообщений с устройства в облако через реальное устройство с ОС Linux][lnk-physical-device]
- [Пакет SDK для шлюза IoT (бета-версия): управление устройствами с помощью пакета SDK для шлюза][lnk-manage-devices]
- [Пакет SDK для шлюза IoT Azure][lnk-gateway-sdk]

Для дальнейшего изучения возможностей центра IoT см. следующие статьи:

- [Разработка решения][lnk-design]
- [Руководство разработчика по центру Azure IoT (IoT — Интернет вещей)][lnk-devguide]
- [Обзор управления устройствами центра IoT с помощью примера пользовательского интерфейса][lnk-dmui]
- [Управление центрами IoT через портал Azure][lnk-portal]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-create-hub]: iot-hub-manage-through-portal.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md
[lnk-manage-devices]: iot-hub-gateway-sdk-device-management.md

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0713_2016-->