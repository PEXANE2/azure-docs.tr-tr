---
title: Azure Cosmos DB için Azure Synapse Link hakkında sık sorulan sorular
description: Faturalandırma, analitik mağaza, güvenlik ve analitik depoda yaşam süresi gibi alanlarda Azure Cosmos DB için SYNAPSE bağlantısı hakkında sık sorulan soruların yanıtlarını alın.
author: srchi
ms.author: srchi
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.openlocfilehash: d16da1588c2fa90c4a40a829ed660fd089ff29ac
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85392378"
---
# <a name="frequently-asked-questions-about-azure-synapse-link-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Synapse Link hakkında sık sorulan sorular

Azure Cosmos DB için Azure SYNAPSE bağlantısı Azure Cosmos DB ile Azure SYNAPSE Analytics arasında sıkı bir tümleştirme oluşturur. Müşterilerin, işletimsel iş yüklerinden ve ETL işlem hattı olmadan tam performans yalıtımıyla neredeyse gerçek zamanlı analizler çalıştırmasına olanak sağlar. Bu makalede, Azure Cosmos DB için SYNAPSE bağlantısı hakkında sıkça sorulan sorular yanıtlanmaktadır.

## <a name="general-faq"></a>Genel SSS

### <a name="is-synapse-link-supported-for-all-azure-cosmos-db-apis"></a>Tüm Azure Cosmos DB API 'Leri için SYNAPSE link destekleniyor mu?
Genel Önizleme sürümünde, SYNAPSE link yalnızca Azure Cosmos DB SQL (Core) API 'SI için desteklenir. Cosmos DB Mongo DB & API 'SI için destek Cassandra API Şu anda geçitli önizleme aşamasındadır.  Geçitli önizlemeye erişim istemek için [Azure Cosmos DB ekibine](mailto:cosmosdbsynapselink@microsoft.com)ulaşın.

### <a name="is-synapse-link-supported-for-multi-region-azure-cosmos-accounts"></a>Çok bölgeli Azure Cosmos hesapları için SYNAPSE bağlantısı destekleniyor mu?
Evet, çok bölgeli Azure Cosmos hesaplarında, analitik depoda depolanan veriler de küresel olarak dağıtılır. Tek bir yazma bölgesinden (tek yönetici) veya birden fazla yazma bölgesinden (çoklu yönetici olarak da bilinir) bağımsız olarak, Azure SYNAPSE Analytics 'ten gerçekleştirilen analitik sorgular, en yakın yerel bölgeden sunulabilir.

Analitik mağaza desteğiyle çok bölgeli bir Azure Cosmos hesabını yapılandırmayı planlarken, hesap oluşturma sırasında tüm gerekli bölgelerin eklenmesini öneririz.

### <a name="can-i-choose-to-enable-synapse-link-for-only-certain-region-and-not-all-regions-in-a-multi-region-account-set-up"></a>Yalnızca belirli bir bölge için SYNAPSE bağlantısını etkinleştirmeyi seçebilir ve çok bölgeli bir hesap kümesi içindeki tüm bölgeleri değil mi?
Önizleme sürümünde, SYNAPSE link çok bölgeli bir hesap için etkinleştirildiğinde, analitik depo tüm bölgelerde oluşturulur. Temel alınan veriler işlem deposunda işleme ve işlem tutarlılığı için iyileştirilmiştir.

### <a name="can-i-disable-the-synapse-link-feature-for-my-azure-cosmos-account"></a>Azure Cosmos hesabımın SYNAPSE Link özelliğini devre dışı bırakabilir miyim?
Şu an için Synapse Link özelliğini hesap düzeyinde etkinleştirdikten sonra devre dışı bırakmanız mümkün değildir.  Özelliği devre dışı bırakmak istiyorsanız, yeni bir Azure Cosmos hesabını silip yeniden oluşturmanız gerekir.

Hesap düzeyinde SYNAPSE bağlantısı özelliği etkinse ancak analitik depo etkin kapsayıcılar yoksa faturalandırma **etkilerine sahip olmamak** istediğinizi anlayın.

## <a name="azure-cosmos-db-analytical-store"></a>Analitik depo Azure Cosmos DB

