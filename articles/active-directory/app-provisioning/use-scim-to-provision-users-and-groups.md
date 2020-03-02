---
title: Azure AD 'den uygulamalara Kullanıcı hazırlama için bir SCıM uç noktası geliştirin
description: Etki alanları arası kimlik yönetimi (SCıM) sistemi, otomatik Kullanıcı sağlamayı standartlaştırlar. Bir SCıM uç noktası geliştirmeyi, SCıM API 'nizi Azure Active Directory ile tümleştirmeyi ve bulut uygulamalarınıza kullanıcıları ve grupları sağlamayı otomatik hale getirmeye başlamasını öğrenin.
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
ms.date: 03/01/2020
ms.author: mimart
ms.reviewer: arvinh
ms.custom: aaddev;it-pro;seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: a2fda5d1bdd00a601df363bd930e5f2f6d610c7f
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78208721"
---
# <a name="build-a-scim-endpoint-and-configure-user-provisioning-with-azure-active-directory-azure-ad"></a>Azure Active Directory bir SCıM uç noktası oluşturun ve Kullanıcı sağlamasını yapılandırın (Azure AD)

Uygulama geliştiricisi olarak, uygulamanız ile Azure AD arasında kullanıcıları ve grupları otomatik olarak sağlamayı etkinleştirmek için etki alanları arası kimlik yönetimi (SCıM) Kullanıcı yönetimi API 'sini kullanabilirsiniz. Bu makalede, bir SCıM uç noktası oluşturma ve Azure AD sağlama hizmeti ile tümleştirme açıklanır. SCıM belirtimi, sağlama için ortak bir Kullanıcı şeması sağlar. SAML veya OpenID Connect gibi Federasyon standartlarıyla birlikte kullanıldığında, SCıM yöneticilere erişim yönetimi için uçtan uca standartlara dayalı bir çözüm sunar.

SCıM iki uç noktanın standartlaştırılmış bir tanımıdır: bir/Users uç noktası ve bir/Groups uç noktası. Nesneleri oluşturmak, güncelleştirmek ve silmek için ortak REST fiillerini ve grup adı, Kullanıcı adı, adı, soyadı, soyadı ve e-posta gibi ortak özniteliklerin önceden tanımlanmış bir şemasını kullanır. Bir SCıM 2,0 REST API sunan uygulamalar, özel bir kullanıcı yönetimi API 'siyle çalışmayı azaltabilir veya ortadan kaldırabilir. Örneğin, tüm uyumlu SCıM istemcileri yeni bir kullanıcı girişi oluşturmak için/Users uç noktasına bir JSON nesnesinin HTTP GÖNDERISINI nasıl yapılacağını bilir. Aynı temel eylemler için biraz farklı bir API sağlamak yerine, SCıM standardına uyan uygulamalar, önceden var olan istemcilerden, araçlardan ve koddan hemen faydalanabilir. 

