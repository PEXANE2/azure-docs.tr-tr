---
author: baanders
description: Azure Digital TWINS kurulumu 'nda erişim izinleri için dosya ekleme adımı
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: cbcaf4b4ad1b6c00f8c452582b986b6ee3b2806e
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92478868"
---
Azure dijital TWINS, rol tabanlı erişim denetimi (RBAC) için [Azure Active Directory (Azure AD)](../articles/active-directory/fundamentals/active-directory-whatis.md) kullanır. Bu, bir Kullanıcı Azure dijital TWINS örneğiniz için veri düzlemi çağrısı yapmadan önce, söz konusu kullanıcının bu kullanıcı için uygun izinlere sahip bir rol atanması gerektiğini gösterir.

Azure dijital TWINS için bu rol, _**Azure dijital TWINS veri sahibidir**_. Kavramlar ve güvenlik hakkında daha fazla bilgi için bkz. [*Azure dijital TWINS çözümleri Için güvenlik*](../articles/digital-twins/concepts-security.md).

[!INCLUDE [digital-twins-role-rename-note.md](digital-twins-role-rename-note.md)]

Bu bölümde, Azure dijital TWINS örneğinizdeki bir kullanıcı için Azure aboneliğinizdeki Azure AD kiracısındaki e-postasını kullanarak bir rol atamasının nasıl oluşturulduğu gösterilir. Kuruluşunuzdaki rolünüze bağlı olarak, bu izni kendiniz ayarlayabilir veya Azure dijital TWINS örneğini yönetebilecek başka bir kişi adına ayarlayabilirsiniz.

### <a name="assign-the-role"></a>Rolü ata

Bir Azure dijital TWINS örneğini yönetmek üzere bir kullanıcıya izin vermek için, _**Azure Digital TWINS veri sahibi**_ rolünü örnek içinde atamanız gerekir.

> [!NOTE]
> Bu rolün Azure AD *sahibi* rolünden farklı olduğunu, bu da Azure dijital TWINS örneğinin kapsamına da atanmadığını unutmayın. Bunlar iki ayrı yönetim rolleridir ve Azure AD *sahibi* , *Azure dijital TWINS veri sahibiyle*verilen veri düzlemi özelliklerine erişim vermez.