---
title: Azure Işlevleri Premium planı
description: Azure Işlevleri Premium planı için Ayrıntılar ve yapılandırma seçenekleri (VNet, soğuk başlatma yok, sınırsız yürütme süresi).
author: jeffhollan
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: jehollan
ms.custom: references_regions
ms.openlocfilehash: 4f6e2008cad66ce7cd68016d3873ecbc18b1961c
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89145767"
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

### <a name="always-ready-instances"></a>Her zaman için hazırlanma örnekleri

Tüketim planında bugün hiçbir olay ve yürütme gerçekleşmiyor ise, uygulamanız sıfır örneğine göre ölçeklendirebilir. Yeni olaylar geldiğinde, üzerinde çalışan uygulamanız için yeni bir örnek kullanılması gerekir.  Yeni örneklerin özelleştirilmesi uygulamaya bağlı olarak biraz zaman alabilir.  İlk çağrıda bu ek gecikme süresi genellikle uygulama soğuk başlatması olarak adlandırılır.

Premium planda, uygulamanızın belirtilen sayıda örnek üzerinde her zaman kullanılabilir olmasını sağlayabilirsiniz.  Her zaman için izin verilen en fazla örnek sayısı 20 ' dir.  Olaylar, uygulamayı tetiklemeye başladıktan sonra, ilk olarak her zaman için kullanılabilir örneklere yönlendirilir.  İşlev etkin hale geldiğinde, ek örnekler bir arabellek olarak çarpanacaktır.  Bu arabellek, ölçek sırasında gereken yeni örnekler için soğuk başlatmaya engel olur.  Bu arabelleğe alınmış örneklere, [önceden çarpımış örnekler](#pre-warmed-instances)denir.  Her zaman kullanıma uygun örneklerin ve önceden çarpımış arabelleğin birleşimiyle, uygulamanız soğuk başlatmayı etkili bir şekilde ortadan kaldırabilir.

> [!NOTE]
> Her Premium planda her zaman en az bir adet etkin ve faturalandırılan örnek olacaktır.

**İşlev uygulaması**seçtiğiniz, **platform özellikleri** sekmesine giderek ve **Ölçek Genişletme** seçeneklerini belirleyerek Azure Portal her zaman hazır örneklerin sayısını yapılandırabilirsiniz. İşlev uygulaması düzenleme penceresinde, her zaman kullanılabilir örnekler o uygulamaya özeldir.

![Elastik ölçek ayarları](./media/functions-premium-plan/scale-out.png)

Azure CLı ile bir uygulama için her zaman kullanılabilir örnekleri de yapılandırabilirsiniz.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.minimumElasticInstanceCount=<desired_always_ready_count> --resource-type Microsoft.Web/sites 
```

#### <a name="pre-warmed-instances"></a>Önceden çarpımış örnekler

Önceden çarpımış örnekler, ölçek ve etkinleştirme olayları sırasında arabellek olarak çarpımış örnek sayısıdır.  Ölçek Genişletme sınırına ulaşılana kadar önceden çarpımış örnekler arabelleğe devam eder.  Varsayılan önceden çarpımış örnek sayısı 1 ' dir ve çoğu senaryo için 1 olarak kalmalıdır.  Uygulamanın uzun bir ısınma süresi varsa (özel bir kapsayıcı görüntüsü gibi), bu arabelleği artırmak isteyebilirsiniz.  Önceden çarpımış bir örnek, yalnızca tüm etkin örnekler yeterince kullanıldıktan sonra etkin hale gelir.

Bu örnek, her zaman kullanıma yönelik örneklerin ve önceden çarpımış örneklerin birlikte nasıl çalıştığını göz önünde bulundurun.  Premium işlev uygulamasında beş her zaman hazır örnek yapılandırılır ve bir prewarmed örneği varsayılan olarak bulunur.  Uygulama boştayken ve hiçbir olay tetiklenirken uygulama, beş örnek üzerinde sağlanır ve çalışır.  

İlk tetikleyici geldiğinde, her zaman kullanılabilir örnek etkin olur ve daha önceden çarpımış bir örnek ayrılır.  Uygulama Şu anda altı sağlanan örnek ile çalışıyor: Şu anda beş adet etkin her zaman hazır örnekler ve altıncı önceden çarpımış ve etkin olmayan arabellek.  Yürütmelerin oranı artmaya devam ediyorsa, beş etkin örnek sonunda kullanılır.  Platform beş örnekten daha fazla ölçeklendirme yapmaya karar verdiğinde, önceden çarpımış örnekle ölçeklendirecektir.  Bu durumda, artık altı etkin örnek olur ve yedinci bir örnek anında temin edilir ve önceden çarpımış arabelleği doldurur.  Bu ölçek ve ön işleme sırası, uygulamanın en büyük örnek sayısına ulaşılana kadar devam eder.  Hiçbir örnek, ön örneklendirilecektir veya en yüksek değer üzerinde etkinleştirilmez.

Azure CLı kullanarak bir uygulama için önceden çarpımış örnek sayısını değiştirebilirsiniz.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites 
```

#### <a name="maximum-instances-for-an-app"></a>Bir uygulama için en fazla örnek

