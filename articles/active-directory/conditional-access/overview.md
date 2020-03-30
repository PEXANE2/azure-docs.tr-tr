---
title: Azure Etkin Dizinde Koşullu Erişim Nedir?
description: Conditional Access'in yeni kimlik odaklı kontrol düzleminin kalbinde nasıl olduğunu öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 09/17/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b044a4fd4e29bfe35abff7a4b36e5bae783328b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240915"
---
# <a name="what-is-conditional-access"></a>Koşullu Erişim nedir?

Modern güvenlik alanı artık bir kuruluşun ağının ötesine kullanıcı ve aygıt kimliğini içerecek şekilde genişletilir. Kuruluşlar bu kimlik sinyallerini erişim denetimi kararlarının bir parçası olarak kullanabilirler. 

Koşullu Erişim, Azure Active Directory tarafından sinyalleri bir araya getirmek, kararlar almak ve kuruluş ilkelerini uygulamak için kullanılan araçtır. Conditional Access, yeni kimlik güdümlü kontrol uçağının kalbinde yer alan bir yerdir.

![Kavramsal Koşullu sinyal artı uygulama almak için karar](./media/overview/conditional-access-signal-decision-enforcement.png)

Koşullu Erişim ilkeleri en basit haliyle, bir kullanıcı bir kaynağa erişmek istiyorsa, bir eylemi tamamlaması gerekir. Örnek: Bordro yöneticisi bordro uygulamasına erişmek ister ve bu uygulamaya erişmek için çok faktörlü kimlik doğrulaması gerçekleştirmesi gerekir.

Yöneticiler iki temel hedefle karşı karşıyadır:

- Kullanıcıların her yerde ve her zaman üretken olmasını sağlama
- Kuruluşun varlıklarını koruma

Koşullu Erişim ilkelerini kullanarak, kuruluşunuzun güvenliğini sağlamak ve gerekmediğinde kullanıcınızın yolundan uzak durmak için gerektiğinde doğru erişim denetimlerini uygulayabilirsiniz.

![Kavramsal Koşullu Erişim süreci akışı](./media/overview/conditional-access-overview-how-it-works.png)

Koşullu Erişim ilkeleri, ilk faktör kimlik doğrulaması tamamlandıktan sonra uygulanır. Koşullu Erişim, hizmet reddi (DoS) saldırıları gibi senaryolar için bir kuruluşun ilk savunma hattı olarak tasarlanmamıştır, ancak erişimi belirlemek için bu olaylardan gelen sinyalleri kullanabilir.

## <a name="common-signals"></a>Ortak sinyaller

Koşullu Erişim'in bir ilke kararı verirken dikkate alabileceği yaygın sinyaller aşağıdaki sinyalleri içerir:

- Kullanıcı veya grup üyeliği
   - İlkeler, yöneticilere erişim üzerinde ayrıntılı denetim sağlayan belirli kullanıcıları ve grupları hedefleyebilir.
- IP Konum bilgileri
   - Kuruluşlar, ilke kararları verirken kullanılabilecek güvenilir IP adresi aralıkları oluşturabilir. 
   - Yöneticiler, trafiğinengellenmesini veya trafiğe izin vermek için tüm ülke IP aralıklarını belirtebilir.
- Cihaz
   - Koşullu Erişim ilkeleri zorlarken belirli platformlarda veya belirli bir durumla işaretlenmiş aygıtlara sahip kullanıcılar kullanılabilir.
- Uygulama
   - Belirli uygulamalara erişmeye çalışan kullanıcılar farklı Koşullu Erişim ilkelerini tetikleyebilir. 
- Gerçek zamanlı ve hesaplanan risk tespiti
   - Azure AD Kimlik Koruması ile Sinyal tümleştirmesi, Koşullu Erişim ilkelerinin riskli oturum açma davranışını tanımlamasına olanak tanır. İlkeler daha sonra kullanıcıları, risk düzeylerini azaltmak için parola değişiklikleri veya çok faktörlü kimlik doğrulaması gerçekleştirmeye veya yönetici el ile eyleme geçene kadar erişimlerinin engellenmesine zorlayabilir.
