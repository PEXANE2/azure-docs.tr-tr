---
title: Redis kümeleme yapılandırma - Redis için Premium Azure Önbelleği
description: Redis örnekleri için Premium katman Azure Önbelleği için Redis kümelemesini nasıl oluşturup yöneteceğinizi öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 06/13/2018
ms.openlocfilehash: 4a0e5b0c18264e1f7a98e81bcdfd56a7159235da
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010928"
---
# <a name="how-to-configure-redis-clustering-for-a-premium-azure-cache-for-redis"></a>Redis için Premium Azure Önbelleği için Redis kümeleme sini yapılandırma
Redis için Azure Önbellek, kümeleme, kalıcılık ve sanal ağ desteği gibi Premium katman özellikleri de dahil olmak üzere önbellek boyutu ve özellikleri seçiminde esneklik sağlayan farklı önbellek tekliflerine sahiptir. Bu makalede, Redis örneğinde birinci sınıf bir Azure Önbelleğinde kümelemenin nasıl yapılandırılabildiği açıklanmaktadır.

Diğer premium önbellek özellikleri hakkında daha fazla bilgi için [redis premium katmanı için Azure Önbelleğine Giriş'e](cache-premium-tier-intro.md)bakın.

## <a name="what-is-redis-cluster"></a>Redis Kümesi Nedir?
Redis için Azure Önbelleği [Redis'te uygulandığı gibi Redis kümesini](https://redis.io/topics/cluster-tutorial)sunar. Redis Cluster ile aşağıdaki avantajlardan yararlanırsınız: 

* Veri kümenizi otomatik olarak birden çok düğüme bölme yeteneği. 
* Düğümlerin bir alt kümesinde hatalar olduğunda veya kümenin geri kalanıyla iletişim kuramadığında işlemleri devam edebilme yeteneği. 
* Daha fazla iş elde etmek: Parça sayısını artırdıkça üretim doğrusal olarak artar. 
* Daha fazla bellek boyutu: Parça sayısını artırdıkça doğrusal olarak artar.  

Kümeleme, kümelenmiş önbellek için kullanılabilir bağlantı sayısını artırmaz. Boyut, iş ortası ve premium önbelleklerle bant genişliği hakkında daha fazla bilgi için [redis sunan ve boyutu kullanmam gereken Azure Önbelleği'ne bakın.](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)

Azure'da Redis kümesi, her parçanın çoğaltmayla birlikte bir birincil/çoğaltma çiftine sahip olduğu ve çoğaltmanın Redis hizmeti için Azure Önbelleği tarafından yönetildiği bir birincil/yineleme modeli olarak sunulur. 

## <a name="clustering"></a>Kümeleme
Önbellek oluşturma sırasında Redis için **Yeni Azure Önbelleğinde** kümeleme etkinleştirilir. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Kümeleme **Redis Cluster** bıçak üzerinde yapılandırılmıştır.

![Kümeleme][redis-cache-clustering]

Kümede en fazla 10 parça olabilir. **Etkin'i** tıklatın ve kaydırıcıyı kaydırın veya **Shard sayısı** için 1 ile 10 arasında bir sayı yazın ve **Tamam'ı**tıklatın.

Her parça Azure tarafından yönetilen bir birincil/yineleme önbellek çiftidir ve önbelleğin toplam boyutu, parça sayısının fiyatlandırma katmanında seçilen önbellek boyutuyla çarpılmasıyla hesaplanır. 

![Kümeleme][redis-cache-clustering-selected]

Önbellek oluşturulduktan sonra ona bağlanır ve kümelenmemiş bir önbellek gibi kullanırsınız ve Redis verileri Önbellek parçalarına dağıtır. Tanılama [etkinse,](cache-how-to-monitor.md#enable-cache-diagnostics)ölçümler her parça için ayrı olarak yakalanır ve Redis kılıcı için Azure Önbelleğinde [görüntülenebilir.](cache-how-to-monitor.md) 

> [!NOTE]
> 
> Kümeleme yapılandırıldığında istemci uygulamanızda gereken bazı küçük farklılıklar vardır. Daha fazla bilgi için bkz. Kümeleme kullanmak [için istemci uygulamamda herhangi bir değişiklik yapmam gerekiyor mu?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 

StackExchange.Redis istemcisi ile kümeleme ile çalışma örnek kodu için [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örneğinin [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) bölümüne bakın.

<a name="cluster-size"></a>

## <a name="change-the-cluster-size-on-a-running-premium-cache"></a>Çalışan premium önbellekte küme boyutunu değiştirme
Kümeleme etkinleştirilmiş çalışan premium önbellekteki küme boyutunu değiştirmek için **Kaynak menüsünden** **Küme Boyutu'nu** tıklatın.

![Redis küme boyutu][redis-cache-redis-cluster-size]

Küme boyutunu değiştirmek için kaydırıcıyı kullanın veya **Shard sayısı** metin kutusuna 1 ile 10 arasında bir sayı yazın ve kaydetmek için **Tamam'ı** tıklatın.

Küme boyutunu artırmak, maksimum iş ve önbellek boyutunu artırır. Küme boyutunu artırmak maksimum uyrmaz. istemciler tarafından kullanılabilir bağlantılar.

> [!NOTE]
> Bir kümeyi ölçekleme, pahalı bir komut olan [MIGRATE](https://redis.io/commands/migrate) komutunu çalıştırın, bu nedenle en az etki için bu işlemi yoğun olmayan saatlerde çalıştırmayı düşünün. Geçiş işlemi sırasında, sunucu yükünde bir artış görürsünüz. Kümeyi ölçekleme uzun süren bir işlemdir ve alınan süre, anahtar sayısına ve bu anahtarlarla ilişkili değerlerin boyutuna bağlıdır.
> 
> 

## <a name="clustering-faq"></a>Kümeleme SSS
Aşağıdaki liste, Redis kümeleme için Azure Önbelleği hakkında sık sorulan soruların yanıtlarını içerir.

* [Kümeleme kullanmak için istemci uygulamamda herhangi bir değişiklik yapmam gerekiyor mu?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
* [Anahtarlar kümede nasıl dağıtılır?](#how-are-keys-distributed-in-a-cluster)
* [Oluşturabileceğim en büyük önbellek boyutu nedir?](#what-is-the-largest-cache-size-i-can-create)
* [Tüm Redis istemcileri kümelenmeyi destekliyor mu?](#do-all-redis-clients-support-clustering)
* [Kümeleme etkinleştirildiğinde önbelleğe nasıl bağlanıyorum?](#how-do-i-connect-to-my-cache-when-clustering-is-enabled)
* [Önbelleğimin tek tek kırıklarına doğrudan bağlanabilir miyim?](#can-i-directly-connect-to-the-individual-shards-of-my-cache)
* [Daha önce oluşturulmuş bir önbellek için kümeoluşturmayı yapılandırabilir miyim?](#can-i-configure-clustering-for-a-previously-created-cache)
* [Kümelemi temel veya standart önbellek için yapılandırabilir miyim?](#can-i-configure-clustering-for-a-basic-or-standard-cache)
* [Redis ASP.NET Oturum Durumu ve Çıktı Önbelleğe Alma sağlayıcılarıyla kümelendirmeyi kullanabilir miyim?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)
* [StackExchange.Redis ve kümeleme kullanırken MOVE özel durumları alıyorum, ne yapmalıyım?](#i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do)

### <a name="do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering"></a>Kümeleme kullanmak için istemci uygulamamda herhangi bir değişiklik yapmam gerekiyor mu?
* Kümeleme etkinleştirildiğinde, yalnızca veritabanı 0 kullanılabilir. İstemci uygulamanız birden çok veritabanı kullanıyorsa ve 0 dışındaki bir veritabanını okumaya veya yazmaya çalışırsa, aşağıdaki özel durum atılır. `Unhandled Exception: StackExchange.Redis.RedisConnectionException: ProtocolFailure on GET --->` `StackExchange.Redis.RedisCommandException: Multiple databases are not supported on this server; cannot switch to database: 6`
  
  Daha fazla bilgi için [Redis Cluster Belirtimi - Uygulanan alt kümeye](https://redis.io/topics/cluster-spec#implemented-subset)bakın.
* [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/)kullanıyorsanız, 1.0.481 veya daha sonra kullanmanız gerekir. Kümeleme etkin olmayan bir önbelleğe bağlanırken kullandığınız aynı [uç noktaları, bağlantı noktalarını ve anahtarları](cache-configure.md#properties) kullanarak önbelleğe bağlanırsınız. Tek fark, tüm okuma ve yazma veritabanı 0 yapılmalıdır.
  
  * Diğer istemcilerin farklı gereksinimleri olabilir. Tüm [Redis istemcileri kümelenmeyi destekliyor mu?](#do-all-redis-clients-support-clustering)
* Uygulamanız tek bir komuta toplu olarak birden çok anahtar işlemi kullanıyorsa, tüm anahtarlar aynı parçada bulunmalıdır. Anahtarları aynı parçada bulmak için, [anahtarlar kümede nasıl dağıtılır?](#how-are-keys-distributed-in-a-cluster)
* Redis ASP.NET Oturum Devlet sağlayıcısı kullanıyorsanız 2.0.1 veya daha yüksek kullanmanız gerekir. Bkz. [Redis ASP.NET Oturum Durumu ve Çıktı Önbelleğe Alma sağlayıcılarıyla kümeleme kullanabilir miyim?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)

### <a name="how-are-keys-distributed-in-a-cluster"></a>Anahtarlar kümede nasıl dağıtılır?
Redis [Keys dağıtım modeli](https://redis.io/topics/cluster-spec#keys-distribution-model) belgelerine göre: Anahtar alanı 16384 yuvaya ayrılmıştır. Her anahtar haşlanır ve kümenin düğümleri arasında dağıtılan bu yuvalardan birine atanır. Birden çok anahtarın karma etiketleri kullanarak aynı parçada bulunduğundan emin olmak için anahtarın hangi bölümünün karma olarak yapılandırılabilir.

* Karma etiketli anahtarlar - anahtarın herhangi bir bölümü `{` `}`kapalıysa ve anahtarın yalnızca bu bölümü, anahtarın karma yuvasını belirlemek amacıyla ele alınır. Örneğin, aşağıdaki 3 tuş aynı parçada `{key}1`bulunur: , `{key}2`, `{key}3` ve `key` adın yalnızca bir kısmı haşlandığı için. Anahtar karma etiket özelliklerinin tam listesi için [Keys karma etiketleri bölümüne](https://redis.io/topics/cluster-spec#keys-hash-tags)bakın.
* Karma etiketi olmayan anahtarlar - tüm anahtar adı karma için kullanılır. Bu, önbelleğin parçaları arasında istatistiksel olarak eşit bir dağılıma neden olabilir.

En iyi performans ve iş için tuşları eşit olarak dağıtmanızı öneririz. Karma etiketli anahtarlar kullanıyorsanız, anahtarların eşit olarak dağıtıldığından emin olmak uygulamanın sorumluluğundadır.

Daha fazla bilgi için [Bkz. Keys dağıtım modeli,](https://redis.io/topics/cluster-spec#keys-distribution-model) [Redis Cluster veri parçaları](https://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding)ve [Keys karma etiketleri.](https://redis.io/topics/cluster-spec#keys-hash-tags)

StackExchange.Redis istemcisi ile aynı parçada kümeleme ve konumbelirleme tuşları ile çalışma örnek kod için [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örneğinin [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) bölümüne bakın.

### <a name="what-is-the-largest-cache-size-i-can-create"></a>Oluşturabileceğim en büyük önbellek boyutu nedir?
En büyük premium önbellek boyutu 120 GB'dır. Maksimum 1,2 TB GB boyutu sağlayan en fazla 10 parça oluşturabilirsiniz. Daha büyük bir boyuta ihtiyacınız varsa [daha fazlasını talep](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase)edebilirsiniz. Daha fazla bilgi [için Redis Fiyatlandırması için Azure Önbelleği'ne](https://azure.microsoft.com/pricing/details/cache/)bakın.

### <a name="do-all-redis-clients-support-clustering"></a>Tüm Redis istemcileri kümelenmeyi destekliyor mu?
Tüm istemciler Redis kümelenme destek! Kümelenmeyi destekleyen bir kitaplık ve sürüm kullandığınızı doğrulamak için lütfen kullanmakta olduğunuz kitaplığın belgelerini kontrol edin. StackExchange.Redis, yeni sürümlerinde kümelenmeyi destekleyen bir kitaplıktır. Diğer istemciler hakkında daha fazla bilgi için [Redis küme öğreticisinin](https://redis.io/topics/cluster-tutorial) [küme bölümüyle oynama](https://redis.io/topics/cluster-tutorial#playing-with-the-cluster) bölümüne bakın. 

Redis kümeleme protokolü, her istemcinin kümeleme modunda her parçaya doğrudan bağlanmasını gerektirir ve 'MOVED' na 'CROSSSLOTS' gibi yeni hata yanıtlarını tanımlar. Kümelemeyi kümelemeyi desteklemeyen bir istemciyi kümeleme yi desteklemeye çalışmak, çok sayıda [TAŞıNıR yeniden yönlendirme özel durumuyla](https://redis.io/topics/cluster-spec#moved-redirection)sonuçlanabilir veya çok tuşlu çok tuşlu isteklerde bulunuyorsanız, uygulamanızı bozabilir.

> [!NOTE]
> StackExchange.Redis'i müşteriniz olarak kullanıyorsanız, [clusterexchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 veya daha sonra kümelemenin en son sürümünü kullanarak doğru çalıştığından emin olun. Taşıma özel durumları ile ilgili herhangi bir sorun varsa, daha fazla bilgi için [taşıma özel durumlar](#move-exceptions) bakın.
>

### <a name="how-do-i-connect-to-my-cache-when-clustering-is-enabled"></a>Kümeleme etkinleştirildiğinde önbelleğe nasıl bağlanıyorum?
Kümeleme etkin olmayan bir önbelleğe bağlanırken kullandığınız aynı [uç noktaları,](cache-configure.md#properties) [bağlantı noktalarını](cache-configure.md#properties)ve [tuşları](cache-configure.md#access-keys) kullanarak önbelleğinize bağlanabilirsiniz. Redis arka uçtaki kümelemayı yönetir, böylece müşterinizden yönetmek zorunda kalmamanız gerekir.

### <a name="can-i-directly-connect-to-the-individual-shards-of-my-cache"></a>Önbelleğimin tek tek kırıklarına doğrudan bağlanabilir miyim?
Kümeleme protokolü, istemcinin doğru parça bağlantıları kurması gerekir. Yani müşteri sizin için doğru yapmalıdır. Bununla birlikte, her parça, topluca önbellek örneği olarak bilinen bir birincil/yineleme önbellek çiftinden oluşur. GitHub'daki Redis deposunun [kararsız](https://redis.io/download) dalındaki redis-cli yardımcı programını kullanarak bu önbellek örneklerine bağlanabilirsiniz. Bu `-c` sürüm, anahtarla başlatıldığında temel desteği uygular. Daha fazla bilgi için Redis [https://redis.io](https://redis.io) küme [öğreticisinde](https://redis.io/topics/cluster-tutorial) [kümeyle oynama](https://redis.io/topics/cluster-tutorial#playing-with-the-cluster) bilgisine bakın.

TLS olmayanlar için aşağıdaki komutları kullanın.

    Redis-cli.exe –h <<cachename>> -p 13000 (to connect to instance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (to connect to instance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (to connect to instance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (to connect to instance N)

TLS için `1300N` , `1500N`.

### <a name="can-i-configure-clustering-for-a-previously-created-cache"></a>Daha önce oluşturulmuş bir önbellek için kümeoluşturmayı yapılandırabilir miyim?
Evet. Öncelikle önbelleğinizin premium olduğundan emin olun, değilse ölçekle. Ardından, küme etkinleştirme seçeneği de dahil olmak üzere küme yapılandırma seçeneklerini görebilmeniz gerekir. Önbellek oluşturulduktan sonra veya kümeleme özelliğini ilk kez etkinleştirdikten sonra küme boyutunu değiştirebilirsiniz.

   >[!IMPORTANT]
   >Kümeleme etkinleştirme geri alamazsınız. Ve kümeleme etkin ve yalnızca bir parça ile bir önbellek *kümeleme olmadan* aynı boyutta bir önbellek *farklı* çalışır.

### <a name="can-i-configure-clustering-for-a-basic-or-standard-cache"></a>Kümelemi temel veya standart önbellek için yapılandırabilir miyim?
Kümeleme yalnızca premium önbellekler için kullanılabilir.

### <a name="can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers"></a>Redis ASP.NET Oturum Durumu ve Çıktı Önbelleğe Alma sağlayıcılarıyla kümelendirmeyi kullanabilir miyim?
* **Redis Output Önbellek sağlayıcısı** - değişiklik gerektirmez.
* **Redis Session State sağlayıcısı** - kümeleme kullanmak için [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 veya daha yüksek kullanmanız gerekir veya bir özel durum atılır. Bu bir kırılma değişikliktir; daha fazla bilgi için [v2.0.0 Breaking Change Ayrıntıları'na](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details)bakın.

<a name="move-exceptions"></a>

### <a name="i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do"></a>StackExchange.Redis ve kümeleme kullanırken MOVE özel durumları alıyorum, ne yapmalıyım?
StackExchange.Redis kullanıyorsanız ve `MOVE` kümeleme kullanırken özel durumlar alıyorsanız, [StackExchange.Redis 1.1.603](https://www.nuget.org/packages/StackExchange.Redis/) veya daha sonra kullandığınızdan emin olun. StackExchange.Redis'i kullanmak üzere .NET uygulamalarınızı yapılandırma yönergeleri için [bkz.](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliğini nasıl kullanacağınızı öğrenin.

* [Redis Premium katmanı için Azure Önbelleğine Giriş](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png

[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png

[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png
