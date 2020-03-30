---
title: Azure Active Directory Koşullu Erişim için geliştirici kılavuzu
description: Azure AD Koşullu Erişim ve Microsoft kimlik platformu için geliştirici kılavuzu ve senaryoları.
services: active-directory
keywords: ''
author: rwike77
manager: CelesteDG
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda
ms.date: 03/16/2020
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9d82840681450ec855cb35c8700da2a53b9dd6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481458"
---
# <a name="developer-guidance-for-azure-active-directory-conditional-access"></a>Azure Active Directory Koşullu Erişim için geliştirici kılavuzu

Azure Etkin Dizini'ndeki Koşullu Erişim özelliği (Azure AD), uygulamanızı korumak ve bir hizmeti korumak için kullanabileceğiniz birkaç yöntemden birini sunar. Koşullu Erişim, geliştiricilerin ve kurumsal müşterilerin hizmetleri çeşitli şekillerde korumalarını sağlar:

* Multi-factor authentication
* Yalnızca Intune kayıtlı aygıtların belirli hizmetlere erişmesine izin verme
* Kullanıcı konumlarını ve IP aralıklarını kısıtlama

Koşullu Erişim'in tüm özellikleri hakkında daha fazla bilgi için [Azure Etkin Dizini'nde Koşullu Erişim](../active-directory-conditional-access-azure-portal.md)bölümüne bakın.

Azure AD için uygulama oluşturan geliştiriciler için bu makalede, Koşullu Erişim'i nasıl kullanabileceğiniz gösterilmektedir ve koşullu erişim ilkeleri nin uygulanması için denetimi olmayan kaynaklara erişimin etkisi hakkında bilgi edinebilirsiniz. Makalede ayrıca Koşullu Erişim'in akış adına, web uygulamalarında, Microsoft Graph'a erişmeve API'leri aramadaki etkileri de inceleniyor.

[Tek](quickstart-register-app.md) ve [çok kiracılı](howto-convert-app-to-be-multi-tenant.md) uygulamalar ve [ortak kimlik doğrulama desenleri](authentication-scenarios.md) hakkında bilgi edinilir.

