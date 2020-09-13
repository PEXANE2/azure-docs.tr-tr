---
title: 'ExpressRoute: yönlendirme filtreleri-Microsoft eşlemesi: Azure portal'
description: Bu makalede, Azure portal kullanılarak Microsoft eşlemesi için yol filtrelerinin nasıl yapılandırılacağı açıklanır.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 07/01/2019
ms.author: duau
ms.custom: seodec18
ms.openlocfilehash: 37f8903adbc676ae2e48e2ef5841d8f5b122842c
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89566254"
---
# <a name="configure-route-filters-for-microsoft-peering-azure-portal"></a>Microsoft eşlemesi için yol filtrelerini yapılandırın: Azure portal
> [!div class="op_single_selector"]
> * [Azure Portalı](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure CLI](how-to-routefilter-cli.md)
> 

Rota filtreleri, desteklenen servislerin bir alt kümesini Microsoft eşlemesi aracılığıyla kullanmanın bir yoludur. Bu makaledeki adımlar, ExpressRoute devreleri için yol filtrelerini yapılandırmanıza ve yönetmenize yardımcı olur.

Exchange Online, SharePoint Online ve Skype Kurumsal gibi Microsoft 365 hizmetlere ve depolama ve SQL DB gibi Azure hizmetlerine Microsoft eşlemesi aracılığıyla erişilebilir. ExpressRoute bağlantı hattındaki Microsoft eşlemesi yapılandırıldığında, bu hizmetlerle ilgili tüm önekler oluşturulan BGP oturumları aracılığıyla bildirilir. Ön ek aracılığıyla sunulan hizmeti tanımlamak için her ön eke BGP topluluk değeri eklenir. BGP topluluk değerlerinin ve eşlendikleri hizmetlerin listesi için bkz. [BGP toplulukları](expressroute-routing.md#bgp).

Tüm hizmetlere bağlantı gerekiyorsa, BGP aracılığıyla çok sayıda önek tanıtılabilir. Bu, ağınızdaki yönlendiriciler tarafından tutulan yol tablolarının boyutunu önemli ölçüde artırır. Yalnızca Microsoft eşlemesi üzerinden sunulan hizmetlerin bir alt kümesini kullanmak istiyorsanız, yol tablolarınızın boyutunu iki şekilde azaltabilirsiniz. Seçenekleriniz şunlardır:

- BGP topluluklarına rota filtreleri uygulayarak istenmeyen önekleri filtreleyin. Bu standart bir ağ uygulamasıdır ve genellikle birçok ağ içinde kullanılır.

- Rota filtrelerini tanımlayın ve bunları ExpressRoute devrenizi ile uygulayın. Yol filtresi, Microsoft eşlemesi aracılığıyla tüketmek üzere planladığınız hizmetlerin listesini seçmenize olanak sağlayan yeni bir kaynaktır. ExpressRoute yönlendiricileri yalnızca yol filtresinde tanımlanan hizmetlere ait olan ön eklerin listesini gönderir.

### <a name="about-route-filters"></a><a name="about"></a>Yol filtreleri hakkında

ExpressRoute bağlantı hattınızda Microsoft eşlemesi yapılandırıldığında, Microsoft Edge yönlendiricileri, uç yönlendiricilerle (sizinki veya bağlantı sağlayıcınız) bir çift BGP oturumu oluşturur. Ağınıza rota tanıtılmaz. Ağınızda rota tanıtılmasını sağlamak için bir rota filtresiyle ilişkilendirmeniz gerekir.

Rota filtresi, ExpressRoute bağlantı hattınızın Microsoft eşlemesi üzerinden kullanmak istediğiniz hizmetleri tanımlamanızı sağlar. Bu aslında, izin vermek istediğiniz tüm BGP topluluk değerlerinin bir listesidir. Rota filtresi kaynağı tanımlandıktan ve bir ExpressRoute bağlantı hattına eklendikten sonra BGP topluluk değerleriyle eşleşen tüm ön ekler ağınızda tanıtılır.

Yönlendirme filtrelerini bunlara Microsoft 365 hizmetleriyle iliştirebilmek için, ExpressRoute aracılığıyla Microsoft 365 hizmetleri kullanma yetkisiyle sahip olmanız gerekir. ExpressRoute aracılığıyla Microsoft 365 hizmetlerini kullanma yetkiniz yoksa, yol filtreleri iliştirme işlemi başarısız olur. Yetkilendirme işlemi hakkında daha fazla bilgi için bkz. [Azure ExpressRoute for Microsoft 365](/microsoft-365/enterprise/azure-expressroute).

> [!IMPORTANT]
> 1 Ağustos 2017 ' den önce yapılandırılmış ExpressRoute bağlantı hattı Microsoft eşlemesi, yol filtreleri tanımlanmasa bile Microsoft eşlemesi aracılığıyla tanıtılan tüm hizmet öneklerini alacak. 1 Ağustos 2017 ' de veya sonrasında yapılandırılan ExpressRoute devrelerinin Microsoft eşlemesi, bir yol filtresi devresine iliştirilene kadar tanıtılan öneklere sahip olmayacaktır.
> 
> 

### <a name="workflow"></a><a name="workflow"></a>İş akışı

Microsoft eşlemesi aracılığıyla hizmetlere başarıyla bağlanabiliyor olması için aşağıdaki yapılandırma adımlarını gerçekleştirmeniz gerekir:

- Microsoft eşlemesi sağlanmış etkin bir ExpressRoute devresine sahip olmanız gerekir. Aşağıdaki yönergeleri kullanarak şu görevleri gerçekleştirebilirsiniz:
  - Devam etmeden önce [bir ExpressRoute devresi oluşturun](expressroute-howto-circuit-portal-resource-manager.md) ve bağlantı sağlayıcınız tarafından devre dışı bırakıldı. ExpressRoute bağlantı hattı sağlanmış ve etkinleştirilmiş durumda olmalıdır.
  - BGP oturumunu doğrudan yönetiyorsanız, [Microsoft eşlemesi oluşturun](expressroute-howto-routing-portal-resource-manager.md) . Ya da bağlantı sağlayıcınızın, devreniz için Microsoft eşlemesi sağlamasını sağlayın.

-  Bir yol filtresi oluşturmanız ve yapılandırmanız gerekir.
    - Microsoft eşlemesi aracılığıyla kullanmak için kullandığınız Hizmetleri tanımla
    - Hizmetlerle ilişkili BGP topluluk değerlerinin listesini belirler
    - BGP topluluk değerleriyle eşleşen ön ek listesine izin veren bir kural oluşturma

-  Yol filtresini ExpressRoute devresine bağlamanız gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki ölçütleri karşıladığınızdan emin olun:

 - Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

 - Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. ExpressRoute bağlantı hattı sağlanmış ve etkinleştirilmiş durumda olmalıdır.

 - Etkin bir Microsoft eşlemesine sahip olmanız gerekir. [Eşleme yapılandırması oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md) yönergelerini izleyin


## <a name="step-1-get-a-list-of-prefixes-and-bgp-community-values"></a><a name="prefixes"></a>1. Adım: ön eklerin ve BGP topluluk değerlerinin listesini alın

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP topluluk değerlerinin bir listesini alın

Microsoft eşlemesi aracılığıyla erişilebilen hizmetlerle ilişkili BGP topluluk değerleri, [ExpressRoute yönlendirme gereksinimleri](expressroute-routing.md) sayfasında bulunabilir.

### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. kullanmak istediğiniz değerlerin bir listesini oluşturun

Yol filtresinde kullanmak istediğiniz [BGP topluluk değerlerinin](expressroute-routing.md#bgp) bir listesini oluşturun. 

## <a name="step-2-create-a-route-filter-and-a-filter-rule"></a><a name="filter"></a>2. Adım: bir yol filtresi ve bir filtre kuralı oluşturma

Yol filtresi yalnızca bir kurala sahip olabilir ve kuralın ' Allow ' türünde olması gerekir. Bu kural, kendisiyle ilişkili BGP topluluk değerlerinin bir listesine sahip olabilir.

### <a name="1-create-a-route-filter"></a>1. yol filtresi oluşturma
Yeni kaynak oluşturma seçeneğini belirleyerek bir yol filtresi oluşturabilirsiniz. **Create a resource**  >  **Networking**  >  Aşağıdaki görüntüde gösterildiği gibi, kaynak ağı oluştur ağ**routefilter**öğesine tıklayın:

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/CreateRouteFilter1.png)

Yol filtresini bir kaynak grubuna yerleştirmeniz gerekir. 

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a>2. filtre kuralı oluşturma

Yönlendirme filtreniz için kural Yönet sekmesini seçerek kuralları ekleyebilir ve güncelleştirebilirsiniz.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/ManageRouteFilter.png)


