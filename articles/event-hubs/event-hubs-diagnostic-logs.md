---
title: Tanılama günlüklerini ayarlama-Azure Olay Hub 'ı | Microsoft Docs
description: Azure 'da Olay Hub 'ları için etkinlik günlüklerini ve tanılama günlüklerini ayarlamayı öğrenin.
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
ms.date: 04/28/2020
ms.author: shvija
ms.openlocfilehash: 3010ee7b996c9d3e96082edeb9447c960da321bd
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82509811"
---
# <a name="set-up-diagnostic-logs-for-an-azure-event-hub"></a>Azure olay hub'ı için tanılama günlüklerini ayarlama

Azure Event Hubs için iki tür günlük görüntüleyebilirsiniz:

* **[Etkinlik günlükleri](../azure-monitor/platform/platform-logs-overview.md)**: Bu günlüklerde, bir işte yapılan işlemlerle ilgili bilgiler vardır. Günlükler her zaman etkindir. Azure portal olay hub 'ı ad alanınız için sol bölmedeki **etkinlik günlüğü** ' ni seçerek etkinlik günlüğü girdilerini görebilirsiniz. Örneğin: "ad alanı oluştur veya güncelleştir", "Olay Hub 'ı oluştur veya güncelleştir".

    ![Event Hubs ad alanı için etkinlik günlüğü](./media/event-hubs-diagnostic-logs/activity-log.png)
* **[Tanılama günlükleri](../azure-monitor/platform/platform-logs-overview.md)**: bir işle gerçekleşen her şeyin daha zengin bir görünümü için tanılama günlüklerini yapılandırabilirsiniz. Tanılama günlükleri işi çalışırken gerçekleşen güncelleştirmeler ve etkinlikler dahil olmak üzere işin oluşturulduğu zamandan itibaren etkinlikleri kapsar.

    Aşağıdaki bölümde bir Event Hubs ad alanı için tanılama günlüklerinin nasıl etkinleştirileceği gösterilmektedir.

## <a name="enable-diagnostic-logs"></a>Tanılama günlüklerini etkinleştirme
Tanılama günlükleri varsayılan olarak devre dışıdır. Tanılama günlüklerini etkinleştirmek için şu adımları izleyin:

