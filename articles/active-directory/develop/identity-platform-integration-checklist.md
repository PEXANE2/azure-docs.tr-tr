---
title: Microsoft Identity platform ile tümleştirin | Mavisi
description: Microsoft Identity platform (v 2.0) ile tümleştirilirken en iyi uygulamalar ve ortak aşırı sürüm hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: ryanwi
ms.reviewer: lenalepa, sureshja, jesakowi
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 33128cbece3b217778182b3831b02e2f3f654f3b
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68853209"
---
# <a name="microsoft-identity-platform-integration-checklist"></a>Microsoft Identity platform tümleştirmesi denetim listesi

Microsoft Identity platform tümleştirme denetim listesi, sizi yüksek kaliteli ve güvenli bir tümleştirmede size kılavuzluk etmek üzere tasarlanmıştır. Microsoft Identity platformu ile tümleştirilirken en iyi uygulamaları ve ortak bosıflarını vurgular, böylece uygulamanın kimlik platformuyla tümleştirmesinin kalitesini ve güvenliğini koruduğunuzdan emin olmak için listeyi düzenli aralıklarla gözden geçirin. Denetim listesi, tüm uygulamanızı gözden geçirmeyi amaçlanmamış. Platform üzerinde geliştirmeler yaptığımız için denetim listesinin içeriği değişebilir.

Yeni başladıysanız, kimlik doğrulama temelleri, Microsoft Identity platform 'daki uygulama senaryoları ve daha fazlası hakkında bilgi edinmek için [belgelere](index.yml) göz atın.

## <a name="testing-your-integration"></a>Tümleştirmenizi test etme

