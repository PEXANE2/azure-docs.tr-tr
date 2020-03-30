---
title: Microsoft kimlik platformu için en iyi uygulamalar | Azure
description: Microsoft kimlik platformuyla tümleştirme yaparken en iyi uygulamalar, öneriler ve genel gözetimler hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/11/2019
ms.author: ryanwi
ms.reviewer: lenalepa, sureshja, jesakowi
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: 56975cebbfe4f6dd6452c850c338d431faea27bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050489"
---
# <a name="microsoft-identity-platform-best-practices-and-recommendations"></a>Microsoft kimlik platformu en iyi uygulamalar ve öneriler

Bu makalede, Microsoft kimlik platformu ile tümleştirme yaparken en iyi uygulamalar, öneriler ve ortak denetimler vurgulanır.  Bu denetim listesi size yüksek kaliteli ve güvenli bir entegrasyon için rehberlik edecektir. Uygulamanızın kimlik platformuyla tümleştirilmesinin kalitesini ve güvenliğini koruduğunuzdan emin olmak için bu listeyi düzenli olarak gözden geçirin. Denetim listesi, tüm uygulamanızı gözden geçirmek için tasarlanmamıştır. Platformda iyileştirmeler yaptığımız için denetim listesinin içeriği değişebilir.

Yeni başlıyorsanız, kimlik doğrulama temelleri, Microsoft kimlik platformundaki uygulama senaryoları ve daha fazlası hakkında bilgi edinmek için [Microsoft kimlik platformu belgelerine](index.yml) göz atın.

