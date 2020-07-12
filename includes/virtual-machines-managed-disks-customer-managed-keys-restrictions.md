---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/10/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c303527c7411ead585e70fc2e31db4dd2d35e996
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259806"
---
- Yalnızca 2.048-bit, 3.072-bit ve 4.096-bit boyutlarının [yazılım ve HSM RSA anahtarları](../articles/key-vault/keys/about-keys.md) desteklenir, başka hiçbir anahtar veya boyut yoktur.
    - [HSM](../articles/key-vault/keys/hsm-protected-keys.md) anahtarları Için Azure Anahtar Kasası 'nın **Premium** katmanı gerekir.
- Sunucu tarafı şifreleme ve müşterinin yönettiği anahtarlar kullanılarak şifrelenen özel görüntülerden oluşturulan diskler, müşteri tarafından yönetilen aynı anahtar kullanılarak şifrelenmelidir ve aynı abonelikte olmalıdır.
- Sunucu tarafı şifreleme ve müşteri tarafından yönetilen anahtarlarla şifrelenen disklerden oluşturulan anlık görüntüler, müşteri tarafından yönetilen aynı anahtarlarla şifrelenmelidir.
- Müşteri tarafından yönetilen anahtarlarınızla ilgili tüm kaynakların (Azure Anahtar kasaları, disk şifreleme kümeleri, VM 'Ler, diskler ve anlık görüntüler) aynı abonelikte ve bölgede olması gerekir.
- Müşteri tarafından yönetilen anahtarlarla şifrelenen diskler, anlık görüntüler ve görüntüler başka bir aboneliğe taşınamaz.
- Müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenen yönetilen diskler, Azure disk şifrelemesi ile de şifrelenemez.
- , Her abonelik için bölge başına en fazla 50 disk şifreleme kümesi oluşturabilir.
- Paylaşılan görüntü galerileriyle müşteri tarafından yönetilen anahtarları kullanma hakkında daha fazla bilgi için bkz. [Önizleme: görüntüleri şifrelemek için müşteri tarafından yönetilen anahtarları kullanma](../articles/virtual-machines/image-version-encryption.md).
