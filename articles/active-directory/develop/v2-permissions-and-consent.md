---
title: Microsoft kimlik platformu kapsamları, izinleri ve onayı
description: Kapsamlar, izinler ve onay dahil olmak üzere Microsoft kimlik platformu bitiş noktasında yetkilendirme açıklaması.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 1/3/2020
ms.author: ryanwi
ms.reviewer: hirsin, jesakowi, jmprieur
ms.custom: aaddev, fasttrack-edit
ms.openlocfilehash: 5495aa6fda189897985ed2f198f6e92c996f6fef
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81868376"
---
# <a name="permissions-and-consent-in-the-microsoft-identity-platform-endpoint"></a>Microsoft kimlik platformu bitiş noktasında izinler ve onay

Microsoft kimlik platformuyla tümlebilen uygulamalar, kullanıcılara ve yöneticilere verilere nasıl erişilebileceği üzerinde denetim sağlayan bir yetkilendirme modeli izler. Yetkilendirme modelinin uygulanması Microsoft kimlik platformu bitiş noktasında güncelleştirildi ve bir uygulamanın Microsoft kimlik platformuyla nasıl etkileşimde olması gerektiğini değiştiriyor. Bu makalede, kapsamlar, izinler ve onay dahil olmak üzere bu yetkilendirme modelinin temel kavramları yer alıyor.

## <a name="scopes-and-permissions"></a>Kapsamlar ve izinler

Microsoft kimlik platformu [OAuth 2.0](active-directory-v2-protocols.md) yetkilendirme protokolünü uygular. OAuth 2.0, üçüncü taraf bir uygulamanın kullanıcı adına web tarafından barındırılan kaynaklara erişebildiği bir yöntemdir. Microsoft kimlik platformu ile tümlağen herhangi bir web barındırılan kaynak bir kaynak tanımlayıcısı veya *Application ID URI*vardır. Örneğin, Microsoft'un web tarafından barındırılan kaynaklarından bazıları şunlardır:

* Microsoft Grafiği:`https://graph.microsoft.com`
* Office 365 Posta API'si:`https://outlook.office.com`
* Azure Anahtar Kasası:`https://vault.azure.net`

> [!NOTE]
> Office 365 Posta API'si vb. yerine Microsoft Graph'ı kullanmanızı şiddetle öneririz.

