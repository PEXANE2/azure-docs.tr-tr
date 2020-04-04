---
title: Şirket içinde Azure AD Parola Koruması'nı etkinleştirme
description: Şirket içi Aktif Dizin Etki Alanı Hizmetleri ortamı için Azure AD Parola Koruması'nı nasıl etkinleştirin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 03/05/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ee0f3d89d48b23db48e3bf4b78203b09fbcbdbd
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652630"
---
# <a name="enable-on-premises-azure-active-directory-password-protection"></a>Şirket içi Azure Etkin Dizin Parola Koruması'nı etkinleştirme

Kullanıcılar genellikle okul, spor takımı veya ünlü kişi gibi yaygın yerel sözcükleri kullanan parolalar oluşturur. Bu parolaları tahmin etmek kolaydır ve sözlük tabanlı saldırılara karşı zayıftır. Kuruluşunuzdaki güçlü parolaları zorlamak için Azure Active Directory (Azure AD) Parola Koruması, genel ve özel olarak yasaklanmış bir parola listesi sağlar. Bu yasaklı parola listesinde eşleşme varsa parola değiştirme isteği başarısız olur.

Şirket içi Active Directory Domain Services (AD DS) ortamınızı korumak için, ön lisans DC'nizle çalışmak üzere Azure AD Parola Koruması'nı yükleyebilir ve yapılandırabilirsiniz. Bu makalede, şirket içi ortamınız için Azure AD Parola Koruması'nı nasıl etkinleştirdiğiniz gösterilmektedir.

Azure AD Parola Koruması'nın şirket içi ortamda nasıl çalıştığı hakkında daha fazla bilgi için [Windows Server Etkin Dizin için Azure AD Parola Koruması'nı nasıl uygulayacağınızı](concept-password-ban-bad-on-premises.md)öğrenin.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, şirket içi ortamınız için Azure AD Parola Koruması'nı nasıl etkinleştirdiğiniz gösterilmektedir. Bu makaleyi tamamlamadan önce, Azure AD Parola Koruması proxy hizmetini ve DC aracılarını şirket içi AD DS ortamınızda [yükleyin ve kaydedin.](howto-password-ban-bad-on-premises-deploy.md)

## <a name="enable-on-premises-password-protection"></a>Şirket içi parola korumasını etkinleştirme

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Azure Active Directory** > **Security** > **Kimlik Doğrulama yöntemlerine** > göz atın**Parola koruması**.
1. Windows Server **Active Directory'de Parola Korumayı Etkinleştir** seçeneğini *Evet'e*ayarlayın.

    Bu ayar *Hayır*olarak ayarlandığında, dağıtılan tüm Azure AD Parola Koruması DC aracıları, tüm parolaların olduğu gibi kabul edildiği bir moda gider. Doğrulama etkinliği gerçekleştirilmiyor ve denetim olayları oluşturulmuyor.

1. Modu başlangıçta *Denetime*ayarlamak **Mode** önerilir. Bu özellik ve kuruluşunuzdaki kullanıcılar üzerindeki etkisiyle rahat ettikten **sonra, Modu** *Zorunlu*Olarak Değiştirebilirsiniz. Daha fazla bilgi [için, çalışma modları](#modes-of-operation)ile ilgili aşağıdaki bölüme bakın.
1. Hazır olduğunda **Kaydet'i**seçin.

    [![](media/howto-password-ban-bad-on-premises-operations/enable-configure-custom-banned-passwords-cropped.png "Enable on-premises password protection under Authentication Methods in the Azure portal")](media/howto-password-ban-bad-on-premises-operations/enable-configure-custom-banned-passwords.png#lightbox)

## <a name="modes-of-operation"></a>İşlem modları

Şirket içinde Azure AD Parola Koruması'nı etkinleştirdiğinizde, *denetim* modunu veya *zorlama* modunu kullanabilirsiniz. İlk dağıtım ve sınamanın her zaman denetim modunda başlamasını öneririz. Daha sonra, *Uygulama* modu etkinleştirildiğinde varolan operasyonel işlemlerin rahatsız edilip edilmeyeceğini tahmin etmek için olay günlüğündeki girişler izlenmelidir.

### <a name="audit-mode"></a>Denetim modu

*Denetim* modu, yazılımı "ne olur" modunda çalıştırmanın bir yolu olarak tasarlanmıştır. Her Azure AD Parola Koruma DC aracı hizmeti, gelen parolayı şu anda etkin olan ilkeye göre değerlendirir.

Geçerli ilke denetim modunda olacak şekilde yapılandırılırsa, "kötü" parolalar olay günlüğü iletilerine neden olur, ancak işlenir ve güncelleştirilir. Bu davranış, denetim ve zorlama modu arasındaki tek farktır. Diğer tüm işlemler aynı şekilde çalışır.

### <a name="enforced-mode"></a>Zorlanan Mod

*Zorlanan* mod son yapılandırma olarak tasarlanmıştır. Denetim modundaolduğu gibi, her Azure AD Parola Koruması DC aracı hizmeti, gelen parolaları şu anda etkin olan ilkeye göre değerlendirir. Ancak zorunlu mod etkinleştirildiğinde, ilkeye göre güvenli olmadığı düşünülen bir parola reddedilir.

Bir parola Azure AD Password Protection DC aracısı tarafından zorunlu modda reddedildiğinde, son kullanıcı parolalarının geleneksel şirket içi parola karmaşıklığı zorlaması tarafından reddedilip reddedilmediklerini göreceği gibi benzer bir hata görür. Örneğin, bir kullanıcı Windows oturum açma da aşağıdaki geleneksel hata iletisini görebilir veya parolayı değiştirebilir:

*"Parolayı güncelleştiremiyoruz. Yeni parola için sağlanan değer etki alanının uzunluk, karmaşıklık veya geçmiş gereksinimlerini karşılamaz."*

Bu ileti, birkaç olası sonuçların yalnızca bir örneğidir. Belirli hata iletisi, güvenli olmayan bir parola ayarlamaya çalışan gerçek yazılıma veya senaryoya bağlı olarak değişebilir.

Etkilenen son kullanıcıların yeni gereksinimleri anlamak ve güvenli parolaları seçmek için BT personeliyle birlikte çalışmaları gerekebilir.

> [!NOTE]
> Azure AD Parola Koruması, zayıf bir parola reddedildiğinde istemci makine tarafından görüntülenen belirli hata iletisi üzerinde hiçbir denetime sahip değildir.

## <a name="next-steps"></a>Sonraki adımlar

Kuruluşunuz için yasaklı parola listesini özelleştirmek için Azure [AD Parola Koruması özel yasaklı parola listesini yapılandırma'ya](tutorial-configure-custom-password-protection.md)bakın.

Prem'deki olayları izlemek için [bkz.](howto-password-ban-bad-on-premises-monitor.md)
