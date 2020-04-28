---
title: Tutarlılık, kullanılabilirlik ve performans avantajları Azure Cosmos DB
description: Azure Cosmos DB çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans avantajları.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/23/2020
ms.reviewer: sngun
ms.openlocfilehash: 4de696e2538bf1fa4823aafe30f931b7852535a7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82191745"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Tutarlılık, kullanılabilirlik ve performans dengeleri

Yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi için çoğaltmayı temel alan dağıtılan veritabanlarında bazı avantajlar arasında seçim yapılması gerekir. Bu seçim okuma tutarlılığı ve kullanılabilirlik, gecikme süresi ve aktarım hızı arasındadır.

Azure Cosmos DB, veri tutarlılığını bir seçenek yelpazesi olarak yaklaşır. Bu yaklaşım, güçlü ve nihai tutarlılık için iki aşırı uç daha fazla seçenek içerir. Tutarlılık yelpazesi üzerinde beş iyi tanımlanmış düzey arasından seçim yapabilirsiniz. En güçlü ve en zayıf olan düzeyler şunlardır:

- *Güçlü*
- *Sınırlanmış Eskime durumu*
- *Oturum*
- *Tutarlı ön ek*
- *Nihai*

Her düzey kullanılabilirlik ve performans avantajları sağlar ve kapsamlı SLA 'Lar tarafından desteklenir.

## <a name="consistency-levels-and-latency"></a>Tutarlılık düzeyleri ve gecikme

Tüm tutarlılık düzeyleri için okuma gecikmesi her zaman 99 ' luk yüzdede 10 milisaniyeden az olacak şekilde garanti edilir. Bu okuma gecikmesi SLA tarafından desteklenir. 50. yüzdede ortalama okuma gecikmesi, genellikle 4 milisaniyedir veya küçüktür.

Tüm tutarlılık düzeyleri için yazma gecikmesi her zaman 99 ' luk yüzdede 10 milisaniyeden az olacak şekilde garanti edilir. Bu yazma gecikmesi SLA tarafından desteklenir. 50. yüzdede ortalama yazma gecikmesi genellikle 5 milisaniyedir veya küçüktür. Çeşitli bölgelere yayılan ve güçlü tutarlılık ile yapılandırılmış Azure Cosmos hesapları, bu garanti için bir özel durumdur.

### <a name="write-latency-and-strong-consistency"></a>Yazma gecikmesi ve güçlü tutarlılık

Birden fazla bölgeyle güçlü tutarlılık ile yapılandırılmış Azure Cosmos hesaplarında, yazma gecikmesi iki en uzak bölge arasında iki kat gidiş dönüş süresine (RTT) ve 99. yüzdede 10 milisaniyeye eşittir. Güçlü tutarlılık bir işlemi yalnızca bir hesap içindeki tüm bölgelere taahhüt ettikten sonra tamamladıktan sonra, bölgeler arasındaki yüksek ağ RTT, Cosmos DB isteklerinde daha yüksek gecikme süresine çevrilecek.

Tam RTT gecikmesi, hafif bir mesafe ve Azure ağ topolojisi 'nin bir işlevidir. Azure ağ, iki Azure bölgesi arasındaki RTT için herhangi bir gecikme SLA sağlamaz. Azure Cosmos hesabınız için çoğaltma gecikmeleri Azure portal görüntülenir. Azure Cosmos hesabınızla ilişkili çeşitli bölgeler arasındaki çoğaltma gecikme sürelerini izlemek için Azure portal (ölçümler dikey penceresine gidin, tutarlılık sekmesi ' ni seçin).

> [!IMPORTANT]
> Yüksek yazma gecikmesi nedeniyle 5000 mil 'tan fazla (8000 kiloters) yayılan bölgelere sahip hesaplar için güçlü tutarlılık, varsayılan olarak engellenir. Bu özelliği etkinleştirmek için lütfen desteğe başvurun.

## <a name="consistency-levels-and-throughput"></a>Tutarlılık düzeyleri ve verimlilik

- Güçlü ve sınırlı bir uyumluluk için, okuma, tutarlılık garantisi sağlamak üzere dört çoğaltma kümesindeki (minınlık çekirdeği) iki kopyaya karşı yapılır. Oturum, tutarlı ön ek ve nihai tek çoğaltma okumaları. Sonuç olarak, aynı sayıda istek birimi için, güçlü ve sınırlanmış eskime için okuma aktarım hızı diğer tutarlılık seviyelerinin yarısıdır.

