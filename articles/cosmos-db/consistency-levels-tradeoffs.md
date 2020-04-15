---
title: Azure Cosmos DB tutarlılığı, kullanılabilirlik ve performans dengeleri
description: Azure Cosmos DB'de çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans dengeleri.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/06/2020
ms.reviewer: sngun
ms.openlocfilehash: 7cdaa9699b15000359c438bcc410e300415b759a
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81379955"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Tutarlılık, kullanılabilirlik ve performans dengeleri

Yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi için çoğaltmayı temel alan dağıtılan veritabanlarında bazı avantajlar arasında seçim yapılması gerekir. Bu seçim okuma tutarlılığı ve kullanılabilirlik, gecikme süresi ve aktarım hızı arasındadır.

Azure Cosmos DB, veri tutarlılığını bir seçenek yelpazesi olarak yaklaşır. Bu yaklaşım, güçlü ve nihai tutarlılık iki uç daha fazla seçenek içerir. Tutarlılık spektrumundaki iyi tanımlanmış beş düzey arasından seçim yapabilirsiniz. En güçlüden en zayıfa, seviyeler şunlardır:

- *Güçlü*
- *Sınırlı bayatlık*
- *Oturum*
- *Tutarlı önek*
- *Nihai*

Her seviye kullanılabilirlik ve performans dengeleri sağlar ve kapsamlı SLA'lar tarafından desteklenir.

## <a name="consistency-levels-and-latency"></a>Tutarlılık düzeyleri ve gecikme süresi

Tüm tutarlılık düzeyleri için okuma gecikmesi, 99 yüzdelik değerde her zaman 10 milisaniyeden az olması garanti edilir. Bu okuma gecikmesi SLA tarafından desteklenen. Ortalama okuma gecikmesi, 50 yüzdelik, genellikle 4 milisaniye veya daha azdır.

Tüm tutarlılık düzeyleri için yazma gecikmesi, 99 yüzdelik değerde her zaman 10 milisaniyeden az olması garanti edilir. Bu yazma gecikmesi SLA tarafından desteklenen. Ortalama yazma gecikmesi, 50 yüzdelik, genellikle 5 milisaniye veya daha azdır. Çeşitli bölgelere yayılan ve güçlü tutarlılıkla yapılandırılan Azure Cosmos hesapları bu garantinin bir istisnasIdır.

### <a name="write-latency-and-strong-consistency"></a>Gecikme süresi ve Güçlü tutarlılık yazın

Birden fazla bölgeyle güçlü bir tutarlılıkla yapılandırılan Azure Cosmos hesapları için, yazma gecikme süresi, en uzak iki bölgeden herhangi biri arasında iki kez gidiş-dönüş süresine (RTT) ve 99 yüzdelik dönemde 10 milisaniyeye eşittir. Bölgeler arasındaki yüksek ağ RTT, güçlü tutarlılık bir işlemi yalnızca bir hesap içindeki tüm bölgelere taahhüt edildikten sonra tamamladıktan sonra tamamladığından, Cosmos DB istekleri için daha yüksek gecikme süresi anlamına gelir.

Tam RTT gecikmesi, ışık hızı mesafesinin ve Azure ağ topolojisinin bir fonksiyonudur. Azure ağı, rtt için iki Azure bölgesi arasında gecikme sla'ları sağlamaz. Azure Cosmos hesabınız için çoğaltma gecikmeleri Azure portalında görüntülenir. Azure Cosmos hesabınızla ilişkili çeşitli bölgeler arasındaki çoğaltma gecikmelerini izlemek için Azure portalını (Ölçümler sekmesine gidin, Tutarlılık sekmesini seçin) kullanabilirsiniz.

> [!IMPORTANT]
> 5000 milden (8000 kilometre) fazla alana sahip bölgelere sahip hesaplar için güçlü tutarlılık, yüksek yazma gecikmesi nedeniyle varsayılan olarak engellenir. Bu özelliği etkinleştirmek için lütfen desteğe başvurun.

## <a name="consistency-levels-and-throughput"></a>Tutarlılık düzeyleri ve iş çıktısı

- Güçlü ve sınırlı bayatlık için, okuma tutarlılık garantileri sağlamak için dört çoğaltma kümesi (azınlık çoğunluk) iki yineleme karşı yapılır. Oturum, tutarlı önek ve sonunda tek yineleme okuma yapmak. Sonuç olarak, aynı sayıda istek birimi için, güçlü ve sınırlı bayatlık için iş çıktısını okumak diğer tutarlılık düzeylerinin yarısıdır.

