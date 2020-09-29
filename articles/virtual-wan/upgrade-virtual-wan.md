---
title: Azure sanal WAN 'ı temel 'ten standart Azure portal yükseltin | Microsoft Docs
description: Sanal WAN türünü daha fazla işlevsellik için yükseltebilirsiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 6290d89ed0ee539b4df4c2c8bc9070097da98c81
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91447350"
---
# <a name="upgrade-a-virtual-wan-from-basic-to-standard"></a>Bir sanal WAN 'ı temel 'ten Standart sürüme yükseltme

Bu makale, temel bir WAN 'ı standart bir WAN 'a yükseltmenize yardımcı olur. ' Temel ' WAN türü, onun içindeki tüm Hub 'ları temel SKU hub 'ları olarak oluşturur. Temel bir hub 'da, yalnızca siteden siteye VPN işlevselliği ile sınırlı olursunuz. ' Standart ' WAN türü, içindeki tüm Hub 'ları standart SKU hub 'ları olarak oluşturur. Standart Hub 'ları kullanırken ExpressRoute, Kullanıcı (Noktadan siteye) VPN, tam bir ağ hub 'ı ve Azure hub 'ları aracılığıyla VNet 'ten VNet 'e geçiş sağlayabilirsiniz.

Aşağıdaki tabloda her WAN türü için kullanılabilen yapılandırma gösterilmektedir:

[!INCLUDE [Basic and Standard SKUs](../../includes/virtual-wan-standard-basic-include.md)]

## <a name="to-change-the-virtual-wan-type"></a>Sanal WAN türünü değiştirmek için

1. Sanal WAN 'nizin sayfasında yapılandırma sayfasını açmak için **yapılandırma** ' yı seçin.

   ![Alt kısımdaki standart türdeki bir sanal WAN 'a yükseltme hakkında bilgi metin kutusu içeren "yapılandırma" sayfasını gösteren ekran görüntüsü.](./media/upgrade-virtual-wan/1.png)
2. Sanal WAN türü için açılan listeden **Standart** ' ı seçin.

   !["Sanal WAN türü" açılan menüsünü gösteren ekran görüntüsü.](./media/upgrade-virtual-wan/2.png)
3. Standart bir sanal WAN 'a yükseltirseniz, temel bir sanal WAN 'a geri dönmemek istediğinizi anlayın. Yükseltmek istiyorsanız **Onayla** ' yı seçin.

   !["Yükseltme onayı" iletişim kutusunu gösteren ekran görüntüsü.](./media/upgrade-virtual-wan/4.png)
4. Değişiklik kaydedildikten sonra, sanal WAN sayfanız bu örneğe benzer şekilde görünür.

   ![Sanal WAN diyagramı](./media/upgrade-virtual-wan/5.png)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.