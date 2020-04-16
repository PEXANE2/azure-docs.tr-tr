---
title: Kullanıcı yönetimini (önizleme) temsilci vermek için Personelimi Kullanın - Azure AD | Microsoft Dokümanlar
description: Personelimi ve yönetim birimlerini kullanarak kullanıcı yönetimini temsilciolarak
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.topic: article
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.date: 04/14/2020
ms.author: curtand
ms.reviewer: sahenry
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 3f7c12612dbe37de6b08cb05a64af460296ade93
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81394221"
---
# <a name="manage-your-users-with-my-staff-preview"></a>Personelim (önizleme) ile kullanıcılarınızı yönetin

Personelim, çalışanların Azure REKLAM hesaplarına erişebilmelerini sağlamak için mağaza yöneticisi veya ekip müşteri adayı gibi bir yetkili figüre, izinlere yetki vermenizi sağlar. Kuruluşlar, merkezi bir yardım masasına güvenmek yerine parolaları sıfırlama veya telefon numaralarını bir takım yöneticisine değiştirme gibi yaygın görevleri devredebilir. Personelim ile, hesabına erişemeyen bir kullanıcı, yardım masası veya BT personeli gerektirmeden yalnızca birkaç tıklamayla erişim sağlayabilir.

Personelimi kuruluşunuz için yapılandırmadan önce, bu özelliğin işlevselliğini ve kullanıcılarınız üzerindeki etkisini anladığınızdan emin olmak için bu belgeleri ve [kullanıcı belgelerini](../user-help/my-staff-team-manager.md) gözden geçirmenizi öneririz. Kullanıcılarınızı yeni deneyime hazırlamak ve başarılı bir kullanıma yardımcı olmak için kullanıcı belgelerinden yararlanabilirsiniz.

## <a name="how-my-staff-works"></a>Personelim Nasıl Çalışır?

