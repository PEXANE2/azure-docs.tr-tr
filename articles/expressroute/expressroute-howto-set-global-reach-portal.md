---
title: 'Azure ExpressRoute: Azure portal kullanarak Global Reach yapılandırma'
description: Bu makale, ExpressRoute bağlantı devresine, şirket içi ağlarınızla özel bir ağ oluşturmak ve Azure portal kullanarak Global Reach etkinleştirmek için bir araya yönlendirmenize yardımcı olur.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 03/05/2021
ms.author: duau
ms.openlocfilehash: 336bd4aaf881b7315921ef374c92a2ac95ff3c8c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102431364"
---
# <a name="configure-expressroute-global-reach-using-the-azure-portal"></a>Azure portal kullanarak ExpressRoute Global Reach yapılandırma

Bu makale, PowerShell kullanarak ExpressRoute Global Reach yapılandırmanıza yardımcı olur. Daha fazla bilgi için bkz. [ExpressRouteRoute Global Reach](expressroute-global-reach.md).

 ## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki ölçütleri onaylayın:

* ExpressRoute devre sağlama [iş akışlarını](expressroute-workflows.md)anlamış olursunuz.
* ExpressRoute devrelerinizi sağlanan bir durumda.
* Azure özel eşlemesi, ExpressRoute devrelerinizi yapılandırılmış.
* PowerShell 'i yerel olarak çalıştırmak istiyorsanız, Azure PowerShell en son sürümünün bilgisayarınızda yüklü olduğunu doğrulayın.

## <a name="identify-circuits"></a>Devreleri tanımla

1. Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

2. Kullanmak istediğiniz ExpressRoute devreleri belirler. Desteklenen ülkelerde/bölgelerde bulundukları sürece, her iki ExpressRoute devresine ait özel eşleme arasında ExpressRoute Global Reach etkinleştirebilirsiniz. Devrelerin farklı eşleme konumlarında oluşturulması gerekir. 

   * Aboneliğiniz her iki devrede sahipse, aşağıdaki bölümlerde yapılandırmayı çalıştırmak için devre seçeneğini belirleyebilirsiniz.
   * İki devre farklı Azure aboneliklerdeyse, bir Azure aboneliğinden yetkilendirme yapmanız gerekir. Ardından, diğer Azure aboneliğinde yapılandırma komutunu çalıştırdığınızda yetkilendirme anahtarını geçirirsiniz.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-global-reach-list.png" alt-text="ExpressRoute devreleri listesinin ekran görüntüsü.":::

## <a name="enable-connectivity"></a>Bağlantıyı etkinleştir

Şirket içi ağlarınız arasında bağlantıyı etkinleştirin. Aynı Azure aboneliğinde ve farklı abonelikler içeren devrelere yönelik ayrı yönergeler kümesi vardır.

### <a name="expressroute-circuits-in-the-same-azure-subscription"></a>Aynı Azure aboneliğindeki ExpressRoute devreleri

1. **Azure özel** eşleme yapılandırmasını seçin. 

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-private-peering.png" alt-text="ExpressRoute Genel Bakış sayfasının ekran görüntüsü.":::

1. *Global Reach yapılandırma Ekle* sayfasını açmak Için **Global Reach Ekle** ' yi seçin.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/private-peering-enable-global-reach.png" alt-text="Özel eşlemeden küresel erişim sağlama":::

1. Global Reach yapılandırma *Ekle* sayfasında bu yapılandırmaya bir ad verin. Bu devreyi bağlamak istediğiniz *ExpressRoute* bağlantı hattını seçin ve *Global Reach alt ağı* Için bir **/29 IPv4** girin. İki ExpressRoute devreleri arasında bağlantı kurmak için bu alt ağdaki IP adreslerini kullanıyoruz. Bu alt ağdaki adresleri Azure sanal ağlarınızda veya şirket içi ağınızda kullanmayın. Devreyi özel eşleme yapılandırmasına eklemek için **Ekle** ' yi seçin.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/add-global-reach-configuration.png" alt-text="Özel eşlemeye Global Reach eklemenin ekran görüntüsü.":::