[En fazla örnek sayısı planının](#plan-and-sku-settings)yanı sıra, uygulama başına en fazla yapılandırma yapılandırabilirsiniz.  En fazla uygulama, [uygulama ölçek sınırı](./functions-scale.md#limit-scale-out)kullanılarak yapılandırılabilir.

### <a name="private-network-connectivity"></a>Özel ağ bağlantısı

Premium plana dağıtılan Azure Işlevleri, [Web Apps için yeni VNET tümleştirmesinden](../app-service/web-sites-integrate-with-vnet.md)yararlanır.  Yapılandırıldığında, uygulamanız VNet 'iniz içindeki kaynaklarla iletişim kurabilir veya hizmet uç noktaları aracılığıyla güvenli hale getirilir.  Ayrıca, gelen trafiği kısıtlamak için uygulamada IP kısıtlamaları da mevcuttur.

Bir Premium planında işlev uygulamanıza bir alt ağ atarken, her potansiyel örnek için yeterli IP adresi olan bir alt ağa ihtiyacınız vardır. Kullanılabilir en az 100 adresi olan bir IP bloğuna ihtiyacımız var.

Daha fazla bilgi için bkz. [işlev uygulamanızı VNET ile tümleştirme](functions-create-vnet.md).

### <a name="rapid-elastic-scale"></a>Hızlı elastik ölçek

Tüketim planıyla aynı hızlı ölçeklendirme mantığı kullanılarak uygulamanız için ek işlem örnekleri otomatik olarak eklenir. Aynı App Service plan içindeki uygulamalar, tek bir uygulamanın ihtiyaçlarına göre birbirinden bağımsız olarak ölçeklendirilir. Ancak, aynı App Service uygulamalar, mümkün olduğunda maliyetleri azaltmaya yardımcı olmak için VM kaynaklarını paylaşır. Bir VM ile ilişkili uygulama sayısı, her uygulamanın ayak izine ve VM 'nin boyutuna bağlıdır.

Ölçeklendirmenin nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [işlev ölçeği ve barındırma](./functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="longer-run-duration"></a>Uzun çalışma süresi

Bir tüketim planındaki Azure Işlevleri tek bir yürütme için 10 dakikaya sınırlandırılmıştır.  Premium planda, yürütme süresi, ard ların önlenmesi için en az 30 dakika olur. Ancak, süresi Premium plan uygulamaları için süreyi sınırlandırılmamış hale getirmek için [yapılandırma host.jsdeğiştirebilirsiniz](./functions-host-json.md#functiontimeout) (garantili 60 dakika).

## <a name="plan-and-sku-settings"></a>Plan ve SKU ayarları

Planı oluşturduğunuzda, iki plan boyutu ayarı vardır: minimum örnek sayısı (veya plan boyutu) ve en fazla patlama sınırı.

Uygulamanız her zaman hazır örneklerin ötesinde örnekler gerektiriyorsa, örnek sayısı maksimum patlama sınırına aşana kadar ölçeği ölçeklendirmeye devam edebilir.  Plan boyutlarınızın ötesinde örnekler için faturalandırılırsınız ve siz de çalışır.  Uygulamanızı, tanımlanan maksimum sınırına göre ölçeklendirirken en iyi çabayı yapacağız.

Plan veya bu plana dağıtılan bir işlev uygulamasındaki **Ölçek Genişletme** seçeneklerini ( **platform özellikleri**altında) seçerek Azure Portal plan boyutunu ve en yüksek özellikleri yapılandırabilirsiniz.

Azure CLı 'den en fazla patlama sınırını de artırabilirsiniz:

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set properties.maximumElasticWorkerCount=<desired_max_burst> --resource-type Microsoft.Web/serverfarms 
```

Her plan için en az bir örnek olacaktır.  Gerçek minimum örnek sayısı, plandaki uygulamalar tarafından istenen her zaman kullanılabilir örneklere dayanarak sizin için otomatik olarak yapılandırılır.  Örneğin, A uygulaması beş her zaman kullanılabilir örnek isterse ve B uygulaması aynı planda her zaman kullanılabilir örnek isterse, minimum plan boyutu beş olarak hesaplanır.  A uygulaması tüm 5 ' te çalışır ve B uygulaması yalnızca 2 ' de çalışır.

> [!IMPORTANT]
> İşlevlerin yürütülmesi veya olmaması durumunda ne olursa olsun, minimum örnek sayısı için ayrılan her bir örnek için ücretlendirilirsiniz.

Çoğu durumda, bu en düşük değer yeterince gerekli olmalıdır.  Ancak, en az bir çaba için en düşük düzeyde ölçekleme gerçekleşir.  Büyük olasılıkla, belirli bir zaman ölçeği, ek örnekler kullanılamaz durumdaysa geciktirilebilir.  En küçük değeri, tekrar hesaplanan en düşük değere ayarlayarak, örnekleri ölçeği yüksekten önce ayırmış olursunuz.

Bir plan için hesaplanan minimum değerin artırılması, Azure CLı kullanılarak yapılabilir.

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set sku.capacity=<desired_min_instances> --resource-type Microsoft.Web/serverfarms 
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
|Avustralya Güneydoğu | 100 | 20 |
|Brezilya Güney| 60 | 20 |
|Orta Kanada| 100 | 20 |
|Central US| 100 | 20 |
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
|West Europe| 100 | 20 |
|Batı Hindistan| 100 | 20 |
|Orta Batı ABD| 20 | 20 |
|Batı ABD| 100 | 20 |
|Batı ABD 2| 100 | 20 |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Işlevleri ölçeklendirme ve barındırma seçeneklerini anlama](functions-scale.md)
