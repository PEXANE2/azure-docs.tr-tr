---
title: 'ExpressRoute: yönlendirme filtreleri-Microsoft eşlemesi: Azure CLı'
description: Bu makalede, Azure CLı kullanarak Microsoft eşlemesi için yol filtrelerinin nasıl yapılandırılacağı açıklanır.
services: expressroute
author: kumudD
ms.service: expressroute
ms.topic: how-to
ms.date: 12/07/2018
ms.author: kumud
ms.openlocfilehash: 43a3e7672a6bfdf89bce67ec54e4500cd84b6dcf
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86521641"
---
# <a name="configure-route-filters-for-microsoft-peering-azure-cli"></a>Microsoft eşlemesi için rota filtrelerini yapılandırma: Azure CLı

> [!div class="op_single_selector"]
> * [Azure Portalı](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure CLI](how-to-routefilter-cli.md)
> 

Rota filtreleri, desteklenen servislerin bir alt kümesini Microsoft eşlemesi aracılığıyla kullanmanın bir yoludur. Bu makaledeki adımlar, ExpressRoute devreleri için yol filtrelerini yapılandırmanıza ve yönetmenize yardımcı olur.

Exchange Online, SharePoint Online ve Skype Kurumsal gibi Office 365 hizmetlerine Microsoft eşlemesi aracılığıyla erişilebilir. ExpressRoute bağlantı hattındaki Microsoft eşlemesi yapılandırıldığında, bu hizmetlerle ilgili tüm önekler oluşturulan BGP oturumları aracılığıyla bildirilir. Ön ek aracılığıyla sunulan hizmeti tanımlamak için her ön eke BGP topluluk değeri eklenir. BGP topluluk değerlerinin ve eşlendikleri hizmetlerin listesi için bkz. [BGP toplulukları](expressroute-routing.md#bgp).

Tüm hizmetlere bağlantı gerekiyorsa, BGP aracılığıyla çok sayıda önek tanıtılabilir. Bu, ağınızdaki yönlendiriciler tarafından tutulan yol tablolarının boyutunu önemli ölçüde artırır. Yalnızca Microsoft eşlemesi üzerinden sunulan hizmetlerin bir alt kümesini kullanmak istiyorsanız, yol tablolarınızın boyutunu iki şekilde azaltabilirsiniz. Şunları yapabilirsiniz:

* BGP topluluklarına rota filtreleri uygulayarak istenmeyen önekleri filtreleyin. Bu standart bir ağ uygulamasıdır ve genellikle birçok ağ içinde kullanılır.

* Rota filtrelerini tanımlayın ve bunları ExpressRoute devrenizi ile uygulayın. Yol filtresi, Microsoft eşlemesi aracılığıyla tüketmek üzere planladığınız hizmetlerin listesini seçmenize olanak sağlayan yeni bir kaynaktır. ExpressRoute yönlendiricileri yalnızca yol filtresinde tanımlanan hizmetlere ait olan ön eklerin listesini gönderir.

### <a name="about-route-filters"></a><a name="about"></a>Yol filtreleri hakkında

ExpressRoute bağlantı hattınızda Microsoft eşlemesi yapılandırıldığında, Microsoft Edge yönlendiricileri, uç yönlendiricilerle (sizinki veya bağlantı sağlayıcınız) bir çift BGP oturumu oluşturur. Ağınıza rota tanıtılmaz. Ağınızda rota tanıtılmasını sağlamak için bir rota filtresiyle ilişkilendirmeniz gerekir.

Rota filtresi, ExpressRoute bağlantı hattınızın Microsoft eşlemesi üzerinden kullanmak istediğiniz hizmetleri tanımlamanızı sağlar. Bu genelde tüm BGP topluluk değerlerinden oluşan bir izin verilenler listesidir. Rota filtresi kaynağı tanımlandıktan ve bir ExpressRoute bağlantı hattına eklendikten sonra BGP topluluk değerleriyle eşleşen tüm ön ekler ağınızda tanıtılır.

Office 365 hizmetleriyle yönlendirme filtrelerini eklemek için ExpressRoute aracılığıyla Office 365 hizmetlerini kullanma yetkisine sahip olmanız gerekir. ExpressRoute aracılığıyla Office 365 hizmetlerini kullanma yetkiniz yoksa, yol filtrelerini iliştirme işlemi başarısız olur. Yetkilendirme işlemi hakkında daha fazla bilgi için bkz. [Office Için Azure ExpressRoute 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).

> [!IMPORTANT]
> 1 Ağustos 2017 ' den önce yapılandırılmış ExpressRoute bağlantı hattı Microsoft eşlemesi, yol filtreleri tanımlanmasa bile Microsoft eşlemesi aracılığıyla tanıtılan tüm hizmet öneklerini alacak. 1 Ağustos 2017 ' de veya sonrasında yapılandırılan ExpressRoute devrelerinin Microsoft eşlemesi, bir yol filtresi devresine iliştirilene kadar tanıtılan öneklere sahip olmayacaktır.
> 
> 

### <a name="workflow"></a><a name="workflow"></a>Akışıyla

Microsoft eşlemesi aracılığıyla hizmetlere başarıyla bağlanabiliyor olması için aşağıdaki yapılandırma adımlarını gerçekleştirmeniz gerekir:

* Microsoft eşlemesi sağlanmış etkin bir ExpressRoute devresine sahip olmanız gerekir. Aşağıdaki yönergeleri kullanarak şu görevleri gerçekleştirebilirsiniz:
  * Devam etmeden önce [bir ExpressRoute devresi oluşturun](howto-circuit-cli.md) ve bağlantı sağlayıcınız tarafından devre dışı bırakıldı. ExpressRoute bağlantı hattı sağlanmış ve etkinleştirilmiş durumda olmalıdır.
  * BGP oturumunu doğrudan yönetiyorsanız, [Microsoft eşlemesi oluşturun](howto-routing-cli.md) . Ya da bağlantı sağlayıcınızın, devreniz için Microsoft eşlemesi sağlamasını sağlayın.

* Bir yol filtresi oluşturmanız ve yapılandırmanız gerekir.
  * Microsoft eşlemesi aracılığıyla kullanmak için kullandığınız Hizmetleri tanımla
  * Hizmetlerle ilişkili BGP topluluk değerlerinin listesini belirler
  * BGP topluluk değerleriyle eşleşen ön ek listesine izin veren bir kural oluşturma

* Yol filtresini ExpressRoute devresine bağlamanız gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce, CLI komutlarının en son sürümünü (2.0 veya üzeri) yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli) ve [Azure CLI’yi Kullanmaya Başlama](/cli/azure/get-started-with-azure-cli).

* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](howto-circuit-cli.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. ExpressRoute bağlantı hattı sağlanmış ve etkinleştirilmiş durumda olmalıdır.

* Etkin bir Microsoft eşlemesine sahip olmanız gerekir. [Eşleme yapılandırması oluşturma ve değiştirme](howto-routing-cli.md) yönergelerini izleyin

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin

Yapılandırmanızı başlatmak için Azure hesabınızda oturum açın. "Deneyin" i kullanıyorsanız, otomatik olarak oturum açtınız ve oturum açma adımını atlayabilirsiniz. Bağlanmanıza yardımcı olması için aşağıdaki örnekleri kullanın:

```azurecli
az login
```

Hesapla ilişkili abonelikleri kontrol edin.

```azurecli-interactive
az account list
```

ExpressRoute bağlantı hattı oluşturmak istediğiniz aboneliği seçin.

```azurecli-interactive
az account set --subscription "<subscription ID>"
```

## <a name="step-1-get-a-list-of-prefixes-and-bgp-community-values"></a><a name="prefixes"></a>1. Adım: ön eklerin ve BGP topluluk değerlerinin listesini alın

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP topluluk değerlerinin bir listesini alın

Microsoft eşlemesi aracılığıyla erişilebilen hizmetlerle ilişkili BGP topluluk değerlerinin listesini ve bunlarla ilişkili ön eklerin listesini almak için aşağıdaki cmdlet 'i kullanın:

```azurecli-interactive
az network route-filter rule list-service-communities
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. kullanmak istediğiniz değerlerin bir listesini oluşturun

Yol filtresinde kullanmak istediğiniz BGP topluluk değerlerinin bir listesini oluşturun.

## <a name="step-2-create-a-route-filter-and-a-filter-rule"></a><a name="filter"></a>2. Adım: bir yol filtresi ve bir filtre kuralı oluşturma

Yol filtresi yalnızca bir kurala sahip olabilir ve kuralın ' Allow ' türünde olması gerekir. Bu kural, kendisiyle ilişkili BGP topluluk değerlerinin bir listesine sahip olabilir.

### <a name="1-create-a-route-filter"></a>1. yol filtresi oluşturma

İlk olarak, yol filtresini oluşturun. Komut `az network route-filter create` yalnızca bir yol filtre kaynağı oluşturur. Kaynağı oluşturduktan sonra bir kural oluşturmanız ve bunu yol filtresi nesnesine bağlamanız gerekir. Bir yol filtresi kaynağı oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az network route-filter create -n MyRouteFilter -g MyResourceGroup
```

### <a name="2-create-a-filter-rule"></a>2. filtre kuralı oluşturma

Yeni bir kural oluşturmak için aşağıdaki komutu çalıştırın:
 
```azurecli-interactive
az network route-filter rule create --filter-name MyRouteFilter -n CRM --communities 12076:5040 --access Allow -g MyResourceGroup
```

## <a name="step-3-attach-the-route-filter-to-an-expressroute-circuit"></a><a name="attach"></a>3. Adım: yol filtresini bir ExpressRoute devresine Iliştirme

Yol filtresini ExpressRoute devresine eklemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroupName --name MicrosoftPeering --route-filter MyRouteFilter
```

## <a name="common-tasks"></a><a name="tasks"></a>Genel görevler

### <a name="to-get-the-properties-of-a-route-filter"></a><a name="getproperties"></a>Bir yol filtresinin özelliklerini almak için

Bir yol filtresinin özelliklerini almak için aşağıdaki komutu kullanın:

```azurecli-interactive
az network route-filter show -g ExpressRouteResourceGroupName --name MyRouteFilter 
```

### <a name="to-update-the-properties-of-a-route-filter"></a><a name="updateproperties"></a>Bir yol filtresinin özelliklerini güncelleştirmek için

Yol filtresi zaten bir bağlantı hattına eklenmişse, BGP topluluk listesi güncelleştirmeleri otomatik olarak, belirlenen BGP oturumlarıyla ilgili önek tanıtım değişikliklerini yayar. Aşağıdaki komutu kullanarak yol filtrenizin BGP topluluk listesini güncelleştirebilirsiniz:

```azurecli-interactive
az network route-filter rule update --filter-name MyRouteFilter -n CRM -g ExpressRouteResourceGroupName --add communities '12076:5040' --add communities '12076:5010'
```

### <a name="to-detach-a-route-filter-from-an-expressroute-circuit"></a><a name="detach"></a>Bir ExpressRoute bağlantı hattından yol filtresini ayırmak için

Yol filtresi ExpressRoute bağlantı hattı 'ndan ayrıldıktan sonra BGP oturumu aracılığıyla hiçbir önek tanıtılmaz. Aşağıdaki komutu kullanarak bir ExpressRoute bağlantı hattından bir yol filtresini ayırabilirsiniz:

```azurecli-interactive
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroupName --name MicrosoftPeering --remove routeFilter
```

### <a name="to-delete-a-route-filter"></a><a name="delete"></a>Yol filtresini silmek için

Bir yol filtresini yalnızca herhangi bir devreye iliştirilmişse silebilirsiniz. Yol filtresinin silmeyi denemeden önce herhangi bir devreye bağlı olmadığından emin olun. Aşağıdaki komutu kullanarak bir yol filtresini silebilirsiniz:

```azurecli-interactive
az network route-filter delete -n MyRouteFilter -g MyResourceGroup
```

## <a name="next-steps"></a>Sonraki Adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
