---
title: 'ExpressRoute: yönlendirme filtreleri-Microsoft eşlemesi: Azure PowerShell'
description: Bu makalede, PowerShell kullanarak Microsoft eşlemesi için yol filtrelerinin nasıl yapılandırılacağı açıklanır.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: how-to
ms.date: 02/25/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: f5a294a051350c4b08b34356abcd883b7580164e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84729311"
---
# <a name="configure-route-filters-for-microsoft-peering-powershell"></a>Microsoft eşlemesi için rota filtrelerini yapılandırma: PowerShell
> [!div class="op_single_selector"]
> * [Azure Portalı](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure CLI](how-to-routefilter-cli.md)
> 

Rota filtreleri, desteklenen servislerin bir alt kümesini Microsoft eşlemesi aracılığıyla kullanmanın bir yoludur. Bu makaledeki adımlar, ExpressRoute devreleri için yol filtrelerini yapılandırmanıza ve yönetmenize yardımcı olur.

Exchange Online, SharePoint Online ve Skype Kurumsal gibi Office 365 hizmetleri ve depolama ve SQL DB gibi Azure genel hizmetlerine Microsoft eşlemesi aracılığıyla erişilebilir. Azure genel hizmetleri bölge başına seçilebilir ve genel hizmet başına tanımlanamaz.

