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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75450857"
---
1. **Siteleri** Bağla sayfasını açmak için **VPN Sitelerine Bağlan'ı** seçin.

    ![Bağlamak](./media/virtual-wan-tutorial-connect-vpn-site-include/connect.png "bağlanmaya")

   Aşağıdaki alanları tamamlayın:

   * Önceden paylaşılan bir anahtar girin. Bir anahtar girmezseniz, Azure bir anahtarı sizin için otomatik olarak oluşturur.
   * Protokol ve IPsec ayarlarını seçin. [Varsayılan/özel IPSec ayrıntıları] (https://docs.microsoft.com/azure/virtual-wan/virtual-wan-ipsec)
   * **Yayılma Varsayılan Rotasını Yaymak**için uygun seçeneği seçin. **Etkinleştir** seçeneği, sanal hub'ın bu bağlantıya öğrenilen varsayılan bir rotayı yaymasına olanak tanır. Bu bayrak, varsayılan rotanın yalnızca hub'a bir güvenlik duvarı nın dağıtılması sonucunda Sanal WAN hub'ı tarafından zaten öğrenilmişse veya bağlı başka bir site tünel lendirmeyi etkinleştirilmişse, varsayılan rota nın bağlantıya yayılmasını sağlar. Varsayılan rota Sanal WAN hub'ında kaynaklanmaz.

2. **Bağlan**’ı seçin.
3. Birkaç dakika içinde, site bağlantı ve bağlantı durumunu gösterir.

   ![durum](./media/virtual-wan-tutorial-connect-vpn-site-include/status.png "durum")

   **Bağlantı Durumu:** Bu, VPN Sitesini Azure hub'ının VPN ağ geçidine bağlayan bağlantı için Azure kaynağının durumudur. Bu kontrol düzlemi işlemi başarılı olduğunda, Azure VPN ağ geçidi ve şirket içi VPN aygıtı bağlantı oluşturmaya devam edecektir.

   **Bağlantı Durumu:** Bu, Azure'un hub'daki VPN ağ geçidi ile VPN Sitesi arasındaki gerçek bağlantı (veri yolu) durumudur. Aşağıdaki hallerden herhangi birini gösterebilir:

    * **Bilinmiyor**: Arka uç sistemleri başka bir duruma geçiş için çalışıyorsa, bu durum genellikle görülür.
    * **Bağlanma**: Azure VPN ağ geçidi, gerçek şirket içi VPN sitesine ulaşmaya çalışıyor.
    * **Connected**: Azure VPN ağ geçidi ile şirket içi VPN sitesi arasında bağlantı kurulur.
    * **Bağlantısızlık**: Herhangi bir nedenle (şirket içinde veya Azure'da) bağlantı kesilirse bu durum görülür.
4. Hub VPN sitesinde ayrıca aşağıdakileri yapabilirsiniz: 

   * VPN Bağlantısını düzenle veya silin.
   * Azure portalındaki siteyi silin.
   * Sitenin yanındaki bağlam (...) menüsünü kullanarak Azure tarafı yla ilgili ayrıntılar için şubeye özgü bir yapılandırma indirin. Hub'ınızdaki tüm bağlı sitelerin yapılandırmasını indirmek istiyorsanız, üst menüden **VPN Config'i İndir'i** seçin.
