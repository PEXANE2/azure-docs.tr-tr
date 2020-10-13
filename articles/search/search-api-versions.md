---
title: API sürümleri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama REST API 'Leri ve .NET SDK içindeki istemci kitaplığı için sürüm ilkesi.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 71862bedc009d560adc8131eacc37c0afba25d81
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91761728"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Azure Bilişsel Arama API sürümleri

Azure Bilişsel Arama, özellik güncelleştirmelerini düzenli olarak kaydeder. Bazen, her zaman olmasa da, geriye dönük uyumluluğu korumak için bu güncelleştirmeler API 'nin yeni bir sürümünü gerektirir. Yeni bir sürümü yayımlamak, kodunuzda arama hizmeti güncelleştirmelerini ne zaman ve nasıl tümleştirmenizi denetlemenize olanak tanır.

Bir kural olarak, Azure Bilişsel Arama ekibi, kodunuzu yeni bir API sürümü kullanacak şekilde yükseltmek için biraz çaba içerebileceği için, yalnızca gerektiğinde yeni sürümler yayımlar. Yeni bir sürüm yalnızca API 'nin bazı yönlerinin geriye dönük uyumluluğu kesen bir şekilde değişmesi durumunda gereklidir. Bu tür değişiklikler, mevcut özelliklerle ilgili düzeltmeler veya var olan API yüzey alanını değiştiren yeni özelliklerden kaynaklanıyor olabilir.

