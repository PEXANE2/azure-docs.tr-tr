---
title: Koşullu Erişim - Birleştirilmiş güvenlik bilgileri - Azure Etkin Dizini
description: Güvenlik bilgileri kaydı için özel bir Koşullu Erişim ilkesi oluşturma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f69a94e17155ff93510d09f666bce12f628274f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295167"
---
# <a name="conditional-access-securing-security-info-registration"></a>Koşullu Erişim: Güvenlik bilgileri kaydının güvenliğini sağlama

Kullanıcıların Azure Çok Faktörlü Kimlik Doğrulama ve self servis parola sıfırlama için ne zaman ve nasıl kaydolduklarını güvence altına almak, Koşullu Erişim ilkesindeki kullanıcı eylemleriyle artık mümkün. Bu önizleme özelliği, [birleştirilmiş kayıt önizlemesini](../authentication/concept-registration-mfa-sspr-combined.md)etkinleştiren kuruluşlar tarafından kullanılabilir. Bu işlevsellik, Azure Çok Faktörlü Kimlik Doğrulaması ve self servis parola sıfırlama (SSPR) için kaydolmak için erişimi kısıtlamak için güvenilen ağ konumu gibi koşulları kullanmak istedikleri kuruluşlarda etkinleştirilebilir. Kullanılabilir koşullar hakkında daha fazla bilgi için [Koşullu Erişim makalesine bakın: Koşullar.](concept-conditional-access-conditions.md)

## <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Güvenilir bir konumdan kayıt gerektiren bir ilke oluşturma

Aşağıdaki ilke, birleştirilmiş kayıt deneyimini kullanarak kaydolmaya çalışan tüm seçili kullanıcılar için geçerlidir ve güvenilir ağ olarak işaretlenmiş bir konumdan bağlanmadıkları sürece erişimi engeller.

1. Azure **portalında**Azure **Etkin Dizin** > **Güvenliği** > **Koşullu Erişimine**göz atın.
1. **Yeni ilke**yi seçin.
1. Ad olarak, bu ilke için bir Ad girin. Örneğin, **Güvenilir Ağlarda Birleşik Güvenlik Bilgisi Kaydı.**
1. **Atamalar** **altında, Kullanıcıları ve grupları**seçin ve bu politikanın uygulanmasını istediğiniz kullanıcıları ve grupları seçin.

   > [!WARNING]
   > Birleştirilmiş [kayıt önizlemesi](../authentication/howto-registration-mfa-sspr-combined.md)için kullanıcılar etkinleştirilmelidir.

1. **Bulut uygulamaları veya eylemleri**altında, Kullanıcı **eylemlerini**seçin, **Güvenlik bilgilerini kaydet (önizleme)** seçeneğini işaretleyin.
1. **Koşullar** > **Altında Yerler**.
   1. **Evet'i**yapılandır.
   1. **Herhangi bir konum**ekle.
   1. **Tüm güvenilen konumları**hariç tut.
   1. Yerlerde **Bitti'yi** seçin.
   1. Koşullar bıçak **üzerinde Bitti'yi** seçin.
1. **Koşullar** > Altında**İstemci uygulamaları (Önizleme)**, **Evet** **yapılaşını** ayarlayın ve **Bitti'yi**seçin.
1. **Erişim denetimleri** > altında**Grant**.
   1. **Erişimi Engelle'yi**seçin.
   1. Ardından **Seç**'e tıklayın.
1. **İlkeyi etkinleştir**’i **Açık** duruma getirin.
1. Ardından **Kaydet'i**seçin.

Bu politikanın 6. Yukarıdaki ilke, güvenilir bir ağ konumundan kayıt gerektirir. Kuruluşlar Konumlar yerine mevcut koşulları kullanmayı **seçebilirler.** Bu ilkenin bir blok ilkesi olduğunu unutmayın, bu nedenle dahil edilen her şey engellenir ve dahil olanla eşleşmeyen her şeye izin verilir. 

Bazıları yukarıdaki adım 6'da konum yerine aygıt durumunu kullanmayı seçebilir:

6. **Koşullar** > Altında**Aygıt durumu (Önizleme)**.
   1. **Evet'i**yapılandır.
   1. **Tüm aygıt durumunu**ekleyin.
   1. Aygıt **Hibrit Azure AD'yi ve/veya** **Uyumlu olarak işaretlenmiş Aygıtı** hariç tut
   1. Yerlerde **Bitti'yi** seçin.
   1. Koşullar bıçak **üzerinde Bitti'yi** seçin.

> [!WARNING]
> İlkenizde aygıt durumunu bir koşul olarak kullanırsanız, bu durum dizindeki konuk kullanıcıları etkileyebilir. [Yalnızca rapor modu,](concept-conditional-access-report-only.md) ilke kararlarının etkisini belirlemeye yardımcı olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu Erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu Erişim Ne Varsa aracını kullanarak oturum açma davranışını simüle edin](troubleshoot-conditional-access-what-if.md)
