---
title: Azure Active Directory için SMS tabanlı kullanıcı oturum açma
description: SMS kullanarak kullanıcıların Azure Active Directory'de oturum açmalarını nasıl sağlayacağınızı ve bunları nasıl etkinleştireceklerinizi öğrenin (önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rateller
ms.collection: M365-identity-device-management
ms.openlocfilehash: 845948d9aec28ee79a11fb11aaef4cfbf1b263fa
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770561"
---
# <a name="configure-and-enable-users-for-sms-based-authentication-using-azure-active-directory-preview"></a>Azure Active Directory (önizleme) kullanarak sms tabanlı kimlik doğrulaması için kullanıcıları yapılandırma ve etkinleştirme

Kullanıcıların uygulamalarda ve hizmetlerde oturum açmalarının karmaşıklığını ve güvenlik risklerini azaltmak için Azure Active Directory (Azure AD) birden çok kimlik doğrulama seçeneği sunar. Şu anda önizlemede olan SMS tabanlı kimlik doğrulama, kullanıcıların kullanıcı adlarını ve parolalarını sağlamaya veya bilmeye gerek kalmadan oturum açmalarına olanak tanır. Hesapları bir kimlik yöneticisi tarafından oluşturulduktan sonra, oturum açma istemine telefon numarasını girebilir ve kısa mesaj la kendilerine gönderilen bir kimlik doğrulama kodu sağlayabilirler. Bu kimlik doğrulama yöntemi, özellikle ön cephe çalışanları için uygulamalara ve hizmetlere erişimi kolaylaştırır.

Bu makalede, Azure AD'deki belirli kullanıcılar veya gruplar için SMS tabanlı kimlik doğrulamayı nasıl etkinleştirdiğiniz gösterilmektedir.

|     |
| --- |
| Kullanıcılar için SMS tabanlı kimlik doğrulaması, Azure Active Directory'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bakın|
|     |

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* SMS tabanlı kimlik doğrulamayı etkinleştirmek için Azure AD kiracınızda *genel yönetici* ayrıcalıklarına ihtiyacınız vardır.
* Metin iletisi kimlik doğrulama yöntemi ilkesinde etkinleştirilen her kullanıcı, kullanmasalar bile lisanslanmalıdır. Etkinleştirilen her kullanıcı aşağıdaki Azure AD veya Microsoft 365 lisanslarından birine sahip olmalıdır:
    * [Azure AD Premium P1 veya P2][azuread-licensing]
    * [Microsoft 365 (M365) F1 veya F3][m365-firstline-workers-licensing]
    * [Kurumsal Mobilite + Güvenlik (EMS) E3 veya E5][ems-licensing] veya [Microsoft 365 (M365) E3 veya E5][m365-licensing]

## <a name="limitations"></a>Sınırlamalar

SMS tabanlı kimlik doğrulamasının genel önizlemesi sırasında aşağıdaki sınırlamalar geçerlidir:

* SMS tabanlı kimlik doğrulama, şu anda Azure Çok Faktörlü Kimlik Doğrulaması ile uyumlu değildir.
* Takımlar dışında, SMS tabanlı kimlik doğrulama şu anda yerel Office uygulamalarıyla uyumlu değildir.
* B2B hesapları için SMS tabanlı kimlik doğrulama önerilmez.
* Federe kullanıcılar ev kiracı doğrulaması olmaz. Sadece bulutta doğrularlar.

## <a name="enable-the-sms-based-authentication-method"></a>SMS tabanlı kimlik doğrulama yöntemini etkinleştirme

Kuruluşunuzdaki SMS tabanlı kimlik doğrulamasını etkinleştirmek ve kullanmak için üç ana adım vardır:

* Kimlik doğrulama yöntemi ilkesini etkinleştirin.
* SMS tabanlı kimlik doğrulama yöntemini kullanabilen kullanıcıları veya grupları seçin.
* Her kullanıcı hesabı için bir telefon numarası atayın.
    * Bu telefon numarası Azure portalına (bu makalede gösterilen) ve Personelim veya *Profilim'de* atanabilir. *My Profile*

İlk olarak, Azure AD kiracınız için SMS tabanlı kimlik doğrulamasını etkinleştirelim.

