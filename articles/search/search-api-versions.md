---
title: API sürümleri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama REST API 'Leri ve .NET SDK içindeki istemci kitaplığı için sürüm ilkesi.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/20/2020
ms.openlocfilehash: 5be50453dff9acaf4a9876eec1d95b56abebf745
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87029850"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Azure Bilişsel Arama API sürümleri

Azure Bilişsel Arama, özellik güncelleştirmelerini düzenli olarak kaydeder. Bazen, her zaman olmasa da, geriye dönük uyumluluğu korumak için bu güncelleştirmeler API 'nin yeni bir sürümünü gerektirir. Yeni bir sürümü yayımlamak, kodunuzda arama hizmeti güncelleştirmelerini ne zaman ve nasıl tümleştirmenizi denetlemenize olanak tanır.

Bir kural olarak, Azure Bilişsel Arama ekibi, kodunuzu yeni bir API sürümü kullanacak şekilde yükseltmek için biraz çaba içerebileceği için, yalnızca gerektiğinde yeni sürümler yayımlar. Yeni bir sürüm yalnızca API 'nin bazı yönlerinin geriye dönük uyumluluğu kesen bir şekilde değişmesi durumunda gereklidir. Bu tür değişiklikler, mevcut özelliklerle ilgili düzeltmeler veya var olan API yüzey alanını değiştiren yeni özelliklerden kaynaklanıyor olabilir.

