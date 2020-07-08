---
title: Önizleme özelliği listesi
titleSuffix: Azure Cognitive Search
description: Önizleme özellikleri, müşterilerin tasarımı ve yardımcı programları hakkında geri bildirimde bulunmak için serbest bırakılır. Bu makale, şu anda önizleme aşamasında olan tüm özelliklerin kapsamlı bir listesidir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: b0c6672dcc9340e727c36b0bcf03fc8a8b176a3a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85830137"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Azure Bilişsel Arama Önizleme özellikleri

Bu makale, önizleme aşamasında olan tüm özelliklerin kapsamlı bir listesidir ve bu sayede bunları üretim kodunda kullanıp kullanmayacağınızı belirleyebilirsiniz. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Genel kullanılabilirliğe geçiş yapan Önizleme özellikleri bu listeden kaldırılır. Bir özellik aşağıda listelenmiyorsa, genel kullanıma açık olduğunu varsayabilirsiniz. Genel kullanılabilirliğiyle ilgili duyurular için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=search) [veya yenilikler.](whats-new.md)

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [**featuresMode parametresi**](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/search-documents#featuresmode) | İlgi (Puanlama) | Ayrıntıları dahil etmek için yakınlık puanı genişletmesi: alan başına benzerlik puanı, alan dönemi sıklığı başına ve eşleşen benzersiz belirteçlerin başına alan sayısı. Bu veri noktalarını [özel Puanlama çözümlerinde](https://github.com/Azure-Samples/search-ranking-tutorial)kullanabilirsiniz. | Bu sorgu parametresini, [arama belgeleri (REST)](https://docs.microsoft.com/rest/api/searchservice/search-documents) kullanarak api-Version = 2020-06 -30-preview veya 2019-05-06-Preview ile ekleyin. |
| [**Yönetilen hizmet kimliği**](search-howto-managed-identities-data-sources.md) | Dizin oluşturucular, güvenlik| Bir arama hizmetini güvenilen bir hizmet yapmak için Azure Active Directory ile kaydedin ve ardından bir Dizin Oluşturucu tarafından salt okuma erişimine izin vermek için Azure veri kaynaklarında RBAC izinlerini kullanın. | Portalı kullanırken veya [veri kaynağı (REST)](https://docs.microsoft.com/rest/api/searchservice/create-datasource) ile api-Version = 2020-06 -30-Preview veya api-Version = 2019-05 -06-Preview ile bu özelliğe erişin. |
| [**Hata ayıklama oturumları**](cognitive-search-debug-session.md) | Portal, AI zenginleştirme (beceri) | Bir beceri ile ilgili sorunları araştırmak ve çözmek için kullanılan bir oturum içi beceri Düzenleyicisi. Hata ayıklama oturumu sırasında uygulanan düzeltmeler, hizmette bir beceri kaydedilebilir. | Bir hata ayıklama oturumu açmak için genel bakış sayfasındaki orta sayfalı bağlantıları kullanarak yalnızca Portal. |
| [**Yerel blob geçici silme**](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) | Dizin oluşturucular, Azure Blob 'ları| Azure Bilişsel Arama 'deki Azure Blob depolama Dizin Oluşturucu, geçici olarak silinen bir durumdaki Blobları tanır ve dizin oluşturma sırasında ilgili arama belgesini kaldırır. | Api-Version = 2020-06 -30-Preview veya api-Version = 2019-05 -06-Preview ile [Create Indexer (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) kullanarak bu yapılandırma ayarını ekleyin. |
| [**Özel varlık arama becerisi**](cognitive-search-skill-custom-entity-lookup.md ) | AI zenginleştirme (beceri) | Özel, Kullanıcı tanımlı bir sözcük ve tümcecik listesinden metin gibi görünen bilişsel bir yetenek. Bu listeyi kullanarak tüm belgeleri eşleşen varlıklarla Etiketler. Bu beceri, benzer ancak tam olmayan eşleşmeleri bulmak için uygulanabilecek belirsiz eşleştirmeyi de destekler. | Portalda beceri düzenleyicisini kullanarak bu önizleme yeteneklerine başvurun veya api-Version = 2020-06 -30-Preview veya api-Version = 2019-05 -06-Preview ile [Beceri (REST) oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-skillset) . |
| [**PII algılama yeteneği**](cognitive-search-skill-pii-detection.md) | AI zenginleştirme (beceri) | Dizin oluşturma sırasında, kişisel olarak tanımlanabilir bilgileri bir giriş metinden çıkaran ve bu metni çeşitli yollarla maskeleme seçeneği sunan bilişsel bir yetenek. | Portalda beceri düzenleyicisini kullanarak bu önizleme yeteneklerine başvurun veya api-Version = 2020-06 -30-Preview veya api-Version = 2019-05 -06-Preview ile [Beceri (REST) oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-skillset) . |
| [**Artımlı zenginleştirme**](cognitive-search-incremental-indexing-conceptual.md) | Dizin Oluşturucu yapılandırması| Bir zenginleştirme işlem hattına önbelleğe alma ekler ve bir beceri veya başka bir nesne için bir güncelleştirme gibi hedeflenen bir değişiklik, içeriği değiştirmezse mevcut çıktıyı yeniden kullanmanıza olanak tanır. Önbelleğe alma yalnızca bir beceri tarafından üretilen zenginleştirilmiş belgeler için geçerlidir.| Api-Version = 2020-06 -30-Preview veya api-Version = 2019-05 -06-Preview ile [Create Indexer (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) kullanarak bu yapılandırma ayarını ekleyin. |
| [**Cosmos DB Indexer: MongoDB API 'SI, Gremlin API, Cassandra API**](search-howto-index-cosmosdb.md) | Dizin Oluşturucu veri kaynağı | Cosmos DB için, SQL API 'SI genel kullanıma sunulmuştur, ancak MongoDB, Gremlin ve Cassandra API 'Leri önizlemededir. | Yalnızca Gremlin ve Cassandra için, [ilk olarak kaydolun](https://aka.ms/azure-cognitive-search/indexer-preview) ve bu sayede, arka uçta aboneliğiniz için destek etkinleştirilebilir. MongoDB veri kaynakları portalda yapılandırılabilir. Aksi takdirde, üç API için veri kaynağı yapılandırması, API-Version = 2020-06 -30-Preview veya api-Version = 2019-05 -06-Preview ile [veri kaynağı oluşturma (REST)](https://docs.microsoft.com/rest/api/searchservice/create-datasource) kullanılarak desteklenir. |
|  [**Azure Data Lake Storage 2. Dizin Oluşturucu**](search-howto-index-azure-data-lake-storage.md) | Dizin Oluşturucu veri kaynağı | Data Lake Storage 2. ' den içerik ve meta verileri dizine edin.| [Kaydolma](https://aka.ms/azure-cognitive-search/indexer-preview) işlemi, arka uçta aboneliğiniz için destek etkinleştirilecek şekilde gereklidir. Bu veri kaynağına API-Version = 2020-06 -30-Preview veya api-Version = 2019-05 -06-Preview ile [veri kaynağı oluştur (REST)](https://docs.microsoft.com/rest/api/searchservice/create-datasource) kullanarak erişin. |
| [**moreLikeThis**](search-more-like-this.md) | Sorgu | Belirli bir belgeyle ilgili belgeleri bulur. Bu özellik daha önceki önizlemelerde yer aldı. | Bu sorgu parametresini, [arama belgeleri (REST)](https://docs.microsoft.com/rest/api/searchservice/search-documents) çağrılarında api-Version = 2020-06 -30-preview, 2019-05-06-preview, 2016-09-01-preview veya 2017-11-11-Preview ile ekleyin. |

## <a name="calling-preview-rest-apis"></a>Önizleme REST API 'Leri çağırma

Azure Bilişsel Arama, önce REST API önce, sonra .NET SDK 'nin ön sürüm sürümlerini kullanarak deneysel özellikleri önceden yayınlar.

Önizleme özellikleri, test ve deneme için, özellik tasarımı ve uygulama hakkında geri bildirim toplama hedefi ile kullanılabilir. Bu nedenle, Önizleme özellikleri zaman içinde değişebilir, belki de geriye doğru uyumluluğu kesen şekillerde değişiklik yapabilir. Bu, bir GA sürümündeki özelliklerin aksine, bu durum, küçük ve geriye dönük olarak uyumlu düzeltmeler ve geliştirmeler dışında değişmez ve çok düşüktür. Ayrıca, Önizleme özellikleri her zaman bir GA sürümüne değildir.

Portalda ve .NET SDK 'sında bazı Önizleme özellikleri kullanılabilir olsa da REST API her zaman önizleme özelliklerine sahiptir.

+ Arama işlemleri için [**`2020-06-30-Preview`**](https://docs.microsoft.com/rest/api/searchservice/index-preview) geçerli önizleme sürümüdür.

+ Yönetim işlemleri için [**`2019-10-01-Preview`**](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) geçerli önizleme sürümüdür.

Daha eski önizlemeler hala çalışır, ancak zaman içinde eski hale gelir. Kodunuz `api-version=2019-05-06-Preview` veya `api-version=2016-09-01-Preview` veya `api-version=2017-11-11-Preview` ' i çağırırsa, bu çağrılar hala geçerlidir. Ancak, geliştirmelerle yalnızca en yeni önizleme sürümü yenilenir. 

Aşağıdaki örnek sözdizimi, önizleme API sürümüne yapılan çağrıyı gösterir.

```HTTP
GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2020-06-30-Preview
```

Azure Bilişsel Arama hizmeti birden çok sürümde kullanılabilir. Daha fazla bilgi için bkz. [API sürümleri](search-api-versions.md).

## <a name="next-steps"></a>Sonraki adımlar

Arama REST önizleme API 'SI başvuru belgelerini inceleyin. Sorunlarla karşılaşırsanız [Stack Overflow](https://stackoverflow.com/) yardım almak için bize danışın veya [desteğe başvurun](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Arama hizmeti REST API başvurusu (Önizleme)](https://docs.microsoft.com/rest/api/searchservice/index-preview)