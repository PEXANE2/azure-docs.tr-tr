---
title: Azure Cosmos DB'ye Giriş
description: Azure Cosmos DB hakkında bilgi edinin. Bu genel olarak dağıtılan çok modelli veritabanı; düşük gecikme süresi, esnek ölçeklenebilirlik, yüksek kullanılabilirlik için oluşturulmuştur ve NoSQL verileri için yerel destek sunar.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: overview
ms.date: 10/23/2019
ms.openlocfilehash: 2a09d5af013e804f33327855fb7b9f2104bc225c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240397"
---
# <a name="welcome-to-azure-cosmos-db"></a>Azure Cosmos DB’ye hoş geldiniz

Bugünün uygulamaları son derece duyarlı ve her zaman online olması gereklidir. Düşük gecikme süresi ve yüksek kullanılabilirlik elde etmek için, bu uygulamaların örneklerinin kullanıcılarına yakın veri merkezlerinde dağıtılması gerekir. Uygulamaların yoğun saatlerde kullanımdaki büyük değişikliklere gerçek zamanlı olarak yanıt vermesi, artan veri hacimlerini depolaması ve bu verileri milisaniyeler içinde kullanıcılara sunması gerekir.

Azure Cosmos DB, Microsoft'un genel olarak dağıtılan çok modelli veritabanı hizmetidir. Cosmos DB, bir düğmeye tıklamayla, dünya çapında ki herhangi bir Azure bölgesinde iş ve depolama alanını elastik ve bağımsız olarak ölçeklendirmenize olanak tanır. SQL, MongoDB, Cassandra, Tables veya Gremlin gibi favori API'nizi kullanarak iş verisini ve depolamayı elastik olarak ölçeklendirebilir ve hızlı, tek basamaklı milisaniyelik veri erişiminden yararlanabilirsiniz. Cosmos DB, başka hiçbir veritabanı [hizmetinin](https://aka.ms/acdbsla) sunmaması nedeniyle iş sonu, kullanılabilirlik ve tutarlılık garantileri için kapsamlı hizmet düzeyi anlaşmaları (SLA'lar) sağlar.

Azure [Cosmos DB'yi Azure](https://azure.microsoft.com/try/cosmosdb/) aboneliği olmadan, ücretsiz ve taahhütsüz olarak ücretsiz olarak deneyebilir veya ilk 400 RU/s ve 5 GB depolama alanına ücretsiz bir hesap almak için [Azure Cosmos DB ücretsiz katmanını](optimize-dev-test.md#azure-cosmos-db-free-tier) kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB'yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)

![Azure Cosmos DB, Microsoft'un esnek ölçek genişletme, garantili düşük gecikme süresi, beş tutarlılık modeli ve kapsamlı SLA garantisi ile genel olarak dağıtılmış veritabanı hizmetidir](./media/introduction/azure-cosmos-db.png)

## <a name="key-benefits"></a>Önemli Avantajlar

### <a name="turnkey-global-distribution"></a>Anahtar teslim küresel dağıtım

Cosmos DB, dünya çapında son derece duyarlı ve yüksek oranda kullanılabilir uygulamalar oluşturmanıza olanak tanır. Cosmos DB, kullanıcılarınız nerede olurlarsa olsunlar verilerinizi saydam bir şekilde çoğaltır, böylece kullanıcılarınız kendilerine en yakın olan verilerin bir kopyasıyla etkileşimkurabilir.

Cosmos DB, tek bir düğmeye tıklayarak Azure bölgelerinden herhangi birini cosmos hesabınıza istediğiniz zaman eklemenize veya kaldırmanıza olanak tanır. Cosmos DB, uygulamanız hizmetin *çoklu homing* yetenekleri sayesinde son derece kullanılabilir olmaya devam ederken, verilerinizi Cosmos hesabınızla ilişkili tüm bölgelere sorunsuz bir şekilde çoğaltır. Daha fazla bilgi için [genel dağıtım](distribute-data-globally.md) makalesine bakın.

### <a name="always-on"></a>Her Zaman Anın

Cosmos DB, Azure altyapısıyla derin tümleştirme ve [saydam çoklu ana çoğaltma](global-dist-under-the-hood.md)sayesinde hem okuma hem de yazma için [%99,999 yüksek kullanılabilirlik](high-availability.md) sağlar. Cosmos DB ayrıca size programlı (veya Portal üzerinden) Cosmos hesabınızın bölgesel başarısızlığını teşvik etme olanağı sağlar. Bu özellik, uygulamanızın bölgesel felaket durumunda başarısız olacak şekilde tasarlandığından emin olmaya yardımcı olur.

### <a name="elastic-scalability-of-throughput-and-storage-worldwide"></a>Dünya çapında elastik ölçeklenebilirlik ve depolama

Şeffaf yatay bölümleme ve çok ana çoğaltma ile tasarlanan Cosmos DB, dünyanın her yerinde yazdığınız ve okuduğunuz için eşi görülmemiş elastik ölçeklenebilirlik sunar. Tek bir API çağrısıyla dünya çapında binlerce istek/sn'den yüz milyonlarca sn'ye kadar elastik olarak ölçeklendirebilir ve yalnızca ihtiyacınız olan iş (ve depolama) için ödeme yapabilirsiniz. Bu özellik, tepe noktası için aşırı sağlama gerekmeden iş yüklerinizdeki beklenmeyen ani artışlarla başa çıkmanıza yardımcı olur. Daha fazla bilgi için, [Cosmos DB bölümleme](partitioning-overview.md)bakın , [kapsayıcılar ve veritabanları üzerinde sağlanan iş bölümü](set-throughput.md), ve [ölçekleme küresel olarak sağlanan iş girdisi](scaling-throughput.md).

### <a name="guaranteed-low-latency-at-99th-percentile-worldwide"></a>Dünya çapında 99 yüzdelik düşük gecikme garantisi

Cosmos DB kullanarak, son derece duyarlı, gezegen ölçekli uygulamalar oluşturabilirsiniz. Onun yeni çok-master çoğaltma protokolü ve mandal ücretsiz ve [yazma-optimize veritabanı motoru](index-policy.md)ile, Cosmos DB her ikisi için 10-ms gecikme daha az garanti , okur (endeksli) ve 99 yüzdelik olarak yazıyor, tüm dünyada. Bu özellik, yüksek duyarlı uygulamalar için sürekli veri ve hızlı sorguların yutulmasını sağlar.

### <a name="precisely-defined-multiple-consistency-choices"></a>Tam olarak tanımlanmış, birden çok tutarlılık seçeneği

Cosmos DB'de genel olarak dağıtılmış uygulamalar oluştururken, [artık tutarlılık, kullanılabilirlik, gecikme süresi ve üretim arasında](consistency-levels-tradeoffs.md)aşırı dengeler kurmanız gerekmez. Cosmos DB'nin çok analı çoğaltma protokolü, küresel olarak dağıtılan uygulamanız için düşük gecikme süresi ve yüksek kullanılabilirliğe sahip sezgisel bir programlama modeli için [güçlü,](consistency-levels.md) - *strong* *sınırlı bayatlık,* *oturum,* *tutarlı önek*ve *nihai* olarak beş iyi tanımlanmış tutarlılık seçeneği sunmak üzere özenle tasarlanmıştır.

### <a name="no-schema-or-index-management"></a>Şema veya dizin yönetimi yok

Veritabanı şealarını ve dizinlerini bir uygulamanın şemasıyla senkronize tutmak, özellikle küresel olarak dağıtılan uygulamalar için acı vericidir. Cosmos DB ile şema veya dizin yönetimi ile uğraşmak gerekmez. Veritabanı motoru tamamen şema-agnostik.  Şema ve dizin yönetimi gerekmediğinden, şemaları geçirerken uygulama kapalı kalma süresi konusunda da endişelenmenize gerek yoktur. Cosmos DB [tüm verileri otomatik olarak dizine eder](index-policy.md) ve sorguları hızlı bir şekilde yanıtlar.

### <a name="battle-tested-database-service"></a>Savaş test veritabanı hizmeti

Cosmos DB, Azure'da temel bir hizmettir. Cosmos DB, yaklaşık on yıldır Skype, Xbox, Office 365, Azure ve diğerleri de dahil olmak üzere, küresel ölçekte kritik görev uygulamaları için Microsoft'un birçok ürünü tarafından kullanılmaktadır. Bugün Cosmos DB, birçok harici müşteri ve elastik ölçek, anahtar teslimi küresel dağıtım, düşük gecikme süresi ve her ikisinin de yüksek kullanılabilirliği için çok büyük çoğaltma gerektiren görev açısından kritik uygulamalar tarafından kullanılan Azure'daki en hızlı büyüyen hizmetlerden biridir. okur ve yazar.

### <a name="ubiquitous-regional-presence"></a>Her yerde bölgesel varlık

Cosmos DB, genel buluttaki 54+bölge, Azure China [21Vianet, Azure](https://www.azure.cn/en-us/)Almanya, Azure Resmi ve Savunma Bakanlığı için Azure Kamu (DoD) dahil olmak üzere dünya çapındaki tüm Azure bölgelerinde kullanılabilir. [Cosmos DB'nin bölgesel varlığına](regional-presence.md)bakın.

### <a name="secure-by-default-and-enterprise-ready"></a>Varsayılan olarak güvenli ve kurumsal hazır

Cosmos DB, çok [çeşitli uyumluluk standartları](compliance.md)için sertifikaya uygundur. Ayrıca, Cosmos DB'deki tüm veriler istirahatte ve hareket halinde şifrelenir. Cosmos DB satır düzeyinde yetkilendirme sağlar ve sıkı güvenlik standartlarına bağlıdır.

### <a name="significant-tco-savings"></a>Önemli TCO tasarrufları

Cosmos DB tam olarak yönetilen bir hizmet olduğundan, veritabanı yazılımınızın karmaşık çoklu veri merkezi dağıtımlarını ve yükseltmelerini yönetmeniz ve çalıştırmanız, destek, lisanslama veya işlemleri için ödeme yapmanız veya veritabanınızı en yüksek iş yükü için sağlamanız gerekmez. Daha fazla bilgi için [Cosmos DB ile maliyeti Optimize Edin'](total-cost-ownership.md)e bakın.

### <a name="industry-leading-comprehensive-slas"></a>Sektör lideri kapsamlı SLA'lar

Cosmos DB, %99,999 yüksek kullanılabilirliği kapsayan [endüstri lideri kapsamlı SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/) sunan ilk ve tek hizmettir, %99'luk oranda gecikme süresi, garantili iş ortası ve tutarlılık.

### <a name="globally-distributed-operational-analytics-and-ai-with-natively-built-in-apache-spark"></a>Yerel olarak yerleşik Apache Spark ile küresel olarak dağıtılan operasyonel analitik ve AI

[Spark'ı](spark-connector.md) doğrudan Cosmos DB'de depolanan veriler üzerinde çalıştırabilirsiniz. Bu özellik, doğrudan Cosmos DB'ye karşı çalışan işlem iş yüklerini etkilemeden küresel ölçekte düşük gecikmeli, operasyonel analitik yapmanızı sağlar. Daha fazla bilgi için [bkz.](lambda-architecture.md)

### <a name="develop-applications-on-cosmos-db-using-popular-open-source-software-oss-apis"></a>Popüler Açık Kaynak Yazılım (OSS) API'lerini kullanarak Cosmos DB'de uygulama geliştirme

Cosmos DB, Cosmos veritabanınızda depolanan verilerinizle çalışmak için çeşitli API'ler sunar. Varsayılan olarak, Cosmos veritabanınızı sorgulamak için SQL 'i (çekirdek API) [kullanabilirsiniz.](how-to-sql-query.md) Cosmos DB ayrıca [Cassandra,](cassandra-introduction.md) [MongoDB,](mongodb-introduction.md) [Gremlin](graph-introduction.md) ve [Azure Tablo Depolama](table-introduction.md)için API'ler uygular. Sık kullanılan NoSQL (örneğin, MongoDB, Cassandra, Gremlin) için istemci sürücüleri (ve araçları) doğrudan Cosmos veritabanınıza yönlendirebilirsiniz. Cosmos DB, yaygın olarak kullanılan NoSQL API'lerinin tel protokollerini destekleyerek şunları yapmanızı sağlar:

* Uygulama mantığınızın önemli bir bölümünü korurken uygulamanızı cosmos DB'ye kolayca geçirin.
* Uygulamanızı taşınabilir tutun ve bulut satıcısı-agnostik kalmaya devam edin.
* Ortak NoSQL API'leri için sektör lideri, mali destekli SLA'larla tam yönetilen bir bulut hizmeti alın. 
* İhtiyaçlarınıza göre veritabanlarınız için sağlanan veri veri ve depolamayı elastik olarak ölçeklendirin ve yalnızca ihtiyacınız olan iş ve depolama için ödeme leyin. Bu önemli maliyet tasarrufu yol açar.

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Azure Cosmos DB'den yararlanan çözümler

Büyük miktarda veriyi işlemesi gereken, okur ve çeşitli veriler için neredeyse gerçek yanıt süreleri ile [küresel ölçekte](distribute-data-globally.md) yazan herhangi bir [web, mobil, oyun ve IoT uygulaması](use-cases.md) Cosmos DB'nin garantili yüksek [kullanılabilirlik,](https://azure.microsoft.com/support/legal/sla/cosmos-db/)yüksek iş kaynağı, düşük gecikme süresi ve tunable tutarlılıktan yararlanacaktır. Azure Cosmos DB'nin [IoT ve telematik,](use-cases.md#iot-and-telematics) [perakende ve pazarlama,](use-cases.md#retail-and-marketing) [oyun ve](use-cases.md#gaming) web ve [mobil uygulamalar](use-cases.md#web-and-mobile-applications)oluşturmak için nasıl kullanılabileceğini öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB'nin temel kavramları hakkında daha fazla bilgi [anahtar teslim imiş küresel dağıtım](distribute-data-globally.md) ve [bölümleme](partitioning-overview.md) ve [tedarik edilen iş için.](request-units.md)

Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

* [Azure Cosmos DB SQL API’yi kullanmaya başlama](create-sql-api-dotnet.md)
* [MongoDB için Azure Cosmos DB'nin API'si ile başlayın](create-mongodb-nodejs.md)
* [Azure Cosmos DB Cassandra API’yi kullanmaya başlama](create-cassandra-dotnet.md)
* [Azure Cosmos DB Graph API’yi kullanmaya başlama](create-graph-dotnet.md)
* [Azure Cosmos DB Tablo API’yi kullanmaya başlama](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)
