---
title: "Azure ExpressRoute: ExpressRoute Direct 'i yapılandırma: Portal"
description: Bu sayfa, portalı kullanarak ExpressRoute Direct 'i yapılandırmanıza yardımcı olur.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: how-to
ms.date: 06/12/2020
ms.author: cherylmc
ms.openlocfilehash: f22f81d69d746764af43d9167874aabedbd56f51
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84765445"
---
# <a name="create-expressroute-direct-using-the-azure-portal"></a>Azure portal kullanarak doğrudan ExpressRoute oluşturma

Bu makalede, Azure portal kullanarak doğrudan ExpressRoute 'a nasıl oluşturacağınız gösterilmektedir.
ExpressRoute Direct, Microsoft 'un Dünya genelinde stratejik olarak dağıtılan eşleme konumlarında doğrudan Microsoft 'un küresel ağına bağlanmanızı sağlar. Daha fazla bilgi için bkz. [ExpressRoute Direct Hakkında](expressroute-erdirect-about.md).

## <a name="1-create-expressroute-direct"></a><a name="create-erdir"></a>1. ExpressRoute Direct oluşturma

1. [Azure Portal](https://portal.azure.com) menüsünde veya **giriş** sayfasında, **kaynak oluştur**' u seçin.

1. **Yeni** sayfada, ***marketi ara*** alanında **ExpressRoute Direct**yazın ve arama sonuçlarına ulaşmak için **ENTER** ' u seçin.

1. Sonuçlardan **ExpressRoute Direct**' i seçin.

1. **ExpressRoute** Direct sayfasında **Oluştur** ' u seçerek **ExpressRoute doğrudan oluştur** sayfasını açın.

1. **Temel bilgiler** sayfasındaki alanları tamamlayarak başlayın.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/basics.png" alt-text="Temel bilgiler sayfası":::

    * **Abonelik**: yeni bir ExpressRoute Direct oluşturmak için kullanmak istediğiniz Azure aboneliği. ExpressRoute doğrudan kaynağı ve ExpressRoute devreleri aynı abonelikte olmalıdır.
    * **Kaynak grubu**: yeni ExpressRoute doğrudan kaynağının oluşturulacağı Azure Kaynak grubu. Mevcut bir kaynak grubunuz yoksa yeni bir tane oluşturabilirsiniz.
    * **Bölge**: kaynağın oluşturulacağı Azure ortak bölgesi.
    * **Ad**: yeni ExpressRoute doğrudan kaynağının adı.

1. Sonra **yapılandırma** sayfasındaki alanları doldurun.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/configuration.png" alt-text="Yapılandırma sayfası":::

    * **Eşleme konumu**: ExpressRoute doğrudan kaynağına bağlandığınız eşleme konumu. Eşleme konumları hakkında daha fazla bilgi için bkz. [ExpressRoute konumlarını](expressroute-locations-providers.md)gözden geçirin.
   * **Bant genişliği**: ayırmak istediğiniz bağlantı noktası çifti bant genişliği. ExpressRoute Direct, hem 10 GB hem de 100 GB bant genişliği seçeneklerini destekler. İstediğiniz bant genişliğiniz belirtilen eşleme konumunda yoksa, [Azure Portal bir destek isteği açın](https://aka.ms/azsupt).
   * **Kapsülleme**: ExpressRoute Direct hem QinQ hem de Dot1Q kapsüllemeyi destekler.
     * QinQ seçilirse, her bir ExpressRoute devresine dinamik olarak bir S etiketi atanır ve ExpressRoute doğrudan kaynağında benzersiz olacaktır.
     *  Devrendeki her C etiketi, Devroute Direct üzerinde değil, devre üzerinde benzersiz olmalıdır.
     * Dot1Q kapsülleme seçilirse, tüm ExpressRoute doğrudan kaynağı genelinde C-Tag (VLAN) öğesinin benzersizliğini yönetmeniz gerekir.
     >[!IMPORTANT]
     >ExpressRoute Direct yalnızca bir kapsülleme türü olabilir. ExpressRoute doğrudan oluşturulduktan sonra kapsülleme değiştirilemez.
     >

1. Herhangi bir kaynak etiketi belirtin ve ardından ExpressRoute doğrudan kaynak ayarlarını doğrulamak için **gözden geçir + oluştur** ' u seçin.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/validate.png" alt-text="Gözden geçirme ve oluşturma":::

1. **Oluştur**'u seçin. Dağıtımınızın devam ettiğinden emin olarak bir ileti görürsünüz. Kaynaklar oluşturulduğundan bu sayfada durum görüntülenecektir. 

## <a name="2-change-admin-state-of-links"></a><a name="state"></a>2. bağlantıların yönetici durumunu değiştirin

Bu işlem, katman 1 testi yürütmek için kullanılmalıdır, her bir çapraz bağlantının, birincil ve ikincil için her bir yönlendiriciye düzgün bir şekilde düzeltme eki olmasını sağlar.

1. ExpressRoute doğrudan kaynağına **genel bakış** sayfasında, **Bağlantılar** bölümünde **link1**' yi seçin.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/link.png" alt-text="Bağlantı 1" lightbox="./media/how-to-expressroute-direct-portal/link-expand.png":::

1. **Yönetici durumu** ayarını **etkin**olarak değiştirin, sonra **Kaydet**' i seçin.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/state.png" alt-text="Yönetici durumu":::

    >[!IMPORTANT]
    >Faturalandırma, her iki bağlantıda de yönetici durumu etkinleştirildiğinde başlar.
    >

1. **Link2**için aynı işlemi tekrarlayın.

## <a name="3-create-a-circuit"></a><a name="circuit"></a>3. devre oluşturma

Varsayılan olarak, abonelikte ExpressRoute doğrudan kaynağının olduğu 10 devre oluşturabilirsiniz. Bu sayı, destek ile artırılabilir. Hem sağlanan hem de kullanılan bant genişliğini izlemeden sorumlusunuz. Sağlanan bant genişliği, ExpressRoute doğrudan kaynağındaki tüm devrelerin bant genişliğinin toplamıdır. Kullanılan bant genişliği, temeldeki fiziksel arabirimlerin fiziksel kullanımdır.

* Yalnızca yukarıda özetlenen senaryoları desteklemek üzere ExpressRoute Direct üzerinde kullanılabilen ek devre bant genişlikleri vardır. Bunlar: 40 Gbps ve 100 Gbps.

* SkuTier yerel, standart veya Premium olabilir.

* SkuFamily yalnızca MeteredData olmalıdır. ExpressRoute Direct 'te sınırsız desteklenmez.

Aşağıdaki adımlar, ExpressRoute doğrudan iş akışından bir ExpressRoute devresi oluşturmanıza yardımcı olur. Bunun yerine, bu yapılandırma için normal devre iş akışı adımlarını kullanmanın bir avantajı olmasa da, normal devre iş akışını kullanarak da bir devre oluşturabilirsiniz. Bkz. [bir ExpressRoute bağlantı hattı oluşturma ve değiştirme](expressroute-howto-circuit-portal-resource-manager.md).

1. ExpressRoute doğrudan **ayarları** bölümünde **devreler**' i seçin ve **+ Ekle**' yi seçin. 

    :::image type="content" source="./media/how-to-expressroute-direct-portal/add.png" alt-text="Ekle" lightbox="./media/how-to-expressroute-direct-portal/add-expand.png":::

1. **Yapılandırma** sayfasında ayarları yapılandırın.

   :::image type="content" source="./media/how-to-expressroute-direct-portal/configuration2.png" alt-text="Yapılandırma sayfası":::

1. Kaynağı oluşturmadan önce değerleri doğrulamak için bir kaynak etiketi seçin, **gözden geçir + oluştur** seçeneğini belirleyin.

   :::image type="content" source="./media/how-to-expressroute-direct-portal/review.png" alt-text="Gözden geçirme ve oluşturma":::

1. **Oluştur**'u seçin. Dağıtımınızın devam ettiğinden emin olarak bir ileti görürsünüz. Kaynaklar oluşturulduğundan bu sayfada durum görüntülenecektir. 

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute Direct hakkında daha fazla bilgi için bkz. [genel bakış](expressroute-erdirect-about.md).