### <a name="can-i-enable-analytical-store-on-existing-containers"></a>Mevcut kapsayıcılar üzerinde analitik depolamayı etkinleştirebilir miyim?
Şu anda analitik depo yalnızca yeni kapsayıcılar için etkinleştirilebilir (hem yeni hem de mevcut hesaplarda).

### <a name="can-i-disable-analytical-store-on-my-azure-cosmos-containers-after-enabling-it-during-container-creation"></a>Azure Cosmos kapsayıcılarındaki analitik depoyu, kapsayıcı oluşturma sırasında etkinleştirildikten sonra devre dışı bırakabilir miyim?
Şu anda analitik depo, kapsayıcı oluşturma sırasında etkinleştirildikten sonra bir Azure Cosmos kapsayıcısında devre dışı bırakılamaz.

### <a name="is-analytical-store-supported-for-azure-cosmos-containers-with-autoscale-provisioned-throughput"></a>Analitik depo, otomatik ölçeklendirme sağlanan aktarım hızı ile Azure Cosmos kapsayıcıları için destekleniyor mu?
Evet, analitik depo otomatik ölçeklendirme sağlanan aktarım hızı ile kapsayıcılarda etkinleştirilebilir.

### <a name="is-there-any-effect-on-azure-cosmos-db-transactional-store-provisioned-rus"></a>Azure Cosmos DB işlemsel depoda sağlanan bir efekt var mı?
Azure Cosmos DB, işlem ve analitik iş yükleri arasında performans yalıtımını garanti eder. Bir kapsayıcıda analitik deponun etkinleştirilmesi, Azure Cosmos DB işlem deposunda sağlanan RU/s 'yi etkilemez. Analitik mağazaya yönelik işlemler (okuma & yazma) ve depolama maliyetleri de ayrı olarak ücretlendirilir. Daha fazla bilgi için bkz. [analitik mağaza Azure Cosmos DB fiyatlandırması](analytical-store-introduction.md#analytical-store-pricing) .