1. Azure [portalında][azure-portal] *genel yönetici*olarak oturum açın.
1. **Azure Active Directory**'yi bulun ve seçin.
1. Azure Etkin Dizin penceresinin sol tarafındaki gezinti menüsünden, **Kimlik Doğrulama yöntemi (önizleme) > Güvenlik > Kimlik Doğrulama yöntemlerini**seçin.

    [![](media/howto-authentication-sms-signin/authentication-method-policy-cropped.png "Browse to and select the Authentication method policy (preview) window in the Azure portal")](media/howto-authentication-sms-signin/authentication-method-policy.png#lightbox)

1. Kullanılabilir kimlik doğrulama yöntemleri listesinden **Metin iletisini**seçin.
1. **Etkinleştir'i** *Evet'e*Ayarla.

    ![Kimlik doğrulama yöntemi ilkesi penceresinde metin kimlik doğrulamasını etkinleştirme](./media/howto-authentication-sms-signin/enable-text-authentication-method.png)

    *Tüm kullanıcılar* veya *Select* kullanıcıları ve grupları için SMS tabanlı kimlik doğrulamasını etkinleştirmeyi seçebilirsiniz. Sonraki bölümde, bir test kullanıcısı için SMS tabanlı kimlik doğrulamasını etkinleştirirsiniz.

## <a name="assign-the-authentication-method-to-users-and-groups"></a>Kimlik doğrulama yöntemini kullanıcılara ve gruplara atama

Azure AD kiracınızda SMS tabanlı kimlik doğrulaması etkinleştirildiğinde, artık bu kimlik doğrulama yöntemini kullanmasına izin verilecek bazı kullanıcıları veya grupları seçin.

1. Metin iletikimlik doğrulama ilkesi penceresinde, **Hedef'i** *kullanıcıları seçmek*için ayarlayın.
1. Kullanıcı **veya grup eklemeyi**seçin, ardından *Contoso User* veya *Contoso SMS Kullanıcıları*gibi bir test kullanıcısı veya grubu seçin.

    [![](media/howto-authentication-sms-signin/add-users-or-groups-cropped.png "Choose users or groups to enable for SMS-based authentication in the Azure portal")](media/howto-authentication-sms-signin/add-users-or-groups.png#lightbox)

1. Kullanıcılarınızı veya gruplarınızı seçtiğinizde, **güncelleştirilmiş**kimlik doğrulama yöntemi ilkesini **kaydet** seçeneğini belirleyin.

Metin iletisi kimlik doğrulama yöntemi ilkesinde etkinleştirilen her kullanıcı, kullanmasalar bile lisanslanmalıdır. Kimlik doğrulama yöntemi ilkesinde etkinleştirdiğiniz kullanıcılar için uygun lisanslara sahip olduğunuzdan emin olun, özellikle de özelliği büyük kullanıcı grupları için etkinleştirdiğinizde.

## <a name="set-a-phone-number-for-user-accounts"></a>Kullanıcı hesapları için bir telefon numarası ayarlama

Kullanıcılar artık SMS tabanlı kimlik doğrulaması için etkinleştirilir, ancak oturum açabilmeleri için telefon numaralarının Azure AD'deki kullanıcı profiliyle ilişkilendirilmesi gerekir. Kullanıcı bu telefon numarasını *Profilim'de* [kendileri ayarlayabilir](../user-help/sms-sign-in-explainer.md) veya Azure portalını kullanarak telefon numarasını atayabilirsiniz. Telefon numaraları global *yöneticiler,* *kimlik doğrulama yöneticileri*veya ayrıcalıklı kimlik doğrulama *yöneticileri*tarafından ayarlanabilir.

Sms işareti için bir telefon numarası ayarlandığında, Azure Çok [Faktörlü Kimlik Doğrulama][tutorial-azure-mfa] ve [self servis parola sıfırlama][tutorial-sspr]ile de kullanılabilir.

1. **Azure Active Directory**'yi bulun ve seçin.
1. Azure Etkin Dizin penceresinin sol tarafındaki gezinti menüsünden **Kullanıcılar'ı**seçin.
1. *Contoso User*gibi önceki bölümde SMS tabanlı kimlik doğrulama için etkinleştirdiğiniz kullanıcıyı seçin, ardından **Kimlik Doğrulama yöntemlerini**seçin.
1. *+1 xxxxxxxxx*gibi ülke kodu da dahil olmak üzere kullanıcının telefon numarasını girin. Azure portalı telefon numarasının doğru biçimde olduğunu doğrular.

    ![Azure portalındaki bir kullanıcının SMS tabanlı kimlik doğrulamayla kullanması için bir telefon numarası ayarlama](./media/howto-authentication-sms-signin/set-user-phone-number.png)

    Telefon numarası kiracınızda benzersiz olmalıdır. Birden çok kullanıcı için aynı telefon numarasını kullanmaya çalışırsanız, bir hata iletisi gösterilir.

1. Telefon numarasını kullanıcının hesabına uygulamak için **Kaydet'i**seçin.

Başarılı bir şekilde sağlandığında, SMS *Oturum Açma etkinliği*için bir onay işareti görüntülenir.

## <a name="test-sms-based-sign-in"></a>SMS tabanlı oturum açma test

SMS tabanlı oturum açma için etkinleştirilen kullanıcı hesabını sınamak için aşağıdaki adımları tamamlayın:

1. Yeni bir InPrivate veya Incognito web tarayıcısı penceresi aç[https://www.office.com][office]
1. Sağ üst köşede **Oturum Aç'ı**seçin.
1. Oturum açma isteminde, önceki bölümde kullanıcıyla ilişkili telefon numarasını girin ve **ardından İleri'yi**seçin.

    ![Test kullanıcısı için oturum açma istemine bir telefon numarası girin](./media/howto-authentication-sms-signin/sign-in-with-phone-number.png)

1. Sağlanan telefon numarasına kısa mesaj gönderilir. Oturum açma işlemini tamamlamak için, oturum açma istemine metin iletisinde sağlanan 6 haneli kodu girin.

    ![Kısa mesajla gönderilen onay kodunu kullanıcının telefon numarasına girin](./media/howto-authentication-sms-signin/sign-in-with-phone-number-confirmation-code.png)

1. Kullanıcı artık bir kullanıcı adı veya parola sağlamaya gerek kalmadan oturum açmış durumda.

## <a name="troubleshoot-sms-based-sign-in"></a>Sorun giderme SMS tabanlı oturum açma

SMS tabanlı oturum açmayı etkinleştirme ve kullanmada sorun yaşıyorsanız, aşağıdaki senaryolar ve sorun giderme adımları kullanılabilir.

### <a name="phone-number-already-set-for-a-user-account"></a>Bir kullanıcı hesabı için ayarlanmış telefon numarası

Bir kullanıcı Azure Çok Faktörlü Kimlik Doğrulama ve/veya self servis parola sıfırlama (SSPR) için zaten kaydolmuşsa, hesabıyla ilişkili bir telefon numarasına sahiptir. Bu telefon numarası SMS tabanlı oturum açma ile otomatik olarak kullanılamaz.

Hesabı için ayarlanmış bir telefon numarasına sahip olan bir kullanıcı, **Profilim** sayfasında *SMS oturum açmayı etkinleştirmek için* bir düğme görüntülenir. Bu düğmeyi seçin ve hesap SMS tabanlı oturum açma ve önceki Azure Çok Faktörlü Kimlik Doğrulama veya SSPR kaydıyla kullanım için etkinleştirilir.

Son kullanıcı deneyimi hakkında daha fazla bilgi için [telefon numarası (önizleme) için SMS oturum açma kullanıcı deneyimine](../user-help/sms-sign-in-explainer.md)bakın.

### <a name="error-when-trying-to-set-a-phone-number-on-a-users-account"></a>Bir kullanıcının hesabına telefon numarası ayarlamaya çalışırken hata

Azure portalındaki bir kullanıcı hesabı için telefon numarası ayarlamaya çalıştığınızda bir hata alırsanız, aşağıdaki sorun giderme adımlarını gözden geçirin:

1. SMS tabanlı oturum açma önizlemesi için etkinleştirildiğinden emin olun.
1. *Kısa mesaj* kimlik doğrulama yöntemi ilkesinde kullanıcı hesabının etkin olduğunu doğrulayın.
1. Telefon numarasını Azure portalında doğrulanan *(+1 4251234567*gibi) uygun biçimlendirmeyle ayarladığınızdan emin olun.
1. Telefon numarasının kiracınızın başka bir yerinde kullanılmadığından emin olun.
1. Hesapta ses numarası olmadığını kontrol et. Bir ses numarası ayarlanırsa, silin ve telefon numarasını yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Authenticator App veya FIDO2 güvenlik anahtarları gibi parola olmadan Azure AD'de oturum açmanın ek yolları [için Azure AD için Parolasız kimlik doğrulama seçeneklerine][concepts-passwordless]bakın.

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
