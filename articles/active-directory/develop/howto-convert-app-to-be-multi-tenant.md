---
title: Azure AD kullanıcılarında oturum açan uygulamalar oluşturun
titleSuffix: Microsoft identity platform
description: Herhangi bir Azure Etkin Dizin kiracısından bir kullanıcıda oturum açabilen çok kiracılı bir uygulamanın nasıl oluşturulabildiğini gösterir.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/17/2020
ms.author: ryanwi
ms.reviewer: jmprieur, lenalepa, sureshja, kkrishna
ms.custom: aaddev
ms.openlocfilehash: e15fb60ec339eae45f9f14a3333e8afe51fc05c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480870"
---
# <a name="how-to-sign-in-any-azure-active-directory-user-using-the-multi-tenant-application-pattern"></a>Nasıl yapilir: Çok kiracılı uygulama deseni kullanarak herhangi bir Azure Active Directory kullanıcısında oturum açın

Birçok kuruluşa Hizmet Olarak Yazılım (SaaS) uygulaması sunuyorsanız, uygulamanızı herhangi bir Azure Etkin Dizin (Azure AD) kiracısından oturum açma kabul etmek üzere yapılandırabilirsiniz. Bu yapılandırma, *uygulamanızı çok kiracı yapma*denir. Herhangi bir Azure AD kiracısındaki kullanıcılar, uygulamalarınız için hesaplarını kullanmayı kabul ettikten sonra uygulamanızda oturum açabilecektir.

Kendi hesap sistemine sahip varolan bir uygulamanız varsa veya diğer bulut sağlayıcılarından gelen diğer oturum açma türlerini destekliyorsa, herhangi bir kiracıdan Azure AD oturum açma eklemek kolaydır. Uygulamanızı kaydedin, OAuth2, OpenID Connect veya SAML üzerinden oturum açma kodu ekleyin ve uygulamanıza ["Microsoft ile oturum açın" düğmesini][AAD-App-Branding] koyun.

> [!NOTE]
> Bu makalede, Azure AD için tek bir kiracı uygulaması oluşturmaya zaten aşina olduğunuzu varsayar. Eğer değilseniz, [geliştirici kılavuzu ana sayfasında][AAD-Dev-Guide]quickstarts biri ile başlayın.

Uygulamanızı Azure AD çok kiracılı bir uygulamaya dönüştürmek için dört basit adım vardır:

