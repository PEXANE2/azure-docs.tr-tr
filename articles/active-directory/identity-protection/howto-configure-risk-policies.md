---
title: Azure Active Directory Identity Protection 'da risk ilkeleri yapılandırma (YENİLENDİ) | Microsoft Docs
description: Azure Active Directory Identity Protection 'da risk ilkeleri yapılandırma (yenileme).
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ce4e2958978de9339f4340755e3740730025a5f
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334036"
---
# <a name="how-to-configure-risk-policies-in-azure-active-directory-identity-protection-refreshed"></a>Nasıl Yapılır: Azure Active Directory kimlik koruması 'nda risk ilkelerini yapılandırma (yenileme)

Azure AD, riskli olabilecek kimlikler için göstergeler olan risk olaylarını algılar. Risk ilkelerini yapılandırarak, algılama sonuçlarına yönelik otomatikleştirilmiş yanıtları tanımlayabilirsiniz:

- Oturum açma riski ilkesiyle, kullanıcının oturum açması sırasında algılanan gerçek zamanlı risk olaylarına bir yanıt yapılandırabilirsiniz. 
- Kullanıcı risk ilkesiyle, bir kullanıcı için zaman içinde algılanan tüm etkin kullanıcı risklerine yanıt yapılandırabilirsiniz.  

