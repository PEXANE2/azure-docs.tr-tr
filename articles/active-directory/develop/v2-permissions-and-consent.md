---
title: Microsoft Identity platform kapsamları, izinleri ve onayı
description: Microsoft Identity platform uç noktasındaki kapsamlar, izinler ve onay dahil olmak üzere yetkilendirme açıklaması.
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
ms.openlocfilehash: d513dbd8449dad1d34117e06970f0c0881462aa3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84263236"
---
# <a name="permissions-and-consent-in-the-microsoft-identity-platform-endpoint"></a>Microsoft kimlik platformu uç noktasında izinler ve onay

Microsoft Identity platformu ile tümleştirilen uygulamalar, kullanıcılara ve yöneticilere verilere nasıl erişilebileceği üzerinde denetim sağlayan bir yetkilendirme modeli izler. Yetkilendirme modelinin uygulanması Microsoft Identity platform uç noktasında güncelleştirilmiştir ve bir uygulamanın Microsoft Identity platformu ile nasıl etkileşim kurması gerektiğini değiştiriyor. Bu makalede kapsamlar, izinler ve onay dahil olmak üzere bu yetkilendirme modelinin temel kavramları ele alınmaktadır.

## <a name="scopes-and-permissions"></a>Kapsamlar ve izinler

Microsoft Identity platformu, [OAuth 2,0](active-directory-v2-protocols.md) yetkilendirme protokolünü uygular. OAuth 2,0, bir üçüncü taraf uygulamanın bir kullanıcı adına Web 'de barındırılan kaynaklara erişebileceği bir yöntemdir. Microsoft Identity platformu ile tümleştirilen Web 'de barındırılan herhangi bir kaynağın bir kaynak tanımlayıcısı veya *uygulama kimliği URI 'si*vardır. Örneğin, Microsoft 'un web 'de barındırılan kaynaklarından bazıları şunlardır:

* Microsoft Graph:`https://graph.microsoft.com`
* Office 365 posta API 'SI:`https://outlook.office.com`
* Azure Key Vault:`https://vault.azure.net`

> [!NOTE]
> Office 365 posta API 'SI vb. yerine Microsoft Graph kullanmanızı kesinlikle öneririz.