Açılan listeden bağlanmak istediğiniz hizmetleri seçebilir ve tamamlandığında kuralı kaydedebilirsiniz.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/AddRouteFilterRule.png)


## <a name="step-3-attach-the-route-filter-to-an-expressroute-circuit"></a><a name="attach"></a>3. Adım: yol filtresini bir ExpressRoute devresine Iliştirme

"Devre Ekle" düğmesini seçerek ve açılan listeden ExpressRoute bağlantı hattını seçerek yol filtresini bir devreye ekleyebilirsiniz.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/AddCktToRouteFilter.png)

Bağlantı sağlayıcısı ExpressRoute devreniz için eşlemeyi yapılandırırsa, "devre Ekle" düğmesini seçmeden önce ExpressRoute bağlantı hattı dikey penceresinden devreyi yenileyin.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/RefreshExpressRouteCircuit.png)

## <a name="common-tasks"></a><a name="tasks"></a>Genel görevler

### <a name="to-get-the-properties-of-a-route-filter"></a><a name="getproperties"></a>Bir yol filtresinin özelliklerini almak için

Kaynağı portalda açtığınızda yol filtresinin özelliklerini görüntüleyebilirsiniz.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/ViewRouteFilter.png)


### <a name="to-update-the-properties-of-a-route-filter"></a><a name="updateproperties"></a>Bir yol filtresinin özelliklerini güncelleştirmek için

"Kuralı Yönet" düğmesini seçerek bir bağlantı hattına eklenen BGP topluluk değerlerinin listesini güncelleştirebilirsiniz.


![Rota filtresi oluşturma](./media/how-to-routefilter-portal/ManageRouteFilter.png)

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/AddRouteFilterRule.png) 


### <a name="to-detach-a-route-filter-from-an-expressroute-circuit"></a><a name="detach"></a>Bir ExpressRoute bağlantı hattından yol filtresini ayırmak için

Bir devreyi yol filtresinden ayırmak için, devreye sağ tıklayın ve "ilişkiyi kaldır" a tıklayın.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/DetachRouteFilter.png) 


### <a name="to-delete-a-route-filter"></a><a name="delete"></a>Yol filtresini silmek için

Bir rota filtresini Sil düğmesini seçerek silebilirsiniz. 

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/DeleteRouteFilter.png) 

## <a name="next-steps"></a>Sonraki Adımlar

* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).

* Yönlendirici yapılandırma örnekleri hakkında daha fazla bilgi için bkz. [yönlendirme ayarlamak ve yönetmek Için yönlendirici yapılandırma örnekleri](expressroute-config-samples-routing.md). 