ExpressRoute devresi üzerinde Microsoft eşlemesi yapılandırıldığında ve bir yol filtresi eklendiğinde, bu hizmetler için seçilen tüm önekler oluşturulan BGP oturumları aracılığıyla bildirilir. Ön ek aracılığıyla sunulan hizmeti tanımlamak için her ön eke BGP topluluk değeri eklenir. BGP topluluk değerlerinin ve eşlendikleri hizmetlerin listesi için bkz. [BGP toplulukları](expressroute-routing.md#bgp).

Tüm hizmetlere bağlantı gerekiyorsa, BGP aracılığıyla çok sayıda önek tanıtılabilir. Bu, ağınızdaki yönlendiriciler tarafından tutulan yol tablolarının boyutunu önemli ölçüde artırır. Yalnızca Microsoft eşlemesi üzerinden sunulan hizmetlerin bir alt kümesini kullanmak istiyorsanız, yol tablolarınızın boyutunu iki şekilde azaltabilirsiniz. Seçenekleriniz şunlardır:

- BGP topluluklarına rota filtreleri uygulayarak istenmeyen önekleri filtreleyin. Bu standart bir ağ uygulamasıdır ve genellikle birçok ağ içinde kullanılır.

- Rota filtrelerini tanımlayın ve bunları ExpressRoute devrenizi ile uygulayın. Yol filtresi, Microsoft eşlemesi aracılığıyla tüketmek üzere planladığınız hizmetlerin listesini seçmenize olanak sağlayan yeni bir kaynaktır. ExpressRoute yönlendiricileri yalnızca yol filtresinde tanımlanan hizmetlere ait olan ön eklerin listesini gönderir.

### <a name="about-route-filters"></a><a name="about"></a>Yol filtreleri hakkında

ExpressRoute bağlantı hattınızda Microsoft eşlemesi yapılandırıldığında, Microsoft ağ uç yönlendiricileri, uç yönlendiricilerle (sizinki veya bağlantı sağlayıcınız) bir çift BGP oturumu oluşturur. Ağınıza rota tanıtılmaz. Ağınızda rota tanıtılmasını sağlamak için bir rota filtresiyle ilişkilendirmeniz gerekir.

Rota filtresi, ExpressRoute bağlantı hattınızın Microsoft eşlemesi üzerinden kullanmak istediğiniz hizmetleri tanımlamanızı sağlar. Bu aslında tüm BGP topluluk değerlerinin izin verilenler listesidir. Rota filtresi kaynağı tanımlandıktan ve bir ExpressRoute bağlantı hattına eklendikten sonra BGP topluluk değerleriyle eşleşen tüm ön ekler ağınızda tanıtılır.

Office 365 hizmetleriyle yönlendirme filtrelerini eklemek için ExpressRoute aracılığıyla Office 365 hizmetlerini kullanma yetkisine sahip olmanız gerekir. ExpressRoute aracılığıyla Office 365 hizmetlerini kullanma yetkiniz yoksa, yol filtrelerini iliştirme işlemi başarısız olur. Yetkilendirme işlemi hakkında daha fazla bilgi için bkz. [Office Için Azure ExpressRoute 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).

> [!IMPORTANT]
> 1 Ağustos 2017 ' den önce yapılandırılmış ExpressRoute bağlantı hattı Microsoft eşlemesi, yol filtreleri tanımlanmasa bile Microsoft eşlemesi aracılığıyla tanıtılan tüm hizmet öneklerini alacak. 1 Ağustos 2017 ' de veya sonrasında yapılandırılan ExpressRoute devrelerinin Microsoft eşlemesi, bir yol filtresi devresine iliştirilene kadar tanıtılan öneklere sahip olmayacaktır.
> 
> 

### <a name="workflow"></a><a name="workflow"></a>Akışıyla

Microsoft eşlemesi aracılığıyla hizmetlere başarıyla bağlanabiliyor olması için aşağıdaki yapılandırma adımlarını gerçekleştirmeniz gerekir:

- Microsoft eşlemesi sağlanmış etkin bir ExpressRoute devresine sahip olmanız gerekir. Aşağıdaki yönergeleri kullanarak şu görevleri gerçekleştirebilirsiniz:
  - Devam etmeden önce [bir ExpressRoute devresi oluşturun](expressroute-howto-circuit-arm.md) ve bağlantı sağlayıcınız tarafından devre dışı bırakıldı. ExpressRoute bağlantı hattı sağlanmış ve etkinleştirilmiş durumda olmalıdır.
  - BGP oturumunu doğrudan yönetiyorsanız, [Microsoft eşlemesi oluşturun](expressroute-circuit-peerings.md) . Ya da bağlantı sağlayıcınızın, devreniz için Microsoft eşlemesi sağlamasını sağlayın.

-  Bir yol filtresi oluşturmanız ve yapılandırmanız gerekir.
    - Microsoft eşlemesi aracılığıyla kullanmak için kullandığınız Hizmetleri tanımla
    - Hizmetlerle ilişkili BGP topluluk değerlerinin listesini belirler
    - BGP topluluk değerleriyle eşleşen ön ek listesine izin veren bir kural oluşturma

-  Yol filtresini ExpressRoute devresine bağlamanız gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki ölçütleri karşıladığınızdan emin olun:

 - Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

 - Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. ExpressRoute bağlantı hattı sağlanmış ve etkinleştirilmiş durumda olmalıdır.

 - Etkin bir Microsoft eşlemesine sahip olmanız gerekir. [Eşleme yapılandırması oluşturma ve değiştirme](expressroute-circuit-peerings.md) makalesindeki yönergeleri izleyin.


### <a name="working-with-azure-powershell"></a>Azure PowerShell çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

### <a name="log-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Bu yapılandırmaya başlamadan önce Azure hesabınızda oturum açmalısınız. Bu cmdlet, Azure hesabınıza ilişkin oturum açma kimlik bilgilerinizi girmenizi ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir.

PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın. Azure Cloud Shell kullanıyorsanız, otomatik olarak oturum açabileceksiniz, bu cmdlet 'i çalıştırmanız gerekmez.

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

## <a name="step-1-get-a-list-of-prefixes-and-bgp-community-values"></a><a name="prefixes"></a>1. Adım: ön eklerin ve BGP topluluk değerlerinin listesini alın

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP topluluk değerlerinin bir listesini alın

Microsoft eşlemesi aracılığıyla erişilebilen hizmetlerle ilişkili BGP topluluk değerlerinin listesini ve bunlarla ilişkili ön eklerin listesini almak için aşağıdaki cmdlet 'i kullanın:

```azurepowershell-interactive
Get-AzBgpServiceCommunity
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. kullanmak istediğiniz değerlerin bir listesini oluşturun

Yol filtresinde kullanmak istediğiniz BGP topluluk değerlerinin bir listesini oluşturun.

## <a name="step-2-create-a-route-filter-and-a-filter-rule"></a><a name="filter"></a>2. Adım: bir yol filtresi ve bir filtre kuralı oluşturma

Yol filtresi yalnızca bir kurala sahip olabilir ve kuralın ' Allow ' türünde olması gerekir. Bu kural, kendisiyle ilişkili BGP topluluk değerlerinin bir listesine sahip olabilir.

### <a name="1-create-a-route-filter"></a>1. yol filtresi oluşturma

İlk olarak, yol filtresini oluşturun. ' New-AzRouteFilter ' komutu yalnızca bir yol filtre kaynağı oluşturuyor. Kaynağı oluşturduktan sonra bir kural oluşturmanız ve bunu yol filtresi nesnesine bağlamanız gerekir. Bir yol filtresi kaynağı oluşturmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
New-AzRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a>2. filtre kuralı oluşturma

Bir BGP toplulukları kümesini, örnekte gösterildiği gibi, virgülle ayrılmış bir liste olarak belirtebilirsiniz. Yeni bir kural oluşturmak için aşağıdaki komutu çalıştırın:
 
```azurepowershell-interactive
$rule = New-AzRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList 12076:5010,12076:5040
```

### <a name="3-add-the-rule-to-the-route-filter"></a>3. kuralı rota filtresine ekleyin

Filtre kuralını rota filtresine eklemek için aşağıdaki komutu çalıştırın:
 
```azurepowershell-interactive
$routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzRouteFilter -RouteFilter $routefilter
```

## <a name="step-3-attach-the-route-filter-to-an-expressroute-circuit"></a><a name="attach"></a>3. Adım: yol filtresini bir ExpressRoute devresine Iliştirme

Yönlendirme filtresini ExpressRoute bağlantı hattına eklemek için aşağıdaki komutu çalıştırın ve yalnızca Microsoft 'un eşlemesini kullanabilirsiniz:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="common-tasks"></a><a name="tasks"></a>Genel görevler

### <a name="to-get-the-properties-of-a-route-filter"></a><a name="getproperties"></a>Bir yol filtresinin özelliklerini almak için

Bir yol filtresinin özelliklerini almak için aşağıdaki adımları kullanın:

1. Yol filtresi kaynağını almak için aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   $routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
   ```
2. Aşağıdaki komutu çalıştırarak yol filtresi kaynağı için yol filtresi kurallarını alın:

   ```azurepowershell-interactive
   $routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
   $rule = $routefilter.Rules[0]
   ```

### <a name="to-update-the-properties-of-a-route-filter"></a><a name="updateproperties"></a>Bir yol filtresinin özelliklerini güncelleştirmek için

Yol filtresi zaten bir bağlantı hattına eklenmişse, BGP topluluk listesi güncelleştirmeleri otomatik olarak, belirlenen BGP oturumlarıyla ilgili önek tanıtım değişikliklerini yayar. Aşağıdaki komutu kullanarak yol filtrenizin BGP topluluk listesini güncelleştirebilirsiniz:

```azurepowershell-interactive
$routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzRouteFilter -RouteFilter $routefilter
```

### <a name="to-detach-a-route-filter-from-an-expressroute-circuit"></a><a name="detach"></a>Bir ExpressRoute bağlantı hattından yol filtresini ayırmak için

Yol filtresi ExpressRoute bağlantı hattı 'ndan ayrıldıktan sonra BGP oturumu aracılığıyla hiçbir önek tanıtılmaz. Aşağıdaki komutu kullanarak bir ExpressRoute bağlantı hattından bir yol filtresini ayırabilirsiniz:
  
```azurepowershell-interactive
$ckt.Peerings[0].RouteFilter = $null
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-a-route-filter"></a><a name="delete"></a>Yol filtresini silmek için

Bir yol filtresini yalnızca herhangi bir devreye iliştirilmişse silebilirsiniz. Yol filtresinin silmeyi denemeden önce herhangi bir devreye bağlı olmadığından emin olun. Aşağıdaki komutu kullanarak bir yol filtresini silebilirsiniz:

```azurepowershell-interactive
Remove-AzRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a>Sonraki Adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
