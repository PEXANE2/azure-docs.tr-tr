---
title: Azure API Yönetimi şablonu veri modeli başvurusu | Microsoft Dokümanlar
description: Azure API Yönetimi'ndeki geliştirici portalı şablonları için veri modellerinde kullanılan ortak öğelerin varlık ve tür gösterimleri hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243945"
---
# <a name="azure-api-management-template-data-model-reference"></a>Azure API Yönetimi şablonu veri modeli başvurusu
Bu konu, Azure API Yönetimi'ndeki geliştirici portalı şablonları için veri modellerinde kullanılan ortak öğelerin varlık ve tür gösterimlerini açıklar.  
  
 Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/)bakın.  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="reference"></a>Başvuru

-   [API](#API)  
-   [API özeti](#APISummary)  
-   [Uygulama](#Application)  
-   [Ek](#Attachment)  
-   [Kod örneği](#Sample)  
-   [Yorum](#Comment)  
-   [Filtreleme](#Filtering)  
-   [Üst bilgi](#Header)  
-   [HTTP İsteği](#HTTPRequest)  
-   [HTTP Yanıt](#HTTPResponse)  
-   [Sorunu](#Issue)  
-   [İşlem](#Operation)  
-   [İşlem menüsü](#Menu)  
-   [İşlem menüsü öğesi](#MenuItem)  
-   [Sayfalama](#Paging)  
-   [Parametre](#Parameter)  
-   [Ürün](#Product)  
-   [Sağlayıcı](#Provider)  
-   [Gösterimi](#Representation)  
-   [Abonelik](#Subscription)  
-   [Abonelik özeti](#SubscriptionSummary)  
-   [Kullanıcı hesabı bilgileri](#UserAccountInfo)  
-   [Kullanıcı oturumu açma](#UseSignIn)  
-   [Kullanıcı kaydolma](#UserSignUp)  
  
##  <a name="api"></a><a name="API"></a>Apı  
 Varlık `API` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`id`|string|Kaynak tanımlayıcısı. Geçerli API Yönetimi hizmeti örneğindeki API'yi benzersiz olarak tanımlar. Değer, API tanımlayıcısının `apis/{id}` bulunduğu biçimde `{id}` geçerli bir göreli URL'dir. Bu özellik salt okunur durumdadır.|  
|`name`|string|API'nin adı. Boş olmamalı. Maksimum uzunluk 100 karakterdir.|  
|`description`|string|API'nin açıklaması. Boş olmamalı. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`serviceUrl`|string|Bu API'yi uygulayan arka uç hizmetinin mutlak URL'si.|  
|`path`|string|Bu API'yi ve API Yönetimi hizmeti örneği içindeki tüm kaynak yollarını benzersiz olarak tanımlayan göreli URL. Bu API için ortak bir URL oluşturmak için hizmet örneği oluşturma sırasında belirtilen API uç nokta temel URL eklenir.|  
|`protocols`|sayı dizisi|Bu API'deki işlemlerin hangi protokollerde çağrılabileceğini açıklar. İzin verilen `1 - http` `2 - https`değerler ve , veya her ikisi de.|  
|`authenticationSettings`|[Yetkilendirme sunucusu kimlik doğrulama ayarları](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Bu API'de yer alan kimlik doğrulama ayarlarının toplanması.|  
|`subscriptionKeyParameterNames`|object|Abonelik anahtarını içeren sorgu ve/veya üstbilgi parametreleri için özel adlar belirtmek için kullanılabilecek isteğe bağlı özellik. Bu özellik mevcut olduğunda, aşağıdaki iki özellikten en az birini içermelidir.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="api-summary"></a><a name="APISummary"></a>API özeti  
 Varlık `API summary` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`id`|string|Kaynak tanımlayıcısı. Geçerli API Yönetimi hizmeti örneğindeki API'yi benzersiz olarak tanımlar. Değer, API tanımlayıcısının `apis/{id}` bulunduğu biçimde `{id}` geçerli bir göreli URL'dir. Bu özellik salt okunur durumdadır.|  
|`name`|string|API'nin adı. Boş olmamalı. Maksimum uzunluk 100 karakterdir.|  
|`description`|string|API'nin açıklaması. Boş olmamalı. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
  
##  <a name="application"></a><a name="Application"></a>Uygulama  
 Varlık `application` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|string|Uygulamanın benzersiz tanımlayıcısı.|  
|`Title`|string|Uygulamanın başlığı.|  
|`Description`|string|Uygulamanın açıklaması.|  
|`Url`|URI|Uygulama için URI.|  
|`Version`|string|Uygulama için sürüm bilgileri.|  
|`Requirements`|string|Uygulama için gereksinimlerin açıklaması.|  
|`State`|number|Uygulamanın geçerli durumu.<br /><br /> - 0 - Kayıtlı<br /><br /> - 1 - Gönderilen<br /><br /> - 2 - Yayınlandı<br /><br /> - 3 - Reddedildi<br /><br /> - 4 - Yayınlanmamış|  
|`RegistrationDate`|DateTime|Başvurunun kaydedildiği tarih ve saat.|  
|`CategoryId`|number|Uygulama kategorisi (Finans, eğlence, vb.)|  
|`DeveloperId`|string|Uygulamayı gönderen geliştiricinin benzersiz tanımlayıcısı.|  
|`Attachments`|[Ek](#Attachment) varlıkların toplanması.|Ekran görüntüleri veya simgeler gibi uygulama için herhangi bir ekleri.|  
|`Icon`|[Ek](#Attachment)|Uygulama için simge.|  
  
##  <a name="attachment"></a><a name="Attachment"></a>Eki  
 Varlık `attachment` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`UniqueId`|string|Ek için benzersiz tanımlayıcı.|  
|`Url`|string|Kaynağın URL'si.|  
|`Type`|string|Ek türü.|  
|`ContentType`|string|Eki medya türü.|  
  
##  <a name="code-sample"></a><a name="Sample"></a>Kod örneği  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`title`|string|Operasyonun adı.|  
|`snippet`|string|Bu özellik amortismana sokulmadı ve kullanılmamalıdır.|  
|`brush`|string|Kod örneğini görüntülerken kullanılacak kod sözdizimi boyama şablonu. İzin verilen `plain` `php`değerler `java` `xml`, `objc` `python`, `ruby`, `csharp`, , , , ve .|  
|`template`|string|Bu kod örnek şablonunun adı.|  
|`body`|string|Parçacığın kod örnek kısmı için bir yer tutucu.|  
|`method`|string|İşlemin HTTP yöntemi.|  
|`scheme`|string|İşlem isteği için kullanılacak protokol.|  
|`path`|string|Operasyonun yolu.|  
|`query`|string|Tanımlı parametrelerle string örneğini sorgula.|  
|`host`|string|Bu işlemi içeren API'nin API Yönetimi hizmet ağ geçidinin URL'si.|  
|`headers`|[Üstbilgi](#Header) varlıkları koleksiyonu.|Bu operasyon için başlıklar.|  
|`parameters`|[Parametre](#Parameter) varlıklarının toplanması.|Bu işlem için tanımlanan parametreler.|  
  
##  <a name="comment"></a><a name="Comment"></a>Yorum  
 Varlık `API` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|number|Yorumun kimliği.|  
|`CommentText`|string|Yorumun gövdesi. HTML içerebilir.|  
|`DeveloperCompany`|string|Geliştiricinin şirket adı.|  
|`PostedOn`|DateTime|Yorumun yayınlandığı tarih ve saat.|  
  
##  <a name="issue"></a><a name="Issue"></a>Sorunu  
 Varlık `issue` aşağıdaki özelliklere sahiptir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|string|Sorun için benzersiz tanımlayıcı.|  
|`ApiID`|string|Bu sorunun bildirilen API kimliği.|  
|`Title`|string|Konunun başlığı.|  
|`Description`|string|Sorunun açıklaması.|  
|`SubscriptionDeveloperName`|string|Sorunu bildiren geliştiricinin adı.|  
|`IssueState`|string|Sorunun geçerli durumu. Olası değerler Önerilir, Açılır, Kapatılır.|  
|`ReportedOn`|DateTime|Sorunun bildirilen tarih ve saat.|  
|`Comments`|[Açıklama](#Comment) varlıklarının toplanması.|Bu konuda yorumlar.|  
|`Attachments`|[Ek](api-management-template-data-model-reference.md#Attachment) varlıkların toplanması.|Soruna herhangi bir ekleri.|  
|`Services`|[API](#API) varlıklarının toplanması.|ApI'ler sorunu dosyalayan kullanıcı tarafından abone.|  
  
##  <a name="filtering"></a><a name="Filtering"></a>Filtreleme  
 Varlık `filtering` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Pattern`|string|Geçerli arama terimi; veya `null` arama terimi yoksa.|  
|`Placeholder`|string|Arama terimi belirtilmediğinde arama kutusunda görüntülenecek metin.|  
  
##  <a name="header"></a><a name="Header"></a>Üstbilgi  
 Bu bölümde `parameter` gösterim açıklanır.  
  
|Özellik|Tür|Açıklama|  
|--------------|-----------------|----------|  
|`name`|string|Parametre adı.|  
|`description`|string|Parametre açıklaması.|  
|`value`|string|Üstbilgi değeri.|  
|`typeName`|string|Üstbilgi değerinin veri türü.|  
|`options`|string|Seçenekler’i seçin.|  
|`required`|boole|Üstbilginin gerekli olup olmadığı.|  
|`readOnly`|boole|Üstbilginin salt okunur olup olmadığı.|  
  
##  <a name="http-request"></a><a name="HTTPRequest"></a>HTTP İstek  
 Bu bölümde `request` gösterim açıklanır.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`description`|string|Operasyon isteği açıklaması.|  
|`headers`|[üstbilgi](#Header) varlıkları dizisi.|Üstbilgi isteyin.|  
|`parameters`|[Parametre](#Parameter) dizisi|Operasyon istek parametrelerinin toplanması.|  
|`representations`|[Temsil](#Representation) dizisi|Operasyon isteği beyanlarının toplanması.|  
  
##  <a name="http-response"></a><a name="HTTPResponse"></a>HTTP Yanıt  
 Bu bölümde `response` gösterim açıklanır.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`statusCode`|pozitif tamsayı|Yanıt durumu kodunu işley.|  
|`description`|string|Operasyon yanıt açıklaması.|  
|`representations`|[Temsil](#Representation) dizisi|Operasyon yanıt gösterimlerinin toplanması.|  
  
##  <a name="operation"></a><a name="Operation"></a>Işlem  
 Varlık `operation` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`id`|string|Kaynak tanımlayıcısı. Geçerli API Yönetimi hizmeti örneği içindeki işlemi benzersiz bir şekilde tanımlar. Değer, API tanımlayıcısının `apis/{aid}/operations/{id}` bulunduğu yerde `{aid}` geçerli bir göreli URL'dir ve `{id}` bir işlem tanımlayıcısıdır. Bu özellik salt okunur durumdadır.|  
|`name`|string|Operasyonun adı. Boş olmamalı. Maksimum uzunluk 100 karakterdir.|  
|`description`|string|Operasyonun açıklaması. Boş olmamalı. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`scheme`|string|Bu API'deki işlemlerin hangi protokollerde çağrılabileceğini açıklar. İzin verilen `http` `https`değerler , `http` `https`, veya her ikisi ve .|  
|`uriTemplate`|string|Bu işlem için hedef kaynağı tanımlayan göreli URL şablonu. Parametreleri içerebilir. Örnek: `customers/{cid}/orders/{oid}/?date={date}`|  
|`host`|string|API'yi barındıran API Yönetimi ağ geçidi URL'si.|  
|`httpMethod`|string|Operasyon HTTP yöntemi.|  
|`request`|[HTTP İsteği](#HTTPRequest)|İstek ayrıntılarını içeren bir varlık.|  
|`responses`|HTTP [Yanıt](#HTTPResponse) dizisi|Operasyon dizisi [HTTP Yanıt](#HTTPResponse) varlıkları.|  
  
##  <a name="operation-menu"></a><a name="Menu"></a>İşlem menüsü  
 Varlık `operation menu` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`ApiId`|string|Geçerli API kimliği.|  
|`CurrentOperationId`|string|Geçerli işlemin kimliği.|  
|`Action`|string|Menü türü.|  
|`MenuItems`|İşlem [menüsü öğesi](#MenuItem) varlıklarının toplanması.|Geçerli API işlemleri.|  
  
##  <a name="operation-menu-item"></a><a name="MenuItem"></a>İşlem menüsü öğesi  
 Varlık `operation menu item` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|string|Operasyonun kimliği.|  
|`Title`|string|Operasyonun açıklaması.|  
|`HttpMethod`|string|İşlemin Http yöntemi.|  
  
##  <a name="paging"></a><a name="Paging"></a>Sayfalama  
 Varlık `paging` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Page`|number|Geçerli sayfa numarası.|  
|`PageSize`|number|Tek bir sayfada görüntülenecek maksimum sonuçlar.|  
|`TotalItemCount`|number|Görüntülenecek öğe sayısı.|  
|`ShowAll`|boole|Tek bir sayfada tüm sonuçları sho olup olmadığını.|  
|`PageCount`|number|Sonuçların sayfa sayısı.|  
  
##  <a name="parameter"></a><a name="Parameter"></a>Parametre  
 Bu bölümde `parameter` gösterim açıklanır.  
  
|Özellik|Tür|Açıklama|  
|--------------|-----------------|----------|  
|`name`|string|Parametre adı.|  
|`description`|string|Parametre açıklaması.|  
|`value`|string|Parametre değeri.|  
|`options`|dize dizisi|Sorgu parametre değerleri için tanımlanan değerler.|  
|`required`|boole|Parametrenin gerekli olup olmadığını belirtir.|  
|`kind`|number|Bu parametre nin bir yol parametresi (1) veya sorgulayıcı parametresi (2) olup olmadığı.|  
|`typeName`|string|Parametre türü.|  
  
##  <a name="product"></a><a name="Product"></a>Ürün  
 Varlık `product` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|string|Kaynak tanımlayıcısı. Geçerli API Yönetimi hizmeti örneğinde ürünü benzersiz bir şekilde tanımlar. Değer, ürün tanımlayıcısının `products/{pid}` bulunduğu yerde `{pid}` geçerli bir göreli URL'dir. Bu özellik salt okunur durumdadır.|  
|`Title`|string|Ürünün adı. Boş olmamalı. Maksimum uzunluk 100 karakterdir.|  
|`Description`|string|Ürünün açıklaması. Boş olmamalı. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`Terms`|string|Ürün kullanım koşulları. Ürüne abone olmaya çalışan geliştiricilere, abonelik işlemini tamamlamadan önce bu koşulları kabul etmeleri gerekir.|  
|`ProductState`|number|Ürünün yayınlanıp yayınlanmayacağını belirtir. Yayınlanan ürünler geliştirici portalında geliştiriciler tarafından keşfedilebilir. Yayımlanmamış ürünler yalnızca yöneticiler tarafından görülebilir.<br /><br /> Ürün durumu için izin verilebilen değerler şunlardır:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|`AllowMultipleSubscriptions`|boole|Bir kullanıcının bu ürüne aynı anda birden çok aboneliği olup olmadığını belirtir.|  
|`MultipleSubscriptionsCount`|number|Bu ürüniçin kullanıcının aynı anda sahip olabileceği maksimum abonelik sayısı.|  
  
##  <a name="provider"></a><a name="Provider"></a>Sağlayıcı  
 Varlık `provider` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Properties`|dize sözlüğü|Bu kimlik doğrulama sağlayıcısının özellikleri.|  
|`AuthenticationType`|string|Sağlayıcı türü. (Azure Active Directory, Facebook giriş, Google Hesabı, Microsoft Hesabı, Twitter).|  
|`Caption`|string|Sağlayıcının görüntü adı.|  
  
##  <a name="representation"></a><a name="Representation"></a>Gösterimi  
 Bu bölümde `representation`bir .  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`contentType`|string|Bu gösterim için kayıtlı veya özel içerik türünü `application/xml`belirtir, örneğin.|  
|`sample`|string|Temsil bir örnek.|  
  
##  <a name="subscription"></a><a name="Subscription"></a>Abonelik  
 Varlık `subscription` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|string|Kaynak tanımlayıcısı. Geçerli API Yönetimi hizmeti örneği içindeki aboneliği benzersiz olarak tanımlar. Değer, abonelik tanımlayıcısının `subscriptions/{sid}` bulunduğu yerde `{sid}` geçerli bir göreli URL'dir. Bu özellik salt okunur durumdadır.|  
|`ProductId`|string|Abone olunan ürünün ürün kaynağı tanımlayıcısı. Değer, ürün tanımlayıcısının `products/{pid}` bulunduğu yerde `{pid}` geçerli bir göreli URL'dir.|  
|`ProductTitle`|string|Ürünün adı. Boş olmamalı. Maksimum uzunluk 100 karakterdir.|  
|`ProductDescription`|string|Ürünün açıklaması. Boş olmamalı. HTML biçimlendirme etiketleri içerebilir. Maksimum uzunluk 1000 karakterdir.|  
|`ProductDetailsUrl`|string|Ürün ayrıntılarına göreli URL.|  
|`state`|string|Aboneliğin durumu. Olası durumlar şunlardır:<br /><br /> - `0 - suspended`– abonelik engellenir ve abone ürünün herhangi bir API'sini arayamaz.<br /><br /> - `1 - active`– abonelik etkindir.<br /><br /> - `2 - expired`– aboneliğin son kullanma tarihine ulaştı ve devre dışı bırakıldı.<br /><br /> - `3 - submitted`– abonelik isteği geliştirici tarafından yapılmış, ancak henüz onaylanmamış veya reddedilmemiştir.<br /><br /> - `4 - rejected`– abonelik isteği bir yönetici tarafından reddedildi.<br /><br /> - `5 - cancelled`– abonelik geliştirici veya yönetici tarafından iptal edilmiştir.|  
|`DisplayName`|string|Aboneliğin görüntü adı.|  
|`CreatedDate`|tarih saat|ABONELIĞIn oluşturulduğu tarih, ISO 8601 `2014-06-24T16:25:00Z`formatında: .|  
|`CanBeCancelled`|boole|Aboneliğin geçerli kullanıcı tarafından iptal edilip edilemeyeceği.|  
|`IsAwaitingApproval`|boole|Aboneliğin onay beklep beklemediği.|  
|`StartDate`|tarih saat|ISO 8601 formatında aboneliğin başlangıç `2014-06-24T16:25:00Z`tarihi: .|  
|`ExpirationDate`|tarih saat|ISO 8601 formatında aboneliğin son `2014-06-24T16:25:00Z`kullanma tarihi: .|  
|`NotificationDate`|tarih saat|ISO 8601 formatında abonelik için `2014-06-24T16:25:00Z`bildirim tarihi: .|  
|`primaryKey`|string|Birincil abonelik anahtarı. Maksimum uzunluk 256 karakterdir.|  
|`secondaryKey`|string|İkincil abonelik anahtarı. Maksimum uzunluk 256 karakterdir.|  
|`CanBeRenewed`|boole|Aboneliğin geçerli kullanıcı tarafından yenilenip yenilenemeyeceği.|  
|`HasExpired`|boole|Aboneliğin süresinin dolup dolmadığı.|  
|`IsRejected`|boole|Abonelik isteğinin reddedilip reddedilmediği.|  
|`CancelUrl`|string|Aboneliği iptal etmek için göreli Url.|  
|`RenewUrl`|string|Aboneliği yenilemek için göreli Url.|  
  
##  <a name="subscription-summary"></a><a name="SubscriptionSummary"></a>Abonelik özeti  
 Varlık `subscription summary` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Id`|string|Kaynak tanımlayıcısı. Geçerli API Yönetimi hizmeti örneği içindeki aboneliği benzersiz olarak tanımlar. Değer, abonelik tanımlayıcısının `subscriptions/{sid}` bulunduğu yerde `{sid}` geçerli bir göreli URL'dir. Bu özellik salt okunur durumdadır.|  
|`DisplayName`|string|Aboneliğin görüntü adı|  
  
##  <a name="user-account-info"></a><a name="UserAccountInfo"></a>Kullanıcı hesabı bilgileri  
 Varlık `user account info` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`FirstName`|string|Adı. Boş olmamalı. Maksimum uzunluk 100 karakterdir.|  
|`LastName`|string|Soyadı. Boş olmamalı. Maksimum uzunluk 100 karakterdir.|  
|`Email`|string|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. Maksimum uzunluk 254 karakterdir.|  
|`Password`|string|Kullanıcı hesabı parolası.|  
|`NameIdentifier`|string|Hesap tanımlayıcısı, kullanıcı e-postasıyla aynıdır.|  
|`ProviderName`|string|Kimlik doğrulama sağlayıcı adı.|  
|`IsBasicAccount`|boole|Bu hesap e-posta ve şifre kullanılarak kaydedilmişse doğrudur; hesap bir sağlayıcı kullanılarak kaydedilmişse yanlış olur.|  
  
##  <a name="user-sign-in"></a><a name="UseSignIn"></a>Kullanıcı oturum açma  
 Varlık `user sign in` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Email`|string|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. Maksimum uzunluk 254 karakterdir.|  
|`Password`|string|Kullanıcı hesabı parolası.|  
|`ReturnUrl`|string|Kullanıcının oturum aç'ı tıklattığı sayfanın URL'si.|  
|`RememberMe`|boole|Geçerli kullanıcı nın bilgilerini kaydedip kaydetmemek.|  
|`RegistrationEnabled`|boole|Kaydın etkin olup olmadığı.|  
|`DelegationEnabled`|boole|Temsilci oturum açma etkinleştirilip etkinleştirilmediği.|  
|`DelegationUrl`|string|Etkinse, temsilci oturum açma url'si.|  
|`SsoSignUpUrl`|string|Varsa, kullanıcı için URL'deki tek işaret.|  
|`AuxServiceUrl`|string|Geçerli kullanıcı bir yöneticiyse, bu Azure portalındaki hizmet örneğine bir bağlantıdır.|  
|`Providers`|[Sağlayıcı](#Provider) varlıkların toplanması|Bu kullanıcı için kimlik doğrulama sağlayıcıları.|  
|`UserRegistrationTerms`|string|Oturum açmadan önce kullanıcının kabul etmesi gereken şartlar.|  
|`UserRegistrationTermsEnabled`|boole|Terimlerin etkin olup olmadığı.|  
  
##  <a name="user-sign-up"></a><a name="UserSignUp"></a>Kullanıcı kaydolun  
 Varlık `user sign up` aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`PasswordConfirm`|boole|[Kayıt](api-management-page-controls.md#sign-up)ol denetimi tarafından kullanılan değer.|  
|`Password`|string|Kullanıcı hesabı parolası.|  
|`PasswordVerdictLevel`|number|[Kayıt](api-management-page-controls.md#sign-up)ol denetimi tarafından kullanılan değer.|  
|`UserRegistrationTerms`|string|Oturum açmadan önce kullanıcının kabul etmesi gereken şartlar.|  
|`UserRegistrationTermsOptions`|number|[Kayıt](api-management-page-controls.md#sign-up)ol denetimi tarafından kullanılan değer.|  
|`ConsentAccepted`|boole|[Kayıt](api-management-page-controls.md#sign-up)ol denetimi tarafından kullanılan değer.|  
|`Email`|string|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. Maksimum uzunluk 254 karakterdir.|  
|`FirstName`|string|Adı. Boş olmamalı. Maksimum uzunluk 100 karakterdir.|  
|`LastName`|string|Soyadı. Boş olmamalı. Maksimum uzunluk 100 karakterdir.|  
|`UserData`|string|[Kayıt](api-management-page-controls.md#sign-up) denetimi tarafından kullanılan değer.|  
|`NameIdentifier`|string|[Kayıt](api-management-page-controls.md#sign-up)ol denetimi tarafından kullanılan değer.|  
|`ProviderName`|string|Kimlik doğrulama sağlayıcı adı.|

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](api-management-developer-portal-templates.md)bakın.
