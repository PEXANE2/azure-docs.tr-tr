---
title: Azure İşlevler Premium planı
description: Azure İşlevler Premium planı için ayrıntılar ve yapılandırma seçenekleri (VNet, soğuk başlangıç yok, sınırsız yürütme süresi).
author: jeffhollan
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: jehollan
ms.openlocfilehash: dd7f6d0760f2b848435e7c77657e261517d29dd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276913"
---
# <a name="azure-functions-premium-plan"></a>Azure İşlevler Premium planı

Azure İşlevler Premium planı (bazen Elastik Premium plan olarak da adlandırılır) işlev uygulamaları için bir barındırma seçeneğidir. Premium plan, VNet bağlantısı, soğuk başlangıç ve premium donanım gibi özellikler sağlar.  Aynı Premium plana birden çok işlevli uygulama dağıtılabilir ve plan, işlem örneği boyutunu, temel plan boyutunu ve maksimum plan boyutunu yapılandırmanıza olanak tanır.  Premium plan ve diğer plan ve barındırma türlerinin karşılaştırılması için [işlev ölçeği ve barındırma seçeneklerine](functions-scale.md)bakın.

## <a name="create-a-premium-plan"></a>Premium plan oluşturma

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

Azure CLI'de [az functionapp planını kullanarak](/cli/azure/functionapp/plan#az-functionapp-plan-create) bir Premium planı da oluşturabilirsiniz. Aşağıdaki örnek, _Elastik Premium 1_ katman planı oluşturur:

```azurecli-interactive
az functionapp plan create --resource-group <RESOURCE_GROUP> --name <PLAN_NAME> \
--location <REGION> --sku EP1
```

