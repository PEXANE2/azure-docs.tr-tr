---
title: Azure Işlevleri Premium planı
description: Azure Işlevleri Premium planı için Ayrıntılar ve yapılandırma seçenekleri (VNet, soğuk başlatma yok, sınırsız yürütme süresi).
author: jeffhollan
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: jehollan
ms.custom:
- references_regions
- fasttrack-edit
- devx-track-azurecli
ms.openlocfilehash: 26dd3f7df5a71c687bfb4935f290e7a54b4e01fe
ms.sourcegitcommit: 8a74ab1beba4522367aef8cb39c92c1147d5ec13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98610276"
---
# <a name="azure-functions-premium-plan"></a>Azure Işlevleri Premium planı

Azure Işlevleri Premium planı (bazen elastik Premium plan olarak adlandırılır), işlev uygulamaları için bir barındırma seçeneğidir. Diğer barındırma planı seçenekleri için [barındırma planı makalesine](functions-scale.md)bakın.

Premium plan barındırma, işlevleriniz için aşağıdaki avantajları sağlar:

* Adet sürekli sıcak örneklerle soğuk çalışmaya özen gösterin
* Sanal ağ bağlantısı.
* Sınırsız yürütme süresi, 60 dakika garanti edilir.
* Premium örnek boyutları: bir çekirdek, iki çekirdek ve dört temel örnek.
* Tüketim planıyla karşılaştırıldığında daha öngörülebilir fiyatlandırma.
* Birden çok işlev uygulaması olan planlar için yüksek yoğunluklu uygulama ayırması.

Premium planı kullanırken, Azure Işlevleri ana bilgisayarının örnekleri, [Tüketim planı](consumption-plan.md)gibi gelen olayların sayısına göre eklenir ve kaldırılır. Birden çok işlev uygulaması aynı Premium plana dağıtılabilir ve plan, işlem örneği boyutunu, temel plan boyutunu ve maksimum plan boyutunu yapılandırmanıza olanak tanır. 

## <a name="billing"></a>Faturalandırma

Premium plan için faturalandırma, örnek genelinde ayrılan çekirdek saniye ve bellek sayısını temel alır. Bu Faturalandırma, yürütme ve tüketilen bellek başına faturalandırılan tüketim planından farklıdır. Premium planla yürütme ücreti yoktur. Plan başına her zaman en az bir örnek ayrılmalıdır. Bu Faturalandırma, işlevin etkin veya boşta olmasına bakılmaksızın, etkin plan başına en az aylık maliyetle sonuçlanır. Premium planındaki tüm işlev uygulamalarının ayrılmış örnekleri paylaştığı göz önünde bulundurun. Daha fazla bilgi edinmek için bkz. [Azure işlevleri fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/).

## <a name="create-a-premium-plan"></a>Premium planı oluşturma

Azure portal bir işlev uygulaması oluşturduğunuzda, tüketim planı varsayılandır. Premium bir planda çalışan bir işlev uygulaması oluşturmak için, _elastik Premium_ SKU 'lardan birini kullanarak açıkça bir App Service planı oluşturmanız gerekir. Oluşturduğunuz işlev uygulaması daha sonra bu planda barındırılır. Azure portal hem Premium planı hem de işlev uygulamasını aynı anda oluşturmayı kolaylaştırır. Aynı Premium planda birden fazla işlev uygulaması çalıştırabilirsiniz, ancak bunların çoğu her ikisi de aynı işletim sisteminde (Windows veya Linux) çalışır. 

Aşağıdaki makalelerde, programlama yoluyla veya Azure portal bir Premium plana sahip bir işlev uygulamasının nasıl oluşturulacağı gösterilmektedir:

