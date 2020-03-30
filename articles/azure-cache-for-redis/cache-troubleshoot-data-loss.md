---
title: Redis için Azure Cache'de veri kaybı sorunlarını giderme
description: Redis için Azure Önbelleği ile ilgili veri kaybı sorunlarını (kısmi anahtar kaybı, anahtar son kullanma tarihi veya tam anahtar kaybı gibi) nasıl çözeceğinizi öğrenin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 10/17/2019
ms.openlocfilehash: d54506b94f076f0a3d967f88bd4e2960a1ca6396
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75530910"
---
# <a name="troubleshoot-data-loss-in-azure-cache-for-redis"></a>Redis için Azure Cache'de veri kaybı sorunlarını giderme

Bu makalede, Redis için Azure Önbelleğinde oluşabilecek gerçek veya algılanan veri kayıplarının nasıl tanılandığı açıklanmaktadır.

> [!NOTE]
> Bu kılavuzdaki sorun giderme adımlarından bazıları Redis komutlarını çalıştırmak ve çeşitli performans ölçümlerini izlemek için yönergeler içerir. Daha fazla bilgi ve talimatlar için [Ek bilgi](#additional-information) bölümündeki makalelere bakın.
>

## <a name="partial-loss-of-keys"></a>Anahtarların kısmi kaybı

Redis için Azure Önbelleği, anahtarlar bellekte depolandıktan sonra rasgele silemiyor. Ancak, son kullanma veya çıkarma ilkelerine yanıt olarak anahtarları ve açık anahtar silme komutlarını kaldırır. Redis örneğinde Premium veya Standart Azure Önbelleğinde ana düğüme yazılmış olan anahtarlar da hemen bir yinelemede kullanılamayabilir. Veriler, ana bilgisayardan yinelemeye asynchronous ve engelleyici olmayan bir şekilde çoğaltılır.

Anahtarların önbelleğinizden kaybolduğunu fark ederseniz, aşağıdaki olası nedenleri kontrol edin:

| Nedeni | Açıklama |
|---|---|
| [Anahtar sona erme](#key-expiration) | Anahtarlar, üzerlerine ayarlanan zaman çıkışları nedeniyle kaldırılır. |
| [Anahtar tahliye](#key-eviction) | Anahtarlar bellek baskısı altında kaldırılır. |
| [Anahtar silme](#key-deletion) | Anahtarlar açık silme komutları tarafından kaldırılır. |
| [Async çoğaltma](#async-replication) | Veri çoğaltma gecikmeleri nedeniyle bir yinelemede anahtarlar kullanılamaz. |

### <a name="key-expiration"></a>Anahtar sona erme

Redis için Azure Önbelleği, anahtara bir zaman dilimi atanmışsa ve bu süre geçmişse, anahtarı otomatik olarak kaldırır. Redis anahtar süresi hakkında daha fazla bilgi için [EXPIRE](https://redis.io/commands/expire) komut belgelerine bakın. Zaman dışı [değerler,](https://redis.io/commands/set) [SET , SETEX](https://redis.io/commands/setex), [GETSET](https://redis.io/commands/getset)ve diğer ** \*STORE** komutları kullanılarak da ayarlanabilir.

Kaç tuşun süresinin dolduğunu istatistiklere almak için [INFO](https://redis.io/commands/info) komutunu kullanın. Bölüm, `Stats` süresi dolmuş toplam anahtar sayısını gösterir. Bölüm, `Keyspace` zaman zaman ları olan anahtar sayısı ve ortalama zaman kaybı değeri hakkında daha fazla bilgi sağlar.

```
# Stats

expired_keys:46583

# Keyspace

db0:keys=3450,expires=2,avg_ttl=91861015336
```

Önbelleğinizin tanılama ölçümlerine de bakarak, anahtarın kaybolduğu zaman ile süresi dolmuş anahtarlarda ani artış arasında bir korelasyon olup olmadığını görebilirsiniz. Bu tür sorunları hata ayıklamak için anahtar alanı bildirimleri veya **MONITOR** kullanma hakkında bilgi için [Redis Anahtar Alanı Misses Hata Ayıklama](https://gist.github.com/JonCole/4a249477142be839b904f7426ccccf82#appendix) Ekine bakın.

### <a name="key-eviction"></a>Anahtar tahliye

Redis için Azure Önbelleği, verileri depolamak için bellek alanı gerektirir. Gerektiğinde kullanılabilir belleği boşaltmak için anahtarları temizler. [INFO](https://redis.io/commands/info) komutundaki **used_memory** veya **used_memory_rss** değerleri yapılandırılmış **maksimum bellek** ayarına yaklaştığında, Redis için Azure Önbelleği [önbellek ilkesine](https://redis.io/topics/lru-cache)dayalı olarak anahtarları bellekten çıkarmaya başlar.

[INFO](https://redis.io/commands/info) komutunu kullanarak tahliye edilen anahtarların sayısını izleyebilirsiniz:

```
# Stats

evicted_keys:13224
```

Önbelleğinizin tanıölçümlerine de bakabilir ve anahtarın kaybolduğu zaman ile tahliye edilen anahtarlarda ani artış arasında bir korelasyon olup olmadığını görebilirsiniz. Bu tür sorunları hata ayıklamak için anahtar alanı bildirimleri veya **MONITOR** kullanma hakkında bilgi için [Redis Anahtar Alanı Misses Hata Ayıklama](https://gist.github.com/JonCole/4a249477142be839b904f7426ccccf82#appendix) Ekine bakın.

### <a name="key-deletion"></a>Anahtar silme

Redis istemcileri, Redis için Azure Önbelleğinden anahtarları açıkça kaldırmak için [DEL](https://redis.io/commands/del) veya [HDEL](https://redis.io/commands/hdel) komutunu verebilir. [Info](https://redis.io/commands/info) komutunu kullanarak silme işlemlerinin sayısını izleyebilirsiniz. **DEL** veya **HDEL** komutları çağrıldıysa, `Commandstats` bunlar bölümde listelenir.

```
# Commandstats

cmdstat_del:calls=2,usec=90,usec_per_call=45.00

cmdstat_hdel:calls=1,usec=47,usec_per_call=47.00
```

### <a name="async-replication"></a>Async çoğaltma

Standart veya Premium katmanındaki Redis örneği için herhangi bir Azure Önbelleği, bir ana düğüm ve en az bir yinelemeyle yapılandırılır. Veriler, arka plan işlemi kullanılarak ana bilgisayardan eşzamanlı olarak bir yinelemeye kopyalanır. [redis.io](https://redis.io/topics/replication) web sitesi Redis veri çoğaltma genel olarak nasıl çalıştığını açıklar. İstemcilerin Redis'e sık sık yazdıkları senaryolarda, bu çoğaltmanın anlık olacağı garanti olduğundan kısmi veri kaybı oluşabilir. Örneğin, bir istemci anahtarı *yazdıktan sonra* asıl gider, ancak arka plan işlemi bu anahtarı yinelemeye gönderme şansı olmadan *önce,* yineleme yeni ana olarak devraldığında anahtar kaybolur.

## <a name="major-or-complete-loss-of-keys"></a>Büyük veya tam anahtar kaybı

Önbelleğinizden anahtarların çoğu veya tümü kaybolduysa, aşağıdaki olası nedenleri kontrol edin:

| Nedeni | Açıklama |
|---|---|
| [Anahtar yıkama](#key-flushing) | Anahtarlar elle temizlendi. |
| [Yanlış veritabanı seçimi](#incorrect-database-selection) | Redis için Azure Önbelleği varsayılan olmayan bir veritabanı kullanacak şekilde ayarlanmıştır. |
| [Redis örnek hatası](#redis-instance-failure) | Redis sunucusu kullanılamıyor. |

### <a name="key-flushing"></a>Anahtar yıkama

İstemciler, redis önbelleğindeki tüm *single* *veritabanlarındaki* tüm anahtarları kaldırmak için [FLUSHDB](https://redis.io/commands/flushdb) komutunu arayabilir veya [FLUSHALL'ı](https://redis.io/commands/flushall) arayabilir. Anahtarların temizlenip temizlenmediğini öğrenmek için [INFO](https://redis.io/commands/info) komutunu kullanın. Bölüm, `Commandstats` **FLUSH** komutundan herhangi biri çağrılıp çağrılmadığını gösterir:

```
# Commandstats

cmdstat_flushall:calls=2,usec=112,usec_per_call=56.00

cmdstat_flushdb:calls=1,usec=110,usec_per_call=52.00
```

### <a name="incorrect-database-selection"></a>Yanlış veritabanı seçimi

Redis için Azure Önbelleği varsayılan olarak **db0** veritabanını kullanır. Başka bir veritabanına (örneğin, **db1)** geçer ve anahtarları okumaya çalışırsanız, Redis için Azure Önbelleği bunları orada bulamaz. Her veritabanı mantıksal olarak ayrı bir birimdir ve farklı bir veri kümesi tutar. Diğer kullanılabilir veritabanlarını kullanmak ve her birinde anahtarları aramak için [SELECT](https://redis.io/commands/select) komutunu kullanın.

### <a name="redis-instance-failure"></a>Redis örnek hatası

Redis bir bellek içi veri deposudur. Veriler Redis önbelleğini barındıran fiziksel veya sanal makinelerde tutulur. Temel katmandaki Redis örneği için bir Azure Önbelleği yalnızca tek bir sanal makinede (VM) çalışır. Bu VM düştüyse, önbellekte depoladığınız tüm veriler kaybolur. 

Standart ve Premium katmanlarda önbellekler, çoğaltılmış bir yapılandırmada iki VM kullanarak veri kaybına karşı çok daha yüksek esneklik sağlar. Böyle bir önbellekteki ana düğüm başarısız olduğunda, yineleme düğümü verileri otomatik olarak sunmak için devralır. Bu VM'ler, her ikisinin de aynı anda kullanılamaması olasılığını en aza indirmek için hatalar ve güncelleştirmeler için ayrı etki alanlarında bulunur. Ancak, büyük bir veri merkezi kesintisi gerçekleşirse, VM'ler yine de birlikte batabilir. Verileriniz bu nadir durumlarda kaybolur.

Verilerinizin bu altyapı hatalarına karşı korunmasını iyileştirmek için [Redis veri kalıcılığını](https://redis.io/topics/persistence) ve [coğrafi çoğaltmayı](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-geo-replication) kullanmayı düşünün.

## <a name="additional-information"></a>Ek bilgiler

- [Redis için Azure Cache sunucu tarafı sorunlarını giderme](cache-troubleshoot-server.md)
- [Redis sunan ve boyutu için hangi Azure Önbelleğini kullanmalıyım?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
- [Redis için Azure Önbelleği nasıl izlenir?](cache-how-to-monitor.md)
- [Redis komutlarını nasıl çalıştırabilirim?](cache-faq.md#how-can-i-run-redis-commands)
