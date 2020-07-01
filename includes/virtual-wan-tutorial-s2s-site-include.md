---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 06/23/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ac23912b12bc6c7fcd40d7433cb4e2d257eeb5d0
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85562954"
---
1. Sanal WAN 'nizin portal sayfasında, **bağlantı** bölümünde, VPN siteleri sayfasını açmak için **VPN siteleri** ' ni seçin.
2. **VPN siteleri** sayfasında **+Site oluştur**’a tıklayın.

   ![Temel Bilgiler](./media/virtual-wan-tutorial-site-include/basics.png "Temel Bilgiler")
3. **VPN sitesi oluştur** sayfasında, **temel bilgiler** sekmesinde aşağıdaki alanları doldurun:

    * **Bölge** -daha önce konum olarak adlandırılır. Bu, içinde bu site kaynağını oluşturmak istediğiniz konumdur.
    * **Ad** -şirket içi sitenize başvurmak istediğiniz addır.
    * **Cihaz satıcısı** -VPN cihaz satıcısının adı (örneğin: Citrix, Cisco, Barbcuda). Bunun yapılması, Azure ekibinin daha sonra ek iyileştirme olanakları eklemek veya sorunu gidermenize yardımcı olmak için ortamınızı daha iyi anlamasına yardımcı olabilir.
    * **Sınır Ağ Geçidi Protokolü** -etkinleştir, siteden tüm bağlantıların BGP 'yi etkin hale etkinleşeceği anlamına gelir. Bağlantı bölümündeki VPN sitesinden her bir bağlantı için BGP bilgilerini son olarak ayarlayacaksınız. Sanal WAN üzerinde BGP 'yi yapılandırmak, Azure sanal ağ geçidi VPN üzerinde BGP 'yi yapılandırmaya eşdeğerdir. Şirket içi BGP eşi adresiniz, cihaza VPN 'nizin genel IP adresi veya VPN sitesinin VNet adres alanı ile aynı olmamalıdır. BGP eşi IP 'niz için VPN cihazında farklı bir IP adresi kullanın. Bu aygıttaki geri döngü arabirimine atanmış bir adres olabilir. Ancak, bir APIPA (169.254. x. x) adresi olamaz. Bu adresi, konumu temsil eden ilgili VPN sitesinde belirtin. BGP önkoşulları için bkz. [Azure VPN Gateway Ile BGP hakkında](../articles/vpn-gateway/vpn-gateway-bgp-overview.md). VPN sitesi BGP ayarı etkinleştirildikten sonra, BGP parametrelerini (bağlantı IP 'si ve AS # olarak eşleme) güncelleştirmek üzere her zaman bir VPN bağlantısını düzenleyebilirsiniz.
    * **Özel adres alanı** -şirket içi SITENIZDE bulunan IP adres alanı. Bu adres alanını hedefleyen trafik yerel sitenize yönlendirilir. Bu, site için BGP etkinleştirilmediğinde gereklidir.
    * **Hub 'lar** -sitenizin bağlanmasını istediğiniz Merkez. Bir site yalnızca VPN Gateway olan hublara bağlanabilir. Bir hub görmüyorsanız, önce o hub 'da bir VPN Ağ Geçidi oluşturun.
4. Daldaki fiziksel bağlantılarla ilgili bilgi eklemek için **Bağlantılar** ' ı seçin. Bir sanal WAN iş ortağı CPE cihazınız varsa, bu bilgilerin Azure ile birlikte kullanıma alınıp ayarlanmalarını sağlamak için BT 'nin sistemlerine ait olan şube bilgileri karşıya yükleme işleminin bir parçası olarak bu bilgileri inceleyin.

   ![Köprü](./media/virtual-wan-tutorial-site-include/links.png "Bağlantılar")

    * **Bağlantı adı** -VPN sitesindeki fiziksel bağlantı için sağlamak istediğiniz addır. Örnek: mylink1.
    * **Sağlayıcı adı** -VPN sitesindeki fiziksel bağlantının adı. Örnek: ATT, Verizon.
    * **Hız** -bu, şube konumundaki VPN cihazının hızıdır. Örnek: 50, bu, Şube sitesindeki VPN cihazının hızındaki 50 Mbps anlamına gelir.
    * **IP adresi/FQDN** -bu bağlantıyı kullanarak şirket içi CIHAZıN genel IP adresi. İsteğe bağlı olarak, ExpressRoute 'un arkasındaki şirket içi VPN cihazınızın özel IP adresini sağlayabilirsiniz. Tam etki alanı adını da dahil edebilirsiniz. Örneğin, *Something.contoso.com*. FQDN, VPN ağ geçidinden çözümlenebilmelidir. Bu FQDN 'yi barındıran DNS sunucusuna Internet üzerinden ulaşılabiliyorsa, bu mümkündür. IP adresi ve FQDN belirtildiğinde IP adresi öncelikli olur.

      >[!NOTE]
      >* Her FQDN için bir IPv4 adresini destekler. FQDN birden çok IP adresine çözümlenmişse VPN Gateway, listeden ilk ıP4 adresini alır. IPv6 adresleri şu anda desteklenmiyor.
      >* VPN Gateway, 5 dakikada bir yenilenen DNS önbelleğini korur. Ağ Geçidi, yalnızca bağlantısı kesilen tüneller için FQDN 'leri çözümlemeye çalışır. Ağ Geçidi sıfırlama veya yapılandırma değişikliği, FQDN çözümlemesini de tetikleyebilir.
      >
5. Ek bağlantıları silmek veya eklemek için onay kutusunu kullanabilirsiniz. VPN sitesi başına dört bağlantı desteklenir. Örneğin, dal konumunda dört ISS 'niz (Internet servis sağlayıcınız) varsa dört bağlantı oluşturabilirsiniz. Her bir ISS için bir tane olmak üzere her bir bağlantı için bilgi sağlayın.
6. Alanları doldurmayı tamamladıktan sonra, siteyi doğrulamak ve oluşturmak için **gözden geçir + oluştur** ' u seçin.
7. VPN siteleri sayfasında durumu görüntüleyin. Site henüz hub 'a bağlı olmadığından site **gerekli bağlantıya** gidecektir.