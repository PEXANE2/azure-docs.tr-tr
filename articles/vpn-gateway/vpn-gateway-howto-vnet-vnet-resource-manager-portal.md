---
title: 'VNet-VNet VPN Gateway bağlantısını yapılandırma: Azure portal'
description: Resource Manager ve Azure portalı kullanarak sanal ağlar arasında VPN ağ geçidi bağlantısı oluşturun.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: cherylmc
ms.openlocfilehash: 18ef9d89a2366e6d4db3c3154bae0bd83e0386f1
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77654784"
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-by-using-the-azure-portal"></a>Azure portalını kullanarak bir VNet-VNet VPN ağ geçidi bağlantısı yapılandırma

Bu makalede, sanal ağlar (Vnet'ler) bağlanma VNet-VNet bağlantı türünü kullanarak yardımcı olur. Sanal ağlar farklı bölgelerde ve farklı aboneliklere ait olabilir. Farklı aboneliklerden sanal ağları birbirine bağlama, aboneliklerin aynı Active Directory kiracısıyla ilişkilendirilmiş olması gerekmez. 

![v2v diyagramı](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Bu makaledeki adımlarda, Azure Resource Manager dağıtım modeli için geçerlidir ve Azure portalını kullanın. Aşağıdaki makalelerde açıklanan seçeneklerini kullanarak, farklı bir dağıtım aracı veya modeli ile bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portalındaki](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure portal (klasik)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Farklı dağıtım modellerini bağlama - Azure portalı](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Farklı dağıtım modellerini bağlama - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

## <a name="about-connecting-vnets"></a>Sanal ağları bağlama hakkında

Aşağıdaki bölümlerde, sanal ağları bağlamak için farklı yollar açıklanmaktadır.

### <a name="vnet-to-vnet"></a>Sanal Ağdan Sanal Ağa

Bir VNet-VNet bağlantısını yapılandırma Vnet'leri bağlamak için basit bir yoludur. VNet-VNet bağlantı türünü (VNet2VNet) bir sanal ağı başka bir sanal ağa bağlama, bir şirket içi konumuna siteden siteye IPSec bağlantısı oluşturma işlemiyle benzerdir. Her iki bağlantı türü de IPSec/IKE ile güvenli bir tünel sunmak ve iletişim kurarken aynı şekilde işlev için bir VPN ağ geçidi kullanın. Ancak, yerel ağ geçidi yapılandırılmış biçimde farklıdır. 

Bir VNet-VNet bağlantı oluşturduğunuzda yerel ağ geçidi adres alanını otomatik olarak oluşturulan doldurulur ve. Bir sanal ağın adres alanını güncelleştirirseniz diğer sanal ağ güncelleştirilmiş adres alanına otomatik olarak yönlendirir. Genellikle, daha hızlı ve kolay bir siteden siteye bağlantı'dan VNet-VNet bağlantısı oluşturmak.

### <a name="site-to-site-ipsec"></a>Siteden Siteye (IPsec)

Karmaşık bir ağ yapılandırmasıyla çalışıyorsanız, sanal ağlarınızı [siteden siteye bağlantı](vpn-gateway-howto-site-to-site-resource-manager-portal.md) kullanarak bağlamayı tercih edebilirsiniz. Siteden siteye IPSec adımlarını takip, yerel ağ geçitlerini kendiniz oluşturup yapılandırırsınız. Her sanal ağa ait yerel ağ geçidi, diğer sanal ağa yerel bir site gibi davranır. Bu adımlar yerel ağ geçidi trafiği yönlendirmek için ek adres alanları belirtmenizi sağlar. Bir sanal ağın adres alanı değiştiğinde, ona karşılık gelen yerel ağ geçidini el ile güncelleştirmeniz gerekir.

### <a name="vnet-peering"></a>VNet eşlemesi

Sanal ağlarınız VNet eşlemesi kullanarak da bağlanabilirsiniz. VNet eşlemesi bir VPN ağ geçidi kullanmaz ve farklı kısıtlamaları vardır. Ayrıca, [sanal ağ eşleme fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network), [Sanal Ağlar Arası VPN Gateway fiyatlandırmasından](https://azure.microsoft.com/pricing/details/vpn-gateway) farklı olarak hesaplanır. Daha fazla bilgi için bkz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md).

## <a name="why-create-a-vnet-to-vnet-connection"></a>Neden bir VNet-VNet bağlantı oluştururum

Aşağıdaki nedenlerle bir VNet-VNet bağlantısı kullanarak sanal ağları bağlama isteyebilirsiniz:

### <a name="cross-region-geo-redundancy-and-geo-presence"></a>Çapraz bölgede coğrafi yedeklilik ve iletişim durumu

  * Kendi coğrafi çoğaltma veya eşitlemenizi güvenli bağlantıyla internet'e yönelik Uç noktalara gitmeden ayarlayabilirsiniz.
  * Azure Traffic Manager ve Azure Load Balancer ile yüksek oranda kullanılabilir iş yükü coğrafi yedeklilik ile birçok Azure bölgesinde ayarlayabilirsiniz. Örneğin, SQL Server Always On kullanılabilirlik gruplarını Azure bölgelerinde ayarlayabilirsiniz.

### <a name="regional-multi-tier-applications-with-isolation-or-administrative-boundaries"></a>Yalıtım veya yönetim sınır bölgesel çok katmanlı uygulamalar

  * Aynı bölge içinde yalıtım ve yönetim gereksinimlerinden dolayı birbirlerine bağlı birden fazla sanal ağ ile çok katmanlı uygulamalar ayarlayabilirsiniz.

Hatta Sanal Ağdan Sanal Ağa iletişim çok siteli yapılandırmalarla bile birleştirilebilir. Bu yapılandırmalar birleştiren ağ topolojileri kurabilmenize olanak sağlar içi ve dışı karışık bağlantı ile sanal ağlar arası bağlantı, aşağıdaki diyagramda gösterildiği gibi:

![Bağlantılar hakkında](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Bağlantılar hakkında")

Bu makale, VNet-VNet bağlantı türünü kullanarak sanal ağları bağlama işlemini göstermektedir. Bir alıştırma olarak adımları izlediğinizde, aşağıdaki örnek ayar değerlerini kullanabilirsiniz. Örnekte, sanal ağlar aynı abonelikte ancak farklı kaynak gruplarındadır. Sanal ağlarınız farklı aboneliklerdeyse, portalda bağlantı oluşturamazsınız. Bunun yerine [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) veya [CLI](vpn-gateway-howto-vnet-vnet-cli.md) kullanın. VNet-VNet bağlantıları hakkında daha fazla bilgi için bkz. [VNET-VNET hakkında SSS](#vnet-to-vnet-faq).

### <a name="example-settings"></a>Örnek ayarlar

**VNet1 için değerler:**

- **Sanal ağ ayarları**
    - **Ad**: VNet1
    - **Adres alanı**: 10.11.0.0/16
    - **Abonelik**: kullanmak istediğiniz aboneliği seçin.
    - **Kaynak grubu**: TestRG1
    - **Konum**: Doğu ABD
    - **Alt ağ**
        - **Ad**: ön uç
        - **Adres aralığı**: 10.11.0.0/24
    - **Ağ geçidi alt ağı**:
        - **Ad**: *gatewaysubnet* , oto doldurulmuştur
        - **Adres aralığı**: 10.11.255.0/27

- **Sanal ağ geçidi ayarları**
    - **Ad**: VNet1GW
    - **Ağ geçidi türü**: **VPN**’i seçin.
    - **VPN türü**: **Rota tabanlı**' ı seçin.
    - **SKU**: kullanmak istediğiniz ağ geçidi SKU 'sunu seçin.
    - **Genel IP adresi adı**: VNet1GWpip
    - **Bağlanma**
       - **Ad**: VNet1toVNet4
       - **Paylaşılan anahtar**: paylaşılan anahtarı kendiniz oluşturabilirsiniz. Sanal ağlar arası bağlantı oluşturduğunuzda, değerlerin eşleşmesi gerekir. Bu alıştırma için abc123 kullanın.

**Ile vnet4 arasında için değerler:**

- **Sanal ağ ayarları**
   - **Ad**: ile vnet4 arasında
   - **Adres alanı**: 10.41.0.0/16
   - **Abonelik**: kullanmak istediğiniz aboneliği seçin.
   - **Kaynak grubu**: TestRG4
   - **Konum**: Batı ABD
   - **Alt ağ** 
      - **Ad**: ön uç
      - **Adres aralığı**: 10.41.0.0/24
   - **GatewaySubnet** 
      - **Ad**: *gatewaysubnet* , oto doldurulmuştur
      - **Adres aralığı**: 10.41.255.0/27

- **Sanal ağ geçidi ayarları** 
    - **Ad**: VNet4GW
    - **Ağ geçidi türü**: **VPN**’i seçin.
    - **VPN türü**: **Rota tabanlı**' ı seçin.
    - **SKU**: kullanmak istediğiniz ağ geçidi SKU 'sunu seçin.
    - **Genel IP adresi adı**: VNet4GWpip
    - **Bağlanma** 
       - **Ad**: VNet4toVNet1
       - **Paylaşılan anahtar**: paylaşılan anahtarı kendiniz oluşturabilirsiniz. Sanal ağlar arası bağlantı oluşturduğunuzda, değerlerin eşleşmesi gerekir. Bu alıştırma için abc123 kullanın.

## <a name="create-and-configure-vnet1"></a>VNet1 oluşturma ve yapılandırma
Zaten bir sanal ağınız varsa, ayarların VPN ağ geçidi tasarımınızla uyumlu olduğunu doğrulayın. Diğer ağlarla çakışabilecek herhangi bir alt ağ olup olmadığına özellikle dikkat edin. Çakışan alt ağlarınız varsa bağlantınız düzgün çalışmaz.

### <a name="to-create-a-virtual-network"></a>Sanal ağ oluşturmak için
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="add-additional-address-space-and-create-subnets"></a>Ek adres alanı ekleme ve alt ağ oluşturma
Sanal ağınız oluşturulduktan sonra ek adres alanı ekleyebilir ve alt ağ oluşturabilirsiniz.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="create-a-virtual-network-gateway"></a>Sanal ağ geçidi oluşturma
Bu adımda sanal ağınız için sanal ağ geçidi oluşturacaksınız. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir. Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız, bkz. [örnek ayarlar](#example-settings).

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

### <a name="to-create-a-virtual-network-gateway"></a>Bir sanal ağ geçidi oluşturmak için
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="create-and-configure-vnet4"></a>Ile vnet4 arasında oluşturma ve yapılandırma
VNet1 yapılandırdıktan sonra, önceki adımları tekrarlayarak ve değerleri Ile vnet4 arasında değerlerle değiştirerek Ile vnet4 arasında oluşturun. Ile vnet4 arasında yapılandırmadan önce VNet1 için sanal ağ geçidi oluşturmayı bitirene kadar beklemeniz gerekmez. Kendi değerlerinizi kullanıyorsanız, tüm bağlanmak istediğiniz sanal ağ adres alanlarının çakışmadığından emin olun.

## <a name="configure-the-vnet1-gateway-connection"></a>VNet1 ağ geçidi bağlantısını yapılandırma
Hem VNet1 hem de Ile vnet4 arasında için sanal ağ geçitleri tamamlandığında, sanal ağ geçidi bağlantılarınızı oluşturabilirsiniz. Bu bölümde VNet1 ile VNet4 arasında bir bağlantı oluşturursunuz. Bu adımlar yalnızca aynı abonelikteki sanal ağlar için geçerlidir. Sanal ağlarınız farklı aboneliklerdeyse, bağlantıyı kurmak için [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) kullanmanız gerekir. Sanal ağlarınız aynı Abonelikteki farklı kaynak gruplarında yoksa, ancak, bunları portalını kullanarak bağlanabilirsiniz.

1. Azure portal, **tüm kaynaklar**' ı seçin, arama kutusuna *sanal ağ geçidi* girin ve ardından VNET 'iniz için sanal ağ geçidine gidin. Örneğin, **VNet1GW**. **Sanal ağ geçidi** sayfasını açmak için ağ geçidini seçin. **Ayarlar**altında **Bağlantılar**' ı seçin.

   ![Bağlantılar sayfası](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connections.png "Bağlantılar sayfası")
2. **Bağlantı ekle** sayfasını açmak Için **+ Ekle** ' yi seçin.

   ![Bağlantı ekle](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/vnet1-to-vnet4.png "bağlantı ekleme")
3. **Bağlantı ekle** sayfasında, bağlantınızın değerlerini girin:

   - **Ad**: bağlantınız için bir ad girin. Örneğin, *VNet1toVNet4*.

   - **Bağlantı türü**: açılan listeden VNet **-VNET** ' i seçin.

   - **İlk sanal ağ geçidi**: Bu bağlantıyı belirtilen sanal ağ geçidinden oluşturduğunuz için, bu alan değeri otomatik olarak doldurulur.

   - **İkinci sanal ağ geçidi**: Bu alan, bağlantı oluşturmak istediğiniz VNET 'in sanal ağ geçidindir. **Sanal ağ geçidi Seç** sayfasını açmak için **başka bir sanal ağ geçidi seçin öğesini** seçin.

     - Bu sayfada listelenen sanal ağ geçitlerini görüntüleyin. Yalnızca aboneliğinizdeki sanal ağ geçitleri listelenir. Aboneliğinizde olmayan bir sanal ağ geçidine bağlanmak istiyorsanız, [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)kullanın.

     - Bağlanmak istediğiniz sanal ağ geçidi seçin.

     - **Paylaşılan anahtar (PSK)** : Bu alana, bağlantınız için paylaşılan bir anahtar girin. Bu anahtarı kendiniz üretebilir veya oluşturabilirsiniz. Siteden siteye bağlantı, kullanacağınız anahtarın şirket içi Cihazınızı ve sanal ağ geçidi bağlantınızı aynıdır. Başka bir sanal ağ geçidi için bağlantı kurduğunuz, kavram, yerine bir VPN cihazına bağlanmak dışında burada benzer.
    
4. Değişikliklerinizi kaydetmek için **Tamam ' ı** seçin.

## <a name="configure-the-vnet4-gateway-connection"></a>Ile vnet4 arasında ağ geçidi bağlantısını yapılandırma
Sonra, Ile vnet4 arasında ile VNet1 arasında bir bağlantı oluşturun. Portalda, Ile vnet4 arasında ile ilişkili sanal ağ geçidini bulun. Ile vnet4 arasında ile VNet1 arasında bağlantı oluşturmak için değerleri değiştirerek önceki bölümdeki adımları izleyin. Aynı paylaşılan anahtarı kullandığınızdan emin olun.

## <a name="verify-your-connections"></a>Bağlantılarınızı doğrulayın

1. Azure portalında sanal ağ geçidini bulun. 
2. Sanal **ağ geçidi** sayfasında, sanal ağ geçidinin **Bağlantılar** sayfasını görüntülemek için **Bağlantılar** ' ı seçin. Bağlantı kurulduktan sonra **durum** değerlerinin **bağlı**olarak değiştirilmesini görürsünüz.

   ![Bağlantıları doğrula](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/view-connections.png "Bağlantıları doğrula")
3. **Ad** sütununda, daha fazla bilgi görüntülemek için bağlantılardan birini seçin. Veriler akışa başladığında, **Içindeki verilerin** değerlerini ve **dışarı veri**akışını görürsünüz.

   ![Durum](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/status.png "Durum")

## <a name="add-additional-connections"></a>Başka bağlantılar eklemek

Ek bağlantılar eklemek istiyorsanız, bağlantıyı oluşturmak istediğiniz sanal ağ geçidine gidin ve **Bağlantılar**' ı seçin. Başka bir VNet-VNet bağlantısı oluşturabilir veya bir şirket içi konum ile IPSec Siteden Siteye bağlantısı oluşturabilirsiniz. **Bağlantı türünü**, oluşturmak istediğiniz bağlantı türüyle eşleşecek şekilde ayarladığınızdan emin olun. Başka bağlantılar oluşturmadan önce sanal ağınız için adres alanlarının bağlanmak istediğiniz adres alanlarından herhangi biriyle ile çakışmayacak doğrulayın. Siteden Siteye bağlantı oluşturma adımları için bkz. [Siteden Siteye bağlantı oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md).

## <a name="vnet-to-vnet-faq"></a>Sanal Ağdan Sanal Ağa - SSS
Sanal ağlar arası bağlantılar hakkında ek bilgi için SSS sayfasını görüntüleyin.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ trafiğini bir sanal ağ içindeki kaynaklarla nasıl sınırlayabilmeniz hakkında bilgi için bkz. [ağ güvenliği](../virtual-network/security-overview.md).

Azure, şirket içi ve İnternet kaynakları arasındaki trafiğin Azure tarafından nasıl yönlendirdiği hakkında bilgi için bkz. [Sanal ağ trafiği yönlendirme](../virtual-network/virtual-networks-udr-overview.md).
