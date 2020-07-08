---
title: Giden IP adresi değişikliğine hazırlanma
description: Giden IP adresiniz değiştirilebiliyorsa, uygulamanızın değişiklikten sonra çalışmaya devam etmesi için ne yapılacağını öğrenin.
ms.topic: article
ms.date: 06/28/2018
ms.custom: seodec18
ms.openlocfilehash: 2be4bc92dde278b054bd04f412f937440027ece7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74671676"
---
# <a name="how-to-prepare-for-an-outbound-ip-address-change"></a>Giden IP adresi değişikliğine hazırlanma

Azure App Service uygulamanızın giden IP adreslerinin değiştiğini belirten bir bildirim aldıysanız, bu makaledeki yönergeleri izleyin.

## <a name="determine-if-you-have-to-do-anything"></a>Herhangi bir şey yapmanız gerekip gerekmediğini belirleme

* Seçenek 1: App Service uygulamanız IP filtreleme, açık bir ekleme listesi veya yönlendirme veya güvenlik duvarı gibi giden trafiğin özel bir işlemesini kullanmıyorsa, hiçbir eylem gerekmez.

* Seçenek 2: uygulamanızın giden IP adresleri için özel işleme varsa (aşağıdaki örneklere bakın), mevcut olanların her yerde yeni giden IP adreslerini ekleyin. Mevcut IP adreslerini değiştirme. Sonraki bölümdeki yönergeleri izleyerek yeni giden IP adreslerini bulabilirsiniz.

  Örneğin, bir giden IP adresi uygulamanızın dışında bir güvenlik duvarına açık bir şekilde dahil edilebilir veya bir dış ödeme hizmeti, uygulamanızın giden IP adresini içeren izin verilen bir listeye sahip olabilir. Giden adresiniz, uygulamanızın dışında herhangi bir yerde bir listede yapılandırılmışsa, bunun değişmesi gerekir.

## <a name="find-the-outbound-ip-addresses-in-the-azure-portal"></a>Azure portal giden IP adreslerini bulma

Yeni giden IP adresleri, portalda etkili olmadan önce görüntülenir. Azure yeni olanları kullanmaya başladığında, eski olanlar artık kullanılmaz. Tek seferde yalnızca bir küme kullanılır, bu nedenle ekleme listelerindeki girişlerin, anahtar gerçekleştiğinde kesinti oluşmasını engellemek için hem eski hem de yeni IP adreslerine sahip olması gerekir. 

1.  [Azure Portal](https://portal.azure.com)açın.

2.  Sol taraftaki gezinti menüsünde **uygulama hizmetleri**' ni seçin.

3.  Listeden App Service uygulamanızı seçin.

1.  Uygulama bir işlev uygulamadır, bkz. [işlev uygulaması gıden IP adresleri](../azure-functions/ip-addresses.md#find-outbound-ip-addresses).

4.  **Ayarlar** üst bilgisinde, sol gezinti bölmesinde **Özellikler** ' e tıklayın ve **giden IP adresleri**etiketli bölümü bulun.

5. IP adreslerini kopyalayın ve bir filtre veya izin verilenler listesi gibi giden trafiğinizi özel işleme ekleyin. Listedeki mevcut IP adreslerini silmeyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure tarafından başlatılan bir IP adresi değişikliğine hazırlanma konusu açıklanmaktadır. Azure App Service IP adresleri hakkında daha fazla bilgi için, bkz. [Azure App Service gelen ve gıden IP adresleri](overview-inbound-outbound-ips.md).
