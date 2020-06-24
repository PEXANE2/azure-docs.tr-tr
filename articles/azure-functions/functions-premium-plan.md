---
title: Azure Işlevleri Premium planı
description: Azure Işlevleri Premium planı için Ayrıntılar ve yapılandırma seçenekleri (VNet, soğuk başlatma yok, sınırsız yürütme süresi).
author: jeffhollan
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: jehollan
ms.openlocfilehash: d59335c5c4ebd2688097539594f11ea349939eff
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85298523"
---
# <a name="azure-functions-premium-plan"></a>Azure Işlevleri Premium planı

Azure Işlevleri Premium planı (bazen elastik Premium plan olarak adlandırılır), işlev uygulamaları için bir barındırma seçeneğidir. Premium planı, VNet bağlantısı, soğuk başlatma ve Premium donanım gibi özellikler sağlar.  Birden çok işlev uygulaması aynı Premium plana dağıtılabilir ve plan, işlem örneği boyutunu, temel plan boyutunu ve maksimum plan boyutunu yapılandırmanıza olanak tanır.  Premium planı ve diğer plan ve barındırma türlerinin karşılaştırması için bkz. [işlev ölçekleme ve barındırma seçenekleri](functions-scale.md).

## <a name="create-a-premium-plan"></a>Premium planı oluşturma

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

Azure CLı 'de [az functionapp plan Create](/cli/azure/functionapp/plan#az-functionapp-plan-create) ' i kullanarak da Premium bir plan oluşturabilirsiniz. Aşağıdaki örnek, esnek bir _Premium 1_ katman planı oluşturur:

```azurecli-interactive
az functionapp plan create --resource-group <RESOURCE_GROUP> --name <PLAN_NAME> \
--location <REGION> --sku EP1
```

