<properties
 pageTitle="Сравнение центра IoT Azure и концентраторов событий Azure | Microsoft Azure"
 description="Сравнение центра IoT Azure и служб концентраторов событий Azure, в котором выделены функциональные различия и случаи использования."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="06/06/2016"
 ms.author="elioda"/>

# Сравнение центра IoT и концентраторов событий

Одним из основных способов использования центра IoT Azure (IoT — «Интернет вещей») является сбор телеметрических данных с устройств. По этой причине центр IoT часто сравнивается с [концентраторами событий Azure][]. Как и центры IoT, концентраторы событий — это служба обработки событий, используемая для крупномасштабной передачи событий и данных телеметрии в облако при низкой задержке и с высокой надежностью.

Однако между службами имеется множество различий, которые подробно описаны в следующей таблице.

| Область | Центр IoT | Концентраторы событий |
| ---- | ------- | ---------- |
| Модели связи | Включает передачу сообщений с устройства в облако и из облака на устройство. | Обеспечивает только передачу событий (обычно учитывается при передаче данных с устройства в облако). |
| Поддержка протоколов | Поддерживает протоколы MQTT, AMQP, AMQP через WebSocket и HTTP/1. Кроме того, центр IoT работает со [шлюзом протокола Azure IoT][lnk-azure-protocol-gateway] — настраиваемой реализацией шлюза протокола для поддержки пользовательских протоколов. | Поддерживает протоколы AMQP, AMQP через WebSocket и HTTP/1. |
| Безопасность | Обеспечивает идентификацию каждого устройства и контроль доступа с возможностью отзыва. См. [раздел "Безопасность" руководства для разработчиков центра IoT]. | Обеспечивает [политики общего доступа][Event Hub - security], действующие для всего концентратора событий, с ограниченными возможностями отзыва посредством [политик издателя][Event Hub publisher policies]. От решений IoT часто требуется реализация настраиваемого решения для поддержки учетных данных отдельных устройств и принятие мер против спуфинга. |
| Мониторинг операций | Позволяет решениям IoT подписываться на широкий ряд событий управления удостоверениями и подключения устройств, включая отдельные ошибки проверки подлинности устройств, регулирование и исключения, связанные с недопустимым форматом. С помощью этих событий можно быстро выявлять неполадки на уровне отдельных устройств. | Предоставляются только статистические показатели. |
| Масштаб | Оптимизирован для поддержки миллионов одновременно подключенных устройств. | Поддерживает меньшее число одновременных подключений: до 5000 подключений AMQP (согласно [квотам служебной шины Azure][]). С другой стороны, концентраторы событий позволяют пользователям указывать раздел для каждого отправляемого сообщения. |
| Пакеты SDK для устройств | Предоставляет [пакеты SDK для устройств][Azure IoT Hub SDKs] под большое количество платформ и языков. | Поддерживается в .NET и языке C. Кроме того, предоставляет интерфейсы передачи AMQP и HTTP. |
| Передача файла | Позволяет решениям IoT передавать файлы из устройств в облако. Включает в себя конечную точку уведомления о файлах для интеграции рабочих процессов и категорию мониторинга операций для поддержки отладки. | Использует схему проверки утверждений для ручного запроса файлов из устройств и предоставления устройствам ключа к хранилищу данных для транзакции. |

В заключение скажем, что, даже если единственным вариантом использования является передача телеметрических данных от устройства в облако, центр IoT предоставляет службу, специально разработанную для подключения устройств IoT. Для центра IoT будут и дальше появляться новые возможности для IoT, ориентированные на такие сценарии. Концентраторы событий предназначены для приема огромных объемов событий как в контексте обмена данными между разными ЦОД, так и в пределах одного ЦОД.

Использование центра IoT вместе с концентраторами событий в одном и том же решении не является экзотикой. В таких случаях центр IoT обрабатывает передачу данных с устройств в облако, а концентраторы событий — дальнейшую передачу событий в модули обработки в реальном времени.

## Дальнейшие действия

Дополнительные сведения о планировании развертывания центра IoT см. в статье [Масштабирование центра IoT][lnk-scaling].

Для дальнейшего изучения возможностей центра IoT см. следующие статьи:

- [Руководство разработчика по центру Azure IoT (IoT — Интернет вещей)][lnk-devguide]
- [Обзор управления устройствами центра IoT с помощью примера пользовательского интерфейса][lnk-dmui]
- [Пакет SDK для шлюза IoT (бета-версия): отправка сообщений с устройства в облако через виртуальное устройство с помощью Linux][lnk-gateway]
- [Управление центрами IoT через портал Azure][lnk-portal]

[концентраторами событий Azure]: ../event-hubs/event-hubs-what-is-event-hubs.md
[раздел "Безопасность" руководства для разработчиков центра IoT]: iot-hub-devguide.md#security
[Event Hub - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hub publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[квотам служебной шины Azure]: ../service-bus/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0907_2016-->