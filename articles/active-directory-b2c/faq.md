---
title: Azure Active Directory B2C için sık sorulan sorular (SSS)
description: Azure Active Directory B2C hakkında sık sorulan soruların yanıtları.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 40285c811cd6f407c20c40bf3a90ec5b779a9c18
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264407"
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Azure AD B2C: Sık sorulan sorular (SSS)

Bu sayfa, Azure Etkin Dizin B2C (Azure AD B2C) hakkında sık sorulan soruları yanıtlar. Güncelleştirmeleri tekrar kontrol etmeye devam edin.

### <a name="why-cant-i-access-the-azure-ad-b2c-extension-in-the-azure-portal"></a>Azure portalındaki Azure AD B2C uzantısına neden erişemiyorum?

Azure AD uzantısının sizin için çalışmaması için iki yaygın neden vardır. Azure AD B2C, dizindeki kullanıcı rolünün genel yönetici olmasını gerektirir. Erişiminiz olması gerektiğini düşünüyorsanız lütfen yöneticinize başvurun. Genel yönetici ayrıcalıklarınız varsa, Azure Etkin Dizin dizininde değil, Azure AD B2C dizininde olduğunuzu unutmayın. [Azure AD B2C kiracıoluşturmak](tutorial-create-tenant.md)için yönergeleri görebilirsiniz.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Mevcut, çalışan tabanlı Azure AD kiracımda Azure AD B2C özelliklerini kullanabilir miyim?

Azure AD ve Azure AD B2C ayrı ürün teklifleridir ve aynı kiracıda bir arada bulunamaz. Azure AD kiracısı bir kuruluşu temsil eder. Azure AD B2C kiracı, güvenilen parti uygulamalarıyla kullanılacak kimlik koleksiyonunu temsil eder. **Azure AD B2C > Kimlik sağlayıcıları altında** veya özel ilkelerle Yeni **OpenID Bağlantı sağlayıcısı** ekleyerek, Azure AD B2C bir kuruluştaki çalışanların kimlik doğrulamasına izin vererek Azure AD'ye federate yapabilir.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Office 365'e sosyal giriş (Facebook ve Google+) sağlamak için Azure AD B2C'yi kullanabilir miyim?

Azure AD B2C, Microsoft Office 365 kullanıcılarının kimliğini doğrulamak için kullanılamaz. Azure AD, Çalışanların SaaS uygulamalarına erişimini yönetmek için Microsoft'un çözümüdür ve lisanslama ve Koşullu Erişim gibi bu amaç için tasarlanmış özelliklere sahiptir. Azure AD B2C, web ve mobil uygulamalar oluşturmak için bir kimlik ve erişim yönetimi platformu sağlar. Azure AD B2C, bir Azure AD kiracısına federalerate olarak yapılandırıldığını, Azure AD kiracısı çalışanların Azure AD B2C'ye dayanan uygulamalara erişimini yönetir.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Azure AD B2C'deki yerel hesaplar nelerdir? Azure AD'deki iş veya okul hesaplarından farkı nedir?

Azure AD kiracısında, formun e-posta adresiyle kiracıya ait kullanıcılar oturum `<xyz>@<tenant domain>`açsın. Kiracıveya `<tenant domain>` ilk `<...>.onmicrosoft.com` etki alanında doğrulanmış etki alanlarından biridir. Bu tür bir hesap bir çalışma veya okul hesabıdır.

Azure AD B2C kiracısında, çoğu uygulama kullanıcının herhangi bir rasgele e-posta joe@comcast.net bob@gmail.comadresiyle (örneğin, , , sarah@contoso.comveya) jim@live.comoturum açmasını ister. Bu tür bir hesap yerel bir hesaptır. Ayrıca rasgele kullanıcı adlarını yerel hesaplar olarak da destekliyoruz (örneğin, joe, bob, sarah veya jim). Azure portalında Azure AD B2C için kimlik sağlayıcılarını yapılandırırken bu iki yerel hesap türünden birini seçebilirsiniz. Azure AD B2C kiracınızda **Kimlik sağlayıcıları,** **Yerel hesabı**seçin ve ardından Kullanıcı **Adı'nı**seçin.

