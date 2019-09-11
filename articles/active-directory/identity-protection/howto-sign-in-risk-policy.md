---
title: Azure Active Directory Kimlik Koruması oturum açma risk ilkesini yapılandırma | Microsoft Docs
description: Azure AD Kimlik Koruması oturum açma risk ilkesini nasıl yapılandıracağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: d00376c6689b6be773f24e8acd09c3697fb6a799
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126303"
---
# <a name="how-to-configure-the-sign-in-risk-policy"></a>Nasıl Yapılır: Oturum açma risk ilkesini yapılandırma

Azure Active Directory, [risk algılama türlerini](../reports-monitoring/concept-risk-events.md#risk-detection-types) gerçek zamanlı ve çevrimdışı olarak algılar. Bir kullanıcının oturum açması için algılanan her risk algılaması, riskli oturum açma adlı bir mantıksal kavram 'e katkıda bulunur. Riskli oturum açma, bir kullanıcı hesabının meşru sahibi tarafından gerçekleştirilmiş olabilecek bir oturum açma girişimine yönelik bir göstergedir.

## <a name="what-is-the-sign-in-risk-policy"></a>Oturum açma riski ilkesi nedir?

Azure AD, kullanıcının her oturum açmasını analiz eder. Çözümlemenin amacı, oturum açma işlemiyle birlikte gelen şüpheli eylemleri algılamadır. Örneğin, anonim bir IP adresi kullanılarak yapılan oturum açma işlemi mi, yoksa bilmediğiniz bir konumdan başlatılan oturum açma mı? Azure AD 'de sistemin algılayabildiği şüpheli eylemler risk algılamaları olarak da bilinir. Azure AD, oturum açma sırasında algılanan risk algılamalarını temel alarak bir değeri hesaplar. Bu değer, oturum açmanın meşru Kullanıcı tarafından gerçekleştirilmediğini (düşük, orta, yüksek) temsil eder. Olasılığa, **oturum açma risk düzeyi**denir.

Oturum açma risk ilkesi, belirli bir oturum açma risk düzeyi için yapılandırabileceğiniz otomatik bir yanıttır. Yanıtlarınızda kaynaklarınıza erişimi engelleyebilir veya erişim kazanmak için Multi-Factor Authentication (MFA) sınamasını geçirmeyi zorunlu kılabilirsiniz.
   
## <a name="how-do-i-access-the-sign-in-risk-policy"></a>Nasıl yaparım? oturum açma risk ilkesine erişin mi?
   
Oturum açma risk ilkesi, [Azure AD kimlik koruması sayfasındaki](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy) **Yapılandır** bölümünde bulunur.
   
![Oturum açma risk ilkesi](./media/howto-sign-in-risk-policy/1014.png "Oturum açma risk ilkesi")

## <a name="policy-settings"></a>İlke ayarları

Oturum açma risk ilkesini yapılandırırken şunları ayarlamanız gerekir:

- İlkenin geçerli olduğu kullanıcılar ve gruplar:

    ![Kullanıcılar ve gruplar](./media/howto-sign-in-risk-policy/11.png)

- İlkeyi tetikleyen oturum açma risk düzeyi:

    ![Oturum açma risk düzeyi](./media/howto-sign-in-risk-policy/12.png)

- Oturum açma riski düzeyi karşılandığında uygulanmasını istediğiniz erişimin türü:  

    ![Access](./media/howto-sign-in-risk-policy/13.png)

- İlkenizin durumu:

    ![İlkeyi zorla](./media/howto-sign-in-risk-policy/14.png)

İlke yapılandırma iletişim kutusu, yeniden yapılandırmanın etkilerini tahmin etmek için size bir seçenek sağlar.

![Tahmini etki](./media/howto-sign-in-risk-policy/15.png)

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

MFA gerektirecek bir oturum açma risk güvenlik ilkesi yapılandırabilirsiniz:

![MFA gerektirme](./media/howto-sign-in-risk-policy/16.png)

Ancak, güvenlik nedenleriyle bu ayar yalnızca MFA için kaydedilmiş kullanıcılar için geçerlidir. Kimlik koruması, henüz MFA için kaydedilmediyse kullanıcılara MFA gereksinimini engeller.

Riskli oturum açma işlemleri için MFA 'yı zorunlu kılmak istiyorsanız şunları yapmalısınız:

1. Etkilenen kullanıcılar için [Multi-Factor Authentication kayıt ilkesini](howto-mfa-policy.md) etkinleştirin.
2. Etkilenen kullanıcıların MFA kaydı gerçekleştirmek için riskli olmayan bir oturumda oturum açmasını gerektir.

Bu adımların tamamlanması, çok faktörlü kimlik doğrulamasının riskli oturum açma için gerekli olmasını sağlar.

Oturum açma risk ilkesi:

- Modern kimlik doğrulaması kullanan tüm tarayıcı trafiğine ve oturum açma işlemleri için geçerlidir.
- ADFS gibi federe ıDP 'de WS-Trust uç noktasını devre dışı bırakarak eski güvenlik protokollerini kullanan uygulamalara uygulanmaz.

İlgili Kullanıcı deneyimine genel bir bakış için bkz.:

* [Riskli oturum açma kurtarması](flows.md#risky-sign-in-recovery)
* [Riskli oturum açma engellendi](flows.md#risky-sign-in-blocked)  
* [Azure AD Kimlik Koruması oturum açma deneyimleri](flows.md)  

## <a name="best-practices"></a>En iyi uygulamalar

**Yüksek** bir eşik seçilmesi, bir ilkenin tetiklenme sayısını azaltır ve kullanıcılara etkiyi en aza indirir.  

Ancak, ilkeden riskli olarak işaretlenmiş **düşük** ve **Orta düzeyde** oturum açma işlemlerini dışlayıp, bir saldırganın güvenliği aşılmış bir kimlikle yararlanmasını engellemeyebilir.

İlke ayarlanırken,

- Çok faktörlü kimlik doğrulamasına sahip olmayan ve olmayan kullanıcıları hariç tut
- İlkenin etkinleştirilmesi pratik olmayan yerel ayarlarda (örneğin, yardım masasına erişim yok) kullanıcıları hariç tutun
- Büyük olasılıkla çok sayıda hatalı pozitif sonuç üreten kullanıcıları hariç tut (geliştiriciler, Güvenlik analistleri)
- İlk ilke toplaması sırasında **yüksek** bir eşik kullanın veya son kullanıcılar tarafından görülen zorlukları en aza indirmeli.
- Kuruluşunuz daha fazla güvenlik gerektiriyorsa, **düşük** bir eşik kullanın. **Düşük** bir eşiğin seçilmesi, ek kullanıcı oturum açma güçlükleri sunarak daha fazla güvenlik sağlar.

Çoğu kuruluş için önerilen varsayılan değer, **Orta** eşiğe yönelik bir kuralı, kullanılabilirlik ve güvenlik arasında bir denge altına alacak şekilde yapılandırmaktır.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD Kimlik Koruması genel bakış almak için bkz. [Azure AD kimlik koruması genel bakış](overview.md).