> [!NOTE]
> Bu özelliği kullanmak için Azure AD Premium P1 lisansı gerektirir. Gereksinimlerinize uygun lisansı bulmak için bkz. [Ücretsiz, Temel ve Premium sürümlerinin genel olarak sağlanan özelliklerini karşılaştırma](https://azure.microsoft.com/pricing/details/active-directory/).
> Microsoft [365 İşletme lisanslarına](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) sahip müşteriler de Koşullu Erişim özelliklerine erişebilir.

## <a name="how-does-conditional-access-impact-an-app"></a>Koşullu Erişim bir uygulamayı nasıl etkiler?

### <a name="app-types-impacted"></a>Etkilenen uygulama türleri

Çoğu durumda, Koşullu Erişim bir uygulamanın davranışını değiştirmez veya geliştiriciden herhangi bir değişiklik gerektirir.Yalnızca belirli durumlarda, bir uygulama dolaylı veya sessizce bir hizmet için belirteç istediğinde, bir uygulama Koşullu Erişim "zorlukları" işlemek için kod değişiklikleri gerektirir.Etkileşimli oturum açma isteği gerçekleştirmek kadar basit olabilir.

Özellikle, aşağıdaki senaryolar Koşullu Erişim "zorlukları" işlemek için kod gerektirir:

* Akış adına gerçekleştiren uygulamalar
* Birden çok hizmete/kaynağa erişen uygulamalar
* MSAL.js kullanan tek sayfalı uygulamalar
* Kaynak çağıran Web Apps

Koşullu Erişim ilkeleri uygulamaya uygulanabilir, ancak uygulamanızın erişebildiği bir web API'sine de uygulanabilir. Koşullu Erişim ilkesini nasıl yapılandırılabilenler hakkında daha fazla bilgi edinmek [için Azure Active Directory Koşullu Erişim'e sahip belirli uygulamalar için Hızlı Başlangıç: MFA'yı zorunlu kılmasını gerektirmeye](../conditional-access/app-based-mfa.md)bakın.

Senaryoya bağlı olarak, kurumsal bir müşteri koşullu erişim ilkelerini istediği zaman uygulayabilir ve kaldırabilir. Uygulamanızın yeni bir ilke uygulandığında çalışmaya devam edebilmesi için "meydan okuma" işlemesini uygulamanız gerekir. Aşağıdaki örnekler, meydan okuma işleme yi göstermektedir.

### <a name="conditional-access-examples"></a>Koşullu Erişim örnekleri

Bazı senaryolar Koşullu Erişim işlemek için kod değişiklikleri gerektirirken, diğerleri olduğu gibi çalışır. Burada fark bazı fikir verir çok faktörlü kimlik doğrulama yapmak için Koşullu Erişim kullanarak birkaç senaryo vardır.

* Tek kiracılı bir iOS uygulaması oluşturuyorsunuz ve Koşullu Erişim ilkesi uyguluyorsunuz. Uygulama bir kullanıcıda işaretler ve bir API'ye erişim istemiyor. Kullanıcı kaydolduğunda, ilke otomatik olarak çağrılır ve kullanıcının çok faktörlü kimlik doğrulaması (MFA) gerçekleştirmesi gerekir. 
* Akış aşağı API'sine erişmek için orta katman hizmeti kullanan yerel bir uygulama oluşturuyorsunuz. Bu uygulamayı kullanan şirketteki bir kurumsal müşteri, akış aşağı API'sine bir ilke uygular. Son kullanıcı giriş yaptığında, yerel uygulama orta katmana erişim ister ve belirteci gönderir. Orta katman, akış aşağı API'sine erişim istemek için akış adına gerçekleştirir. Bu noktada, bir iddia "meydan okuma" orta katmana sunulur. Orta katman, zorluğu Koşullu Erişim ilkesine uyması gereken yerel uygulamaya geri gönderir.

#### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph'ın Koşullu Erişim ortamlarında uygulama yaparken özel hususları vardır. Genellikle, Koşullu Erişim mekaniği aynı şekilde olur, ancak kullanıcılarınızın gördüğü ilkeler uygulamanızın grafikten istediği temel verilere dayalı olacaktır. 

Özellikle, tüm Microsoft Graph kapsamları tek tek uygulanan ilkeler olabilir bazı veri kümesi temsil eder. Koşullu Erişim ilkelerine belirli veri kümeleri atandığından, Azure AD, Grafiğin kendisinden ziyade Grafik'in arkasındaki verilere dayalı Koşullu Erişim ilkelerini uygular.

Örneğin, bir uygulama aşağıdaki Microsoft Graph kapsamlarını isterse,

```
scopes="Bookings.Read.All Mail.Read"
```

Bir uygulama, kullanıcılarının Rezervasyon ve Değişim ile ilgili tüm politikaları yerine getirmesini bekleyebilir. Erişim izni veriyorsa, bazı kapsamlar birden çok veri kümesiyle eşlenebilir. 

### <a name="complying-with-a-conditional-access-policy"></a>Koşullu Erişim ilkesine uyma

Birkaç farklı uygulama topolojisi için, oturum oluşturulduğunda Koşullu Erişim ilkesi değerlendirilir. Koşullu Erişim ilkesi uygulamaların ve hizmetlerin parçalı özelliklerine göre çalıştığından, çağrıldığı nokta büyük ölçüde gerçekleştirmeye çalıştığınız senaryoya bağlıdır.

Uygulamanız Koşullu Erişim ilkesine sahip bir hizmete erişmeye çalıştığında, Koşullu Erişim sorunuyla karşılaşabilir. Bu zorluk, Azure AD'den gelen bir yanıtta gelen `claims` parametrede kodlanır. Bu meydan okuma parametresinin bir örneği aşağıda verilmiştir: 

```
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Geliştiriciler bu zorluğu alabilir ve Azure AD'ye yeni bir istek ekleyebilir. Bu durumun geçirilmesi, son kullanıcıdan Koşullu Erişim ilkesine uymak için gerekli her türlü eylemi gerçekleştirmesini ister. Aşağıdaki senaryolarda, hatanın ayrıntıları ve parametrenin nasıl ayıklanı açıklanır.

## <a name="scenarios"></a>Senaryolar

### <a name="prerequisites"></a>Ön koşullar

Azure AD Koşullu Erişim, [Azure AD Premium'da](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)yer alan bir özelliktir. Microsoft [365 İşletme lisanslarına](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) sahip müşteriler de Koşullu Erişim özelliklerine erişebilir.

### <a name="considerations-for-specific-scenarios"></a>Belirli senaryolar için dikkat edilmesi gerekenler

Aşağıdaki bilgiler yalnızca bu Koşullu Erişim senaryolarında geçerlidir:

* Akış adına gerçekleştiren uygulamalar
* Birden çok hizmete/kaynağa erişen uygulamalar
* MSAL.js kullanan tek sayfalı uygulamalar

Aşağıdaki bölümlerde daha karmaşık olan yaygın senaryolar tartışılır. Temel çalışma prensibi, Koşullu Erişim ilkesi uygulanan hizmet için belirteç istendiği anda Koşullu Erişim ilkelerinin değerlendirilmesidir.

## <a name="scenario-app-performing-the-on-behalf-of-flow"></a>Senaryo: Uygulama akışı adına gerçekleştirme

Bu senaryoda, yerel bir uygulamanın web hizmeti/API olarak adlandırdığı durumda yürüyoruz. Buna karşılık, bu hizmet bir downstream hizmeti aramak için "adına"akışı yapar. Bizim durumumuzda, Koşullu Erişim politikamızı akış aşağı hizmetine (Web API 2) uyguladık ve sunucu/daemon uygulaması yerine yerel bir uygulama kullanıyoruz. 

![Akış diyagramını adına gerçekleştiren uygulama](./media/v2-conditional-access-dev-guide/app-performing-on-behalf-of-scenario.png)

Web API 1 için ilk belirteç isteği, Web API 1 her zaman akış aşağı API'sine çarpmayabileceğinden, son kullanıcıyı çok faktörlü kimlik doğrulamasına gerektirmez. Web API 1, Web API 2 için kullanıcı adına bir belirteç istemeye çalıştığında, kullanıcı çok faktörlü kimlik doğrulamayla oturum açılmadığından istek başarısız olur.

Azure AD, bazı ilginç verilerle birlikte bir HTTP yanıtı döndürür:

> [!NOTE]
> Bu durumda, çok faktörlü bir kimlik doğrulama hatası açıklaması, ancak `interaction_required` Koşullu Erişim ile ilgili olası geniş bir yelpazede var.

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API 2 App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Web API 1'de hatayı `error=interaction_required`yakalar ve `claims` bu zorluğu masaüstü uygulamasına geri göndeririz. Bu noktada, masaüstü uygulaması yeni `acquireToken()` bir arama yapabilir `claims`ve ek bir sorgu dize parametresi olarak meydan ekleyebilirsiniz. Bu yeni istek, kullanıcının çok faktörlü kimlik doğrulaması yapmasını ve ardından bu yeni belirteci Web API 1'e geri göndermesini ve adına akışı tamamlamasını gerektirir.

Bu senaryoyu denemek için [.NET kod örneğimize](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/master/Microsoft.Identity.Web/README.md#handle-conditional-access)bakın. Web API 1'den yerel uygulamaya talep meydan okumasının nasıl geçirilebildiğini ve istemci uygulaması içinde yeni bir istek oluşturmanın nasıl yapılacağını gösterir.

## <a name="scenario-app-accessing-multiple-services"></a>Senaryo: Birden çok hizmete erişen uygulama

Bu senaryoda, bir web uygulamasının biri Koşullu Erişim ilkesi atanmış iki hizmete erişebilen bir durumda yürüyoruz. Uygulama mantığınıza bağlı olarak, uygulamanızın her iki web hizmetine de erişim gerektirmediği bir yol olabilir. Bu senaryoda, belirteç istediğiniz sıra son kullanıcı deneyiminde önemli bir rol oynar.

Web hizmeti A ve B'ye sahip olduğumuzu ve Web hizmeti B'nin Koşullu Erişim politikamızı uyguladığını varsayalım. İlk etkileşimli auth isteği her iki hizmet için de onay gerektirmese de, Koşullu Erişim ilkesi her durumda gerekli değildir. Uygulama web hizmeti B için bir belirteç isterse, ilke çağrılır ve sonraki web hizmeti A istekleri de aşağıdaki gibi başarılı olur.

![Birden çok hizmet akış diyagramına erişen uygulama](./media/v2-conditional-access-dev-guide/app-accessing-multiple-services-scenario.png)

Alternatif olarak, uygulama başlangıçta web hizmeti A için bir belirteç isterse, son kullanıcı Koşullu Erişim ilkesini çağırmaz. Bu, uygulama geliştiricisinin son kullanıcı deneyimini denetlemesine ve Koşullu Erişim ilkesini her durumda çağrılmaya zorlamamasına olanak tanır. Zor durumda uygulama daha sonra web hizmeti B için bir belirteç talep eğer. Bu noktada, son kullanıcının Koşullu Erişim ilkesine uyması gerekir. Uygulama , aşağıdaki `acquireToken`hatayı oluşturmaya çalıştığında (aşağıdaki diyagramda gösterilmiştir):

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

![Yeni bir belirteç isteyen birden çok hizmete erişen uygulama](./media/v2-conditional-access-dev-guide/app-accessing-multiple-services-new-token.png)

Uygulama MSAL kitaplığını kullanıyorsa, belirteci elde edilememesi her zaman etkileşimli olarak yeniden denenir. Bu etkileşimli istek oluştuğunda, son kullanıcı Koşullu Erişim'e uyma fırsatına sahiptir. Bu, isteğ inatla `AcquireTokenSilentAsync` `PromptBehavior.Never` kullanıcıya politikaya uyma fırsatı ```AcquireToken``` vermek için etkileşimli bir istek tesbit etmesi gerekmediği sürece geçerlidir.

## <a name="scenario-single-page-app-spa-using-msaljs"></a>Senaryo: MSAL.js kullanarak tek sayfalık uygulama (SPA)

Bu senaryoda, koşullu erişim korumalı web API'sını aramak için MSAL.js kullanarak tek sayfalık bir uygulamamız (SPA) olduğunda servis etütediyoruz. Bu basit bir mimari dir, ancak Koşullu Erişim etrafında geliştirirken dikkate alınması gereken bazı nüansları vardır.

MSAL.js'de belirteçleri elde eden birkaç `loginPopup()` `acquireTokenSilent(...)`işlev `acquireTokenPopup(…)`vardır: , , , ve `acquireTokenRedirect(…)`.

* `loginPopup()`Etkileşimli oturum açma isteği aracılığıyla bir kimlik belirteci alır, ancak herhangi bir hizmet için erişim belirteçleri almaz (Koşullu Erişim korumalı web API'si dahil).
* `acquireTokenSilent(…)`daha sonra sessizce hiçbir koşulda UI göstermez anlamı bir erişim belirteci elde etmek için kullanılabilir.
* `acquireTokenPopup(…)`ve `acquireTokenRedirect(…)` her ikisi de her zaman oturum açma kullanıcı arabirimi göstermek anlamına gelen bir kaynak için etkileşimli olarak bir belirteç istemek için kullanılır.

Bir uygulamanın Web API'sini çağırmak için bir `acquireTokenSilent(…)`erişim jetonuna ihtiyacı olduğunda, bir . Belirteç oturumunun süresi dolmuşsa veya Koşullu Erişim ilkesine uymamız gerekiyorsa, `acquireTokenPopup()` elde `acquireTokenRedirect()` *edilen Token* işlevi başarısız olur ve uygulama kullanır veya .

![MSAL akış diyagramını kullanarak tek sayfalık uygulama](./media/v2-conditional-access-dev-guide/spa-using-msal-scenario.png)

Koşullu Erişim senaryomuzla bir örnek üzerinden bakalım. Son kullanıcı sadece siteye indi ve bir oturum yok. Bir `loginPopup()` arama gerçekleştiriyoruz, çok faktörlü kimlik doğrulaması olmadan bir kimlik belirteci alıyoruz. Daha sonra kullanıcı, uygulamanın bir web API'sinden veri istemesini gerektiren bir düğmeye basar. Uygulama bir `acquireTokenSilent()` arama yapmaya çalışır, ancak kullanıcı henüz çok faktörlü kimlik doğrulaması gerçekleştirmediğinden ve Koşullu Erişim ilkesine uyması gerektiğinden başarısız olur.

Azure AD aşağıdaki HTTP yanıtını geri gönderir:

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
```

`error=interaction_required`Uygulamamızın. Uygulama daha sonra `acquireTokenPopup()` aynı `acquireTokenRedirect()` kaynakta veya üzerinde kullanabilirsiniz. Kullanıcı çok faktörlü bir kimlik doğrulaması yapmak zorunda kalır. Kullanıcı çok faktörlü kimlik doğrulamasını tamamladıktan sonra, uygulama istenen kaynak için yeni bir erişim belirteci verilir.

Bu senaryoyu denemek [için, kod örneği adına JS SPA'mıza](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/master/Microsoft.Identity.Web/README.md#handle-conditional-access)bakın. Bu kod örneği, bu senaryoyu göstermek için daha önce bir JS SPA'ya kaydettiğiniz Koşullu Erişim ilkesini ve web API'sını kullanır. Talepler meydan okumasını nasıl düzgün bir şekilde ele alacağınızı ve Web API'nız için kullanılabilecek bir erişim belirteci elde etmeyi gösterir. Alternatif olarak, bir Açısal SPA rehberlik için genel [Açısal.js kod örneği](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2) çıkış

## <a name="see-also"></a>Ayrıca bkz.

* Özellikler hakkında daha fazla bilgi edinmek için [Azure Etkin Dizini'nde Koşullu Erişim'e](/azure/active-directory/conditional-access/overview)bakın.
* Daha fazla Azure AD kodu örneği için [örneklere](sample-v2-code.md)bakın.
* MSAL SDK hakkında daha fazla bilgi için ve başvuru belgelerine erişmek için [Microsoft Kimlik Doğrulama Kitaplığı'na genel bakış'a](msal-overview.md)bakın.
* Çok kiracılı senaryolar hakkında daha fazla bilgi edinmek [için, çok kiracılı deseni kullanarak kullanıcıları nasıl oturum alabildiğini](howto-convert-app-to-be-multi-tenant.md)öğrenin.
* [Koşullu erişim ve IoT uygulamalarına erişimi güvence altına alma](/azure/architecture/example-scenario/iot-aad/iot-aad)hakkında daha fazla bilgi edinin.
