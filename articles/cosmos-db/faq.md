---
title: Azure Cosmos DB'de farklı API'lerde sık sorulan sorular
description: Dünya çapında dağıtılan çok modelli veritabanı hizmeti Azure Cosmos DB hakkında sık sorulan soruların yanıtlarını alın. Kapasite, performans düzeyleri ve ölçekleme hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 114c632d43dbb254222def2437fbffc22ec84688
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80672456"
---
# <a name="frequently-asked-questions-about-different-apis-in-azure-cosmos-db"></a>Azure Cosmos DB'deki farklı API'ler hakkında sık sorulan sorular

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Azure Cosmos DB için tipik kullanım örnekleri nelerdir?

Azure Cosmos DB, otomatik ölçek, öngörülebilir performans, milisaniye yanıt sürelerinin hızlı sırası ve şema içermeyen veriler üzerinden sorgulama olanağının önemli olduğu yeni web, mobil, oyun ve IoT uygulamaları için iyi bir seçimdir. Azure Cosmos DB, uygulama veri modellerinin sürekli yinelemesini hızlı bir şekilde geliştirmeye ve desteklemeye kendini ödünç verir. Kullanıcı tarafından oluşturulan içeriği ve verileri yöneten uygulamalar [Azure Cosmos DB için yaygın kullanım örnekleridir.](use-cases.md)

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Azure Cosmos DB öngörülebilir performansı nasıl sunar?

[İstek birimi](request-units.md) (RU), Azure Cosmos DB'deki iş ortası ölçüsüdür. 1RU'luk bir iş, 1-KB belgenin GET'inin elde edilen emeğe karşılık gelir. Okuma, yazma, SQL sorguları ve depolanmış yordam yürütmeleri de dahil olmak üzere Azure Cosmos DB'deki her işlem, işlemi tamamlamak için gereken iş metod'u temel alan bir deterministik RU değerine sahiptir. CPU, IO ve bellek ve bunların her birinin uygulama bilgibirikiminizi nasıl etkilediğini düşünmek yerine, tek bir RU ölçüsü açısından düşünebilirsiniz.

