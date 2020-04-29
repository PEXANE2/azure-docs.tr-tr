---
title: Koşullu erişim-blok eski kimlik doğrulaması-Azure Active Directory
description: Eski kimlik doğrulama protokollerini engellemek için özel bir koşullu erişim ilkesi oluşturma
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
ms.openlocfilehash: 6a868c8199ac34a498a280e2522d6b1e4c7ec370
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80295224"
---
# <a name="conditional-access-block-legacy-authentication"></a>Koşullu erişim: eski kimlik doğrulamasını engelle

Eski kimlik doğrulama protokolleriyle ilişkili risk artışı nedeniyle, Microsoft kuruluşların bu protokolleri kullanarak kimlik doğrulama isteklerini engellemesini ve modern kimlik doğrulaması gerektirmesini önerir.

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Aşağıdaki adımlar, eski kimlik doğrulama isteklerini engellemek için bir koşullu erişim ilkesi oluşturmanıza yardımcı olur. Bu ilke, yöneticilerin var olan kullanıcılar üzerinde sahip olacağı etkiyi belirleyebilmeleri için, [yalnızca rapor moduna](howto-conditional-access-report-only.md) konur. Yöneticiler ilkenin bu şekilde uygulanmaları konusunda rahatsızsa, belirli gruplar ekleyerek ve diğerlerini dışlayarak dağıtımı **Açık** veya aşamalı olarak değiştirebilir.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory** > **Security**güvenlik > **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Hariç tut**' un altında, **Kullanıcılar ve gruplar** ' ı seçin ve eski kimlik doğrulamasını kullanma yeteneğinin korunması gereken tüm hesapları seçin. Kendinizi kilitlenmesini engellemek için en az bir hesabı dışlayın. Herhangi bir hesabı dışlayamazsınız, bu ilkeyi oluşturabileceksiniz.
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemler**altında **tüm bulut uygulamaları**' nı seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **İstemci uygulamaları (Önizleme)** **Koşulları** > altında **Yapılandır** ' ı **Evet**olarak ayarlayın.
   1. Yalnızca **mobil uygulamalar ve Masaüstü istemcileri** > **diğer istemcileri**onay kutularını işaretleyin.
   1. **Done** (Bitti) öğesini seçin.
1. **Erişim denetimleri** > **izni**' nın altında, **erişimi engelle**' yi seçin.
   1. **Seç**’i seçin.
1. Ayarlarınızı onaylayın ve **ilkeyi** **yalnızca rapor**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)
