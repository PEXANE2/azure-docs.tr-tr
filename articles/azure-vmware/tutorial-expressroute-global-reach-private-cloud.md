---
title: Eğitim-şirket içi ortamlarından özel bir buluta
description: Azure VMware çözümünde özel buluta ExpressRoute Global Reach eşlemesi oluşturmayı öğrenin.
ms.topic: tutorial
ms.date: 01/27/2021
ms.openlocfilehash: 04be94849f1cd78ff915ca271845b283c8cfc64c
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100558799"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Öğretici: şirket içi ortamlar için özel bir buluta

ExpressRoute Global Reach şirket içi ortamınızı Azure VMware çözümünüz özel bulutuna bağlar. ExpressRoute Global Reach bağlantısı, özel bulut ExpressRoute devresi ve şirket içi ortamlarınızla mevcut bir ExpressRoute bağlantısı arasında oluşturulur. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Şirket içinden özel bulut ExpressRoute Global Reach eşlemesini etkinleştirmek için Azure portal kullanın.


## <a name="before-you-begin"></a>Başlamadan önce

ExpressRoute Global Reach kullanarak iki ExpressRoute bağlantı hattı arasında bağlantıyı etkinleştirmeden önce, [farklı Azure aboneliklerinde bağlantının nasıl etkinleştirileceği](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions)hakkındaki belgeleri gözden geçirin.  


## <a name="prerequisites"></a>Önkoşullar

- Şirket içi ortamları Azure 'a bağlamak için kullanılan ayrı, çalışan bir ExpressRoute devresi.
- ExpressRoute sağlayıcısının hizmeti de dahil olmak üzere tüm ağ geçitlerinin 4 baytlık otonom sistem numarası 'nı (ASN) desteklemesini sağlayın. Azure VMware çözümü, reklam rotaları için 4 baytlık genel ASNs kullanır.


## <a name="create-an-expressroute-authorization-key-in-the-on-premises-expressroute-circuit"></a>Şirket içi ExpressRoute devresinde ExpressRoute yetkilendirme anahtarı oluşturma

1. **ExpressRoute devreleri** dikey penceresinde, ayarlar altında **yetkilendirmeler**' i seçin.

2. Yetkilendirme anahtarı için bir ad girin ve **Kaydet**' i seçin.

    :::image type="content" source="media/expressroute-global-reach/start-request-auth-key-on-premises-expressroute.png" alt-text="Yetkilendirmeler ' i seçin ve yetkilendirme anahtarının adını girin.":::
  
     Oluşturulduktan sonra yeni anahtar, devre için yetkilendirme anahtarları listesinde görünür.
 
 4. Yetkilendirme anahtarını ve ExpressRoute KIMLIĞINI bir yere göz önünde alın. Eşlemeyi tamamlayabilmeniz için bir sonraki adımda kullanacaksınız.
 
 
 ## <a name="peer-private-cloud-to-on-premises"></a>Şirket içi özel buluta eşler arası

1. Özel buluta **genel bakış**' da, Yönet altında **bağlantı > expressroute Global Reach > Ekle**' yi seçin.

    :::image type="content" source="./media/expressroute-global-reach/expressroute-global-reach-tab.png" alt-text="Menüden bağlantı, ExpressRoute Global Reach sekmesini seçin ve ardından ekleyin.":::

2. ExpressRoute KIMLIĞI ve önceki bölümde oluşturulan yetkilendirme anahtarını girin.

    :::image type="content" source="./media/expressroute-global-reach/on-premises-cloud-connections.png" alt-text="ExpressRoute KIMLIĞI ve yetkilendirme anahtarını girip oluştur ' u seçin.":::

3. **Oluştur**’u seçin. Yeni bağlantı, şirket içi bulut bağlantıları listesinde gösterilir.  

>[!TIP]
>**Daha fazla** seçim yaparak bir bağlantıyı silebilir veya listeden bağlantısını kesebilirsiniz.  
>
> :::image type="content" source="./media/expressroute-global-reach/on-premises-connection-disconnect.png" alt-text="Şirket içi bağlantının bağlantısını kesme veya silme":::


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şirket içi-özel Cloud ExpressRoute Global Reach eşlemeyi nasıl etkinleştireceğinizi öğrendiniz. 

Azure VMware çözümünüz özel bulutunuz için VMware HCX çözümünü dağıtma ve yapılandırma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [VMware HCX’i dağıtma ve yapılandırma](tutorial-deploy-vmware-hcx.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
