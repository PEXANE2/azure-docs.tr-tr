---
title: Etkinliği özel Azure Event Grid konusuna gönder
description: Bu makalede, özel bir konuya bir olay nasıl nakledeceğiniz açıklanmaktadır. Gönderi ve olay verilerinin biçimini gösterir.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 0afad249f71a36bf7552da499e985b68d48ee7a9
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721566"
---
# <a name="post-to-custom-topic-for-azure-event-grid"></a>Azure Event Grid için özel konuya gönderi gönder

Bu makalede, özel bir konuya bir olay nasıl nakledeceğiniz açıklanmaktadır. Gönderi ve olay verilerinin biçimini gösterir. [Hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/) yalnızca beklenen biçimle eşleşen gönderimler için geçerlidir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="endpoint"></a>Uç Nokta

HTTP GÖNDERISINI özel bir konuya gönderirken, URI biçimini kullanın: `https://<topic-endpoint>?api-version=2018-01-01`.

Örneğin, geçerli bir URI: `https://exampletopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01`.

Azure CLı ile özel bir konu için uç noktayı almak üzere şunu kullanın:

```azurecli-interactive
az eventgrid topic show --name <topic-name> -g <topic-resource-group> --query "endpoint"
```

Azure PowerShell bir özel konu için uç noktayı almak için şunu kullanın:

```powershell
(Get-AzEventGridTopic -ResourceGroupName <topic-resource-group> -Name <topic-name>).Endpoint
```

## <a name="header"></a>Üst bilgi

İstekte, kimlik doğrulaması için bir anahtar içeren `aeg-sas-key` adlı bir başlık değeri ekleyin.

Örneğin, geçerli bir üst bilgi değeri `aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==`.

Azure CLı ile özel bir konunun anahtarını almak için şunu kullanın:

```azurecli
az eventgrid topic key list --name <topic-name> -g <topic-resource-group> --query "key1"
```

PowerShell ile özel bir konunun anahtarını almak için şunu kullanın:

```powershell
(Get-AzEventGridTopicKey -ResourceGroupName <topic-resource-group> -Name <topic-name>).Key1
```

## <a name="event-data"></a>Olay verileri

Özel konularda, en üst düzey veriler standart kaynak tanımlı olaylarla aynı alanları içerir. Bu özelliklerden biri, özel konuya özgü özellikleri içeren bir veri özelliğidir. Olay yayımcısı olarak, bu veri nesnesinin özelliklerini belirlersiniz. Aşağıdaki şemayı kullanın:

```json
[
  {
    "id": string,    
    "eventType": string,
    "subject": string,
    "eventTime": string-in-date-time-format,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string
  }
]
```

Bu özelliklerin açıklaması için bkz. [olay şeması Azure Event Grid](event-schema.md). Olayları bir Event Grid konusuna naklederken, dizi toplam boyutu 1 MB olabilir. Dizideki her olay 64 KB (genel kullanılabilirlik) veya 1 MB (Önizleme) ile sınırlıdır.

> [!NOTE]
> 64 KB 'a kadar olan bir olay, genel kullanım (GA) Hizmet Düzeyi Sözleşmesi (SLA) ile ele alınmıştır. 1 MB 'a kadar olan bir olay desteği şu anda önizleme aşamasındadır. 64 KB üzerindeki olaylar 64 KB 'lik artışlarla ücretlendirilir. 

Örneğin, geçerli bir olay veri şeması:

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0"
}]
```

## <a name="response"></a>Yanıt

Konu uç noktasına gönderdikten sonra bir yanıt alırsınız. Yanıt standart bir HTTP yanıt kodudur. Bazı genel yanıtlar şunlardır:

|Sonuç  |Yanıt  |
|---------|---------|
|Başarılı  | 200 TAMAM  |
|Olay verileri hatalı biçime sahip | 400 Hatalı Istek |
|Geçersiz erişim anahtarı | 401 Yetkisiz |
|Geçersiz uç nokta | 404 bulunamadı |
|Dizi veya olay boyut sınırlarını aşıyor | 413 yükü çok büyük |

Hatalar için ileti gövdesi aşağıdaki biçimdedir:

```json
{
    "error": {
        "code": "<HTTP status code>",
        "message": "<description>",
        "details": [{
            "code": "<HTTP status code>",
            "message": "<description>"
    }]
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimatlarını izleme hakkında bilgi için bkz. [izleyici Event Grid ileti teslimi](monitor-event-delivery.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz. [Event Grid Security and Authentication](security-authentication.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
