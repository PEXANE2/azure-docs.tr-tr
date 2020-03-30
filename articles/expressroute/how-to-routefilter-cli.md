---
title: 'ExpressRoute: Rota filtreleri - Microsoft peering:Azure CLI'
description: Bu makalede, Azure CLI kullanarak Microsoft Peering için rota filtrelerinin nasıl yapılandırılabildiğini açıklanmaktadır
services: expressroute
author: anzaman
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: anzaman
ms.openlocfilehash: c3c50a005e119890fb17fcf7b3114a747bbe34bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033408"
---
# <a name="configure-route-filters-for-microsoft-peering-azure-cli"></a>Microsoft'un eşlemesi için rota filtrelerini yapılandırma: Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portalı](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure CLI](how-to-routefilter-cli.md)
> 

Rota filtreleri, desteklenen servislerin bir alt kümesini Microsoft eşlemesi aracılığıyla kullanmanın bir yoludur. Bu makaledeki adımlar, ExpressRoute devreleri için rota filtrelerini yapılandırmanıza ve yönetmenize yardımcı olur.

Exchange Online, SharePoint Online ve Skype for Business gibi Office 365 hizmetlerine Microsoft'un bakışları aracılığıyla erişilebilir. Microsoft eşleme bir ExpressRoute devresinde yapılandırıldığında, bu hizmetlerle ilgili tüm öneekler oluşturulan BGP oturumları aracılığıyla duyurulur. Ön ek aracılığıyla sunulan hizmeti tanımlamak için her ön eke BGP topluluk değeri eklenir. BGP topluluk değerlerinin ve eşledikleri hizmetlerin listesi için [Bkz. BGP toplulukları.](expressroute-routing.md#bgp)

Tüm hizmetlere bağlantı gerektirirseniz, BGP üzerinden çok sayıda önek ilan edilir. Bu, ağınızdaki yönlendiriciler tarafından tutulan rota tablolarının boyutunu önemli ölçüde artırır. Microsoft'un bakışları aracılığıyla sunulan hizmetlerin yalnızca bir alt kümesini tüketmeyi planlıyorsanız, rota tablolarınızın boyutunu iki şekilde küçültebilirsiniz. Şunları yapabilirsiniz:

* BGP topluluklarına rota filtreleri uygulayarak istenmeyen önekleri filtreleyin. Bu standart bir ağ uygulamasıdır ve birçok ağda yaygın olarak kullanılır.

* Rota filtrelerini tanımlayın ve ExpressRoute devrenize uygulayın. Rota filtresi, Microsoft'un bakışları aracılığıyla tüketmeyi planladığınız hizmetlerin listesini seçmenize olanak tanıyan yeni bir kaynaktır. ExpressRoute yönlendiricileri yalnızca rota filtresinde tanımlanan hizmetlere ait öneklistesini gönderir.

### <a name="about-route-filters"></a><a name="about"></a>Rota filtreleri hakkında

Microsoft'un bakışları ExpressRoute devrenizde yapılandırıldığında, Microsoft kenar yönlendiricileri kenar yönlendiricileri (sizin veya bağlantı sağlayıcınızın) bir çift BGP oturumu kurar. Ağınıza rota tanıtılmaz. Ağınızda rota tanıtılmasını sağlamak için bir rota filtresiyle ilişkilendirmeniz gerekir.

Rota filtresi, ExpressRoute bağlantı hattınızın Microsoft eşlemesi üzerinden kullanmak istediğiniz hizmetleri tanımlamanızı sağlar. Bu genelde tüm BGP topluluk değerlerinden oluşan bir izin verilenler listesidir. Rota filtresi kaynağı tanımlandıktan ve bir ExpressRoute bağlantı hattına eklendikten sonra BGP topluluk değerleriyle eşleşen tüm ön ekler ağınızda tanıtılır.

