---
title: Geliştirdiğiniz mobil uygulamalarda çoklu oturum açma ve uygulama koruma ilkelerini destekleme | Mavisi
titleSuffix: Microsoft identity platform
description: Microsoft Identity platformunu kullanarak çoklu oturum açma ve uygulama koruma ilkelerini destekleyen ve Azure Active Directory ile tümleştirerek mobil uygulamalar oluşturmaya yönelik açıklama ve genel bakış.
services: active-directory
author: knicholasa
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/14/2020
ms.author: nichola
ms.openlocfilehash: 4f0588667df6acb11a43e8c3469c67f65ed3cdd9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98165187"
---
# <a name="support-single-sign-on-and-app-protection-policies-in-mobile-apps-you-develop"></a>Geliştirdiğiniz mobil uygulamalarda çoklu oturum açma ve uygulama koruma ilkelerini destekleme

Çoklu oturum açma (SSO), uygulamanızın kullanıcıları için kolay ve güvenli oturum açmaları sağlayan Microsoft Identity platform ve Azure Active Directory temel bir tekliftir. Ayrıca, uygulama koruma ilkeleri (uygulama), kullanıcılarınızın verilerini güvende tutan anahtar güvenlik ilkelerinin desteğini etkinleştirir. Birlikte, bu özellikler, uygulamanızın verilerinin güvenli kullanıcı oturumlarını ve yönetimini etkinleştirir.

