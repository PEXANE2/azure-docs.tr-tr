---
title: Azure Cosmos DB hizmet kotaları
description: Farklı kaynak türlerindeki hizmet kotalarını ve varsayılan limitleri Azure Cosmos DB.
author: abhijitpai
ms.author: abpai
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/03/2020
ms.openlocfilehash: 296f212e26d066613c54f7d69802654e5f7b5090
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85391851"
---
# <a name="azure-cosmos-db-service-quotas"></a>Azure Cosmos DB hizmet kotaları

Bu makale, Azure Cosmos DB’deki farklı kaynaklara sunulan varsayılan kotalara ilişkin bir genel bakış sunar.

## <a name="storage-and-throughput"></a>Depolama ve aktarım hızı

Aboneliğiniz kapsamında bir Azure Cosmos hesabı oluşturduktan sonra [veritabanları, kapsayıcılar ve öğeler oluşturarak](databases-containers-items.md)hesabınızdaki verileri yönetebilirsiniz. [İstek birimi (ru/s veya Rus)](request-units.md)bakımından bir kapsayıcı düzeyinde veya veritabanı düzeyinde üretilen iş sağlayabilirsiniz. Aşağıdaki tabloda kapsayıcı/veritabanı başına depolama ve işleme sınırları listelenmektedir.

