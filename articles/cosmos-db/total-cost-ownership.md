---
title: Azure Cosmos DB ile Toplam Sahip Olma Maliyeti (TCO)
description: Bu makale, Azure Cosmos DB'nin toplam sahip olma maliyetini IaaS ve şirket içi veritabanları ile karşılaştırır
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.reviewer: sngun
ms.openlocfilehash: b24b69716e472082abfdb388e7d79e88a8e23e64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72754802"
---
# <a name="total-cost-of-ownership-tco-with-azure-cosmos-db"></a>Azure Cosmos DB ile Toplam Sahip Olma Maliyeti (TCO)

Azure Cosmos DB, ayrıntılı çok kiracılı olarak ve kaynak idaresi özelliğiyle tasarlanmıştır. Bu tasarım, Azure Cosmos DB’nin çok daha az maliyetlerle çalıştırılmasına olanak tanır ve kullanıcıların tasarruf etmesine yardımcı olur. Şu anda, Azure Cosmos DB tek makinede 280’den fazla müşteri iş yükünü ve bir kümede binlerce müşteri iş yükünü destekler. Bu yoğunluk, sürekli olarak artış gösteriyor. Müşterilerin iş yüklerinin çoğaltmalarına ilişkin yükü bir kümedeki farklı makinelerde ve bir veri merkezindeki birden fazla kümede dengeler. Daha fazla bilgi edinmek için Azure [Cosmos DB: Küresel olarak dağıtılan veritabanlarının sınırlarını zorlamak.](https://azure.microsoft.com/blog/azure-cosmos-db-pushing-the-frontier-of-globally-distributed-databases/) Azure Cosmos DB, Azure altyapısının geri kalanıyla kaynak yönetimi, çoklu kiralama ve yerel entegrasyon nedeniyle, IaaS üzerinde çalışan MongoDB, Cassandra veya diğer OSS NoSQL'den ortalama 4 ila 6 kat daha ucuzdur ve veritabanından 10 kata kadar daha ucuzdur motorlar tesislerinde çalışıyor. [NoSQL veritabanı bulut hizmetinin toplam sahipolma (non) sahipliği](https://documentdbportalstorage.blob.core.windows.net/papers/11.15.2017/NoSQL%20TCO%20paper.pdf)hakkındaki makaleye bakın.

Apache Cassandra, MongoDB, HBase gibi OSS NoSQL veritabanı çözümleri şirket içi için tasarlanmıştır. Yönetilen bir hizmet olarak sunulduğunda, sağlanan kümeleri yönetmek ve destek izlemek için kiracı veritabanına sahip kaynak yöneticisi şablonuna eşdeğerdirler. OSS NoSQL mimarileri önemli ölçüde operasyonel ek yükler gerektirir ve uzmanlık bulmak zor ve pahalı olabilir. Öte yandan Azure Cosmos DB, geliştiricilerin veritabanı altyapısını yönetmek ve korumak yerine iş inovasyonuna odaklanmalarını sağlayan tam olarak yönetilen bir bulut hizmetidir. 

Bulut ait veritabanı hizmeti Azure Cosmos DB'nin aksine, OSS NoSQL veritabanı motorları temel mimari ilkeler olarak kaynak yönetimi veya ince taneli çoklu kira ile tasarlanmamış ve üretilmiştir. Cassandra ve MongoDB gibi OSS NoSQL veritabanı motorları, üzerinde çalıştıkları sanal makinenin tüm kaynaklarının kullanımları için kullanılabildiğini temel bir varsayımda bulunur. Kaynak miktarı belirli bir eşiğin altına düşerse, bu veritabanı altyapılarının çoğu çalışamaz. Örneğin, küçük VM örnekleri için ve genellikle daha yüksek maliyetli büyük ölçekli VM'ler öneren satıcı tarafından önerilen yapılandırmaları ile kullanılabilir. Bu nedenle, bir OSS NoSQL veya başka bir şirket içi veritabanı altyapısı barındırmak ve saniyede istekler veya tüketilen depolama gibi tüketim tabanlı bir şarj modeli kullanarak kullanılabilir hale getirmek mümkün değildir.

## <a name="total-cost-of-ownership-of-azure-cosmos-db"></a>Azure Cosmos DB'nin toplam sahip olma maliyeti 

Azure Cosmos DB'nin sunucusuz sağlama modeli, veritabanı altyapısının aşırı sağlanması gereksinimini ortadan kaldırır. Azure Cosmos DB kaynakları, özel yapılandırmalara veya lisanslama gerekmeden sağlanır. Sonuç olarak, Azure Cosmos DB destekli uygulamalar, OSS NoSQL veritabanlarıyla karşılaştırıldığında toplam sahip olma maliyeti yüzde 70'e kadar olabilir. Bazı gerçek zamanlı örnekler [için, müşteri kullanım örneklerine](https://customers.microsoft.com/en-us/search?sq=Cosmos%20DB&ff=&p=0&so=story_publish_date%20desc)bakın. Azure Cosmos DB fiyatlandırma modelinin diğer avantajları şunlardır:

* **Fiyat için büyük değer:** Pazar analistleri, müşteriler ve iş ortakları, Azure Cosmos DB'nin sunduğu tüm özelliklerin, müşterilerin bu çözümleri kendi başlarına veya diğer satıcılar aracılığıyla uygularken elde edilebildiklerine kıyasla çok daha düşük bir fiyata sunduğunu doğruladı. Veritabanı, bu tür küresel dağıtım, çok yönlü, iyi tanımlanmış ve sezgisel tutarlılık modelleri ne kadar basitleştirilir, otomatik dizin oluşturma, Azure Cosmos DB ile herhangi bir karmaşıklık, genel ek veya kapalı kalma süresi olmadan büyük ölçüde basitleştirilir.

* **NoSQL DevOps yönetimi gereklidir:** Azure Cosmos DB ile dağıtımları yönetmek, bakım yapmak, ölçeklendirmek veya yama yapmak için DevOps'leri kullanmak gerekmez. Şirket içinde veya bulut altyapısında barındırılan OSS NoSQL kümesiyle yapacağınız tüm iş yüklerini çalıştırabilirsiniz.

![Azure Cosmos DB sahip olma maliyeti](./media/total-cost-ownership/tco.png)

* **Elastik ölçeklendirme yeteneği:** Azure Cosmos DB iş bölümü yukarı ve aşağı ölçeklendirilerek yoğun olmayan zamanlarda sahip olma maliyetini düşürmenize olanak tanır. Bulut altyapısında dağıtılan OSS NoSQL kümeleri sınırlı esneklik sunar ve şirket içi dağıtımlar tanım gereği esnek değildir. Azure Cosmos DB'de, daha fazla iş elde etme sağlamanız durumunda, iş lerinizin doğrusal olarak ölçeklendirilmesi garanti edilir. Bu garanti finansal SLA'lar tarafından ve herhangi bir ölçekte yüzde 99 oranında yedeklenir.

* **Ölçek ekonomileri:** Azure Cosmos DB gibi yönetilen bir hizmet, ağ, depolama ve hesaplamalarla yerel olarak tümleştirilmiş çok sayıda düğümle çalışır. Azure Cosmos DB'nin büyük ölçekli olması nedeniyle, standartlaştırma maliyetlerinden tasarruf edebilirsiniz.

* **Bulut için optimize edin:** Azure Cosmos DB, ince taneli çoklu kira ve performans yalıtımı ile sıfırdan tasarlanmıştır. Bu, binlerce kiracıyı ve bunların iş yüklerini kümeler ve veri merkezleri arasında en iyi şekilde yerleştirmenize, yürütmeye ve dengelemeye olanak tanır. Buna karşılık, mevcut OSS NoSQL veritabanları nesli, tek bir kiracının iş yükünü çalıştırdığı varsayılan tüm sanal makineyle şirket içinde çalışır. Bu veritabanları, bir bulut sağlayıcısının altyapısını ve donanımını tam olarak sağlamak üzere tasarlanmaz. Örneğin, bir OSS NoSQL veritabanı altyapısı, sanal bir makinenin rutin bir görüntü yükseltmesi vs aşağı olması veya premium diskin zaten üç yönlü çoğaltılmış olması arasındaki farkların farkında değildir. Bu avantajlardan yararlanamaz ve müşterilere avantaj ve tasarruf aktaramaz.

* **Saat başı ödeme:** Zaman içinde herhangi bir noktada ölçeklendirmesi gereken büyük ölçekli iş yükleri için yalnızca saat e göre ücretlendirilirsiniz. Bir uygulamadaki iş yükleri genellikle yılın zamanlarına ve sorgulanmış verilere göre değişir. Azure Cosmos DB ile, ihtiyacınız olan şekilde ölçeklendirebilir veya küçültebilir ve yalnızca ihtiyacınız olan şey için ödeme yapabilirsiniz. Şirket içi veya IaaS barındırılan sistemlerle, donanımı her saat hizmetdışı bırakmanın bir yolu olmadığından, bu modeli eşleştiremezsiniz. Bu gibi durumlarda, Azure Cosmos DB ile ortalama 10 ila 14 kat arasında tasarruf edebilirsiniz.

* **Ücretsiz olarak çok sayıda özellik elde edin:** Azure Cosmos DB'de yazma iş yükleri, alternatif veritabanı hizmetlerine kıyasla önemli ölçüde daha ucuza dır. Buna ek olarak, Azure Cosmos DB [otomatik dizin oluşturma,](indexing-policies.md) [Yaşamak Için Zaman (TTL)](time-to-live.md), [Feed'i Değiştir](change-feed.md) ve diğer lerini ek ücret ödemeden, diğer veritabanı hizmetlerinin genellikle ücretlendireceği bir şey gibi özellikler sunar.

* **Farklı iş yükleri için birleşik para birimi kullanır:** Azure Cosmos DB'de alternatif tekliflerin aksine, iş yüklerini örneğin okuma ve yazma olarak bölümlere bölmeniz gerekmez. Veya iş yükü başına iş başına iş başına iş başına iş başına iş başına iş başına iş başına iş başına iş başına iş başına iş başına iş başına iş yükü ve yazma iş başına iş yükü. Azure Cosmos DB'de, sağlanan iş ortası, İstek Birimleri veya RU/sn açısından birleşik ve normalleştirilmiş bir para birimi kullanılarak ayrılmıştır. Bu tür bir yaklaşım, aynı RU/s'yi çeşitli işlemler ve iş yükü türleri arasında kolayca değiştirmenizi sağlar.

* **Ölçeklendirmek için VM'lerin sağlanması gerekmez:** Çoğu operasyonel veritabanları, gürültülü komşulardan kaçınmak ve ölçeklendirmek istiyorsanız gevşek kaynak yönetimi için büyük sanal makinelerle gitmenizi gerektirir. Bu, maliyetin yükünü ve ön taahhüdünü müşterilere yükler. Azure Cosmos DB ile küçük başlayıp büyük ölçekli iş yükü boyutlarına sorunsuz bir şekilde ve veri kullanılabilirliği üzerinde herhangi bir kesinti veya etki olmadan büyüyebilirsiniz.

* **Sağlanan iş verimini maksimum sınıra kadar kullanabilirsiniz:** Azure Cosmos DB'deki alt çekirdek çoklama sayesinde, sağlanan iş ortasını IaaS'ın barındırılan seçeneklerden veya üçüncü taraf tekliflerinden daha büyük ölçüde doygunlaştırabilirsiniz. Bu yöntem, alternatif çözümlerden çok daha fazla tasarruf sağlar.

* **Azure Cosmos DB'nin diğer Azure hizmetleriyle derin entegrasyonu.** Azure Cosmos DB, Ağ, İşlem, Azure İşlevleri (sunucusuz), Azure IoT ve diğer Azure hizmetleriyle yerel bir tümleştirmeye sahiptir. Bu entegrasyon ile, sağlam garantilerle dünya çapında en iyi performansı, veri çoğaltma hızını elde elabilirsiniz. Üçüncü taraf çözümleri eşleşemez veya genellikle bu tür özellikleri sunmak için bir prim ücret olacaktır.

* **Varsayılan olarak en az 10-20 hata etki alanıyla otomatik olarak yüksek kullanılabilirlik elde elabilirsiniz:** Azure Cosmos DB, yüksek kullanılabilirlik için kritik öneme sahip bir özellik olan hata etki alanları arasında iş yüklerinin dağıtımını destekler. Dünyanın herhangi bir yerinde 99.999 yüksek kullanılabilirlik sunuyor okuma ve yazma yüzde 99 yüzdelik olarak. Böyle bir şeyi kendi başına veya üçüncü taraf bir çözüm le uygulamanın maliyeti yüksek olacaktır.

* **Ek ücret ödemeden tüm kurumsal yetenekleri otomatik olarak alırsınız.** Azure Cosmos DB, en kapsamlı uyumluluk sertifikaları, güvenlik ve şifreleme setini hiçbir ek ücret ödemeden (rakiplerimizle karşılaştırıldığında) hiçbir ücret ödemeden sunar. Dünyanın herhangi bir yerinde otomatik olarak bölgesel kullanılabilirlik elde elabilirsiniz. Veritabanınızı istediğiniz sayıda Azure bölgesine yayabilir ve istediğiniz noktada bölgeler ekleyebilir veya kaldırabilirsiniz.

* **Rezerve edilmiş kapasite yle maliyetlerin %65'ine kadar tasarruf edebilirsiniz:** Azure Cosmos DB [rezerve etme kapasitesi,](cosmos-db-reserved-capacity.md) Azure Cosmos DB kaynakları için bir yıl veya üç yıl için ön ödeme yaparak tasarruf etmenizi sağlar. Bir yıllık veya üç yıllık peşin taahhütlerle maliyetlerinizi önemli ölçüde azaltabilir ve normal fiyatlandırmaya kıyasla %20-65 arasında indirimden tasarruf edebilirsiniz. Görev açısından kritik iş yüklerinizde, kapasite sağlama açısından daha iyi SLA'lar alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB fiyatlandırma modelinin müşteriler için maliyet etkin olması](total-cost-ownership.md) hakkında daha fazla bilgi edinin
* [Geliştirme ve test için Optimizasyon](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [İş memat maliyetini optimize etme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini optimize etme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetini optimize etme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini optimize etme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin
* [Çok bölgeli Cosmos hesaplarının maliyetini optimize etme](optimize-cost-regions.md) hakkında daha fazla bilgi edinin
* [NoSQL Veritabanı Bulut Hizmetinin Toplam (Non) Sahipliğinin Toplam Maliyeti](https://documentdbportalstorage.blob.core.windows.net/papers/11.15.2017/NoSQL%20TCO%20paper.pdf) hakkında daha fazla bilgi edinin
