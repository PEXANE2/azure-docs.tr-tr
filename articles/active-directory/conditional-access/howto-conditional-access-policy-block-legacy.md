---
title: Koşullu erişim-blok eski kimlik doğrulaması-Azure Active Directory
description: Eski kimlik doğrulama protokollerini engellemek için özel bir koşullu erişim ilkesi oluşturma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 08/07/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0eaa2f2bc0ee26a4f543ea0938db6c226a9a6575
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89049256"
---
# <a name="conditional-access-block-legacy-authentication"></a>Koşullu erişim: eski kimlik doğrulamasını engelle

Eski kimlik doğrulama protokolleriyle ilişkili risk artışı nedeniyle, Microsoft kuruluşların bu protokolleri kullanarak kimlik doğrulama isteklerini engellemesini ve modern kimlik doğrulaması gerektirmesini önerir.

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Aşağıdaki adımlar, eski kimlik doğrulama isteklerini engellemek için bir koşullu erişim ilkesi oluşturmanıza yardımcı olur. Bu ilke, yöneticilerin var olan kullanıcılar üzerinde sahip olacağı etkiyi belirleyebilmeleri için, [yalnızca rapor moduna](howto-conditional-access-insights-reporting.md) konur. Yöneticiler ilkenin bu şekilde uygulanmaları konusunda rahatsızsa, belirli gruplar ekleyerek ve diğerlerini dışlayarak dağıtımı **Açık** veya aşamalı olarak değiştirebilir.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Hariç tut**' un altında, **Kullanıcılar ve gruplar** ' ı seçin ve eski kimlik doğrulamasını kullanma yeteneğinin korunması gereken tüm hesapları seçin. Kendinizi kilitlenmesini engellemek için en az bir hesabı dışlayın. Herhangi bir hesabı dışlayamazsınız, bu ilkeyi oluşturabileceksiniz.
   1. **Bitti**'yi seçin.
1. **Bulut uygulamaları veya eylemler**altında **tüm bulut uygulamaları**' nı seçin.
   1. **Bitti**'yi seçin.
1. **Conditions**  >  **İstemci uygulamaları**Koşulları ' nın altında **Yapılandır** ' ı **Evet**olarak ayarlayın.
   1. Yalnızca **Exchange ActiveSync istemcileri** ve **diğer istemciler**için kutuyu işaretleyin.
   1. **Bitti**'yi seçin.
1. **Erişim denetimleri**  >  **izni**' nın altında, **erişimi engelle**' yi seçin.
   1. **Seç**’i seçin.
1. Ayarlarınızı onaylayın ve **ilkeyi** **yalnızca rapor**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-insights-reporting.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)

[Office 365 ve Microsoft 365 kullanarak e-posta göndermek üzere çok işlevli bir cihaz veya uygulamayı ayarlama](/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-office-3)
