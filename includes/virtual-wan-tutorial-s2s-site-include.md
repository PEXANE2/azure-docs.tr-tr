---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 02/11/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 708baa83ca919adcc374be36c229ce3ff30da384
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100362978"
---
1. Sanal WAN 'nizin portal sayfasında, **bağlantı** bölümünde, VPN siteleri sayfasını açmak için **VPN siteleri** ' ni seçin.
1. **VPN siteleri** sayfasında **+Site oluştur**’a tıklayın.
1. **VPN sitesi oluştur** sayfasında, **temel bilgiler** sekmesinde aşağıdaki alanları doldurun:

   :::image type="content" source="./media/virtual-wan-tutorial-site-include/site-basics.png" alt-text="Temel bilgiler sekmesi" lightbox="./media/virtual-wan-tutorial-site-include/site-basics.png":::

    * **Bölge** -daha önce konum olarak adlandırılır. Bu, içinde bu site kaynağını oluşturmak istediğiniz konumdur.
    * **Ad** -şirket içi sitenize başvurmak istediğiniz addır.
    * **Cihaz satıcısı** -VPN cihaz satıcısının adı (örneğin: Citrix, Cisco, Barbcuda). Cihaz satıcısının eklenmesi, Azure ekibinin daha sonra ek en iyi duruma getirme olanakları eklemek veya sorunu gidermenize yardımcı olmak için ortamınızı daha iyi anlamasına yardımcı olabilir.
    * **Özel adres alanı** -şirket içi SITENIZDE bulunan IP adres alanı. Bu adres alanını hedefleyen trafik yerel sitenize yönlendirilir. Bu, site için BGP etkinleştirilmediğinde gereklidir.
    
      >[!NOTE]
      >Siteyi oluşturduktan sonra adres alanını düzenlerseniz (örneğin, ek bir adres alanı eklerseniz), bileşenler yeniden oluşturulduğunda etkin yolların güncelleştirilmesi 8-10 dakika sürebilir.
      >
1. Daldaki fiziksel bağlantılarla ilgili bilgi eklemek için **Bağlantılar** ' ı seçin. Bir sanal WAN iş ortağı CPE cihazınız varsa, bu bilgilerin Azure ile birlikte kullanıma alınıp ayarlanmalarını sağlamak için BT 'nin sistemlerine ait olan şube bilgileri karşıya yükleme işleminin bir parçası olarak bu bilgileri inceleyin.

   :::image type="content" source="./media/virtual-wan-tutorial-site-include/site-links.png" alt-text="Bağlantılar sekmesi" lightbox="./media/virtual-wan-tutorial-site-include/site-links.png":::

   * **Bağlantı adı** -VPN sitesindeki fiziksel bağlantı için sağlamak istediğiniz addır. Örnek: mylink1.
   * **Bağlantı hızı** -VPN cihazının şube konumundaki hızıdır. Örnek: 50, bu, Şube sitesindeki VPN cihazının hızındaki 50 Mbps anlamına gelir.
   * **Bağlantı sağlayıcı adı** -VPN sitesindeki fiziksel bağlantının adı. Örnek: ATT, Verizon.
   * **Bağlantı IP adresi/FQDN** -bu bağlantıyı kullanarak şirket içi CIHAZıN genel IP adresi. İsteğe bağlı olarak, ExpressRoute 'un arkasındaki şirket içi VPN cihazınızın özel IP adresini sağlayabilirsiniz. Tam etki alanı adını da dahil edebilirsiniz. Örneğin, *Something.contoso.com*. FQDN, VPN ağ geçidinden çözümlenebilmelidir. Bu FQDN 'yi barındıran DNS sunucusuna Internet üzerinden ulaşılabiliyorsa, bu mümkündür. IP adresi ve FQDN belirtildiğinde IP adresi öncelikli olur.

     >[!NOTE]
     >
     >* Her FQDN için bir IPv4 adresini destekler. FQDN birden çok IP adresine çözümlenmişse VPN Gateway, listeden ilk ıP4 adresini alır. IPv6 adresleri şu anda desteklenmiyor.
     >
     >* VPN Gateway, 5 dakikada bir yenilenen DNS önbelleğini korur. Ağ Geçidi, yalnızca bağlantısı kesilen tüneller için FQDN 'leri çözümlemeye çalışır. Ağ Geçidi sıfırlama veya yapılandırma değişikliği, FQDN çözümlemesini de tetikleyebilir.
     >
   * **Bağlantı Sınır Ağ Geçidi Protokolü** -sanal WAN bağlantısında BGP 'yi yapılandırma bir Azure sanal ağ geçidi VPN üzerinde BGP 'yi yapılandırmaya eşdeğerdir. Şirket içi BGP eşi adresiniz, cihaza VPN 'nizin genel IP adresi veya VPN sitesinin VNet adres alanı ile aynı olmamalıdır. BGP eşi IP 'niz için VPN cihazında farklı bir IP adresi kullanın. Bu aygıttaki geri döngü arabirimine atanmış bir adres olabilir. Bu adresi, konumu temsil eden ilgili VPN sitesinde belirtin.  BGP önkoşulları için bkz. [Azure VPN Gateway Ile BGP hakkında](../articles/vpn-gateway/vpn-gateway-bgp-overview.md). Bir VPN bağlantısı bağlantısını, BGP parametrelerini (bağlantı IP 'si ve AS # olarak eşleme) güncelleştirmek üzere her zaman düzenleyebilirsiniz.
1. Daha fazla bağlantı ekleyebilir veya silebilirsiniz. VPN sitesi başına dört bağlantı desteklenir. Örneğin, şube konumunda dört ISS 'niz varsa, her bir ISS için bir tane olmak üzere dört bağlantı oluşturabilir ve her bir bağlantı için bilgi verebilirsiniz.
1. Alanları doldurmayı tamamladıktan sonra, siteyi doğrulamak ve oluşturmak için **gözden geçir + oluştur** ' u seçin.
1. İstediğiniz sanal hub 'a gidin ve VPN sitenizi hub 'a bağlamak için **hub ilişkilendirmesi** seçimini kaldırın.

   :::image type="content" source="./media/virtual-wan-tutorial-site-include/connect.png" alt-text="Bu hub 'a Bağlan" lightbox="./media/virtual-wan-tutorial-site-include/connect.png":::
