---
title: Redis için Azure Cache sunucu tarafı sorunlarını giderme
description: Bellek basıncı, yüksek CPU, uzun çalışan komutlar veya bant genişliği sınırlamaları gibi Redis için Azure Önbelleği ile sunucu tarafındaki yaygın sorunları nasıl çözeceğinizi öğrenin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 10/18/2019
ms.openlocfilehash: a68c27de304a0da6470745ee4abf69590d9bf78c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277940"
---
# <a name="troubleshoot-azure-cache-for-redis-server-side-issues"></a>Redis için Azure Cache sunucu tarafı sorunlarını giderme

Bu bölümde, Redis için bir Azure Önbelleği veya onu barındıran sanal makine(ler) koşulu nedeniyle oluşan sorun giderme sorunları açıklanmaktadır.

- [Redis sunucusunda bellek baskısı](#memory-pressure-on-redis-server)
- [Yüksek CPU kullanımı veya sunucu yükü](#high-cpu-usage-or-server-load)
- [Uzun süre çalışan komutlar](#long-running-commands)
- [Sunucu tarafı bant genişliği sınırlaması](#server-side-bandwidth-limitation)

> [!NOTE]
> Bu kılavuzdaki sorun giderme adımlarından bazıları Redis komutlarını çalıştırmak ve çeşitli performans ölçümlerini izlemek için yönergeler içerir. Daha fazla bilgi ve talimatlar için [Ek bilgi](#additional-information) bölümündeki makalelere bakın.
>

## <a name="memory-pressure-on-redis-server"></a>Redis sunucusunda bellek baskısı

Sunucu tarafındaki bellek baskısı, isteklerin işlenmesini geciktirebilecek her türlü performans sorununa yol açar. Bellek basıncı çarptığında, sistem verileri diske sayfalayabilir. Bu _sayfa hatası,_ sistemin önemli ölçüde yavaşlamasına neden olur. Bu bellek basıncının birkaç olası nedeni vardır:

- Önbellek, maksimum kapasitesine yakın verilerle doldurulur.
- Redis yüksek hafıza parçalanması görüyor. Redis küçük nesneler için en iyi duruma geldiğinden, bu parçalanma ya da büyük nesnelerin depolanmasından kaynaklanır.

Redis, [INFO](https://redis.io/commands/info) komutu aracılığıyla bu sorunu belirlemenize yardımcı olabilecek iki durumu ortaya çıkarır: "used_memory" ve "used_memory_rss". Portalı kullanarak [bu ölçümleri görüntüleyebilirsiniz.](cache-how-to-monitor.md#view-metrics-with-azure-monitor)

Bellek kullanımını sağlıklı tutmaya yardımcı olmak için yapabileceğiniz birkaç olası değişiklik vardır:

- [Bir bellek ilkesini yapılandırın](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) ve anahtarlarınızın son kullanma saatlerini ayarlayın. Parçalanma nız varsa bu ilke yeterli olmayabilir.
- Bellek parçalanmasını telafi edecek kadar büyük [bir maxmemory ayrılmış değeri yapılandırın.](cache-configure.md#maxmemory-policy-and-maxmemory-reserved)
- Büyük önbelleğe alınmış nesneleri daha küçük ilgili nesnelere ayırın.
- Olası etkileri hakkında erken bildirimde kullanılmak üzere kullanılmış bellek gibi ölçümlerde [uyarılar oluşturun.](cache-how-to-monitor.md#alerts)
- [Scale](cache-how-to-scale.md) Daha fazla bellek kapasitesiyle daha büyük bir önbellek boyutuna ölçeklendirin.

## <a name="high-cpu-usage-or-server-load"></a>Yüksek CPU kullanımı veya sunucu yükü

Yüksek sunucu yükü veya CPU kullanımı, sunucunun istekleri zamanında işleyamayacağı anlamına gelir. Sunucu yanıt vermekte yavaş olabilir ve istek oranlarına ayak uyduramayabilir.

CPU veya sunucu yükü gibi [ölçümleri izleyin.](cache-how-to-monitor.md#view-metrics-with-azure-monitor) Zaman ekmelerine karşılık gelen CPU kullanımındaki ani artışlara dikkat edin.

Yüksek sunucu yükünü azaltmak için yapabileceğiniz birkaç değişiklik vardır:

- Aşağıda belirtilen [uzun süreli komutlar](#long-running-commands) veya yüksek bellek basıncı nedeniyle sayfa hatası gibi CPU ani artışlarına neyin neden olduğunu araştırın.
- Olası etkileri hakkında erken bildirimde olmak üzere CPU veya sunucu yükü gibi ölçümlerde [uyarılar oluşturun.](cache-how-to-monitor.md#alerts)
- [Scale](cache-how-to-scale.md) Daha fazla CPU kapasitesine sahip daha büyük bir önbellek boyutuna ölçeklendirin.

## <a name="long-running-commands"></a>Uzun süre çalışan komutlar

Bazı Redis komutlarının yürütülmesi diğerlerinden daha pahalıdır. [Redis komutları belgeler](https://redis.io/commands) her komutun zaman karmaşıklığını gösterir. Redis komut işleme tek iş parçacığı olduğundan, çalıştırmak için zaman alan bir komut sonra gelen tüm diğer engeller. Performans etkilerini anlamak için Redis sunucunuza verdiğiniz komutları gözden geçirmelisiniz. Örneğin, [KEYS](https://redis.io/commands/keys) komutu genellikle bir O(N) işlemi olduğunu bilmeden kullanılır. CPU artışlarını azaltmak için [SCAN'ı](https://redis.io/commands/scan) kullanarak KEYS'ten kaçınabilirsiniz.

[SLOWLOG](https://redis.io/commands/slowlog) komutunu kullanarak, sunucuya karşı yürütülen pahalı komutları ölçebilirsiniz.

## <a name="server-side-bandwidth-limitation"></a>Sunucu tarafı bant genişliği sınırlaması

Farklı önbellek boyutları farklı ağ bant genişliği kapasitelerine sahiptir. Sunucu kullanılabilir bant genişliğini aşarsa, veriler istemciye bu kadar hızlı gönderilmez. Sunucu verileri istemciye yeterince hızlı itemediği için istemci istekleri zaman alabilir.

Sunucu tarafı bant genişliğinin ne kadar kullanıldığını görmek için "Önbellek Okuma" ve "Önbellek Yazma" ölçümleri kullanılabilir. Bu [ölçümleri](cache-how-to-monitor.md#view-metrics-with-azure-monitor) portalda görüntüleyebilirsiniz.

Ağ bant genişliği kullanımının maksimum kapasiteye yakın olduğu durumları azaltmak için:

- Ağ talebini azaltmak için istemci çağrı davranışını değiştirin.
- Olası etkileri hakkında erken bildirimde olmak üzere önbellek okuma veya önbellek yazma gibi ölçümler de [uyarılar oluşturun.](cache-how-to-monitor.md#alerts)
- [Scale](cache-how-to-scale.md) Daha fazla ağ bant genişliği kapasitesine sahip daha büyük bir önbellek boyutuna ölçeklendirin.

## <a name="additional-information"></a>Ek bilgiler

- [Redis için Azure Cache istemci tarafı sorunlarını giderme](cache-troubleshoot-client.md)
- [Redis sunan ve boyutu için hangi Azure Önbelleğini kullanmalıyım?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
- [Önbelleğimin performansını nasıl kıyaslayabilir ve test edebilirim?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
- [Redis için Azure Önbelleği nasıl izlenir?](cache-how-to-monitor.md)
- [Redis komutlarını nasıl çalıştırabilirim?](cache-faq.md#how-can-i-run-redis-commands)
