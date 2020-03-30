---
title: Öğretici - Azure Virtual WAN kullanarak ExpressRoute bağlantıları oluşturma
description: Bu eğitimde, Azure ve şirket içi ortamlara ExpressRoute bağlantıları oluşturmak için Azure Virtual WAN'ı nasıl kullanacağınızı öğrenin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 02/13/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my corporate on-premises network(s) to my VNets using Virtual WAN and ExpressRoute.
ms.openlocfilehash: 35ca071cd8495611f0f350511ef9406f82c5be23
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77209435"
---
# <a name="tutorial-create-an-expressroute-association-using-azure-virtual-wan"></a>Öğretici: Azure Virtual WAN'ı kullanarak ExpressRoute ilişkilendirme oluşturma

Bu öğretici, Bir ExpressRoute devresi üzerinden Azure'daki kaynaklarınıza bağlanmak için Sanal WAN'ı nasıl kullanacağınızı gösterir. Sanal WAN ve Virtual WAN kaynakları hakkında daha fazla bilgi için [Sanal WAN Genel Bakış'a](virtual-wan-about.md)bakın.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sanal WAN oluşturma
> * Hub ve ağ geçidi oluşturma
> * Bir sanal ağı bir hub'a bağlama
> * Bir devreyi hub ağ geçidine bağlama
> * Bağlantıyı test etme
> * Ağ geçidi boyutunu değiştirme
> * Varsayılan rotanın reklamını yapmak

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Bağlanmak istediğiniz bir sanal ağınız var. Şirket içi ağlarınızın hiçbirinin bağlanmak istediğiniz sanal ağlarla örtüşmediğini doğrulayın. Azure portalında sanal ağ oluşturmak için [Hızlı Başlangıç'a](../virtual-network/quick-create-portal.md)bakın.

* Sanal ağınızın herhangi bir sanal ağ ağ geçidi yok. Sanal ağınızın bir ağ geçidi (VPN veya ExpressRoute) varsa, tüm ağ geçitlerini kaldırmanız gerekir. Bu yapılandırma, sanal ağların bunun yerine Sanal WAN hub ağ geçidine bağlanmasını gerektirir.

* Hub bölgenizden bir IP adresi aralığı edinin. Hub, Virtual WAN tarafından oluşturulan ve kullanılan sanal bir ağdır. Hub için belirttiğiniz adres aralığı, bağlandığınız varolan sanal ağlarınizle çakışamaz. Ayrıca bağlandığınız şirket içi adres aralıklarıyla da çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarını bilmiyorsanız, bu ayrıntıları sizin için sağlayabilecek biriyle işbirliği yapabilirsiniz.

* ExpressRoute devresi hub ağ geçidine bağlanmak için premium devre olmalıdır.

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

## <a name="create-a-virtual-wan"></a><a name="openvwan"></a>Sanal WAN oluşturma

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

1. Sanal WAN sayfasına gidin. Portalda **+Kaynak oluştur**’a tıklayın. Arama kutusuna **Sanal WAN** yazın ve Enter'u seçin.
2. Sonuçlardan **Sanal WAN'ı** seçin. Sanal WAN sayfasında, WAN Oluştur sayfasını açmak için **Oluştur'u** tıklatın.
3. WAN **Oluştur** sayfasında, **Temel Bilgiler** sekmesinde aşağıdaki alanları doldurun:

   ![WAN oluşturma](./media/virtual-wan-expressroute-portal/createwan.png)

   * **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
   * **Kaynak Grubu**: Yeni oluşturun veya var olanı kullanın.
   * **Kaynak grubu konumu** - Açılır kaynak konumundan bir kaynak konumu seçin. WAN, global bir kaynaktır ve belirli bir bölgeyle sınırlı değildir. Ancak oluşturduğunuz WAN kaynağını daha kolay yönetmek ve bulmak için bir bölge seçmeniz gerekir.
   * **Ad** - WAN adını vermek istediğiniz adı yazın.
   * **Türü** - **Standart**seçin. Temel SKU'yu kullanarak bir ExpressRoute ağ geçidi oluşturamazsınız.
4. Alanları doldurmayı bitirdikten sonra **Gözden Geçir +Oluştur'u**seçin.
5. Doğrulama geçtikten sonra, sanal WAN oluşturmak için **Oluştur'u** seçin.

## <a name="create-a-virtual-hub-and-gateway"></a><a name="hub"></a>Sanal hub ve ağ geçidi oluşturma

Sanal hub, Virtual WAN tarafından oluşturulan ve kullanılan sanal ağdır. VPN ve ExpressRoute gibi çeşitli ağ geçitleri içerebilir. Bu bölümde, sanal hub'ınız için bir ExpressRoute ağ geçidi oluşturacaksınız. Yeni bir sanal hub [oluşturduğunuzda](#newhub)ağ geçidini oluşturabilir veya düzenleyerek varolan bir [hub'da](#existinghub) ağ geçidi oluşturabilirsiniz. 

ExpressRoute ağ geçitleri 2 Gbps'lik birimler halinde sağlanmaktadır. 1 ölçek birimi = 10 ölçek birimine kadar destek ile 2 Gbps = 20 Gbps. Sanal bir hub ve ağ geçidinin tam olarak oluşturulması yaklaşık 30 dakika sürer.

### <a name="to-create-a-new-virtual-hub-and-a-gateway"></a><a name="newhub"></a>Yeni bir sanal hub ve ağ geçidi oluşturmak için

Yeni bir sanal hub oluşturun. Bir hub oluşturulduktan sonra, herhangi bir site eklemeseniz bile hub için ücretlendirilirsiniz.

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-er-hub-include.md)]

