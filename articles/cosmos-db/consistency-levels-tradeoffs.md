---
title: Azure Cosmos DB çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans avantajları
description: Azure Cosmos DB çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans avantajları.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.reviewer: sngun
ms.openlocfilehash: 9178b8007d707af2df150102b2d344a44106a9ca
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72755177"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Tutarlılık, kullanılabilirlik ve performans dengeleri 

Yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi için çoğaltmaya dayanan dağıtılmış veritabanları, bir denge sağlamalıdır. Bu denge, okuma tutarlılığı ile kullanılabilirlik, gecikme süresi ve aktarım hızı arasındadır.

Azure Cosmos DB, veri tutarlılığını bir seçenek yelpazesi olarak yaklaşır. Bu yaklaşım, güçlü ve nihai tutarlılık için iki aşırı uç daha fazla seçenek içerir. Tutarlılık yelpazesi üzerinde beş iyi tanımlanmış model arasından seçim yapabilirsiniz. En güçlü ve en zayıf, modeller şunlardır:

- *Lemenize*
- *Sınırlanmış Eskime durumu*
- *Oturumuna*
- *Tutarlı ön ek*
- *Son*

Her model kullanılabilirlik ve performans avantajları sağlar ve kapsamlı SLA 'Lar tarafından desteklenir.

## <a name="consistency-levels-and-latency"></a>Tutarlılık düzeyleri ve gecikme

Tüm tutarlılık düzeyleri için okuma gecikmesi her zaman 99 ' luk yüzdede 10 milisaniyeden az olacak şekilde garanti edilir. Bu okuma gecikmesi SLA tarafından desteklenir. 50. yüzdede ortalama okuma gecikmesi, genellikle 2 milisaniyedir veya küçüktür. Çeşitli bölgelere yayılan ve güçlü tutarlılık ile yapılandırılmış Azure Cosmos hesapları, bu garanti için bir özel durumdur.

Tüm tutarlılık düzeyleri için yazma gecikmesi her zaman 99 ' luk yüzdede 10 milisaniyeden az olacak şekilde garanti edilir. Bu yazma gecikmesi SLA tarafından desteklenir. 50. yüzdede ortalama yazma gecikmesi genellikle 5 milisaniyedir veya küçüktür.

Birden fazla bölgeyle güçlü tutarlılık ile yapılandırılmış Azure Cosmos hesaplarında, yazma gecikmesi iki adet en uzak bölge arasında iki kat gidiş dönüş süresinden (RTT) ve 99. yüzdede 10 milisaniyeye eşit olur.

Tam RTT gecikmesi, hafif bir mesafe ve Azure ağ topolojisi 'nin bir işlevidir. Azure ağ, iki Azure bölgesi arasındaki RTT için herhangi bir gecikme SLA sağlamaz. Azure Cosmos hesabınız için çoğaltma gecikmeleri Azure portal görüntülenir. Azure Cosmos hesabınızla ilişkili çeşitli bölgeler arasındaki çoğaltma gecikme sürelerini izlemek için Azure portal (ölçümler dikey penceresine gidebilirsiniz) kullanabilirsiniz.

## <a name="consistency-levels-and-throughput"></a>Tutarlılık düzeyleri ve verimlilik

- Aynı sayıda istek birimi için, oturum, tutarlı ön ek ve nihai tutarlılık düzeyleri, güçlü ve sınırlandırılmış bir şekilde karşılaştırıldığında, okuma aktarım hızını iki kat sağlar.

- INSERT, Replace, upsert ve DELETE gibi belirli bir yazma işlemi türü için, istek birimleri için yazma üretilen işi tüm tutarlılık düzeyleri için aynıdır.

## <a id="rto"></a>Tutarlılık düzeyleri ve veri dayanıklılığı

Küresel olarak dağıtılmış bir veritabanı ortamında, bölge genelinde bir kesinti olması durumunda tutarlılık düzeyi ve veri dayanıklılığı arasında doğrudan bir ilişki vardır. İş sürekliliği planınızı geliştirirken, kesintiye uğratan bir olaydan sonra uygulamanın tam olarak kurtarmadan önce kabul edilebilir en uzun süreyi anlamanız gerekir. Uygulamanın tam olarak kurtarılması için gereken süre, **kurtarma zamanı hedefi** (**RTO**) olarak bilinir. Ayrıca, uygulamanın, kesintiye uğratan bir olaydan sonra kurtarılırken kabul edebildiği en son veri güncelleştirme süresini de anlamanız gerekir. Kaybetmemek için uygun olan güncelleştirmelerin zaman dilimi **Kurtarma noktası hedefi** (**RPO**) olarak bilinir.

Aşağıdaki tabloda, bölge genelinde kesinti olması durumunda tutarlılık modeli ve veri dayanıklılığı arasındaki ilişki tanımlanmaktadır. Dağıtılmış bir sistemde, güçlü tutarlılıkla bile, bir RPO ile dağıtılmış bir veritabanına sahip olma imkanını ve KTO 'nın, sınır sayısına bağlı olarak sıfır olmasını sağlamak önemlidir. Neden hakkında daha fazla bilgi edinmek için [Azure Cosmos DB ' deki tutarlılık düzeyleri](consistency-levels.md)' ne bakın.

|**Bölge (ler)**|**Çoğaltma modu**|**Tutarlılık düzeyi**|**RPO**|**RTO**|
|---------|---------|---------|---------|---------|
|1|Tek veya birden çok yönetici|Herhangi bir tutarlılık düzeyi|< 240 dakika|< 1 hafta|
|> 1|Tek yönetici|Oturum, tutarlı ön ek, nihai|< 15 dakika|< 15 dakika|
|> 1|Tek yönetici|Sınırlanmış Eskime Durumu|*K* & *t*|< 15 dakika|
|> 1|Tek yönetici|Strong|0|< 15 dakika|
|> 1|Çoklu yönetici|Oturum, tutarlı ön ek, nihai|< 15 dakika|0|
|> 1|Çoklu yönetici|Sınırlanmış Eskime Durumu|*K* & *t*|0|

*K* = bir öğenin *"K"* sürümlerinin (yani Updates) sayısı.

*T* = son güncelleştirmeden bu yana *"t"* zaman aralığı.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtılmış sistemlerdeki genel dağıtım ve genel tutarlılık avantajları hakkında daha fazla bilgi edinin. Aşağıdaki makalelere bakın:

- [Modern dağıtılmış veritabanı sistemleri tasarımında tutarlılık avantajları](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Yüksek kullanılabilirlik](high-availability.md)
- [SLA Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
