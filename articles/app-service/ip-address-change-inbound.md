---
title: Gelen IP adresi değişikliğine hazırlanma
description: Gelen IP adresiniz değişecekse, uygulamanızın değişiklikten sonra çalışmaya devam etmesi için ne yapmanız gerektiğini öğrenin.
ms.topic: article
ms.date: 06/28/2018
ms.custom: seodec18
ms.openlocfilehash: 999511c4759ee761f028a61fbfaa0310ac93d9b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74672410"
---
# <a name="how-to-prepare-for-an-inbound-ip-address-change"></a>Gelen IP adresi değişikliğine nasıl hazırlanılır?

Azure Uygulama Hizmeti uygulamanızın gelen IP adresinin değiştiğine dair bir bildirim aldıysanız, bu makaledeki yönergeleri izleyin.

## <a name="determine-if-you-have-to-do-anything"></a>Bir şey yapmak zorunda olup olmadığını belirleyin

* Seçenek 1: Uygulama Hizmeti uygulamanızda Özel Etki Alanı yoksa, herhangi bir işlem yapılması gerekmez.

* Seçenek 2: Etki Alanı Kayıt Portalınızda (üçüncü taraf DNS Sağlayıcısı veya Azure DNS) yalnızca bir CNAME kaydı (URI'yi işaret eden DNS kaydı) yapılandırılırsa, herhangi bir işlem yapılması gerekmez.

* Seçenek 3: Etki Alanı Kayıt Portalınızda (üçüncü taraf DNS Sağlayıcısı veya Azure DNS) bir A kaydı (doğrudan IP adresinizi gösteren DNS kaydı) yapılandırılırsa, varolan IP adresini yenisiyle değiştirin. Yeni IP adresini sonraki bölümdeki talimatları izleyerek bulabilirsiniz.

* Seçenek 4: Uygulamanız bir yük dengeleyicisinin, IP Filtresinin veya uygulamanızın IP adresini gerektiren başka bir IP mekanizmasının arkasındaysa, mevcut IP adresini yenisiyle değiştirin. Yeni IP adresini sonraki bölümdeki talimatları izleyerek bulabilirsiniz.

## <a name="find-the-new-inbound-ip-address-in-the-azure-portal"></a>Azure portalında yeni gelen IP Adresini bulma

Uygulamanıza verilen yeni gelen IP adresi **Sanal IP adresi** alanındaki portaldadır. Hem bu yeni IP adresi hem de eski adres artık uygulamanıza bağlıdır ve daha sonra eskisinin bağlantısı kesilir.

1.  Azure [portalını](https://portal.azure.com)açın.

2.  Sol daki gezinme menüsünde **Uygulama Hizmetleri'ni**seçin.

3.  Uygulama Hizmeti uygulamanızı listeden seçin.

1.  Uygulama bir işlev uygulamasıysa, [Bkz. Fonksiyon uygulaması gelen IP adresi.](../azure-functions/ip-addresses.md#function-app-inbound-ip-address)

4.  **Ayarlar** üstbilgisinin altında, sol gezintideki **Özellikler'i** tıklatın ve Sanal **IP adresi**etiketli bölümü bulun.

5. IP adresini kopyalayın ve etki alanı kaydınızı veya IP mekanizmanızı yeniden yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure tarafından başlatılan bir IP adresi değişikliğine nasıl hazırlanacağı açıklanmıştır. Azure Uygulama Hizmeti'ndeki IP adresleri hakkında daha fazla bilgi için [Azure Uygulama Hizmeti'nde gelen ve giden IP adreslerine](overview-inbound-outbound-ips.md)bakın.
