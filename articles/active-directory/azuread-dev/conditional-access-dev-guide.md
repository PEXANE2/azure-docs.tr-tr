---
title: Azure AD koşullu erişim Geliştirici Kılavuzu
description: Azure AD koşullu erişim için Geliştirici Kılavuzu ve senaryoları
services: active-directory
author: rwike77
manager: CelesteDG
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda
ms.date: 02/28/2019
ms.service: active-directory
ms.subservice: azuread-dev
ms.custom: aaddev
ms.topic: conceptual
ms.workload: identity
ms.openlocfilehash: 5b7434af51cca2677b6b3d678ca8031cc6dbffab
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77165013"
---
# <a name="developer-guidance-for-azure-active-directory-conditional-access"></a>Koşullu erişim Azure Active Directory için Geliştirici Kılavuzu

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Active Directory (Azure AD) içindeki koşullu erişim özelliği, uygulamanızın güvenliğini sağlamak ve bir hizmeti korumak için kullanabileceğiniz çeşitli yollarla bir tane sunmaktadır. Koşullu erişim, geliştiricilerin ve kurumsal müşterilerin hizmetleri dahil etmek için çok sayıda şekilde korunmasını sağlar:

* Multi-factor authentication
* Yalnızca Intune 'a kayıtlı cihazların belirli hizmetlere erişmesine izin verme
* Kullanıcı konumlarını ve IP aralıklarını kısıtlama

Koşullu erişimin tam özellikleri hakkında daha fazla bilgi için bkz. [Azure Active Directory Koşullu erişim](../active-directory-conditional-access-azure-portal.md).

Azure AD için uygulama oluşturan geliştiriciler için, bu makalede Koşullu erişimi nasıl kullanabileceğiniz gösterilmektedir ve ayrıca, üzerinde denetim sahibi olmadığınız ve koşullu erişim ilkeleri uygulanmış olabilecek kaynaklara erişmenin etkileri hakkında bilgi edineceksiniz. Makalede ayrıca, şirket içi akış, Web uygulamaları, Microsoft Graph erişme ve API 'Leri çağırma içindeki Koşullu erişimin etkileri ele alınabilir.

[Tek ve çok kiracılı](../develop/howto-convert-app-to-be-multi-tenant.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) uygulamalar ve [ortak kimlik doğrulama desenleri](v1-authentication-scenarios.md) hakkında bilgi verilir.

## <a name="how-does-conditional-access-impact-an-app"></a>Koşullu erişim bir uygulamayı nasıl etkiler?

### <a name="app-types-impacted"></a>Etkilenen uygulama türleri

Çoğu ortak durumda, koşullu erişim bir uygulamanın davranışını değiştirmez veya geliştiriciden herhangi bir değişiklik yapılmasını gerektirmez. Yalnızca bir uygulama bir hizmet için dolaylı olarak veya sessizce sessizce bir belirteç istediğinde, bir uygulama, koşullu erişimi "zorluk" işlemek için kod değişiklikleri gerektirir. Etkileşimli bir oturum açma isteği gerçekleştirmek kadar kolay olabilir.

Özellikle, aşağıdaki senaryolar koşullu erişimi "zorluk" işleyecek şekilde kod gerektirir:

* Şirket adına akış gerçekleştiren uygulamalar
* Birden çok hizmete/kaynağa erişen uygulamalar
* ADAL. js kullanan tek sayfalı uygulamalar
* Kaynak çağırma Web Apps

Koşullu erişim ilkeleri uygulamaya uygulanabilir, ancak uygulamanızın eriştiği bir Web API 'sine de uygulanabilir. Koşullu erişim ilkesini yapılandırma hakkında daha fazla bilgi edinmek için bkz. [hızlı başlangıç: koşullu erişim Azure Active Directory belirli uygulamalar IÇIN MFA gerektirme](../conditional-access/app-based-mfa.md).

Senaryoya bağlı olarak, bir kurumsal müşteri, koşullu erişim ilkelerini istediğiniz zaman uygulayabilir ve kaldırabilir. Yeni bir ilke uygulandığında uygulamanızın çalışmaya devam edebilmesi için, "Challenge" işlemesini uygulamanız gerekir. Aşağıdaki örneklerde, sınama işleme gösterilmektedir.

### <a name="conditional-access-examples"></a>Koşullu erişim örnekleri

Bazı senaryolar, koşullu erişimi işlemek için kod değişikliği gerektirir, diğerleri ise olduğu gibi çalışır. Daha fazla bilgi veren çok faktörlü kimlik doğrulaması yapmak için koşullu erişimi kullanan bazı senaryolar aşağıda verilmiştir.

