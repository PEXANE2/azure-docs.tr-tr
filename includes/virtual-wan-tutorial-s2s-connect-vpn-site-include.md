---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8b338f25e9771f5947fd494cfb00d0f6cb9ef67a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75450857"
---
1. **Siteleri BAĞLAN** sayfasını açmak Için **VPN sitelerini bağla** ' yı seçin.

    ![bağlanabilmeniz](./media/virtual-wan-tutorial-connect-vpn-site-include/connect.png "bağlanmaya")

   Aşağıdaki alanları doldurun:

   * Önceden paylaşılan bir anahtar girin. Anahtar girmezseniz, Azure oto sizin için bir tane oluşturur.
   * Protokol ve IPSec ayarlarını seçin. [Varsayılan/özel IPSec Ayrıntıları] bölümüne bakın (https://docs.microsoft.com/azure/virtual-wan/virtual-wan-ipsec)
   * **Varsayılan yolu yayma**için uygun seçeneği belirleyin. **Etkinleştir** seçeneği, sanal hub 'ın bu bağlantıya öğrenilen bir varsayılan yol yaymasını sağlar. Bu bayrak, yalnızca varsayılan yol sanal WAN hub 'ı hub 'da bir güvenlik duvarı dağıtımının bir sonucu olarak zaten öğrenildiği ya da başka bir bağlı sitede zorlamalı tünelyi etkin olan bir bağlantıya varsayılan yol yaymayı sağlar. Varsayılan yol, sanal WAN hub 'ında değil.

2. **Bağlan**’ı seçin.
3. Birkaç dakika içinde, site bağlantının ve bağlantı durumunun gösterilmesi gerekir.

   ![durumlarına](./media/virtual-wan-tutorial-connect-vpn-site-include/status.png "durum")

   **Bağlantı durumu:** Bu, VPN sitesini Azure hub 'ının VPN Gateway 'e bağlayan bağlantı için Azure kaynağının durumudur. Bu denetim düzlemi işlemi başarılı olduktan sonra, Azure VPN ağ geçidi ve şirket içi VPN cihazı bağlantı kurmaya devam eder.

   **Bağlantı durumu:** Bu, Merkez ve VPN sitesinde Azure 'un VPN ağ geçidi arasındaki gerçek bağlantı (veri yolu) durumudur. Aşağıdaki durumlardan herhangi birini gösterebilir:

    * **Bilinmiyor**: Bu durum genellikle arka uç sistemleri başka bir duruma geçişe çalışıyorsa görülür.
    * **Bağlanılıyor**: Azure VPN Gateway, gerçek ŞIRKET içi VPN sitesine ulaşmaya çalışıyor.
    * **Bağlandı**: Azure VPN ağ geçidi ve ŞIRKET içi VPN sitesi arasında bağlantı kuruldu.
    * Bağlantı **kesildi**: herhangi bir nedenle (Şirket Içi veya Azure 'da) bağlantının bağlantısı kesildiğinde bu durum görülür.
4. Bir hub VPN sitesi içinde şunları da yapabilirsiniz: 

   * VPN bağlantısını düzenleyin veya silin.
   * Azure portal siteyi silin.
   * Sitenin yanındaki bağlam (...) menüsünü kullanarak Azure tarafı hakkındaki ayrıntılar için dala özgü bir yapılandırma indirin. Hub 'ınızdaki tüm bağlı siteler için yapılandırmayı indirmek isterseniz, üst menüdeki **VPN yapılandırmasını indir** ' i seçin.