Uygulamanızın [Microsoft Identity platformu](https://docs.microsoft.com/azure/active-directory/develop/)ile etkin bir şekilde tümleştirildiğinden emin olmak için aşağıdaki denetim listesini kullanın.

### <a name="basics"></a>Temel

|   |   |
|---|---|
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | [Microsoft Platformu ilkelerini](https://go.microsoft.com/fwlink/?linkid=2090497&clcid=0x409)okuyun ve anlayın. Uygulamanızın, kullanıcıları ve platformu korumak üzere tasarlandıkları koşullara uyduğundan emin olun. |

### <a name="ownership"></a>Fazlasının

|   |   |
|---|---|
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamaları kaydetmek ve yönetmek için kullandığınız hesapla ilişkili bilgilerin güncel olduğundan emin olun. |

### <a name="branding"></a>Markalama

|   |   |
|---|---|
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | [Uygulamalar Için marka yönergelerine](howto-add-branding-in-azure-ad-apps.md)uyar. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanız için anlamlı bir ad ve logo sağlayın. Bu bilgiler, uygulamanızın izin isteminde görüntülenir. Kullanıcılarınızın bilinçli kararlar verebilmeleri için, adınızın ve logonuzun şirketinizin/ürününüzün temsilciyle aynı olduğundan emin olun. Herhangi bir ticari marka ihlal olmadığından emin olun. |

### <a name="privacy"></a>Gizlilik

|   |   |
|---|---|
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanızın hizmet koşulları ve gizlilik bildirimi için bağlantılar sağlar. |

### <a name="security"></a>Güvenlik

|   |   |
|---|---|
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Tüm yeniden yönlendirme URI 'Lerinin sahipliğini koruyun ve DNS kayıtlarını güncel tutun. URI 'larınız için joker karakterler (*) kullanmayın. Web uygulamaları için tüm URI 'Lerin güvenli ve şifreli olduğundan emin olun (örneğin, https şemaları kullanarak). Genel istemciler için, varsa platforma özgü yeniden yönlendirme URI 'Lerini kullanın (temel olarak iOS ve Android için). Aksi takdirde, uygulamanıza geri çağrı yaparken çarpışmaları engellemek için yüksek miktarda rasgelelik ile yeniden yönlendirme URI 'Leri kullanın. Uygulamanız yalıtılmış bir Web aracısından kullanılıyorsa, kullanabilirsiniz https://login.microsoftonline.com/nativeclient. Tüm kullanılmayan veya gereksiz yeniden yönlendirme URI 'Lerini düzenli olarak gözden geçirin ve kırpın. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanız bir dizine kayıtlıysa, uygulama kayıt sahiplerinin listesini simge durumuna küçültün ve el ile izleyin. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Açıkça gerekli olmadığı takdirde [OAuth2 örtük verme akışı](v2-oauth2-implicit-grant-flow.md) desteğini etkinleştirmeyin. Geçerli senaryo hakkında [buradan](v1-oauth2-implicit-grant-flow.md#suitable-scenarios-for-the-oauth2-implicit-grant)bilgi edinin. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Kullanıcıların parolalarını doğrudan işleyen [kaynak sahibi parola kimlik bilgisi akışını (ROPC)](v2-oauth-ropc.md)kullanmayın. Bu akış, yüksek derecede güven ve Kullanıcı pozlaması gerektirir ve yalnızca diğer, daha güvenli ve akış kullanılmıyorsa kullanılmalıdır. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulama kimlik bilgilerinizi koruyun ve yönetin. Parola kimlik bilgilerini (istemci gizli dizileri) değil [sertifika kimlik bilgilerini](active-directory-certificate-credentials.md)kullanın. Parola kimlik bilgisi kullanmanız gerekiyorsa, el ile ayarlama. Kimlik bilgilerini kod veya yapılandırmaya depolamayın ve bu kullanıcıların insanların işlenmesine izin vermez. Mümkünse, [Azure kaynakları için Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) veya kimlik bilgilerinizi depolamak ve düzenli aralıklarla döndürmek için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) kullanın. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanızın en az ayrıcalık izinleri istediğinizden emin olun. Yalnızca uygulamanızın kesinlikle ihtiyacı olan ve yalnızca ihtiyacınız olduğunda izin ister. Farklı [izin türlerini](v1-permissions-and-consent.md#types-of-permissions)anlayın. Yalnızca gerekirse uygulama izinlerini kullanın; mümkün olduğunca temsilci izinleri kullanın. Microsoft Graph izinlerinin tam listesi için bu [izin başvurusuna](https://docs.microsoft.com/graph/permissions-reference)bakın. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Bir API 'YI Microsoft Identity platformunu kullanarak güvenli hale getiriyorsanız, ortaya çıkarmak gereken izinleri dikkatle düşünün. Çözümünüz için doğru ayrıntı düzeyini ve hangi izinlerin yönetici onayı gerektirdiğini göz önünde bulundurun. Herhangi bir yetkilendirme kararı vermeden önce gelen belirteçlerde beklenen izinleri denetleyin. |

### <a name="implementation"></a>Uygulama

|   |   |
|---|---|
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Kullanıcıların güvenli bir şekilde oturum açması için modern kimlik doğrulama çözümlerini (OAuth 2,0, [OpenID Connect](v2-protocols-oidc.md)) kullanın. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Protokolleri kendiniz uygulamayın – [Microsoft tarafından desteklenen kimlik doğrulama kitaplıklarını](reference-v2-libraries.md) kullanın (msal, sunucu ara yazılımı). Kimlik doğrulama kitaplığının tümleştirildiği en son sürümünü kullandığınızdan emin olun. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanızın gerektirdiği veriler [Microsoft Graph](https://developer.microsoft.com/graph)aracılığıyla kullanılabiliyorsa, bireysel apı yerine Microsoft Graph uç noktasını kullanarak bu veriler için izinleri isteyin. |

### <a name="end-user-experience"></a>Son kullanıcı deneyimi

|   |   |
|---|---|
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Son kullanıcılar ve yöneticilerin uygulamanıza güvenip güvenmediğine yönelik yeterli bilgiye sahip olması için [onay deneyimini anlayın](application-consent-experience.md) ve uygulamanızın izin sorma parçalarını yapılandırın. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Etkileşimli akışlardan önce sessiz kimlik doğrulaması (sessiz belirteç alımı) gerçekleştirmeye çalışırken uygulamanızı kullanırken, kullanıcının oturum açma kimlik bilgilerini kaç kez girmesi gerektiğini en aza indirin. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Her oturum açma için "Prompt = onay" kullanmayın. Yalnızca ek izinler için onay sormanız gerektiğini belirlerseniz (örneğin, uygulamanızın gerekli izinlerini değiştirdiyseniz), yalnızca Prompt = onay kullanın. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygun olduğunda, uygulamanızı kullanıcı verileriyle zenginleştirin. [Microsoft Graph API 'yi](https://developer.microsoft.com/graph) kullanmak, bunu yapmanın kolay bir yoludur. Başlamanıza yardımcı olabilecek [grafik Gezgini](https://developer.microsoft.com/graph/graph-explorer) aracı. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Yöneticilerin kiracılarına kolayca onay verebilmeleri için uygulamanızın gerektirdiği tüm izin kümesini kaydedin. Kullanıcıların, uygulamanın ilk kez başlatıldığında istekte bulunma veya karışmasına neden olabilecek izinler istediğini anlamasına yardımcı olmak için çalışma zamanında [artımlı onay](azure-ad-endpoint-comparison.md#incremental-and-dynamic-consent) kullanın. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Temiz bir [Çoklu oturum açma deneyimi](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut)uygulayın. Bu bir gizlilik ve güvenlik gereksinimidir ve iyi bir kullanıcı deneyimi sunar. |

### <a name="testing"></a>Test Etme

|   |   |
|---|---|
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Kullanıcılarınızın uygulamanızı kullanma yeteneğini etkileyebilecek [koşullu erişim ilkeleri](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut) için test. |
| ![kutusunun](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanızı, desteklemeyi planladığınız tüm olası hesapları (örneğin, iş veya okul hesapları, kişisel Microsoft hesapları, alt hesaplar ve bağımsız hesaplar) test edin. |

## <a name="additional-resources"></a>Ek kaynaklar

V2.0 hakkında ayrıntılı bilgileri keşfedin:

* [Microsoft Identity platform (v 2.0 genel bakış)](v2-overview.md)
* [Microsoft Identity platform protokolleri başvurusu](active-directory-v2-protocols.md)
* [Erişim belirteçleri başvurusu](access-tokens.md)
* [Kimlik belirteçleri başvurusu](id-tokens.md)
* [Kimlik doğrulama kitaplıkları başvurusu](reference-v2-libraries.md)
* [Microsoft Identity platformunda izinler ve onay](v2-permissions-and-consent.md)
* [Microsoft Graph API'si](https://developer.microsoft.com/graph)
