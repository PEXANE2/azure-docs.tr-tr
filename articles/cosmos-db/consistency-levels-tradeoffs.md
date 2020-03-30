---
title: Azure Cosmos DB tutarlılığı, kullanılabilirlik ve performans dengeleri
description: Azure Cosmos DB'de çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans dengeleri.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.reviewer: sngun
ms.openlocfilehash: a16acfc8f9be820e9cc9b3bd59d6675b7f75d2ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445557"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Tutarlılık, kullanılabilirlik ve performans dengeleri 

Yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi için çoğaltmayı temel alan dağıtılan veritabanlarında bazı avantajlar arasında seçim yapılması gerekir. Bu seçim okuma tutarlılığı ve kullanılabilirlik, gecikme süresi ve aktarım hızı arasındadır.

Azure Cosmos DB, veri tutarlılığını bir seçenek yelpazesi olarak yaklaşır. Bu yaklaşım, güçlü ve nihai tutarlılık iki uç daha fazla seçenek içerir. Tutarlılık spektrumundaki iyi tanımlanmış beş model arasından seçim yapabilirsiniz. En güçlüden en zayıfa, modeller şunlardır:

- *Güçlü*
- *Sınırlı bayatlık*
- *Oturum*
- *Tutarlı önek*
- *Nihai*

Her model kullanılabilirlik ve performans dengeleri sağlar ve kapsamlı SLA'lar tarafından desteklenir.

## <a name="consistency-levels-and-latency"></a>Tutarlılık düzeyleri ve gecikme süresi

Tüm tutarlılık düzeyleri için okuma gecikmesi, 99 yüzdelik değerde her zaman 10 milisaniyeden az olması garanti edilir. Bu okuma gecikmesi SLA tarafından desteklenen. Ortalama okuma gecikmesi, 50 yüzdelik, genellikle 2 milisaniye veya daha azdır. Çeşitli bölgelere yayılan ve güçlü tutarlılıkla yapılandırılan Azure Cosmos hesapları bu garantinin bir istisnasIdır.

Tüm tutarlılık düzeyleri için yazma gecikmesi, 99 yüzdelik değerde her zaman 10 milisaniyeden az olması garanti edilir. Bu yazma gecikmesi SLA tarafından desteklenen. Ortalama yazma gecikmesi, 50 yüzdelik, genellikle 5 milisaniye veya daha azdır.

Birden fazla bölgeyle güçlü bir tutarlılıkla yapılandırılan Azure Cosmos hesapları için, yazma gecikme süresinin en uzak iki bölgeden herhangi biri arasında iki kat daha az gidiş-dönüş süresi (RTT) ve 99 yüzdelik dönemde 10 milisaniyeden az olması garanti edilir.

Tam RTT gecikmesi, ışık hızı mesafesinin ve Azure ağ topolojisinin bir fonksiyonudur. Azure ağı, rtt için iki Azure bölgesi arasında gecikme sla'ları sağlamaz. Azure Cosmos hesabınız için çoğaltma gecikmeleri Azure portalında görüntülenir. Azure Cosmos hesabınızla ilişkili çeşitli bölgeler arasındaki çoğaltma gecikmelerini izlemek için Azure portalını (Ölçümler bıçağına gidin) kullanabilirsiniz.

## <a name="consistency-levels-and-throughput"></a>Tutarlılık düzeyleri ve iş çıktısı

- Aynı sayıda istek birimi için oturum, tutarlı önek ve nihai tutarlılık düzeyleri, güçlü ve sınırlı bayatlıkla karşılaştırıldığında okuma iş lerinin yaklaşık iki katını sağlar.

- Ekleme, değiştirme, yükseltme ve silme gibi belirli bir yazma işlemi türü için, istek birimleri için yazma iş tüsuvarı tüm tutarlılık düzeyleri için aynıdır.

## <a name="consistency-levels-and-data-durability"></a><a id="rto"></a>Tutarlılık düzeyleri ve veri dayanıklılığı

Küresel olarak dağıtılan bir veritabanı ortamında, bölge çapında bir kesintinin varlığında tutarlılık düzeyi ile veri dayanıklılığı arasında doğrudan bir ilişki vardır. İş sürekliliği planınızı geliştirirken, uygulamanın rahatsız edici bir olaydan sonra tamamen iyileşmesi için kabul edilebilir maksimum zamanı anlamanız gerekir. Bir uygulamanın tam olarak kurtarılması için gereken **süre, kurtarma süresi hedefi** **(RTO)** olarak bilinir. Ayrıca, uygulamanın yıkıcı bir olaydan sonra kurtarma yaparken kaybetmeyi tolere edebileceği en son veri güncelleştirmelerinin en uzun süresini anlamanız gerekir. Kaybetmeyi göze alabileceğiniz güncelleştirmelerin süresi kurtarma **noktası hedefi** **(RPO)** olarak bilinir.

Aşağıdaki tabloda, bölge genelinde kesinti varlığında tutarlılık modeli ile veri dayanıklılığı arasındaki ilişki tanımlanıyor. Dağıtılmış bir sistemde, güçlü tutarlılıkla bile, CAP Teoremi nedeniyle RPO ve RTO sıfır olan dağıtılmış bir veritabanına sahip olmak imkansızdır. Neden hakkında daha fazla bilgi edinmek için [Azure Cosmos DB'deki Tutarlılık düzeylerine](consistency-levels.md)bakın.

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