* Tek kiracılı bir iOS uygulaması oluşturuyor ve koşullu erişim ilkesi uygulayacaksınız. Uygulama bir kullanıcıya oturum açar ve bir API 'ye erişim isteğinde yoktur. Kullanıcı oturum açtığında, ilke otomatik olarak çağrılır ve kullanıcının Multi-Factor Authentication (MFA) gerçekleştirmesi gerekir. 
* Bir aşağı akış API 'sine erişmek için orta katman hizmeti kullanan yerel bir uygulama derleniyor. Bu uygulamayı kullanan şirketteki bir kurumsal müşteri, aşağı akış API 'sine bir ilke uygular. Son Kullanıcı oturum açtığında, yerel uygulama orta katmana erişim ister ve belirteci gönderir. Orta katman, aşağı akış API 'sine erişim istemek için şirket adına akış gerçekleştirir. Bu noktada, Orta katmana bir "Challenge" talebi sunulur. Orta katman, bir testi, koşullu erişim ilkesiyle uyumlu olması gereken yerel uygulamaya geri gönderir.

#### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph, koşullu erişim ortamlarında uygulama oluştururken özel hususlar vardır. Genellikle, Koşullu erişimin mekanizması aynı şekilde davranır, ancak kullanıcılarınızın göreceği ilkeler, uygulamanızın grafikte istediği temel verileri temel alır. 

Özellikle, tüm Microsoft Graph kapsamları, ilkelerin uygulanabileceğini tek bir veri kümesini temsil eder. Koşullu erişim ilkelerine belirli veri kümelerine atandıklarından Azure AD, grafik kendisi yerine Graph 'ın arkasındaki verileri temel alan koşullu erişim ilkelerini zorlayacak.

Örneğin, bir uygulama aşağıdaki Microsoft Graph kapsamları isterse,

```
scopes="Bookings.Read.All Mail.Read"
```

Bir uygulama, kullanıcılarının, kayıtlar ve Exchange üzerinde ayarlanan tüm ilkeleri yerine getirmelerini bekleyebilir. Bazı kapsamlar, erişim izni veriyorsa birden çok veri kümesine eşlenir. 

### <a name="complying-with-a-conditional-access-policy"></a>Koşullu erişim ilkesiyle uyumlu

Birçok farklı uygulama topolojisi için, oturum oluşturulduğunda koşullu erişim ilkesi değerlendirilir. Bir koşullu erişim ilkesi uygulama ve hizmetlerin ayrıntı düzeyi üzerinde çalışırken, çağrıldığı nokta, gerçekleştirmeye çalıştığınız senaryoya göre büyük ölçüde değişir.

Uygulamanız koşullu erişim ilkesiyle bir hizmete erişmeye çalıştığında, koşullu erişim zorluğu ile karşılaşabilir. Bu zorluk, Azure AD 'den yanıt olarak gelen `claims` parametresinde kodlanır. Bu sınama parametresine bir örnek aşağıda verilmiştir: 

