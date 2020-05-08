---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 9ac9865afe37916f1777d92eab8637884eba0c08
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82562142"
---
Cihazınız, Azure 'daki verileriniz için hedef olarak kullanılan bir depolama hesabıyla ilişkilendirilir. Depolama hesabına erişim, bu depolama hesabıyla ilişkili abonelik ve 2 512-bit depolama erişim anahtarlarına göre denetlenir.

Azure Stack Edge cihazı depolama hesabına eriştiğinde, anahtarlardan biri kimlik doğrulaması için kullanılır. Diğer anahtar ayrılmış olarak tutulur, bu sayede anahtarları düzenli olarak döndürebilirsiniz.

Güvenlik nedenleriyle birçok veri merkezi için anahtar döndürme gerekir. Anahtar dönüşü için aşağıdaki en iyi yöntemleri izlemeniz önerilir:

- Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Hesap anahtarınızı dikkatle koruyun. Parolayı diğer kullanıcılara dağıtmayın, sabit kodlarım veya başkalarının erişebileceği düz metin içinde herhangi bir yere kaydedin.
- Tehlikede olduğunu düşünüyorsanız, Azure portal aracılığıyla hesap anahtarınızı yeniden oluşturun. Daha fazla bilgi için bkz. [depolama hesabı erişim anahtarlarını yönetme](../articles/storage/common/storage-account-keys-manage.md).
- Azure yöneticiniz, depolama hesabına doğrudan erişmek için Azure portal depolama bölümünü kullanarak birincil veya ikincil anahtarı düzenli aralıklarla değiştirmeli veya yeniden üretmelidir.