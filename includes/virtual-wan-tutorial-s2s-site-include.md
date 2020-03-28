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
ms.openlocfilehash: 4bcee1097010bb8746b11185a470ca2584485c3f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73488876"
---
1. Sanal wan için portal sayfasında, **Bağlantı** bölümünde, VPN siteleri sayfasını açmak için **VPN sitelerini** seçin.
2. **VPN siteleri** sayfasında **+Site oluştur**’a tıklayın.

   ![Temel Bilgiler](./media/virtual-wan-tutorial-site-include/basics.png "Temel Bilgiler")
3. VPN **Sitesi Oluştur** sayfasında, **Temel Bilgiler** sekmesinde aşağıdaki alanları tamamlayın:

    * **Bölge** - Daha önce konum olarak adlandırılır. Bu site kaynağını oluşturmak istediğiniz konum budur.
    * **Ad** - Şirket içi sitenize başvurmak istediğiniz ad.
    * **Aygıt satıcısı** - VPN aygıt satıcısının adı (örneğin: Citrix, Cisco, Barracuda). Bunu yapmak, Azure Ekibi'nin gelecekte ek optimizasyon olanakları eklemek için ortamınızı daha iyi anlamasına veya sorun gidermenize yardımcı olabilir.
    * **Kenarlık Ağ Geçidi Protokolü** - Etkinleştir, sitedeki tüm bağlantıların BGP etkin olacağını ima eder. Sonunda Bağlantılar bölümünde VPN Sitesinden her bağlantı için BGP bilgilerini kuracak. BGP'yi Sanal WAN'da yapılandırmak, BGP'yi Azure sanal ağ ağ geçidi VPN'de yapılandırmaya eşdeğerdir. Şirket içi BGP eş adresiniz, VPN'inizin cihaza genel IP adresiyle veya VPN sitesinin VNet adres alanıyla aynı olmamalıdır. BGP eş IP'niz için VPN aygıtında farklı bir IP adresi kullanın. Bu aygıttaki geri döngü arabirimine atanmış bir adres olabilir. Ancak, bir APIPA (169.254.x.x) adresi olamaz. Bu adresi konumu temsil eden ilgili VPN sitesinde belirtin. BGP ön koşulları için [Azure VPN Ağ Geçidi ile BGP Hakkında'ya](../articles/vpn-gateway/vpn-gateway-bgp-overview.md)bakın. VPN Sitesi BGP ayarı etkinleştirildikten sonra BGP parametrelerini (bağlantıdaki IP'yi ve AS #'ı) güncellemek için her zaman VPN bağlantısını ayarlayabilirsiniz.
    * **Özel adres alanı** - Şirket içi sitenizde bulunan IP adres alanı. Bu adres alanını hedefleyen trafik yerel sitenize yönlendirilir. BGP site için etkinleştirilemediğinde bu gereklidir.
    * **Hub'lar** - Sitenizin bağlanmasını istediğiniz hub. Bir site yalnızca VPN Ağ Geçidi olan hub'lara bağlanabilir. Bir hub görmüyorsanız, lütfen önce bu hub'da bir VPN ağ geçidi oluşturun.
4. Şubedeki fiziksel bağlantılar hakkında bilgi eklemek için **Bağlantılar'ı** seçin. Sanal bir wan iş ortağı CPE aygıtınız varsa, bu bilgilerin sistemlerinden ayarlanan şube bilgileri yüklemesinin bir parçası olarak Azure ile değiş tokuş edilip edilemediğinizi kontrol edin.

   ![Bağlantı](./media/virtual-wan-tutorial-site-include/links.png "Bağlantılar")

    * **Bağlantı Adı** - VPN Sitesindeki fiziksel bağlantı için sağlamak istediğiniz bir ad. Örnek: mylink1.
    * **Sağlayıcı Adı** - VPN Sitesindeki fiziksel bağlantının adı. Örnek: ATT, Verizon.
    * **Hız** - Bu şube konumundaVPN cihazının hızıdır. Örnek: 50, yani 50 Mbps, şube alanındaki VPN cihazının hızıdır.
    * **IP Adresi** - Bu bağlantıyı kullanarak prem cihazının genel IP adresi. İsteğe bağlı olarak, ExpressRoute'un arkasındaki şirket içi VPN cihazınızın özel IP adresini sağlayabilirsiniz.
5. Onay kutusunu silmek veya ek bağlantılar eklemek için kullanabilirsiniz. VPN Sitesi başına dört bağlantı desteklenir. Örneğin, şube konumunda dört ISS (Internet servis sağlayıcısı) varsa, dört bağlantı oluşturabilirsiniz. her ISS için bir tane ve her bağlantı için bilgi sağlayın.
6. Alanları doldurmayı bitirdikten sonra, siteyi doğrulamak ve oluşturmak için **Gözden Geçir + oluştur'u** seçin.
7. VPN siteleri sayfasındaki durumu görüntüleyin. Site henüz hub'a bağlı olmadığından, site **Gerekli Bağlantı'ya** gider.