Bu örnekte, kaynak grubu `<RESOURCE_GROUP>` ile ve `<PLAN_NAME>` plan için kaynak grubunda benzersiz olan bir adla değiştirin. [Desteklenen `<REGION>` ](https://azure.microsoft.com/global-infrastructure/services/?products=functions)bir belirtin. Linux destekleyen bir Premium planı oluşturmak için, seçeneğini dahil edin `--is-linux` .

Oluşturma planı ile, işlev uygulamanızı oluşturmak için [az functionapp Create](/cli/azure/functionapp#az-functionapp-create) kullanabilirsiniz. Portalda hem plan hem de uygulama aynı anda oluşturulur. Azure CLı komut dosyasının tamamı hakkında bir örnek için bkz. [Premium planda işlev uygulaması oluşturma](scripts/functions-cli-create-premium-plan.md).

## <a name="features"></a>Özellikler

Aşağıdaki özellikler bir Premium planına dağıtılan işlev uygulamaları için kullanılabilir.

### <a name="pre-warmed-instances"></a>Önceden çarpımış örnekler

Tüketim planında bugün hiçbir olay ve yürütme gerçekleşmiyor ise, uygulamanız sıfır örneğine göre ölçeklendirebilir. Yeni olaylar geldiğinde, üzerinde çalışan uygulamanız için yeni bir örnek kullanılması gerekir.  Yeni örneklerin özelleştirilmesi uygulamaya bağlı olarak biraz zaman alabilir.  İlk çağrıda bu ek gecikme süresi genellikle uygulama soğuk başlatması olarak adlandırılır.

Premium planda, uygulamanızın en düşük plan boyutuna kadar belirtilen sayıda örnek üzerinde önceden çarpmış olmasını sağlayabilirsiniz.  Önceden çarpımış örnekler, yüksek yük öncesi bir uygulamayı önceden ölçeklendirmenize de olanak tanır. Uygulama ölçeklendirilirken, önce önceden çarpımış örneklere ölçeklendirir. Ek örnekler, sonraki ölçek işlemine hazırlık sırasında arabelleğe alma ve sıcak olmaya devam eder. Önceden çarpımış örneklerin arabelleğine sahip olarak, soğuk başlangıç gecikmelerinin etkin bir şekilde kaçınabilirsiniz.  Önceden çarpımış örnekler Premium planın bir özelliğidir ve planın etkin olduğu her zaman en az bir örneği çalışır durumda tutmanız gerekir.

**İşlev uygulaması**seçtiğiniz, **platform özellikleri** sekmesine giderek ve **Ölçek Genişletme** seçeneklerini belirleyerek Azure Portal önceden çarpımış örneklerin sayısını yapılandırabilirsiniz. İşlev uygulaması düzenleme penceresinde, önceden çarpımış örnekler bu uygulamaya özeldir, ancak en küçük ve en büyük örnekler tüm planınız için geçerlidir.

![Elastik ölçek ayarları](./media/functions-premium-plan/scale-out.png)

Azure CLı ile bir uygulama için önceden çarpımış örnekler de yapılandırabilirsiniz.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="private-network-connectivity"></a>Özel ağ bağlantısı

Premium plana dağıtılan Azure Işlevleri, [Web Apps için yeni VNET tümleştirmesinden](../app-service/web-sites-integrate-with-vnet.md)yararlanır.  Yapılandırıldığında, uygulamanız VNet 'iniz içindeki kaynaklarla iletişim kurabilir veya hizmet uç noktaları aracılığıyla güvenli hale getirilir.  Ayrıca, gelen trafiği kısıtlamak için uygulamada IP kısıtlamaları da mevcuttur.

Bir Premium planında işlev uygulamanıza bir alt ağ atarken, her potansiyel örnek için yeterli IP adresi olan bir alt ağa ihtiyacınız vardır. Kullanılabilir en az 100 adresi olan bir IP bloğuna ihtiyacımız var.

Daha fazla bilgi için bkz. [işlev uygulamanızı VNET ile tümleştirme](functions-create-vnet.md).

### <a name="rapid-elastic-scale"></a>Hızlı elastik ölçek

Tüketim planıyla aynı hızlı ölçeklendirme mantığı kullanılarak uygulamanız için ek işlem örnekleri otomatik olarak eklenir. Aynı App Service plan içindeki uygulamalar, tek bir uygulamanın ihtiyaçlarına göre birbirinden bağımsız olarak ölçeklendirilir. Ancak, aynı App Service uygulamalar, mümkün olduğunda maliyetleri azaltmaya yardımcı olmak için VM kaynaklarını paylaşır. Bir VM ile ilişkili uygulama sayısı, her uygulamanın ayak izine ve VM 'nin boyutuna bağlıdır.

Ölçeklendirmenin nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [işlev ölçeği ve barındırma](./functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="longer-run-duration"></a>Uzun çalışma süresi

Bir tüketim planındaki Azure Işlevleri tek bir yürütme için 10 dakikaya sınırlandırılmıştır.  Premium planda, yürütme süresi, ard ların önlenmesi için en az 30 dakika olur. Ancak, bunu, Premium plan uygulamaları için sınırlandırılmamış hale getirmek üzere [yapılandırma host.jsdeğiştirebilirsiniz](./functions-host-json.md#functiontimeout) (garantili 60 dakika).

## <a name="plan-and-sku-settings"></a>Plan ve SKU ayarları

Planı oluştururken iki ayarı yapılandırırsınız: minimum örnek sayısı (veya plan boyutu) ve en fazla patlama sınırı.  Minimum örnekler ayrılmıştır ve her zaman çalışır.

> [!IMPORTANT]
> İşlevlerin yürütülmesi veya olmaması durumunda ne olursa olsun, minimum örnek sayısı için ayrılan her bir örnek için ücretlendirilirsiniz.

Uygulamanız plan boyutlarınızın ötesinde örnekler gerektiriyorsa, örnek sayısı maksimum patlama sınırına aşana kadar ölçeği ölçeklendirmeye devam edebilir.  Plan boyutlarınızın ötesinde örnekler için faturalandırılırsınız ve siz de çalışır.  Uygulamanızı tanımlanan maksimum sınıra ölçeklendirirken en iyi çabayı yapacağız, ancak uygulamanız için en düşük plan örnekleri garanti edilir.

Plan veya bu plana dağıtılan bir işlev uygulamasındaki **Ölçek Genişletme** seçeneklerini ( **platform özellikleri**altında) seçerek Azure Portal plan boyutunu ve en yüksek özellikleri yapılandırabilirsiniz.

Azure CLı 'den en fazla patlama sınırını de artırabilirsiniz:

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set properties.maximumElasticWorkerCount=<desired_max_burst> --resource-type Microsoft.Web/serverfarms 
```

### <a name="available-instance-skus"></a>Kullanılabilir örnek SKU 'Ları

Planınızı oluştururken veya ölçeklendirirken üç örnek boyutu arasından seçim yapabilirsiniz.  Saniye başına tüketilen toplam çekirdek sayısı ve bellek miktarı üzerinden faturalandırılırsınız.  Uygulamanız gerektiğinde birden çok örneğe otomatik olarak ölçeklenebilirler.  

|SKU|Çekirdekler|Bellek|Depolama|
|--|--|--|--|
|EP1|1|3,5 GB|GB|
|EP2|2|7 GB|GB|
|EP3|4|14 GB|GB|

### <a name="memory-utilization-considerations"></a>Bellek kullanımı konuları
Daha fazla belleğe sahip bir makinede çalıştırmak, işlev uygulamanızın kullanılabilir tüm belleği kullanacağı her zaman anlamına gelmez.

Örneğin, JavaScript işlev uygulaması Node.js varsayılan bellek sınırı ile kısıtlanır. Bu sabit bellek sınırını artırmak için, `languageWorkers:node:arguments` bir değeri olan uygulama ayarını ekleyin `--max-old-space-size=<max memory in MB>` .

## <a name="region-max-scale-out"></a>Bölge maksimum ölçeği genişletme

Aşağıda, her bir bölgedeki ve işletim sistemi yapılandırmasındaki tek bir plan için desteklenen en büyük ölçek genişletme değerleri verilmiştir. Bir artış istemek için lütfen bir destek bileti açın.

Işlevlerin tüm bölgesel kullanılabilirliğine buradan bakın: [Azure.com](https://azure.microsoft.com/global-infrastructure/services/?products=functions)

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
|Almanya Orta Batı| 100 | Kullanılamaz |
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
> [Azure Işlevleri ölçeklendirme ve barındırma seçeneklerini anlama](functions-scale.md)
