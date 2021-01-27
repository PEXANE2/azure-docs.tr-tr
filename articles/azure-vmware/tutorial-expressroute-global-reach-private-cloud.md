---
title: Eğitim-şirket içi ortamlarından özel bir buluta
description: Azure VMware çözümünde özel buluta ExpressRoute Global Reach eşlemesi oluşturmayı öğrenin.
ms.topic: tutorial
ms.date: 01/27/2021
ms.openlocfilehash: e7b1e349f67fe63f63183c0ff6d1522498c65f8c
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98918729"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Öğretici: şirket içi ortamlar için özel bir buluta

ExpressRoute Global Reach şirket içi ortamınızı Azure VMware çözümünüz özel bulutuna bağlar. ExpressRoute Global Reach bağlantısı, özel bulut ExpressRoute devresi ve şirket içi ortamlarınızla mevcut bir ExpressRoute bağlantısı arasında oluşturulur. 

[Azure 'dan özel bulut ağını yapılandırırken](tutorial-configure-networking.md) kullandığınız ExpressRoute bağlantı hattı, yetkilendirme anahtarları oluşturmanızı ve kullanmanızı gerektirir.  ExpressRoute bağlantı hattı tarafından zaten bir yetkilendirme anahtarı kullandınız ve bu öğreticide şirket içi ExpressRoute bağlantı hattını kullanarak ikinci bir tane oluşturabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * _Devre 2_ için ikinci bir yetkilendirme anahtarı oluşturun, özel bulut ExpressRoute devresi
> * Şirket içinden özel bulut ExpressRoute Global Reach eşliğini etkinleştirmek için [Azure portal](#azure-portal-method) [Cloud Shell](#azure-cli-in-a-cloud-shell-method) veya _devre_ dışı bırakın.


## <a name="before-you-begin"></a>Başlamadan önce

ExpressRoute Global Reach kullanarak iki ExpressRoute bağlantı hattı arasında bağlantıyı etkinleştirmeden önce, [farklı Azure aboneliklerinde bağlantının nasıl etkinleştirileceği](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions)hakkındaki belgeleri gözden geçirin.  


## <a name="prerequisites"></a>Önkoşullar

- Azure VMware Çözüm özel bulutuyla, ExpressRoute bağlantı hattı, bir Azure sanal ağı 'nda (VNet) bir ExpressRoute bağlantı hattı ile, eşleme yordamlarından itibaren _devre 2_ olan bağlantı kurdu.  
- Şirket içi ortamları Azure 'a bağlamak için kullanılan, eşleme yordamlarının perspektifinden _devre_ dışı olan ayrı, çalışan bir ExpressRoute devresi.
- ExpressRoute Global Reach eşlemesi için/29 çakışmayan bir [ağ adresi bloğu](../expressroute/expressroute-routing.md#ip-addresses-used-for-peerings) .
- ExpressRoute sağlayıcısının hizmeti de dahil olmak üzere tüm ağ geçitlerinin 4 baytlık otonom sistem numarası 'nı (ASN) desteklemesini sağlayın. Azure VMware çözümü, reklam rotaları için 4 baytlık genel ASNs kullanır.

> [!TIP]
> Bu önkoşulların bağlamında, şirket içi ExpressRoute bağlantı hattı devre _1_' dir ve özel bulut ExpressRoute devreniz farklı bir abonelikte ve _devre 2_' etiketlidir. 


## <a name="create-an-expressroute-authorization-key-in-the-private-cloud"></a>Özel bulutta bir ExpressRoute yetkilendirme anahtarı oluşturma

1. Özel buluta **genel bakış**' da, Yönet altında **bağlantı > ExpressRoute > yetkilendirme anahtarı iste**' yi seçin.

   :::image type="content" source="media/expressroute-global-reach/start-request-auth-key.png" alt-text="> ExpressRoute > bağlantı seçin ve yeni bir istek başlatmak için bir yetkilendirme anahtarı Isteyin.":::

2. Yetkilendirme anahtarı için bir ad girin ve **Oluştur**' u seçin. 

   :::image type="content" source="media/expressroute-global-reach/create-global-reach-auth-key.png" alt-text="Yeni bir yetkilendirme anahtarı oluşturmak için Oluştur ' u seçin. ":::

   Yeni anahtar oluşturulduktan sonra, özel bulut için yetkilendirme anahtarları listesinde görünür. 

   :::image type="content" source="media/expressroute-global-reach/show-global-reach-auth-key.png" alt-text="Yeni yetkilendirme anahtarının, özel bulut için anahtarlar listesinde göründüğünü doğrulayın. ":::

3. Yetkilendirme anahtarını ve ExpressRoute KIMLIĞINI/29 adres bloğunu birlikte bir yere unutmayın. Eşlemeyi tamamlayabilmeniz için bir sonraki adımda kullanacaksınız. 

## <a name="peer-private-cloud-to-on-premises-using-authorization-key"></a>Yetkilendirme anahtarını kullanarak eşler arası özel bulut şirket içi

Özel bulut ExpressRoute bağlantı hattı için bir yetkilendirme anahtarı oluşturduğunuza göre, bunu şirket içi ExpressRoute bağlantı hattınızla birlikte kullanabilirsiniz.  Eşleme, [Azure Portal](#azure-portal-method) şirket Içi ExpressRoute bağlantı hattı perspektifinden ya da [Azure clı 'yi bir Cloud Shell](#azure-cli-in-a-cloud-shell-method)kullanılarak yapılır. Her iki yöntemde de eşlemeyi tamamlaması için özel bulut ExpressRoute bağlantı hattının kaynak KIMLIĞI ve yetkilendirme anahtarını kullanırsınız.

### <a name="azure-portal-method"></a>Azure portal yöntemi

1. Şirket içi ExpressRoute devresi ile aynı aboneliği kullanarak [Azure Portal](https://portal.azure.com) oturum açın.

1. Özel buluta **genel bakış**' da, Yönet altında **bağlantı > expressroute Global Reach > Ekle**' yi seçin.

   :::image type="content" source="./media/expressroute-global-reach/expressroute-global-reach-tab.png" alt-text="Menüden bağlantı, ExpressRoute Global Reach sekmesini seçin ve ardından ekleyin.":::

1. Aşağıdaki seçeneklerden birini gerçekleştirerek şirket içi bir bulut bağlantısı oluşturabilirsiniz:

   - Listeden ExpressRoute devresini seçin.
   - Devre KIMLIĞINIZ varsa kopyalayın ve yapıştırın.

1. **Bağlan**’ı seçin. Yeni bağlantı, şirket içi bulut bağlantıları listesinde gösterilir.  

>[!TIP]
>**Daha fazla** seçim yaparak bir bağlantıyı silebilir veya listeden bağlantısını kesebilirsiniz.  
>
> :::image type="content" source="./media/expressroute-global-reach/on-premises-connection-disconnect.png" alt-text="Şirket içi bağlantının bağlantısını kesme veya silme":::

### <a name="azure-cli-in-a-cloud-shell-method"></a>Cloud Shell yönteminde Azure CLı

[CLI komutlarını](../expressroute/expressroute-howto-set-global-reach-cli.md) , şirket içi ortamlar arasında Azure VMware Çözüm özel bulutuna yönelik expressroute Global Reach eşlemeyi yapılandırmanıza yardımcı olacak belirli Ayrıntılar ve örneklerle genişlettik.  

> [!TIP]  
> Azure CLı komut çıkışında bu yönergeler, [ `–query` yalnızca gerekli sonuçları göstermek Için bir JMESPath sorgusu yürütmek üzere bir bağımsız değişken](/cli/azure/query-azure-cli)kullanabilir.


1. Şirket içi ExpressRoute devresi ile aynı aboneliği kullanarak Azure portal oturum açın ve bir Cloud Shell açın. Kabuğu Bash olarak bırakın.
 
   :::image type="content" source="media/expressroute-global-reach/open-cloud-shell.png" alt-text="Azure portal oturum açın ve bir Cloud Shell açın.":::
 
2. Eşlemeyi oluşturmak için Azure CLı komutunu girin. Belirli bilgileri ve kaynak KIMLIĞINIZI, yetkilendirme anahtarınızı ve/29 CıDR ağ bloğunu kullanın. 

   Görüntüde, kullanacağınız komutun ve başarılı bir eşlemeyi belirten çıktının bir örneği gösterilir. Örnek komut, [Adım 3 ' te "farklı Azure aboneliklerinde ExpressRoute devreleri arasında bağlantıyı etkinleştirme"](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions)bölümünde kullanılan komutu temel alır.

   :::image type="content" source="media/expressroute-global-reach/azure-command-with-results.png" alt-text="Azure CLı komutuyla bir Cloud Shell ExpressRoute Global Reach eşleme oluşturun.":::
 
   Şirket içi ortamlarından ExpressRoute Global Reach eşlemesi üzerinden özel bulutunuzun bağlantısını yapabilirsiniz.

> [!TIP]
> [Şirket içi ağlarınızla ilgili yönergeler arasında bağlantıyı devre dışı bırak bağlantısını](../expressroute/expressroute-howto-set-global-reach-cli.md#disable-connectivity-between-your-on-premises-networks) izleyerek yeni oluşturduğunuz eşlemeyi silebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel bulut ExpressRoute devresi için ikinci bir yetkilendirme anahtarı oluşturmayı öğrendiniz. Ayrıca, şirket içi-özel Cloud ExpressRoute Global Reach eşlemeyi nasıl etkinleştireceğinizi öğrendiniz. 

Azure VMware çözümünüz özel bulutunuz için VMware HCX çözümünü dağıtma ve yapılandırma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [VMware HCX’i dağıtma ve yapılandırma](tutorial-deploy-vmware-hcx.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->