1. [Çok kiracı lı olmak için başvuru kaydınızı güncelleştirin](#update-registration-to-be-multi-tenant)
2. [/ortak bitiş noktasına istek göndermek için kodunuzu güncelleştirin](#update-your-code-to-send-requests-to-common)
3. [Birden çok veren değerlerini işlemek için kodunuzu güncelleştirme](#update-your-code-to-handle-multiple-issuer-values)
4. [Kullanıcı ve yönetici nin onayonayLarını anlama ve uygun kod değişiklikleri yapma](#understand-user-and-admin-consent)

Her adıma ayrıntılı olarak bakalım. Ayrıca doğrudan örneğe atlayabilirsiniz [Azure AD ve OpenID Connect kullanarak Microsoft Graph çağıran çok kiracılı bir SaaS web uygulaması oluşturun.](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/2-WebApp-graph-user/2-3-Multi-Tenant/README.md)

## <a name="update-registration-to-be-multi-tenant"></a>Çok kiracı olacak kaydı güncelleştirme

Varsayılan olarak, Azure AD'deki web uygulaması/API kayıtları tek kiracıdır. [Azure portalındaki][AZURE-portal] uygulama kaydınızın **Kimlik Doğrulama** bölmesinde Desteklenen **hesap türlerini** bularak ve **herhangi bir kuruluş dizininde Hesaplara**ayarlayarak kayıt çok kiracılı yapabilirsiniz.

Bir uygulamanın birden çok kiracılı olarak yapIlemeden önce Azure AD, uygulamanın App ID URI'sinin genel olarak benzersiz olmasını gerektirir. Uygulama Kimliği URI'si, uygulamanın protokol iletileri içinde tanımlanması için kullanılan yollardan biridir. Tek kiracılı bir uygulamada Uygulama Kimliği URI'sinin kiracı içinde benzersiz olması yeterlidir. Azure AD'nin uygulamayı tüm kiracılar arasında bulabilmesi için çok kiracılı uygulamada bu değerin genel olarak benzersiz olması gerekir. Genel olarak benzersiz olma gereksinimi, Uygulama Kimliği URI'sinin Azure AD kiracısının doğrulanmış etki alanı ile eşleşen bir ana bilgisayar adına sahip olması şartıyla sağlanır.

Varsayılan olarak, Azure portalı üzerinden oluşturulan uygulamaların uygulama oluşturma da ayarlanmış genel olarak benzersiz bir App ID URI'si vardır, ancak bu değeri değiştirebilirsiniz. Örneğin, kiracınızın adı contoso.onmicrosoft.com ise geçerli bir App ID `https://contoso.onmicrosoft.com/myapp`URI olacaktır. Kiracınızın doğrulanmış bir etki `contoso.com`alanı varsa, geçerli bir `https://contoso.com/myapp`App ID URI de olacaktır. Uygulama Kimliği URI'si bu düzene uygun olmadığında uygulamayı çok kiracılı hale getirme işlemi başarısız olur.

> [!NOTE]
> Yerel istemci kayıtları nın yanı sıra [Microsoft kimlik platformu uygulamaları](./active-directory-appmodel-v2-overview.md) varsayılan olarak çok kiracıdır. Bu uygulama kayıtlarını çok kiracılı yapmak için herhangi bir işlem yapmanız gerekmez.

## <a name="update-your-code-to-send-requests-to-common"></a>/common'a istek göndermek için kodunuzu güncelleştirin

Tek bir kiracı uygulamasında, oturum açma istekleri kiracının oturum açma bitiş noktasına gönderilir. Örneğin, bitiş noktası contoso.onmicrosoft.com: `https://login.microsoftonline.com/contoso.onmicrosoft.com`. Kiracının bitiş noktasına gönderilen istekler, o kiracıdaki kullanıcılarda (veya misafirlerde) o kiracıdaki uygulamalarda oturum açabilir.

Çok kiracılı bir uygulamayla, uygulama kullanıcının hangi kiracıdan geldiğini önceden bilmez, bu nedenle istekleri kiracının bitiş noktasına gönderemezsiniz. Bunun yerine, istekler, tüm Azure AD kiracılarında çok katlı bir uç noktaya gönderilir:`https://login.microsoftonline.com/common`

Microsoft kimlik platformu /ortak bitiş noktası yla ilgili bir istek aldığında, kullanıcıyı imzalar ve sonuç olarak kullanıcının hangi kiracıdan olduğunu keşfeder. /ortak bitiş noktası, Azure AD tarafından desteklenen tüm kimlik doğrulama protokolleriyle çalışır: OpenID Connect, OAuth 2.0, SAML 2.0 ve WS-Federation.

Uygulamaya verilen oturum açma yanıtı daha sonra kullanıcıyı temsil eden bir belirteç içerir. Belirtecideki veren değeri, bir uygulamaya kullanıcının ne kadar kiracıdan olduğunu bildirir. /ortak bitiş noktasından bir yanıt döndüğünde, belirteçteki veren değeri kullanıcının kiracısına karşılık gelir.

> [!IMPORTANT]
> /common endpoint kiracı değildir ve bir veren değildir, sadece bir çoklayıcı. /common kullanırken, belirteçleri doğrulamak için uygulamanızdaki mantığın bunu dikkate almak için güncelleştirilmesi gerekir.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Birden çok veren değerlerini işlemek için kodunuzu güncelleştirme

Web uygulamaları ve web API'leri Microsoft kimlik platformundan belirteçleri alır ve doğrular.

> [!NOTE]
> Yerel istemci uygulamaları Microsoft kimlik platformundan belirteçleri ister ve alır, ancak bunları doğrulanır lar API'lere göndermek için bunu yaparlar. Yerel uygulamalar belirteçleri doğrulamaz ve bunları opak olarak ele almalıdır.

Bir uygulamanın Microsoft kimlik platformundan aldığı belirteçleri nasıl doğruladığını inceleyelim. Tek bir kiracı uygulaması normalde gibi bir bitiş noktası değeri alır:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

ve bir meta veri URL 'si oluşturmak için kullanır (bu durumda, OpenID Connect) gibi:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

belirteçleri doğrulamak için kullanılan iki kritik bilgi parçasını indirmek için: kiracının imzalama anahtarları ve veren değeri. Her Azure REKLAM kiracısı formun benzersiz bir veren değerine sahiptir:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

GUID değerinin kiracının kiracı kimliğinin yeniden ad güvenli sürümü olduğu yerde. Yukarıdaki meta veri bağlantısını `contoso.onmicrosoft.com`seçerseniz, belgede bu veren değerini görebilirsiniz.

Tek bir kiracı uygulaması bir belirteci doğruladığında, belirteci imzasını meta veri belgesindeki imza anahtarlarıyla karşılar. Bu sınama, belirteçteki veren değerinin meta veri belgesinde bulunanla eşleştiğinden emin olmasını sağlar.

/ortak bitiş noktası kiracıya karşılık geldiğinden ve veren olmadığından, /common için meta verilerdeki veren değerini incelediğinizde gerçek bir değer yerine şablonlanmış bir URL'si vardır:

    https://sts.windows.net/{tenantid}/

Bu nedenle, çok kiracılı bir uygulama belirteçleri yalnızca meta verilerdeki veren `issuer` değerini belirteçteki değerle eşleştirerek doğrulayamaz. Çok kiracılı bir uygulama, hangi veren değerlerinin geçerli olduğuna ve hangisinin verenin değerinin kiracı kimliği bölümüne dayanmayan bir karar alabilmek için mantık gerekir. 

Örneğin, çok kiracılı bir uygulama yalnızca hizmetiçin kaydolmuş belirli kiracıların oturum açmasına izin veriyorsa, `tid` kiracının abone listesinde olduğundan emin olmak için belirteci değerini veya belirtecideki talep değerini denetlemelidir. Çok kiracılı bir uygulama yalnızca kişilerle ilgileniyorsa ve kiracılara dayalı herhangi bir erişim kararı vermiyorsa, verenin değerini tamamen yok sayabilir.

Çok [kiracılı örneklerde,][AAD-Samples-MT]herhangi bir Azure AD kiracısının oturum açmasını sağlamak için veren doğrulaması devre dışı bırakılır.

## <a name="understand-user-and-admin-consent"></a>Kullanıcı ve yönetici onayLarını anlama

Bir kullanıcının Azure AD'deki bir uygulamada oturum açabilmesi için uygulamanın kullanıcının kiracısında temsil edilmesi gerekir. Bu, kuruluşun, kiracılarından kullanıcılar uygulamaya oturum açtığında benzersiz ilkeler uygulama gibi şeyler yapmasına olanak tanır. Tek bir kiracı uygulaması için bu kayıt basittir; uygulamayı [Azure portalına][AZURE-portal]kaydettiğinizde gerçekleşen uygulamadır.

Çok kiracılı bir uygulama için, uygulamanın ilk kaydı geliştirici tarafından kullanılan Azure AD kiracısında yaşar. Farklı bir kiracıdan bir kullanıcı uygulamayı ilk kez imzaladığında, Azure AD uygulama tarafından istenen izinleri kabul etmelerini ister. Onay verirlerse, kullanıcının kiracısında *hizmet ilkesi* adı verilen uygulamanın bir gösterimi oluşturulur ve oturum açma devam edebilir. Ayrıca, kullanıcının uygulamaya onayını kaydeden dizinde bir delegasyon oluşturulur. Uygulamanın Uygulama ve HizmetAna nesneleri ve bunların birbiriyle nasıl ilişkili olduğu hakkında ayrıntılı bilgi için [Bkz.][AAD-App-SP-Objects]

![Tek katmanlı uygulamaya onay gösterme][Consent-Single-Tier]

Bu onay deneyimi, uygulama tarafından istenen izinlerden etkilenir. Microsoft kimlik platformu, yalnızca uygulama ve temsilci olmak üzere iki tür izinleri destekler.

* Yetkiverilen izin, bir uygulamaya, kullanıcının yapabileceği şeylerin bir alt kümesi için oturum açmış bir kullanıcı olarak hareket etme olanağı sağlar. Örneğin, bir uygulamaya, kullanıcının takviminde imzalanan ı okuma izni verebilirsiniz.
* Yalnızca uygulama izni doğrudan uygulamanın kimliğine verilir. Örneğin, bir uygulamaya, uygulamada kimlerin oturum açmış olduğuna bakılmaksızın, kiracıdaki kullanıcıların listesini okuması için yalnızca uygulama izni verebilirsiniz.

Bazı izinler normal bir kullanıcı tarafından onaylanırken, diğerleri kiracı yöneticinin onayına ihtiyaç duyar. 

### <a name="admin-consent"></a>Yönetici onayı

Yalnızca uygulama izinleri her zaman kiracı yöneticinin onayGerektirir. Uygulamanız yalnızca uygulama izni isterse ve bir kullanıcı uygulamada oturum açmaya çalışırsa, kullanıcının izin veremeyeceklerini belirten bir hata iletisi görüntülenir.

Belirli yetkilendirilmiş izinler de kiracı yöneticinin onayGerektirir. Örneğin, oturum açmış kullanıcı olarak Azure AD'ye geri yazma özelliği, kiracı yöneticinin onayını gerektirir. Yalnızca uygulama izinleri gibi, sıradan bir kullanıcı yönetici onayı gerektiren bir yetkili izin isteyen bir uygulamada oturum açmaya çalışırsa, uygulamanız bir hata alır. İznin yönetici onayı gerektip gerektirmediği, kaynağı yayımlayan geliştirici tarafından belirlenir ve kaynak için belgelerde bulunabilir. [Microsoft Graph API'nin][MSFT-Graph-permission-scopes] izin belgeleri, hangi izinlerin yönetici onayı gerektirdiğini gösterir.

Uygulamanız yönetici onayı gerektiren izinler kullanıyorsa, yöneticinin eylemi başlatabileceği bir düğme veya bağlantı gibi bir hareket inmesi gerekir. Uygulamanızın bu eylem için gönderdiği istek, sorgu dizesi parametresini `prompt=admin_consent` de içeren olağan OAuth2/OpenID Connect yetkilendirme isteğidir. Yönetici nin rızası alındıktan ve müşterinin kiracısında servis ilkesi oluşturulduktan sonra, sonraki `prompt=admin_consent` oturum açma isteklerinin parametreye ihtiyacı olmaz. Yönetici istenen izinlerin kabul edilebilir olduğuna karar verdiğinden, kiracıdaki diğer kullanıcılardan bu noktadan sonra onay istenmez.

Kiracı yönetici, normal kullanıcıların uygulamalara onay verme yeteneğini devre dışı kılabilir. Bu özellik devre dışı bırakılırsa, uygulamanın kiracıda kullanılması için her zaman yönetici onayı gerekir. Son kullanıcı onayı devre dışı bırakılmış olarak uygulamanızı test etmek istiyorsanız, Yapılandırma anahtarını **Kurumsal uygulamalar**altında **[Kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/)** bölümünde [Azure portalında][AZURE-portal] bulabilirsiniz.

Parametre, `prompt=admin_consent` yönetici onayı gerektirmeyen izinleri isteyen uygulamalar tarafından da kullanılabilir. Bunun ne zaman kullanılacağına bir örnek, uygulamanın kiracı yöneticinin bir kez "kaydolduğu" bir deneyim gerektirmesi ve bu noktadan itibaren başka hiçbir kullanıcıdan izin istenmediğidir.

Bir uygulama yönetici onayı gerektiriyorsa ve `prompt=admin_consent` parametre gönderilmeden bir yönetici imzalarsa, yönetici uygulamayı başarıyla kabul ettiğinde **yalnızca kullanıcı hesabı için**geçerli olacaktır. Normal kullanıcılar yine de oturum açamaz veya uygulamaya onay veremeyecektir. Bu özellik, kiracı yöneticiye diğer kullanıcıların erişmesine izin vermeden önce uygulamanızı keşfetme olanağı vermek istiyorsanız yararlıdır.

> [!NOTE]
> Bazı uygulamalar, normal kullanıcıların başlangıçta onay verebilmesi bir deneyim ister ve daha sonra uygulama yöneticiyi içerebilir ve yönetici onayı gerektiren izinler isteyebilir. Bugün Azure AD'de v1.0 uygulama kaydı yla bunu yapmanın bir yolu yoktur; ancak, Microsoft kimlik platformu (v2.0) bitiş noktasını kullanmak, uygulamaların kayıt zamanı yerine çalışma zamanında izin istemesine izin verir ve bu da bu senaryoyu sağlar. Daha fazla bilgi için [Microsoft kimlik platformu bitiş noktasına][AAD-V2-Dev-Guide]bakın.

### <a name="consent-and-multi-tier-applications"></a>İzin ve çok katmanlı uygulamalar

Uygulamanızın her biri Azure AD'de kendi kaydıyla temsil edilen birden çok katmana sahip olabilir. Örneğin, web API çağıran yerel bir uygulama veya web API çağıran bir web uygulaması. Bu durumların her ikisinde de, istemci (yerel uygulama veya web uygulaması) kaynağı (web API) çağırmak için izin ister. Müşterinin bir müşterinin kiracısına başarıyla onay vermesi için, izin talep ettiği tüm kaynakların müşterinin kiracısında zaten bulunması gerekir. Bu durum karşılanmazsa, Azure AD önce kaynağın eklenmesi gereken bir hatayı döndürür.

#### <a name="multiple-tiers-in-a-single-tenant"></a>Tek bir kiracıda birden çok katman

Mantıksal uygulamanız iki veya daha fazla uygulama kaydından (örneğin ayrı bir istemci ve kaynak) oluşuyorsa, bu bir sorun olabilir. Kaynağı önce müşteri kiracısına nasıl alabilirsiniz? Azure AD, istemci ve kaynağın tek bir adımda onaylanmasına olanak sağlayarak bu durumu kapsar. Kullanıcı, onay sayfasında hem istemci hem de kaynak tarafından istenen izinlerin toplamını görür. Bu davranışı etkinleştirmek için, kaynağın uygulama kaydı, istemcinin `knownClientApplications` Uygulama Kimliğini [uygulama bildirimine][AAD-App-Manifest]dahil etmelidir. Örnek:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Bu, bu makalenin sonundaİlgili [içerik](#related-content) bölümünde web API çağıran çok katmanlı bir yerel istemci de gösterilmiştir. Aşağıdaki diyagram, tek bir kiracıda kayıtlı çok katmanlı bir uygulama için onay alanına genel bir bakış sağlar.

![Çok katmanlı bilinen istemci uygulamasına onay gösterme][Consent-Multi-Tier-Known-Client]

#### <a name="multiple-tiers-in-multiple-tenants"></a>Birden çok kiracıda birden çok katman

Benzer bir durum, bir uygulamanın farklı katmanları farklı kiracılara kaydedilirse de olur. Örneğin, Office 365 Exchange Online API'yi çağıran yerel bir istemci uygulaması oluşturma durumunu göz önünde bulundurun. Yerel uygulamayı geliştirmek ve daha sonra yerel uygulamanın müşterinin kiracısında çalışması için Exchange Online hizmet sorumlusunun bulunması gerekir. Bu durumda, geliştirici ve müşterinin, kiracılarında oluşturulacak hizmet sorumlusu için Exchange Online'ı satın alması gerekir.

Microsoft dışındaki bir kuruluş tarafından oluşturulmuş bir API ise, API geliştiricisinin müşterilerinin uygulamayı müşterilerinin kiracılarına onaylaması için bir yol sağlaması gerekir. Önerilen tasarım, üçüncü taraf geliştiricinin API'yi oluşturmasıiçindir ki, kaydolma uygulamak için bir web istemcisi olarak da çalışabilir. Bunu yapmak için:

1. API'nin çok kiracılı uygulama kaydı/kod gereksinimlerini uyguladığından emin olmak için önceki bölümleri izleyin.
2. API'nin kapsamlarını/rollerini ortaya çıkarmanın yanı sıra, kaydın "Oturum aç ve kullanıcı profilini oku" iznini içerdiğinden emin olun (varsayılan olarak sağlanır).
3. Web istemcisinde bir oturum açma/kaydolma sayfası uygulayın ve [yönetici onayı](#admin-consent) kılavuzunu izleyin.
4. Kullanıcı uygulamayı kabul ettikten sonra, hizmet müdürü ve onay temsilciliği bağlantıları kiracılarında oluşturulur ve yerel uygulama API için belirteçler alabilir.

Aşağıdaki diyagram, farklı kiracılara kayıtlı çok katmanlı bir uygulama için onay alanına genel bir bakış sağlar.

![Çok katmanlı çok partili uygulamaya onay gösterir][Consent-Multi-Tier-Multi-Party]

### <a name="revoking-consent"></a>İzin iptali

Kullanıcılar ve yöneticiler uygulamanızın onayını istediği zaman iptal edebilir:

* Kullanıcılar, tek tek uygulamalara [erişimi Erişim Paneli Uygulamaları][AAD-Access-Panel] listesinden kaldırarak iptal eder.
* Yöneticiler, [Azure portalının][AZURE-portal] [Kurumsal uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) bölümünü kullanarak uygulamalara erişimi kaldırır.

Bir yönetici kiracıdaki tüm kullanıcılar için bir uygulamayı kabul ederse, kullanıcılar erişimi tek tek iptal edemez. Yalnızca yönetici erişimi ve yalnızca tüm uygulama için iptal edebilir.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Çok kiracılı uygulamalar ve önbelleğe alma erişim belirteçleri

Çok kiracılı uygulamalar, Azure AD tarafından korunan API'leri aramak için erişim belirteçleri de alabilir. Çok kiracılı bir uygulama yla Active Directory Authentication Library'yi (ADAL) kullanırken sık karşılaşılan bir hata, başlangıçta /ortak kullanan bir kullanıcı için bir belirteç istemek, yanıt almak ve sonra aynı kullanıcı için aynı kullanıcı için bir sonraki belirteç istemektir/ ortak. Azure AD'den gelen yanıt, /yaygın değil, bir kiracıdan geldiği için, ADAL belirteci kiracıdan olarak önbelleğe almaz. Kullanıcı için bir erişim belirteci almak için sonraki çağrı / ortak önbellek girişini özlüyor ve kullanıcı yeniden oturum açması istenir. Önbelleği kaçırmamak için, kullanıcıda zaten imzalanmış bir sonraki çağrıların kiracının bitiş noktasına yapıldığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, herhangi bir Azure AD kiracısından bir kullanıcıda oturum açabilen bir uygulamanın nasıl oluşturülabileceğini öğrendiniz. Uygulamanız ve Azure AD arasında Tek Oturum Açma 'yı (SSO) etkinleştirdikten sonra, Office 365 gibi Microsoft kaynakları tarafından açığa çıkarılan API'lere erişmek için uygulamanızı güncelleyebilirsiniz. Bu, uygulamanızda, kullanıcılara profil resmi veya bir sonraki takvim randevusu gibi bağlamsal bilgileri gösterme gibi kişiselleştirilmiş bir deneyim sunmanıza olanak tanır. Exchange, SharePoint, OneDrive, OneNote ve daha fazlası gibi Azure AD ve Office 365 hizmetlerine API çağrıları yapma hakkında daha fazla bilgi edinmek için Microsoft Graph API'yi ziyaret [edin.][MSFT-Graph-overview]

## <a name="related-content"></a>İlgili içerik

* [Çok kiracılı uygulama örneği](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/2-WebApp-graph-user/2-3-Multi-Tenant/README.md)
* [Uygulamalar için markalama yönergeleri][AAD-App-Branding]
* [Uygulama nesneleri ve hizmet temel nesneleri][AAD-App-SP-Objects]
* [Uygulamaları Azure Active Directory ile tümleştirme][AAD-Integrating-Apps]
* [İzin Çerçevesine Genel Bakış][AAD-Consent-Overview]
* [Microsoft Graph API izin kapsamları][MSFT-Graph-permission-scopes]

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]:howto-add-branding-in-azure-ad-apps.md
[AAD-App-Manifest]:reference-azure-ad-app-manifest.md
[AAD-App-SP-Objects]:app-objects-and-service-principals.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Consent-Overview]:consent-framework.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AAD-Samples-MT]: https://docs.microsoft.com/samples/browse/?products=azure-active-directory
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-portal]: https://portal.azure.com
[MSFT-Graph-overview]: https://developer.microsoft.com/graph/docs/overview/overview
[MSFT-Graph-permission-scopes]: https://developer.microsoft.com/graph/docs/concepts/permissions_reference

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/howto-convert-app-to-be-multi-tenant/consent-flow-single-tier.svg
[Consent-Multi-Tier-Known-Client]: ./media/howto-convert-app-to-be-multi-tenant/consent-flow-multi-tier-known-clients.svg
[Consent-Multi-Tier-Multi-Party]: ./media/howto-convert-app-to-be-multi-tenant/consent-flow-multi-tier-multi-party.svg

<!--Reference style links -->
[AAD-App-Manifest]:reference-azure-ad-app-manifest.md
[AAD-App-SP-Objects]:app-objects-and-service-principals.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]:access-tokens.md
[AAD-V2-Dev-Guide]: v2-overview.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: https://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: https://openid.net/specs/openid-connect-core-1_0.html#IDToken