Her Azure Cosmos konteynerini, saniyede elde edilen iş başına rus açısından sağlanan iş ortası ile yapılandırabilirsiniz. Herhangi bir ölçekteki uygulamalar için, RU değerlerini ölçmek için tek tek istekleri kıyaslayabilir ve tüm istekler arasında istek birimlerinin toplamını işlemek için bir kapsayıcı sağlayabilirsiniz. Ayrıca, uygulamanızın gereksinimleri geliştikçe kabınızın iş bilgililiğini büyütebilir veya küçültebilirsiniz. İstek birimleri hakkında daha fazla bilgi ve konteyner ihtiyaçlarınızı belirleme konusunda yardım [için, iş birimi hesap makinesini](https://www.documentdb.com/capacityplanner)deneyin.

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Azure Cosmos DB, anahtar/değer, sütun, belge ve grafik gibi çeşitli veri modellerini nasıl destekler?

Anahtar/değer (tablo), sütun, belge ve grafik veri modellerinin tümü, Azure Cosmos DB'nin üzerine inşa edildiği ARS (atomlar, kayıtlar ve diziler) tasarımı nedeniyle yerel olarak desteklenir. Atomlar, kayıtlar ve diziler kolayca eşlenebilir ve çeşitli veri modellerine yansıtılabilir. Modellerin bir alt kümesi için API'ler şu anda mevcuttur (SQL, MongoDB, Tablo ve Gremlin) ve ek veri modellerine özgü diğerleri gelecekte kullanılabilir.

Azure Cosmos DB, geliştiriciden herhangi bir şema veya ikincil dizin gerektirmeden yutturdığı tüm verileri otomatik olarak dizine ekleyen bir şema agnostik dizin oluşturma motoruna sahiptir. Altyapı, depolama düzenini dizin ve sorgu işleme alt sistemlerinden ayıran bir dizi mantıksal dizin düzenine (ters çevrilmiş, sütunlu, ağaç) dayanır. Cosmos DB ayrıca bir dizi tel protokolünü ve API'yi genişletilebilir bir şekilde destekleme ve bunları verimli bir şekilde çekirdek veri modeline (1) ve mantıksal dizin düzenleri (2) ile benzersiz bir şekilde birden fazla veri modelini doğal olarak destekleyebilecek şekilde çevirme yeteneğine sahiptir.

### <a name="can-i-use-multiple-apis-to-access-my-data"></a>Verilerime erişmek için birden çok API kullanabilir miyim?

Azure Cosmos DB, Microsoft'un genel olarak dağıtılan çok modelli veritabanı hizmetidir. Çoklu model, Azure Cosmos DB'nin birden çok API'yi ve birden çok veri modelini desteklediği anlamına geldiği durumlarda, farklı API'ler depolama ve tel iletişim protokolü için farklı veri biçimleri kullanır. Örneğin, SQL JSON kullanır, MongoDB BSON kullanır, Tablo EDM kullanır, Cassandra CQL kullanır, Gremlin GraphSON kullanır. Sonuç olarak, belirli bir hesaptaki verilere tüm erişim için aynı API'yi kullanmanızı öneririz.

Her API, birlikte çalışabilir Gremlin ve SQL API dışında bağımsız olarak çalışır.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Azure Cosmos DB HIPAA uyumlu mu?

Evet, Azure Cosmos DB HIPAA uyumludur. HIPAA, bağımsız olarak tanımlanabilen sağlık bilgilerinin kullanımı, açıklanması ve korunması için gereksinimler belirler. Daha fazla bilgi için bkz. [Microsoft Güven Merkezi](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Azure Cosmos DB'nin depolama sınırları nelerdir?

Bir kapsayıcının Azure Cosmos DB'de depolayabileceği toplam veri miktarı nın sınırı yoktur.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Azure Cosmos DB'nin iş olarak iş sınırları nelerdir?

Azure Cosmos DB'de bir kapsayıcının destekedebileceği toplam iş miktarı nın sınırı yoktur. Önemli fikir, iş yükünüzü yeterince çok sayıda bölüm anahtarı arasında kabaca eşit olarak dağıtmaktır.

### <a name="are-direct-and-gateway-connectivity-modes-encrypted"></a>Doğrudan ve Ağ Geçidi bağlantı modları şifrelenmiş mi?

Evet her iki mod da her zaman tamamen şifrelenir.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Azure Cosmos DB'nin maliyeti nedir?

Ayrıntılar için Azure [Cosmos DB fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cosmos-db/) sayfasına bakın. Azure Cosmos DB kullanım ücretleri, sağlanan kapsayıcı sayısına, kapsayıcıların çevrimiçi olduğu saat sayısına ve her konteyner için sağlanan iş sonuna göre belirlenir.

### <a name="is-a-free-account-available"></a>Ücretsiz bir hesap kullanılabilir mi?

Evet, hiçbir taahhüt olmadan, ücretsiz olarak zaman sınırlı bir hesaba kaydolabilirsiniz. Kaydolmak için [Azure Cosmos DB'yi ücretsiz olarak deneyin](https://azure.microsoft.com/try/cosmosdb/) veya Azure [Cosmos DB SSS'yi deneyin'de](#try-cosmos-db)daha fazla bilgi edinin.

Azure'da yeniyseniz, tüm Azure hizmetlerini denemeniz için size 30 gün ve kredi veren bir [Azure ücretsiz hesabına](https://azure.microsoft.com/free/)kaydolabilirsiniz. Visual Studio aboneliğiniz varsa, herhangi bir [Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) hizmetinde kullanabileceğiniz ücretsiz Azure kredisi için de uygunsunuz.

Azure aboneliği oluşturmadan uygulamanızı yerel olarak ücretsiz olarak geliştirmek ve test etmek için [Azure Cosmos DB Emülatörü](local-emulator.md) de kullanabilirsiniz. Uygulamanızın Azure Cosmos DB Öykünücüsü’ndeki performansından memnun olduğunuzda bulut üzerinde Azure Cosmos DB hesabı kullanmaya başlayabilirsiniz.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Azure Cosmos DB ile ilgili nasıl ek yardım alabilirim?

Teknik bir soru sormak için, bu iki soru ve yanıt forumlarından birine gönderebilirsiniz:

* [MSDN forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurecosmosdb)
* [Yığın Taşma](https://stackoverflow.com/questions/tagged/azure-cosmosdb). Stack Overflow programlama soruları için en iyisidir. Sorunuzun konu [üzerinde](https://stackoverflow.com/help/on-topic) olduğundan emin olun ve [soruyu açık ve yanıtlanabilir hale getirerek mümkün olduğunca çok ayrıntı sağlayın.](https://stackoverflow.com/help/how-to-ask)

Yeni özellikler istemek [için, Kullanıcı sesinde](https://feedback.azure.com/forums/263030-azure-cosmos-db)yeni bir istek oluşturun.

Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun.

## <a name="try-azure-cosmos-db-subscriptions"></a><a id="try-cosmos-db"></a>Azure Cosmos DB aboneliklerini deneyin

Artık abonelik olmadan, ücretsiz ve taahhütler olmadan zaman sınırlı Azure Cosmos DB deneyiminin keyfini çıkarabilirsiniz. Azure Cosmos DB aboneliğini deneyin için [Azure Cosmos DB'yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) ve herhangi bir kişisel Microsoft hesabını (MSA) kullanın. Bu abonelik Azure [Ücretsiz Deneme](https://azure.microsoft.com/free/)sürümünden ayrıdır ve Azure Ücretsiz Deneme sürümü veya Azure ücretli aboneliğiyle birlikte kullanılabilir.

Azure Cosmos DB aboneliklerini deneyin, kullanıcı kimliğinizle ilişkili diğer aboneliklerin yanındaki Azure portalında görünür.

Azure Cosmos DB aboneliklerini deneyin için aşağıdaki koşullar geçerlidir:

* Kişisel Microsoft hesaplarına (MSA) hesap erişimi verilebilir. Etkin Dizin (AAD) hesaplarını veya kurumsal AAD Kiracılarına ait hesapları kullanmaktan kaçının, erişim izni vermeyi engelleyebilecek sınırlamalar olabilir.
* SQL, Gremlin API ve Tablo hesapları için abonelik başına bir [iş mesuliyonu sağlanan kapsayıcı.](./set-throughput.md#set-throughput-on-a-container)
* MongoDB hesapları için abonelik başına en fazla üç [iş teslimi tahsilatı.](./set-throughput.md#set-throughput-on-a-container)
* Abonelik başına bir [iş veri tabanı.](./set-throughput.md#set-throughput-on-a-database) Through-throughprovisioned veritabanları içinde kapsayıcılar herhangi bir sayı içerebilir.
* 10 GB depolama kapasitesi.
* Genel çoğaltma aşağıdaki Azure [bölgelerinde](https://azure.microsoft.com/regions/)kullanılabilir : Orta ABD, Kuzey Avrupa ve Güneydoğu Asya
* Konteyner seviyesinde sağlandığında maksimum 5 K RU/s'lik verim.
* Veritabanı düzeyinde sağlandığında maksimum 20 K RU/s'lik verim.
* Aboneliklerin süresi 30 gün sonra sona erer ve toplam 31 güne kadar uzatılabilir.
* Azure Cosmos DB hesaplarını deneyin için Azure destek biletleri oluşturulamaz; ancak, mevcut destek planlarına sahip aboneler için destek sağlanır.

## <a name="set-up-azure-cosmos-db"></a>Azure Cosmos DB'yi ayarlama

### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Azure Cosmos DB'ye nasıl kaydolurum?

Azure Cosmos DB, Azure portalında kullanılabilir. İlk olarak, Azure aboneliğine kaydolun. Kaydolduktan sonra Azure aboneliğinize bir Azure Cosmos DB hesabı ekleyebilirsiniz.

### <a name="what-is-a-master-key"></a>Ana anahtar nedir?

Ana anahtar, bir hesaptaki tüm kaynaklara erişmeyi sağlayan bir güvenlik belirtecidir. Anahtara sahip kişiler veritabanı hesabındaki tüm kaynaklara erişim okudu ve yazdı. Ana anahtarları dağıtırken dikkatli olun. Birincil ana anahtar ve ikincil ana anahtar, [Azure portalının][azure-portal] **Keys** bıçak üzerinde mevcuttur. Anahtarlar hakkında daha fazla bilgi için bkz. [Erişim tuşlarını görüntüleme, kopyalama ve yeniden oluşturma](manage-with-cli.md#list-account-keys).

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>PreferredLocations'ın ayarlanabileceği bölgeler nelerdir?

PreferredLocations değeri, Cosmos DB'nin kullanılabildiği Azure bölgelerinden herhangi biri için ayarlanabilir. Kullanılabilir bölgelerin listesi için [Azure bölgelerine](https://azure.microsoft.com/regions/)bakın.

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>Azure veri merkezleri aracılığıyla tüm dünyaya veri dağıtırken dikkat etmem gereken bir şey var mı?

Azure Cosmos DB, [Azure bölgeleri](https://azure.microsoft.com/regions/) sayfasında belirtildiği gibi tüm Azure bölgelerinde bulunur. Temel hizmet olduğundan, her yeni veri merkezinin bir Azure Cosmos DB varlığı vardır.

Bir bölge ayarladığınızda, Azure Cosmos DB'nin egemen ve devlet bulutlarına saygı gösterdiğini unutmayın. Diğer bir deyişle, egemen bir [bölgede](https://azure.microsoft.com/global-infrastructure/)bir hesap oluşturursanız, o [egemen bölgeden](https://azure.microsoft.com/global-infrastructure/)çoğalamazsınız. Benzer şekilde, dış hesaptan diğer egemen konumlara çoğaltmayı etkinleştiremezsiniz.

### <a name="is-it-possible-to-switch-from-container-level-throughput-provisioning-to-database-level-throughput-provisioning-or-vice-versa"></a>Konteyner düzeyinde ki iş verme den veritabanı düzeyinde iş verme için geçiş yapmak mümkün mü? Ya da tam tersi

Kapsayıcı ve veritabanı düzeyinde iş yeri sağlama ayrı tekliflerdir ve bunlardan biri arasında geçiş yapmak, verilerin kaynaktan hedefe geçişini gerektirir. Bu da, yeni bir veritabanı veya yeni bir kapsayıcı oluşturmanız ve ardından [toplu yürütme kitaplığı](bulk-executor-overview.md) veya [Azure Veri Fabrikası'nı](../data-factory/connector-azure-cosmos-db.md)kullanarak verileri aktarmanız gerektiği anlamına gelir.

### <a name="does-azure-cosmosdb-support-time-series-analysis"></a>Azure CosmosDB zaman serisi çözümlemesi destekliyor mu?

Evet Azure CosmosDB zaman serisi analizi destekler, burada [zaman serisi desen](https://github.com/Azure/azure-cosmosdb-dotnet/tree/master/samples/Patterns)için bir örnektir. Bu örnek, zaman serisi verileri üzerinde toplu görünümler oluşturmak için değişiklik akışının nasıl kullanılacağını gösterir. Bu yaklaşımı, kıvılcım akışı veya başka bir akış veri işlemcisi kullanarak genişletebilirsiniz.

## <a name="what-are-the-azure-cosmos-db-service-quotas-and-throughput-limits"></a>Azure Cosmos DB hizmet kotaları ve iş bölümü limitleri nelerdir

Daha fazla bilgi için Azure Cosmos DB [hizmet kotalarına](concepts-limits.md) ve kapsayıcı ve veritabanı makaleleri [başına tüm sınırlara](set-throughput.md#comparison-of-models) bakın.

## <a name="sql-api"></a>SQL API’si

### <a name="how-do-i-start-developing-against-the-sql-api"></a>SQL API'ye karşı nasıl geliştirmeye başlarım?

Öncelikle bir Azure aboneliğine kaydolmalısınız. Azure aboneliğine kaydolduktan sonra, Azure aboneliğinize bir SQL API kapsayıcısı ekleyebilirsiniz. Azure Cosmos DB hesabı ekleme yle ilgili talimatlar [için](create-sql-api-dotnet.md#create-account)bkz.

.NET, Python, Node.js, JavaScript ve Java için [SDK'lar](sql-api-sdk-dotnet.md) kullanılabilir. Geliştiriciler, çeşitli platformlardan ve dillerden Gelen Azure Cosmos DB kaynaklarıyla etkileşim kurmak için yeniden kullanılabilir [HTTP API'lerini](/rest/api/cosmos-db/) de kullanabilir.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>Bir başlangıç yapmak için bazı hazır örneklere erişebilir miyim?

SQL API [.NET](sql-api-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](sql-api-nodejs-samples.md)ve [Python](sql-api-python-samples.md) SDK'lar için örnekler GitHub'da mevcuttur.

### <a name="does-the-sql-api-database-support-schema-free-data"></a>SQL API veritabanı şema içermeyen verileri destekliyor mu?

Evet, SQL API uygulamaları şema tanımları veya ipuçları olmadan rasgele JSON belgeleri depolamak için izin verir. Veriler, Azure Cosmos DB SQL sorgu arabirimi üzerinden sorgu için hemen kullanılabilir.

### <a name="does-the-sql-api-support-acid-transactions"></a>SQL API ACID hareketlerini destekliyor mu?

Evet, SQL API, JavaScript tarafından depolanan yordamlar ve tetikleyiciler olarak ifade edilen çapraz belge hareketlerini destekler. Hareketler her kapsayıcı içinde tek bir bölüme kapsamlı dır ve acid semantiği ile "tümü veya hiçbir şey" olarak yürütülür, kod ve kullanıcı isteklerini aynı anda yürüten diğer lerden yalıtılır. JavaScript uygulama kodunun sunucu tarafındaki yürütmesi boyunca özel durumlar atılırsa, tüm işlem geri alınır. 

### <a name="what-is-a-container"></a>Kapsayıcı nedir?

Kapsayıcı, bir belge grubu ve bunların ilişkili JavaScript uygulama mantığıdır. Kapsayıcı, [maliyetin](performance-levels.md) elde edilen ve kullanılan depolama tarafından belirlendiği faturalandırılabilir bir varlıktır. Kapsayıcılar bir veya daha fazla bölümü veya sunucuyu kaplayabilir ve neredeyse sınırsız depolama veya iş hacmi hacmini işlemek için ölçeklenebilir.

* SQL API için bir kapsayıcı, bir Kapsayıcı eşlenebleri.
* MongoDB hesapları için Cosmos DB'nin API'si için, Bir Koleksiyon için bir konteyner eşlemleri.
* Cassandra ve Tablo API hesapları için bir tabloya bir kapsayıcı eşlemleri.
* Gremlin API hesapları için, bir kapsayıcı grafik eşler.

Kapsayıcılar aynı zamanda Azure Cosmos DB'nin fatura landırma varlıklarıdır. Her kapsayıcı, sağlanan iş ve kullanılmış depolama alanına bağlı olarak saatlik olarak faturalandırılır. Daha fazla bilgi için Azure [Cosmos DB Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/cosmos-db/)bakın.

### <a name="how-do-i-create-a-database"></a>Veritabanı nasıl oluşturulur?

[Azure veritabanını](https://portal.azure.com)kullanarak veritabanları oluşturabilirsiniz , bir [kapsayıcı ekle'de](create-sql-api-java.md#add-a-container)açıklandığı gibi , [Azure Cosmos DB SDK'larından](sql-api-sdk-dotnet.md)biri veya [REST API'leri.](/rest/api/cosmos-db/)

### <a name="how-do-i-set-up-users-and-permissions"></a>Kullanıcıları ve izinleri nasıl ayarlarım?

[Cosmos DB API SDK'lardan](sql-api-sdk-dotnet.md) veya [REST API'lerinden](/rest/api/cosmos-db/)birini kullanarak kullanıcıları ve izinleri oluşturabilirsiniz.

### <a name="does-the-sql-api-support-sql"></a>SQL API SQL'i destekliyor mu?

SQL API hesapları tarafından desteklenen SQL sorgu dili, SQL Server tarafından desteklenen sorgu işlevinin gelişmiş bir alt kümesidir. Azure Cosmos DB SQL sorgu dili, JavaScript tabanlı, kullanıcı tanımlı işlevler (UDF' ler) aracılığıyla zengin hiyerarşik ve ilişkisel işleçler ve genişletilebilirlik sağlar. JSON dilbilgisi, JSON belgelerinin hem Azure Cosmos DB otomatik dizin oluşturma teknikleri hem de Azure Cosmos DB'nin SQL sorgu lehçesi tarafından kullanılan etiketli düğümlere sahip ağaçlar olarak modellenmesine olanak tanır. SQL dilbilgisi kullanma hakkında daha fazla bilgi için [SQL Query][query] makalesine bakın.

### <a name="does-the-sql-api-support-sql-aggregation-functions"></a>SQL API SQL toplama işlevlerini destekliyor mu?

SQL API, herhangi bir ölçekte düşük gecikmeli toplamayı `COUNT` `MIN`toplu `MAX` `AVG`işlevler `SUM` , , , ve SQL dilbilgisi yoluyla destekler. Daha fazla bilgi için [Bkz. Toplu işlevler.](sql-query-aggregates.md)

### <a name="how-does-the-sql-api-provide-concurrency"></a>SQL API eşzamanlılık nasıl sağlar?

SQL API, HTTP entity etiketleri veya ETags aracılığıyla iyimser eşzamanlılık denetimini (OCC) destekler. Her SQL API kaynağının bir ETag'ı vardır ve bir belge her güncelleştirinde ETag sunucuda ayarlanır. ETag üstbilgi ve geçerli değer tüm yanıt iletilerine dahildir. ETags, sunucunun kaynağın güncelleştirilip güncelleştirilmeyeceğine karar vermesine izin vermek için If-Match üstbilgisiyle kullanılabilir. If-Match değeri, denetlenecek ETag değeridir. ETag değeri sunucu ETag değeriyle eşleşirse, kaynak güncelleştirilir. ETag artık geçerli değilse, sunucu işlemi "HTTP 412 Ön Koşul hatası" yanıt koduyla reddeder. İstemci daha sonra kaynak için geçerli ETag değerini elde etmek için kaynağı yeniden getirir. Ayrıca, ETags, bir kaynağın yeniden alınması gerekip gerekmediğini belirlemek için If-None-Match üstbilgisiyle kullanılabilir.

.NET'te iyimser eşzamanlılık kullanmak için [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) sınıfını kullanın. Bir .NET örneği için GitHub'daki DocumentManagement örneğindeki [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) bakın.

### <a name="how-do-i-perform-transactions-in-the-sql-api"></a>SQL API'de işlemleri nasıl gerçekleştirebilirim?

SQL API, JavaScript tarafından depolanan yordamlar ve tetikleyiciler aracılığıyla dil tümleşik işlemleri destekler. Komut dosyaları içindeki tüm veritabanı işlemleri anlık görüntü yalıtımı altında yürütülür. Tek bölmeli bir kapsayıcıysa, yürütme kapsayıcıya doğru genişler. Kapsayıcı bölümlenmişse, yürütme kapsayıcı içinde aynı bölüm anahtarı değerine sahip belgelere kapsamlıdır. Belge sürümlerinin anlık görüntüsü (ETag'ler) ise işlem başlangıcında alınır ve yalnızca betik başarılı olursa uygulanır. JavaScript bir hata oluşturursa işlem geri alınır. Daha fazla bilgi için [Azure Cosmos DB için Sunucu tarafındaki JavaScript programlamaya](stored-procedures-triggers-udfs.md)bakın.

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Cosmos DB'ye belgeleri toplu olarak nasıl ekleyebilirim?

Belgeleri Azure Cosmos DB'ye toplu olarak aşağıdaki yollardan biriyle ekleyebilirsiniz:

* Toplu uygulayıcı [.NET kitaplığı ve toplu uygulayıcı](bulk-executor-dot-net.md) [Java kitaplığı kullanma'da](bulk-executor-java.md) açıklandığı gibi toplu yürütme aracı
* [Azure Cosmos DB için Veritabanı geçiş aracında](import-data.md)açıklandığı gibi veri geçiş aracı.
* [Azure Cosmos DB için Sunucu tarafındaki JavaScript programlamada](stored-procedures-triggers-udfs.md)açıklandığı gibi depolanan yordamlar.

### <a name="does-the-sql-api-support-resource-link-caching"></a>SQL API kaynak bağlantı önbelleğe destek mi?

Evet, Azure Cosmos DB verimli bir hizmet olduğundan, kaynak bağlantıları değişmez ve önbelleğe alınabilir. SQL API istemcileri, kaynak benzeri herhangi bir belgeye veya kapsayıcıya karşı okumalar için bir "If-None-Match" üstbilgisini belirtebilir ve sunucu sürümü değiştikten sonra yerel kopyalarını güncelleyebilir.

### <a name="is-a-local-instance-of-sql-api-available"></a>SQL API'nin yerel bir örneği kullanılabilir mi?

Evet. [Azure Cosmos DB Emulator,](local-emulator.md) Cosmos DB hizmetinin yüksek doğrulukta öykünmesini sağlar. JSON belgelerinin oluşturulması ve sorgulanması, koleksiyonların sağlanması ve ölçekletilmesi ve depolanan yordamların ve tetikleyicilerin yürütülmesi de dahil olmak üzere Azure Cosmos DB ile aynı işlevselliği destekler. Azure Cosmos DB Emulator'u kullanarak uygulamaları geliştirebilir ve sınayabilir ve Azure Cosmos DB'nin bağlantı bitiş noktasında tek bir yapılandırma değişikliği yaparak bunları genel ölçekte Azure'a dağıtabilirsiniz.

### <a name="why-are-long-floating-point-values-in-a-document-rounded-when-viewed-from-data-explorer-in-the-portal"></a>Portaldaki veri gezgininden görüntülendiğinde bir belgede uzun kayan nokta değerleri neden yuvarlanır?

Bu JavaScript sınırlamasıdır. JavaScript, IEEE 754'te belirtildiği gibi çift duyarlıklı kayan nokta lı sayılar kullanır ve sayıları yalnızca -(2<sup>53</sup> - 1) ve 2<sup>53</sup>-1 (yani 9007199254740991) arasında güvenle tutabilir.

### <a name="where-are-permissions-allowed-in-the-object-hierarchy"></a>Nesne hiyerarşisinde izinlere nerede izin veriliyor?

Kaynak Tokens kullanarak izinleri oluşturma kapsayıcı düzeyinde ve onun torunları (belgeler, ekler gibi) izin verilir. Bu, veritabanında veya hesap düzeyinde izin oluşturmaya çalışmanın şu anda izin verilmediği anlamına gelir.

## <a name="azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API'si

### <a name="what-is-the-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB'nin MongoDB için API'si nedir?

Azure Cosmos DB'nin MongoDB için API'si, uygulamaların MongoDB için mevcut, topluluk destekli SDK'ları ve sürücüleri kullanarak yerel Azure Cosmos veritabanı altyapısıyla kolayca ve şeffaf bir şekilde iletişim kurmasını sağlayan bir kablo protokolü uyumluluk katmanıdır. Geliştiriciler artık Azure Cosmos DB'den yararlanan uygulamalar oluşturmak için mevcut MongoDB araç zincirlerini ve becerilerini kullanabilir. Geliştiriciler, çok büyük çoğaltma, otomatik dizine ekleme, yedekleme bakımı, mali destekli hizmet düzeyi anlaşmaları (SLA' lar) vb. ile küresel dağıtımı içeren Azure Cosmos DB'nin benzersiz özelliklerinden yararlanır.

### <a name="how-do-i-connect-to-my-database"></a>Veritabanıma nasıl bağlanıyorum?

Azure Cosmos DB'nin MongoDB api'si ile Cosmos veritabanına bağlanmanın en hızlı yolu [Azure portalına](https://portal.azure.com)geçmektir. Hesabınıza gidin ve ardından soldaki gezinme menüsünde **Hızlı Başlat'ı**tıklatın. Quickstart, veritabanınıza bağlanmak için kod parçacıkları elde etmenin en iyi yoludur.

Azure Cosmos DB sıkı güvenlik gereksinimleri ve standartları uygular. Azure Cosmos DB hesapları TLS üzerinden kimlik doğrulama ve güvenli iletişim gerektirir, bu nedenle TLSv1.2 kullandığınızdan emin olun.

Daha fazla bilgi için Azure [Cosmos DB'nin MongoDB için API'si ile Cosmos veritabanınıza bağlanın.](connect-mongodb-account.md)

### <a name="are-there-additional-error-codes-that-i-need-to-deal-with-while-using-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB'nin MongoDB apisini kullanırken uğraşmam gereken ek hata kodları var mı?

Ortak MongoDB hata kodlarının yanı sıra, Azure Cosmos DB'nin MongoDB için API'sinin kendine özgü hata kodları vardır:

| Hata               | Kod  | Açıklama  | Çözüm  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Tüketilen toplam istek birimi sayısı, konteyner için sağlanan istek birimi oranından fazladır ve azaltılmıştır. | Azure portalından bir kapsayıcıya veya kapsayıcı kümesine atanan iş kısmını ölçeklemeyi veya yeniden denemeyi düşünün. |
| ExceededMemoryLimit | 16501 | Çok kiracılı bir hizmet olarak, işlem istemcinin bellek alanı üzerinden gitti. | Daha kısıtlayıcı sorgu ölçütleri veya [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)başvuru desteği yle işlemin kapsamını küçültün. <br><br><em> &nbsp; &nbsp;Örnek: &nbsp; &nbsp;db.getCollection('kullanıcılar').aggregate([<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$match: {isim: "Andy"}}, <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$sort: {yaş: -1}}<br>&nbsp;&nbsp;&nbsp;&nbsp;])</em>) |

### <a name="is-the-simba-driver-for-mongodb-supported-for-use-with-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB'nin Simba sürücüsü Azure Cosmos DB'nin MongoDB için API'si ile kullanılmak üzere desteklendi mi?

Evet, MongoDB için Azure Cosmos DB'nin API'si ile Simba'nın Mongo ODBC sürücüsünü kullanabilirsiniz

## <a name="table-api"></a><a id="table"></a>Tablo API’si

### <a name="how-can-i-use-the-table-api-offering"></a>Tablo API teklifini nasıl kullanabilirim?

Azure Cosmos DB Tablo API'si [Azure portalında][azure-portal]kullanılabilir. Öncelikle bir Azure aboneliğine kaydolmalısınız. Kaydolduktan sonra, Azure aboneliğinize bir Azure Cosmos DB Table API hesabı ekleyebilir ve ardından hesabınıza tablolar ekleyebilirsiniz.

Desteklenen dilleri ve ilişkili hızlı başlangıçları [Azure Cosmos DB Tablo API'sine Giriş'te](table-introduction.md)bulabilirsiniz.

### <a name="do-i-need-a-new-sdk-to-use-the-table-api"></a>Tablo API'sini kullanmak için yeni bir SDK'ya ihtiyacım var mı?

Hayır, varolan depolama SDK'ları çalışmaya devam etmelidir. Ancak, bir her zaman en iyi destek ve birçok durumda üstün performans için en son SDK'lar alır önerilir. [Azure Cosmos DB Tablo API'sine Giriş'te](table-introduction.md)kullanılabilir dillerin listesine bakın.

### <a name="where-is-table-api-not-identical-with-azure-table-storage-behavior"></a>Tablo API'si ile Azure Tablo depolama alanı arasındaki farklar nelerdir?

Azure Cosmos DB Tablo API ile tablo oluşturmak isteyen Azure Table depolama sından gelen kullanıcıların dikkat etmesi gereken bazı davranış farklılıkları vardır:

* Azure Cosmos DB Tablo API garantili performans sağlamak için ayrılmış bir kapasite modeli kullanır, ancak bu, kapasite kullanılmasa bile, tablo oluşturulur oluşturulmaz kapasitenin karşılığını ödediği anlamına gelir. Azure Tablo depolama ile yalnızca kullanılan kapasitenin ödemesi kullanılır. Bu, Azure Tablo depolama alanı 10 saniyelik bir SLA sunarken, Tablo API'nin neden 10 ms okuma sunabileceğini ve 15 ms'in 99 yüzdelik oranda SLA yazabileceğini açıklamaya yardımcı olur. Ancak sonuç olarak, Tablo API tabloları ile, hatta herhangi bir istek olmadan boş tablolar, kapasite azure cosmos DB tarafından sunulan SLA herhangi bir istek işlemek için kullanılabilir olduğundan emin olmak için maliyet para.
* Tablo API'si tarafından döndürülen sorgu sonuçları, Azure Tablo depolama alanında olduğu gibi bölüm tuşu/satır anahtar sırasına göre sıralanmaz.
* Satır tuşları sadece 255 bayt kadar olabilir
* Toplu işlerden sadece 2 MB'ye kadar çıkabilir
* CORS şu anda desteklenmiyor
* Azure Tablo depolamasındaki tablo adları büyük/küçük harf duyarlı değildir, ancak Azure Cosmos DB Tablo API'sinde
* Azure Cosmos DB'nin ikili alanlar gibi bilgileri kodlamak için iç biçimlerinden bazıları şu anda istediğiniz kadar verimli değil. Bu nedenle, veri boyutunda beklenmeyen sınırlamalar neden olabilir. Örneğin, kodlama verilerin boyutunu artırdığından, şu anda ikili verileri depolamak için tablo varlığının tam meg'ini kullanamaz.
* Entity özellik adı 'Id' şu anda desteklenmiyor
* TableQuery TakeCount 1000 ile sınırlı değildir

REST API açısından Azure Cosmos DB Tablo API tarafından desteklenmeyen bir dizi uç nokta/sorgu seçeneği vardır:

| Dinlenme Yöntemi(ler) | Dinlenme Bitiş Noktası/Sorgu Seçeneği | Doküman URL'leri | Açıklama |
| ------------| ------------- | ---------- | ----------- |
| AL, KOY | /?restype=service@comp=özellikler| [Tablo Servis Özelliklerini Ayarla](https://docs.microsoft.com/rest/api/storageservices/set-table-service-properties) ve [Tablo Servis Özelliklerini Al](https://docs.microsoft.com/rest/api/storageservices/get-table-service-properties) | Bu bitiş noktası, CORS kurallarını, depolama analizi yapılandırmasını ve günlüğe kaydetme ayarlarını ayarlamak için kullanılır. CORS şu anda desteklenmiyor ve Azure Cosmos DB'de Azure Depolama Tabloları'ndan farklı şekilde işlenir ve analitik ve günlüğe kaydetme |
| Seçenekler | /\<tablo-kaynak-adı> | [Uçuş öncesi CORS masa talebi](https://docs.microsoft.com/rest/api/storageservices/preflight-table-request) | Bu, Azure Cosmos DB'nin şu anda desteklemediği CORS'un bir parçasıdır. |
| GET | /?restype=service@comp=istatistikler | [Masa Servisi İstatistiklerini Alın](https://docs.microsoft.com/rest/api/storageservices/get-table-service-stats) | Verilerin birincil ve ikinci basamaklar arasında ne kadar hızlı çoğaltıldığını bilgi sağlar. Çoğaltma yazmanın bir parçası olduğu için Cosmos DB'de buna gerek yoktur. |
| AL, KOY | /mytable?comp=acl | [Tablo ACL](https://docs.microsoft.com/rest/api/storageservices/get-table-acl) ve [Set Tablo ACL](https://docs.microsoft.com/rest/api/storageservices/set-table-acl) alın | Bu, Paylaşılan Erişim İmzalarını (SAS) yönetmek için kullanılan depolanan erişim ilkelerini alır ve ayarlar. SAS desteklenmiş olsa da, bunlar farklı olarak ayarlanır ve yönetilir. |

Buna ek olarak Azure Cosmos DB Table API, ATOM'u değil, yalnızca JSON biçimini destekler.

Azure Cosmos DB Paylaşılan Erişim İmzalarını (SAS) desteklerken, özellikle yeni tablolar oluşturma hakkı gibi yönetim işlemleriyle ilgili bazı ilkeler vardır.

Özellikle .NET SDK için Azure Cosmos DB'nin şu anda desteklemediği bazı sınıflar ve yöntemler vardır.

| Sınıf | Desteklenmeyen Yöntem |
|-------|-------- |
| CloudTableİsteistemci | \*Hizmet Özellikleri* |
|                  | \*ServiceStats* |
| CloudTable | SetPermissions* |
|            | İzin alma* |
| TabloHizmet Bağlamı | * (Bu sınıf amortismana uğradı) |
| TabloHizmet Varlığı | " " |
| TabloServiceExtensions | " " |
| TabloServiceQuery | " " |

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>SDK veya hatalar hakkında nasıl geri bildirimde bulunurum?

Geri bildiriminizi aşağıdaki yollardan herhangi birinde paylaşabilirsiniz:

* [Kullanıcı sesi](https://feedback.azure.com/forums/263030-azure-cosmos-db)
* [MSDN forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurecosmosdb)
* [Yığın Taşma](https://stackoverflow.com/questions/tagged/azure-cosmosdb). Stack Overflow programlama soruları için en iyisidir. Sorunuzun konu [üzerinde](https://stackoverflow.com/help/on-topic) olduğundan emin olun ve [soruyu açık ve yanıtlanabilir hale getirerek mümkün olduğunca çok ayrıntı sağlayın.](https://stackoverflow.com/help/how-to-ask)

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api"></a>Tablo API'sine bağlanmak için kullanmam gereken bağlantı dizesi nedir?

Bağlantı dizesi:

```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountName>.table.cosmosdb.azure.com
```

Bağlantı dizesini Azure portalındaki Bağlantı String sayfasından alabilirsiniz.

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-net-sdk-for-the-table-api"></a>Tablo API'si için .NET SDK'daki istek seçenekleriiçin config ayarlarını nasıl geçersiz kılarım?

Bazı ayarlar CreateCloudTableClient yönteminde ve diğer ayarlar istemci uygulamasındaki appSettings bölümündeki app.config üzerinden işlenir. Config ayarları hakkında daha fazla bilgi için [Azure Cosmos DB özelliklerine](tutorial-develop-table-dotnet.md)bakın.

### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-azure-table-storage-sdks"></a>Varolan Azure Tablo depolama SDK'larını kullanan müşterileriçin herhangi bir değişiklik var mı?

Yok. Varolan Azure Tablo depolama SDK'larını kullanan varolan veya yeni müşteriler için değişiklik yoktur.

### <a name="how-do-i-view-table-data-thats-stored-in-azure-cosmos-db-for-use-with-the-table-api"></a>Tablo API'sinde kullanılmak üzere Azure Cosmos DB'de depolanan tablo verilerini nasıl görüntüleyebilirim?

Verilere göz atmak için Azure portalını kullanabilirsiniz. Tablo API kodunu veya bir sonraki yanıtta belirtilen araçları da kullanabilirsiniz.

### <a name="which-tools-work-with-the-table-api"></a>Tablo API'si ile hangi araçlar çalışır?

Azure Depolama [Gezgini'ni](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)kullanabilirsiniz.

Daha önce belirtilen biçimde bir bağlantı dizesini alma esnekliğine sahip araçlar yeni Tablo API'sini destekleyebilir. [Azure Depolama İstemci Araçları](../storage/common/storage-explorers.md) sayfasında tablo araçlarının listesi sağlanır.

### <a name="is-the-concurrency-on-operations-controlled"></a>Operasyonlarda eşzamanlılık kontrol ediliyor mu?

Evet, eTag mekanizmasının kullanımı yla iyimser eşzamanlılık sağlanır.

### <a name="is-the-odata-query-model-supported-for-entities"></a>OData sorgu modeli varlıklar için destekleniyor mu?

Evet, Tablo API'si OData sorgusunu ve LINQ sorgusunu destekler.

### <a name="can-i-connect-to-azure-table-storage-and-azure-cosmos-db-table-api-side-by-side-in-the-same-application"></a>Aynı uygulamada Azure Tablo Depolama ve Azure Cosmos DB Tablo API'sine yan yana bağlanabilir miyim?

Evet, bağlantı dizesi aracılığıyla her biri kendi URI'sini işaret eden CloudTableClient'ın iki ayrı örneğini oluşturarak bağlanabilirsiniz.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-offering"></a>Varolan bir Azure Tablo depolama uygulamasını bu teklife nasıl geçirebilirim?

[AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) ve [Azure Cosmos DB Veri Geçiş Aracı](import-data.md) desteklenir.

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>Örneğin n *GB* veriyle başlarsam ve verilerim zaman içinde 1 TB'ye çıkacaksa, depolama boyutunun genişletilmesi bu hizmet için nasıl yapılır?

Azure Cosmos DB, yatay ölçekleme kullanarak sınırsız depolama alanı sağlamak üzere tasarlanmıştır. Hizmet, depolama alanınızı izleyebilir ve etkin bir şekilde artırabilir.

### <a name="how-do-i-monitor-the-table-api-offering"></a>Tablo API teklifini nasıl izleyebilirim?

İstekleri ve depolama kullanımını izlemek için Tablo API **Ölçümleri** bölmesini kullanabilirsiniz.

### <a name="how-do-i-calculate-the-throughput-i-require"></a>İstediğim iş buzun değerini nasıl hesaplayabilirim?

İşlemler için gerekli olan TabloİşlemE'yi hesaplamak için kapasite tahmincisini kullanabilirsiniz. Daha fazla bilgi için bkz: [Tahmin İstek Birimleri ve Veri Depolama.](https://www.documentdb.com/capacityplanner) Genel olarak, varlığınızı JSON olarak gösterebilir ve operasyonlarınız için numaraları sağlayabilirsiniz.

### <a name="can-i-use-the-table-api-sdk-locally-with-the-emulator"></a>Tablo API SDK'yı emülatörle yerel olarak kullanabilir miyim?

Şu anda değil.

### <a name="can-my-existing-application-work-with-the-table-api"></a>Mevcut uygulamam Tablo API ile çalışabilir mi?

Evet, aynı API desteklenir.

### <a name="do-i-need-to-migrate-my-existing-azure-table-storage-applications-to-the-sdk-if-i-dont-want-to-use-the-table-api-features"></a>Tablo API özelliklerini kullanmak istemiyorsam, varolan Azure Tablo depolama uygulamalarımı SDK'ya geçirmem gerekiyor mu?

Hayır, varolan Azure Tablo depolama varlıklarını herhangi bir kesinti olmaksızın oluşturabilir ve kullanabilirsiniz. Ancak, Tablo API'sini kullanmıyorsanız, otomatik dizinden, ek tutarlılık seçeneğinden veya genel dağıtımdan yararlanamazsınız.

### <a name="how-do-i-add-replication-of-the-data-in-the-table-api-across-more-than-one-region-of-azure"></a>Azure'un birden fazla bölgesinde Tablo API'sindeki verilerin çoğaltılması nasıl eklenir?

Azure Cosmos DB portalının [genel çoğaltma ayarlarını](tutorial-global-distribution-sql-api.md#portal) kullanarak uygulamanız için uygun bölgeler ekleyebilirsiniz. Genel olarak dağıtılan bir uygulama geliştirmek için, düşük okuma gecikmesi sağlamak için preferredLocation bilgileri kümesiyle uygulamanızı yerel bölgeye eklemeniz gerekir.

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-table-api"></a>Tablo API'deki hesabın birincil yazma bölgesini nasıl değiştirebilirim?

Bir bölge eklemek ve ardından gerekli bölgeye başarısız olmak için Azure Cosmos DB küresel çoğaltma portalı bölmesini kullanabilirsiniz. Yönergeler için bkz: [Çok bölgeli Azure Cosmos DB hesapları yla geliştirme.](high-availability.md)

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Verilerimi dağıtırken tercih ettiğim okuma bölgelerimi düşük gecikme süreyle nasıl yapılandırıyorum?

Yerel konumdan okumaya yardımcı olmak için app.config dosyasındaki PreferredLocation tuşunu kullanın. Varolan uygulamalar için, LocationMode ayarlanmışsa Tablo API'si bir hata atar. Tablo API bu bilgileri app.config dosyasından aldığı için bu kodu kaldırın. 

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api"></a>Tablo API'deki tutarlılık düzeyleri hakkında nasıl düşünmem gerekir?

Azure Cosmos DB, tutarlılık, kullanılabilirlik ve gecikme süresi arasında iyi gerekçeli dengeler sağlar. Azure Cosmos DB, Tablo API geliştiricileri için beş tutarlılık düzeyi sunar, böylece tablo düzeyinde doğru tutarlılık modelini seçebilir ve verileri sorgularken tek tek isteklerde bulunabilirsiniz. İstemci bağlandığında, bir tutarlılık düzeyi belirtebilir. CreateCloudTableClient'ın tutarlılık Düzeyi bağımsız değişkeni aracılığıyla düzeyi değiştirebilirsiniz.

Tablo API'si, varsayılan olarak Bounded-bayatlık tutarlılığı yla "Kendi yazılarınızı okuyun" ile düşük gecikme li okumalar sağlar. Daha fazla bilgi için [Tutarlılık düzeylerine](consistency-levels.md)bakın.

Varsayılan olarak, Azure Tablo depolama alanı bir bölge içinde güçlü tutarlılık ve ikincil konumlarda Nihai tutarlılık sağlar.

### <a name="does-azure-cosmos-db-table-api-offer-more-consistency-levels-than-azure-table-storage"></a>Azure Cosmos DB Tablo API'si Azure Tablo depolama alanına göre daha fazla tutarlılık düzeyi sunuyor mu?

Evet, Azure Cosmos DB'nin dağıtılmış yapısından nasıl yararlanacağınız hakkında bilgi için [Tutarlılık düzeylerine](consistency-levels.md)bakın. Tutarlılık düzeyleri için garantiler sağlandığı için, bunları güvenle kullanabilirsiniz.

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>Küresel dağıtım etkinleştirildiğinde, verilerin çoğaltılması ne kadar sürer?

Azure Cosmos DB, verileri yerel bölgede dayanıklı bir şekilde işler ve verileri milisaniyeler içinde hemen diğer bölgelere iter. Bu çoğaltma yalnızca veri merkezinin gidiş-dönüş süresine (RTT) bağlıdır. Azure Cosmos DB'nin genel dağıtım özelliği hakkında daha fazla bilgi edinmek için Azure [Cosmos DB: Azure'da küresel olarak dağıtılmış bir veritabanı hizmeti](distribute-data-globally.md)bölümüne bakın.

### <a name="can-the-read-request-consistency-level-be-changed"></a>Okuma isteği tutarlılık düzeyi değiştirilebilir mi?

Azure Cosmos DB ile tutarlılık düzeyini kapsayıcı düzeyinde (tabloda) ayarlayabilirsiniz. .NET SDK'yı kullanarak app.config dosyasındaki TableConsistencyLevel tuşu için değer sağlayarak düzeyi değiştirebilirsiniz. Olası değerler şunlardır: Güçlü, Sınırlı Bayatlık, Oturum, Tutarlı Önek ve Eventual. Daha fazla bilgi için Azure [Cosmos DB'deki Tunable veri tutarlılığı düzeylerine](consistency-levels.md)bakın. Anahtar fikir, istek tutarlılık düzeyini tablonun ayarından daha fazla ayarlayamadığınızdır. Örneğin, Eventual'deki tablonun tutarlılık düzeyini ve Güçlü'deki istek tutarlılığı düzeyini ayarlayamadığınızda.

### <a name="how-does-the-table-api-handle-failover-if-a-region-goes-down"></a>Bir bölge çökerse Tablo API'si nasıl başarısız olur?

Tablo API, Azure Cosmos DB'nin genel olarak dağıtılan platformundan yararlanır. Uygulamanızın veri merkezi kapalı kalma süresini tolere edebileceğinden emin olmak için, [çok bölgeli Azure Cosmos DB hesaplarıyla geliştirme](high-availability.md)Azure Cosmos DB portalındaki hesap için en az bir bölge daha etkinleştirin. [Çok bölgeli Azure Cosmos DB hesaplarıyla Geliştirme](high-availability.md)portalını kullanarak bölgenin önceliğini belirleyebilirsiniz.

Hesap için istediğiniz kadar bölge ekleyebilir ve bir hata önceliği sağlayarak hesap üzerinde başarısız olabilir denetim. Veritabanını kullanmak için, orada da bir uygulama sağlamanız gerekir. Bunu yaptığınızda, müşterileriniz kapalı kalma süresi yle karşılaşmazlar. En [son .NET istemciSDK](table-sdk-dotnet.md) otomatik homing ama diğer SDK'lar değildir. Diğer bir arada, kapanan bölgeyi algılayabilir ve otomatik olarak yeni bölgeye geçebiliyor.

### <a name="is-the-table-api-enabled-for-backups"></a>Yedeklemeler için Tablo API'si etkin mi?

Evet, Tablo API yedeklemeler için Azure Cosmos DB platformundan yararlanır. Yedeklemeler otomatik olarak yapılır. Daha fazla bilgi için [Azure Cosmos DB ile Çevrimiçi yedekleme ve geri yükleme](../synapse-analytics/sql-data-warehouse/backup-and-restore.md)ye bakın.

### <a name="does-the-table-api-index-all-attributes-of-an-entity-by-default"></a>Tablo API varsayılan olarak bir varlığın tüm özniteliklerini dizine yapar mı?

Evet, bir varlığın tüm öznitelikleri varsayılan olarak dizine göre dizine alınır. Daha fazla bilgi için Azure [Cosmos DB: Dizin oluşturma ilkeleri'](index-policy.md)ne bakın.

### <a name="does-this-mean-i-dont-have-to-create-more-than-one-index-to-satisfy-the-queries"></a>Bu, sorguları karşılamak için birden fazla dizin oluşturmam gerekmediği anlamına mı geliyor?

Evet, Azure Cosmos DB Tablo API herhangi bir şema tanımı olmadan tüm öznitelikleri otomatik dizin sağlar. Bu otomasyon, geliştiricilerin dizin oluşturma ve yönetimi yerine uygulamaya odaklanmalarını sağlar. Daha fazla bilgi için Azure [Cosmos DB: Dizin oluşturma ilkeleri'](index-policy.md)ne bakın.

### <a name="can-i-change-the-indexing-policy"></a>Dizin oluşturma ilkesini değiştirebilir miyim?

Evet, dizin tanımı sağlayarak dizin oluşturma ilkesini değiştirebilirsiniz. Düzgün kodlamak ve ayarları kaçmak gerekir.

SDK'lar non-.NET için, dizin oluşturma ilkesi yalnızca **Veri Gezgini'ndeki**portalda ayarlanabilir, değiştirmek istediğiniz belirli tabloya gidebilir ve ardından Ayarlar >Dizin Oluşturma İlkesi'& **Ölçekle**gidin, istediğiniz değişikliği yapın ve sonra **Kaydet**.

.NET SDK'dan app.config dosyasında sunulabilir:

```JSON
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        }
      ]
    }
  ],
  "excludedPaths":
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-to-the-table-api"></a>Platform olarak Azure Cosmos DB'nin sıralama, toplama, hiyerarşi ve diğer işlevler gibi birçok özelliği vardır. Bu yetenekleri Tablo API'sine ekleyecek misiniz?

Tablo API, Azure Tablo depolama alanıyla aynı sorgu işlevini sağlar. Azure Cosmos DB ayrıca sıralama, toplamalar, jeo-uzamsal sorgu, hiyerarşi ve çok çeşitli yerleşik işlevleri de destekler. Daha fazla bilgi için [SQL sorgularına](how-to-sql-query.md)bakın.

### <a name="when-should-i-change-tablethroughput-for-the-table-api"></a>Tablo API'si için TabloThroughput'u ne zaman değiştirmeliyim?

Aşağıdaki koşullardan biri geçerli olduğunda TableThroughput'u değiştirmelisiniz:

* Bir veri ekstresi, dönüştürme ve yükleme (ETL) gerçekleştirin veya kısa sürede çok fazla veri yüklemek istiyorsunuz.
* Konteynerden veya arka uçtaki bir dizi kapsayıcıdan daha fazla kaynak elde etmek gerekir. Örneğin, kullanılan iş buzun sağlanan iş tükenen iş lerden daha fazla olduğunu görürsünüz ve daraltılırsınız. Daha fazla bilgi için Azure [Cosmos kapsayıcıları için iş ortası](set-throughput.md)ayarla'ya bakın.

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-table"></a>Tablo API tablomun çıktısını büyütebilir veya küçültebilir miyim?

Evet, iş buzunu ölçeklendirmek için Azure Cosmos DB portalının ölçek bölmesini kullanabilirsiniz. Daha fazla bilgi için [bkz.](set-throughput.md)

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Yeni sağlanan tablolar için varsayılan bir TableThroughput ayarlandı mı?

Evet, tableThroughput'u app.config üzerinden geçersiz kılmaz ve Azure Cosmos DB'de önceden oluşturulmuş bir kapsayıcı kullanmazsanız, hizmet 400'lük bir tablo oluşturur.

### <a name="is-there-any-change-of-pricing-for-existing-customers-of-the-azure-table-storage-service"></a>Azure Tablo depolama hizmetinin mevcut müşterileri için herhangi bir fiyatlandırma değişikliği var mı?

Yok. Mevcut Azure Tablo depolama müşterileri için fiyatta değişiklik yoktur.

### <a name="how-is-the-price-calculated-for-the-table-api"></a>Tablo API'si için fiyat nasıl hesaplanır?

Fiyat, ayrılan TableThroughput'a bağlıdır.

### <a name="how-do-i-handle-any-rate-limiting-on-the-tables-in-table-api-offering"></a>Tablo API teklifindeki tablolarda sınırlayıcı herhangi bir oranı nasıl işleyebilirim?

İstek oranı, temel kapsayıcı veya kapsayıcı kümesi için sağlanan iş girdisinin kapasitesinden fazlaysa, bir hata alırsınız ve SDK yeniden deneme ilkesini uygulayarak aramayı yeniden dener.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-table-api-offering-of-azure-cosmos-db"></a>Azure Cosmos DB'nin Tablo API teklifinden yararlanmak için neden PartitionKey ve RowKey dışında bir iş metodu seçmem gerekiyor?

Azure Cosmos DB, app.config dosyasında veya portal üzerinden bir tane sağlamazsanız, kapsayıcınız için varsayılan bir iş gücü ayarlar.

Azure Cosmos DB, üst sınırlarla birlikte performans ve gecikme sonu için garanti sağlar. Bu garanti, motor kiracının işlemlerinde yönetim icra edebildiği zaman mümkündür. TableThroughput'un ayarlanması, platform bu kapasiteyi rezerve ettiği ve operasyonel başarıyı garanti ettiği için garantili iş sonu ve gecikme gecikmesini elde etmenizi sağlar.

İş verme özelliklerini kullanarak, uygulamanızın mevsimselliğinden yararlanmak, iş girdisi gereksinimlerini karşılamak ve maliyetlerden tasarruf etmek için elastik olarak değiştirebilirsiniz.

### <a name="azure-table-storage-has-been-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-azure-cosmos-db-table-api-offering-seems-to-be-charging-me-even-though-i-havent-performed-a-single-transaction-or-stored-anything-can-you-explain"></a>Azure Tablo depolama alanı benim için ucuz olmuştur, çünkü yalnızca verileri depolamak için ödeme atarım ve nadiren sorgularım. Azure Cosmos DB Table API teklifi, tek bir işlem gerçekleştirmemiş veya hiçbir şey saklamamış olsam bile beni ücretlendirmiş gibi görünüyor. Açıklayabilir misin?

Azure Cosmos DB, kullanılabilirlik, gecikme gecikmesi ve üretim için garantilere sahip, küresel olarak dağıtılan, SLA tabanlı bir sistem olarak tasarlanmıştır. Azure Cosmos DB'de iş için rezerve ettiğinizde, diğer sistemlerin iş lerinden farklı olarak bu garanti altına alınmıştır. Azure Cosmos DB, müşterilerin istediği ikincil dizinler ve genel dağıtım gibi ek özellikler sağlar.

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-azure-table-storage-with-the-table-api-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>Azure Tablo depolama alanına veri yutturduğumda kota dolu" bildirimi (bir bölümün dolu olduğunu belirten) asla alamıyorum. Tablo API ile, bu mesajı alıyorum. Bu teklif beni sınırlandırıyor ve mevcut uygulamamı değiştirmeye zorluyor mu?

Azure Cosmos DB, gecikme, iş ortası, kullanılabilirlik ve tutarlılık garantileriyle sınırsız ölçek sağlayan SLA tabanlı bir sistemdir. Garantili premium performans sağlamak için, veri boyutunuzve dizinizin yönetilebilir ve ölçeklenebilir olduğundan emin olun. Bölüm anahtarı başına varlık veya öğe sayısındaki 10 GB sınırı, harika bir arama ve sorgu performansı sağladığımızı sağlamaktır. Uygulamanızın Azure Depolama için bile iyi ölçeklendirildiğından emin olmak için, tüm bilgileri tek bir bölümde depolayıp sorgulayarak sıcak bir bölüm *oluşturmamanızı* öneririz.

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-table-api"></a>Yani PartitionKey ve RowKey hala Tablo API ile gereklidir?

Evet. Tablo API'sinin yüzey alanı Azure Tablo depolama SDK'sına benzediği için, bölüm anahtarı verileri dağıtmak için etkili bir yol sağlar. Satır tuşu bu bölüm içinde benzersizdir. Satır anahtarının bulunması gerekir ve standart SDK'daki gibi null olamaz. RowKey uzunluğu 255 bayt ve PartitionKey uzunluğu 1 KB olduğunu.

### <a name="what-are-the-error-messages-for-the-table-api"></a>Tablo API'si için hata iletileri nelerdir?

Azure Tablo depolama ve Azure Cosmos DB Tablo API'si aynı SDK'ları kullanır, bu nedenle hataların çoğu aynı olacaktır.

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api"></a>Tablo API'sinde birbiri ardına çok sayıda tablo oluşturmaya çalıştığımda neden daraltılırım?

Azure Cosmos DB, gecikme, iş ortası, kullanılabilirlik ve tutarlılık garantileri sağlayan SLA tabanlı bir sistemdir. Bu, sağlanan bir sistem olduğundan, bu gereksinimleri garanti etmek için kaynak ayırır. Tabloların hızlı oluşturma hızı algılanır ve azaltılır. Tabloların oluşturulmama oranına bakmanızı ve dakikada 5'ten aza indirmenizi öneririz. Tablo API'sinin sağlanan bir sistem olduğunu unutmayın. Bunu temin ettiğiniz anda, parasını ödemeye başlayacaksınız.

## <a name="gremlin-api"></a>Gremlin API

### <a name="for-cnet-development-should-i-use-the-microsoftazuregraphs-package-or-gremlinnet"></a>C#/.NET geliştirme için Microsoft.Azure.Graphs paketini mi kullanmalıyım yoksa Gremlin.NET?

Azure Cosmos DB Gremlin API, hizmetin ana bağlayıcıları olarak açık kaynak sürücülerinden yararlanır. Yani önerilen seçenek [Apache Tinkerpop tarafından desteklenen sürücüleri](https://tinkerpop.apache.org/)kullanmaktır.

### <a name="how-are-rus-charged-when-running-queries-on-a-graph-database"></a>Grafik veritabanında sorgu çalıştırılırken RU/s nasıl ücretlendirilir?

Tüm grafik nesneleri, tepe leri ve kenarları arka uçtaki JSON belgeleri olarak gösterilir. Bir Gremlin sorgusu aynı anda bir veya birden çok grafik nesnesini değiştirebildiği için, onunla ilişkili maliyet doğrudan sorgu tarafından işlenen nesnelerle, kenarlarla ilişkilidir. Bu, Azure Cosmos DB'nin diğer tüm API'ler için kullandığı işlemle aynıdır. Daha fazla bilgi için bkz. [Azure Cosmos DB'de İstek Birimleri](request-units.md).

RU ücreti, sonuç kümesine değil, geçişin çalışma veri kümesine dayanır. Örneğin, bir sorgu sonuç olarak tek bir tepe noktası elde etmeyi hedefliyorsa ancak yolda birden fazla başka nesnenin geçişi gerekiyorsa, maliyet tek bir sonuç tepe noktasını hesaplamak için gereken tüm grafik nesnelerini temel alır.

### <a name="whats-the-maximum-scale-that-a-graph-database-can-have-in-azure-cosmos-db-gremlin-api"></a>Bir grafik veritabanının Azure Cosmos DB Gremlin API'sinde sahip olabileceği maksimum ölçek nedir?

Azure Cosmos DB, depolama ve üretim gereksinimlerindeki artışı otomatik olarak gidermek için [yatay bölümleme](partition-data.md) kullanır. Bir iş yükünün maksimum verim ve depolama kapasitesi, belirli bir kapsayıcıyla ilişkili bölüm sayısına göre belirlenir. Ancak, gremlin API konteyner ölçekte uygun bir performans deneyimi sağlamak için kurallar belirli bir dizi vardır. Bölümleme ve en iyi uygulamalar hakkında daha fazla bilgi için [Azure Cosmos DB makalesinde bölümleme](partition-data.md) bölümüne bakın.

### <a name="how-can-i-protect-against-injection-attacks-using-gremlin-drivers"></a>Gremlin sürücülerini kullanarak enjeksiyon saldırılarına karşı nasıl koruyabilirim?

Çoğu yerli Apache Tinkerpop Gremlin sürücüleri, sorgu yürütme için bir parametre sözlüğü sağlama seçeneğine izin verir. Bu [nasıl Gremlin.Net](https://tinkerpop.apache.org/docs/3.2.7/reference/#gremlin-DotNet) ve [Gremlin-Javascript](https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started/blob/master/app.js)bunu yapmak için bir örnektir.

### <a name="why-am-i-getting-the-gremlin-query-compilation-error-unable-to-find-any-method-error"></a>Neden "Gremlin Sorgu Derleme Hatası: Herhangi bir yöntem bulamıyorum" hatasını alıyorum?

Azure Cosmos DB Gremlin API, Gremlin yüzey alanında tanımlanan işlevselliğin bir alt kümesini uygular. Desteklenen adımlar ve daha fazla bilgi için [Gremlin destek](gremlin-support.md) makalesine bakın.

Tüm temel Gremlin adımları Azure Cosmos DB tarafından desteklendirildiğından, en iyi geçici çözüm, desteklenen işlevsellikle gerekli Gremlin adımlarını yeniden yazmaktır.

### <a name="why-am-i-getting-the-websocketexception-the-server-returned-status-code-200-when-status-code-101-was-expected-error"></a>Neden "WebSocketException: sunucu durum kodu '200' durum kodu '101' bekleniyor" hatası döndü döndü alıyorum?

Bu hata büyük olasılıkla yanlış bitiş noktası kullanılırken atılır. Bu hatayı oluşturan bitiş noktası aşağıdaki desene sahiptir:

`https:// YOUR_DATABASE_ACCOUNT.documents.azure.com:443/`

Bu, grafik veritabanınızın belge bitiş noktasıdır.  Kullanılacak doğru bitiş noktası, aşağıdaki biçime sahip Gremlin Bitiş Noktası'dır:

`https://YOUR_DATABASE_ACCOUNT.gremlin.cosmosdb.azure.com:443/`

### <a name="why-am-i-getting-the-requestrateistoolarge-error"></a>Neden "RequestRateIsTooLarge" hatası alıyorum?

Bu hata, saniyede ayrılan İstek Birimlerinin sorguya hizmet etmek için yeterli olmadığı anlamına gelir. Bu hata genellikle tüm vertices elde eden bir sorgu çalıştırdığınızda görülür:

```
// Query example:
g.V()
```

Bu sorgu, grafikteki tüm vertices almak için çalışacağız. Yani, bu sorgunun maliyeti RUs açısından en az vertices sayısına eşit olacaktır. RU/s ayarı bu sorguyu ele alacak şekilde ayarlanmalıdır.

### <a name="why-do-my-gremlin-driver-connections-get-dropped-eventually"></a>Neden Gremlin sürücü bağlantılarım eninde sonunda bırakılalı?

Bir Gremlin bağlantısı WebSocket bağlantısı üzerinden yapılır. WebSocket bağlantılarının yaşamak için belirli bir zamanı olmamasına rağmen, Azure Cosmos DB Gremlin API 30 dakikalık bir etkinlik ten sonra boşta kalan bağlantıları sonlandırır.

### <a name="why-cant-i-use-fluent-api-calls-in-the-native-gremlin-drivers"></a>Yerel Gremlin sürücülerinde neden akıcı API çağrıları kullanamıyorum?

Akıcı API çağrıları henüz Azure Cosmos DB Gremlin API tarafından desteklenmez. Akıcı API çağrıları, şu anda Azure Cosmos DB Gremlin API tarafından desteklenmeyen bytecode desteği olarak bilinen bir dahili biçimlendirme özelliği gerektirir. Aynı nedenle, en son Gremlin-JavaScript sürücüsü de şu anda desteklenmiyor.

### <a name="how-can-i-evaluate-the-efficiency-of-my-gremlin-queries"></a>Gremlin sorgularımın verimliliğini nasıl değerlendirebilirim?

**executionProfile()** önizleme adımı sorgu yürütme planının bir çözümlemesi sağlamak için kullanılabilir. Bu adım, aşağıdaki örnekte gösterildiği gibi herhangi bir Gremlin sorgusunun sonuna eklenmesi gerekir:

**Sorgu örneği**

```
g.V('mary').out('knows').executionProfile()
```

**Örnek çıktı**

```json
[
  {
    "gremlin": "g.V('mary').out('knows').executionProfile()",
    "totalTime": 8,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 3,
        "annotations": {
          "percentTime": 37.5
        },
        "counts": {
          "resultCount": 1
        }
      },
      {
        "name": "GetEdges",
        "time": 5,
        "annotations": {
          "percentTime": 62.5
        },
        "counts": {
          "resultCount": 0
        },
        "storeOps": [
          {
            "count": 0,
            "size": 0,
            "time": 0.6
          }
        ]
      },
      {
        "name": "GetNeighborVertices",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 0
        }
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 0
        }
      }
    ]
  }
]
```

Yukarıdaki profilin çıktısı, tepe noktası nesnelerini, kenar nesnelerini ve çalışma veri kümesinin boyutunu elde etmek için ne kadar zaman harcandırDığını gösterir. Bu, Azure Cosmos DB sorguları için standart maliyet ölçümleri ile ilgilidir.

## <a name="cassandra-api"></a><a id="cassandra"></a>Cassandra API

### <a name="what-is-the-protocol-version-supported-by-azure-cosmos-db-cassandra-api-is-there-a-plan-to-support-other-protocols"></a>Azure Cosmos DB Cassandra API tarafından desteklenen protokol sürümü nedir? Diğer protokolleri desteklemek için bir plan var mı?

Azure Cosmos DB için Apache Cassandra API bugün CQL sürüm 4'ü destekler. Diğer protokolleri destekleme hakkında geri bildiriminiz varsa, [kullanıcı ses geri](https://feedback.azure.com/forums/263030-azure-cosmos-db) [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com)bildirimi aracılığıyla bize bildirin veya e-posta gönderin.

### <a name="why-is-choosing-a-throughput-for-a-table-a-requirement"></a>Tablo için iş elde etme gereksinimi neden gereklidir?

Azure Cosmos DB, tabloyu oluşturduğunuz yerden - portal veya CQL' ye göre kapsayıcınız için varsayılan iş ortasını ayarlar.
Azure Cosmos DB, üst sınırlarla birlikte performans ve gecikme sonu için garanti sağlar. Bu garanti, motor kiracının işlemlerinde yönetim icra edebildiği zaman mümkündür. Platform bu kapasiteyi rezerve ettiği ve operasyon başarısını garanti ettiği için, iş ortamının ayarlanması garantili iş sonu ve gecikme si elde etmenizi sağlar.
Uygulamanızın mevsimsellikten yararlanmak ve maliyetlerden tasarruf etmek için iş bişini elastik olarak değiştirebilirsiniz.

İş ortası kavramı, [Azure Cosmos DB makalesindeki İstek Birimlerinde](request-units.md) açıklanmıştır. Bir tablonun iş bölümü, temel fiziksel bölümler arasında eşit olarak dağıtılır.

### <a name="what-is-the-default-rus-of-table-when-created-through-cql-what-if-i-need-to-change-it"></a>CQL ile oluşturulduğunda tablonun varsayılan RU/s nedir? Ya değiştirmem gerekirse?

Azure Cosmos DB, iş ortası sağlamak için bir para birimi olarak istek birimlerini (RU/s) saniyede kullanır. CQL ile oluşturulan tablolar 400 RU'ya sahiptir. Ru'yu portaldan değiştirebilirsiniz.

CQL

```
CREATE TABLE keyspaceName.tablename (user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=1200
```

.NET

```csharp
int provisionedThroughput = 400;
var simpleStatement = new SimpleStatement($"CREATE TABLE {keyspaceName}.{tableName} (user_id int PRIMARY KEY, lastname text)");
var outgoingPayload = new Dictionary<string, byte[]>();
outgoingPayload["cosmosdb_provisioned_throughput"] = Encoding.UTF8.GetBytes(provisionedThroughput.ToString());
simpleStatement.SetOutgoingPayload(outgoingPayload);
```

### <a name="what-happens-when-throughput-is-used-up"></a>Elde iş bittiğinde ne olur?

Azure Cosmos DB, üst sınırlarla birlikte performans ve gecikme sonu için garanti sağlar. Bu garanti, motor kiracının işlemlerinde yönetim icra edebildiği zaman mümkündür. Platform bu kapasiteyi rezerve ettiği ve operasyon başarısını garanti ettiği için, garantili iş sonu ve gecikme si elde etmenizi sağlayan iş yerini ayarlamaya dayalı olarak bu mümkündür.
Bu kapasitenin üzerinden geçdiğinizde, kapasitenizin kullanıldığını gösteren aşırı yüklü hata iletisi alırsınız.
0x1001 Aşırı yüklendi: "İstek Oranı büyük" olduğundan istek işlenemez. Bu noktada, hangi işlemlerin ve seslerinin bu soruna neden olduğunu görmek önemlidir. Portaldaki ölçümlerle birlikte, tüketilen kapasite nin kullanılabilir kapasiteyi aşarak ilgili bir fikir edinebilirsiniz. Daha sonra kapasitenin tüm temel bölümlerde neredeyse eşit olarak tüketilmesini sağlamanız gerekir. Elde edilen iş bölümünün çoğunun tek bir bölüm tarafından tüketilince görürseniz, iş yükünün çarpıklığı vardır.

İşlem in saat, gün ve yedi gün içinde, bölümler arasında veya toplu olarak nasıl kullanıldığını gösteren ölçümler kullanılabilir. Daha fazla bilgi için Azure [Cosmos DB'deki ölçümleri izleme ve hata ayıklama](use-metrics.md)bölümüne bakın.

Tanılama günlükleri [Azure Cosmos DB tanılama günlüğü](logging.md) makalesinde açıklanır.

### <a name="does-the-primary-key-map-to-the-partition-key-concept-of-azure-cosmos-db"></a>Azure Cosmos DB'nin bölüm anahtarı kavramının birincil anahtarı eşlenemiyor mu?

Evet, bölüm anahtarı varlığı doğru konuma yerleştirmek için kullanılır. Azure Cosmos DB'de, fiziksel bir bölümüzerinde depolanan doğru mantıksal bölümü bulmak için kullanılır. Bölümleme kavramı, [Azure Cosmos DB makalesinde Bölüm ve ölçekte](partition-data.md) iyi bir şekilde açıklanmıştır. Burada önemli almak mantıksal bir bölüm bugün 10 GB sınırı üzerinden gitmemelidir.

### <a name="what-happens-when-i-get-a-quota-full-notification-indicating-that-a-partition-is-full"></a>Bir bölümün dolu olduğunu belirten bir kota tam" bildirimi alırsam ne olur?

Azure Cosmos DB, gecikme, iş ortası, kullanılabilirlik ve tutarlılık garantileriyle sınırsız ölçek sağlayan SLA tabanlı bir sistemdir. Bu sınırsız depolama anahtar kavram olarak bölümleme kullanarak veri dışında yatay ölçek dayanmaktadır. Bölümleme kavramı, [Azure Cosmos DB makalesinde Bölüm ve ölçekte](partition-data.md) iyi bir şekilde açıklanmıştır.

Uymanız gereken mantıksal bölüm başına varlık veya öğe sayısına ilişkin 10 GB sınırı. Uygulamanızın iyi ölçeklendirildiğından emin olmak için, tüm bilgileri tek bir bölümde depolayıp sorgulayarak sıcak bir bölüm *oluşturmamanızı* öneririz. Bu hata yalnızca verileriniz çarpıksa gelebilir: yani, bir bölüm anahtarı (10&nbsp;GB'dan fazla) için çok fazla veriniz vardır. Depolama portalını kullanarak veri dağıtımını bulabilirsiniz. Bu hatayı düzeltmenin yolu, tabloyu yeniden oluşturmak ve verilerin daha iyi dağıtılmasına olanak tanıyan parçalı birincil (bölüm anahtarı) seçmektir.

### <a name="is-it-possible-to-use-cassandra-api-as-key-value-store-with-millions-or-billions-of-individual-partition-keys"></a>Cassandra API'yi milyonlarca veya milyarlarca ayrı bölüm anahtarıyla anahtar değer deposu olarak kullanmak mümkün mü?

Azure Cosmos DB, depolama alanını ölçekleyerek sınırsız veri depolayabilir. Bu, iş açısından bağımsızdır. Evet, sağ birincil/bölüm anahtarını belirterek anahtar/değerleri depolamak ve almak için cassandra API'yi her zaman kullanabilirsiniz. Bu tek tek anahtarlar kendi mantıksal bölüm olsun ve sorunları olmadan fiziksel bölüm üstüne oturup.

### <a name="is-it-possible-to-create-more-than-one-table-with-apache-cassandra-api-of-azure-cosmos-db"></a>Azure Cosmos DB'nin Apache Cassandra API'si ile birden fazla tablo oluşturmak mümkün mü?

Evet, Apache Cassandra API ile birden fazla tablo oluşturmak mümkündür. Bu tabloların her biri, iş artışı ve depolama birimi olarak kabul edilir.

### <a name="is-it-possible-to-create-more-than-one-table-in-succession"></a>Art arda birden fazla tablo oluşturmak mümkün mü?

Azure Cosmos DB, hem veri hem de denetim düzlemi etkinlikleri için kaynak yönetimli sistemdir. Koleksiyonlar, tablolar gibi kapsayıcılar, verilen iş verme kapasitesi için sağlanan çalışma zamanı varlıklarıdır. Bu kapsayıcıların hızlı bir şekilde oluşturulması beklenen etkinlik ve daraltılmış değildir. Tabloları hemen düşüren/oluşturan testleriniz varsa, bunları boşluğa düşürmeyi deneyin.

### <a name="what-is-maximum-number-of-tables-that-can-be-created"></a>Oluşturulabilecek en fazla tablo sayısı nedir?

Tablo sayısında fiziksel bir sınır yoktur, her [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com) zamanki 10'lardan veya 100'lerden oluşturulması gereken çok sayıda tablonuz varsa (toplam sabit boyutun 10 TB'nin üzerinde veri gittiği) bir e-posta gönderin.

### <a name="what-is-the-maximum--of-keyspace-that-we-can-create"></a>Oluşturabileceğimiz keyspace'in maksimum # değeri nedir?

Meta veri kapsayıcıları oldukları için anahtar alanı sayısında fiziksel bir sınır [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com) yoktur, herhangi bir nedenle çok sayıda anahtar alanınız varsa e-posta gönderin.

### <a name="is-it-possible-to-bring-in-lot-of-data-after-starting-from-normal-table"></a>Normal tablodan başladıktan sonra çok fazla veri getirmek mümkün mü?

Depolama kapasitesi otomatik olarak yönetilir ve siz daha fazla veri ittikçe artar. Böylece düğümleri yönetmeden ve sağlamadan istediğiniz kadar veriyi ve daha fazlasını güvenle içe aktarabilirsiniz.

### <a name="is-it-possible-to-supply-yaml-file-settings-to-configure-apache-casssandra-api-of-azure-cosmos-db-behavior"></a>Azure Cosmos DB davranışının Apache Casssandra API'sini yapılandırmak için yaml dosya ayarlarını sağlama nız mümkün mü?

Azure Cosmos DB'nin Apache Cassandra API'si bir platform hizmetidir. Yürütme işlemleri için protokol düzeyi uyumluluğu sağlar. Yönetim, izleme ve yapılandırmanın karmaşıklığını gizler. Geliştirici/kullanıcı olarak, kullanılabilirlik, mezar taşları, anahtar önbelleği, satır önbelleği, çiçek filtresi ve diğer ayarların çokluğu hakkında endişelenmenize gerek yoktur. Azure Cosmos DB'nin Apache Cassandra API'si, yapılandırma ve yönetim yükü olmadan gereksinim duyduğunuz okuma yazma performansı sağlamaya odaklanır.

### <a name="will-apache-cassandra-api-for-azure-cosmos-db-support-node-additioncluster-statusnode-status-commands"></a>Azure Cosmos DB için Apache Cassandra API düğüm ekleme/küme durumu/düğüm durumu komutlarını destekleyecek mi?

Apache Cassandra API, kapasite planlaması yapan, & depolama için elastikiyet taleplerine cevap veren bir platform hizmetidir. Azure Cosmos DB ile iş ortası sağlamanız gerekir. Daha sonra düğümleri ekleme/silme veya yönetme endişesi duymadan gün boyunca herhangi bir sayıda yukarı ve aşağı ölçeklendirebilirsiniz. Bu düğüm, küme yönetimi aracı nı da kullanmanız gerekmez anlamına gelir.

### <a name="what-happens-with-respect-to-various-config-settings-for-keyspace-creation-like-simplenetwork"></a>Basit/ağ gibi keyspace oluşturma için çeşitli config ayarları ile ilgili olarak ne olur?

Azure Cosmos DB, kullanılabilirlik ve düşük gecikme nedeniyle kutudışında genel dağıtım sağlar. Çoğaltmaları veya başka şeyleri kurmanız gerekmez. Tüm yazılar, performans garantisi sağlarken yazdığınız herhangi bir bölgede her zaman dayanıklı bir çoğunluk vardır.

### <a name="what-happens-with-respect-to-various-settings-for-table-metadata-like-bloom-filter-caching-read-repair-change-gc_grace-compression-memtable_flush_period-and-more"></a>Çiçek filtresi, önbelleğe alma, onarım değişikliği okuma, gc_grace, sıkıştırma memtable_flush_period ve daha fazlası gibi tablo meta verileri için çeşitli ayarlara ilişkin olarak ne olur?

Azure Cosmos DB, yapılandırma ayarlarının hiçbirine dokunmaya ve yanlışlıkla manipüle etmeye gerek kalmadan okuma/yazma ve iş elde etme performansı sağlar.

### <a name="is-time-to-live-ttl-supported-for-cassandra-tables"></a>Cassandra tabloları için zaman-to-live (TTL) desteklenir mi?

Evet, TTL desteklenir.

### <a name="is-it-possible-to-monitor-node-status-replica-status-gc-and-os-parameters-earlier-with-various-tools-what-needs-to-be-monitored-now"></a>Düğüm durumunu, çoğaltma durumunu, gc ve işletim sistemi parametrelerini daha önce çeşitli araçlarla izlemek mümkün mü? Şimdi izlenmesi gereken nedir?

Azure Cosmos DB, üretkenliği artırmanıza ve altyapıyı yönetme ve izleme konusunda endişelenmemenize yardımcı olan bir platform hizmetidir. Yalnızca, daraltılmış olup olmadığınızı bulmak ve bu çıktıyı artırmak veya azaltmak için portal ölçümlerinde kullanılabilen iş bölümüne dikkat etmeniz gerekir.
Monitör [SLA' ları](monitor-accounts.md).
[Ölçümleri](use-metrics.md) Kullanma [Tanılama günlüklerini](logging.md)kullanın.

### <a name="which-client-sdks-can-work-with-apache-cassandra-api-of-azure-cosmos-db"></a>Hangi istemci SDK'lar Azure Cosmos DB'nin Apache Cassandra API'si ile çalışabilir?

Apache Cassandra SDK'nın CQLv3 kullanan istemci sürücüleri istemci programları için kullanılmıştır. Kullandığınız başka sürücüleriniz varsa veya sorunlarla karşı karşıyaysanız, [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com)posta gönderin.

### <a name="is-composite-partition-key-supported"></a>Bileşik bölüm anahtarı desteklendi mi?

Evet, bileşik bölüm anahtarı oluşturmak için normal sözdizimini kullanabilirsiniz.

### <a name="can-i-use-sstableloader-for-data-loading"></a>Veri yüklemesi için sstableloader kullanabilir miyim?

Hayır, sstableloader desteklenmiyor.

### <a name="can-an-on-premises-apache-cassandra-cluster-be-paired-with-azure-cosmos-dbs-cassandra-api"></a>Şirket içi Bir Apache Cassandra kümesi Azure Cosmos DB'nin Cassandra API'si ile eşlenebilir mi?

Şu anda Azure Cosmos DB, genel işlemler olmadan bulut ortamı için optimize edilmiş bir deneyime sahiptir. Eşleştirme ye ihtiyacınız varsa, [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com) senaryonuzun açıklamasıyla birlikte posta gönderin. Şirket içi/farklı bulut Cassandra kümesini Cosomos DB'nin Cassandra API'si ile eşleştirmeye yardımcı olmak için çalışıyoruz.

### <a name="does-cassandra-api-provide-full-backups"></a>Cassandra API tam yedekleme sağlıyor mu?

Azure Cosmos DB, bugün tüm API'lerde dört saatlik aralıklarla alınan iki ücretsiz tam yedekleme sağlar. Bu, bir yedekleme zamanlaması ve diğer şeyler ayarlamanız gerekmez.
Bekletme ve sıklığı değiştirmek istiyorsanız, bir [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com) destek talebine e-posta gönderin veya yükseltin. Yedekleme yeteneği hakkında bilgi [Otomatik çevrimiçi yedekleme ve Azure Cosmos DB](../synapse-analytics/sql-data-warehouse/backup-and-restore.md) makalesi ile geri yükleme sağlanır.

### <a name="how-does-the-cassandra-api-account-handle-failover-if-a-region-goes-down"></a>Bir bölge çökerse Cassandra API hesabı nasıl başarısız olur?

Azure Cosmos DB Cassandra API, azure cosmos DB'nin dünya çapında dağıtılan platformundan ödünç alınr. Uygulamanızın veri merkezi kapalı kalma süresini tolere edebileceğinden emin olmak için, [çok bölgeli Azure Cosmos DB hesaplarıyla geliştirme](high-availability.md)Azure Cosmos DB portalındaki hesap için en az bir bölge daha etkinleştirin. [Çok bölgeli Azure Cosmos DB hesaplarıyla Geliştirme](high-availability.md)portalını kullanarak bölgenin önceliğini belirleyebilirsiniz.

Hesap için istediğiniz kadar bölge ekleyebilir ve bir hata önceliği sağlayarak hesap üzerinde başarısız olabilir denetim. Veritabanını kullanmak için, orada da bir uygulama sağlamanız gerekir. Bunu yaptığınızda, müşterileriniz kapalı kalma süresi yle karşılaşmazlar.

### <a name="does-the-apache-cassandra-api-index-all-attributes-of-an-entity-by-default"></a>Apache Cassandra API varsayılan olarak bir varlığın tüm özniteliklerini dizine yapar mı?

Cassandra API, belirli öznitelikler üzerinde seçici dizin oluşturmaya yardımcı olmak için İkincil dizini desteklemeyi planlıyor. 


### <a name="can-i-use-the-new-cassandra-api-sdk-locally-with-the-emulator"></a>Yeni Cassandra API SDK'yı emülatörle birlikte yerel olarak kullanabilir miyim?

Evet bu desteklenir.

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-change-feed-and-other-functionality-will-these-capabilities-be-added-to-the-cassandra-api"></a>Azure Cosmos DB bir platform olarak, değişiklik akışı ve diğer işlevler gibi birçok özelliğe sahip gibi görünüyor. Bu yetenekler Cassandra API'ye eklenecek mi?

Apache Cassandra API, Apache Cassandra ile aynı CQL işlevselliğini sağlar. Gelecekte çeşitli yetenekleri desteklemenin fizibilitesini araştırıyoruz.

### <a name="feature-x-of-regular-cassandra-api-isnt-working-as-today-where-can-the-feedback-be-provided"></a>Düzenli Cassandra API Özelliği x bugün gibi çalışmıyor, nerede geribildirim sağlanabilir?

[Kullanıcı ses geri bildirimi](https://feedback.azure.com/forums/263030-azure-cosmos-db)ile geri bildirim sağlayın.

[azure-portal]: https://portal.azure.com
[query]: sql-api-sql-query.md
