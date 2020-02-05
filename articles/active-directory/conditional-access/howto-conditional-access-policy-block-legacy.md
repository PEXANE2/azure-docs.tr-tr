---
title: Koşullu erişim-blok eski kimlik doğrulaması-Azure Active Directory
description: Eski kimlik doğrulama protokollerini engellemek için özel bir koşullu erişim ilkesi oluşturma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0aca5019f4f7fca47195fb8fb821b1af1ae9ec77
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024254"
---
# <a name="conditional-access-block-legacy-authentication"></a>Koşullu erişim: eski kimlik doğrulamasını engelle

Eski kimlik doğrulama protokolleriyle ilişkili risk artışı nedeniyle, Microsoft kuruluşların bu protokolleri kullanarak kimlik doğrulama isteklerini engellemesini ve modern kimlik doğrulaması gerektirmesini önerir.

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Aşağıdaki adımlar, eski kimlik doğrulama isteklerini engellemek için bir koşullu erişim ilkesi oluşturmanıza yardımcı olur.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Koşullu erişim** > **Azure Active Directory** > **Güvenliğe** gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Hariç tut**' un altında, **Kullanıcılar ve gruplar** ' ı seçin ve eski kimlik doğrulamasını kullanma yeteneğinin korunması gereken tüm hesapları seçin. Kendinizi kilitleyemek için en az bir hesabı dışmalısınız. Herhangi bir hesabı dışlayamazsınız, bu ilkeyi oluşturabileceksiniz.
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemler** altında **tüm bulut uygulamaları**' nı seçin.
   1. **Done** (Bitti) öğesini seçin.
1. istemci uygulamaları (Önizleme) **Koşulları** > **altında** **Yapılandır** ' ı **Evet**olarak ayarlayın.
   1. Yalnızca **mobil uygulamalar ve Masaüstü istemcileri** > **diğer diğer istemciler**' i işaretleyin.
   1. **Done** (Bitti) öğesini seçin.
1. **Erişim denetimleri** > **izin**' ın altında, **erişimi engelle**' yi seçin.
   1. Seçin **seçin**.
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)