Uygulamalar için kullanıcı hesapları kaydolma kullanıcı akışı, kaydolma veya oturum açma kullanıcı akışı, Microsoft Graph API veya Azure portalı üzerinden oluşturulabilir.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Şu anda hangi sosyal kimlik sağlayıcılarını destekliyorsunuz? Gelecekte hangilerini desteklemeyi planlıyorsun?

Şu anda Amazon, Facebook, GitHub (önizleme), Google, LinkedIn, Microsoft Account (MSA), QQ (önizleme), Twitter, WeChat (önizleme) ve Weibo (önizleme) dahil olmak üzere birçok sosyal kimlik sağlayıcısını destekliyoruz. Diğer popüler sosyal kimlik sağlayıcılarına müşteri talebine dayalı destek eklemeyi değerlendiriyoruz.

Azure AD B2C özel [ilkeleri](custom-policy-overview.md)de destekler. Özel ilkeler, [OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) veya SAML'yi destekleyen herhangi bir kimlik sağlayıcısı için kendi ilkenizi oluşturmanıza olanak sağlar. [Özel ilke başlangıç paketimize](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack)check-in yaparak özel ilkelere başlayın.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Kapsamları, çeşitli sosyal kimlik sağlayıcılardan tüketiciler hakkında daha fazla bilgi toplamak için yapılandırabilir miyim?

Hayır. Desteklenen sosyal kimlik sağlayıcıları kümemiz için kullanılan varsayılan kapsamlar şunlardır:

* Facebook: e-posta
* Google+: e-posta
* Microsoft hesabı: openid e-posta profili
* Amazon: profil
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Azure AD B2C ile çalışması için uygulamamın Azure'da çalıştırılması gerekiyor mu?

Hayır, uygulamanızı istediğiniz yerde (bulutta veya şirket içinde) barındırabilirsiniz. Azure AD B2C ile etkileşim de olması gereken, herkese açık uç noktalarda HTTP istekleri gönderip alma yeteneğidir.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Birden çok Azure AD B2C kiracım var. Azure portalında bunları nasıl yönetebilirim?

Azure portalının sol yan menüsünde 'Azure AD B2C' açmadan önce, yönetmek istediğiniz dizine geçmeniz gerekir. Azure portalının sağ üst kısmında kimliğinizi tıklatarak dizinleri değiştirin ve ardından açılan açılır da görünen bir dizin seçin.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Azure AD B2C tarafından gönderilen doğrulama e-postalarını (içerik ve "Kimden:" alanı) nasıl özelleştirebilirim?

Doğrulama e-postalarının içeriğini özelleştirmek için [şirket markalandırma özelliğini](../active-directory/fundamentals/customize-branding.md) kullanabilirsiniz. Özellikle, e-postanın bu iki öğesi özelleştirilebilir:

* **Banner logosu**: Sağ alt ta gösterilmiştir.
* **Arka plan rengi**: Üstte gösterilir.

    ![Özelleştirilmiş doğrulama e-postasının ekran görüntüsü](./media/faq/company-branded-verification-email.png)

E-posta imzası, Azure AD B2C kiracısını ilk oluşturduğunuzda sağladığınız Azure AD B2C kiracısının adını içerir. Şu yönergeleri kullanarak adı değiştirebilirsiniz:

1. Azure [portalında](https://portal.azure.com/) Global Administrator olarak oturum açın.
1. **Azure Active Directory** dikey penceresini açın.
1. **Özellikler** sekmesini tıklatın.
1. **Ad** alanını değiştirin.
1. Sayfanın üst kısmından **Kaydet**'e tıklayın.

Şu anda e-postadaki "Gönderen:" alanını değiştirmenin bir yolu yoktur.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Varolan kullanıcı adlarımı, parolalarımı ve profillerimi veritabanımdan Azure AD B2C'ye nasıl geçirebilirim?

Geçiş aracınızı yazmak için Microsoft Graph API'yi kullanabilirsiniz. Ayrıntılar için [Kullanıcı geçiş kılavuzuna](user-migration.md) bakın.

### <a name="what-password-user-flow-is-used-for-local-accounts-in-azure-ad-b2c"></a>Azure AD B2C'deki yerel hesaplar için hangi parola kullanıcı akışı kullanılır?

Yerel hesaplar için Azure AD B2C parola kullanıcı akışı, Azure AD ilkesine dayanır. Azure AD B2C'nin kaydolma, kaydolma veya kaydolma ve parola sıfırlama kullanıcı akışları "güçlü" parola gücünü kullanır ve parolasüresi dolmaz. Daha fazla ayrıntı için [Azure Etkin Dizini'ndeki Parola ilkeleri ve kısıtlamalarına](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy)bakın.

