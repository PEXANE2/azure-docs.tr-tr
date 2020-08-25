---
title: Kullanıcı Yönetimi (Önizleme) için personelimi kullanma-Azure AD | Microsoft Docs
description: Personel ve yönetim birimlerimi kullanarak Kullanıcı yönetimi temsilcisi seçme
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.topic: how-to
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.date: 05/08/2020
ms.author: curtand
ms.reviewer: sahenry
ms.custom: oldportal;it-pro;
ms.openlocfilehash: da358ecb126dda21d18120bbe698fec234c85e4f
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88798967"
---
# <a name="manage-your-users-with-my-staff-preview"></a>Personelim ile kullanıcılarınızı yönetme (Önizleme)

Personeliniz, bir mağaza yöneticisi veya bir ekip lideri gibi yetkili bir şekilde temsilci seçmenize olanak sağlar. bu sayede, personel üyelerinin Azure AD hesaplarına erişebildiğinden emin olabilirsiniz. Kuruluşlar, merkezi bir yardım masasına güvenmek yerine, parolaları sıfırlama veya telefon numaralarını bir Team Manager 'a değiştirme gibi genel görevleri temsilcilibilirler. Personeli sayesinde, hesaplarına erişemesiz bir Kullanıcı, yardım masası veya BT personeli gerekmeden yalnızca birkaç tıklamayla erişim elde edebilir.

Kurumumu kuruluşunuz için yapılandırmadan önce bu belgenin yanı sıra bu özelliğin kullanıcılarınıza yönelik işlevselliğini ve etkisini anladığınızdan emin olmak için bu belgeleri ve [Kullanıcı belgelerini](../user-help/my-staff-team-manager.md) incelemenizi öneririz. Kullanıcıları yeni deneyimle eğitmeniz ve hazırlamak için Kullanıcı belgelerinden yararlanabilir ve başarılı bir dağıtım sağlanmasına yardımcı olabilirsiniz.

Kullanıcılar için SMS tabanlı kimlik doğrulaması, Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="how-my-staff-works"></a>Personeliniz nasıl işe yarar?

