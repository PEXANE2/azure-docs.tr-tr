---
title: Azure Cosmos DB'deki tutarlılık düzeyleri
description: Azure Cosmos DB Bakiye nihai tutarlılık, kullanılabilirlik ve gecikme süresi dengelemeler yardımcı olmak üzere beş tutarlılık düzeyi vardır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.openlocfilehash: b5d9df7a0afa9b4270f0eff643e083e5bccfceb8
ms.sourcegitcommit: e6bce4b30486cb19a6b415e8b8442dd688ad4f92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78933739"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Azure Cosmos DB'deki tutarlılık düzeyleri

Yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi de çoğaltma kullanan dağıtılmış veritabanları kullanılabilirlik, gecikme süresi ve aktarım hızı ve okuma tutarlılığı temel etmekten olun. Ticari olarak kullanılabilen en çok dağıtılmış veritabanları, geliştiricilerin iki üstün tutarlılık modeli arasından seçim yapmasını ister: *güçlü* tutarlılık ve *nihai* tutarlılık. Doğrizlebilirlik veya güçlü tutarlılık modeli, veri programlamasına yönelik altın standarttır. Ancak, daha yüksek gecikme süresi (düzenli durumda) ve düşük kullanılabilirlik (hatalarda) sağlar. Öte yandan, nihai tutarlılık daha yüksek kullanılabilirlik ve daha iyi performans sunar, ancak uygulamaların program tarafından kullanılabilmesini sağlar. 

Azure Cosmos DB olarak içeren iki uç nokta yerine seçimleri, veri tutarlılığını yaklaşıyor. Güçlü tutarlılık ve nihai tutarlılık, tayfın uçlarından, ancak SPI üzerinde birçok tutarlılık seçeneği vardır. Geliştiriciler, yüksek kullanılabilirlik ve performans açısından kesin seçimler ve ayrıntılı bir denge sağlamak için bu seçenekleri kullanabilir. 

Azure Cosmos DB ile geliştiriciler beş iyi tanımlanmış tutarlılık modeli üzerinde tutarlılık spektrumu arasından seçim yapabilirsiniz. En güçlü ve daha gevşek bir şekilde modeller, *güçlü*, *sınırlanmış Eskime durumu*, *oturum*, *tutarlı ön ek*ve *nihai* tutarlılık içerir. Modeller iyi tanımlanmış ve sezgisel olur ve belirli gerçek dünyada senaryolar için kullanılabilir. Her model [kullanılabilirlik ve performans avantajları](consistency-levels-tradeoffs.md) sağlar ve SLA 'lar tarafından desteklenir. Aşağıdaki görüntüde, farklı tutarlılık düzeyleri bir speklik olarak gösterilmiştir.

![Tutarlılık spektrumu olarak](./media/consistency-levels/five-consistency-levels.png)

Tutarlılık düzeyleri bölge açısından belirsizdir ve okuma ve yazma işlemleri, Azure Cosmos hesabınızla ilişkili bölge sayısı veya hesabınızın tek bir ile yapılandırılıp yapılandırılmadığını dikkate almaksızın tüm işlemler için garanti edilir. veya birden fazla yazma bölgesi.

## <a name="scope-of-the-read-consistency"></a>Okuma tutarlılığı kapsamı

Bölüm anahtar aralığı veya bir mantıksal bölüm içinde kapsamlı bir tek okuma işlemi okuma tutarlılığı uygular. Okuma işlemi, uzak bir istemci veya saklı yordam tarafından verilebilir.

## <a name="configure-the-default-consistency-level"></a>Varsayılan tutarlılık düzeyini yapılandırma