| Kaynak | Varsayılan limit |
| --- | --- |
| Kapsayıcı başına en fazla ru ([adanmış aktarım hızı sağlanmış mod](databases-containers-items.md#azure-cosmos-containers)) | Varsayılan olarak 1.000.000. [Azure destek bileti](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) kaydederek bunu artırabilirsiniz |
| Veritabanı başına en fazla ru ([paylaşılan verimlilik sağlanmış mod](databases-containers-items.md#azure-cosmos-containers)) | Varsayılan olarak 1.000.000. [Azure destek bileti](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) kaydederek bunu artırabilirsiniz |
| Maksimum ru/(mantıksal) bölüm anahtarı | 10,000 |
| Tüm öğelerin tamamında en fazla depolama alanı (mantıksal) bölüm anahtarı| 20 GB |
| En fazla farklı (mantıksal) bölüm anahtarı sayısı | Sınırsız |
| Kapsayıcı başına en fazla depolama alanı | Sınırsız |
| Veritabanı başına en fazla depolama alanı | Sınırsız |
| Hesap başına en fazla ek boyutu (ek özelliği amorti edilir) | 2 GB |
| 1 GB başına gereken minimum ru | 10 RU/sn |

> [!NOTE]
> Depolama veya işleme için daha fazla sınır gerektiren bölüm anahtarlarına sahip iş yüklerini yönetmeye yönelik en iyi yöntemler hakkında bilgi edinmek için bkz. [yapay bir bölüm anahtarı oluşturma](synthetic-partition-keys.md).
>

Cosmos kapsayıcısının (veya paylaşılan üretilen iş veritabanı) en az 400 RU/sn aktarım hızına sahip olması gerekir. Kapsayıcı büyüdükçe, desteklenen en düşük aktarım hızı da aşağıdaki etkenlere bağlıdır:

* Kapsayıcıda şimdiye kadar sağlanan en yüksek aktarım hızı. Örneğin, verimlilik 50.000 RU/sn 'ye yükseldiğinde, olası en düşük sağlanan aktarım hızı 500 RU/s olacaktır
* Kapsayıcıda GB cinsinden geçerli depolama alanı. Örneğin, kapsayıcınızda 100 GB depolama alanı varsa, en düşük sağlanan verimlilik 1000 RU/s olacaktır
* Paylaşılan bir üretilen iş veritabanında en düşük aktarım hızı, her zaman bir paylaşılan üretilen iş veritabanında oluşturduğunuz toplam kapsayıcı sayısına, kapsayıcı başına 100 RU/sn ile ölçülür. Örneğin, paylaşılan bir üretilen iş veritabanı içinde beş kapsayıcı oluşturduysanız, verimlilik en az 500 RU/sn olmalıdır

Kapsayıcının veya bir veritabanının geçerli ve en düşük aktarım hızı Azure portal veya SDK 'lardan alınabilir. Daha fazla bilgi için bkz. [kapsayıcılar ve veritabanları üzerinde üretilen Iş sağlama](set-throughput.md). 

> [!NOTE]
> Bazı durumlarda, aktarım hızını %10 ' dan küçük bir süre düşürübiliyor olabilirsiniz. Kapsayıcı başına en az ru 'yı tam olarak almak için API 'YI kullanın.
>

Özet bölümünde, sağlanan en düşük RU sınırları aşağıda verilmiştir. 

| Kaynak | Varsayılan limit |
| --- | --- |
| Kapsayıcı başına en az ru ([özel üretilen iş işleme modu](databases-containers-items.md#azure-cosmos-containers)) | 400 |
| Veritabanı başına en az ru ([paylaşılan verimlilik sağlanmış mod](databases-containers-items.md#azure-cosmos-containers)) | 400 |
| Paylaşılan bir üretilen iş veritabanı içinde kapsayıcı başına en az ru | 100 |

Cosmos DB, SDK veya Portal aracılığıyla kapsayıcı veya veritabanı başına esnek işleme (ru) boyutunu destekler. Her kapsayıcı, en düşük ve en yüksek değerler arasında zaman uyumlu olarak ve 10 ila 100 kez bir ölçek aralığı içinde ölçeklendirebilir. İstenen üretilen iş değeri aralığın dışındaysa, ölçekleme zaman uyumsuz olarak gerçekleştirilir. Zaman uyumsuz ölçeklendirmenin, kapsayıcıda istenen işleme ve veri depolama boyutuna bağlı olarak tamamlanması dakika sürebilir.  

## <a name="control-plane-operations"></a>Denetim düzlemi işlemleri

Azure portal, Azure PowerShell, Azure CLı ve Azure Resource Manager şablonlarını kullanarak [Azure Cosmos hesabınızı temin edebilir ve yönetebilirsiniz](how-to-manage-database-account.md) . Aşağıdaki tabloda abonelik, hesap ve işlem sayısına göre sınırlar listelenmektedir.

| Kaynak | Varsayılan limit |
| --- | --- |
| Abonelik başına en fazla veritabanı hesabı | Varsayılan olarak 50. [Azure destek bileti](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) kaydederek bunu artırabilirsiniz|
| En fazla bölgesel yük devretme sayısı | Varsayılan olarak 1/saat. [Azure destek bileti](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) kaydederek bunu artırabilirsiniz|

> [!NOTE]
> Bölgesel yük devretme işlemleri yalnızca tek bölgeye yazma hesapları için geçerlidir. Çok bölgeli yazma hesapları, yazma bölgesinin değiştirilmesini gerektirmez veya hiçbir sınıra sahip olmaz.

Cosmos DB, düzenli aralıklarla verilerinizin yedeklerini otomatik olarak alır. Yedekleme bekletme aralıkları ve Windows hakkında daha fazla bilgi için, bkz. [Azure Cosmos DB çevrimiçi yedekleme ve isteğe bağlı veri yükleme](online-backup-and-restore.md).

## <a name="per-account-limits"></a>Hesap başına sınırlar

| Kaynak | Varsayılan limit |
| --- | --- |
| En fazla veritabanı sayısı | Sınırsız |
| Paylaşılan işleme ile veritabanı başına en fazla kapsayıcı sayısı |25 |
| Adanmış aktarım hızı olan veritabanı veya hesap başına en fazla kapsayıcı sayısı  |sınırsız |
| En fazla bölge sayısı | Sınır yok (tüm Azure bölgeleri) |

## <a name="per-container-limits"></a>Kapsayıcı başına sınırlar

Kullandığınız API 'ye bağlı olarak bir Azure Cosmos kapsayıcısı bir koleksiyon, tablo veya grafik temsil edebilir. Kapsayıcılar [benzersiz anahtar kısıtlamaları](unique-keys.md), [saklı yordamlar, Tetikleyiciler ve UDF](stored-procedures-triggers-udfs.md)'ler ve [Dizin oluşturma ilkesi](how-to-manage-indexing-policy.md)için konfigürasyonları destekler. Aşağıdaki tabloda bir kapsayıcı içindeki yapılandırmalara özgü sınırlar listelenmektedir. 

| Kaynak | Varsayılan limit |
| --- | --- |
| En fazla veritabanı veya kapsayıcı adı uzunluğu | 255 |
| Kapsayıcı başına en fazla saklı yordam | 100<sup>*</sup>|
| Kapsayıcı başına en fazla UDF | 250<sup>*</sup>|
| Dizin oluşturma ilkesindeki en fazla yol sayısı| 100<sup>*</sup>|
| Kapsayıcı başına en fazla benzersiz anahtar sayısı|(<sup>*</sup>|
| Benzersiz anahtar kısıtlaması başına en fazla yol sayısı|k<sup>*</sup>|

<sup>*</sup>Azure desteği ile iletişime geçerek bu kapsayıcı başına limitlerin herhangi birini artırabilirsiniz.

## <a name="per-item-limits"></a>Öğe başına sınırlar

Kullandığınız API 'ye bağlı olarak, bir Azure Cosmos öğesi koleksiyondaki bir belgeyi, tablodaki bir satırı veya bir grafikteki bir düğümü ya da kenarı temsil edebilir. Aşağıdaki tabloda, Cosmos DB her öğe için sınır gösterilmektedir. 

| Kaynak | Varsayılan limit |
| --- | --- |
| En büyük öğe boyutu | 2 MB (UTF-8 JSON gösteriminin uzunluğu) |
| Bölüm anahtarı değerinin uzunluk üst sınırı | 2048 bayt |
| Maksimum KIMLIK değeri uzunluğu | 1023 bayt |
| Öğe başına en fazla özellik sayısı | Pratik sınır yok |
| En fazla iç içe geçme derinliği | Pratik sınır yok |
| Özellik adının uzunluk üst sınırı | Pratik sınır yok |
| En fazla özellik değeri uzunluğu | Pratik sınır yok |
| En fazla dize özelliği değeri uzunluğu | Pratik sınır yok |
| Maksimum sayısal Özellik değeri uzunluğu | IEEE754 çift duyarlıklı 64 bit |

Bölüm anahtarı ve KIMLIK değerleri için uzunluk kısıtlamaları ve 2 MB Toplam boyut kısıtlaması dışında, öğe yükleri üzerinde özellik sayısı ve iç içe geçme derinliği gibi kısıtlamalar yoktur. RU tüketimini azaltmak için büyük veya karmaşık öğe yapılarına sahip kapsayıcılar için dizin oluşturma İlkesi yapılandırmanız gerekebilir. Cosmos DB, gerçek dünyada bir örnek için bkz. [modelleme öğeleri](how-to-model-partition-example.md) ve büyük öğeleri yönetmek için desenler.

## <a name="per-request-limits"></a>İstek başına sınırlar

Azure Cosmos DB kapsayıcılar, öğeler ve veritabanları gibi kaynaklara karşı [CRUD ve sorgu işlemlerini](/rest/api/cosmos-db/) destekler. Aynı zamanda, bir kapsayıcıda aynı bölüm anahtarına sahip birden çok öğeye karşı [işlem toplu iş isteklerini](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.transactionalbatch) destekler.

| Kaynak | Varsayılan limit |
| --- | --- |
| Tek bir işlem için en fazla yürütme süresi (saklı yordam yürütme veya tek bir sorgu sayfası alma gibi)| 5 sn |
| En büyük istek boyutu (örneğin, saklı yordam, CRUD)| 2 MB |
| En büyük yanıt boyutu (örneğin, sayfalandırılmış sorgu) | 4 MB |
| İşlem toplu işlemindeki en fazla işlem sayısı | 100 |

Sorgu gibi bir işlem, yürütme zaman aşımı veya Yanıt boyut sınırına ulaştığında, yürütmeyi sürdürmek için istemciye bir sonuç ve devamlılık belirteci döndürür. Tek bir sorgunun sayfalar/devamlılıklar arasında çalışması için pratik bir sınır yoktur.

Cosmos DB yetkilendirme için HMAC kullanır. Kapsayıcılar, bölüm anahtarları veya öğeler gibi kaynaklara yönelik ayrıntılı erişim denetimi için bir ana anahtar ya da [kaynak belirteçleri](secure-access-to-data.md) kullanabilirsiniz. Aşağıdaki tabloda Cosmos DB yetkilendirme belirteçleri için sınırlar listelenmektedir.

| Kaynak | Varsayılan limit |
| --- | --- |
| En büyük ana belirteç süre sonu zamanı | 15 dk  |
| En düşük kaynak belirteci süre sonu zamanı | 10 dakika  |
| En fazla kaynak belirteci süre sonu zamanı | Varsayılan olarak 24 saat. [Azure destek bileti](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) kaydederek bunu artırabilirsiniz|
| Belirteç yetkilendirme için maksimum saat eğriltme| 15 dk |

Cosmos DB, yazma işlemleri sırasında tetikleyicilerin yürütülmesini destekler. Hizmet, her yazma işlemi için en fazla bir ön tetikleyici ve bir tetikleme tetiklemeyi destekler. 

## <a name="limits-for-autoscale-provisioned-throughput"></a>Otomatik ölçeklendirme sağlanan verimlilik için sınırlar

Otomatik ölçeklendirme ile işleme ve depolama limitlerinin daha ayrıntılı açıklaması için [Otomatik ölçeklendirme](provision-throughput-autoscale.md#autoscale-limits) makalesine ve [SSS](autoscale-faq.md#lowering-the-max-rus) bölümüne bakın.

| Kaynak | Varsayılan limit |
| --- | --- |
| Sistemin ölçekleyebilir en fazla RU/sn |  `Tmax`, otomatik ölçek Kullanıcı tarafından ayarlanan en büyük RU/s|
| Sistemin ölçekleyebilir en az RU/sn | `0.1 * Tmax`|
| Geçerli RU/s, sistem şu şekilde ölçeklendirilir  |  `0.1*Tmax <= T <= Tmax`, kullanıma göre|
| Saat başına minimum faturalanabilir RU/sn| `0.1 * Tmax` <br></br>Faturalandırma, sistem saatte bir veya daha yüksek olan en yüksek RU/sn için faturalandırdığınız saat başına yapılır `0.1*Tmax` . |
| Bir kapsayıcı için en düşük otomatik ölçeklendirme en büyük RU/sn  |  `MAX(4000, highest max RU/s ever provisioned / 10, current storage in GB * 100)`En yakın 1000 RU/sn 'ye yuvarlanır |
| Bir veritabanı için en düşük otomatik ölçeklendirme en büyük RU/sn  |  `MAX(4000, highest max RU/s ever provisioned / 10, current storage in GB * 100,  4000 + (MAX(Container count - 25, 0) * 1000))`, en yakın 1000 RU/sn 'ye yuvarlanır. <br></br>Göz önünde, veritabanınızda 25 ' ten fazla kapsayıcı varsa, sistem ek kapsayıcı başına en az RU/sn 1000 olan en düşük otomatik ölçeklendirme sayısını artırır. Örneğin, 30 kapsayıcılarınız varsa, ayarlayabileceğiniz en düşük otomatik ölçeklendirme RU/sn, 9000 RU/sn 'dir (900-9000 RU/s arasında ölçeklenirken).

## <a name="sql-query-limits"></a>SQL sorgu sınırları

Cosmos DB, [SQL](how-to-sql-query.md)kullanılarak öğelerin sorgulanmasını destekler. Aşağıdaki tabloda sorgu deyimlerinde, örnek tümce sayısı veya sorgu uzunluğu gibi kısıtlamalar açıklanmaktadır.

| Kaynak | Varsayılan limit |
| --- | --- |
| Maksimum SQL sorgusu uzunluğu| 256 KB |
| Sorgu başına en fazla birleşim| e<sup>*</sup>|
| Sorgu başına en fazla UDF| (<sup>*</sup>|
| Çokgen başına en fazla punto| 4096 |
| Kapsayıcı başına en fazla dahil edilen yol| 500 |
| Kapsayıcı başına çıkarılan en fazla yol sayısı| 500 |
| Bileşik dizindeki maksimum Özellikler| 8 |

<sup>*</sup>Azure desteği ile iletişime geçerek bu SQL sorgu sınırlarını artırabilirsiniz.

## <a name="mongodb-api-specific-limits"></a>MongoDB API 'sine özgü sınırlar

Cosmos DB, MongoDB 'ye karşı yazılan uygulamalar için MongoDB tel protokolünü destekler. Desteklenen komut ve protokol sürümlerini [desteklenen MongoDB özellikleri ve söz diziminde](mongodb-feature-support.md)bulabilirsiniz.

Aşağıdaki tabloda MongoDB Özellik desteğine özgü sınırlar listelenmektedir. SQL (çekirdek) API 'SI için bahsedilen diğer hizmet limitleri de MongoDB API 'SI için de geçerlidir.

| Kaynak | Varsayılan limit |
| --- | --- |
| Maksimum MongoDB sorgu belleği boyutu (Bu sınırlama yalnızca 3,2 sunucu sürümü içindir) | 40 MB |
| MongoDB işlemleri için maksimum yürütme süresi| 30 saniye |
| Sunucu tarafı bağlantısı kapanışı için boşta bağlantı zaman aşımı * | 30 dakika |

\*[Azure Yük Dengeleyici için varsayılan zaman aşımı 4 dakika olduğundan,](../load-balancer/load-balancer-tcp-idle-timeout.md#tcp-idle-timeout)istemci uygulamalarının sürücü ayarlarındaki boş bağlantı zaman aşımını 2-3 dakikaya ayarlaması önerilir.  Bu zaman aşımı, boşta bağlantıların, istemci makinesi ve Azure Cosmos DB arasında bir ara yük dengeleyici tarafından kapanmamasını sağlayacaktır.

## <a name="try-cosmos-db-free-limits"></a>Cosmos DB ücretsiz sınırları deneyin

Aşağıdaki tabloda, [Try Azure Cosmos DB ücretsiz deneme için](https://azure.microsoft.com/try/cosmosdb/) sınırlar listelenmektedir.

| Kaynak | Varsayılan limit |
| --- | --- |
| Deneme süresi | 30 gün (süresi dolduktan sonra yeni bir deneme istenebilir) <br> Süre dolduktan sonra, depolanan bilgiler silinir. |
| Abonelik başına en fazla kapsayıcı (SQL, Gremlin, Tablo API'si) | 1 |
| Abonelik başına en fazla kapsayıcı (MongoDB API) | 3 |
| Kapsayıcı başına en fazla aktarım hızı | 5000 |
| Paylaşılan işleme veritabanı başına en fazla aktarım hızı | 20000 |
| Hesap başına en fazla toplam depolama alanı | 10 GB |

Cosmos DB, genel dağıtımı yalnızca Orta ABD, Kuzey Avrupa ve Güneydoğu Asya bölgelerinde destekler. Deneme Azure Cosmos DB hesapları için Azure destek biletleri oluşturulamıyor. Ancak, mevcut destek planlarına sahip aboneler için destek sağlanır.

## <a name="free-tier-account-limits"></a>Ücretsiz katmanı hesap sınırları
Aşağıdaki tabloda [Azure Cosmos DB ücretsiz katman hesapları](optimize-dev-test.md#azure-cosmos-db-free-tier) için sınırlar listelenmektedir.

| Kaynak | Varsayılan limit |
| --- | --- |
| Azure aboneliği başına ücretsiz katman hesabı sayısı | 1 |
| Serbest katmanlı indirimin süresi | Hesabın kullanım süresi. Hesap oluşturma sırasında kabul etmelidir. |
| Ücretsiz olarak en fazla RU/sn | 400 RU/sn |
| En fazla depolama alanı boş | 5 GB |
| En fazla paylaşılan üretilen iş veritabanı sayısı | 5 |
| Paylaşılan bir üretilen iş veritabanında en fazla kapsayıcı sayısı | 25 <br>Ücretsiz katman hesaplarında, 25 ' e kadar kapsayıcı içeren paylaşılan bir üretilen iş veritabanı için en az RU/sn 400 RU/sn 'dir. |

  Yukarıdaki ' a ek olarak, [Hesap başına sınırlar](#per-account-limits) ayrıca ücretsiz katman hesapları için de geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB temel kavramları [genel dağıtım](distribute-data-globally.md) ve [bölümlendirme](partitioning-overview.md) ve [sağlanan aktarım hızı](request-units.md)hakkında daha fazla bilgi edinin.

Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

* [Azure Cosmos DB SQL API’yi kullanmaya başlama](create-sql-api-dotnet.md)
* [MongoDB için Azure Cosmos DB API 'SI ile çalışmaya başlama](create-mongodb-nodejs.md)
* [Azure Cosmos DB Cassandra API’yi kullanmaya başlama](create-cassandra-dotnet.md)
* [Azure Cosmos DB Graph API’yi kullanmaya başlama](create-graph-dotnet.md)
* [Azure Cosmos DB Tablo API’yi kullanmaya başlama](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)
