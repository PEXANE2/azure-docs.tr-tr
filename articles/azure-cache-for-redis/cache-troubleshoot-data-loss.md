---
title: Redsıs veri kaybı için Azure önbelleğinde sorun giderme | Microsoft Docs
description: Redsıs için Azure Cache ile veri kaybı sorunlarını çözmeyi öğrenin
services: cache
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 10/17/2019
ms.author: yegu
ms.openlocfilehash: 4fee7c84b394e84369b28d2a4191d0e581f3beba
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73044357"
---
# <a name="troubleshoot-azure-cache-for-redis-data-loss"></a>Redsıs veri kaybı için Azure önbelleğinde sorun giderme

Bu bölümde, Redsıs için Azure önbelleğinde gerçekleşebileceği gerçek veya algılanan veri kayıplarının nasıl tanılanması anlatılmaktadır.

- [Anahtarların kısmi kaybı](#partial-loss-of-keys)
- [Anahtarların büyük veya tamamen kaybolması](#major-or-complete-loss-of-keys)

> [!NOTE]
> Bu kılavuzdaki sorun giderme adımlarından bazıları Redsıs komutları çalıştırma ve çeşitli performans ölçümlerini izleme yönergelerini içerir. Daha fazla bilgi ve yönergeler için [ek bilgi](#additional-information) bölümündeki makalelere bakın.
>

## <a name="partial-loss-of-keys"></a>Anahtarların kısmi kaybı

Redsıs, bellekte depolandıktan sonra anahtarları rastgele silmez. Ancak, süre sonu veya çıkarma ilkelerine ve açık anahtar silme komutlarına yanıt olarak anahtarları kaldırır. Buna ek olarak, Premium veya standart Azure önbelleğindeki ana düğüme yazılmış anahtarlar, bir çoğaltmada doğrudan kullanılabilir olmayabilir. Veriler, ana bilgisayardan çoğaltmaya zaman uyumsuz ve engellenmeyen bir şekilde çoğaltılır.

Bu anahtarların önbelleğinizi kaybolduğunu fark ederseniz, sorunun nedeni olabileceğini görmek için aşağıdakileri kontrol edebilirsiniz:

| Nedeni | Açıklama |
|---|---|
| [Anahtar süre sonu](#key-expiration) | Anahtarlar, bunlar üzerinde ayarlanan zaman aşımları nedeniyle kaldırılıyor |
| [Anahtar çıkarma](#key-eviction) | Anahtarlar bellek baskısı altında kaldırılır |
| [Anahtar silme](#key-deletion) | Anahtarlar açık silme komutlarıyla kaldırılır |
| [Zaman uyumsuz çoğaltma](#async-replication) | Veri çoğaltma gecikmeleri nedeniyle bir çoğaltmada anahtarlar kullanılamaz |

### <a name="key-expiration"></a>Anahtar süre sonu

Redo, bir zaman aşımı atanmışsa ve bu süre geçtiğinde bir anahtarı otomatik olarak kaldırır. [Süre](http://redis.io/commands/expire) sonu komut belgelerinde redsıs anahtar süre sonu etrafında daha fazla ayrıntı bulabilirsiniz. Zaman aşımı değerleri [set](http://redis.io/commands/set), [SETEX](https://redis.io/commands/setex), [GetSet](https://redis.io/commands/getset)ve diğer \*STORE komutlarıyla de ayarlanabilir.

Kaç anahtarın dolduğunu gösteren bir istatistik almak için [Info](http://redis.io/commands/info) komutunu kullanabilirsiniz. *Stats* bölümü, süre dolma anahtarlarının toplam sayısını gösterir. *Keyspace* bölümü, zaman aşımları ve ortalama zaman aşımı değeri olan anahtar sayısı hakkında ek bilgiler sağlar.

```
# Stats

expired_keys:46583

# Keyspace

db0:keys=3450,expires=2,avg_ttl=91861015336
```

Ayrıca, anahtarın eksik olduğu zaman arasında bir bağıntı olup olmadığını görmek için önbelleğiniz için tanılama ölçümlerine bakabilirsiniz. Bu tür sorunların hatalarını ayıklamak için anahtar alanı bildirimlerini veya IZLEYICISINI kullanma hakkında bilgi için bkz. [ek](https://gist.github.com/JonCole/4a249477142be839b904f7426ccccf82#appendix) .

### <a name="key-eviction"></a>Anahtar çıkarma

Redin, verileri depolamak için bellek alanı gerektirir. Gerektiğinde, kullanılabilir belleği boşaltmak için anahtarları temizler. [Info](http://redis.io/commands/info) komutunda **used_memory** veya **used_memory_rss** değerleri yapılandırılmış **MaxMemory** ayarını yaklaşımında redsıs, [önbellek ilkesine](http://redis.io/topics/lru-cache)göre bellekten anahtar çıkarma işlemi başlatır.

[Info](http://redis.io/commands/info) komutunu kullanarak çıkarılan anahtarların sayısını izleyebilirsiniz.

```
# Stats

evicted_keys:13224
```

Ayrıca, anahtarın eksik olduğu ve çıkarılan anahtarlarda ani bir ilişki olup olmadığını görmek için önbelleğiniz için tanılama ölçümlerine bakabilirsiniz. Bu tür sorunların hatalarını ayıklamak için anahtar alanı bildirimlerini veya IZLEYICISINI kullanma hakkında bilgi için bkz. [ek](https://gist.github.com/JonCole/4a249477142be839b904f7426ccccf82#appendix) .

### <a name="key-deletion"></a>Anahtar silme

Redsıs istemcileri, Redsıs 'ten anahtarları açıkça kaldırmak için [del](http://redis.io/commands/del) veya [hdel](http://redis.io/commands/hdel) komutunu verebilir. [Info](http://redis.io/commands/info) komutunu kullanarak silme işlemlerinin sayısını izleyebilirsiniz. DEL veya HDEL komutları çağrılırsa, bu, *Commandstats* bölümünde listelenecektir.

```
# Commandstats

cmdstat_del:calls=2,usec=90,usec_per_call=45.00

cmdstat_hdel:calls=1,usec=47,usec_per_call=47.00
```

### <a name="async-replication"></a>Zaman uyumsuz çoğaltma

Standart veya Premium katmanda redin için herhangi bir Azure önbelleği, ana düğüm ve en az bir çoğaltma ile yapılandırılır. Veriler, arka plan işlemi kullanılarak ana bilgisayardan bir kopyaya zaman uyumsuz olarak kopyalanır. [Redis.io](http://redis.io/topics/replication) Web sitesi, redsıs veri çoğaltmasının genel olarak nasıl çalıştığını açıklar. İstemcilerin redde sıklıkla yazıldığı senaryolar için, bu çoğaltmanın anında olması garantisini nedeniyle kısmi veri kaybı oluşabilir. Örneğin _, bir istemci_ kendisine bir anahtar yazdığında, ancak arka plan işleminin bu anahtarı çoğaltmaya gönderebilmesinden _önce_ ana düğüm kapalıysa, çoğaltma yeni ana öğe olarak geçtiğinde anahtar kaybedilir.

## <a name="major-or-complete-loss-of-keys"></a>Anahtarların büyük veya tamamen kaybolması

Bir veya daha fazla anahtarın önbelleğinizi kaybolduğunu fark ederseniz, sorunun nedeni olabileceğini görmek için aşağıdakileri kontrol edebilirsiniz:

| Nedeni | Açıklama |
|---|---|
| [Anahtar Temizleme](#key-flushing) | Anahtarlar el ile temizlendi |
| [Yanlış veritabanı seçimi](#incorrect-database-selection) | Reddir varsayılan olmayan bir veritabanı kullanacak şekilde ayarlandı |
| [Redsıs örneği hatası](#redis-instance-failure) | Redsıs sunucusu kullanılamıyor |

### <a name="key-flushing"></a>Anahtar Temizleme

İstemciler, **tek** bir veritabanındaki tüm anahtarları kaldırmak Için [flushdb](http://redis.io/commands/flushdb) komutunu çağırabilir veya bir redsıs önbelleğindeki **tüm veritabanlarından tüm** anahtarları kaldırmak için [flushall](http://redis.io/commands/flushall) ' a çağrı yapabilir. [Info](http://redis.io/commands/info) komutunu kullanarak anahtarların temizlenmiş olup olmadığını görebilirsiniz. *Commandstats* bölümünde temizleme komutu çağrılırsa, görünür.

```
# Commandstats

cmdstat_flushall:calls=2,usec=112,usec_per_call=56.00

cmdstat_flushdb:calls=1,usec=110,usec_per_call=52.00
```

### <a name="incorrect-database-selection"></a>Yanlış veritabanı seçimi

Redsıs için Azure önbelleği varsayılan olarak **DB0** veritabanını kullanır. Başka bir veritabanına geçiş yaparsanız (örn., DB1) ve bundan sonra anahtarları okumaya çalışırsanız, her veritabanı mantıksal olarak ayrı bir birim olduğundan ve farklı bir veri kümesi tuttuğundan, Reddir bunları orada bulmaz. Diğer kullanılabilir veritabanlarını kullanmak ve bunların her birinde anahtarları aramak için [Seç](http://redis.io/commands/select) komutunu kullanın.

### <a name="redis-instance-failure"></a>Redsıs örneği hatası

Redsıs, bellek içi veri deposudur. Veriler redin barındıran fiziksel veya sanal makinelerde tutulur. Temel katmandaki Redsıs örneği için bir Azure önbelleği yalnızca tek bir sanal makinede (VM) çalışır. Bu VM kapalıysa, önbellekte depoladığınız tüm veriler kaybolur. Standart ve Premium katmanlardaki önbellekler, çoğaltılan bir yapılandırmada iki VM kullanarak veri kaybına karşı daha fazla esneklik sunar. Bu tür bir önbellekteki ana düğüm başarısız olduğunda çoğaltma düğümü, verileri otomatik olarak sunacak şekilde alır. Bu VM 'Ler ayrı hata ve güncelleştirme etki alanlarında bulunur ve aynı anda kullanılamaz hale gelme olasılığını en aza indirir. Ancak, ana veri merkezi kesintisi durumunda VM 'Ler yine de devam edebilir. Bu nadir durumlarda verileriniz kaybedilir.

Bu altyapı hatalarıyla karşı verilerinizin korunmasını artırmak için [redsıs veri kalıcılığı](http://redis.io/topics/persistence) ve [coğrafi çoğaltma](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-geo-replication) kullanmayı göz önünde bulundurmanız gerekir.

## <a name="additional-information"></a>Ek Bilgi

- [Redsıs sunucu tarafı sorunları için Azure Cache sorunlarını giderme](cache-troubleshoot-server.md)
- [Redne teklif teklifi ve boyutu için Azure önbelleği kullanmalıyım?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
- [Redsıs için Azure önbelleğini izleme](cache-how-to-monitor.md)
- [Redsıs komutlarını nasıl çalıştırabilirim?](cache-faq.md#how-can-i-run-redis-commands)