Aynı kural SDK güncelleştirmeleri için de geçerlidir. Azure Bilişsel Arama SDK, anlam sürümü [oluşturma](https://semver.org/) kurallarını izler, yani sürümünün üç bölümü vardır: Ana, ikincil ve derleme numarası (örneğin, 1.1.0). SDK 'nın yeni bir ana sürümü yalnızca geriye dönük uyumluluğu kesen değişiklikler için serbest bırakılır. Önemli olmayan özellik güncelleştirmeleri ikincil sürümü artırır ve hata düzeltmeleri yalnızca derleme sürümünü artırır.

> [!Important]
> .NET, Java, Python ve JavaScript için Azure SDK 'Ları, Azure Bilişsel Arama yönelik yeni istemci kitaplıklarını kullanıma alıyor. Şu anda Azure SDK kitaplıklarının hiçbiri en son arama REST API 'Lerini (2020-06-30) veya yönetim REST API 'Lerini (2020-03-13) tam olarak desteklemez, ancak bu zaman içinde değişecektir. Bu sayfayı veya işlevsel geliştirmelerle [ilgili duyurulara yönelik yenilikler](whats-new.md) ' i düzenli olarak denetleyebilirsiniz. 

## <a name="rest-apis"></a>REST API'leri

Azure Bilişsel Arama hizmet örneği, en son sürüm dahil olmak üzere birkaç REST API sürümü destekler. Artık en son bir sürüm olmadığında bir sürümü kullanmaya devam edebilirsiniz, ancak kodunuzu en yeni sürümü kullanmak için [geçirmeniz](search-api-migration.md) önerilir. REST API kullanırken, API sürümü parametresi aracılığıyla her istekte API sürümünü belirtmeniz gerekir. .NET SDK kullanıldığında, kullanmakta olduğunuz SDK sürümü REST API ilgili sürümünü belirler. Daha eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü destekleyecek şekilde yükseltilse bile, bu kodu hiçbir değişiklik yapmadan çalıştırmaya devam edebilirsiniz.

Aşağıdaki tabloda, Arama Hizmeti REST API geçerli ve daha önce yayımlanmış sürümlerinin sürüm geçmişi verilmiştir. Belgeler yalnızca en son kararlı ve önizleme sürümleri için yayımlanır.

### <a name="search-service-apis"></a>Arama Hizmeti API 'Leri

Arama hizmetinde içerik oluşturun ve yönetin.

| Sürüm&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Durum | Geriye dönük uyumluluk sorunu |
|-------------------------|--------|------------------------------|
| [Arama 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/index)| Dengeli | Arama REST API 'lerinin en yeni kararlı sürümü, ilgi Puanlama ve genellikle bilgi deposu için kullanılabilirlik ile yapılır.|
| [Arama 2020-06-30-Preview](https://docs.microsoft.com/rest/api/searchservice/index-preview)| Önizleme | Kararlı sürümle ilişkili önizleme sürümü. Birden çok [Önizleme özelliği](search-api-preview.md)içerir. |
| Arama 2019-05-06 | Dengeli | [Karmaşık türler](search-howto-complex-data-types.md)ekler. |
| Arama 2019-05-06-Preview | Önizleme | Kararlı sürümle ilişkili önizleme sürümü. |
| Arama 2017-11-11 | Dengeli  | Becerileri ve [AI zenginleştirme](cognitive-search-concept-intro.md)ekler. |
| Arama 2017-11-11-Önizleme | Önizleme | Kararlı sürümle ilişkili önizleme sürümü. |
| Arama 2016-09-01 |Dengeli | [Dizin oluşturucular](search-indexer-overview.md)ekler. |
| Arama 2016-09-01-Önizleme | Önizleme | Kararlı sürümle ilişkili önizleme sürümü.|
| Arama 2015-02-28 | Dengeli  | İlk genel olarak kullanılabilir sürüm.  |
| Arama 2015-02-28-Önizleme | Önizleme | Kararlı sürümle ilişkili önizleme sürümü. |
| Arama 2014-10-20-Önizleme | Önizleme | İkinci genel önizleme. |
| Arama 2014-07-31-Önizleme | Önizleme | İlk genel önizleme. |

### <a name="management-rest-apis"></a>Yönetim REST API’leri

Bir arama hizmeti oluşturup yapılandırın ve API anahtarlarını yönetin.

| Sürüm&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Durum | Geriye dönük uyumluluk sorunu |
|-------------------------|--------|------------------------------|
| [Yönetim 13.03.2020](https://docs.microsoft.com/rest/api/searchmanagement/) | Dengeli | Uç nokta korumasında ilerleme ile yönetim REST API 'lerinin en yeni kararlı sürümü. Özel bağlantı aracılığıyla özel [uç nokta](service-create-private-endpoint.md) ve yeni hizmetler IÇIN [ağ IP kuralları](service-configure-firewall.md) ekler. |
| [Yönetim 2019-10-01-Preview](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) | Önizleme  | Sürüm numarasına rağmen bu, hala yönetim REST API 'lerinin güncel önizleme sürümüdür. Şu anda önizleme özelliği yok. Tüm Önizleme özellikleri kısa bir süre önce genel kullanıma geçti. |
| Yönetim 2015-08-19  | Dengeli | Yönetim REST API 'lerinin ilk genel olarak kullanılabilir sürümü. Hizmet sağlama, ölçek artırma ve API anahtar yönetimi sağlar. |
| Yönetim 2015-08-19-Önizleme | Önizleme | Yönetim REST API 'lerinin ilk önizleme sürümü. |

## <a name="azure-sdk-for-net"></a>.NET için Azure SDK

Paket sürümü geçmişi NuGet.org üzerinde kullanılabilir. Bu tablo, her paket sayfasına bağlantılar sağlar.


| SDK sürümü | Durum | Açıklama |
|-------------|--------|------------------------------|
| [Azure.Search.Documstaları 11,0](https://www.nuget.org/packages/Azure.Search.Documents/1.0.0-preview.4) | Dengeli | Azure .NET SDK 'dan yeni istemci kitaplığı, 2020 Temmuz 'da yayınlandı. Arama REST API-Version = 2020-06-30 REST API, ancak henüz, coğrafi filtreler veya [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder?view=azure-dotnet)'ı desteklemez. |
| [Microsoft. Azure. Search 10,0](https://www.nuget.org/packages/Microsoft.Azure.Search/) | Dengeli | Yayınlanan 2019 Mayıs. Arama REST API-Version = 2019-05-06 ' i hedefler.|
| [Microsoft. Azure. Search 8,0-Önizleme](https://www.nuget.org/packages/Microsoft.Azure.Search/8.0.0-preview) | Önizleme | Yayın tarihi 2019 Nisan. Arama REST API-Version = 2019-05 -06-Preview ' i hedefler.|
| [Microsoft. Azure. Management. Search 3.0.0](https://docs.microsoft.com/dotnet/api/overview/azure/search/management?view=azure-dotnet) | Dengeli | Yönetim REST API-Version = 2015-08-19 ' i hedefler.  |

## <a name="azure-sdk-for-java"></a>Java için Azure SDK

| SDK sürümü | Durum | Açıklama  |
|-------------|--------|------------------------------|
| [Java Azure-Arama-belgeler 11](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-search-documents/11.0.0/index.html) | Dengeli | Azure .NET SDK 'dan yeni istemci kitaplığı, 2020 Temmuz 'da yayınlandı. Arama REST API-Version = 2019-05-06 ' i hedefler. |
| [Java yönetim Istemcisi 1.35.0](https://docs.microsoft.com/java/api/overview/azure/search/management?view=azure-java-stable) | Dengeli | Yönetim REST API-Version = 2015-08-19 ' i hedefler. |

## <a name="azure-sdk-for-javascript"></a>JavaScript için Azure SDK

| SDK sürümü | Durum | Açıklama  |
|-------------|--------|------------------------------|
| [JavaScript Azure-Arama 11,0](https://azure.github.io/azure-sdk-for-node/azure-search/latest/) | Dengeli | Azure .NET SDK 'dan yeni istemci kitaplığı, 2020 Temmuz 'da yayınlandı. Arama REST API-Version = 2016-09-01 ' i hedefler. |
| [JavaScript Azure-ARM-arama](https://azure.github.io/azure-sdk-for-node/azure-arm-search/latest/) | Dengeli | Yönetim REST API-Version = 2015-08-19 ' i hedefler. |

## <a name="azure-sdk-for-python"></a>Python için Azure SDK

| SDK sürümü | Durum | Açıklama  |
|-------------|--------|------------------------------|
| [Python Azure-Arama-belgeler 11,0](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-search-documents/11.0.0/index.html) | Dengeli | Azure .NET SDK 'dan yeni istemci kitaplığı, 2020 Temmuz 'da yayınlandı. Arama REST API-Version = 2019-05-06 ' i hedefler. |
| [Python Azure-MGMT-arama 1,0](https://docs.microsoft.com/python/api/overview/azure/search?view=azure-python) | Dengeli | Yönetim REST API-Version = 2015-08-19 ' i hedefler. |