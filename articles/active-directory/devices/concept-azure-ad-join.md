---
title: Azure AD'ye katılan aygıt nedir?
description: Aygıt kimlik yönetiminin ortamınızdaki kaynaklara erişen aygıtları yönetmenize nasıl yardımcı olabileceğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40f89fbe19b93601f9e0525f0387e402bd175fe4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672675"
---
# <a name="azure-ad-joined-devices"></a>Azure AD’ye katılmış cihazlar

Azure AD join, buluta ilk veya yalnızca bulut olmak isteyen kuruluşlar için tasarlanmıştır. Herhangi bir kuruluş, boyutu veya endüstrisi ne olursa olsun Azure AD'ye katılan aygıtları dağıtabilir. Azure AD join, hem bulut hem de şirket içi uygulamalara ve kaynaklara erişim sağlayarak karma bir ortamda bile çalışır.

|   | Azure AD Join |
| --- | --- |
| **Tanım** | Yalnızca kurumsal hesabın aygıtta oturum açmasını gerektiren Azure AD'ye katıldı |
| **Birincil hedef kitle** | Yalnızca bulut ve hibrit kuruluşlar için uygundur. |
|   | Kuruluştaki tüm kullanıcılar için geçerlidir |
| **Cihaz sahipliği** | Kuruluş |
| **İşletim Sistemleri** | Tüm Windows 10 cihazları |
| **Sağlanıyor** | Self servis: Windows OOBE veya Ayarlar |
|   | Toplu kayıt |
|   | Windows Autopilot |
| **Cihaz oturum açma seçenekleri** | Şu ları kullanarak kuruluş hesapları: |
|   | Parola |
|   | İş İçin Windows Hello |
|   | FIDO2.0 güvenlik tuşları (önizleme) |
| **Cihaz yönetimi** | Mobil Aygıt Yönetimi (örnek: Microsoft Intune) |
|   | Microsoft Intune ve Microsoft Endpoint Configuration Manager ile birlikte yönetim |
| **Temel işlevler** | SSO'dan hem buluta hem de şirket içi kaynaklara |
|   | MDM kaydı ve MDM uyumluluk değerlendirmesi ile Koşullu Erişim |
|   | Self servis Parola Sıfırlama ve Windows Hello PIN sıfırlama kilit ekranında |
|   | Aygıtlar arasında Kurumsal Durum Dolaşımı |

Azure AD'ye katılan aygıtlar, kuruluş eldeki bir Azure AD hesabı nı kullanmak üzere oturum açMaktadır. Kuruluştaki kaynaklara erişim, bu Azure REKLAM hesabına ve aygıt kimliğine uygulanan [Koşullu Erişim ilkelerine](../conditional-access/overview.md) bağlı olarak daha da sınırlanabilir.

Yöneticiler, Microsoft Intune gibi Mobil Aygıt Yönetimi (MDM) araçlarını veya Microsoft Endpoint Configuration Manager'ı kullanarak ortak yönetim senaryolarını kullanarak Azure AD'ye katılan aygıtları güvenli hale getirebilir ve daha fazla denetleyebilir. Bu araçlar, depolamanın şifrelenmesini gerektirmek, parola karmaşıklığı, yazılım yüklemeleri ve yazılım güncelleştirmeleri gibi kuruluş için gerekli yapılandırmaları zorlamak için bir araç sağlar. Yöneticiler, [Microsoft İş ve Eğitim Mağazası'ndaki uygulamaları yönetmek için](/configmgr/apps/deploy-use/manage-apps-from-the-windows-store-for-business)Configuration Manager'ı kullanarak Azure AD'ye katılan aygıtlarda kuruluş uygulamalarını kullanılabilir hale getirebilir.

Azure AD join, Kutu dan sı(OOBE), toplu kayıt veya [Windows Otomatik Pilot](/intune/enrollment-autopilot)gibi self servis seçenekleri kullanılarak gerçekleştirilebilir.

Azure AD'ye katılan aygıtlar, kuruluş ağındayken şirket içi kaynaklara tek oturum açma erişimi ne kadar sürdürebilir. Azure AD'ye katılan aygıtlar, dosya, yazdırma ve diğer uygulamalar gibi şirket içi sunucularda kimlik doğrulaması yapmaya devam edebilir.

## <a name="scenarios"></a>Senaryolar

Azure AD’ye katılma özelliği temel olarak bir şirket içi Windows Server Active Directory altyapısı bulunmayan kuruluşlar için tasarlanmıştır ancak aşağıdaki senaryolarda siz de bu özellikten yararlanabilirsiniz:

- Azure AD ve MDM benzeri Intune kullanarak bulut tabanlı altyapıya geçiş yapmak istediğinizde.
- Şirket içi etki alanına katılma özelliğini kullanamadığınız durumlarda; örneğin, tabletler ve telefonlar gibi mobil cihazlar üzerinde denetim sağlamanız gerektiğinde.
- Kullanıcılarınızın temel olarak Office 365 veya Azure AD ile tümleşik diğer SaaS uygulamalarına erişmesi gerektiğinde.
- Active Directory yerine Azure AD’de bir kullanıcı grubunu yönetmek istediğinizde. Bu senaryo, örneğin mevsimlik işçiler, yükleniciler veya öğrenciler için geçerli olabilir.
- Sınırlı şirket içi altyapısı olan uzak şube ofislerindeki çalışanlara katılma özellikleri sağlamak istediğinizde.

Windows 10 cihazları için Azure AD’ye katılmış cihazları yapılandırabilirsiniz.

Azure AD'ye katılmış cihazların hedefi şu işlemlerde basitleştirme sağlamaktır:

- İşe ait cihazların Windows dağıtımları
- Herhangi bir Windows cihazından kuruluş uygulamalarına ve kaynaklarına erişim
- İşe ait cihazların bulut tabanlı yönetimi
- Kullanıcıların cihazlarında Azure AD'leriyle oturum açmaları veya Etkin Dizin çalışmaları veya okul hesaplarıyla eşitlenenler.

![Azure AD’ye katılmış cihazlar](./media/concept-azure-ad-join/azure-ad-joined-device.png)

Aşağıdaki yöntemlerden birini kullanarak Azure AD'ye Katılım dağıtımı yapılabilir:

- [Windows Autopilot](/windows/deployment/windows-autopilot/windows-10-autopilot)
- [Toplu dağıtım](/intune/windows-bulk-enroll)
- [Self servis deneyimi](azuread-joined-devices-frx.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD katılımınızı uygulamayı planlama](azureadjoin-plan.md)
- [Azure AD'deki yerel yöneticiler grubunu yönetme](assign-local-admin.md)
- [Azure portalını kullanarak aygıt kimliklerini yönetme](device-management-azure-portal.md)
- [Azure AD'de eski aygıtları yönetme](manage-stale-devices.md)