- INSERT, Replace, upsert ve DELETE gibi belirli bir yazma işlemi türü için, istek birimleri için yazma üretilen işi tüm tutarlılık düzeyleri için aynıdır.

|**Tutarlılık Düzeyi**|**Çekirdek okuma**|**Çekirdek yazmaları**|
|--|--|--|
|**Güçlü**|Yerel Minınlık|Küresel çoğunluk|
|**Sınırlanmış Eskime Durumu**|Yerel Minınlık|Yerel çoğunluk|
|**Oturum**|Tek çoğaltma (oturum belirtecini kullanarak)|Yerel çoğunluk|
|**Tutarlı Ön Ek**|Tek çoğaltma|Yerel çoğunluk|
|**Nihai**|Tek çoğaltma|Yerel çoğunluk|

## <a name="consistency-levels-and-data-durability"></a><a id="rto"></a>Tutarlılık düzeyleri ve veri dayanıklılığı

Küresel olarak dağıtılmış bir veritabanı ortamında, bölge genelinde bir kesinti olması durumunda tutarlılık düzeyi ve veri dayanıklılığı arasında doğrudan bir ilişki vardır. İş sürekliliği planınızı geliştirirken, kesintiye uğratan bir olaydan sonra uygulamanın tam olarak kurtarmadan önce kabul edilebilir en uzun süreyi anlamanız gerekir. Uygulamanın tam olarak kurtarılması için gereken süre, **kurtarma zamanı hedefi** (**RTO**) olarak bilinir. Ayrıca, uygulamanın, kesintiye uğratan bir olaydan sonra kurtarılırken kabul edebildiği en son veri güncelleştirme süresini de anlamanız gerekir. Kaybetmemek için uygun olan güncelleştirmelerin zaman dilimi **Kurtarma noktası hedefi** (**RPO**) olarak bilinir.

Aşağıdaki tabloda, bölge genelinde kesinti olması durumunda tutarlılık modeli ve veri dayanıklılığı arasındaki ilişki tanımlanmaktadır. Dağıtılmış bir sistemde, güçlü tutarlılıkla bile, bir RPO ile dağıtılmış bir veritabanına sahip olma imkanını ve KTO 'nın, sınır sayısına bağlı olarak sıfır olmasını sağlamak önemlidir. Neden hakkında daha fazla bilgi edinmek için [Azure Cosmos DB ' deki tutarlılık düzeyleri](consistency-levels.md)' ne bakın.

|**Bölge (ler)**|**Çoğaltma modu**|**Tutarlılık düzeyi**|**RPO**|**RTO**|
|---------|---------|---------|---------|---------|
|1|Tek veya birden çok yönetici|Herhangi bir tutarlılık düzeyi|< 240 dakika|<1 hafta|
|>1|Tek yönetici|Oturum, tutarlı ön ek, nihai|< 15 dakika|< 15 dakika|
|>1|Tek yönetici|Sınırlanmış Eskime Durumu|*K* & *T*|< 15 dakika|
|>1|Tek yönetici|Güçlü|0|< 15 dakika|
|>1|Çoklu yönetici|Oturum, tutarlı ön ek, nihai|< 15 dakika|0|
|>1|Çoklu yönetici|Sınırlanmış Eskime Durumu|*K* & *T*|0|

*K* = bir öğenin *"K"* sürümlerinin (yani Updates) sayısı.

*T* = son güncelleştirmeden bu yana *"t"* zaman aralığı.

## <a name="strong-consistency-and-multi-master"></a>Güçlü tutarlılık ve çoklu yönetici

Birden çok yönetici için yapılandırılmış Cosmos hesapları, dağıtılmış bir sistem için sıfır ve 0 ' ın bir RPO sağlaması mümkün olmadığından, güçlü tutarlılık için yapılandırılamaz. Ayrıca, herhangi bir bölgedeki herhangi bir yazma çoğaltılması ve hesaptaki tüm yapılandırılmış bölgelere yürütülmesi için, çok yöneticili ile güçlü tutarlılık kullanılmasına yönelik yazma gecikme avantajları yoktur. Bu, tek bir ana hesapla aynı yazma gecikmesine neden olur.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtılmış sistemlerdeki genel dağıtım ve genel tutarlılık avantajları hakkında daha fazla bilgi edinin. Aşağıdaki makalelere bakın:

- [Modern dağıtılmış veritabanı sistemleri tasarımında tutarlılık avantajları](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Yüksek kullanılabilirlik](high-availability.md)
- [SLA Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