### <a name="to-create-a-gateway-in-an-existing-hub"></a><a name="existinghub"></a>Varolan bir hub'da ağ geçidi oluşturmak için

Ayrıca, varolan bir hub'da bir ağ geçidini düzenleyerek oluşturabilirsiniz.

1. Bunu ve onu seçmek istediğiniz sanal hub'a gidin.
2. Sanal **hub'ı edit** sayfasında, **ExpressRoute ağ geçidiekle**onay kutusunu seçin.
3. Değişikliklerinizi onaylamak için **Onayla'yı** seçin. Hub ve hub kaynaklarının tam olarak oluşturulması yaklaşık 30 dakika sürer.

   ![mevcut hub](./media/virtual-wan-expressroute-portal/edithub.png "hub'ı birden bir eve save")

### <a name="to-view-a-gateway"></a>Bir ağ geçidini görüntülemek için

Bir ExpressRoute ağ geçidi oluşturduktan sonra ağ geçidi ayrıntılarını görüntüleyebilirsiniz. Hub'a gidin, **ExpressRoute'u**seçin ve ağ geçidini görüntüleyin.

![Ağ geçidini görüntüle](./media/virtual-wan-expressroute-portal/viewgw.png "ağ geçidini görüntüleme")

## <a name="connect-your-vnet-to-the-hub"></a><a name="connectvnet"></a>VNet'inizi hub'a bağlayın

Bu bölümde, hub'ınız la VNet arasındaki eşleme bağlantısını oluşturursunuz. Bu adımları bağlanmak istediğiniz tüm sanal ağlar için tekrarlayın.

1. Sanal WAN'ınızın sayfasında **Sanal ağ bağlantısı**'na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+Bağlantı ekle**'ye tıklayın.
3. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağ zaten varolan bir sanal ağ ağ geçidine sahip olamaz (ne VPN, ne de ExpressRoute).

## <a name="connect-your-circuit-to-the-hub-gateway"></a><a name="connectcircuit"></a>Devrenizi hub ağ geçidine bağlayın

Ağ geçidi oluşturulduktan sonra, bir [ExpressRoute devresini](../expressroute/expressroute-howto-circuit-portal-resource-manager.md) ona bağlayabilirsiniz. ExpressRoute Global Reach destekli konumlardaki ExpressRoute Premium devreleri Sanal WAN ExpressRoute ağ geçidine bağlanabilir.

### <a name="to-connect-the-circuit-to-the-hub-gateway"></a>Devreyi hub ağ geçidine bağlamak için

