---
title: 'ExpressRoute: Rota filtreleri - Microsoft peering:Azure portalı'
description: Bu makalede, Azure portalını kullanarak Microsoft'un bakışları için rota filtrelerinin nasıl yapılandırılabildiğini açıklanmaktadır.
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: article
ms.date: 07/01/2019
ms.author: ganesr
ms.custom: seodec18
ms.openlocfilehash: 0b8e06ad5688374e5ab4aaa72d8485e6da797afe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74037450"
---
# <a name="configure-route-filters-for-microsoft-peering-azure-portal"></a>Microsoft'un bakışları için rota filtrelerini yapılandırma: Azure portalı
> [!div class="op_single_selector"]
> * [Azure Portalı](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure CLI](how-to-routefilter-cli.md)
> 

Rota filtreleri, desteklenen servislerin bir alt kümesini Microsoft eşlemesi aracılığıyla kullanmanın bir yoludur. Bu makaledeki adımlar, ExpressRoute devreleri için rota filtrelerini yapılandırmanıza ve yönetmenize yardımcı olur.

Exchange Online, SharePoint Online ve Skype for Business gibi Office 365 hizmetleri ve depolama ve SQL DB gibi Azure hizmetlerine Microsoft'un bakışları aracılığıyla erişilebilir. Microsoft eşleme bir ExpressRoute devresinde yapılandırıldığında, bu hizmetlerle ilgili tüm öneekler oluşturulan BGP oturumları aracılığıyla duyurulur. Ön ek aracılığıyla sunulan hizmeti tanımlamak için her ön eke BGP topluluk değeri eklenir. BGP topluluk değerlerinin ve eşledikleri hizmetlerin listesi için [Bkz. BGP toplulukları.](expressroute-routing.md#bgp)

Tüm hizmetlere bağlantı gerektirirseniz, BGP üzerinden çok sayıda önek ilan edilir. Bu, ağınızdaki yönlendiriciler tarafından tutulan rota tablolarının boyutunu önemli ölçüde artırır. Microsoft'un bakışları aracılığıyla sunulan hizmetlerin yalnızca bir alt kümesini tüketmeyi planlıyorsanız, rota tablolarınızın boyutunu iki şekilde küçültebilirsiniz. Şunları yapabilirsiniz:

- BGP topluluklarına rota filtreleri uygulayarak istenmeyen önekleri filtreleyin. Bu standart bir ağ uygulamasıdır ve birçok ağda yaygın olarak kullanılır.

- Rota filtrelerini tanımlayın ve ExpressRoute devrenize uygulayın. Rota filtresi, Microsoft'un bakışları aracılığıyla tüketmeyi planladığınız hizmetlerin listesini seçmenize olanak tanıyan yeni bir kaynaktır. ExpressRoute yönlendiricileri yalnızca rota filtresinde tanımlanan hizmetlere ait öneklistesini gönderir.

### <a name="about-route-filters"></a><a name="about"></a>Rota filtreleri hakkında

Microsoft'un bakışları ExpressRoute devrenizde yapılandırıldığında, Microsoft kenar yönlendiricileri kenar yönlendiricileri (sizin veya bağlantı sağlayıcınızın) bir çift BGP oturumu kurar. Ağınıza rota tanıtılmaz. Ağınızda rota tanıtılmasını sağlamak için bir rota filtresiyle ilişkilendirmeniz gerekir.

Rota filtresi, ExpressRoute bağlantı hattınızın Microsoft eşlemesi üzerinden kullanmak istediğiniz hizmetleri tanımlamanızı sağlar. Bu aslında izin vermek istediğiniz tüm BGP topluluk değerlerinin bir listesidir. Rota filtresi kaynağı tanımlandıktan ve bir ExpressRoute bağlantı hattına eklendikten sonra BGP topluluk değerleriyle eşleşen tüm ön ekler ağınızda tanıtılır.

