---
title: Azure Cosmos DB uygulamanız için doğru tutarlılık düzeyini seçin
description: Azure Cosmos DB'deki uygulamanız için doğru tutarlılık düzeyini seçme.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.reviewer: sngun
ms.openlocfilehash: cea157e272a2bf464141e592b8e742f314a83233
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75441914"
---
# <a name="choose-the-right-consistency-level"></a>Doğru tutarlılık düzeyini seçme 

Yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi için çoğaltmaya dayanan dağıtılmış veritabanları, okuma tutarlılığı ile kullanılabilirlik, gecikme süresi ve üretim arasındaki temel dengeyi sağlar. Ticari olarak kullanılabilen dağıtılmış veritabanlarının çoğu geliştiricilerden iki aşırı tutarlılık modeli arasında seçim yapmalarını ister: *güçlü* tutarlılık ve *nihai* tutarlılık. Azure Cosmos DB geliştiriciler beş iyi tanımlanmış tutarlılık modelleri arasında seçim yapmanızı sağlar: *güçlü*, *sınırlı bayatlık*, *oturum*, *tutarlı önek* ve *nihai*. Bu tutarlılık modellerinin her biri iyi tanımlanmış, sezgiseldir ve belirli gerçek dünya senaryoları için kullanılabilir. Beş tutarlılık modelinin her biri hassas [kullanılabilirlik ve performans dengeleri](consistency-levels-tradeoffs.md) sağlar ve kapsamlı SLA'lar tarafından desteklenir. Aşağıdaki basit hususlar birçok yaygın senaryoda doğru seçimi yapmak yardımcı olacaktır.

## <a name="sql-api-and-table-api"></a>SQL API ve Tablo API'si

Uygulamanız SQL API veya Tablo API'si kullanılarak oluşturulmuşsa aşağıdaki noktaları göz önünde bulundurun:

- Birçok gerçek dünya senaryosu için oturum tutarlılığı en uygun maliyetlidir ve önerilen seçenektir. Daha fazla bilgi için bkz: [Uygulamanız için oturum belirteci nasıl yönetilir.](how-to-manage-consistency.md#utilize-session-tokens)

- Uygulamanız güçlü tutarlılık gerektiriyorsa, sınırlı bayatlık tutarlılık düzeyi kullanmanız önerilir.

- Oturum tutarlılığı ve yazmalar için tek basamaklı-milisaniye gecikmesi tarafından sağlananlardan daha sıkı tutarlılık garantilerine ihtiyacınız varsa, sınırlı bayatlık tutarlılık düzeyi kullanmanız önerilir.  

- Uygulamanız nihai tutarlılık gerektiriyorsa, tutarlı önek tutarlılığı düzeyi kullanmanız önerilir.

- Oturum tutarlılığı tarafından sağlananlardan daha az sıkı tutarlılık garantilerine ihtiyacınız varsa, tutarlı önek tutarlılık düzeyi kullanmanız önerilir.

- En yüksek kullanılabilirliğe ve en düşük gecikme süresine ihtiyacınız varsa, nihai tutarlılık düzeyini kullanın.

- Performanstan ödün vermeden daha yüksek veri dayanıklılığına ihtiyacınız varsa, uygulama katmanında özel bir tutarlılık düzeyi oluşturabilirsiniz. Daha fazla bilgi için bkz: [Uygulamalarınızda özel senkronizasyon nasıl uygulanır.](how-to-custom-synchronization.md)

## <a name="cassandra-mongodb-and-gremlin-apis"></a>Cassandra, MongoDB ve Gremlin API'leri

- Apache Cassandra ve Cosmos DB tutarlılık düzeylerinde sunulan "Tutarlılık Düzeyini Oku" arasındaki haritalama yla ilgili ayrıntılar için [Tutarlılık düzeyleri ve Cosmos DB API'leri'ne](consistency-levels-across-apis.md#cassandra-mapping)bakın.

- MongoDB'nin "Okuma Endişesi" ve Azure Cosmos DB tutarlılık düzeyleri arasındaki haritalama yla ilgili ayrıntılar için [Tutarlılık düzeyleri ve Cosmos DB API'leri'ne](consistency-levels-across-apis.md#mongo-mapping)bakın.

## <a name="consistency-guarantees-in-practice"></a>Uygulamada tutarlılık garantileri

Uygulamada, genellikle daha güçlü tutarlılık garantileri alabilirsiniz. Okuma işlemi için tutarlılık garantileri, istediğiniz veritabanı durumunun tazeliğine ve siparişine karşılık gelir. Okuma tutarlılığı, yazma/güncelleştirme işlemlerinin sıralanmasına ve yayılmasına bağlıdır.  

* Tutarlılık düzeyi **bayatlık sınırlı**olarak ayarlandığında, Cosmos DB müşterilerin her zaman bir önceki yazma değerini okumak garanti, bayatlık penceresi ile sınırlı bir gecikme ile.

* Tutarlılık düzeyi **güçlü**olarak ayarlandığında, bayatlık penceresi sıfıra eşdeğerdir ve istemcilerin yazma işleminin en son taahhüt edilen değerini okumaları garanti edilir.

* Kalan üç tutarlılık düzeyi için, bayatlık penceresi büyük ölçüde iş yükünüze bağlıdır. Örneğin, veritabanında yazma işlemi yoksa, **nihai**, **oturum**veya **tutarlı önek** tutarlılık düzeyleriiçeren bir okuma işlemi, güçlü tutarlılık düzeyine sahip bir okuma işlemiyle aynı sonuçları verir.

Azure Cosmos hesabınız güçlü tutarlılık dışında bir tutarlılık düzeyiyle yapılandırılırsa, *Probabilistically Bounded Staleness* (PBS) ölçüsüne bakarak müşterilerinizin iş yükleri için güçlü ve tutarlı okumalar alma olasılığını öğrenebilirsiniz. Bu metrik, daha fazla bilgi edinmek için Azure portalında açıklanır, [bkz.](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)

Olasılıklı sınırlı bayatlık ne kadar nihai tutarlılık olduğunu gösterir. Bu metrik, Azure Cosmos hesabınızda şu anda yapılandırdığınız tutarlılık düzeyine göre ne sıklıkta daha güçlü bir tutarlılık elde edebileceğinize dair bir fikir sağlar. Başka bir deyişle, yazma ve okuma bölgelerinin birleşimi için güçlü tutarlı okumalar alma olasılığını (milisaniye cinsinden ölçülür) görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde tutarlılık düzeyleri hakkında daha fazla bilgi edinin:

* [Cosmos DB API'leri arasında tutarlılık düzeyi eşleme](consistency-levels-across-apis.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans dengeleri](consistency-levels-tradeoffs.md)
* [Başvurunuz için oturum belirteci nasıl yönetilir?](how-to-manage-consistency.md#utilize-session-tokens)
* [Olasılığa Dayalı Sınırlanmış Eskime Durumu (PBS) ölçümünü izleme](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)
