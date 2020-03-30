---
title: Koşullu Erişim - Konuma göre erişimi engelleme - Azure Etkin Dizini
description: IP konumuna göre kaynaklara erişimi engellemek için özel bir Koşullu Erişim ilkesi oluşturun
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
ms.openlocfilehash: 34b29ceadaaf85e69d1214039fa1b563ed21a77d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295179"
---
# <a name="conditional-access-block-access-by-location"></a>Koşullu Erişim: Konuma göre erişimi engelleyin

Koşullu Erişim'deki konum koşuluyla, bulut uygulamalarınıza erişimi bir kullanıcının ağ konumuna göre denetleyebilirsiniz. Konum koşulu genellikle kuruluşunuzun trafiğin gelmemesi gerektiğini bildiği ülkelerden gelen erişimi engellemek için kullanılır.

## <a name="define-locations"></a>Konumları tanımlama

1. **Azure portalında** global yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim** > **Adlandırılmış konumlara**göz atın.
1. **Yeni konum**seçin.
1. Konumunuza bir ad verin.
1. Bu konumu veya **Ülkeler/Bölgeler'i**oluşturan harici olarak erişilebilen belirli IPv4 adres aralıklarını biliyorsanız **IP aralıklarını** seçin.
   1. IP **aralıklarını** sağlayın veya belirttiğiniz konum için **Ülkeler/Bölgeler'i** seçin.
      * Ülkeler/Bölgeler'i seçerseniz, isteğe bağlı olarak bilinmeyen alanları eklemeyi seçebilirsiniz.
1. **Kaydet'i** seçin

Koşullu Erişim'deki konum durumu hakkında daha fazla bilgiyi makalede bulabilirsiniz, [Azure Etkin Dizin Koşullu Erişim'de konum durumu nedir](location-condition.md)

## <a name="create-a-conditional-access-policy"></a>Koşullu Erişim ilkesi oluşturma

1. **Azure portalında** global yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar**altında, **Kullanıcıları ve grupları** seçin
   1. **Altında Ekle**, **Tüm kullanıcıları**seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > altında**Ekle,** **Tüm bulut uygulamalarını**seçin ve **Bitti'yi**seçin.
1. **Koşullar** > **Altında Yer**.
   1. **Yapılandırmayı** **Evet** olarak ayarlama
   1. **Seçili konumları** **seç**
   1. Kuruluşunuz için oluşturduğunuz engellenen konumu seçin.
   1.  > **Bitti'yi** > **Done** **seç'i**tıklatın.
1. **Koşullar** > Altında**İstemci uygulamaları (Önizleme)**, **Evet** **yapılaşını** ayarlayın ve **Bitti'yi**seçin.
1. **Access denetimleri** > altında**Engelle**ve **Seç'i**seçin.
1. Ayarlarınızı onaylayın ve Etkinleştir **ilkesini** **A'ya**ayarlayın.
1. İlkinizi etkinleştirmek için **Oluştur'u** seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu Erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu Erişim Ne Varsa aracını kullanarak oturum açma davranışını simüle edin](troubleshoot-conditional-access-what-if.md)
