---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 03/11/2020
ms.author: mimart
ms.openlocfilehash: 4e03d076fc9ed7d87326a4c8b4f8bf6c0b21c750
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79126718"
---
## <a name="disable-email-verification"></a>E-posta doğrulamayı devre dışı bırakma

Varsayılan olarak, Azure Active Directory B2C (Azure AD B2C), müşterinin e-posta adresini yerel hesaplar için doğrular (e-posta adresi veya Kullanıcı adı ile kaydolan kullanıcılar için hesaplar). Azure AD B2C, müşterilerin kaydolma işlemi sırasında bunları doğrulamasını zorunlu kılarak geçerli e-posta adreslerini sağlar. Ayrıca, kötü amaçlı aktörlerin uygulamalarınızda sahte hesaplar oluşturmak için otomatik süreçler kullanmalarını da engeller.

Bazı uygulama geliştiricileri, kaydolma işlemi sırasında e-posta doğrulamasını atlamayı tercih eder ve bunun yerine müşterilerin e-posta adreslerini daha sonra doğrular. Bunu desteklemek için Azure AD B2C, e-posta doğrulamasını devre dışı bırakacak şekilde yapılandırılabilir. Bunun yapılması daha sorunsuz bir kaydolma işlemi oluşturur ve geliştiricilere, e-posta adreslerini doğrulayan müşterileri, kendilerine ait olmayan müşterilerin ayırt edilmesine yönelik esneklik sağlar.

> [!WARNING]
> Kaydolma işleminde e-posta doğrulamasını devre dışı bırakmak istenmeyen postalara neden olabilir. Varsayılan Azure AD B2C tarafından belirtilen e-posta doğrulamasını devre dışı bırakırsanız, bir değiştirme doğrulama sistemi uygulamanızı öneririz.
