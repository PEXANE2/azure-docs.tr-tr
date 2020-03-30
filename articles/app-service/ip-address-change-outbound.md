---
title: Giden IP adresi değişikliğine hazırlanın
description: Giden IP adresiniz değişecekse, uygulamanızın değişiklikten sonra çalışmaya devam etmesi için ne yapmanız gerektiğini öğrenin.
ms.topic: article
ms.date: 06/28/2018
ms.custom: seodec18
ms.openlocfilehash: 2be4bc92dde278b054bd04f412f937440027ece7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74671676"
---
# <a name="how-to-prepare-for-an-outbound-ip-address-change"></a>Giden IP adresi değişikliğine nasıl hazırlanılır?

Azure Uygulama Hizmeti uygulamanızın giden IP adreslerinin değiştiğine dair bir bildirim aldıysanız, bu makaledeki yönergeleri izleyin.

## <a name="determine-if-you-have-to-do-anything"></a>Bir şey yapmak zorunda olup olmadığını belirleyin

* Seçenek 1: Uygulama Hizmeti uygulamanız IP filtreleme, açık bir dahil etme listesi veya yönlendirme veya güvenlik duvarı gibi giden trafiğin özel kullanımını kullanmıyorsa, herhangi bir işlem gerekmez.

* Seçenek 2: Uygulamanızda giden IP adresleri için özel kullanım varsa (aşağıdaki örneklere bakın), mevcut ip adreslerinin göründüğü yere yeni giden IP adreslerini ekleyin. Varolan IP adreslerini değiştirmeyin. Yeni giden IP adreslerini sonraki bölümdeki yönergeleri izleyerek bulabilirsiniz.

  Örneğin, giden bir IP adresi uygulamanızın dışındaki bir güvenlik duvarına açıkça eklenebilir veya harici bir ödeme hizmetinde uygulamanızın giden IP adresini içeren izin verilen bir liste bulunabilir. Giden adresiniz uygulamanızın dışındaki herhangi bir listede yapılandırılırsa, bunun değişmesi gerekir.

## <a name="find-the-outbound-ip-addresses-in-the-azure-portal"></a>Azure portalında giden IP adreslerini bulma

Yeni giden IP adresleri, yürürlüğe girmeden önce portalda gösterilir. Azure yenilerini kullanmaya başladığında, eskileri artık kullanılmaz. Aynı anda yalnızca bir küme kullanılır, bu nedenle ekleme listelerindeki girişlerin geçiş gerçekleştiğinde bir kesintisi önlemek için hem eski hem de yeni IP adreslerine sahip olması gerekir. 

1.  Azure [portalını](https://portal.azure.com)açın.

2.  Sol daki gezinme menüsünde **Uygulama Hizmetleri'ni**seçin.

3.  Uygulama Hizmeti uygulamanızı listeden seçin.

1.  Uygulama bir işlev uygulamasıysa, [Bkz. Fonksiyon uygulaması giden IP adresleri.](../azure-functions/ip-addresses.md#find-outbound-ip-addresses)

4.  **Ayarlar** üstbilgisinin altında, sol gezintideki **Özellikler'i** tıklatın ve Giden **IP adresleri**etiketli bölümü bulun.

5. IP adreslerini kopyalayın ve filtre veya izin verilen liste gibi giden trafiği özel işlemenize ekleyin. Listedeki varolan IP adreslerini silmeyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure tarafından başlatılan bir IP adresi değişikliğine nasıl hazırlanacağı açıklanmıştır. Azure Uygulama Hizmeti'ndeki IP adresleri hakkında daha fazla bilgi için [Azure Uygulama Hizmeti'nde gelen ve giden IP adreslerine](overview-inbound-outbound-ips.md)bakın.
