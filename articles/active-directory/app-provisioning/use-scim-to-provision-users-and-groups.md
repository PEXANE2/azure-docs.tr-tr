---
title: Azure AD'den uygulamalara kullanıcı sağlama için bir SCIM bitiş noktası geliştirme
description: Etki alanları arası Kimlik Yönetimi Sistemi (SCIM), otomatik kullanıcı sağlamayı standartlaştırır. Bir SCIM bitiş noktası geliştirmeyi öğrenin, SCIM API'nizi Azure Active Directory ile tümleştirin ve sağlayan kullanıcıları ve grupları bulut uygulamalarınız halinde otomatikleştirmeye başlayın.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/07/2020
ms.author: mimart
ms.reviewer: arvinh
ms.custom: aaddev;it-pro;seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0507989ec25db595a85b89f15d8ff7d056a970f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297673"
---
# <a name="build-a-scim-endpoint-and-configure-user-provisioning-with-azure-active-directory-azure-ad"></a>Azure Etkin Dizini (Azure AD) ile bir SCIM bitiş noktası oluşturun ve kullanıcı sağlamayı yapılandırma

Bir uygulama geliştiricisi olarak, uygulamanız ile Azure AD arasında kullanıcıların ve grupların otomatik olarak sağlanmasını etkinleştirmek için Etki Alanı Arası Kimlik Yönetimi Sistemi (SCIM) kullanıcı yönetimi API'sini kullanabilirsiniz. Bu makalede, scim bitiş noktası oluşturma ve Azure AD sağlama hizmetiyle tümleştirme nasıl açıklanmaktadır. SCIM belirtimi, sağlama için ortak bir kullanıcı şeması sağlar. SAML veya OpenID Connect gibi federasyon standartlarıyla birlikte kullanıldığında SCIM, yöneticilere erişim yönetimi için uçtan uca, standartlara dayalı bir çözüm sunar.

SCIM iki uç noktanın standartlaştırılmış bir tanımıdır: bir /Users bitiş noktası ve /Gruplar bitiş noktası. Nesneleri oluşturmak, güncelleştirmek ve silmek için yaygın REST fiilleri ve grup adı, kullanıcı adı, ad, soyad ve e-posta gibi yaygın öznitelikler için önceden tanımlanmış bir şema kullanır. SCIM 2.0 REST API sunan uygulamalar, özel bir kullanıcı yönetimi API'si ile çalışmanın acısını azaltabilir veya ortadan kaldırabilir. Örneğin, uyumlu herhangi bir SCIM istemcisi, yeni bir kullanıcı girişi oluşturmak için /Users bitiş noktasına bir JSON nesnesinin HTTP POST'u nasıl yapılacağını bilir. Aynı temel eylemler için biraz daha farklı bir API'ye ihtiyaç duyulması yerine, SCIM standardına uygun uygulamalar önceden varolan istemcilerden, araçlardan ve kodlardan anında yararlanabilir. 

