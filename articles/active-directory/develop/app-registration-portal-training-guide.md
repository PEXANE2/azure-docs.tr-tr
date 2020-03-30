---
title: Yeni Azure portalı uygulama kaydı deneyimi
titleSuffix: Microsoft identity platform
description: Azure portalındaki yeni Uygulama kaydı deneyimine giriş
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 11/8/2019
ms.author: marsma
ms.reviewer: lenalepa, alamaral
ms.custom: aaddev
ms.openlocfilehash: 50c88dd1785bd9177219054fed3800ca725a5274
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154602"
---
# <a name="the-new-azure-portal-app-registration-experience"></a>Yeni Azure portalı uygulama kayıt deneyimi

Azure portalındaki yeni [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminde birçok iyileştirme vardır. Eski deneyim olarak adlandırılan yakınsanmış uygulamaları kaydetmek veya yönetmek için Uygulama kayıt portalı (apps.dev.microsoft.com) deneyimini daha iyi biliyorsanız, bu eğitim kılavuzu yeni deneyimi kullanmaya başlamanızı sağlar.

## <a name="whats-not-changing"></a>Ne değişmiyor?

- Uygulamalarınız ve ilgili yapılandırmalarınız yeni deneyimde olduğu gibi bulunabilir. Uygulamaları yeniden kaydetmeniz gerekmez ve uygulamalarınızın kullanıcılarının yeniden oturum açmalarına gerek yoktur.

    > [!NOTE]
    > Azure portalında bulmak için uygulamaları kaydetmek için kullandığınız hesapla oturum açmanız gerekir. Azure portalındaki oturum açmış kullanıcının profilinizdeki e-posta adresini karşılaştırarak Uygulama kayıt portalında oturum açan kullanıcıyla eşleştiğini kontrol etmenizi öneririz.
    >
    > Bazı durumlarda, özellikle Azure AD e-posta adresiyle kişisel Microsoft hesaplarını (Outlook, Live, Xbox, vb.) kullanarak oturum açtığınızda, azure portalına eski deneyimden gittiğinizde, aynı hesapla farklı bir hesaba imzaladığını fark ettik. Azure AD kiracınıza e-posta gönder. Uygulamalarınızın hala eksik olduğunu düşünüyorsanız, oturumunuzu açın ve doğru hesapla oturum açın.

- Kişisel Microsoft hesapları kullanılarak oluşturulan canlı SDK uygulamaları henüz Azure portalında desteklenmez ve yakın gelecekte de eski deneyiminde kalmaya devam eder.

## <a name="key-changes"></a>Önemli değişiklikler

-   Eski deneyimlerde, uygulamalar varsayılan olarak tüm kuruluş hesaplarını (çok kiracılı) ve kişisel Microsoft hesaplarını destekleyen yakınsanmış uygulamalar olarak kaydedilmiştir. Bu, eski deneyim le değiştirilemedi ve bu da yalnızca kuruluş hesaplarını (çok kiracılı veya tek kiracı) destekleyen uygulamalar oluşturmayı zorlaştırdı.
    Yeni deneyim, tüm bu seçenekleri destekleyen uygulamaları kaydetmenize olanak tanır. [Uygulama türleri hakkında daha fazla bilgi edinin.](active-directory-v2-registration-portal.md)

-   Yeni deneyimde, kişisel Microsoft hesabınız da bir Azure AD kiracısındaysa, kiracıdaki tüm uygulamalar, kiracıdaki sahip olunan uygulamalar ve kişisel hesabınızdaki uygulamalar olmak üzere üç sekme görürsünüz. Bu nedenle, kişisel Microsoft hesabınıza kayıtlı uygulamaların eksik olduğunu düşünüyorsanız, **Kişisel hesap sekmenizden Uygulamalar'ı** kontrol edin.

-   Yeni deneyimde, profilinize yönlendirerek ve anahtar dizini seçerek kiracılar arasında kolayca geçiş yapabilirsiniz.

## <a name="list-of-applications"></a>Başvuru listesi

-   Yeni uygulama listesi, Azure portalındaki eski uygulama kayıtları deneyimi (yalnızca Azure AD hesaplarında oturum açan uygulamalar) aracılığıyla kaydedilmiş uygulamaları ve [Uygulama kayıt portalına](https://apps.dev.microsoft.com/) rağmen kaydedilmiş uygulamaları (hem Azure AD hem de kişisel Microsoft hesaplarında oturum açan uygulamalar) gösterir.

-   Yeni uygulama listesinde iki sütun daha vardır: Sütun **ve** **Sertifikalar & sırlar** sütununda oluşturulan ve uygulamada kayıtlı olan kimlik bilgilerinin durumunu (geçerli, süresi dolan veya süresi dolan) gösterir.

## <a name="new-app-registration"></a>Yeni uygulama kaydı

Eski deneyimlerime göre, yakınsanmış bir uygulamayı kaydetmek için yalnızca bir Ad sağlamanız gerekiyordu. Oluşturulan uygulamalar, tüm kuruluş dizinini (çok kiracılı) ve kişisel Microsoft hesaplarını destekleyen yakınsanmış uygulamalar olarak kaydedildi.  Bu, eski deneyim le değiştirilemedi ve bu da yalnızca kuruluş hesaplarını (çok kiracılı veya tek kiracı) destekleyen uygulamalar oluşturmayı zorlaştırdı. [Desteklenen hesap türleri hakkında daha fazla bilgi edinin](v2-supported-account-types.md)

Yeni deneyimde, uygulama için bir Ad sağlamanız ve Desteklenen hesap türlerini seçmeniz gerekir. İsteğe bağlı olarak yeniden yönlendirme URI sağlayabilirsiniz.
Uri'yi yeniden yönlendirme sağlıyorsanız, web/herkese açık (yerel/mobil ve masaüstü) olup olmadığını belirtmeniz gerekir. Yeni uygulama kayıtları deneyimini kullanarak bir uygulamayı nasıl kaydedebilirsiniz hakkında daha fazla bilgi için [bu hızlı başlangıç'a](quickstart-register-app.md)bakın.

## <a name="app-management-page"></a>Uygulama yönetimi sayfası

Eski deneyimaşağıdaki bölümlerle yakınsama uygulamaları için tek bir uygulama yönetim sayfası vardı: Özellikler, Uygulama sırları, Platformlar, Sahipleri, Microsoft Grafik İzinleri, Profil ve Gelişmiş Seçenekler.

Azure portalındaki yeni deneyim, bu özellikleri ayrı sayfalarda temsil eder. Eşdeğer işlevselliği burada bulabilirsiniz:

-   Özellikler - Ad ve Uygulama Kimliği Genel Bakış sayfasındadır.

-   Uygulama Sırları Sertifikalar & sırları sayfasında

-   Platformlar yapılandırması Kimlik Doğrulama sayfasında

-   Microsoft Graph izinleri diğer izinlerle birlikte API izinleri sayfasında

-   Profil Markalama sayfasında

-   Gelişmiş seçenek - Canlı SDK desteği Kimlik Doğrulama sayfasındadır.

## <a name="application-secretscertificates--secrets"></a>Uygulama sırları/Sertifikalar & sırları

Yeni deneyim, **Uygulama sırları** **Sertifikalar & sırları**olarak yeniden adlandırılmıştır. Buna ek olarak, **Ortak anahtarlar** **Sertifikalar** olarak adlandırılır ve **Parolalar** **İstemci sırları**olarak adlandırılır. Güvenlik nedenleriyle bu işlevselliği yeni deneyimde getirmemeyi seçtik, bu nedenle artık yeni bir anahtar çifti oluşturamazsınız.

## <a name="platformsauthentication-reply-urlsredirect-uris"></a>Platformlar/Kimlik Doğrulama: URL'leri yanıtla/URL'leri yeniden yönlendirme
Eski deneyimde, bir uygulama, Yönlendirme URL'lerini, Logout URL'sini ve Örtülü akışı yapılandırmak için Web, yerel ve Web API'sı için Platformlar bölümüne sahipti.

Yeni deneyimde, Yanıt URL'leri bir uygulamanın\'Kimlik Doğrulama bölümünde bulunabilir. Buna ek olarak, bunlar yeniden yönlendirme URI'leri olarak adlandırılır ve yeniden yönlendirme URI biçimi değişti. Bir uygulama türüyle (web veya genel istemci - mobil ve masaüstü) ilişkilendirilmeleri gerekir. [Daha fazlasını öğrenin](quickstart-configure-app-access-web-apis.md#add-redirect-uris-to-your-application)

Web API'leri Bir API Ortaya Çıkar sayfasında yapılandırılır.

> [!NOTE]
> Hedeflemek istediğiniz platforma veya cihaza göre uygulamanızın ayarlarını yapılandırabileceğiniz yeni Kimlik Doğrulama ayarları deneyimini deneyin. [Daha fazlasını öğrenin](quickstart-configure-app-access-web-apis.md#configure-platform-settings-for-your-application)

## <a name="microsoft-graph-permissionsapi-permissions"></a>Microsoft Graph izinleri/API izinleri

-   Eski deneyimde bir API seçerken, yalnızca Microsoft Grafik API'leri arasından seçim yapabilirsiniz. Yeni deneyimde, Microsoft Graph, kuruluşunuzdaki API'ler ve API'ler dahil olmak üzere birçok Microsoft API'si arasından seçim yapabilirsiniz, bu üç sekmede sunulur: Microsoft API'leri, kuruluşumun kullandığı API'ler veya API'lerim. Kuruluşumdaki API'ler üzerindeki arama çubuğu, kiracıdaki servis ilkeleri aracılığıyla sekme aramaları kullanır.

    > [!NOTE]
    > Uygulamanız bir kiracıyla ilişkili değilse bu sekmeyi görmezsiniz. Yeni deneyimi kullanarak izinleri nasıl isteyeceksiniz hakkında daha fazla bilgi için [bu hızlı başlangıç'a](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/active-directory/develop/quickstart-configure-app-access-web-apis.md)bakın.

-   Eski deneyimbir Hibe **izinleri** düğmesi yoktu. Yeni deneyimde, bir uygulamanın API izinleri bölümünde **Grant yöneticisi onayı** düğmesi bulunan bir Hibe onayı bölümü vardır. Yalnızca bir yönetici izin verebilir ve bu düğme yalnızca yöneticiler için etkinleştirilir. Bir yönetici Grant **admin onay** düğmesini seçtiğinde, istenen tüm izinlere yönetici onayı verilir.

## <a name="profile"></a>Profil
Eski deneyimde Profil'de Logo, Giriş sayfası URL'si, Hizmet Şartları URL'si ve Gizlilik Bildirimi URL yapılandırması vardı. Yeni deneyim, bu Marka lama sayfasında bulunabilir.

## <a name="application-manifest"></a>Uygulama bildirimi
Yeni deneyimde, Manifest sayfası uygulamanın özniteliklerini güncellemenize ve güncellemenize olanak tanır. Daha fazla bilgi için [Uygulama bildirimine](reference-app-manifest.md)bakın.

## <a name="new-ui"></a>Yeni UI
Daha önce yalnızca bildirim düzenleyicisi veya API kullanılarak ayarlanabilen veya var olmayan özellikler için yeni bir arama arabirimi vardır.

-   Örtülü hibe akışı (oauth2AllowImplicitFlow) Kimlik Doğrulama sayfasında bulunabilir. Eski deneyimin aksine, erişim belirteçleri veya kimlik belirteçleri veya her ikisini de etkinleştirebilirsiniz.

-   Bu API (oauth2Permissions) ve Yetkili istemci uygulamaları (ön Yetkili Uygulamalar) tarafından tanımlanan kapsamlar, api açığa çıkarma sayfası üzerinden yapılandırılabilir. Bir uygulamanın web API'si olarak nasıl yapılandırılabildiğini ve izinleri/kapsamları nasıl açıkhale erdireceğiniz hakkında daha fazla bilgi için [bu hızlı başlangıç'a](quickstart-configure-app-expose-web-apis.md)bakın.

-   Publisher etki alanı [(uygulamanın\'onay isteminde](application-consent-experience.md)kullanıcılara görüntülenen) Markalama bıçağı sayfasında bulunabilir. Yayımcı etki alanını yapılandırma hakkında daha fazla bilgi için [bu nasıl yapılandırıldık bilgisine](howto-configure-publisher-domain.md)bakın.

## <a name="limitations"></a>Sınırlamalar

Yeni deneyimaşağıdaki sınırlamalar vardır:

-   Yeni deneyim, Azure AD B2C kiracıları için uygulama kayıtlarını henüz desteklememektedir.

-   Yeni deneyim henüz kişisel Microsoft hesapları ile oluşturulan Live SDK uygulamalarını desteklememektedir.

-   Desteklenen hesapların değerini değiştirmek UI'de desteklenmez. Azure AD tek kiracılı\'ve çok kiracılı arasında geçiş yaptığınız sürece uygulama bildirimini kullanmanız gerekir.

   > [!NOTE]
   > Azure AD kiracısında kişisel bir Microsoft hesabı kullanıcısıysanız ve kiracı yöneticinin Azure portalına erişimi kısıtlanmışsa, erişim reddedilebilir. Ancak, kısayol üzerinden arama çubuğuna Uygulama kayıtlarını yazarak veya sabitleyerek gelirseniz, yeni deneyime erişebilirsiniz.
