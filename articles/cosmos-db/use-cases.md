---
title: Azure Cosmos DB için yaygın kullanım durumları ve senaryolar
description: 'Azure Cosmos DB için en iyi beş kullanım durumu hakkında bilgi edinin: kullanıcı tarafından oluşturulan içerik, olay günlüğü, katalog verileri, kullanıcı tercihleri verileri ve Nesnelerin İnterneti (IoT).'
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 05/21/2019
ms.openlocfilehash: de2bc551547706fb820813e57996e77bf49148d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73888928"
---
# <a name="common-azure-cosmos-db-use-cases"></a>Ortak Azure Cosmos DB kullanım örnekleri
Bu makalede, Azure Cosmos DB için birkaç yaygın kullanım örneğine genel bir bakış sağlanmaktadır.  Bu makaledeki öneriler, Cosmos DB ile uygulamanızı geliştirirken bir başlangıç noktası olarak hizmet vermektedir.   

Bu makaleyi okuduktan sonra, aşağıdaki soruları yanıtlamak mümkün olacak: 

* Azure Cosmos DB için yaygın kullanım örnekleri nelerdir?
* Perakende uygulamalarda Azure Cosmos DB kullanmanın avantajları nelerdir?
* Nesnelerin İnterneti (IoT) sistemleri için veri deposu olarak Azure Cosmos DB kullanmanın avantajları nelerdir?
* Web ve mobil uygulamalar için Azure Cosmos DB kullanmanın avantajları nelerdir?

## <a name="introduction"></a>Giriş
[Azure Cosmos DB,](../cosmos-db/introduction.md) Microsoft'un genel olarak dağıtılmış veritabanı hizmetidir. Hizmet, müşterilerin herhangi bir sayıda coğrafi bölgede elastik (ve bağımsız olarak) ölçeklendirmesine olanak sağlamak üzere tasarlanmıştır. Azure Cosmos DB, bugün piyasada üretim, gecikme süresi, kullanılabilirlik ve tutarlılık kapsayan kapsamlı [hizmet düzeyi anlaşmaları](https://azure.microsoft.com/support/legal/sla/cosmos-db/) sunan ilk küresel olarak dağıtılan veritabanı hizmetidir. 