![Azure AD'den SCIM içeren bir uygulamaya sağlama](media/use-scim-to-provision-users-and-groups/scim-provisioning-overview.png)

SCIM 2.0 (RFC [7642](https://tools.ietf.org/html/rfc7642), [7643](https://tools.ietf.org/html/rfc7643), [7644)](https://tools.ietf.org/html/rfc7644)tanımlanan yönetim için standart kullanıcı nesnesi şeması ve dinlenme API'leri kimlik sağlayıcılarının ve uygulamaların birbirleriyle daha kolay entegre olmasını sağlar. SCIM bitiş noktası oluşturan uygulama geliştiricileri, özel çalışma yapmak zorunda kalmadan herhangi bir SCIM uyumlu istemciyle tümleştirilebilir.

Bir uygulamaya sağlamanın otomatikleştirilmesi için, SCIM bitiş noktasının Oluşturulması ve Azure AD SCIM istemcisiyle tümleştirilmesi gerekiyor. Kullanıcıları ve grupları uygulamanızda sağlamabaşlatmak için aşağıdaki adımları gerçekleştirin. 
    
  * **[Adım 1: Kullanıcınızı ve grup şemanızı tasarla.](#step-1-design-your-user-and-group-schema)** Uygulama gereksinimleriniz in nesneleri ve özniteliklerini belirleyin ve Azure AD SCIM uygulaması tarafından desteklenen kullanıcı ve grup şemasını nasıl eşlediklerini belirleyin.

  * **[Adım 2: Azure AD SCIM uygulamasını anlayın.](#step-2-understand-the-azure-ad-scim-implementation)** Azure AD SCIM istemcisinin nasıl uygulandığını anlayın ve SCIM protokolünüzişleme ve yanıtları isteyin' i modelleyin.

  * **[Adım 3: SCIM bitiş noktası oluşturun.](#step-3-build-a-scim-endpoint)** Azure AD sağlama hizmetiyle tümleştirmek için bir bitiş noktası NıN SCIM 2.0 uyumlu olması gerekir. Seçenek olarak, bitiş noktanızı oluşturmak için Microsoft Ortak Dil Altyapısı (CLI) kitaplıklarını ve kod örneklerini kullanabilirsiniz. Bu örnekler sadece başvuru ve test içindir; onlara bağımlı olmak için üretim uygulamanızı kodlamaya karşı öneririz.

  * **[Adım 4: SCIM bitiş noktanızı Azure AD SCIM istemcisiyle tümleştirin.](#step-4-integrate-your-scim-endpoint-with-the-azure-ad-scim-client)** Kuruluşunuz, Azure AD'nin desteklediği SCIM 2.0 profilini uygulayan bir üçüncü taraf uygulaması kullanıyorsa, kullanıcıların ve grupların hem sağlanmasını hem de yok etme sini otomatiklemeye hemen başlayabilirsiniz.

  * **[Adım 5: Uygulamanızı Azure AD uygulama galerisinde yayımlayın.](#step-5-publish-your-application-to-the-azure-ad-application-gallery)** Müşterilerin uygulamanızı keşfetmesini ve sağlamayı kolayca yapılandırmasını kolaylaştırın. 

![SCIM bitiş noktasını Azure AD ile tümleştirme adımları](media/use-scim-to-provision-users-and-groups/process.png)

## <a name="step-1-design-your-user-and-group-schema"></a>Adım 1: Kullanıcınızı ve grup şemanızı tasarla

Her uygulama bir kullanıcı veya grup oluşturmak için farklı öznitelikleri gerektirir. Uygulamanızın gerektirdiği nesneleri (kullanıcılar, gruplar) ve öznitelikleri (ad, yönetici, iş unvanı, vb.) tanımlayarak tümleştirmenizi başlatın. SCIM standardı, kullanıcıları ve grupları yönetmek için bir şema tanımlar. Temel kullanıcı şeması yalnızca üç öznitelik gerektirir: **id** (servis sağlayıcı tanımtanımlayıcısı tanımlı), **externalId** (istemci tanımlı tanımlayıcı) ve **meta** (hizmet sağlayıcısı tarafından tutulan salt okunur meta veriler). Diğer tüm öznitelikler isteğe bağlıdır. Temel kullanıcı şemasına ek olarak, SCIM standardı bir kurumsal kullanıcı uzantısı ve uygulamanızın gereksinimlerini karşılamak üzere kullanıcı şemasını genişletmek için bir model tanımlar. Örneğin, uygulamanız bir kullanıcı yöneticisi gerektiriyorsa, kullanıcının yöneticisini ve temel şemasını toplayıp kullanıcının e-postasını toplamak için kurumsal kullanıcı şemasını kullanabilirsiniz. Şemanızı tasarlamak için aşağıdaki adımları izleyin:
  1. Uygulamanızın gerektirdiği öznitelikleri listele. Gereksinimlerinizi kimlik doğrulama için gerekli özniteliklere (örn. loginName ve e-posta), kullanıcının yaşam döngüsünü yönetmek için gereken özniteliklere (örn. durum / etkin) ve özel uygulamanızın çalışması için gereken diğer özniteliklere (örn. yönetici, etiket) ayırmak yararlı olabilir.
  2. Bu özniteliklerin temel kullanıcı şemasında mı yoksa kurumsal kullanıcı şemasında mı tanımlandığını denetleyin. İhtiyacınız olan ve çekirdek veya kurumsal kullanıcı şemalarında kapsamadığınız öznitelikler varsa, kullanıcı şemasına gereksinim duyduğunuz öznitelikleri kapsayan bir uzantı tanımlamanız gerekir. Aşağıdaki örnekte, kullanıcıya bir kullanıcıüzerinde "etiket" sağlanmasına izin verecek bir uzantı ekledik. Sadece çekirdek ve kurumsal kullanıcı şemaile başlamak ve daha sonra ek özel şemalara genişletmek en iyisidir.  
  3. SCIM özniteliklerini Azure AD'deki kullanıcı özniteliklerine göre eşle. SCIM bitiş noktanızda tanımladığınız özniteliklerden birinin Azure AD kullanıcı şemasında net bir muadili yoksa, verilerin çoğu kiracıda kullanıcı nesnesinde depolanmama olasılığı yüksektir. Bu özniteliğin kullanıcı oluşturmak için isteğe bağlı olup olmadığını göz önünde bulundurun. Öznitelik, uygulamanızın çalışması için kritik önemtaşıyorsa, kiracı yöneticiye şemalarını genişletmesi için rehberlik edin veya "etiketler" özelliği için aşağıda gösterildiği gibi bir uzantı özniteliği kullanın.

### <a name="table-1-outline-the-attributes-that-you-need"></a>Tablo 1: İhtiyacınız olan öznitelikleri anahat 
| Adım 1: Uygulamanızın gerektirdiği öznitelikleri belirleme| Adım 2: SCIM standardına göre harita uygulaması gereksinimleri| Adım 3: Azure AD özniteliklerine SCIM özniteliklerini harita|
|--|--|--|
|Loginname|userName|userPrincipalName|
|firstName|name.givenName|givenName|
|lastName|name.lastName|lastName|
|workMail|E-postalar[yazın eq "iş"].value|Mail|
|manager|manager|manager|
|etiket|urn:ietf:params:scim:schemas:extension:2.0:CustomExtension:etiket|extensionAttribute1|
|durum|Etkin|isSoftDeleted (kullanıcıda depolanmayan hesaplanan değer)|

Yukarıda tanımlanan şema aşağıdaki Json yükü kullanılarak temsil edilecektir. Uygulama için gerekli özniteliklere ek olarak, JSON gösteriminin gerekli "id", "externalId" ve "meta" özniteliklerini içerdiğini unutmayın.

```json
{
     "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User",
      "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User",
      "urn:ietf:params:scim:schemas:extension:CustomExtensionName:2.0:User"],
     "userName":"bjensen",
     "externalId":"bjensen",
     "name":{
       "familyName":"Jensen",
       "givenName":"Barbara"
     },
     "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User": {
     "Manager": "123456"
   },
     "urn:ietf:params:scim:schemas:extension:CustomExtensionName:2.0:CustomAttribute:User": {
     "tag": "701984",
   },
   "meta": {
     "resourceType": "User",
     "created": "2010-01-23T04:56:22Z",
     "lastModified": "2011-05-13T04:42:34Z",
     "version": "W\/\"3694e05e9dff591\"",
     "location":
 "https://example.com/v2/Users/2819c223-7f76-453a-919d-413861904646"
   }
 ```

### <a name="table-2-default-user-attribute-mapping"></a>Tablo 2: Varsayılan kullanıcı öznitelik eşleme
Ardından, uygulamanızın gerektirdiği özniteliklerin Azure AD ve SCIM RFC'deki bir öznitelikle nasıl eşlenebildiğini anlamak için aşağıdaki tabloyu kullanabilirsiniz. Azure AD ile SCIM bitiş noktanız arasında özniteliklerin nasıl eşlenebileceğini [özelleştirebilirsiniz.](customize-application-attributes.md) Hem kullanıcıları hem de grupları veya aşağıda gösterilen tüm öznitelikleri desteklemeniz gerekmediğini unutmayın. Bunlar, Azure AD'deki özniteliklerin SCIM protokolündeki özelliklere sık sık nasıl eşlendirildiklerine yönelik bir başvurudur. 

| Azure Active Directory kullanıcısı | "vazo:ietf:params:scim:schemas:extension:enterprise:2.0:Kullanıcı" |
| --- | --- |
| IsSoftDeleted |Etkin |
|bölüm|urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|
| displayName |displayName |
|Employeeıd|urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|
| Faks-Telefon Numarası |phoneNumbers[eq yazın "faks"].value |
| givenName |name.givenName |
| jobTitle |başlık |
| posta |e-postalar[yazın eq "iş"].value |
| Mailnickname |externalId |
| manager |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager |
| mobil |phoneNumbers[eq yazın "mobil"].value |
| Postakodu |adresleri[yazın eq "iş"].postalCode |
| proxy Adresleri |e-postalar[eq "diğer" yazın]. Değer |
| fiziksel-Teslimat-OfficeName |adresleri[eq "diğer" yazın]. Biçimlendirilmiş |
| Streetaddress |adresleri[yazın eq "iş"].streetAddress |
| surname |name.familyName |
| telefon Numarası |phoneNumbers[yazın eq "iş"].value |
| kullanıcı-PrincipalName |userName |


### <a name="table-3-default-group-attribute-mapping"></a>Tablo 3: Varsayılan grup öznitelik eşleme

| Azure Etkin Dizin grubu | urn:ietf:params:scim:schemas:core:2.0:Grup |
| --- | --- |
| displayName |displayName |
| posta |e-postalar[yazın eq "iş"].value |
| Mailnickname |displayName |
| üyeler |üyeler |
| Objectıd |externalId |
| proxyAddresses |e-postalar[eq "diğer" yazın]. Değer |

SCIM RFC'de tanımlanan birkaç uç nokta vardır. /Kullanıcı bitiş noktası ile başlayıp oradan genişletebilirsiniz. /Şema bitiş noktası, özel öznitelikleri kullanırken veya şemanız sık sık değişirse yararlıdır. İstemcinin en güncel şemayı otomatik olarak almasına olanak tanır. /Bulk bitiş noktası özellikle grupları desteklerken yararlıdır. Aşağıdaki tabloda SCIM standardında tanımlanan çeşitli uç noktalar açıklanmaktadır. /Şema bitiş noktası, özel öznitelikleri kullanırken veya şemanız sık sık değişirse yararlıdır. İstemcinin en güncel şemayı otomatik olarak almasına olanak tanır. /Bulk bitiş noktası özellikle grupları desteklerken yararlıdır. Aşağıdaki tabloda SCIM standardında tanımlanan çeşitli uç noktalar açıklanmaktadır. 
 
### <a name="table-4-determine-the-endpoints-that-you-would-like-to-develop"></a>Tablo 4: Geliştirmek istediğiniz uç noktaları belirleyin
|Bitiş noktası|AÇIKLAMA|
|--|--|
|/Kullanıcı|Bir kullanıcı nesnesi üzerinde CRUD işlemleri gerçekleştirin.|
|/Group|Bir grup nesnesi üzerinde CRUD işlemleri gerçekleştirin.|
|/ServiceProviderConfig|Desteklenen kaynaklar ve kimlik doğrulama yöntemi gibi desteklenen SCIM standardının özellikleri hakkında ayrıntılı bilgi sağlar.|
|/Kaynak Türleri|Her kaynak la ilgili meta verileri belirtir|
|/Şema|Her istemci ve hizmet sağlayıcısı tarafından desteklenen öznitelikler kümesi değişebilir. Bir hizmet sağlayıcısı "ad", "başlık" ve "e-postalar" içerebilirken, başka bir hizmet sağlayıcısı "ad", "başlık" ve "telefon Numaraları" kullanır. Şema uç noktası desteklenen özniteliklerin keşfiiçin izin verir.|
|/Toplu|Toplu işlemler, tek bir işlemde büyük bir kaynak nesnesi koleksiyonunda işlem gerçekleştirmenize olanak sağlar (örn. büyük bir grup için üyelikleri güncelleştirme).|


## <a name="step-2-understand-the-azure-ad-scim-implementation"></a>Adım 2: Azure AD SCIM uygulamasını anlama
> [!IMPORTANT]
> Azure AD SCIM uygulamasının davranışı en son 18 Aralık 2018 tarihinde güncelleştirilmişti. Ne değiştirilen hakkında bilgi için, [Azure AD Kullanıcı Sağlama hizmetinin SCIM 2.0 protokolü uyumluluğuna](application-provisioning-config-problem-scim-compatibility.md)bakın.

SCIM 2.0 kullanıcı yönetimi API'sini destekleyen bir uygulama oluşturuyorsanız, bu bölümde Azure AD SCIM istemcisinin nasıl uygulandığı ayrıntılı olarak açıklanmaktadır. Ayrıca, SCIM protokolü istek işleme ve yanıtlarınızı nasıl modellendireceğimi de gösterir. SCIM bitiş noktanızı uyguladıktan sonra, önceki bölümde açıklanan yordamı izleyerek test edebilirsiniz.

[SCIM 2.0 protokol belirtimi](http://www.simplecloud.info/#Specification)içinde, uygulamanız aşağıdaki gereksinimleri karşılamalıdır:

* [SCIM protokolünün bölüm 3.3'üne](https://tools.ietf.org/html/rfc7644#section-3.3)göre kullanıcı oluşturmayı destekler ve isteğe bağlı olarak da gruplar.  
* [SCIM protokolünün bölüm 3.5.2'ye](https://tools.ietf.org/html/rfc7644#section-3.5.2)göre, kullanıcıları veya grupları PATCH istekleriyle değiştirmeyi destekler.  
* [SCIM protokolünün 3.4.1 bölümüne](https://tools.ietf.org/html/rfc7644#section-3.4.1)göre, daha önce oluşturulan bir kullanıcı veya grup için bilinen bir kaynağın alınmasını destekler.  
* [SCIM protokolünün bölüm 3.4.2'ye](https://tools.ietf.org/html/rfc7644#section-3.4.2)göre kullanıcı veya grupları sorgulamayı destekler.  Varsayılan olarak, kullanıcılar tarafından `id` alınır ve kendi `username` `externalid`ve , tarafından sorgulanır ve gruplar tarafından `displayName`sorgulanır.  
* SCIM protokolünün bölüm 3.4.2'ye göre kullanıcıyı kimlik ve yönetici tarafından sorgulamayı destekler.  
* SCIM protokolünün bölüm 3.4.2'ye göre, grupları kimlik ve üyetarafından sorgulamayı destekler.  
* Azure AD'nin uygulamanıza kimlik doğrulama ve yetkilendirmesi için tek bir taşıyıcı belirteci kabul eder.

Azure AD ile uyumluluk sağlamak için bir SCIM bitiş noktası uygularken aşağıdaki genel yönergeleri izleyin:

* `id`tüm kaynaklar için gerekli bir özelliktir. Bir kaynak döndüren her yanıt, sıfır üye `ListResponse` dışında her kaynağın bu özelliğe sahip olduğundan emin olmalıdır.
* Bir sorgu/filtre isteğine yanıt `ListResponse`her zaman bir .
* Gruplar isteğe bağlıdır, ancak yalnızca SCIM uygulaması PATCH isteklerini destekliyorsa desteklenir.
* Tüm kaynağı NAMA yanıtına dahil etmek gerekli değildir.
* Microsoft Azure AD yalnızca aşağıdaki işleçleri kullanır:  
    - `eq`
    - `and`
* SCIM'deki yapısal elemanlar, özellikle PATCH `op` çalışma değerleri üzerinde, 'de https://tools.ietf.org/html/rfc7644#section-3.5.2tanımlandığı gibi büyük/küçük harf duyarlı bir eşleşme gerektirmez. Azure AD , 'op' `Add` `Replace`olarak , ve `Remove`.
* Microsoft Azure AD, bitiş noktasının ve kimlik bilgilerinin geçerli olduğundan emin olmak için rasgele bir kullanıcı ve grup getirme isteklerinde dir. Ayrıca [Azure portalındaki](https://portal.azure.com)Test **Bağlantısı** akışının bir parçası olarak da yapılır. 
* Kaynakların sorgulanabileceği [öznitelik, Azure portalındaki](https://portal.azure.com)uygulamada eşleşen bir öznitelik olarak ayarlanmalıdır. Daha fazla bilgi için bkz: [Kullanıcı Sağlama Öznitelik Eşlemelerini Özelleştirme](customize-application-attributes.md)

### <a name="user-provisioning-and-deprovisioning"></a>Kullanıcı sağlama ve deprovisioning

Aşağıdaki resimde, Azure Etkin Dizin'inin uygulamanızın kimlik deposundaki bir kullanıcının yaşam döngüsünü yönetmek için bir SCIM hizmetine gönderdiği iletiler gösterilmektedir.  

![Kullanıcı sağlama ve deprovisioning sırasını gösterir](media/use-scim-to-provision-users-and-groups/scim-figure-4.png)<br/>
*Kullanıcı sağlama ve deprovisioning sırası*

### <a name="group-provisioning-and-deprovisioning"></a>Grup sağlama ve deprovisioning

Grup sağlama ve deprovisioning isteğe bağlıdır. Uygulandığında ve etkinleştirildiğinde, aşağıdaki resimde Azure AD'nin uygulamanızın kimlik deposundaki bir grubun yaşam döngüsünü yönetmek için bir SCIM hizmetine gönderdiği iletiler gösterilmektedir.  Bu iletiler, kullanıcılar hakkındaki iletilerden iki şekilde farklıdır:

* Grup alma istekleri, üye özniteliğinin isteğe yanıt olarak sağlanan herhangi bir kaynaktan dışlanacağını belirtir.  
* Bir referans özniteliğinin belirli bir değere sahip olup olmadığını belirleme istekleri, üye özniteliği yle ilgili isteklerdir.  

![Grup sağlama ve deprovisioning sırasını gösterir](media/use-scim-to-provision-users-and-groups/scim-figure-5.png)<br/>
*Grup sağlama ve deprovisioning sırası*

### <a name="scim-protocol-requests-and-responses"></a>SCIM protokol istekleri ve yanıtları
Bu bölümde, Azure AD SCIM istemcisi tarafından yayılan örnek SCIM istekleri ve örnek beklenen yanıtlar sağlanmaktadır. En iyi sonuçlar için, bu biçimde bu istekleri işlemek ve beklenen yanıtları yontmak için uygulamanızı kodlamanız gerekir.

> [!IMPORTANT]
> Azure AD kullanıcı sağlama hizmetinin aşağıda açıklanan işlemleri nasıl ve ne zaman yadığını anlamak için, sağlama nın [nasıl çalıştığına](how-provisioning-works.md)ilişkin [başlangıç ve artımlı bölüme](how-provisioning-works.md#provisioning-cycles-initial-and-incremental) bakın.

[Kullanıcı İşlemleri](#user-operations)
  - [Kullanıcı Oluşturma](#create-user) ([İstek](#request) / [Yanıtı](#response))
  - [Kullanıcı Al](#get-user) ([İstek](#request-1) / [Yanıtı](#response-1))
  - [Sorguya Göre Kullanıcı alma](#get-user-by-query) ([İstek](#request-2) / [Yanıtı](#response-2))
  - [Sorguile Kullanıcı alın - Sıfır sonuç](#get-user-by-query---zero-results) ([İstek](#request-3)
/ [Yanıtı](#response-3))
  - [Kullanıcıyı Güncelleştir [Çok değerli özellikler]](#update-user-multi-valued-properties) ([İstek](#request-4) /  [Yanıtı](#response-4))
  - [Kullanıcıyı Güncelleştir [Tek değerli özellikler]](#update-user-single-valued-properties) ([İstek](#request-5)
/ [Yanıtı](#response-5)) 
  - [Kullanıcıyı Devre Dışı Alabilme](#disable-user) ([İstek](#request-14) / 
[Yanıtı](#response-14))
  - [Kullanıcıyı Sil](#delete-user) ([İstek](#request-6) / 
[Yanıtı](#response-6))


[Grup İşlemleri](#group-operations)
  - [Grup Oluştur](#create-group) ( [İstek](#request-7) / [Yanıtı](#response-7))
  - [Grubu Al](#get-group) ( [İstek](#request-8) / [Yanıtı](#response-8))
  - [DisplayNameye Göre Grup Al](#get-group-by-displayname) ([İstek](#request-9) / [Yanıtı](#response-9))
  - [Güncelleştirme Grubu [Üye olmayan öznitelikler]](#update-group-non-member-attributes) ([İstek](#request-10) /
  [Yanıtı](#response-10))
  - [Grubu Güncelleştir [Üye Ekle]](#update-group-add-members) ( [Yanıt İste](#request-11) /
[)](#response-11)
  - [Güncelleştirme Grubu [Üye Kaldır]](#update-group-remove-members) ( [Yanıt İste](#request-12) /
[)](#response-12)
  - [Grubu Sil](#delete-group) ([İstek](#request-13) /
[Yanıtı](#response-13))

### <a name="user-operations"></a>Kullanıcı İşlemleri

* Kullanıcılar tarafından veya `userName` `email[type eq "work"]` öznitelikleri tarafından sorgulanabilir.  

#### <a name="create-user"></a>Kullanıcı Oluştur

###### <a name="request"></a>İstek

*POST / Kullanıcılar*
```json
{
    "schemas": [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"],
    "externalId": "0a21f0f2-8d2a-4f8e-bf98-7363c4aed4ef",
    "userName": "Test_User_ab6490ee-1e48-479e-a20b-2d77186b5dd1",
    "active": true,
    "emails": [{
        "primary": true,
        "type": "work",
        "value": "Test_User_fd0ea19b-0777-472c-9f96-4f70d2226f2e@testuser.com"
    }],
    "meta": {
        "resourceType": "User"
    },
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName"
    },
    "roles": []
}
```

##### <a name="response"></a>Yanıt

*HTTP/1.1 201 Oluşturuldu*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "48af03ac28ad4fb88478",
    "externalId": "0a21f0f2-8d2a-4f8e-bf98-7363c4aed4ef",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_ab6490ee-1e48-479e-a20b-2d77186b5dd1",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_fd0ea19b-0777-472c-9f96-4f70d2226f2e@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```

#### <a name="get-user"></a>Get User

###### <a name="request"></a><a name="request-1"></a>İstek
*GET /Kullanıcılar/5d48a0a8e9f04aa38008* 

###### <a name="response-user-found"></a><a name="response-1"></a>Yanıt (Kullanıcı bulundu)
*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "5d48a0a8e9f04aa38008",
    "externalId": "58342554-38d6-4ec8-948c-50044d0a33fd",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_feed3ace-693c-4e5a-82e2-694be1b39934",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_22370c1a-9012-42b2-bf64-86099c2a1c22@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```

###### <a name="request"></a>İstek
*GET /Kullanıcı/5171a35d82074e068ce2* 

###### <a name="response-user-not-found-note-that-the-detail-is-not-required-only-status"></a>Yanıt (Kullanıcı bulunamadı. Ayrıntının gerekli olmadığını, yalnızca durum olduğunu unutmayın.)

```json
{
    "schemas": [
        "urn:ietf:params:scim:api:messages:2.0:Error"
    ],
    "status": "404",
    "detail": "Resource 23B51B0E5D7AE9110A49411D@7cca31655d49f3640a494224 not found"
}
```

#### <a name="get-user-by-query"></a>Sorguya göre Kullanıcı alma

##### <a name="request"></a><a name="request-2"></a>İstek

*GET /Users?filter=userName eq "Test_User_dfeef4c5-5681-4387-b016-bdf221e82081"*

##### <a name="response"></a><a name="response-2"></a>Yanıt

*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 1,
    "Resources": [{
        "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
        "id": "2441309d85324e7793ae",
        "externalId": "7fce0092-d52e-4f76-b727-3955bd72c939",
        "meta": {
            "resourceType": "User",
            "created": "2018-03-27T19:59:26.000Z",
            "lastModified": "2018-03-27T19:59:26.000Z"
            
        },
        "userName": "Test_User_dfeef4c5-5681-4387-b016-bdf221e82081",
        "name": {
            "familyName": "familyName",
            "givenName": "givenName"
        },
        "active": true,
        "emails": [{
            "value": "Test_User_91b67701-697b-46de-b864-bd0bbe4f99c1@testuser.com",
            "type": "work",
            "primary": true
        }]
    }],
    "startIndex": 1,
    "itemsPerPage": 20
}

```

#### <a name="get-user-by-query---zero-results"></a>Sorguya göre Kullanıcı alın - Sıfır sonuç

##### <a name="request"></a><a name="request-3"></a>İstek

*GET /Users?filter=userName eq "var olmayan kullanıcı"*

##### <a name="response"></a><a name="response-3"></a>Yanıt

*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 0,
    "Resources": [],
    "startIndex": 1,
    "itemsPerPage": 20
}

```

#### <a name="update-user-multi-valued-properties"></a>Kullanıcıyı Güncelleştirme [Çok değerli özellikler]

##### <a name="request"></a><a name="request-4"></a>İstek

*PATCH /Kullanıcı/6764549bef60420686bc HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [
            {
            "op": "Replace",
            "path": "emails[type eq \"work\"].value",
            "value": "updatedEmail@microsoft.com"
            },
            {
            "op": "Replace",
            "path": "name.familyName",
            "value": "updatedFamilyName"
            }
    ]
}
```

##### <a name="response"></a><a name="response-4"></a>Yanıt

*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "6764549bef60420686bc",
    "externalId": "6c75de36-30fa-4d2d-a196-6bdcdb6b6539",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_fbb9dda4-fcde-4f98-a68b-6c5599e17c27",
    "name": {
        "formatted": "givenName updatedFamilyName",
        "familyName": "updatedFamilyName",
        "givenName": "givenName"
    },
    "active": true,
    "emails": [{
        "value": "updatedEmail@microsoft.com",
        "type": "work",
        "primary": true
    }]
}
```

#### <a name="update-user-single-valued-properties"></a>Kullanıcıyı Güncelleştirme [Tek değerli özellikler]

##### <a name="request"></a><a name="request-5"></a>İstek

*PATCH /Kullanıcılar/5171a35d82074e068ce2 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Replace",
        "path": "userName",
        "value": "5b50642d-79fc-4410-9e90-4c077cdd1a59@testuser.com"
    }]
}
```

##### <a name="response"></a><a name="response-5"></a>Yanıt

*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "5171a35d82074e068ce2",
    "externalId": "aa1eca08-7179-4eeb-a0be-a519f7e5cd1a",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
        
    },
    "userName": "5b50642d-79fc-4410-9e90-4c077cdd1a59@testuser.com",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_49dc1090-aada-4657-8434-4995c25a00f7@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```

### <a name="disable-user"></a>Disable User

##### <a name="request"></a><a name="request-14"></a>İstek

*PATCH /Kullanıcılar/5171a35d82074e068ce2 HTTP/1.1*
```json
{
    "Operations": [
        {
            "op": "Replace",
            "path": "active",
            "value": false
        }
    ],
    "schemas": [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"
    ]
}
```

##### <a name="response"></a><a name="response-14"></a>Yanıt

```json
{
    "schemas": [
        "urn:ietf:params:scim:schemas:core:2.0:User"
    ],
    "id": "CEC50F275D83C4530A495FCF@834d0e1e5d8235f90a495fda",
    "userName": "deanruiz@testuser.com",
    "name": {
        "familyName": "Harris",
        "givenName": "Larry"
    },
    "active": false,
    "emails": [
        {
            "value": "gloversuzanne@testuser.com",
            "type": "work",
            "primary": true
        }
    ],
    "addresses": [
        {
            "country": "ML",
            "type": "work",
            "primary": true
        }
    ],
    "meta": {
        "resourceType": "Users",
        "location": "/scim/5171a35d82074e068ce2/Users/CEC50F265D83B4530B495FCF@5171a35d82074e068ce2"
    }
}
```
#### <a name="delete-user"></a>Kullanıcıyı Silme

##### <a name="request"></a><a name="request-6"></a>İstek

*DELETE /Kullanıcılar/5171a35d82074e068ce2 HTTP/1.1*

##### <a name="response"></a><a name="response-6"></a>Yanıt

*HTTP/1.1 204 İçerik Yok*

### <a name="group-operations"></a>Grup İşlemleri

* Gruplar her zaman boş bir üye listesi ile oluşturulacaktır.
* Gruplar öznitelik tarafından `displayName` sorgulanabilir.
* Grup PATCH isteğine yapılan güncellemeyanıtta *bir HTTP 204 İçerik içermez.* Tüm üyelerin bir listesini içeren bir organı iade etmek tavsiye edilmez.
* Grubun tüm üyelerinin geri dönmesini desteklemeye gerek yoktur.

#### <a name="create-group"></a>Grup Oluşturma

##### <a name="request"></a><a name="request-7"></a>İstek

*POST /Gruplar HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group", "http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/2.0/Group"],
    "externalId": "8aa1a0c0-c4c3-4bc0-b4a5-2ef676900159",
    "displayName": "displayName",
    "meta": {
        "resourceType": "Group"
    }
}
```

##### <a name="response"></a><a name="response-7"></a>Yanıt

*HTTP/1.1 201 Oluşturuldu*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
    "id": "927fa2c08dcb4a7fae9e",
    "externalId": "8aa1a0c0-c4c3-4bc0-b4a5-2ef676900159",
    "meta": {
        "resourceType": "Group",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
        
    },
    "displayName": "displayName",
    "members": []
}
```

#### <a name="get-group"></a>Get Group

##### <a name="request"></a><a name="request-8"></a>İstek

*GET /Groups/40734ae655284ad3abcc?excludedAttributes=üye http/1.1*

##### <a name="response"></a><a name="response-8"></a>Yanıt
*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
    "id": "40734ae655284ad3abcc",
    "externalId": "60f1bb27-2e1e-402d-bcc4-ec999564a194",
    "meta": {
        "resourceType": "Group",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "displayName": "displayName",
}
```

#### <a name="get-group-by-displayname"></a>DisplayName'ye Göre Grup Al

##### <a name="request"></a><a name="request-9"></a>İstek
*GET /Groups?excludedAttributes=üye&filter=displayName eq "displayName" HTTP/1.1*

##### <a name="response"></a><a name="response-9"></a>Yanıt

*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 1,
    "Resources": [{
        "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
        "id": "8c601452cc934a9ebef9",
        "externalId": "0db508eb-91e2-46e4-809c-30dcbda0c685",
        "meta": {
            "resourceType": "Group",
            "created": "2018-03-27T22:02:32.000Z",
            "lastModified": "2018-03-27T22:02:32.000Z",
            
        },
        "displayName": "displayName",
    }],
    "startIndex": 1,
    "itemsPerPage": 20
}
```

#### <a name="update-group-non-member-attributes"></a>Grubu Güncelleştirme [Üye olmayan öznitelikler]

##### <a name="request"></a><a name="request-10"></a>İstek

*Patch /Gruplar/fa2ce26709934589afc5 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Replace",
        "path": "displayName",
        "value": "1879db59-3bdf-4490-ad68-ab880a269474updatedDisplayName"
    }]
}
```

##### <a name="response"></a><a name="response-10"></a>Yanıt

*HTTP/1.1 204 İçerik Yok*

### <a name="update-group-add-members"></a>Grubu Güncelleştir [Üye Ekle]

##### <a name="request"></a><a name="request-11"></a>İstek

*YAMA /Gruplar/a99962b9f99d4c4fac67 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Add",
        "path": "members",
        "value": [{
            "$ref": null,
            "value": "f648f8d5ea4e4cd38e9c"
        }]
    }]
}
```

##### <a name="response"></a><a name="response-11"></a>Yanıt

*HTTP/1.1 204 İçerik Yok*

#### <a name="update-group-remove-members"></a>Grubu Güncelleştir [Üye Kaldır]

##### <a name="request"></a><a name="request-12"></a>İstek

*YAMA /Gruplar/a99962b9f99d4c4fac67 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Remove",
        "path": "members",
        "value": [{
            "$ref": null,
            "value": "f648f8d5ea4e4cd38e9c"
        }]
    }]
}
```

##### <a name="response"></a><a name="response-12"></a>Yanıt

*HTTP/1.1 204 İçerik Yok*

#### <a name="delete-group"></a>Grubu Silme

##### <a name="request"></a><a name="request-13"></a>İstek

*DELETE /Groups/cdb1ce18f65944079d37 HTTP/1.1*

##### <a name="response"></a><a name="response-13"></a>Yanıt

*HTTP/1.1 204 İçerik Yok*

### <a name="security-requirements"></a>Güvenlik gereksinimleri
**TLS Protokol Sürümleri**

Kabul edilebilir tek TLS protokol sürümleri TLS 1.2 ve TLS 1.3'tür. TLS'nin başka hiçbir sürümüne izin verilmez. SSL sürümüne izin verilmez. 
- RSA tuşları en az 2.048 bit olmalıdır.
- ECC tuşları onaylı bir eliptik eğri kullanılarak oluşturulan en az 256 bit olmalıdır


**Anahtar Uzunlukları**

Tüm hizmetler, yeterli uzunluktaki şifreleme anahtarları kullanılarak oluşturulan X.509 sertifikalarını kullanmalıdır, yani:

**Cipher Süitler**

Tüm hizmetler, aşağıda belirtilen sırayla aşağıdaki şifre paketlerini kullanacak şekilde yapılandırılmalıdır. Yalnızca rsa sertifikanız varsa, ECDSA şifre paketlerinin yüklü olmasının herhangi bir etkisi olmadığını unutmayın. </br>

TLS 1.2 Cipher Suites minimum bar:

- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384


## <a name="step-3-build-a-scim-endpoint"></a>Adım 3: SCIM bitiş noktası oluşturma

Şemanızı tasarladığınızda ve Azure AD SCIM uygulamasını anladığınıza göre, SCIM bitiş noktanızı geliştirmeye başlayabilirsiniz. Sıfırdan başlayıp uygulamayı tamamen kendi üzerine oluşturmak yerine, SCIM topluluğu tarafından yayınlanan bir dizi açık kaynak lı SCIM kitaplığına güvenebilirsiniz.

Azure AD sağlama ekibi tarafından yayınlanan açık kaynak .NET Core [başvuru kodu,](https://aka.ms/SCIMReferenceCode) geliştirmenizi hızlandırabilecek bu tür bir kaynaktır. SCIM bitiş noktanızı oluşturmuşsunuz da, test etmek isteyeceksiniz. Referans kodunun bir parçası olarak sağlanan [postacı testlerinin](https://github.com/AzureAD/SCIMReferenceCode/wiki/Test-Your-SCIM-Endpoint) koleksiyonunu kullanabilir veya [yukarıda](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#user-operations)verilen örnek istekler / yanıtlar üzerinden çalıştırabilirsiniz.  

   > [!Note]
   > Referans kodu, SCIM bitiş noktanızı oluşturmaya başlamanıza yardımcı olmak için tasarlanmıştır ve "AS IS" sağlanır. Topluluğun katkıları, kodun oluşturulmasına ve korunmasına yardımcı olabilir.

Çözüm iki proje, _Microsoft.SCIM_ ve _Microsoft.SCIM.WebHostSample_oluşur.

_Microsoft.SCIM_ projesi, WEB hizmetinin SCIM belirtimine uygun bileşenleritanımlayan kitaplıktır. Bu arabirim _Microsoft.SCIM.IProvider_bildirir, istekleri bir kimlik deposunda çalışmak üzere programlanmış olacak sağlayıcının yöntemleri, aramalar açevirilir.

![Arıza: Sağlayıcının yöntemlerine yapılan çağrılara çevrilmiş bir istek](media/use-scim-to-provision-users-and-groups/scim-figure-3.png)

_Microsoft.SCIM.WebHostSample_ _projesi, Boş_ şablonu temel alan bir Visual Studio ASP.NET Core Web Uygulamasıdır. Bu, örnek kodun tek başına, kapsayıcılarda veya Internet Bilgi Hizmetleri içinde barındırılan olarak dağıtılmasına olanak tanır. Ayrıca, sınıfları örnek bir kimlik deposu olarak bellekte tutan _Microsoft.SCIM.IProvider_ arabirimini de uygular.

```csharp
    public class Startup
    {
        ...
        public IMonitor MonitoringBehavior { get; set; }
        public IProvider ProviderBehavior { get; set; }

        public Startup(IWebHostEnvironment env, IConfiguration configuration)
        {
            ...
            this.MonitoringBehavior = new ConsoleMonitor();
            this.ProviderBehavior = new InMemoryProvider();
        }
        ...
```

### <a name="building-a-custom-scim-endpoint"></a>Özel bir SCIM bitiş noktası oluşturma

SCIM hizmeti, kök sertifika yetkilisinin aşağıdaki adlardan biri olduğu bir HTTP adresine ve sunucu kimlik doğrulama sertifikasına sahip olmalıdır:

* Cnnıc
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* Globalsign
* Git Baba
* Verisign
* WoSign

.NET Core SDK geliştirme sırasında kullanılabilecek bir HTTPS geliştirme sertifikası içerir, sertifika ilk çalıştırma deneyiminin bir parçası olarak yüklenir. ASP.NET Core Web Uygulamasını nasıl çalıştırdığınıza bağlı olarak farklı bir bağlantı noktasını dinleyecek:

* Microsoft.SCIM.WebHostSample:https://localhost:5001
* IIS Express:https://localhost:44359/

ASP.NET Core'da HTTPS hakkında daha fazla bilgi için aşağıdaki bağlantıyı kullanın: [ASP.NET Core'da HTTPS'yi uygulayın](https://docs.microsoft.com/aspnet/core/security/enforcing-ssl)

### <a name="handling-endpoint-authentication"></a>Uç nokta kimlik doğrulaması işleme

Azure Etkin Dizin'inden gelen istekler arasında OAuth 2.0 taşıyıcı belirteci içerir. İsteğe bağlı hizmeti alan herhangi bir hizmet, verenin beklenen Azure Etkin Dizin ideninin Azure Etkin Dizini olduğunu doğrulamalıdır.

Belirteci olarak, veren bir iss iddiası ile `"iss":"https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/"`tanımlanır, gibi . Bu örnekte, talep değerinin temel `https://sts.windows.net`adresi, Azure Etkin Dizini'ni veren kuruluş olarak tanımlarken, göreli adres kesimi _cbb1a5ac-f33b-45fa-9bf5-f37db0fed422,_ belirteç verilen Azure Active Directory kiracısının benzersiz bir tanımlayıcısıdır.

Belirteci için hedef kitle galeride uygulama için uygulama şablonu kimliği olacak, tek bir kiracı `iss` kayıtlı uygulamaların her SCIM istekleri ile aynı talep alabilir. Galerideki her uygulama için uygulama şablonu kimliği [ProvisioningFeedback@microsoft.com](mailto:ProvisioningFeedback@microsoft.com) değişir, lütfen bir galeri uygulaması için uygulama şablonu kimliği yle ilgili sorular için irtibata geçiniz. Tüm özel uygulamalar için uygulama şablonu kimliği _8adf8e6e-67b2-4cf2-a259-e3dc5476c621_olduğunu.

Örnek kodda, isteklerin kimliği Microsoft.AspNetCore.Authentication.JwtBearer paketi kullanılarak doğrulanır. Aşağıdaki kod, hizmetin uç noktalarından herhangi birinde yapılan isteklerin, Azure Active Directory tarafından belirli bir kiracı için verilen taşıyıcı belirteci kullanılarak doğrulanmasına izin verir:

```csharp
        public void ConfigureServices(IServiceCollection services)
        {
            if (_env.IsDevelopment())
            {
                ...
            }
            else
            {
                services.AddAuthentication(options =>
                {
                    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
                    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
                    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
                })
                    .AddJwtBearer(options =>
                    {
                        options.Authority = " https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/";
                        options.Audience = "8adf8e6e-67b2-4cf2-a259-e3dc5476c621";
                        ...
                    });
            }
            ...
        }

        public void Configure(IApplicationBuilder app)
        {
            ...
            app.UseAuthentication();
            app.UseAuthorization();
            ...
       }
```

Bir taşıyıcı belirteci de sağlanan [postman testleri](https://github.com/AzureAD/SCIMReferenceCode/wiki/Test-Your-SCIM-Endpoint) kullanmak ve localhost kullanarak yerel hata ayıklama gerçekleştirmek için gereklidir. Örnek kod, geliştirme aşamasında kimlik doğrulama seçeneklerini değiştirmek ve kendi imzaladığı bir belirteç kullanımını etkinleştirmek için ASP.NET Çekirdek ortamlarını kullanır.

ASP.NET Core'daki birden fazla ortam hakkında daha fazla bilgi için aşağıdaki bağlantıyı kullanın: [ASP.NET Core'da birden fazla ortam kullanın](
https://docs.microsoft.com/aspnet/core/fundamentals/environments)

Aşağıdaki kod, hizmetin uç noktalarından herhangi birinde yapılan isteklerin, özel bir anahtarla imzalanmış bir taşıyıcı belirteci kullanılarak doğrulanmasını zorunlu kılır:

```csharp
        public void ConfigureServices(IServiceCollection services)
        {
            if (_env.IsDevelopment())
            {
                services.AddAuthentication(options =>
                {
                    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
                    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
                    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
                })
                    .AddJwtBearer(options =>
                    {
                        options.TokenValidationParameters =
                            new TokenValidationParameters
                            {
                                ValidateIssuer = false,
                                ValidateAudience = false,
                                ValidateLifetime = false,
                                ValidateIssuerSigningKey = false,
                                ValidIssuer = "Microsoft.Security.Bearer",
                                ValidAudience = "Microsoft.Security.Bearer",
                                IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("A1B2C3D4E5F6A1B2C3D4E5F6"))
                            };
                    });
            }
        ...
```

Geçerli bir taşıyıcı belirteci almak için Token denetleyicisine GET isteği gönderin, _GenerateJSONWebToken_ yöntemi geliştirme için yapılandırılan parametrelerle eşleşen bir belirteç oluşturmak için sorumludur:

```csharp
        private string GenerateJSONWebToken()
        {
            // Create token key
            SymmetricSecurityKey securityKey =
                new SymmetricSecurityKey(Encoding.UTF8.GetBytes("A1B2C3D4E5F6A1B2C3D4E5F6"));
            SigningCredentials credentials =
                new SigningCredentials(securityKey, SecurityAlgorithms.HmacSha256);

            // Set token expiration
            DateTime startTime = DateTime.UtcNow;
            DateTime expiryTime = startTime.AddMinutes(120);

            // Generate the token
            JwtSecurityToken token =
                new JwtSecurityToken(
                    "Microsoft.Security.Bearer",
                    "Microsoft.Security.Bearer",
                    null,
                    notBefore: startTime,
                    expires: expiryTime,
                    signingCredentials: credentials);

            string result = new JwtSecurityTokenHandler().WriteToken(token);
            return result;
        }
```

### <a name="handling-provisioning-and-deprovisioning-of-users"></a>Kullanıcıların sağlanması ve bunların sağlanmasının işlenmesi

***Örnek 1. Eşleşen bir kullanıcı için hizmeti sorgula***

Azure Active Directory, Azure AD'deki bir kullanıcının mailNickname öznitelik değeriyle eşleşen harici Id özniteliği değerine sahip bir kullanıcı için hizmeti sorgular. Sorgu, bu örnek gibi bir Hypertext Aktarım Protokolü (HTTP) isteği olarak ifade edilir ve jyoung, Azure Etkin Dizini'ndeki bir kullanıcının mailNickname'ının bir örneğidir.

>[!NOTE]
> Bu sadece bir örnektir. Tüm kullanıcılar bir mailNickname özniteliğine sahip değildir ve bir kullanıcının sahip olduğu değer dizinde benzersiz olmayabilir. Ayrıca, eşleştirme için kullanılan öznitelik (bu durumda externalId) [Azure AD öznitelik eşlemelerinde](customize-application-attributes.md)yapılandırılabilir.

```
GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
 Authorization: Bearer ...
```

Örnek kodda istek, hizmet sağlayıcısının QueryAsync yöntemine bir çağrıya çevrilir. İşte bu yöntemin imzası: 

```csharp
 // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
 // Microsoft.SCIM.IRequest is defined in 
 // Microsoft.SCIM.Service.  
 // Microsoft.SCIM.Resource is defined in 
 // Microsoft.SCIM.Schemas.  
 // Microsoft.SCIM.IQueryParameters is defined in 
 // Microsoft.SCIM.Protocol.  

 Task<Resource[]> QueryAsync(IRequest<IQueryParameters> request);
```

Örnek sorguda, dışsal Id özniteliği için belirli bir değere sahip bir kullanıcı için QueryAsync yöntemine geçirilen bağımsız değişkenlerin değerleri şunlardır:

* Parametre. AlternatifFiltreler.Sayısı: 1
* Parametre. AlternativeFilters.ElementAt(0). ÖznitelikPath: "externalId"
* Parametre. AlternativeFilters.ElementAt(0). ComparisonOperator: ComparisonOperator.Equals
* Parametre. AlternateFilter.Elementat(0). ComparisonValue: "jyoung"

***Örnek 2. Kullanıcısağlama***

Bir kullanıcının mailNickname öznitelik değeriyle eşleşen harici Id öznitelik değerine sahip bir kullanıcı için web hizmetine verilen bir sorguya verilen yanıt herhangi bir kullanıcıyı döndürmüyorsa, Azure Etkin Dizini hizmet insasının bir kullanıcıya karşılık gelen bir kullanıcıyı sağlamasını isterse Azure Etkin Dizini'nde.  İşte böyle bir istek örneği: 

```
 POST https://.../scim/Users HTTP/1.1
 Authorization: Bearer ...
 Content-type: application/scim+json
 {
   "schemas":
   [
     "urn:ietf:params:scim:schemas:core:2.0:User",
     "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
   "externalId":"jyoung",
   "userName":"jyoung",
   "active":true,
   "addresses":null,
   "displayName":"Joy Young",
   "emails": [
     {
       "type":"work",
       "value":"jyoung@Contoso.com",
       "primary":true}],
   "meta": {
     "resourceType":"User"},
    "name":{
     "familyName":"Young",
     "givenName":"Joy"},
   "phoneNumbers":null,
   "preferredLanguage":null,
   "title":null,
   "department":null,
   "manager":null}
```

Örnek kodda istek, hizmet sağlayıcısının CreateAsync yöntemine bir çağrıya çevrilir. İşte bu yöntemin imzası: 

```csharp
 // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
 // Microsoft.SCIM.IRequest is defined in 
 // Microsoft.SCIM.Service.  
 // Microsoft.SCIM.Resource is defined in 
 // Microsoft.SCIM.Schemas.  

 Task<Resource> CreateAsync(IRequest<Resource> request);
```

Bir kullanıcıyı sağlama isteğinde, kaynak bağımsız değişkeninin değeri Microsoft.SCIM.Schemas kitaplığında tanımlanan Microsoft.SCIM.Core2EnterpriseUser sınıfının bir örneğidir.  Kullanıcıyı sağlama isteği başarılı olursa, yöntemin uygulanmasının Microsoft.SCIM.Core2EnterpriseUser sınıfının bir örneğini döndürür ve tanımlayıcı özelliğinin değeri yeni sağlanan ın benzersiz tanımlayıcısına ayarlanmıştır Kullanıcı.  

***Örnek 3. Kullanıcının geçerli durumunu sorgula*** 

Bir SCIM tarafından ön planda bulunan bir kimlik deposunda bulunduğu bilinen bir kullanıcıyı güncelleştirmek için Azure Active Directory, söz konusu kullanıcının geçerli durumunu hizmetten aşağıdaki gibi bir istekle isteyerek devam eder: 

```
 GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
 Authorization: Bearer ...
```

Örnek kodda istek, hizmet sağlayıcısının RetrieveAsync yöntemine yapılan bir çağrıya çevrilir. İşte bu yöntemin imzası: 

```csharp
 // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
 // Microsoft.SCIM.IRequest is defined in 
 // Microsoft.SCIM.Service.  
 // Microsoft.SCIM.Resource and 
 // Microsoft.SCIM.IResourceRetrievalParameters 
 // are defined in Microsoft.SCIM.Schemas 

 Task<Resource> RetrieveAsync(IRequest<IResourceRetrievalParameters> request);
```

Bir kullanıcının geçerli durumunu almak için bir istek örneği, parametreleri argüman değeri olarak sağlanan nesnenin özelliklerinin değerleri şunlardır: 
  
* Tanımlayıcı: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "vazo:ietf:params:scim:şema:uzantı:kurumsal:2.0:Kullanıcı"

***Örnek 4. Güncellenecek bir başvuru özniteliğinin değerini sorgula*** 

Bir başvuru özniteliği güncellenecekse, Azure Etkin Dizini, hizmetin önündeki kimlik deposundaki başvuru özniteliğinin geçerli değerinin Azure Etkin'deki bu özniteliğin değeriyle zaten eşleşip eşleşmediğini belirlemek için hizmeti sorgular Dizin. Kullanıcılar için, geçerli değerin bu şekilde sorgulandığı tek öznitelik yönetici özniteliğidir. Aşağıda, bir kullanıcı nesnesinin yönetici atasözünün şu anda belirli bir değere sahip olup olmadığını belirlemek için bir istek örneği verilmiştir: Örnek kodda istek, hizmet sağlayıcısının QueryAsync yöntemine bir çağrıya dönüştürülür. Parametreler bağımsız değişkeninin değeri olarak sağlanan nesnenin özelliklerinin değeri aşağıdaki gibidir: 
  
* Parametre. AlternatifFiltreler.Sayısı: 2
* Parametre. AlternatifFiltreler.ElementAt(x). ÖznitelikPath: "KIMLIK"
* Parametre. AlternatifFiltreler.ElementAt(x). ComparisonOperator: ComparisonOperator.Equals
* Parametre. AlternatifFilter.ElementAt(x). Karşılaştırma Değeri: "54D382A4-2050-4C03-94D1-E769F1D15682"
* Parametre. AlternativeFilters.ElementAt(y). AttributePath: "yönetici"
* Parametre. AlternativeFilters.ElementAt(y). ComparisonOperator: ComparisonOperator.Equals
* Parametre. AlternatifFilter.ElementAt(y). Karşılaştırma Değeri: "2819c223-7f76-453a-919d-413861904646"
* Parametre. İstenenAttributePaths.ElementAt(0): "ID"
* Parametre. SchemaIdentifier: "vazo:ietf:params:scim:şema:uzantı:kurumsal:2.0:Kullanıcı"

Burada, dizin x değeri 0, dizin y değeri 1, x değeri 1, y değeri filtre sorgu parametresinin ifadelerinin sırasına bağlı olarak 0 olabilir.   

***Örnek 5. Bir kullanıcıyı güncelleştirmek için Azure AD'den SCIM hizmetine istek*** 

Aşağıda, Azure Etkin Dizin'inden bir kullanıcıyı güncelleştirmek için bir SCIM hizmetine yapılan bir isteğin bir örneği verilmiştir: 

```
  PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
  Authorization: Bearer ...
  Content-type: application/scim+json
  {
    "schemas": 
    [
      "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations":
    [
      {
        "op":"Add",
        "path":"manager",
        "value":
          [
            {
              "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
              "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
```

Örnek kodda istek, hizmet sağlayıcısının UpdateAsync yöntemine yapılan bir çağrıya çevrilir. İşte bu yöntemin imzası: 

```csharp
 // System.Threading.Tasks.Tasks and 
 // System.Collections.Generic.IReadOnlyCollection<T>  // are defined in mscorlib.dll.  
 // Microsoft.SCIM.IRequest is defined in
 // Microsoft.SCIM.Service.
 // Microsoft.SCIM.IPatch, 
 // is defined in Microsoft.SCIM.Protocol. 

 Task UpdateAsync(IRequest<IPatch> request);
```

Bir kullanıcıyı güncelleştirme isteği örneğinde, yama bağımsız değişkeninin değeri olarak sağlanan nesnenin şu özellik değerleri vardır: 
  
* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (PatchRequest olarak PatchRequest2). Operations.Count: 1
* (PatchRequest olarak PatchRequest2). Operations.Elementat(0). Operasyon Adı: OperationName.Add
* (PatchRequest olarak PatchRequest2). Operations.Elementat(0). Path.AttributePath: "yönetici"
* (PatchRequest olarak PatchRequest2). Operations.Elementat(0). Değer.Sayısı: 1
* (PatchRequest olarak PatchRequest2). Operations.Elementat(0). Value.Elementat(0). Referans: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (PatchRequest olarak PatchRequest2). Operations.Elementat(0). Value.Elementat(0). Değer: 2819c223-7f76-453a-919d-413861904646

***Örnek 6. Bir kullanıcıyı sağlamasilme***

Azure AD, bir scim hizmetinin önündeki bir kimlik mağazasından kullanıcıyı çıkarmak için şu gibi bir istek gönderir:

```
  DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
  Authorization: Bearer ...
```

Örnek kodda istek, hizmet sağlayıcısının DeleteAsync yöntemine bir çağrıya çevrilir. İşte bu yöntemin imzası: 

```csharp
 // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
 // Microsoft.SCIM.IRequest is defined in 
 // Microsoft.SCIM.Service.  
 // Microsoft.SCIM.IResourceIdentifier, 
 // is defined in Microsoft.SCIM.Protocol. 

 Task DeleteAsync(IRequest<IResourceIdentifier> request);
```

Kaynak Identifier bağımsız değişkeninin değeri olarak sağlanan nesne, bir kullanıcıyı sağlama isteği örneğinde şu özellik değerlerine sahiptir: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="step-4-integrate-your-scim-endpoint-with-the-azure-ad-scim-client"></a>Adım 4: SCIM bitiş noktanızı Azure AD SCIM istemcisiyle tümleştirme

Azure AD, atanan kullanıcıları ve grupları [SCIM 2.0 protokolünün](https://tools.ietf.org/html/rfc7644)belirli bir profilini uygulayan uygulamalara otomatik olarak sağlayacak şekilde yapılandırılabilir. Profilin özellikleri [Adım 2: Azure AD SCIM uygulamasını anlama'da](#step-2-understand-the-azure-ad-scim-implementation)belgelenmiştir.

Bu gereksinimlerle uyumluluk bildirimleri için uygulama sağlayıcınıza veya uygulama sağlayıcınızın belgelerine danışın.

> [!IMPORTANT]
> Azure AD SCIM uygulaması, kullanıcıları Azure AD ile hedef uygulama arasında sürekli olarak senkronize tutmak üzere tasarlanmış ve çok özel bir standart işlem kümesi ni uygulayan Azure AD kullanıcı sağlama hizmetinin üzerine inşa edilmiştir. Azure AD SCIM istemcisinin davranışını anlamak için bu davranışları anlamak önemlidir. Daha fazla bilgi için, sağlama döngüleri bölümüne [bakın: İlk ve artımlı](how-provisioning-works.md#provisioning-cycles-initial-and-incremental) nasıl [sağlama çalışır.](how-provisioning-works.md)

### <a name="getting-started"></a>Başlarken

Bu makalede açıklanan SCIM profilini destekleyen uygulamalar, Azure AD uygulama galerisindeki "galeri dışı uygulama" özelliğini kullanarak Azure Etkin Dizini'ne bağlanabilir. Azure AD, bağlandıktan sonra, atanan kullanıcılar ve gruplar için uygulamanın SCIM bitiş noktasını sorguladığı ve bunları atama ayrıntılarına göre oluşturduğu veya değiştiren bir eşitleme işlemi yürütmektedir.

**SCIM'i destekleyen bir uygulamayı bağlamak için:**

1. Azure Active [Directory portalında](https://aad.portal.azure.com)oturum açın. Geliştirici programına kaydolarak P2 lisansları ile Azure Active Directory için ücretsiz deneme [sürümüne](https://developer.microsoft.com/office/dev-program) erişebileceğinizi unutmayın
2. Sol bölmeden **Enterprise uygulamalarını** seçin. Galeriden eklenen uygulamalar da dahil olmak üzere, yapılandırılan tüm uygulamaların listesi gösterilir.
3. Seçin **+ Yeni uygulama** > **Tüm** > **Galeri Dışı uygulama**.
4. Uygulamanız için bir ad girin ve uygulama nesnesi oluşturmak için **Ekle'yi** seçin. Yeni uygulama kurumsal uygulamalar listesine eklenir ve uygulama yönetimi ekranına açılır.

   ![Ekran görüntüsü Azure REKLAM uygulama galerisini gösterir](media/use-scim-to-provision-users-and-groups/scim-figure-2a.png)<br/>
   *Azure AD uygulama galerisi*

5. Uygulama yönetimi ekranında, sol panelde **Sağlama'yı** seçin.
6. Sağlama **Modu** menüsünde **Otomatik'i**seçin.

   ![Örnek: Azure portalında bir uygulamanın Sağlama sayfası](media/use-scim-to-provision-users-and-groups/scim-figure-2b.png)<br/>
   *Azure portalında sağlama yapılandırma*

7. Kiracı **URL** alanına, uygulamanın SCIM bitiş noktasının URL'sini girin. Örnek: `https://api.contoso.com/scim/`
8. SCIM bitiş noktası, Azure AD dışındaki bir verenden bir OAuth taşıyıcı belirteci gerektiriyorsa, gerekli OAuth taşıyıcı belirtecisini isteğe bağlı **Gizli Belirteç** alanına kopyalayın. Bu alan boş bırakılırsa, Azure AD her istekle birlikte Azure AD'den verilen bir OAuth taşıyıcı belirteci içerir. Azure AD'yi kimlik sağlayıcısı olarak kullanan uygulamalar, Bu Azure AD tarafından verilen belirteci doğrulayabilir. 
   > [!NOTE]
   > Bu alanı ***not*** boş bırakmak ve Azure AD tarafından oluşturulan bir belirteci güvenmek önerilmez. Bu seçenek öncelikle sınama amacıyla kullanılabilir.
9. SCIM bitiş noktasına bağlanmak için Azure Active Directory'nin denemesi için **Test Bağlantısı'nı** seçin. Deneme başarısız olursa, hata bilgileri görüntülenir.  

    > [!NOTE]
    > **Test Bağlantısı,** Azure AD yapılandırmasında seçilen eşleşen özellik olarak rasgele bir GUID kullanarak, var olmayan bir kullanıcı için SCIM uç noktasını sorgular. Beklenen doğru yanıt, boş bir SCIM ListResponse iletisi ile HTTP 200 Ok'tur.

10. Uygulamaya bağlanma girişimleri başarılı olursa, yönetici kimlik bilgilerini kaydetmek için **Kaydet'i** seçin.
11. **Eşlemeler** bölümünde, seçilebilir iki [öznitelik eşleme](customize-application-attributes.md)kümesi vardır: biri kullanıcı nesneleri için, diğeri grup nesneleri için. Azure Active Directory'den uygulamanıza senkronize edilen öznitelikleri gözden geçirmek için her birini seçin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleme işlemleri için uygulamanızdaki kullanıcılar ve gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    > [!NOTE]
    > "Gruplar" eşlemeyi devre dışı bırakarak grup nesnelerinin eşitlemeyi isteğe bağlı olarak devre dışı bırakabilirsiniz.

12. **Ayarlar**altında, **Kapsam** alanı hangi kullanıcıların ve grupların eşitleedildiğini tanımlar. Yalnızca Kullanıcılar ve gruplar sekmesinde atanan kullanıcıları ve grupları eşitlemek için **yalnızca atanan kullanıcıları ve grupları** eşitle'yi **(önerilir)** eşitle'yi seçin.
13. Yapılandırmanız tamamlandıktan **sonra, Sağlama Durumunu** **A.B.D.** olarak ayarlayın.
14. Azure AD sağlama hizmetini başlatmak için **Kaydet'i** seçin.
15. Yalnızca atanan kullanıcıları ve grupları eşitleme (önerilir), **Kullanıcılar ve gruplar** sekmesini seçtiğinizden ve eşitlemek istediğiniz kullanıcıları veya grupları atadığınızdan emin olun.

İlk döngü başladıktan sonra, uygulamanızdaki sağlama hizmeti tarafından yapılan tüm eylemleri gösteren ilerlemeyi izlemek için sol paneldeki **Sağlama günlüklerini** seçebilirsiniz. Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](check-status-user-account-provisioning.md)

> [!NOTE]
> İlk çevrimin gerçekleştirilemi, hizmet yürütüldüğü sürece yaklaşık her 40 dakikada bir meydana gelen daha sonraki eşitlemelere göre daha uzun sürüyor.

## <a name="step-5-publish-your-application-to-the-azure-ad-application-gallery"></a>Adım 5: Uygulamanızı Azure AD uygulama galerisinde yayımlayın

Birden fazla kiracı tarafından kullanılacak bir uygulama oluşturuyorsanız, uygulamayı Azure AD uygulama galerisinde kullanılabilir hale getirebilirsiniz. Bu, kuruluşların uygulamayı keşfetmesini ve sağlamayı yapılandırmasını kolaylaştırır. Uygulamanızı Azure REKLAM galerisinde yayınlamak ve sağlamayı başkalarının kullanımına sağlamak kolaydır. [Burada](../develop/howto-app-gallery-listing.md)adımları göz atın . Microsoft, uygulamanızı galerimize entegre etmek, bitiş noktanızı test etmek ve müşterilerin kullanması için onboarding [belgeleri](../saas-apps/tutorial-list.md) yayınlamak için sizinle birlikte çalışacaktır. 

### <a name="gallery-onboarding-checklist"></a>Galeri onboarding denetim listesi
Uygulamanızın hızlı bir şekilde yerleşik olduğundan ve müşterilerin sorunsuz bir dağıtım deneyimine sahip olduğundan emin olmak için aşağıdaki denetim listesini izleyin. Bilgiler galeriye binerken sizden toplanacaktır. 
> [!div class="checklist"]
> * [SCIM 2.0](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#step-2-understand-the-azure-ad-scim-implementation) kullanıcıve grup bitiş noktasını destekleyin (Yalnızca bir tane gereklidir, ancak her ikisi de önerilir)
> * Kiracı başına saniyede en az 25 istek desteği (Gerekli)
> * Müşterilere biniş sonrası galeri göndermeleri için mühendislik ve destek kişileri oluşturun (Gerekli)
> * 3 Başvurunuz için süresi dolmayan test kimlik bilgileri (Gerekli)
> * Aşağıda açıklandığı gibi OAuth yetkilendirme kodu hibesini veya uzun ömürlü bir jetonu destekleyin (Gerekli)
> * Müşterilerin biniş sonrası galeriyi desteklemeleri için bir mühendislik ve destek iletişim noktası oluşturun (Gerekli)
> * Birden çok grup üyeliğini tek bir YAMA ile güncelleştirmeyi destekleme (Önerilen) 
> * SCIM bitiş noktanızı herkese açık olarak belgele (Önerilen) 
> * [Şema keşfini destekleyin](https://tools.ietf.org/html/rfc7643#section-6) (Önerilen)


### <a name="authorization-for-provisioning-connectors-in-the-application-gallery"></a>Uygulama galerisinde bağlayıcı sağlama yetkisi
SCIM spec kimlik doğrulama ve yetkilendirme için SCIM'e özgü bir düzen tanımlamaz. Mevcut endüstri standartlarının kullanımına dayanır. Azure AD sağlama istemcisi, galerideki uygulamalar için iki yetkilendirme yöntemini destekler. 

|Yetkilendirme yöntemi|Artıları|Simgeler|Destek|
|--|--|--|--|
|Kullanıcı adı ve parola (Azure AD tarafından tavsiye edilmez veya desteklenmez)|Uygulaması kolay|Güvensiz - [Pa$$word önemli değil](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/your-pa-word-doesn-t-matter/ba-p/731984)|Galeri uygulamaları için ayrı ayrı desteklenir. Galeri dışı uygulamalar için desteklenmez.|
|Uzun ömürlü taşıyıcı belirteci|Uzun ömürlü belirteçler, bir kullanıcının bulunmasını gerektirmez. Bunlar, yöneticilerin sağlama yı ayarlarken kullanması kolaydır.|Uzun ömürlü belirteçleri e-posta gibi güvensiz yöntemler kullanmadan bir yönetici ile paylaşmak zor olabilir. |Galeri ve galeri dışı uygulamalar için desteklenir. |
|OAuth yetki kodu hibe|Erişim belirteçleri parolalardan çok daha kısa ömürlüdür ve uzun ömürlü taşıyıcı belirteçlerinin olmadığı otomatik bir yenileme mekanizmasına sahiptir.  Gerçek bir kullanıcı ilk yetkilendirme sırasında, bir sorumluluk düzeyi ekleyerek mevcut olmalıdır. |Bir kullanıcının bulunmasını gerektirir. Kullanıcı kuruluşbırakırsa, belirteç geçersiz dir ve yetkilendirmenin yeniden tamamlanması gerekir.|Galeri uygulamaları için desteklenir. Galeri dışı uygulamalar için destek devam ediyor.|
|OAuth istemci kimlik bilgileri hibe|Erişim belirteçleri parolalardan çok daha kısa ömürlüdür ve uzun ömürlü taşıyıcı belirteçlerinin olmadığı otomatik bir yenileme mekanizmasına sahiptir. Hem yetkilendirme kodu hibesi hem de istemci kimlik bilgileri hibesi aynı tür de erişim belirteci oluşturur, bu nedenle bu yöntemler arasında geçiş API için saydamdır.  Sağlama tamamen otomatik olabilir ve yeni belirteçler sessizce kullanıcı etkileşimi olmadan istenebilir. ||Galeri ve galeri dışı uygulamalar için desteklenmez. Destek biriktirme listemizde.|

[!NOTE] Azure AD sağlama yapılandırmaözel uygulama Kullanıcı Arabirimi'nde belirteç alanını boş bırakmak önerilmez. Oluşturulan belirteç öncelikle sınama amacıyla kullanılabilir.

**OAuth yetki kodu hibe akışı:** Sağlama hizmeti yetkilendirme [kodu hibedestekler.](https://tools.ietf.org/html/rfc6749#page-24) Uygulamanızı galeride yayınlama isteğinizi gönderdikten sonra, ekibimiz aşağıdaki bilgileri toplamak için sizinle birlikte çalışacaktır:
*  Yetkilendirme URL'si: Kullanıcı aracısının yeniden yönlendirmesi yoluyla kaynak sahibinden yetki almak için istemci tarafından bir URL. Kullanıcı, erişim yetkisi vermek için bu URL'ye yönlendirilir. 
*  Belirteç değişimi URL'si: Genellikle istemci kimlik doğrulaması ile bir erişim belirteci için yetkilendirme izni değişimi için istemci tarafından bir URL.
*  İstemci Kimliği: Yetkilendirme sunucusu kayıtlı istemci ye sahip bir istemci tanımlayıcısı, istemci tarafından sağlanan kayıt bilgilerini temsil eden benzersiz bir dize yayınlar.  İstemci tanımlayıcısı bir sır değildir; kaynak sahibine maruz kalır ve istemci kimlik doğrulaması için tek başına **kullanılmamalıdır.**  
*  İstemci sırrı: İstemci sırrı yetkilendirme sunucusu tarafından oluşturulan bir sırdır. Yalnızca yetkilendirme sunucusu tarafından bilinen benzersiz bir değer olmalıdır. 

OAuth v1 istemci sırrının açığa çıkması nedeniyle desteklenmediğini unutmayın. OAuth v2 desteklenir.  

En iyi uygulamalar (önerilen ancak gerekli değil):
* Birden çok yönlendirme URL'si destekleyin. Yöneticiler hem "portal.azure.com" hem de "aad.portal.azure.com" ile birlikte sağlamayı yapılandırabilir. Birden çok yeniden yönlendirme URL'si desteklemek, kullanıcıların her iki portaldan da erişim yetkisi verebilmesini sağlar.
* Kesinti süresi olmadan sorunsuz gizli yenileme sağlamak için birden çok sırrı destekleyin. 

**Uzun ömürlü OAuth taşıyıcı belirteçleri:** Başvurunuz OAuth yetkilendirme kodu hibe akışını desteklemiyorsa, yöneticinin sağlama tümleştirmesini kurmak için kullanabileceğinden daha uzun ömürlü bir OAuth taşıyıcı belirteci de oluşturabilirsiniz. Belirteç sürekli olmalıdır, aksi takdirde belirteç sona erdiğinde sağlama işi [karantinaya](application-provisioning-quarantine-status.md) alınır. Bu belirteç boyutu 1KB altında olmalıdır.  

Ek kimlik doğrulama ve yetkilendirme yöntemleri için [UserVoice'ta](https://aka.ms/appprovisioningfeaturerequest)bize bildirin.

### <a name="gallery-go-to-market-launch-check-list"></a>Galeri go-to-market başlatma kontrol listesi
Ortak entegrasyonumuzun farkındalığını ve talebini artırmaya yardımcı olmak için, mevcut belgelerinizi güncellemenizi ve pazarlama kanallarınızdaki tümleştirmeyi yükseltmenizi öneririz.  Aşağıda, başlatmayı desteklemek için tamamlamanızı tavsiye ettiğimiz bir dizi denetim listesi aktivitesi

* **Satış ve müşteri desteğine hazırlık.** Satış ve destek ekiplerinizin entegrasyon yeteneklerinin farkında olduğundan ve konuşabildiğinden emin olun. Satış ve destek ekibinizi bilgilendirin, sSS'leri sağlayın ve satış malzemelerinize tümleştirmeyi ekleyin. 
* **Blog gönderisi ve/veya basın bülteni.** Ortak entegrasyonu, faydaları ve nasıl başlayacağımı anlatan bir blog yazısı veya basın açıklaması hazırlanın. [Örnek: Imprivata ve Azure Active Directory Basın Bülteni](https://www.imprivata.com/company/press/imprivata-introduces-iam-cloud-platform-healthcare-supported-microsoft) 
* **Sosyal medya.** Müşterilerinizle tümleştirmeyi teşvik etmek için Twitter, Facebook veya LinkedIn gibi sosyal medyanızdan yararlanın. Gönderinizi retweetleyebilmemiz için ekleyişinizi @AzureAD unutmayın. [Örnek: Imprivata Twitter Post](https://twitter.com/azuread/status/1123964502909779968)
* **Pazarlama web sitesi.** Ortak entegrasyonun kullanılabilirliğini içerecek şekilde pazarlama sayfalarınızı (örn. entegrasyon sayfası, iş ortağı sayfası, fiyatlandırma sayfası, vb.) oluşturun veya güncelleyin. [Örnek: Pingboard entegrasyon Sayfası](https://pingboard.com/org-chart-for), [Smartsheet entegrasyon sayfası](https://www.smartsheet.com/marketplace/apps/microsoft-azure-ad), Monday.com fiyatlandırma [sayfası](https://monday.com/pricing/) 
* **Teknik dokümantasyon.** Müşterilerin nasıl başlatabileceklerine ilişkin bir yardım merkezi makalesi veya teknik dokümantasyon oluşturun. [Örnek: Elçi + Microsoft Azure Active Directory tümleştirmesi.](https://envoy.help/en/articles/3453335-microsoft-azure-active-directory-integration/
) 
* **Müşteri iletişimi.** Müşteri iletişiminiz (aylık bültenler, e-posta kampanyaları, ürün yayın notları) aracılığıyla müşterileri yeni entegrasyon konusunda uyarın. 

### <a name="allow-ip-addresses-used-by-the-azure-ad-provisioning-service-to-make-scim-requests"></a>SCIM isteklerini gerçekleştirmek için Azure AD sağlama hizmeti tarafından kullanılan IP adreslerine izin ver

Bazı uygulamalar, uygulamalarına gelen trafiğin yapılmasına izin verir. Azure AD sağlama hizmetinin beklendiği gibi çalışması için kullanılan IP adreslerine izin verilmesi gerekir. Her hizmet etiketi/bölgesi için IP adreslerinin listesi için JSON dosyasına bakın - [Azure IP Aralıkları ve Hizmet Etiketleri – Genel Bulut](https://www.microsoft.com/download/details.aspx?id=56519). Gerektiğinde bu IP'leri güvenlik duvarınıza indirebilir ve programlayabilirsiniz. Azure AD sağlama için ayrılmış IP aralıkları "AzureActiveDirectoryDomainServices" altında bulunabilir.

## <a name="related-articles"></a>İlgili makaleler:

* [SaaS uygulamalarına kullanıcı sağlama ve sağlamayı otomatikleştirme](user-provisioning.md)
* [Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
* [Öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md)
* [Kullanıcı sağlama için kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md)
* [Hesap sağlama bildirimleri](user-provisioning.md)
* [SaaS uygulamalarının nasıl entegre edilebildiğini anlatan öğreticiler listesi](../saas-apps/tutorial-list.md)
