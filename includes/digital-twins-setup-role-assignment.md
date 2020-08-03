---
author: baanders
description: Azure Digital TWINS kurulumu 'nda erişim izinleri için dosya ekleme adımı
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 6f43a55b3a409a84e3baf99dae24af7616ea4385
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408369"
---
Azure dijital TWINS, rol tabanlı erişim denetimi (RBAC) için [Azure Active Directory (Azure AD)](../articles/active-directory/fundamentals/active-directory-whatis.md) kullanır. Bu, bir Kullanıcı Azure dijital TWINS örneğiniz için veri düzlemi çağrısı yapmadan önce, söz konusu kullanıcının bu kullanıcı için uygun izinlere sahip bir rol atanması gerektiğini gösterir.

Azure dijital TWINS için bu rol, _**Azure dijital TWINS sahibindedir (Önizleme)**_. Kavramlar ve güvenlik hakkında daha fazla bilgi için bkz. [*Azure dijital TWINS çözümleri Için güvenlik*](../articles/digital-twins/concepts-security.md).

Bu bölümde, Azure dijital TWINS örneğinizdeki bir kullanıcı için Azure aboneliğinizdeki Azure AD kiracısındaki e-postasını kullanarak bir rol atamasının nasıl oluşturulduğu gösterilir. Kuruluşunuzdaki rolünüze bağlı olarak, bu izni kendiniz ayarlayabilir veya Azure dijital TWINS örneğini yönetebilecek başka bir kişi adına ayarlayabilirsiniz.

### <a name="assign-the-role"></a>Rolü ata

Bir Azure dijital TWINS örneğini yönetmek üzere bir Kullanıcı izinleri vermek için, bu kullanıcılara _**Azure dijital TWINS sahibi (Önizleme)**_ rolünü örnek içinde atamanız gerekir.

> [!NOTE]
> Bu rolün Azure AD *sahibi* rolünden farklı olduğunu, bu da Azure dijital TWINS örneğinin kapsamına da atanmadığını unutmayın. Bunlar iki ayrı yönetim rolleridir ve Azure AD *sahibi* , *Azure Digital TWINS sahibine (Önizleme)* verilen veri düzlemi özelliklerine erişim izni vermez.