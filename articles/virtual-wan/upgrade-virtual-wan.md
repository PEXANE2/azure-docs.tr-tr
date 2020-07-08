---
title: Azure sanal WAN 'ı temel 'ten standart Azure portal yükseltin | Microsoft Docs
description: Sanal WAN türünü daha fazla işlevsellik için yükseltebilirsiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 769aa6c0d546b7a9bcbe355136bad811fb4f47b3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84753318"
---
# <a name="upgrade-a-virtual-wan-from-basic-to-standard"></a>Bir sanal WAN 'ı temel 'ten Standart sürüme yükseltme

Bu makale, temel bir WAN 'ı standart bir WAN 'a yükseltmenize yardımcı olur. ' Temel ' WAN türü, onun içindeki tüm Hub 'ları temel SKU hub 'ları olarak oluşturur. Temel bir hub 'da, yalnızca siteden siteye VPN işlevselliği ile sınırlı olursunuz. ' Standart ' WAN türü, içindeki tüm Hub 'ları standart SKU hub 'ları olarak oluşturur. Standart Hub 'ları kullanırken ExpressRoute, Kullanıcı (Noktadan siteye) VPN, tam bir ağ hub 'ı ve Azure hub 'ları aracılığıyla VNet 'ten VNet 'e geçiş sağlayabilirsiniz.

Aşağıdaki tabloda her WAN türü için kullanılabilen yapılandırma gösterilmektedir:

[!INCLUDE [Basic and Standard SKUs](../../includes/virtual-wan-standard-basic-include.md)]

## <a name="to-change-the-virtual-wan-type"></a>Sanal WAN türünü değiştirmek için

1. Sanal WAN 'nizin sayfasında yapılandırma sayfasını açmak için **yapılandırma** ' yı seçin.

   ![Sanal WAN diyagramı](./media/upgrade-virtual-wan/1.png)
2. Sanal WAN türü için açılan listeden **Standart** ' ı seçin.

   ![Sanal WAN diyagramı](./media/upgrade-virtual-wan/2.png)
3. Standart bir sanal WAN 'a yükseltirseniz, temel bir sanal WAN 'a geri dönmemek istediğinizi anlayın. Yükseltmek istiyorsanız **Onayla** ' yı seçin.

   ![Sanal WAN diyagramı](./media/upgrade-virtual-wan/4.png)
4. Değişiklik kaydedildikten sonra, sanal WAN sayfanız bu örneğe benzer şekilde görünür.

   ![Sanal WAN diyagramı](./media/upgrade-virtual-wan/5.png)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.