Personelim, bir rol atamasının yönetim denetiminin kapsamını sınırlamak için kullanılabilecek bir kaynak kapsayıcısı olan idari birimlere (AUs) dayanır. Personelim'de, AUs, bir kuruluşun mağaza veya departman gibi kullanıcılarının bir alt kümesini tanımlamak için kullanılır. Daha sonra, örneğin, kapsamı bir veya daha fazla AUs olan bir role bir takım yöneticisi atanabilir. Aşağıdaki örnekte, kullanıcıya Kimlik Doğrulama Yönetimi rolü verilmiştir ve üç U'su rolün kapsamıdır. Yönetim birimleri hakkında daha fazla bilgi için [Azure Etkin Dizini'nde Yönetim birimleri yönetimine](directory-administrative-units.md)bakın.

## <a name="how-to-enable-my-staff"></a>Personelimi etkinleştirme

OSB'leri yapılandırdıktan sonra, bu kapsamı Personelime erişen kullanıcılarınız için uygulayabilirsiniz. Yalnızca yönetimrolü alan kullanıcılar Personelime erişebilir. Personelimi etkinleştirmek için aşağıdaki adımları tamamlayın:

1. Kullanıcı yöneticisi olarak Azure portalında oturum açın.
2. **Azure Active Directory** > **Kullanıcı ayarlarına** > göz atın**Kullanıcı özelliği önizlemeleri**Kullanıcı özelliği > **önizlemeleri Kullanıcı özelliği önizleme ayarlarını yönetin.**
3. Yöneticiler altında **Personelim'e erişebilir,** tüm kullanıcılar, seçili kullanıcılar veya kullanıcı erişimi için etkinleştirmeyi seçebilirsiniz.

> [!Note]
> Yalnızca yönetici rolü atanan kullanıcılar Personelime erişebilir. Yönetici rolü atanmamış bir kullanıcı için Personelimi etkinleştiriseniz, bunlar Personelime erişemez.

## <a name="using-my-staff"></a>Personelimi Kullanma

Bir kullanıcı Personelime gittiğinde, üzerinde yönetim izinleri bulunan [idari birimlerin](directory-administrative-units.md) adları gösterilir. [Personelim kullanıcı belgelerinde,](../user-help/my-staff-team-manager.md)idari birimlere başvurmak için "konum" terimini kullanırız. Yöneticiizinde AU kapsamı yoksa, izinler kuruluş genelinde geçerlidir. Personelim etkinleştirildikten sonra, etkinleştirilmiş ve yönetim rolü atanan kullanıcılar [https://mystaff.microsoft.com](https://mystaff.microsoft.com)bu göreve . Bu AU'daki kullanıcıları görüntülemek için bir AU seçebilir ve profillerini açmak için bir kullanıcı seçebilirler.

## <a name="licenses"></a>Lisanslar

Personelim portalımı kullanmasalar bile, Personelim'de etkinleştirilen her kullanıcı nın lisanslı olması gerekir. Etkinleştirilen her kullanıcı aşağıdaki Azure AD veya Microsoft 365 lisanslarından birine sahip olmalıdır:

- Azure AD Premium P1 veya P2
- Microsoft 365 F1 veya F3

## <a name="reset-a-users-password"></a>Kullanıcı parolasını sıfırlama

Aşağıdaki roller, kullanıcının parolasını sıfırlama iznine sahiptir:

- [Kimlik doğrulama yöneticisi](directory-assign-admin-roles.md#authentication-administrator)
- [Ayrıcalıklı kimlik doğrulama yöneticisi](directory-assign-admin-roles.md#privileged-authentication-administrator)
- [Genel yönetici](directory-assign-admin-roles.md#global-administrator--company-administrator)
- [Yardım Masası yöneticisi](directory-assign-admin-roles.md#helpdesk-administrator)
- [Kullanıcı yöneticisi](directory-assign-admin-roles.md#user-administrator)
- [Parola yöneticisi](directory-assign-admin-roles.md#password-administrator)

**Personelim'den**bir kullanıcının profilini açın. **Parolayı Sıfırla'yı**seçin.

- Kullanıcı yalnızca buluttaysa, kullanıcıya verebileceğiniz geçici bir parola görebilirsiniz.
- Kullanıcı şirket içi Active Directory'den senkronize edilirse, şirket içi REKLAM ilkelerinizi karşılayan bir parola girebilirsiniz. Daha sonra bu parolayı kullanıcıya verebilirsiniz.

    ![Parola sıfırlama ilerleme göstergesi ve başarı bildirimi](media/my-staff-configure/reset-password.png)

Kullanıcının bir sonraki oturum açıncın parolasını değiştirmesi gerekir.

## <a name="manage-a-phone-number"></a>Telefon numarasını yönetme

**Personelim'den**bir kullanıcının profilini açın.

- Kullanıcıiçin telefon numarası eklemek için **telefon numarası** ekle bölümünü seçin
- Telefon numarasını değiştirmek için **telefon numarasını değiştir'i** seçin
- Kullanıcının telefon numarasını kaldırmak için **telefon numarasını kaldır'ı** seçin

Ayarlarınıza bağlı olarak, kullanıcı daha sonra SMS ile oturum açmak için ayarladığınız telefon numarasını kullanabilir, çok faktörlü kimlik doğrulama sıyrıklarını gerçekleştirebilir ve self servis parola sıfırlama gerçekleştirebilir.

Bir kullanıcının telefon numarasını yönetmek için aşağıdaki rollerden birine atanmalısınız:

- [Kimlik doğrulama yöneticisi](directory-assign-admin-roles.md#authentication-administrator)
- [Ayrıcalıklı kimlik doğrulama yöneticisi](directory-assign-admin-roles.md#privileged-authentication-administrator)
- [Genel yönetici](directory-assign-admin-roles.md#global-administrator--company-administrator)

## <a name="search"></a>Arama

Personelim'deki arama çubuğunu kullanarak kuruluşunuzdaki AU'ları ve kullanıcıları arayabilirsiniz. Kuruluşunuzdaki tüm ABD'lerde ve kullanıcılarda arama yapabilirsiniz, ancak yalnızca size yönetici izni verilen bir AU'da bulunan kullanıcılarda değişiklik yapabilirsiniz.

Ayrıca, BIR AU içinde bir kullanıcı için arama yapabilirsiniz. Bunu yapmak için, kullanıcı listesinin en üstündeki arama çubuğunu kullanın.

## <a name="audit-logs"></a>Denetim günlükleri

Azure Active Directory portalında Personelim'de gerçekleştirilen işlemleriçin denetim günlüklerini görüntüleyebilirsiniz. Personelim'de gerçekleştirilen bir eylem tarafından bir denetim günlüğü oluşturulduysa, denetim olayında EK BİlGİLER altında bu durum belirtilir.

## <a name="next-steps"></a>Sonraki adımlar

[Personelim kullanıcı belgeleri](../user-help/my-staff-team-manager.md)
[İdari birimler dokümantasyonu](directory-administrative-units.md)