### <a name="are-delete-and-update-operations-on-the-transactional-store-reflected-in-the-analytical-store"></a>, Analitik depoya yansıtılan işlem deposunda silme ve güncelleştirme işlemleri mi var? 
Evet, işlem deposundaki verilerin silmeleri ve güncelleştirmeleri analitik depoya yansıtılır. Analitik deponun, analitik TTL ölçütlerine uyan tüm öğelerin tüm sürümlerini tutması için, kapsayıcıda yaşam süresi (TTL) değerini geçmiş verileri içerecek şekilde yapılandırabilirsiniz. Daha fazla ayrıntı için bkz. [ANALITIK TTL 'ye genel bakış](analytical-store-introduction.md#analytical-ttl) .

### <a name="can-i-connect-to-analytical-store-from-analytics-engines-other-than-azure-synapse-analytics"></a>Azure SYNAPSE Analytics dışındaki analiz altyapılarından analitik depoya bağlanabilir miyim?
Analiz deposuna erişmek ve sorgu çalıştırmak için Azure Synapse Analytics tarafından sağlanan çeşitli çalışma zamanlarını kullanmanız gerekir. Analiz deposunu sorgulamak ve analiz etmek için şu bileşenleri kullanabilirsiniz:

* Scala, Python, Mini SQL ve C# için tam destekle SYNAPSE spark. Synapse Spark, veri mühendisliği ve veri bilimi senaryolarında temel bir rol oynar.
* T-SQL dili ve bilindik BI araçları (Power BI Premium gibi) desteğiyle sunucusuz SQL

### <a name="can-i-connect-to-analytical-store-from-synapse-sql-provisioned"></a>SYNAPSE SQL tarafından sağlanan analitik depoya bağlanabilir miyim?
Şu anda, SYNAPSE SQL tarafından sağlanan analitik depoya erişilemez.

### <a name="can-i-write-back-the-query-aggregation-results-from-synapse-back-to-the-analytical-store"></a>Sorgu toplama sonuçlarını SYNAPSE 'den analitik depoya geri yazabilir miyim?
Analitik depo, bir Azure Cosmos kapsayıcısında salt bir salt okuma deposudur. Bu nedenle, toplama sonuçlarını doğrudan analitik depoya geri yazamaz, ancak başka bir kapsayıcının Azure Cosmos DB işlem deposuna yazabilir, daha sonra bir hizmet katmanı olarak yararlanılabilir.

### <a name="is-the-autosync-replication-from-transactional-store-to-the-analytical-store-asynchronous-or-synchronous-and-what-are-the-latencies"></a>İşlemsel depodan analitik depoya zaman uyumsuz veya zaman uyumlu olarak yapılan ve gecikme sürelerinin ne olduğu ile olan bir oto Sync çoğaltması 
Çoğaltma zaman uyumsuzdur ve şu anda beklenen gecikme süresi 2 dakikadan fazla.

### <a name="are-there-any-scenarios-where-the-items-from-the-transactional-store-are-not-automatically-propagated-to-the-analytical-store"></a>İşlem deposundaki öğelerin otomatik olarak analitik depoya yayılmadığı senaryolar var mı?
Kapsayıcıda belirli öğeler [analiz için iyi tanımlanmış şemayı](analytical-store-introduction.md#analytical-schema)ihlal ediyor ise, bu, analitik depoya dahil edilmez. Analiz için iyi tanımlanmış şema ile engellenen senaryolar varsa, yardım için [Azure Cosmos DB ekibine](mailto:cosmosdbsynapselink@microsoft.com) e-posta gönderin.

### <a name="can-i-partition-the-data-in-analytical-store-differently-from-transactional-store"></a>Analitik depodaki verileri işlem deposundan farklı şekilde bölümleyebilir miyim?
Analiz deposundaki veriler, işlem deposundaki parçaların yatay bölümlemesi temel alınarak bölümlenir. Şu an için analiz deposunda farklı bir bölümleme stratejisi belirlemek mümkün değildir.

### <a name="can-i-customize-or-override-the-way-transactional-data-is-transformed-into-columnar-format-in-the-analytical-store"></a>İşlem verilerinin analitik depodaki sütunlu biçime nasıl dönüştürüleceklerini özelleştirebilir veya geçersiz kılabilir miyim?
Şu anda, işlem deposundan otomatik olarak analitik depoya yayıldığında veri öğelerini dönüştüremiyoruz. Bu sınırlama tarafından engellenen senaryolarınız varsa [Azure Cosmos DB ekibine](mailto:cosmosdbsynapselink@microsoft.com)e-posta gönderin.

## <a name="analytical-time-to-live-ttl"></a>Analitik yaşam süresi (TTL)

### <a name="is-ttl-for-analytical-data-supported-at-both-container-and-item-level"></a>Her iki kapsayıcıda ve öğe düzeyinde desteklenen analitik veriler için TTl mi?
Şu anda, analitik veriler için TTl yalnızca kapsayıcı düzeyinde yapılandırılabilir ve öğe düzeyinde analitik TTL ayarlama desteği yoktur.

### <a name="after-setting-the-container-level--analytical-ttl-on-an-azure-cosmos-db-container-can-i-change-to-a-different-value-later"></a>Azure Cosmos DB kapsayıcısında kapsayıcı düzeyi analitik TTL ayarlandıktan sonra, daha sonra farklı bir değere değiştirebilir miyim?
Evet, analitik TTL geçerli herhangi bir değere güncelleştirilebilen olabilir. Analitik TTL hakkında daha fazla bilgi için bkz. [ANALITIK TTL](analytical-store-introduction.md#analytical-ttl) makalesi.

### <a name="can-i-update-or-delete-an-item-from-the-analytical-store-after-it-has-been-ttld-out-from-the-transactional-store"></a>Bir öğeyi, işlem deposundan TTL kapatıldıktan sonra analiz deposundan güncelleştirebilir veya silebilir miyim?
Tüm işlem güncelleştirmeleri ve silmeleri analitik depoya kopyalanır ancak öğe, işlem deposundan temizlendiyse, analitik depoda güncelleştirilemez. Daha fazla bilgi için bkz. [ANALITIK TTL](analytical-store-introduction.md#analytical-ttl) makalesi.

## <a name="billing"></a>Faturalandırma

### <a name="what-is-the-billing-model-of-synapse-link-for-azure-cosmos-db"></a>Azure Cosmos DB için SYNAPSE bağlantısının faturalandırma modeli nedir?
[Azure Cosmos DB analitik mağaza](analytical-store-introduction.md) , 30 Ağustos 2020 ' a kadar analitik mağaza ücreti olmadan genel önizlemede kullanılabilir. SYNAPSE Spark ve SYNAPSE SQL, [SYNAPSE hizmet tüketimine](https://azure.microsoft.com/pricing/details/synapse-analytics/)göre faturalandırılır.

## <a name="security"></a>Güvenlik

### <a name="what-are-the-ways-to-authenticate-with-the-analytical-store"></a>Analitik depoda kimlik doğrulaması yapma yolları nelerdir?
Analitik depo ile kimlik doğrulaması, bir işlem deposuyla aynıdır. Belirli bir veritabanı için, ana veya salt okunurdur anahtarıyla kimlik doğrulaması yapabilirsiniz. Spark Not defterlerindeki Azure Cosmos DB anahtarlarının yapıştırmayı engellemek için SYNAPSE Studio 'daki bağlı hizmetten yararlanabilirsiniz. Bu bağlı hizmete erişim, çalışma alanına erişimi olan herkes için kullanılabilir.

## <a name="synapse-run-times"></a>SYNAPSE çalışma zamanları

### <a name="what-are-the-currently-supported-synapse-run-times-to-access-azure-cosmos-db-analytical-store"></a>Azure Cosmos DB analitik depoya erişmek için şu anda desteklenen SYNAPSE çalışma zamanları nelerdir?

|SYNAPSE çalışma zamanı |Geçerli destek |
|---------|---------|
|SYNAPSE Spark havuzları | Okuma, yazma (işlem deposu aracılığıyla), tablo, geçici görünüm |
|SYNAPSE SQL Server daha az    | Okuma, görüntüleme (geçişli Önizleme)  |
|SYNAPSE SQL sağlandı   |  Kullanılamaz |

### <a name="do-my-synapse-spark-tables-sync-with-my-synapse-sql-serverless-tables-the-same-way-they-do-with-azure-data-lake"></a>SYNAPSE Spark tablolarım SYNAPSE SQL sunucusuz tablolarım Azure Data Lake ile aynı şekilde eşitlensin mi?
Şu anda bu özellik kullanılamıyor.

### <a name="can-i-do-spark-structured-streaming-from-analytical-store"></a>Analitik depodan Spark yapılandırılmış akışı yapabilir miyim?
Azure Cosmos DB için Spark yapılandırılmış akış desteği, işlem deposunun değişiklik akışı işlevselliği kullanılarak uygulanır ve henüz analitik depolamadan desteklenmez.

## <a name="synapse-studio"></a>Synapse Studio

### <a name="in-the-synapse-studio-how-do-i-recognize-if-im-connected-to-an-azure-cosmos-db-container-with-the-analytics-store-enabled"></a>SYNAPSE Studio 'da, analiz deposu etkinleştirilmiş bir Azure Cosmos DB kapsayıcısına bağlanıp bağlandığımı nasıl anlayabilirim?
Analitik depo ile etkinleştirilen bir Azure Cosmos DB kapsayıcısı aşağıdaki simgeye sahiptir:

:::image type="content" source="./media/synapse-link-frequently-asked-questions/analytical-store-icon.png" alt-text="Analitik depolama ile etkin Azure Cosmos DB kapsayıcısı-simge":::

Bir işlem deposu kapsayıcısı aşağıdaki simgeyle temsil edilir:

:::image type="content" source="./media/synapse-link-frequently-asked-questions/transactional-store-icon.png" alt-text="İşlem deposu ile etkin Azure Cosmos DB kapsayıcısı-simge":::
 
### <a name="how-do-you-pass-azure-cosmos-db-credentials-from-synapse-studio"></a>Azure Cosmos DB kimlik bilgilerini SYNAPSE Studio 'dan nasıl geçitirsiniz?
Bağlı hizmet, Azure Cosmos DB veritabanlarına erişimi olan kullanıcı tarafından oluşturulurken şu anda Azure Cosmos DB kimlik bilgileri geçirilir. Bu mağazaya erişim, çalışma alanına erişimi olan diğer kullanıcılar tarafından kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [SYNAPSE bağlantısının avantajları](synapse-link.md#synapse-link-benefits) hakkında bilgi edinin

* [SYNAPSE bağlantısı ve Azure Cosmos DB arasında tümleştirme](synapse-link.md#synapse-link-integration)hakkında bilgi edinin.
