---
title: Azure API Yönetimi'nde etkinlikleri Azure Etkinlik Hub'larına kaydetme | Microsoft Dokümanlar
description: Azure API Yönetimi'nde etkinlikleri Azure Etkinlik Hub'larına nasıl günlüğe kaydedebilirsiniz öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/29/2018
ms.author: apimpm
ms.openlocfilehash: 2f67079938ddcf4a65e01ef50ab7e5cdf7078b73
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260947"
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Azure API Yönetimi'nde etkinlikleri Azure Etkinlik Hub'larına kaydetme
Azure Event Hubs, bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki verileri işleyip analiz edebilmeniz için saniye başına milyonlarca olayı işleyebilen ileri düzeyde ölçeklenebilir bir veri alım sistemidir. Olay Hub'ları bir olay ardışık alanı için "ön kapı" görevi görür ve veriler bir olay hub'ına toplandıktan sonra, herhangi bir gerçek zamanlı analiz sağlayıcısı veya toplu işleme/depolama bağdaştırıcıları kullanılarak dönüştürülebilir ve depolanabilir. Event Hubs olay akışı üretimlerini bu olayların tüketilmesinden ayırır, böylece olay tüketicileri olaylara kendi zamanlamalarında erişebilir.

Bu makale, Azure [API Yönetimini Olay Hub'ları videosuyla tümleştir'e](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) eşlik eder ve Azure Olay Hub'larını kullanarak API Yönetimi etkinliklerini nasıl günlüğe kaydedeceğimiz açıklanır.

## <a name="create-an-azure-event-hub"></a>Azure Olay Hub’ı oluşturma

Etkinlik Hub'ına ve Olay Hub'ına olay hub'ı gönderip almanız gereken bağlantı dizelerini nasıl oluşturabileceğinize ilişkin ayrıntılı adımlar için Azure [portalını kullanarak etkinlik hub'ları ad alanı ve etkinlik hub'ı oluştur'a](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)bakın.

## <a name="create-an-api-management-logger"></a>API Yönetimi kaydedici oluşturma
Artık bir Olay Hub'ınız olduğuna göre, bir sonraki adım, olayları Olay Hub'ına günlüğe kaydedebilmek için API Yönetimi hizmetinizde bir [Logger'ı](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/logger) yapılandırmaktır.

API Yönetimi [kaydediciler, API Yönetimi REST API](https://aka.ms/apimapi)kullanılarak yapılandırılır. Ayrıntılı istek örnekleri için [Loggers'ın nasıl oluşturulabildiğini](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/logger/createorupdate)görün.

## <a name="configure-log-to-eventhubs-policies"></a>Günlük-olay hub'ları ilkelerini yapılandırma

Logger'ınız API Yönetimi'nde yapılandırıldıktan sonra, log-to-eventhubs ilkelerinizi istenen olayları günlüğe kaydetmek üzere yapılandırabilirsiniz. Giriş-olay hub'ları ilkesi gelen ilke bölümünde veya giden ilke bölümünde kullanılabilir.

1. APIM örneğinize göz atın.
2. API sekmesini seçin.
3. İlke eklemek istediğiniz API'yi seçin. Bu örnekte, **Sınırsız** üründeki **Echo API'ye** bir ilke ekliyoruz.
4. **Tüm işlemler**’i seçin.
5. Ekranın üst kısmında Tasarım sekmesini seçin.
6. Gelen veya Giden işleme penceresinde üçgeni (kalemin yanında) tıklatın.
7. Kod düzenleyicisini seçin. Daha fazla bilgi için [ilkeleri nasıl ayarlayınız veya nasıl dizeedin.](set-edit-policies.md)
8. İmlecinizi veya `outbound` `inbound` ilke bölümüne yerleştirin.
9. Sağdaki pencerede, **Gelişmiş ilkeler** > **Oturum Aç'ı seçin EventHub'a**. Bu, ilke `log-to-eventhub` bildirimi şablonu ekler.

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```
Önceki `logger-id` adımda kaydedicioluşturmak `{new logger name}` için URL'de kullandığınız değeri değiştirin.

Öğenin değeri `log-to-eventhub` olarak dize döndüren herhangi bir ifade kullanabilirsiniz. Bu örnekte, tarih ve saat, hizmet adı, istek kimliği, istek ip adresi ve işlem adını içeren bir dize günlüğe kaydedilir.

Güncelleştirilmiş ilke yapılandırmasını kaydetmek için **Kaydet'i** tıklatın. Kaydedilir kaydedilmez ilke etkin olur ve olaylar belirlenen Olay Hub'ına günlüğe kaydedilir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Etkinlik Hub'ları hakkında daha fazla bilgi edinin
  * [Azure Etkinlik Hub'ları ile başlayın](../event-hubs/event-hubs-c-getstarted-send.md)
  * [EventProcessorHost bulunan iletiler alma](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programlama kılavuzu](../event-hubs/event-hubs-programming-guide.md)
* API Yönetimi ve Olay Hub'ları entegrasyonu hakkında daha fazla bilgi edinin
  * [Logger varlık başvurusu](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/logger)
  * [log-to-eventhub ilke başvurusu](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [API'lerinizi Azure API Yönetimi, Etkinlik Hub'ları ve Moesif ile izleme](api-management-log-to-eventhub-sample.md)  
* [Azure Uygulama Öngörüleri ile tümleştirme](api-management-howto-app-insights.md) hakkında daha fazla bilgi edinin

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
