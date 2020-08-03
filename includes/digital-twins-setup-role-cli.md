---
author: baanders
description: Azure Digital TWINS kurulumunda rol gereksinimi için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 1714fac465531a2d3a5829e7080132510ccd1ec4
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407576"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

## <a name="prerequisites-permission-requirements"></a>Önkoşullar: Izin gereksinimleri

Bu makaledeki tüm adımları tamamlayabilmeniz için, Azure aboneliğinizde bir sahip olarak sınıflandırılmalıdır. 

Bu komutu Cloud Shell ' de çalıştırarak izin düzeyinizi kontrol edebilirsiniz:

```azurecli-interactive
az role assignment list --assignee <your-Azure-email>
```

Sahibiyseniz, `roleDefinitionName` çıkışdaki değer *sahipdir*:

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/cloud-shell/owner-role.png" alt-text="Az role atama listesi komutunun çıkışını gösteren Cloud Shell pencere":::

Değerin *katkıda* bulunduğunu veya *sahip*dışında bir şeyi fark ederseniz aşağıdaki yollarla devam edebilirsiniz:
* Sizin adınıza bu makaledeki adımları tamamlamaya yönelik abonelik sahibine ve sahibine yönelik isteğe başvurun
* Abonelik sahibine veya Kullanıcı erişimi yönetici rolüne sahip birine abone olur ve devam etmek için gereken izinlere sahip olacak şekilde aboneliğinizi aboneliğe sahip olarak yükseltmenizi isteyin. Bunun uygun olup olmadığı, kuruluşunuza ve onun içindeki rolünüze bağlıdır.