> [!VIDEO https://www.youtube.com/embed/zEsbbik-BTE]

## <a name="what-is-the-sign-in-risk-policy"></a>Oturum açma riski ilkesi nedir?

Azure AD, kullanıcının her oturum açmasını analiz eder. Çözümlemenin amacı, oturum açma işlemiyle birlikte gelen şüpheli eylemleri algılamadır. Örneğin, anonim bir IP adresi kullanılarak yapılan oturum açma işlemi mi, yoksa bilmediğiniz bir konumdan başlatılan oturum açma mı? Azure AD 'de sistemin algılayabildiği şüpheli eylemler risk olayları olarak da bilinir. Azure AD, oturum açma sırasında algılanan risk olaylarına bağlı olarak bir değeri hesaplar. Bu değer, oturum açmanın meşru Kullanıcı tarafından gerçekleştirilmediğini (düşük, orta, yüksek) temsil eder. Olasılığa, **oturum açma risk düzeyi**denir.

Oturum açma risk ilkesi, belirli bir oturum açma risk düzeyi için yapılandırabileceğiniz otomatik bir yanıttır. Yanıtlarınızda kaynaklarınıza erişimi engelleyebilir veya erişim kazanmak için Multi-Factor Authentication (MFA) sınamasını geçirmeyi zorunlu kılabilirsiniz.

Kullanıcı, oturum açma risk ilkesi tarafından tetiklenen bir MFA isteğini başarıyla tamamladığında, oturum açmanın meşru kullanıcıdan kaynaklandığı kimlik korumasına geri bildirimde bulunun. Bu nedenle, MFA isteğini tetikleyen oturum açma risk olayı otomatik olarak kapatılır ve kimlik koruması, bu olayın Kullanıcı riski yükselmesine katkıda bulunmak için izin vermez. Oturum açma riski ilkesinin etkinleştirilmesi riskli oturum açma işlemlerinin, kullanıcıların MFA için istendiğinde kendi kendini düzeltmesine ve ardından ilişkili riskli oturum açma işlemini otomatik olarak kapatmaya izin vererek, riskli oturum açma görünümündeki sürekliliği azaltabilir.

## <a name="how-do-i-access-the-sign-in-risk-policy"></a>Nasıl yaparım? oturum açma risk ilkesine erişin mi?
   
Oturum açma risk ilkesi, [Azure AD kimlik koruması sayfasındaki](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy) **Yapılandır** bölümünde bulunur.
   
![Oturum açma risk ilkesi](./media/howto-configure-risk-policies/1014.png "Oturum açma risk ilkesi")

## <a name="sign-in-risk-policy-settings"></a>Oturum açma risk ilkesi ayarları

Oturum açma risk ilkesini yapılandırırken şunları ayarlamanız gerekir:

- İlkenin geçerli olduğu kullanıcılar ve gruplar:

   ![Kullanıcılar ve gruplar](./media/howto-configure-risk-policies/11.png)

- İlkeyi tetikleyen oturum açma risk düzeyi:

   ![Oturum açma risk düzeyi](./media/howto-configure-risk-policies/12.png)

- Oturum açma riski düzeyi karşılandığında uygulanmasını istediğiniz erişimin türü:  

   ![Access](./media/howto-configure-risk-policies/13.png)

- İlkenizin durumu:

   ![İlkeyi zorla](./media/howto-configure-risk-policies/14.png)

İlke yapılandırma iletişim kutusu, yeniden yapılandırmanın etkilerini tahmin etmek için size bir seçenek sağlar.

![Tahmini etki](./media/howto-configure-risk-policies/15.png)

## <a name="what-you-should-know-about-sign-in-risk-policies"></a>Oturum açma riski ilkeleri hakkında bilmeniz gerekenler

MFA gerektirecek bir oturum açma risk güvenlik ilkesi yapılandırabilirsiniz:

![MFA gerektirme](./media/howto-configure-risk-policies/16.png)

Ancak, güvenlik nedenleriyle bu ayar yalnızca MFA için kaydedilmiş kullanıcılar için geçerlidir. Kimlik koruması, henüz MFA için kaydedilmediyse kullanıcılara MFA gereksinimini engeller.

Riskli oturum açma işlemleri için MFA 'yı zorunlu kılmak istiyorsanız şunları yapmalısınız:

1. Etkilenen kullanıcılar için Multi-Factor Authentication kayıt ilkesini etkinleştirin.
2. Etkilenen kullanıcıların MFA kaydı gerçekleştirmek için riskli olmayan bir oturumda oturum açmasını gerektir.

Bu adımların tamamlanması, çok faktörlü kimlik doğrulamasının riskli oturum açma için gerekli olmasını sağlar.

Oturum açma risk ilkesi:

- Modern kimlik doğrulaması kullanan tüm tarayıcı trafiğine ve oturum açma işlemleri için geçerlidir.
- ADFS gibi federe ıDP 'de WS-Trust uç noktasını devre dışı bırakarak eski güvenlik protokollerini kullanan uygulamalara uygulanmaz.

İlgili Kullanıcı deneyimine genel bir bakış için bkz.:

* [Riskli oturum açma kurtarması](flows.md#risky-sign-in-recovery)
* [Riskli oturum açma engellendi](flows.md#risky-sign-in-blocked)  
* [Azure AD Kimlik Koruması oturum açma deneyimleri](flows.md)  

## <a name="what-is-a-user-risk-policy"></a>Kullanıcı risk ilkesi nedir?

Azure AD, kullanıcının her oturum açmasını analiz eder. Çözümlemenin amacı, oturum açma işlemiyle birlikte gelen şüpheli eylemleri algılamadır. Azure AD 'de sistemin algılayabildiği şüpheli eylemler risk olayları olarak da bilinir. Bazı risk olayları gerçek zamanlı olarak algılanırken, daha fazla zaman gerektiren risk olayları da vardır. Örneğin, normal konumlara karşı imkansız bir yolculuğu algılamak için, sistemin normal davranışı hakkında bilgi edinmek için ilk öğrenme dönemi 14 gün gerektirir. Algılanan risk olaylarını çözümlemek için çeşitli seçenekler vardır. Örneğin, bireysel risk olaylarını el ile çözümleyebilir veya bir oturum açma riski veya Kullanıcı riski koşullu erişim ilkesi kullanarak bunları çözülebilir.

Bir kullanıcı için algılanan ve çözümlenemeyen tüm risk olayları etkin risk olayları olarak bilinir. Bir kullanıcıyla ilişkili etkin risk olayları Kullanıcı riski olarak bilinir. Azure AD, Kullanıcı riskine bağlı olarak, bir kullanıcının tehlikeye girdiği bir olasılığı (düşük, orta, yüksek) hesaplar. Olasılığa Kullanıcı risk düzeyi denir.

![Kullanıcı riskleri](./media/howto-configure-risk-policies/11031.png)

Kullanıcı risk ilkesi, belirli bir Kullanıcı risk düzeyi için yapılandırabileceğiniz otomatik bir yanıttır. Bir Kullanıcı risk ilkesiyle, bir kullanıcı hesabını temiz bir duruma geri almak için kaynaklarınıza erişimi engelleyebilir veya parola değişikliğini zorunlu kılabilirsiniz.

## <a name="how-do-i-access-the-user-risk-policy"></a>Nasıl yaparım? Kullanıcı risk ilkesine erişin mi?
   
Kullanıcı risk ilkesi, [Azure AD kimlik koruması sayfasındaki](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy) **Yapılandır** bölümünde bulunur.
   
![Kullanıcı riski ilkesi](./media/howto-configure-risk-policies/11014.png)

## <a name="user-risk-policy-settings"></a>Kullanıcı risk ilkesi ayarları

Kullanıcı risk ilkesini yapılandırırken şunları ayarlamanız gerekir:

- İlkenin geçerli olduğu kullanıcılar ve gruplar:

   ![Kullanıcılar ve gruplar](./media/howto-configure-risk-policies/111.png)

- İlkeyi tetikleyen oturum açma risk düzeyi:

   ![Kullanıcı risk düzeyi](./media/howto-configure-risk-policies/112.png)

- Oturum açma riski düzeyi karşılandığında uygulanmasını istediğiniz erişimin türü:  

   ![Access](./media/howto-configure-risk-policies/113.png)

- İlkenizin durumu:

   ![İlkeyi zorla](./media/howto-configure-risk-policies/114.png)

İlke yapılandırması iletişim kutusu size yapılandırmanızın etkisini tahmin etmek için bir seçenek sağlar.

![Tahmini etki](./media/howto-configure-risk-policies/115.png)

## <a name="what-you-should-know-about-user-risk-polices"></a>Kullanıcı risk ilkeleri hakkında bilmeniz gerekenler

Risk düzeyine bağlı olarak oturum açarken kullanıcıları engellemek için bir Kullanıcı risk güvenlik ilkesi ayarlayabilirsiniz.

![Engelleme](./media/howto-configure-risk-policies/116.png)

Oturum açmayı engelleme:

* Etkilenen Kullanıcı için Yeni Kullanıcı risk olaylarının oluşturulmasını engeller
* Yöneticilerin, kullanıcının kimliğini etkileyen risk olaylarını el ile düzeltmesini ve güvenli bir duruma geri yüklemesini sağlar

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

 [Kanal 9: Azure AD ve kimlik göster: Kimlik koruması önizlemesi](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
