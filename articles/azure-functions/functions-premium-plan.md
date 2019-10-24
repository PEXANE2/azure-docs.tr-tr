---
title: Azure Işlevleri Premium planı (Önizleme) | Microsoft Docs
description: Azure Işlevleri Premium planı için Ayrıntılar ve yapılandırma seçenekleri (VNet, soğuk başlatma yok, sınırsız yürütme süresi).
services: functions
author: jeffhollan
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: jehollan
ms.openlocfilehash: ce83d521d5bc986be7bb24ef874f1f0e1051e3ae
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72755407"
---
# <a name="azure-functions-premium-plan-preview"></a>Azure Işlevleri Premium planı (Önizleme)

Azure Işlevleri Premium planı, işlev uygulamaları için bir barındırma seçeneğidir. Premium planı, VNet bağlantısı, soğuk başlatma ve Premium donanım gibi özellikler sağlar.  Birden çok işlev uygulaması aynı Premium plana dağıtılabilir ve plan, işlem örneği boyutunu, temel plan boyutunu ve maksimum plan boyutunu yapılandırmanıza olanak tanır.  Premium planı ve diğer plan ve barındırma türlerinin karşılaştırması için bkz. [işlev ölçekleme ve barındırma seçenekleri](functions-scale.md).

## <a name="create-a-premium-plan"></a>Premium planı oluşturma

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

Azure CLı 'de [az functionapp plan Create](/cli/azure/functionapp/plan#az-functionapp-plan-create) ' i kullanarak da Premium bir plan oluşturabilirsiniz. Aşağıdaki örnek, esnek bir _Premium 1_ katman planı oluşturur:

```azurecli-interactive
az functionapp plan create --resource-group <RESOURCE_GROUP> --name <PLAN_NAME> \
--location <REGION> --sku EP1
```