Aynı değer, Microsoft Identity platformu ile tümleştirilmiş olan tüm üçüncü taraf kaynakları için de geçerlidir. Bu kaynakların herhangi biri aynı zamanda söz konusu kaynağın işlevselliğini daha küçük parçalara bölmek için kullanılabilecek bir izinler kümesi tanımlayabilir. Örnek olarak [Microsoft Graph](https://graph.microsoft.com) , diğerleri arasında aşağıdaki görevleri yapmak için tanımlı izinlere sahiptir:

* Kullanıcının takvimini oku
* Kullanıcının takvimine yazma
* Kullanıcı olarak posta gönder

Bu tür izinleri tanımlayarak, kaynak, verileri üzerinde ayrıntılı denetime sahiptir ve API işlevinin sunulma şeklini içerir. Üçüncü taraf bir uygulama, kullanıcıların ve yöneticilerin bu izinleri talep edebilir ve uygulamanın verilere erişebilmeleri veya Kullanıcı adına işlem yapması için isteği onaylaması gerekir. Kaynağın işlevselliğini daha küçük izin kümelerine ayırarak, üçüncü taraf uygulamalar yalnızca kendi işlevlerini gerçekleştirmeleri için gereken belirli izinleri istemek üzere oluşturulabilir. Kullanıcılar ve Yöneticiler, uygulamanın erişebileceği verileri tam olarak bilir ve kötü amaçlı olarak davranmadığından daha emin olabilirler. Geliştiriciler, yalnızca uygulamalarının çalışması için ihtiyaç duydukları izinleri soran en az ayrıcalık kavramını her zaman bilmelidir.

OAuth 2,0 ' de, bu tür izinler *kapsam*olarak adlandırılır. Bunlar da genellikle *izinler*olarak adlandırılır. Bir izin, Microsoft Identity platformunda bir dize değeri olarak temsil edilir. Microsoft Graph örnekle devam edildiğinde, her izin için dize değeri:

* Kullanarak bir kullanıcının takvimini okuma`Calendars.Read`
* Kullanarak bir kullanıcının takvimine yazma`Calendars.ReadWrite`
* İle Kullanıcı olarak posta gönder`Mail.Send`

Bu izinleri en yaygın olarak, Microsoft Identity platform yetkilendirme uç noktası isteklerindeki kapsamları belirterek ister. Ancak, belirli yüksek ayrıcalık izinleri yalnızca yönetici onayı üzerinden verilebilir ve [Yönetici onay uç noktası](v2-permissions-and-consent.md#admin-restricted-permissions)kullanılarak istenir/verilir. Daha fazla bilgi edinmek için okumaya devam edin.

## <a name="permission-types"></a>İzin türleri

Microsoft Identity platform iki tür izni destekler: **temsilci izinleri** ve **Uygulama izinleri**.

* **Temsilci izinleri** , oturum açmış bir Kullanıcı bulunan uygulamalar tarafından kullanılır. Bu uygulamalar için, Kullanıcı veya yönetici, uygulamanın istediği izinlere sahiptir ve uygulamanın, hedef kaynağa çağrı yaparken oturum açmış kullanıcı olarak görev yapması için izin verilir. Bazı temsilci izinleri yönetici olmayan kullanıcılar tarafından yapılabilir, ancak daha yüksek ayrıcalıklı izinler [yönetici onayı](v2-permissions-and-consent.md#admin-restricted-permissions)gerektirir. Hangi Yönetici rollerinin temsilci izinleri onaylamasına izin verebileceğini öğrenmek için bkz. [Azure AD 'de yönetici rolü izinleri](../users-groups-roles/directory-assign-admin-roles.md).

* **Uygulama izinleri** , oturum açmış bir kullanıcı olmadan çalışan uygulamalar tarafından kullanılır; Örneğin, arka plan hizmetleri veya Daemon 'ları olarak çalışan uygulamalar.  Uygulama izinleri yalnızca [bir yönetici tarafından](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant)alınabilir.

_Etkili izinler_ , uygulamanızın hedef kaynağa istek yaparken sahip olacağı izinlerdir. Hedef kaynağa çağrı yaparken uygulamanızın izin verdiği ve geçerli izinlerinin kullanıldığı temsilci ve uygulama izinleri arasındaki farkı anlamak önemlidir.

- Temsilci izinleri için, uygulamanızın _etkili izinleri_ , uygulamaya atanan izinlerin en az ayrıcalıklı kesişimi (izin aracılığıyla) ve şu anda oturum açmış kullanıcının ayrıcalıkları olacaktır. Uygulamanızın ayrıcalıkları hiçbir zaman oturum açmış kullanıcının ayrıcalıklarından fazla olamaz. Kuruluşların içinde, oturum açmış kullanıcının ayrıcalıkları ilkeyle ya da bir veya birden çok yönetici rolü üyeliğiyle belirlenebilir. Hangi Yönetici rollerinin temsilci izinleri onaylamasına izin verebileceğini öğrenmek için bkz. [Azure AD 'de yönetici rolü izinleri](../users-groups-roles/directory-assign-admin-roles.md).

   Örneğin, uygulamanıza _User. ReadWrite. All_ temsilcisi izni verildiğini varsayalım. Adından da anlaşıldığı gibi bu izin uygulamanıza kuruluştaki her kullanıcının profilini okuma ve güncelleştirme izni verir. Oturum açmış kullanıcının bir genel yönetici olması durumunda, uygulamanız kuruluştaki her kullanıcının profilini güncelleştirebilir. Ancak, oturum açmış kullanıcı yönetici rolünde değilse, uygulamanız yalnızca oturum açan kullanıcının profilini güncelleştirebilir. Kuruluştaki diğer kullanıcıların profillerini güncelleştiremez çünkü adına çalışma iznine sahip olduğu kullanıcı söz konusu ayrıcalıklara sahip değildir.

- Uygulama izinleri için uygulamanızın _etkili izinleri_ , izin tarafından ima edilen ayrıcalıkların tam düzeyi olacaktır. Örneğin, _User. ReadWrite_ olan bir uygulama. tüm uygulama izinleri, kuruluştaki her kullanıcının profilini güncelleştirebilir.

## <a name="openid-connect-scopes"></a>OpenID Connect kapsamları

OpenID Connect 'in Microsoft Identity platform uygulamasının Microsoft Graph,,, ve ' de barındırıldığı bazı iyi tanımlanmış kapsamları vardır:,, `openid` `email` `profile` ve `offline_access` . `address`Ve `phone` OpenID Connect kapsamları desteklenmez.

OıDC kapsamlarını ve bir belirteci istemek, [UserInfo uç noktasını](userinfo.md)çağırmak için size bir belirteç verir.

### <a name="openid"></a>OpenID

Bir uygulama, [OpenID Connect](active-directory-v2-protocols.md)kullanarak oturum açma işlemini gerçekleştiriyorsa, kapsam istemesi gerekir `openid` . `openid`Kapsam, çalışma hesabı onay sayfasında "oturumunuzu aç" izninin yanı sıra kişisel Microsoft hesabı onayı sayfasında "profilinizi görüntüleme ve Microsoft hesabı kullanarak uygulamalara ve hizmetlere bağlanma" iznini gösterir. Bu izinle, bir uygulama, talep biçiminde kullanıcı için benzersiz bir tanımlayıcı alabilir `sub` . Ayrıca uygulama, UserInfo uç noktasına erişim sağlar. `openid`Kapsam, uygulama tarafından kimlik doğrulaması için KULLANıLABILECEK kimlik belirteçleri almak Için Microsoft Identity platform belirteci uç noktasında kullanılabilir.

### <a name="email"></a>e-posta

`email`Kapsam, `openid` kapsam ve diğer kullanıcılarla birlikte kullanılabilir. Uygulama, kullanıcının birincil e-posta adresine talep biçiminde erişim sağlar `email` . `email`Talep, yalnızca bir e-posta adresi kullanıcı hesabıyla ilişkiliyse, her zaman durum olmayan bir belirtece dahil edilir. `email`Kapsamı kullanıyorsa, uygulamanız, talebin belirteçte olmadığı bir servis talebini işleyecek şekilde hazırlanmalıdır `email` .

### <a name="profile"></a>profil

`profile`Kapsam, `openid` kapsam ve diğer kullanıcılarla birlikte kullanılabilir. Uygulamanın kullanıcı hakkındaki önemli miktarda bilgiye erişmesini sağlar. Erişebileceği bilgiler, kullanıcının verilen adı, soyadı, tercih edilen Kullanıcı adı ve nesne KIMLIĞINI içerir, ancak bunlarla sınırlı değildir. Belirli bir kullanıcı için id_tokens parametresinde kullanılabilir olan profil taleplerinin tüm listesi için, [ `id_tokens` başvuruya](id-tokens.md)bakın.

### <a name="offline_access"></a>offline_access

[ `offline_access` Kapsam](https://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) , uygulamanızın kullanıcı adına uzun bir süre boyunca kaynaklara erişmesini sağlar. Onay sayfasında, bu kapsam "erişim vermiş olduğunuz verilere erişimi koru" izni olarak görünür. Kullanıcı kapsamı onayladığında `offline_access` , uygulamanız Microsoft Identity platform belirteci uç noktasından yenileme belirteçleri alabilir. Yenileme belirteçleri uzun süreli. Uygulamanız, eski kullanım süreleri dolana kadar yeni erişim belirteçleri alabilir.

> [!NOTE]
> Bu izin, yenileme belirteci sağlamayan akışlar ( [örtük akış](v2-oauth2-implicit-grant-flow.md)) için bile, bugün tüm onay ekranlarında görünür.  Bu, bir istemcinin örtük akış içinde başlayabileceği senaryoları kapsar ve sonra bir yenileme belirtecinin beklendiği kod akışına geçebilir.

Microsoft Identity platformunda (v 2.0 uç noktasında yapılan istekler), `offline_access` yenileme belirteçleri almak için uygulamanızın kapsamı açıkça istemesi gerekir. Bu, [OAuth 2,0 yetkilendirme kodu akışında](active-directory-v2-protocols.md)bir yetkilendirme kodu kullandığınızda uç noktadan yalnızca bir erişim belirteci alacağınız anlamına gelir `/token` . Erişim belirteci kısa bir süre için geçerlidir. Erişim belirtecinin genellikle bir saat içinde süresi dolar. Bu noktada, uygulamanızın `/authorize` Yeni bir yetkilendirme kodu almak için kullanıcıyı uç noktaya yeniden yönlendirmesi gerekir. Bu yeniden yönlendirme sırasında, uygulamanın türüne bağlı olarak, kullanıcının kimlik bilgilerini yeniden girmesi veya izinleri yeniden onaylaması gerekebilir.

Yenileme belirteçleri alma ve kullanma hakkında daha fazla bilgi için bkz. [Microsoft Identity platform protokol başvurusu](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Bireysel kullanıcı izni isteme

Bir [OpenID Connect veya OAuth 2,0](active-directory-v2-protocols.md) yetkilendirme isteğinde bir uygulama, sorgu parametresini kullanarak ihtiyaç duyacağı izinleri isteyebilir `scope` . Örneğin, bir Kullanıcı bir uygulamada oturum açtığında, uygulama aşağıdaki örnek gibi bir istek gönderir (okunabilirliği sağlamak için satır sonları eklenmiştir):

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

`scope`Parametresi, uygulamanın istediği atanan izinlerin boşlukla ayrılmış bir listesidir. Her izin, izin değeri kaynağın tanımlayıcısına eklenerek belirtilir (uygulama KIMLIĞI URI 'SI). İstek örneğinde, uygulamanın kullanıcının takvimini okumak ve Kullanıcı olarak e-posta göndermek için izin gerekir.

Kullanıcı kimlik bilgilerini girdikten sonra, Microsoft Identity platform Endpoint, eşleşen bir *Kullanıcı onayı*kaydı olup olmadığını denetler. Kullanıcı geçmişte istenen izinlerden herhangi birine onay vermezse veya bir yöneticiye tüm kuruluş adına bu izinlere onay verildiyse, Microsoft Identity platform uç noktası kullanıcıdan istenen izinleri vermesini ister.

> [!NOTE]
>Şu anda, `offline_access` ("erişim vermiş olduğunuz verilere erişimi korur") ve `user.read` ("profilinizi oturum açma ve profilinizi okuma") izinleri, bir uygulamaya ilk onaylamada otomatik olarak eklenir.  Bu izinler genellikle uygun uygulama işlevleri için gereklidir `offline_access` . uygulama, yerel ve Web uygulamaları için kritik olan belirteçleri yenileme, isteğe erişim izni verirken `user.read` `sub` istemcinin veya uygulamanın zaman içinde kullanıcıyı doğru şekilde tanımlamasına ve ilkel Kullanıcı bilgilerine erişmesine izin verir.

![İş hesabı onayını gösteren örnek ekran görüntüsü](./media/v2-permissions-and-consent/work_account_consent.png)

Kullanıcı izin isteğini onayladığında, izin kaydedilir ve kullanıcının sonraki oturum açma işlemlerini uygulamaya yeniden onaylaması gerekmez.

## <a name="requesting-consent-for-an-entire-tenant"></a>Tüm kiracı için izin isteme

Genellikle, bir kuruluş bir uygulama için bir lisans veya abonelik satın aldığında, kuruluşun uygulamayı kuruluşun tüm üyeleri tarafından kullanılmak üzere önceden ayarlamak ister. Bu işlemin bir parçası olarak, bir yönetici, uygulamanın Kiracıdaki herhangi bir kullanıcı adına işlem yapması için izin verebilir. Yönetici Kiracı genelinde onay veriyorsa, kuruluşun kullanıcıları uygulama için bir onay sayfası görmez.

Bir Kiracıdaki tüm kullanıcılar için temsilci izinleri için izin istemek üzere, uygulamanız yönetici onay uç noktasını kullanabilir.

Ayrıca, uygulamalar uygulama Izinleri istemek için yönetici onay uç noktasını kullanmalıdır.

## <a name="admin-restricted-permissions"></a>Yönetici kısıtlı izinler

Microsoft ekosistemindeki bazı yüksek ayrıcalıklı izinler, *Yönetici kısıtlı*olarak ayarlanabilir. Bu tür izinlere örnek olarak şunlar verilebilir:

* Kullanarak tüm kullanıcıların tam profillerini okuyun`User.Read.All`
* Kullanarak bir kuruluşun dizinine veri yazma`Directory.ReadWrite.All`
* Kullanarak bir kuruluşun dizinindeki tüm grupları okuma`Groups.Read.All`

Bir tüketici kullanıcısı bu tür verilere erişim izni verebilir, ancak kuruluş kullanıcılarının aynı hassas şirket verileri kümesine erişim izni verilmesi kısıtlıdır. Uygulamanız bir kuruluş kullanıcısının bu izinlerinden birine erişim isterse, kullanıcı uygulamanızın izinlerini kabul etmek için yetkilendirilmediğini bildiren bir hata iletisi alır.

Uygulamanız kuruluşlar için yönetici tarafından kısıtlanmış kapsamlara erişim gerektiriyorsa, daha sonra açıklanan yönetici onay uç noktasını kullanarak, bunları doğrudan bir şirket yöneticisinden istemeniz gerekir.

Uygulama yüksek ayrıcalıklı temsilci izinleri istiyorsa ve bir yönetici bu izinleri yönetici onay uç noktası üzerinden veriyorsa, Kiracıdaki tüm kullanıcılar için izin verilir.

Uygulama uygulama izinleri istiyorsa ve yönetici bu izinleri yönetici onay uç noktası aracılığıyla veriyorsa, bu izin belirli bir kullanıcı adına yapılmaz. Bunun yerine, istemci uygulamasına *doğrudan*izinler verilir. Bu tür izinler yalnızca Daemon Hizmetleri ve arka planda çalışan diğer etkileşimli olmayan uygulamalar tarafından kullanılır.

## <a name="using-the-admin-consent-endpoint"></a>Yönetici onay uç noktasını kullanma

> [!NOTE]
> Yönetici onay uç noktası kullanılarak yönetici onayı verdikten sonra, yönetici onayı verilmesinin tamamlandığını ve kullanıcıların daha fazla ek eylem gerçekleştirmesine gerek olmadığını unutmayın. Yönetici onayı verildikten sonra, kullanıcılar tipik bir kimlik doğrulama akışı aracılığıyla erişim belirteci alabilir ve elde edilen erişim belirteci, onaylanan izinlere sahip olur.

Bir şirket yöneticisi uygulamanızı kullandığında ve yetkilendirme uç noktasına yönlendirirse, Microsoft Identity platform kullanıcının rolünü algılar ve istediğiniz izinler için tüm kiracının adına onay vermek isteyip istemediğini sorar. Ancak, bir yöneticinin tüm kiracı adına izin vermesini sağlamak istiyorsanız kullanabileceğiniz bir adanmış yönetici onay uç noktası da vardır. Bu uç noktanın kullanılması, uygulama Izinleri istemek için de gereklidir (yetkilendirme uç noktası kullanılarak istenemez).

Bu adımları izlerseniz, uygulamanız, yönetici tarafından kısıtlanmış kapsamlar dahil olmak üzere bir Kiracıdaki tüm kullanıcılar için izinler isteyebilir. Bu yüksek ayrıcalıklı bir işlemdir ve yalnızca senaryonuz için gerekliyse gerçekleştirilmelidir.

Adımları uygulayan bir kod örneğini görmek için [yönetici tarafından kısıtlanmış kapsamlar örneğine](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2)bakın.

### <a name="request-the-permissions-in-the-app-registration-portal"></a>Uygulama kayıt portalında izinleri isteyin

Uygulamalar, uygulama kayıt portalı 'nda hangi izinleri (hem temsilci olarak hem de uygulama) gerektirdiğini hatırlatamaz.  Bu, `/.default` kapsamın ve Azure Portal "yönetici onayı verme" seçeneğinin kullanılmasına izin verir.  Genel olarak, belirli bir uygulama için statik olarak tanımlanan izinlerin, dinamik/artımlı olarak istediği izinlerin bir üst kümesi olduğundan emin olmak en iyi uygulamadır.

> [!NOTE]
>Uygulama izinleri yalnızca kullanılarak istenebilir. bu nedenle, uygulamanız [`/.default`](#the-default-scope) uygulama izinlerine ihtiyaç duyuyorsa, bunların uygulama kayıt portalı 'nda listelendiğinden emin olun.

#### <a name="to-configure-the-list-of-statically-requested-permissions-for-an-application"></a>Bir uygulama için statik olarak istenen izinlerin listesini yapılandırmak için

1. [Azure Portal – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminde uygulamanıza gidin veya henüz yapmadıysanız [bir uygulama oluşturun](quickstart-register-app.md) .
2. **API izinleri** bölümünü bulun ve API izinleri Içinde izin Ekle ' ye tıklayın.
3. Kullanılabilir API 'Ler listesinden **Microsoft Graph** ' yi seçin ve ardından uygulamanızın gerektirdiği izinleri ekleyin.
3. Uygulama kaydını **kaydedin** .

### <a name="recommended-sign-the-user-into-your-app"></a>Önerilir: Kullanıcı uygulamanızda Imzalanın

Genellikle, yönetici onay uç noktasını kullanan bir uygulama oluşturduğunuzda, uygulamanın, yöneticinin uygulamanın izinlerini onaylayabileceği bir sayfa veya görünüm gerekir. Bu sayfa, uygulamanın kaydolma akışının bir parçası, uygulamanın ayarlarının bir parçası olabilir veya adanmış bir "Connect" akışı olabilir. Çoğu durumda, uygulamanın bu "Bağlan" görünümünü yalnızca bir kullanıcı iş veya okul Microsoft hesabı oturum açtıktan sonra göstermesini mantıklı hale getirir.

Kullanıcıyı uygulamanıza imzaladığınızda, yöneticinin gerekli izinleri onaylamasını istemeden önce sahip olduğu kuruluşu belirleyebilirsiniz. Kesinlikle gerekli olmasa da, kurumsal kullanıcılarınız için daha sezgisel bir deneyim oluşturmanıza yardımcı olabilir. Kullanıcı oturumu açmak için [Microsoft Identity platform protokol öğreticilerimizi](active-directory-v2-protocols.md)izleyin.

### <a name="request-the-permissions-from-a-directory-admin"></a>Dizin yöneticisinden izinleri isteme

Kuruluşunuzun yöneticisinden izin istemek için hazırsanız, kullanıcıyı Microsoft Identity Platform *Yöneticisi onay uç noktasına*yönlendirebilirsiniz.

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
| `tenant` | Gerekli | İzin istemek istediğiniz dizin kiracısı. , Örnekte görüldüğü gibi organizasyonlarla birlikte GUID veya kolay ad biçiminde veya genel olarak başvuru yapılabilir. Kişisel hesaplar kiracı bağlamı haricinde yönetici onayı sağlayamadığından ' Common ' kullanmayın. Kiracıların yönetiminde kişisel hesaplarla en iyi uyumluluğu sağlamak için, mümkün olduğunda kiracı KIMLIĞINI kullanın. |
| `client_id` | Gerekli | [Azure Portal – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminin uygulamanıza atandığı **uygulama (istemci) kimliği** . |
| `redirect_uri` | Gerekli |Uygulamanızın işlenmesi için yanıtın gönderilmesini istediğiniz yeniden yönlendirme URI 'SI. Uygulama kayıt portalı 'nda kaydettiğiniz yeniden yönlendirme URI 'lerinden biriyle tam olarak eşleşmesi gerekir. |
| `state` | Önerilen | İsteğin belirteç yanıtında de döndürülecek bir değer. İstediğiniz herhangi bir içerik dizesi olabilir. Kullanıcının uygulamadaki durumuyla ilgili bilgileri, uygulamanın bulunduğu sayfa veya görünüm gibi kimlik doğrulama isteği olmadan önce kodlamak için bu durumu kullanın. |
|`scope`        | Gerekli        | Uygulama tarafından istenen izin kümesini tanımlar. Bu statik (kullanılarak [`/.default`](#the-default-scope) ) ya da dinamik kapsamlar olabilir.  Bu, OıDC kapsamlarını ( `openid` , `profile` ,) içerebilir `email` . Uygulama izinlerine ihtiyacınız varsa, `/.default` statik olarak yapılandırılmış izin listesini istemek için kullanmanız gerekir.  |


Bu noktada, Azure AD 'nin isteği tamamlaması için bir kiracı yöneticisinin oturum açması gerekir. Yöneticinin, parametresinde istediğiniz tüm izinleri onaylaması istenir `scope` .  Statik ( `/.default` ) değeri kullandıysanız, bu, uygulama için gerekli izinlerde bulunan tüm kapsamlar için v 1.0 Yönetici onay uç noktası ve istek onayı gibi çalışır.

#### <a name="successful-response"></a>Başarılı yanıt

Yönetici, uygulamanız için izinleri onayladığında, başarılı yanıt şöyle görünür:

```HTTP
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametre | Açıklama |
| --- | --- |
| `tenant` | Uygulamanıza istenen izinleri (GUID biçiminde) veren dizin kiracısı. |
| `state` | İstekte bulunan ve belirteç yanıtında de döndürülen bir değer. İstediğiniz herhangi bir içerik dizesi olabilir. Durum, kullanıcının uygulamadaki durumu hakkında bilgi kodlamak için kullanılır; Örneğin, bulunan sayfa veya görünüm gibi kimlik doğrulama isteği gerçekleştirilmeden önce. |
| `admin_consent` | , Olarak ayarlanır `True` . |

#### <a name="error-response"></a>Hata yanıtı

Yönetici, uygulamanız için izinleri onaylamadıysanız, başarısız yanıt şöyle görünür:

```HTTP
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametre | Açıklama |
| --- | --- |
| `error` | Oluşan hata türlerini sınıflandırmak için kullanılabilen ve hatalara yanıt vermek için kullanılabilen bir hata kodu dizesi. |
| `error_description` | Bir geliştiricinin hatanın kök nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

Yönetici onay uç noktasından başarılı bir yanıt aldıktan sonra, uygulamanız istediği izinleri kazandı. Ardından, istediğiniz kaynak için bir belirteç isteyebilirsiniz.

## <a name="using-permissions"></a>İzinleri kullanma

Kullanıcı uygulamanıza yönelik izinlere geçtikten sonra, uygulamanız, uygulamanızın bazı kapasitede bir kaynağa erişme iznini temsil eden erişim belirteçleri alabilir. Erişim belirteci yalnızca tek bir kaynak için kullanılabilir, ancak erişim belirtecinin içinde kodlanan, uygulamanızın bu kaynak için verilen her izindir. Uygulamanız, bir erişim belirteci almak için Microsoft Identity platform belirteç uç noktasına aşağıdaki gibi bir istek yapabilir:

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

HTTP isteklerinde elde edilen erişim belirtecini kaynağa gönderebilirsiniz. Bu, uygulamanızın belirli bir görevi gerçekleştirmek için doğru izinlere sahip olduğunu güvenilir bir şekilde gösterir.

OAuth 2,0 protokolü ve erişim belirteçleri alma hakkında daha fazla bilgi için bkz. [Microsoft Identity platform uç nokta protokol başvurusu](active-directory-v2-protocols.md).

## <a name="the-default-scope"></a>/.Exe varsayılan kapsamı

`/.default`Uygulamalarınızı v 1.0 uç noktasından Microsoft Identity platform uç noktasına geçirmeye yardımcı olması için kapsamı kullanabilirsiniz. Bu, uygulama kaydında yapılandırılan izinlerin statik listesine başvuran her uygulama için yerleşik bir kapsamdır. Bir `scope` değeri,, `https://graph.microsoft.com/.default` v 1.0 uç noktalarıyla aynı işleve sahiptir; `resource=https://graph.microsoft.com` Yani, Azure Portal uygulamanın kaydolmadığından Microsoft Graph kapsamları ile bir belirteç ister.  Kaynak URI + kullanılarak oluşturulur `/.default` (örneğin, kaynak URI 'si ise `https://contosoApp.com` , istenen kapsam olacaktır `https://contosoApp.com/.default` ).  Belirteci doğru bir şekilde istemek için ikinci bir eğik çizgi eklemeniz gereken durumlarda, [sonundaki eğik çizgilerin bölümüne](#trailing-slash-and-default) bakın.

/.Default kapsamı herhangi bir OAuth 2,0 akışında kullanılabilir, ancak uygulama izinleri istemek için v2 yönetici onay uç noktası kullanılırken ve [Şirket adına](v2-oauth2-on-behalf-of-flow.md) , [istemci kimlik bilgileri akışında](v2-oauth2-client-creds-grant-flow.md)gereklidir.

> [!NOTE]
> İstemciler statik ( `/.default` ) ve dinamik onayı tek bir istekte birleştiremez. Bu nedenle, `scope=https://graph.microsoft.com/.default+mail.read` kapsam türlerinin birleşimi nedeniyle hata oluşur.

### <a name="default-and-consent"></a>/.exe varsayılan ve onay

`/.default`Kapsam, v 1.0 uç nokta davranışını de tetikler `prompt=consent` . Kaynak ne olursa olsun, uygulama tarafından kaydedilen tüm izinler için onay ister. İsteğin bir parçası olarak dahil edildiyse kapsam, `/.default` istenen kaynak için Kapsamları içeren bir belirteç döndürür.

### <a name="default-when-the-user-has-already-given-consent"></a>/.exe Kullanıcı zaten izin vermiş olduğunda varsayılan

, `/.default` `resource` -Merkezli v 1.0 uç noktasının davranışına özdeş olduğundan, bu, v 1.0 uç noktasının izin davranışını de beraberinde getirir. Yani, `/.default` yalnızca istemci ile kaynak arasında izin verilmediği takdirde bir onay istemi tetikler. Böyle bir onay varsa, bu kaynak için Kullanıcı tarafından verilen tüm kapsamları içeren bir belirteç döndürülür. Ancak, izin verilmezse veya `prompt=consent` parametresi sağlanmışsa, istemci uygulaması tarafından kaydedilen tüm kapsamlar için bir onay istemi gösterilir.

#### <a name="example-1-the-user-or-tenant-admin-has-granted-permissions"></a>Örnek 1: Kullanıcı veya kiracı yöneticisi izin verdi

Bu örnekte, Kullanıcı (veya bir kiracı yöneticisi) istemciye Microsoft Graph izinlerini `mail.read` ve ' ı verdi `user.read` . İstemci için bir istek yapıyorsa `scope=https://graph.microsoft.com/.default` , istemci uygulamalarının içerikleri Microsoft Graph için kayıtlı izinlere bakılmaksızın hiçbir onay istemi gösterilmez. Kapsamları ve içeren bir belirteç döndürülür `mail.read` `user.read` .

#### <a name="example-2-the-user-hasnt-granted-permissions-between-the-client-and-the-resource"></a>Örnek 2: kullanıcıya istemci ve kaynak arasında izin verilmemiş

Bu örnekte, istemci ve Microsoft Graph arasında Kullanıcı için hiçbir onay yok. İstemci, `user.read` ve `contacts.read` izinlerinin yanı sıra Azure Key Vault kapsamına da kaydoldu `https://vault.azure.net/user_impersonation` . İstemci için bir belirteç istediğinde, Kullanıcı,, `scope=https://graph.microsoft.com/.default` `user.read` `contacts.read` ve Key Vault kapsamları için bir onay ekranı görür `user_impersonation` . Döndürülen belirteç yalnızca `user.read` `contacts.read` içindeki ve kapsamlarına sahip olacak ve yalnızca Microsoft Graph için kullanılabilir.

#### <a name="example-3-the-user-has-consented-and-the-client-requests-additional-scopes"></a>Örnek 3: Kullanıcı onaylı ve istemci ek kapsamlar istediğinde

Bu örnekte, Kullanıcı istemcisi için zaten bir daha onayladı `mail.read` . İstemci, kaydında kapsama kaydoldu `contacts.read` . İstemci bir belirteci kullanarak bir belirteç isteği yaptığında `scope=https://graph.microsoft.com/.default` ve aracılığıyla onay istediğinde `prompt=consent` , Kullanıcı, uygulama tarafından kaydedilen izinlerin tümü (ve yalnızca) için bir onay ekranı görür. `contacts.read`onay ekranında mevcut olacaktır, ancak olmayacaktır `mail.read` . Döndürülen belirteç Microsoft Graph olur ve `mail.read` ve içerir `contacts.read` .

### <a name="using-the-default-scope-with-the-client"></a>İstemcisiyle//varsayılan kapsamını kullanma

`/.default`Bir istemcinin kendi kapsamını istemesi durumunda kapsamın özel bir durumu vardır `/.default` . Aşağıdaki örnekte bu senaryo gösterilmektedir.

```HTTP
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
response_type=token            //code or a hybrid flow is also possible here
&client_id=9ada6f8a-6d83-41bc-b169-a306c21527a5
&scope=9ada6f8a-6d83-41bc-b169-a306c21527a5/.default
&redirect_uri=https%3A%2F%2Flocalhost
&state=1234
```

Bu, tüm kayıtlı izinler için bir onay ekranı üretir (yukarıdaki izin ve bu açıklamalara göre geçerliyse `/.default` ), erişim belirteci yerine bir id_token döndürür.  Bu davranış, ADAL 'den MSAL 'e taşınan bazı eski istemciler için ve Microsoft Identity platform uç noktasını hedefleyen yeni istemciler **tarafından kullanılmamalıdır.**

### <a name="trailing-slash-and-default"></a>Sondaki eğik çizgi ve//varsayılan

Bazı kaynak URI 'Lerinin sonunda eğik çizgi vardır ( `https://contoso.com/` aksine `https://contoso.com` ), bu, belirteç doğrulamasında sorun oluşmasına neden olabilir.  Bu `https://management.azure.com/` , öncelikle kaynak URI 'sinde bir eğik çizgiye sahip olan ve belirteç istendiğinde mevcut olmasını gerektiren Azure Kaynak Yönetimi () için bir belirteç istenirken meydana gelebilir.  Bu nedenle, ve kullanarak bir belirteç talep ettiğinizde `https://management.azure.com/` `/.default` , `https://management.azure.com//.default` çift eğik çizgi yazmanız gerekir!

Genel olarak, belirtecin verildiğini ve belirtecin kabul etmesi gereken API tarafından reddedildiği doğrulandıktan sonra, ikinci bir eğik çizgi ekleyip yeniden denemeyi düşünün. Bu durum, oturum açma sunucusunun,, parametresindeki URI 'Ler ile eşleşen hedef kitle ile bir belirteç gösterdiği ve `scope` `/.default` uçtan çıkarıldığı için oluşur.  Bu, sondaki eğik çizgiyi kaldırırsa, oturum açma sunucusu yine de isteği işler ve kaynak URI 'sine karşı doğrular, ancak bu standart değildir ve uygulamanız tarafından güvenilmemelidir.

## <a name="troubleshooting-permissions-and-consent"></a>İzinler ve onay sorunlarını giderme

Siz veya uygulamanızın kullanıcıları onay işlemi sırasında beklenmeyen hatalar görüyor ise, sorun giderme adımları için şu makaleye bakın: [bir uygulamaya onay uygularken beklenmeyen hata](../manage-apps/application-sign-in-unexpected-user-consent-error.md).
