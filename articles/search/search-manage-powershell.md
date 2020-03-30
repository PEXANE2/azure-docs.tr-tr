---
title: Az.Search modüllerini kullanarak PowerShell komut dosyaları
titleSuffix: Azure Cognitive Search
description: PowerShell ile bir Azure Bilişsel Arama hizmeti oluşturun ve yapılandırıldı. Bir hizmeti yukarı veya aşağı ölçeklendirebilir, yönetici ve sorgu api anahtarlarını yönetebilir ve sistem bilgilerini sorgulayabilirsiniz.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: 711071e08a52a0075512bc8b3ffe14707238cdfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77209305"
---
# <a name="manage-your-azure-cognitive-search-service-with-powershell"></a>PowerShell ile Azure Bilişsel Arama hizmetinizi yönetin
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [Powershell](search-manage-powershell.md)
> * [REST API](https://docs.microsoft.com/rest/api/searchmanagement/)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure Bilişsel Arama'yı oluşturmak ve yapılandırmak için Windows, Linux veya [Azure Cloud Shell'de](https://docs.microsoft.com/azure/cloud-shell/overview) PowerShell cmdlets ve komut dosyaları çalıştırabilirsiniz. **Az.Search** [modülü, Azure PowerShell'i](https://docs.microsoft.com/powershell/) tam pariteyle [Arama Yönetimi REST API'lerine](https://docs.microsoft.com/rest/api/searchmanagement) ve aşağıdaki görevleri gerçekleştirebilme özelliğine genişletir:

> [!div class="checklist"]
> * [Abonelikteki arama hizmetlerini listele](#list-search-services)
> * [İade hizmeti bilgileri](#get-search-service-information)
> * [Bir hizmet oluşturma veya silme](#create-or-delete-a-service)
> * [Yönetici API anahtarlarını yeniden oluşturma](#regenerate-admin-keys)
> * [Sorgu api anahtarları oluşturma veya silme](#create-or-delete-query-keys)
> * [Yinelemeler ve bölümlerle yukarı veya aşağı ölçeklendirme](#scale-replicas-and-partitions)

Bazen, yukarıdaki listede *olmayan* görevler hakkında sorular sorulur. Şu anda, bir sunucu adını, bölgesini veya katmanını değiştirmek için **Az.Search** modülünü veya yönetim REST API'sini kullanamazsınız. Bir hizmet oluşturulduğunda ayrılmış kaynaklar ayrılır. Bu nedenle, temel donanımı (konum veya düğüm türü) değiştirmek için yeni bir hizmet gerekir. Benzer şekilde, bir hizmetten diğerine içerik aktarmak için hiçbir araç veya API yoktur.

Bir hizmet içinde, içerik oluşturma ve yönetimi [Arama Hizmeti REST API](https://docs.microsoft.com/rest/api/searchservice/) veya [.NET SDK](https://docs.microsoft.com/dotnet/api/?term=microsoft.azure.search)geçer. İçerik için özel PowerShell komutları olmamasına gerek yok, dizinoluşturmak ve yüklemek için REST veya .NET API'lerini çağıran PowerShell komut dosyası yazabilirsiniz.

<a name="check-versions-and-load"></a>

## <a name="check-versions-and-load-modules"></a>Sürümleri ve yükleme modüllerini kontrol edin

Bu makaledeki örnekler etkileşimlidir ve yüksek izinler gerektirir. Azure PowerShell **(Az** modülü) yüklenmelidir. Daha fazla bilgi için Azure [PowerShell'i Yükle'ye](/powershell/azure/overview)bakın.

### <a name="powershell-version-check-51-or-later"></a>PowerShell sürüm denetimi (5.1 veya sonrası)

Local PowerShell desteklenen herhangi bir işletim sisteminde 5.1 veya daha sonra olmalıdır.

```azurepowershell-interactive
$PSVersionTable.PSVersion
```

### <a name="load-azure-powershell"></a>Azure PowerShell'i Yükleyin

**Az** yüklü olup olmadığından emin değilseniz, doğrulama adımı olarak aşağıdaki komutu çalıştırın. 

```azurepowershell-interactive
Get-InstalledModule -Name Az
```

Bazı sistemler modülleri otomatik yüklemez. Önceki komutta bir hata alırsanız, modülü yüklemeyi deneyin ve bu başarısız olursa, bir adımı kaçırıp kaçırmadığınızı görmek için yükleme yönergelerine geri dön.

```azurepowershell-interactive
Import-Module -Name Az
```

### <a name="connect-to-azure-with-a-browser-sign-in-token"></a>Tarayıcı oturum açma belirteciyle Azure'a bağlanma

PowerShell'deki bir aboneye bağlanmak için portal oturum açma kimlik bilgilerinizi kullanabilirsiniz. Alternatif [olarak, bir hizmet müdürü ile etkileşimli olmayan bir şekilde kimlik doğrulaması](../active-directory/develop/howto-authenticate-service-principal-powershell.md)yapabilirsiniz.

```azurepowershell-interactive
Connect-AzAccount
```

Birden çok Azure aboneliğiniz varsa, Azure aboneliğinizi ayarlayın. Geçerli aboneliklerinizin listesini görmek için bu komutu çalıştırın.

```azurepowershell-interactive
Get-AzSubscription | sort SubscriptionName | Select SubscriptionName
```

Aboneliği belirtmek için aşağıdaki komutu çalıştırın. Aşağıdaki örnekte, abonelik `ContosoSubscription`adı.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName ContosoSubscription
```

<a name="list-search-services"></a>

## <a name="list-services-in-a-subscription"></a>Abonelikteki hizmetleri listele

Aşağıdaki komutlar [**Az.Resources'tan,**](https://docs.microsoft.com/powershell/module/az.resources/?view=azps-1.4.0#resources)aboneliğinizde zaten sağlanan varolan kaynaklar ve hizmetler hakkında bilgi döndürerek edinilir. Zaten kaç arama hizmeti oluşturulduğunu bilmiyorsanız, bu komutlar bu bilgileri döndürecek ve portala bir yolculuk kaydedebilirsiniz.

İlk komut tüm arama hizmetlerini döndürür.

```azurepowershell-interactive
Get-AzResource -ResourceType Microsoft.Search/searchServices | ft
```

Hizmetler listesinden, belirli bir kaynak la ilgili bilgileri iade edin.

```azurepowershell-interactive
Get-AzResource -ResourceName <service-name>
```

Sonuçlar aşağıdaki çıktıya benzer olmalıdır.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : westus
ResourceId        : /subscriptions/<alpha-numeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="import-azsearch"></a>Az.Search'u İthalat

[**Az.Search**](https://docs.microsoft.com/powershell/module/az.search/?view=azps-1.4.0#search) komutları, modülü yükleyene kadar kullanılamaz.

```azurepowershell-interactive
Install-Module -Name Az.Search
```

### <a name="list-all-azsearch-commands"></a>Tüm Az.Search komutlarını listele

Doğrulama adımı olarak, modülde sağlanan komutların listesini döndürün.

```azurepowershell-interactive
Get-Command -Module Az.Search
```

Sonuçlar aşağıdaki çıktıya benzer olmalıdır.

```
CommandType     Name                                Version    Source
-----------     ----                                -------    ------
Cmdlet          Get-AzSearchAdminKeyPair            0.7.1      Az.Search
Cmdlet          Get-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          Get-AzSearchService                 0.7.1      Az.Search
Cmdlet          New-AzSearchAdminKey                0.7.1      Az.Search
Cmdlet          New-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          New-AzSearchService                 0.7.1      Az.Search
Cmdlet          Remove-AzSearchQueryKey             0.7.1      Az.Search
Cmdlet          Remove-AzSearchService              0.7.1      Az.Search
Cmdlet          Set-AzSearchService                 0.7.1      Az.Search
```

## <a name="get-search-service-information"></a>Arama hizmeti bilgilerini alın

**Az.Search** alındıktan ve arama hizmetinizi içeren kaynak grubunu biliyor olduktan sonra, ad, bölge, katman ve yineleme ve bölüm sayıları da dahil olmak üzere hizmet tanımını döndürmek için [Get-AzSearchService'i](https://docs.microsoft.com/powershell/module/az.search/get-azsearchservice?view=azps-1.4.0) çalıştırın.

```azurepowershell-interactive
Get-AzSearchService -ResourceGroupName <resource-group-name>
```

Sonuçlar aşağıdaki çıktıya benzer olmalıdır.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : West US
Sku               : Standard
ReplicaCount      : 1
PartitionCount    : 1
HostingMode       : Default
ResourceId        : /subscriptions/<alphanumeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="create-or-delete-a-service"></a>Bir hizmet oluşturma veya silme

[**Yeni-AzSearchService**](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) yeni [bir arama hizmeti oluşturmak](search-create-service-portal.md)için kullanılır.

```azurepowershell-interactive
New-AzSearchService -ResourceGroupName "demo-westus" -Name "my-demo-searchapp" -Sku "Standard" -Location "West US" -PartitionCount 3 -ReplicaCount 3
``` 
Sonuçlar aşağıdaki çıktıya benzer olmalıdır.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Id                : /subscriptions/<alphanumeric-subscription-ID>/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 3
PartitionCount    : 3
HostingMode       : Default
Tags
```     

## <a name="regenerate-admin-keys"></a>Yönetici anahtarlarını yeniden oluşturma

[**Yeni-AzSearchAdminKey**](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) admin [API tuşları](search-security-api-keys.md)üzerinde rulo kullanılır. Kimlik doğrulaması erişim için her hizmette iki yönetici anahtarı oluşturulur. Anahtarlar her istek te gereklidir. Her iki yönetici anahtarı da işlevsel olarak eşdeğerdir ve herhangi bir bilgi alma veya herhangi bir nesne oluşturma ve silme olanağı olan bir arama hizmetine tam yazma erişimi sağlar. Diğerini değiştirirken birini kullanabilmeniz için iki anahtar vardır. 

Yalnızca bir seferde, `primary` anahtar veya anahtar `secondary` olarak belirtilen yeniden oluşturabilirsiniz. Kesintisiz hizmet için, birincil anahtarı niçin yuvarlanırken ikincil bir anahtar kullanmak için tüm istemci kodunu güncelleştirmeyi unutmayın. Operasyonlar uçuş sırasında anahtarları değiştirmekten kaçının.

Tahmin edebileceğiniz gibi, istemci kodunu güncelleştirmeden anahtarları yeniden oluşturursanız, eski anahtarı kullanarak istekleri başarısız olur. Tüm yeni anahtarları yeniden oluşturmak sizi kalıcı olarak hizmet dışı etmez ve hizmete portal üzerinden erişmeye devam edebilirsiniz. Birincil ve ikincil anahtarları yeniden oluşturduktan sonra, yeni anahtarları kullanmak için istemci kodunu güncelleştirebilirsiniz ve işlemler buna göre devam edecektir.

API anahtarları için değerler hizmet tarafından oluşturulur. Azure Bilişsel Arama'nın kullanması için özel bir anahtar sağlayamazsınız. Benzer şekilde, yönetici API anahtarları için kullanıcı tanımlı bir ad yoktur. Anahtara yapılan başvurular sabit dizeleri veya `primary` `secondary`. 

```azurepowershell-interactive
New-AzSearchAdminKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -KeyKind Primary
```

Sonuçlar aşağıdaki çıktıya benzer olmalıdır. Her seferinde yalnızca bir tane değiştirseniz bile her iki anahtar da döndürülür.

```
Primary                    Secondary
-------                    ---------
<alphanumeric-guid>        <alphanumeric-guid>  
```

## <a name="create-or-delete-query-keys"></a>Sorgu anahtarları oluşturma veya silme

[**New-AzSearchQueryKey,**](https://docs.microsoft.com/powershell/module/az.search/new-azsearchquerykey?view=azps-1.4.0) istemci uygulamalarından Azure Bilişsel Arama dizinine salt okunur erişim için sorgu [API anahtarları](search-security-api-keys.md) oluşturmak için kullanılır. Sorgu anahtarları, arama sonuçlarını almak amacıyla belirli bir dizine kimlik doğrulamak için kullanılır. Sorgu anahtarları, hizmetteki dizin, veri kaynağı veya dizin leyici gibi diğer öğelere salt okunur erişim izni vermez.

Azure Bilişsel Arama'nın kullanması için bir anahtar sağlayamazsınız. API anahtarları hizmet tarafından oluşturulur.

```azurepowershell-interactive
New-AzSearchQueryKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -Name <query-key-name> 
```

## <a name="scale-replicas-and-partitions"></a>Yinelemeleri ve bölümleri ölçeklendir

[**Set-AzSearchService,**](https://docs.microsoft.com/powershell/module/az.search/set-azsearchservice?view=azps-1.4.0) hizmetinizdeki faturalandırılabilir kaynakları ayarlamak için [yinelemeleri ve bölümleri artırmak veya azaltmak](search-capacity-planning.md) için kullanılır. Yinelemeleri veya bölümleri artırmak, hem sabit hem de değişken ücretleri olan faturanıza ekler. Ek işlem gücüne geçici bir gereksiniminiz varsa, iş yükünü işlemek için yinelemeleri ve bölümleri artırabilirsiniz. Genel Bakış portalı sayfasındaki izleme alanında sorgu gecikmesi, saniyedeki sorgular ve azaltma, geçerli kapasitenin yeterli olup olmadığını gösteren kutucuklar bulunur.

Kaynak eklemesi veya kaldırması biraz zaman alabilir. Kapasite ayarlamaları arka planda oluşur ve varolan iş yüklerinin devam etmesine olanak tanır. Ek kapasite, hazır olur olmaz gelen istekler için kullanılır ve ek yapılandırma gerekmez. 

Kapasitenin kaldırılması rahatsız edici olabilir. Kapasiteyi azaltmadan önce tüm dizin oluşturma ve dizinleyici işlerin durdurulması, bırakılan istekleri önlemek için önerilir. Bu mümkün değilse, yeni hedef düzeylerinize ulaşılıncaya kadar kapasiteyi kademeli olarak, bir yineleme ve bölünmeyi azaltabilirsiniz.

Komutu gönderdikten sonra, komutu yarı yolda sonlandırmanın bir yolu yoktur. Sayımları gözden geçirmeden önce komutun tamamlanmasını beklemeniz gerekir.

```azurepowershell-interactive
Set-AzSearchService -ResourceGroupName <resource-group-name> -Name <search-service-name> -PartitionCount 6 -ReplicaCount 6
```

Sonuçlar aşağıdaki çıktıya benzer olmalıdır.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 6
PartitionCount    : 6
HostingMode       : Default
Id                : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="next-steps"></a>Sonraki adımlar

Portal, REST API'leri veya .NET SDK'yı kullanarak bir [dizin](search-what-is-an-index.md)oluşturun, [dizin sorgula.](search-query-overview.md)

* [Azure portalında Azure Bilişsel Arama dizini oluşturma](search-create-index-portal.md)
* [Diğer hizmetlerden veri yüklemek için bir dizin leyici ayarlama](search-indexer-overview.md)
* [Azure portalında Arama gezginini kullanarak bir Azure Bilişsel Arama dizinini sorgula](search-explorer.md)
* [.NET'te Azure Bilişsel Arama nasıl kullanılır?](search-howto-dotnet-sdk.md)