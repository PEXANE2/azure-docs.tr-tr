---
title: 'ExpressRoute: sanal ağı bir devreye bağlama: Azure portal'
description: VNet 'i Azure ExpressRoute bağlantı hattına bağlayın. Nasıl yapılır adımları.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 4c7a24ad692086398059d1afd48c8927e9d18582
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78384249"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-the-portal"></a>Portalı kullanarak bir ExpressRoute bağlantı hattına bir sanal ağı bağlama
> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
> 

Bu makale, Azure portalını kullanarak bir Azure ExpressRoute bağlantı hattına bir sanal ağa bağlamak için bir bağlantı oluşturmanıza yardımcı olur. Azure ExpressRoute devreniz bağlandığınız sanal ağlar aynı abonelikte olabilir veya başka bir abonelik parçası olabilir.

## <a name="before-you-begin"></a>Başlamadan önce

* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md), [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir.
  * [ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) bağlantı hattı oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcınız tarafından devre dışı bırakıldı.
  * Bağlantı hattınız için yapılandırılmış Azure özel eşleme olduğundan emin olun. Eşleme ve yönlendirme yönergeleri için [bir ExpressRoute bağlantı hattı oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md) makalesine bakın.
  * Azure özel eşdüzey hizmet sağlama yapılandırılır ve uçtan uca bağlantıyı etkinleştirmek üzere ağınız ile Microsoft arasında BGP eşliği ayarlama olduğundan emin olun.
  * Bir sanal ağ ve oluşturulan ve tam olarak sağlanan sanal ağ geçidi olduğundan emin olun. [ExpressRoute için sanal ağ geçidi oluşturma](expressroute-howto-add-gateway-resource-manager.md)yönergelerini izleyin. ExpressRoute için sanal ağ geçidi, GatewayType 'ExpressRoute' VPN'değil kullanır.

* Standart bir ExpressRoute bağlantı hattı için en fazla 10 sanal ağlara bağlayabilirsiniz. Tüm sanal ağları, standart bir ExpressRoute bağlantı hattını kullanırken aynı jeopolitik bölgede olması gerekir.

* En fazla dört ExpressRoute bağlantı hatları için tek bir sanal ağa bağlanabilir. Bağlanmakta olduğunuz her bir ExpressRoute bağlantı hattı için yeni bir bağlantı nesnesi oluşturmak için aşağıdaki işlemi kullanın. ExpressRoute bağlantı hatları, aynı abonelik, farklı Aboneliklerde veya her ikisinin bir karışımı olabilir.

* Bir sanal ağ ExpressRoute bağlantı hattının coğrafi bölge dışında bağlama ya da ExpressRoute premium eklentisi etkinleştirildiğinde, çok sayıda sanal ağları ExpressRoute devreniz bağlayın. Premium eklenti hakkında daha fazla bilgi için [SSS bölümüne](expressroute-faqs.md) bakın.

* Adımları daha iyi anlamak için başlamadan önce [bir video görüntüleyebilirsiniz](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) .

## <a name="connect-a-vnet-to-a-circuit---same-subscription"></a>Devreye - aynı Abonelikteki bir sanal ağa bağlama

> [!NOTE]
> BGP yapılandırma bilgilerini Katman 3 sağlayıcısı, eşlemeler yapılandırılıp yapılandırılmadığını gösterilmez. Bağlantı hattınızın sağlanmış bir durumda ise, bağlantıları oluşturabilirsiniz.
>

### <a name="to-create-a-connection"></a>Bir bağlantı oluşturmak için

1. ExpressRoute bağlantı hattı ve Azure özel eşdüzey hizmet sağlama başarılı bir şekilde yapılandırıldığından emin olun. ExpressRoute bağlantı hattı [oluşturma](expressroute-howto-circuit-arm.md) ve [bir ExpressRoute bağlantı hattı için eşleme oluşturma ve değiştirme](expressroute-howto-routing-arm.md)bölümündeki yönergeleri izleyin. ExpressRoute devreniz şu resimdeki gibi görünmelidir:

   [![ExpressRoute devresi ekran görüntüsü](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png "Devreyi görüntüle")](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1-exp.png#lightbox)
2. ExpressRoute bağlantı hattı için sanal ağ geçidinizin bağlantı için bağlantı sağlama şimdi başlayabilirsiniz. Bağlantı ** > ** **Ekle** ' ye tıklayarak **bağlantı ekle** sayfasını açın ve değerleri yapılandırın.

   [![Bağlantı ekran görüntüsü ekleme](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png "Bağlantı ekran görüntüsü ekleme")](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1-exp.png#lightbox)
3. Bağlantınızın başarılı bir şekilde yapılandırıldıktan sonra bağlantı nesnesi bağlantı bilgilerini gösterir.

   ![Bağlantı nesnesi ekran görüntüsü](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)


## <a name="connect-a-vnet-to-a-circuit---different-subscription"></a>Devreye - farklı bir aboneliğe bir VNet bağlama

Bir ExpressRoute bağlantı hattı birden çok farklı abonelikler arasında paylaşabilirsiniz. Aşağıdaki şekilde basit bir ExpressRoute bağlantı hatları için nasıl paylaşım Works şematik, birden fazla aboneliği analiz gösterilmektedir.

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Her küçük bulutların büyük bulut içinde bir kuruluştaki farklı departmanlara ait abonelikleri temsil etmek için kullanılır.
- Her kuruluş içindeki bölümlerin hizmetlerini dağıtmak için kendi aboneliği kullanabilirsiniz ancak, şirket içi ağınıza bağlanmak için tek bir ExpressRoute bağlantı hattı paylaşabilirler.
- Tek bir bölüm (Bu örnekte: BT) ExpressRoute bağlantı hattına sahip olabilir. Kuruluştaki diğer Aboneliklerdeki ilişkili diğer Azure Active Directory kiracıları ve Kurumsal Anlaşma kayıtları bağlantılı abonelikleri de dahil olmak üzere, bağlantı hattı için yetkilendirme ve ExpressRoute bağlantı hattı kullanabilirsiniz.

  > [!NOTE]
  > ExpressRoute bağlantı hattı sahibinden için adanmış bir bağlantı hattı için bağlantı ve bant genişliği ücretleri uygulanır. Tüm sanal ağları, aynı bant genişliğini paylaşır.
  >
  >

### <a name="administration---about-circuit-owners-and-circuit-users"></a>Bağlantı hattı sahipleri ve bağlantı hattı kullanıcılar hakkındaki - yönetim

'Bağlantı hattı sahibinden' ExpressRoute bağlantı hattı kaynak yetkili bir güç kullanıcıdır. Bağlantı hattı sahibinden 'devre kullanıcıları tarafından' ödenebilecek yetkilendirmeleri oluşturabilirsiniz. ExpressRoute bağlantı hattı aynı abonelik içinde olmayan sanal ağ geçitleri sahipleri bağlantı hattını kullanıcılardır. Devre kullanıcılarının, yetkilendirmeleri (sanal ağ başına bir yetkilendirme) kullanmak.

Bağlantı hattı sahibinden yetkilendirme dilediğiniz zaman iptal et ve değiştirmek için gücüne sahiptir. Bir yetkilendirme sonuçlarına tüm bağlantı erişimini iptal edildi abonelikten silinmesini iptal ediliyor.

### <a name="circuit-owner-operations"></a>Bağlantı hattı sahibi işlemleri

**Bağlantı yetkilendirmesi oluşturmak için**

Bağlantı hattı sahibinden bir yetkilendirme oluşturur. ExpressRoute bağlantı hattına kendi sanal ağ geçitlerine bağlanmak için bir bağlantı hattı kullanıcısı tarafından kullanılan bir yetkilendirme anahtarına oluşturulmasını sonuçlanır. Bir yetkilendirme yalnızca bir bağlantı için geçerli değil.

> [!NOTE]
> Her bağlantı için ayrı bir yetkilendirme gerekir.
>

1. ExpressRoute sayfasında, **yetkilendirmeler** ' e tıklayın ve ardından yetkilendirme için bir **ad** yazın ve **Kaydet**' e tıklayın.

   ![Yetkilendirmeler](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)
2. Yapılandırma kaydedildikten sonra **kaynak kimliği** ve **Yetkilendirme anahtarı**' nı kopyalayın.

   ![Yetkilendirme anahtarı](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**Bir bağlantı yetkilendirmesini silmek için**

Bağlantınız için sayfadaki **Sil** simgesini seçerek bir bağlantıyı silebilirsiniz.

### <a name="circuit-user-operations"></a>Bağlantı hattı kullanıcı işlemleri

Bağlantı hattı kullanıcısı, kaynak Kimliğini ve bağlantı hattı sahibinden yetkilendirme anahtarı gerekir.

**Bir bağlantı yetkilendirmesini kullanma**

1. **+ Yeni** düğmesine tıklayın.

   ![Yeni'yi tıklatın](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)
2. Market 'te **"bağlantı"** araması yapın, seçin ve **Oluştur**' a tıklayın.

   ![Bağlantı arayın](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)
3. **Bağlantı türünün** "ExpressRoute" olarak ayarlandığından emin olun.
4. Ayrıntıları girin ve ardından temel bilgiler sayfasında **Tamam** ' a tıklayın.

   ![Temel bilgileri sayfası](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)
5. **Ayarlar** sayfasında, **sanal ağ geçidini** seçin ve **Yetkilendirmeyi** kullan onay kutusunu işaretleyin.
6. **Yetkilendirme anahtarını** ve **eş bağlantı devre URI** 'sini girin ve bağlantıya bir ad verin. **Tamam** düğmesine tıklayın. **Eş devre URI 'si** , ExpressRoute bağlantı HATTıNıN kaynak kimliğidir (ExpressRoute devresinin Özellikler ayar bölmesinde bulabilirsiniz).

   ![Ayarlar sayfası](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)
7. **Özet** sayfasındaki bilgileri gözden geçirin ve **Tamam**' a tıklayın.

**Bir bağlantı yetkilendirmesini serbest bırakmak için**

Sanal ağı ExpressRoute bağlantı hattına bağlayan bağlantı silerek bir yetkilendirme serbest bırakabilirsiniz.

## <a name="delete-a-connection-to-unlink-a-vnet"></a>Bir sanal ağ bağlantısını için bağlantıyı silme

Bağlantınız için sayfadaki **Sil** simgesini seçerek bir bağlantıyı silebilir ve sanal ağınızın bir ExpressRoute bağlantı hattına bağlantısını kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
