---
title: Yönetici ve sorgu api anahtarları oluşturma, yönetme ve güvenli hale
titleSuffix: Azure Cognitive Search
description: Api anahtarı hizmet bitiş noktasına erişimi denetler. Yönetici anahtarları yazma erişimi verir. Yalnızca okunur erişim için sorgu anahtarları oluşturulabilir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 68a17b8b3587077222a9ed2057927c8f16253c1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72794383"
---
# <a name="create-and-manage-api-keys-for-an-azure-cognitive-search-service"></a>Azure Bilişsel Arama hizmeti için api anahtarları oluşturma ve yönetme

Bir arama hizmetine yapılan tüm istekler, hizmetiniz için özel olarak oluşturulan salt okunur api anahtarına ihtiyaç duyar. Api anahtarı, arama hizmeti bitiş noktanıza erişimin doğruluğunu doğrulamak için tek mekanizmadır ve her isteğe eklenmelidir. [REST çözümlerinde,](search-get-started-postman.md)api anahtarı genellikle bir istek üstbilgisinde belirtilir. [.NET çözümlerinde,](search-howto-dotnet-sdk.md#core-scenarios)anahtar genellikle yapılandırma ayarı olarak belirtilir ve [searchserviceclient'da](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) [Kimlik Bilgileri](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.credentials) (yönetici anahtarı) veya [Search Credentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.searchcredentials) (sorgu anahtarı) olarak geçirilir.

Anahtarlar, hizmet sağlama sırasında arama hizmetinizle birlikte oluşturulur. [Azure portalında](https://portal.azure.com)temel değerleri görüntüleyebilir ve elde edebilirsiniz.

![Portal sayfası, Ayarlar, Anahtarlar bölümü](media/search-manage/azure-search-view-keys.png)

## <a name="what-is-an-api-key"></a>Api anahtarı nedir

Api-tuşu rasgele oluşturulan sayılar ve harflerden oluşan bir dizedir. [Rol tabanlı izinler](search-security-rbac.md)aracılığıyla, anahtarları silebilir veya okuyabilirsiniz, ancak bir anahtarı kullanıcı tanımlı bir parolayla değiştiremezsiniz veya arama işlemlerine erişmek için birincil kimlik doğrulama yöntemi olarak Active Directory'yi kullanamazsınız. 

Arama hizmetinize erişmek için iki tür anahtar kullanılır: yönetici (okuma-yazma) ve sorgu (salt okunur).

|Anahtar|Açıklama|Sınırlar|  
|---------|-----------------|------------|  
|Yönetici|Hizmeti yönetme, dizin oluşturma ve silme, dizin oluştur ve veri kaynakları da dahil olmak üzere tüm işlemlere tam haklar verir.<br /><br /> Portalda *birincil* ve *ikincil* anahtarlar olarak adlandırılan iki yönetici anahtarı, hizmet oluşturulduğunda oluşturulur ve isteğe bağlı olarak tek tek yeniden oluşturulabilir. İki tuşa sahip olmak, hizmete sürekli erişim için ikinci anahtarı kullanırken bir tuşu yuvarlamanızı sağlar.<br /><br /> Yönetici anahtarları yalnızca HTTP istek üstbilgilerinde belirtilir. Bir YÖNETICI api anahtarını URL'ye yerleştiremezsiniz.|Hizmet başına en fazla 2|  
|Sorgu|Dizinlere ve belgelere salt okunur erişim verir ve genellikle arama istekleri veren istemci uygulamalarına dağıtılır.<br /><br /> Sorgu anahtarları isteğe bağlı olarak oluşturulur. Bunları portalda el ile veya Yönetim REST [API'si](https://docs.microsoft.com/rest/api/searchmanagement/)aracılığıyla programlı olarak oluşturabilirsiniz.<br /><br /> Sorgu anahtarları arama, öneri veya arama işlemi için bir HTTP istek üstbilgisinde belirtilebilir. Alternatif olarak, bir sorgu anahtarını URL'de parametre olarak geçirebilirsiniz. İstemci uygulamanızın isteği nasıl formüle ettigine bağlı olarak, anahtarı sorgu parametresi olarak geçirmek daha kolay olabilir:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2019-05-06&api-key=[query key]`|Hizmet başına 50|  

 Görsel olarak, yönetici anahtarı veya sorgu anahtarı arasında bir ayrım yoktur. Her iki anahtar da rasgele oluşturulan 32 alfa-sayısal karakterden oluşan dizeleridir. Uygulamanızda ne tür bir anahtar belirtildiğinin izini kaybederseniz, [portaldaki anahtar değerlerini denetleyebilir](https://portal.azure.com) veya değeri ve anahtar türünü döndürmek için [REST API'sini](https://docs.microsoft.com/rest/api/searchmanagement/) kullanabilirsiniz.  

> [!NOTE]  
>  İstek URI gibi hassas verileri aktarmak kötü `api-key` bir güvenlik uygulaması olarak kabul edilir. Bu nedenle, Azure Bilişsel Arama bir sorgu `api-key` anahtarını yalnızca sorgu dizesinde kabul eder ve dizininizin içeriği herkese açık olmadığı sürece bunu yapmaktan kaçınmalısınız. Genel bir kural olarak, `api-key` istek üstbilginiz olarak geçirmenizi öneririz.  

## <a name="find-existing-keys"></a>Varolan anahtarları bulma

Erişim anahtarlarını portaldan veya [Management REST API'den](https://docs.microsoft.com/rest/api/searchmanagement/)alabilirsiniz. Daha fazla bilgi için [yöneticiyi yönet ve api tuşlarını sorgula' ya](search-security-api-keys.md)bakın.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Aboneliğiniz için [arama hizmetlerini](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) listele.
3. Hizmeti seçin ve Genel Bakış sayfasında, yönetici ve sorgu tuşlarını görüntülemek için **Ayarlar** >**Tuşları'nı** tıklatın.

   ![Portal sayfası, Ayarlar, Anahtarlar bölümü](media/search-security-overview/settings-keys.png)

## <a name="create-query-keys"></a>Sorgu anahtarları oluşturma

Sorgu anahtarları, belge toplamayı hedefleyen işlemler için dizin içindeki belgelere salt okunur erişim için kullanılır. Arama, filtre leme ve öneri sorguları, sorgu anahtarı nı alan tüm işlemlerdir. Dizin tanımı veya dizin oluşturma durumu gibi sistem verilerini veya nesne tanımlarını döndüren salt okunur işlem, yönetici anahtarı gerektirir.

İstemci uygulamalarındaki erişimi ve işlemleri kısıtlamak, hizmetinizdeki arama varlıklarını korumak için çok önemlidir. İstemci uygulamasından kaynaklanan herhangi bir sorgu için yönetici anahtarı yerine her zaman bir sorgu anahtarı kullanın.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Aboneliğiniz için [arama hizmetlerini](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) listele.
3. Hizmeti seçin ve Genel Bakış sayfasında **Ayarlar** >**Tuşları'nı**tıklatın.
4. **Sorgu tuşlarını yönet'i**tıklatın.
5. Hizmetiniz için zaten oluşturulmuş sorgu anahtarını kullanın veya en fazla 50 yeni sorgu anahtarı oluşturun. Varsayılan sorgu anahtarı adlandırılmış değildir, ancak yönetilebilirlik için ek sorgu anahtarları adlandırılabilir.

   ![Sorgu anahtarı oluşturma veya kullanma](media/search-security-overview/create-query-key.png) 

> [!Note]
> Sorgu anahtarı kullanımını gösteren bir kod örneği [C# 'da bir Azure Bilişsel Arama dizini sorgula'da](search-query-dotnet.md)bulunabilir.

<a name="regenerate-admin-keys"></a>

## <a name="regenerate-admin-keys"></a>Yönetici anahtarlarını yeniden oluşturma

İş sürekliliği için ikincil anahtarı kullanarak birincil anahtarı döndürebilmeniz için her hizmet için iki yönetici anahtarı oluşturulur.

1. **Ayarlar** >**Tuşları** sayfasında ikincil anahtarı kopyalayın.
2. Tüm uygulamalar için ikincil anahtarı kullanmak için api tuşu ayarlarını güncelleştirin.
3. Birincil anahtarı yeniden oluşturun.
4. Yeni birincil anahtarı kullanmak için tüm uygulamaları güncelleştirin.

İstemeden her iki anahtarı da aynı anda yeniden oluşturursanız, bu anahtarları kullanan tüm istemci istekleri HTTP 403 Forbidden ile başarısız olur. Ancak, içerik silinmez ve kalıcı olarak kilitlendirin. 

Hizmete portal veya yönetim katmanı[(REST API,](https://docs.microsoft.com/rest/api/searchmanagement/) [PowerShell](https://docs.microsoft.com/azure/search/search-manage-powershell)veya Azure Kaynak Yöneticisi) üzerinden erişebilirsiniz. Yönetim işlevleri bir hizmet api anahtarı değil bir abonelik kimliği üzerinden çalışır ve bu nedenle api anahtarlarınız olmasa bile hala kullanılabilir. 

Portal veya yönetim katmanı üzerinden yeni anahtarlar oluşturduktan sonra, yeni anahtarlara sahip olduktan ve isteklerde bu anahtarları sağladıktan sonra içeriğinize (dizinler, dizin leyiciler, veri kaynakları, eşanlamlı haritalar) erişim geri yüklenir.

## <a name="secure-api-keys"></a>Güvenli api tuşları
Anahtar güvenliği, portal veya Kaynak Yöneticisi arabirimleri (PowerShell veya komut satırı arabirimi) üzerinden erişim kısıtlayarak sağlanır. Belirtildiği gibi, abonelik yöneticileri tüm api anahtarlarını görüntüleyebilir ve yeniden oluşturabilir. Önlem olarak, yönetici anahtarlarına kimlerin erişebildiğinizi anlamak için rol atamalarını gözden geçirin.

+ Hizmet panosunda, hizmetinizin rol **atamalarını** görüntülemek için **Access denetimi (IAM)** ve ardından Rol atamaları sekmesini tıklatın.

Aşağıdaki rollerin üyeleri anahtarları görüntüleyebilir ve yeniden oluşturabilir: Sahibi, Katılımcısı, [Arama Hizmeti Nekatkıda Bulunanlar](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor)

> [!Note]
> Arama sonuçlarına göre kimlik tabanlı erişim için, isteklerin erişmemesi gereken belgeleri kaldırarak sonuçları kimlikle kırpmak için güvenlik filtreleri oluşturabilirsiniz. Daha fazla bilgi için [Bkz. Güvenlik filtreleri](search-security-trimming-for-azure-search.md) ve [Active Directory ile Güvenli.](search-security-trimming-for-azure-search-with-aad.md)

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama'da rol tabanlı erişim denetimi](search-security-rbac.md)
+ [PowerShell kullanarak yönetme](search-manage-powershell.md) 
+ [Performans ve optimizasyon makalesi](search-performance-optimization.md)