---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 653c175a559f5c0b7dc551b396e91276332df20a
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96468047"
---
Cihazınız, Azure’daki verileriniz için hedef olarak kullanılan bir depolama hesabıyla ilişkilendirilir. Depolama hesabına erişim, abonelikle ve bu depolama hesabıyla ilişkili iki 512 bit depolama hesabı anahtarıyla denetlenir.

Data Box Edge cihaz depolama hesabına eriştiğinde, anahtarlardan biri kimlik doğrulaması için kullanılır. Diğer anahtar ayrılmış olarak tutulur, bu sayede düzenli olarak anahtarları döndürebilirsiniz.

Güvenlik nedenleriyle birçok veri merkezi anahtar döndürmeyi gerektirir. Anahtar dönüşü için aşağıdaki en iyi yöntemleri izlemeniz önerilir:

- Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Hesap anahtarınızı dikkatle koruyun. Parolayı diğer kullanıcılara dağıtmayın, sabit olarak kodlamayın veya başkalarının erişebileceği bir konumda düz metin olarak kaydetmeyin.
- Tehlikede olduğunu düşünüyorsanız, Azure portal aracılığıyla [Hesap anahtarınızı yeniden oluşturun](../articles/storage/common/storage-account-manage.md#regenerate-access-keys) .
- Azure yöneticiniz, depolama hesabına doğrudan erişmek için Azure portal depolama bölümünü kullanarak birincil veya ikincil anahtarı düzenli aralıklarla değiştirmeli veya yeniden üretmelidir.