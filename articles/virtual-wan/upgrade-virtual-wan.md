---
title: Temelden Standarda Azure Sanal WAN yükseltme - Azure portalı | Microsoft Dokümanlar
description: Daha fazla işlevsellik için sanal WAN türünü yükseltebilirsiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 83fd5bafb5496908403c50dc0e000fd33a836c95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73515817"
---
# <a name="upgrade-a-virtual-wan-from-basic-to-standard"></a>Temelden Standarda sanal bir WAN yükseltme

Bu makale, Temel WAN'ı Standart WAN'a yükseltmenize yardımcı olur. 'Basic' WAN türü, temel SKU hub'ları olarak içindeki tüm hub'ları oluşturur. Temel hub'da yalnızca siteden siteye VPN işleviyle sınırlıdırsınız. 'Standart' WAN türü, içindeki tüm hub'ları Standart SKU hub'ları olarak oluşturur. Standart hub'ları kullandığınızda, Azure hub'ları aracılığıyla ExpressRoute, User (Noktaya Sayfa) VPN,tam ağ hub'ı ve VNet'ten VNet'e geçişi etkinleştirebilirsiniz.

Aşağıdaki tablo, her WAN türü için kullanılabilir yapılandırmaları gösterir:

[!INCLUDE [Basic and Standard SKUs](../../includes/virtual-wan-standard-basic-include.md)]

## <a name="to-change-the-virtual-wan-type"></a>Sanal WAN türünü değiştirmek için

1. Sanal WAN'ınızın sayfasında Yapılandırma sayfasını açmak için **Yapılandırma'yı** seçin.

   ![Sanal WAN diyagramı](./media/upgrade-virtual-wan/1.png)
2. Sanal WAN türü için açılır dosyadan **Standart'ı** seçin.

   ![Sanal WAN diyagramı](./media/upgrade-virtual-wan/2.png)
3. Standart sanal WAN'a yükseltilerseniz, Temel sanal WAN'a geri dönemeyeceğinizdir. Yükseltmek istiyorsanız **Onayla'yı** seçin.

   ![Sanal WAN diyagramı](./media/upgrade-virtual-wan/4.png)
4. Değişiklik kaydedildikten sonra, sanal WAN sayfanız bu örneğe benzer.

   ![Sanal WAN diyagramı](./media/upgrade-virtual-wan/5.png)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.