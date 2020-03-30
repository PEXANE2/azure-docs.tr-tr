---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 03/11/2020
ms.author: mimart
ms.openlocfilehash: 4e03d076fc9ed7d87326a4c8b4f8bf6c0b21c750
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126718"
---
## <a name="disable-email-verification"></a>E-posta doğrulamayı devre dışı bırakma

Varsayılan olarak, Azure Active Directory B2C (Azure AD B2C), yerel hesaplar için müşterinizin e-posta adresini doğrular (e-posta adresi veya kullanıcı adı ile kaydolan kullanıcıların hesapları). Azure AD B2C, müşterilerin kayıt işlemi sırasında bunları doğrulamalarını isteyerek geçerli e-posta adresleri sağlar. Ayrıca, kötü amaçlı aktörlerin uygulamalarınızda sahte hesaplar oluşturmak için otomatik leştirilmiş işlemler kullanmasını da engeller.

Bazı uygulama geliştiricileri, kaydolma işlemi sırasında e-posta doğrulamasını atlamayı ve bunun yerine müşterilerin e-posta adreslerini daha sonra doğrulamalarını tercih eder. Bunu desteklemek için Azure AD B2C e-posta doğrulamasını devre dışı kılacak şekilde yapılandırılabilir. Bunu yapmak daha sorunsuz bir kayıt işlemi oluşturur ve geliştiricilere e-posta adreslerini doğrulamamış müşterilerden ayırt etme esnekliği sağlar.

> [!WARNING]
> Kayıt işleminde e-posta doğrulamanın devre dışı bırakılması istenmeyen postaya yol açabilir. Varsayılan Azure AD B2C sağlanan e-posta doğrulamasını devre dışı ederseniz, yeni bir doğrulama sistemi uygulamanızı öneririz.