Personeliniz, bir rol atamasının yönetim denetiminin kapsamını kısıtlamak için kullanılabilecek bir kaynak kapsayıcısı olan yönetim birimlerini (Avustralya) temel alır. Personelimde, bir mağaza veya departman gibi kuruluş kullanıcılarının bir alt kümesini tanımlamak için Avustralya kullanılır. Daha sonra, örneğin, kapsamı bir veya daha fazla au olan bir role ekip yöneticisi atanabilir. Aşağıdaki örnekte, kullanıcıya kimlik doğrulama yönetim rolü verildi ve üç Avustralya da rolün kapsamıdır. Yönetim birimleri hakkında daha fazla bilgi için bkz. [Azure Active Directory yönetim birimleri yönetimi](directory-administrative-units.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlayabilmeniz için aşağıdaki kaynaklar ve ayrıcalıklar gereklidir:

* Etkin bir Azure aboneliği.

  * Azure aboneliğiniz yoksa [bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Aboneliğinizle ilişkili bir Azure Active Directory kiracısı.

  * Gerekirse, [bir Azure Active Directory kiracı oluşturun](../fundamentals/sign-up-organization.md) veya [bir Azure aboneliğini hesabınızla ilişkilendirin](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
* SMS tabanlı kimlik doğrulamasını etkinleştirmek için Azure AD kiracınızda *genel yönetici* ayrıcalıklarına sahip olmanız gerekir.
* SMS mesajı kimlik doğrulama yöntemi ilkesinde etkin olan her bir kullanıcının, kullanmasa bile lisanslanması gerekir. Her etkin kullanıcı aşağıdaki Azure AD veya Microsoft 365 lisanslarından birine sahip olmalıdır:

  * [Azure AD Premium P1 veya P2](https://azure.microsoft.com/pricing/details/active-directory/)
  * [Microsoft 365 (M365) F1 veya F3](https://www.microsoft.com/licensing/news/m365-firstline-workers)
  * [Enterprise Mobility + Security (EMS) E3 veya E5](https://www.microsoft.com/microsoft-365/enterprise-mobility-security/compare-plans-and-pricing) ya da [Microsoft 365 (M365) E3 veya E5](https://www.microsoft.com/microsoft-365/compare-microsoft-365-enterprise-plans)

## <a name="how-to-enable-my-staff"></a>Personelimi etkinleştirme

Au 'yı yapılandırdıktan sonra, bu kapsamı personelime erişen kullanıcılarınıza uygulayabilirsiniz. Yalnızca bir yönetim rolü atanmış kullanıcılar personelime erişebilir. Personelimi etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure portal Kullanıcı Yöneticisi olarak oturum açın.
2. Kullanıcı ayarlarına **Azure Active Directory**, Kullanıcı  >  **User settings**  >  **özelliği önizlemeleri**  >  **Kullanıcı özelliği Önizleme ayarlarını yönet**' e gidin.
3. **Yöneticiler temsilcime erişebilme**altında, tüm kullanıcılar, seçili kullanıcılar veya Kullanıcı erişimi olmadan ' yi etkinleştirmeyi seçebilirsiniz.

> [!Note]
> Yalnızca bir yönetici rolü atanmış kullanıcılar personelime erişebilir. Yönetici rolü atanmamış bir kullanıcı için personelimi etkinleştirirseniz, personelime erişemezler.

## <a name="conditional-access"></a>Koşullu erişim

Azure AD koşullu erişim ilkesini kullanarak personel portalından koruma sağlayabilirsiniz. Personelinize erişmeden önce Multi-Factor Authentication gerektirme gibi görevler için kullanın.

[Azure AD koşullu erişim ilkelerini](../conditional-access/index.yml)kullanarak personelimi korumanızı kesinlikle öneririz. Personelinize koşullu erişim ilkesi uygulamak için, PowerShell kullanarak personel hizmeti sorumlusunu el ile oluşturmanız gerekir.

### <a name="apply-a-conditional-access-policy-to-my-staff"></a>Personelinize koşullu erişim ilkesi uygulama

1. [Microsoft Graph Beta PowerShell cmdlet 'lerini](https://github.com/microsoftgraph/msgraph-sdk-powershell/blob/dev/samples/0-InstallModule.ps1)yükler.
1. Aşağıdaki komutları çalıştırın:

   ```powershell
   Connect-Graph -Scopes "Directory.AccessAsUser.All"
   New-MgServicePrincipal -DisplayName "My Staff" -AppId "ba9ff945-a723-4ab5-a977-bd8c9044fe61"
   ```
1. Personel bulut uygulaması için geçerli bir koşullu erişim ilkesi oluşturun.

    ![Personel uygulaması için koşullu erişim ilkesi oluşturma](media/my-staff-configure/conditional-access.png)

## <a name="using-my-staff"></a>Personelimi kullanma

Bir Kullanıcı personelime gittiğinde, yönetim izinlerinin bulunduğu [yönetim birimlerinin](directory-administrative-units.md) adları gösterilir. [Personelin kullanıcı belgelerinde](../user-help/my-staff-team-manager.md), yönetim birimlerine başvurmak için "konum" terimini kullanırız. Yöneticinin izinlerinin AU kapsamı yoksa, izinler kuruluş genelinde uygulanır. Personeliniz etkinleştirildikten sonra, etkin olan ve bir yönetim rolü atanmış olan kullanıcılar buna aracılığıyla erişebilir [https://mystaff.microsoft.com](https://mystaff.microsoft.com) . Bu otomatik olarak, bu AU 'daki kullanıcıları görüntülemek için bir AU seçebilir ve profillerini açmak için bir kullanıcı seçebilirsiniz.

## <a name="reset-a-users-password"></a>Kullanıcı parolasını sıfırlama

Aşağıdaki rollerin bir kullanıcının parolasını sıfırlama izni vardır:

- [Kimlik doğrulama Yöneticisi](directory-assign-admin-roles.md#authentication-administrator)
- [Ayrıcalıklı kimlik doğrulama Yöneticisi](directory-assign-admin-roles.md#privileged-authentication-administrator)
- [Genel yönetici](directory-assign-admin-roles.md#global-administrator--company-administrator)
- [Yardım Masası Yöneticisi](directory-assign-admin-roles.md#helpdesk-administrator)
- [Kullanıcı yöneticisi](directory-assign-admin-roles.md#user-administrator)
- [Parola yöneticisi](directory-assign-admin-roles.md#password-administrator)

**Personelimin**içinden bir kullanıcının profilini açın. **Parolayı Sıfırla**' yı seçin.

- Kullanıcı yalnızca bulutta ise, kullanıcıya verebileceğiniz geçici bir parola görebilirsiniz.
- Kullanıcı şirket içi Active Directory eşitlendiğinde, şirket içi AD ilkelerinizi karşılayan bir parola girebilirsiniz. Daha sonra bu parolayı kullanıcıya verebilirsiniz.

    ![Parola sıfırlama ilerleme göstergesi ve başarı bildirimi](media/my-staff-configure/reset-password.png)

Kullanıcının bir sonraki oturum açışlarında parolasını değiştirmesi gerekir.

## <a name="manage-a-phone-number"></a>Telefon numarasını yönetme

**Personelimin**içinden bir kullanıcının profilini açın.

- Kullanıcı için telefon numarası eklemek üzere **telefon numarası Ekle** bölümünü seçin
- Telefon numarasını değiştirmek için **telefon numarasını Düzenle** ' yi seçin
- Kullanıcının telefon numarasını kaldırmak için **telefon numarasını Kaldır** ' ı seçin

Ayarlarınıza bağlı olarak, kullanıcı SMS ile oturum açmak için ayarladığınız telefon numarasını kullanabilir, çok faktörlü kimlik doğrulaması gerçekleştirebilir ve self servis parola sıfırlama işlemini gerçekleştirebilir.

Bir kullanıcının telefon numarasını yönetmek için aşağıdaki rollerden birine atanmış olmanız gerekir:

- [Kimlik doğrulama Yöneticisi](directory-assign-admin-roles.md#authentication-administrator)
- [Ayrıcalıklı kimlik doğrulama Yöneticisi](directory-assign-admin-roles.md#privileged-authentication-administrator)
- [Genel yönetici](directory-assign-admin-roles.md#global-administrator--company-administrator)

## <a name="search"></a>Ara

Kuruluşumdaki arama çubuğunu kullanarak kuruluşunuzdaki Avustralya ve kullanıcılar için arama yapabilirsiniz. Kuruluşunuzdaki tüm Avustralya ve kullanıcılar arasında arama yapabilirsiniz, ancak yalnızca yönetici izinlerine sahip olduğunuz bir AU 'daki kullanıcılar için değişiklikler yapabilirsiniz.

Ayrıca, bir AU içinde kullanıcı da arayabilirsiniz. Bunu yapmak için, kullanıcı listesinin en üstündeki arama çubuğunu kullanın.

## <a name="audit-logs"></a>Denetim günlükleri

Azure Active Directory portalında personelimde gerçekleştirilen eylemler için Denetim günlüklerini görüntüleyebilirsiniz. Bir denetim günlüğü, personelimde gerçekleştirilen bir eylem tarafından oluşturulduysa, bunu denetim olayında ek ayrıntılar altında görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

[Personel Kullanıcı belgeleri](../user-help/my-staff-team-manager.md) 
 [Yönetim birimleri belgeleri](directory-administrative-units.md)