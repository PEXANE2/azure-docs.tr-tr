---
title: Tanılama günlüklerini ayarlama - Azure Etkinlik Hub | Microsoft Dokümanlar
description: Azure'daki etkinlik merkezleri için etkinlik günlüklerini ve tanılama günlüklerini nasıl ayarlayabilirsiniz öğrenin.
keywords: ''
documentationcenter: ''
services: event-hubs
author: ShubhaVijayasarathy
manager: ''
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 68aa62ad34f8db531d439a581ef024862da0f90c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162319"
---
# <a name="set-up-diagnostic-logs-for-an-azure-event-hub"></a>Azure olay hub'ı için tanılama günlüklerini ayarlama

Azure Etkinlik Hub'ları için iki tür günlük görüntüleyebilirsiniz:

* **[Etkinlik günlükleri](../azure-monitor/platform/platform-logs-overview.md)**: Bu günlüklerde bir işte gerçekleştirilen işlemler hakkında bilgi vardır. Günlükler her zaman etkinleştirilir.
* **[Tanılama günlükleri](../azure-monitor/platform/platform-logs-overview.md)**: Bir iş ile olur her şeyi daha zengin bir görünüm için tanılama günlükleri yapılandırabilirsiniz. Tanılama günlükleri, iş çalışırken gerçekleşen güncelleştirmeler ve etkinlikler de dahil olmak üzere, iş silinene kadar olan bitene kadar olan etkinlikleri kapsar.

## <a name="enable-diagnostic-logs"></a>Tanılama günlüklerini etkinleştirme

Tanılama günlükleri varsayılan olarak devre dışı bırakılır. Tanılama günlüklerini etkinleştirmek için aşağıdaki adımları izleyin:

1.  Azure [portalında](https://portal.azure.com), **İzleme + Yönetim**altında **Tanılama günlüklerini**tıklatın.

    ![Tanılama günlüklerine bölme gezintisi](./media/event-hubs-diagnostic-logs/image1.png)

2.  İzlemek istediğiniz kaynağı tıklatın.

3.  **Tanılamayı aç**’a tıklayın.

    ![Tanılama günlüklerini açma](./media/event-hubs-diagnostic-logs/image2.png)

4.  **Durum**için , **On'a**tıklayın.

    ![Tanılama günlüklerinin durumunu değiştirme](./media/event-hubs-diagnostic-logs/image3.png)

5.  İstediğinize arşiv hedefini ayarlayın; örneğin, bir depolama hesabı, bir olay merkezi veya Azure Monitor günlükleri.

6.  Yeni tanılama ayarlarını kaydedin.

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Bundan sonra, günlükler yapılandırılan arşiv hedefinde, **Tanılama günlükleri** bölmesinde görünür.

Tanılamayı yapılandırma hakkında daha fazla bilgi için [Azure tanı günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md)alameti dir.

## <a name="diagnostic-logs-categories"></a>Tanılama günlükleri kategorileri

Olay Hub'ları iki kategoriiçin tanıgünlüklerini yakalar:

* **Arşiv Günlükleri**: Olay Hub'ları arşivleri ile ilgili günlükler, özellikle arşiv hataları ile ilgili günlükler.
* **Operasyonel Günlükler:** Olay Hub'ları işlemleri sırasında neler olduğu hakkında bilgi, özellikle olay merkezi oluşturma, kullanılan kaynaklar ve operasyonun durumu da dahil olmak üzere işlem türü.

## <a name="diagnostic-logs-schema"></a>Tanılama günlükleri şema

Tüm günlükler JavaScript Nesne Gösterimi (JSON) biçiminde depolanır. Her giriş, aşağıdaki bölümlerde açıklanan biçimi kullanan dize alanları vardır.

### <a name="archive-logs-schema"></a>Arşiv günlükleri şema

Arşiv günlüğü JSON dizeleri aşağıdaki tabloda listelenen öğeleri içerir:

Adı | Açıklama
------- | -------
Görevadı | Başarısız olan görevin açıklaması.
Activityıd | İç kimlik, izleme için kullanılır.
trackingId | İç kimlik, izleme için kullanılır.
resourceId | Azure Kaynak Yöneticisi kaynak kimliği.
eventHub | Olay merkezi tam adı (ad alanı adı içerir).
Partitionıd | Olay Hub bölümüne yazılıyor.
arşivAdım | ArşivFlushWriter
startTime | Başarısızlık başlangıç zamanı.
Başarısızlık | Hatanın kaç kez oluştuğu.
süreInSeconds | Başarısızlık süresi.
message | Hata iletisi.
category | ArşivGünlükleri

Aşağıdaki kod bir arşiv günlüğü JSON dize örneğidir:

```json
{
   "TaskName": "EventHubArchiveUserError",
   "ActivityId": "21b89a0b-8095-471a-9db8-d151d74ecf26",
   "trackingId": "21b89a0b-8095-471a-9db8-d151d74ecf26_B7",
   "resourceId": "/SUBSCRIPTIONS/854D368F-1828-428F-8F3C-F2AFFA9B2F7D/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
   "eventHub": "fbettati-opera-eventhub:eventhub:eh123~32766",
   "partitionId": "1",
   "archiveStep": "ArchiveFlushWriter",
   "startTime": "9/22/2016 5:11:21 AM",
   "failures": 3,
   "durationInSeconds": 360,
   "message": "Microsoft.WindowsAzure.Storage.StorageException: The remote server returned an error: (404) Not Found. ---> System.Net.WebException: The remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
   "category": "ArchiveLogs"
}
```

### <a name="operational-logs-schema"></a>Operasyonel günlükler şema

Operasyonel günlük JSON dizeleri aşağıdaki tabloda listelenen öğeleri içerir:

Adı | Açıklama
------- | -------
Activityıd | Dahili kimlik, amacı izlemek için kullanılır.
EventName | İşlem adı.  
resourceId | Azure Kaynak Yöneticisi kaynak kimliği.
SubscriptionId | Abonelik Kimliği.
EventTimeString | Operasyon zamanı.
Olay Özellikleri | Çalışma özellikleri.
Durum | Operasyon durumu.
Çağıran | İşleyiş arayan (Azure portalı veya yönetim istemcisi).
category | OperationalLogs

Aşağıdaki kod, bir işlem günlüğü JSON dizesinin bir örneğidir:

```json
Example:
{
   "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
   "EventName": "Create EventHub",
   "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/SHOEBOXEHNS-CY4001",
   "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
   "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
   "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
   "Status": "Succeeded",
   "Caller": "ServiceBus Client",
   "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Sonraki adımlar
- [Etkinlik Merkezlerine Giriş](event-hubs-what-is-event-hubs.md)
- [Event Hubs API’sine genel bakış](event-hubs-api-overview.md)
- Event Hubs kullanmaya başlayın
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [Javascript](get-started-java-send-v2.md)