Uygulamanızın [Microsoft kimlik platformuyla](https://docs.microsoft.com/azure/active-directory/develop/)etkin bir şekilde tümleşik olduğundan emin olmak için aşağıdaki denetim listesini kullanın.

## <a name="basics"></a>Temel Bilgiler

|   |   |
|---|---|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Microsoft Platform [İlkeleri'ni](https://go.microsoft.com/fwlink/?linkid=2090497&clcid=0x409)okuyun ve anlayın. Uygulamanızın, kullanıcıları ve platformu korumak için tasarlanmış olan koşullara uyduduğundan emin olun. |

## <a name="ownership"></a>Sahiplik

|   |   |
|---|---|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamaları kaydetmek ve yönetmek için kullandığınız hesapla ilişkili bilgilerin güncel olduğundan emin olun. |

## <a name="branding"></a>Markalama

|   |   |
|---|---|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | [Uygulamalar için Markalama yönergelerine uyun.](howto-add-branding-in-azure-ad-apps.md) |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanız için anlamlı bir ad ve logo sağlayın. Bu [bilgiler, uygulamanızın onay isteminde](application-consent-experience.md)görünür. Kullanıcıların bilinçli kararlar alabilmesi için adınızın ve logonuzun şirketinizi/ürününüzü temsil ettiğinden emin olun. Herhangi bir ticari markayı ihlal etmediğinizden emin olun. |

## <a name="privacy"></a>Gizlilik

|   |   |
|---|---|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanızın hizmet koşullarına ve gizlilik bildirimine bağlantılar sağlayın. |

## <a name="security"></a>Güvenlik

|   |   |
|---|---|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Yönlendirme URI'nizi yönetin: <ul><li>Tüm yönlendirme URI'lerinizin sahipliğini koruyun ve dns kayıtlarını güncel tutun.</li><li>URI'lerinizde joker karakterler (*) kullanmayın.</li><li>Web uygulamaları için, tüm ÜrBİ'lerin güvenli ve şifreli olduğundan emin olun (örneğin, https şemaları kullanarak).</li><li>Genel müşteriler için, varsa platforma özel yönlendirme URI'lerini kullanın (özellikle iOS ve Android için). Aksi takdirde, uygulamanızı geri ararken çakışmayı önlemek için yüksek miktarda rastgelelik ile yeniden yönlendirme URI'leri kullanın.</li><li>Uygulamanız yalıtılmış bir web aracısından kullanılıyorsa, .' yi kullanabilirsiniz. `https://login.microsoftonline.com/common/oauth2/nativeclient`</li><li>Kullanılmayan veya gereksiz yönlendirme URI'lerini düzenli olarak gözden geçirin ve kırpın.</li></ul> |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanız bir dizinde kayıtlıysa, uygulama kayıt sahiplerinin listesini en aza indirin ve el ile izleyin. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Açıkça gerekmedikçe [OAuth2 örtülü hibe akışı](v2-oauth2-implicit-grant-flow.md) için destek etkinleştirin. Geçerli senaryo hakkında [buradan](v2-oauth2-implicit-grant-flow.md#suitable-scenarios-for-the-oauth2-implicit-grant)bilgi edinin. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Kullanıcı adının/parolanın ötesine geçin. Doğrudan kullanıcıların parolalarını işleyen [kaynak sahibi parola kimlik bilgisi akışını (ROPC)](v2-oauth-ropc.md)kullanmayın. Bu akış yüksek derecede güven ve kullanıcı maruziyeti gerektirir ve yalnızca diğer, daha güvenli akışlar kullanılamıyorduğunda kullanılmalıdır. Bu akış hala bazı senaryolarda (DevOps gibi) gereklidir, ancak bunu kullanarak uygulamanıza kısıtlamalar empoze edeceğini sakının.  Daha modern yaklaşımlar için [Kimlik Doğrulama akışlarını ve uygulama senaryolarını](authentication-flows-app-scenarios.md)okuyun.|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Web uygulamaları, web API'leri ve daemon uygulamaları için gizli uygulama kimlik bilgilerinizi koruyun ve yönetin. Parola kimlik bilgilerini (istemci sırları) değil, [sertifika kimlik bilgilerini](active-directory-certificate-credentials.md)kullanın. Parola kimlik bilgisi kullanmanız gerekiyorsa, el ile ayarlamayın. Kimlik bilgilerini kod veya config'de saklamayın ve insanlar tarafından ele alınmasına asla izin vermeyin. Mümkünse, kimlik bilgilerinizi depolamak ve düzenli olarak döndürmek için Azure kaynakları veya [Azure Anahtar Kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) [için yönetilen kimlikleri](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) kullanın. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanızın en az ayrıcalık izinlerini istediğinden emin olun. Yalnızca uygulamanızın kesinlikle ihtiyaç duyduğu izinleri isteyin ve yalnızca ihtiyacınız olduğunda isteyin. Farklı [izin türlerini](v2-permissions-and-consent.md#permission-types)anlayın. Yalnızca gerekirse uygulama izinlerini kullanın; mümkünse temsilci izinlerini kullanın. Microsoft Graph izinlerinin tam listesi için şu [izinler başvurusuna](https://docs.microsoft.com/graph/permissions-reference)bakın. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Microsoft kimlik platformlarını kullanarak bir API'yi güvence altına alıyoruzsanız, ortaya çıkarması gereken izinleri dikkatlice düşünün. Çözümünüz için doğru ayrıntılılığın ne olduğunu ve hangi izinizin yönetici onayı gerektirdiğini düşünün. Herhangi bir yetkilendirme kararı vermeden önce gelen belirteçlerde beklenen izinleri denetleyin. |

## <a name="implementation"></a>Uygulama

|   |   |
|---|---|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Kullanıcıları güvenli bir şekilde oturum açmak için modern kimlik doğrulama çözümlerini (OAuth 2.0, [OpenID Connect)](v2-protocols-oidc.md)kullanın. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) |  OAuth 2.0 ve Open ID gibi protokollere karşı doğrudan program yapmayın. Bunun yerine, [Microsoft Kimlik Doğrulama Kitaplığı'ndan (MSAL)](msal-overview.md)yararlanın. MSAL kitaplıkları güvenlik protokollerini kullanımı kolay bir kitaplıkta güvenli bir şekilde kaydırır ve [Koşullu Erişim](/azure/active-directory/conditional-access/overview) senaryoları, aygıt genelinde tek oturum açma [(SSO)](/azure/active-directory/manage-apps/what-is-single-sign-on)ve dahili belirteç önbelleğe alma desteği için yerleşik destek alırsınız. Daha fazla bilgi için, Microsoft desteklenen [istemci kitaplıkları](reference-v2-libraries.md#microsoft-supported-client-libraries) ve [ara yazılım kitaplıklarının](reference-v2-libraries.md#microsoft-supported-server-middleware-libraries) listesine ve [uyumlu üçüncü taraf istemci kitaplıklarının](reference-v2-libraries.md#compatible-client-libraries)listesine bakın.<br/><br/>Kimlik doğrulama protokolleri için el kodu kullanmanız gerekiyorsa, [Microsoft SDL](https://www.microsoft.com/sdl/default.aspx)gibi bir metodoloji izlemeniz gerekir. Her protokol için standart belirtimlerinde güvenlik hususlarına dikkat edin.|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) |  Varolan uygulamaları [Azure Etkin Dizin Kimlik Doğrulama Kitaplığı'ndan (ADAL)](../azuread-dev/active-directory-authentication-libraries.md) [Microsoft Kimlik Doğrulama Kitaplığı'na](msal-overview.md)geçirin. MSAL, Microsoft'un en son kimlik platformu çözümüdür ve ADAL'a tercih edilir. .NET, JavaScript, Android, iOS, macOS'ta mevcuttur ve Python ve Java için genel önizlemede de mevcuttur. [ADAL.NET,](msal-net-migration.md) [ADAL.js](msal-compare-msal-js-and-adal-js.md)ve [ADAL.NET ve iOS broker](msal-net-migration-ios-broker.md) uygulamaları hakkında daha fazla bilgi edinin.|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) |  Mobil uygulamalar için, uygulama kayıt deneyimini kullanarak her platformu yapılandırın. Uygulamanızın tek oturum açma için Microsoft Authenticator veya Microsoft Company Portal'dan yararlanabilmesi için uygulamanızın yapılandırılan bir "broker yeniden yönlendirme URI"ye ihtiyacı vardır. Bu, Microsoft'un kimlik doğrulamadan sonra denetimi uygulamanıza döndürmesine olanak tanır. Her platformu yapılandırırken, uygulama kayıt deneyimi süreç boyunca size rehberlik edecektir. Çalışan bir örneği indirmek için hızlı başlat'ı kullanın. iOS'ta mümkün olduğunca broker ve sistem web görünümünü kullanın.|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) |  Web uygulamalarında veya web API'lerinde, hesap başına bir belirteç önbelleği bulundurun.  Web uygulamaları için belirteç önbelleği hesap kimliğine göre anahtarlanmalıdır.  Web API'leri için, hesap API aramak için kullanılan belirteç karma tarafından anahtarlanmış olmalıdır. MSAL.NET .NET Framework ve .NET Core alt platformlarında özel belirteç önbelleği serileştirme sağlar. Güvenlik ve performans nedenleriyle, tavsiyemiz kullanıcı başına bir önbelleği seri hale getirmektir. Daha fazla bilgi için [belirteç önbelleği serileştirme](msal-net-token-cache-serialization.md#token-cache-for-a-web-app-confidential-client-application)si hakkında bilgi edinin.|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanızın gerektirdiği veriler Microsoft [Graph](https://developer.microsoft.com/graph)aracılığıyla kullanılabilse, tek tek API yerine Microsoft Graph bitiş noktasını kullanarak bu veriler için izin isteyin. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) |Erişim belirteci değerine bakmayın veya istemci olarak ayrışdırmaya çalışmayın.  Değerleri, biçimleri değiştirebilir, hatta uyarı olmadan şifrelenebilirler - istemciniz kullanıcı hakkında bir şeyler öğrenmesi gerekiyorsa veya Microsoft Graph'ı aramak gerekirse her zaman id_token kullanabilirler.  Yalnızca web API'leri erişim belirteçlerini ayrıştırmalıdır (çünkü formatı tanımlayan ve şifreleme anahtarlarını ayarlayanlar bunlardır). |

## <a name="end-user-experience"></a>Son kullanıcı deneyimi

|   |   |
|---|---|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Son kullanıcıların ve yöneticilerin uygulamanıza güvenip güvenmediklerini belirlemek için yeterli bilgiye sahip olmaları için [onay deneyimini anlayın](application-consent-experience.md) ve uygulamanızın onay isteminin parçalarını yapılandırın. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Etkileşimli akışlardan önce sessiz kimlik doğrulamayı (sessiz belirteç edinimi) deneyerek uygulamanızı kullanırken kullanıcının oturum açma kimlik bilgilerini girme sayısını en aza indirin. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Her oturum açma için "prompt=consent" kullanmayın. Yalnızca ek izinler için izin istemeniz gerektiğini belirlediyseniz (örneğin, uygulamanızın gerekli izinlerini değiştirdiyseniz) istemi=onayı kullanın. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Mümkün sayılsa da, uygulamanızı kullanıcı verileriyle zenginleştirin. Microsoft [Graph API'sini](https://developer.microsoft.com/graph) kullanmak, bunu yapmanın kolay bir yoludur. Başlangıç yardımcı olabilecek [Grafik Gezgini](https://developer.microsoft.com/graph/graph-explorer) aracı. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Yöneticilerin kiracılarına kolayca onay verebilmeleri için uygulamanızın gerektirdiği tüm izinleri kaydedin. Kullanıcıların, uygulamanızın ilk başlangıçta istendiğinde kullanıcıları ilgilendirebilecek veya kafasını karıştırabilecek izinler istediğini anlamalarına yardımcı olmak için çalışma zamanında [artımlı onayı](../azuread-dev/azure-ad-endpoint-comparison.md#incremental-and-dynamic-consent) kullanın. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Temiz [bir tek oturum açma deneyimi](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut)uygulayın. Bu bir gizlilik ve güvenlik gereksinimidir ve iyi bir kullanıcı deneyimi sağlar. |

## <a name="testing"></a>Sınama

|   |   |
|---|---|
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Kullanıcılarınızın uygulamanızı kullanma yeteneğini etkileyebilecek [Koşullu Erişim ilkelerini](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut) sınayın. |
| ![onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanızı desteklemeyi planladığınız tüm olası hesaplarla (örneğin, iş veya okul hesapları, kişisel Microsoft hesapları, alt hesaplar ve egemen hesaplar) test edin. |

## <a name="additional-resources"></a>Ek kaynaklar

V2.0 hakkında ayrıntılı bilgileri keşfedin:

* [Microsoft kimlik platformu (v2.0 genel bakış)](v2-overview.md)
* [Microsoft kimlik platformu protokolleri başvuru](active-directory-v2-protocols.md)
* [Erişim belirteçleri başvurusu](access-tokens.md)
* [Kimlik belirteçleri başvurusu](id-tokens.md)
* [Kimlik doğrulama kitaplıkları başvurusu](reference-v2-libraries.md)
* [Microsoft kimlik platformunda izinler ve onay](v2-permissions-and-consent.md)
* [Microsoft Graph API'si](https://developer.microsoft.com/graph)
