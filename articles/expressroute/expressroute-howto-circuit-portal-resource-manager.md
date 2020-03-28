---
title: Öğretici - ExpressRoute ile bir devre oluşturun ve değiştirin
description: Bu öğreticide, expressroute devresi oluşturma, sağlama, doğrulama, güncelleme, silme ve silme hakkında bilgi edinin.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: tutorial
ms.date: 10/20/2018
ms.author: cherylmc
ms.openlocfilehash: 686ac8013879eff8adc4476d56119bbb4a169900
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74813120"
---
# <a name="tutorial-create-and-modify-an-expressroute-circuit"></a>Öğretici: ExpressRoute devresi oluşturun ve değiştirin

> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-circuit-portal-resource-manager.md)
> * [Powershell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makale, Azure portalı ve Azure Kaynak Yöneticisi dağıtım modelini kullanarak bir ExpressRoute devresi oluşturmanıza yardımcı olur. Ayrıca durumu denetleyebilir, güncelleyebilir, silebilir veya bir devreyi devreyi de devreyi devreyi devreyi yok edebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

* Yapılandırmaya başlamadan önce [ön koşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.
* [Azure portalına](https://portal.azure.com)erişebildiğinizden emin olun.
* Yeni ağ kaynakları oluşturmak için izinlere sahip olduğundan emin olun. Doğru izinlere sahip değilseniz hesap yöneticinize başvurun.
* Adımları daha iyi anlamak için videoyu başlamadan önce [görüntüleyebilirsiniz.](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)

## <a name="create-and-provision-an-expressroute-circuit"></a><a name="create"></a>ExpressRoute devresi oluşturma ve sağlama

### <a name="1-sign-in-to-the-azure-portal"></a>1. Azure portalında oturum açın

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

### <a name="2-create-a-new-expressroute-circuit"></a>2. Yeni bir ExpressRoute devresi oluşturun

> [!IMPORTANT]
> ExpressRoute devreniz, bir servis anahtarı verildiği andan itibaren faturalandırılır. Bağlantı sağlayıcısı devreyi sağlamaya hazır olduğunda bu işlemi gerçekleştirdiğinizi sağlayın.

Yeni bir kaynak oluşturma seçeneğini seçerek bir ExpressRoute devresi oluşturabilirsiniz. 

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin. Aşağıdaki resimde gösterildiği gibi **Ağ** > **ExpressRoute'u**seçin:

   ![ExpressRoute bağlantı hattı oluşturma](./media/expressroute-howto-circuit-portal-resource-manager/create-an-expressroute-circuit.png)

2. **ExpressRoute'u**tıklattıktan sonra **ExpressRoute oluştur devre** sayfasını görürsünüz. Bu sayfadaki değerleri doldururken, doğru SKU katmanını (Standart veya Premium) ve veri ölçüm faturalandırma modelini (Sınırsız veya Ölçülü) belirttiğinden emin olun.

   ![SKU katmanını ve veri ölçümlerini yapılandırın](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit.png)

   * **Katman,** ExpressRoute standardının mı yoksa ExpressRoute premium eklentisi mi etkinleştirildiğini belirler. Premium eklenti için standart SKU veya **Premium'u** almak için **Standart'ı** belirtebilirsiniz.
   * **Veri ölçme** fatura türünü belirler. Tarifeli veri planı için **Ölçülü** ve sınırsız veri planı için **Sınırsız** belirtebilirsiniz. Fatura türünü **Tarifeli'den** **Sınırsız'a**değiştirebileceğinizi unutmayın.

     > [!IMPORTANT]
     > **Türü Sınırsız'dan** **Tarifeli'ye**değiştiremezsiniz.

   * **Eşleyen Konum,** Microsoft ile birlikte bakacağınız fiziksel konumdur.

     > [!IMPORTANT]
     > Eşleyen Konum, Microsoft ile birlikte bakmakta olduğunuz [fiziksel konumu](expressroute-locations.md) gösterir. Bu, Azure Ağ Kaynak Sağlayıcısı'nın bulunduğu coğrafyayı ifade eden "Konum" özelliğiyle bağlantılı **değildir.** Bunlar ilişkili olmasa da, coğrafi olarak devrenin Eşleyen Konumu'na yakın bir Ağ Kaynak Sağlayıcısı seçmek iyi bir uygulamadır.

### <a name="3-view-the-circuits-and-properties"></a>3. Devreleri ve özellikleri görüntüleyin

**Tüm devreleri görüntüleyin**

Sol taraftaki menüdeki **Tüm kaynakları** seçerek oluşturduğunuz tüm devreleri görüntüleyebilirsiniz.

![Görünüm devreleri](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**Özellikleri görüntüleyin**

Devrenin özelliklerini seçerek görüntüleyebilirsiniz. Devreniz için **Genel Bakış** sayfasında servis anahtarı servis anahtarı alanında görünür. Devreniz için servis anahtarını kopyalamanız ve sağlama işlemini tamamlamak için servis sağlayıcısına aktarmanız gerekir. Devre servis anahtarı devrenize özgüdür.

![Özellikleri görüntüle](./media/expressroute-howto-circuit-portal-resource-manager/servicekey1.png)

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>4. Hizmet anahtarını sağlama için bağlantı sağlayıcınıza gönderin

Bu sayfada, **Sağlayıcı durumu,** hizmet sağlayıcı tarafında geçerli sağlama durumu hakkında bilgi sağlar. **Devre durumu** Microsoft tarafında durumu sağlar. Devre sağlama durumları hakkında daha fazla bilgi için [İş Akışları](expressroute-workflows.md#expressroute-circuit-provisioning-states) makalesine bakın.

Yeni bir ExpressRoute devresi oluşturduğunuzda, devre aşağıdaki durumdadır:

Sağlayıcı durumu: Sağlanmadı<BR>
Devre durumu: Etkin

![Sağlama sürecini başlatma](./media/expressroute-howto-circuit-portal-resource-manager/status.png)

Bağlantı sağlayıcısı sizin için etkinleştirme sürecindeyken devre aşağıdaki duruma değişir:

Sağlayıcı durumu: Sağlama<BR>
Devre durumu: Etkin

Bir ExpressRoute devresini kullanabilmek için aşağıdaki durumda olmalıdır:

Sağlayıcı durumu: Sağlanan<BR>
Devre durumu: Etkin

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>5. Devre anahtarının durumunu ve durumunu periyodik olarak kontrol edin

İlgilendiğiniz devrenin özelliklerini seçerek görüntüleyebilirsiniz. Sağlayıcı **durumunu** denetleyin ve devam etmeden önce **Hükmün'e** taşındığından emin olun.

![Devre ve sağlayıcı durumu](./media/expressroute-howto-circuit-portal-resource-manager/provisioned.png)

### <a name="6-create-your-routing-configuration"></a>6. Yönlendirme yapılandırmanızı oluşturun

Adım adım talimatlar için, devre eşlemeleri oluşturmak ve değiştirmek için [ExpressRoute devre yönlendirme yapılandırma](expressroute-howto-routing-portal-resource-manager.md) makalesine bakın.

> [!IMPORTANT]
> Bu yönergeler yalnızca katman 2 bağlantı hizmetleri sunan servis sağlayıcılarla oluşturulan devreler için geçerlidir. Yönetilen katman 3 hizmetleri (genellikle MPLS gibi bir IP VPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi sizin için yapılandırır ve yönetir.

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a>7. Bir sanal ağı ExpressRoute devresine bağla

Ardından, bir sanal ağı ExpressRoute devrenize bağla. Kaynak Yöneticisi dağıtım modeliyle çalışırken [ExpressRoute devreleri makalesine bağlama sanal ağları](expressroute-howto-linkvnet-arm.md) kullanın.

## <a name="getting-the-status-of-an-expressroute-circuit"></a><a name="status"></a>ExpressRoute devresinin durumunu alma

Bir devrenin durumunu seçerek ve Genel Bakış sayfasını görüntüleyerek görüntüleyebilirsiniz.

## <a name="modifying-an-expressroute-circuit"></a><a name="modify"></a>ExpressRoute devresini değiştirme

Bir ExpressRoute devresinin belirli özelliklerini bağlantı etkilemeden değiştirebilirsiniz. Bant genişliğini, SKU'yu, faturalandırma modelini değiştirebilir ve **Yapılandırma** sayfasında klasik işlemlere izin verebilirsiniz. Sınırlar ve sınırlamalar hakkında bilgi için [ExpressRoute SSS](expressroute-faqs.md)bölümüne bakın.

Aşağıdaki görevleri kapalı kalma süresi olmadan gerçekleştirebilirsiniz:

* ExpressRoute devreniz için ExpressRoute Premium eklentisini etkinleştirin veya devre dışı edin.
* Bağlantı noktasında kapasite olması koşuluyla ExpressRoute devrenizin bant genişliğini artırın.

  > [!IMPORTANT]
  > Bir devrenin bant genişliğini düşürme desteklenmez.

* Ölçüm planını *Tarifeli Verilerden* *Sınırsız Veriye*değiştirin.

  > [!IMPORTANT]
  > Ölçüm planının Sınırsız Verilerden Ölçülü Verilere değiştirilmesi desteklenmez.

* *Klasik İşlemler'e İzin Ver'i*etkinleştirebilir ve devre dışı kullanabilirsiniz.
  > [!IMPORTANT]
  > Varolan bağlantı noktasında yetersiz kapasite varsa ExpressRoute devresini yeniden oluşturmanız gerekebilir. O konumda ek kapasite yoksa devreyi yükseltemezsiniz.
  >
  > Bant genişliğini sorunsuz bir şekilde yükseltebiliyor sanız da, expressroute devresinin bant genişliğini kesintiye uğramadan azaltamazsınız. Bant genişliğini düşürmek, ExpressRoute devresini kaldırmanızı ve ardından yeni bir ExpressRoute devresini yeniden sağlamanızı gerektirir.
  >
  > Standart devre için izin verilenden daha büyük kaynaklar kullanıyorsanız, Premium eklenti işlemini devre dışı bırakmak başarısız olabilir.

ExpressRoute devresini değiştirmek için **Yapılandırma'yı**tıklatın.

![Devreyi değiştir](./media/expressroute-howto-circuit-portal-resource-manager/modify-circuit-configuration.png)

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><a name="delete"></a>Bir ExpressRoute bağlantı hattının sağlamasını kaldırma ve silme

**Delete** simgesini seçerek ExpressRoute devrenizi silebilirsiniz. Aşağıdaki bilgileri not edin:

* ExpressRoute bağlantı hattınızdaki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, herhangi bir sanal ağların devreye bağlı olup olmadığını kontrol edin.
* ExpressRoute devre servis sağlayıcısı sağlama durumu **Hükmünde** veya **Provisioned** ise, devreyi kendi taraflarında devreyi devreyi devreyi devreden çıkarmak için servis sağlayıcınızla birlikte çalışmanız gerekir. Hizmet sağlayıcı devreyi devreyi devreyi tamamlayıp bizi bize iletene kadar kaynak ayırmaya ve size fatura landırmaya devam ediyoruz.
* Hizmet sağlayıcısı devreyi devreyi devreyi devreyi iptal ettiyse (servis sağlayıcı sağlama durumu **değil**olarak ayarlanır), devreyi silebilirsiniz. Bu durumda bağlantı hattının faturalandırılması durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Devrenizi oluşturduktan sonra aşağıdaki sonraki adımlarla devam edin:

* [ExpressRoute devreniz için yönlendirme oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md)
* [Sanal ağınızı ExpressRoute devrenize bağla](expressroute-howto-linkvnet-arm.md)
