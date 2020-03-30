---
title: Azure Cosmos DB Gremlin API'ye Giriş
description: Düşük gecikme süresine sahip yoğun grafikleri Apache TinkerPop’un Gremlin grafik sorgu dilini kullanarak depolamak, sorgulamak ve dolaştırmak için Azure Cosmos DB’yi nasıl kullanabileceğinizi öğrenin.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 07/18/2019
ms.author: lbosq
ms.openlocfilehash: 9151b54d7fa0b64a465aa8384cb4bfdb8e72c482
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75500011"
---
# <a name="introduction-to-azure-cosmos-db-gremlin-api"></a>Azure Cosmos DB: Gremlin API'sine giriş

[Azure Cosmos DB,](introduction.md) görev açısından kritik uygulamalar için Microsoft'un dünya çapında dağıtılan çok modelli veritabanı hizmetidir. Çok modelli bir veritabanıdır ve belge, anahtar değeri, grafik ve sütun-aile veri modellerini destekler. Azure Cosmos DB Gremlin API, herhangi bir ölçek için tasarlanmış tam olarak yönetilen bir veritabanı hizmetinde grafik verilerini depolamak ve çalıştırmak için kullanılır.  

![Azure Cosmos DB grafik mimarisi](./media/graph-introduction/cosmosdb-graph-architecture.png)

Bu makale, Azure Cosmos DB Gremlin API'ye genel bir bakış sağlar ve milyarlarca köşesi ve kenarı olan yoğun grafikleri depolamak için bunu nasıl kullanabileceğinizi açıklar. Milisaniye gecikmeile grafikleri sorgulayabilir ve grafik yapısını kolayca geliştirebilirsiniz. Azure Cosmos DB'nin Gremlin API'si [Apache TinkerPop](https://tinkerpop.apache.org) grafik veritabanı standardını temel alıyor ve Gremlin sorgu dilini kullanıyor. 

Azure Cosmos DB'nin Gremlin API'si, grafik veritabanı algoritmalarının gücünü, esneklik eksikliği ve ilişkisel yaklaşımlarla ilişkili en yaygın veri sorunlarına benzersiz ve esnek bir çözüm sağlamak için yüksek ölçeklenebilir, yönetilen altyapıyla birleştirir. 

## <a name="features-of-azure-cosmos-db-graph-database"></a>Azure Cosmos DB grafik veritabanının özellikleri
 
Azure Cosmos DB; genel dağıtım, depolama ve aktarım hızında esnek ölçeklendirme, otomatik dizinleme ve sorgu, ayarlanabilir tutarlılık düzeyleri ve TinkerPop standardı desteği sunan, tam olarak yönetilen bir grafik veritabanıdır. 

Azure Cosmos DB Gremlin API'nin sunduğu farklılaştırılmış özellikler şunlardır:

