---
title: Şirket içi ortamlar için özel bir buluta
description: Bu Azure VMware Çözüm öğreticisinde, Azure VMware çözümünde özel bir buluta ExpressRoute Global Reach eşleme oluşturacaksınız.
ms.topic: tutorial
ms.date: 07/16/2020
ms.openlocfilehash: db3f5988cb8c07d9b6e80f500ac6aff8f96dfded
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750455"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Öğretici: şirket içi ortamlar için özel bir buluta

ExpressRoute Global Reach şirket içi ortamınızı özel bulutlarınıza bağlar. ExpressRoute Global Reach bağlantısı, özel bir bulut ExpressRoute devresi ve şirket içi ortamlarınızla mevcut bir ExpressRoute bağlantısı arasında oluşturulur.  Azure CLı ve PowerShell ile ExpressRoute Global Reach yapılandırmaya yönelik yönergeler vardır ve şirket içi ortamlar arasındaki Azure VMware Çözüm özel bulutuna yönelik ExpressRoute Global Reach eşlemeyi yapılandırmanıza yardımcı olmak için belirli Ayrıntılar ve örneklerle [CLI komutlarını](../expressroute/expressroute-howto-set-global-reach-cli.md) genişlettik.   

ExpressRoute Global Reach kullanarak iki ExpressRoute bağlantı hattı arasında bağlantıyı etkinleştirmeden önce, [farklı Azure aboneliklerinde bağlantının nasıl etkinleştirileceği](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions)hakkındaki belgeleri gözden geçirin.  [Azure 'dan özel bulut ağını yapılandırırken](tutorial-configure-networking.md) kullandığınız ExpressRoute bağlantı hattı, ExpressRoute ağ geçitlerine veya Global Reach kullanarak diğer ExpressRoute devrelerine eşler için yetkilendirme anahtarları oluşturmanızı ve kullanmanızı gerektirir. ExpressRoute bağlantı hattı tarafından zaten bir yetkilendirme anahtarı kullandınız ve şirket içi ExpressRoute bağlantı hattını kullanarak ikinci bir tane oluşturabilirsiniz.

> [!TIP]
> Bu yönergelerin bağlamında, şirket içi ExpressRoute bağlantı hattı devre _1_' dir ve özel bulut ExpressRoute devreniz farklı bir abonelikte ve _devre 2_' etiketlidir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * ExpressRoute devreleri ve ExpressRoute Global Reach eşleme yönetimi için mevcut yönergelerden yararlanın
> * Özel bulut ExpressRoute devresi için, _devre 2_için bir yetkilendirme anahtarı oluşturun
> * Şirket içinden özel bulut ExpressRoute Global Reach eşlemeyi etkinleştirmek için, _1. devrenin_ aboneliğindeki Cloud Shell Azure CLI 'yi kullanın

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticinin önkoşulları şunlardır:
- ExpressRoute devresine sahip bir Azure sanal ağı (VNet) içindeki bir ExpressRoute bağlantı hattı ile özel bir bulut – bu, eşleme yordamlarının perspektifinden _devre 2_ ' dir.
- Şirket içi ortamları Azure 'a bağlamak için kullanılan ayrı, çalışan bir ExpressRoute devresi, bu, eşleme yordamlarının perspektifinden _devre 1_ ' dir.
- ExpressRoute Global Reach eşlemesi için/29 çakışmayan bir [ağ adresi bloğu](../expressroute/expressroute-routing.md#ip-addresses-used-for-peerings) .

## <a name="create-an-expressroute-authorization-key-in-the-azure-vmware-solution-private-cloud"></a>Azure VMware Çözüm özel bulutu 'nda bir ExpressRoute yetkilendirme anahtarı oluşturma

1. Özel buluta **genel bakış**' da, Yönet altında **bağlantı > ExpressRoute > yetkilendirme anahtarı iste**' yi seçin.

   :::image type="content" source="media/expressroute-global-reach/start-request-auth-key.png" alt-text="> ExpressRoute > bağlantı seçin ve yeni bir istek başlatmak için bir yetkilendirme anahtarı Isteyin.":::

2. Yetkilendirme anahtarı için bir ad girin ve **Oluştur**' u seçin. 

   :::image type="content" source="media/expressroute-global-reach/create-global-reach-auth-key.png" alt-text="Yeni bir yetkilendirme anahtarı oluşturmak için Oluştur ' a tıklayın. ":::

   Yeni anahtar oluşturulduktan sonra, özel bulut için yetkilendirme anahtarları listesinde görünür. 

   :::image type="content" source="media/expressroute-global-reach/show-global-reach-auth-key.png" alt-text="Yeni yetkilendirme anahtarının, özel bulut için anahtarlar listesinde göründüğünü doğrulayın. ":::

3. Yetkilendirme anahtarını ve ExpressRoute KIMLIĞINI/29 adres bloğunu birlikte bir yere unutmayın. Eşlemeyi tamamlayabilmeniz için bir sonraki adımda kullanacaksınız. 

## <a name="peer-private-cloud-to-on-premises-using-authorization-key"></a>Yetkilendirme anahtarını kullanarak eşler arası özel bulut şirket içi

Özel bulut ExpressRoute bağlantı hattı için bir yetkilendirme anahtarı oluşturduğunuza göre, bunu şirket içi ExpressRoute bağlantı hattınızla birlikte kullanabilirsiniz.  Eşleme, önceki adımlarda oluşturulan özel bulut ExpressRoute devreninin bir Cloud Shell ve kaynak KIMLIĞI ve yetkilendirme anahtarı kullanılarak şirket içi ExpressRoute bağlantı hattı perspektifinden yapılır.

> [!TIP]  
> Azure CLı komut çıkışında bu yönergeler, [ `–query` yalnızca gerekli sonuçları göstermek Için bir JMESPath sorgusu yürütmek üzere bir bağımsız değişken](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest)kullanabilir.


1. Şirket içi ExpressRoute devresi ile aynı aboneliği kullanarak Azure portal oturum açın ve bir Cloud Shell açın. Kabuğu Bash olarak bırakın.
 
   :::image type="content" source="media/expressroute-global-reach/open-cloud-shell.png" alt-text="Azure portal oturum açın ve bir Cloud Shell açın.":::
 
2. Özel bilgilerinizi ve kaynak KIMLIĞINIZI, yetkilendirme anahtarınızı ve/29 CıDR ağ bloğunu kullanarak eşleme oluşturmak için komut satırında Azure CLı komutunu girin. 

   Aşağıda, kullanacağınız komutun ve başarılı bir eşlemeyi belirten çıktının bir örneği verilmiştir. Örnek komut, [Adım 3 ' te "farklı Azure aboneliklerinde ExpressRoute devreleri arasında bağlantıyı etkinleştirme"](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions)bölümünde kullanılan komutu temel alır.

   :::image type="content" source="media/expressroute-global-reach/azure-command-with-results.png" alt-text="Azure CLı komutuyla bir Cloud Shell ExpressRoute Global Reach eşleme oluşturun.":::
 
   Artık, şirket içi ortamlardan ExpressRoute Global Reach eşlemesi üzerinden özel bulutunuzun bağlantısını sağlayabilmelisiniz.

> [!TIP]
> [Şirket içi ağlarınızla ilgili yönergeler arasında bağlantıyı devre dışı bırak bağlantısını](../expressroute/expressroute-howto-set-global-reach-cli.md#disable-connectivity-between-your-on-premises-networks) izleyerek yeni oluşturduğunuz eşlemeyi silebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

VM iş yüklerini özel buluta geçirmek için karma bulut uzantısı (HCX) kullanmayı planlıyorsanız, [Azure IÇIN HCX VMware çözümünü](hybrid-cloud-extension-installation.md) kullanın.


<!-- LINKS - external-->

<!-- LINKS - internal -->
