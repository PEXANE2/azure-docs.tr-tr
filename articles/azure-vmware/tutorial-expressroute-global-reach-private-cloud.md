---
title: Eğitim-şirket içi ortamlarından özel bir buluta
description: Azure VMware çözümünde özel buluta ExpressRoute Global Reach eşlemesi oluşturmayı öğrenin.
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: 679887e1998a534001e72ddff7a1a184a84bd831
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91254744"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Öğretici: şirket içi ortamlar için özel bir buluta

ExpressRoute Global Reach şirket içi ortamınızı Azure VMware çözümünüz özel bulutuna bağlar. ExpressRoute Global Reach bağlantısı, özel bulut ExpressRoute devresi ve şirket içi ortamlarınızla mevcut bir ExpressRoute bağlantısı arasında oluşturulur. 

[Azure 'dan özel bulut ağını yapılandırırken](tutorial-configure-networking.md) kullandığınız ExpressRoute bağlantı hattı, ExpressRoute ağ geçitlerine veya Global Reach kullanarak diğer ExpressRoute devrelerine eşler için yetkilendirme anahtarları oluşturmanızı ve kullanmanızı gerektirir. ExpressRoute bağlantı hattı tarafından zaten bir yetkilendirme anahtarı kullandınız ve bu öğreticide şirket içi ExpressRoute bağlantı hattını kullanarak ikinci bir tane oluşturabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * _Devre 2_için ikinci bir yetkilendirme anahtarı oluşturun, özel bulut ExpressRoute devresi
> * Şirket içinden özel bulut ExpressRoute Global Reach eşliğini etkinleştirmek için [Azure portal](#azure-portal-method) [Cloud Shell](#azure-cli-in-a-cloud-shell-method) veya _devre_ dışı bırakın.


## <a name="before-you-begin"></a>Başlamadan önce

ExpressRoute Global Reach kullanarak iki ExpressRoute bağlantı hattı arasında bağlantıyı etkinleştirmeden önce, [farklı Azure aboneliklerinde bağlantının nasıl etkinleştirileceği](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions)hakkındaki belgeleri gözden geçirin.  


## <a name="prerequisites"></a>Önkoşullar

1. Azure VMware Çözüm özel bulutuyla, ExpressRoute bağlantı hattı, bir Azure sanal ağı 'nda (VNet) bir ExpressRoute bağlantı hattı ile, eşleme yordamlarından itibaren _devre 2_ olan bağlantı kurdu.  
1. Şirket içi ortamları Azure 'a bağlamak için kullanılan, eşleme yordamlarının perspektifinden _devre_ dışı olan ayrı, çalışan bir ExpressRoute devresi.
1. ExpressRoute Global Reach eşlemesi için/29 çakışmayan bir [ağ adresi bloğu](../expressroute/expressroute-routing.md#ip-addresses-used-for-peerings) .

> [!TIP]
> Bu önkoşulların bağlamında, şirket içi ExpressRoute bağlantı hattı devre _1_' dir ve özel bulut ExpressRoute devreniz farklı bir abonelikte ve _devre 2_' etiketlidir. 


## <a name="create-an-expressroute-authorization-key-in-the-private-cloud"></a>Özel bulutta bir ExpressRoute yetkilendirme anahtarı oluşturma

1. Özel buluta **genel bakış**' da, Yönet altında **bağlantı > ExpressRoute > yetkilendirme anahtarı iste**' yi seçin.

   :::image type="content" source="media/expressroute-global-reach/start-request-auth-key.png" alt-text="> ExpressRoute > bağlantı seçin ve yeni bir istek başlatmak için bir yetkilendirme anahtarı Isteyin.":::

2. Yetkilendirme anahtarı için bir ad girin ve **Oluştur**' u seçin. 

   :::image type="content" source="media/expressroute-global-reach/create-global-reach-auth-key.png" alt-text="Yeni bir yetkilendirme anahtarı oluşturmak için Oluştur ' a tıklayın. ":::

   Yeni anahtar oluşturulduktan sonra, özel bulut için yetkilendirme anahtarları listesinde görünür. 

   :::image type="content" source="media/expressroute-global-reach/show-global-reach-auth-key.png" alt-text="Yeni yetkilendirme anahtarının, özel bulut için anahtarlar listesinde göründüğünü doğrulayın. ":::

3. Yetkilendirme anahtarını ve ExpressRoute KIMLIĞINI/29 adres bloğunu birlikte bir yere unutmayın. Eşlemeyi tamamlayabilmeniz için bir sonraki adımda kullanacaksınız. 

## <a name="peer-private-cloud-to-on-premises-using-authorization-key"></a>Yetkilendirme anahtarını kullanarak eşler arası özel bulut şirket içi

Özel bulut ExpressRoute bağlantı hattı için bir yetkilendirme anahtarı oluşturduğunuza göre, bunu şirket içi ExpressRoute bağlantı hattınızla birlikte kullanabilirsiniz.  Eşleme, [Azure Portal](#azure-portal-method) şirket Içi ExpressRoute bağlantı hattı perspektifinden ya da [Azure clı 'yi bir Cloud Shell](#azure-cli-in-a-cloud-shell-method)kullanılarak yapılır. Her iki yöntemde de, eşlemeyi tamamlaması için önceki adımlarda oluşturduğunuz özel bulut ExpressRoute devrenizin kaynak KIMLIĞI ve yetkilendirme anahtarını kullanacaksınız.

### <a name="azure-portal-method"></a>Azure portal yöntemi

1. Şirket içi ExpressRoute devresi ile aynı aboneliği kullanarak [Azure Portal](https://portal.azure.com) oturum açın.

1. Özel buluta **genel bakış**' da, Yönet altında **bağlantı > expressroute Global Reach > Ekle**' yi seçin.

   :::image type="content" source="./media/expressroute-global-reach/expressroute-global-reach-tab.png" alt-text="Menüden bağlantı, ExpressRoute Global Reach sekmesini seçin ve ardından ekleyin.":::

1. Aşağıdakilerden birini yaparak şirket içi bir bulut bağlantısı oluşturabilirsiniz:

   - Listeden ExpressRoute devresini seçin.
   - Devre KIMLIĞINIZ varsa kopyalayın ve yapıştırın.

1. **Bağlan**’ı seçin. Yeni bağlantı, şirket içi bulut bağlantıları listesinde gösterilir.  

>[!TIP]
>**Daha fazla**seçim yaparak bir bağlantıyı silebilir veya listeden bağlantısını kesebilirsiniz.  
>
> :::image type="content" source="./media/expressroute-global-reach/on-premises-connection-disconnect.png" alt-text="Şirket içi bağlantının bağlantısını kesme veya silme":::

### <a name="azure-cli-in-a-cloud-shell-method"></a>Cloud Shell yönteminde Azure CLı

[CLI komutlarını](../expressroute/expressroute-howto-set-global-reach-cli.md) , şirket içi ortamlar arasında Azure VMware Çözüm özel bulutuna yönelik expressroute Global Reach eşlemeyi yapılandırmanıza yardımcı olacak belirli Ayrıntılar ve örneklerle genişlettik.  

> [!TIP]  
> Azure CLı komut çıkışında bu yönergeler, [ `–query` yalnızca gerekli sonuçları göstermek Için bir JMESPath sorgusu yürütmek üzere bir bağımsız değişken](https://docs.microsoft.com/cli/azure/query-azure-cli)kullanabilir.


1. Şirket içi ExpressRoute devresi ile aynı aboneliği kullanarak Azure portal oturum açın ve bir Cloud Shell açın. Kabuğu Bash olarak bırakın.
 
   :::image type="content" source="media/expressroute-global-reach/open-cloud-shell.png" alt-text="Azure portal oturum açın ve bir Cloud Shell açın.":::
 
2. Özel bilgilerinizi ve kaynak KIMLIĞINIZI, yetkilendirme anahtarınızı ve/29 CıDR ağ bloğunu kullanarak eşleme oluşturmak için komut satırında Azure CLı komutunu girin. 

   Aşağıda, kullanacağınız komutun ve başarılı bir eşlemeyi belirten çıktının bir örneği verilmiştir. Örnek komut, [Adım 3 ' te "farklı Azure aboneliklerinde ExpressRoute devreleri arasında bağlantıyı etkinleştirme"](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions)bölümünde kullanılan komutu temel alır.

   :::image type="content" source="media/expressroute-global-reach/azure-command-with-results.png" alt-text="Azure CLı komutuyla bir Cloud Shell ExpressRoute Global Reach eşleme oluşturun.":::
 
   Artık, şirket içi ortamlardan ExpressRoute Global Reach eşlemesi üzerinden özel bulutunuzun bağlantısını sağlayabilmelisiniz.

> [!TIP]
> [Şirket içi ağlarınızla ilgili yönergeler arasında bağlantıyı devre dışı bırak bağlantısını](../expressroute/expressroute-howto-set-global-reach-cli.md#disable-connectivity-between-your-on-premises-networks) izleyerek yeni oluşturduğunuz eşlemeyi silebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel bulut ExpressRoute bağlantı hattı için ikinci bir yetkilendirme anahtarı oluşturmayı ve şirket içi-özel Cloud ExpressRoute Global Reach eşlemesini nasıl etkinleştirdiklerini öğrendiniz. 

Azure VMware çözümünüz özel bulutunuz için VMware HCX çözümünü dağıtma ve yapılandırma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [VMware HCX 'i dağıtma ve yapılandırma](hybrid-cloud-extension-installation.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
