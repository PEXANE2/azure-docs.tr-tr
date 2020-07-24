---
title: Azure Multi-Factor Authentication için Kullanıcı ayarlarını yönetme-Azure Active Directory
description: Azure Multi-Factor Authentication için Azure Active Directory Kullanıcı ayarlarını nasıl yapılandırabileceğinizi öğrenin
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/20/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9861e8e8be39781ad1b256b9939df3ab03e74be6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87027538"
---
# <a name="manage-user-settings-for-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication için Kullanıcı ayarlarını yönetme

Azure Multi-Factor Authentication kullanıcılarının yönetilmesine yardımcı olmak için kullanıcıların parolalarını sıfırlamasını, MFA için yeniden kaydolmayı veya mevcut MFA oturumlarını iptal etmeyi isteyebilirsiniz. Uygulama parolaları tanımlı kullanıcılar için, bu parolaları silmeyi de seçebilirsiniz ve bu durumda eski kimlik doğrulamanın Bu uygulamalarda başarısız olmasına neden olabilir. Bu eylemler, bir kullanıcıya yardım sağlamanız veya güvenlik durumlarını sıfırlamak istediğinizde gerekli olabilir.

## <a name="manage-user-authentication-options"></a>Kullanıcı kimlik doğrulama seçeneklerini yönetme

*Kimlik doğrulama Yöneticisi* rolünü atadıysanız, kullanıcıların kendi parolalarını SıFıRLAMALARıNı, MFA için yeniden kaydolmayı veya mevcut MFA oturumlarını kullanıcı nesnesinden iptal etmeyi zorunlu kılabilirsiniz. Kullanıcı ayarlarını yönetmek için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol tarafta **Azure Active Directory** > **Kullanıcılar** > **Tüm kullanıcılar**'ı seçin.
1. Üzerinde bir eylem gerçekleştirmek istediğiniz kullanıcıyı seçin ve **kimlik doğrulama yöntemleri**' ni seçin. Pencerenin üst kısmında, Kullanıcı için aşağıdaki seçeneklerden birini belirleyin:
   - **Parola sıfırlama** Kullanıcı parolasını sıfırlar ve sonraki oturum açma sırasında değiştirilmesi gereken geçici bir parola atar.
   - **MFA 'Yı yeniden kaydettirme** , Kullanıcı bir sonraki sefer oturum açtığında yenı bir MFA kimlik doğrulama yöntemi ayarlaması istenmesini sağlar.
   
      > [!NOTE]
      > Yönetici MFA için yeniden kayıt gerektirdiğinde kullanıcının şu anda kayıtlı kimlik doğrulama yöntemleri silinmez. Bir Kullanıcı MFA için yeniden kaydolduktan sonra, güvenlik bilgilerini gözden geçirmelerini ve artık kullanılamayan önceden kaydedilmiş kimlik doğrulama yöntemlerini silmenizi öneririz.
   
   - **MFA oturumlarını Iptal etme** , kullanıcının HATıRLANAN MFA oturumlarını temizler ve cihazdaki ilke için bir sonraki ihtiyaç DUYULıŞıNDA MFA gerçekleştirmesini gerektirir.

   ![Azure portal kimlik doğrulama yöntemlerini yönetin](./media/howto-mfa-userdevicesettings/manage-authentication-methods-in-azure.png)

## <a name="delete-users-existing-app-passwords"></a>Kullanıcı var olan uygulama parolalarını silme

Gerekirse, bir kullanıcının oluşturduğu tüm uygulama parolalarını silebilirsiniz. Bu uygulama parolalarıyla ilişkilendirilen tarayıcı olmayan uygulamalar, yeni bir uygulama parolası oluşturuluncaya kadar çalışmayı durdurur. Bu eylemi gerçekleştirmek için *genel yönetici* izinleri gereklidir.

Bir kullanıcının uygulama parolalarını silmek için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol tarafta **Azure Active Directory**  >  **Kullanıcılar**  >  **tüm kullanıcılar**' ı seçin.
1. **Multi-Factor Authentication**'ı seçin. Bu menü seçeneğini görmek için sağa kaydırmanız gerekebilir. Tam Azure portal pencere ve menü konumunu görmek için aşağıdan örnek ekran görüntüsünü seçin:[![](media/howto-mfa-userstates/selectmfa-cropped.png "Azure AD 'de kullanıcılar penceresinden Multi-Factor Authentication seçin")](media/howto-mfa-userstates/selectmfa.png#lightbox)
1. Yönetmek istediğiniz kullanıcı veya kullanıcıların yanındaki kutuyu işaretleyin. Sağ tarafta hızlı adım seçeneklerinin bir listesi görüntülenir.
1. **Kullanıcı ayarlarını yönet**' i seçin, sonra aşağıdaki örnekte gösterildiği gibi **Seçilen kullanıcılar tarafından oluşturulan mevcut tüm uygulama parolalarını Sil**kutusunu işaretleyin. ![ var olan tüm uygulama parolalarını Sil](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
1. **Kaydet**' i ve ardından **Kapat**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bireysel kullanıcı ayarlarını nasıl yapılandıracağınız açıklanır. Genel Azure Multi-Factor Authentication hizmet ayarlarını yapılandırmak için bkz. [azure Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md).

Kullanıcılarınızın yardıma ihtiyacı varsa bkz. [Azure Multi-Factor Authentication Için Kullanıcı Kılavuzu](../user-help/multi-factor-authentication-end-user-first-time.md).
