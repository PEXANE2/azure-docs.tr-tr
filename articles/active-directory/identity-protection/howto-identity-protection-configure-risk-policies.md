---
title: Risk ilkeleri - Azure Active Directory Identity Protection
description: Azure Active Directory Identity Protection'da risk ilkelerini etkinleştirme ve yapılandırma
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ffa08f7ebf013d42d6da0589ce0f1ccc97289de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75707014"
---
# <a name="how-to-configure-and-enable-risk-policies"></a>Nasıl Yapilir: Risk ilkelerini yapılandırma ve etkinleştirme

Bir önceki makalede öğrendiğimiz gibi, [Kimlik Koruma politikaları](concept-identity-protection-policies.md) dizinimizde etkinleştirebileceğimiz iki risk politikamız vardır. 

- Oturum açma risk ilkesi
- Kullanıcı risk ilkesi

![Kullanıcı ve oturum açma risk ilkelerini etkinleştirmek için güvenliğe genel bakış sayfası](./media/howto-identity-protection-configure-risk-policies/identity-protection-security-overview.png)

Her iki ilke de ortamınızdaki risk algılamalarına verilen yanıtı otomatikleştirmek ve risk algılandığında kullanıcıların kendi kendini düzeltmelerine olanak sağlamak için çalışır. 

> [!VIDEO https://www.youtube.com/embed/zEsbbik-BTE]

## <a name="prerequisites"></a>Ön koşullar 

Kuruluşunuz, riskler algılandığında kullanıcıların kendi kendini düzeltmesine izin vermek istiyorsa, kullanıcıların hem self servis parola sıfırlama hem de Azure Çok Faktörlü Kimlik Doğrulaması için kaydedilmesi gerekir. En iyi deneyimi yaşamak için [birleştirilmiş güvenlik bilgi kaydı deneyimini etkinleştirmenizi](../authentication/howto-registration-mfa-sspr-combined.md) öneririz. Kullanıcıların kendi kendilerini düzeltmelerine izin vermek, yönetici müdahalesine gerek kalmadan onları daha hızlı üretken bir duruma geri getirir. Yöneticiler bu olayları görmeye devam edebilir ve olaydan sonra bunları araştırabilirler. 

## <a name="choosing-acceptable-risk-levels"></a>Kabul edilebilir risk düzeylerini seçme

Kuruluşlar, kullanıcı deneyimini ve güvenlik duruşunu dengelemeyi kabul etmeye istekli oldukları risk düzeyine karar vermelidir. 

Microsoft'un önerisi, kullanıcı risk ilkesi eşiğini **Yüksek** ve oturum açma risk ilkesini **Orta ve üstü**olarak ayarlamaktır.

**Yüksek** eşik seçmek, bir ilkenin tetiklenme sayısını azaltır ve kullanıcılar üzerindeki etkisini en aza indirir. Ancak, bir saldırganın gizliliği ihlal edilen bir kimlikten yararlanabını engellemeyebilir, **düşük** ve **orta** riskli algılamaları ilkenin dışında tutar. **Düşük** eşleği seçmek ek kullanıcı kesintileri sunar, ancak güvenlik duruşunu artırır.

## <a name="exclusions"></a>Dışlamalar

Tüm [ilkeler, acil durum erişiminiz veya kesme cam yöneticisi hesaplarınız](../users-groups-roles/directory-emergency-access.md)gibi kullanıcıları hariç taramaya olanak sağlar. Kuruluşlar, hesapların kullanılma şekline bağlı olarak diğer hesapları belirli ilkelerden hariç tutmaları gerektiğini belirleyebilir. Tüm dışlamalar hala geçerli olup olmadığını görmek için düzenli olarak gözden geçirilmelidir.

Yapılandırılmış güvenilir [ağ konumları,](../conditional-access/location-condition.md) yanlış pozitif leri azaltmak için bazı risk algılamalarında Kimlik Koruması tarafından kullanılır.

## <a name="enable-policies"></a>İlkeleri etkinleştirme

Kullanıcı riski ve oturum açma risk ilkelerinin aşağıdaki adımları tamamlamasını sağlamak için.

1. [Azure portalına](https://portal.azure.com) gidin.
1. **Azure Etkin Dizin** > **Güvenlik** > **Kimlik Koruması** > **Genel Bakış'a**göz atın.
1. **Kullanıcı risk ilkesini yapılandır'ı**seçin.
   1. **Atamalar** Altında
      1. **Kullanıcılar** - Ürününüzü sınırlandırıyorsa **Tüm kullanıcıları** seçin veya kişileri ve **grupları seçin.**
         1. İsteğe bağlı olarak, kullanıcıları ilkeden hariç tutmayı seçebilirsiniz.
      1. **Koşullar** - **Kullanıcı riski** Microsoft'un önerisi yüksek bu seçeneği ayarlamaktır. **High**
   1. **Denetimler** Altında
      1. **Access** - Microsoft'un önerisi **erişime izin** vermek ve **parola değişikliği ni gerektirmektir.**
   1. **Politikayı** - **Uygula**
   1. **Kaydet** - Bu eylem sizi **Genel Bakış** sayfasına döndürecektir.
1. **Oturum açma risk ilkesini yapılandır'ı**seçin.
   1. **Atamalar** Altında
      1. **Kullanıcılar** - Ürününüzü sınırlandırıyorsa **Tüm kullanıcıları** seçin veya kişileri ve **grupları seçin.**
         1. İsteğe bağlı olarak, kullanıcıları ilkeden hariç tutmayı seçebilirsiniz.
      1. **Koşullar** - **Oturum Açma riski** Microsoft'un önerisi bu seçeneği Orta ve **üzeri**olarak ayarlamaktır.
   1. **Denetimler** Altında
      1. **Access** - Microsoft'un önerisi **erişime izin** vermek ve **çok faktörlü kimlik doğrulamayı**gerektirmektir.
   1. **Politikayı** - **Uygula**
   1. **Kaydet**

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Çok Faktörlü Kimlik Doğrulama kayıt ilkesini etkinleştirme](howto-identity-protection-configure-mfa-policy.md)

- [Risk nedir](concept-identity-protection-risks.md)

- [Risk algılamalarını araştırma](howto-identity-protection-investigate-risk.md)

- [Risk algılamalarını simüle edin](howto-identity-protection-simulate-risk.md)
