---
title: Azure Active Directory için SMS tabanlı kullanıcı oturum açma
description: Kullanıcıların SMS kullanarak Azure Active Directory oturum açmasını nasıl yapılandıracağınızı ve etkinleştireceğinizi öğrenin (Önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rateller
ms.collection: M365-identity-device-management
ms.openlocfilehash: 517ff16cd1cbf9bfe31404d3b1b3993a02d6dae9
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871563"
---
# <a name="configure-and-enable-users-for-sms-based-authentication-using-azure-active-directory-preview"></a>Azure Active Directory kullanarak SMS tabanlı kimlik doğrulaması için kullanıcıları yapılandırma ve etkinleştirme (Önizleme)

Kullanıcıların uygulama ve hizmetlerde oturum açmalarına yönelik karmaşıklık ve güvenlik risklerini azaltmak için, Azure Active Directory (Azure AD) birden çok kimlik doğrulama seçeneği sağlar. Şu anda önizleme aşamasında olan SMS tabanlı kimlik doğrulaması, kullanıcıların kullanıcı adını ve parolasını sağlamasına gerek kalmadan oturum açmasını sağlar. Hesabı bir kimlik yöneticisi tarafından oluşturulduktan sonra, oturum açma isteminde telefon numaralarını girebilir ve SMS mesajı aracılığıyla bunlara gönderilen bir kimlik doğrulama kodu sağlayabilirsiniz. Bu kimlik doğrulama yöntemi, özellikle ön hat çalışanları için uygulamalara ve hizmetlere erişimi basitleştirir.

Bu makalede, Azure AD 'de Kullanıcı veya grup seçme için SMS tabanlı kimlik doğrulamanın nasıl etkinleştirileceği gösterilmektedir.

|     |
| --- |
| Kullanıcılar için SMS tabanlı kimlik doğrulaması, Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlayabilmeniz için aşağıdaki kaynaklar ve ayrıcalıklar gereklidir:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Aboneliğinizle ilişkili bir Azure Active Directory kiracısı.
    * Gerekirse, [bir Azure Active Directory kiracı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin][associate-azure-ad-tenant].
* SMS tabanlı kimlik doğrulamasını etkinleştirmek için Azure AD kiracınızda *genel yönetici* ayrıcalıklarına sahip olmanız gerekir.
* SMS mesajı kimlik doğrulama yöntemi ilkesinde etkin olan her bir kullanıcının, kullanmasa bile lisanslanması gerekir. Her etkin kullanıcı aşağıdaki Azure AD, Office 365 veya Microsoft 365 lisanslarından birine sahip olmalıdır:
    * [Azure AD Premium P1 veya P2][azuread-licensing]
    * [Microsoft 365 (M365) F1 veya F3][m365-firstline-workers-licensing] ya da [Office 365 F1][o365-f1] veya [F3][o365-f3]
    * [Enterprise Mobility + Security (EMS) E3 veya E5][ems-licensing] ya da [Microsoft 365 (M365) E3 veya E5][m365-licensing]

## <a name="limitations"></a>Sınırlamalar

SMS tabanlı kimlik doğrulamasının genel önizlemesi sırasında, aşağıdaki sınırlamalar geçerlidir:

* SMS tabanlı kimlik doğrulaması şu anda Azure Multi-Factor Authentication ile uyumlu değildir.
* Takımlar dışında, SMS tabanlı kimlik doğrulaması şu anda yerel Office uygulamalarıyla uyumlu değildir.
* B2B hesapları için SMS tabanlı kimlik doğrulaması önerilmez.
* Federasyon kullanıcıları, ana kiracıda kimlik doğrulaması yapamaz. Bunlar yalnızca Bulutta kimlik doğrular.

## <a name="enable-the-sms-based-authentication-method"></a>SMS tabanlı kimlik doğrulama yöntemini etkinleştirme

Kuruluşunuzda SMS tabanlı kimlik doğrulamasını etkinleştirmek ve kullanmak için üç ana adım vardır:

* Kimlik doğrulama yöntemi ilkesini etkinleştirin.
* SMS tabanlı kimlik doğrulama yöntemini kullanılabilecek kullanıcıları veya grupları seçin.
* Her Kullanıcı hesabı için bir telefon numarası atayın.
    * Bu telefon numarası Azure portal (Bu makalede gösterilen) ve *personelimde* veya *profilimde*atanabilir.

İlk olarak, Azure AD kiracınız için SMS tabanlı kimlik doğrulamasını etkinleştirelim.

1. [Azure Portal][azure-portal] *genel yönetici*olarak oturum açın.
1. **Azure Active Directory**'yi bulun ve seçin.
1. Azure Active Directory penceresinin sol tarafındaki gezinti menüsünde **güvenlik > kimlik doğrulama yöntemleri > kimlik doğrulama yöntemi ilkesi (Önizleme)** seçeneğini belirleyin.

    [![](media/howto-authentication-sms-signin/authentication-method-policy-cropped.png "Browse to and select the Authentication method policy (preview) window in the Azure portal")](media/howto-authentication-sms-signin/authentication-method-policy.png#lightbox)

1. Kullanılabilir kimlik doğrulama yöntemleri listesinden **kısa mesaj**' ı seçin.
1. **Enable** ' i *Evet*olarak ayarlayın.

    ![Kimlik doğrulama yöntemi ilke penceresinde metin kimlik doğrulamasını etkinleştir](./media/howto-authentication-sms-signin/enable-text-authentication-method.png)

    SMS tabanlı kimlik doğrulamasını *tüm kullanıcılar* için etkinleştirmeyi veya Kullanıcı ve grup *seçmenizi* seçebilirsiniz. Bir sonraki bölümde, bir test kullanıcısı için SMS tabanlı kimlik doğrulamasını etkinleştirirsiniz.

## <a name="assign-the-authentication-method-to-users-and-groups"></a>Kimlik doğrulama yöntemini kullanıcılara ve gruplara atama

Azure AD kiracınızda SMS tabanlı kimlik doğrulaması etkinken, bu kimlik doğrulama yöntemini kullanmasına izin verilecek bazı kullanıcıları veya grupları seçin.

1. SMS mesajı kimlik doğrulama İlkesi penceresinde, **hedefi** *Kullanıcı Seç*olarak ayarlayın.
1. **Kullanıcı veya grup eklemeyi**seçin, sonra *contoso KULLANıCıSı* veya *contoso SMS kullanıcıları*gibi bir test kullanıcısı veya grubu seçin.

    [![](media/howto-authentication-sms-signin/add-users-or-groups-cropped.png "Choose users or groups to enable for SMS-based authentication in the Azure portal")](media/howto-authentication-sms-signin/add-users-or-groups.png#lightbox)

1. Kullanıcılarınızı veya gruplarınızı seçtiğinizde, **Seç**' i seçin ve ardından güncelleştirilmiş kimlik doğrulama yöntemi ilkesini **kaydedin** .

SMS mesajı kimlik doğrulama yöntemi ilkesinde etkin olan her bir kullanıcının, kullanmasa bile lisanslanması gerekir. Özellikle büyük Kullanıcı grupları için özelliği etkinleştirdiğinizde, kimlik doğrulama yöntemi ilkesinde etkinleştirdiğiniz kullanıcılar için uygun lisanslara sahip olduğunuzdan emin olun.

## <a name="set-a-phone-number-for-user-accounts"></a>Kullanıcı hesapları için telefon numarası ayarlama

Kullanıcılar artık SMS tabanlı kimlik doğrulaması için etkinleştirilmiştir, ancak oturum açabilmeniz için telefon numarasının Azure AD 'deki Kullanıcı profiliyle ilişkilendirilmesi gerekir. Kullanıcı [Bu telefon numarasını](../user-help/sms-sign-in-explainer.md) *profilimde*kendileri ayarlayabilir veya Azure Portal kullanarak telefon numarasını atayabilirsiniz. Telefon numaraları *Genel Yöneticiler*, *kimlik doğrulama yöneticileri*veya *ayrıcalıklı kimlik doğrulama yöneticileri*tarafından ayarlanabilir.

SMS-Sign için bir telefon numarası ayarlandığında ayrıca [Azure Multi-Factor Authentication][tutorial-azure-mfa] ve [self servis parola sıfırlama][tutorial-sspr]ile birlikte kullanılabilir.

1. **Azure Active Directory**'yi bulun ve seçin.
1. Azure Active Directory penceresinin sol tarafındaki gezinti menüsünde **Kullanıcılar**' ı seçin.
1. Önceki bölümde bulunan *contoso kullanıcısı*gibi SMS tabanlı kimlik doğrulaması için etkinleştirdiğiniz kullanıcıyı seçin ve ardından **kimlik doğrulama yöntemleri**' ni seçin.
1. Ülke kodu da dahil olmak üzere kullanıcının telefon numarasını girin ( *+ 1 xxxxxxxxx*gibi). Azure portal telefon numarasının doğru biçimde olduğunu doğrular.

    ![SMS tabanlı kimlik doğrulamasıyla kullanmak üzere Azure portal Kullanıcı için telefon numarası ayarlama](./media/howto-authentication-sms-signin/set-user-phone-number.png)

    Telefon numarası kiracınızda benzersiz olmalıdır. Birden çok kullanıcı için aynı telefon numarasını kullanmayı denerseniz, bir hata iletisi gösterilir.

1. Telefon numarasını bir kullanıcının hesabına uygulamak için **Kaydet**' i seçin.

Başarılı bir şekilde sağlandığında, *SMS oturum açma özelliğinin etkin olduğu*bir onay işareti görünür.

## <a name="test-sms-based-sign-in"></a>SMS tabanlı oturum açmayı sına

SMS tabanlı oturum açma için etkin durumda olan kullanıcı hesabını test etmek için aşağıdaki adımları izleyin:

1. Yeni bir InPrivate veya ınbilito Web tarayıcı penceresi açın[https://www.office.com][office]
1. Sağ üst köşede **oturum aç**' ı seçin.
1. Oturum açma isteminde, önceki bölümde kullanıcıyla ilişkili telefon numarasını girin ve ardından **İleri**' yi seçin.

    ![Test kullanıcısı için oturum açma isteminde bir telefon numarası girin](./media/howto-authentication-sms-signin/sign-in-with-phone-number.png)

1. Girilen telefon numarasına bir SMS mesajı gönderilir. Oturum açma işlemini gerçekleştirmek için, oturum açma isteminde kısa mesajda sunulan 6 basamaklı kodu girin.

    ![Kullanıcı telefon numarasına SMS mesajı aracılığıyla gönderilen onay kodunu girin](./media/howto-authentication-sms-signin/sign-in-with-phone-number-confirmation-code.png)

1. Kullanıcı, Kullanıcı adı veya parola sağlamaya gerek kalmadan oturum açmıştır.

## <a name="troubleshoot-sms-based-sign-in"></a>SMS tabanlı oturum açma sorunlarını giderme

SMS tabanlı oturum açma ve kullanmayla ilgili sorun yaşıyorsanız, aşağıdaki senaryolar ve sorun giderme adımları kullanılabilir.

### <a name="phone-number-already-set-for-a-user-account"></a>Bir kullanıcı hesabı için zaten ayarlanmış telefon numarası

Bir Kullanıcı Azure Multi-Factor Authentication ve/veya self servis parola sıfırlama (SSPR) için zaten kaydedilmişse, kendi hesabıyla ilişkili bir telefon numarası zaten vardır. Bu telefon numarası SMS tabanlı oturum açma ile birlikte kullanılamaz.

Hesabı için zaten ayarlanmış telefon numarası olan bir Kullanıcı, kendi **Profilim** sayfasında *SMS oturum açma özelliğini etkinleştirmek* üzere bir düğme görüntülenir. Bu düğmeyi seçtiğinizde, hesap SMS tabanlı oturum açma ve önceki Azure Multi-Factor Authentication veya SSPR kaydı ile kullanım için etkinleştirilmiştir.

Son Kullanıcı deneyimi hakkında daha fazla bilgi için bkz. [telefon numarası (Önizleme) Için SMS oturum açma kullanıcı deneyimi](../user-help/sms-sign-in-explainer.md).

### <a name="error-when-trying-to-set-a-phone-number-on-a-users-account"></a>Kullanıcının hesabında telefon numarası ayarlanmaya çalışılırken hata oluştu

Azure portal bir kullanıcı hesabı için telefon numarası ayarlamaya çalıştığınızda bir hata alırsanız, aşağıdaki sorun giderme adımlarını gözden geçirin:

1. SMS tabanlı oturum açma önizlemesi için etkin olduğunuzdan emin olun.
1. *SMS mesajı* kimlik doğrulama yöntemi ilkesinde Kullanıcı hesabının etkinleştirildiğini doğrulayın.
1. Telefon numarasını Azure portal ( *+ 1 4251234567*gibi) doğrulanan şekilde doğru biçimlendirmeyle ayarladığınızdan emin olun.
1. Telefon numarasının kiracınızda başka bir yerde kullanılmadığından emin olun.
1. Hesapta bir ses numarası ayarlanmamışsa emin olun. Bir ses numarası ayarlandıysa, silin ve telefon numarasını yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD 'de Microsoft Authenticator App veya FIDO2 güvenlik anahtarları gibi bir parola olmadan oturum açma konusunda ek yollar için bkz. [Azure AD Için passwordless kimlik doğrulama seçenekleri][concepts-passwordless].

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../fundamentals/active-directory-how-subscriptions-associated-directory.md
[concepts-passwordless]: concept-authentication-passwordless.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[tutorial-sspr]: tutorial-enable-sspr.md

<!-- EXTERNAL LINKS -->
[azure-portal]: https://portal.azure.com
[office]: https://www.office.com
[m365-firstline-workers-licensing]: https://www.microsoft.com/licensing/news/m365-firstline-workers
[azuread-licensing]: https://azure.microsoft.com/pricing/details/active-directory/
[ems-licensing]: https://www.microsoft.com/microsoft-365/enterprise-mobility-security/compare-plans-and-pricing
[m365-licensing]: https://www.microsoft.com/microsoft-365/compare-microsoft-365-enterprise-plans
[o365-f1]: https://www.microsoft.com/microsoft-365/business/office-365-f1?market=af
[o365-f3]: https://www.microsoft.com/microsoft-365/business/office-365-f3?activetab=pivot%3aoverviewtab
