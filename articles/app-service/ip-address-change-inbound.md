---
title: Gelen IP adresi değişikliğine hazırlanma
description: Gelen IP adresiniz değiştirilebiliyorsa, uygulamanızın değişiklikten sonra çalışmaya devam etmesi için ne yapılacağını öğrenin.
ms.topic: article
ms.date: 06/28/2018
ms.custom: seodec18
ms.openlocfilehash: 999511c4759ee761f028a61fbfaa0310ac93d9b4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74672410"
---
# <a name="how-to-prepare-for-an-inbound-ip-address-change"></a>Gelen IP adresi değişikliğine hazırlanma

Azure App Service uygulamanızın gelen IP adresinin değiştiğini belirten bir bildirim aldıysanız, bu makaledeki yönergeleri izleyin.

## <a name="determine-if-you-have-to-do-anything"></a>Herhangi bir şey yapmanız gerekip gerekmediğini belirleme

* Seçenek 1: App Service uygulamanızın özel bir etki alanı yoksa, herhangi bir eylem gerekmez.

* Seçenek 2: etki alanı kayıt portalınızda (üçüncü taraf DNS sağlayıcısı veya Azure DNS) yalnızca bir CNAME kaydı (bir URI 'yi işaret eden DNS kaydı) yapılandırılmışsa, hiçbir işlem yapmanız gerekmez.

* Seçenek 3: etki alanı kayıt portalınızda (üçüncü taraf DNS sağlayıcısı veya Azure DNS) bir kayıt (doğrudan IP adresinizi işaret eden DNS kaydı) yapılandırılmışsa, var olan IP adresini yeni bir adres ile değiştirin. Sonraki bölümdeki yönergeleri izleyerek yeni IP adresini bulabilirsiniz.

* 4. seçenek: uygulamanız bir yük dengeleyici, IP filtresi veya uygulamanızın IP adresini gerektiren başka bir IP mekanizmasından sonsındaysa, var olan IP adresini yeni bir adresle değiştirin. Sonraki bölümdeki yönergeleri izleyerek yeni IP adresini bulabilirsiniz.

## <a name="find-the-new-inbound-ip-address-in-the-azure-portal"></a>Azure portal yeni gelen IP adresini bulun

Uygulamanıza verilen yeni gelen IP adresi, **sanal IP adresi** alanındaki portalda bulunur. Hem bu yeni IP adresi hem de eskileri uygulamanıza bağlanır ve daha sonra eski bir bağlantı kesilir.

1.  [Azure Portal](https://portal.azure.com)açın.

2.  Sol taraftaki gezinti menüsünde **uygulama hizmetleri**' ni seçin.

3.  Listeden App Service uygulamanızı seçin.

1.  Uygulama bir işlev uygulaması ise, bkz. [işlev uygulaması gelen IP adresi](../azure-functions/ip-addresses.md#function-app-inbound-ip-address).

4.  **Ayarlar** üst bilgisinde, sol gezinti bölmesinde **Özellikler** ' e tıklayın ve **sanal IP adresi**etiketli bölümü bulun.

5. IP adresini kopyalayın ve etki alanı kaydınızı veya IP mekanizmanızı yeniden yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure tarafından başlatılan bir IP adresi değişikliğine hazırlanma konusu açıklanmaktadır. Azure App Service IP adresleri hakkında daha fazla bilgi için, bkz. [Azure App Service gelen ve gıden IP adresleri](overview-inbound-outbound-ips.md).
