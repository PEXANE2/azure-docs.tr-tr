---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 2e0a7ca8fb9eaafbc50c0ce60799dd68d83b2fa3
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98901253"
---
Cihazınız, Azure’daki verileriniz için hedef olarak kullanılan bir depolama hesabıyla ilişkilendirilir. Depolama hesabına erişim, abonelikle ve bu depolama hesabıyla ilişkili iki 512 bit depolama hesabı anahtarıyla denetlenir.

Data Box Edge cihaz depolama hesabına eriştiğinde, anahtarlardan biri kimlik doğrulaması için kullanılır. Diğer anahtar ayrılmış olarak tutulur, bu sayede düzenli olarak anahtarları döndürebilirsiniz.

Güvenlik nedenleriyle birçok veri merkezi anahtar döndürmeyi gerektirir. Anahtar dönüşü için aşağıdaki en iyi yöntemleri izlemeniz önerilir:

- Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Hesap anahtarınızı dikkatle koruyun. Parolayı diğer kullanıcılara dağıtmayın, sabit olarak kodlamayın veya başkalarının erişebileceği bir konumda düz metin olarak kaydetmeyin.
- Tehlikede olduğunu düşünüyorsanız, Azure portal aracılığıyla [Hesap anahtarınızı yeniden oluşturun](../articles/storage/common/storage-account-keys-manage.md#manually-rotate-access-keys) .
- Azure yöneticiniz, depolama hesabına doğrudan erişmek için Azure portal depolama bölümünü kullanarak birincil veya ikincil anahtarı düzenli aralıklarla değiştirmeli veya yeniden üretmelidir.
