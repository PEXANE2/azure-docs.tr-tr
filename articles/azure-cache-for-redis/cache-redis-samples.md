---
title: Redis için Azure Önbelleği örnekleri
description: 'Bu kod örnekleriyle Redis için Azure Önbelleğini nasıl kullanacağınızı öğrenin: önbelleğe bağlanma, önbellekte veri okuma ve yazma, Redis sağlayıcıları için Azure Önbelleği ASP.NET.'
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: sample
ms.date: 01/23/2017
ms.openlocfilehash: 8057a7db3d1a2db0e51ec15e4966ed0b31c84226
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75433400"
---
# <a name="azure-cache-for-redis-samples"></a>Redis için Azure Önbelleği örnekleri
Bu konu, önbelleğe bağlanma, önbelleğe veri okuma ve yazma ve Redis sağlayıcıları için Azure Önbelleği ASP.NET kullanma gibi senaryoları kapsayan Redis örnekleri için Azure Önbelleğinin bir listesini sağlar. Örneklerin bazıları indirilebilir projelerdir ve bazıları adım adım kılavuzluk sağlar ve kod parçacıkları içerir, ancak indirilebilir bir projeye bağlantı vermez.

## <a name="hello-world-samples"></a>Merhaba dünya örnekleri
Bu bölümdeki örnekler, Redis örneği için bir Azure Önbelleğine bağlanmanın ve çeşitli diller ve Redis istemcileri kullanarak önbelleğe veri okuma ve yazma temellerini gösterir.

[Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örneği [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET istemcisini kullanarak çeşitli önbellek işlemlerinin nasıl gerçekleştirileceğini gösterir.

Bu örnek nasıl gösterin:

* Çeşitli bağlantı seçeneklerini kullanma
* Senkron ve eşzamanlı işlemleri kullanarak önbelleğe gelen nesneleri okuma ve yazma
* Belirtilen anahtarların değerlerini döndürmek için Redis MGET/MSET komutlarını kullanın
* Redis işlem işlemlerini gerçekleştirin
* Redis listeleri ve sıralanmış kümelerle çalışma
* JsonConvert serializers kullanarak .NET nesnelerini depola
* Etiketlemeyi uygulamak için Redis kümelerini kullanma
* Redis Cluster ile çalışma

Daha fazla bilgi için GitHub'daki [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) belgelerine bakın ve daha fazla kullanım senaryosu için [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/tests) birim testlerine bakın.

[Python ile Redis için Azure Önbelleği'nin nasıl kullanılacağı,](cache-python-get-started.md) Python ve [redis-py](https://github.com/andymccurdy/redis-py) istemcisini kullanarak Redis için Azure Önbelleği'ne nasıl başlanırsınız.

[Önbellekteki .NET nesneleriyle çalışmak,](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) .NET nesnelerini serihale etmenin bir yolunu gösterir, böylece bunları Redis örneği için bir Azure Önbelleğinden yazabilir ve okuyabilirsiniz. 

## <a name="use-azure-cache-for-redis-as-a-scale-out-backplane-for-aspnet-signalr"></a>ASP.NET SignalR için Arka Düzlemi Ölçeklendirmek için Redis için Azure Önbelleğini kullanma
ASP.NET SinyalR örneği [için Arka Düzlem olarak Redis için Azure Önbelleğini Kullanın,](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) Redis için Azure Önbelleğini SignalR arka düzlemi olarak nasıl kullanabileceğinizi gösterir. Backplane hakkında daha fazla bilgi için [Redis ile SignalR Scaleout'a](https://www.asp.net/signalr/overview/performance/scaleout-with-redis)bakın.

## <a name="azure-cache-for-redis-customer-query-sample"></a>Redis müşteri sorgu örneği için Azure Önbelleği
Bu örnek, önbellekteki verilere erişmekle kalıcılık depolamadan verilere erişmek arasındaki performansı karşılaştırır. Bu örnekte iki proje vardır.

* [Redis için Azure Önbelleğinin verileri önbelleğe alarak performansı nasıl artırabileceğini gösterim](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [Demo için Veritabanını ve Önbelleği Tohumla](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET Oturum Durumu ve Çıktı Önbelleğe Alma
[ASP.NET SessionState ve OutputCache örneğini depolamak için Redis için Azure Önbelleğini kullanın ve](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) Redis için SessionState ve OutputCache sağlayıcılarını kullanarak ASP.NET Oturum ve Çıktı Önbelleği depolamak için Redis için Azure Önbelleği'ni nasıl kullanacağınızı gösterir.

## <a name="manage-azure-cache-for-redis-with-maml"></a>MAML ile Redis için Azure Önbelleğini Yönetme
Azure Yönetim Kitaplıkları örneğini [kullanarak Redis için Azure Önbelleğini Yönet,](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) Önbelleğinizi yönetmek için Azure Yönetim Kitaplıklarını nasıl kullanabileceğinizi gösterir - (Oluştur/ Güncelle/ sil) önbelleği. 

## <a name="custom-monitoring-sample"></a>Özel izleme örneği
Redis İzleme veri örneği [için Access Azure Önbelleği,](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) Azure Portalı dışında Redis için Azure Önbelleğiniz için izleme verilerine nasıl erişebileceğinizi gösterir.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>PHP ve Redis kullanılarak yazılmış Twitter tarzı bir klon
[Retwis](https://github.com/SyntaxC4-MSFT/retwis) örneği Redis Hello World'dür. Bu [predis](https://github.com/nrk/predis) istemcisi kullanarak Redis ve PHP kullanılarak yazılmış bir minimal Twitter tarzı sosyal ağ klon. Kaynak kodu çok basit ve aynı zamanda farklı Redis veri yapıları göstermek için tasarlanmıştır.

## <a name="bandwidth-monitor"></a>Bant genişliği monitörü
[Bant Genişliği monitör](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) örneği istemcide kullanılan bant genişliğini izlemenizi sağlar. Bant genişliğini ölçmek için, önbellek istemcisi makinesinde örneği çalıştırın, önbelleğe arama yapın ve bant genişliği monitör örneği tarafından bildirilen bant genişliğini gözlemleyin.
