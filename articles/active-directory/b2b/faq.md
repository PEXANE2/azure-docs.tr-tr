---
title: B2B işbirliği SSS - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory B2B işbirliği hakkında sık sorulan soruların yanıtlarını alın.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: reference
ms.date: 03/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 92c3e0d77a26db406e24d6d2fa07e96349613634
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050812"
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Azure Active Directory B2B işbirliği SSS

Azure Etkin Dizini (Azure AD) iş-iş (B2B) işbirliği yle ilgili sık sorulan bu sorular (SSSS) yeni konuları içerecek şekilde düzenli aralıklarla güncelleştirilir.

   > [!IMPORTANT]
   > **31 Mart 2021'den itibaren Microsoft,** B2B işbirliği senaryoları için yönetilmeyen Azure AD hesapları ve kiracılar oluşturarak artık davetlerin itfasını desteklemez. Hazırlık olarak, müşterilerin tek [seferlik parola kimlik doğrulaması e-postasını](one-time-passcode.md)seçmelerini öneririz. Bu genel önizleme özelliği hakkındaki görüşlerinizi memnuniyetle karşılıyoruz ve işbirliği yapmak için daha fazla yol oluşturmaktan heyecan duyuyoruz.

### <a name="can-we-customize-our-sign-in-page-so-its-more-intuitive-for-our-b2b-collaboration-guest-users"></a>B2B işbirliği konuk kullanıcılarımız için daha sezgisel olacak şekilde oturum açma sayfamızı özelleştirebilir miyiz?
Kesinlikle! Bu [özellik hakkında blog yazımıza](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/)bakın. Kuruluşunuzun oturum açma sayfasını nasıl özelleştirebilirsiniz hakkında daha fazla bilgi için, [oturum açmanız için şirket markasını ekle ve Panel sayfalarına eriş'](../fundamentals/customize-branding.md)e bakın.

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>B2B işbirliği kullanıcıları SharePoint Online ve OneDrive'a erişebilir mi?
Evet. Ancak, kişi seçiciyi kullanarak SharePoint Online'daki varolan konuk kullanıcıları arama olanağı varsayılan olarak **kapalıdır.** Varolan konuk kullanıcıları arama seçeneğini açmak için **ShowPeoplePickerSuggestionsForGuestUsers'ı** **Açimaya**ayarlayın. Bu ayarı kiracı düzeyinde veya site toplama düzeyinde açabilirsiniz. Set-SPOTenant ve Set-SPOSite cmdlets kullanarak bu ayarı değiştirebilirsiniz. Bu cmdlets ile, üyeler dizindeki tüm mevcut konuk kullanıcıları arayabilir. Kiracı kapsamındaki değişiklikler, önceden verilmiş olan SharePoint Online sitelerini etkilemez.

### <a name="is-the-csv-upload-feature-still-supported"></a>CSV yükleme özelliği hala destekleniyor mu?
Evet. .csv dosya yükleme özelliğini kullanma hakkında daha fazla bilgi için [bu PowerShell örneğine](code-samples.md)bakın.

### <a name="how-can-i-customize-my-invitation-emails"></a>Davet e-postalarımı nasıl özelleştirebilirim?
[B2B davet API'lerini](customize-invitation-api.md)kullanarak davetli süreci yle ilgili hemen hemen her şeyi özelleştirebilirsiniz.

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Konuk kullanıcılar çok faktörlü kimlik doğrulama yöntemini sıfırlayabilir mi?
Evet. Konuk kullanıcılar, çok faktörlü kimlik doğrulama yöntemini normal kullanıcılarla aynı şekilde sıfırlayabilir.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Çok faktörlü kimlik doğrulama lisanslarından hangi kuruluş sorumludur?
Davet eden kuruluş çok faktörlü kimlik doğrulaması gerçekleştirir. Davet eden kuruluş, kuruluşun çok faktörlü kimlik doğrulaması kullanan B2B kullanıcıları için yeterli lisansa sahip olduğundan emin olmalıdır.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Bir ortak kuruluş zaten çok faktörlü kimlik doğrulaması ayarlanmışsa ne olur? Onların çok faktörlü kimlik doğrulamalarına güvenebilir miyiz ve kendi çok faktörlü kimlik doğrulamamızı kullanmayabilir miyiz?
Bu özellik şu anda desteklenmez. Kuruluşunuzun kaynaklarına erişim çok faktörlü kimlik doğrulaması gerektiriyorsa, ortak kuruluşun (davet eden) kuruluşunuzdaki çok faktörlü kimlik doğrulaması için kaydolması gerekir.

### <a name="how-can-i-use-delayed-invitations"></a>Gecikmeli davetiyeleri nasıl kullanabilirim?
Bir kuruluş B2B işbirliği kullanıcıları eklemek, gerektiğinde uygulamalara sağlamak ve sonra davetiye göndermek isteyebilir. B2B işbirliği daveti API'sini, biniş akışını özelleştirmek için kullanabilirsiniz.

### <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Konuk kullanıcıları Exchange Global Adres Listesi'nde görünür hale getirebilir miyim?
Evet. Konuk nesneler varsayılan olarak kuruluşunuzun genel adres listesinde (GAL) görünmez, ancak bunları görünür kılmak için Azure Active Directory PowerShell'i kullanabilirsiniz. Bkz. [Konuk nesneleri genel adres listesinde görünür hale getirebilir miyim?](https://docs.microsoft.com/office365/admin/create-groups/manage-guest-access-in-groups#add-guests-to-the-global-address-list)

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Konuk kullanıcıyı sınırlı bir yönetici yapabilir miyim?
Kesinlikle. Daha fazla bilgi için [bkz.](add-guest-to-role.md)

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-to-access-the-azure-portal"></a>Azure AD B2B işbirliği B2B kullanıcılarının Azure portalına erişmesine izin veriyor mu?
Bir kullanıcıya sınırlı yönetici rolü atanmadığı sürece, B2B işbirliği kullanıcılarının Azure portalına erişime ihtiyacı olmaz. Ancak, sınırlı yönetici rolü atanan B2B işbirliği kullanıcıları portala erişebilir. Ayrıca, bu yönetici rollerinden birine atanmamış bir konuk kullanıcı portala erişirse, kullanıcı deneyimin belirli bölümlerine erişebilir. Konuk kullanıcı rolünün dizinde bazı izinleri vardır.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Konuk kullanıcılar için Azure portalına erişimi engelleyebilir miyim?

Evet! Tüm konuk ve dış kullanıcıların Azure portalına erişmelerini engelleyen bir Koşullu Erişim ilkesi oluşturabilirsiniz. Bu ilkeyi yapılandırDığınızda, yanlışlıkla üyelere ve yöneticilere erişimi engellemeyi önlemeye dikkat edin.

1. [Azure portalınızda](https://portal.azure.com/) güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
2. Azure portalında **Azure Active Directory** seçeneğini belirleyin. 
3. **Yönet**altında, **Güvenlik'i**seçin.
4. **Koruma**altında, **Koşullu Erişim'i**seçin. **Yeni ilke**yi seçin.
5. **Yeni** sayfada, **Ad** metin kutusuna, poliçe için bir ad girin (örneğin "Konukların portala erişimini engelleyin").
6. **Atamalar** altında **Kullanıcılar ve gruplar**’ı seçin.
7. **Ekle** sekmesinde, **kullanıcıları ve grupları seçin**ve ardından Tüm konuk ve dış **kullanıcıları (Önizleme)** seçin.
9. **Done** (Bitti) öğesini seçin.
10. **Yeni** sayfada, **Atamalar** bölümünde Bulut **uygulamalarını veya eylemlerini**seçin.
11. Bulut **uygulamaları veya eylemleri** **sayfasında, uygulamaları seç'i**seçin ve ardından **Seç'i**seçin.
12. **Seç** sayfasında **Microsoft Azure Management**’ı ve sonra **Seç** öğesini seçin.
13. Bulut **uygulamaları veya eylemleri** sayfasında **Bitti'yi**seçin.

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Azure AD B2B işbirliği çok faktörlü kimlik doğrulamayı ve tüketici e-posta hesaplarını destekliyor mu?
Evet. Azure AD B2B işbirliği için çok faktörlü kimlik doğrulama ve tüketici e-posta hesapları desteklenir.

### <a name="do-you-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Azure AD B2B işbirliği kullanıcıları için parola sıfırlamayı destekliyor musunuz?
Azure AD kiracınız bir kullanıcının ev diziniyse, [kullanıcının parolasını](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-reset-password-azure-portal) Azure portalından sıfırlayabilirsiniz. Ancak, başka bir Azure REKLAM dizini veya harici kimlik sağlayıcısı tarafından yönetilen bir hesapla birlikte kaydolan konuk kullanıcıiçin parolayı doğrudan sıfırlayamaz. Parolayı yalnızca konuk kullanıcı veya kullanıcının ev dizinindeki bir yönetici sıfırlayabilir. Parola sıfırlamanın konuk kullanıcılar için nasıl çalıştığına dair bazı örnekler aşağıda verilmiştir:
 
* Bir Microsoft hesabıyla oturum açan konuk guestuser@live.comkullanıcılar (örneğin), Microsoft hesabı self servis parola sıfırlama (SSPR) kullanarak kendi parolalarını sıfırlayabilirler. Bkz. [Microsoft hesap parolanızı nasıl sıfırlarsınız.](https://support.microsoft.com/help/4026971/microsoft-account-how-to-reset-your-password)
* Bir Google hesabı veya başka bir harici kimlik sağlayıcısıyla oturum açan konuk kullanıcılar, kimlik sağlayıcılarının SSPR yöntemini kullanarak kendi parolalarını sıfırlayabilir. Örneğin, Google hesabı guestuser@gmail.com olan bir konuk kullanıcı Değiştir'deki yönergeleri izleyerek parolasını sıfırlayabilir veya [parolanızı sıfırlayabilir.](https://support.google.com/accounts/answer/41078)
* Kimlik kiracısı tam zamanında (JIT) veya "viral" bir kiracıysa (ayrı, yönetilmeyen bir Azure kiracısısa), parolasını yalnızca konuk kullanıcı sıyredebilir. Bazen bir kuruluş, çalışanlar hizmetlere kaydolmak için iş e-posta adreslerini kullandıklarında oluşturulan [viral kiracıların yönetimini devralır.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover) Kuruluş viral bir kiracıyı devraldıktan sonra, yalnızca bu kuruluştaki bir yönetici kullanıcının parolasını sıfırladı veya SSPR'yi etkinleştirebilir. Gerekirse, davet eden kuruluş olarak konuk kullanıcı hesabını dizininizden kaldırabilir ve davetiyeyi yeniden gönderebilirsiniz.

* Konuk kullanıcının ev dizini Azure AD kiracınızsa, kullanıcının parolasını sıfırlayabilirsiniz. Örneğin, şirket içi Active Dizininiz'den bir kullanıcı oluşturmuş veya bir kullanıcıyı senkronize etmiş ve UserType'ını Konuk olarak ayarlamış olabilirsiniz. Bu kullanıcı dizininizde yer aldığı için, parolalarını Azure portalından sıfırlayabilirsiniz.

### <a name="does-microsoft-dynamics-365-provide-online-support-for-azure-ad-b2b-collaboration"></a>Microsoft Dynamics 365, Azure AD B2B işbirliği için çevrimiçi destek sağlıyor mu?
Evet, Dynamics 365 (çevrimiçi) Azure AD B2B işbirliğini destekler. Daha fazla bilgi için Dynamics 365 makalesini Azure [AD B2B işbirliği ne kadar](https://docs.microsoft.com/dynamics365/customer-engagement/admin/invite-users-azure-active-directory-b2b-collaboration)çok sayıda kullanıcıya davet edin.

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Yeni oluşturulan B2B işbirliği kullanıcısı için ilk parolanın ömrü nedir?
Azure AD, tüm Azure AD bulut u kullanıcı hesapları için eşit olarak geçerli olan sabit bir karakter kümesine, parola gücüne ve hesap kilitleme gereksinimlerine sahiptir. Bulut kullanıcı hesapları, başka bir kimlik sağlayıcısıyla birlikte federated olmayan hesaplardır, örneğin 
* Microsoft hesabı
* Facebook
* Active Directory Federasyon Hizmetleri
* Başka bir bulut kiracı (B2B işbirliği için)

Federe hesaplar için parola ilkesi, şirket içi kirada uygulanan ilkeve kullanıcının Microsoft hesap ayarlarına bağlıdır.

### <a name="an-organization-might-want-to-have-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-correct-model-to-use"></a>Bir kuruluş, kiracı kullanıcılar ve konuk kullanıcılar için uygulamalarında farklı deneyimler esahip olmak isteyebilir. Bunun için standart bir kılavuz var mı? Kimlik sağlayıcısının varlığı kullanılacak doğru modeli talep ediyor mu?
Konuk kullanıcı kimlik doğrulaması için herhangi bir kimlik sağlayıcısını kullanabilir. Daha fazla bilgi için [b2B işbirliği kullanıcısının özellikleri](user-properties.md)bölümüne bakın. Kullanıcı deneyimini belirlemek için **UserType** özelliğini kullanın. **UserType** talebi şu anda belirteç te dahil değildir. Uygulamalar, kullanıcının dizinini sorgulamak ve UserType'ı almak için Microsoft Graph API'sini kullanmalıdır.

### <a name="where-can-i-find-a-b2b-collaboration-community-to-share-solutions-and-to-submit-ideas"></a>Çözümleri paylaşmak ve fikir göndermek için b2B işbirliği topluluğunu nerede bulabilirim?
B2B işbirliğini geliştirmek için görüşlerinizi sürekli dinliyoruz. Lütfen kullanıcı senaryolarınızı, en iyi uygulamaları nızı ve Azure AD B2B işbirliği hakkında neleri beğendiğinizi paylaşın. [Microsoft Tech Community'deki](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b)tartışmaya katılın.
 
Ayrıca fikirlerinizi sunmaya ve [B2B İşbirliği Fikirleri'nde](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas)gelecekteki özellikler için oy vermeye davet ediyoruz.

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-the-user-is-just-ready-to-go-or-does-the-user-always-have-to-click-through-to-the-redemption-url"></a>Kullanıcının sadece "kullanıma hazır" olması için otomatik olarak kullanılan bir davetiye gönderebilir miyiz? Yoksa kullanıcı her zaman kullanım URL'sine tıklamak zorunda mıdır?
Kullanıcı UI, PowerShell komut dosyaları veya API'leri kullanarak iş ortağı kuruluşundaki diğer kullanıcıları davet edebilirsiniz. Daha sonra konuk kullanıcıya paylaşılan bir uygulamaya doğrudan bağlantı gönderebilirsiniz. Çoğu durumda, artık e-posta davetini açıp bir ödeme URL'sini tıklatma nız gerekmez. Bkz. [Azure Active Directory B2B işbirliği daveti kullanımı.](redemption-experience.md)

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>B2B işbirliği, davet edilen ortak kendi şirket içi kimlik doğrulamasını eklemek için federasyonu kullanırken nasıl çalışır?
İş ortağının şirket içi kimlik doğrulama altyapısına bıkmış bir Azure AD kiracısı varsa, şirket içi tek oturum açma (SSO) otomatik olarak elde edilir. İş ortağının Azure AD kiracısı yoksa, yeni kullanıcılar için bir Azure AD hesabı oluşturulur. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>Azure AD B2B'nin gmail.com ve outlook.com e-posta adreslerini kabul etmediğini ve B2C'nin bu tür hesaplar için kullanıldığını sanıyordum.
B2B ile işletmelerarası (B2C) işbirliği arasındaki kimliklerin desteklenmesi açısından arasındaki farkları ortadan kaldırıyoruz. Kullanılan kimlik B2B kullanarak veya B2C kullanarak arasında seçim yapmak için iyi bir neden değildir. İşbirliği seçeneğinizi seçme hakkında daha fazla bilgi için [Azure Active Directory'de B2B işbirliğini ve B2C'yi karşılaştır'a](compare-with-b2c.md)bakın.

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Hangi uygulamalar ve hizmetler Azure B2B konuk kullanıcılarını destekler?
Azure AD tümleşik tümuygulamaları Azure B2B konuk kullanıcılarını destekleyebilir, ancak konuk kullanıcıların kimliğini doğrulamak için kiracı olarak ayarlanmış bir bitiş noktası kullanmaları gerekir. Ayrıca, bir konuk kullanıcı uygulamaya kimlik doğruladığında verilen SAML belirtecindeki [talepleri özelleştirmeniz](claims-mapping.md) gerekebilir. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>Ortaklarımızın çok faktörlü kimlik doğrulaması yoksa B2B konuk kullanıcıları için çok faktörlü kimlik doğrulaması zorlayabilir miyiz?
Evet. Daha fazla bilgi [için B2B işbirliği kullanıcıları için Koşullu Erişim'e](conditional-access.md)bakın.

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>SharePoint'te, dış kullanıcılar için "izin ver" veya "reddet" listesi tanımlayabilirsiniz. Bunu Azure'da yapabilir miyiz?
Evet. Azure AD B2B işbirliği, listelere izin verme ve listeleri reddetmeyi destekler. 

### <a name="what-licenses-do-we-need-to-use-azure-ad-b2b"></a>Azure AD B2B'yi kullanmak için hangi lisanslara ihtiyacımız var?
Kuruluşunuzun Azure AD B2B'yi kullanması için hangi lisanslar alması gerektiği hakkında bilgi için [Azure Active Directory B2B işbirliği lisanslama kılavuzuna](licensing-guidance.md)bakın.

### <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)

