---
title: 'ExpressRoute: Rota filtreleri- Microsoft peering:Azure PowerShell'
description: Bu makalede, PowerShell kullanarak Microsoft Peering için rota filtreleri nasıl yapılandırılabilen
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: ganesr
ms.custom: seodec18
ms.openlocfilehash: cade33e77eb0d3ddd818a6ce3dbd7c6cf72811d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74037417"
---
# <a name="configure-route-filters-for-microsoft-peering-powershell"></a>Microsoft'un eşleneme için rota filtrelerini yapılandırma: PowerShell
> [!div class="op_single_selector"]
> * [Azure Portalı](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure CLI](how-to-routefilter-cli.md)
> 

Rota filtreleri, desteklenen servislerin bir alt kümesini Microsoft eşlemesi aracılığıyla kullanmanın bir yoludur. Bu makaledeki adımlar, ExpressRoute devreleri için rota filtrelerini yapılandırmanıza ve yönetmenize yardımcı olur.

Exchange Online, SharePoint Online ve Skype for Business gibi Office 365 hizmetleri ve depolama ve SQL DB gibi Azure kamu hizmetlerine Microsoft'un bakışları aracılığıyla erişilebilir. Azure genel hizmetleri bölge bazında seçilebilir ve kamu hizmeti başına tanımlanamaz.

