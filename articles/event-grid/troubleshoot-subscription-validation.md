---
title: Azure Event Grid-Abonelik doğrulama sorunlarını giderme
description: Bu makalede, abonelik doğrulamalarında nasıl sorun giderebileceğiniz gösterilmektedir.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 48844859013507ab684ef8879b7b85dd6b6fe8cd
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86118996"
---
# <a name="troubleshoot-azure-event-grid-subscription-validations"></a>Azure Event Grid abonelik doğrulamaları sorunlarını giderme
Bu makale, olay aboneliği doğrulamaları sorunlarını giderme hakkında bilgi sağlar. 

> [!IMPORTANT]
> Web kancaları için uç nokta doğrulaması hakkında ayrıntılı bilgi için bkz. [Web kancası olay teslimi](webhook-event-delivery.md).

## <a name="validate-event-grid-event-subscription-using-postman"></a>Postman kullanarak olay aboneliğini Event Grid doğrulama
Aşağıda, bir Event Grid olayının Web kancası aboneliğini doğrulamak için Postman kullanmanın bir örneği verilmiştir: 

![Postman kullanarak olay Kılavuzu olay aboneliği doğrulaması](./media/troubleshoot-subscription-validation/event-subscription-validation-postman.png)

Örnek bir **Subscriptionvalidationevent** JSON 'si aşağıda verilmiştir:

```json
[
  {
    "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
    "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "subject": "",
    "data": {
      "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    },
    "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
    "eventTime": "2018-01-25T22:12:19.4556811Z",
    "metadataVersion": "1",
    "dataVersion": "1"
  }
]
```

Örnek başarılı yanıt aşağıda verilmiştir:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Web kancaları için Event Grid olay doğrulama hakkında daha fazla bilgi edinmek için bkz. [Event Grid olayları Ile Endpoint Validation](webhook-event-delivery.md#endpoint-validation-with-event-grid-events).


## <a name="validate-event-grid-event-subscription-using-curl"></a>Kıvrımlı kullanarak olay aboneliğini Event Grid doğrulama 
Aşağıda, bir Event Grid olayının Web kancası aboneliğini doğrulamaya yönelik örnek kıvrımlı komut verilmiştir: 

```bash
curl -X POST -d '[{"id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66","topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx","subject": "","data": {"validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"},"eventType": "Microsoft.EventGrid.SubscriptionValidationEvent","eventTime": "2018-01-25T22:12:19.4556811Z", "metadataVersion": "1","dataVersion": "1"}]' -H 'Content-Type: application/json' https://{your-webhook-url.com}
```

## <a name="validate-cloud-event-subscription-using-postman"></a>Postman kullanarak bulut olay aboneliğini doğrulama
Bir bulut olayının Web kancası aboneliğini doğrulamak için Postman kullanmanın bir örneği aşağıda verilmiştir: 

![Postman kullanarak bulut olay aboneliği doğrulaması](./media/troubleshoot-subscription-validation/cloud-event-subscription-validation-postman.png)

Bulut olaylarıyla doğrulama için **http seçenekleri** yöntemini kullanın. Web kancaları için bulut olay doğrulama hakkında daha fazla bilgi edinmek için bkz. [Cloud Events Ile Endpoint Validation](webhook-event-delivery.md#endpoint-validation-with-event-grid-events).

## <a name="error-code-403"></a>Hata kodu: 403
Web kancası yanıt karşılığında 403 (yasak) döndürüyorsa, Web kancasının bir Azure Application Gateway veya Web uygulaması güvenlik duvarının arkasında olup olmadığını denetleyin. Varsa, aşağıdaki güvenlik duvarı kurallarını devre dışı bırakmanız ve bir HTTP GÖNDERISINI yeniden yapmanız gerekir:

  - 920300 (istekte bir Accept üst bilgisi eksik, bunu çözebiliriz)
  - 942430 (kısıtlanan SQL karakter anomali algılama (args): özel karakter sayısı aşıldı (12))
  - 920230 (birden çok URL kodlaması algılandı)
  - 942130 (SQL ekleme saldırısı: SQL tautology algılandı.)
  - 931130 (olası uzaktan dosya ekleme (RFı) saldırısı = kapalı etki alanı başvurusu/bağlantı)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa sorununuzu [Stack Overflow forumuna](https://stackoverflow.com/questions/tagged/azure-eventgrid) gönderin veya bir [destek bileti](https://azure.microsoft.com/support/options/)açın. 