Bu örnekte, `<RESOURCE_GROUP>` kaynak grubunuzla ve `<PLAN_NAME>` planınızın kaynak grubunda benzersiz bir adla değiştirin. [Desteklenen `<REGION>` ](https://azure.microsoft.com/global-infrastructure/services/?products=functions)bir belirtin. Linux'u destekleyen bir Premium planı `--is-linux` oluşturmak için seçeneği ekleyin.

Oluşturulan plan ile, işlev uygulamanızı oluşturmak için [az functionapp create'ı](/cli/azure/functionapp#az-functionapp-create) kullanabilirsiniz. Portalda, hem plan hem de uygulama aynı anda oluşturulur. Tam bir Azure CLI komut dosyası örneği için [bkz.](scripts/functions-cli-create-premium-plan.md)

## <a name="features"></a>Özellikler

Aşağıdaki özellikler, Premium plana dağıtılan işlevleri işlevleri için kullanılabilir.

### <a name="pre-warmed-instances"></a>Önceden ısıtılmış örnekler

Bugün Tüketim planında hiçbir olay ve yürütme meydana gelmezse, uygulamanız sıfır alakadar ölçeklenebilir. Yeni etkinlikler geldiğinde, uygulamanızın üzerinde çalışırken yeni bir örneğin özelleştirilmiş olması gerekir.  Yeni örnekleri uzmanlaşmak uygulamaya bağlı olarak biraz zaman alabilir.  İlk aramadaki bu ek gecikme ye genellikle uygulama soğuk başlangıç denir.

Premium planda, uygulamanızın minimum plan boyutunuza kadar belirli sayıda örnekte önceden ısıtTırMasını sağlayabilirsiniz.  Önceden ısıtılmış örnekler, yüksek yüklemeden önce bir uygulamayı önceden ölçeklendirmenize de izin sağlar. Uygulama ölçeklendikçe, önce önceden ısıtılmış örneklere ölçeklenir. Ek örnekler arabelleğe almaya ve bir sonraki ölçek işlemine hazırlık olarak hemen ısınmaya devam eder. Önceden ısıtılmış örneklerin bir arabelleği olan, etkili soğuk başlangıç gecikmeleri önleyebilirsiniz.  Önceden ısıtılmış örnekler Premium planın bir özelliğidir ve en az bir örneğini çalışır durumda tutmanız ve planın etkin olduğu her zaman kullanılabilir durumda olmanız gerekir.

**İşlev Uygulamanızı**seçerek **, Platform Özellikleri** sekmesine giderek ve Dışarı **Ölçekleme** seçeneklerini seçerek Azure portalındaki önceden ısıtılmış örneklerin sayısını yapılandırabilirsiniz. İşlev uygulaması edit penceresinde, önceden ısıtılmış örnekler bu uygulamaya özgüdür, ancak en küçük ve en büyük örnekler tüm planınız için geçerlidir.

![Elastik Ölçek Ayarları](./media/functions-premium-plan/scale-out.png)

Azure CLI ile bir uygulama için önceden ısıtılmış örnekleri de yapılandırabilirsiniz.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="private-network-connectivity"></a>Özel ağ bağlantısı

Premium plana dağıtılan Azure [Fonksiyonları, web uygulamaları için yeni VNet tümleştirmesi](../app-service/web-sites-integrate-with-vnet.md)avantajlarından yararlanır.  Yapılandırıldığınızda, uygulamanız VNet'iniz içindeki kaynaklarla iletişim kurabilir veya hizmet bitiş noktaları aracılığıyla güvenli hale getirebilir.  Gelen trafiği kısıtlamak için uygulamada IP kısıtlamaları da mevcuttur.

Premium planda işlev uygulamanıza bir alt ağ atarken, her olası örnek için yeterli IP adresine sahip bir alt ağa ihtiyacınız vardır. En az 100 kullanılabilir adrese sahip bir IP bloğu na ihtiyacımız var.

Daha fazla bilgi için [işlev uygulamanızı bir VNet ile tümleştirin.](functions-create-vnet.md)

### <a name="rapid-elastic-scale"></a>Hızlı elastik ölçek

Tüketim planıyla aynı hızlı ölçekleme mantığını kullanarak uygulamanız için ek bilgi işlem örnekleri otomatik olarak eklenir.  Ölçeklendirmenin nasıl çalıştığı hakkında daha fazla bilgi edinmek için [İşlev ölçeği ve barındırma](./functions-scale.md#how-the-consumption-and-premium-plans-work)işlevine bakın.

### <a name="longer-run-duration"></a>Daha uzun çalışma süresi

Tüketim planındaki Azure İşlevler, tek bir yürütme için 10 dakikaile sınırlıdır.  Premium planda, kaçak yürütmeleri önlemek için çalışma süresi varsayılan olarak 30 dakikaya kadar dır. Ancak, [host.json yapılandırmasını,](./functions-host-json.md#functiontimeout) Premium plan uygulamaları için sınırsız hale getirmek için değiştirebilirsiniz (garantili 60 dakika).

## <a name="plan-and-sku-settings"></a>Plan ve SKU ayarları

Planı oluşturduğunuzda, iki ayar yapılandırırsınız: en az örnek sayısı (veya plan boyutu) ve maksimum patlama sınırı.  Minimum örnekler ayrılmıştır ve her zaman çalışır.

> [!IMPORTANT]
> İşlevler yürütülüp yürütülmese de minimum örnek sayısına ayrılan her örnek için ücretlendirilirsiniz.

Uygulamanız plan boyutunuzun ötesinde örnekler gerektiriyorsa, örnek sayısı maksimum patlama sınırına gelene kadar ölçeklendirmeye devam edebilir.  Yalnızca çalışırken ve size kiralanırken plan boyutunun ötesindeki durumlar için faturalandırılırsınız.  Uygulamanızı tanımlanan maksimum limite kadar ölçekleme için en iyi çabayı gösterirken, minimum plan örnekleri uygulamanız için garanti edilir.

Plandaki **Çıkış Ölçeği** seçeneklerini veya bu plana dağıtılan bir işlev uygulamasını **(Platform Özellikleri**altında) seçerek plan boyutunu ve maksimumları Azure portalında yapılandırabilirsiniz.

Azure CLI'den maksimum patlama sınırını da artırabilirsiniz:

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set properties.maximumElasticWorkerCount=<desired_max_burst> --resource-type Microsoft.Web/serverfarms 
```

### <a name="available-instance-skus"></a>Kullanılabilir örnek SUS'lar

Planınızı oluştururken veya ölçeklerken, üç örnek boyutu arasında seçim yapabilirsiniz.  Saniyede tüketilen toplam çekirdek ve bellek sayısı için faturalandırılırsınız.  Uygulamanız gerektiğinde otomatik olarak birden fazla örneğe ölçeklenebilir.  

|SKU|Çekirdekler|Bellek|Depolama|
|--|--|--|--|
|EP1|1|3.5 GB|250 GB|
|EP2|2|7 GB|250 GB|
|EP3|4|14 GB|250 GB|

### <a name="memory-utilization-considerations"></a>Bellek kullanımı hususları
Daha fazla belleğe sahip bir makinede çalıştırmak, işlev uygulamanızın kullanılabilir tüm belleği kullanacağı anlamına gelmez.

Örneğin, bir JavaScript işlev uygulaması, Düğüm.js'deki varsayılan bellek sınırıyla sınırlandırılmıştır. Bu sabit bellek sınırını artırmak için `languageWorkers:node:arguments` uygulama ayarını '' değeri yle `--max-old-space-size=<max memory in MB>`ekleyin.

## <a name="region-max-scale-out"></a>Bölge Max Ölçeği

Aşağıda, her bölge ve işletim sistemi yapılandırmasında tek bir plan için şu anda desteklenen maksimum ölçeklendirme değerleri verilmiştir. Artış talep etmek için lütfen bir destek bileti açın.

Fonksiyonların tam bölgesel kullanılabilirliğine buradan bakın: [Azure.com](https://azure.microsoft.com/global-infrastructure/services/?products=functions)

|Bölge| Windows | Linux |
|--| -- | -- |
|Orta Avustralya| 20 | Kullanılamaz |
|Orta Avustralya 2| 20 | Kullanılamaz |
|Doğu Avustralya| 100 | 20 |
|Güneydoğu Avustralya | 100 | 20 |
|Güney Brezilya| 60 | 20 |
|Orta Kanada| 100 | 20 |
|Orta ABD| 100 | 20 |
|Doğu Asya| 100 | 20 |
|Doğu ABD | 100 | 20 |
|Doğu ABD 2| 100 | 20 |
|Orta Fransa| 100 | 20 |
|Almanya Batı Orta| 100 | Kullanılamaz |
|Doğu Japonya| 100 | 20 |
|Batı Japonya| 100 | 20 |
|Güney Kore - Orta| 100 | 20 |
|Orta Kuzey ABD| 100 | 20 |
|Kuzey Avrupa| 100 | 20 |
|Norveç Doğu| 20 | 20 |
|Orta Güney ABD| 100 | 20 |
|Güney Hindistan | 100 | Kullanılamaz |
|Güneydoğu Asya| 100 | 20 |
|Güney Birleşik Krallık| 100 | 20 |
|Batı Birleşik Krallık| 100 | 20 |
|Batı Avrupa| 100 | 20 |
|Batı Hindistan| 100 | 20 |
|Orta Batı ABD| 20 | 20 |
|Batı ABD| 100 | 20 |
|Batı ABD 2| 100 | 20 |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevlerini ve barındırma seçeneklerini anlama](functions-scale.md)