1. Global Reach yapılandırmasını gerçekleştirmek için **Kaydet** ' i seçin. İşlem tamamlandığında, hem ExpressRoute devreleri aracılığıyla iki şirket içi ağınız arasında bağlantınız olur.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/save-private-peering-configuration.png" alt-text="Özel eşleme yapılandırmalarının kaydedilmesinin ekran görüntüsü.":::

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Farklı Azure aboneliklerinde ExpressRoute devreleri

İki devrede aynı Azure aboneliğinde yoksa, yetkilendirme yapmanız gerekir. Aşağıdaki yapılandırmada, yetkilendirme, dev2 aboneliğinden oluşturulmuştur. Yetkilendirme anahtarı daha sonra devre 1 ' e geçirilir.

1. Yetkilendirme anahtarı oluşturun.

   :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/create-authorization-expressroute-circuit.png" alt-text="Yetkilendirme anahtarı oluşturma ekranının ekran görüntüsü."::: 

   Devre 2 ' nin devre dışı kaynak KIMLIĞINI ve yetkilendirme anahtarını bir yere getirin.

1. **Azure özel** eşleme yapılandırmasını seçin. 

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-private-peering.png" alt-text="Genel Bakış sayfasında özel eşleme ekran görüntüsü.":::

1. *Global Reach yapılandırma Ekle* sayfasını açmak Için **Global Reach Ekle** ' yi seçin.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/private-peering-enable-global-reach.png" alt-text="Özel eşleme içinde Global Reach ekleme görüntüsünün ekran görüntüsü.":::

1. Global Reach yapılandırma *Ekle* sayfasında bu yapılandırmaya bir ad verin. **Kullanma yetkilendirme** kutusunu işaretleyin. 1. adımda oluşturulan ve edinilen **Yetkilendirme anahtarını** ve **EXPRESSROUTE** bağlantı hattı kimliğini girin. Sonra *Global Reach alt ağı* için bir **/29 IPv4** sağlayın. İki ExpressRoute devreleri arasında bağlantı kurmak için bu alt ağdaki IP adreslerini kullanıyoruz. Bu alt ağdaki adresleri Azure sanal ağlarınızda veya şirket içi ağınızda kullanmayın. Devreyi özel eşleme yapılandırmasına eklemek için **Ekle** ' yi seçin.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/add-global-reach-configuration-with-authorization.png" alt-text="Yetkilendirme anahtarıyla Global Reach Ekle ekran görüntüsü.":::

1. Global Reach yapılandırmasını gerçekleştirmek için **Kaydet** ' i seçin. İşlem tamamlandığında, hem ExpressRoute devreleri aracılığıyla iki şirket içi ağınız arasında bağlantınız olur.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/save-private-peering-configuration.png" alt-text="Global Reach ile özel eşleme yapılandırmasını kaydetme ekran görüntüsü.":::

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama

ExpressRoute bağlantı hattı yapılandırması altında *özel eşleme* ' i seçerek Global Reach yapılandırmasını doğrulayın. Doğru yapılandırıldığında yapılandırmanız şu şekilde görünmelidir:

:::image type="content" source="./media/expressroute-howto-set-global-reach-portal/verify-global-reach-configuration.png" alt-text="Global Reach 'ın ekran görüntüsü.":::

## <a name="disable-connectivity"></a>Bağlantıyı devre dışı bırakma

Tek bir devre arasındaki bağlantıyı devre dışı bırakmak için *Global Reach adının* yanındaki Sil düğmesini seçerek aralarında bağlantıyı kaldırın. Ardından, işlemi gerçekleştirmek için **Kaydet** ' i seçin.

:::image type="content" source="./media/expressroute-howto-set-global-reach-portal/disable-global-reach-configuration.png" alt-text="Global Reach devre dışı bırakmayı gösteren ekran görüntüsü.":::

İşlem tamamlandıktan sonra, ExpressRoute devrelerinizi kullanarak şirket içi ağınız arasında artık bağlantı kuramadınız.

## <a name="next-steps"></a>Sonraki adımlar
- [ExpressRoute Global Reach hakkında daha fazla bilgi edinin](expressroute-global-reach.md)
- [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
- [Azure sanal ağına bir ExpressRoute bağlantı hattı bağlama](expressroute-howto-linkvnet-arm.md)
