---
title: Azure Event Hubs olayları Azure 'da günlüğe kaydetme | API Management | Microsoft Docs
description: Azure API Management 'da olayları Azure Event Hubs günlüğe kaydetme hakkında bilgi edinin.
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
ms.openlocfilehash: 2f07f6a27e78ee4df8c64a09918758d02c28c6d4
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76898797"
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Azure Event Hubs olayları Azure 'da günlüğe kaydetme API Management
Azure Event Hubs, bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki verileri işleyip analiz edebilmeniz için saniye başına milyonlarca olayı işleyebilen ileri düzeyde ölçeklenebilir bir veri alım sistemidir. Event Hubs, bir olay ardışık düzeni için "ön kapı" olarak görev yapar ve veriler bir olay hub 'ına toplandıktan sonra herhangi bir gerçek zamanlı analiz sağlayıcısı veya toplu işlem/depolama bağdaştırıcısı kullanılarak dönüştürülebilir ve depolanabilir. Event Hubs olay akışı üretimlerini bu olayların tüketilmesinden ayırır, böylece olay tüketicileri olaylara kendi zamanlamalarında erişebilir.

Bu makale, [azure API Management Event Hubs videoyla tümleştirmenize](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) yardımcı olur ve Azure Event Hubs kullanarak API Management olaylarının nasıl günlüğe alınacağını açıklar.

## <a name="create-an-azure-event-hub"></a>Azure Olay Hub’ı oluşturma

Olay Hub 'ı oluşturma ve Olay Hub 'ından olayları göndermeniz ve almanız gereken bağlantı dizelerini alma hakkında ayrıntılı adımlar için, bkz. [Azure Portal kullanarak bir Event Hubs ad alanı ve bir olay hub 'ı oluşturma](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="create-an-api-management-logger"></a>API Management günlükçüsü oluşturma
Artık bir olay hub 'ına sahip olduğunuza göre, bir sonraki adım, olayları Olay Hub 'ına kaydetmek için API Management hizmetinizde bir [günlükçü](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger) yapılandırmaktır.

API Management Günlükçüler [API Management REST API](https://aka.ms/apimapi)kullanılarak yapılandırılır. Ayrıntılı istek örnekleri için bkz. [Günlükçüler oluşturma](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger/createorupdate).

## <a name="configure-log-to-eventhubs-policies"></a>Günlük-eventhubs ilkelerini yapılandırma

Günlükçü API Management ' de yapılandırıldıktan sonra, günlük-eventhubs ilkelerinizi istenen olayları günlüğe kaydetmek için yapılandırabilirsiniz. Günlük-eventhubs ilkesi, gelen ilke bölümünde veya giden ilke bölümünde kullanılabilir.

1. APIM örneğinize göz atın.
2. API sekmesini seçin.
3. İlkeyi eklemek istediğiniz API 'yi seçin. Bu örnekte, **sınırsız** ürüne **yankı API** 'sine bir ilke ekliyoruz.
4. **Tüm işlemler**’i seçin.
5. Ekranın üst kısmında Tasarım sekmesini seçin.
6. Gelen veya giden işleme penceresinde üçgeni (kurşun kalem ' ın yanında) tıklatın.
7. Kod düzenleyicisini seçin. Daha fazla bilgi için bkz. [ilkeleri ayarlama veya düzenleme](set-edit-policies.md).
8. İmlecinizi `inbound` veya `outbound` ilkesi bölümüne konumlandırın.
9. Sağdaki pencerede, **gelişmiş > ilkeler** ' i seçerek EventHub ' **ye**tıklayın. Bu, `log-to-eventhub` ilkesi ekstresi şablonunu ekler.

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```
`logger-id`, önceki adımda günlükçü oluşturmak için URL 'deki `{new logger name}` için kullandığınız değerle değiştirin.

`log-to-eventhub` öğesi için değer olarak bir dize döndüren herhangi bir ifadeyi kullanabilirsiniz. Bu örnekte, tarih ve saat, hizmet adı, istek kimliği, istek IP adresi ve işlem adı içeren bir dize günlüğe kaydedilir.

Güncelleştirilmiş ilke yapılandırmasını kaydetmek için **Kaydet** ' e tıklayın. Kaydedilen ilke etkin olur ve olaylar belirlenen Olay Hub 'ına kaydedilir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Event Hubs hakkında daha fazla bilgi
  * [Azure Event Hubs kullanmaya başlama](../event-hubs/event-hubs-c-getstarted-send.md)
  * [EventProcessorHost ile ileti alma](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programlama kılavuzu](../event-hubs/event-hubs-programming-guide.md)
* API Management ve Event Hubs tümleştirmesi hakkında daha fazla bilgi edinin
  * [Günlükçü varlık başvurusu](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger)
  * [günlük-eventhub ilke başvurusu](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Azure API Management, Event Hubs ve Moesif ile API 'lerinizi izleme](api-management-log-to-eventhub-sample.md)  
* [Azure Application Insights ile tümleştirme](api-management-howto-app-insights.md) hakkında daha fazla bilgi edinin

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
