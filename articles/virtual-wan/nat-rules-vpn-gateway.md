---
title: Ağ geçidiniz için VPN NAT kurallarını yapılandırma
titleSuffix: Azure Virtual WAN
description: VWAN VPN ağ geçidiniz için NAT kurallarını yapılandırma hakkında bilgi edinin
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/05/2021
ms.author: cherylmc
ms.openlocfilehash: 6fbee31f015953bd7e65648ea273e3ca84686115
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102431249"
---
# <a name="configure-nat-rules-for-your-virtual-wan-vpn-gateway---preview"></a>Sanal WAN VPN Gateway için NAT kurallarını yapılandırma-Önizleme

> [!IMPORTANT]
> NAT kuralları şu anda genel önizlemededir.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sanal WAN VPN ağ geçidinizi statik bire bir NAT kurallarıyla yapılandırabilirsiniz. NAT kuralı, IP adreslerinin bire bir çevirisini ayarlamaya yönelik bir mekanizma sağlar. NAT, uyumsuz veya çakışan IP adreslerine sahip iki IP ağını bağlamak için kullanılabilir. Tipik bir senaryo, Azure VNet kaynaklarına erişmek isteyen çakışan IP 'Lere sahip dallarıdır.

Bu yapılandırma, bir dış (ana bilgisayar) IP adresinden gelen trafiği bir sanal ağ (sanal makine, bilgisayar, kapsayıcı, vb.) içindeki bir uç noktayla ilişkili iç IP adresine yönlendirmek için bir akış tablosu kullanır.

   :::image type="content" source="./media/nat-rules-vpn-gateway/diagram.png" alt-text="Mimariyi gösteren diyagram.":::

## <a name="configure-nat-rules"></a><a name="rules"></a>NAT kurallarını yapılandırma

NAT kurallarını VPN Gateway ayarlarınızda dilediğiniz zaman yapılandırabilir ve görüntüleyebilirsiniz.

   :::image type="content" source="./media/nat-rules-vpn-gateway/edit-rules.png" alt-text="Düzenleme kurallarını gösteren ekran görüntüsü.":::

1. Sanal hub 'ınıza gidin.
1. **VPN (siteden siteye)** seçeneğini belirleyin.
1. **NAT kuralları (düzenleme)** seçeneğini belirleyin.
1. **NAT kuralını Düzenle** sayfasında, aşağıdaki değerleri kullanarak bir NAT kuralı **ekleyebilir/düzenleyebilir/silebilirsiniz** :

   * **Ad:** NAT kuralınız için benzersiz bir ad.
   * **Şunu yazın:** Se. Statik bire bir NAT, iç adres ve dış adres arasında bire bir ilişki kurar.
   * **Mod:** Inresssnat veya Yumurresssnat.  
      * IngressSnat modu (giriş kaynağı NAT olarak da bilinir), Azure hub 'ının siteden siteye VPN ağ geçidini giren trafiğe uygulanabilir.
      * Yumurresssnat modu (çıkış kaynağı NAT olarak da bilinir), Azure hub 'ından siteden siteye VPN ağ geçidi olan trafik için geçerlidir.
   * **InternalMapping:** Bir dış IP kümesiyle eşlenecek, içindeki kaynak IP 'lerin adres ön eki aralığı. Diğer bir deyişle, NAT öncesi adres ön eki aralığınız.
   * **Externalmapping:** Kaynak IP 'Lerinin eşlendiği, dış ağdaki hedef IP 'lerin adres ön eki aralığı. Diğer bir deyişle, NAT-posta adresi ön eki aralığıdır.
   * Bağlantı **bağlantısı:** Bir VPN sitesini neredeyse Azure hub 'ından site VPN Gateway 'e bağlayan bağlantı kaynağı.

### <a name="configuration-considerations"></a><a name="considerations"></a>Yapılandırma konuları

* Hem iç hem de dış eşleme için alt ağ boyutu statik bire bir NAT için aynı olmalıdır.
* ' Özel adres alanı ' alanına **Externalmapping** öneklerini eklemek IÇIN Azure Portal VPN sitesini düzenlemediğinizden emin olun. Şu anda, BGP etkin olan sitelerin, şirket içi BGP Announcer (cihaz BGP ayarları) dış eşleme önekleri için bir giriş içerdiğinden emin olması gerekir.

## <a name="examples-and-verification"></a><a name="examples"></a>Örnekler ve doğrulama

### <a name="ingress-mode-nat"></a>Giriş modu NAT

Giriş modu NAT kuralları, sanal WAN siteden siteye VPN ağ geçidi üzerinden Azure 'a giren paketlere uygulanır. Bu senaryoda, iki siteden siteye VPN dallarını Azure 'a bağlamak istiyorsunuz. VPN sitesi 1, link1 aracılığıyla bağlanır ve VPN site 2 bağlantı 2 aracılığıyla bağlanır. Her sitenin 192.169.1.0/24 adres alanı vardır.

Aşağıdaki diyagramda, öngörülen bitiş sonucu gösterilmektedir:

:::image type="content" source="./media/nat-rules-vpn-gateway/ingress.png" alt-text="Giriş modu NAT gösteren diyagram.":::

