---
title: Az. Search modülünü kullanan PowerShell betikleri
titleSuffix: Azure Cognitive Search
description: PowerShell ile bir Azure Bilişsel Arama hizmeti oluşturun ve yapılandırın. Bir hizmeti yukarı veya aşağı ölçeklendirebilir, yönetici ve sorgu api anahtarlarını yönetebilir ve sistem bilgileri için sorgu yapabilirsiniz.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: 711071e08a52a0075512bc8b3ffe14707238cdfe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77209305"
---
# <a name="manage-your-azure-cognitive-search-service-with-powershell"></a>Azure Bilişsel Arama hizmetinizi PowerShell ile yönetme
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [REST API](https://docs.microsoft.com/rest/api/searchmanagement/)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure Bilişsel Arama oluşturmak ve yapılandırmak için Windows, Linux veya [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) 'de PowerShell cmdlet 'leri ve betikleri çalıştırabilirsiniz. **Az. Search** modülü, [arama yönetimi REST API 'lerine](https://docs.microsoft.com/rest/api/searchmanagement) tam eşlik ile [Azure PowerShell](https://docs.microsoft.com/powershell/) genişletir ve aşağıdaki görevleri gerçekleştirebilir:

> [!div class="checklist"]
> * [Bir abonelikte arama hizmetlerini listeleme](#list-search-services)
> * [Hizmet bilgilerini döndür](#get-search-service-information)
> * [Hizmet oluşturma veya silme](#create-or-delete-a-service)
> * [Yönetici API anahtarlarını yeniden oluştur](#regenerate-admin-keys)
> * [Sorgu API 'si oluşturma veya silme](#create-or-delete-query-keys)
> * [Çoğaltmalar ve bölümlerle ölçeği artırma veya azaltma](#scale-replicas-and-partitions)

Bazen, yukarıdaki listede *olmayan* görevlerle ilgili soruların sorulması istenir. Şu anda, sunucu adını, bölgeyi veya katmanını değiştirmek için **az. Search** modülünü ya da yönetim REST API kullanamazsınız. Adanmış kaynaklar bir hizmet oluşturulduğunda ayrılır. Bu nedenle, temel alınan donanımın (konum veya düğüm türü) değiştirilmesi için yeni bir hizmet gerekir. Benzer şekilde, bir hizmetten diğerine bir dizin gibi içerik aktarmaya yönelik bir araç veya API yoktur.

Bir hizmette, içerik oluşturma ve Yönetim [Arama Hizmeti REST API](https://docs.microsoft.com/rest/api/searchservice/) veya [.NET SDK](https://docs.microsoft.com/dotnet/api/?term=microsoft.azure.search)ile yapılır. İçerik için adanmış bir PowerShell komutu bulunmadığından, dizinler oluşturmak ve yüklemek için REST veya .NET API 'Leri çağıran PowerShell betiği yazabilirsiniz.

<a name="check-versions-and-load"></a>

## <a name="check-versions-and-load-modules"></a>Sürümleri denetleme ve modülleri yükleme

Bu makaledeki örnekler etkileşimlidir ve yükseltilmiş izinler gerektirir. Azure PowerShell ( **az** Module) yüklü olmalıdır. Daha fazla bilgi için bkz. [ınstall Azure PowerShell](/powershell/azure/overview).

### <a name="powershell-version-check-51-or-later"></a>PowerShell sürüm denetimi (5,1 veya üzeri)

Yerel PowerShell, desteklenen herhangi bir işletim sisteminde 5,1 veya üzeri olmalıdır.

```azurepowershell-interactive
$PSVersionTable.PSVersion
```

### <a name="load-azure-powershell"></a>Azure PowerShell yükle

**Az** yüklü olup olmadığından emin değilseniz, doğrulama adımı olarak aşağıdaki komutu çalıştırın. 

```azurepowershell-interactive
Get-InstalledModule -Name Az
```

Bazı sistemler modülleri otomatik olarak yüklemez. Önceki komutta bir hata alırsanız, modülü yüklemeyi deneyin ve başarısız olursa, bir adım kaçırdığınızı görmek için yükleme yönergelerine geri dönün.

```azurepowershell-interactive
Import-Module -Name Az
```

### <a name="connect-to-azure-with-a-browser-sign-in-token"></a>Tarayıcı oturum açma belirteciyle Azure 'a bağlanma

PowerShell 'deki bir aboneliğe bağlanmak için Portal oturum açma kimlik bilgilerinizi kullanabilirsiniz. Alternatif olarak, [hizmet sorumlusu ile etkileşimli olmayan kimlik doğrulaması](../active-directory/develop/howto-authenticate-service-principal-powershell.md)yapabilirsiniz.

```azurepowershell-interactive
Connect-AzAccount
```

Birden çok Azure aboneliği tutarsanız, Azure aboneliğinizi ayarlayın. Geçerli aboneliklerinizin listesini görmek için bu komutu çalıştırın.

```azurepowershell-interactive
Get-AzSubscription | sort SubscriptionName | Select SubscriptionName
```

Aboneliği belirtmek için aşağıdaki komutu çalıştırın. Aşağıdaki örnekte, abonelik adı `ContosoSubscription` .

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName ContosoSubscription
```

<a name="list-search-services"></a>

## <a name="list-services-in-a-subscription"></a>Bir abonelikteki hizmetleri listeleme

Aşağıdaki komutlar [**az. resources**](https://docs.microsoft.com/powershell/module/az.resources/?view=azps-1.4.0#resources), aboneliğinizde zaten sağlanmış olan mevcut kaynaklar ve hizmetler hakkında bilgi döndürüyor. Kaç tane arama hizmeti oluşturulduğunu bilmiyorsanız, bu komutlar bu bilgileri döndürür ve portala seyahat ister.

İlk komut tüm arama hizmetlerini döndürür.

```azurepowershell-interactive
Get-AzResource -ResourceType Microsoft.Search/searchServices | ft
```

Hizmetler listesinden belirli bir kaynakla ilgili bilgileri döndürür.

```azurepowershell-interactive
Get-AzResource -ResourceName <service-name>
```

Sonuçlar aşağıdaki çıktıya benzer görünmelidir.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : westus
ResourceId        : /subscriptions/<alpha-numeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="import-azsearch"></a>Al az. Search

[**Az. Search**](https://docs.microsoft.com/powershell/module/az.search/?view=azps-1.4.0#search) komutları, modülü yüklenene kadar kullanılamaz.

```azurepowershell-interactive
Install-Module -Name Az.Search
```

### <a name="list-all-azsearch-commands"></a>Tüm az. Search komutlarını listeleyin

Doğrulama adımı olarak, modülde sunulan komutların bir listesini döndürün.

```azurepowershell-interactive
Get-Command -Module Az.Search
```

Sonuçlar aşağıdaki çıktıya benzer görünmelidir.

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

## <a name="get-search-service-information"></a>Arama hizmeti bilgilerini al

**Az. Search** içeri aktarıldıktan sonra ve arama hizmetinizi içeren kaynak grubunu öğrendikten sonra, ad, bölge, katman, çoğaltma ve bölüm sayıları dahil olmak üzere hizmet tanımını döndürmek için [Get-azsearchservice](https://docs.microsoft.com/powershell/module/az.search/get-azsearchservice?view=azps-1.4.0) komutunu çalıştırın.

```azurepowershell-interactive
Get-AzSearchService -ResourceGroupName <resource-group-name>
```

Sonuçlar aşağıdaki çıktıya benzer görünmelidir.

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

## <a name="create-or-delete-a-service"></a>Hizmet oluşturma veya silme

New [**-AzSearchService**](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) , [Yeni bir arama hizmeti oluşturmak](search-create-service-portal.md)için kullanılır.

```azurepowershell-interactive
New-AzSearchService -ResourceGroupName "demo-westus" -Name "my-demo-searchapp" -Sku "Standard" -Location "West US" -PartitionCount 3 -ReplicaCount 3
``` 
Sonuçlar aşağıdaki çıktıya benzer görünmelidir.

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

## <a name="regenerate-admin-keys"></a>Yönetici anahtarlarını yeniden oluştur

[**New-AzSearchAdminKey**](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) , yönetici [API anahtarlarını](search-security-api-keys.md)almak için kullanılır. Kimliği doğrulanmış erişim için her bir hizmetle iki yönetici anahtarı oluşturulur. Her istekte anahtarlar gereklidir. Her iki yönetici anahtarı da işlevsel olarak eşdeğerdir, bir arama hizmetine tüm bilgileri alma veya herhangi bir nesne oluşturma ve silme imkanına tam yazma erişimi veriliyor. Diğerini değiştirirken bir tane kullanabilmeniz için iki anahtar mevcuttur. 

Ya da anahtarı olarak belirtilen tek seferde yalnızca bir tane oluşturabilirsiniz `primary` `secondary` . Kesintisiz hizmet için, birincil anahtar üzerinde çalışırken tüm istemci kodlarını ikincil anahtar kullanacak şekilde güncelleştirmeyi unutmayın. İşlemler uçuşdayken anahtarları değiştirmekten kaçının.

Beklenirken, istemci kodunu güncelleştirmeden anahtarları yeniden oluşturursanız, eski anahtarı kullanan istekler başarısız olur. Tüm yeni anahtarların yeniden oluşturulması, hizmetinizi kalıcı olarak kilitlemez ve Portal üzerinden hizmete erişmeye devam edebilirsiniz. Birincil ve ikincil anahtarları yeniden oluşturduktan sonra, yeni anahtarları kullanmak için istemci kodunu güncelleştirebilirsiniz ve işlemler buna uygun şekilde sürdürülür.

API anahtarlarının değerleri hizmet tarafından oluşturulur. Azure Bilişsel Arama kullanılmak üzere özel bir anahtar sağlayamezsiniz. Benzer şekilde, yönetici API anahtarları için Kullanıcı tanımlı ad yoktur. Anahtara yapılan başvurular veya ya da sabit dizelerdir `primary` `secondary` . 

```azurepowershell-interactive
New-AzSearchAdminKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -KeyKind Primary
```

Sonuçlar aşağıdaki çıktıya benzer görünmelidir. Her iki anahtar de tek seferde yalnızca birini değiştirdiğiniz halde döndürülür.

```
Primary                    Secondary
-------                    ---------
<alphanumeric-guid>        <alphanumeric-guid>  
```

## <a name="create-or-delete-query-keys"></a>Sorgu anahtarları oluşturma veya silme

[**New-AzSearchQueryKey**](https://docs.microsoft.com/powershell/module/az.search/new-azsearchquerykey?view=azps-1.4.0) , istemci uygulamalarından bir Azure bilişsel arama dizinine salt okuma erişimi Için sorgu [API anahtarları](search-security-api-keys.md) oluşturmak üzere kullanılır. Sorgu anahtarları, arama sonuçlarının alınması amacıyla belirli bir dizin için kimlik doğrulaması yapmak üzere kullanılır. Sorgu anahtarları, hizmette, dizin, veri kaynağı veya Dizin Oluşturucu gibi diğer öğelere salt okuma erişimi vermez.

Azure Bilişsel Arama kullanılmak üzere bir anahtar sağlayamezsiniz. API anahtarları hizmet tarafından oluşturulur.

```azurepowershell-interactive
New-AzSearchQueryKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -Name <query-key-name> 
```

## <a name="scale-replicas-and-partitions"></a>Çoğaltmaları ve bölümleri ölçeklendirme

[**Set-AzSearchService**](https://docs.microsoft.com/powershell/module/az.search/set-azsearchservice?view=azps-1.4.0) , hizmet içinde yalnızca faturalanabilir kaynakları ayarlamak için [çoğaltmaları ve bölümleri artırmak veya azaltmak](search-capacity-planning.md) için kullanılır. Çoğaltmaları veya bölümleri, hem sabit hem de değişken ücretlerine sahip olan faturanızda artırır. Ek işlem gücü için geçici bir ihtiyacınız varsa, iş yükünü işlemek için çoğaltmaları ve bölümleri artırabilirsiniz. Genel Bakış portalı sayfasındaki izleme alanında sorgu gecikmesi üzerinde Kutucuklar, saniye başına sorgu ve kısıtlama, geçerli kapasitenin yeterli olup olmadığını gösterir.

Kaynak kaynağı ekleme veya kaldırma biraz zaman alabilir. Kapasite ayarlamaları arka planda gerçekleşir ve mevcut iş yüklerinin devam etmesine izin verir. Daha fazla yapılandırma gerekmeden, gelen istekler için ek kapasite kullanılır. 

Kapasitenin kaldırılması kesintiye uğratan olabilir. Atlanan isteklerin olmaması için kapasiteyi azaltmadan önce tüm dizin oluşturma ve Dizin Oluşturucu işlerinin durdurulması önerilir. Bu uygun değilse, yeni hedef düzeyleriniz ulaşılana kadar kapasiteyi artımlı olarak, bir çoğaltma ve bölüm olarak azaltmayı düşünebilirsiniz.

Komutu gönderdikten sonra, bu işlemi geçişli olarak sonlandırmak için bir yol yoktur. Sayıları yeniden ayarlamadan önce komutun bitmesini beklemeniz gerekir.

```azurepowershell-interactive
Set-AzSearchService -ResourceGroupName <resource-group-name> -Name <search-service-name> -PartitionCount 6 -ReplicaCount 6
```

Sonuçlar aşağıdaki çıktıya benzer görünmelidir.

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

Bir [Dizin](search-what-is-an-index.md)oluşturun, PORTALı, REST API 'leri veya .NET SDK 'sını kullanarak [bir dizini sorgulayın](search-query-overview.md) .

* [Azure portal Azure Bilişsel Arama dizini oluşturma](search-create-index-portal.md)
* [Diğer hizmetlerden veri yüklemek için Dizin Oluşturucu ayarlama](search-indexer-overview.md)
* [Azure portal arama Gezgini 'ni kullanarak bir Azure Bilişsel Arama dizinini sorgulama](search-explorer.md)
* [.NET Bilişsel Arama Azure 'da kullanma](search-howto-dotnet-sdk.md)