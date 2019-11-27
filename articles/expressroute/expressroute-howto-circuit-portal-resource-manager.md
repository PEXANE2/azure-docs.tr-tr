---
title: Öğretici-ExpressRoute ile devre oluşturma ve değiştirme
description: Bu öğreticide, bir ExpressRoute bağlantı hattı oluşturma, sağlama, doğrulama, güncelleştirme, silme ve sağlamayı kaldırma hakkında bilgi edinin.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: tutorial
ms.date: 10/20/2018
ms.author: cherylmc
ms.openlocfilehash: 7327031a7cd05674e9823f21601aab34c859f540
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74423571"
---
# <a name="tutorial-create-and-modify-an-expressroute-circuit"></a>Öğretici: ExpressRoute bağlantı hattı oluşturma ve değiştirme

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makalede Azure portalı ve Azure Resource Manager dağıtım modeli kullanarak ExpressRoute devresi oluşturmanıza yardımcı olur. Ayrıca durumu denetleme, güncelleştirme silin veya bir bağlantı hattının sağlamasını Kaldır.

## <a name="before-you-begin"></a>Başlamadan önce

* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.
* [Azure Portal](https://portal.azure.com)erişiminizin olduğundan emin olun.
* Yeni ağ kaynakları oluşturma izni olduğundan emin olun. Doğru izinlere sahip değilsiniz, hesap yöneticinize başvurun.
* Adımları daha iyi anlamak için başlamadan önce [bir video görüntüleyebilirsiniz](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) .

## <a name="create"></a>ExpressRoute bağlantı hattı oluşturma ve sağlama

### <a name="1-sign-in-to-the-azure-portal"></a>1. Azure portal oturum açın

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

### <a name="2-create-a-new-expressroute-circuit"></a>2. yeni bir ExpressRoute devresi oluşturun

> [!IMPORTANT]
> ExpressRoute bağlantı hattı, bir hizmet anahtarı verildiğinde andan itibaren faturalandırılır. Bağlantı sağlayıcısı devreyi sağlamak hazır olduğunda bu işlem bir şekilde gerçekleştirdiğinizden emin olun.

1. Yeni bir kaynak oluşturma seçeneğini belirleyerek bir ExpressRoute bağlantı hattı oluşturabilirsiniz. Aşağıdaki görüntüde gösterildiği gibi > **ağ** > **ExpressRoute** **kaynak oluştur ' a** tıklayın:

   ![ExpressRoute bağlantı hattı oluşturma](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. **ExpressRoute**' a tıkladıktan sonra **ExpressRoute** bağlantı hattı oluştur sayfasını görürsünüz. Bu sayfadaki değerleri doldurmayı, faturalandırma modeli (sınırsız veya ölçülen) kullanım ölçümü verilerini ve doğru SKU katmanını (standart veya Premium) belirtmeniz emin olun.

   ![SKU katmanı ve ölçüm verilerini yapılandırma](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit.png)

   * **Katman** , bir ExpressRoute standardı veya ExpressRoute Premium eklentisinin etkinleştirilip etkinleştirilmediğini belirler. Premium eklenti için standart SKU veya **Premium** almak üzere **Standart** belirtebilirsiniz.
   * **Veri ölçümü** Faturalandırma türünü belirler. Tarifeli bir veri planı için **tarifeli** ve sınırsız bir veri planı için **sınırsız** bir şekilde belirtebilirsiniz. Faturalandırma türünü **tarifeli** iken **sınırsız**olarak değiştirebileceğinizi unutmayın.

     > [!IMPORTANT]
     > Türü **sınırsız** iken **tarifeli**olarak değiştiremezsiniz.

   * **Eşleme konumu** , Microsoft ile eşolduğunuz fiziksel konumdur.

     > [!IMPORTANT]
     > Eşleme konumu, Microsoft ile eşolduğunuz [fiziksel konumu](expressroute-locations.md) gösterir. Bu, Azure ağ kaynak sağlayıcısı 'nın bulunduğu coğrafi konuma başvuran "location" özelliği ile bağlantılı **değildir** . Bunlar ilişkili değildir, ancak bir ağ kaynağı sağlayıcı eşleme konumu bağlantı hattının coğrafi olarak yakın seçmek için iyi bir uygulamadır.

### <a name="3-view-the-circuits-and-properties"></a>3. devreleri ve özellikleri görüntüleyin

**Tüm devreleri görüntüleme**

Sol taraftaki menüden **tüm kaynaklar** ' a seçerek oluşturduğunuz tüm devreleri görüntüleyebilirsiniz.

![Görünüm devreler](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**Özellikleri görüntüle**

Bağlantı hattı özelliklerini seçerek görüntüleyebilirsiniz. Devreniz için **genel bakış** sayfasında hizmet anahtarı, hizmet anahtarı alanında görüntülenir. Bağlantı hattınız için hizmet anahtarı kopyalayın ve sağlama işlemini tamamlamak için hizmet sağlayıcısı aşağı geçmesi gerekir. Bağlantı hattı hizmet anahtarını bağlantı hattınız için özeldir.

![Özellikleri görüntüle](./media/expressroute-howto-circuit-portal-resource-manager/servicekey1.png)

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>4. sağlama için hizmet anahtarını bağlantı sağlayıcınıza gönderin

Bu sayfada, **sağlayıcı durumu** hizmet sağlayıcı tarafında sağlama durumunun geçerli durumu hakkında bilgi sağlar. **Devre durumu** , Microsoft tarafında durumu sağlar. Devre sağlama durumları hakkında daha fazla bilgi için bkz. [Iş akışları](expressroute-workflows.md#expressroute-circuit-provisioning-states) makalesi.

Yeni bir ExpressRoute bağlantı hattı'ı oluşturduğunuzda, bağlantı hattı şu durumda olur:

Sağlayıcı Durumu: sağlanmadı<BR>
Bağlantı hattı durumu: etkin

![Sağlama işlemini başlatın](./media/expressroute-howto-circuit-portal-resource-manager/status.png)

Bağlantı sağlayıcısı, etkinleştirmeden sürecinde olduğunda bağlantı hattının aşağıdaki duruma değiştirir:

Sağlayıcı Durumu: sağlama<BR>
Bağlantı hattı durumu: etkin

Bir ExpressRoute bağlantı hattı kullanabilmek için şu durumda olmalıdır:

Sağlayıcı Durumu: sağlanan<BR>
Bağlantı hattı durumu: etkin

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>5. devre anahtarının durumunu ve durumunu düzenli olarak kontrol edin

Siz de seçerek ilginizi çeken bağlantı hattı özelliklerini görüntüleyebilirsiniz. Devam etmeden önce **sağlayıcının durumunu** kontrol edin ve **sağlanan** ' a taşındığını doğrulayın.

![Bağlantı hattı ve sağlayıcısı durumu](./media/expressroute-howto-circuit-portal-resource-manager/provisioned.png)

### <a name="6-create-your-routing-configuration"></a>6. yönlendirme yapılandırmanızı oluşturun

Adım adım yönergeler için, devre eşlemeleri oluşturmak ve değiştirmek üzere [ExpressRoute devrouting yapılandırma](expressroute-howto-routing-portal-resource-manager.md) makalesine başvurun.

> [!IMPORTANT]
> Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen sunan bir hizmet sağlayıcısı kullanıyorsanız, Katman 3 Hizmetleri (genellikle bir IP VPN, MPLS gibi), bağlantı sağlayıcınız yapılandırır ve yönlendirmeyi sizin için yönetir.

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a>7. bir ExpressRoute devresine sanal ağ bağlama

Ardından, bir sanal ağ, ExpressRoute bağlantı hattına bağlayın. Kaynak Yöneticisi dağıtım modeliyle çalışırken [sanal ağları ExpressRoute devrelerine bağlama](expressroute-howto-linkvnet-arm.md) makalesini kullanın.

## <a name="status"></a>ExpressRoute devresinin durumunu alma

Seçip genel bakış sayfası görüntüleme bir bağlantı hattının durumunu görüntüleyebilirsiniz.

## <a name="modify"></a>ExpressRoute devresini değiştirme

Belirli bir ExpressRoute bağlantı hattı özelliklerini bağlantıyı etkilemeden değiştirebilirsiniz. Bant genişliği, SKU, faturalandırma modelini değiştirebilir ve **yapılandırma** sayfasında klasik işlemlere izin verebilirsiniz. Sınırlamalar ve sınırlamalar hakkında daha fazla bilgi için bkz. [ExpressRoute SSS](expressroute-faqs.md).

Kapalı kalma süresi olmadan aşağıdaki görevleri gerçekleştirebilirsiniz:

* Etkinleştirmek veya ExpressRoute bağlantı hattı için bir ExpressRoute Premium eklentisi devre dışı bırakın.
* Bant genişliği var. sağlanan ExpressRoute devreniz bağlantı noktasında kapasite artıştır.

  > [!IMPORTANT]
  > Bağlantı hattı bant önceki sürüme indirme desteklenmiyor.

* Ölçüm planını *ölçülen verilerden* *sınırsız veriye*değiştirin.

  > [!IMPORTANT]
  > Ölçüm plan sınırsız verilerden ölçülen veri değiştirme desteklenmiyor.

* *Klasik Işlemlere Izin ver*' i etkinleştirebilir ve devre dışı bırakabilirsiniz.
  > [!IMPORTANT]
  > ExpressRoute bağlantı hattı mevcut bağlantı noktası üzerinde yetersiz kapasite ise yeniden oluşturmanız gerekebilir. Yoksa hiçbir ek kapasite kullanılabilir o konumda devre yükseltemezsiniz.
  >
  > Bant genişliği sorunsuzca yükseltme yapabilirsiniz ancak kesintiye uğratmadan ExpressRoute bağlantı hattının bant genişliğini indiremezsiniz. Bant genişliği eski sürüme düşürme, ExpressRoute bağlantı hattının sağlamasını kaldırma ve ardından yeni ExpressRoute bağlantı hattı yeniden sağlamak istiyor.
  >
  > Standart bağlantı hattı için izin daha büyük olan kaynaklar kullanıyorsanız, Premium eklenti işlemi devre dışı bırakma başarısız olabilir.

Bir ExpressRoute devresini değiştirmek için **yapılandırma**' ya tıklayın.

![Bağlantı hattını değiştirme](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

## <a name="delete"></a>ExpressRoute devresini sağlamayı ve silmeyi kaldırma

ExpressRoute devrenizi **Sil** simgesini seçerek silebilirsiniz. Aşağıdaki bilgileri not edin:

* ExpressRoute bağlantı hattınızdaki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, herhangi bir sanal ağa bağlantı hattına bağlı olup olmadığını denetleyin.
* ExpressRoute bağlantı hattı hizmeti sağlayıcısı sağlama **durumu sağlandıysa veya** sağlanmışsa , devre dışı bırakmak için hizmet sağlayıcınızla birlikte çalışmanız gerekir. Kaynak ayırmanıza ve hizmeti sağlayıcısı devreyi sağlamayı kaldırma tamamlandıktan ve bize bildiren kadar faturalandırılırsınız devam ediyoruz.
* Hizmet sağlayıcı devre sağlamasını kaldırmışsa (hizmet sağlayıcı sağlama durumu **sağlanmadı**olarak ayarlandıysa), devresini silebilirsiniz. Bu durumda bağlantı hattının faturalandırılması durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantı hattınızı oluşturduktan sonra sonraki adımlara devam edin:

* [ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md)
* [Sanal ağınızı ExpressRoute devrenizi ile bağlama](expressroute-howto-linkvnet-arm.md)
