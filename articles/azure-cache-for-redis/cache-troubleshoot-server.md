---
title: Redis için Azure Cache sunucu tarafı sorunlarını giderme
description: Bellek baskısı, yüksek CPU, uzun süre çalışan komutlar veya bant genişliği sınırlamaları gibi Reda için Azure önbelleğiyle ilgili genel sunucu tarafı sorunlarını çözmeyi öğrenin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 10/18/2019
ms.openlocfilehash: 12d78846f5892e71388de6e6e76b868f9b14d4de
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88008925"
---
# <a name="troubleshoot-azure-cache-for-redis-server-side-issues"></a>Redis için Azure Cache sunucu tarafı sorunlarını giderme

Bu bölümde, redya ya da barındıran sanal makineler için Azure önbelleğindeki bir koşul nedeniyle oluşan sorun giderme sorunları ele alınmaktadır.

- [Redis sunucusunda bellek baskısı](#memory-pressure-on-redis-server)
- [Yüksek CPU kullanımı veya sunucu yükü](#high-cpu-usage-or-server-load)
- [Uzun süre çalışan komutlar](#long-running-commands)
- [Sunucu tarafı bant genişliği sınırlaması](#server-side-bandwidth-limitation)

> [!NOTE]
> Bu kılavuzdaki sorun giderme adımlarından bazıları Redsıs komutları çalıştırma ve çeşitli performans ölçümlerini izleme yönergelerini içerir. Daha fazla bilgi ve yönergeler için [ek bilgi](#additional-information) bölümündeki makalelere bakın.
>

## <a name="memory-pressure-on-redis-server"></a>Redis sunucusunda bellek baskısı

Sunucu tarafında bellek baskısı, isteklerin işlenmesini geciktirebileceği tüm performans sorunlarına yol açar. Bellek baskısı ne zaman olursa, sistem diske veri sayfası verebilir. Bu _sayfa hatalı_ , sistemin önemli ölçüde yavaşlamasına neden olur. Bu bellek basıncının birkaç olası nedeni vardır:

- Önbellek, maksimum kapasitesinin yakınında verilerle doldurulmuştur.
- Redsıs yüksek bellek parçalanması görüyor. Bu parçalanma çoğunlukla büyük nesneler depolanmasından kaynaklanır çünkü redin küçük nesneler için iyileştirildi.

Redin, bu sorunu belirlemenize yardımcı olabilecek [Info](https://redis.io/commands/info) komutu aracılığıyla iki istatistik ortaya çıkarır: "used_memory" ve "used_memory_rss". Portalı kullanarak [bu ölçümleri görüntüleyebilirsiniz](cache-how-to-monitor.md#view-metrics-with-azure-monitor) .

Bellek kullanımını sağlıklı tutmaya yardımcı olmak için kullanabileceğiniz birkaç olası değişiklik vardır:

- [Bir bellek Ilkesi yapılandırın](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) ve anahtarlarınızın süre sonu zamanlarını ayarlayın. Parçalandıysanız Bu ilke yeterli olmayabilir.
- Bellek parçalanmasını dengelemek için yeterince büyük olan [MaxMemory ile ayrılmış bir değer yapılandırın](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) .
- Büyük önbelleğe alınmış nesnelerinizi daha küçük ilgili nesnelere bölün.
- Olası etkilerden erken haberdar olmak için kullanılan bellek gibi ölçümler üzerinde [uyarılar oluşturun](cache-how-to-monitor.md#alerts) .
- Daha fazla bellek kapasitesine sahip daha büyük bir önbellek boyutuna [ölçeklendirin](cache-how-to-scale.md) .

## <a name="high-cpu-usage-or-server-load"></a>Yüksek CPU kullanımı veya sunucu yükü

Yüksek sunucu yükü veya CPU kullanımı, sunucunun istekleri zamanında işleyemediği anlamına gelir. Sunucunun yanıt vermesi yavaş olabilir ve istek ücretleri ile devam edemiyor.

CPU veya sunucu yükü gibi [ölçümleri izleyin](cache-how-to-monitor.md#view-metrics-with-azure-monitor) . CPU kullanımında, zaman aşımları ile karşılık gelen ani artışlar izleyin.

Yüksek sunucu yükünü azaltmak için yapabileceğiniz birkaç değişiklik vardır:

- Yüksek bellek baskısı nedeniyle, aşağıda belirtilen [uzun süre çalışan komutlar](#long-running-commands) veya sayfa hatalı gibi CPU artışlarına neden olduğunu araştırın.
- Olası etkilerden önce uyarılmak için CPU veya sunucu yükü gibi ölçümler üzerinde [uyarılar oluşturun](cache-how-to-monitor.md#alerts) .
- Daha fazla CPU kapasitesine sahip daha büyük bir önbellek boyutuna [ölçeklendirin](cache-how-to-scale.md) .

## <a name="long-running-commands"></a>Uzun süre çalışan komutlar

Bazı redin komutlarının yürütülmesi diğerlerine göre daha pahalıdır. [Redsıs komutları belgeleri](https://redis.io/commands) , her komutun zaman karmaşıklığını gösterir. Redsıs komut işleme tek iş parçacıklı olduğundan, çalıştırılacak zaman alan bir komut bundan sonra gelen tüm diğerlerini engeller. Performans etkilerini anlamak için, Redsıs sunucunuza dağıttığınız komutları gözden geçirmeniz gerekir. Örneğin, [anahtarlar](https://redis.io/commands/keys) komutu genellikle bir O (N) işlemi olduğunu bilmeksizin kullanılır. CPU artışlarını azaltmak için [tarama](https://redis.io/commands/scan) kullanarak anahtarlardan kaçınabilirsiniz.

[Yavaşlatma log](https://redis.io/commands/slowlog) komutunu kullanarak, sunucuda yürütülen pahalı komutları ölçebilirsiniz.

## <a name="server-side-bandwidth-limitation"></a>Sunucu tarafı bant genişliği sınırlaması

Farklı önbellek boyutları farklı ağ bant genişliği kapasiteye sahiptir. Sunucu kullanılabilir bant genişliğini aşarsa, veriler istemciye hızlı bir şekilde gönderilmez. Sunucu istemciye verileri yeterince hızlı bir şekilde gönderemediğinden, istemci istekleri zaman aşımına uğrar.

"Önbellek okuma" ve "önbellek yazma" ölçümleri, sunucu tarafı bant genişliğinin ne kadar kullanıldığını görmek için kullanılabilir. [Bu ölçümleri portalda görüntüleyebilirsiniz](cache-how-to-monitor.md#view-metrics-with-azure-monitor) .

Ağ bant genişliği kullanımının en yüksek kapasiteye yakın olduğu durumları azaltmak için:

- Ağ talebini azaltmak için istemci çağrısı davranışını değiştirin.
- Olası etkilerden önce bildirilmesi için önbellek okuma veya önbellek yazma gibi ölçümler üzerinde [uyarılar oluşturun](cache-how-to-monitor.md#alerts) .
- Daha fazla ağ bant genişliği kapasitesine sahip daha büyük bir önbellek boyutuna [ölçeklendirin](cache-how-to-scale.md) .

## <a name="additional-information"></a>Ek bilgiler

- [Redis için Azure Cache istemci tarafı sorunlarını giderme](cache-troubleshoot-client.md)
- [Doğru katmanı seçme](cache-overview.md#choosing-the-right-tier)
- [Önbelleğim performansını nasıl kıyaslarım ve test edebilirim?](cache-management-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
- [Redsıs için Azure önbelleğini izleme](cache-how-to-monitor.md)
- [Redsıs komutlarını nasıl çalıştırabilirim?](cache-development-faq.md#how-can-i-run-redis-commands)