Hesap kilitlemeleri ve parolalar hakkında bilgi için [bkz.](threat-management.md)

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Şirket içi Active Directory'mde depolanan tüketici kimliklerini Azure AD B2C'ye geçirmek için Azure AD Connect'i kullanabilir miyim?

Hayır, Azure AD Connect Azure AD B2C ile çalışacak şekilde tasarlanmaz. Kullanıcı geçişi için [Microsoft Graph API'yi](manage-user-accounts-graph-api.md) kullanmayı düşünün. Ayrıntılar için [Kullanıcı geçiş kılavuzuna](user-migration.md) bakın.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Uygulamam bir iFrame içinde Azure AD B2C sayfalarını açabilir mi?

Hayır, güvenlik nedeniyle Azure AD B2C sayfaları iFrame içinde açılamaz. Hizmetimiz iFrame'leri yasaklamak için tarayıcıyla iletişim kurar. Genel olarak güvenlik topluluğu ve OAUTH2 belirtimi, tıklama kaçırma riski nedeniyle kimlik deneyimleri için iFrame kullanmamanızı önerir.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure AD B2C, Microsoft Dynamics gibi CRM sistemleriyle çalışıyor mu?

Microsoft Dynamics 365 Portal ile tümleştirme kullanılabilir. Kimlik [doğrulama için Azure AD B2C'yi kullanmak için Dynamics 365 Portal'ı yapılandırma ya](https://docs.microsoft.com/dynamics365/customer-engagement/portals/azure-ad-b2c)da bkz.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Azure AD B2C, SharePoint 2016 veya daha önce şirket içinde çalışıyor mu?

Azure AD B2C, SharePoint dış iş ortağı paylaşımı senaryosu için değildir; bunun yerine [Azure AD B2B'ye](https://cloudblogs.microsoft.com/enterprisemobility/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview/) bakın.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Dış kimlikleri yönetmek için Azure AD B2C veya B2B kullanmalı mıyım?

Uygun özellikleri dış kimlik senaryolarınıza uygulama hakkında daha fazla bilgi edinmek için [Azure AD'de B2B işbirliğini ve B2C'yi karşılaştır'ı](../active-directory/b2b/compare-with-b2c.md) okuyun.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Azure AD B2C hangi raporlama ve denetim özelliklerini sağlar? Azure AD Premium'dakiyle aynı mı?

Hayır, Azure AD B2C, Azure AD Premium ile aynı rapor kümesini desteklemez. Ancak birçok ortak nokta vardır:

* **Oturum açma raporları,** her oturum açma nın bir kaydını daha az ayrıntıyla birlikte sağlar.
* **Denetim raporları** hem yönetici etkinliğini hem de uygulama etkinliğini içerir.
* **Kullanım raporları,** kullanıcı sayısını, oturum açma sayısını ve MFA'nın hacmini içerir.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Azure AD B2C tarafından sunulan sayfaların web'ini yerelleştirebilir miyim? Hangi diller desteklenir?

Evet, [bkz. dil özelleştirme.](user-flow-language-customization.md) 36 dil için çeviri ler sağlıyoruz ve ihtiyaçlarınıza uygun herhangi bir dizeyi geçersiz kılabilirsiniz.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-contosob2clogincom-to-logincontosocom"></a>Azure AD B2C tarafından sunulan kayıt ve oturum açma sayfalarımda kendi URL'lerimi kullanabilir miyim? Örneğin, URL'yi contoso.b2clogin.com'dan login.contoso.com değiştirebilir miyim?

Şu anda hayır. Bu özellik yol haritamızda yer almaktadır. Azure portalındaki **Etki Alanları** sekmesinde etki alanınızı doğrulamak bu amaca ulaşmaz. Ancak, b2clogin.com ile, [tarafsız](b2clogin.md)bir üst düzey etki alanı sunuyoruz , ve böylece dış görünüm Microsoft söz olmadan uygulanabilir.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Azure AD B2C kiracımı nasıl silerim?

Azure AD B2C kiracınızı silmek için aşağıdaki adımları izleyin.

Geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. *Abonelik Yöneticisi*olarak [Azure portalında](https://portal.azure.com/) oturum açın. Azure'a kaydolmak için kullandığınız aynı iş veya okul hesabını veya aynı Microsoft hesabını kullanın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. Azure AD B2C kiracınızdaki tüm **Kullanıcı akışlarını (ilkeleri)** silin.
1. Azure AD B2C kiracınızda kaydettiğiniz tüm **Uygulamaları** silin.
1. Sol menüde **Azure Etkin Dizini'ni** seçin.
1. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
1. Sırayla her kullanıcıyı seçin (şu anda oturum açmış olduğunuz *Abonelik Yöneticisi* kullanıcısını hariç tutun). Sayfanın altındaki **Sil'i** seçin ve istendiğinde **EVET'i** seçin.
1. **Yönet**altında, **Uygulama kayıtlarını** (veya Uygulama kayıtlarını **(Eski)** seçin.
1. **Tüm uygulamaları görüntüle'yi** seçin
1. **b2c-extensions-app**adlı uygulamayı seçin , **Sil'i**seçin ve istendiğinde **Evet'i** seçin.
1. **Yönet**altında, **Kullanıcı ayarlarını**seçin.
1. Varsa, **LinkedIn hesap bağlantıları**altında **Hayır'ı**seçin, sonra **Kaydet'i**seçin.
1. **Yönet**altında Özellikleri **seçin**
1. **Azure kaynakları için erişim yönetimi**'nin altında **Evet**'i ve sonra da **Kaydet**'i seçin.
1. Azure portalından çıkış ve erişiminizi yenilemek için tekrar oturum açın.
1. Sol menüde **Azure Etkin Dizini'ni** seçin.
1. Genel **Bakış** sayfasında, **Dizinin Silin'i**seçin. İşlemi tamamlamak için ekrandaki yönergeleri izleyin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. *Abonelik Yöneticisi*olarak [Azure portalında](https://portal.azure.com/) oturum açın. Azure'a kaydolmak için kullandığınız aynı iş veya okul hesabını veya aynı Microsoft hesabını kullanın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. Azure AD B2C kiracınızdaki tüm **Kullanıcı akışlarını (ilkeleri)** silin.
1. **Uygulama kayıtları (Önizleme) seçeneğini**belirleyin, ardından Tüm **uygulamalar** sekmesini seçin.
1. Kaydettiğiniz tüm uygulamaları silin.
1. **B2c uzantıları-uygulamasını**silin.
1. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
1. Sırayla her kullanıcıyı seçin (şu anda oturum açmış olduğunuz *Abonelik Yöneticisi* kullanıcısını hariç tutun). Sayfanın altındaki **Sil'i** seçin ve istendiğinde **Evet'i** seçin.
1. Sol menüde **Azure Etkin Dizini'ni** seçin.
1. **Yönet**altında, **Kullanıcı ayarlarını**seçin.
1. Varsa, **LinkedIn hesap bağlantıları**altında **Hayır'ı**seçin, sonra **Kaydet'i**seçin.
1. **Yönet**altında Özellikleri **seçin**
1. **Azure kaynakları için erişim yönetimi**'nin altında **Evet**'i ve sonra da **Kaydet**'i seçin.
1. Azure portalından çıkış ve erişiminizi yenilemek için tekrar oturum açın.
1. Sol menüde **Azure Etkin Dizini'ni** seçin.
1. Genel **Bakış** sayfasında, **Dizinin Silin'i**seçin. İşlemi tamamlamak için ekrandaki yönergeleri izleyin.

* * *

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Kurumsal Mobilite Paketi'nin bir parçası olarak Azure AD B2C alabilir miyim?

Hayır, Azure AD B2C istediğiniz kadar öde Azure hizmetidir ve Enterprise Mobility Suite'in bir parçası değildir.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Azure AD B2C ile ilgili sorunları nasıl bildirebilirim?

[Azure Etkin Dizin B2C için Dosya destek isteklerine](support-options.md)bakın.
