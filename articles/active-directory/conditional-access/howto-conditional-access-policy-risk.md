---
title: Koşullu Erişim - Risk Tabanlı Koşullu Erişim - Azure Etkin Dizini
description: İlkelerde Kimlik Koruması geliştirmelerini etkinleştirmek için Koşullu Erişim ilkeleri oluşturma
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
ms.openlocfilehash: 8054d8985596095db32d9262322d7fb0f4aab8c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295147"
---
# <a name="conditional-access-risk-based-conditional-access"></a>Koşullu Erişim: Risk Tabanlı Koşullu Erişim

Azure AD Premium P2 lisanslarına sahip kuruluşlar, Azure AD Kimlik Koruması risk algılamalarını içeren Koşullu Erişim ilkeleri oluşturabilir. Kutudan etkinleştirilebilen üç varsayılan ilke vardır. 

* Tüm kullanıcıların Azure Çok Faktörlü Kimlik Doğrulaması'na kaydolmasını zorunlu kılmasını zorunlu kınla.
* Yüksek risk altındaki kullanıcılar için parola değişikliği gerektirir.
* Orta veya yüksek oturum açma riski olan kullanıcılar için çok faktörlü kimlik doğrulaması gerektirir.

## <a name="require-all-users-to-register-for-azure-multi-factor-authentication"></a>Tüm kullanıcıların Azure Çok Faktörlü Kimlik Doğrulamasına kaydolmasını zorunlu kılmasını zorunlu kılmasını

Bu ilkeyi etkinleştirmek için tüm kullanıcıların 14 gün içinde Azure Çok Faktörlü Kimlik Doğrulama'ya kaydolması gerekir. 

1. **Azure portalında**oturum açın.
1. **Tüm hizmetler**'e tıklayıp **Azure AD Kimlik Koruması**'na gidin.
1. **MFA kaydı**'na tıklayın.
1. **Atamalar**altında, **Kullanıcıları**seçin.
   1. **Altında Ekle**, **Tüm kullanıcıları**seçin.
   1. **Dışla'nın**altında, **dışlanmış kullanıcıları seçin,** kuruluşunuzun acil durum erişimini veya kesme cam hesaplarını seçin ve **Seç'i**seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. İlkeyi **On'a** **Ayarla.**
1. **Kaydet**'e tıklayın.

## <a name="require-a-password-change-high-risk-users"></a>Parola değişikliği yüksek riskli kullanıcılar gerektirir

Microsoft, kullanıcı adı ve parola çiftlerini bulma amacıyla araştırmacılar, kolluk kuvvetleri, Microsoft'taki çeşitli güvenlik ekipleri ve diğer güvenilir kaynaklarla birlikte çalışmalar yapmaktadır. Bu çiftlerden biri ortamınızdaki bir hesapla eşleştiğinde aşağıdaki ilkeyi kullanarak risk tabanlı parola değişikliğinin tetiklenmesini sağlayabilirsiniz.

1. **Azure portalında**oturum açın.
1. **Tüm hizmetler**'e tıklayıp **Azure AD Kimlik Koruması**'na gidin.
1. Kullanıcı **risk ilkesini**tıklatın.
1. **Atamalar**altında, **Kullanıcıları** seçin
   1. **Altında Ekle**, **Tüm kullanıcıları**seçin.
   1. **Dışla'nın**altında, **dışlanmış kullanıcıları seçin,** kuruluşunuzun acil durum erişimini veya kesme cam hesaplarını seçin ve **Seç'i**seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **Koşullar**altında, **Kullanıcı riskini**seçin, sonra **Yüksek**seçin.
   1. **Sonra Bitti'yi Seç'i** tıklatın. **Done**
1. **Denetimler** > **Access**altında, **erişime izin ver'i**seçin ve ardından parola değişikliği ni **gerek'** seçeneğini belirleyin.
   1. **Seç'i**tıklatın.
1. İlkeyi **On'a** **Ayarla.**
1. **Kaydet**'e tıklayın.

## <a name="require-mfa-medium-or-high-sign-in-risk-users"></a>MFA orta veya yüksek oturum açma riski olan kullanıcılara ihtiyaç duyma

Çoğu kullanıcı, takip edilebilen normal bir davranışa sahiptir ve davranışları normalin dışına çıktığında oturum açmalarına izin vermek riskli olabilir. Bu kullanıcıyı engellemek veya belki de sadece onlar gerçekten kim olduklarını kanıtlamak için çok faktörlü kimlik doğrulama gerçekleştirmek için onlardan sormak isteyebilirsiniz. Riskli oturum açma algılandığında MFA gerektiren ilkeyi kullanmak için aşağıdaki ilkeyi etkinleştirin.

1. **Azure portalında**oturum açın.
1. **Tüm hizmetler**'e tıklayıp **Azure AD Kimlik Koruması**'na gidin.
1. Oturum **Açma risk ilkesine** tıklayın
1. **Atamalar**altında, **Kullanıcıları** seçin
   1. **Altında Ekle**, **Tüm kullanıcıları**seçin.
   1. **Dışla'nın**altında, **dışlanmış kullanıcıları seçin,** kuruluşunuzun acil durum erişimini veya kesme cam hesaplarını seçin ve **Seç'i**seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **Koşullar**altında, **Oturum açma riskini**seçin, ardından Orta ve **üzerini**seçin.
   1. **Sonra Bitti'yi Seç'i** tıklatın. **Done**
1. **Denetimler** > **Erişimi**altında, **Erişime İzin Ver'i**seçin ve ardından çok **faktörlü kimlik doğrulamayı denetle'yi**seçin.
   1. **Seç'i**tıklatın.
1. İlkeyi **On'a** **Ayarla.**
1. **Kaydet**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu Erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu Erişim Ne Varsa aracını kullanarak oturum açma davranışını simüle edin](troubleshoot-conditional-access-what-if.md)

[Nasıl çalışır: Azure Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md)

[Azure Active Directory Kimlik Koruması nedir?](../identity-protection/overview.md)
