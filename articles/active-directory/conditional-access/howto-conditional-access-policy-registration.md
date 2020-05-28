---
title: Koşullu erişim-Birleşik güvenlik bilgileri-Azure Active Directory
description: Güvenlik bilgileri kaydı için özel bir koşullu erişim ilkesi oluşturma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: acf17f4cef43fe4221781deedb130ef587208550
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83993754"
---
# <a name="conditional-access-securing-security-info-registration"></a>Koşullu erişim: güvenlik bilgilerini kaydettirme güvenliğini sağlama

Kullanıcıların Azure Multi-Factor Authentication ve self servis parola sıfırlaması için ne zaman ve nasıl kaydoldığına ilişkin güvenlik, koşullu erişim ilkesindeki Kullanıcı eylemleri ile mümkün değildir. Bu önizleme özelliği, [birleştirilmiş kayıt önizlemesini](../authentication/concept-registration-mfa-sspr-combined.md)etkinleştirmiş kuruluşlar tarafından kullanılabilir. Bu işlev, Azure Multi-Factor Authentication ve self servis parola sıfırlama (SSPR) için kaydolmaya erişimi kısıtlamak üzere güvenilen ağ konumu gibi koşullar kullanmak istedikleri kuruluşlarda etkinleştirilebilir. Kullanılabilir koşullar hakkında daha fazla bilgi için [koşullu erişim: koşullar](concept-conditional-access-conditions.md)makalesine bakın.

## <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Güvenilen bir konumdan kayıt gerektirecek bir ilke oluşturun

Aşağıdaki ilke, birleştirilmiş kayıt deneyimini kullanarak kaydolmayı deneyen ve güvenilen ağ olarak işaretlenen bir konumdan bağlanmadıkları sürece erişimi engelleyen tüm seçili kullanıcılar için geçerlidir.

1. **Azure Portal** **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişimi**' ne gidin.
1. **Yeni ilke**' yi seçin.
1. Ad alanına bu ilke için bir ad girin. Örneğin, **güvenilir ağlarda Birleşik güvenlik bilgileri kaydı**.
1. **Atamalar**' ın altında, **Kullanıcılar ve gruplar**' ı seçin ve bu ilkenin uygulanmasını istediğiniz kullanıcıları ve grupları seçin.

   > [!WARNING]
   > [Birleşik kayıt](../authentication/howto-registration-mfa-sspr-combined.md)için kullanıcıların etkinleştirilmiş olması gerekir.

1. **Bulut uygulamaları veya eylemler**altında **Kullanıcı eylemleri**' ni seçin, **güvenlik bilgilerini kaydet**' i işaretleyin.
1. **Koşul**  >  **konumları**altında.
   1. **Evet 'i**yapılandırın.
   1. **Herhangi bir konum**ekleyin.
   1. **Tüm güvenilen konumları**hariç tutun.
   1. Konumlar dikey penceresinde **bitti** ' yi seçin.
   1. Koşullar dikey penceresinde **bitti** ' yi seçin.
1. **Koşullar**  >  **istemci uygulamaları (Önizleme)** altında **Yapılandır** ' ı **Evet**olarak ayarlayın ve **bitti**' yi seçin.
1. **Erişim denetimleri**  >  **izni**altında.
   1. **Erişimi engelle**' yi seçin.
   1. Ardından **Seç**'e tıklayın.
1. **İlkeyi etkinleştir**’i **Açık** duruma getirin.
1. Sonra **Kaydet**' i seçin.

Bu ilkedeki 6. adımda, kuruluşların yapabilecekleri seçimler vardır. Yukarıdaki ilke, güvenilir bir ağ konumundan kayıt gerektiriyor. Kuruluşlar, **konumların**yerine tüm kullanılabilir koşulları kullanmayı seçebilir. Bu ilkenin bir blok ilkesi olduğunu, bu nedenle dahil edilen herhangi bir şeyin engellenmesini ve dahil etme ile eşleşmeyen herhangi bir şeye izin verildiğini unutmayın. 

Bazıları, yukarıdaki 6. adımdaki konum yerine cihaz durumunu kullanmayı seçebilir:

6. **Koşullar**  >  **cihaz durumu (Önizleme)** altında.
   1. **Evet 'i**yapılandırın.
   1. **Tüm cihaz durumunu**ekleyin.
   1. **Cihaza karma Azure AD 'ye katılmış** ve/veya **uyumlu olarak işaretlenmiş cihaz** hariç tut
   1. Konumlar dikey penceresinde **bitti** ' yi seçin.
   1. Koşullar dikey penceresinde **bitti** ' yi seçin.

> [!WARNING]
> İlkenizde bir koşul olarak cihaz durumunu kullanıyorsanız, bu, dizindeki Konuk kullanıcıları etkileyebilir. [Yalnızca rapor modu](concept-conditional-access-report-only.md) , ilke kararlarının etkisini belirlemenize yardımcı olabilir.
> Yalnızca rapor modunun "Kullanıcı eylemleri" kapsamına sahip CA ilkeleri için geçerli olmadığına unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)