Microsoft eşleme bir ExpressRoute devresi üzerinde yapılandırıldığında ve bir rota filtresi eklendiğinde, bu hizmetler için seçilen tüm öneekler oluşturulan BGP oturumları aracılığıyla duyurulur. Ön ek aracılığıyla sunulan hizmeti tanımlamak için her ön eke BGP topluluk değeri eklenir. BGP topluluk değerlerinin ve eşledikleri hizmetlerin listesi için [Bkz. BGP toplulukları.](expressroute-routing.md#bgp)

Tüm hizmetlere bağlantı gerektirirseniz, BGP üzerinden çok sayıda önek ilan edilir. Bu, ağınızdaki yönlendiriciler tarafından tutulan rota tablolarının boyutunu önemli ölçüde artırır. Microsoft'un bakışları aracılığıyla sunulan hizmetlerin yalnızca bir alt kümesini tüketmeyi planlıyorsanız, rota tablolarınızın boyutunu iki şekilde küçültebilirsiniz. Şunları yapabilirsiniz:

- BGP topluluklarına rota filtreleri uygulayarak istenmeyen önekleri filtreleyin. Bu standart bir ağ uygulamasıdır ve birçok ağda yaygın olarak kullanılır.

- Rota filtrelerini tanımlayın ve ExpressRoute devrenize uygulayın. Rota filtresi, Microsoft'un bakışları aracılığıyla tüketmeyi planladığınız hizmetlerin listesini seçmenize olanak tanıyan yeni bir kaynaktır. ExpressRoute yönlendiricileri yalnızca rota filtresinde tanımlanan hizmetlere ait öneklistesini gönderir.

### <a name="about-route-filters"></a><a name="about"></a>Rota filtreleri hakkında

Microsoft'un bakışları ExpressRoute devrenizde yapılandırıldığında, Microsoft ağ kenarı yönlendiricileri kenar yönlendiricileri (sizin veya bağlantı sağlayıcınızın) bir çift BGP oturumu kurar. Ağınıza rota tanıtılmaz. Ağınızda rota tanıtılmasını sağlamak için bir rota filtresiyle ilişkilendirmeniz gerekir.

Rota filtresi, ExpressRoute bağlantı hattınızın Microsoft eşlemesi üzerinden kullanmak istediğiniz hizmetleri tanımlamanızı sağlar. Aslında tüm BGP topluluk değerlerinin izin listesidir. Rota filtresi kaynağı tanımlandıktan ve bir ExpressRoute bağlantı hattına eklendikten sonra BGP topluluk değerleriyle eşleşen tüm ön ekler ağınızda tanıtılır.

Üzerlerinde Office 365 hizmetleri olan rota filtreleri ekleyebilmek için ExpressRoute üzerinden Office 365 hizmetlerini kullanma yetkiniz olması gerekir. ExpressRoute üzerinden Office 365 hizmetlerini tüketme yetkiniz yoksa, rota filtrelerini ekleme işlemi başarısız olur. Yetkilendirme işlemi hakkında daha fazla bilgi için [Office 365 için Azure ExpressRoute'a](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)bakın.

> [!IMPORTANT]
> 1 Ağustos 2017 tarihinden önce yapılandırılan ExpressRoute devrelerinin Microsoft'a bakması, rota filtreleri tanımlanmamış olsa bile, Microsoft'un bakışları aracılığıyla duyurulan tüm hizmet önekleri olacaktır. Microsoft' un 1 Ağustos 2017 tarihinde veya sonrasında yapılandırılan ExpressRoute devrelerine bakması, devreye bir rota filtresi iliştirilene kadar herhangi bir önek reklamı yapmayacaktır.
> 
> 

### <a name="workflow"></a><a name="workflow"></a>Iş akışı

Microsoft'un bakışları aracılığıyla hizmetlere başarılı bir şekilde bağlanabilmek için aşağıdaki yapılandırma adımlarını tamamlamanız gerekir:

- Microsoft'a bakan etkin leştirilmiş bir ExpressRoute devreniz olmalıdır. Bu görevleri gerçekleştirmek için aşağıdaki yönergeleri kullanabilirsiniz:
  - [Bir ExpressRoute devresi oluşturun](expressroute-howto-circuit-arm.md) ve devam etmeden önce devreyi bağlantı sağlayıcınız tarafından etkinleştirin. ExpressRoute devresi, hüküm verilen ve etkin leştirilmiş bir durumda olmalıdır.
  - BGP oturumunu doğrudan yönetiyorsanız [Microsoft'un bakışını oluşturun.](expressroute-circuit-peerings.md) Veya, bağlantı sağlayıcınızın Microsoft'un devrenize bakmasına izin ver.

-  Bir rota filtresi oluşturmalı ve yapılandırmanız gerekir.
    - Microsoft'un bakışları aracılığıyla tüketmek üzere olduğunuz hizmetleri tanımlayın
    - Hizmetlerle ilişkili BGP topluluk değerlerinin listesini belirleme
    - BGP topluluk değerleriyle eşleşen önek listesine izin vermek için bir kural oluşturma

-  Rota filtresini ExpressRoute devresine takmanız gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki ölçütleri karşıladığınızdan emin olun:

 - Yapılandırmaya başlamadan önce [ön koşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

 - Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. ExpressRoute devresi, hüküm verilen ve etkin leştirilmiş bir durumda olmalıdır.

 - Etkin bir Microsoft bakışa sahip olmalısınız. [Örnek oluşturma ve eşleme yapılandırması](expressroute-circuit-peerings.md) makalesindeki yönergeleri izleyin.


### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

### <a name="log-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Bu yapılandırmaya başlamadan önce Azure hesabınızda oturum açmalısınız. Bu cmdlet, Azure hesabınıza ilişkin oturum açma kimlik bilgilerinizi girmenizi ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir.

PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olmak için aşağıdaki örneği kullanın. Azure Bulut Su Şur'u kullanıyorsanız, otomatik olarak oturum açtığınız için bu cmdlet'i çalıştırmanız gerekmez.

```azurepowershell
Connect-AzAccount
```

Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

```azurepowershell-interactive
Get-AzSubscription
```

Kullanmak istediğiniz aboneliği belirtin.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="step-1-get-a-list-of-prefixes-and-bgp-community-values"></a><a name="prefixes"></a>Adım 1: Öneklerin ve BGP topluluk değerlerinin bir listesini alın

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP topluluk değerlerinin bir listesini alın

Microsoft'un bakışları aracılığıyla erişilebilen hizmetlerle ilişkili BGP topluluk değerlerinin listesini ve bunlarla ilişkili öneklistesini almak için aşağıdaki cmdlet'i kullanın:

```azurepowershell-interactive
Get-AzBgpServiceCommunity
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Kullanmak istediğiniz değerlerin bir listesini yapın

Rota filtresinde kullanmak istediğiniz BGP topluluk değerlerinin bir listesini yapın.

## <a name="step-2-create-a-route-filter-and-a-filter-rule"></a><a name="filter"></a>Adım 2: Rota filtresi ve filtre kuralı oluşturma

Bir rota filtresinin yalnızca bir kuralı olabilir ve kural 'İzin Ver' türünde olmalıdır. Bu kural, onunla ilişkili BGP topluluk değerlerinin bir listesi olabilir.

### <a name="1-create-a-route-filter"></a>1. Rota filtresi oluşturma

İlk olarak, rota filtresioluşturun. 'New-AzRouteFilter' komutu yalnızca bir rota filtresi kaynağı oluşturur. Kaynağı oluşturduktan sonra bir kural oluşturmanız ve rota filtresi nesnesine eklemeniz gerekir. Bir rota filtresi kaynağı oluşturmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
New-AzRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a>2. Filtre kuralı oluşturma

Örnekte gösterildiği gibi, virgülle ayrılmış bir liste olarak bgp toplulukları kümesini belirtebilirsiniz. Yeni bir kural oluşturmak için aşağıdaki komutu çalıştırın:
 
```azurepowershell-interactive
$rule = New-AzRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList 12076:5010,12076:5040
```

### <a name="3-add-the-rule-to-the-route-filter"></a>3. Kuralı rota filtresine ekleme

Filtre kuralını rota filtresine eklemek için aşağıdaki komutu çalıştırın:
 
```azurepowershell-interactive
$routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzRouteFilter -RouteFilter $routefilter
```

## <a name="step-3-attach-the-route-filter-to-an-expressroute-circuit"></a><a name="attach"></a>Adım 3: Rota filtresini ExpressRoute devresine takın

Yalnızca Microsoft'un eşlediğinizi varsayarak rota filtresini ExpressRoute devresine eklemek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="common-tasks"></a><a name="tasks"></a>Genel görevler

### <a name="to-get-the-properties-of-a-route-filter"></a><a name="getproperties"></a>Rota filtresi özelliklerini almak için

Bir rota filtresinin özelliklerini almak için aşağıdaki adımları kullanın:

1. Rota filtresi kaynağını almak için aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   $routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
   ```
2. Aşağıdaki komutu çalıştırarak rota filtresi kaynağının rota filtresi kurallarını alın:

   ```azurepowershell-interactive
   $routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
   $rule = $routefilter.Rules[0]
   ```

### <a name="to-update-the-properties-of-a-route-filter"></a><a name="updateproperties"></a>Rota filtresinin özelliklerini güncelleştirmek için

Rota filtresi zaten bir devreye bağlıysa, BGP topluluk listesindeki güncelleştirmeler, kurulan BGP oturumları aracılığıyla uygun önek reklam değişikliklerini otomatik olarak yayıltın. Aşağıdaki komutu kullanarak rota filtrenizin BGP topluluk listesini güncelleştirebilirsiniz:

```azurepowershell-interactive
$routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzRouteFilter -RouteFilter $routefilter
```

### <a name="to-detach-a-route-filter-from-an-expressroute-circuit"></a><a name="detach"></a>Bir Route filtresini ExpressRoute devresinden ayırmak için

Bir rota filtresi ExpressRoute devresinden ayrıldıktan sonra, BGP oturumu aracılığıyla önek ilan edilemez. Aşağıdaki komutu kullanarak bir ExpressRoute devresinden bir rota filtresini ayırabilirsiniz:
  
```azurepowershell-interactive
$ckt.Peerings[0].RouteFilter = $null
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-a-route-filter"></a><a name="delete"></a>Rota filtresini silmek için

Bir rota filtresini yalnızca herhangi bir devreye bağlı değilse silebilirsiniz. Rota filtresini silmeye çalışmadan önce herhangi bir devreye bağlı olmadığından emin olun. Aşağıdaki komutu kullanarak bir rota filtresini silebilirsiniz:

```azurepowershell-interactive
Remove-AzRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a>Sonraki Adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
