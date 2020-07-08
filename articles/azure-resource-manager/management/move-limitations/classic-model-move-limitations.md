---
title: Klasik Azure dağıtım kaynaklarını taşıma
description: Klasik dağıtım kaynaklarını yeni bir kaynak grubuna veya aboneliğe taşımak için Azure Resource Manager kullanın.
ms.topic: conceptual
ms.date: 07/09/2019
ms.openlocfilehash: 78b9769a31fa0c96c12e18d05cb9c484aa52a1d5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75485292"
---
# <a name="move-guidance-for-classic-deployment-model-resources"></a>Klasik dağıtım modeli kaynakları için kılavuzu taşıma

Klasik model aracılığıyla dağıtılan kaynakları taşıma adımları, kaynakları bir abonelik içinde veya yeni bir aboneliğe taşısanız göre farklılık gösterir.

## <a name="move-in-the-same-subscription"></a>Aynı abonelikte taşı

Kaynakları bir kaynak grubundan aynı abonelik içindeki başka bir kaynak grubuna taşırken aşağıdaki kısıtlamalar geçerlidir:

* Sanal ağlar (klasik) taşınamaz.
* Sanal makineler (klasik), bulut hizmetiyle birlikte taşınmalıdır.
* Bulut hizmeti yalnızca taşıma tüm sanal makinelerini içerdiğinde taşınabilir.
* Tek seferde yalnızca bir bulut hizmeti taşınabilir.
* Tek seferde yalnızca bir depolama hesabı (klasik) taşınabilir.
* Depolama hesabı (klasik), bir sanal makine veya bulut hizmeti ile aynı işlemde taşınamaz.

Klasik kaynakları aynı abonelik içindeki yeni bir kaynak grubuna taşımak için Portal, Azure PowerShell, Azure CLı veya REST API aracılığıyla [Standart taşıma işlemlerini](../move-resource-group-and-subscription.md) kullanın. Kaynak Yöneticisi kaynaklarını taşımak için kullandığınız işlemleri kullanırsınız.

## <a name="move-across-subscriptions"></a>Abonelikler arasında geçiş

Kaynakları yeni bir aboneliğe taşırken aşağıdaki kısıtlamalar geçerlidir:

* Abonelikteki tüm klasik kaynaklar aynı işlemde taşınmalıdır.
* Hedef aboneliğin klasik başka bir kaynağı olmamalıdır.
* Taşıma yalnızca klasik taşıma için ayrı bir REST API istenebilir. Klasik kaynakları yeni bir aboneliğe taşırken standart Kaynak Yöneticisi taşıma komutları çalışmaz.

Klasik kaynakları yeni bir aboneliğe taşımak için, Klasik kaynaklara özgü REST işlemlerini kullanın. REST 'i kullanmak için aşağıdaki adımları uygulayın:

1. Kaynak aboneliğin çapraz abonelik taşımasına katılıp katılamayacağını denetleyin. Aşağıdaki işlemi kullanın:

   ```HTTP
   POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     İstek gövdesinde şunları dahil edin:

   ```json
   {
    "role": "source"
   }
   ```

     Doğrulama işleminin yanıtı aşağıdaki biçimdedir:

   ```json
   {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
   }
   ```

1. Hedef aboneliğin bir çapraz abonelik taşımasına katılıp katılamayacağını kontrol edin. Aşağıdaki işlemi kullanın:

   ```HTTP
   POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     İstek gövdesinde şunları dahil edin:

   ```json
   {
    "role": "target"
   }
   ```

     Yanıt, kaynak abonelik doğrulamayla aynı biçimde.
1. Her iki abonelik da doğrulamayı geçtiğinde, tüm klasik kaynakları bir aboneliğden başka bir aboneliğe aşağıdaki işlemle taşıyın:

   ```HTTP
   POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
   ```

    İstek gövdesinde şunları dahil edin:

   ```json
   {
    "target": "/subscriptions/{target-subscription-id}"
   }
   ```

İşlem birkaç dakika çalışabilir.

## <a name="next-steps"></a>Sonraki adımlar

Klasik kaynakları taşırken sorun yaşarsanız [desteğe](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)başvurun.

Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../move-resource-group-and-subscription.md).
