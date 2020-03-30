---
title: Azure Cosmos DB NoSQL ve ilişkisel veritabanları arasındaki farkları anlama
description: Bu makalede, NoSQL ve ilişkisel veritabanları arasındaki farkları sıralar
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/16/2019
ms.reviewer: sngun
ms.openlocfilehash: 1cd80fee51565f2a2c1afa38ed883c10f51a5ee3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75896629"
---
# <a name="understanding-the-differences-between-nosql-and-relational-databases"></a>NoSQL ve ilişkisel veritabanları arasındaki farkları anlama

Bu makalede, ilişkisel veritabanları üzerinden NoSQL veritabanlarının bazı önemli yararları sıralanacaktır. NoSQL ile çalışırken karşılaşılan bazı zorlukları da tartışacağız. Var olan farklı veri depolarını derinlemesine bir şekilde inceleyin, [doğru veri deposunu seçme](https://docs.microsoft.com/azure/architecture/guide/technology-choices/data-store-overview)makalemize göz atın.

## <a name="high-throughput"></a>Yüksek verimlilik

Bir ilişkisel veritabanı sistemi korurken en belirgin zorluklardan biri en ilişkisel motorlar sıkı [ACID semantics](https://en.wikipedia.org/wiki/ACID)uygulamak için kilitler ve mandallar uygulamak olduğunu. Bu yaklaşım, veritabanı içinde tutarlı bir veri durumu sağlama açısından yararları vardır. Ancak, eşzamanlılık, gecikme ve kullanılabilirlik açısından ağır trade-off vardır. Bu temel mimari kısıtlamalar nedeniyle, yüksek işlem hacimleri verileri el ile parçalama gereksinimine neden olabilir. Manuel parçalama uygulanması zaman alıcı ve ağrılı bir egzersiz olabilir.

Bu senaryolarda, [dağıtılmış veritabanları](https://en.wikipedia.org/wiki/Distributed_database) daha ölçeklenebilir bir çözüm sunabilir. Ancak, bakım hala pahalı ve zaman alıcı bir egzersiz olabilir. Yöneticilerin, sistemin dağıtılmış yapısının saydam olduğundan emin olmak için ek iş yapmaları gerekebilir. Ayrıca veritabanının "bağlantısız" doğası için hesap gerekebilir.

[Azure Cosmos DB,](https://docs.microsoft.com/azure/cosmos-db/introduction) tüm Azure bölgelerinde dünya çapında dağıtılarak bu zorlukları basitleştirir. Bölüm aralıkları, veritabanını uygulamayla aynı anda aynı anda yüksek kullanılabilirlik korurken sorunsuz bir şekilde büyütecek şekilde dinamik olarak alt bölümlere ayrılabilir. İnce taneli çoklu kira ve sıkı bir şekilde kontrol edilen bulut-yerel kaynak [yönetimi, şaşırtıcı gecikme garantilerini](https://docs.microsoft.com/azure/cosmos-db/consistency-levels-tradeoffs#consistency-levels-and-latency) ve öngörülebilir performansı kolaylaştırır. Bölümleme tam olarak yönetilir, bu nedenle yöneticilerin kod yazması veya bölümleri yönetmesi gerekmez.

İşlem birimleriniz saniyede binlerce işlem gibi aşırı düzeylere ulaşıyorsa, dağıtılmış bir NoSQL veritabanı nı göz önünde bulundurmalısınız. Maksimum verimlilik, bakım kolaylığı ve azaltılmış toplam sahip olma maliyeti için Azure Cosmos DB'yi düşünün.

![Arka uç](./media/relational-or-nosql/backend-scaled.png)

## <a name="hierarchical-data"></a>Hiyerarşik veriler

Veritabanındaki hareketlerin birçok üst-alt ilişkisi içerebileceği önemli sayıda kullanım örnekleri vardır. Bu ilişkiler zaman içinde önemli ölçüde büyüyebilir ve yönetilmesi zor olabilir. [Hiyerarşik veritabanları](https://en.wikipedia.org/wiki/Hierarchical_database_model) formları 1980'lerde ortaya çıktı, ancak depolama verimsizliği nedeniyle popüler değildi. [Ted Codd'un ilişkisel modelinin](https://en.wikipedia.org/wiki/Relational_model) hemen hemen tüm ana veritabanı yönetim sistemleri tarafından kullanılan fiili standart haline geldikçe çekiş gücünü de kaybettiler.

Ancak, bugün belge tarzı veritabanlarının popülaritesi önemli ölçüde artmıştır. Bu veritabanları hiyerarşik veritabanı paradigmasının yeniden keşfedilmesi olarak kabul edilebilir, şimdi diskte veri depolama maliyeti ile ilgili endişeler tarafından sınırdışı. Sonuç olarak, ilişkisel bir veritabanında birçok karmaşık üst-alt varlık ilişkileri bakımı artık modern belge yönelimli yaklaşımlarile karşılaştırıldığında bir anti-desen olarak kabul edilebilir.

[Nesne yönelimli tasarımın](https://en.wikipedia.org/wiki/Object-oriented_design)ortaya çıkışı ve ilişkisel modellerle birleştirildiğinde ortaya çıkan [empedans uyumsuzluğu,](https://en.wikipedia.org/wiki/Object-relational_impedance_mismatch) belirli kullanım örnekleri için ilişkisel veritabanlarında bir anti-desen vurgulamaktadır. Gizli ama genellikle önemli bakım maliyetleri bir sonucu olarak ortaya çıkabilir. [ORM yaklaşımları](https://en.wikipedia.org/wiki/Object-relational_mapping) bunu kısmen hafifletmek için evrimleşmiş olsa da, belge yönelimli veritabanları yine de nesne yönelimli yaklaşımlarla çok daha iyi birleşir. Bu yaklaşımla, geliştiriciler ORM sürücüleri veya ısmarlama dil özel [OO Veritabanı motorları](https://en.wikipedia.org/wiki/Object_database)taahhüt olmak zorunda değildir. Verileriniz çok sayıda üst-alt ilişkisi ve derin hiyerarşi düzeyleri içeriyorsa, [Azure Cosmos DB SQL API](https://docs.microsoft.com/azure/cosmos-db/introduction)gibi bir NoSQL belge veritabanı kullanmayı düşünebilirsiniz.

![Orderdetails](./media/relational-or-nosql/order-orderdetails.jpg)

## <a name="complex-networks-and-relationships"></a>Karmaşık ağlar ve ilişkiler

İronik olarak, adlarına verilen ilişkisel veritabanları, derin ve karmaşık ilişkileri modellemek için en uygun çözümden daha az bir çözüm sunar. Bunun nedeni, varlıklar arasındaki ilişkilerin ilişkisel bir veritabanında gerçekten bulunmamasıdır. Sorguları kullanarak eşleme sağlamak için kartezyen birleştirmegerektiren karmaşık ilişkiler le, çalışma zamanında hesaplanması gerekir. Sonuç olarak, ilişkiler arttıkça işlemler hesaplama açısından katlanarak daha pahalı hale gelir. Bazı durumlarda, bu tür varlıkları yönetmeye çalışan ilişkisel bir veritabanı kullanılamaz hale gelir.

"Ağ" veritabanlarının çeşitli biçimleri ilişkisel veritabanlarının ortaya çıktığı dönemde ortaya çıkmıştır, ancak hiyerarşik veritabanlarında olduğu gibi, bu sistemler popülerlik kazanmak için mücadele etti. Yavaş benimsenmesi, o dönemde ki kullanım durumlarının eksikliği ve depolama verimsizliklerinden kaynaklanıyordu. Bugün, grafik veritabanı motorları ağ veritabanı paradigmasının yeniden ortaya çıkması olarak kabul edilebilir. Bu sistemlerin en önemli yararı, ilişkilerin veritabanında "birinci sınıf vatandaşlar" olarak depolanmış olmasıdır. Böylece, geçiş ilişkileri her yeni birleştirme veya çapraz ürün ile zaman karmaşıklığı artan yerine, sabit bir süre içinde yapılabilir.

Veritabanınızda karmaşık bir ilişki ağı sürdürüyorsanız, bu verileri yönetmek için Azure [Cosmos DB Gremlin API](https://docs.microsoft.com/azure/cosmos-db/graph-introduction) gibi bir grafik veritabanını göz önünde bulundurmak isteyebilirsiniz.

![Graf](./media/relational-or-nosql/graph.png)

Azure Cosmos DB, tüm büyük NoSQL model türleri için API projeksiyonu sunan çok modelli bir veritabanı hizmetidir; Sütun-aile, Belge, Grafik ve Anahtar Değeri. [Gremlin (grafik)](https://docs.microsoft.com/azure/cosmos-db/gremlin-support) ve SQL (Core) Belge API katmanları tamamen birlikte çalışabilir. Bu, programlanabilirlik düzeyinde farklı modeller arasında geçiş yapmak için yararları vardır. Grafik depoları, hem karmaşık ağ geçişleri hem de aynı mağazada belge kayıtları olarak modellenen hareketler açısından sorgulanabilir.

## <a name="fluid-schema"></a>Sıvı şeması

İlişkisel veritabanlarının bir diğer özel özelliği de şemaların tasarım zamanında tanımlanması gerektiğidir. Bu, referans bütünlüğü ve verilerin uygunluğu açısından yararları vardır. Ancak, uygulama büyüdükçe de kısıtlayıcı olabilir. Aynı tabloyu veya veritabanı tanımını paylaşan mantıksal olarak ayrı modellerdeki şemadaki değişikliklere yanıt vermek zaman içinde karmaşık hale gelebilir. Bu tür kullanım örnekleri genellikle şema uygulama için kayıt bazında yönetmek için devolved olan yarar. Bu, veritabanının "şema agnostik" olmasını ve kayıtların içlerinde bulunan veriler açısından "kendi kendini tanımlamasına" olanak sağlar.

Yapıları sürekli olarak yüksek oranda değişen verileri yönetiyorsanız, özellikle de işlemler veritabanında uygunluğu zorlamanın zor olduğu dış kaynaklardan geliyorsa, daha şema-agnostik bir yaklaşım göz önünde bulundurmak isteyebilirsiniz Azure Cosmos DB gibi yönetilen bir NoSQL veritabanı hizmetini kullanarak.

## <a name="microservices"></a>Mikro hizmetler

[Mikrohizmetler](https://en.wikipedia.org/wiki/Microservices) modeli son yıllarda önemli ölçüde büyüdü. Bu örüntün, [Hizmet Yönelimli Mimari'ye](https://en.wikipedia.org/wiki/Service-oriented_architecture)dayanır. Bu modern mikrohizmetler mimarilerinde veri iletimi için de-facto standart [JSON](https://en.wikipedia.org/wiki/JSON), aynı zamanda belge odaklı NoSQL Veritabanları büyük çoğunluğu için depolama aracı olur. Bu, NoSQL belgesinin karmaşık Microservice uygulamaları arasında hem kalıcılık hem de senkronizasyon [(olay kaynak desenleri](https://en.wikipedia.org/wiki/Event-driven_architecture)kullanarak) için çok daha sorunsuz bir uyum sağlar. Daha geleneksel ilişkisel veritabanları bu mimarilerde korumak için çok daha karmaşık olabilir. Bunun nedeni, API'ler arasında hem durum hem de eşitleme için gereken dönüşüm miktarının artmasıdır. Özellikle Azure Cosmos DB, JSON tabanlı Microservices Architectures için birçok NoSQL veritabanından daha sorunsuz bir uyum sağlayan bir dizi özelliğe sahiptir:

* saf JSON veri türleri seçimi
* veritabanında yerleşik bir JavaScript altyapısı ve [sorgu API'si.](https://docs.microsoft.com/azure/cosmos-db/javascript-query-api)
* bir kapsayıcıda yapılan değişikliklerden haberdar olmak için istemcilerin abone olabileceği son teknoloji [değişiklik akışı.](https://docs.microsoft.com/azure/cosmos-db/change-feed)

## <a name="some-challenges-with-nosql-databases"></a>NoSQL veritabanları ile bazı zorluklar

NoSQL veritabanlarını uygularken bazı net avantajlar olsa da, dikkate almak isteyebileceğin bazı zorluklar da vardır. Bunlar ilişkisel modelle çalışırken aynı derecede mevcut olmayabilir:

* aynı varlığa işaret eden birçok ilişki ile işlemler.
* tüm veri kümesi nde güçlü tutarlılık gerektiren hareketler.

İlk soruna bakıldığında, NoSQL veritabanlarındaki kural kuralı genellikle daha önce ifade edildiği gibi dağıtılmış bir sistemde daha verimli okumalar üreten normalleştirmedir. Ancak, bu yaklaşım ile devreye giren bazı tasarım zorlukları vardır. Bir kategori ve birden çok etiketle ilgili bir ürün ebakalım:

![Birleştirme](./media/relational-or-nosql/many-joins.png)

NoSQL belge veritabanındaki en iyi uygulama yaklaşımı, kategori adını ve etiket adlarını doğrudan bir "ürün belgesinde" normalden çıkarmaktır. Ancak, kategorileri, etiketleri ve ürünleri eşit tutmak için, verileri "bir-çok" basit bir güncelleştirme olmak yerine üründeki birden çok kayıt arasında çoğaltıldığından, bunu kolaylaştıracak tasarım seçenekleri bakım karmaşıklığı nı ekledi. ve verileri almak için birleşim. 

Dengeleme, okumaların normale dönüşen kayıtta daha verimli olması ve kavramsal olarak birleştirilmiş varlıkların sayısı arttıkça giderek daha verimli hale gelmesidir. Ancak, normalileştirme kaydında birleştirilmiş varlıkların sayısı arttıkça, okuma verimliliği de artarsa, varlıkları eşit tutmanın bakım karmaşıklığı da artar. Bu trade-off azaltmanın bir [yolu, karma](https://docs.microsoft.com/azure/cosmos-db/modeling-data#hybrid-data-models)bir veri modeli oluşturmaktır.

NoSQL veritabanlarında bu dengelerle başa çıkmak için daha fazla esneklik olsa da, artan esneklik daha fazla tasarım kararı da üretebilir. [Azure Cosmos DB'deki verileri,](https://docs.microsoft.com/azure/cosmos-db/how-to-model-partition-example)kullanıcıların yalnızca farklı bölümlerde değil, farklı kapsayıcılarda da aynı [anda eşitlenmiş](https://docs.microsoft.com/azure/cosmos-db/how-to-model-partition-example#denormalizing-usernames) olarak senkronize tutma yaklaşımını içeren gerçek dünya örneğini kullanarak verileri nasıl modelleyip bölümlere ayıracağınız makalemize başvurun.

Güçlü tutarlılık açısından, bu tüm veri kümesi boyunca gerekli olacaktır nadirdir. Ancak, bunun gerekli olduğu durumlarda, dağıtılmış veritabanlarında bir sorun olabilir. Güçlü tutarlılık sağlamak için, istemcilerin okumasına izin vermeden önce verilerin tüm yinelemeler ve bölgeler arasında eşitlemesi gerekir. Bu, okumaların gecikmesini artırabilir.

Yine, Azure Cosmos DB burada ilgili çeşitli dengeler için ilişkisel veritabanlarıdaha fazla esneklik sunar, ancak küçük ölçekli uygulamalar için bu yaklaşım daha fazla tasarım konuları ekleyebilirsiniz. Bu konuda daha fazla ayrıntı için [Tutarlılık, kullanılabilirlik ve performans dengeleri](https://docs.microsoft.com/azure/cosmos-db/consistency-levels-tradeoffs) hakkındaki makalemize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos hesabınızı ve diğer kavramları nasıl yöneteceklerini öğrenin:

* [Azure Cosmos hesabınızı yönetme](how-to-manage-database-account.md)
* [Küresel dağıtım](distribute-data-globally.md)
* [Tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos kapsayıcıları ve öğeleriyle çalışma](databases-containers-items.md)
* [Azure Cosmos hesabınız için VNET hizmet bitiş noktası](vnet-service-endpoint.md)
* [Azure Cosmos hesabınız için IP güvenlik duvarı](firewall-support.md)
* [Azure Cosmos hesabınıza Azure bölgeleri ekleme ve kaldırma](how-to-manage-database-account.md)
* [Azure Cosmos DB SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