+ [Azure portalı](create-premium-plan-function-app-portal.md)
+ [Azure CLI](scripts/functions-cli-create-premium-plan.md)
+ [Azure Resource Manager şablonu](functions-infrastructure-as-code.md#deploy-on-premium-plan)

## <a name="eliminate-cold-starts"></a>Soğuk çalışmaya engel

Tüketim planında olaylar veya yürütmeler gerçekleşmezseniz, uygulamanız sıfır örneğe ölçeklendirebilir. Yeni olaylar geldiğinde, uygulamanız üzerinde çalışan yeni bir örnek özel olmalıdır. Yeni örneklerin özelleştirilmesi uygulamaya bağlı olarak biraz zaman alabilir. İlk çağrıda bu ek gecikme süresi genellikle uygulama _soğuk başlatması_ olarak adlandırılır.

Premium plan, işlevlerinizdeki soğuk başlatılmaların etkili bir şekilde ortadan kaldırılması için birlikte çalışan iki özellik sağlar: _her zaman kullanıma_ sunulan örnekler ve _önceden çarpımış örnekler_. 

### <a name="always-ready-instances"></a>Her zaman için hazırlanma örnekleri

Premium planda, uygulamanızın belirtilen sayıda örnek üzerinde her zaman kullanılabilir olmasını sağlayabilirsiniz. Her zaman için izin verilen en fazla örnek sayısı 20 ' dir. Olaylar, uygulamayı tetiklemeye başladıktan sonra, ilk olarak her zaman kullanılabilir örneklere yönlendirilir. İşlev etkin hale geldiğinde, ek örnekler bir arabellek olarak çarpanacaktır. Bu arabellek, ölçek sırasında gereken yeni örnekler için soğuk başlatmaya engel olur. Bu arabelleğe alınmış örneklere, [önceden çarpımış örnekler](#pre-warmed-instances)denir. Her zaman kullanıma uygun örneklerin ve önceden çarpımış arabelleğin birleşimiyle, uygulamanız soğuk başlatmayı etkili bir şekilde ortadan kaldırabilir.

> [!NOTE]
> Her Premium planda her zaman en az bir adet etkin (Faturalanan) örnek vardır.

# <a name="portal"></a>[Portal](#tab/portal)

**İşlev uygulaması** seçtiğiniz, **platform özellikleri** sekmesine giderek ve **Ölçek Genişletme** seçeneklerini belirleyerek Azure Portal her zaman hazır örneklerin sayısını yapılandırabilirsiniz. İşlev uygulaması düzenleme penceresinde, her zaman kullanılabilir örnekler o uygulamaya özeldir.

![Elastik ölçek ayarları](./media/functions-premium-plan/scale-out.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

Azure CLı ile bir uygulama için her zaman kullanılabilir örnekleri de yapılandırabilirsiniz.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.minimumElasticInstanceCount=<desired_always_ready_count> --resource-type Microsoft.Web/sites
```
---

### <a name="pre-warmed-instances"></a>Önceden çarpımış örnekler

Önceden çarpımış örnekler, ölçek ve etkinleştirme olayları sırasında arabellek olarak çarpımış örneklerdir. Ölçek Genişletme sınırına ulaşılana kadar önceden çarpımış örnekler arabelleğe devam eder. Varsayılan önceden çarpımış örnek sayısı 1 ' dir ve çoğu senaryoda bu değer 1 olarak kalmalıdır.

Bir uygulamanın uzun bir ısınma süresi varsa (özel bir kapsayıcı görüntüsü gibi), bu arabelleği artırmanız gerekebilir. Önceden çarpımış bir örnek, yalnızca tüm etkin örnekler yeterince kullanıldıktan sonra etkin hale gelir.

Her zaman hazırlı örneklerin ve önceden çarpımış örneklerin birlikte nasıl çalıştığını gösteren bu örneği göz önünde bulundurun. Premium işlev uygulamasında beş her zaman hazır örnek yapılandırılır ve bir önceden çarpımış örnek varsayılan olarak bulunur. Uygulama boşta kaldığında ve hiçbir olay tetiklenirken, uygulama sağlandığında ve beş örnek ile çalışır. Şu anda, her zaman kullanıma uygun örnekler kullanılmadığından ve önceden çarpımış örnek ayrılmadığından, önceden çarpımış bir örnek için faturalandırılmaz.

İlk tetikleyici geldiğinde, her zaman için beş örnek etkin olur ve önceden çarpımış bir örnek ayrılır. Uygulama Şu anda altı sağlanan örnek ile çalışıyor: Şu anda beş adet etkin her zaman hazır örnekler ve altıncı önceden çarpımış ve etkin olmayan arabellek. Yürütmelerin oranı artmaya devam ediyorsa, beş etkin örnek kullanılır. Platform beş örnekten daha fazla ölçeklendirme yapmaya karar verdiğinde, önceden çarpımış örneğe ölçeklendirebilir. Bu durumda, artık altı etkin örnek vardır ve yedinci bir örnek anında sağlanır ve önceden çarpımış arabelleği doldurur. Bu ölçek ve ön işleme sırası, uygulamanın en büyük örnek sayısına ulaşılana kadar devam eder. Ön örnek yok ya da en yüksek değer üst sınırı aşıyor.

Azure CLı kullanarak bir uygulama için önceden çarpımış örnek sayısını değiştirebilirsiniz.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="maximum-function-app-instances"></a>Maksimum işlev uygulaması örnekleri

[En fazla örnek sayısı planının](#plan-and-sku-settings)yanı sıra, uygulama başına en fazla yapılandırma yapılandırabilirsiniz. En fazla uygulama, [uygulama ölçek sınırı](./event-driven-scaling.md#limit-scale-out)kullanılarak yapılandırılabilir.

## <a name="private-network-connectivity"></a>Özel ağ bağlantısı

Premium bir plana dağıtılan işlev uygulamaları, [Web Apps Için VNET tümleştirmesinden](../app-service/web-sites-integrate-with-vnet.md)yararlanabilir. Yapılandırıldığında, uygulamanız VNet 'iniz içindeki kaynaklarla iletişim kurabilir veya hizmet uç noktaları aracılığıyla güvenli hale getirilir. Ayrıca, gelen trafiği kısıtlamak için uygulamada IP kısıtlamaları da mevcuttur.

Bir Premium planında işlev uygulamanıza bir alt ağ atarken, her potansiyel örnek için yeterli IP adresi olan bir alt ağa ihtiyacınız vardır. Kullanılabilir en az 100 adresi olan bir IP bloğuna ihtiyacımız var.

Daha fazla bilgi için bkz. [işlev uygulamanızı VNET ile tümleştirme](functions-create-vnet.md).

## <a name="rapid-elastic-scale"></a>Hızlı elastik ölçek

Tüketim planıyla aynı hızlı ölçeklendirme mantığı kullanılarak uygulamanız için ek işlem örnekleri otomatik olarak eklenir. Aynı App Service plan içindeki uygulamalar, tek bir uygulamanın ihtiyaçlarına göre birbirinden bağımsız olarak ölçeklendirilir. Ancak, aynı App Service uygulamalar, mümkün olduğunda maliyetleri azaltmaya yardımcı olmak için VM kaynaklarını paylaşır. Bir VM ile ilişkili uygulama sayısı, her uygulamanın ayak izine ve VM 'nin boyutuna bağlıdır.

Ölçeklendirmenin nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [Azure Işlevlerinde olay odaklı ölçeklendirme](event-driven-scaling.md).

## <a name="longer-run-duration"></a>Uzun çalışma süresi

Bir tüketim planındaki Azure Işlevleri tek bir yürütme için 10 dakikaya sınırlandırılmıştır. Premium planda, yürütme süresi, ard ların önlenmesi için en az 30 dakika olur. Ancak, süresi Premium plan uygulamaları için süreyi sınırlandırılmamış hale getirmek üzere [yapılandırma host.jsdeğiştirebilirsiniz](./functions-host-json.md#functiontimeout) . Sınırsız bir süreye ayarlandığında, işlev uygulamanızın en az 60 dakika boyunca çalışması garanti edilir. 

## <a name="plan-and-sku-settings"></a>Plan ve SKU ayarları

Planı oluşturduğunuzda, iki plan boyutu ayarı vardır: minimum örnek sayısı (veya plan boyutu) ve en fazla patlama sınırı.

Uygulamanız her zaman hazır örneklerin ötesinde örnekler gerektiriyorsa, örnek sayısı maksimum patlama sınırına aşana kadar ölçeği ölçeklendirmeye devam edebilir. Plan boyutunuzu aşan örnekler için, yalnızca çalışırken ve size ikinci olarak tahsis edilirken faturalandırılırsınız. Platform, uygulamanızı tanımlanan maksimum sınıra ölçeklendirirken en iyi çabayı sağlar.

Plan veya bu plana dağıtılan bir işlev uygulamasındaki **Ölçek Genişletme** seçeneklerini ( **platform özellikleri** altında) seçerek Azure Portal plan boyutunu ve en yüksek özellikleri yapılandırabilirsiniz.

Azure CLı 'den en fazla patlama sınırını de artırabilirsiniz:

```azurecli-interactive
az functionapp plan update -g <resource_group> -n <premium_plan_name> --max-burst <desired_max_burst>
```

Her plan için en az bir örnek olacaktır. Gerçek minimum örnek sayısı, plandaki uygulamalar tarafından istenen her zaman kullanılabilir örneklere dayanarak sizin için otomatik olarak yapılandırılır. Örneğin, A uygulaması beş her zaman kullanılabilir örnek isterse ve B uygulaması aynı planda her zaman kullanılabilir örnek isterse, minimum plan boyutu beş olarak hesaplanır. A uygulaması tüm 5 ' te çalışır ve B uygulaması yalnızca 2 ' de çalışır.

> [!IMPORTANT]
> İşlevlerin yürütülmesi veya olmaması durumunda ne olursa olsun, minimum örnek sayısı için ayrılan her bir örnek için ücretlendirilirsiniz.

Çoğu durumda, bu tekrar hesaplanan minimum değer yeterlidir. Ancak, en az bir çaba için en düşük düzeyde ölçekleme gerçekleşir. Büyük olasılıkla, belirli bir zaman ölçeği, ek örnekler kullanılamaz durumdaysa geciktirilebilir. En küçük değeri, tekrar hesaplanan en düşük değere ayarlayarak, örnekleri ölçeği yüksekten önce ayırmış olursunuz.

Bir plan için hesaplanan minimum değerin artırılması, Azure CLı kullanılarak yapılabilir.

```azurecli-interactive
az functionapp plan update -g <resource_group> -n <premium_plan_name> --min-instances <desired_min_instances>
```

### <a name="available-instance-skus"></a>Kullanılabilir örnek SKU 'Ları

Planınızı oluştururken veya ölçeklendirirken üç örnek boyutu arasından seçim yapabilirsiniz. Her bir örneğin size ayrıldığı toplam çekirdek sayısı ve bellek için faturalandırılırsınız. Uygulamanız gerektiğinde birden çok örneğe otomatik olarak ölçeklenebilirler.

|SKU|Çekirdekler|Bellek|Depolama|
|--|--|--|--|
|EP1|1|3,5 GB|GB|
|EP2|2|7 GB|GB|
|EP3|4|14 GB|GB|

### <a name="memory-usage-considerations"></a>Bellek kullanımı konuları

Daha fazla belleğe sahip bir makinede çalıştırmak, işlev uygulamanızın tüm kullanılabilir belleği kullandığı anlamına gelir.

Örneğin, JavaScript işlev uygulaması Node.js varsayılan bellek sınırı ile kısıtlanır. Bu sabit bellek sınırını artırmak için, `languageWorkers:node:arguments` bir değeri olan uygulama ayarını ekleyin `--max-old-space-size=<max memory in MB>` .

Ve 4 GB 'den fazla bellek içeren planlar için, bit genişliği platformu ayarının `64 Bit` [Genel ayarlar](/azure/app-service/configure-common#configure-general-settings)altında olarak ayarlandığından emin olun.

## <a name="region-max-scale-out"></a>Bölge maksimum ölçeği genişletme

Aşağıda, her bir bölgedeki ve işletim sistemi yapılandırmasındaki tek bir plan için desteklenen en büyük ölçek genişletme değerleri verilmiştir. Bir artış istemek için bir destek bileti açabilirsiniz.

[Azure Web sitesindeki](https://azure.microsoft.com/global-infrastructure/services/?products=functions)işlevlerin tüm bölgesel kullanılabilirliğine bakın.

|Bölge| Windows | Linux |
|--| -- | -- |
|Orta Avustralya| 100 | Kullanılamaz |
|Orta Avustralya 2| 100 | Kullanılamaz |
|Doğu Avustralya| 100 | 20 |
|Avustralya Güneydoğu | 100 | 20 |
|Brezilya Güney| 100 | 20 |
|Orta Kanada| 100 | 20 |
|Central US| 100 | 20 |
|Çin Doğu 2| 100 | 20 |
|Çin Kuzey 2| 100 | 20 |
|Doğu Asya| 100 | 20 |
|Doğu ABD | 100 | 20 |
|Doğu ABD 2| 100 | 20 |
|Orta Fransa| 100 | 20 |
|Almanya Orta Batı| 100 | Kullanılamaz |
|Doğu Japonya| 100 | 20 |
|Batı Japonya| 100 | 20 |
|Güney Kore - Orta| 100 | 20 |
|Güney Kore - Güney| Kullanılamaz | 20 |
|Orta Kuzey ABD| 100 | 20 |
|Kuzey Avrupa| 100 | 20 |
|Norveç Doğu| 100 | 20 |
|Orta Güney ABD| 100 | 20 |
|Güney Hindistan | 100 | Kullanılamaz |
|Güneydoğu Asya| 100 | 20 |
|İsviçre Kuzey| 100 | Kullanılamaz |
|İsviçre Batı| 100 | Kullanılamaz |
|Güney Birleşik Krallık| 100 | 20 |
|Batı Birleşik Krallık| 100 | 20 |
|USGov Arizona| 100 | 20 |
|USGov Virginia| 100 | 20 |
|USNat Doğu| 100 | Kullanılamaz |
|USNat Batı| 100 | Kullanılamaz |
|West Europe| 100 | 20 |
|Batı Hindistan| 100 | 20 |
|Orta Batı ABD| 100 | 20 |
|Batı ABD| 100 | 20 |
|Batı ABD 2| 100 | 20 |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Işlevleri barındırma seçeneklerini anlama](functions-scale.md)
