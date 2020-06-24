---
title: 'ExpressRoute: sanal ağı bir devreye bağlama: Azure portal'
description: VNet 'i Azure ExpressRoute bağlantı hattına bağlayın. Nasıl yapılır adımları.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: how-to
ms.date: 09/17/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: a4ed99d69d1c2389e73c215bccfd6112895e791d
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84737079"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-the-portal"></a>Portalı kullanarak ExpressRoute bağlantı hattına bir sanal ağı bağlama
> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
> 

Bu makale, Azure portal kullanarak bir sanal ağı Azure ExpressRoute bağlantı hattını bağlamak için bir bağlantı oluşturmanıza yardımcı olur. Azure ExpressRoute bağlantı hattına bağlandığınız sanal ağlar aynı abonelikte olabilir ya da başka bir aboneliğin parçası olabilir.

## <a name="before-you-begin"></a>Başlamadan önce

* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md), [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir.
  * [ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) bağlantı hattı oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcınız tarafından devre dışı bırakıldı.
  * Devreniz için Azure özel eşlemesi 'nin yapılandırıldığından emin olun. Eşleme ve yönlendirme yönergeleri için [bir ExpressRoute bağlantı hattı oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md) makalesine bakın.
  * Uçtan uca bağlantıyı etkinleştirebilmeniz için Azure özel eşlemesinin yapılandırıldığından ve ağınız ile Microsoft arasındaki BGP eşlemesinin yapıldığından emin olun.
  * Oluşturulmuş ve tam olarak sağlanmış bir sanal ağa ve sanal ağ geçidine sahip olduğunuzdan emin olun. [ExpressRoute için sanal ağ geçidi oluşturma](expressroute-howto-add-gateway-resource-manager.md)yönergelerini izleyin. ExpressRoute için bir sanal ağ geçidi, VPN değil GatewayType ' ExpressRoute ' kullanır.

* 10 adede kadar sanal ağı standart bir ExpressRoute devresine bağlayabilirsiniz. Standart bir ExpressRoute bağlantı hattı kullanılırken tüm sanal ağların aynı geopolitik bölgede olması gerekir.

* Tek bir sanal ağ, en fazla dört ExpressRoute devresine bağlanabilir. Bağlandığınız her ExpressRoute bağlantı hattı için yeni bir bağlantı nesnesi oluşturmak için aşağıdaki işlemi kullanın. ExpressRoute devreleri aynı abonelikte, farklı aboneliklerde veya her ikisinin karışımı olabilir.

* ExpressRoute bağlantı hattını etkinleştirdiyseniz, sanal bir ağı ExpressRoute devresine ait geopolitik bölgesinin dışına veya ExpressRoute bağlantı hattına daha fazla sayıda sanal ağ bağlayabilirsiniz. Premium eklenti hakkında daha fazla bilgi için [SSS bölümüne](expressroute-faqs.md) bakın.

* Adımları daha iyi anlamak için başlamadan önce [bir video görüntüleyebilirsiniz](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) .

## <a name="connect-a-vnet-to-a-circuit---same-subscription"></a>VNet 'i devre dışı bir aboneliğe bağlama

> [!NOTE]
> Katman 3 sağlayıcısı eşlerinizi yapılandırdıysa BGP yapılandırma bilgileri gösterilmez. Devreniz sağlanmış durumdaysa bağlantı oluşturabileceksiniz.
>

### <a name="to-create-a-connection"></a>Bağlantı oluşturmak için

1. ExpressRoute devreniz ve Azure özel eşlerinizin başarıyla yapılandırıldığından emin olun. ExpressRoute bağlantı hattı [oluşturma](expressroute-howto-circuit-arm.md) ve [bir ExpressRoute bağlantı hattı için eşleme oluşturma ve değiştirme](expressroute-howto-routing-arm.md)bölümündeki yönergeleri izleyin. ExpressRoute bağlantı hattınızı aşağıdaki görüntüde şöyle görünmelidir:

   [![ExpressRoute devresi ekran görüntüsü](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png "Devreyi görüntüle")](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1-exp.png#lightbox)
2. Artık sanal ağ geçidinizin ExpressRoute Devrenize bağlamak için bir bağlantı sağlamaya başlayabilirsiniz. Bağlantı **Ekle ' ye tıklayarak**  >  **Add** **bağlantı ekle** sayfasını açın ve değerleri yapılandırın.

   [![Bağlantı ekran görüntüsü ekleme](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png "Bağlantı ekran görüntüsü ekleme")](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1-exp.png#lightbox)
3. Bağlantınız başarıyla yapılandırıldıktan sonra bağlantı nesneniz bağlantı bilgilerini gösterir.

   ![Bağlantı nesnesi ekran görüntüsü](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)


## <a name="connect-a-vnet-to-a-circuit---different-subscription"></a>VNet 'i devre dışı bir aboneliğe bağlama

Bir ExpressRoute devresini birden çok abonelik arasında paylaşabilirsiniz. Aşağıdaki şekilde, birden çok aboneliğin ExpressRoute devreleri için paylaşımın nasıl çalıştığı basit bir şematik gösterilmektedir.

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Büyük buluttaki küçük bulutların her biri, bir kuruluştaki farklı departmanlara ait olan abonelikleri temsil etmek için kullanılır.
- Kuruluştaki bölümlerin her biri, hizmetlerini dağıtmak için kendi aboneliğini kullanabilir, ancak şirket içi ağınıza geri bağlanmak için tek bir ExpressRoute bağlantı hattı paylaşabilir.
- Tek bir departman (Bu örnekte:) ExpressRoute devresine sahip olabilir. Kuruluştaki diğer abonelikler, diğer Azure Active Directory kiracılarına ve Kurumsal Anlaşma kayıtlarına bağlı abonelikler de dahil olmak üzere, bağlantı hattına ilişkin ExpressRoute bağlantı hattını ve yetkilendirmeleri kullanabilir.

  > [!NOTE]
  > Adanmış devre için bağlantı ve bant genişliği ücretleri ExpressRoute bağlantı hattı sahibine uygulanır. Tüm sanal ağlar aynı bant genişliğini paylaşır.
  >
  >

### <a name="administration---about-circuit-owners-and-circuit-users"></a>Yönetim-devre sahipleri ve devre kullanıcıları hakkında

' Devowner ', ExpressRoute bağlantı hattı kaynağının yetkili bir uzman kullanıcısı. Devre sahibi, ' devre kullanıcıları ' tarafından kullanılabilecek yetkilendirmeler oluşturabilir. Devre kullanıcıları ExpressRoute bağlantı hattı ile aynı abonelikte yer alan sanal ağ geçitlerinin sahiplerinden oluşur. Devre kullanıcıları yetkilendirmeleri (sanal ağ başına bir yetkilendirme) kullanabilir.

Devre sahibinin, her zaman yetkilendirmeleri değiştirme ve iptal etme gücü vardır. Erişim iptal edilen abonelikten tüm bağlantı bağlantılarında silinen bir yetkilendirme sonuçları iptal ediliyor.

### <a name="circuit-owner-operations"></a>Devre sahibi işlemleri

**Bağlantı yetkilendirmesi oluşturmak için**

Devre sahibi bir yetkilendirme oluşturur. Bu, bir devre kullanıcısı tarafından sanal ağ geçitlerini ExpressRoute devresine bağlamak üzere kullanılabilecek bir yetkilendirme anahtarı oluşturulmasına neden olur. Yetkilendirme yalnızca bir bağlantı için geçerlidir.

> [!NOTE]
> Her bağlantı için ayrı bir yetkilendirme gerekir.
>

1. ExpressRoute sayfasında, **yetkilendirmeler** ' e tıklayın ve ardından yetkilendirme için bir **ad** yazın ve **Kaydet**' e tıklayın.

   ![Yetkilendirmeler](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)
2. Yapılandırma kaydedildikten sonra **kaynak kimliği** ve **Yetkilendirme anahtarı**' nı kopyalayın.

   ![Yetkilendirme anahtarı](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**Bir bağlantı yetkilendirmesini silmek için**

Bağlantınız için sayfadaki **Sil** simgesini seçerek bir bağlantıyı silebilirsiniz.

### <a name="circuit-user-operations"></a>Devre Kullanıcı işlemleri

Devre kullanıcısının, devre sahibinden kaynak KIMLIĞI ve yetkilendirme anahtarı olması gerekir.

**Bir bağlantı yetkilendirmesini kullanma**

1. **+ Yeni** düğmesine tıklayın.

   ![Yeni ' ye tıklayın](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)
2. Market 'te **"bağlantı"** araması yapın, seçin ve **Oluştur**' a tıklayın.

   ![Bağlantı ara](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)
3. **Bağlantı türünün** "ExpressRoute" olarak ayarlandığından emin olun.
4. Ayrıntıları girin ve ardından temel bilgiler sayfasında **Tamam** ' a tıklayın.

   ![Temel bilgiler sayfası](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)
5. **Ayarlar** sayfasında, **sanal ağ geçidini** seçin ve **Yetkilendirmeyi** kullan onay kutusunu işaretleyin.
6. **Yetkilendirme anahtarını** ve **eş bağlantı devre URI** 'sini girin ve bağlantıya bir ad verin. **Tamam**'a tıklayın. **Eş devre URI 'si** , ExpressRoute bağlantı HATTıNıN kaynak kimliğidir (ExpressRoute devresinin Özellikler ayar bölmesinde bulabilirsiniz).

   ![Ayarlar sayfası](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)
7. **Özet** sayfasındaki bilgileri gözden geçirin ve **Tamam**' a tıklayın.

**Bir bağlantı yetkilendirmesini serbest bırakmak için**

ExpressRoute bağlantı hattını sanal ağa bağlayan bağlantıyı silerek bir yetkilendirmeyi serbest bırakabilirsiniz.

## <a name="delete-a-connection-to-unlink-a-vnet"></a>VNet bağlantısını kaldırmak için bir bağlantıyı silme

Bağlantınız için sayfadaki **Sil** simgesini seçerek bir bağlantıyı silebilir ve sanal ağınızın bir ExpressRoute bağlantı hattına bağlantısını kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
