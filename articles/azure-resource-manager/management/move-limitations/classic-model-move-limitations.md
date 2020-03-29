---
title: Azure Klasik dağıtım kaynaklarını taşıma
description: Klasik dağıtım kaynaklarını yeni bir kaynak grubuna veya aboneye taşımak için Azure Kaynak Yöneticisi'ni kullanın.
ms.topic: conceptual
ms.date: 07/09/2019
ms.openlocfilehash: 78b9769a31fa0c96c12e18d05cb9c484aa52a1d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75485292"
---
# <a name="move-guidance-for-classic-deployment-model-resources"></a>Klasik dağıtım modeli kaynakları için kılavuzu taşıma

Klasik modelde dağıtılan kaynakları taşıma adımları, kaynakları abonelik içinde mi yoksa yeni bir aboneye mi taşıyorsanız farklı olarak değişir.

## <a name="move-in-the-same-subscription"></a>Aynı abonelikte taşıma

Kaynakları aynı abonelik içinde bir kaynak grubundan başka bir kaynak grubuna taşımakta aşağıdaki kısıtlamalar uygulanır:

* Sanal ağlar (klasik) hareket ettirilemiyor.
* Sanal makineler (klasik) bulut hizmeti ile taşınmalıdır.
* Bulut hizmeti yalnızca taşıma tüm sanal makinelerini içerdiğinde taşınabilir.
* Aynı anda yalnızca bir bulut hizmeti taşınabilir.
* Aynı anda yalnızca bir depolama hesabı (klasik) taşınabilir.
* Depolama hesabı (klasik) sanal bir makine veya bulut hizmetiyle aynı işlemde taşıılamaz.

Klasik kaynakları aynı abonelik içinde yeni bir kaynak grubuna taşımak için portal, Azure PowerShell, Azure CLI veya REST API'de [standart taşıma işlemlerini](../move-resource-group-and-subscription.md) kullanın. Kaynak Yöneticisi kaynaklarını taşımak için kullandığınız işlemleri kullanırsınız.

## <a name="move-across-subscriptions"></a>Abonelikler arasında taşıma

Kaynakları yeni bir aboneye taşımak ta aşağıdaki kısıtlamalar geçerlidir:

* Abonelikteki tüm klasik kaynakların aynı işlemde taşınması gerekir.
* Hedef aboneliğin başka klasik kaynakları olmamalıdır.
* Hareket yalnızca klasik hareketler için ayrı bir REST API ile istenebilir. Standart Kaynak Yöneticisi taşıma komutları, klasik kaynakları yeni bir aboneye taşırken çalışmaz.

Klasik kaynakları yeni bir aboneye taşımak için, klasik kaynaklara özgü REST işlemlerini kullanın. REST'i kullanmak için aşağıdaki adımları yapın:

1. Kaynak aboneliğin çapraz abonelik hareketine katılıp katılamayacağını kontrol edin. Aşağıdaki işlemi kullanın:

   ```HTTP
   POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     İstek gövdesinde şunlar yer almaktadır:

   ```json
   {
    "role": "source"
   }
   ```

     Doğrulama işlemi için yanıt aşağıdaki biçimdedir:

   ```json
   {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
   }
   ```

1. Hedef aboneliğin çapraz abonelik hareketine katılıp katılamayacağını kontrol edin. Aşağıdaki işlemi kullanın:

   ```HTTP
   POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     İstek gövdesinde şunlar yer almaktadır:

   ```json
   {
    "role": "target"
   }
   ```

     Yanıt, kaynak abonelik doğrulamayla aynı biçimdedir.
1. Her iki abonelik de doğrulamayı geçerse, aşağıdaki işlemle tüm klasik kaynakları bir abonelikten başka bir aboneye taşıyın:

   ```HTTP
   POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
   ```

    İstek gövdesinde şunlar yer almaktadır:

   ```json
   {
    "target": "/subscriptions/{target-subscription-id}"
   }
   ```

İşlem birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Klasik kaynakları taşımakta sorun yaşıyorsanız [Destek'e](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)başvurun.

Komutların kaynakları taşıması [için](../move-resource-group-and-subscription.md)bkz.
