---
title: Azure Bilişsel Arama yenilikleri
description: Azure Bilişsel Arama Azure Search hizmet yeniden adlandırma özelliği de dahil olmak üzere yeni ve geliştirilmiş özelliklerin duyuruları.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 07/10/2020
ms.openlocfilehash: 01968335ffcaffb26cabd2a5174d00e5471409c1
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86232041"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure Bilişsel Arama yenilikleri

Hizmette nelerin yeni olduğunu öğrenin. Hizmette güncel kalmasını sağlamak için bu sayfaya yer işareti ekleyin.

## <a name="feature-announcements"></a>Özellik Duyuruları

### <a name="june-2020"></a>Haziran 2020

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
[**Bilgi deposu**](knowledge-store-concept-intro.md) | Yapay zeka zenginleştirme | Bir AI zenginleştirme dizin oluşturucunun çıkışı, Azure depolama 'da içeriği diğer uygulamalarda ve işlemlerde kullanılmak üzere depolar. | Genel olarak kullanılabilir. </br> [Arama REST API 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) veya üzerini ya da portalını kullanın. |
| [**REST API ara 2020-06-30**](https://docs.microsoft.com/rest/api/searchservice/) | REST | REST API 'lerin yeni kararlı bir sürümü. Bilgi deposuna ek olarak, bu sürüm arama ilgisi ve Puanlama geliştirmeleri içerir. | Genel olarak kullanılabilir. |
| [**Okapi BM25 ilgi algoritması**](https://en.wikipedia.org/wiki/Okapi_BM25) | Sorgu | Yeni ilgi derecelendirme algoritması, 15 Temmuz 'dan sonra oluşturulan tüm yeni arama hizmetleri için otomatik olarak kullanılır. Daha önce oluşturulan hizmetler için `similarity` Dizin alanları üzerinde özelliğini ayarlayarak kabul edebilirsiniz. | Genel olarak kullanılabilir. </br> [Arama REST API 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) veya üstünü ya da REST API 2019-05-06 kullanın. |
| **executionEnvironment** | Güvenlik (Dizin oluşturucular) | `private`Özel bir uç nokta üzerinden dış veri kaynaklarına yönelik tüm bağlantıları zorlamak için bu Dizin Oluşturucu yapılandırma özelliğini olarak ayarlayın. Yalnızca Azure özel bağlantı özelliğinden yararlanan Arama Hizmetleri için geçerlidir. | Genel olarak kullanılabilir. </br> Bu genel yapılandırma parametresini ayarlamak için [arama REST API 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) ' i kullanın. |
| [**Azure Machine Learning (AML) yeteneği**](cognitive-search-aml-skill.md) | Yapay zeka zenginleştirme| Azure Machine Learning bir Inde sınırlama uç noktasını bütünleştirmek için yeni bir yetenek türü. [Bu öğreticiyi](cognitive-search-tutorial-aml-custom-skill.md)kullanmaya başlayın. | Genel olarak kullanılabilir. </br> [Arama REST API 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) veya üstünü kullanın. Portal 'da, Beceri tasarımında, Bilişsel Arama ve Azure ML hizmetlerinin aynı abonelikte dağıtıldığı varsayıldığında de mevcuttur. |

### <a name="may-2020-microsoft-build"></a>Mayıs 2020 (Microsoft derleme)

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [**Hata ayıklama oturumları**](cognitive-search-debug-session.md) | Yapay zeka zenginleştirme | Hata ayıklama oturumları, mevcut bir beceri sorunları araştırmak ve çözmek için portal tabanlı bir arabirim sağlar. Hata ayıklama oturumunda oluşturulan düzeltmeler üretim becerileri kaydedilebilir. [Bu öğreticiyi](cognitive-search-tutorial-debug-sessions.md)kullanmaya başlayın. | Genel Önizleme, portalda. |
| [**Bağlantılı güvenlik duvarı desteği için IP kuralları**](service-configure-firewall.md) | Güvenlik | Arama hizmeti uç noktasına erişimi belirli IP adresleriyle sınırlayın. | Genel olarak kullanılabilir. </br> [Yönetim REST API 2020-03-13](https://docs.microsoft.com/rest/api/searchmanagement/) veya üstünü ya da portalını kullanın. |
| [**Özel arama uç noktası için Azure özel bağlantısı**](service-create-private-endpoint.md) | Güvenlik| Yalnızca istemci uygulamaları ve aynı sanal ağdaki diğer Azure hizmetleri tarafından erişilebilen bir özel bağlantı kaynağı olarak çalıştırarak, genel İnternet 'ten bir arama hizmetini koruyun. | Genel olarak kullanılabilir. </br> [Yönetim REST API 2020-03-13](https://docs.microsoft.com/rest/api/searchmanagement/) veya üstünü ya da portalını kullanın. |
| [**sistem tarafından yönetilen kimlik (Önizleme)**](search-howto-managed-identities-data-sources.md) | Güvenlik (Dizin oluşturucular) | Dizin oluşturma için desteklenen Azure veri kaynağıyla bağlantıları kurmak üzere Azure Active Directory ile bir arama hizmetini Güvenilen hizmet olarak kaydettirin. Azure SQL veritabanı, Azure Cosmos DB ve Azure depolama gibi Azure veri kaynaklarından içerik alan [Dizin oluşturucular](search-indexer-overview.md) için geçerlidir. | Genel Önizleme. </br> Arama hizmetini kaydetmek için portalı kullanın. |
| [**SessionID sorgu parametresi**](index-similarity-and-scoring.md), [scoringStatistics = Global parametre](index-similarity-and-scoring.md#scoring-statistics) | Sorgu (ilgi) | Arama puanlarını hesaplama için bir oturum oluşturmak üzere bir sorguya SessionID ekleyerek, daha tutarlı arama puanı hesaplamaları için scoringStatistics = Global ile tüm parçalardan puanları toplayın. | Genel olarak kullanılabilir. </br> [Arama REST API 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) veya üstünü ya da REST API 2019-05-06 kullanın. |
| [**featuresMode (Önizleme)**](index-similarity-and-scoring.md#featuresMode-param) | Sorgu | Daha fazla ayrıntı göstermek için bu sorgu parametresini ekleyin: alan başına benzerlik puanı, alan dönemi sıklığı başına ve eşleşen benzersiz belirteçlerin başına alan sayısı. Bu veri noktalarını özel Puanlama algoritmalarında kullanabilirsiniz. Bu özelliği gösteren bir örnek için bkz. [ilgiyi aramak için makine öğrenimi ekleme (LearnToRank)](https://github.com/Azure-Samples/search-ranking-tutorial). | Genel Önizleme. </br> [Arama REST API 2020-06-30-önizleme](https://docs.microsoft.com/rest/api/searchservice/index-preview) veya REST API 2019-05-06-Preview kullanın. |

### <a name="march-2020"></a>Mart 2020

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [**Yerel blob geçici silme (Önizleme)**](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) | Dizin Oluşturucular | Azure Bilişsel Arama 'deki bir Azure Blob depolama Dizin Oluşturucu, geçici olarak silinen bir durumdaki Blobları tanır ve dizin oluşturma sırasında karşılık gelen arama belgesini kaldırır. | Genel Önizleme. </br> Yerel "geçici silme" özelliği etkin olan bir Azure blob veri kaynağında Dizin Oluşturucu Çalıştır [REST API 2020-06-30-Preview](https://docs.microsoft.com/rest/api/searchservice/index-preview) ve REST API 2019-05-06-Preview ' i kullanın. |
| [**Yönetim REST API (2020-03-13)**](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) | REST | Bir arama hizmeti oluşturmak ve yönetmek için yeni kararlı REST API. IP güvenlik duvarı ve özel bağlantı desteği ekler | Genel olarak kullanılabilir. |

### <a name="february-2020"></a>Şubat 2020

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [**PII algılama (Önizleme)**](cognitive-search-skill-pii-detection.md) | Yapay zeka zenginleştirme | Dizin oluşturma sırasında, kişisel olarak tanımlanabilir bilgileri bir giriş metinden çıkaran ve bu metni çeşitli yollarla maskelemek için kullanılan yeni bilişsel bir yetenek. | Genel Önizleme. </br> Portalı veya [arama REST API 2020-06-30-önizleme](https://docs.microsoft.com/rest/api/searchservice/index-preview) veya REST API 2019-05-06-Preview ' i kullanın. |
| [**Özel varlık arama (Önizleme)**](cognitive-search-skill-custom-entity-lookup.md )| Yapay zeka zenginleştirme | Özel, Kullanıcı tanımlı bir sözcük ve tümcecik listesinden metin sağlayan yeni bir bilişsel beceri. Bu listeyi kullanarak tüm belgeleri eşleşen varlıklarla Etiketler. Bu beceri, benzer ancak tam olmayan eşleşmeleri bulmak için uygulanabilecek belirsiz eşleştirmeyi de destekler. | Genel Önizleme. </br> Portalı veya [arama REST API 2020-06-30-önizleme](https://docs.microsoft.com/rest/api/searchservice/index-preview) veya REST API 2019-05-06-Preview ' i kullanın. |

### <a name="january-2020"></a>Ocak 2020

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [**Müşteri tarafından yönetilen şifreleme anahtarları**](search-security-manage-encryption-keys.md) |Güvenlik | Platformun yerleşik şifrelemeye ek olarak ek bir şifreleme katmanı ekler. Oluşturduğunuz ve yönettiğiniz bir şifreleme anahtarını kullanarak, yük bir arama hizmetine ulaşmadan önce dizin içeriğini ve eş anlamlı haritaları şifreleyebilirsiniz. | Genel olarak kullanılabilir. </br> Arama REST API 2019-05-06 veya üstünü kullanın. Yönetilen kod için, özellik Önizleme dışında olsa bile, doğru paket yine de [.NET SDK sürüm 8,0-Önizleme](search-dotnet-sdk-migration-version-9.md) ' dir. |
| [**Bağlantılı güvenlik duvarı desteği için IP kuralları (Önizleme)**](service-configure-firewall.md) | Güvenlik | Arama hizmeti uç noktasına erişimi belirli IP adresleriyle sınırlayın. Preview API 'sinin [CreateOrUpdate API](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service)'de yeni **ıprule** ve **networkruleset** özellikleri vardır. Bu önizleme özelliği seçili bölgelerde kullanılabilir. |  Api-Version = 2019-10 -01-Preview kullanarak genel önizleme.  |
| [**Özel arama uç noktası için Azure özel bağlantısı (Önizleme)**](service-create-private-endpoint.md) | Güvenlik| Yalnızca istemci uygulamaları ve aynı sanal ağdaki diğer Azure hizmetleri tarafından erişilebilen bir özel bağlantı kaynağı olarak çalıştırarak, genel İnternet 'ten bir arama hizmetini koruyun. | Api-Version = 2019-10 -01-Preview kullanarak genel önizleme.  |

## <a name="feature-announcements-in-2019"></a>2019 sürümündeki Özellik Duyuruları

### <a name="december-2019"></a>Aralık 2019

+ [Demo uygulaması oluşturma (Önizleme)](search-create-app-portal.md) , portalda bir dizine erişim (salt okuma) ile INDIRILEBILIR bir HTML dosyası üreten yeni bir sihirbazdır. Dosya, arama hizmetinizde bir dizine bağlı olan, işlemsel bir "localhost" stili Web uygulaması işleyen katıştırılmış komut dosyası ile birlikte gelir. Sayfalar sihirbazda yapılandırılabilir ve bir arama çubuğu, sonuç alanı, kenar çubuğu gezintisi ve typeahead sorgu desteği bulunabilir. İş akışını veya görünümü genişletmek veya özelleştirmek için HTML 'yi çevrimdışı değiştirebilirsiniz. Tanıtım uygulaması, genellikle üretim senaryolarında gerekli olan güvenlik ve barındırma katmanlarını içerecek şekilde kolayca genişletilmez. Tam istemci uygulaması için kısa bir kesme yerine doğrulama ve test aracı olarak düşünmeniz gerekir.

+ [Güvenli bağlantılar için özel bir uç nokta oluşturma (Önizleme)](service-create-private-endpoint.md) arama hizmetinize güvenli bağlantılar için özel bir bağlantı ayarlamayı açıklar. Bu önizleme özelliği, istek üzerine kullanılabilir ve çözümün bir parçası olarak [Azure özel bağlantısı](../private-link/private-link-overview.md) ve [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) kullanır.

### <a name="november-2019---ignite-conference"></a>Kasım 2019-Menite Konferansı

+ [Artımlı zenginleştirme (Önizleme)](cognitive-search-incremental-indexing-conceptual.md) , bir zenginleştirme ardışık düzenine önbelleğe alma ve statefullstate ekler. böylece, zaten işlenmiş olan içeriği kaybetmeden belirli adımlarla veya aşamalarda çalışabilirsiniz. Daha önce, bir zenginleştirme ardışık düzeninde yapılan herhangi bir değişiklik tam yeniden oluşturma gerektirdi. Artımlı çözümlemenin, özellikle de görüntü analizinden oluşan çıkış, korunur.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ [Belge ayıklama (Önizleme)](cognitive-search-skill-document-extraction.md) , dizin oluşturma sırasında kullanılan bilişsel bir beceriye sahiptir ve bir dosyanın içeriğini bir beceri içinden ayıklamanızı sağlar. Daha önce belge çözme yalnızca beceri yürütmeden önce oluşmuştur. Bu beceriye ek olarak, bu işlemi beceri yürütme içinde de gerçekleştirebilirsiniz.

+ [Metin çevirisi](cognitive-search-skill-text-translation.md) , dizinleme sırasında metni değerlendiren ve her kayıt için belirtilen hedef dile çevrilen metni döndüren bilişsel bir yetenküldür.

+ [Power BI şablonlar](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) , görselleştirmelerinizi ve Power BI masaüstündeki bir bilgi deposunda zenginleştirme içeriği analizini başlatabilir. Bu şablon, [veri Içeri aktarma Sihirbazı](knowledge-store-create-portal.md)aracılığıyla oluşturulan Azure Tablo projeksiyonlarını için tasarlanmıştır.

+ [Azure Data Lake Storage 2. (Önizleme)](search-howto-index-azure-data-lake-storage.md), [Cosmos DB GREMLIN API (önizleme)](search-howto-index-cosmosdb.md)ve [Cosmos DB Cassandra API (Önizleme)](search-howto-index-cosmosdb.md) Dizin oluşturucularda artık desteklenmektedir. [Bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)kullanarak kaydolabilirsiniz. Önizleme programına kabul edildikten sonra bir onay e-postası alacaksınız.

### <a name="july-2019"></a>Temmuz 2019

+ [Azure Kamu Bulutu](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search)'nda genel olarak kullanılabilir.

<a name="new-service-name"></a>

## <a name="new-service-name"></a>Yeni hizmet adı

Azure Search, çekirdek işlemlerde bilişsel yeteneklerin ve AI işlemenin genişletilmiş (henüz isteğe bağlı) kullanımını yansıtmak için artık **Azure bilişsel arama** olarak yeniden adlandırıldı. API sürümleri, NuGet paketleri, ad alanları ve uç noktalar değiştirilmez. Yeni ve mevcut arama çözümleri, hizmet adı değişikliğinden etkilenmez.

## <a name="service-updates"></a>Hizmet güncelleştirmeleri

Azure Bilişsel Arama için [hizmet güncelleştirme duyuruları](https://azure.microsoft.com/updates/?product=search&status=all) Azure Web sitesinde bulunabilir.