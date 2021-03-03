---
title: Azure Bilişsel Arama yenilikleri
description: Azure Bilişsel Arama Azure Search hizmet yeniden adlandırma özelliği de dahil olmak üzere yeni ve geliştirilmiş özelliklerin duyuruları.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: 606b102b1922d382f479235ffc4c61b68e7db766
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101676388"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure Bilişsel Arama yenilikleri

Hizmette nelerin yeni olduğunu öğrenin. Hizmette güncel kalmasını sağlamak için bu sayfaya yer işareti ekleyin. Henüz genel olarak kullanılamayan özelliklerin kapsamlı bir listesini görüntülemek için [Önizleme özelliği listesini](search-api-preview.md) gözden geçirin.

## <a name="march-2021"></a>Mart 2021

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  |  Açıklama | Kullanılabilirlik  |
|------------------------------|---------------|---------------|
| [Anlamsal arama](semantic-search-overview.md) | Daha ilgili sonuçlar oluşturmak için arama sonuçlarını değiştiren sorgu ile ilgili özelliklerin bir koleksiyonu. Arama isteğinde küçük değişikliklerle, mevcut dizinlerde bu özellikleri deneyebilirsiniz.</br></br>[Anlamsal sorgu](semantic-how-to-query-request.md) , derecelendirmeyi geliştirmek için doğal dil işlemede ilerlendirme sağlayan yeni bir sorgu türüdür ve yanıt, açıklamalı alt yazı ve anlam vurgulamaları sağlamak için sorgu hedefini anlayın.</br></br>[Anlamsal sıralama ve yanıtlar (yanıtlar, açıklamalı alt yazılar ve vurgular)](semantic-how-to-query-response.md) , anlam sorgusu ve yanıtın yapısına yönelik algoritmaya başvurur. | Genel Önizleme ([isteğe göre](https://aka.ms/SemanticSearchPreviewSignup)). </br></br>[Arama belgelerini kullanın (REST)](/rest/api/searchservice/preview-api/search-documents) API-Version = 2020-06 -30-önizleme ve [Arama Gezgini](search-explorer.md) Azure Portal. </br></br>Bölge ve katman kısıtlamaları geçerlidir. |
| [Yazım denetimi sorgu terimleri](speller-how-to-add.md) | Sorgu terimleri arama altyapısına ulaşmadan önce, yazım hatalarını denetlebilirler. `speller`Seçeneği herhangi bir sorgu türü (basit, tam veya anlamsal) ile birlikte kullanılır. |  Genel Önizleme, yalnızca REST, api-Version = 2020-06 -30-Önizleme|
| [SharePoint Online Dizin Oluşturucu](search-howto-index-sharepoint-online.md) | Bu Dizin Oluşturucu, bir belge kitaplığından içerik dizini oluşturabilmeniz için sizi bir SharePoint Online sitesine bağlar. | Genel Önizleme, yalnızca REST, api-Version = 2020-06 -30-Önizleme |

## <a name="february-2021"></a>Şubat 2021

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  |  Açıklama | Kullanılabilirlik  |
|------------------------------|---------------|---------------|
| [Belgeleri Sıfırla (Önizleme)](search-howto-run-reset-indexers.md) |  Dizin Oluşturucu iş yüklerindeki ayrı ayrı seçili arama belgelerini yeniden işler. | [Arama REST API 2020-06-30-Önizleme](/rest/api/searchservice/index-preview) |
| [Kullanılabilirlik Alanları](search-performance-optimization.md#availability-zones)| Belirli bölgelerde iki veya daha fazla çoğaltma ile, [performans Için ölçeklendirerek](search-performance-optimization.md#availability-zones), iki veya daha fazla ayrı fiziksel konumda çoğaltmalar bulundurarak dayanıklılık elde edin.  | Arama hizmeti oluşturma 'nin bölgesi ve tarihi kullanılabilirliği belirlenir. Ayrıntılar için bkz. performans için ölçek makalesi. |
| [Azure CLI](/cli/azure/search) </br>[Azure PowerShell](/powershell/module/az.search/) | Yeni düzeltmeler artık, IP güvenlik duvarı kuralları ve özel uç nokta desteği dahil olmak üzere yönetim REST API 2020-08-01 ' de tam işlem aralığını sağlıyor. | Genel olarak kullanılabilir. |

## <a name="january-2021"></a>Ocak 2021

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  |  Açıklama | Kullanılabilirlik  |
|------------------------------|-------------|---------------|
| [Azure Bilişsel Arama ve Soru-Cevap Oluşturma için Çözüm Hızlandırıcısı](https://github.com/Azure-Samples/search-qna-maker-accelerator) | Belgedeki soruları ve yanıtları çeker ve en ilgili yanıtları önerin. Live demo uygulaması adresinde bulunabilir [https://aka.ms/qnaWithAzureSearchDemo](https://aka.ms/qnaWithAzureSearchDemo) .  | Açık kaynak proje (SLA yok) |

## <a name="2020-archive"></a>2020 Arşivi

| Ay | Özellik | Açıklama |
|-------|---------|-------------|
| Kasım | [Müşteri tarafından yönetilen anahtar şifrelemesi (genişletilmiş)](search-security-manage-encryption-keys.md) | Müşteri tarafından yönetilen şifrelemeyi, bir arama hizmeti tarafından oluşturulan ve yönetilen tüm varlık aralığı üzerinde genişletir. Genel olarak kullanılabilir.|
| Eylül | [Azure Bilişsel Arama için Visual Studio Code uzantısı](search-get-started-vs-code.md) | Dizinler, Dizin oluşturucular, veri kaynakları ve becerileri oluşturmak için bir çalışma alanı, gezinti, IntelliSense ve şablonlar ekler. | Genel Önizleme |
| Eylül | [Yönetilen hizmet kimliği (Dizin oluşturucular)](search-howto-managed-identities-data-sources.md) | Genel olarak kullanılabilir.  |
| Eylül | [Özel bağlantı kullanan giden istekler](search-indexer-howto-access-private.md) | Genel olarak kullanılabilir.  |
| Eylül | [Yönetim REST API (2020-08-01)](/rest/api/searchmanagement/management-api-versions) | Genel olarak kullanılabilir. |
| Eylül | [Yönetim REST API (2020-08-01-Önizleme)](/rest/api/searchmanagement/management-api-versions) | Azure Işlevleri için paylaşılan özel bağlantı kaynağı ve MySQL için Azure SQL veritabanları ekler. |
| Eylül | [Yönetim .NET SDK 4,0](/dotnet/api/overview/azure/search/management) |  2020-08-01 sürümünü hedefleyen Yönetim SDK 'Sı için Azure SDK güncelleştirmesi REST API. Genel olarak kullanılabilir.|
| Ağustos | [Çift şifreleme](search-security-overview.md#encryption) | 1 Ağustos 2020 ' den sonra oluşturulan tüm Arama hizmetlerinde, bu bölgelerde genel kullanıma sunulmuştur: Batı ABD 2, Doğu ABD, Orta Güney ABD, US Gov Virginia, US Gov Arizona. |
| Temmuz | [Azure.Search.Documstalar istemci kitaplığı](/dotnet/api/overview/azure/search.documents-readme) | .NET için Azure SDK, genel kullanıma sunuldu. |
| Temmuz | [azure.search.documstalar istemci kitaplığı](/python/api/overview/azure/search-documents-readme)  | Python için Azure SDK, genel kullanıma sunuldu. |
| Temmuz | [@azure/search-documents istemci kitaplığı](/javascript/api/overview/azure/search-documents-readme)  | JavaScript için Azure SDK, genel kullanıma sunuldu. |
| Haziran | [Bilgi deposu](knowledge-store-concept-intro.md) | Genel olarak kullanılabilir. |
| Haziran | [REST API ara 2020-06-30](/rest/api/searchservice/) | Genel olarak kullanılabilir. |
| Haziran | [Arama REST API 2020-06-30-Önizleme](/rest/api/searchservice/) | , İsteğe bağlı olarak yeniden işleme becerileri ve artımlı zenginleştirme için Reset beceri ekler. |
| Haziran | [Okapi BM25 ilgi algoritması](index-ranking-similarity.md) | Genel olarak kullanılabilir. |
| Haziran |  **ExecutionEnvironment** (Azure özel bağlantısı kullanılarak arama hizmetleri için geçerlidir.) | Genel olarak kullanılabilir. |
| Haziran | [AML becerisi (Önizleme)](cognitive-search-aml-skill.md) | Özel bir Azure Machine Learning (AML) modeliyle AI zenginleştirme 'yi genişleten bilişsel bir yetenek. |
| Mayıs | [Hata ayıklama oturumları (Önizleme)](cognitive-search-debug-session.md) | Portalda beceri hata ayıklayıcı.  |
| Mayıs | [Bağlantılı güvenlik duvarı desteği için IP kuralları](service-configure-firewall.md) | Genel olarak kullanılabilir.  |
| Mayıs | [Özel arama uç noktası için Azure özel bağlantısı](service-create-private-endpoint.md) | Genel olarak kullanılabilir.  |
| Mayıs | [Yönetilen hizmet kimliği (Dizin oluşturucular)-(Önizleme)](search-howto-managed-identities-data-sources.md) | Yönetilen bir kimlik kullanarak Azure veri kaynaklarına bağlanın.  |
| Mayıs | [SessionID sorgu parametresi](index-similarity-and-scoring.md), [scoringStatistics = Global parametre](index-similarity-and-scoring.md#scoring-statistics)  | Genel Arama İstatistikleri, [arama ilgisi için makine öğrenimi (LearnToRank) modelleri](https://github.com/Azure-Samples/search-ranking-tutorial)için kullanışlıdır.  |
| Mayıs | [featuresMode uygunluk puanı genişletmesi (Önizleme)](index-similarity-and-scoring.md#featuresMode-param)  |   |
|Mart  | [Yerel blob geçici silme (Önizleme)](search-howto-index-changed-deleted-blobs.md) | Kaynak blobu blob depolamada geçici olarak silinirse, arama belgelerini siler. |
|Mart  | [Yönetim REST API (2020-03-13)](/rest/api/searchmanagement/management-api-versions) | Genel olarak kullanılabilir. |
|Şubat | [PII algılama yeteneği (Önizleme)](cognitive-search-skill-pii-detection.md)  | Kişisel bilgileri çıkaran ve maskeden bilişsel bir yetenek. |
|Şubat | [Özel varlık arama becerisi (Önizleme)](cognitive-search-skill-custom-entity-lookup.md) | Bir listeden kelimeleri ve tümceleri bulan ve eşleşen varlıkları olan tüm belgeleri etiketleyen bilişsel bir yetenek.  |
|Ocak | [Müşteri tarafından yönetilen anahtar şifrelemesi](search-security-manage-encryption-keys.md) | Genel kullanıma sunuldu  |
|Ocak | [Bağlantılı güvenlik duvarı desteği için IP kuralları (Önizleme)](service-configure-firewall.md) | [CreateOrUpdate API](/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service)'de yeni **ıprule** ve **networkruleset** özellikleri.  |
|Ocak | [Özel uç nokta oluşturma (Önizleme)](service-create-private-endpoint.md) | Arama hizmetinize güvenli bağlantılar için özel bir bağlantı kurun. Bu önizleme özelliğinin, çözümün bir parçası olarak bir [Azure özel bağlantısı](../private-link/private-link-overview.md) ve [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) vardır. |

## <a name="2019-archive"></a>2019 Arşivi

| Ay | Özellik | Açıklama |
|-------|---------|-------------|
|Aralık | [Demo uygulaması oluşturma (Önizleme)](search-create-app-portal.md) | Bir dizine erişim (salt okuma) ile indirilebilir bir HTML dosyası üreten ve tam istemci uygulaması için kısa bir kesme yerine doğrulama ve test aracı olarak tasarlanan bir sihirbaz.|
|Kasım | [Artımlı zenginleştirme (Önizleme)](cognitive-search-incremental-indexing-conceptual.md) | Daha sonra yeniden kullanmak üzere beceri işlemeyi önbelleğe alır.  |
|Kasım | [Belge ayıklama becerisi (Önizleme)](cognitive-search-skill-document-extraction.md) | Bir dosyanın içeriğini bir beceri içinden ayıklamak için bilişsel bir yetenek.|
|Kasım | [Metin çevirisi becerisi](cognitive-search-skill-text-translation.md) | Metin değerlendiren ve çeviren dizin oluşturma sırasında kullanılan bilişsel bir yetenek. Genel olarak kullanılabilir.|
|Kasım | [Power BI şablonları](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) | Bilgi deposundaki içeriği görselleştirmeyi sağlayan şablon |
|Kasım | [Azure Data Lake Storage 2. (Önizleme)](search-howto-index-azure-data-lake-storage.md), [Cosmos DB GREMLIN API (önizleme)](search-howto-index-cosmosdb.md)ve [Cosmos DB Cassandra API (Önizleme)](search-howto-index-cosmosdb.md) | Yeni Dizin Oluşturucu veri kaynakları genel önizlemede. |
|Temmuz | [Azure Kamu bulut desteği](https://azure.microsoft.com/global-infrastructure/services/?regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-texas,usgov-virginia&products=search) | Genel olarak kullanılabilir.|

<a name="new-service-name"></a>

## <a name="new-service-name"></a>Yeni hizmet adı

Azure Search, ana işlemlerde bilişsel yeteneklerin ve AI işlemenin genişletilmiş (henüz isteğe bağlı) kullanımını yansıtmak üzere 2019 Ekim 'de **Azure bilişsel arama** olarak yeniden adlandırıldı. API sürümleri, NuGet paketleri, ad alanları ve uç noktalar değiştirilmez. Yeni ve mevcut arama çözümleri, hizmet adı değişikliğinden etkilenmez.

## <a name="service-updates"></a>Hizmet güncelleştirmeleri

Azure Bilişsel Arama için [hizmet güncelleştirme duyuruları](https://azure.microsoft.com/updates/?product=search&status=all) Azure Web sitesinde bulunabilir.