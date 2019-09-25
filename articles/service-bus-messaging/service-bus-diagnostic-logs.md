---
title: Tanılama günlüklerini Azure Service Bus | Microsoft Docs
description: Azure 'da Service Bus için tanılama günlüklerini ayarlamayı öğrenin.
keywords: ''
documentationcenter: .net
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 6443cb727573645792a4e6c929b80c3406d72025
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71261800"
---
# <a name="service-bus-diagnostic-logs"></a>Tanılama günlüklerini Service Bus

Azure Service Bus için iki tür günlük görüntüleyebilirsiniz:
* **[Etkinlik günlükleri](../azure-monitor/platform/activity-logs-overview.md)** . Bu Günlükler, bir işte gerçekleştirilen işlemlerle ilgili bilgiler içerir. Günlükler her zaman etkindir.
* **[Tanılama günlükleri](../azure-monitor/platform/resource-logs-overview.md)** . Tanılama günlüklerini, bir iş içinde gerçekleşen her şey hakkında daha zengin bilgiler için yapılandırabilirsiniz. Tanılama günlüklerine yönelik güncelleştirmeleri ve iş çalışırken gerçekleşen etkinlikler dahil olmak üzere iş silinene kadar iş oluşturulur zamanından kapak etkinlikler.

## <a name="turn-on-diagnostic-logs"></a>Tanılama günlüklerini açın

Tanılama günlükleri varsayılan olarak devre dışıdır. Tanılama günlüklerini etkinleştirmek için aşağıdaki adımları uygulayın:

1.  İçinde [Azure portalında](https://portal.azure.com)altında **izleme + Yönetim**, tıklayın **tanılama günlükleri**.

    ![Tanılama günlüklerine dikey pencere gezintisi](./media/service-bus-diagnostic-logs/image1.png)

2. İzlemek istediğiniz kaynağa tıklayın.  

3.  **Tanılamayı aç**’a tıklayın.

    ![Tanılama günlüklerini aç](./media/service-bus-diagnostic-logs/image2.png)

4.  İçin **durumu**, tıklayın **üzerinde**.

    ![durum tanılama günlüklerini değiştirme](./media/service-bus-diagnostic-logs/image3.png)

5.  İstediğiniz arşiv hedefini ayarlayın; Örneğin, bir depolama hesabı, Olay Hub 'ı veya Azure Izleyici günlükleri.

6.  Yeni tanılama ayarları kaydedin.

Yeni ayarları yaklaşık 10 dakika içinde etkinleşir. Bundan sonra, **tanılama günlükleri** dikey penceresinde yapılandırılan arşiv hedefinde Günlükler görüntülenir.

Tanılama yapılandırma hakkında daha fazla bilgi için bkz. [Azure tanılama günlükleri'ne genel bakış](../azure-monitor/platform/resource-logs-overview.md).

## <a name="diagnostic-logs-schema"></a>Tanılama günlükleri şeması

Tüm günlükler, JavaScript nesne gösterimi (JSON) biçiminde depolanır. Her girdinin aşağıdaki bölümde açıklanan biçimi kullanan dize alanları vardır.

## <a name="operational-logs-schema"></a>İşlem günlüklerinde şeması

**Operationallogs** kategorisindeki Günlükler Service Bus işlemler sırasında ne olacağını yakalar. Özellikle, bu Günlükler kuyruk oluşturma, kullanılan kaynaklar ve işlemin durumu dahil olmak üzere işlem türünü yakalar.

İşlem günlüğü JSON dizelerini aşağıdaki tabloda listelenen öğeleri içerir:

Ad | Açıklama
------- | -------
Etkinlik Kimliği | İzleme için kullanılan iç KIMLIK
EventName | İşlem adı           
resourceId | Azure Resource Manager kaynak KIMLIĞI
SubscriptionId | Abonelik Kimliği
EventTimeString | İşlem süresi
EventProperties | İşlem özellikleri
Durum | İşlem durumu
Caller | İşlem (Azure portal veya yönetim istemcisi) çağıranı
category | OperationalLogs

İşte işlem günlüğü JSON dizesine bir örnek:

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Service Bus hakkında daha fazla bilgi edinmek için aşağıdaki bağlantılara bakın:

* [Service Bus giriş](service-bus-messaging-overview.md)
* [Service Bus kullanmaya başlayın](service-bus-dotnet-get-started-with-queues.md)