Portalda, **Virtual hub -> Bağlantısı -> ExpressRoute** sayfasına gidin. Aboneliğinizde expressroute devresine erişiminiz varsa, kullanmak istediğiniz devreyi devreler listesinde görürsünüz. Herhangi bir devre görmüyorsanız, ancak bir yetkilendirme anahtarı ve eş devresi URI ile sağlanmışsa, bir devreyi kullanabilir ve bağlayabilirsiniz. Bkz. [Yetkilendirme anahtarını katarak bağlanmak](#authkey)için.

1. Devreyi seçin.
2. **Connect devresi(ler)'i**seçin.

   ![bağlantı devreleri](./media/virtual-wan-expressroute-portal/cktconnect.png "bağlantı devreleri")

### <a name="to-connect-by-redeeming-an-authorization-key"></a><a name="authkey"></a>Yetkilendirme anahtarını katarak bağlanmak için

Bağlanmak için size sağlanan yetkilendirme anahtarını ve devreURI'yi kullanın.

1. ExpressRoute sayfasında **+Yetkilendirme anahtarını kullan'ı** tıklatın

   ![Kurtarmak](./media/virtual-wan-expressroute-portal/redeem.png "Kurtarmak")
2. Yetkilendirme anahtarını kullan sayfasında, değerleri doldurun.

   ![anahtar değerleri kullanma](./media/virtual-wan-expressroute-portal/redeemkey2.png "anahtar değerleri kullanma")
3. Anahtarı eklemek için **Ekle'yi** seçin.
4. Devreyi görüntüleyin. Kullanılan devre yalnızca kullanıcınınkinden farklı bir abonelikte olduğundan adı (türü, sağlayıcı ve diğer bilgiler olmadan) gösterir.

## <a name="to-test-connectivity"></a>Bağlantıyı test etmek için

Devre bağlantısı kurulduktan sonra hub bağlantı durumu 'bu hub'ı gösterir ve bağlantının ExpressRoute ağ geçidine kurulduğunu gösterir. ExpressRoute devrenizin arkasındaki bir istemciden (örneğin, daha önce oluşturduğunuz VNet'te bir VM) bağlantıtest etmeden önce yaklaşık 5 dakika bekleyin.

ExpressRoute ağ geçidiyle aynı hub'da Sanal WAN VPN ağ geçidine bağlı siteleriniz varsa, VPN ve ExpressRoute bitiş noktaları arasında çift yönlü bağlantı sağlayabilirsiniz. Dinamik yönlendirme (BGP) desteklenir. Hub'daki ağ geçitlerinin ASN'si sabittir ve şu anda düzenlenemez.

## <a name="to-change-the-size-of-a-gateway"></a>Ağ geçidinin boyutunu değiştirmek için

ExpressRoute ağ geçidinizin boyutunu değiştirmek istiyorsanız, hub içindeki ExpressRoute ağ geçidini bulun ve açılır geçitten ölçek birimlerini seçin. Bozuk paranızı kaydedin. Hub ağ geçidini güncelleştirmek yaklaşık 30 dakika sürer.

![ağ geçidi boyutunu değiştirme](./media/virtual-wan-expressroute-portal/changescale.png "ağ geçidi boyutunu değiştirme")

## <a name="to-advertise-default-route-00000-to-endpoints"></a>Varsayılan rotanın reklamını yapmak için 0.0.0.0/0 uç noktaları

Azure sanal hub'ın ExpressRoute bitiş noktalarınıza varsayılan 0.0.0.0/0'ın reklamını yapmalarını istiyorsanız, 'Varsayılan rotayı yayıştır' diye etkinleştirmeniz gerekir.

1. Devre **->...-> Edit bağlantınızı**seçin.

   ![Bağlantıyı ede](./media/virtual-wan-expressroute-portal/defaultroute1.png "Bağlantıyı ede")

2. Varsayılan rotayı yaymak için **Etkinleştir'i** seçin.

   ![Varsayılan rotayı yayma](./media/virtual-wan-expressroute-portal/defaultroute2.png "Varsayılan rotayı yayma")

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
