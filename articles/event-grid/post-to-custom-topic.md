---
title: Olayı özel Azure Olay Ağı konusuna gönder
description: Bu makalede, bir olay özel bir konuya nasıl gönderilen açıklanmaktadır. Gönderi ve olay verilerinin biçimini gösterir.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 0afad249f71a36bf7552da499e985b68d48ee7a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721566"
---
# <a name="post-to-custom-topic-for-azure-event-grid"></a>Azure Olay Idamı için özel konuya gönderme

Bu makalede, bir olay özel bir konuya nasıl gönderilen açıklanmaktadır. Gönderi ve olay verilerinin biçimini gösterir. [Hizmet Düzeyi Sözleşmesi (SLA),](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/) yalnızca beklenen biçimle eşleşen gönderiler için geçerlidir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="endpoint"></a>Uç Nokta

HTTP POST'u özel bir konuya gönderirken `https://<topic-endpoint>?api-version=2018-01-01`URI biçimini kullanın: .

Örneğin, geçerli bir URI: `https://exampletopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01`.

Azure CLI ile özel bir konu için bitiş noktasını elde etmek için şunları kullanın:

```azurecli-interactive
az eventgrid topic show --name <topic-name> -g <topic-resource-group> --query "endpoint"
```

Azure PowerShell ile özel bir konu için bitiş noktasını elde etmek için şunları kullanın:

```powershell
(Get-AzEventGridTopic -ResourceGroupName <topic-resource-group> -Name <topic-name>).Endpoint
```

## <a name="header"></a>Üst bilgi

İstekte, kimlik doğrulaması için `aeg-sas-key` bir anahtar içeren adlandırılmış bir üstbilgi değeri ekleyin.

Örneğin, geçerli bir üstbilgi `aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==`değeri.

Azure CLI ile özel bir konunun anahtarını almak için şunları kullanın:

```azurecli
az eventgrid topic key list --name <topic-name> -g <topic-resource-group> --query "key1"
```

PowerShell ile özel bir konunun anahtarını almak için şunları kullanın:

```powershell
(Get-AzEventGridTopicKey -ResourceGroupName <topic-resource-group> -Name <topic-name>).Key1
```

## <a name="event-data"></a>Olay verileri

Özel konular için, üst düzey veriler standart kaynak tanımlı olaylarla aynı alanları içerir. Bu özelliklerden biri, özel konuya özgü özellikleri içeren bir veri özelliğidir. Olay yayımcısı olarak, bu veri nesnesinin özelliklerini siz belirlersiniz. Aşağıdaki şema kullanın:

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

Bu özelliklerin açıklaması için [Azure Olay Ağı etkinliği şemasına](event-schema.md)bakın. Olayları bir olay ızgarası konusuna gönderirken, dizinin toplam boyutu 1 MB'a kadar olabilir. Dizideki her olay 64 KB (Genel Kullanılabilirlik) veya 1 MB (önizleme) ile sınırlıdır.

> [!NOTE]
> 64 KB'ye kadar boyut landırma olayı Genel Kullanılabilirlik (GA) Hizmet Düzeyi Sözleşmesi (SLA) kapsamındadır. 1 MB'a kadar boyutlandırma etkinliği desteği şu anda önizlemededir. 64 KB üzerindeki olaylar 64-KB artışlarla ücretlendirilir. 

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

Konu bitiş noktasına gönderdikten sonra bir yanıt alırsınız. Yanıt standart bir HTTP yanıt kodudur. Bazı yaygın yanıtlar şunlardır:

|Sonuç  |Yanıt  |
|---------|---------|
|Başarılı  | 200 TAMAM  |
|Olay verilerinin yanlış biçimi vardır | 400 Kötü İstek |
|Geçersiz erişim anahtarı | 401 Yetkisiz |
|Yanlış bitiş noktası | 404 Bulunamadı |
|Dizi veya olay boyut sınırlarını aşıyor | 413 Yük Çok Büyük |

Hatalar için ileti gövdesi aşağıdaki biçime sahiptir:

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

* Olay teslimlerini izleme hakkında daha fazla bilgi için [Bkz.](monitor-event-delivery.md)
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulama](security-authentication.md)sı'na bakın.
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