Azure Cosmos DB, çok çeşitli uygulamalarda ve kullanım örneklerinde kullanılan genel olarak dağıtılmış, çok modelli bir veritabanıdır. Bu, milisaniye sırası düşük yanıt süreleri gerektiren ve hızlı ve küresel ölçeklendirmesi gereken [sunucusuz](https://azure.com/serverless) uygulamalar için iyi bir seçimdir. Birden çok veri modelini (anahtar değeri, belgeler, grafikler ve sütunlayıcı) ve [Azure Cosmos DB'nin MongoDB,](mongodb-introduction.md) [SQL API,](documentdb-introduction.md) [Gremlin API](graph-introduction.md)ve [Tablolar API'si](table-introduction.md) dahil olmak üzere veri erişimi için birçok API'yi yerel olarak ve genişletilebilir bir şekilde destekler. 

Azure Cosmos DB'nin küresel hırslı yüksek performanslı uygulamalar için uygun hale getiren bazı özellikleri aşağıda veda edilmiştir.

* Azure Cosmos DB, yüksek kullanılabilirlik ve ölçeklenebilirlik için verilerinizi yerel olarak bölümlere ayırır. Azure Cosmos DB, tüm tek bölgeli hesaplarda ve tüm çok bölgeli hesaplarda rahat tutarlılığa sahip tüm hesaplarda kullanılabilirlik, iş sonu, düşük gecikme süresi ve tutarlılık için %99,99 garanti sunarken, tüm çok bölgeli veritabanı hesaplarında %99,999 okuma kullanılabilirliği sunar.
* Azure Cosmos DB, milisaniyenin düşük gecikme li yanıt sürelerine sahip SSD destekli depolama alanına sahiptir.
* Azure Cosmos DB'nin nihai, tutarlı önek, oturum ve sınırlı bayatlık gibi tutarlılık düzeylerine yönelik desteği tam esneklik ve düşük performans maliyeti oranı sağlar. Hiçbir veritabanı hizmeti, düzey tutarlılığında Azure Cosmos DB kadar esneklik sunmaz. 
* Azure Cosmos DB, depolama ve iş verilerini bağımsız olarak ölçer esnek bir veri dostu fiyatlandırma modeline sahiptir.
* Azure Cosmos DB'nin ayrılmış iş verme modeli, temel donanımın CPU/bellek/IOPs yerine okuma/yazma sayısı açısından düşünmenizi sağlar.
* Azure Cosmos DB'nin tasarımı, günde trilyonlarca istek sırasına göre büyük istek hacimlerine ölçeklendirmenizi sağlar.

Bu özellikler, düşük yanıt süreleri gerektiren ve büyük miktarda okuma ve yazma işlemesi gereken web, mobil, oyun ve IoT uygulamalarında yararlıdır.

## <a name="iot-and-telematics"></a>IoT ve telematik
IoT kullanım örnekleri genellikle verileri nasıl yuttukları, işleyip depoladıklarıyla ilgili bazı desenleri paylaşır.  İlk olarak, bu sistemlerin çeşitli yerel aygıt sensörlerinden veri patlamaları yutması gerekir. Daha sonra, bu sistemler gerçek zamanlı öngörüler elde etmek için akış verilerini işler ve analiz eder. Veriler daha sonra toplu iş analitiği için soğuk depolama ya arşivlenir. Microsoft Azure, Azure Cosmos DB, Azure Etkinlik Hub'ları, Azure Akış Analizi, Azure Bildirim Merkezi, Azure Machine Learning, Azure HDInsight ve Power BI gibi IoT kullanım durumları için uygulanabilen zengin hizmetler sunar. 

![Azure Cosmos DB IoT başvuru mimarisi](./media/use-cases/iot.png)

Düşük gecikme gecikmesi ile yüksek iş verisi alımı sunduğundan, veri patlamaları Azure Etkinlik Hub'ları tarafından yutulabilir. Gerçek zamanlı öngörüiçin işlenmesi gereken veriler, gerçek zamanlı analiz için Azure Akış Analizi'ne aktarılabilir. Veriler geçici sorgulama için Azure Cosmos DB'ye yüklenebilir. Veriler Azure Cosmos DB'ye yüklendikten sonra veriler sorgulanmaya hazır olur. Buna ek olarak, yeni veriler ve varolan verilerdeki değişiklikler değişiklik akışında okunabilir. Akış değiştir, yalnızca Cosmos kaplarında değişiklikleri sırayla depolayan kalıcı, ek bir günlüktür. Azure Cosmos DB'deki tüm veriler veya yalnızca verilerdeki değişiklikler, gerçek zamanlı analizin bir parçası olarak referans verisi olarak kullanılabilir. Ayrıca, Veriler Azure Cosmos DB verilerini Pig, Hive veya Harita/Azalt işleri için HDInsight'a bağlayarak daha da rafine edilebilir ve işlenebilir.  Rafine edilmiş veriler daha sonra raporlama için Azure Cosmos DB'ye geri yüklenir.   

Azure Cosmos DB, EventHub'lar ve Storm'u kullanan örnek bir IoT çözümü için [GitHub'daki hdinsight-storm-örnek deposuna](https://github.com/hdinsight/hdinsight-storm-examples/)bakın.

IoT için Azure teklifleri hakkında daha fazla bilgi için şunları bakın: [Nesnelerinizin İnternetini Oluştur.](https://www.microsoft.com/en-us/internet-of-things) 

## <a name="retail-and-marketing"></a>Perakende ve pazarlama
Azure Cosmos DB, Windows Mağazası ve XBox Live'ı çalıştıran Microsoft'un kendi e-ticaret platformlarında yaygın olarak kullanılır. Ayrıca katalog verilerinin saklanması ve boru hatlarının işlenmesinde olay kaynağı için perakende sektöründe de kullanılır.

Katalog veri kullanım senaryoları, kişiler, yerler ve ürünler gibi varlıklar için bir öznitelikler kümesini depolamayı ve sorgulamayı içerir. Katalog verilerine örnek olarak kullanıcı hesapları, ürün katalogları, IoT cihaz kayıtları ve malzeme faturası sistemleri verilebilir. Bu verilerin öznitelikleri değişebilir ve uygulama gereksinimlerine uyacak şekilde zaman içinde değişebilir.

Bir otomotiv parçaları tedarikçisi için ürün kataloğu örneğini düşünün. Her parça, tüm parçaların paylaştığı ortak özniteliklere ek olarak kendi özniteliklerine sahip olabilir. Ayrıca, belirli bir bölümün öznitelikleri, yeni bir model yayımlandığında bir sonraki yıl değişebilir. Azure Cosmos DB esnek şemaları ve hiyerarşik verileri destekler ve bu nedenle ürün kataloğu verilerini depolamak için uygundur.

![Azure Cosmos DB perakende kataloğu başvuru mimarisi](./media/use-cases/product-catalog.png)

Azure Cosmos DB, [değişiklik akışı](change-feed.md) işlevini kullanarak olay odaklı mimarileri güç sağlamak için genellikle olay kaynağı için kullanılır. Değişiklik akışı, alt akış mikro hizmetlerine, azure cosmos DB'de yapılan ekler ve güncelleştirmeleri (örneğin, etkinlik siparişi) güvenilir ve aşamalı olarak okuma olanağı sağlar. Bu işlevsellik, durum değiştiren olaylar için bir ileti aracısı olarak kalıcı bir olay deposu sağlamak ve birçok mikro hizmet arasında [(sunucusuz Azure İşlevleri](https://azure.com/serverless)olarak uygulanabilir) sipariş işleme iş akışını artırmak için kullanılabilir.

![Azure Cosmos DB sipariş boru hattı başvuru mimarisi](./media/use-cases/event-sourcing.png)

Ayrıca, Azure Cosmos DB'de depolanan veriler, Apache Spark işleri aracılığıyla büyük veri analitiği için HDInsight ile entegre edilebilir. Azure Cosmos DB için Kıvılcım Konektörü hakkında ayrıntılı bilgi için [Cosmos DB ve HDInsight ile Bir Kıvılcım çalıştır'a](spark-connector.md)bakın.

## <a name="gaming"></a>Oyun
Veritabanı katmanı oyun uygulamalarının önemli bir bileşenidir. Modern oyunlar mobil/konsol istemcilerinde grafik işleme yapar, ancak oyun içi istatistikler, sosyal medya entegrasyonu ve yüksek puanlı lider panoları gibi özelleştirilmiş ve kişiselleştirilmiş içerik sunmak için buluta güvenir. Oyunlar genellikle ilgi çekici bir oyun içi deneyim sağlamak için okuma ve yazma için tek milisaniyelik gecikmeler gerektirir. Bir oyun veritabanı hızlı olması ve yeni oyun lansmanları ve özellik güncellemeleri sırasında istek oranlarında büyük ani işlemek gerekir.

Azure Cosmos DB, [The Walking Dead: No Man's Land](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) by Next [Games](https://www.nextgames.com/)ve [Halo 5: Guardians](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/)gibi oyunlar tarafından kullanılır. Azure Cosmos DB, oyun geliştiricileri için aşağıdaki avantajları sağlar:

* Azure Cosmos DB, performansın elastik olarak yukarı veya aşağı ölçeklendirilmesine olanak tanır. Bu, oyunların tek bir API çağrısı yaparak düzinelerceden milyonlarca eşzamanlı oyuncuya kadar profil ve istatistikleri güncellemeyi ele almalarına olanak tanır.
* Azure Cosmos DB, oyun sırasında herhangi bir gecikmeyi önlemek için milisaniye okumaları destekler ve yazar.
* Azure Cosmos DB'nin otomatik dizine eklemeözelliği, oyuncuları kendi dahili oyuncu kimliklerini veya GameCenter, Facebook, Google Kimliklerini veya bir loncadaki oyuncu üyeliğine dayalı sorgularına göre bulma gibi gerçek zamanlı olarak birden çok farklı özelliğe karşı filtreleme yapılmasına olanak tanır. Bu karmaşık dizin oluşturma veya altyapı parçalama olmadan mümkündür.
* Oyun içi sohbet mesajları, oyuncu loncası üyelikleri, tamamlanan zorluklar, yüksek puanlı lider panoları ve sosyal grafikler gibi sosyal özelliklerin esnek bir şema ile uygulanması daha kolaydır.
* Azure Cosmos DB, hızlı yinelemeye olanak sağlamak ve pazara varma süresini kısaltmak için en az kurulum ve yönetim çalışması gerektirmekteydi.

![Azure Cosmos DB oyun referans mimarisi](./media/use-cases/gaming.png)

## <a name="web-and-mobile-applications"></a>Web ve mobil uygulamalar
Azure Cosmos DB, web ve mobil uygulamalarda yaygın olarak kullanılır ve sosyal etkileşimleri modellemek, üçüncü taraf hizmetleriyle bütünleştirmek ve zengin kişiselleştirilmiş deneyimler oluşturmak için uygundur. Cosmos DB SDK'lar popüler [Xamarin çerçevesini](mobile-apps-with-xamarin.md)kullanarak zengin iOS ve Android uygulamaları oluşturmak kullanılabilir.  

### <a name="social-applications"></a>Sosyal Uygulamalar
Azure Cosmos DB için yaygın bir kullanım örneği, web, mobil ve sosyal medya uygulamaları için kullanıcı tarafından oluşturulan içeriği (UGC) depolamak ve sorgulamaktır. UGC'ye bazı örnekler sohbet oturumları, tweetler, blog gönderileri, derecelendirmeler ve yorumlardır. Genellikle, sosyal medya uygulamalarında UGC serbest biçimmetin bir karışımıdır, özellikleri, etiketleri, ve katı yapısı ile sınırlı değildir ilişkileri. Sohbetler, yorumlar ve gönderiler gibi içerik, dönüşümler veya karmaşık nesne den ilişkisel eşleme katmanlarına gerek kalmadan Cosmos DB'de depolanabilir.  Geliştiriciler uygulama kodu üzerinde yinelenirken gereksinimleri eşleşecek şekilde veri özellikleri kolayca eklenebilir veya değiştirilebilir ve böylece hızlı geliştirme teşvik edilebilir.  

Üçüncü taraf sosyal ağlarla bütünleşen uygulamalar, bu ağlardaki değişen şemalara yanıt vermelidir. Veriler Cosmos DB'de varsayılan olarak otomatik olarak dizine eklenmiştir, veriler her an sorgulanmaya hazırdır. Bu nedenle, bu uygulamalar kendi ihtiyaçlarına göre projeksiyonları almak için esnekliğe sahip.

Sosyal uygulamaların çoğu küresel ölçekte çalışır ve öngörülemeyen kullanım alışkanlıkları gösterebilir. Uygulama katmanı kullanım talebini eşleşecek şekilde ölçeklendirildikçe, veri deposunun ölçeklendirmesinde esneklik esastır.  Cosmos DB hesabının altına ek veri bölümleri ekleyerek ölçeklendirebilirsiniz.  Ayrıca, birden çok bölgede ek Cosmos DB hesapları da oluşturabilirsiniz. Cosmos DB hizmet bölgesi kullanılabilirliği için [Azure Bölgeleri'ne](https://azure.microsoft.com/regions/#services)bakın.

![Azure Cosmos DB web uygulaması başvuru mimarisi](./media/use-cases/apps-with-global-reach.png)

### <a name="personalization"></a>Kişiselleştirme
Günümüzde, modern uygulamalar karmaşık görüş ve deneyimler ile birlikte gelir. Bu genellikle dinamik, kullanıcı tercihleri veya ruh hali ve marka ihtiyaçlarına catering vardır. Bu nedenle, uygulamaların Kullanıcı Arabirimi öğelerini ve deneyimlerini hızlı bir şekilde işlemek için kişiselleştirilmiş ayarları etkili bir şekilde alabilmeleri gerekir. 

JSON, Cosmos DB tarafından desteklenen bir biçim, sadece hafif değil, aynı zamanda kolayca JavaScript tarafından yorumlanabilir gibi UI düzen verilerini temsil etmek için etkili bir biçimdir. Cosmos DB, düşük gecikme süresi yle hızlı okumalar sağlayan tutabilen tutarlılık düzeyleri sunar. Bu nedenle, Cosmos DB JSON belgeleri olarak kişiselleştirilmiş ayarları da dahil olmak üzere UI düzen verileri depolama tel üzerinden bu verileri almak için etkili bir araçtır.

![Azure Cosmos DB web uygulaması başvuru mimarisi](./media/use-cases/personalization.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB ile başlamak için, hesap oluşturma ve Cosmos DB ile başlama konusunda size yol gösteren [hızlı başlangıçlarımızı](create-sql-api-dotnet.md)takip edin.

* Azure Cosmos DB kullanan müşteriler hakkında daha fazla bilgi almak istiyorsanız, [müşteri örnek çalışmaları](https://azure.microsoft.com/case-studies/?service=cosmos-db) sayfasına bakın.