İstediğiniz zaman Azure Cosmos hesabınızdaki varsayılan tutarlılık düzeyini yapılandırabilirsiniz. Hesabınızda yapılandırılan varsayılan tutarlılık düzeyi, bu hesap altındaki tüm Azure Cosmos veritabanları ve kapsayıcıları için geçerlidir. Tüm okuma ve verilen bir kapsayıcı veya bir veritabanına karşı sorgular belirtilen tutarlılık düzeyi varsayılan olarak kullanır. Daha fazla bilgi için bkz. [varsayılan tutarlılık düzeyini yapılandırma](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Tutarlılık düzeyleri ile ilişkili garanti eder

Yüzde 100 okuma isteklerinin seçtiğiniz herhangi bir tutarlılık düzeyi için tutarlılık garantisini karşılayan Azure Cosmos DB garantisi tarafından sağlanan kapsamlı SLA'lar. Tutarlılık düzeyi ile ilişkili tüm tutarlılık garantileri sağlanırsa Okuma isteği tutarlılık SLA karşılar. [Azure-Cosmos-TLA](https://github.com/Azure/azure-cosmos-tla) GitHub deposunda Azure Cosmos DB içindeki beş tutarlılık DÜZEYININ, TLA + belirtim dilini kullanarak kesin tanımları sunulmaktadır.

Beş tutarlılık düzeyi semantiği aşağıda açıklanmıştır:

- **Güçlü**: güçlü tutarlılık, bir doğrizlebilirlik garantisi sunar. Doğrizlebilirlik istekleri aynı anda sunma anlamına gelir. Okuma işlemleri, bir öğe işlenen en son sürümünü döndürmek için garanti edilir. Bir istemci hiçbir zaman işlenmemiş ya da kısmi bir yazma görür. Kullanıcıların her zaman en son kabul edilen yazma okumak için garanti edilir.

  Aşağıdaki grafikte, müzik notlarıyla güçlü tutarlılık gösterilmektedir. Veriler "Doğu ABD" bölgesine yazıldıktan sonra, diğer bölgelerden gelen verileri okurken en son değeri alırsınız:

  ![video](media/consistency-levels/strong-consistency.gif)

- **Sınırlanmış Eskime durumu**: okumaların tutarlı ön ek garantisi kabul edilmesi garanti edilir. Okuma işlemleri, bir öğenin en çok *"K"* sürümünde (yani, "Güncelleştirmeler") veya *"T"* zaman aralığına göre yazma işlemlerinin arkasında kalabilir. Diğer bir deyişle, sınırlanmış Eskime durumu ' nu seçtiğinizde, "stalet" iki şekilde yapılandırılabilir: 

  * Öğenin sürüm sayısı (*K*)
  * Okumaların yazma işlemlerinin arkasında gecikme olabileceği zaman aralığı (*T*) 

  Sınırlanmış eskime durumu teklifler toplam genel sıra dışında "eskime durumu penceresi içinde." Monoton okuma garantisi, hem Azure içindeki hem eskime durumu penceresi dışında bir bölge içinde mevcut. Güçlü tutarlılık, sınırlı stalet tarafından sunulan ile aynı semantiğe sahiptir. Eskime durumu penceresi sıfıra eşittir. Sınırlanmış eskime durumu, doğrusallaştırılabilme zaman Gecikmeli da bilinir. Bir istemci, yazmaları kabul eden bir bölgede okuma işlemleri gerçekleştirdiğinde, sınırlı stalet tutarlılığı tarafından sunulan garantiler, güçlü tutarlılık açısından bu garantilerle aynıdır.

  Sınırlı Eskime durumu, genellikle düşük yazma gecikmeleri bekleyen ancak toplam genel sipariş garantisi gerektiren küresel olarak dağıtılan uygulamalar tarafından seçilir. Sınırlı stalet, grup işbirliği ve paylaşım, stok şeridi, yayımlama-abonelik/sıraya alma gibi uygulamalar için harika bir yöntemdir. Aşağıdaki grafik, müzik notları ile sınırlı stalet tutarlılığını gösterir. Veriler "Doğu ABD" bölgesine yazıldıktan sonra, "Batı ABD" ve "Avustralya Doğu" bölgeleri, yapılandırılan en fazla gecikme süresine veya en yüksek işlemlere göre yazılan değeri okur:

  ![video](media/consistency-levels/bounded-staleness-consistency.gif)

- **Oturum**: tek bir istemci oturumu okumalarının içinde, tutarlı ön ek (tek bir "yazıcı" oturumu varsayılıyor), monoton okuma, monoton yazmaları, okuma-yazma ve yazma işlemleri için bir okuma garantisi garanti edilir. Oturum yazma işlemi dışındaki istemciler nihai tutarlılığı görür.

  Oturum tutarlılığı, hem tek bölge hem de küresel olarak dağıtılan uygulamalar için yaygın olarak kullanılan tutarlılık düzeyidir. Nihai tutarlılık ile karşılaştırılabilir yazma gecikmeleri, kullanılabilirlik ve okuma üretilen işi sağlar, ancak aynı zamanda bir kullanıcı bağlamında çalışmak üzere yazılmış uygulamaların ihtiyaçlarına uygun tutarlılık garantisi sağlar. Aşağıdaki grafik, müzik notları ile oturum tutarlılığını göstermektedir. "Batı ABD" bölgesi ve "Doğu ABD" bölgeleri aynı oturumu (oturum A) kullanarak her ikisi de aynı anda verileri okuyabilirler. "Avustralya Doğu" bölgesi "oturum B" kullandığından, verileri daha sonra, yazma işlemleri ile aynı sırada alır.

  ![video](media/consistency-levels/session-consistency.gif)

- **Tutarlı ön ek**: döndürülen güncelleştirmeler boşluklar olmadan tüm güncelleştirmelerin bazı ön eklerini içerir. Tutarlı ön ek tutarlılık düzeyi, okumaların hiçbir şekilde sıra dışı yazmaları görmeme garantisi sağlar.

  Sıralamada `A, B, C`, bir istemci `A`, `A,B`veya `A,B,C`görür, ancak hiçbir şekilde `A,C` veya `B,A,C`gibi bir sıra değil. Tutarlı ön ek, nihai tutarlılık ile karşılaştırılabilir yazma gecikmeleri, kullanılabilirlik ve okuma üretilen işi sağlar, ancak aynı zamanda siparişin önemli olduğu senaryoların ihtiyaçlarına uygun sıra garantisi sağlar. Aşağıdaki grafikte, müzik notlarıyla tutarlılık ön eki tutarlılığı gösterilmektedir. Tüm bölgelerde okumalar hiçbir şey yazma sırası görmez:

  ![video](media/consistency-levels/consistent-prefix.gif)

- **Nihai**: okumalar için sıralama garantisi yoktur. Başka yazma işlemlerinin olmaması durumunda, yineleme sonunda birbirine yaklaşır.  
Son tutarlılık, bir istemci, daha önce okuduğundan daha eski olan değerleri okuyabileceğinden en zayıf tutarlılık biçimidir. Nihai tutarlılık, uygulamanın herhangi bir sıralama garantisi gerektirmediğinden idealdir. Örnek olarak yeniden doldurulabilir, beğeni veya iş parçacıklı yorumların sayısını verilebilir. Aşağıdaki grafikte, müzik notlarıyla nihai tutarlılık gösterilmektedir.

  ![video](media/consistency-levels/eventual-consistency.gif)

## <a name="additional-reading"></a>Ek okuma

Tutarlılık kavramları hakkında daha fazla bilgi edinmek için bu makaleleri okuyun:

- [Azure Cosmos DB tarafından sunulan beş tutarlılık düzeyi için üst düzey TLA + belirtimleri](https://github.com/Azure/azure-cosmos-tla)
- [Doug Terry tarafından bebey(video) Ile açıklanan çoğaltılan veri tutarlılığı](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Doug Terry tarafından bebey(Teknik İnceleme) Ile açıklanan çoğaltılan veri tutarlılığı](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Zayıf tutarlı çoğaltılan veriler için oturum garantisi](https://dl.acm.org/citation.cfm?id=383631)
- [Modern dağıtılmış veritabanı sistemleri tasarımında tutarlılık avantajları: CAP yalnızca hikayenin bir parçasıdır](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Pratik kısmi çekirdeklerine için dayalı sınırlanmış Stalet (PBS)](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Sonuçta tutarlı yeniden ziyaret edildi](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'deki tutarlılık düzeyleri hakkında daha fazla bilgi edinmek için bu makaleleri okuyun:

* [Uygulamanız için doğru tutarlılık düzeyini seçin](consistency-levels-choosing.md)
* [Azure Cosmos DB API 'lerde tutarlılık düzeyleri](consistency-levels-across-apis.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans avantajları](consistency-levels-tradeoffs.md)
* [Varsayılan tutarlılık düzeyini yapılandırma](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Varsayılan tutarlılık düzeyini geçersiz kıl](how-to-manage-consistency.md#override-the-default-consistency-level)

