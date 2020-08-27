---
title: Azure Search .NET Yönetim SDK 'sına yükseltin
titleSuffix: Azure Cognitive Search
description: Önceki sürümlerden Azure Search .NET Yönetim SDK 'sına yükseltin. Yeni özellikler ve geçiş için gereken kod değişiklikleri hakkında bilgi edinin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 07/08/2020
ms.openlocfilehash: 8648347eb48081389cf360fa949b31bbd0b8c71e
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88936716"
---
# <a name="upgrading-versions-of-the-azure-search-net-management-sdk"></a>Azure Search .NET Yönetim SDK 'sının sürümlerini yükseltme

Bu makalede, arama hizmetlerinin sağlanması veya sağlanması, kapasiteyi ayarlamak ve API anahtarlarını yönetmek için kullanılan Azure Search .NET Yönetim SDK 'sının birbirini izleyen sürümlerine nasıl geçiş yapılacağı açıklanır.

Yönetim SDK 'Ları, yönetim REST API 'nin belirli bir sürümünü hedeflemelidir. Kavramlar ve işlemler hakkında daha fazla bilgi için bkz. [Arama Yönetimi (REST)](/rest/api/searchmanagement/).

## <a name="versions"></a>Sürümler

| SDK sürümü | Karşılık gelen REST API sürümü | Özellik ekleme veya davranış değişikliği |
|-------------|--------------------------------|-------------------------------------|
| [3.0](https://www.nuget.org/packages/Microsoft.Azure.Management.Search/3.0.0) | api-Version = 2020-30-20 | Uç nokta güvenliği ekler (IP güvenlik duvarları ve [Azure özel bağlantısı](../private-link/private-endpoint-overview.md)ile tümleştirme) |
| [2.0](https://www.nuget.org/packages/Microsoft.Azure.Management.Search/2.0.0) | api-Version = 2019-10-01 | Kullanılabilirlik geliştirmeleri. [Liste sorgu anahtarlarında](/rest/api/searchmanagement/querykeys/listbysearchservice) Son değişiklik (Get kullanımdan kaldırılmıştır). |
| [1.0](https://www.nuget.org/packages/Microsoft.Azure.Management.Search/1.0.1) | api-Version = 2015-08-19  | İlk sürüm |

## <a name="how-to-upgrade"></a>Yükseltme

1. NuGet `Microsoft.Azure.Management.Search` , NuGet Paket Yöneticisi konsolunu veya proje başvurularınızı sağ tıklayıp "NuGet Paketlerini Yönet..." seçeneğini belirleyerek Için NuGet başvurunuz ' ı güncelleştirin. Visual Studio 'da.

1. NuGet yeni paketleri ve bağımlılıklarını indirdikten sonra projenizi yeniden derleyin. Kodunuzun nasıl yapılandırıldığına bağlı olarak, başarıyla yeniden oluşturulabilir ve bu durumda işiniz yapılır.

1. Derlemeniz başarısız olursa, bunun nedeni bazı SDK arabirimlerini (örneğin, birim testi amaçları için) uygulamış olmanız olabilir. Bunu çözmek için gibi daha yeni yöntemler uygulamanız gerekir `BeginCreateOrUpdateWithHttpMessagesAsync` .

1. Herhangi bir derleme hatasını düzelttikten sonra, yeni işlevlerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz. 

## <a name="upgrade-to-30"></a>3,0 sürümüne yükselt

Sürüm 3,0, IP aralıklarına erişimi kısıtlayarak ve isteğe bağlı olarak, genel İnternet 'te görünmemelidir olması gereken arama hizmetleri için Azure özel bağlantısıyla tümleştirerek özel uç nokta koruması ekler.

### <a name="new-apis"></a>Yeni API'ler

| API | Kategori| Ayrıntılar |
|-----|--------|------------------|
| [NetworkRuleSet](/rest/api/searchmanagement/services/createorupdate#networkruleset) | IP güvenlik duvarı | Bir hizmet uç noktasına erişimi izin verilen IP adresleri listesine kısıtlama. Bkz. kavramlar ve Portal yönergeleri için [IP güvenlik duvarını yapılandırma](service-configure-firewall.md) . |
| [Paylaşılan özel bağlantı kaynağı](/rest/api/searchmanagement/sharedprivatelinkresources) | Özel Bağlantı | Bir arama hizmeti tarafından kullanılacak paylaşılan bir özel bağlantı kaynağı oluşturun.  |
| [Özel uç nokta bağlantıları](/rest/api/searchmanagement/privateendpointconnections) | Özel Bağlantı | Özel uç nokta aracılığıyla bir arama hizmetine bağlantılar oluşturun ve yönetin. Kavramlar ve Portal yönergeleri için [Özel uç nokta oluşturma](service-create-private-endpoint.md) konusuna bakın.|
| [Özel bağlantı kaynakları](/rest/api/searchmanagement/privatelinkresources/) | Özel Bağlantı | Özel bir uç noktası bağlantısına sahip bir arama hizmeti için, aynı sanal ağda kullanılan tüm hizmetlerin bir listesini alın. Arama Çözümünüz Azure veri kaynaklarından (Azure Storage, Cosmos DB, Azure SQL) çekenler veya bilişsel hizmetler veya Key Vault kullanıyorsa, bu kaynakların tümünde sanal ağda uç noktalar olmalıdır ve bu API bir liste döndürmelidir. |
| [PublicNetworkAccess](/rest/api/searchmanagement/services/createorupdate#publicnetworkaccess)| Özel Bağlantı | Bu, oluşturma veya güncelleştirme hizmeti istekleri üzerindeki bir özelliktir. Devre dışı bırakıldığında, özel bağlantı tek erişim modülüdir. |

### <a name="breaking-changes"></a>Yeni değişiklikler

Artık bir [liste sorgu anahtarları](/rest/api/searchmanagement/querykeys/listbysearchservice) ISTEğI için Al ' i kullanamazsınız. Önceki sürümlerde, bu yayında Al veya postala ' yı kullanabilirsiniz. bu sürümde, tüm yayınlar ileri taşınır, yalnızca GÖNDERI desteklenir. 

## <a name="upgrade-to-20"></a>2,0 sürümüne yükselt

Azure Search .NET Yönetim SDK 'sının 2. sürümü küçük bir yükseltmedir, bu nedenle kodunuzun değiştirilmesi yalnızca en az çaba gerektirir. SDK 'nın üzerinde yapılan değişiklikler, SDK 'nın kullanılabilirliğini geliştirmek için kesinlikle istemci tarafı değişikliklerdir. Bu değişiklikler şunları içerir:

* `Services.CreateOrUpdate` ve zaman uyumsuz sürümleri artık sağlamayı otomatik olarak `SearchService` yoklamıştır ve hizmet sağlama tamamlanana kadar geri dönmez. Bu, sizi bir tür yoklama kodu yazmak zorunda kalmanızı sağlar.

* Hizmet sağlamayı hala el ile yoklamak istiyorsanız yeni `Services.BeginCreateOrUpdate` yöntemini veya zaman uyumsuz sürümlerinden birini kullanabilirsiniz.

* `Services.Update`SDK 'ya yeni yöntemler ve zaman uyumsuz sürümler eklenmiştir. Bu yöntemler, bir hizmetin artımlı güncelleştirilmesini desteklemek için HTTP PATCH kullanır. Örneğin, artık `SearchService` yalnızca istenen ve özellikleri içeren bu yöntemlere bir örnek geçirerek bir hizmeti ölçeklendirebilirsiniz `partitionCount` `replicaCount` . Çağırma `Services.Get` , döndürülme ve geçirme gibi eski Yöntem `SearchService` `Services.CreateOrUpdate` desteklenmeye devam eder, ancak artık gerekli değildir. 

## <a name="next-steps"></a>Sonraki adımlar

Sorunlarla karşılaşırsanız, soruların nakledilmesi için en iyi forum [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-cognitive-search?tab=Newest). Bir hata bulursanız, [Azure .NET SDK GitHub deposunda](https://github.com/Azure/azure-sdk-for-net/issues)bir sorun oluşturabilirsiniz. Sorun başlığınız "[Search]" ile etiketlediğinizden emin olun.