```
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Geliştiriciler bu zorluğu alabilir ve Azure AD 'ye yeni bir istek ekleyebilir. Bu durumun geçirilmesi, son kullanıcıya koşullu erişim ilkesiyle uyum sağlamak için gereken tüm işlemleri gerçekleştirmesini ister. Aşağıdaki senaryolarda hatanın ayrıntıları ve parametreyi ayıklama açıklanmaktadır.

## <a name="scenarios"></a>Senaryolar

### <a name="prerequisites"></a>Önkoşullar

Azure AD koşullu erişimi, [Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)eklenen bir özelliktir. [Lisanssız kullanım raporundaki](../active-directory-conditional-access-unlicensed-usage-report.md)lisanslama gereksinimleri hakkında daha fazla bilgi edinebilirsiniz. Geliştiriciler, Azure AD Premium içeren Enterprise Mobility Suite 'e ücretsiz bir abonelik içeren [Microsoft Geliştirici ağı](https://msdn.microsoft.com/dn308572.aspx)'na katılabilir.

### <a name="considerations-for-specific-scenarios"></a>Belirli senaryolara yönelik konular

Aşağıdaki bilgiler yalnızca bu koşullu erişim senaryolarında geçerlidir:

* Şirket adına akış gerçekleştiren uygulamalar
* Birden çok hizmete/kaynağa erişen uygulamalar
* ADAL. js kullanan tek sayfalı uygulamalar

Aşağıdaki bölümlerde daha karmaşık olan yaygın senaryolar ele alınmaktadır. Çekirdek işletim ilkesi, koşullu erişim ilkeleri uygulanmış olan hizmet için belirtecin istendiği sırada değerlendirilir.

## <a name="scenario-app-performing-the-on-behalf-of-flow"></a>Senaryo: uygulama, şirket adına akış gerçekleştiriyor

Bu senaryoda, bir yerel uygulamanın bir Web hizmetini/API 'yi çağırdığı durumu ele aldık. Bu hizmet, "yerinde" akışını, bir aşağı akış hizmetini çağırmak için yapar. Bu durumda, koşullu erişim ilkenizi aşağı akış hizmetine (Web API 2) uyguladık ve sunucu/Daemon uygulaması yerine yerel bir uygulama kullanıyor. 

![Uygulama şirket adına akış diyagramı 'nı gerçekleştiriyor](./media/conditional-access-dev-guide/app-performing-on-behalf-of-scenario.png)

Web API 1 her zaman aşağı akış API 'sine ulaşmayamayacağından, Web API 1 için ilk belirteç isteği son kullanıcıya Multi-Factor Authentication için istemde bulunmaz. Web API 1, Web API 2 için Kullanıcı adına bir belirteç istemeyi denediğinde, Kullanıcı Multi-Factor Authentication ile oturum açmadığından, istek başarısız olur.

Azure AD bazı ilgi çekici verilerle HTTP yanıtı döndürür:

> [!NOTE]
> Bu örnekte, bir Multi-Factor Authentication hata açıklaması, ancak koşullu erişimle ilgili çok sayıda `interaction_required` olabilir.

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API 2 App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Web API 1 ' de hata `error=interaction_required`yakalar ve `claims` sınamasını masaüstü uygulamasına geri gönderirsiniz. Bu noktada, masaüstü uygulaması yeni bir `acquireToken()` çağrısı yapabilir ve `claims`sınamasını ek bir sorgu dizesi parametresi olarak ekleyebilir. Bu yeni istek, kullanıcının çok faktörlü kimlik doğrulamasını yapması ve ardından bu yeni belirteci Web API 1 ' e geri göndermesini ve şirket adına akışını tamamlamasını gerektirir.

Bu senaryoyu denemek için bkz. [.NET kod](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca)örneğimiz. Talep sınamasını Web API 1 ' den yerel uygulamaya nasıl geçirebileceğinizi ve istemci uygulaması içinde yeni bir istek oluşturmayı gösterir.

## <a name="scenario-app-accessing-multiple-services"></a>Senaryo: birden çok hizmete erişen uygulama

Bu senaryoda, bir Web uygulamasının, bir koşullu erişim ilkesinin atandığı iki hizmete eriştiği bir durum ele aldık. Uygulama mantığınıza bağlı olarak, uygulamanızın her iki Web hizmetine de erişmesi gerekmeyen bir yol bulunabilir. Bu senaryoda, belirteç isteme sırası, son kullanıcı deneyiminde önemli bir rol oynar.

A ve B Web hizmeti ve B Web hizmeti, koşullu erişim ilkenizin uygulanmış olduğunu varsayalım. İlk etkileşimli kimlik doğrulama isteği her iki hizmet için de onay gerektirdiğinden, koşullu erişim ilkesi her durumda gerekli değildir. Uygulama Web hizmeti B için bir belirteç isterse, ilke çağrılır ve sonraki Web hizmeti istekleri de aşağıdaki gibi başarılı olur.

![Birden çok hizmet akışı diyagramına erişen uygulama](./media/conditional-access-dev-guide/app-accessing-multiple-services-scenario.png)

Alternatif olarak, uygulama başlangıçta Web hizmeti için bir belirteç isterse, Son Kullanıcı koşullu erişim ilkesini çağırmaz. Bu, uygulama geliştiricisinin Son Kullanıcı deneyimini denetlemesine izin verir ve koşullu erişim ilkesinin her durumda çağrılmasını zorlamaz. Karmaşık durum, uygulamanın daha sonra Web hizmeti B için bir belirteç istemesi durumunda olur. Bu noktada, son kullanıcının koşullu erişim ilkesiyle uyumlu olması gerekir. Uygulama `acquireToken`denediğinde aşağıdaki hata oluşabilir (Aşağıdaki diyagramda gösterilmiştir):

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

![Yeni bir belirteç isteyen birden çok hizmete erişen uygulama](./media/conditional-access-dev-guide/app-accessing-multiple-services-new-token.png)

Uygulama ADAL kitaplığını kullanıyorsa, belirteci alma hatası her zaman etkileşimli olarak yeniden denenir. Bu etkileşimli istek gerçekleştiğinde, son kullanıcının koşullu erişime uyum sağlamak için bir fırsat vardır. Bu, istek bir `AcquireTokenSilentAsync` veya `PromptBehavior.Never`, bu durumda uygulamanın son kullanıcıya ilkeyle uyum sağlaması için bir etkileşimli ```AcquireToken``` isteği gerçekleştirmesi gereken durumlar için geçerlidir.

## <a name="scenario-single-page-app-spa-using-adaljs"></a>Senaryo: ADAL. js kullanan tek sayfalı uygulama (SPA)

Bu senaryoda, koşullu erişim korumalı bir Web API 'SI çağırmak için ADAL. js ' yi kullanarak tek sayfalı bir uygulama (SPA) olduğunda büyük/küçük harfe kılavuzluk ederiz. Bu basit bir mimaridir, ancak koşullu erişim konusunda geliştirme yaparken dikkate alınması gereken bazı nuslar vardır.

ADAL. js ' de, belirteçleri elde eden birkaç işlev vardır: `login()`, `acquireToken(...)`, `acquireTokenPopup(…)`ve `acquireTokenRedirect(…)`.

* `login()`, etkileşimli bir oturum açma isteği aracılığıyla bir KIMLIK belirteci edinir, ancak herhangi bir hizmet için (koşullu erişim korumalı Web API 'SI dahil) erişim belirteçleri almaz.
* `acquireToken(…)`, bir erişim belirtecini sessizce almak için kullanılabilir ve bu, herhangi bir koşulda Kullanıcı arabirimini göstermez.
* `acquireTokenPopup(…)` ve `acquireTokenRedirect(…)` her ikisi de, her zaman oturum açma kullanıcı arabirimini gösteren bir kaynak için bir belirteç istemek üzere kullanılır.

Bir uygulama, Web API 'sini çağırmak için bir erişim belirtecine ihtiyaç duyduğunda, bir `acquireToken(…)`dener. Belirteç oturumunun kullanım alanı dolmuşsa veya koşullu erişim ilkesiyle uyumlu olması gerekiyorsa, *Acquiretoken* işlevi başarısız olur ve uygulama `acquireTokenPopup()` veya `acquireTokenRedirect()`kullanır.

![ADAL akış diyagramı kullanan tek sayfalı uygulama](./media/conditional-access-dev-guide/spa-using-adal-scenario.png)

Koşullu erişim senaryoümüzü kullanarak bir örnek yürülim. Son Kullanıcı siteye tam olarak sahiptir ve bir oturumu yoktur. `login()` çağrısı yaptık, çok faktörlü kimlik doğrulaması olmadan bir KIMLIK belirteci alacaksınız. Daha sonra Kullanıcı, uygulamanın bir Web API 'sinden veri istemesini gerektiren bir düğmeye rastar. Uygulama bir `acquireToken()` çağrısı gerçekleştirmeye çalışır, ancak Kullanıcı Multi-Factor Authentication 'ı henüz gerçekleştirmediğinden ve koşullu erişim ilkesiyle uyumlu olması gerektiğinden başarısız olur.

Azure AD aşağıdaki HTTP yanıtını geri gönderir:

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
```

