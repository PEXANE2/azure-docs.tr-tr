---
title: Azure Cosmos DB hizmet kotaları
description: Azure Cosmos DB hizmet kotaları ve farklı kaynak türlerinde varsayılan sınırlar.
author: abhijitpai
ms.author: abpai
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/03/2020
ms.openlocfilehash: b24d7db679bb9cb9dacd5e1db8e6410b883548cc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415703"
---
# <a name="azure-cosmos-db-service-quotas"></a>Azure Cosmos DB hizmet kotaları

Bu makale, Azure Cosmos DB’deki farklı kaynaklara sunulan varsayılan kotalara ilişkin bir genel bakış sunar.

## <a name="storage-and-throughput"></a>Depolama ve iş artışı

Aboneliğiniz altında bir Azure Cosmos hesabı oluşturduktan sonra, [veritabanları, kapsayıcılar ve öğeler oluşturarak](databases-containers-items.md)hesabınızdaki verileri yönetebilirsiniz. İstek [birimleri (RU/s veya RUs)](request-units.md)açısından kapsayıcı düzeyinde veya veritabanı düzeyinde iş lenebilirsiniz. Aşağıdaki tabloda, kapsayıcı/veritabanı başına depolama ve iş başı için sınırlar listelenebönemlidir.

| Kaynak | Varsayılan limit |
| --- | --- |
| Konteyner başına maksimum RUs[(özel iş verimi sağlama modu)](databases-containers-items.md#azure-cosmos-containers) | Varsayılan olarak 1,000,000. [Azure destek bileti doldurarak](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) artırabilirsiniz |
| Veritabanı başına maksimum RUs[(paylaşılan iş girdisi sağlama modu)](databases-containers-items.md#azure-cosmos-containers) | Varsayılan olarak 1,000,000. [Azure destek bileti doldurarak](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) artırabilirsiniz |
| (mantıksal) bölüm anahtarı başına maksimum RUs | 10,000 |
| (mantıksal) bölüm anahtarı başına tüm öğeler arasında maksimum depolama| 20 GB |
| Maksimum farklı (mantıksal) bölüm anahtarları sayısı | Sınırsız |
| Kapsayıcı başına maksimum depolama | Sınırsız |
| Veritabanı başına maksimum depolama | Sınırsız |
| Hesap başına maksimum ek boyutu (Ek özelliği amortismana tabi tutuluyor) | 2 GB |
| 1 GB başına minimum RUs gereklidir | 10 RU/s |

> [!NOTE]
> Depolama veya iş fazlası için daha yüksek sınırlar gerektiren bölüm anahtarlarına sahip iş yüklerini yönetmek için en iyi uygulamalar hakkında bilgi edinmek için [bkz.](synthetic-partition-keys.md)
>

Bir Cosmos kapsayıcı (veya paylaşılan iş verme veritabanı) en az 400 RUs'luk bir iş verime sahip olmalıdır. Kapsayıcı büyüdükçe, desteklenen minimum iş artışı da aşağıdaki etkenlere bağlıdır:

* Bir kapsayıcıüzerinde ayarlayabildiğiniz minimum iş verimi, kapsayıcıda şimdiye kadar sağlanan maksimum verime bağlıdır. Örneğin, iş beşiniz 10000 RUs'a yükseltildiyse, mümkün olan en düşük sağlanan iş
* Paylaşılan bir iş veri tabanındaki minimum iş başı, paylaşılan bir iş veri tabanında oluşturduğunuz toplam kapsayıcı sayısına da bağlıdır. Örneğin, paylaşılan bir iş veri tabanında beş kapsayıcı oluşturduysanız, iş bilgili en az 500 RUs olmalıdır

Bir kapsayıcının veya veritabanının geçerli ve minimum iş bölümü Azure portalından veya SDK'lardan alınabilir. Daha fazla bilgi için, [kapsayıcılar ve veritabanları üzerinde Tedarik iş bilgilerine](set-throughput.md)bakın. 

> [!NOTE]
> Bazı durumlarda, iş fazlasını %10'dan daha azına düşürebilirsiniz. Kapsayıcı başına tam minimum RUs almak için API'yi kullanın.
>

Özetle, burada minimum verilen RU sınırları vardır. 

| Kaynak | Varsayılan limit |
| --- | --- |
| Konteyner başına minimum RUs[(özel iş lenme modu)](databases-containers-items.md#azure-cosmos-containers) | 400 |
| Veritabanı başına minimum RUs[(paylaşılan iş girdisi sağlama modu)](databases-containers-items.md#azure-cosmos-containers) | 400 |
| Paylaşılan bir iş oluşturma veritabanıiçinde kapsayıcı başına minimum RUs | 100 |

Cosmos DB, SDK'lar veya portal aracılığıyla konteyner veya veritabanı başına elastik iş verisini (RUs) destekler. Her kapsayıcı, en az ve maksimum değerler arasında 10 ila 100 kez arasında eşzamanlı ve hemen ölçeklenebilir. İstenen iş gücü değeri aralığın dışındaysa, ölçekleme eşzamanlı olarak gerçekleştirilir. Asynchronous ölçekleme, kapsayıcıda istenen iş kaynağı ve veri depolama boyutuna bağlı olarak tamamlanması dakika ile saatler sürebilir.  

## <a name="control-plane-operations"></a>Düzlem operasyonlarını kontrol edin

Azure portalı, Azure PowerShell, Azure CLI ve Azure Kaynak Yöneticisi şablonlarını kullanarak [Azure Cosmos hesabınızı sağlayabilir](how-to-manage-database-account.md) ve yönetebilirsiniz. Aşağıdaki tabloda abonelik, hesap ve işlem sayısı başına sınırlar listelenir.

| Kaynak | Varsayılan limit |
| --- | --- |
| Abonelik başına maksimum veritabanı hesapları | Varsayılan olarak 50. [Azure destek bileti doldurarak](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) artırabilirsiniz|
| Maksimum bölgesel arıza sayısı | Varsayılan olarak 1/saat. [Azure destek bileti doldurarak](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) artırabilirsiniz|

> [!NOTE]
> Bölgesel arızalar yalnızca tek bölge için geçerli dir. Hesaplar yazar. Çok bölgeli yazma hesapları, yazma bölgesini değiştirme konusunda herhangi bir sınırlama gerektirmez veya yoktur.

Cosmos DB düzenli aralıklarla verilerinizin yedeklerini otomatik olarak alır. Yedekleme bekletme aralıkları ve pencerelerhakkında ayrıntılı bilgi için [Azure Cosmos DB'de Çevrimiçi yedekleme ve isteğe bağlı veri geri yüklemesi 'ne](online-backup-and-restore.md)bakın.

## <a name="per-account-limits"></a>Hesap başına limitler

| Kaynak | Varsayılan limit |
| --- | --- |
| Maksimum veritabanları sayısı | Sınırsız |
| Paylaşılan iş bölümüyle veritabanı başına maksimum kapsayıcı sayısı |25 |
| Özel iş verimi ile veritabanı veya hesap başına maksimum kapsayıcı sayısı  |sınırsız |
| Maksimum bölge sayısı | Sınır yok (Tüm Azure bölgeleri) |

## <a name="per-container-limits"></a>Konteyner başına limitler

Hangi API'yi kullandığınıza bağlı olarak, bir Azure Cosmos kapsayıcısı bir koleksiyonu, tabloyu veya grafiği temsil edebilir. [Kapsayıcılar, benzersiz anahtar kısıtlamaları,](unique-keys.md) [depolanan yordamlar, tetikleyiciler ve UDF'ler ve](stored-procedures-triggers-udfs.md) [dizin oluşturma ilkesi](how-to-manage-indexing-policy.md)yapılandırmalarını destekler. Aşağıdaki tabloda, kapsayıcıiçindeki yapılandırmalara özgü sınırlar listelendir. 

| Kaynak | Varsayılan limit |
| --- | --- |
| Veritabanı veya kapsayıcı adının maksimum uzunluğu | 255 |
| Kapsayıcı başına maksimum depolanan yordamlar | 100<sup>*</sup>|
| Kapsayıcı başına maksimum UDF | 25<sup>*</sup>|
| Dizin oluşturma ilkesinde en fazla yol sayısı| 100<sup>*</sup>|
| Kapsayıcı başına maksimum benzersiz anahtar sayısı|10<sup>*</sup>|
| Benzersiz anahtar kısıtlaması başına maksimum yol sayısı|16<sup>*</sup>|

<sup>*</sup>Azure Desteği'ne başvurarak bu kapsayıcı başına sınırlardan herhangi birini artırabilirsiniz.

## <a name="per-item-limits"></a>Madde başına limitler

Hangi API'yi kullandığınıza bağlı olarak, bir Azure Cosmos öğesi koleksiyondaki bir belgeyi, tablodaki bir satırı veya grafikteki düğümü veya kenarı temsil edebilir. Aşağıdaki tablo, Cosmos DB'de öğe başına sınırları gösterir. 

| Kaynak | Varsayılan limit |
| --- | --- |
| Bir öğenin maksimum boyutu | 2 MB (UTF-8 JSON gösterimuzunluğu) |
| Bölüm anahtar değerinin maksimum uzunluğu | 2048 bayt |
| Kimlik değerinin maksimum uzunluğu | 1023 bayt |
| Madde başına maksimum özellik sayısı | Pratik sınır yok |
| Maksimum iç içe geçme derinliği | Pratik sınır yok |
| Özellik adının maksimum uzunluğu | Pratik sınır yok |
| Özellik değerinin maksimum uzunluğu | Pratik sınır yok |
| Dize özellik değerinin maksimum uzunluğu | Pratik sınır yok |
| Sayısal özellik değerinin maksimum uzunluğu | IEEE754 çift duyarlıklı 64-bit |

Bölüm anahtarı ve kimlik değerlerindeki uzunluk kısıtlamaları ve 2 MB'lık genel boyut kısıtlaması dışında madde yüklerinde özellik sayısı ve iç içe geçme derinliği gibi herhangi bir kısıtlama yoktur. RU tüketimini azaltmak için büyük veya karmaşık madde yapıları olan kapsayıcılar için dizin oluşturma ilkesini yapılandırmanız gerekebilir. Gerçek bir örnek için [Cosmos DB'deki öğeleri modelleme](how-to-model-partition-example.md) ye ve büyük öğeleri yönetme desenlerine bakın.

## <a name="per-request-limits"></a>İstek başına limitler

Azure Cosmos DB, kapsayıcılar, öğeler ve veritabanları gibi kaynaklara karşı [CRUD ve sorgu işlemlerini](https://docs.microsoft.com/rest/api/cosmos-db/) destekler. Ayrıca, bir kapsayıcıda aynı bölüm tuşu ile birden çok öğeye karşı [işlem toplu isteklerini](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.transactionalbatch) destekler.

| Kaynak | Varsayılan limit |
| --- | --- |
| Tek bir işlem için maksimum yürütme süresi (depolanan yordam yürütme veya tek bir sorgu sayfası alma gibi)| 5 sn |
| Maksimum istek boyutu (örneğin, depolanan yordam, CRUD)| 2 MB |
| Maksimum yanıt boyutu (örneğin, paginated sorgu) | 4 MB |
| İşlem toplu işlemlerinde maksimum işlem sayısı | 100 |

Sorgu gibi bir işlem yürütme zaman anına veya yanıt boyutu sınırına ulaştığında, yürütmeyi sürdürmek için istemciye bir sonuç sayfası ve bir devam belirteci döndürür. Tek bir sorgunun sayfalar/devamlar arasında çalıştırılabildiği süre için pratik bir sınır yoktur.

Cosmos DB yetkilendirme için HMAC kullanır. Kapsayıcılar, bölme anahtarları veya öğeler gibi kaynaklara ince taneli erişim denetimi için bir ana anahtar veya [kaynak belirteçleri](secure-access-to-data.md) kullanabilirsiniz. Aşağıdaki tabloda Cosmos DB'deki yetkilendirme belirteçleri için sınırlar listeleneme.

| Kaynak | Varsayılan limit |
| --- | --- |
| Maksimum ana belirteç son kullanma süresi | 15 dk  |
| Minimum kaynak belirteci son kullanma süresi | 10 dk  |
| Maksimum kaynak belirteci son kullanma süresi | Varsayılan olarak 24 saat. [Azure destek bileti doldurarak](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) artırabilirsiniz|
| Belirteç yetkilendirmesi için maksimum saat eğriltme| 15 dk |

Cosmos DB yazma sırasında tetikleyicilerin yürütülmesini destekler. Hizmet, yazma işlemi başına en fazla bir ön tetikleyici ve bir post-trigger'ı destekler. 

## <a name="autopilot-mode-limits"></a>Otomatik pilot modu sınırları

Otomatik [pilot](provision-throughput-autopilot.md#autopilot-limits) modunda, iş ve depolama sınırları için Otomatik Pilot makalesine bakın.

## <a name="sql-query-limits"></a>SQL sorgu sınırları

Cosmos [DB, SQL](how-to-sql-query.md)kullanarak öğeleri sorgulamayı destekler. Aşağıdaki tabloda, örneğin yan tümce sayısı veya sorgu uzunluğu açısından sorgu ifadelerindeki kısıtlamalar açıklanmaktadır.

| Kaynak | Varsayılan limit |
| --- | --- |
| SQL sorgusunun maksimum uzunluğu| 256 KB |
| Sorgu başına maksimum JO| 5<sup>*</sup>|
| Sorgu başına maksimum UDF'ler| 10<sup>*</sup>|
| Çokgen başına maksimum puan| 4096 |
| Kapsayıcı başına en fazla dahil edilmiş yollar| 500 |
| Kapsayıcı başına en fazla dışlanmış yollar| 500 |
| Bileşik dizindeki maksimum özellikler| 8 |

<sup>*</sup>Azure Destek'e başvurarak bu SQL sorgu sınırlarını artırabilirsiniz.

## <a name="mongodb-api-specific-limits"></a>MongoDB API'ye özel sınırlar

Cosmos DB, MongoDB'ye karşı yazılmış uygulamalar için MongoDB tel protokolünü destekler. Desteklenen komutları ve protokol sürümlerini [Desteklenen MongoDB özelliklerinde ve sözdiziminde](mongodb-feature-support.md)bulabilirsiniz.

Aşağıdaki tabloda MongoDB özellik desteğine özgü sınırlar listeleneb.) SQL (çekirdek) API için belirtilen diğer hizmet sınırları da MongoDB API için geçerlidir.

| Kaynak | Varsayılan limit |
| --- | --- |
| Maksimum MongoDB sorgu bellek boyutu (Bu sınırlama sadece 3.2 sunucu sürümü içindir) | 40 MB |
| MongoDB operasyonları için maksimum yürütme süresi| 30s |
| Sunucu yan bağlantı kapatma için boşta bağlantı zaman amı* | 30 dakika |

\*[Azure LoadBalancer için varsayılan zaman sonu 4 dakika olduğundan,](../load-balancer/load-balancer-tcp-idle-timeout.md#tcp-idle-timeout)istemci uygulamalarının sürücü ayarlarındaki boşta bağlantı zaman dilimini 2-3 dakika olarak ayarlamasını öneririz.  Bu zaman aşımı, boşta kalan bağlantıların istemci makine ile Azure Cosmos DB arasında bir ara yük dengeleyicisi tarafından kapatılmamasını sağlar.

## <a name="try-cosmos-db-free-limits"></a>Cosmos DB Ücretsiz limitleri deneyin

Aşağıdaki tabloda Ücretsiz deneme sürümü [için Azure Cosmos DB'yi deneyin](https://azure.microsoft.com/try/cosmosdb/) sınırları listelanmaktadır.

| Kaynak | Varsayılan limit |
| --- | --- |
| Duruşma süresi | 30 gün (herhangi bir sayıda yenilenebilir) |
| Abonelik başına maksimum kapsayıcılar (SQL, Gremlin, Tablo API) | 1 |
| Abonelik başına maksimum kapsayıcılar (MongoDB API) | 3 |
| Konteyner başına maksimum verim | 5000 |
| Paylaşılan iş oluşturma veritabanı başına maksimum iş verimi | 20000 |
| Hesap başına maksimum toplam depolama | 10 GB |

Cosmos DB'yi deneyin, yalnızca Orta ABD, Kuzey Avrupa ve Güneydoğu Asya bölgelerinde küresel dağıtımı destekler. Azure Cosmos DB hesaplarını deneyin için Azure destek biletleri oluşturulamaz. Ancak, mevcut destek planlarına sahip aboneler için destek sağlanır.

## <a name="free-tier-account-limits"></a>Ücretsiz katman hesap limitleri
Aşağıdaki tabloda Azure [Cosmos DB ücretsiz katman hesaplarının](optimize-dev-test.md#azure-cosmos-db-free-tier) sınırları listelanmaktadır.

| Kaynak | Varsayılan limit |
| --- | --- |
| Azure aboneliği başına ücretsiz katman hesabı sayısı | 1 |
| Serbest seviye indirim süresi | Hesabın ömrü. Hesap oluşturma sırasında kabul edilmelidir. |
| Maksimum RU/s ücretsiz | 400 RU/sn |
| Ücretsiz maksimum depolama alanı | 5 GB |
| Maksimum paylaşılan iş oluşturma veritabanı sayısı | 5 |
| Paylaşılan bir iş oluşturma veritabanında maksimum kapsayıcı sayısı | 25 <br>Serbest katman hesaplarında, 25 kapsayıcıya kadar paylaşılan bir iş veri tabanı için minimum RU/s 400 RU/s'dir. |

  Yukarıdakilere ek olarak, [hesap başına limitler](#per-account-limits) ücretsiz katman hesapları için de geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB'nin temel kavramları hakkında daha fazla bilgi [küresel dağıtım](distribute-data-globally.md) ve [bölümleme](partitioning-overview.md) ve [tedarik edilen iş bölümü.](request-units.md)

Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

* [Azure Cosmos DB SQL API’yi kullanmaya başlama](create-sql-api-dotnet.md)
* [MongoDB için Azure Cosmos DB'nin API'si ile başlayın](create-mongodb-nodejs.md)
* [Azure Cosmos DB Cassandra API’yi kullanmaya başlama](create-cassandra-dotnet.md)
* [Azure Cosmos DB Graph API’yi kullanmaya başlama](create-graph-dotnet.md)
* [Azure Cosmos DB Tablo API’yi kullanmaya başlama](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)
