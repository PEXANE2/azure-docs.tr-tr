---
title: Azure AD Connect bulut eşitlemesi yanlışlıkla silmeler
description: Bu konuda, silme işlemini engellemek için yanlışlıkla silme özelliğinin nasıl kullanılacağı açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/19/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ef7b6d9495b1431e03808b830671e839b90d436
ms.sourcegitcommit: 8a74ab1beba4522367aef8cb39c92c1147d5ec13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98614288"
---
# <a name="accidental-delete-prevention"></a>Yanlışlıkla silme engellemesi

Aşağıdaki belge Azure AD Connect bulut eşitlemesine yönelik yanlışlıkla silme özelliğini açıklamaktadır.  Yanlışlıkla silme özelliği, çok sayıda kullanıcı ve grubu etkileyecek şirket içi dizininizde yanlışlıkla yapılan yapılandırma değişikliklerinden ve değişikliklerden korunmak için tasarlanmıştır.  Bu özellik şunları yapmanıza olanak sağlar:

- yanlışlıkla silmeleri otomatik olarak engelleme özelliğini yapılandırın. 
- Yapılandırmanın yürürlüğe alınacağı nesne (eşik) sayısını ayarlayın 
- söz konusu senaryo için söz konusu eşitleme işi karantinaya alındıktan sonra bir e-posta bildirimi almak için bir bildirim e-posta adresi ayarlayın 

Bu özelliği kullanmak için, eşitleme durmak gerekirse, bu özellik için eşiği ayarlarsınız.  Bu sayıya ulaşıldığında, eşitleme durdurulur ve belirtilen e-postaya bir bildirim gönderilir.  Bu, neler olduğunu araştırmanıza olanak sağlar.


## <a name="configure-accidental-delete-prevention"></a>Yanlışlıkla silme engellemesini yapılandırma
Yeni özelliği kullanmak için aşağıdaki adımları izleyin.


1.  Azure portalında **Azure Active Directory** seçeneğini belirleyin.
2.  **Azure AD Connect** seçin.
3.  **Bulut eşitlemesini Yönet**' i seçin.
4. **Yapılandırma** altında yapılandırmanızı seçin.
5. **Ayarlar** altında şunları girin:
    - **Bildirim e-postası** -e-posta bildirimleri için kullanılır
    - **Yanlışlıkla Silmeleri engelle** -özelliği etkinleştirmek için bu kutuyu işaretleyin
    - **Yanlışlıkla silme eşiği** -eşitleme durdurma ve bildirim tetiklenecek nesne sayısını girin

![Yanlışlıkla silme](media/how-to-accidental-deletes/accident-1.png)

## <a name="next-steps"></a>Sonraki adımlar 

- [Azure AD Connect bulut eşitlemesi nedir?](what-is-cloud-sync.md)
- [Azure AD Connect bulut eşitlemesi nasıl yüklenir](how-to-install.md)
 