Aynı kural SDK güncelleştirmeleri için de geçerlidir. Azure Bilişsel Arama SDK, anlam sürümü [oluşturma](https://semver.org/) kurallarını izler, yani sürümünün üç bölümü vardır: Ana, ikincil ve derleme numarası (örneğin, 1.1.0). SDK 'nın yeni bir ana sürümü yalnızca geriye dönük uyumluluğu kesen değişiklikler için serbest bırakılır. Önemli olmayan özellik güncelleştirmeleri ikincil sürümü artırır ve hata düzeltmeleri yalnızca derleme sürümünü artırır.

> [!Important]
> .NET, Java, Python ve JavaScript için Azure SDK 'Ları, Azure Bilişsel Arama yönelik yeni istemci kitaplıklarını kullanıma alıyor. Şu anda Azure SDK kitaplıklarının hiçbiri en son arama REST API 'Lerini (2020-06-30) veya yönetim REST API 'Lerini (2020-03-13) tam olarak desteklemez, ancak bu zaman içinde değişecektir. Bu sayfayı veya işlevsel geliştirmelerle [ilgili duyurulara yönelik yenilikler](whats-new.md) ' i düzenli olarak denetleyebilirsiniz.

<a name="unsupported-versions"></a>

## <a name="unsupported-versions"></a>Desteklenmeyen sürümler

Mevcut arama çözümlerini 15 Ekim 2020 ' e kadar en son REST API sürümüne yükseltin. Bu sırada, Azure Bilişsel Arama REST API aşağıdaki sürümleri kullanımdan kaldırılacak ve artık desteklenmeyecektir:

+ **2015-02-28**
+ **2015-02-28-Önizleme**
+ **2014-07-31-Önizleme**
+ **2014-10-20-Önizleme**

Ayrıca, Azure Bilişsel Arama .NET SDK 'nın [**3.0.0-RC**](https://www.nuget.org/packages/Microsoft.Azure.Search/3.0.0-rc) ' den daha eski sürümleri de bu REST API sürümlerinden birini hedeflerse devre dışı bırakılacaktır. 

Bu tarihten sonra, kullanımdan kaldırılan REST API veya SDK sürümlerinden herhangi birini kullanan uygulamalar artık çalışmaz ve yükseltilmesi gerekir. Bu türde herhangi bir değişiklik yaparken, 12 aylık bir bildirim sunuyoruz, bu nedenle ayarlamak için yeterli zaman vardır.

Azure Bilişsel Arama 'yi kullanmaya devam etmek için lütfen [REST API](search-api-migration.md) hedefleyen mevcut kodu [REST API 2020-06-30 sürümünü](https://docs.microsoft.com/rest/api/searchservice/) veya daha yeni bir SDK 'yı 15 Ekim 2020 ' e geçirin.  En son sürüme güncelleştirme hakkında sorularınız varsa, azuresearch_contact@microsoft.com kodunuzu güncelleştirmek için yeterli zamana sahip olduğunuzdan emin olmak için lütfen 15 mayıs 2020 ' e kadar posta gönderin.

## <a name="rest-apis"></a>REST API'leri

Azure Bilişsel Arama hizmet örneği, en son sürüm dahil olmak üzere birkaç REST API sürümü destekler. Artık en son bir sürüm olmadığında bir sürümü kullanmaya devam edebilirsiniz, ancak kodunuzu en yeni sürümü kullanmak için [geçirmeniz](search-api-migration.md) önerilir. REST API kullanırken, API sürümü parametresi aracılığıyla her istekte API sürümünü belirtmeniz gerekir. .NET SDK kullanıldığında, kullanmakta olduğunuz SDK sürümü REST API ilgili sürümünü belirler. Daha eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü destekleyecek şekilde yükseltilse bile, bu kodu hiçbir değişiklik yapmadan çalıştırmaya devam edebilirsiniz.

Aşağıdaki tabloda, Arama Hizmeti REST API geçerli ve daha önce yayımlanmış sürümlerinin sürüm geçmişi verilmiştir. Belgeler yalnızca en son kararlı ve önizleme sürümleri için yayımlanır.

### <a name="search-service-apis"></a>Arama Hizmeti API 'Leri

Arama hizmetinde içerik oluşturun ve yönetin.

| Sürüm&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Durum | Açıklama |
|-------------------------|--------|------------------------------|
| [Arama 2020-06-30](/rest/api/searchservice/index)| Dengeli | Arama REST API 'lerinin en yeni kararlı sürümü, ilgi Puanlama ve genellikle bilgi deposu için kullanılabilirlik ile yapılır.|
| [Arama 2020-06-30-Preview](/rest/api/searchservice/index-preview)| Önizleme | Kararlı sürümle ilişkili önizleme sürümü. Birden çok [Önizleme özelliği](search-api-preview.md)içerir. |
| Arama 2019-05-06 | Dengeli  | [Karmaşık türler](search-howto-complex-data-types.md)ekler. |
| Arama 2019-05-06-Preview | Önizleme | Kararlı sürümle ilişkili önizleme sürümü. |
| Arama 2017-11-11 | Dengeli | Becerileri ve [AI zenginleştirme](cognitive-search-concept-intro.md)ekler. |
| Arama 2017-11-11-Önizleme | Önizleme | Kararlı sürümle ilişkili önizleme sürümü. |
| Arama 2016-09-01 |Dengeli | [Dizin oluşturucular](search-indexer-overview.md)ekler. |
| Arama 2016-09-01-Önizleme | Önizleme | Kararlı sürümle ilişkili önizleme sürümü.|
| Arama 2015-02-28 | 10-10-2020 sonrasında desteklenmez   | İlk genel olarak kullanılabilir sürüm.  |
| Arama 2015-02-28-Önizleme | 10-10-2020 sonrasında desteklenmez  | Kararlı sürümle ilişkili önizleme sürümü. |
| Arama 2014-10-20-Önizleme | 10-10-2020 sonrasında desteklenmez | İkinci genel önizleme. |
| Arama 2014-07-31-Önizleme | 10-10-2020 sonrasında desteklenmez  | İlk genel önizleme. |

### <a name="management-rest-apis"></a>Yönetim REST API’leri

Bir arama hizmeti oluşturup yapılandırın ve API anahtarlarını yönetin.

| Sürüm&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Durum | Açıklama |
|-------------------------|--------|------------------------------|
| [Yönetim 2020-08-01](https://docs.microsoft.com/rest/api/searchmanagement/) | Dengeli | Yönetim REST API 'lerinin en yeni kararlı sürümü. Önizleme sürümünde belirtilenler hariç tüm giden erişilen kaynaklar için genel kullanıma açık paylaşılan özel bağlantı kaynağı desteğini ekler |
| [Yönetim 2020-08-01-Önizleme](https://docs.microsoft.com/rest/api/searchmanagement/index-preview) | Önizleme  | Şu anda önizlemede: Azure Işlevleri için paylaşılan özel bağlantı kaynağı ve MySQL için Azure veritabanı desteği. |
| Yönetim 13.03.2020  | Dengeli | Özel bağlantı aracılığıyla özel [uç nokta](service-create-private-endpoint.md) ve yeni hizmetler IÇIN [ağ IP kuralları](service-configure-firewall.md) ekler. Daha fazla bilgi için bu [Swagger belirtimine](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/search/resource-manager/Microsoft.Search/stable/2020-08-01)bakın. |
| Yönetim 2019-10-01-Preview | Önizleme  | Bu listede tanıtılan önizleme özelliği yoktu. Bu önizleme, işlevsel olarak 2020-03-13 ile eşdeğerdir. Daha fazla bilgi için bu [Swagger belirtimine](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/search/resource-manager/Microsoft.Search/preview/2019-10-01-preview)bakın. |
| Yönetim 2015-08-19  | Dengeli | Yönetim REST API 'lerinin ilk genel olarak kullanılabilir sürümü. Hizmet sağlama, ölçek artırma ve API anahtar yönetimi sağlar. Daha fazla bilgi için bu [Swagger belirtimine](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/search/resource-manager/Microsoft.Search/stable/2015-08-19)bakın. |
| Yönetim 2015-08-19-Önizleme  | Önizleme | Yönetim REST API 'lerinin ilk önizleme sürümü. Daha fazla bilgi için bu [Swagger belirtimine](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/search/resource-manager/Microsoft.Search/stable/2015-08-19)bakın. |

## <a name="azure-sdk-for-net"></a>.NET için Azure SDK

Aşağıdaki tabloda, daha güncel SDK sürümlerinin bağlantıları verilmiştir. 

| SDK sürümü | Durum | Açıklama |
|-------------|--------|------------------------------|
| [Azure.Search.Documstaları 11,0](/dotnet/api/overview/azure/search.documents-readme) | Dengeli | Azure .NET SDK 'dan yeni istemci kitaplığı, 2020 Temmuz 'da yayınlandı. Arama REST API-Version = 2020-06-30 REST API, ancak henüz, coğrafi filtreler veya [FieldBuilder](/dotnet/api/microsoft.azure.search.fieldbuilder)'ı desteklemez. |
| [Microsoft. Azure. Search 10,0](https://www.nuget.org/packages/Microsoft.Azure.Search/) | Dengeli | Yayınlanan 2019 Mayıs. Arama REST API-Version = 2019-05-06 ' i hedefler.|
| [Microsoft. Azure. Management. Search 4.0.0](/dotnet/api/overview/azure/search/management) | Dengeli | Yönetim REST API-Version = 2020-08-01 ' i hedefler.  |
| Microsoft. Azure. Management. Search 3.0.0 | Dengeli | Yönetim REST API-Version = 2015-08-19 ' i hedefler.  |

## <a name="azure-sdk-for-java"></a>Java için Azure SDK

| SDK sürümü | Durum | Açıklama  |
|-------------|--------|------------------------------|
| [Java Azure-Arama-belgeler 11](https://newreleases.io/project/github/Azure/azure-sdk-for-java/release/azure-search-documents_11.1.0) | Dengeli | Azure .NET SDK 'dan yeni istemci kitaplığı, 2020 Temmuz 'da yayınlandı. Arama REST API-Version = 2019-05-06 ' i hedefler. |
| [Java yönetim Istemcisi 1.35.0](/java/api/overview/azure/search/management) | Dengeli | Yönetim REST API-Version = 2015-08-19 ' i hedefler. |

## <a name="azure-sdk-for-javascript"></a>JavaScript için Azure SDK

| SDK sürümü | Durum | Açıklama  |
|-------------|--------|------------------------------|
| [JavaScript Azure-Arama 11,0](https://azure.github.io/azure-sdk-for-node/azure-search/latest/) | Dengeli | Azure .NET SDK 'dan yeni istemci kitaplığı, 2020 Temmuz 'da yayınlandı. Arama REST API-Version = 2016-09-01 ' i hedefler. |
| [JavaScript Azure-ARM-arama](https://azure.github.io/azure-sdk-for-node/azure-arm-search/latest/) | Dengeli | Yönetim REST API-Version = 2015-08-19 ' i hedefler. |

## <a name="azure-sdk-for-python"></a>Python için Azure SDK

| SDK sürümü | Durum | Açıklama  |
|-------------|--------|------------------------------|
| [Python Azure-Arama-belgeler 11,0](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-search-documents/11.0.0/index.html) | Dengeli | Azure .NET SDK 'dan yeni istemci kitaplığı, 2020 Temmuz 'da yayınlandı. Arama REST API-Version = 2019-05-06 ' i hedefler. |
| [Python Azure-MGMT-arama 1,0](/python/api/overview/azure/search) | Dengeli | Yönetim REST API-Version = 2015-08-19 ' i hedefler. |