Üzerlerinde Office 365 hizmetleri olan rota filtreleri ekleyebilmek için ExpressRoute üzerinden Office 365 hizmetlerini kullanma yetkiniz olması gerekir. ExpressRoute üzerinden Office 365 hizmetlerini tüketme yetkiniz yoksa, rota filtrelerini ekleme işlemi başarısız olur. Yetkilendirme işlemi hakkında daha fazla bilgi için [Office 365 için Azure ExpressRoute'a](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)bakın.

> [!IMPORTANT]
> 1 Ağustos 2017 tarihinden önce yapılandırılan ExpressRoute devrelerinin Microsoft'a bakması, rota filtreleri tanımlanmamış olsa bile, Microsoft'un bakışları aracılığıyla duyurulan tüm hizmet önekleri olacaktır. Microsoft' un 1 Ağustos 2017 tarihinde veya sonrasında yapılandırılan ExpressRoute devrelerine bakması, devreye bir rota filtresi iliştirilene kadar herhangi bir önek reklamı yapmayacaktır.
> 
> 

### <a name="workflow"></a><a name="workflow"></a>Iş akışı

Microsoft'un bakışları aracılığıyla hizmetlere başarılı bir şekilde bağlanabilmek için aşağıdaki yapılandırma adımlarını tamamlamanız gerekir:

- Microsoft'a bakan etkin leştirilmiş bir ExpressRoute devreniz olmalıdır. Bu görevleri gerçekleştirmek için aşağıdaki yönergeleri kullanabilirsiniz:
  - [Bir ExpressRoute devresi oluşturun](expressroute-howto-circuit-portal-resource-manager.md) ve devam etmeden önce devreyi bağlantı sağlayıcınız tarafından etkinleştirin. ExpressRoute devresi, hüküm verilen ve etkin leştirilmiş bir durumda olmalıdır.
  - BGP oturumunu doğrudan yönetiyorsanız [Microsoft'un bakışını oluşturun.](expressroute-howto-routing-portal-resource-manager.md) Veya, bağlantı sağlayıcınızın Microsoft'un devrenize bakmasına izin ver.

-  Bir rota filtresi oluşturmalı ve yapılandırmanız gerekir.
    - Microsoft'un bakışları aracılığıyla tüketmek üzere olduğunuz hizmetleri tanımlayın
    - Hizmetlerle ilişkili BGP topluluk değerlerinin listesini belirleme
    - BGP topluluk değerleriyle eşleşen önek listesine izin vermek için bir kural oluşturma

-  Rota filtresini ExpressRoute devresine takmanız gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki ölçütleri karşıladığınızdan emin olun:

 - Yapılandırmaya başlamadan önce [ön koşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

 - Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. ExpressRoute devresi, hüküm verilen ve etkin leştirilmiş bir durumda olmalıdır.

 - Etkin bir Microsoft bakışa sahip olmalısınız. [Oluşturma ve eşleme yapılandırması değiştirme](expressroute-howto-routing-portal-resource-manager.md) yönergeleriizleyin


## <a name="step-1-get-a-list-of-prefixes-and-bgp-community-values"></a><a name="prefixes"></a>Adım 1: Öneklerin ve BGP topluluk değerlerinin bir listesini alın

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP topluluk değerlerinin bir listesini alın

Microsoft'un bakışları aracılığıyla erişilebilen hizmetlerle ilişkili BGP topluluk değerleri [ExpressRoute yönlendirme gereksinimleri](expressroute-routing.md) sayfasında kullanılabilir.

### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Kullanmak istediğiniz değerlerin bir listesini yapın

Rota filtresinde kullanmak istediğiniz [BGP topluluk değerlerinin](expressroute-routing.md#bgp) bir listesini yapın. 

## <a name="step-2-create-a-route-filter-and-a-filter-rule"></a><a name="filter"></a>Adım 2: Rota filtresi ve filtre kuralı oluşturma

Bir rota filtresinin yalnızca bir kuralı olabilir ve kural 'İzin Ver' türünde olmalıdır. Bu kural, onunla ilişkili BGP topluluk değerlerinin bir listesi olabilir.

### <a name="1-create-a-route-filter"></a>1. Rota filtresi oluşturma
Yeni bir kaynak oluşturma seçeneğini seçerek bir rota filtresi oluşturabilirsiniz. Aşağıdaki resimde gösterildiği gibi bir > kaynak**Ağ** > **Rotası Filtresi** **Oluştur'u**tıklatın:

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/CreateRouteFilter1.png)

Rota filtresini bir kaynak grubuna yerleştirmeniz gerekir. 

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a>2. Filtre kuralı oluşturma

Rota filtreniz için yönet kuralı sekmesini seçerek kurallar ekleyebilir ve güncelleştirebilirsiniz.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/ManageRouteFilter.png)


Açılan listeden bağlanmak istediğiniz hizmetleri seçebilir ve yapıldığında kuralı kaydedebilirsiniz.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/AddRouteFilterRule.png)


## <a name="step-3-attach-the-route-filter-to-an-expressroute-circuit"></a><a name="attach"></a>Adım 3: Rota filtresini ExpressRoute devresine takın

"Devre ekle" düğmesini seçerek ve açılan listeden ExpressRoute devresini seçerek rota filtresini bir devreye ekleyebilirsiniz.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/AddCktToRouteFilter.png)

Bağlantı sağlayıcısı ExpressRoute devreniz için eşleme yapılandırırsa, "Devre ekle" düğmesini seçmeden önce devreyi ExpressRoute devre bıçağından yenileyin.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/RefreshExpressRouteCircuit.png)

## <a name="common-tasks"></a><a name="tasks"></a>Genel görevler

### <a name="to-get-the-properties-of-a-route-filter"></a><a name="getproperties"></a>Rota filtresi özelliklerini almak için

Portaldaki kaynağı açtığınızda rota filtresiözelliklerini görüntüleyebilirsiniz.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/ViewRouteFilter.png)


### <a name="to-update-the-properties-of-a-route-filter"></a><a name="updateproperties"></a>Rota filtresinin özelliklerini güncelleştirmek için

"Yönet kuralı" düğmesini seçerek bir devreye bağlı BGP topluluk değerleri listesini güncelleştirebilirsiniz.


![Rota filtresi oluşturma](./media/how-to-routefilter-portal/ManageRouteFilter.png)

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/AddRouteFilterRule.png) 


### <a name="to-detach-a-route-filter-from-an-expressroute-circuit"></a><a name="detach"></a>Bir Route filtresini ExpressRoute devresinden ayırmak için

Bir devreyi rota filtresinden ayırmak için, devreye sağ tıklayın ve "bağlantıyı kop" düğmesine tıklayın.

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/DetachRouteFilter.png) 


### <a name="to-delete-a-route-filter"></a><a name="delete"></a>Rota filtresini silmek için

Sil düğmesini seçerek bir rota filtresini silebilirsiniz. 

![Rota filtresi oluşturma](./media/how-to-routefilter-portal/DeleteRouteFilter.png) 

## <a name="next-steps"></a>Sonraki Adımlar

* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).

* Yönlendirici yapılandırma örnekleri hakkında daha fazla bilgi için [yönlendirmeyi ayarlamak ve yönetmek için Yönlendirici yapılandırma örneklerine](expressroute-config-samples-routing.md)bakın. 
