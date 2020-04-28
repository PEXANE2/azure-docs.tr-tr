---
title: Azure AD 'ye katılmış cihaz nedir?
description: Cihaz kimlik yönetimi 'nin ortamınızdaki kaynaklara erişen cihazları yönetmenize nasıl yardımcı olabileceğini öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78672675"
---
# <a name="azure-ad-joined-devices"></a>Azure AD’ye katılmış cihazlar

Azure AD katılımı, yalnızca bulutta veya bulutta olmak isteyen kuruluşlar için tasarlanmıştır. Herhangi bir kuruluş, boyut veya sektöre bakılmaksızın Azure AD 'ye katılmış cihazları dağıtabilir. Azure AD JOIN, Karma ortamda bile çalışarak hem buluta hem de şirket içi uygulamalara ve kaynaklara erişimi etkinleştirir.

|   | Azure AD Join |
| --- | --- |
| **Tanım** | Yalnızca cihazda oturum açmak için kurumsal hesap gerektiren Azure AD 'ye katılmış |
| **Birincil hedef kitle** | Yalnızca bulut ve hibrit kuruluşlar için uygundur. |
|   | Bir kuruluştaki tüm kullanıcılar için geçerlidir |
| **Cihaz sahipliği** | Kuruluş |
| **İşletim Sistemleri** | Tüm Windows 10 cihazları |
| **Sağlanıyor** | Self Servis: Windows OOBE veya Settings |
|   | Toplu kayıt |
|   | Windows Autopilot |
| **Cihaz oturum açma seçenekleri** | Kullanılarak kurumsal hesaplar: |
|   | Parola |
|   | İş İçin Windows Hello |
|   | FıDO 2.0 güvenlik anahtarları (Önizleme) |
| **Cihaz yönetimi** | Mobil cihaz yönetimi (örnek: Microsoft Intune) |
|   | Microsoft Intune ve Microsoft uç noktası ile ortak yönetim Configuration Manager |
| **Temel işlevler** | Hem bulut hem de şirket içi kaynaklara yönelik SSO |
|   | MDM kaydı ve MDM uyumluluk değerlendirmesi aracılığıyla koşullu erişim |
|   | Kilitleme ekranında self servis parola sıfırlama ve Windows Hello PIN sıfırlama |
|   | Cihazlar arasında Enterprise State Roaming |

Azure AD 'ye katılmış cihazlar, kurumsal bir Azure AD hesabı kullanarak oturum açtı. Kuruluştaki kaynaklara erişim, bu Azure AD hesabına ve cihaz kimliğine uygulanan [koşullu erişim ilkelerine](../conditional-access/overview.md) göre daha da sınırlı olabilir.

Yöneticiler, Microsoft Intune gibi mobil cihaz yönetimi (MDM) araçlarını kullanarak Azure AD 'ye katılmış cihazları güvenlik altına alabilir ve daha fazla denetleyebilir (Microsoft uç nokta Configuration Manager kullanan ortak yönetim senaryolarında). Bu araçlar, depolamanın şifreli, parola karmaşıklığı, yazılım yüklemeleri ve yazılım güncelleştirmeleri gibi kuruluş gerektiren yapılandırmaların zorlanmasını sağlamak için bir yol sağlar. Yöneticiler [, iş ve eğitim için Microsoft Store uygulamaları yönetmek](/configmgr/apps/deploy-use/manage-apps-from-the-windows-store-for-business)üzere Configuration Manager kullanarak kuruluş UYGULAMALARıNı Azure AD 'ye katılmış cihazlara kullanabilir hale getirir.

Azure AD JOIN, hazır olmayan deneyim (OOBE), toplu kayıt veya [Windows Autopilot](/intune/enrollment-autopilot)gibi self servis seçenekleri kullanılarak gerçekleştirilebilir.

Azure AD 'ye katılmış cihazlar, kuruluşun ağı üzerinde olduklarında şirket içi kaynaklara yönelik çoklu oturum açma erişimi sürdürmeye devam edebilir. Azure AD 'ye katılmış cihazlar, dosya, yazdırma ve diğer uygulamalar gibi şirket içi sunucularda kimlik doğrulaması yapabilir.

## <a name="scenarios"></a>Senaryolar

Azure AD’ye katılma özelliği temel olarak bir şirket içi Windows Server Active Directory altyapısı bulunmayan kuruluşlar için tasarlanmıştır ancak aşağıdaki senaryolarda siz de bu özellikten yararlanabilirsiniz:

- Azure AD ve MDM benzeri Intune kullanarak bulut tabanlı altyapıya geçiş yapmak istediğinizde.
- Şirket içi etki alanına katılma özelliğini kullanamadığınız durumlarda; örneğin, tabletler ve telefonlar gibi mobil cihazlar üzerinde denetim sağlamanız gerektiğinde.
- Kullanıcılarınızın temel olarak Office 365 veya Azure AD ile tümleşik diğer SaaS uygulamalarına erişmesi gerektiğinde.
- Active Directory yerine Azure AD’de bir kullanıcı grubunu yönetmek istediğinizde. Bu senaryo, örneğin, dönemsel çalışanlar, yükleniciler veya öğrenciler için uygulanabilir.
- Sınırlı şirket içi altyapısı olan uzak şube ofislerindeki çalışanlara katılma özellikleri sağlamak istediğinizde.

Windows 10 cihazları için Azure AD’ye katılmış cihazları yapılandırabilirsiniz.

Azure AD'ye katılmış cihazların hedefi şu işlemlerde basitleştirme sağlamaktır:

- İşe ait cihazların Windows dağıtımları
- Herhangi bir Windows cihazından kuruluş uygulamalarına ve kaynaklarına erişim
- İşe ait cihazların bulut tabanlı yönetimi
- Kullanıcıların cihazlarında Azure AD 'ye oturum açması veya Active Directory iş veya okul hesaplarıyla eşitlenmesi.

![Azure AD’ye katılmış cihazlar](./media/concept-azure-ad-join/azure-ad-joined-device.png)

Aşağıdaki yöntemlerden birini kullanarak Azure AD'ye Katılım dağıtımı yapılabilir:

- [Windows Autopilot](/windows/deployment/windows-autopilot/windows-10-autopilot)
- [Toplu dağıtım](/intune/windows-bulk-enroll)
- [Self servis deneyimi](azuread-joined-devices-frx.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD katılımınızı uygulamayı planlama](azureadjoin-plan.md)
- [Azure AD 'ye katılmış cihazlarda yerel Yöneticiler grubunu yönetme](assign-local-admin.md)
- [Azure portal kullanarak cihaz kimliklerini yönetme](device-management-azure-portal.md)
- [Azure AD 'de eski cihazları yönetme](manage-stale-devices.md)