Üzerlerinde Office 365 hizmetleri olan rota filtreleri ekleyebilmek için ExpressRoute üzerinden Office 365 hizmetlerini kullanma yetkiniz olması gerekir. ExpressRoute üzerinden Office 365 hizmetlerini tüketme yetkiniz yoksa, rota filtrelerini ekleme işlemi başarısız olur. Yetkilendirme işlemi hakkında daha fazla bilgi için [Office 365 için Azure ExpressRoute'a](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)bakın.

> [!IMPORTANT]
> 1 Ağustos 2017 tarihinden önce yapılandırılan ExpressRoute devrelerinin Microsoft'a bakması, rota filtreleri tanımlanmamış olsa bile, Microsoft'un bakışları aracılığıyla duyurulan tüm hizmet önekleri olacaktır. Microsoft' un 1 Ağustos 2017 tarihinde veya sonrasında yapılandırılan ExpressRoute devrelerine bakması, devreye bir rota filtresi iliştirilene kadar herhangi bir önek reklamı yapmayacaktır.
> 
> 

### <a name="workflow"></a><a name="workflow"></a>Iş akışı

Microsoft'un bakışları aracılığıyla hizmetlere başarılı bir şekilde bağlanabilmek için aşağıdaki yapılandırma adımlarını tamamlamanız gerekir:

* Microsoft'a bakan etkin leştirilmiş bir ExpressRoute devreniz olmalıdır. Bu görevleri gerçekleştirmek için aşağıdaki yönergeleri kullanabilirsiniz:
  * [Bir ExpressRoute devresi oluşturun](howto-circuit-cli.md) ve devam etmeden önce devreyi bağlantı sağlayıcınız tarafından etkinleştirin. ExpressRoute devresi, hüküm verilen ve etkin leştirilmiş bir durumda olmalıdır.
  * BGP oturumunu doğrudan yönetiyorsanız [Microsoft'un bakışını oluşturun.](howto-routing-cli.md) Veya, bağlantı sağlayıcınızın Microsoft'un devrenize bakmasına izin ver.

* Bir rota filtresi oluşturmalı ve yapılandırmanız gerekir.
  * Microsoft'un bakışları aracılığıyla tüketmek üzere olduğunuz hizmetleri tanımlayın
  * Hizmetlerle ilişkili BGP topluluk değerlerinin listesini belirleme
  * BGP topluluk değerleriyle eşleşen önek listesine izin vermek için bir kural oluşturma

* Rota filtresini ExpressRoute devresine takmanız gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce, CLI komutlarının en son sürümünü (2.0 veya üzeri) yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli) ve [Azure CLI’yi Kullanmaya Başlama](/cli/azure/get-started-with-azure-cli).

* Yapılandırmaya başlamadan önce [ön koşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](howto-circuit-cli.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. ExpressRoute devresi, hüküm verilen ve etkin leştirilmiş bir durumda olmalıdır.

* Etkin bir Microsoft bakışa sahip olmalısınız. [Oluşturma ve eşleme yapılandırması değiştirme](howto-routing-cli.md) yönergeleriizleyin

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin

Yapılandırmanıza başlamak için Azure hesabınızda oturum açın. "Try It" sözcüğündeki "Try It"i kullanıyorsanız, otomatik olarak oturum açarsınız ve giriş adımını atlayabilirsiniz. Bağlanmanıza yardımcı olmak için aşağıdaki örnekleri kullanın:

```azurecli
az login
```

Hesapla ilişkili abonelikleri kontrol edin.

```azurecli-interactive
az account list
```

ExpressRoute devresi oluşturmak istediğiniz aboneliği seçin.

```azurecli-interactive
az account set --subscription "<subscription ID>"
```

## <a name="step-1-get-a-list-of-prefixes-and-bgp-community-values"></a><a name="prefixes"></a>Adım 1: Öneklerin ve BGP topluluk değerlerinin bir listesini alın

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP topluluk değerlerinin bir listesini alın

Microsoft'un bakışları aracılığıyla erişilebilen hizmetlerle ilişkili BGP topluluk değerlerinin listesini ve bunlarla ilişkili öneklistesini almak için aşağıdaki cmdlet'i kullanın:

```azurecli-interactive
az network route-filter rule list-service-communities
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Kullanmak istediğiniz değerlerin bir listesini yapın

Rota filtresinde kullanmak istediğiniz BGP topluluk değerlerinin bir listesini yapın.

## <a name="step-2-create-a-route-filter-and-a-filter-rule"></a><a name="filter"></a>Adım 2: Rota filtresi ve filtre kuralı oluşturma

Bir rota filtresinin yalnızca bir kuralı olabilir ve kural 'İzin Ver' türünde olmalıdır. Bu kural, onunla ilişkili BGP topluluk değerlerinin bir listesi olabilir.

### <a name="1-create-a-route-filter"></a>1. Rota filtresi oluşturma

İlk olarak, rota filtresioluşturun. Komut `az network route-filter create` yalnızca bir rota filtresi kaynağı oluşturur. Kaynağı oluşturduktan sonra bir kural oluşturmanız ve rota filtresi nesnesine eklemeniz gerekir. Bir rota filtresi kaynağı oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az network route-filter create -n MyRouteFilter -g MyResourceGroup
```

### <a name="2-create-a-filter-rule"></a>2. Filtre kuralı oluşturma

Yeni bir kural oluşturmak için aşağıdaki komutu çalıştırın:
 
```azurecli-interactive
az network route-filter rule create --filter-name MyRouteFilter -n CRM --communities 12076:5040 --access Allow -g MyResourceGroup
```

## <a name="step-3-attach-the-route-filter-to-an-expressroute-circuit"></a><a name="attach"></a>Adım 3: Rota filtresini ExpressRoute devresine takın

Rota filtresini ExpressRoute devresine takmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroupName --name MicrosoftPeering --route-filter MyRouteFilter
```

## <a name="common-tasks"></a><a name="tasks"></a>Genel görevler

### <a name="to-get-the-properties-of-a-route-filter"></a><a name="getproperties"></a>Rota filtresi özelliklerini almak için

Bir rota filtresinin özelliklerini almak için aşağıdaki komutu kullanın:

```azurecli-interactive
az network route-filter show -g ExpressRouteResourceGroupName --name MyRouteFilter 
```

### <a name="to-update-the-properties-of-a-route-filter"></a><a name="updateproperties"></a>Rota filtresinin özelliklerini güncelleştirmek için

Rota filtresi zaten bir devreye bağlıysa, BGP topluluk listesindeki güncelleştirmeler, kurulan BGP oturumları aracılığıyla uygun önek reklam değişikliklerini otomatik olarak yayıltın. Aşağıdaki komutu kullanarak rota filtrenizin BGP topluluk listesini güncelleştirebilirsiniz:

```azurecli-interactive
az network route-filter rule update --filter-name MyRouteFilter -n CRM -g ExpressRouteResourceGroupName --add communities '12076:5040' --add communities '12076:5010'
```

### <a name="to-detach-a-route-filter-from-an-expressroute-circuit"></a><a name="detach"></a>Bir Route filtresini ExpressRoute devresinden ayırmak için

Bir rota filtresi ExpressRoute devresinden ayrıldıktan sonra, BGP oturumu aracılığıyla önek ilan edilemez. Aşağıdaki komutu kullanarak bir ExpressRoute devresinden bir rota filtresini ayırabilirsiniz:

```azurecli-interactive
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroupName --name MicrosoftPeering --remove routeFilter
```

### <a name="to-delete-a-route-filter"></a><a name="delete"></a>Rota filtresini silmek için

Bir rota filtresini yalnızca herhangi bir devreye bağlı değilse silebilirsiniz. Rota filtresini silmeye çalışmadan önce herhangi bir devreye bağlı olmadığından emin olun. Aşağıdaki komutu kullanarak bir rota filtresini silebilirsiniz:

```azurecli-interactive
az network route-filter delete -n MyRouteFilter -g MyResourceGroup
```

## <a name="next-steps"></a>Sonraki Adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
