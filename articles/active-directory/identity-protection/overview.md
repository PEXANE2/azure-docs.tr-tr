---
title: Azure Active Directory Kimlik Koruması | Microsoft Docs
description: Azure AD Kimlik Koruması, bir saldırganın güvenliği aşılmış bir kimlik veya cihazdan yararlanma ve daha önce şüphelenildiği ya da tehlikede olduğu bilinen bir cihazın güvenliğini sağlama becerisini nasıl sağlayabileceğinizi öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: overview
ms.date: 01/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b89cab41061376fc1d8b4cbffc8fe87b9677688
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70125668"
---
# <a name="what-is-azure-active-directory-identity-protection"></a>Azure Active Directory Kimlik Koruması nedir?

Azure Active Directory [kimlik](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis#terminology) koruması, kuruluşların otomatik yanıtları, Kullanıcı kimlikleriyle ilgili şüpheli eylemleri tespit etmek üzere yapılandırmalarına olanak sağlar.

## <a name="get-started"></a>başlarken

Microsoft, bulut tabanlı kimliklerin bir yılda bir kereden fazla olması için güvenli hale getirildi. Azure Active Directory Kimlik Koruması, ortamınızda, Microsoft 'un kimlikleri güvenli hale getirmek için kullandığı koruma sistemlerini de kullanabilirsiniz.

Saldırganlar bir kullanıcının kimliğini çalarak bir ortama erişim kazanıyorsa güvenlik ihlallerinin büyük çoğunluğu bu şekilde gerçekleşir. Yıllar boyunca saldırganlar, üçüncü taraf ihlallerinin kullanılmasıyla ve gelişmiş kimlik avı saldırıları kullanılarak giderek daha etkili hale gelmiştir. Bir saldırgan bile düşük ayrıcalıklı kullanıcı hesaplarına erişim kazandığında, bu kullanıcıların yan yana hareket aracılığıyla önemli şirket kaynaklarına erişmesini nispeten daha kolay hale gelir.

Bunun bir sonucu olarak şunları yapmanız gerekir:

- Ayrıcalık düzeyinden bağımsız olarak tüm kimlikleri koruyun
- Güvenliği aşılmış kimliklerin karşı önlem almasını önleme

Güvenliği aşılmış kimlikleri bulma işlemi kolay bir görev değildir. Azure Active Directory, güvenliği tehlikeye giren kimlikleri belirten anormallikleri ve şüpheli olayları algılamak için uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Kimlik koruması, bu verileri kullanarak algılanan sorunları değerlendirmenizi ve uygun risk azaltma veya düzeltme eylemleri gerçekleştirmeniz sağlayan raporlar ve uyarılar oluşturur.

Azure Active Directory Kimlik Koruması, bir izleme ve raporlama aracından daha fazla. Kuruluşunuzun kimliklerini korumak için, belirtilen risk düzeyine ulaşıldığında algılanan sorunlara otomatik olarak yanıt veren risk tabanlı ilkeler yapılandırabilirsiniz. Bu ilkeler, Azure Active Directory ve [Enterprise Mobility + Security](https://docs.microsoft.com/enterprise-mobility-security/) (EMS) tarafından sunulan diğer koşullu erişim denetimlerine ek olarak, parola sıfırlama dahil Uyarlamalı düzeltme eylemlerini otomatik olarak engelleyebilir veya başlatabilir. Multi-Factor Authentication zorlaması.

### <a name="identity-protection-capabilities"></a>Kimlik koruma özellikleri

**Güvenlik Açıkları ve riskli hesaplar algılanıyor:**  

- Güvenlik açıklarını vurgulayarak genel güvenlik duruşunu geliştirmek için özel öneriler sağlama
- Oturum açma risk düzeylerini hesaplama
- Kullanıcı risk düzeylerini hesaplama

**Risk algılamalarını araştırma:**

- Risk algılamaları için bildirimler gönderme
- İlgili ve bağlamsal bilgileri kullanarak risk algılamalarını araştırma
- Araştırmalar izlemek için temel iş akışları sağlama
- Parola sıfırlama gibi Düzeltme eylemlerine kolay erişim sağlama

**Risk tabanlı koşullu erişim ilkeleri:**

- Oturum açma işlemlerini engelleyerek veya Multi-Factor Authentication sorunlarını gerektirerek riskli oturum açma işlemlerini Azaltma ilkesi
- Riskli Kullanıcı hesaplarını engelleme veya güvenli hale getirme ilkesi
- Kullanıcıların Multi-Factor Authentication 'a kaydolmesini isteme ilkesi

## <a name="identity-protection-roles"></a>Kimlik koruması rolleri

Kimlik koruma uygulamanız etrafında yönetim etkinliklerinin yükünü dengelemek için birkaç rol atayabilirsiniz. Azure AD Kimlik Koruması 3 dizin rolünü destekler:

| Role | Yapılabilir | Yapılamıyor |
| :-- | --- | --- |
| Genel yönetici | Kimlik koruması, yerleşik kimlik koruması ile tam erişim| |
| Güvenlik yöneticisi | Kimlik korumasına tam erişim | Kimlik koruması ekleme, bir kullanıcı için parolaları sıfırlama |
| Güvenlik okuyucusu | Kimlik korumasına salt okuma erişimi | Kimlik koruması ekleme, kullanıcıları düzeltme, ilkeleri yapılandırma, parolaları sıfırlama |

Daha ayrıntılı bilgi için bkz. [Azure Active Directory yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md)

## <a name="detection"></a>Algılama

### <a name="vulnerabilities"></a>Güvenlik Açıkları

Yapılandırmanızı analiz Azure Active Directory Kimlik Koruması ve Kullanıcı kimliklerinizde etki verebilen güvenlik açıklarını algılar. Daha ayrıntılı bilgi için bkz. [Azure Active Directory kimlik koruması algılanan güvenlik açıkları](vulnerabilities.md).

### <a name="risk-detections"></a>Risk algılamaları

Azure Active Directory, Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılamak için uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Sistem algılanan her şüpheli eylem için bir kayıt oluşturur. Bu kayıtlar risk algılamaları olarak da bilinir.  
Daha fazla ayrıntı için bkz. [risk algılamalarını Azure Active Directory](../active-directory-identity-protection-risk-events.md).

## <a name="investigation"></a>Araştırma

Kimlik koruması ile ilgili yolculukta, genellikle kimlik koruma panosu ile başlar.

![Düzeltme](./media/overview/1000.png "Düzeltme")

Pano şunlara erişmenizi sağlar:

- **Risk için Işaretlenmiş kullanıcılar**, **risk algılamaları** ve **güvenlik açıkları** gibi raporlar
- **Güvenlik Ilkelerinizin**, **bildirimlerin** ve **Multi-Factor Authentication kaydının** yapılandırması gibi ayarlar

Genellikle düzeltme veya azaltma adımlarının gerekli olup olmadığına ve kimliğin nasıl olduğunu belirlemek için, risk algılamasıyla ilgili etkinlikleri, günlükleri ve diğer ilgili bilgileri gözden geçirme süreciniz olan araştırma için başlangıç noktasıdır. güvenliği aşılmış ve riskli kimliğin nasıl kullanıldığını anlayın.

Araştırma etkinliklerinizi e-posta başına gönderilen Azure Active Directory [bildirimlerine](notifications.md) bağlayabilirsiniz.

## <a name="policies"></a>İlkeler

Otomatikleştirilmiş yanıtları uygulamak için Azure Active Directory Kimlik Koruması üç ilke sağlar:

- [Multi-Factor Authentication kayıt ilkesi](howto-mfa-policy.md)
- [Kullanıcı risk ilkesi](howto-user-risk-policy.md)
- [Oturum açma risk ilkesi](howto-sign-in-risk-policy.md)

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Active Directory P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Kanal 9: Azure AD ve kimlik göster: Kimlik koruması önizlemesi](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
- [Azure Active Directory Kimlik Koruması etkinleştiriliyor](enable.md)
