---
title: "VNet'e vnet VPN Ağ Geçidi bağlantısını yapılandırma: Azure portalı"
description: Resource Manager ve Azure portalı kullanarak sanal ağlar arasında VPN ağ geçidi bağlantısı oluşturun.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/05/2020
ms.author: cherylmc
ms.openlocfilehash: 3d91203253c08acdaa159fc70f7a34fa7fca20c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78674180"
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-by-using-the-azure-portal"></a>Azure portalını kullanarak VNet'ten VNet VPN ağ geçidi bağlantısını yapılandırın

Bu makale, VNet-to-VNet bağlantı türünü kullanarak sanal ağları (VNet) bağlamanıza yardımcı olur. Sanal ağlar farklı bölgelerde ve farklı aboneliklerden olabilir. Farklı aboneliklerden VNet bağladığınızda, aboneliklerin aynı Active Directory kiracısıyla ilişkilendirilmesi gerekmez. 

![v2v diyagramı](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Bu makaledeki adımlar Azure Kaynak Yöneticisi dağıtım modeline uygulanır ve Azure portalını kullanır. Aşağıdaki makalelerde açıklanan seçenekleri kullanarak farklı bir dağıtım aracı veya modeli ile bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portalında](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Powershell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure portal (klasik)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Farklı dağıtım modellerini bağlama - Azure portalı](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Farklı dağıtım modellerini bağlama - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

## <a name="about-connecting-vnets"></a>Sanal ağları bağlama hakkında

Aşağıdaki bölümlerde sanal ağları bağlamanın farklı yolları açıklanabilir.

### <a name="vnet-to-vnet"></a>Sanal Ağdan Sanal Ağa

VNet'den VNet bağlantısına yapılandırmak, VNet'leri bağlamanın basit bir yoludur. Sanal bir ağı VNet'ten VNet bağlantı türüne (VNet2VNet) sahip başka bir sanal ağa bağladığınızda, bu, şirket içi bir konuma Siteden Siteye IPsec bağlantısı oluşturmaya benzer. Her iki bağlantı türü de IPsec/IKE ile güvenli bir tünel sağlamak için vpn ağ geçidi kullanır ve iletişim kurarken aynı şekilde çalışır. Ancak, yerel ağ ağ geçidinin yapılandırılması nda farklılık gösterirler. 

VNet'ten VNet'e bağlantı oluşturduğunuzda, yerel ağ ağ bağlantısı alanı otomatik olarak oluşturulur ve doldurulur. Bir VNet'in adres alanını güncellerseniz, diğer VNet otomatik olarak güncelleştirilmiş adres alanına yönlendirir. Genellikle Bir VNet-to-VNet bağlantısı oluşturmak, Site'den Siteye bağlantıdan daha hızlı ve daha kolaydır.

### <a name="site-to-site-ipsec"></a>Siteden Siteye (IPsec)

Karmaşık bir ağ yapılandırmasıyla çalışıyorsanız, Bunun yerine [Site'den Siteye bağlantı](vpn-gateway-howto-site-to-site-resource-manager-portal.md) kullanarak VNet'lerinizi bağlamayı tercih edebilirsiniz. Siteden Siteye IPsec adımlarını izlediğinizde, yerel ağ ağ ağ geçitlerini el ile oluşturur ve yapılandırırsınız. Her sanal ağa ait yerel ağ geçidi, diğer sanal ağa yerel bir site gibi davranır. Bu adımlar, yerel ağ ağ ağ geçidi için ek adres alanları belirtmenize olanak tanır. Bir VNet'in adres alanı değişirse, ilgili yerel ağ ağ ağ geçidini el ile güncelleştirmeniz gerekir.

### <a name="vnet-peering"></a>VNet eşlemesi

VNet peering'i kullanarak VNet'lerinizi de bağlayabilirsiniz. VNet peering VPN ağ geçidi kullanmaz ve farklı kısıtlamaları vardır. Ayrıca, [sanal ağ eşleme fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network), [Sanal Ağlar Arası VPN Gateway fiyatlandırmasından](https://azure.microsoft.com/pricing/details/vpn-gateway) farklı olarak hesaplanır. Daha fazla bilgi için bkz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md).

## <a name="why-create-a-vnet-to-vnet-connection"></a>Neden sanal ağdan sanal ağa bağlantı oluşturmalısınız?

Aşağıdaki nedenlerden dolayı VNet-to-VNet bağlantısı kullanarak sanal ağları bağlamak isteyebilirsiniz:

### <a name="cross-region-geo-redundancy-and-geo-presence"></a>Çapraz bölgede coğrafi yedeklilik ve iletişim durumu

  * Internet'e bakan uç noktaların üzerinden geçmeden güvenli bağlantı ile kendi coğrafi çoğaltma veya senkronizasyon ayarlayabilirsiniz.
  * Azure Trafik Yöneticisi ve Azure Yük Dengeleyicisi ile, birden çok Azure bölgesinde coğrafi artıklıkla yüksek oranda kullanılabilir iş yükü ayarlayabilirsiniz. Örneğin, SQL Server Always On'u birden çok Azure bölgesinde kullanılabilirlik gruplarında ayarlayabilirsiniz.

### <a name="regional-multi-tier-applications-with-isolation-or-administrative-boundaries"></a>Yalıtım lı veya idari sınırları olan bölgesel çok katmanlı uygulamalar

  * Aynı bölge içinde, yalıtım veya yönetim gereksinimleri nedeniyle birbirine bağlı birden çok sanal ağ içeren çok katmanlı uygulamalar ayarlayabilirsiniz.

Hatta Sanal Ağdan Sanal Ağa iletişim çok siteli yapılandırmalarla bile birleştirilebilir. Bu yapılandırmalar, aşağıdaki diyagramda gösterildiği gibi, binalar arası bağlantıyı sanal ağ bağlantısıyla birleştiren ağ topolojileri oluşturmanıza olanak tanır:

![Bağlantılar hakkında](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Bağlantılar hakkında")

Bu makalede, VNet-to-VNet bağlantı türünü kullanarak VNet'leri nasıl bağlayabileceğiniz gösterilmektedir. Bu adımları bir alıştırma olarak izlediğinizde, aşağıdaki örnek ayarlar değerlerini kullanabilirsiniz. Örnekte, sanal ağlar aynı abonelikte ancak farklı kaynak gruplarındadır. Sanal ağlarınız farklı aboneliklerdeyse, portalda bağlantı oluşturamazsınız. Bunun yerine [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) veya [CLI](vpn-gateway-howto-vnet-vnet-cli.md) kullanın. VNet'ten VNet'e bağlantılar hakkında daha fazla bilgi için [VNet-to-VNet SSS bölümüne](#vnet-to-vnet-faq)bakın.

### <a name="example-settings"></a>Örnek ayarlar

**VNet1 değerleri:**

- **Sanal ağ ayarları**
    - **Ürün Adi**: VNet1
    - **Adres alanı**: 10.1.0.0/16
    - **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
    - **Kaynak grubu**: TestRG1
    - **Yer**: Doğu ABD
    - **Alt ağ**
        - **Adı**: FrontEnd
        - **Adres aralığı**: 10.1.0.0/24
    - **Ağ geçidi alt ağı**:
        - **Adı**: *GatewaySubnet* otomatik doldurulmuş
        - **Adres aralığı**: 10.1.255.0/27

- **Sanal ağ ağ geçidi ayarları**
    - **Ürün Adi**: VNet1GW
    - **Ağ geçidi türü**: **VPN**’i seçin.
    - **VPN türü**: **Rota tabanlı**seçin.
    - **SKU**: Kullanmak istediğiniz ağ geçidi SKU'yu seçin.
    - **Genel IP adresi adı**: VNet1GWpip
    - **Bağlantı**
       - **Adı**: VNet1toVNet4
       - **Paylaşılan anahtar**: Paylaşılan anahtarı kendiniz oluşturabilirsiniz. VNet'ler arasındaki bağlantıyı oluşturduğunuzda, değerlerin eşleşmesi gerekir. Bu egzersiz için, abc123 kullanın.

**VNet4 değerleri:**

- **Sanal ağ ayarları**
   - **Ürün Adi**: VNet4
   - **Adres alanı**: 10.41.0.0/16
   - **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
   - **Kaynak grubu**: TestRG4
   - **Yer**: Batı ABD
   - **Alt ağ** 
      - **Adı**: FrontEnd
      - **Adres aralığı**: 10.41.0.0/24
   - **GatewaySubnet** 
      - **Adı**: *GatewaySubnet* otomatik doldurulmuş
      - **Adres aralığı**: 10.41.255.0/27

- **Sanal ağ ağ geçidi ayarları** 
    - **Ürün Adi**: VNet4GW
    - **Ağ geçidi türü**: **VPN**’i seçin.
    - **VPN türü**: **Rota tabanlı**seçin.
    - **SKU**: Kullanmak istediğiniz ağ geçidi SKU'yu seçin.
    - **Genel IP adresi adı**: VNet4GWpip
    - **Bağlantı** 
       - **Adı**: VNet4toVNet1
       - **Paylaşılan anahtar**: Paylaşılan anahtarı kendiniz oluşturabilirsiniz. VNet'ler arasındaki bağlantıyı oluşturduğunuzda, değerlerin eşleşmesi gerekir. Bu egzersiz için, abc123 kullanın.

## <a name="create-and-configure-vnet1"></a>VNet1 oluşturma ve yapılandırma
Zaten bir sanal ağınız varsa, ayarların VPN ağ geçidi tasarımınızla uyumlu olduğunu doğrulayın. Diğer ağlarla çakışabilecek herhangi bir alt ağ olup olmadığına özellikle dikkat edin. Çakışan alt ağlarınız varsa bağlantınız düzgün çalışmaz.

### <a name="to-create-a-virtual-network"></a>Sanal ağ oluşturmak için
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="create-the-vnet1-gateway"></a>VNet1 ağ geçidini oluşturma
Bu adımda sanal ağınız için sanal ağ geçidi oluşturacaksınız. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir. Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız, [Örnek ayarlarına](#example-settings)bakın.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

### <a name="to-create-a-virtual-network-gateway"></a>Bir sanal ağ geçidi oluşturmak için
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="create-and-configure-vnet4"></a>VNet4 oluşturma ve yapılandırma
VNet1'i yapılandırdıktan sonra, önceki adımları yineleyerek ve değerleri VNet4 değerleriyle değiştirerek VNet4 ve VNet4 ağ geçidi oluşturun. VNet4'ü yapılandırmadan önce VNet1'in sanal ağ ağ geçidinin oluşturmayı bitirmesini beklemeniz gerekmez. Kendi değerlerinizi kullanıyorsanız, adres boşluklarının bağlanmak istediğiniz VNet'lerden biriyle çakışmadığından emin olun.

## <a name="configure-the-vnet1-gateway-connection"></a>VNet1 ağ geçidi bağlantısını yapılandırma
Hem VNet1 hem de VNet4 için sanal ağ ağ geçitleri tamamlandığında, sanal ağ ağ ağ geçidi bağlantılarınızı oluşturabilirsiniz. Bu bölümde VNet1 ile VNet4 arasında bir bağlantı oluşturursunuz. Bu adımlar yalnızca aynı abonelikteki sanal ağlar için geçerlidir. VNet'leriniz farklı aboneliklerdeyse, bağlantıyı yapmak için [PowerShell'i](vpn-gateway-vnet-vnet-rm-ps.md) kullanmanız gerekir. Ancak, VNet'leriniz aynı abonelikte farklı kaynak gruplarındaysa, portalı kullanarak bunları bağlayabilirsiniz.

1. Azure portalında **Tüm kaynaklar'ı**seçin, arama kutusuna *sanal ağ ağ geçidine* girin ve ardından VNet'inizin sanal ağ ağ geçidine gidin. Örneğin, **VNet1GW**. Sanal ağ ağ **geçidi** sayfasını açmak için ağ geçidini seçin. **Ayarlar** **altında, Bağlantılar'ı**seçin.

   ![Bağlantılar sayfası](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connections.png "Bağlantılar sayfası")
2. **Bağlantı Ekle** sayfasını açmak için **+Ekle'yi** seçin.

   ![Bağlantı ekleme](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/vnet1-vnet4-connection.png "Bağlantı ekleme")
3. Bağlantı **Ekle** sayfasında bağlantınızın değerlerini doldurun:

   - **Adı**: Bağlantınız için bir ad girin. Örneğin, *VNet1toVNet4*.

   - **Bağlantı türü**: Açılan yerden **VNet'ten VNet'e** seçin.

   - **İlk sanal ağ ağ geçidi**: Belirtilen sanal ağ ağ ağ geçidinden bu bağlantıyı oluşturduğunuz için bu alan değeri otomatik olarak doldurulur.

   - **İkinci sanal ağ ağ geçidi**: Bu alan, bağlantı oluşturmak istediğiniz VNet'in sanal ağ ağ geçididir. Sanal ağ ağ geçidi sayfasını açmak için **başka bir sanal ağ ağ geçidi seçin'i** seçin. **Choose virtual network gateway**

     - Bu sayfada listelenen sanal ağ geçitlerini görüntüleyin. Yalnızca aboneliğinizdeki sanal ağ geçitleri listelenir. Aboneliğinizde olmayan bir sanal ağ ağ geçidine bağlanmak istiyorsanız [PowerShell'i](vpn-gateway-vnet-vnet-rm-ps.md)kullanın.

     - Bağlanmak istediğiniz sanal ağ ağ ağ geçidini seçin.

     - **Paylaşılan anahtar (PSK)**: Bu alana bağlantınız için paylaşılan bir anahtar girin. Bu anahtarı kendiniz üretebilir veya oluşturabilirsiniz. Siteden siteye bağlantıda, kullandığınız anahtar şirket içi aygıtınız ve sanal ağ ağ geçidi bağlantınız için aynıdır. Bir VPN aygıtına bağlanmak yerine başka bir sanal ağ ağ geçidine bağlanmanız dışında, kavram burada da benzer.
    
4. Değişikliklerinizi kaydetmek için **Tamam**’ı seçin.

## <a name="configure-the-vnet4-gateway-connection"></a>VNet4 ağ geçidi bağlantısını yapılandırma
Ardından, VNet4'ten VNet1'e bir bağlantı oluşturun. Portalda, VNet4 ile ilişkili sanal ağ ağ geçidini bulun. VNet4'ten VNet1'e bağlantı oluşturmak için değerleri değiştirerek önceki bölümden adımları izleyin. Aynı paylaşılan anahtarı kullandığınızdan emin olun.

## <a name="verify-your-connections"></a>Bağlantılarınızı doğrulayın

1. Azure portalında sanal ağ ağ geçidini bulun. 
2. Sanal **ağ ağ geçidi** sayfasında, sanal ağ ağ geçidinin **Bağlantılar** sayfasını görüntülemek için **Bağlantılar'ı** seçin. Bağlantı kurulduktan **sonra, Durum** değerlerinin **Bağlı**olarak değiştiğini görürsünüz.

   ![Bağlantıları doğrulama](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/view-connections.png "Bağlantıları doğrulama")
3. **Ad** sütununun altında, daha fazla bilgi görüntülemek için bağlantılardan birini seçin. Veriler akmaya başladığında, **Veri girişi** ve **Veri çıkışı**değerlerini görürsünüz.

   ![Durum](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/status.png "Durum")

## <a name="add-additional-connections"></a>Ek bağlantılar ekleme

Ek bağlantılar eklemek istiyorsanız, bağlantıyı oluşturmak istediğiniz sanal ağ ağ geçidine gidin ve **ardından Bağlantılar'ı**seçin. Başka bir VNet-VNet bağlantısı oluşturabilir veya bir şirket içi konum ile IPSec Siteden Siteye bağlantısı oluşturabilirsiniz. **Bağlantı türünü**, oluşturmak istediğiniz bağlantı türüyle eşleşecek şekilde ayarladığınızdan emin olun. Ek bağlantılar oluşturmadan önce, sanal ağınızın adres alanının bağlanmak istediğiniz adres alanlarıyla örtüşmediğini doğrulayın. Siteden Siteye bağlantı oluşturma adımları için bkz. [Siteden Siteye bağlantı oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md).

## <a name="vnet-to-vnet-faq"></a>Sanal Ağdan Sanal Ağa - SSS
Sanal ağlar arası bağlantılar hakkında ek bilgi için SSS sayfasını görüntüleyin.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ trafiğini sanal ağdaki kaynaklarla nasıl sınırlandırabileceğiniz hakkında bilgi için [Bkz.](../virtual-network/security-overview.md)

Azure, şirket içi ve İnternet kaynakları arasındaki trafiğin Azure tarafından nasıl yönlendirdiği hakkında bilgi için bkz. [Sanal ağ trafiği yönlendirme](../virtual-network/virtual-networks-udr-overview.md).