![SCıM ile Azure AD 'den bir uygulamaya sağlama](media/use-scim-to-provision-users-and-groups/scim-provisioning-overview.png)

SCıM 2,0 (RFC [7642](https://tools.ietf.org/html/rfc7642), [7643](https://tools.ietf.org/html/rfc7643), [7644](https://tools.ietf.org/html/rfc7644)) ' de tanımlanan yönetim için standart Kullanıcı nesne şeması ve REST API 'leri, kimlik sağlayıcılarının ve uygulamaların birbirleriyle daha kolay tümleşmesine olanak tanır. Bir SCıM uç noktası oluşturan uygulama geliştiricileri, özel iş yapmak zorunda kalmadan herhangi bir SCıM uyumlu istemcisiyle tümleştirilebilir.

Bir uygulamaya sağlamanın otomatikleştirilmesi, Azure AD SCıM ile uyumlu bir SCıM uç noktası oluşturulmasını ve tümleştirmesini gerektirir. Uygulamanıza Kullanıcı ve grup sağlamaya başlamak için aşağıdaki adımları gerçekleştirin. 
    
  * **[1. Adım: Kullanıcı ve grup şemanızı tasarlayın.](#step-1-design-your-user-and-group-schema)** Uygulamanız için gereken nesneleri ve öznitelikleri tanımlayabilir ve bunların Azure AD SCıM uygulaması tarafından desteklenen Kullanıcı ve grup şemasına nasıl eşlendiğini saptayın.

  * **[2. Adım: Azure AD SCıM uygulamasını anlayın.](#step-2-understand-the-azure-ad-scim-implementation)** Azure AD SCıM istemcisinin nasıl uygulandığını anlayın ve SCıM protokol isteği işleme ve yanıtlarınızı modelleyin.

  * **[3. Adım: SCıM uç noktası oluşturma.](#step-3-build-a-scim-endpoint)** Azure AD sağlama hizmeti ile tümleştirilecek bir uç noktanın SCıM 2,0 ile uyumlu olması gerekir. Bir seçenek olarak, uç noktanızı derlemek için Microsoft ortak dil altyapısı (CLı) kitaplıklarını ve kod örneklerini kullanabilirsiniz. Bu örnekler yalnızca başvuru ve test içindir; üretim uygulamanızın bir bağımlılığı alacak şekilde kodlanmasını öneririz.

  * **[4. Adım: SCıM uç noktanızı Azure AD SCıM istemcisiyle tümleştirin.](#step-4-integrate-your-scim-endpoint-with-the-azure-ad-scim-client)** Kuruluşunuz, Azure AD 'nin desteklediği SCıM 2,0 profilini uygulayan bir üçüncü taraf uygulama kullanıyorsa, kullanıcıların ve grupların sağlanması ve sağlanması için hemen otomatikleştirmeye başlayabilirsiniz.

  * **[5. Adım: uygulamanızı Azure AD Uygulama Galerisi 'nde yayımlayın.](#step-5-publish-your-application-to-the-azure-ad-application-gallery)** Müşterilerin uygulamanızı bulmasını ve sağlamayı kolay bir şekilde yapılandırmasını kolaylaştırın. 

![SCıM uç noktasını Azure AD ile tümleştirme adımları](media/use-scim-to-provision-users-and-groups/process.png)

## <a name="step-1-design-your-user-and-group-schema"></a>1\. Adım: Kullanıcı ve grup şemanızı tasarlama

Her uygulama, Kullanıcı veya grup oluşturmak için farklı öznitelikler gerektirir. Uygulamanızın gerektirdiği nesneleri (Kullanıcı, Grup) ve öznitelikleri (ad, yönetici, iş unvanı vb.) tanımlayarak tümleştirmenizi başlatın. SCıM standardı, kullanıcıları ve grupları yönetmek için bir şema tanımlar. Çekirdek Kullanıcı şeması yalnızca üç öznitelik gerektirir: **kimlik** (hizmet sağlayıcı tanımlı tanımlayıcı), **externalId** (istemci tanımlı tanımlayıcı) ve **meta** (hizmet sağlayıcı tarafından tutulan salt okuma meta verileri). Diğer tüm öznitelikler isteğe bağlıdır. Çekirdek Kullanıcı şemasına ek olarak, SCıM standardı, uygulamanızın ihtiyaçlarını karşılamak için Kullanıcı şemasını genişletmek üzere bir kurumsal Kullanıcı uzantısı ve modeli tanımlar. Örneğin, uygulamanız bir kullanıcının yöneticisini gerektiriyorsa, kullanıcının yöneticisini toplamak için kuruluş Kullanıcı şemasını ve kullanıcının e-postasını toplamak için çekirdek şemayı kullanabilirsiniz. Şemanızı tasarlamak için aşağıdaki adımları izleyin:
  1. Uygulamanızın gerektirdiği öznitelikleri listeleyin. Gereksinimleriniz, kimlik doğrulaması için gereken özniteliklere (örn. loginName ve e-posta), kullanıcının yaşam döngüsünü (ör. durum/etkin) yönetmek için gereken öznitelikleri ve belirli uygulamanızın çalışması için gereken diğer öznitelikleri (ör. yönetici, etiket) için gerekli olan özniteliklere bölmek faydalı olabilir.
  2. Bu özniteliklerin zaten çekirdek Kullanıcı şemasında veya kurumsal Kullanıcı şemasında tanımlanmış olup olmadığını denetleyin. İhtiyacınız olan ve çekirdek ya da kurumsal Kullanıcı şemaları kapsamında olmayan herhangi bir öznitelik varsa, Kullanıcı şemasına, ihtiyacınız olan öznitelikleri içeren bir uzantı tanımlamanız gerekir. Aşağıdaki örnekte, bir kullanıcıya bir "Tag" sağlamaya izin vermek için kullanıcıya bir uzantı ekledik. Yalnızca çekirdek ve kurumsal Kullanıcı şemaları ile başlamak ve daha sonra ek özel şemalara genişletmek en iyisidir.  
  3. SCıM özniteliklerini Azure AD 'deki Kullanıcı öznitelikleriyle eşleyin. SCıM uç noktanıza tanımladığınız özniteliklerden birinin Azure AD Kullanıcı şemasında açık bir karşılığı yoksa, verilerin çoğu kiracının üzerinde Kullanıcı nesnesinde depolanmadığından iyi bir şansınız olur. Bu özniteliğin Kullanıcı oluşturmak için isteğe bağlı olup olmadığını göz önünde bulundurun. Uygulamanızın çalışması için özniteliği önemliyse, kiracı yöneticisinin şemasını genişletmesine veya "Etiketler" özelliği için aşağıda gösterildiği gibi bir uzantı özniteliği kullanmasına kılavuzluk edin.

### <a name="table-1-outline-the-attributes-that-you-need"></a>Tablo 1: ihtiyacınız olan özniteliklerin anahatlarını belirleme 
| 1\. Adım: uygulamanızın gerektirdiği öznitelikleri belirleme| 2\. Adım: uygulama gereksinimlerini SCıM standardına eşleme| 3\. Adım: SCıM özniteliklerini Azure AD öznitelikleriyle eşleme|
|--|--|--|
|loginName|userName adı|userPrincipalName|
|firstName|name.givenName|givenName|
|lastName|Name. lastName|lastName|
|Iş posta|E-postalar [tür EQ "iş"]. değer|Mail|
|Yöneticisi|Yöneticisi|Yöneticisi|
|tag|urn: IETF: params: Scim: schemas: Extension: 2.0: Customexgerilim: Tag|extensionAttribute1|
|durum|etkin|ısofdeleted (hesaplanan değer kullanıcı üzerinde depolanmaz)|

Yukarıda tanımlanan şema aşağıdaki JSON yükü kullanılarak temsil edilir. Uygulama için gerekli olan özniteliklere ek olarak, JSON temsili gereken "ID," "externalId" ve "meta" özniteliklerini içerir.

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

### <a name="table-2-default-user-attribute-mapping"></a>Tablo 2: varsayılan kullanıcı özniteliği eşleme
Daha sonra, uygulamanızın gerektirdiği özniteliklerin Azure AD 'de bir özniteliğe ve SCıM RFC 'ye nasıl eşlendiğini anlamak için aşağıdaki tabloyu kullanabilirsiniz. Özniteliklerin Azure AD ile SCıM uç noktanız arasında nasıl eşlendiğini [özelleştirebilirsiniz](customize-application-attributes.md) . Hem kullanıcıları hem de grupları veya aşağıda gösterilen tüm öznitelikleri desteklememeniz gerekmez. Bunlar, Azure AD 'deki özniteliklerin genellikle SCıM protokolündeki özelliklerle nasıl eşlendiğine yönelik bir başvurudur. 

| Azure Active Directory kullanıcısı | "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User" |
| --- | --- |
| IsSoftDeleted |etkin |
|Bölüm|urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Department|
| displayName |displayName |
|Çalışan|urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: employeeNumber|
| Faks TelephoneNumber |PhoneNumber [türü eq "faks"] .value |
| givenName |name.givenName |
| İş Unvanı |title |
| posta |e-postaları [türü eq "İş"] .value |
| mailNickname |externalId |
| Yöneticisi |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Manager |
| Mobil |PhoneNumber [türü eq "mobile"] .value |
| posta kodu |adresler [türü eq "İş"] .postalCode |
| Ara sunucu adresleri |e-postaları [type eq "diğer"]. Değer |
| fiziksel teslim OfficeName |adresler [type eq "diğer"]. Biçimlendirilmiş |
| streetAddress |adresler [türü eq "İş"] .streetAddress |
| Soyadı |name.familyName |
| telefon numarası |PhoneNumber [türü eq "İş"] .value |
| Kullanıcı PrincipalName |userName adı |


### <a name="table-3-default-group-attribute-mapping"></a>Tablo 3: varsayılan Grup özniteliği eşleme

| Azure Active Directory grubu | urn: IETF: params: Scim: schemas: Core: 2.0: Grup |
| --- | --- |
| displayName |displayName |
| posta |e-postaları [türü eq "İş"] .value |
| mailNickname |displayName |
| üyeler |üyeler |
| objectId |externalId |
| proxyAddresses |e-postaları [type eq "diğer"]. Değer |

SCıM RFC 'de tanımlanmış birkaç uç nokta vardır. /User uç noktası ile çalışmaya başlayabilir ve ardından buradan genişletebilirsiniz. /Schemas uç noktası özel öznitelikler kullanılırken veya şemanızın sıklıkla değiştiği durumlarda faydalıdır. İstemcinin en güncel şemayı otomatik olarak almasına olanak sağlar. /Bulk uç noktası özellikle destekleyici gruplar olduğunda faydalıdır. Aşağıdaki tabloda, SCıM standardında tanımlanan çeşitli uç noktalar açıklanmaktadır. /Schemas uç noktası özel öznitelikler kullanılırken veya şemanızın sıklıkla değiştiği durumlarda faydalıdır. Bir istemcinin en güncel şemayı otomatik olarak almasına olanak sağlar. /Bulk uç noktası özellikle destekleyici gruplar olduğunda faydalıdır. Aşağıdaki tabloda, SCıM standardında tanımlanan çeşitli uç noktalar açıklanmaktadır. 
 
### <a name="table-4-determine-the-endpoints-that-you-would-like-to-develop"></a>Tablo 4: geliştirmek istediğiniz uç noktaları belirleme
|BKZ|AÇIKLAMA|
|--|--|
|/User|Bir kullanıcı nesnesi üzerinde CRUD işlemleri gerçekleştirin.|
|/Group|Bir grup nesnesi üzerinde CRUD işlemleri gerçekleştirin.|
|/ServiceProviderConfig|Desteklenen SCıM standardının özelliklerine ilişkin ayrıntıları, örneğin desteklenen kaynakları ve kimlik doğrulama yöntemini sağlar.|
|/ResourceTypes|Her kaynakla ilgili meta verileri belirtir|
|/Schemas|Her istemci ve hizmet sağlayıcı tarafından desteklenen öznitelik kümesi farklılık gösterebilir. Bir hizmet sağlayıcı "ad", "başlık" ve "e-postalar" içerebilir, ancak başka bir hizmet sağlayıcı "ad", "başlık" ve "phoneNumbers" kullanır. Şemalar uç noktası desteklenen özniteliklerin bulunmasına izin verir.|
|/Toplu|Toplu işlemler, tek bir işlemde büyük kaynak nesneleri koleksiyonu üzerinde işlemler gerçekleştirmenize olanak tanır (örneğin, büyük bir grup için üyelikleri güncelleştirme).|


## <a name="step-2-understand-the-azure-ad-scim-implementation"></a>2\. Adım: Azure AD SCıM uygulamasını anlama
> [!IMPORTANT]
> Azure AD SCıM uygulamasının davranışı en son 18 Aralık 2018 tarihinde güncelleştirildi. Nelerin değiştiği hakkında daha fazla bilgi için bkz. [Azure AD Kullanıcı sağlama hizmeti 'Nin SCIM 2,0 protokol uyumluluğu](application-provisioning-config-problem-scim-compatibility.md).

SCıM 2,0 kullanıcı yönetim API 'sini destekleyen bir uygulama oluşturuyorsanız, bu bölümde Azure AD SCıM istemcisinin nasıl uygulandığı ayrıntılı bilgiler açıklanmaktadır. Ayrıca, SCıM protokol isteği işleme ve yanıtlarınızı nasıl modellebileceğiniz gösterilmektedir. SCıM uç noktanızı uygulandıktan sonra, önceki bölümde açıklanan yordamı izleyerek test edebilirsiniz.

[SCIM 2,0 protokol belirtimi](http://www.simplecloud.info/#Specification)içinde, uygulamanız şu gereksinimleri karşılamalıdır:

* , [SCıM protokolünün 3,3](https://tools.ietf.org/html/rfc7644#section-3.3)bölümünde olduğu gibi, kullanıcıların ve isteğe bağlı olarak grupların oluşturulmasını destekler.  
* , [SCıM protokolünün Bölüm 3.5.2](https://tools.ietf.org/html/rfc7644#section-3.5.2)göre, yama istekleri olan kullanıcı veya grupların değiştirilmesini destekler.  
* , [SCıM protokolünün bölüm başına 3.4.1](https://tools.ietf.org/html/rfc7644#section-3.4.1)göre daha önce oluşturulmuş bir kullanıcı veya grup için bilinen bir kaynağı almayı destekler.  
* , [SCıM protokolünün](https://tools.ietf.org/html/rfc7644#section-3.4.2)bölüm başına 3.4.2 göre Kullanıcı veya grupların sorgulanmasını destekler.  Varsayılan olarak, kullanıcılar `id` alınır ve `username` ve `externalid`tarafından sorgulanır ve gruplar `displayName`tarafından sorgulanır.  
* , SCıM protokolünün Bölüm 3.4.2 göre, kullanıcının KIMLIĞE ve yöneticiye göre sorgulanmasını destekler.  
* , SCıM protokolünün Bölüm 3.4.2 göre grupların KIMLIĞE ve üyeye göre sorgulanmasını destekler.  
* Azure AD 'nin uygulamanıza kimlik doğrulaması ve yetkilendirmesi için tek bir taşıyıcı belirtecini kabul eder.

Azure AD ile uyumluluğu sağlamak için bir SCıM uç noktası uygularken bu genel yönergeleri izleyin:

* `id` tüm kaynaklar için gerekli bir özelliktir. Kaynak döndüren her yanıt, sıfır üyeli `ListResponse` hariç olmak üzere her bir kaynağın bu özelliğe sahip olduğundan emin olmalıdır.
* Bir sorgu/filtre isteğine yanıt her zaman bir `ListResponse`olmalıdır.
* Gruplar isteğe bağlıdır, ancak yalnızca SCıM uygulamasının yama isteklerini desteklemesi durumunda desteklenir.
* Bu, tüm kaynağı yama yanıtına dahil etmek için gerekli değildir.
* Microsoft Azure AD yalnızca aşağıdaki işleçleri kullanır:  
    - `eq`
    - `and`
* Bazı düzeltme ekinde, https://tools.ietf.org/html/rfc7644#section-3.5.2tanımlanan şekilde işlem değerlerini `op`, özel bir düzeltme ekinde, SCıM içindeki yapısal öğelerde büyük/küçük harfe duyarlı bir eşleşme gerektirmez. Azure AD, ' op ' değerlerini `Add`, `Replace`ve `Remove`olarak yayar.
* Microsoft Azure AD bitiş noktasının ve kimlik bilgilerinin geçerli olduğundan emin olmak için rastgele bir Kullanıcı ve grup getirme isteklerinin olmasını sağlar. Ayrıca, [Azure Portal](https://portal.azure.com) **Test bağlantı** akışının bir parçası olarak da yapılır. 
* Kaynakların sorgulanabileceği öznitelik, [Azure Portal](https://portal.azure.com)uygulamada eşleşen bir öznitelik olarak ayarlanmalıdır. Daha fazla bilgi için bkz. [Kullanıcı hazırlama öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)

### <a name="user-provisioning-and-deprovisioning"></a>Kullanıcı hazırlama ve sağlamayı kaldırma

Aşağıdaki çizim Azure Active Directory, uygulamanızın kimlik deposundaki bir kullanıcının yaşam döngüsünü yönetmek üzere bir SCıM hizmetine gönderdiği iletileri gösterir.  

![, Kullanıcı hazırlama ve sağlamayı kaldırma sırasını gösterir](media/use-scim-to-provision-users-and-groups/scim-figure-4.png)<br/>
*Kullanıcı hazırlama ve sağlamayı kaldırma sırası*

### <a name="group-provisioning-and-deprovisioning"></a>Grup sağlama ve sağlamayı kaldırma

Grup sağlama ve sağlamayı kaldırma isteğe bağlıdır. Uygulandığında ve etkinleştirildiğinde aşağıdaki çizimde, Azure AD 'nin uygulamanızın kimlik deposundaki bir grubun yaşam döngüsünü yönetmek için bir SCıM hizmetine gönderdiği iletiler gösterilmektedir.  Bu iletiler kullanıcılar hakkındaki iletilerden iki şekilde farklılık gösterir:

* Grupları alma istekleri, Üyeler özniteliğinin, isteğe yanıt olarak belirtilen herhangi bir kaynaktan dışlanacağını belirtir.  
* Bir başvuru özniteliği, belirli bir değere sahip olup olmadığını belirlemek üzere istekleri üyeleri özniteliği hakkında isteklerdir.  

![grup sağlama ve sağlamayı kaldırma sırasını gösterir](media/use-scim-to-provision-users-and-groups/scim-figure-5.png)<br/>
*Grup sağlama ve sağlamayı kaldırma sırası*

### <a name="scim-protocol-requests-and-responses"></a>SCıM protokol istekleri ve yanıtları
Bu bölümde, Azure AD SCıM istemcisi tarafından yayılan örnek SCıM istekleri ve örnek beklenen yanıtlar sağlanmaktadır. En iyi sonuçlar için, bu istekleri bu biçimde işleyecek ve beklenen yanıtları yaymak üzere uygulamanızı kodmalısınız.

> [!IMPORTANT]
> Azure AD Kullanıcı sağlama hizmeti 'nin aşağıda açıklanan işlemleri nasıl ve ne zaman yaydığı hakkında bilgi edinmek için bkz. sağlama [döngüleri: başlangıç ve artımlı](how-provisioning-works.md#provisioning-cycles-initial-and-incremental) , [sağlama nasıl?](how-provisioning-works.md).

[Kullanıcı Işlemleri](#user-operations)
  - [Kullanıcı oluştur](#create-user) ([istek](#request) / [yanıtı](#response))
  - [Kullanıcı al](#get-user) ([istek](#request-1) / [yanıtı](#response-1))
  - [Sorguya göre Kullanıcı al](#get-user-by-query) ([istek](#request-2) / [yanıtı](#response-2))
  - [Sorgu sıfıra sahip sonuçlara göre Kullanıcı al](#get-user-by-query---zero-results) ([istek](#request-3)
/ [yanıtı](#response-3))
  - [Kullanıcı [çok değerli özellikler] güncelleştirme](#update-user-multi-valued-properties) ([istek](#request-4) /  [yanıtı](#response-4))
  - [Kullanıcı güncelleştirme [tek değerli özellikler]](#update-user-single-valued-properties) ([istek](#request-5)
/ [yanıtı](#response-5)) 
  - [Kullanıcıyı devre dışı bırak](#disable-user) ([istek](#request-14) / 
[yanıtı](#response-14))
  - [Kullanıcı silme](#delete-user) ([istek](#request-6) / 
[yanıtı](#response-6))


[Grup Işlemleri](#group-operations)
  - [Grup Oluştur](#create-group) ( [istek](#request-7) / [yanıtı](#response-7))
  - [Grup Al](#get-group) ( [istek](#request-8) / [yanıtı](#response-8))
  - [Group By DisplayName 'e göre Al](#get-group-by-displayname) ([istek](#request-9) / [yanıtı](#response-9))
  - [Güncelleştirme grubu [Üye olmayan öznitelikler]](#update-group-non-member-attributes) ([istek](#request-10) /
 [yanıtı](#response-10))
  - [Güncelleştirme grubu [Üye Ekleme]](#update-group-add-members) ( [istek](#request-11) /
[yanıtı](#response-11))
  - [Güncelleştirme grubu [üyeleri kaldırma]](#update-group-remove-members) ( [istek](#request-12) /
[yanıtı](#response-12))
  - [Grubu Sil](#delete-group) ([istek](#request-13) /
[yanıtı](#response-13))

### <a name="user-operations"></a>Kullanıcı Işlemleri

* Kullanıcılar, `userName` veya `email[type eq "work"]` öznitelikleriyle sorgulanabilir.  

#### <a name="create-user"></a>Kullanıcı oluşturma

###### <a name="request"></a>İstek

*POST/Users*
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

*HTTP/1.1 201 oluşturuldu*
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

#### <a name="get-user"></a>Kullanıcı Al

###### <a name="request-1"></a>İsteyen
*/Users/5d48a0a8e9f04aa38008 al* 

###### <a name="response-1"></a>Yanıt (Kullanıcı bulundu)
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
*/Users/5171a35d82074e068ce2 al* 

###### <a name="response-user-not-found-note-that-the-detail-is-not-required-only-status"></a>Yanıt (Kullanıcı bulunamadı. Ayrıntı gerekli değildir, yalnızca durum olur.)

```json
{
    "schemas": [
        "urn:ietf:params:scim:api:messages:2.0:Error"
    ],
    "status": "404",
    "detail": "Resource 23B51B0E5D7AE9110A49411D@7cca31655d49f3640a494224 not found"
}
```

#### <a name="get-user-by-query"></a>Sorguya göre Kullanıcı al

##### <a name="request-2"></a>İsteyen

*GET/Users? Filter = userName EQ "Test_User_dfeef4c5-5681 -4387-B016-bdf221e82081"*

##### <a name="response-2"></a>Yanıtıyla

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

#### <a name="get-user-by-query---zero-results"></a>Sorguya göre Kullanıcı al-sıfır sonuçları

##### <a name="request-3"></a>İsteyen

*/Users al? Filter = userName EQ "varolmayan kullanıcı"*

##### <a name="response-3"></a>Yanıtıyla

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

#### <a name="update-user-multi-valued-properties"></a>Kullanıcı güncelleştirme [çok değerli özellikler]

##### <a name="request-4"></a>İsteyen

*PATCH/Users/6764549bef60420686bc HTTP/1.1*
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

##### <a name="response-4"></a>Yanıtıyla

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

#### <a name="update-user-single-valued-properties"></a>Kullanıcı güncelleştirme [tek değerli özellikler]

##### <a name="request-5"></a>İsteyen

*PATCH/Users/5171a35d82074e068ce2 HTTP/1.1*
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

##### <a name="response-5"></a>Yanıtıyla

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

### <a name="disable-user"></a>Kullanıcı devre dışı bırak

##### <a name="request-14"></a>İsteyen

*PATCH/Users/5171a35d82074e068ce2 HTTP/1.1*
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

##### <a name="response-14"></a>Yanıtıyla

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

##### <a name="request-6"></a>İsteyen

*/Users/5171a35d82074e068ce2 HTTP/1.1 SILME*

##### <a name="response-6"></a>Yanıtıyla

*HTTP/1.1 204 Içerik yok*

### <a name="group-operations"></a>Grup Işlemleri

* Gruplar her zaman bir boş üye listesiyle oluşturulur.
* Gruplar `displayName` özniteliğiyle sorgulanabilir.
* Grup düzeltme eki isteğine güncelleştirme, yanıtta bir *HTTP 204 içeriği* getirmelidir. Tüm üyelerin listesini içeren bir gövde döndürülmesi önerilmez.
* Grubun tüm üyelerini döndürmeyi desteklemek gerekli değildir.

#### <a name="create-group"></a>{1&gt;Grup Oluştur&lt;1}

##### <a name="request-7"></a>İsteyen

*Son/gruplar HTTP/1.1*
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

##### <a name="response-7"></a>Yanıtıyla

*HTTP/1.1 201 oluşturuldu*
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

#### <a name="get-group"></a>Grubunu Al

##### <a name="request-8"></a>İsteyen

*/Groups/40734ae655284ad3abcc? excludedAttributes = Members HTTP/1.1 alın*

##### <a name="response-8"></a>Yanıtıyla
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

#### <a name="get-group-by-displayname"></a>Grubu displayName 'e göre al

##### <a name="request-9"></a>İsteyen
*/Groups al? excludedAttributes = Members & Filter = displayName EQ "displayName" HTTP/1.1*

##### <a name="response-9"></a>Yanıtıyla

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

#### <a name="update-group-non-member-attributes"></a>Güncelleştirme grubu [Üye olmayan öznitelikler]

##### <a name="request-10"></a>İsteyen

*PATCH/Groups/fa2ce26709934589afc5 HTTP/1.1*
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

##### <a name="response-10"></a>Yanıtıyla

*HTTP/1.1 204 Içerik yok*

### <a name="update-group-add-members"></a>Güncelleştirme grubu [Üye Ekleme]

##### <a name="request-11"></a>İsteyen

*PATCH/Groups/a99962b9f99d4c4fac67 HTTP/1.1*
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

##### <a name="response-11"></a>Yanıtıyla

*HTTP/1.1 204 Içerik yok*

#### <a name="update-group-remove-members"></a>Güncelleştirme grubu [üyeleri kaldır]

##### <a name="request-12"></a>İsteyen

*PATCH/Groups/a99962b9f99d4c4fac67 HTTP/1.1*
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

##### <a name="response-12"></a>Yanıtıyla

*HTTP/1.1 204 Içerik yok*

#### <a name="delete-group"></a>Grubu Silme

##### <a name="request-13"></a>İsteyen

*/Groups/cdb1ce18f65944079d37 HTTP/1.1 SILME*

##### <a name="response-13"></a>Yanıtıyla

*HTTP/1.1 204 Içerik yok*

### <a name="security-requirements"></a>Güvenlik gereksinimleri
**TLS protokol sürümleri**

Kabul edilebilir tek TLS protokol sürümleri, TLS 1,2 ve TLS 1,3 ' dir. Başka TLS sürümüne izin verilmez. SSL sürümüne izin verilmez. 
- RSA anahtarları en az 2.048 bit olmalıdır.
- ECC anahtarları, onaylanmış bir eliptik eğri kullanılarak oluşturulan en az 256 bit olmalıdır


**Anahtar uzunlukları**

Tüm hizmetlerin, yeterli uzunlukta şifreleme anahtarları kullanılarak oluşturulan X. 509.440 sertifikalarını kullanması gerekir, anlamı:

**Şifre paketleri**

Tüm hizmetlerin aşağıdaki şifre paketlerini kullanmak için aşağıda belirtilen kesin sırada yapılandırılması gerekir. Yalnızca bir RSA sertifikanız varsa, yüklü ECDSA şifre paketlerinin herhangi bir etkisi olmadığını unutmayın. </br>

TLS 1,2 şifre paketleri minimum çubuğu:

- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384


## <a name="step-3-build-a-scim-endpoint"></a>3\. Adım: SCıM uç noktası oluşturma

Şemanızı öğrendiğinize ve Azure AD SCıM uygulamasını anladığınıza göre, SCıM uç noktanızı geliştirmeye başlamanızı sağlayabilirsiniz. Sıfırdan başlamak ve uygulamayı tamamen kendi kendinize oluşturmak yerine, SCıM commuinty tarafından yayınlanan bir dizi açık kaynaklı SCıM kitaplıklarını kullanabilirsiniz.  
Azure AD sağlama ekibi tarafından yayımlanan açık kaynaklı .NET Core [başvuru kodu](https://aka.ms/SCIMReferenceCode) , geliştirmeye başlayabilmenizi sağlayan bir kaynaktır. SCıM uç noktanızı oluşturduktan sonra test etmek isteyeceksiniz. Başvuru kodunun bir parçası olarak verilen [Postman testleri](https://github.com/AzureAD/SCIMReferenceCode/wiki/Test-Your-SCIM-Endpoint) koleksiyonunu kullanabilir veya [yukarıda](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#user-operations)belirtilen örnek istekler/yanıtlar üzerinden çalıştırabilirsiniz.  

Note: başvuru kodu, SCıM uç noktanızı oluşturmaya başlamanıza yardımcı olmaya yöneliktir ve "olduğu gıbı" sunulmaktadır. Topluluk katkılarına, kodu oluşturmaya ve tutmaya yardımcı olmak için hoş geldiniz. 

## <a name="step-4-integrate-your-scim-endpoint-with-the-azure-ad-scim-client"></a>4\. Adım: SCıM uç noktanızı Azure AD SCıM istemcisiyle tümleştirme

Azure AD, [SCIM 2,0 protokolünün](https://tools.ietf.org/html/rfc7644)belirli bir profilini uygulayan uygulamalara atanan kullanıcıları ve grupları otomatik olarak sağlayacak şekilde yapılandırılabilir. Profilin özellikleri [2. Adım: Azure AD SCıM uygulamasını anlama](#step-2-understand-the-azure-ad-scim-implementation)bölümünde belgelenmiştir.

Uygulama sağlayıcınıza veya bilgilerinin bu gereksinimleri ile uyumluluk için uygulama sağlayıcının belgelerine başvurun.

> [!IMPORTANT]
> Azure AD SCıM uygulaması, Azure AD Kullanıcı sağlama hizmeti 'nin üzerine kurulmuştur. Bu, kullanıcıların Azure AD ile hedef uygulama arasında sürekli olarak eşitlenmesi için tasarlanan ve çok özel bir standart işlem kümesi uyguladığı şekilde tasarlanmıştır. Azure AD SCıM istemcisinin davranışını anlamak için bu davranışları anlamak önemlidir. Daha fazla bilgi için bkz. sağlama [döngüleri: başlangıç ve artımlı](how-provisioning-works.md#provisioning-cycles-initial-and-incremental) , [sağlama nasıl?](how-provisioning-works.md).

### <a name="getting-started"></a>Başlarken

Bu makalede açıklanan SCIM profilini destekleyen uygulamalar, Azure Active Directory Azure AD uygulama galerisinde bulunan "galeri dışı uygulama" özelliğini kullanarak bağlanabilir. Bağlantı kurulduktan sonra Azure AD eşitleme işlemi burada, uygulamanın SCIM uç noktası için atanan kullanıcılar ve gruplar, sorgular ve oluşturuyor veya bunları göre atama ayrıntıları 40 dakikada bir çalışır.

**SCıM 'i destekleyen bir uygulamaya bağlanmak için:**

1. [Azure Active Directory portalında](https://aad.portal.azure.com)oturum açın. [Geliştirici programına](https://developer.microsoft.com/office/dev-program) kaydolduktan sonra, P2 lisanslarıyla Azure Active Directory ücretsiz bir denemeye erişebileceğinizi unutmayın.
2. Sol bölmeden **Kurumsal uygulamalar** ' ı seçin. Galeriden eklenen uygulamalar dahil olmak üzere, yapılandırılan tüm uygulamaların bir listesi gösterilir.
3. **Galeri dışı** > **Tüm** >  **+ Yeni uygulama** ' yı seçin.
4. Uygulamanız için bir ad girin ve uygulama nesnesi oluşturmak için **Ekle** ' yi seçin. Yeni uygulama, kurumsal uygulamalar listesine eklenir ve uygulama yönetimi ekranına açılır.

   ![ekran görüntüsünde Azure AD Uygulama Galerisi gösterilmektedir](media/use-scim-to-provision-users-and-groups/scim-figure-2a.png)<br/>
   *Azure AD Uygulama Galerisi*

5. Uygulama yönetimi ekranında, sol panelde **sağlama** ' yı seçin.
6. **Sağlama modu** menüsünde **Otomatik**' i seçin.

   ![örnek: bir uygulamanın sağlama sayfası Azure portal](media/use-scim-to-provision-users-and-groups/scim-figure-2b.png)<br/>
   *Azure portal sağlamayı yapılandırma*

7. **Kiracı URL 'si** alanına uygulamanın SCIM uç noktasının URL 'sini girin. Örnek: https://api.contoso.com/scim/
8. SCıM uç noktası, Azure AD 'den başka bir verenin bir OAuth taşıyıcı belirteci gerektiriyorsa, gerekli OAuth taşıyıcı belirtecini isteğe bağlı **gizli belirteç** alanına kopyalayın. Bu alan boş bırakılırsa Azure AD, Azure AD 'den her istekle verilen bir OAuth taşıyıcı belirteci içerir. Kimlik sağlayıcısı olarak Azure AD kullanan uygulamalar, bu Azure AD veren belirtecini doğrulayabilir. 
   > [!NOTE]
   > Bu alanı boş bırakmanız ve Azure AD tarafından oluşturulan bir belirtece güvenmamanız ***önerilmez.*** Bu seçenek öncelikle test amacıyla kullanılabilir.
9. SCıM uç noktasına bağlanmak Azure Active Directory denemek için **Bağlantıyı Sına** ' yı seçin. Deneme başarısız olursa, hata bilgileri görüntülenir.  

    > [!NOTE]
    > **Test bağlantısı** , mevcut olmayan bir kullanıcı için SCIM uç noktasını sorgular, bu da Azure AD yapılandırmasında eşleşen özellik olarak rastgele bir GUID kullanılıyor. Beklenen doğru yanıt, boş bir SCIM ListResponse iletisi ile HTTP 200 Tamam ' dır.

10. Uygulamaya bağlanma denemeleri başarılı olursa, yönetici kimlik bilgilerini kaydetmek için **Kaydet** ' i seçin.
11. **Eşlemeler** bölümünde, biri Kullanıcı nesneleri ve bir grup nesnesi için olmak üzere iki seçilebilir [öznitelik eşlemesi](customize-application-attributes.md)vardır. Uygulamanızı Azure Active Directory'den eşitlenen öznitelikler gözden geçirmek için her birini seçin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri için uygulamanızdaki kullanıcıları ve grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** ' i seçin.

    > [!NOTE]
    > İsteğe bağlı olarak, "eşleme grupları" devre dışı bırakarak Grup nesnelerini eşitlemeyi devre dışı bırakın.

12. **Ayarlar**' ın altında, **kapsam** alanı hangi kullanıcıların ve grupların eşitleneceğini tanımlar. Yalnızca **Kullanıcılar ve gruplar** sekmesinde atanan kullanıcıları ve grupları eşitlemek için **yalnızca atanmış kullanıcıları ve grupları Eşitle** (önerilir) öğesini seçin.
13. Yapılandırmanız tamamlandıktan sonra **sağlama durumunu** **Açık**olarak ayarlayın.
14. Azure AD sağlama hizmetini başlatmak için **Kaydet** ' i seçin.
15. Yalnızca atanmış kullanıcıları ve grupları eşitlese (önerilir), **Kullanıcılar ve gruplar** sekmesini seçtiğinizden emin olun ve eşitlemek istediğiniz kullanıcıları veya grupları atayın.

Başlangıç çevrimi başladıktan sonra, uygulamanızdaki sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri gösteren ilerlemeyi izlemek için sol panelde **sağlama günlükleri** ' ni seçebilirsiniz. Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](check-status-user-account-provisioning.md).

> [!NOTE]
> İlk döngüyü daha sonra, hizmetin çalıştığı sürece yaklaşık 40 dakikada bir gerçekleşen daha sonraki eşitliden daha uzun sürer.

## <a name="step-5-publish-your-application-to-the-azure-ad-application-gallery"></a>5\. Adım: uygulamanızı Azure AD Uygulama Galerisi 'nde yayımlama

Birden fazla kiracı tarafından kullanılacak bir uygulama oluşturuyorsanız, Azure AD uygulama galerisinde kullanılabilir hale getirebilirsiniz. Bu, kuruluşların uygulamayı keşfetmesini ve sağlamayı yapılandırmasını kolaylaştırır. Uygulamanızı Azure AD galerisinde yayımlama ve sağlama sağlamak kolaydır. [Buradaki](../develop/howto-app-gallery-listing.md)adımlara göz atın. Microsoft, uygulamanızı galerimize tümleştirme, uç noktanızı test etme ve müşterilerin kullanması için ekleme [belgelerini](../saas-apps/tutorial-list.md) yayınlama konusunda sizinle birlikte çalışacaktır. 

### <a name="gallery-onboarding-checklist"></a>Galeri ekleme denetim listesi
Uygulamanızın eklendi Quicky olduğundan ve müşterilerin sorunsuz bir dağıtım deneyimine sahip olduğundan emin olmak için aşağıdaki denetim listesini izleyin. Bu bilgiler, galeriye ekleme sırasında sizin için toplanacaktır. 
> [!div class="checklist"]
> * [SCIM 2,0](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#step-2-understand-the-azure-ad-scim-implementation) Kullanıcı ve grup uç noktasını destekleme (yalnızca bir tane gereklidir ancak her ikisi de önerilir)
> * Her kiracı için saniyede en az 25 istek desteklenir (gerekli)
> * Mühendisler için mühendislik ve destek kişileri oluşturun müşteri galerisini ekleme (gerekli)
> * uygulamanız için 3 süresi dolan test kimlik bilgileri (gerekli)
> * Aşağıda açıklandığı gibi, OAuth yetkilendirme kodu yetkisini veya uzun süreli bir belirteci destekler (gerekli)
> * Müşteri galerisinin ekleme işlemini desteklemesi için mühendislik ve destek iletişim noktası oluşturma (gerekli)
> * Tek bir düzeltme ekiyle birden çok grup üyeliğini güncelleştirme desteği (önerilir) 
> * SCıM uç noktanızı genel olarak belgeleyin (önerilir) 
> * [Şema bulmayı destekle](https://tools.ietf.org/html/rfc7643#section-6) (önerilir)


### <a name="authorization-for-provisioning-connectors-in-the-application-gallery"></a>Uygulama galerisinde bağlayıcıları sağlama yetkilendirmesi
SCıM özelliği, kimlik doğrulama ve yetkilendirme için bir SCıM 'e özgü düzen tanımlamaz. Mevcut sektör standartlarının kullanımını temel alır. Azure AD sağlama istemcisi galerideki uygulamalar için iki yetkilendirme yöntemini destekler. 

|Yetkilendirme Yöntemi|Uzmanları|Simgeler|Destek|
|--|--|--|--|
|Kullanıcı adı ve parola (Azure AD tarafından önerilmez veya desteklenmez)|Kolayca uygulanır|Güvenli olmayan- [PA $ $Word önemi yoktur](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/your-pa-word-doesn-t-matter/ba-p/731984)|Galeri uygulamaları için büyük/küçük harf esasına göre desteklenir. Galeri olmayan uygulamalar için desteklenmez.|
|Uzun süreli taşıyıcı belirteci|Uzun süreli belirteçler için bir kullanıcının mevcut olması gerekmez. Yöneticiler, sağlama ayarlarken kolayca kullanılabilir.|Uzun süreli belirteçlerin, e-posta gibi güvenli olmayan yöntemler kullanmadan bir yönetici ile paylaşılması zor olabilir. |Galeri ve Galeri olmayan uygulamalar için desteklenir. |
|OAuth yetkilendirme kodu verme|Erişim belirteçleri parolalardan çok daha kısa süreli ve uzun süreli taşıyıcı belirteçlerinin sahip olmadığı otomatik bir yenileme mekanizmasına sahiptir.  İlk yetkilendirme sırasında, bir sorumluluk düzeyi ekleyerek gerçek bir kullanıcının mevcut olması gerekir. |Bir kullanıcının mevcut olmasını gerektirir. Kullanıcı kuruluştan ayrılırsa, belirteç geçersizdir ve yetkilendirmenin yeniden tamamlanması gerekir.|Galeri uygulamaları için desteklenir. Galeri olmayan uygulamalar için destek çalışma.|
|OAuth istemci kimlik bilgileri verme|Erişim belirteçleri parolalardan çok daha kısa süreli ve uzun süreli taşıyıcı belirteçlerinin sahip olmadığı otomatik bir yenileme mekanizmasına sahiptir. Hem yetkilendirme kodu verme hem de istemci kimlik bilgileri, aynı tür erişim belirtecini oluşturur, bu nedenle bu yöntemler arasında geçiş yapmak API 'ye saydamdır.  Sağlama tamamen otomatikleştirilebilir ve yeni belirteçler Kullanıcı etkileşimi olmadan sessizce istenebilir. ||Galeri ve Galeri olmayan uygulamalar için desteklenmez. Destek kapsamımızda.|

[!NOTE] Azure AD sağlama yapılandırması özel uygulama kullanıcı arabiriminde belirteç alanını boş bırakmanız önerilmez. Oluşturulan belirteç öncelikle test amacıyla kullanılabilir.

**OAuth yetkilendirme kodu verme akışı:** Sağlama hizmeti [yetkilendirme kodu vermeyi](https://tools.ietf.org/html/rfc6749#page-24)destekler. Uygulamanızı galeride yayımlama isteğinizi gönderdikten sonra, takımımız aşağıdaki bilgileri toplamak için sizinle birlikte çalışacaktır:
*  Yetkilendirme URL 'SI: istemci tarafından, Kullanıcı Aracısı yeniden yönlendirme aracılığıyla kaynak sahibinden yetkilendirme elde etmek için bir URL. Kullanıcı, erişim yetkisi vermek için bu URL 'ye yeniden yönlendirilir. 
*  Belirteç Exchange URL 'SI: bir erişim belirtecine, genellikle istemci kimlik doğrulamasıyla bir yetkilendirme izni vermek için istemcinin URL 'SI.
*  İstemci KIMLIĞI: yetkilendirme sunucusu, kayıtlı istemciye, istemci tarafından sunulan kayıt bilgilerini temsil eden benzersiz bir dize olan istemci tanımlayıcısını verir.  İstemci tanımlayıcısı gizli değil; Kaynak sahibine gösterilir ve yalnızca istemci kimlik doğrulaması **için kullanılmamalıdır.**  
*  Gizli anahtar: istemci parolası, yetkilendirme sunucusu tarafından oluşturulan bir gizli dizi. Bu, yalnızca yetkilendirme sunucusu için bilinen benzersiz bir değer olmalıdır. 

İstemci parolasının pozlaması nedeniyle OAuth v1 'nin desteklenmediğini unutmayın. OAuth v2 desteklenir.  

En iyi uygulamalar (önerilir ancak gerekli değildir):
* Birden çok yeniden yönlendirme URL 'sini destekler. Yöneticiler, "portal.azure.com" ve "aad.portal.azure.com" ile sağlamayı yapılandırabilir. Birden çok yeniden yönlendirme URL 'Leri desteklemek, kullanıcıların her iki portaldan da erişim yetkisi alabilmek için
* Kapalı kalma süresi olmadan sorunsuz gizli yenilemeyi sağlamak için birden çok gizli dizi desteği. 

**Uzun süreli OAuth taşıyıcı belirteçleri:** Uygulamanız OAuth yetkilendirme kodu verme akışını desteklemiyorsa, yöneticinin sağlama tümleştirmesini ayarlamak için kullanabileceği bir uzun süreli OAuth taşıyıcı belirteci de oluşturabilirsiniz. Belirtecin kalıcı olması gerekir, aksi takdirde, belirtecin süresi dolarsa sağlama işi [karantinaya](application-provisioning-quarantine-status.md) alınır. Bu belirtecin boyutu 1KB 'tan sonra olmalıdır.  

Ek kimlik doğrulama ve yetkilendirme yöntemleri için [UserVoice](https://aka.ms/appprovisioningfeaturerequest)'ta bize bilgi verin.

### <a name="gallery-go-to-market-launch-check-list"></a>Galeri go-to-market başlatma onay listesi
Birleşme tümleştirmemiz için bir tanıma ve talep konusunda yardımcı olmak üzere, var olan belgelerinizi güncelleştirmenizi ve pazarlama kanallarınızın tümleştirmesinin korunmasını öneririz.  Aşağıda, başlatmayı desteklemek için tamamladığımız bir denetim listesi etkinliği kümesi verilmiştir

* **Satış ve müşteri desteği hazırlığı.** Satış ve destek ekiplerinizin farkında olduğundan ve tümleştirme özelliklerine konuşabildiğinden emin olun. Satış ve destek ekibinizin kısa bir yanı, bunları SSS ile sağlayın ve satış malzemelerinize tümleştirme dahil edin. 
* **Blog gönderisi ve/veya basın yayını.** Birleşik tümleştirmeyi, avantajları ve nasıl başlaılacağını açıklayan bir blog gönderisi veya bir yayın yayını oluşturun. [Örnek: ımprivata ve Azure Active Directory Press yayını](https://www.imprivata.com/company/press/imprivata-introduces-iam-cloud-platform-healthcare-supported-microsoft) 
* **Sosyal medya.** Müşterilerinize tümleştirmeyi yükseltmek için Twitter, Facebook veya LinkedIn gibi sosyal medya ortamınızdan yararlanın. Gönderinizi retweet için @AzureAD eklediğinizden emin olun. [Örnek: ımprivata Twitter gönderisi](https://twitter.com/azuread/status/1123964502909779968)
* **Pazarlama Web sitesi.** Ortak tümleştirmenin kullanılabilirliğini dahil etmek için pazarlama sayfalarınızı (örn. tümleştirme sayfası, iş ortağı sayfası, fiyatlandırma sayfası vb.) oluşturun veya güncelleştirin. [Örnek: Pingboard tümleştirme sayfası](https://pingboard.com/org-chart-for), [Smartsheet tümleştirme sayfası](https://www.smartsheet.com/marketplace/apps/microsoft-azure-ad), [Monday.com fiyatlandırma sayfası](https://monday.com/pricing/) 
* **Teknik belgeler.** Müşterilerin nasıl başlatıladığına ilişkin bir yardım merkezi makalesi veya teknik belgeler oluşturun. [Örnek: Envoy + Microsoft Azure Active Directory Tümleştirmesi.](https://envoy.help/en/articles/3453335-microsoft-azure-active-directory-integration/
) 
* **Müşteri iletişimi.** Müşteri iletişiminizdeki yeni tümleştirmede müşterileri uyarır (aylık bültenler, e-posta kampanyaları, ürün sürüm notları). 

### <a name="allow-ip-addresses-used-by-the-azure-ad-provisioning-service-to-make-scim-requests"></a>Azure AD sağlama hizmeti tarafından SCIM isteklerini yapmak için kullanılan IP adreslerine izin ver

Belirli uygulamalar, uygulamalarına gelen trafiğe izin verir. Azure AD sağlama hizmeti 'nin beklenen şekilde çalışması için, kullanılan IP adreslerine izin verilmelidir. Her bir hizmet etiketi/bölgesinin IP adresleri listesi için bkz. JSON dosyası- [Azure IP aralıkları ve hizmet etiketleri – genel bulut](https://www.microsoft.com/download/details.aspx?id=56519). Gerektiğinde bu IP 'Leri güvenlik duvarınızdan indirebilir ve programlayabilirsiniz. Azure AD sağlama için ayrılmış IP aralıkları "AzureActiveDirectoryDomainServices" altında bulunabilir.

## <a name="related-articles"></a>İlgili makaleler:

* [SaaS uygulamalarına Kullanıcı hazırlama ve sağlamayı kaldırma işlemlerini otomatikleştirme](user-provisioning.md)
* [Kullanıcı hazırlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
* [Öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md)
* [Kullanıcı hazırlama için kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md)
* [Hesap sağlama bildirimleri](user-provisioning.md)
* [SaaS uygulamalarının nasıl tümleştirileceği hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)
