---
title: Koşullu erişim-konuma göre erişimi engelleyin-Azure Active Directory
description: Kaynaklara erişimi IP konumuna göre engellemek için özel bir koşullu erişim ilkesi oluşturma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 204026825a5132464e1adda611227db2ac7cf91f
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76543996"
---
# <a name="conditional-access-block-access-by-location"></a>Koşullu erişim: konuma göre erişimi engelleyin

Koşullu erişimde konum koşulu ile, bir kullanıcının ağ konumuna bağlı olarak bulut uygulamalarınıza erişimi denetleyebilirsiniz. Konum koşulu genellikle kuruluşunuzun trafiği bildiği ülkelerden gelen erişimi engellemek için kullanılır.

## <a name="define-locations"></a>Konumları tanımlama

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Adlandırılmış konumlar** > **koşullu erişim** > **Azure Active Directory** > **güvenlik** ' e gidin.
1. **Yeni konum**' ı seçin.
1. Konumunuza bir ad verin.
1. Bu konumu veya **ülkeleri/bölgeleri**oluşturan belirli dışarıdan erişilebilir IPv4 adresi aralıklarını biliyorsanız, **IP aralıklarını** seçin.
   1. Belirttiğiniz konum için **IP aralıklarını** sağlayın veya **ülke/bölge** seçin.
      * Ülkeler/bölgeler ' i seçerseniz, isteğe bağlı olarak bilinmeyen alanları eklemeyi seçebilirsiniz.
1. **Kaydet**’i seçin.

Koşullu erişim 'deki konum koşulu hakkında daha fazla bilgi için, [Azure Active Directory Koşullu erişim ' deki konum koşulunun ne olduğunu](location-condition.md) bulabilirsiniz

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Koşullu erişim** > **Azure Active Directory** > **Güvenliğe** gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemler** altında **dahil** > , **tüm bulut uygulamaları**' nı seçin ve **bitti**' yi seçin.
1. **Koşullar** > **konum**altında.
   1. **Yapılandır** 'ı **Evet** olarak ayarla
   1. **Seçili konumların** seçimini **Ekle**
   1. Kuruluşunuz için oluşturduğunuz engellenen konumu seçin.
   1.  > **Seç** ' e tıklayın > **bitti**' ye tıklayın.
1. **Erişim denetimleri** > **Engelle**altında, **Seç**' i seçin.
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)
