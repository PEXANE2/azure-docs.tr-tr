---
title: "ExpressRoute: Bir VNet'i bir devreye bağla: Azure portalı"
description: Bir VNet'i Azure ExpressRoute devresine bağlayın. Nasıl adımlar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 4c7a24ad692086398059d1afd48c8927e9d18582
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272922"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-the-portal"></a>Portalı kullanarak ExpressRoute bağlantı hattına bir sanal ağı bağlama
> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [Powershell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
> 

Bu makale, sanal bir ağı Azure portalını kullanarak bir Azure ExpressRoute devresine bağlamak için bir bağlantı oluşturmanıza yardımcı olur. Azure ExpressRoute devrenize bağlandığınız sanal ağlar aynı abonelikte olabilir veya başka bir aboneliğin parçası olabilir.

## <a name="before-you-begin"></a>Başlamadan önce

* Yapılandırmaya başlamadan önce [ön koşulları,](expressroute-prerequisites.md) [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir.
  * [Bir ExpressRoute devresi oluşturmak](expressroute-howto-circuit-portal-resource-manager.md) ve devreyi bağlantı sağlayıcınız tarafından etkinleştirmek için yönergeleri izleyin.
  * Devreniz için Azure özel eşlemeyapılı olduğundan emin olun. Eşleme ve yönlendirme yönergeleri için ExpressRoute devresi makalesi [için eşleme oluştur ve değiştirin.](expressroute-howto-routing-portal-resource-manager.md)
  * Azure özel eşlemenin yapılandırıldığından ve ağınızdan Microsoft'a bgp eşlemesi doldu böylece uçuca bağlantı sağlayabilirsiniz.
  * Sanal bir ağa ve sanal ağ ağ geçidinin oluşturulduğundan ve tam olarak sağlanmış olduğundan emin olun. [ExpressRoute için sanal ağ ağ geçidi oluşturmak için](expressroute-howto-add-gateway-resource-manager.md)yönergeleri izleyin. ExpressRoute için bir sanal ağ ağ geçidi, VPN değil, GatewayType 'ExpressRoute' kullanır.

* Standart bir ExpressRoute devresine en fazla 10 sanal ağ bağlayabilirsiniz. Standart bir ExpressRoute devresi kullanırken tüm sanal ağlar aynı jeopolitik bölgede olmalıdır.

* Tek bir VNet en fazla dört ExpressRoute devresine bağlanabilir. Bağlandığınız her ExpressRoute devresi için yeni bir bağlantı nesnesi oluşturmak için aşağıdaki işlemi kullanın. ExpressRoute devreleri aynı abonelikte, farklı aboneliklerde veya her ikisinin bir karışımında olabilir.

* ExpressRoute devresinin jeopolitik bölgesinin dışında bir sanal ağı bağlayabilir veya ExpressRoute premium eklentisini etkinleştirdiyseniz daha fazla sayıda sanal ağı ExpressRoute devrenize bağlayabilirsiniz. Premium eklenti hakkında daha fazla bilgi için [SSS'yi](expressroute-faqs.md) kontrol edin.

* Adımları daha iyi anlamak için bir videoyu daha iyi anlamaya başlamadan önce [görüntüleyebilirsiniz.](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)

## <a name="connect-a-vnet-to-a-circuit---same-subscription"></a>Bir Devre için bir VNet bağlayın - aynı abonelik

> [!NOTE]
> Katman 3 sağlayıcısı eşlemelerinizi yapılandırırsa BGP yapılandırma bilgileri gösterilmez. Devreniz uygun bir durumdaysa, bağlantı oluşturabilmelisiniz.
>

### <a name="to-create-a-connection"></a>Bağlantı oluşturmak için

1. ExpressRoute devrenizin ve Azure özel eşlemenizin başarıyla yapılandırıldığından emin olun. [ExpressRoute devresi Oluştur'daki](expressroute-howto-circuit-arm.md) yönergeleri izleyin ve [ExpressRoute devresi için eşleme oluşturun ve değiştirin.](expressroute-howto-routing-arm.md) ExpressRoute devreniz aşağıdaki görüntüye benzemelidir:

   [![ExpressRoute devre ekran görüntüsü](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png "Görünüm Devresi")](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1-exp.png#lightbox)
2. Artık sanal ağ ağ ağ geçidinizi ExpressRoute devrenize bağlamak için bir bağlantı kurmaya başlayabilirsiniz. **Bağlantı Ekle** sayfasını açmak ve sonra değerleri yapılandırmak için **Bağlantı** > **Ekle'yi** tıklatın.

   [![Bağlantı ekran görüntüsü ekleme](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png "Bağlantı Ekran Görüntüsü Ekle")](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1-exp.png#lightbox)
3. Bağlantınız başarıyla yapılandırıldıktan sonra, bağlantı nesneniz bağlantının bilgilerini gösterir.

   ![Bağlantı nesnesi ekran görüntüsü](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)


## <a name="connect-a-vnet-to-a-circuit---different-subscription"></a>Bir Devre için bir VNet bağlayın - farklı abonelik

Bir ExpressRoute devresi'ni birden çok abonelik arasında paylaşabilirsiniz. Aşağıdaki şekil, expressroute devreleri için birden çok abonelik arasında paylaşımın nasıl çalıştığını gösteren basit bir şema dır.

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Büyük bulutiçindeki küçük bulutların her biri, kuruluştaki farklı bölümlere ait abonelikleri temsil etmek için kullanılır.
- Kuruluş içindeki departmanların her biri hizmetlerini dağıtmak için kendi aboneliklerini kullanabilir, ancak şirket içi ağınıza bağlanmak için tek bir ExpressRoute devresini paylaşabilir.
- Tek bir departman (bu örnekte: BT) ExpressRoute devresi sahibi olabilir. Kuruluştaki diğer abonelikler, diğer Azure Active Directory kiracılarına bağlı abonelikler ve Kurumsal Sözleşme kayıtları da dahil olmak üzere ExpressRoute devresini ve devreyle ilişkili yetkilendirmeleri kullanabilir.

  > [!NOTE]
  > Özel devre için bağlantı ve bant genişliği ücretleri ExpressRoute devre sahibine uygulanacaktır. Tüm sanal ağlar aynı bant genişliğini paylaşır.
  >
  >

### <a name="administration---about-circuit-owners-and-circuit-users"></a>Yönetim - Devre sahipleri ve devre kullanıcıları hakkında

'Devre sahibi' ExpressRoute devre kaynağının yetkili bir Güç Kullanıcısıdır. Devre sahibi 'devre kullanıcıları' tarafından kullanılabilir yetkilendirmeler oluşturabilirsiniz. Devre kullanıcıları, ExpressRoute devresi ile aynı abonelik içinde olmayan sanal ağ ağ ağ geçitlerinin sahipleridir. Devre kullanıcıları yetkilendirmeleri kullanabilir (sanal ağ başına bir yetkilendirme).

Devre sahibi, yetkilendirmeleri istediği zaman değiştirme ve iptal etme yetkisine sahiptir. Yetkilendirmenin iptaledilmesi, erişimi iptal edilen abonelikten tüm bağlantı bağlantılarının silinmelerine neden olur.

### <a name="circuit-owner-operations"></a>Devre sahibi işlemleri

**Bağlantı yetkilendirmesi oluşturmak için**

Devre sahibi bir yetkilendirme oluşturur. Bu, bir devre kullanıcısı tarafından sanal ağ ağ ağ geçitlerini ExpressRoute devresine bağlamak için kullanılabilecek bir yetkilendirme anahtarı oluşturulmasıyla sonuçlanır. Yetkilendirme yalnızca bir bağlantı için geçerlidir.

> [!NOTE]
> Her bağlantı ayrı bir yetkilendirme gerektirir.
>

1. ExpressRoute sayfasında, **Yetkilendirmeler'i** tıklatın ve ardından yetkilendirme için bir **ad** yazın ve **Kaydet'i**tıklatın.

   ![Yetkilendirmeler](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)
2. Yapılandırma kaydedildikten sonra **Kaynak Kimliğini** ve **Yetkilendirme Anahtarını**kopyalayın.

   ![Yetkilendirme anahtarı](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**Bağlantı yetkisini silmek için**

Bağlantınız için sayfadaki **Sil** simgesini seçerek bağlantıyı silebilirsiniz.

### <a name="circuit-user-operations"></a>Devre kullanıcı işlemleri

Devre kullanıcısının kaynak kimliğine ve devre sahibinden bir yetkilendirme anahtarına ihtiyacı vardır.

**Bağlantı yetkisini kullanmak için**

1. **+Yeni** düğmesini tıklatın.

   ![Yeni'yi tıklatın](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)
2. Markette **"Bağlantı"yı** arayın, seçin ve **Oluştur'u**tıklatın.

   ![Bağlantı ara](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)
3. **Bağlantı türünün** "ExpressRoute" olarak ayarlandıklarına emin olun.
4. Ayrıntıları doldurun ve Temel Bilgiler sayfasında **Tamam'ı** tıklatın.

   ![Temel bilgiler sayfası](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)
5. **Ayarlar** sayfasında, Sanal **ağ ağ ağ geçidini** seçin ve **yetki lendirmeyi kullan onay** kutusunu işaretleyin.
6. Yetkilendirme **anahtarını** ve **Eş devresi URI'yi** girin ve bağlantıya bir ad verin. **Tamam**'a tıklayın. **Peer Circuit URI,** ExpressRoute devresinin Kaynak Kimliği'dir (ExpressRoute Circuit'in Özellikleri Ayarı bölmesinin altında bulabilirsiniz).

   ![Ayarlar sayfası](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)
7. **Özet** sayfasındaki bilgileri gözden geçirin ve **Tamam'ı**tıklatın.

**Bağlantı yetkilendirmesi serbest bırakmak için**

ExpressRoute devresini sanal ağa bağlayan bağlantıyı silerek yetkilendirme bırakabilirsiniz.

## <a name="delete-a-connection-to-unlink-a-vnet"></a>VNet bağlantısını künye yle bağlantıyı silme

Bağlantınızın sayfadaki **Sil** simgesini seçerek bağlantıyı silebilir ve VNet'inizi ExpressRoute devresine bağlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
