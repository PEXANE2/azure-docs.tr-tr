---
title: Bellenim bütünlüğü-Azure güvenliği
description: Bellenim bütünlüğünü sağlamak için şifreleme ölçümleri hakkında bilgi edinin.
author: yosharm
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 11/10/2020
ms.openlocfilehash: f085858a9d550623704efd4f051ed525e55a37e0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94558012"
---
# <a name="project-cerberus"></a>Project Cerberus

Cerberus, klonlanamadığı bir kimlikle bir NıST 800-193 uyumlu donanım köküdür. Cerberus, bellenim bütünlüğü için güçlü bir güven bağlantısı sağlayarak Azure altyapısının güvenlik duruşunu daha fazla yükseltmek üzere tasarlanmıştır.

## <a name="enabling-an-anchor-of-trust"></a>Güven bağlayıcısını etkinleştirme
Her Cerberus yongasında, bir Microsoft sertifika yetkilisi (CA) için kök imzalı bir sertifika zinciri kullanılarak oluşturulan benzersiz bir şifreleme kimliği vardır. Cerberus 'lerden alınan ölçümler, şu gibi bileşenlerin bütünlüğünü doğrulamak için kullanılabilir:

- Ana bilgisayar
- Temel Kart Yönetim Denetleyicisi (BMC)
- Ağ arabirim kartı ve [a-bir yonga](https://en.wikipedia.org/wiki/System_on_a_chip) (SoC) dahil tüm çevre birimleri

Bu güven çıpası, platform üretici yazılımının şuradan savunmasına yardımcı olur:

- Platformda çalışan güvenliği aşılmış bellenim ikilileri
- İşletim sistemi, uygulama veya Hiper yöneticide hatalara yararlanan kötü amaçlı yazılımlar ve saldırganlar
- Belirli türde tedarik zinciri saldırıları (üretim, derleme, aktarım)
- Yönetim ayrıcalıklarına veya donanıma erişimi olan kötü amaçlı Insiders

## <a name="cerberus-attestation"></a>Cerberus kanıtlama
Cerberus, platform üretici yazılımı bildirimi (PFM) kullanarak sunucu bileşenleri için bellenim bütünlüğünü doğrular. PFM, yetkili bellenim sürümlerinin bir listesini tanımlar ve Azure [konak kanıtlama hizmetine](measured-boot-host-attestation.md)bir platform ölçümü sağlar. Konak kanıtlama hizmeti ölçümleri doğrular ve yalnızca güvenilen konaklara Azure filo ve ana bilgisayar müşteri iş yüklerine katılmasına izin vermek için bir belirleme sağlar.

Konak kanıtlama hizmeti ile birlikte, Cerberus ' özellikleri, yüksek güvenlikli bir Azure üretim altyapısını geliştirir ve yükseltir.

> [!NOTE]
> Daha fazla bilgi edinmek için bkz. GitHub 'daki [Proje sertifikası](https://github.com/opencomputeproject/Project_Olympus/tree/master/Project_Cerberus) bilgileri.

## <a name="next-steps"></a>Sonraki adımlar
Platform bütünlüğünü ve güvenliğini sağlamak için yaptığımız hakkında daha fazla bilgi edinmek için bkz.:

- [Üretici yazılımı güvenliği](firmware.md)
- [Güvenli önyükleme](secure-boot.md)
- [Ölçülen önyükleme ve konak kanıtlama](measured-boot-host-attestation.md)
- [Bekleme sırasında şifreleme](encryption-atrest.md)
- [Hiper yönetici güvenliği](hypervisor.md)