Aynı durum, Microsoft kimlik platformuyla tümleşik olan tüm üçüncü taraf kaynaklar için de geçerlidir. Bu kaynaklardan herhangi biri, bu kaynağın işlevselliğini daha küçük parçalara bölmek için kullanılabilecek bir izin kümesi de tanımlayabilir. Örnek olarak, [Microsoft Graph](https://graph.microsoft.com) aşağıdaki görevleri yapmak için izinler tanımlamıştır:

* Kullanıcının takvimini okuma
* Kullanıcının takvimine yazma
* Kullanıcı olarak posta gönderme

Bu izin türlerini tanımlayarak, kaynak verileri ve API işlevselliğinin nasıl açıkta olduğu üzerinde ince taneli denetime sahiptir. Bir üçüncü taraf uygulaması, bu izinleri, uygulamanın verilere erişebilmeleri veya kullanıcı adına hareket edeabilmesi için isteği onaylaması gereken kullanıcılardan ve yöneticilerden isteyebilir. Kaynağın işlevselliğini daha küçük izin kümelerine ayırarak, üçüncü taraf uygulamalar yalnızca işlevlerini gerçekleştirmek için gereken belirli izinleri istemek üzere oluşturulabilir. Kullanıcılar ve yöneticiler uygulamanın tam olarak hangi verilere erişebileceğini bilir ve uygulamanın kötü amaçlı davranmadığından daha emin olabilirler. Geliştiriciler her zaman en az ayrıcalık kavramına uymak zorundadır, yalnızca uygulamalarının çalışması için ihtiyaç duydukları izinleri istemelidir.

OAuth 2.0'da bu tür izinlere *kapsam*denir. Bunlar da sık sık *izinleri*olarak adlandırılır. İzin, Microsoft kimlik platformunda dize değeri olarak temsil edilir. Microsoft Graph örneğine devam ederken, her izin için dize değeri:

* Kullanarak bir kullanıcının takvimini okuyun`Calendars.Read`
* Kullanarak kullanıcının takvimine yazma`Calendars.ReadWrite`
* Kullanarak kullanıcı olarak posta gönderme`Mail.Send`

Bir uygulama en sık Microsoft kimlik platformuna isteklerde kapsamları belirterek bu izinleri talep eder bitiş noktası yetkilendirme. Ancak, bazı yüksek ayrıcalık izinleri yalnızca yönetici nin onayı ile verilebilir ve [yönetici onayı bitiş noktası](v2-permissions-and-consent.md#admin-restricted-permissions)kullanılarak istenir/verilebilir. Daha fazla bilgi edinmek için okumaya devam edin.

## <a name="permission-types"></a>İzin türleri

Microsoft kimlik platformu iki tür izinleri destekler: **devredilen izinler** ve **uygulama izinleri.**

* **Yetkin izinler,** oturum açmış bir kullanıcı ya da oturum açmış uygulamalar tarafından kullanılır. Bu uygulamalar için, kullanıcı veya yönetici, uygulamanın talep ettiği izinleri kabul eder ve uygulama, hedef kaynağa çağrı yaparken oturum açmış kullanıcı olarak hareket etme izni verir. Bazı devredilen izinler yönetim dışı kullanıcılar tarafından onaylanabilir, ancak bazı yüksek ayrıcalıklı izinler [yönetici onayı](v2-permissions-and-consent.md#admin-restricted-permissions)gerektirir. Hangi yönetici rollerinin devralınan izinlere izin verebileceğini öğrenmek için Azure [AD'da Yönetici rol izinlerine](../users-groups-roles/directory-assign-admin-roles.md)bakın.

* **Uygulama izinleri,** oturum açmış bir kullanıcı olmadan çalışan uygulamalar tarafından kullanılır; örneğin, arka plan hizmetleri veya daemons olarak çalışan uygulamalar.  Uygulama izinleri yalnızca [bir yönetici tarafından onaylanabilir.](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant)

_Etkili izinler,_ uygulamanızın hedef kaynağa istekte bulunmada sahip olacağı izinlerdir. Hedef kaynağa arama yaparken uygulamanızın verildiği temsilci ve uygulama izinleri ile etkili izinleri arasındaki farkı anlamak önemlidir.

- Temsilci izinleri için, uygulamanızın _etkili izinleri,_ uygulamanın verildiği yetkilendirilmiş izinlerin (rıza yoluyla) ve şu anda oturum açmış kullanıcının ayrıcalıklarının en az ayrıcalıklı kesişimi olacaktır. Uygulamanızın ayrıcalıkları hiçbir zaman oturum açmış kullanıcının ayrıcalıklarından fazla olamaz. Kuruluşların içinde, oturum açmış kullanıcının ayrıcalıkları ilkeyle ya da bir veya birden çok yönetici rolü üyeliğiyle belirlenebilir. Hangi yönetici rollerinin devralınan izinlere izin verebileceğini öğrenmek için Azure [AD'da Yönetici rol izinlerine](../users-groups-roles/directory-assign-admin-roles.md)bakın.

   Örneğin, uygulamanızın _User.ReadWrite.All_ delegated izninin verildiğini varsayalım. Adından da anlaşıldığı gibi bu izin uygulamanıza kuruluştaki her kullanıcının profilini okuma ve güncelleştirme izni verir. Oturum açmış kullanıcının bir genel yönetici olması durumunda, uygulamanız kuruluştaki her kullanıcının profilini güncelleştirebilir. Ancak, oturum açmış kullanıcı yönetici rolünde değilse, uygulamanız yalnızca oturum açmış kullanıcının profilini güncelleyebilir. Kuruluştaki diğer kullanıcıların profillerini güncelleştiremez çünkü adına çalışma iznine sahip olduğu kullanıcı söz konusu ayrıcalıklara sahip değildir.

- Uygulama izinleri için, uygulamanızın _etkin izinleri,_ izintarafından ima edilen tüm ayrıcalıklar düzeyinde olacaktır. Örneğin, _User.ReadWrite.All_ uygulama izni olan bir uygulama kuruluştaki her kullanıcının profilini güncelleyebilir.

## <a name="openid-connect-scopes"></a>OpenID Connect kapsamları

OpenID Connect'in Microsoft kimlik platformu uygulaması, belirli bir kaynak için geçerli `openid`olmayan `email` `profile`birkaç `offline_access`iyi tanımlanmış kapsama sahiptir: , , ve . ve `address` `phone` OpenID Connect kapsamları desteklenmez.

### <a name="openid"></a>Openıd

Bir uygulama [OpenID Connect](active-directory-v2-protocols.md)kullanarak oturum açma gerçekleştiriyorsa, `openid` kapsamı istemesi gerekir. Kapsam, `openid` çalışma hesabı onay sayfasında "Oturum açma" izni olarak, kişisel Microsoft hesap onayı sayfasında ise "Profilinizi görüntüleyin ve Microsoft hesabınızı kullanarak uygulamalara ve hizmetlere bağlanın" izni olarak gösterilmektedir. Bu izinle, bir uygulama `sub` talep şeklinde kullanıcı için benzersiz bir tanımlayıcı alabilir. Ayrıca uygulamanın UserInfo bitiş noktasına erişmesini sağlar. Kapsam, `openid` kimlik doğrulama için uygulama tarafından kullanılabilecek kimlik belirteçleri elde etmek için Microsoft kimlik platformu belirteç uç noktasında kullanılabilir.

### <a name="email"></a>e-posta

`email` Kapsam `openid` kapsamı ve diğerleri ile kullanılabilir. Uygulamanın `email` talep şeklinde kullanıcının birincil e-posta adresine erişmesini sağlar. Talep, `email` yalnızca bir e-posta adresi kullanıcı hesabıyla ilişkiliyse bir belirtecine dahil edilir, ki bu her zaman böyle değildir. `email` Kapsamı kullanıyorsa, uygulamanız belirteçte talebin `email` bulunmadığı bir durumu işlemek için hazır olmalıdır.

### <a name="profile"></a>profil

`profile` Kapsam `openid` kapsamı ve diğerleri ile kullanılabilir. Uygulamanın kullanıcı hakkında önemli miktarda bilgiye erişmesini sağlar. Erişebileceği bilgiler, kullanıcının verilen adını, soyadını, tercih edilen kullanıcı adını ve nesne kimliğini içerir, ancak bununla sınırlı değildir. Belirli bir kullanıcı için id_tokens parametrede bulunan profil taleplerinin tam listesi için [ `id_tokens` başvuruya](id-tokens.md)bakın.

### <a name="offline_access"></a>offline_access

[ `offline_access` Kapsam,](https://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) uygulamanızın kullanıcı adına kaynaklara uzun süre erişmesini sağlar. İzin sayfasında, bu kapsam "Erişim izni vermiş olduğunuz verilere erişimi koruyun" olarak görünür. Bir kullanıcı `offline_access` kapsamı onayladığı zaman, uygulamanız Microsoft kimlik platformu belirteç uç noktasından yeni belirteçleri alabilir. Yenileme belirteçleri uzun ömürlüdür. Eskilerin süresi dolduğunda uygulamanız yeni erişim jetonları alabilir.

> [!NOTE]
> Bu izin, yenileme belirteci [(örtük akış)](v2-oauth2-implicit-grant-flow.md)sağlamayan akışlar için bile bugün tüm onay ekranlarında görünür.  Bu, istemcinin örtük akış içinde başlayıp yenibir belirteç beklendiği kod akışına geçebileceği senaryoları kapsayacak şekildedir.

Microsoft kimlik platformunda (v2.0 bitiş noktasına yapılan istekler) uygulamanızın yeni belirteçleri almak için `offline_access` kapsamı açıkça istemesi gerekir. Bu, [OAuth 2.0 yetkilendirme kodu akışında](active-directory-v2-protocols.md)bir yetkilendirme kodu kullandığınızda, `/token` bitiş noktasından yalnızca bir erişim belirteci alacağınız anlamına gelir. Erişim belirteci kısa bir süre için geçerlidir. Erişim belirteci genellikle bir saat içinde sona erer. Bu noktada, uygulamanızın yeni bir yetkilendirme kodu `/authorize` almak için kullanıcıyı bitiş noktasına yönlendirmesi gerekir. Bu yönlendirme sırasında, uygulamanın türüne bağlı olarak, kullanıcının kimlik bilgilerini yeniden girmesi veya izinleri yeniden kabul etmesi gerekebilir.

Yenileme belirteçlerini alma ve kullanma hakkında daha fazla bilgi için [Microsoft kimlik platformu protokol başvurusuna](active-directory-v2-protocols.md)bakın.

## <a name="requesting-individual-user-consent"></a>Bireysel kullanıcı onayı isteme

[OpenID Connect veya OAuth 2.0](active-directory-v2-protocols.md) yetkilendirme isteğinde, bir uygulama sorgu `scope` parametresini kullanarak ihtiyaç duyduğu izinleri isteyebilir. Örneğin, bir kullanıcı bir uygulamaya girdiğinde, uygulama aşağıdaki örnekteki gibi bir istek gönderir (okunabilirlik için eklenen satır sonları ile):

```HTTP
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendars.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

Parametre, `scope` uygulamanın istediği yetki alan ayrılmış izinlerin listesidir. Her izin, kaynağın tanımlayıcısına (Uygulama Kimliği URI) izin değeri eklenerek gösterilir. İstek örneğinde, uygulamanın kullanıcının takvimini okumak ve kullanıcı olarak posta göndermek için izin alması gerekir.

Kullanıcı kimlik bilgilerini girdikten sonra, Microsoft kimlik platformu bitiş noktası *kullanıcı onayı*eşleşen bir kayıt için denetler. Kullanıcı geçmişte istenen izinlerden herhangi birini kabul etmemişse veya bir yönetici tüm kuruluş adına bu izinlere izin vermemişse, Microsoft kimlik platformu bitiş noktası kullanıcıdan istenen izinleri vermesini ister.

> [!NOTE]
>Şu anda, `offline_access` ("Erişim izni vermiş olduğunuz verilere erişimi `user.read` koruyun") ve ("Oturum açın ve profilinizi okuyun") izinleri otomatik olarak bir uygulamanın ilk iznine dahil edilir.  Bu izinler genellikle uygun uygulama işlevselliği için gereklidir - `offline_access` uygulamanın yerel ve web uygulamaları `user.read` için kritik `sub` olan yenileme belirteçlerine erişim sağlarken, talep temennisine erişim sağlar ve istemcinin veya uygulamanın kullanıcıyı zaman içinde doğru bir şekilde tanımlamasına ve temel kullanıcı bilgilerine erişmesine olanak tanır.

![İş hesabı rızasını gösteren örnek ekran görüntüsü](./media/v2-permissions-and-consent/work_account_consent.png)

Kullanıcı izin isteğini onayladığında, onay kaydedilir ve kullanıcının uygulamanın sonraki oturum açmalarında tekrar onay vermek zorunda kalmaması gerekir.

## <a name="requesting-consent-for-an-entire-tenant"></a>Tüm kiracı için onay isteme

Genellikle, bir kuruluş bir uygulama için lisans veya abonelik satın aldığında, kuruluş uygulamayı kuruluşun tüm üyeleri tarafından kullanılmak üzere proaktif olarak ayarlamak ister. Bu işlemin bir parçası olarak, bir yönetici, uygulamanın kiracıdaki herhangi bir kullanıcı adına hareket etmesi için onay verebilir. Yönetici tüm kiracı için izin verirse, kuruluşun kullanıcıları uygulama için bir onay sayfası görmez.

Kiracıdaki tüm kullanıcılar için temsilci izni için onay istemek için, uygulamanız yönetici onayı bitiş noktasını kullanabilir.

Ayrıca, uygulamaların Uygulama İzinleri istemek için yönetici onayı bitiş noktasını kullanması gerekir.

## <a name="admin-restricted-permissions"></a>Yönetici kısıtlamalı izinler

Microsoft ekosistemindeki bazı yüksek ayrıcalıklı izinler *yönetici kısıtlamalı*olarak ayarlanabilir. Bu tür izinlere örnek olarak aşağıdakiler verilebilir:

* Kullanarak tüm kullanıcının tüm profillerini okuyun`User.Read.All`
* Kullanarak verileri kuruluşun dizinine yazma`Directory.ReadWrite.All`
* Bir kuruluşun dizinindeki tüm grupları kullanarak okuyun`Groups.Read.All`

Bir tüketici kullanıcısı bu tür verilere bir uygulama erişimi tanısa da, kuruluş kullanıcılarının aynı hassas şirket verilerine erişim izni vermesi kısıtlanır. Uygulamanız bu izinlerden birine kuruluş kullanıcısından erişmesini isterse, kullanıcı uygulamanızın izinlerini onaylama yetkisine sahip olmadığını belirten bir hata iletisi alır.

Uygulamanız kuruluşlar için yönetici tarafından kısıtlanmış kapsamlara erişim gerektiriyorsa, bunları bir sonraki açıklanan yönetici onayı bitiş noktasını da kullanarak doğrudan bir şirket yöneticisinden talep etmeniz gerekir.

Uygulama yüksek ayrıcalıklı yetki ler talep ediyorsa ve yönetici bu izinleri yönetici nin onay bitiş noktası üzerinden veriyorsa, kiracıdaki tüm kullanıcılar için onay verilir.

Uygulama uygulama izinleri talep ediyorsa ve yönetici bu izinleri yönetici onayı bitiş noktası üzerinden veriyorsa, bu hibe belirli bir kullanıcı adına yapılmaz. Bunun yerine, istemci uygulaması *doğrudan*izinleri verilir. Bu tür izinler yalnızca daemon hizmetleri ve arka planda çalışan etkileşimli olmayan diğer uygulamalar tarafından kullanılır.

## <a name="using-the-admin-consent-endpoint"></a>Yönetici onayı bitiş noktasını kullanma

> [!NOTE]
> Yönetici onayı bitiş noktasını kullanarak yönetici onayı verdikten sonra, yönetici onayı vermeyi tamamlamış olduğunuz ve kullanıcıların başka ek işlem gerçekleştirmelerine gerek olmadığını lütfen unutmayın. Yönetici onayı verdikten sonra, kullanıcılar tipik bir auth akışı üzerinden bir erişim belirteci alabilirsiniz ve ortaya çıkan erişim belirteci onaylayan izinlere sahip olacaktır.

Bir Şirket Yöneticisi uygulamanızı kullandığında ve yetkili bitiş noktasına yönlendirildiğinde, Microsoft kimlik platformu kullanıcının rolünü algılar ve talep ettiğiniz izinler için tüm kiracı adına onay vermek isteyip istemediklerini sorar. Ancak, bir yöneticinin tüm kiracı adına izin verdiğini proaktif olarak talep etmek istiyorsanız kullanabileceğiniz özel bir yönetici onayı bitiş noktası da vardır. Bu bitiş noktasının kullanılması, Uygulama İzinleri istemek için de gereklidir (yetkili bitiş noktası kullanılarak istenemez).

Bu adımları izlerseniz, uygulamanız yönetici tarafından kısıtlanmış kapsamlar da dahil olmak üzere kiracıdaki tüm kullanıcılar için izin isteyebilir. Bu yüksek bir ayrıcalık işlemidir ve yalnızca senaryonuz için gerekli olduğunda yapılmalıdır.

Adımları uygulayan bir kod örneği görmek [için, yönetici tarafından kısıtlanmış kapsamlar örneğine](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2)bakın.

### <a name="request-the-permissions-in-the-app-registration-portal"></a>Uygulama kayıt portalındaki izinleri isteme

Uygulamalar, uygulama kayıt portalında hangi izinleri (hem temsilci hem de uygulama) istediklerini not alabilirler.  `/.default` Bu, kapsamın ve Azure portalının "Yönetici onayı verin" seçeneğinin kullanılmasına olanak tanır.  Genel olarak, belirli bir uygulama için statik olarak tanımlanan izinlerin dinamik/artımlı olarak isteyeceği izinlerin bir üst kümesi olduğundan emin olmak en iyi yöntemdir.

> [!NOTE]
>Uygulama izinleri yalnızca uygulama izinleri [`/.default`](#the-default-scope) ile istenebilir - bu nedenle uygulamanızın uygulama izinlerine ihtiyacı varsa, uygulama kayıt portalında listelendiklerinden emin olun.

#### <a name="to-configure-the-list-of-statically-requested-permissions-for-an-application"></a>Bir uygulama için statik olarak istenen izinlerin listesini yapılandırmak için

1. [Azure portalında](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza gidin – Uygulama kayıtları deneyimi veya henüz yapmadıysanız [bir uygulama oluşturun.](quickstart-register-app.md)
2. **API İzinleri** bölümünü bulun ve API izinleri içinde İzin ekle'yi tıklatın.
3. Kullanılabilir API'ler listesinden **Microsoft Graph'ı** seçin ve ardından uygulamanızın gerektirdiği izinleri ekleyin.
3. Uygulama kaydını **kaydedin.**

### <a name="recommended-sign-the-user-into-your-app"></a>Önerilen: Kullanıcıyı uygulamanızda oturum

Genellikle, yönetici onayı bitiş noktasını kullanan bir uygulama oluşturduğunuzda, uygulamanın yöneticinin uygulamanın izinlerini onaylayacağı bir sayfaya veya görünüme ihtiyacı vardır. Bu sayfa, uygulamanın kaydolma akışının, uygulamaayarlarının bir parçası olabilir veya özel bir "bağlantı" akışı olabilir. Çoğu durumda, uygulamanın bu "bağlantı" görünümünü yalnızca bir kullanıcı bir iş veya okul Microsoft hesabıyla oturum açtındıktan sonra göstermesi mantıklıdır.

Kullanıcıyı uygulamanızda oturum açtığınızda, gerekli izinleri onaylamalarını istemeden önce yöneticinin ait olduğu kuruluşu tanımlayabilirsiniz. Kesinlikle gerekli olmasa da, kuruluş kullanıcılarınız için daha sezgisel bir deneyim oluşturmanıza yardımcı olabilir. Kullanıcıyı oturum alabilmek için [Microsoft kimlik platformu protokol eğitimlerimizi](active-directory-v2-protocols.md)uygulayın.

### <a name="request-the-permissions-from-a-directory-admin"></a>İzinleri bir dizin yöneticisinden isteme

Kuruluşunuzun yöneticisinden izin istemeye hazır olduğunuzda, kullanıcıyı Microsoft kimlik platformu *yönetici onayı bitiş noktasına*yönlendirebilirsiniz.

```HTTP
// Line breaks are for legibility only.
GET https://login.microsoftonline.com/{tenant}/v2.0/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
&scope=
https://graph.microsoft.com/calendars.read
https://graph.microsoft.com/mail.send
```


| Parametre        | Koşul        | Açıklama                                                                                |
|:--------------|:--------------|:-----------------------------------------------------------------------------------------|
| `tenant` | Gerekli | İzin istemek istediğiniz dizin kiracısı. GUID veya dost ad biçiminde sağlanabilir VEYA genel olarak örnekte görüldüğü gibi kuruluşlarla başvurulabilir. Kişisel hesaplar kiracı bağlamı dışında yönetici onayı sağlayamadığıiçin 'ortak' kullanmayın. Kiracıları yöneten kişisel hesaplarla en iyi uyumluluğu sağlamak için, mümkün olduğunda kiracı kimliğini kullanın. |
| `client_id` | Gerekli | Azure portalı - Uygulama [kayıtlarının](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atandığı **Uygulama (istemci) Kimliği.** |
| `redirect_uri` | Gerekli |UYGULAMANIZIN işlemesi için yanıtın gönderilmesini istediğiniz uri'yi yeniden yönlendirin. Uygulama kayıt portalına kaydettiğiniz yönlendirme URL'lerinden biriyle tam olarak eşleşmelidir. |
| `state` | Önerilen | Belirteç yanıtında da döndürülecek isteğe dahil edilen bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada kodlamak için durumu (örneğin, üzerinde oldukları sayfa veya görünüm) kullanın. |
|`scope`        | Gerekli        | Uygulama tarafından istenen izin kümesini tanımlar. Bu statik (kullanarak) [`/.default`](#the-default-scope)veya dinamik kapsamlar olabilir.  Bu OIDC kapsamları içerebilir`openid` `profile`( `email`, , . Uygulama izinlerine ihtiyacınız varsa, `/.default` statik olarak yapılandırılan izinler listesini istemek için kullanmanız gerekir.  |


Bu noktada, Azure AD isteği tamamlamak için bir kiracı yöneticinin oturum etmesini gerektirir. Yöneticiden `scope` parametrede istediğiniz tüm izinleri onaylaması istenir.  Statik ( )`/.default`bir değer kullandıysanız, v1.0 yönetici onayı bitiş noktası gibi çalışır ve uygulama için gerekli izinlerde bulunan tüm kapsamlar için onay ister.

#### <a name="successful-response"></a>Başarılı yanıt

Yönetici uygulamanızın izinlerini onaylarsa, başarılı yanıt aşağıdaki gibi görünür:

```HTTP
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametre | Açıklama |
| --- | --- |
| `tenant` | Başvurunuza istediği izinleri GUID biçiminde veren dizin kiracısı. |
| `state` | İstekte yer alan ve belirteç yanıtında da döndürülecek bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada kodlamak için kullanılır(örneğin, üzerinde oldukları sayfa veya görünüm). |
| `admin_consent` | Olarak `True`ayarlanacak. |

#### <a name="error-response"></a>Hata yanıtı

Yönetici uygulamanızın izinlerini onaylamazsa, başarısız yanıt aşağıdaki gibi görünür:

```HTTP
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametre | Açıklama |
| --- | --- |
| `error` | Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi. |
| `error_description` | Bir geliştiricinin hatanın temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

Yönetici onayı bitiş noktasından başarılı bir yanıt aldıktan sonra, uygulamanız istediği izinleri almıştır. Ardından, istediğiniz kaynak için bir belirteç isteyebilirsiniz.

## <a name="using-permissions"></a>İzinleri kullanma

Kullanıcı uygulamanızın izinlerini kabul ettikten sonra, uygulamanız bir kaynağa erişmek için uygulamanızın iznini temsil eden erişim belirteçleri edinebilir. Erişim jetonu yalnızca tek bir kaynak için kullanılabilir, ancak erişim belirteci içinde kodlanmış, uygulamanızın bu kaynak için verdiği her izindir. Bir erişim jetonu edinmek için, uygulamanız Microsoft kimlik platformu belirteç bitiş noktasına aşağıdaki gibi bir istekte bulunabilir:

```HTTP
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Elde edilen erişim belirteci'ni KAYNAĞA HTTP isteklerinde kullanabilirsiniz. Kaynağa, uygulamanızın belirli bir görevi gerçekleştirmek için uygun izne sahip olduğunu güvenilir bir şekilde gösterir.

OAuth 2.0 protokolü ve erişim belirteçleri hakkında daha fazla bilgi için [Microsoft kimlik platformu bitiş noktası protokolü başvurusuna](active-directory-v2-protocols.md)bakın.

## <a name="the-default-scope"></a>/.varsayılan kapsam

`/.default` Kapsamı, uygulamalarınızı v1.0 bitiş noktasından Microsoft kimlik platformu bitiş noktasına geçirmenize yardımcı olmak için kullanabilirsiniz. Bu, uygulama kaydında yapılandırılan izinlerin statik listesini ifade eden her uygulama için yerleşik bir kapsamdır. Bir `scope` değer `https://graph.microsoft.com/.default` işlevsel olarak v1.0 uç noktalarıyla `resource=https://graph.microsoft.com` aynıdır - yani, uygulamanın Azure portalında kaydolduğu Microsoft Graph'taki kapsamları içeren bir belirteç ister.  Uri + (örneğin, `/.default` kaynak URI ise, `https://contosoApp.com`istenen kapsam olacaktır) `https://contosoApp.com/.default`kullanılarak oluşturulur.  Belirteci doğru bir şekilde istemek için ikinci bir eğik çizgi eklemeniz gereken servis talepleri için [sondaki kesikler bölümüne](#trailing-slash-and-default) bakın.

/.varsayılan kapsam herhangi bir OAuth 2.0 akışında kullanılabilir, ancak [On-Behalf-Of akışı](v2-oauth2-on-behalf-of-flow.md) ve [istemci kimlik bilgileri akışında](v2-oauth2-client-creds-grant-flow.md)ve başvuru izinlerini istemek için v2 yönetici onay bitiş noktasını kullanırken gereklidir.

> [!NOTE]
> İstemciler statik (`/.default`) ve dinamik onayı tek bir istekte birleştiremez. Böylece, `scope=https://graph.microsoft.com/.default+mail.read` kapsam türlerinin birleşimi nedeniyle bir hataya neden olur.

### <a name="default-and-consent"></a>/.temerrüt ve rıza

Kapsam `/.default` için de v1.0 uç `prompt=consent` noktası davranışını tetikler. Kaynaktan bağımsız olarak, uygulama tarafından kayıtlı tüm izinler için onay ister. İsteğin bir parçası olarak dahil `/.default` edilirse, kapsam istenen kaynağın kapsamlarını içeren bir belirteç döndürür.

### <a name="default-when-the-user-has-already-given-consent"></a>/.varsayılan kullanıcı zaten onay vermiş

İşlevsel olarak -merkezli v1.0 uç noktasının `/.default` `resource`davranışıyla aynı olduğundan, v1.0 bitiş noktasının onay davranışını da beraberinde getirir. Yani, `/.default` yalnızca istemci ve kaynak arasında kullanıcı tarafından izin verilmediyse bir onay istemi tetikler. Böyle bir onay varsa, kullanıcı tarafından söz konusu kaynak için verilen tüm kapsamları içeren bir belirteç döndürülür. Ancak, izin verilmediyse veya `prompt=consent` parametre sağlanmışsa, istemci uygulaması tarafından kayıtlı tüm kapsamlar için bir onay istemi gösterilir.

#### <a name="example-1-the-user-or-tenant-admin-has-granted-permissions"></a>Örnek 1: Kullanıcı veya kiracı yönetici, izinler verdi

Bu örnekte, kullanıcı (veya kiracı yönetici) istemciye Microsoft Graph `mail.read` `user.read`izinleri ve . İstemci için `scope=https://graph.microsoft.com/.default`bir istekte bulunarsa, istemci uygulamalarının içeriğine bakılmaksızın Microsoft Graph için kayıtlı izinler gösterilmez. Bir belirteç kapsamları `mail.read` içeren `user.read`döndürülür ve.

#### <a name="example-2-the-user-hasnt-granted-permissions-between-the-client-and-the-resource"></a>Örnek 2: Kullanıcı istemci ve kaynak arasında izin vermemiştir

Bu örnekte, istemci ve Microsoft Graph arasında kullanıcı için hiçbir onay yoktur. İstemci, Azure `user.read` `contacts.read` Anahtar Kasası kapsamının `https://vault.azure.net/user_impersonation`yanı sıra izinler ve izinler için kaydoldu. İstemci için `scope=https://graph.microsoft.com/.default`bir belirteç istediğinde , kullanıcı `user.read`için `contacts.read`bir onay `user_impersonation` ekranı görür , , ve Anahtar Vault kapsamları. Döndürülen belirteç yalnızca `user.read` `contacts.read` içinde ve kapsamları olacak ve yalnızca Microsoft Graph'a karşı kullanılabilir.

#### <a name="example-3-the-user-has-consented-and-the-client-requests-additional-scopes"></a>Örnek 3: Kullanıcı onay verdi ve istemci ek kapsamlar talep ediyor

Bu örnekte, kullanıcı istemci `mail.read` için zaten izin verdi. Müşteri, bu `contacts.read` kapsamda tescil edilmiştir. İstemci bir belirteç kullanarak `scope=https://graph.microsoft.com/.default` bir istekte bulunup onay `prompt=consent`istediğinde, kullanıcı uygulama tarafından kayıtlı izinler için herkes için bir onay ekranı görür (ve yalnızca). `contacts.read`onay ekranında bulunacaktır, `mail.read` ancak olmayacaktır. Döndürülen belirteç Microsoft Graph için `mail.read` `contacts.read`olacak ve içerecek ve .

### <a name="using-the-default-scope-with-the-client"></a>/.default kapsamını istemciyle kullanma

Bir istemcinin `/.default` kendi `/.default` kapsamını talep ettiği kapsamda özel bir durum vardır. Aşağıdaki örnek, bu senaryoyu göstermektedir.

```HTTP
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
response_type=token            //code or a hybrid flow is also possible here
&client_id=9ada6f8a-6d83-41bc-b169-a306c21527a5
&scope=9ada6f8a-6d83-41bc-b169-a306c21527a5/.default
&redirect_uri=https%3A%2F%2Flocalhost
&state=1234
```

Bu, tüm kayıtlı izinler için bir onay ekranı üretir (yukarıdaki `/.default`onay açıklamalarına göre varsa ve varsa), erişim jetonu yerine bir id_token döndürür.  Bu davranış, ADAL'dan MSAL'a geçen belirli eski istemciler için vardır ve Microsoft kimlik platformu bitiş noktasını hedefleyen yeni istemciler tarafından **kullanılmamalıdır.**

### <a name="trailing-slash-and-default"></a>Sondaki eğik çizgi ve /.varsayılan

Bazı kaynak URI'leri,`https://contoso.com/` belirteç `https://contoso.com`doğrulamayla ilgili sorunlara neden olabilecek bir çizgi (aksine) olabilir.  Bu, öncelikle, kaynakları URI'de son da`https://management.azure.com/`yanaşan ve belirteç istendiğinde bulunmasını gerektiren Azure Kaynak Yönetimi ( ) için bir belirteç isteğinde bulunabilir.  Böylece, için `https://management.azure.com/` bir belirteç `/.default`talep ederken `https://management.azure.com//.default` ve kullanırken , talep etmelidir - çift eğik çizgi dikkat!

Genel olarak - belirteç veriliyor olduğunu doğruladıysanız ve belirteç bunu kabul etmesi gereken API tarafından reddediliyorsa, ikinci bir eğik çizgi eklemeyi ve yeniden denemeyi düşünün. Bu, oturum açma sunucusunun `scope` parametredeki URI'lerle eşleşen hedef kitleyle `/.default` bir belirteç yayık lığı ve sonundan kaldırılmış olması durumunda gerçekleşir.  Bu, sondaki eğik çizgiyi kaldırırsa, oturum açma sunucusu isteği yine de işler ve artık eşleşmeseler bile kaynak URI'ye karşı doğrular - bu standart değildir ve uygulamanız tarafından güvenilmemelidir.

## <a name="troubleshooting-permissions-and-consent"></a>Sorun giderme izinleri ve onayı

Siz veya uygulamanızın kullanıcıları onay işlemi sırasında beklenmeyen hatalar görüyorsanız, sorun giderme adımları için bu makaleye bakın: [Bir uygulamaya onay verirken beklenmeyen hata.](../manage-apps/application-sign-in-unexpected-user-consent-error.md)
