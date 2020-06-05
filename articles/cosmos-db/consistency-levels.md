---
title: Azure Cosmos DB 'deki tutarlılık düzeyleri
description: Azure Cosmos DB nihai tutarlılık, kullanılabilirlik ve gecikme süreleri dengelemeye yardımcı olmak için beş tutarlılık düzeyi vardır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 640884168aef13ef3a8d751c211bf16756ff350f
ms.sourcegitcommit: 8e5b4e2207daee21a60e6581528401a96bfd3184
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84417897"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Azure Cosmos DB 'deki tutarlılık düzeyleri

Yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi için çoğaltmaya bağlı dağıtılmış veritabanları, okuma tutarlılığı ile kullanılabilirlik, gecikme süresi ve aktarım hızı arasında temel zorunluluğunu getirir. Ticari olarak kullanılabilen en çok dağıtılmış veritabanları, geliştiricilerin iki üstün tutarlılık modeli arasından seçim yapmasını ister: *güçlü* tutarlılık ve *nihai* tutarlılık. Güçlü tutarlılık modelinin doğrizlebilirlik, veri programlamasına yönelik altın standarttır. Ancak, daha yüksek yazma gecikme süresi (düzenli durumda) ve düşük kullanılabilirlik (hatalarda) sağlar. Öte yandan, nihai tutarlılık daha yüksek kullanılabilirlik ve daha iyi performans sunar, ancak uygulamaların program tarafından kullanılabilmesini sağlar.

Azure Cosmos DB, veri tutarlılığını iki aşırı uç yerine bir seçenek yelpazesi olarak yaklaşır. Geliştiriciler, yüksek kullanılabilirlik ve performans açısından kesin seçimler ve ayrıntılı bir denge sağlamak için bu seçenekleri kullanabilir.

Azure Cosmos DB, geliştiriciler tutarlılık yelpazesi üzerinde beş iyi tanımlanmış tutarlılık düzeyinden birini seçebilirler. Bu düzeyler *güçlü*, *sınırlanmış Eskime durumu*, *oturum*, *tutarlı ön ek*ve *nihai* tutarlılık içerir. Düzeyler iyi tanımlanmış ve sezgisel olur ve belirli gerçek dünyada senaryolar için kullanılabilir. Her düzey [kullanılabilirlik ve performans avantajları](consistency-levels-tradeoffs.md) sağlar ve SLA 'lar tarafından desteklenir. Aşağıdaki görüntüde, farklı tutarlılık düzeyleri bir speklik olarak gösterilmiştir.

![Bir spekme olarak tutarlılık](./media/consistency-levels/five-consistency-levels.png)

Tutarlılık düzeyleri bölge açısından belirsizdir ve okuma ve yazma işlemleri, Azure Cosmos hesabınızla ilişkili bölge sayısı veya hesabınızın tek veya birden çok yazma bölgesiyle yapılandırılıp yapılandırılmadığını bağımsız olarak tüm işlemler için garanti edilir.

## <a name="scope-of-the-read-consistency"></a>Okuma tutarlılığı kapsamı

Okuma tutarlılığı, mantıksal bir bölüm kapsamındaki tek bir okuma işlemi için geçerlidir. Okuma işlemi bir uzak istemci veya saklı yordam tarafından verilebilir.

## <a name="configure-the-default-consistency-level"></a>Varsayılan tutarlılık düzeyini yapılandırma

