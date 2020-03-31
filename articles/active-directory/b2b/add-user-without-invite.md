---
title: B2B misafirlerini davet bağlantısı veya e-posta olmadan ekleyin - Azure AD
description: Azure Active Directory B2B işbirliğinde bir daveti kullanmadan bir konuk kullanıcının Azure AD'ınıza diğer konuk kullanıcıları eklemesine izin verebilirsiniz.
documentationcenter: ''
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42c9caaf581fab37e2e55cbe408db6d6d846622a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050900"
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation-link-or-email"></a>Davet bağlantısı veya e-posta olmadan B2B işbirliği konuk kullanıcı ekleme

Artık paylaşılan bir uygulamaya doğrudan bağlantı göndererek konuk kullanıcıları davet edebilirsiniz. Bu yöntemle, konuk kullanıcıların bazı özel durumlar dışında davet e-postasını kullanmaları gerekmez. Konuk kullanıcı uygulama bağlantısını tıklar, gizlilik koşullarını inceler ve kabul eder ve uygulamaya sorunsuz bir şekilde erişir. Daha fazla bilgi için [B2B işbirliği daveti itfası'na](redemption-experience.md)bakın.   

Bu yeni yöntem kullanılabilir hale gelmeden önce, konuk davetçi (kuruluşunuzdan veya iş ortağı kuruluştan) **konuk davetçi** dizini rolüne bir davetli ekleyerek davet e-postasına gerek kalmadan konuk kullanıcıları davet edebilir ve ardından davet edenin Kullanıcı kullanıcılarını UI veya PowerShell aracılığıyla dizine, gruplara veya uygulamalara eklemesini sağlayabilirsiniz. (PowerShell kullanıyorsanız, davet e-postasını tamamen bastırabilirsiniz). Örnek:

1. Ana bilgisayar kuruluşundaki bir kullanıcı (örneğin, WoodGrove), ortak kuruluştan bir Sam@litware.comkullanıcıyı (örneğin,) Konuk olarak davet eder.
2. Ana bilgisayar kuruluşundaki yönetici, Sam'in ortak kuruluştan (Litware) diğer kullanıcıları tanımlamasına ve eklemesine olanak tanıyan [ilkeler ayarlar.](delegate-invitations.md) (Sam **Konuk davetli** rolüne eklenmelidir.)
3. Şimdi, Sam WoodGrove dizini, gruplar veya uygulamalar için davetiyeler itfa edilmesi gerek kalmadan Litware diğer kullanıcıları ekleyebilirsiniz. Sam Litware uygun numaralandırma ayrıcalıkları varsa, otomatik olarak olur.
 
Bu özgün yöntem hala çalışır. Ancak, davranış küçük bir fark var. PowerShell kullanıyorsanız, davet edilen konuk hesabının artık **Kabul Edilen'i**hemen göstermek yerine **Beklemeli Kabul** durumuna sahip olduğunu fark edeceksiniz. Durum beklemede olmasına rağmen, konuk kullanıcı yine de oturum açabilir ve bir e-posta daveti bağlantısını tıklatmadan uygulamaya erişebilir. Bekleyen durum, kullanıcının davet eden kuruluşun gizlilik koşullarını kabul ettiği [onay deneyiminden](redemption-experience.md#consent-experience-for-the-guest)henüz geçmediği anlamına gelir. Konuk kullanıcı ilk kez oturum açtığında bu onay ekranını görür. 

Bir kullanıcıyı dizine davet ederseniz, konuk kullanıcının gizlilik koşullarını görüntülemek ve kabul https://portal.azure.com/etmek için doğrudan kaynağa özel Azure portalı URL'sine *(kaynak kiracı*.onmicrosoft.com gibi) erişmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği daveti itfa](redemption-experience.md)
- [Azure Active Directory B2B işbirliği için temsilci davetleri](delegate-invitations.md)
- [Bilgi çalışanları B2B işbirliği kullanıcılarını nasıl ekler?](add-users-information-worker.md)