1. Bir NAT kuralı belirtin.

   Siteden siteye VPN ağ geçidinin çakışan adres alanları (192.168.1.0/24 gibi) ile iki dalı arasında ayrım yapabilmesini sağlamak için bir NAT kuralı belirtin. Bu örnekte, VPN sitesi 1 için link1 'ye odaklanıyoruz.

   Aşağıdaki NAT kuralı ayarlanabilir ve dallardan birinin 1 bağlantısı ile ilişkilendirilebilir. Bu statik bir NAT kuralı olduğundan, InternalMapping ve ExternalMapping 'in adres alanları aynı sayıda IP adresini içerir.

   * **Ad:** IngressRule01
   * **Şunu yazın:** Se
   * **Mod:** Inresssnat
   * **InternalMapping:** 192.168.1.0/24
   * **Externalmapping:** 10.1.1.0/24
   * Bağlantı **bağlantısı:** Bağlantı 1

1. Doğru ExternalMapping 'i duyurun.

   Bu adımda, siteden siteye VPN ağ geçidinizin doğru ExternalMapping adres alanını Azure kaynaklarınızın geri kalanına duyurduğu için emin olun. BGP 'nin etkin olup olmadığına bağlı olarak farklı yönergeler vardır.

   **Örnek 1: BGP etkin**

   * VPN sitesi 1 ' de bulunan şirket içi BGP konuşmacının 10.1.1.0/24 adres alanını tanıtmak üzere yapılandırıldığından emin olun.
   * Bu önizleme sırasında BGP etkin olan sitelerin, şirket içi BGP Announcer (cihaz BGP ayarları) dış eşleme önekleri için bir giriş içerdiğinden emin olması gerekir.

   **Örnek 2: BGP etkin değil**

   * Siteden siteye VPN ağ geçidini içeren sanal hub kaynağına gidin. Sanal hub sayfasında, **bağlantı** altında **VPN (siteden siteye)** seçeneğini belirleyin.
   * Bağlantı 1 üzerinden sanal WAN hub 'ına bağlı VPN sitesini seçin. VPN sitesi için özel adres alanı olarak **site Düzenle** ve giriş 10.1.1.0/24 ' ü seçin.

     :::image type="content" source="./media/nat-rules-vpn-gateway/edit-site.png" alt-text="VPN sitesini Düzenle sayfasını gösteren ekran görüntüsü.":::

### <a name="packet-flow"></a>Paket akışı

Bu örnekte, şirket içi bir cihaz, bir bağlı olan sanal ağa ulaşmak istiyor. Paket akışı, NAT çevirileri kalın olarak, aşağıdaki gibidir.

1. Şirket içinden gelen trafik başlatılır.
   * Kaynak IP adresi: **192.168.1.1**
   * Hedef IP adresi: 30.0.0.1
1. Trafik, siteden siteye ağ geçidine girer ve NAT kuralı kullanılarak çevrilir ve sonra bağlı bileşene gönderilir.
   * Kaynak IP adresi: **10.1.1.1**
   * Hedef IP adresi: 30.0.0.1
1. Bağlı bileşen yanıtı başlatılır.
   * Kaynak IP adresi: 30.0.0.1
   * Hedef IP adresi: **10.1.1.1**
1. Trafik, siteden siteye VPN ağ geçidine girer ve çeviri tersine çevrilir ve şirket içine gönderilir.
   * Kaynak IP adresi: 30.0.0.1
   * Hedef IP adresi: **192.168.1.1**

### <a name="verification-checks"></a>Doğrulama denetimleri

Bu bölümde, yapılandırmanızın doğru şekilde ayarlandığını doğrulamak için denetimler gösterilmektedir.

#### <a name="validate-defaultroutetable-rules-and-routes"></a>DefaultRouteTable, Rules ve rotaları doğrulama

Sanal WAN 'daki dallar **defaultroutetable** ile ilişkilendir, tüm dal bağlantıları, defaultroutetable içinde doldurulan rotaları öğrenmelidir. DefaultRouteTable 'ın etkin yönlendirmelerinin dış eşleme önekiyle NAT kuralını görürsünüz.

Örnek:

* **Ön ek:** 10.1.1.0/24  
* **Sonraki atlama türü:** VPN_S2S_Gateway
* **Sonraki atlama:** VPN_S2S_Gateway kaynağı

#### <a name="validate-address-prefixes"></a>Adres öneklerini doğrula

Bu örnek, DefaultRouteTable ile ilişkili sanal ağlardaki kaynaklar için geçerlidir.

Sanal WAN hub 'ına bağlı olan bir sanal makinenin ağ arabirim kartlarında (NIC) **etkin rotaları** , ayrıca NAT kuralları **externalmapping**'in adres öneklerini içermelidir.

#### <a name="validate-bgp-advertisements"></a>BGP bildirilerini doğrulama

VPN site bağlantısında BGP yapılandırdıysanız, dış eşleme önekleri için bir giriş tanıtmakta olduğundan emin olmak için şirket içi BGP hoparlörünü kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Siteden siteye yapılandırma hakkında daha fazla bilgi için bkz. [sanal WAN siteden siteye bağlantı yapılandırma](virtual-wan-site-to-site-portal.md).
