---
title: 'Azure Virtual WAN: Portal için özel IPsec ilkesini yapılandırma | Microsoft Dokümanlar'
description: Portalı kullanarak Azure Virtual WAN için özel IPsec ilkesini nasıl yapılandırıştırmayı öğrenin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: f37d7f3bfac37253339aab3493fb2c444900e099
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73515752"
---
# <a name="configure-a-custom-ipsec-policy-for-virtual-wan-using-the-portal"></a>Portalı kullanarak Sanal WAN için özel bir IPsec ilkesini yapılandırma

Azure portalında Virtual WAN için özel IPsec ilkesini yapılandırabilirsiniz. Özel ilkeler, her iki tarafın da (şirket içi ve Azure VPN ağ geçidi) IKE Phase 1 ve IKE Phase 2 için aynı ayarları kullanmasını istediğinizde yararlıdır.

## <a name="working-with-custom-policies"></a>Özel ilkelerle çalışma

[!INCLUDE [IPsec](../../includes/virtual-wan-ipsec-custom-include.md)]

## <a name="configure-a-policy"></a>İlke yapılandırma

1. **Sanal hub'ı bulun.** Bir tarayıcıdan [Azure portalına](https://aka.ms/azurevirtualwanpreviewfeatures) gidin ve Azure hesabınızla oturum açın. Sitenizin sanal merkezini bulun.
2. **VPN sitesini seçin.** Hub sayfasından, özel bir ilke ayarlamak istediğiniz VPN Sitesini seçin.

   ![Seçin](./media/virtual-wan-custom-ipsec-portal/locate.png)
3. **VPN bağlantısını edin.** Bağlam **menüsünden** **...**, VPN **Bağlantısını Edit**seçin.

   ![düzenle](./media/virtual-wan-custom-ipsec-portal/contextmenu.png)
4. **Ayarları yapılandırın.** VPN bağlantı sayfasını **edit,** ayarları yapılandırır. Ayarlarınızı kaydetmek için **Kaydet'i** seçin.

   ![yapılandırmak ve kaydetmek](./media/virtual-wan-custom-ipsec-portal/edit.png)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.