Bu örnekte, `<RESOURCE_GROUP>` kaynak grubu ile `<PLAN_NAME>` ve plan için kaynak grubunda benzersiz olan bir adla değiştirin. Desteklenen bir [`<REGION>`](#regions)belirtin. Linux destekleyen bir Premium plan oluşturmak için `--is-linux` seçeneğini ekleyin.

Oluşturma planı ile, işlev uygulamanızı oluşturmak için [az functionapp Create](/cli/azure/functionapp#az-functionapp-create) kullanabilirsiniz. Portalda hem plan hem de uygulama aynı anda oluşturulur. 

## <a name="features"></a>Özellikler

Aşağıdaki özellikler bir Premium planına dağıtılan işlev uygulamaları için kullanılabilir.

### <a name="pre-warmed-instances"></a>Önceden çarpımış örnekler

Tüketim planında bugün hiçbir olay ve yürütme gerçekleşmez, uygulamanız sıfıra ölçeklendirebilir. Yeni olaylar geldiğinde, üzerinde çalışan uygulamanız için yeni bir örnek kullanılması gerekir.  Yeni örneklerin özelleştirilmesi uygulamaya bağlı olarak biraz zaman alabilir.  İlk çağrıda bu ek gecikme süresi genellikle uygulama soğuk başlatması olarak adlandırılır.

Premium planda, uygulamanızın en düşük plan boyutuna kadar belirtilen sayıda örnek üzerinde önceden çarpmış olmasını sağlayabilirsiniz.  Önceden çarpımış örnekler, yüksek yük öncesi bir uygulamayı önceden ölçeklendirmenize de olanak tanır. Uygulama ölçeklendirilirken, önce önceden çarpımış örneklere ölçeklendirir. Ek örnekler, sonraki ölçek işlemine hazırlık sırasında arabelleğe alma ve sıcak olmaya devam eder. Önceden çarpımış örneklerin arabelleğine sahip olarak, soğuk başlangıç gecikmelerinin etkin bir şekilde kaçınabilirsiniz.  Önceden çarpımış örnekler Premium planın bir özelliğidir ve planın etkin olduğu her zaman en az bir örneği çalışır durumda tutmanız gerekir.

**İşlev uygulaması**seçtiğiniz, **platform özellikleri** sekmesine giderek ve **Ölçek Genişletme** seçeneklerini belirleyerek Azure Portal önceden çarpımış örneklerin sayısını yapılandırabilirsiniz. İşlev uygulaması düzenleme penceresinde, önceden çarpımış örnekler bu uygulamaya özeldir, ancak en küçük ve en büyük örnekler tüm planınız için geçerlidir.

![Elastik ölçek ayarları](./media/functions-premium-plan/scale-out.png)

Azure CLı ile bir uygulama için önceden çarpımış örnekleri de yapılandırabilirsiniz

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="private-network-connectivity"></a>Özel ağ bağlantısı

Premium plana dağıtılan Azure Işlevleri, [Web Apps için yeni VNET tümleştirmesinden](../app-service/web-sites-integrate-with-vnet.md)yararlanır.  Yapılandırıldığında, uygulamanız VNet 'iniz içindeki kaynaklarla iletişim kurabilir veya hizmet uç noktaları aracılığıyla güvenli hale getirilir.  Ayrıca, gelen trafiği kısıtlamak için uygulamada IP kısıtlamaları da mevcuttur.

Bir Premium planında işlev uygulamanıza bir alt ağ atarken, her potansiyel örnek için yeterli IP adresi olan bir alt ağa ihtiyacınız vardır. Önizleme sırasında en fazla örnek sayısı değişebilse de, kullanılabilir en az 100 adresi olan bir IP bloğunun olması gerekir.

Daha fazla bilgi için bkz. [işlev uygulamanızı VNET ile tümleştirme](functions-create-vnet.md).

### <a name="rapid-elastic-scale"></a>Hızlı elastik ölçek

Tüketim planıyla aynı hızlı ölçeklendirme mantığı kullanılarak uygulamanız için ek işlem örnekleri otomatik olarak eklenir.  Ölçeklendirmenin nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [işlev ölçeği ve barındırma](./functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="unbounded-run-duration"></a>Sınırsız çalıştırma süresi

Bir tüketim planındaki Azure Işlevleri tek bir yürütme için 10 dakikaya sınırlandırılmıştır.  Premium planda, yürütme süresi, ard ların önlenmesi için en az 30 dakika olur. Bununla birlikte, Premium plan uygulamaları için bunu sınırlandırılmamış hale getirmek üzere [Host. JSON yapılandırmasını değiştirebilirsiniz](./functions-host-json.md#functiontimeout) .

Önizleme aşamasında, süresi 12 dakikadan daha fazla garanti edilmez ve uygulamanız en düşük çalışan sayısının ötesinde ölçeklenmiyorsa 30 dakikadan daha fazla çalışma şansı elde edilir.

## <a name="plan-and-sku-settings"></a>Plan ve SKU ayarları

Planı oluştururken iki ayarı yapılandırırsınız: minimum örnek sayısı (veya plan boyutu) ve en fazla patlama sınırı.  Premium plan için en az örnek 1 ' dir ve Önizleme sırasında en fazla patlama 20 ' dir.  Minimum örnekler ayrılmıştır ve her zaman çalışır.

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

|SKU|Çekirdek|Hafıza|Depolama|
|--|--|--|--|
|EP1|1|3,5 GB|250 GB|
|EP2|2|7 GB|250 GB|
|EP3|4|14 GB|250 GB|

## <a name="regions"></a>Bölgeler

Aşağıda, her bir işletim sisteminin genel önizlemesi için desteklenen şu bölgeler verilmiştir.

|Bölge| Windows | Linux |
|--| -- | -- |
|Avustralya Orta| ✔ * | |
|Avustralya Orta 2| ✔ * | |
|Doğu Avustralya| ✔ | |
|Güneydoğu Avustralya | ✔ | ✔ |
|Brezilya Güney| ✔ * * |  |
|Kanada Orta| ✔ |  |
|Orta ABD| ✔ |  |
|Doğu Asya| ✔ |  |
|Doğu ABD | ✔ | ✔ |
|Doğu ABD 2| ✔ |  |
|Fransa Orta| ✔ |  |
|Doğu Japonya| ✔ | ✔ |
|Batı Japonya| ✔ | |
|Kore Orta| ✔ |  |
|Orta Kuzey ABD| ✔ |  |
|Kuzey Avrupa| ✔ | ✔ |
|Güney Orta ABD| ✔ |  |
|Güney Hindistan | ✔ | |
|Güneydoğu Asya| ✔ | ✔ |
|Birleşik Krallık, Güney| ✔ | |
|Birleşik Krallık, Batı| ✔ |  |
|Batı Avrupa| ✔ | ✔ |
|Batı Hindistan| ✔ |  |
|Batı ABD| ✔ | ✔ |

\* en fazla ölçek genişletme, 20 örneğe sınırlı

\** maksimum ölçeği 60 örneğe sınırlı

## <a name="known-issues"></a>Bilinen Sorunlar

[GitHub 'da genel önizlemeye](https://github.com/Azure/Azure-Functions/wiki/Premium-plan-known-issues)yönelik bilinen sorunların durumunu izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Işlevleri ölçeklendirme ve barındırma seçeneklerini anlama](functions-scale.md)
