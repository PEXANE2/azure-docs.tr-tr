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
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 323b3effb4c4a63d03ab7ea5251e0d59271d9dcd
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70072149"
---
# <a name="azure-api-management-template-data-model-reference"></a>Azure API Management şablonu veri modeli başvurusu
Bu konu başlığı altında, Azure API Management geliştirici portalı şablonlarının veri modellerinde kullanılan ortak öğelerin varlık ve tür gösterimleri açıklanmaktadır.  
  
 Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [şablonları kullanarak API Management Geliştirici Portalını Özelleştirme](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

Geliştirici portalı, tüketim katmanında kullanılamaz.

## <a name="reference"></a>Başvuru

-   [API](#API)  
-   [API Özeti](#APISummary)  
-   [Uygulama](#Application)  
-   [Ekindeki](#Attachment)  
-   [Kod örneği](#Sample)  
-   [Açıklamanın](#Comment)  
-   [Filtreleme](#Filtering)  
-   [Üst Bilgi](#Header)  
-   [HTTP Isteği](#HTTPRequest)  
-   [HTTP yanıtı](#HTTPResponse)  
-   [Konuda](#Issue)  
-   [İşlem](#Operation)  
-   [İşlem menüsü](#Menu)  
-   [İşlem menü öğesi](#MenuItem)  
-   [Sayfalamayı](#Paging)  
-   [Parametre](#Parameter)  
-   [Ürünüyle](#Product)  
-   [Sağlayıcı](#Provider)  
-   [İmle](#Representation)  
-   [Abonelik](#Subscription)  
-   [Abonelik Özeti](#SubscriptionSummary)  
-   [Kullanıcı hesabı bilgileri](#UserAccountInfo)  
-   [Kullanıcı oturumu açma](#UseSignIn)  
-   [Kullanıcı kaydı](#UserSignUp)  
  
##  <a name="API"></a>'SINDEKI  
 `API` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`id`|dize|Kaynak tanımlayıcısı. API 'YI geçerli API Management hizmet örneği içinde benzersiz şekilde tanımlar. Değer, bir API tanımlayıcısı olduğu biçiminde `apis/{id}` `{id}` geçerli bir göreli URL 'dir. Bu özellik salt okunurdur.|  
|`name`|dize|API 'nin adı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`description`|dize|API 'nin açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`serviceUrl`|dize|Bu API 'YI uygulayan arka uç hizmetinin mutlak URL 'SI.|  
|`path`|dize|Göreli URL, bu API 'yi ve API Management hizmeti örneğindeki tüm kaynak yollarını benzersiz bir şekilde tanımlar. Bu API için genel bir URL oluşturmak üzere hizmet örneği oluşturma sırasında belirtilen API uç noktası taban URL 'sine eklenir.|  
|`protocols`|sayı dizisi|Bu API 'deki işlemlerin hangi protokolde çağrılabileceğini açıklar. İzin verilen değerler `1 - http` ve `2 - https`veya her ikisi.|  
|`authenticationSettings`|[Yetkilendirme sunucusu kimlik doğrulama ayarları](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Bu API 'ye dahil edilen kimlik doğrulama ayarları koleksiyonu.|  
|`subscriptionKeyParameterNames`|object|Abonelik anahtarını içeren sorgu ve/veya üst bilgi parametrelerine ilişkin özel adları belirtmek için kullanılabilen isteğe bağlı özellik. Bu özellik varsa, aşağıdaki iki özellikten en az birini içermesi gerekir.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a>API Özeti  
 `API summary` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`id`|dize|Kaynak tanımlayıcısı. API 'YI geçerli API Management hizmet örneği içinde benzersiz şekilde tanımlar. Değer, bir API tanımlayıcısı olduğu biçiminde `apis/{id}` `{id}` geçerli bir göreli URL 'dir. Bu özellik salt okunurdur.|  
|`name`|dize|API 'nin adı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`description`|dize|API 'nin açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
  
##  <a name="Application"></a>Uygulamanızı  
 `application` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|dize|Uygulamanın benzersiz tanımlayıcısı.|  
|`Title`|dize|Uygulamanın başlığı.|  
|`Description`|dize|Uygulamanın açıklaması.|  
|`Url`|URI|Uygulamanın URI 'SI.|  
|`Version`|dize|Uygulamanın sürüm bilgileri.|  
|`Requirements`|dize|Uygulama gereksinimlerinin açıklaması.|  
|`State`|numarası|Uygulamanın geçerli durumu.<br /><br /> -0-kayıtlı<br /><br /> -1-gönderildi<br /><br /> -2-yayımlandı<br /><br /> -3-reddedildi<br /><br /> -4-yayımdan kaldırıldı|  
|`RegistrationDate`|DateTime|Uygulamanın kaydedildiği tarih ve saat.|  
|`CategoryId`|numarası|Uygulamanın kategorisi (Finans, eğlence, vb.)|  
|`DeveloperId`|dize|Uygulamayı gönderen geliştiricinin benzersiz tanıtıcısı.|  
|`Attachments`|[Ek](#Attachment) varlıkların koleksiyonu.|Uygulamanın ekran görüntüleri veya simgeleri gibi ekleri.|  
|`Icon`|[Ekindeki](#Attachment)|Uygulamanın simgesi.|  
  
##  <a name="Attachment"></a>Ekindeki  
 `attachment` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`UniqueId`|dize|Ek için benzersiz tanımlayıcı.|  
|`Url`|dize|Kaynağın URL 'SI.|  
|`Type`|dize|Ek türü.|  
|`ContentType`|dize|Ekin medya türü.|  
  
##  <a name="Sample"></a>Kod örneği  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`title`|dize|İşlemin adı.|  
|`snippet`|dize|Bu özellik kullanım dışıdır ve kullanılmamalıdır.|  
|`brush`|dize|Kod örneği görüntülenirken kullanılacak olan kod sözdizimi şablonu. İzin verilen değerler `plain` `php` ,`objc` ,,`csharp`,, ,`ruby`ve. `java` `xml` `python`|  
|`template`|dize|Bu kod örneği şablonunun adı.|  
|`body`|dize|Kod parçacığının kod örnek bölümü için yer tutucu.|  
|`method`|dize|İşlemin HTTP yöntemi.|  
|`scheme`|dize|İşlem isteği için kullanılacak protokol.|  
|`path`|dize|İşlemin yolu.|  
|`query`|dize|Sorgu dizesi örneği tanımlanmış parametrelerle.|  
|`host`|dize|Bu işlemi içeren API için API Management hizmeti ağ geçidinin URL 'SI.|  
|`headers`|[Üst bilgi](#Header) varlıkları koleksiyonu.|Bu işlemin üst bilgileri.|  
|`parameters`|[Parametre](#Parameter) varlıklarının koleksiyonu.|Bu işlem için tanımlanan parametreler.|  
  
##  <a name="Comment"></a>Açıklamanın  
 `API` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|numarası|Açıklamanın kimliği.|  
|`CommentText`|dize|Açıklamanın gövdesi. HTML içerebilir.|  
|`DeveloperCompany`|dize|Geliştiricinin şirket adı.|  
|`PostedOn`|DateTime|Yorumun gönderildiği tarih ve saat.|  
  
##  <a name="Issue"></a>Konuda  
 `issue` Varlık aşağıdaki özelliklere sahiptir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|dize|Sorun için benzersiz tanımlayıcı.|  
|`ApiID`|dize|Bu sorunun bildirildiği API 'nin kimliği.|  
|`Title`|dize|Sorunun başlığı.|  
|`Description`|dize|Sorunun açıklaması.|  
|`SubscriptionDeveloperName`|dize|Sorunu bildiren geliştiricinin ilk adı.|  
|`IssueState`|dize|Sorunun geçerli durumu. Olası değerler önerilir, açılır, kapalıdır.|  
|`ReportedOn`|DateTime|Sorunun bildirildiği tarih ve saat.|  
|`Comments`|[Açıklama](#Comment) varlıkları koleksiyonu.|Bu sorunla ilgili açıklamalar.|  
|`Attachments`|[Ek](api-management-template-data-model-reference.md#Attachment) varlıkların koleksiyonu.|Sorunun ekleri.|  
|`Services`|[API](#API) varlıkları koleksiyonu.|API 'Ler, sorunu dosyalayan Kullanıcı tarafından abone oldu.|  
  
##  <a name="Filtering"></a>Menin  
 `filtering` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Pattern`|dize|Geçerli arama terimi; veya `null` arama terimi yoksa.|  
|`Placeholder`|dize|Arama terimi belirtilmediğinde arama kutusunda görüntülenecek metin.|  
  
##  <a name="Header"></a>Üst bilgi  
 Bu bölümde `parameter` temsil açıklanmaktadır.  
  
|Özellik|Tür|Açıklama|  
|--------------|-----------------|----------|  
|`name`|dize|Parametre adı.|  
|`description`|dize|Parametre açıklaması.|  
|`value`|dize|Üst bilgi değeri.|  
|`typeName`|dize|Üst bilgi değerinin veri türü.|  
|`options`|dize|Seçenekler.|  
|`required`|boolean|Üstbilginin gerekli olup olmadığı.|  
|`readOnly`|boolean|Üstbilginin Salt okunabilir olup olmadığı.|  
  
##  <a name="HTTPRequest"></a>HTTP Isteği  
 Bu bölümde `request` temsil açıklanmaktadır.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`description`|dize|İşlem isteği açıklaması.|  
|`headers`|[üst bilgi](#Header) varlıkları dizisi.|İstek üst bilgileri.|  
|`parameters`|[parametre](#Parameter) dizisi|İşlem isteği parametreleri koleksiyonu.|  
|`representations`|[temsili](#Representation) dizisi|İşlem isteği temsilleri koleksiyonu.|  
  
##  <a name="HTTPResponse"></a>HTTP yanıtı  
 Bu bölümde `response` temsil açıklanmaktadır.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`statusCode`|pozitif tamsayı|İşlem yanıtı durum kodu.|  
|`description`|dize|İşlem yanıtı açıklaması.|  
|`representations`|[temsili](#Representation) dizisi|İşlem yanıtı temsilleri koleksiyonu.|  
  
##  <a name="Operation"></a>Çalışmasını  
 `operation` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`id`|dize|Kaynak tanımlayıcısı. İşlemi geçerli API Management hizmet örneği içinde benzersiz şekilde tanımlar. Değer, bir API tanımlayıcısı olduğu ve `apis/{aid}/operations/{id}` `{id}` bir işlem tanımlayıcısı olan biçiminde `{aid}` geçerli bir göreli URL 'dir. Bu özellik salt okunurdur.|  
|`name`|dize|İşlemin adı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`description`|dize|İşlemin açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`scheme`|dize|Bu API 'deki işlemlerin hangi protokolde çağrılabileceğini açıklar. İzin verilen değerler `http`, `https`, veya her `http` ikisi `https`ve.|  
|`uriTemplate`|dize|Bu işlemin hedef kaynağını tanımlayan göreli URL şablonu. Parametreleri içerebilir. Örnek: `customers/{cid}/orders/{oid}/?date={date}`|  
|`host`|dize|API 'yi barındıran API Management ağ geçidi URL 'SI.|  
|`httpMethod`|dize|İşlem HTTP metodu.|  
|`request`|[HTTP Isteği](#HTTPRequest)|İstek ayrıntılarını içeren bir varlık.|  
|`responses`|[http yanıtı](#HTTPResponse) dizisi|İşlem [http yanıtı](#HTTPResponse) varlıkları dizisi.|  
  
##  <a name="Menu"></a>İşlem menüsü  
 `operation menu` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`ApiId`|dize|Geçerli API 'nin kimliği.|  
|`CurrentOperationId`|dize|Geçerli işlemin kimliği.|  
|`Action`|dize|Menü türü.|  
|`MenuItems`|[İşlem menüsü öğe](#MenuItem) varlıkları koleksiyonu.|Geçerli API için işlemler.|  
  
##  <a name="MenuItem"></a>İşlem menü öğesi  
 `operation menu item` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|dize|İşlemin kimliği.|  
|`Title`|dize|İşlemin açıklaması.|  
|`HttpMethod`|dize|İşlemin http yöntemi.|  
  
##  <a name="Paging"></a>Sayfalamayı  
 `paging` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Page`|numarası|Geçerli sayfa numarası.|  
|`PageSize`|numarası|Tek bir sayfada görüntülenecek en fazla sonuç.|  
|`TotalItemCount`|numarası|Görüntülenecek öğe sayısı.|  
|`ShowAll`|boolean|Tüm sonuçların tek bir sayfada gösterilip gösterilmeyeceğini belirtir.|  
|`PageCount`|numarası|Sonuçların sayfa sayısı.|  
  
##  <a name="Parameter"></a>Parametresinin  
 Bu bölümde `parameter` temsil açıklanmaktadır.  
  
|Özellik|Tür|Açıklama|  
|--------------|-----------------|----------|  
|`name`|dize|Parametre adı.|  
|`description`|dize|Parametre açıklaması.|  
|`value`|dize|Parametre değeri.|  
|`options`|dize dizisi|Sorgu parametresi değerleri için tanımlanan değerler.|  
|`required`|boolean|Parametrenin gerekli olup olmadığını belirtir.|  
|`kind`|numarası|Bu parametrenin bir yol parametresi (1) veya QueryString parametresi (2) olup olmadığı.|  
|`typeName`|dize|Parametre türü.|  
  
##  <a name="Product"></a>Ürünüyle  
 `product` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|dize|Kaynak tanımlayıcısı. Ürünü geçerli API Management hizmet örneği içinde benzersiz şekilde tanımlar. Değer, bir ürün tanımlayıcısı olan biçiminde `products/{pid}` `{pid}` geçerli bir göreli URL 'dir. Bu özellik salt okunurdur.|  
|`Title`|dize|Ürünün adı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`Description`|dize|Ürünün açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`Terms`|dize|Ürün kullanım koşulları. Ürüne abone olmayı deneyen geliştiriciler, abonelik işlemini tamamlayabilmeleri için önce bu koşulları kabul etmek üzere sunulacaktır ve gerekli olacaktır.|  
|`ProductState`|numarası|Ürünün yayınlanıp yayımlanmadığını belirtir. Yayımlanan ürünler geliştirici portalındaki geliştiriciler tarafından bulunabilir. Yayımlanmamış ürünler yalnızca yöneticiler tarafından görülebilir.<br /><br /> Ürün durumu için izin verilen değerler şunlardır:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|`AllowMultipleSubscriptions`|boolean|Bir kullanıcının aynı anda bu ürüne birden fazla aboneliğine sahip olup olmayacağını belirtir.|  
|`MultipleSubscriptionsCount`|numarası|Bu ürüne bir kullanıcının aynı anda sahip olmasına izin verilen maksimum abonelik sayısı.|  
  
##  <a name="Provider"></a>Sağlayıcısını  
 `provider` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Properties`|dize sözlüğü|Bu kimlik doğrulama sağlayıcısının özellikleri.|  
|`AuthenticationType`|dize|Sağlayıcı türü. (Azure Active Directory, Facebook oturum açma, Google hesabı, Microsoft hesabı, Twitter).|  
|`Caption`|dize|Sağlayıcının görünen adı.|  
  
##  <a name="Representation"></a>İmle  
 Bu bölümde bir `representation`açıklanır.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`contentType`|dize|Bu gösterim için kayıtlı veya özel bir içerik türü belirtir, örneğin `application/xml`.|  
|`sample`|dize|Gösterimine bir örnek.|  
  
##  <a name="Subscription"></a>Aboneliğiniz  
 `subscription` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|dize|Kaynak tanımlayıcısı. Geçerli API Management hizmet örneği içinde aboneliği benzersiz şekilde tanımlar. Değer, `subscriptions/{sid}` nerede `{sid}` bir abonelik tanımlayıcısı olduğu biçiminde geçerli bir göreli URL 'dir. Bu özellik salt okunurdur.|  
|`ProductId`|dize|Abone olunan ürünün ürün kaynak tanımlayıcısı. Değer, bir ürün tanımlayıcısı olan biçiminde `products/{pid}` `{pid}` geçerli bir göreli URL 'dir.|  
|`ProductTitle`|dize|Ürünün adı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`ProductDescription`|dize|Ürünün açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`ProductDetailsUrl`|dize|Ürün ayrıntılarının göreli URL 'SI.|  
|`state`|dize|Aboneliğin durumu. Olası durumlar şunlardır:<br /><br /> - `0 - suspended`– abonelik engellenir ve abone ürünün herhangi bir API 'sini çağıramaz.<br /><br /> - `1 - active`– Abonelik etkin.<br /><br /> - `2 - expired`– abonelik sona erme tarihine ulaştı ve devre dışı bırakıldı.<br /><br /> - `3 - submitted`– abonelik isteği geliştirici tarafından yapıldı, ancak henüz onaylanmamış veya reddedildi.<br /><br /> - `4 - rejected`– abonelik isteği bir yönetici tarafından reddedildi.<br /><br /> - `5 - cancelled`– abonelik, geliştirici veya yönetici tarafından iptal edildi.|  
|`DisplayName`|dize|Aboneliğin görünen adı.|  
|`CreatedDate`|Tarih/saat|Aboneliğin oluşturulduğu tarih, ISO 8601 biçiminde: `2014-06-24T16:25:00Z`.|  
|`CanBeCancelled`|boolean|Aboneliğin geçerli kullanıcı tarafından iptal edilip edilmeyeceğini belirtir.|  
|`IsAwaitingApproval`|boolean|Aboneliğin onay bekliyor olup olmadığı.|  
|`StartDate`|Tarih/saat|Aboneliğin başlangıç tarihi, ISO 8601 biçiminde: `2014-06-24T16:25:00Z`.|  
|`ExpirationDate`|Tarih/saat|Aboneliğin son kullanma tarihi, ISO 8601 biçiminde: `2014-06-24T16:25:00Z`.|  
|`NotificationDate`|Tarih/saat|Aboneliğin bildirim tarihi, ISO 8601 biçiminde: `2014-06-24T16:25:00Z`.|  
|`primaryKey`|dize|Birincil abonelik anahtarı. Maksimum uzunluk 256 karakterdir.|  
|`secondaryKey`|dize|İkincil abonelik anahtarı. Maksimum uzunluk 256 karakterdir.|  
|`CanBeRenewed`|boolean|Aboneliğin geçerli kullanıcı tarafından yenilenebilir olup olmadığı.|  
|`HasExpired`|boolean|Aboneliğin süresi doldu.|  
|`IsRejected`|boolean|Abonelik isteğinin reddedildiğini belirtir.|  
|`CancelUrl`|dize|Aboneliği iptal etmek için göreli URL.|  
|`RenewUrl`|dize|Aboneliği yenilemek için göreli URL.|  
  
##  <a name="SubscriptionSummary"></a>Abonelik Özeti  
 `subscription summary` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|dize|Kaynak tanımlayıcısı. Geçerli API Management hizmet örneği içinde aboneliği benzersiz şekilde tanımlar. Değer, `subscriptions/{sid}` nerede `{sid}` bir abonelik tanımlayıcısı olduğu biçiminde geçerli bir göreli URL 'dir. Bu özellik salt okunurdur.|  
|`DisplayName`|dize|Aboneliğin görünen adı|  
  
##  <a name="UserAccountInfo"></a>Kullanıcı hesabı bilgileri  
 `user account info` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`FirstName`|dize|Ad. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`LastName`|dize|Soyadı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`Email`|dize|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. Maksimum uzunluk 254 karakterdir.|  
|`Password`|dize|Kullanıcı hesabı parolası.|  
|`NameIdentifier`|dize|Hesap tanımlayıcısı, kullanıcı e-postasıdır.|  
|`ProviderName`|dize|Kimlik doğrulama sağlayıcısı adı.|  
|`IsBasicAccount`|boolean|Bu hesap e-posta ve parola kullanılarak kaydedilmişse doğru; hesap bir sağlayıcı kullanılarak kaydedilmişse false.|  
  
##  <a name="UseSignIn"></a>Kullanıcı oturum açma  
 `user sign in` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Email`|dize|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. Maksimum uzunluk 254 karakterdir.|  
|`Password`|dize|Kullanıcı hesabı parolası.|  
|`ReturnUrl`|dize|Kullanıcının oturum açma tıkladığını sayfanın URL 'SI.|  
|`RememberMe`|boolean|Geçerli kullanıcının bilgilerinin kaydedilip edilmeyeceğini belirtir.|  
|`RegistrationEnabled`|boolean|Kaydın etkin olup olmadığı.|  
|`DelegationEnabled`|boolean|Temsilci olarak oturum açma 'nın etkin olup olmadığı.|  
|`DelegationUrl`|dize|Etkinleştirilmişse, temsilci oturum açma URL 'si.|  
|`SsoSignUpUrl`|dize|Varsa, kullanıcının çoklu oturum açma URL 'SI.|  
|`AuxServiceUrl`|dize|Geçerli Kullanıcı bir yöneticise, bu Azure portal hizmet örneğinin bir bağlantıdır.|  
|`Providers`|[Sağlayıcı](#Provider) varlıkları koleksiyonu|Bu Kullanıcı için kimlik doğrulama sağlayıcıları.|  
|`UserRegistrationTerms`|dize|Kullanıcının oturum açmadan önce kabul etmesi gereken koşullar.|  
|`UserRegistrationTermsEnabled`|boolean|Koşulların etkinleştirilip etkinleştirilmeyeceğini belirtir.|  
  
##  <a name="UserSignUp"></a>Kullanıcı kaydı  
 `user sign up` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`PasswordConfirm`|boolean|[Kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi tarafından kullanılan değer.|  
|`Password`|dize|Kullanıcı hesabı parolası.|  
|`PasswordVerdictLevel`|numarası|[Kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi tarafından kullanılan değer.|  
|`UserRegistrationTerms`|dize|Kullanıcının oturum açmadan önce kabul etmesi gereken koşullar.|  
|`UserRegistrationTermsOptions`|numarası|[Kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi tarafından kullanılan değer.|  
|`ConsentAccepted`|boolean|[Kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi tarafından kullanılan değer.|  
|`Email`|dize|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. Maksimum uzunluk 254 karakterdir.|  
|`FirstName`|dize|Ad. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`LastName`|dize|Soyadı. Boş olmamalıdır. Maksimum Uzunluk 100 karakterdir.|  
|`UserData`|dize|[Kaydolma](api-management-page-controls.md#sign-up) denetimi tarafından kullanılan değer.|  
|`NameIdentifier`|dize|[Kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi tarafından kullanılan değer.|  
|`ProviderName`|dize|Kimlik doğrulama sağlayıcısı adı.|

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [şablonları kullanarak API Management Geliştirici Portalını Özelleştirme](api-management-developer-portal-templates.md).
