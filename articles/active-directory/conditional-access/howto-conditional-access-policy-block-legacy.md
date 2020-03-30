---
title: Koşullu Erişim - Eski kimlik doğrulamasını engelleme - Azure Etkin Dizini
description: Eski kimlik doğrulama protokollerini engellemek için özel bir Koşullu Erişim ilkesi oluşturma
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295224"
---
# <a name="conditional-access-block-legacy-authentication"></a>Koşullu Erişim: Eski kimlik doğrulamasını engelleme

Eski kimlik doğrulama protokolleriyle ilişkili artan risk nedeniyle Microsoft, kuruluşların bu protokolleri kullanarak kimlik doğrulama isteklerini engellemesini ve modern kimlik doğrulamasına ihtiyaç duymasını önerir.

## <a name="create-a-conditional-access-policy"></a>Koşullu Erişim ilkesi oluşturma

Aşağıdaki adımlar, eski kimlik doğrulama isteklerini engellemek için Koşullu Erişim ilkesi oluşturulmasına yardımcı olur. Bu ilke, yöneticilerin varolan kullanıcılar üzerindeki etkilerini belirleyebilmeleri için yalnızca [Şikayet moduna](howto-conditional-access-report-only.md) konur. Yöneticiler, iidinin istedikleri gibi uygulandığından memnun olduklarında, belirli gruplar ekleyerek ve diğerlerini hariç tutarak **devreyi Aç'a** veya aşamalı olarak açabilirler.

1. **Azure portalında** global yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar**altında, **Kullanıcıları ve grupları** seçin
   1. **Altında Ekle**, **Tüm kullanıcıları**seçin.
   1. **Dışla'nın**altında, **Kullanıcıları ve grupları** seçin ve eski kimlik doğrulamasını kullanma yeteneğini koruması gereken hesapları seçin. Kilitlenmeyi önlemek için en az bir hesabı hariç taçıklayın. Herhangi bir hesabı hariç tutmazsanız, bu ilkeyi oluşturamazsınız.
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri**altında, Tüm bulut **uygulamalarını**seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **Koşullar** > **Altında İstemci uygulamaları (önizleme)**, **Evet** **yapılaşı** ayarlayın.
   1. Yalnızca kutuları kontrol **edin Mobil uygulamalar ve masaüstü istemcileri** > **Diğer istemciler**.
   1. **Done** (Bitti) öğesini seçin.
1. **Access denetimleri** > altında**Hibe**, **erişimi engelle'yi**seçin.
   1. **Seç**’i seçin.
1. Ayarlarınızı onaylayın ve yalnızca **Rapor'a** **etkinleştir ilkesini** ayarlayın.
1. İlkinizi etkinleştirmek için **Oluştur'u** seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu Erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu Erişim Ne Varsa aracını kullanarak oturum açma davranışını simüle edin](troubleshoot-conditional-access-what-if.md)