Azure Cosmos hesabınızda varsayılan tutarlılık düzeyini dilediğiniz zaman yapılandırabilirsiniz. Hesabınızda yapılandırılan varsayılan tutarlılık düzeyi, bu hesap altındaki tüm Azure Cosmos veritabanları ve kapsayıcıları için geçerlidir. Bir kapsayıcıya veya bir veritabanına karşı verilen tüm okuma ve sorgular varsayılan olarak belirtilen tutarlılık düzeyini kullanır. Daha fazla bilgi için bkz. [varsayılan tutarlılık düzeyini yapılandırma](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Tutarlılık düzeyleriyle ilişkili garanti garantisi

Azure Cosmos DB tarafından sunulan kapsamlı SLA 'Lar, okuma isteklerinin yüzde 100 ' unun seçtiğiniz herhangi bir tutarlılık düzeyi için tutarlılık garantisi karşıladığını garanti eder. Tutarlılık düzeyiyle ilişkili tüm tutarlılık garantisi karşılandıysa, bir okuma isteği tutarlılık SLA 'sını karşılar. [Azure-Cosmos-TLA](https://github.com/Azure/azure-cosmos-tla) GitHub deposunda Azure Cosmos DB içindeki beş tutarlılık DÜZEYININ, TLA + belirtim dilini kullanarak kesin tanımları sunulmaktadır.

Beş tutarlılık düzeyinin semantiği aşağıda açıklanmıştır:

- **Güçlü**: güçlü tutarlılık, bir doğrizlebilirlik garantisi sunar. Doğrizlebilirlik istekleri aynı anda sunma anlamına gelir. Okumaların, bir öğenin en son kaydedilmiş sürümünü döndürmesi garanti edilir. İstemci hiçbir şekilde işlenmemiş veya kısmi yazma görmez. Kullanıcıların her zaman en son yürütülen yazma okuma garantisi vardır.

  Aşağıdaki grafikte, müzik notlarıyla güçlü tutarlılık gösterilmektedir. Veriler "Batı ABD 2" bölgesine yazıldıktan sonra, diğer bölgelerden gelen verileri okurken en son değeri alırsınız:

  ![video](media/consistency-levels/strong-consistency.gif)

- **Sınırlanmış Eskime durumu**: okumaların tutarlı ön ek garantisi kabul edilmesi garanti edilir. Okuma işlemleri, bir öğenin en çok *"K"* sürümünde (yani, "Güncelleştirmeler") veya *"T"* zaman aralığına göre yazma işlemlerinin arkasında kalabilir. Diğer bir deyişle, sınırlanmış Eskime durumu ' nu seçtiğinizde, "stalet" iki şekilde yapılandırılabilir:

- Öğenin sürüm sayısı (*K*)
- Okumaların yazma işlemlerinin arkasında gecikme olabileceği zaman aralığı (*T*)

Sınırlanmış stalet, "stalet penceresi" dışında toplam genel sıra sunar. Bir istemci, yazmaları kabul eden bir bölgede okuma işlemleri gerçekleştirdiğinde, sınırlı stalet tutarlılığı tarafından sunulan garantiler, güçlü tutarlılık açısından bu garantilerle aynıdır.

Stalet penceresinin içinde, sınırlanmış Eskime durumu aşağıdaki tutarlılık garantisi sağlar:

- Tek yöneticili hesap için aynı bölgedeki istemciler için tutarlılık = Strong
- Tek yöneticili bir hesap için farklı bölgelerdeki istemciler için tutarlılık = tutarlı ön ek
- Birden çok ana hesap için tek bir bölgeye yazan istemciler için tutarlılık = tutarlı ön ek
- Birden çok ana hesap için farklı bölgelere yazan istemciler için tutarlılık = son

  Sınırlı Eskime durumu, genellikle düşük yazma gecikmeleri bekleyen ancak toplam genel sipariş garantisi gerektiren küresel olarak dağıtılan uygulamalar tarafından seçilir. Sınırlı stalet, grup işbirliği ve paylaşım, stok şeridi, yayımlama-abonelik/sıraya alma gibi uygulamalar için harika bir yöntemdir. Aşağıdaki grafik, müzik notları ile sınırlı stalet tutarlılığını gösterir. Veriler "Batı ABD 2" bölgesine yazıldıktan sonra, "Doğu ABD 2" ve "Avustralya Doğu" bölgeleri, yapılandırılan en fazla gecikme süresine veya en yüksek işlemlere göre yazılan değeri okur:

  ![video](media/consistency-levels/bounded-staleness-consistency.gif)

- **Oturum**: tek bir istemci oturumu okumalarının içinde, tutarlı ön ek, monoton okumaları, monoton yazmaları, okuma-yazma ve yazma ile okuma garantisi için kabul edilmesi garanti edilir. Bu, tek bir "yazıcı" oturumunun olduğunu varsayar veya birden çok yazıcı için oturum belirtecini paylaşıyor.

Oturum yazma işlemi dışındaki istemciler aşağıdaki garantilere sahip olur:

- Tek yöneticili bir hesap için aynı bölgedeki istemciler için tutarlılık = tutarlı ön ek
- Tek yöneticili bir hesap için farklı bölgelerdeki istemciler için tutarlılık = tutarlı ön ek
- Birden çok ana hesap için tek bir bölgeye yazan istemciler için tutarlılık = tutarlı ön ek
- Çoklu yönetici hesabı için birden çok bölgeye yazan istemciler için tutarlılık = son

  Oturum tutarlılığı, hem tek bölge hem de küresel olarak dağıtılan uygulamalar için en yaygın olarak kullanılan tutarlılık düzeyidir. Nihai tutarlılık ile karşılaştırılabilir yazma gecikmeleri, kullanılabilirlik ve okuma üretilen işi sağlar, ancak aynı zamanda bir kullanıcı bağlamında çalışmak üzere yazılmış uygulamaların ihtiyaçlarına uygun tutarlılık garantisi sağlar. Aşağıdaki grafik, müzik notları ile oturum tutarlılığını göstermektedir. "Batı ABD 2 yazıcısı" ve "Batı ABD 2 okuyucu" aynı oturumu (oturum A) kullanıyor, böylece her ikisi de aynı anda aynı verileri okur. "Avustralya Doğu" bölgesi "oturum B" kullandığından, verileri daha sonra ve yazma sırasıyla aynı sırada alır.

  ![video](media/consistency-levels/session-consistency.gif)

- **Tutarlı ön ek**: döndürülen güncelleştirmeler boşluklar olmadan tüm güncelleştirmelerin bazı ön eklerini içerir. Tutarlı ön ek tutarlılık düzeyi, okumaların hiçbir şekilde sıra dışı yazmaları görmeme garantisi sağlar.

Sıralamada yazma işlemleri gerçekleştirildiyse, istemci, veya `A, B, C` `A` `A,B` `A,B,C` gibi hiçbir bir sıra dışı permütasyon görür `A,C` `B,A,C` . Tutarlı ön ek, nihai tutarlılık ile karşılaştırılabilir yazma gecikmeleri, kullanılabilirlik ve okuma üretilen işi sağlar, ancak aynı zamanda siparişin önemli olduğu senaryoların ihtiyaçlarına uygun sıra garantisi sağlar. 

Tutarlı ön ek için tutarlılık garantisi aşağıda verilmiştir:

- Tek yöneticili bir hesap için aynı bölgedeki istemciler için tutarlılık = tutarlı ön ek
- Tek yöneticili bir hesap için farklı bölgelerdeki istemciler için tutarlılık = tutarlı ön ek
- Birden çok ana hesap için tek bir bölgeye yazan istemciler için tutarlılık = tutarlı ön ek
- Çoklu yönetici hesabı için birden çok bölgeye yazan istemciler için tutarlılık = son

Aşağıdaki grafikte, müzik notlarıyla tutarlılık ön eki tutarlılığı gösterilmektedir. Tüm bölgelerde okumalar hiçbir şey yazma sırası görmez:

  ![video](media/consistency-levels/consistent-prefix.gif)

- **Nihai**: okumalar için sıralama garantisi yoktur. Diğer yazma işlemleri yokluğunda çoğaltmalar, sonunda da yakınsama yapılır.  
Son tutarlılık, bir istemci, daha önce okuduğundan daha eski olan değerleri okuyabileceğinden en zayıf tutarlılık biçimidir. Nihai tutarlılık, uygulamanın herhangi bir sıralama garantisi gerektirmediğinden idealdir. Örnek olarak yeniden doldurulabilir, beğeni veya iş parçacıklı yorumların sayısını verilebilir. Aşağıdaki grafikte, müzik notlarıyla nihai tutarlılık gösterilmektedir.

  ![video](media/consistency-levels/eventual-consistency.gif)

## <a name="additional-reading"></a>Ek okuma

Tutarlılık kavramları hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri okuyun:

- [Azure Cosmos DB tarafından sunulan beş tutarlılık düzeyi için üst düzey TLA + belirtimleri](https://github.com/Azure/azure-cosmos-tla)
- [Doug Terry tarafından bebey(video) Ile açıklanan çoğaltılan veri tutarlılığı](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Doug Terry tarafından bebey(Teknik İnceleme) Ile açıklanan çoğaltılan veri tutarlılığı](https://www.microsoft.com/research/publication/replicated-data-consistency-explained-through-baseball/)
- [Zayıf tutarlı çoğaltılan veriler için oturum garantisi](https://dl.acm.org/citation.cfm?id=383631)
- [Modern dağıtılmış veritabanı sistemleri tasarımında tutarlılık avantajları: CAP yalnızca hikayenin bir parçasıdır](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Pratik kısmi çekirdeklerine için dayalı sınırlanmış Stalet (PBS)](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Sonuçta tutarlı yeniden ziyaret edildi](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB 'deki tutarlılık düzeyleri hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri okuyun:

- [Uygulamanız için doğru tutarlılık düzeyini seçin](consistency-levels-choosing.md)
- [Azure Cosmos DB API 'lerde tutarlılık düzeyleri](consistency-levels-across-apis.md)
- [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans avantajları](consistency-levels-tradeoffs.md)
- [Varsayılan tutarlılık düzeyini yapılandırma](how-to-manage-consistency.md#configure-the-default-consistency-level)
- [Varsayılan tutarlılık düzeyini geçersiz kılma](how-to-manage-consistency.md#override-the-default-consistency-level)
