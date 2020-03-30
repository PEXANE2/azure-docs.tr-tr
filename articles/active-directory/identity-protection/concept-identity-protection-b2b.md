---
title: Kimlik Koruması ve B2B kullanıcıları - Azure Active Directory
description: B2B kullanıcıları ile kimlik korumanın nasıl çalıştığını ve bilinen sınırlamaları kullanma
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95ea7eb470a5880bc88b3df903d33854f363e974
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72881321"
---
# <a name="identity-protection-and-b2b-users"></a>Kimlik Koruması ve B2B kullanıcıları

Azure AD B2B işbirliği yle kuruluşlar, Kimlik Koruması'nı kullanarak B2B kullanıcıları için risk tabanlı ilkeler uygulayabilir. Bu ilkeler iki şekilde yapılandırılmalıdır:

- Yöneticiler, konuk kullanıcıları da içeren tüm uygulamalar için geçerli olan yerleşik Kimlik Koruması risk tabanlı ilkeleri yapılandırabilir.
- Yöneticiler, oturum açma riskini konuk kullanıcıları içeren bir koşul olarak kullanarak Koşullu Erişim ilkelerini yapılandırabilirler.

## <a name="how-is-risk-evaluated-for-b2b-collaboration-users"></a>B2B işbirliği kullanıcıları için risk nasıl değerlendirilir?

B2B işbirliği kullanıcıları için kullanıcı riski kendi ev dizini değerlendirilir. Bu kullanıcılar için gerçek zamanlı oturum açma riski, kaynağa erişmeye çalıştıklarında kaynak dizininde değerlendirilir.

## <a name="limitations-of-identity-protection-for-b2b-collaboration-users"></a>B2B işbirliği kullanıcıları için Kimlik Koruma Sınırlamaları

B2B işbirliği kullanıcıları için kimlik koruma uygulamasında, kendi ev dizinde var olan kimlikleri nedeniyle bir kaynak dizininde sınırlamalar vardır. Ana sınırlamalar aşağıdaki gibidir:

- Bir konuk kullanıcı parola sıfırlama zorlamak için Kimlik Koruması kullanıcı risk ilkesini tetiklerse, **bunlar engellenir.** Bu engelleme, kaynak dizinindeki parolaları sıfırlayamamalarından kaynaklanmaktadır.
- **Konuk kullanıcılar riskli kullanıcı raporunda görünmez.** Bu görünürlük kaybı, B2B kullanıcısının ev dizininde meydana gelen risk değerlendirmesinden kaynaklanmaktadır.
- Yöneticiler, kaynak diziniiçinde **riskli bir B2B işbirliği kullanıcılarını görevden alamaz veya düzeltemez.** Bu işlevsellik kaybı, kaynak dizinindeki yöneticilerin B2B kullanıcısının ev dizinine erişememelerinden kaynak lanmaktadır.

### <a name="why-cant-i-remediate-risky-b2b-collaboration-users-in-my-directory"></a>Neden riskli B2B işbirliği kullanıcılarını dizinimde düzeltemiyorum?

B2B kullanıcıları için risk değerlendirmesi ve düzeltme ev dizini oluşur. Bu nedenle, konuk kullanıcılar kaynak dizininde riskli kullanıcı raporunda görünmez ve kaynak dizinindeki yöneticiler bu kullanıcılar için güvenli bir parola sıfırlama zorlayamaz.

### <a name="what-do-i-do-if-a-b2b-collaboration-user-was-blocked-due-to-a-risk-based-policy-in-my-organization"></a>Kuruluşumdaki risk tabanlı bir ilke nedeniyle bir B2B işbirliği kullanıcısı engellenirse ne yapmalıyım?

Dizininizdeki riskli bir B2B kullanıcısı risk tabanlı politikanız tarafından engellenirse, kullanıcının bu riski kendi dizinde düzeltilmesi gerekir. Kullanıcılar, ev dizinde güvenli bir parola sıfırlama gerçekleştirerek risklerini düzeltebilirler. Ev dizinde self servis parola sıfırlama özelliği yoksa, bir yöneticinin risklerini el ile reddetmesi veya parolalarını sıfırlaması için kendi kuruluşlarının BT Personeli'ne başvurmaları gerekir.

### <a name="how-do-i-prevent-b2b-collaboration-users-from-being-impacted-by-risk-based-policies"></a>B2B işbirliği kullanıcılarının risk tabanlı ilkelerden etkilenmesini nasıl önleyebilirim?

B2B kullanıcılarını kuruluşunuzun risk tabanlı Koşullu Erişim ilkelerinden hariç tutarak B2B kullanıcılarının risk değerlendirmelerinden etkilenmesini veya engellenmesini engeller. Bu B2B kullanıcılarını hariç tutmak için Azure AD'de kuruluşunuzun tüm konuk kullanıcılarını içeren bir grup oluşturun. Ardından, bu grubu yerleşik Kimlik Koruması kullanıcı riskinizin ve oturum açma risk ilkelerinizin yanı sıra oturum açma riskini koşul olarak kullanan Koşullu Erişim ilkeleri için bir dışlama olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](../b2b/what-is-b2b.md)