1.  [Azure Portal](https://portal.azure.com), Event Hubs ad alanına gidin. 
2. Sol bölmede **izleme** altında **Tanılama ayarları** ' nı seçin ve **+ Tanılama ayarı Ekle**' yi seçin. 

    ![Tanılama ayarları sayfası-tanılama ayarı Ekle](./media/event-hubs-diagnostic-logs/diagnostic-settings-page.png)
4. **Kategori ayrıntıları** bölümünde, etkinleştirmek istediğiniz **tanılama günlüğü türlerini** seçin. Bu kategorilerin ilerleyen kısımlarında bu kategorilerin ayrıntılarını bulabilirsiniz. 
5. **Hedef ayrıntıları** bölümünde istediğiniz arşiv hedefini (hedef) ayarlayın; Örneğin, bir depolama hesabı, Olay Hub 'ı veya Log Analytics çalışma alanı.

    ![Tanılama ayarları ekleme sayfası](./media/event-hubs-diagnostic-logs/aDD-diagnostic-settings-page.png)
6.  Tanılama ayarlarını kaydetmek için araç çubuğunda **Kaydet** ' i seçin.

    Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Bundan sonra, **tanılama günlükleri** bölmesinde Günlükler yapılandırılan arşiv hedefinde görüntülenir.

    Tanılamayı yapılandırma hakkında daha fazla bilgi için bkz. [Azure tanılama günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md).

## <a name="diagnostic-logs-categories"></a>Tanılama günlükleri kategorileri

Event Hubs aşağıdaki kategoriler için tanılama günlüklerini yakalar:

- **Arşiv günlükleri**: Arşiv hatalarıyla ilgili olarak, özellikle de Event Hubs arşivleri ile ilgili Günlükler.
- **Işletimsel Günlükler**: Olay Hub 'ı oluşturma, kullanılan kaynaklar ve işlemin durumu dahil olmak üzere Event Hubs işlemleri sırasında neler olduğunu, özellikle de işlem türünü içeren bilgiler.
- **Otomatik ölçeklendirme günlükleri**: Event Hubs ad alanı üzerinde gerçekleştirilen otomatik ölçeklendirme işlemleri hakkında bilgi. 
- **Kafka Coordinator günlükleri** -Event Hubs ilgili Kafka Coordinator işlemleri hakkında bilgi. 
- **Kafka Kullanıcı günlükleri**: Event Hubs Ile ilgili Kafka Kullanıcı işlemleri hakkında bilgiler. 
- **Sanal ağ (VNet) bağlantı olayı Event Hubs**: Event Hubs sanal ağ bağlantısı olayları hakkında bilgi. 
- **Müşteri tarafından yönetilen anahtar Kullanıcı günlükleri**: müşterinin yönettiği anahtarla ilgili işlemler hakkında bilgi. 


    Tüm Günlükler JavaScript Nesne Gösterimi (JSON) biçiminde depolanır. Her girdinin aşağıdaki bölümlerde açıklanan biçimi kullanan dize alanları vardır.

## <a name="archive-logs-schema"></a>Arşiv günlükleri şeması

Arşiv günlüğü JSON dizeleri aşağıdaki tabloda listelenen öğeleri içerir:

Adı | Açıklama
------- | -------
Silinecek | Başarısız olan görevin açıklaması.
Etkinlik kimliği | İzleme için kullanılan iç KIMLIK.
Trackingıd | İzleme için kullanılan iç KIMLIK.
resourceId | Azure Resource Manager kaynak KIMLIĞI.
eventHub | Olay Hub 'ı tam adı (ad alanı adını içerir).
PartitionID | Yazılan olay hub 'ı bölümü.
archiveStep | ArchiveFlushWriter
startTime | Hata başlangıç zamanı.
kesil | Hatanın oluşma sayısı.
durationInSeconds | Başarısızlık süresi.
message | Hata iletisi.
category | ArchiveLogs

Aşağıdaki kod arşiv günlüğü JSON dizesinin bir örneğidir:

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

## <a name="operational-logs-schema"></a>İşletimsel Günlükler şeması

İşletimsel günlük JSON dizeleri aşağıdaki tabloda listelenen öğeleri içerir:

Adı | Açıklama
------- | -------
Etkinlik kimliği | Amacı izlemek için kullanılan iç KIMLIK.
EventName | İşlem adı.  
resourceId | Azure Resource Manager kaynak KIMLIĞI.
SubscriptionId | Abonelik KIMLIĞI.
EventTimeString | İşlem süresi.
EventProperties | İşlem özellikleri.
Durum | İşlem durumu.
Çağıran | İşlem (Azure portal veya yönetim istemcisi) çağırıcısı.
category | OperationalLogs

Aşağıdaki kod, işletimsel günlük JSON dizesinin bir örneğidir:

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

## <a name="autoscale-logs-schema"></a>Otomatik ölçeklendirme günlüğü şeması
Otomatik ölçeklendirme günlüğü JSON, aşağıdaki tabloda listelenen öğeleri içerir:

| Adı | Açıklama |
| ---- | ----------- | 
| Trackingıd | İzleme amacıyla kullanılan iç KIMLIK |
| resourceId | Azure abonelik KIMLIĞI ve ad alanı adı içeren iç KIMLIK |
| message | Otomatik Şişir eylemiyle ilgili ayrıntıları sağlayan bilgilendirici ileti. İleti, belirli bir ad alanı için önceki ve geçerli üretilen iş birimi değerini ve TU 'nın inmi tetikleneceğini içerir. |

## <a name="kafka-coordinator-logs-schema"></a>Kafka Coordinator günlükleri şeması
Kafka Coordinator log JSON, aşağıdaki tabloda listelenen öğeleri içerir:

| Adı | Açıklama |
| ---- | ----------- | 
| No | izleme amacıyla kullanılan istek KIMLIĞI |
| resourceId | Azure abonelik KIMLIĞI ve ad alanı adı içeren iç KIMLIK |
| operationName | Grup düzenlemesi sırasında gerçekleştirilen işlemin adı |
| clientId | İstemci Kimliği |
| Uz | Ad alanı adı | 
| subscriptionId | Azure abonelik KIMLIĞI |
| message | Tüketici grubu düzenlemesi sırasında yapılan eylemlerle ilgili ayrıntıları sağlayan bilgilendirici ileti. |

## <a name="kafka-user-error-logs-schema"></a>Kafka Kullanıcı hatası günlüğü şeması
Kafka Kullanıcı hatası günlüğü JSON, aşağıdaki tabloda listelenen öğeleri içerir:

| Adı | Açıklama |
| ---- | ----------- |
| Trackingıd | izleme amacıyla kullanılan izleme KIMLIĞI. |
| Uz | Ad alanı adı |
| eventhub | Olay hub'ı adı |
| PartitionID | Bölüm Kimliği |
| groupId | Grup Kimliği |
| ClientId | İstemci Kimliği |
| resourceId | Azure abonelik KIMLIĞI ve ad alanı adı içeren iç KIMLIK |
| message | Bir hata hakkındaki ayrıntıları sağlayan bilgilendirici ileti |

## <a name="event-hubs-virtual-network-connection-event-schema"></a>Event Hubs sanal ağ bağlantısı olay şeması

Event Hubs sanal ağ (VNet) bağlantı olayı JSON aşağıdaki tabloda listelenen öğeleri içerir:

| Adı | Açıklama |
| ---  | ----------- | 
| subscriptionId | Azure abonelik KIMLIĞI |
| Uz | Ad alanı adı |
| Belirlenemiyor | Event Hubs hizmetine bağlanan bir istemcinin IP adresi |
| action | Bağlantı istekleri değerlendirilirken Event Hubs hizmeti tarafından gerçekleştirilen eylem. Desteklenen eylemler **Acceptconnection** ve **RejectConnection**' dir. |
| reason | Eylemin neden yapıldığını bir neden sağlar |
| count | Verilen eylem için oluşum sayısı |
| resourceId | Abonelik KIMLIĞI ve ad alanı adı içeren iç kaynak KIMLIĞI. |

## <a name="customer-managed-key-user-logs"></a>Müşteri tarafından yönetilen anahtar Kullanıcı günlükleri
Müşteri tarafından yönetilen anahtar Kullanıcı günlüğü JSON, aşağıdaki tabloda listelenen öğeleri içerir:

| Adı | Açıklama |
| ---- | ----------- | 
| category | İleti kategorisi türü. Şu değerlerden biri: **hata** ve **bilgi** |
| resourceId | Azure abonelik KIMLIĞI ve ad alanı adı içeren iç kaynak KIMLIĞI |
| keyVault | Key Vault kaynağın adı |
| anahtar | Key Vault anahtarının adı. |
| version | Key Vault anahtarının sürümü |
| çalışmasını | İsteklere hizmeti sağlamak için yapılan bir işlemin adı |
| kod | Durum kodu |
| message | Bir hata veya bilgilendirici iletiyle ilgili ayrıntıları sağlayan ileti |



## <a name="next-steps"></a>Sonraki adımlar
- [Event Hubs giriş](event-hubs-what-is-event-hubs.md)
- [Event Hubs API’sine genel bakış](event-hubs-api-overview.md)
- Event Hubs kullanmaya başlayın
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [JavaScript](get-started-java-send-v2.md)
