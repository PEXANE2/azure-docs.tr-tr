---
title: Azure Active Directory Kimlik Koruması 'de Kullanıcı risk ilkesini yapılandırma | Microsoft Docs
description: Azure AD Kimlik Koruması Kullanıcı risk ilkesini nasıl yapılandıracağınızı öğrenin.
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
ms.openlocfilehash: 92bfb921833d99a3538ffa8c4c5d16a9f0cd3acd
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126295"
---
# <a name="how-to-configure-the-user-risk-policy"></a>Nasıl Yapılır: Kullanıcı riski ilkesini yapılandırma

Kullanıcı riskine sahip olan Azure AD, bir kullanıcı hesabının tehlikeye girdiği olasılığı algılar. Yönetici olarak, bir Kullanıcı riski koşullu erişim ilkesini belirli bir Kullanıcı risk düzeyine otomatik olarak yanıt verecek şekilde yapılandırabilirsiniz.
 
Bu makale, bir Kullanıcı risk ilkesini yapılandırmak için gereken bilgileri sağlar.

## <a name="what-is-a-user-risk-policy"></a>Kullanıcı risk ilkesi nedir?

Azure AD, kullanıcının her oturum açmasını analiz eder. Çözümlemenin amacı, oturum açma işlemiyle birlikte gelen şüpheli eylemleri algılamadır. Azure AD 'de sistemin algılayabildiği şüpheli eylemler risk algılamaları olarak da bilinir. Bazı risk algılamaları gerçek zamanlı olarak algılanırken, daha fazla zaman gerektiren risk algılamaları da vardır. Örneğin, normal konumlara karşı imkansız bir yolculuğu algılamak için, sistemin normal davranışı hakkında bilgi edinmek için ilk öğrenme dönemi 14 gün gerektirir. Algılanan risk algılamalarını çözümlemek için çeşitli seçenekler vardır. Örneğin, tek tek risk algılamalarını el ile çözümleyebilir veya bir oturum açma riski veya Kullanıcı riski koşullu erişim ilkesi kullanarak bunları çözümleirsiniz.

Bir kullanıcı için algılanan ve çözülmediği tüm risk algılamaları etkin risk algılamaları olarak bilinir. Bir kullanıcıyla ilişkili etkin risk algılamaları, Kullanıcı riski olarak bilinir. Azure AD, Kullanıcı riskine bağlı olarak, bir kullanıcının tehlikeye girdiği bir olasılığı (düşük, orta, yüksek) hesaplar. Olasılığa Kullanıcı risk düzeyi denir.

![Kullanıcı riskleri](./media/howto-user-risk-policy/1031.png)

Kullanıcı risk ilkesi, belirli bir Kullanıcı risk düzeyi için yapılandırabileceğiniz otomatik bir yanıttır. Bir Kullanıcı risk ilkesiyle, bir kullanıcı hesabını temiz bir duruma geri almak için kaynaklarınıza erişimi engelleyebilir veya parola değişikliğini zorunlu kılabilirsiniz.

## <a name="how-do-i-access-the-user-risk-policy"></a>Nasıl yaparım? Kullanıcı risk ilkesine erişin mi?
   
Oturum açma risk ilkesi, [Azure AD kimlik koruması sayfasındaki](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy) **Yapılandır** bölümünde bulunur.
   
![Kullanıcı riski ilkesi](./media/howto-user-risk-policy/1014.png)

## <a name="policy-settings"></a>İlke ayarları

Oturum açma risk ilkesini yapılandırırken şunları ayarlamanız gerekir:

- İlkenin geçerli olduğu kullanıcılar ve gruplar:

    ![Kullanıcılar ve gruplar](./media/howto-user-risk-policy/11.png)

- İlkeyi tetikleyen oturum açma risk düzeyi:

    ![Kullanıcı risk düzeyi](./media/howto-user-risk-policy/12.png)

- Oturum açma riski düzeyi karşılandığında uygulanmasını istediğiniz erişimin türü:  

    ![Access](./media/howto-user-risk-policy/13.png)

- İlkenizin durumu:

    ![İlkeyi zorla](./media/howto-user-risk-policy/14.png)

İlke yapılandırması iletişim kutusu size yapılandırmanızın etkisini tahmin etmek için bir seçenek sağlar.

![Tahmini etki](./media/howto-user-risk-policy/15.png)

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

Risk düzeyine bağlı olarak oturum açarken kullanıcıları engellemek için bir Kullanıcı risk güvenlik ilkesi ayarlayabilirsiniz.

![Engelleme](./media/howto-user-risk-policy/16.png)

Oturum açmayı engelleme:

* Etkilenen Kullanıcı için Yeni Kullanıcı risk algılamaları oluşturulmasını engeller
* Yöneticilerin, kullanıcının kimliğini etkileyen risk algılamalarını el ile düzeltmesini ve güvenli bir duruma geri yüklemesini sağlar

## <a name="best-practices"></a>En iyi uygulamalar

**Yüksek** bir eşik seçilmesi, bir ilkenin tetiklenme sayısını azaltır ve kullanıcılara etkiyi en aza indirir.
Ancak, ilkeden riskli olarak işaretlenen **düşük** ve **Orta ölçekli** kullanıcıları, daha önce şüphelenildiği veya tehlikeye atıldığı bilinen kimliklerin veya cihazların güvenliğini dışlamayabilir.

İlke ayarlanırken,

* Büyük miktarda hatalı pozitif sonuç üreten kullanıcıları hariç tut (geliştiriciler, Güvenlik analistleri)
* İlkenin etkinleştirilmesi pratik olmayan yerel ayarlarda (örneğin, yardım masasına erişim yok) kullanıcıları hariç tutun
* İlk ilke kullanıma alma sırasında **yüksek** bir eşik kullanın veya son kullanıcılar tarafından görülen zorlukları en aza indirmeli.
* Kuruluşunuz daha fazla güvenlik gerektiriyorsa, **düşük** bir eşik kullanın. **Düşük** bir eşiğin seçilmesi, ek kullanıcı oturum açma güçlükleri sunarak daha fazla güvenlik sağlar.

Çoğu kuruluş için önerilen varsayılan değer, **Orta** eşiğe yönelik bir kuralı, kullanılabilirlik ve güvenlik arasında bir denge altına alacak şekilde yapılandırmaktır.

İlgili Kullanıcı deneyimine genel bir bakış için bkz.:

* [Güvenliği aşılmış hesap kurtarma akışı](flows.md#compromised-account-recovery).  
* [Güvenliği aşılmış hesap engellendi akışı](flows.md#compromised-account-blocked).  

**İlgili yapılandırma iletişim kutusunu açmak için**:

- **Azure AD kimlik koruması** dikey penceresinde, **Yapılandır** bölümünde, **Kullanıcı risk ilkesi**' ne tıklayın.

    ![Kullanıcı risk ilkesi](./media/howto-user-risk-policy/1009.png "Kullanıcı risk ilkesi")

## <a name="next-steps"></a>Sonraki adımlar

Azure AD Kimlik Koruması genel bakış almak için bkz. [Azure AD kimlik koruması genel bakış](overview.md).
