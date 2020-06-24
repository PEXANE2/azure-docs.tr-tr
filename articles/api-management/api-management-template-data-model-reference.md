---
title: Azure API Management şablonu veri modeli başvurusu | Microsoft Docs
description: Azure API Management 'de geliştirici portalı şablonlarının veri modellerinde kullanılan ortak öğelerin varlık ve tür temsilleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 5625ff7e4fc51b9b6b894698719247902a480f44
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84690040"
---
# <a name="azure-api-management-template-data-model-reference"></a>Azure API Management şablonu veri modeli başvurusu
Bu konu başlığı altında, Azure API Management geliştirici portalı şablonlarının veri modellerinde kullanılan ortak öğelerin varlık ve tür gösterimleri açıklanmaktadır.  
  
 Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [şablonları kullanarak API Management Geliştirici Portalını Özelleştirme](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="reference"></a>Başvuru

-   [API](#API)  
-   [API Özeti](#APISummary)  
-   [Uygulama](#Application)  
-   [Ek](#Attachment)  
-   [Kod örneği](#Sample)  
-   [Yorum](#Comment)  
-   [Filtreleme](#Filtering)  
-   [Üst bilgi](#Header)  
-   [HTTP İsteği](#HTTPRequest)  
-   [HTTP yanıtı](#HTTPResponse)  
-   [Konuda](#Issue)  
-   [İşlem](#Operation)  
-   [İşlem menüsü](#Menu)  
-   [İşlem menü öğesi](#MenuItem)  
-   [Sayfalama](#Paging)  
-   [Parametre](#Parameter)  
-   [Ürün](#Product)  
-   [Sağlayıcı](#Provider)  
-   [İmle](#Representation)  
-   [Abonelik](#Subscription)  
-   [Abonelik Özeti](#SubscriptionSummary)  
-   [Kullanıcı hesabı bilgileri](#UserAccountInfo)  
-   [Kullanıcı oturumu açma](#UseSignIn)  
-   [Kullanıcı kaydı](#UserSignUp)  
  
##  <a name="api"></a><a name="API"></a>'SINDEKI  
 `API`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`id`|dize|Kaynak tanımlayıcısı. API 'YI geçerli API Management hizmet örneği içinde benzersiz şekilde tanımlar. Değer, `apis/{id}` BIR API tanımlayıcısı olduğu biçiminde geçerli bir GÖRELI URL 'dir `{id}` . Bu özellik salt okunur durumdadır.|  
|`name`|string|API 'nin adı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`description`|string|API 'nin açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`serviceUrl`|string|Bu API 'YI uygulayan arka uç hizmetinin mutlak URL 'SI.|  
|`path`|string|Göreli URL, bu API 'yi ve API Management hizmeti örneğindeki tüm kaynak yollarını benzersiz bir şekilde tanımlar. Bu API için genel bir URL oluşturmak üzere hizmet örneği oluşturma sırasında belirtilen API uç noktası taban URL 'sine eklenir.|  
|`protocols`|sayı dizisi|Bu API 'deki işlemlerin hangi protokolde çağrılabileceğini açıklar. İzin verilen değerler `1 - http` ve `2 - https` veya her ikisi.|  
|`authenticationSettings`|[Yetkilendirme sunucusu kimlik doğrulama ayarları](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Bu API 'ye dahil edilen kimlik doğrulama ayarları koleksiyonu.|  
|`subscriptionKeyParameterNames`|nesne|Abonelik anahtarını içeren sorgu ve/veya üst bilgi parametrelerine ilişkin özel adları belirtmek için kullanılabilen isteğe bağlı özellik. Bu özellik varsa, aşağıdaki iki özellikten en az birini içermesi gerekir.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="api-summary"></a><a name="APISummary"></a>API Özeti  
 `API summary`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`id`|dize|Kaynak tanımlayıcısı. API 'YI geçerli API Management hizmet örneği içinde benzersiz şekilde tanımlar. Değer, `apis/{id}` BIR API tanımlayıcısı olduğu biçiminde geçerli bir GÖRELI URL 'dir `{id}` . Bu özellik salt okunur durumdadır.|  
|`name`|string|API 'nin adı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`description`|string|API 'nin açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
  
##  <a name="application"></a><a name="Application"></a>Uygulamanızı  
 `application`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Id`|dize|Uygulamanın benzersiz tanımlayıcısı.|  
|`Title`|string|Uygulamanın başlığı.|  
|`Description`|string|Uygulamanın açıklaması.|  
|`Url`|URI|Uygulamanın URI 'SI.|  
|`Version`|string|Uygulamanın sürüm bilgileri.|  
|`Requirements`|string|Uygulama gereksinimlerinin açıklaması.|  
|`State`|sayı|Uygulamanın geçerli durumu.<br /><br /> -0-kayıtlı<br /><br /> -1-gönderildi<br /><br /> -2-yayımlandı<br /><br /> -3-reddedildi<br /><br /> -4-yayımdan kaldırıldı|  
|`RegistrationDate`|DateTime|Uygulamanın kaydedildiği tarih ve saat.|  
|`CategoryId`|sayı|Uygulamanın kategorisi (Finans, eğlence, vb.)|  
|`DeveloperId`|string|Uygulamayı gönderen geliştiricinin benzersiz tanıtıcısı.|  
|`Attachments`|[Ek](#Attachment) varlıkların koleksiyonu.|Uygulamanın ekran görüntüleri veya simgeleri gibi ekleri.|  
|`Icon`|[Ek](#Attachment)|Uygulamanın simgesi.|  
  
##  <a name="attachment"></a><a name="Attachment"></a>Ekindeki  
 `attachment`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`UniqueId`|dize|Ek için benzersiz tanımlayıcı.|  
|`Url`|string|Kaynağın URL 'SI.|  
|`Type`|string|Ek türü.|  
|`ContentType`|string|Ekin medya türü.|  
  
##  <a name="code-sample"></a><a name="Sample"></a>Kod örneği  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`title`|dize|İşlemin adı.|  
|`snippet`|string|Bu özellik kullanım dışıdır ve kullanılmamalıdır.|  
|`brush`|string|Kod örneği görüntülenirken kullanılacak olan kod sözdizimi şablonu. İzin verilen değerler,,,,, `plain` `php` `java` `xml` `objc` `python` , `ruby` ve `csharp` .|  
|`template`|string|Bu kod örneği şablonunun adı.|  
|`body`|string|Kod parçacığının kod örnek bölümü için yer tutucu.|  
|`method`|string|İşlemin HTTP yöntemi.|  
|`scheme`|string|İşlem isteği için kullanılacak protokol.|  
|`path`|string|İşlemin yolu.|  
|`query`|string|Sorgu dizesi örneği tanımlanmış parametrelerle.|  
|`host`|string|Bu işlemi içeren API için API Management hizmeti ağ geçidinin URL 'SI.|  
|`headers`|[Üst bilgi](#Header) varlıkları koleksiyonu.|Bu işlemin üst bilgileri.|  
|`parameters`|[Parametre](#Parameter) varlıklarının koleksiyonu.|Bu işlem için tanımlanan parametreler.|  
  
##  <a name="comment"></a><a name="Comment"></a>Açıklamanın  
 `API`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Id`|sayı|Açıklamanın KIMLIĞI.|  
|`CommentText`|string|Açıklamanın gövdesi. HTML içerebilir.|  
|`DeveloperCompany`|string|Geliştiricinin şirket adı.|  
|`PostedOn`|DateTime|Yorumun gönderildiği tarih ve saat.|  
  
##  <a name="issue"></a><a name="Issue"></a>Konuda  
 `issue`Varlık aşağıdaki özelliklere sahiptir.  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Id`|dize|Sorun için benzersiz tanımlayıcı.|  
|`ApiID`|string|Bu sorunun bildirildiği API 'nin KIMLIĞI.|  
|`Title`|string|Sorunun başlığı.|  
|`Description`|string|Sorunun açıklaması.|  
|`SubscriptionDeveloperName`|string|Sorunu bildiren geliştiricinin ilk adı.|  
|`IssueState`|string|Sorunun geçerli durumu. Olası değerler önerilir, açılır, kapalıdır.|  
|`ReportedOn`|DateTime|Sorunun bildirildiği tarih ve saat.|  
|`Comments`|[Açıklama](#Comment) varlıkları koleksiyonu.|Bu sorunla ilgili açıklamalar.|  
|`Attachments`|[Ek](api-management-template-data-model-reference.md#Attachment) varlıkların koleksiyonu.|Sorunun ekleri.|  
|`Services`|[API](#API) varlıkları koleksiyonu.|API 'Ler, sorunu dosyalayan Kullanıcı tarafından abone oldu.|  
  
##  <a name="filtering"></a><a name="Filtering"></a>Menin  
 `filtering`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Pattern`|dize|Geçerli arama terimi; veya `null` arama terimi yoksa.|  
|`Placeholder`|string|Arama terimi belirtilmediğinde arama kutusunda görüntülenecek metin.|  
  
##  <a name="header"></a><a name="Header"></a>Üst bilgi  
 Bu bölümde temsil açıklanmaktadır `parameter` .  
  
|Özellik|Tür|Description|  
|--------------|-----------------|----------|  
|`name`|dize|Parametre adı.|  
|`description`|string|Parametre açıklaması.|  
|`value`|string|Üst bilgi değeri.|  
|`typeName`|string|Üst bilgi değerinin veri türü.|  
|`options`|string|Seçenekler’i seçin.|  
|`required`|boole|Üstbilginin gerekli olup olmadığı.|  
|`readOnly`|boole|Üstbilginin Salt okunabilir olup olmadığı.|  
  
##  <a name="http-request"></a><a name="HTTPRequest"></a>HTTP Isteği  
 Bu bölümde temsil açıklanmaktadır `request` .  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`description`|dize|İşlem isteği açıklaması.|  
|`headers`|[üst bilgi](#Header) varlıkları dizisi.|İstek üst bilgileri.|  
|`parameters`|[parametre](#Parameter) dizisi|İşlem isteği parametreleri koleksiyonu.|  
|`representations`|[temsili](#Representation) dizisi|İşlem isteği temsilleri koleksiyonu.|  
  
##  <a name="http-response"></a><a name="HTTPResponse"></a>HTTP yanıtı  
 Bu bölümde temsil açıklanmaktadır `response` .  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`statusCode`|pozitif tamsayı|İşlem yanıtı durum kodu.|  
|`description`|string|İşlem yanıtı açıklaması.|  
|`representations`|[temsili](#Representation) dizisi|İşlem yanıtı temsilleri koleksiyonu.|  
  
##  <a name="operation"></a><a name="Operation"></a>Çalışmasını  
 `operation`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`id`|dize|Kaynak tanımlayıcısı. İşlemi geçerli API Management hizmet örneği içinde benzersiz şekilde tanımlar. Değer, `apis/{aid}/operations/{id}` `{aid}` bir API tanımlayıcısı olduğu ve bir işlem tanımlayıcısı olan biçiminde geçerli BIR göreli URL 'dir `{id}` . Bu özellik salt okunur durumdadır.|  
|`name`|string|İşlemin adı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`description`|string|İşlemin açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`scheme`|string|Bu API 'deki işlemlerin hangi protokolde çağrılabileceğini açıklar. İzin verilen değerler `http` ,, `https` veya her ikisi `http` ve `https` .|  
|`uriTemplate`|string|Bu işlemin hedef kaynağını tanımlayan göreli URL şablonu. Parametreleri içerebilir. Örnek: `customers/{cid}/orders/{oid}/?date={date}`|  
|`host`|string|API 'yi barındıran API Management ağ geçidi URL 'SI.|  
|`httpMethod`|string|İşlem HTTP metodu.|  
|`request`|[HTTP İsteği](#HTTPRequest)|İstek ayrıntılarını içeren bir varlık.|  
|`responses`|[http yanıtı](#HTTPResponse) dizisi|İşlem [http yanıtı](#HTTPResponse) varlıkları dizisi.|  
  
##  <a name="operation-menu"></a><a name="Menu"></a>İşlem menüsü  
 `operation menu`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`ApiId`|dize|Geçerli API 'nin KIMLIĞI.|  
|`CurrentOperationId`|string|Geçerli işlemin KIMLIĞI.|  
|`Action`|string|Menü türü.|  
|`MenuItems`|[İşlem menüsü öğe](#MenuItem) varlıkları koleksiyonu.|Geçerli API için işlemler.|  
  
##  <a name="operation-menu-item"></a><a name="MenuItem"></a>İşlem menü öğesi  
 `operation menu item`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Id`|dize|İşlemin KIMLIĞI.|  
|`Title`|string|İşlemin açıklaması.|  
|`HttpMethod`|string|İşlemin http yöntemi.|  
  
##  <a name="paging"></a><a name="Paging"></a>Sayfalamayı  
 `paging`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Page`|sayı|Geçerli sayfa numarası.|  
|`PageSize`|sayı|Tek bir sayfada görüntülenecek en fazla sonuç.|  
|`TotalItemCount`|sayı|Görüntülenecek öğe sayısı.|  
|`ShowAll`|boole|Tüm sonuçların tek bir sayfada gösterilip gösterilmeyeceğini belirtir.|  
|`PageCount`|sayı|Sonuçların sayfa sayısı.|  
  
##  <a name="parameter"></a><a name="Parameter"></a>Parametresinin  
 Bu bölümde temsil açıklanmaktadır `parameter` .  
  
|Özellik|Tür|Description|  
|--------------|-----------------|----------|  
|`name`|dize|Parametre adı.|  
|`description`|string|Parametre açıklaması.|  
|`value`|string|Parametre değeri.|  
|`options`|dize dizisi|Sorgu parametresi değerleri için tanımlanan değerler.|  
|`required`|boole|Parametrenin gerekli olup olmadığını belirtir.|  
|`kind`|sayı|Bu parametrenin bir yol parametresi (1) veya QueryString parametresi (2) olup olmadığı.|  
|`typeName`|string|Parametre türü.|  
  
##  <a name="product"></a><a name="Product"></a>Ürünüyle  
 `product`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Id`|dize|Kaynak tanımlayıcısı. Ürünü geçerli API Management hizmet örneği içinde benzersiz şekilde tanımlar. Değer, `products/{pid}` bir ürün tanımlayıcısı olan biçiminde geçerli bir GÖRELI URL 'dir `{pid}` . Bu özellik salt okunur durumdadır.|  
|`Title`|string|Ürünün adı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`Description`|string|Ürünün açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`Terms`|string|Ürün kullanım koşulları. Ürüne abone olmayı deneyen geliştiriciler, abonelik işlemini tamamlayabilmeleri için önce bu koşulları kabul etmek üzere sunulacaktır ve gerekli olacaktır.|  
|`ProductState`|sayı|Ürünün yayınlanıp yayımlanmadığını belirtir. Yayımlanan ürünler geliştirici portalındaki geliştiriciler tarafından bulunabilir. Yayımlanmamış ürünler yalnızca yöneticiler tarafından görülebilir.<br /><br /> Ürün durumu için izin verilen değerler şunlardır:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|`AllowMultipleSubscriptions`|boole|Bir kullanıcının aynı anda bu ürüne birden fazla aboneliğine sahip olup olmayacağını belirtir.|  
|`MultipleSubscriptionsCount`|sayı|Bu ürüne bir kullanıcının aynı anda sahip olmasına izin verilen maksimum abonelik sayısı.|  
  
##  <a name="provider"></a><a name="Provider"></a>Sağlayıcısını  
 `provider`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Properties`|dize sözlüğü|Bu kimlik doğrulama sağlayıcısının özellikleri.|  
|`AuthenticationType`|string|Sağlayıcı türü. (Azure Active Directory, Facebook oturum açma, Google hesabı, Microsoft hesabı, Twitter).|  
|`Caption`|string|Sağlayıcının görünen adı.|  
  
##  <a name="representation"></a><a name="Representation"></a>İmle  
 Bu bölümde bir açıklanır `representation` .  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`contentType`|dize|Bu gösterim için kayıtlı veya özel bir içerik türü belirtir, örneğin `application/xml` .|  
|`sample`|string|Gösterimine bir örnek.|  
  
##  <a name="subscription"></a><a name="Subscription"></a>Aboneliğiniz  
 `subscription`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Id`|dize|Kaynak tanımlayıcısı. Geçerli API Management hizmet örneği içinde aboneliği benzersiz şekilde tanımlar. Değer, `subscriptions/{sid}` nerede bir abonelik tanımlayıcısı olduğu biçiminde geçerli bir GÖRELI URL 'dir `{sid}` . Bu özellik salt okunur durumdadır.|  
|`ProductId`|string|Abone olunan ürünün ürün kaynak tanımlayıcısı. Değer, `products/{pid}` bir ürün tanımlayıcısı olan biçiminde geçerli bir GÖRELI URL 'dir `{pid}` .|  
|`ProductTitle`|string|Ürünün adı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`ProductDescription`|string|Ürünün açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`ProductDetailsUrl`|string|Ürün ayrıntılarının göreli URL 'SI.|  
|`state`|string|Aboneliğin durumu. Olası durumlar şunlardır:<br /><br /> - `0 - suspended`– abonelik engellenir ve abone ürünün herhangi bir API 'sini çağıramaz.<br /><br /> - `1 - active`– Abonelik etkin.<br /><br /> - `2 - expired`– abonelik sona erme tarihine ulaştı ve devre dışı bırakıldı.<br /><br /> - `3 - submitted`– abonelik isteği geliştirici tarafından yapıldı, ancak henüz onaylanmamış veya reddedildi.<br /><br /> - `4 - rejected`– abonelik isteği bir yönetici tarafından reddedildi.<br /><br /> - `5 - cancelled`– abonelik, geliştirici veya yönetici tarafından iptal edildi.|  
|`DisplayName`|string|Aboneliğin görünen adı.|  
|`CreatedDate`|tarih saat|Aboneliğin oluşturulduğu tarih, ISO 8601 biçiminde: `2014-06-24T16:25:00Z` .|  
|`CanBeCancelled`|boole|Aboneliğin geçerli kullanıcı tarafından iptal edilip edilmeyeceğini belirtir.|  
|`IsAwaitingApproval`|boole|Aboneliğin onay bekliyor olup olmadığı.|  
|`StartDate`|tarih saat|Aboneliğin başlangıç tarihi, ISO 8601 biçiminde: `2014-06-24T16:25:00Z` .|  
|`ExpirationDate`|tarih saat|Aboneliğin son kullanma tarihi, ISO 8601 biçiminde: `2014-06-24T16:25:00Z` .|  
|`NotificationDate`|tarih saat|Aboneliğin bildirim tarihi, ISO 8601 biçiminde: `2014-06-24T16:25:00Z` .|  
|`primaryKey`|string|Birincil abonelik anahtarı. Maksimum uzunluk 256 karakterdir.|  
|`secondaryKey`|string|İkincil abonelik anahtarı. Maksimum uzunluk 256 karakterdir.|  
|`CanBeRenewed`|boole|Aboneliğin geçerli kullanıcı tarafından yenilenebilir olup olmadığı.|  
|`HasExpired`|boole|Aboneliğin süresi doldu.|  
|`IsRejected`|boole|Abonelik isteğinin reddedildiğini belirtir.|  
|`CancelUrl`|string|Aboneliği iptal etmek için göreli URL.|  
|`RenewUrl`|string|Aboneliği yenilemek için göreli URL.|  
  
##  <a name="subscription-summary"></a><a name="SubscriptionSummary"></a>Abonelik Özeti  
 `subscription summary`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Id`|dize|Kaynak tanımlayıcısı. Geçerli API Management hizmet örneği içinde aboneliği benzersiz şekilde tanımlar. Değer, `subscriptions/{sid}` nerede bir abonelik tanımlayıcısı olduğu biçiminde geçerli bir GÖRELI URL 'dir `{sid}` . Bu özellik salt okunur durumdadır.|  
|`DisplayName`|string|Aboneliğin görünen adı|  
  
##  <a name="user-account-info"></a><a name="UserAccountInfo"></a>Kullanıcı hesabı bilgileri  
 `user account info`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`FirstName`|dize|Ad. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`LastName`|string|Soyadı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`Email`|string|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. Maksimum uzunluk 254 karakterdir.|  
|`Password`|string|Kullanıcı hesabı parolası.|  
|`NameIdentifier`|string|Hesap tanımlayıcısı, kullanıcı e-postasıdır.|  
|`ProviderName`|string|Kimlik doğrulama sağlayıcısı adı.|  
|`IsBasicAccount`|boole|Bu hesap e-posta ve parola kullanılarak kaydedilmişse doğru; hesap bir sağlayıcı kullanılarak kaydedilmişse false.|  
  
##  <a name="user-sign-in"></a><a name="UseSignIn"></a>Kullanıcı oturum açma  
 `user sign in`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`Email`|dize|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. Maksimum uzunluk 254 karakterdir.|  
|`Password`|string|Kullanıcı hesabı parolası.|  
|`ReturnUrl`|string|Kullanıcının oturum açma tıkladığını sayfanın URL 'SI.|  
|`RememberMe`|boole|Geçerli kullanıcının bilgilerinin kaydedilip edilmeyeceğini belirtir.|  
|`RegistrationEnabled`|boole|Kaydın etkin olup olmadığı.|  
|`DelegationEnabled`|boole|Temsilci olarak oturum açma 'nın etkin olup olmadığı.|  
|`DelegationUrl`|string|Etkinleştirilmişse, temsilci oturum açma URL 'si.|  
|`SsoSignUpUrl`|string|Varsa, kullanıcının çoklu oturum açma URL 'SI.|  
|`AuxServiceUrl`|string|Geçerli Kullanıcı bir yöneticise, bu Azure portal hizmet örneğinin bir bağlantıdır.|  
|`Providers`|[Sağlayıcı](#Provider) varlıkları koleksiyonu|Bu Kullanıcı için kimlik doğrulama sağlayıcıları.|  
|`UserRegistrationTerms`|string|Kullanıcının oturum açmadan önce kabul etmesi gereken koşullar.|  
|`UserRegistrationTermsEnabled`|boole|Koşulların etkinleştirilip etkinleştirilmeyeceğini belirtir.|  
  
##  <a name="user-sign-up"></a><a name="UserSignUp"></a>Kullanıcı kaydı  
 `user sign up`Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Description|  
|--------------|----------|-----------------|  
|`PasswordConfirm`|boole|[Kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi tarafından kullanılan değer.|  
|`Password`|string|Kullanıcı hesabı parolası.|  
|`PasswordVerdictLevel`|sayı|[Kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi tarafından kullanılan değer.|  
|`UserRegistrationTerms`|string|Kullanıcının oturum açmadan önce kabul etmesi gereken koşullar.|  
|`UserRegistrationTermsOptions`|sayı|[Kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi tarafından kullanılan değer.|  
|`ConsentAccepted`|boole|[Kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi tarafından kullanılan değer.|  
|`Email`|string|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. Maksimum uzunluk 254 karakterdir.|  
|`FirstName`|string|Ad. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`LastName`|string|Soyadı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`UserData`|string|[Kaydolma](api-management-page-controls.md#sign-up) denetimi tarafından kullanılan değer.|  
|`NameIdentifier`|string|[Kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi tarafından kullanılan değer.|  
|`ProviderName`|string|Kimlik doğrulama sağlayıcısı adı.|

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [şablonları kullanarak API Management Geliştirici Portalını Özelleştirme](api-management-developer-portal-templates.md).