- Microsoft Bulut Uygulama Güvenliği (MCAS)
   - Kullanıcı uygulaması erişiminin ve oturumlarının gerçek zamanlı olarak izlenmesini ve kontrol edilmesini sağlayarak bulut ortamınıza erişim ve gerçekleştirilen etkinlikler üzerinde görünürlüğü ve denetimi artırır.

## <a name="common-decisions"></a>Ortak kararlar

- Hizmete erişimi
   - En kısıtlayıcı karar
- Erişim verme
   - En az kısıtlayıcı karar, yine de aşağıdaki seçeneklerden birini veya daha fazlasını gerektirebilir:
      - Çok faktörlü kimlik doğrulama gerektirir
      - Aygıtın uyumlu olarak işaretlemesi
      - Karma Azure AD'nin birleştirilmiş aygıtı gerektirmesi
      - Onaylı istemci uygulaması gerektirir
      - Uygulama koruma ilkesi (önizleme) gerektirir

## <a name="commonly-applied-policies"></a>Yaygın olarak uygulanan ilkeler

Birçok kuruluşun, Koşullu Erişim ilkelerinin şu konularda yardımcı olabileceğine dair ortak erişim kaygıları vardır:

- İdari rolleri olan kullanıcılar için çok faktörlü kimlik doğrulaması gerektirme
- Azure yönetim görevleri için çok faktörlü kimlik doğrulama gerektirmesi
- Eski kimlik doğrulama protokollerini kullanmaya çalışan kullanıcılar için oturum açma ları engelleme
- Azure Çok Faktörlü Kimlik Doğrulama kaydı için güvenilir konumlar gerektirme
- Belirli konumlardan erişimi engelleme veya verme
- Riskli oturum açma davranışlarını engelleme
- Belirli uygulamalar için kuruluş tarafından yönetilen aygıtlar gerektirme

## <a name="customer-case-studies"></a>Müşteri örnek olay incelemeleri

Otomatik erişim denetimi kararlarını tanımlamak ve uygulamak için diğer kuruluşların Azure AD Koşullu Erişimi nasıl kullandığını keşfedin. Aşağıdaki özellikli haberler, bu müşteri gereksinimlerinin nasıl karşılandığını gösterir.

* [Wipro, müşteri etkileşimlerini geliştirmek için Microsoft bulut güvenliği araçlarıyla mobil üretkenliği artırır.](https://customers.microsoft.com/story/wipro-professional-services-enterprise-mobility-security) Azure AD'deki Koşullu Erişim ilkeleri, şirketin kendi kimlik bilgilerini kullanabilen ---diğer yandan kendi kurumsal verileri üzerinde denetim sağlayarak belgeleri, kaynakları ve uygulamaları güvenilir dış kuruluşlarla paylaşmasını --- sağladı.
* [Aramex teslimat sınırlı - Küresel lojistik ve taşımacılık şirketi kimlik ve erişim yönetimi çözümü ile buluta bağlı ofis oluşturur.](https://customers.microsoft.com/story/aramex-azure-active-directory-travel-transportation-united-arab-emirates-en) Aramex'in uzak çalışanları için güvenli erişim sağlamak özellikle zordu. Şirket şimdi bu uzak çalışanların saas uygulamalarına ağ dışından erişmelerini sağlamak için Koşullu Erişim'i uyguluyor. Koşullu Erişim kuralı, Çok Faktörlü Kimlik Doğrulama'yı uygulayıp uygulamamaya karar vererek yalnızca doğru kişilere doğru erişimi sağlayacaktır.

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

Microsoft [365 İşletme lisanslarına](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) sahip müşteriler de Koşullu Erişim özelliklerine erişebilir. 

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ilkesini parça parça oluşturma](concept-conditional-access-policies.md)

Ortamınızda Koşullu Erişim'i nasıl uygulayacağınızı öğrenmek için Azure [Etkin Dizini'nde Koşullu Erişim dağıtımınızı](plan-conditional-access.md)planlayın'a bakın.

[Kimlik Koruması hakkında bilgi edinin](../identity-protection/overview-v2.md)

[Microsoft Cloud App Security hakkında bilgi edinin](/cloud-app-security/what-is-cloud-app-security)

[Microsoft Intune hakkında bilgi edinin](/intune/index)