Uygulamamız `error=interaction_required`yakalayacak. Uygulama daha sonra aynı kaynakta `acquireTokenPopup()` ya da `acquireTokenRedirect()` kullanabilir. Kullanıcı çok faktörlü kimlik doğrulaması yapmak için zorlanır. Kullanıcı Multi-Factor Authentication 'ı tamamladıktan sonra, uygulama istenen kaynak için yeni bir erişim belirteci vermiş olur.

Bu senaryoyu denemek için, bkz. [js Spa-adına sahip kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca). Bu kod örneği, bu senaryoyu göstermek için daha önce bir JS SPA ile kaydettiğiniz koşullu erişim ilkesini ve Web API 'sini kullanır. Talep sınamasını nasıl doğru bir şekilde işleyeceğinizi ve Web API 'niz için kullanılabilecek bir erişim belirteci nasıl alınacağını gösterir. Alternatif olarak, angular SPA ile ilgili yönergeler için genel [angular. js kod örneğini](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) kullanıma alın

## <a name="see-also"></a>Ayrıca bkz.

* Yetenekler hakkında daha fazla bilgi için bkz. [Azure Active Directory Koşullu erişim](../active-directory-conditional-access-azure-portal.md).
* Daha fazla Azure AD kod örneği için bkz. [kod örnekleri GitHub deposu](https://github.com/azure-samples?utf8=%E2%9C%93&q=active-directory).
* ADAL SDK ve başvuru belgelerine erişme hakkında daha fazla bilgi için bkz. [kitaplık Kılavuzu](active-directory-authentication-libraries.md).
* Çok kiracılı senaryolar hakkında daha fazla bilgi edinmek için bkz. [çoklu kiracı düzeniyle Kullanıcı oturumu açma](../develop/howto-convert-app-to-be-multi-tenant.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).