> [!VIDEO https://www.youtube.com/embed/JpeMeTjQJ04]

Bu makalede, SSO ve UYGULAMANıN neden önemli olduğunu ve bu özellikleri destekleyen mobil uygulamalar oluşturmaya yönelik üst düzey kılavuz sağladığını açıklamaktadır. Bu, hem telefon hem de tablet uygulamaları için geçerlidir. Kuruluşunuzun Azure Active Directory kiracısında SSO dağıtmak isteyen bir BT yöneticisiyseniz, [Çoklu oturum açma dağıtımı planlama yönergemizi](../manage-apps/plan-sso-deployment.md) inceleyin

## <a name="about-single-sign-on-and-app-protection-policies"></a>Çoklu oturum açma ve uygulama koruma ilkeleri hakkında

[Çoklu oturum açma (SSO)](../manage-apps/plan-sso-deployment.md) , bir kullanıcının bir kez oturum açmasını ve kimlik bilgilerini yeniden girmeden diğer uygulamalara erişmesini sağlar. Bu, uygulamalara erişimi kolaylaştırır ve kullanıcıların uzun Kullanıcı adı ve parola listelerini hatırlamaları gereksinimini ortadan kaldırır. Uygulamanızı uygulamanıza uygulamak, uygulamanızı daha kolay hale getirir.

Ayrıca, uygulamanızda çoklu oturum açmayı etkinleştirmek, modern kimlik doğrulama ile gelen ve [parolasız oturum açma](../authentication/concept-authentication-passwordless.md)işlemleri gibi yeni kimlik doğrulama mekanizmalarının kilidini açar. Kullanıcı adları ve parolalar uygulamalara karşı en popüler saldırı vektörlerinden biridir ve SSO 'yu etkinleştirmek, koşullu erişimi veya ek güvenlik ekleyen veya daha güvenli kimlik doğrulama mekanizmalarına güvenen, daha az sayıda oturum açma izni vererek bu riski azaltmanıza olanak tanır. Son olarak, çoklu oturum açmayı etkinleştirmek [Çoklu oturum açma](v2-protocols-oidc.md#single-sign-out)olanağı da sunar. Bu, Paylaşılan cihazlarda kullanılacak iş uygulamaları gibi durumlarda faydalıdır.

[Uygulama koruma ilkeleri (uygulama)](/mem/intune/apps/app-protection-policy) bir kuruluşun verilerinin güvende ve dahil olduğundan emin olun. Şirketler, şirketlerin bir uygulamadaki verilerini yönetmesine ve korumasına olanak tanır ve uygulamaya ve verilerine kimlerin erişebilecekleri denetime izin verir. Uygulama koruma ilkelerini uygulamak, uygulamanızın koşullu erişim ilkeleri tarafından korunan kaynaklara bağlanmasını ve diğer korumalı uygulamalardan ve bu uygulamalardan güvenli bir şekilde veri aktarmasını sağlar. Uygulama koruma ilkeleri tarafından kilitlenmemiş senaryolar, bir uygulamayı açmak, uygulamalar arasında veri paylaşımını denetlemek ve şirket uygulama verilerinin kişisel depolama konumlarına kaydedilmesini önlemek için bir PIN gerektirir.

## <a name="implementing-single-sign-on"></a>Çoklu oturum açma uygulama

Uygulamanızın çoklu oturum açma özelliğinden yararlanmasını sağlamak için aşağıdakiler önerilir.

### <a name="use-the-microsoft-authentication-library-msal"></a>Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanma

Uygulamanızda çoklu oturum açmayı uygulamak için en iyi seçenek, [Microsoft kimlik doğrulama kitaplığı 'nı (msal)](msal-overview.md)kullanmaktır. MSAL kullanarak, en az kod ve API çağrılarında uygulamanıza kimlik doğrulaması ekleyebilir, [Microsoft Identity platformunun](./index.yml)tüm özelliklerini alabilir ve Microsoft 'un güvenli bir kimlik doğrulama çözümünün bakımını gerçekleştirmesini sağlayabilirsiniz. Varsayılan olarak, MSAL, uygulamanız için SSO desteği ekler. Ayrıca, uygulama koruma ilkelerini uygulamayı da planlamanız planlanmanız durumunda MSAL kullanılması da bir gereksinimdir.

> [!NOTE]
> MSAL ' i katıştırılmış Web görünümü kullanacak şekilde yapılandırmak mümkündür. Bu, çoklu oturum açmayı engeller. SSO 'nun çalışmasını sağlamak için varsayılan davranışı (yani, sistem Web tarayıcısı) kullanın.

Şu anda uygulamanızdaki [adal kitaplığını](../azuread-dev/active-directory-authentication-libraries.md) kullanıyorsanız, [adal kullanım DıŞı](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/update-your-applications-to-use-microsoft-authentication-library/ba-p/1257363)olduğu [için msal 'e geçirmeniz](msal-migration.md)önemle önerilir.

İOS uygulamaları için, MSAL kullanarak oturum açma işlemlerini nasıl ayarlayabileceğine ve [ÇEŞITLI SSO SENARYOLARıNDA msal yapılandırmasına kılavuzluk](single-sign-on-macos-ios.md)eden bir [hızlı başlangıç](quickstart-v2-ios.md) sunuyoruz.

Android uygulamaları için, MSAL kullanarak oturum açma işlemlerini ayarlamayı ve [msal kullanarak Android 'de uygulamalar arası SSO 'yu nasıl etkinleştireceğinizi](msal-android-single-sign-on.md)gösteren bir [hızlı başlangıç](quickstart-v2-android.md) sunuyoruz.

### <a name="use-the-system-web-browser"></a>Sistem Web tarayıcısını kullanma

Etkileşimli kimlik doğrulaması için bir Web tarayıcısı gerekir. MSAL dışında modern kimlik doğrulama kitaplıklarını kullanan mobil uygulamalar için (diğer bir deyişle, diğer OpenID Connect veya SAML kitaplıkları) veya kendi kimlik doğrulama kodunuzu uygularsanız, SSO 'yu etkinleştirmek için sistem tarayıcısını kimlik doğrulama yüzeyi olarak kullanmanız gerekir.

Google, Android uygulamalarında bunu yapmaya yönelik rehberlik içerir: [Chrome özel sekmeleri-Google Chrome](https://developer.chrome.com/multidevice/android/customtabs).

Apple 'ın bunu iOS uygulamalarında yapma kılavuzu vardır: bir [Web hizmeti aracılığıyla bir kullanıcının kimliğini doğrulama | Apple geliştirici belgeleri](https://developer.apple.com/documentation/authenticationservices/authenticating_a_user_through_a_web_service).

> [!TIP]
> [Apple cihazları Için SSO eklentisi](apple-sso-plugin.md) , Intune kullanarak yönetilen cihazlarda ekli Web görünümlerini kullanan iOS UYGULAMALARıNA yönelik SSO 'ya izin verir. Tüm kullanıcılar için SSO 'yu etkinleştiren uygulamalar geliştirmeye yönelik en iyi seçenek olarak MSAL ve sistem tarayıcısı önerilir, ancak bu, aksi durumda mümkün olmadığı bazı senaryolarda SSO 'ya izin verir.

## <a name="enable-app-protection-policies"></a>Uygulama koruma Ilkelerini etkinleştirme

Uygulama koruma ilkelerini etkinleştirmek için [Microsoft kimlik doğrulama kitaplığı 'nı (msal)](msal-overview.md)kullanın. MSAL, Microsoft Identity platformunun kimlik doğrulama ve yetkilendirme kitaplığı ve Intune SDK 'Sı ile birlikte çalışmak üzere geliştirilmiştir.

Ayrıca, kimlik doğrulaması için bir aracı uygulaması kullanmanız gerekir. Aracı uygulamanın uyumluluğunu sağlamak için uygulamanın uygulama ve cihaz bilgilerini sağlamasını gerektirir. iOS kullanıcıları [Microsoft Authenticator uygulamasını](../user-help/user-help-auth-app-sign-in.md) kullanır ve Android kullanıcıları, [aracılı kimlik doğrulaması](./msal-android-single-sign-on.md)için Microsoft Authenticator uygulamasını veya [Şirket Portalı uygulamasını](https://play.google.com/store/apps/details?id=com.microsoft.windowsintune.companyportal) kullanır. Varsayılan olarak, MSAL, bir kimlik doğrulama isteği yerine bir aracı kullanır; bu nedenle, kimlik doğrulaması için aracı kullanılması, MSAL kullanıma hazır olduğunuzda uygulamanız için otomatik olarak etkinleştirilir.

Son olarak, uygulama koruma ilkelerini etkinleştirmek için [Intune SDK 'sını uygulamanıza ekleyin](/mem/intune/developer/app-sdk-get-started) . Çoğu bölüm için SDK, bir kesme modelini izler ve uygulama koruma ilkelerini otomatik olarak uygulayarak uygulamanın işlem yapmasına izin verilip verilmediğini tespit eder. Ayrıca, belirli eylemlerde kısıtlamalar varsa uygulamayı söylemek için el ile çağırabilen API 'Ler de vardır.

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure Active Directory çoklu oturum açma dağıtımı planlayın](../manage-apps/plan-sso-deployment.md)
- [Nasıl yapılır: macOS ve iOS 'ta SSO 'yu yapılandırma](single-sign-on-macos-ios.md)
- [Apple cihazları için Microsoft Enterprise SSO eklentisi (Önizleme)](apple-sso-plugin.md)
- [Android 'de aracılı kimlik doğrulaması](./msal-android-single-sign-on.md)
- [Yetkilendirme aracıları ve bunların nasıl etkinleştirileceği](./msal-android-single-sign-on.md)
- [Microsoft Intune Uygulama SDK’sını kullanmaya başlama](/mem/intune/developer/app-sdk-get-started)
- [Intune Uygulama SDK'sı ayarlarını yapılandırma](/mem/intune/developer/app-sdk-ios#configure-settings-for-the-intune-app-sdk)
- [Microsoft Intune korumalı uygulamalar](/mem/intune/apps/apps-supported-intune-apps)
