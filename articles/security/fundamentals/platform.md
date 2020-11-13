---
title: Azure platformu bütünlüğü ve güvenliği-Azure güvenliği
description: Azure platformu bütünlüğü ve güvenliğine teknik genel bakış.
author: yosharm
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 11/10/2020
ms.openlocfilehash: 4755bc19a645d196487f0b8e0f4d1ef2120723ca
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94558051"
---
# <a name="platform-integrity-and-security-overview"></a>Platform bütünlüğü ve güvenliğine genel bakış
Azure filo, binlerce sunucudan (ana bilgisayar), her gün daha fazla eklenmiş binlerce sunucudan (ana bilgisayar) oluşur. Ayrıca binlerce ana bilgisayar, yeniden başlatmalar, işletim sistemi yenilemeleri veya onarımlar aracılığıyla günlük olarak bakım de alır. Bir ana bilgisayar filo 'e katılabilmek ve müşteri iş yüklerini kabul etmeye başlamadan önce, Microsoft konağın güvenli ve güvenilir bir durumda olduğunu doğrular. Bu doğrulama, sağlama zinciri veya bakım iş akışları sırasında önyükleme sırası bileşenlerinde kötü veya yanlışlıkla yapılan değişikliklerin gerçekleşmemesini sağlar.

## <a name="securing-azure-hardware-and-firmware"></a>Azure donanımının ve üretici yazılımının güvenliğini sağlama
Bu makale serisi, Microsoft 'un, üretim yaşam döngüsünün gün sonuna kadar olan çeşitli aşamalar aracılığıyla ana bilgisayarların bütünlüğünü ve güvenliğini nasıl sağlar. Makale adresi:
 
- [Bellenim güvenliği](firmware.md)
- [UEFı güvenli önyükleme](secure-boot.md)
- [Ölçülen önyükleme ve konak kanıtlama](measured-boot-host-attestation.md)
- [Proje Cerberus](project-cerberus.md)
- [Bekleme sırasında şifreleme](encryption-atrest.md)
- [Hiper yönetici güvenliği](hypervisor.md)
 
## <a name="next-steps"></a>Sonraki adımlar

- Microsoft 'un sürekli olarak bulut donanım ekosistemi içinde iş ortaklarının sürekli [yazılım güvenlik geliştirmelerini](firmware.md)nasıl sağladığını öğrenin.

- [Bulutta paylaşılan sorumluluğu](shared-responsibility.md)anlayın.