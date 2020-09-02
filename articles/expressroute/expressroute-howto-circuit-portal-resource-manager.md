---
title: Öğretici-ExpressRoute ile devre oluşturma ve değiştirme
description: Bu öğreticide, bir ExpressRoute bağlantı hattı oluşturma, sağlama, doğrulama, güncelleştirme, silme ve sağlamayı kaldırma hakkında bilgi edineceksiniz.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: tutorial
ms.date: 09/01/2020
ms.author: duau
ms.openlocfilehash: 58c35b094d21dc562e61b4819c0d8e063908392d
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89322150"
---
# <a name="tutorial-create-and-modify-an-expressroute-circuit"></a>Öğretici: ExpressRoute bağlantı hattı oluşturma ve değiştirme

> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu öğreticide, Azure portal ve Azure Resource Manager dağıtım modelini kullanarak bir ExpressRoute bağlantı hattı oluşturma gösterilmektedir. Ayrıca, durumu denetleyebilir, güncelleştirebilir, silebilir veya devre sağlamayı kaldırabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * ExpressRoute bağlantı hattı oluşturma
> * Bir devrenin geçerli durumunu al
> * Devreyi değiştirme
> * Devre sağlamasını kaldırma ve silme

## <a name="before-you-begin"></a>Başlamadan önce

* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.
* [Azure Portal](https://portal.azure.com)erişiminizin olduğundan emin olun.
* Yeni ağ kaynakları oluşturmak için izinleriniz olduğundan emin olun. Doğru izinleriniz yoksa, hesap yöneticinize başvurun.
* Adımları daha iyi anlamak için başlamadan önce [bir video görüntüleyebilirsiniz](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) .

## <a name="create-and-provision-an-expressroute-circuit"></a><a name="create"></a>ExpressRoute bağlantı hattı oluşturma ve sağlama

### <a name="1-sign-in-to-the-azure-portal"></a>1. Azure portal oturum açın

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

### <a name="2-create-a-new-expressroute-circuit"></a>2. yeni bir ExpressRoute devresi oluşturun

> [!IMPORTANT]
> ExpressRoute bağlantı hattı, hizmet anahtarının verildiği andan itibaren faturalandırılır. Bağlantı sağlayıcısı devre sağlamaya hazırsa bu işlemi gerçekleştirdiğinizden emin olun.

Yeni kaynak oluşturma seçeneğini belirleyerek bir ExpressRoute bağlantı hattı oluşturabilirsiniz. 

1. Azure portalı menüsünde **Kaynak oluştur**'u seçin. **Networking**  >  Aşağıdaki görüntüde gösterildiği gibi ağ**ExpressRoute**' u seçin:

    :::image type="content" source="./media/expressroute-howto-circuit-portal-resource-manager/create-expressroute-circuit-menu.png" alt-text="ExpressRoute bağlantı hattı oluşturma":::

2. **ExpressRoute**' a tıkladıktan sonra **ExpressRoute oluştur** sayfasını görürsünüz. Devrenin **kaynak grubunu**, **bölgesini**ve  **adını** belirtin. Ileri ' ye tıklayın **: yapılandırma >**.

    :::image type="content" source="./media/expressroute-howto-circuit-portal-resource-manager/expressroute-create-basic.png" alt-text="Kaynak grubunu ve bölgeyi yapılandırma":::

3. Bu sayfadaki değerleri doldururken, doğru SKU katmanını (yerel, standart veya Premium) ve veri ölçümü faturalandırma modelini (sınırsız veya tarifeli) belirttiğinizden emin olun.

    :::image type="content" source="./media/expressroute-howto-circuit-portal-resource-manager/expressroute-create-configuration.png" alt-text="Devresini yapılandırma":::
    
    * **Bağlantı noktası türü** , bir hizmet sağlayıcısına bağlanıp bağlanamayacağını veya doğrudan Microsoft 'un genel ağına bir eşleme konumu olduğunu belirler.
    * Yeni bir devrenin oluşturulup oluşturulamayacağını veya klasik bir bağlantı hattını ARM 'ye geçiriyorsanız yeni bir bağlantı hattı **oluşturma veya klasik** bir bağlantı hattı.
    * **Sağlayıcı** , hizmetinizi istediğiniz Internet hizmetidir.
    * **Eşleme konumu** , Microsoft ile eşolduğunuz fiziksel konumdur.

    > [!IMPORTANT]
    > Eşleme konumu, Microsoft ile eşolduğunuz [fiziksel konumu](expressroute-locations.md) gösterir. Bu, Azure ağ kaynak sağlayıcısı 'nın bulunduğu coğrafi konuma başvuran "location" özelliği ile bağlantılı **değildir** . İlgili olmadıkları sürece, bir ağ kaynağı sağlayıcısını coğrafi olarak devrenin eşleme konumuna yakın bir şekilde tercih eden bir uygulamadır.

    * **SKU** , bir ExpressRoute yerel, ExpressRoute Standard veya ExpressRoute Premium eklentisinin etkinleştirilip etkinleştirilmediğini belirler. Premium eklenti için standart SKU veya **Premium** 'u almak üzere yerel SKU, **Standart** almak için **Yerel** ' i belirtebilirsiniz.
    * **Faturalandırma modeli** Faturalandırma türünü belirler. Tarifeli bir veri planı için **tarifeli** ve sınırsız bir veri planı için **sınırsız** bir şekilde belirtebilirsiniz. Faturalandırma türünü **tarifeli** iken **sınırsız**olarak değiştirebileceğinizi unutmayın.

    > [!IMPORTANT]
    > Türü **sınırsız** iken **tarifeli**olarak değiştiremezsiniz.

    * **Klasik Işleme Izin ver** , klasik sanal ağların devreye bağlanmasına izin verir.

### <a name="3-view-the-circuits-and-properties"></a>3. devreleri ve özellikleri görüntüleyin

**Tüm devreleri görüntüleme**

Sol taraftaki menüdeki **tüm hizmetler > ağ > ExpressRoute devreleri** ' nı seçerek oluşturduğunuz tüm devreleri görüntüleyebilirsiniz.

:::image type="content" source="./media/expressroute-howto-circuit-portal-resource-manager/expressroute-circuit-menu.png" alt-text="ExpressRoute devre menüsü":::

Abonelikte oluşturulan tüm ExpressRoute devreleri burada görünür.

:::image type="content" source="./media/expressroute-howto-circuit-portal-resource-manager/expressroute-circuit-list.png" alt-text="ExpressRoute devre listesi":::

**Özellikleri görüntüle**

Devre dışı seçeneğinin özelliklerini seçerek görüntüleyebilirsiniz. Devreniz için **genel bakış** sayfasında hizmet anahtarı, hizmet anahtarı alanında görüntülenir. Devrenizin için hizmet anahtarına başvurun ve sağlama işlemini tamamlaması için hizmet sağlayıcısına sağlayın. Hizmet anahtarı, devrenizin için özeldir.

:::image type="content" source="./media/expressroute-howto-circuit-portal-resource-manager/expressroute-circuit-overview.png" alt-text="Özellikleri görüntüle":::

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>4. sağlama için hizmet anahtarını bağlantı sağlayıcınıza gönderin

Bu sayfada, **sağlayıcı durumu** hizmet sağlayıcı tarafında sağlama durumunun geçerli durumu hakkında bilgi sağlar. **Devre durumu** , Microsoft tarafında durumu sağlar. Devre sağlama durumları hakkında daha fazla bilgi için bkz. [Iş akışları](expressroute-workflows.md#expressroute-circuit-provisioning-states) makalesi.

Yeni bir ExpressRoute bağlantı hattı oluşturduğunuzda, devre aşağıdaki durumda olur:

Sağlayıcı durumu: **sağlanmadı**<BR>
Devre durumu: **etkin**

:::image type="content" source="./media/expressroute-howto-circuit-portal-resource-manager/expressroute-circuit-overview-provisioning-state.png" alt-text="Sağlama işlemini Başlat":::

Bağlantı sağlayıcısı sizin için etkinleştirme işleminde olduğunda devre aşağıdaki duruma geçer:

Sağlayıcı durumu: **sağlama**<BR>
Devre durumu: **etkin**

ExpressRoute bağlantı hattını kullanabilmeniz için aşağıdaki durumda olmalıdır:

Sağlayıcı durumu: **sağlandı**<BR>
Devre durumu: **etkin**

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>5. devre anahtarının durumunu ve durumunu düzenli olarak kontrol edin

' İ seçerek ilgilendiğiniz devrenin özelliklerini görüntüleyebilirsiniz. Devam etmeden önce **sağlayıcının durumunu** kontrol edin ve **sağlanan** ' a taşındığını doğrulayın.

:::image type="content" source="./media/expressroute-howto-circuit-portal-resource-manager/provisioned.png" alt-text="Devre ve sağlayıcı durumu":::

### <a name="6-create-your-routing-configuration"></a>6. yönlendirme yapılandırmanızı oluşturun

Adım adım yönergeler için, devre eşlemeleri oluşturmak ve değiştirmek üzere [ExpressRoute devrouting yapılandırma](expressroute-howto-routing-portal-resource-manager.md) makalesine başvurun.

> [!IMPORTANT]
> Bu yönergeler yalnızca, katman 2 bağlantı hizmetleri sunan hizmet sağlayıcılarıyla oluşturulan devrelere uygulanır. Yönetilen katman 3 Hizmetleri (genellikle MPLS gibi bir IP VPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi yapılandırır ve yönetir.

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a>7. bir ExpressRoute devresine sanal ağ bağlama

Ardından, bir sanal ağı ExpressRoute bağlantı hattına bağlayın. Kaynak Yöneticisi dağıtım modeliyle çalışırken [sanal ağları ExpressRoute devrelerine bağlama](expressroute-howto-linkvnet-arm.md) makalesini kullanın.

## <a name="getting-the-status-of-an-expressroute-circuit"></a><a name="status"></a>ExpressRoute devresinin durumunu alma

Bir bağlantı hattını seçip genel bakış sayfasını görüntüleyerek bir devrenin durumunu görüntüleyebilirsiniz.

## <a name="modifying-an-expressroute-circuit"></a><a name="modify"></a>ExpressRoute devresini değiştirme

Bağlantıyı etkilemeden bir ExpressRoute devresine ait belirli özellikleri değiştirebilirsiniz. Bant genişliği, SKU, faturalandırma modelini değiştirebilir ve **yapılandırma** sayfasında klasik işlemlere izin verebilirsiniz. Sınırlamalar ve sınırlamalar hakkında daha fazla bilgi için bkz. [ExpressRoute SSS](expressroute-faqs.md).

Kapalı kalma süresi olmadan aşağıdaki görevleri gerçekleştirebilirsiniz:

* ExpressRoute devreniz için bir ExpressRoute Premium eklentisini etkinleştirin veya devre dışı bırakın.
* Bağlantı noktasında kullanılabilir kapasite olması şartıyla ExpressRoute devrenizi için bant genişliğini artırın.

  > [!IMPORTANT]
  > Bağlantı hattının devre dışı olması desteklenmiyor.

* Ölçüm planını *ölçülen verilerden* *sınırsız veriye*değiştirin.

  > [!IMPORTANT]
  > Ölçüm planının **sınırsız verilerden** **ölçülen verilerle** değiştirilmesi desteklenmez.

* *Klasik Işlemlere Izin ver*' i etkinleştirebilir ve devre dışı bırakabilirsiniz.
  > [!IMPORTANT]
  > Mevcut bağlantı noktasında yetersiz kapasite varsa ExpressRoute devresini yeniden oluşturmanız gerekebilir. Bu konumda kullanılabilir ek kapasite yoksa, devreyi yükseltemezsiniz.
  >
  > Bant genişliğini sorunsuzca yükseltebilseniz de, kesintiye uğramadan bir ExpressRoute devresine ait bant genişliğini azaltamaz. Bant genişliğini düşürme işlemi, ExpressRoute bağlantı hattının sağlanması ve ardından yeni bir ExpressRoute devresine yeniden sağlanması gerektirir.
  >
  > Standart devre için izin verilen miktardan daha büyük kaynaklar kullanıyorsanız Premium eklenti işleminin devre dışı bırakılması başarısız olabilir.

Bir ExpressRoute devresini değiştirmek için **yapılandırma**' ya tıklayın.

:::image type="content" source="./media/expressroute-howto-circuit-portal-resource-manager/expressroute-circuit-configuration.png" alt-text="Devre değiştirme":::

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><a name="delete"></a>Bir ExpressRoute bağlantı hattının sağlamasını kaldırma ve silme

ExpressRoute bağlantı hattı hizmeti sağlayıcısı sağlama **durumu sağlandıysa veya** sağlanmışsa **Provisioned** , devre dışı bırakmak için hizmet sağlayıcınızla birlikte çalışmanız gerekir. Hizmet sağlayıcısı, devre sağlamasını kaldırmayı ve bize haber verene kadar, kaynakları ayırmaya ve sizi faturalandırmaya devam ediyoruz.

> [!NOTE]
>* Sağlamayı kaldırmadan önce *tüm sanal ağların* ExpressRoute bağlantı hattından bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, herhangi bir sanal ağın bağlantı hattına bağlanıp bağlanmadığını denetleyin.
>* Hizmet sağlayıcı devre sağlamasını kaldırmışsa (hizmet sağlayıcı sağlama durumu **sağlanmadı**olarak ayarlandıysa), devresini silebilirsiniz. Bu durumda bağlantı hattının faturalandırılması durdurulur.

ExpressRoute devrenizi **Sil** simgesini seçerek silebilirsiniz. 

:::image type="content" source="./media/expressroute-howto-circuit-portal-resource-manager/expressroute-circuit-delete.png" alt-text="Devresini Sil":::

## <a name="next-steps"></a>Sonraki adımlar

Devrenizi oluşturduktan sonra aşağıdaki adımlarla devam edin:

* [ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md)
* [Sanal ağınızı ExpressRoute devrenizi ile bağlama](expressroute-howto-linkvnet-arm.md)