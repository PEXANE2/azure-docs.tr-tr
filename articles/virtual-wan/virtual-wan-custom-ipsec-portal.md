---
title: 'Azure sanal WAN için özel IPSec ilkesini yapılandırma: Portal | Microsoft Docs'
description: Portalı kullanarak Azure sanal WAN için özel IPSec ilkesi yapılandırma hakkında bilgi edinin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: 07eae453441ea8bff81d7cdb60f9c46c08a22829
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91851191"
---
# <a name="configure-a-custom-ipsec-policy-for-virtual-wan-using-the-portal"></a>Portalı kullanarak sanal WAN için özel bir IPSec ilkesi yapılandırma

Azure portal bir sanal WAN VPN bağlantısı için özel bir IPSec ilkesi yapılandırabilirsiniz. Özel ilkeler, her iki tarafının da (Şirket içi ve Azure VPN Gateway) ıKE Aşama 1 ve ıKE aşama 2 için aynı ayarları kullanmasını istediğinizde yararlıdır.

## <a name="working-with-custom-policies"></a>Özel ilkelerle çalışma

[!INCLUDE [IPsec](../../includes/virtual-wan-ipsec-custom-include.md)]

## <a name="configure-a-policy"></a>İlke yapılandırma

1. **Sanal hub 'ı bulun**. Bir tarayıcıdan [Azure portalına](https://aka.ms/azurevirtualwanpreviewfeatures) gidin ve Azure hesabınızla oturum açın. Sanal WAN kaynağınız ' ne gidin ve VPN sitenizin bağlı olduğu sanal hub 'ı bulun.
2. **VPN sitesini seçin**. Hub Genel Bakış sayfasında **VPN (siteden siteye)** seçeneğine tıklayın ve özel bir IPSec ilkesi kurmak Istediğiniz VPN sitesini seçin.

   ![seçin](./media/virtual-wan-custom-ipsec-portal/locate.png)
3. **VPN bağlantısını düzenleyin**. **Bağlam menüsünden** **...**, **VPN bağlantısını Düzenle**' yi seçin.

   ![düzenle](./media/virtual-wan-custom-ipsec-portal/contextmenu.png)
4. **Ayarları yapılandırın**. **VPN bağlantısını Düzenle** sayfasında, IPSec ayarını varsayılan olarak özel olarak değiştirin ve IPSec ilkesini özelleştirin. Ayarlarınızı kaydetmek için **Kaydet** ' i seçin.

   ![Yapılandır ve Kaydet](./media/virtual-wan-custom-ipsec-portal/edit.png)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.