- Ekleme, değiştirme, yükseltme ve silme gibi belirli bir yazma işlemi türü için, istek birimleri için yazma iş tüsuvarı tüm tutarlılık düzeyleri için aynıdır.

|**Tutarlılık Düzeyi**|**Çoğunluk Okuma**|**Çoğunluk Yazıyor**|
|--|--|--|
|**Güçlü**|Yerel Azınlık|Küresel Çoğunluk|
|**Sınırlanmış Eskime Durumu**|Yerel Azınlık|Yerel Çoğunluk|
|**Oturum**|Tek Yineleme (oturum belirteci kullanarak)|Yerel Çoğunluk|
|**Tutarlı Ön Ek**|Tek Çoğaltma|Yerel Çoğunluk|
|**Nihai**|Tek Çoğaltma|Yerel Çoğunluk|

## <a name="consistency-levels-and-data-durability"></a><a id="rto"></a>Tutarlılık düzeyleri ve veri dayanıklılığı

Küresel olarak dağıtılan bir veritabanı ortamında, bölge çapında bir kesintinin varlığında tutarlılık düzeyi ile veri dayanıklılığı arasında doğrudan bir ilişki vardır. İş sürekliliği planınızı geliştirirken, uygulamanın rahatsız edici bir olaydan sonra tamamen iyileşmesi için kabul edilebilir maksimum zamanı anlamanız gerekir. Bir uygulamanın tam olarak kurtarılması için gereken **süre, kurtarma süresi hedefi** **(RTO)** olarak bilinir. Ayrıca, uygulamanın yıkıcı bir olaydan sonra kurtarma yaparken kaybetmeyi tolere edebileceği en son veri güncelleştirmelerinin en uzun süresini anlamanız gerekir. Kaybetmeyi göze alabileceğiniz güncelleştirmelerin süresi kurtarma **noktası hedefi** **(RPO)** olarak bilinir.

Aşağıdaki tabloda tutarlılık modeli ve bir bölge geniş kesintisi varlığında veri dayanıklılığı arasındaki ilişki tanımlanır. Dağıtılmış bir sistemde, güçlü tutarlılıkla bile, CAP Teoremi nedeniyle RPO ve RTO sıfır olan dağıtılmış bir veritabanına sahip olmak imkansızdır. Neden hakkında daha fazla bilgi edinmek için [Azure Cosmos DB'deki Tutarlılık düzeylerine](consistency-levels.md)bakın.

|**Bölge(ler)**|**Çoğaltma modu**|**Tutarlılık düzeyi**|**RPO**|**Rto**|
|---------|---------|---------|---------|---------|
|1|Tek veya Çok Usta|Herhangi bir Tutarlılık Düzeyi|< 240 Dakika|<1 Hafta|
|>1|Tek Master|Oturum, Tutarlı Önek, Nihai|< 15 dakika|< 15 dakika|
|>1|Tek Master|Sınırlanmış Eskime Durumu|*K* & *T*|< 15 dakika|
|>1|Tek Master|Güçlü|0|< 15 dakika|
|>1|Çok Usta|Oturum, Tutarlı Önek, Nihai|< 15 dakika|0|
|>1|Çok Usta|Sınırlanmış Eskime Durumu|*K* & *T*|0|

*K* = Bir öğenin *"K"* sürümlerinin (yani güncelleştirmelerin) sayısı.

*T* = Son güncelleştirmeden bu yana *"T"* zaman aralığı.

## <a name="strong-consistency-and-multi-master"></a>Güçlü tutarlılık ve çoklu master

Çok master için yapılandırılan cosmos hesapları, dağıtılmış bir sistemin sıfır rpo'su ve sıfır RTO'su sağlaması mümkün olmadığı için güçlü tutarlılık için yapılandırılamaz. Ayrıca, herhangi bir bölgeye herhangi bir yazma çoğaltılması ve hesap içinde tüm yapılandırılmış bölgelere taahhüt edilmelidir gibi çok büyük ile güçlü tutarlılık kullanmak için hiçbir yazma gecikme yararları vardır. Bu, tek bir ana hesapla aynı yazma gecikmesi ile sonuçlanır.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtılmış sistemlerde küresel dağıtım ve genel tutarlılık dengelemeleri hakkında daha fazla bilgi edinin. Aşağıdaki makalelere bakın:

- [Modern dağıtılmış veritabanı sistemleri tasarımında tutarlılık dengeleri](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Yüksek kullanılabilirlik](high-availability.md)
- [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