* **Aktarım hızı ve depolamayı esnek bir şekilde ölçeklendirme**

  Gerçek dünyadaki grafiklerin, tek bir sunucunun kapasitesinin yetmeyeceği biçimde ölçeklendirilmesi gerekir. Azure Cosmos DB, depolama ve sağlanan üretim açısından neredeyse sınırsız boyuta sahip yatay ölçeklenebilir grafik veritabanlarını destekler. Grafik veritabanı ölçeği büyüdükçe, veriler [grafik bölümleme](https://docs.microsoft.com/azure/cosmos-db/graph-partitioning)kullanılarak otomatik olarak dağıtılır.

* **Çok bölgeli çoğaltma**

  Azure Cosmos DB, grafik verilerinizi dünya çapındaki herhangi bir Azure bölgesine otomatik olarak çoğaltabilir. Genel çoğaltma, verilere genel erişim gerektiren uygulamaların geliştirilmesini kolaylaştırır. Azure Cosmos DB, dünyanın herhangi bir yerindeki okuma ve yazma gecikmesini en aza indirmenin yanı sıra, bir bölgede hizmet kesintisi durumunda uygulamanızın sürekliliğini sağlayacak otomatik bölgesel hata mekanizması sağlar. 

* **En yaygın olarak benimsenen grafik sorgu standardına sahip hızlı sorgular ve geçişler**

  Heterojen vertices ve kenarları saklayın ve tanıdık bir Gremlin sözdizimi ile sorgulayın. Gremlin ortak grafik algoritmaları uygulamak için zengin bir arayüz sağlayan zorunlu, işlevsel bir sorgu dilidir. 
  
  Azure Cosmos DB, şema ipuçlarını, ikincil dizinleri veya görünümleri belirtmeye gerek kalmadan zengin gerçek zamanlı sorgular ve geçişler sağlar. Daha fazla bilgi için bkz. [Gremlin kullanarak sorgu grafikleri](gremlin-support.md).

* **Tam yönetilen grafik veritabanı**

  Azure Cosmos DB, veritabanı ve makine kaynaklarını yönetme ihtiyacını ortadan kaldırır. Varolan grafik veritabanı platformlarının çoğu altyapısınırlamalarına bağlıdır ve genellikle çalışmasını sağlamak için yüksek derecede bakım gerektirir. 
  
  Tam olarak yönetilen bir hizmet olarak Cosmos DB, sanal makineleri yönetme, çalışma zamanı yazılımını güncelleme, parçalama veya çoğaltma yı yönetme veya karmaşık veri katmanı yükseltmeleriyle başa çıkma gereksinimini ortadan kaldırır. Tüm grafikler otomatik olarak yedeklenir ve bölgesel arızalara karşı korunur. Bu garantiler, geliştiricilerin grafik veritabanlarını işletmek ve yönetmek yerine uygulama değeri sağlamaya odaklanmalarını sağlar. 

* **Otomatik dizin oluşturma**

  Azure Cosmos DB, varsayılan olarak grafiğin düğümlerindeki ve kenarlarındaki tüm özelliklerin otomatik olarak dizinini oluşturur ve herhangi bir şemayı ya da ikincil dizinlerin oluşturulmasını beklemez veya gerektirmez. [Azure Cosmos DB'de dizin oluşturma](https://docs.microsoft.com/azure/cosmos-db/index-overview)hakkında daha fazla bilgi edinin. 

* **Apache TinkerPop ile uyumluluk**

  Azure Cosmos DB [açık kaynak Kodlu Apache TinkerPop standardını](https://tinkerpop.apache.org/)destekler. Tinkerpop standardı, Azure Cosmos DB'nin Gremlin API'si ile kolayca entegre edilebilen geniş bir uygulama ve kitaplık ekosistemine sahiptir. 

* **Tunable tutarlılık düzeyleri**

  Azure Cosmos DB, uygulamanız için tutarlılık ve performans arasında doğru dengeyi elde etmek için beş iyi tanımlanmış tutarlılık düzeyi sağlar. Azure Cosmos DB sorgular ve okuma işlemleri için beş farklı tutarlılık düzeyi sunar: güçlü, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve son. Bu ayrıntılı ve iyi tanımlanmış tutarlılık düzeyleri tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlam bir denge kurmanıza olanak sağlar. [Azure Cosmos DB'deki ayarlanabilir tutarlılık düzeyleri](consistency-levels.md) sayfasına giderek daha fazla bilgi edinin.

## <a name="scenarios-that-can-use-gremlin-api"></a>Gremlin API kullanabilen senaryolar
Azure Cosmos DB grafik desteğinin yararlı olabileceği bazı senaryolar şunlardır:

* **Sosyal ağlar/Müşteri 365**

  Müşterileriniz ve diğer kişilerle etkileşimleri hakkındaki verileri birleştirerek kişiselleştirilmiş deneyimleri geliştirebilir, müşteri davranışını tahmin edebilir veya kişileri benzer ilgi alanlarına sahip başkalarına bağlayabilirsiniz. Azure Cosmos DB, sosyal ağları yönetmek ve müşteri tercihlerini ve verilerini izlemek için kullanılabilir.

* **Öneri altyapıları**

  Bu senaryo, perakende sektöründe yaygın olarak kullanılır. Ürünler, kullanıcılar ve satın alma, gözatma veya bir öğeyi derecelendirme gibi kullanıcı etkileşimleri hakkındaki bilgileri birleştirerek özelleştirilmiş öneriler oluşturabilirsiniz. Azure Cosmos DB'nin düşük gecikme sonu, elastik ölçek ve yerel grafik desteği bu senaryolar için idealdir.

* **Jeo-uzamsal**

  Telekomünikasyon, lojistik ve seyahat planlama alanlarındaki birçok uygulamanın, bir bölgede ilgi çekici bir konum bulması veya iki konum arasındaki en kısa/en uygun yolu belirlemesi gerekir. Azure Cosmos DB, bu sorunları için idealdir.

* **Nesnelerin İnterneti**

  IOT cihazları arasındaki ağ ve bağlantılar bir grafik olarak modellendiğinde, cihazlarınızın ve varlıklarınızın durumunu daha iyi kavrayabilirsiniz. Ayrıca ağın bir bölümündeki değişikliklerin başka bir bölümü nasıl etkileyebileceğini de öğrenebilirsiniz.

## <a name="introduction-to-graph-databases"></a>Grafik veritabanlarına giriş
Gerçek dünyada görünen veriler doğal olarak bağlıdır. Geleneksel veri modelleme, varlıkları ayrı ayrı tanımlamaya ve çalışma zamanında ilişkilerini hesaplamaya odaklanır. Bu modelin avantajları olsa da, yüksek oranda bağlı verileri kısıtlamaları altında yönetmek zor olabilir.  

Grafik veritabanı yaklaşımı, bunun yerine depolama katmanındaki kalıcı ilişkileri kalıcı olarak yerine getirir ve bu da yüksek verimli grafik alma işlemlerine yol açar. Azure Cosmos DB'nin Gremlin API [özelliği grafik modelini](https://tinkerpop.apache.org/docs/current/reference/#intro)destekler.

### <a name="property-graph-objects"></a>Özellik grafiği nesneleri

Özellik [grafiği,](http://mathworld.wolfram.com/Graph.html) [tepe ve](http://mathworld.wolfram.com/GraphVertex.html) [kenarlarından](http://mathworld.wolfram.com/GraphEdge.html)oluşan bir yapıdır. Her iki nesnede de özellik olarak rasgele anahtar değeri çifti sayısı olabilir. 

* **Vertices** - Vertices bir kişi, bir yer veya bir olay gibi ayrık varlıklar, gösterir.

* **Kenarlar** - Kenarlar, köşeler arasındaki ilişkileri gösterir. Bir kişinin başka bir kişiyi tanıması, bir etkinliğe katılması veya kısa süre önce bir konumda bulunması bağlantılar buna örnek gösterilebilir. 

* **Özellikler** - Özellikler, köşeler ve kenarlar hakkında bilgi ifade eder. Vertices veya kenarlarında özellikleri herhangi bir sayı olabilir ve bir sorgunesneleri açıklamak ve filtrelemek için kullanılabilir. Örnek özellikler arasında ad ve yaş veya zaman damgası ve/veya ağırlığı olabilecek bir kenar noktası vardır. 

Grafik veritabanları genellikle NoSQL veya ilişkisel olmayan veritabanı kategorisine dahil edilir, çünkü şema veya kısıtlı veri modeline bağımlılık yoktur. Bu şema eksikliği, bağlı yapıların doğal ve verimli bir şekilde modellemesine ve depolanmasına olanak sağlar. 

### <a name="gremlin-by-example"></a>Örneğe göre Gremlin
Sorguların Gremlin’de nasıl ifade edildiğini anlamak için örnek bir grafik kullanalım. Aşağıdaki şekilde kullanıcılar, ilgi alanları ve cihazlar hakkındaki verileri yöneten bir iş uygulaması grafik biçiminde gösterilir.  

![Kişileri, cihazları ve ilgi alanlarını gösteren örnek grafik](./media/gremlin-support/sample-graph.png) 

Bu grafikaşağıdaki *tepe noktası* türlerine sahiptir (Gremlin'de "etiket" olarak adlandırılır):

- **İnsanlar**: Grafik üç kişi, Robin, Thomas ve Ben vardır
- **İlgi Alanları**: Onların çıkarları, bu örnekte, Futbol oyunu
- **Cihazlar**: İnsanların kullandığı cihazlar
- **İşletim Sistemleri**: Cihazların çalıştığı işletim sistemleri

Bu varlıklar arasındaki ilişkileri aşağıdaki *kenar* türleri/etiketler aracılığıyla temsil ediyoruz:

- **Bilir**: Örneğin, "Thomas Robin bilir"
- **İlgilenen**: Grafiğimizde insanların çıkarlarını temsil etmek, örneğin, "Ben Futbolla ilgileniyor"
- **RunsOS**: Dizüstü Bilgisayar Windows Işletim Sistemi çalışır
- **Kullanım Alanları**: Kişinin hangi cihazı kullandığını temsil etmek. Örneğin Robin, seri numarası 77 olan bir Motorola telefon kullanır

Şimdi [Gremlin Console](https://tinkerpop.apache.org/docs/3.3.2/reference/#gremlin-console)’u kullanarak bu grafiğe yönelik birkaç işlem yapalım. Dilerseniz bu işlemleri, tercih ettiğiniz platformdaki (Java, Node.js, Python veya .NET) Gremlin sürücülerini kullanarak da gerçekleştirebilirsiniz.  Azure Cosmos DB’de nelerin desteklendiğine bakmadan önce söz dizimine hakkında bilgi edinmek için birkaç örneğe bakalım.

İlk olarak CRUD’a bakalım. Aşağıdaki Gremlin deyimi “Thomas” köşesini grafiğe ekler:

```java
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Daha sonra aşağıdaki Gremlin deyimi, Thomas ve Robin arasına bir “Tanıma” kenarı ekler.

```java
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

Aşağıdaki sorgu, “kişiler” köşesini ilk adlarına göre azalan sırada döndürür:
```java
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Grafiklerin asıl iyi olduğu kısımlar, “Thomas’ın arkadaşları hangi işletim sistemini kullanıyor?” gibi sorular sorduğunuzda ortaya çıkıyor. Bu bilgileri grafikten almak için bu Gremlin geçişini çalıştırabilirsiniz:

```java
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB’de grafik desteği hakkında daha fazla bilgi için bkz.

* [Azure Cosmos DB grafik öğreticisi](create-graph-dotnet.md) ile çalışmaya başlayın.
* [Azure Cosmos DB’de Gremlin kullanarak grafikleri sorgulamayı](gremlin-support.md) öğrenin.
