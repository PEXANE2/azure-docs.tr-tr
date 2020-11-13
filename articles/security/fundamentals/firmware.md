---
title: Bellenim güvenliği-Azure güvenliği
description: Microsoft 'un Azure donanım ve bellenim güvenliğini nasıl sağladığını öğrenin.
author: yosharm
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 11/10/2020
ms.openlocfilehash: 8233cc714d977083f4d55716d35c0b7094a069ea
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94558043"
---
# <a name="firmware-security"></a>Bellenim güvenliği
Bu makalede, Microsoft 'un bulut donanımı ekosistemi ve tedarik zincirlerinin nasıl güvenlik altına aldığı açıklanır.

## <a name="securing-the-cloud-hardware-ecosystem"></a>Bulut donanım ekosisteminin güvenliğini sağlama
Microsoft tarafından sürekli güvenlik geliştirmeleri sağlamak için bulut donanım ekosistemi içindeki Microsoft Etkin iş ortakları:

- Azure donanım ve bellenim iş ortakları (bileşen üreticileri ve sistem tümleştiricileri gibi) ile birlikte çalışarak Azure donanım ve bellenim güvenlik gereksinimlerini karşılayın.

- İş ortaklarının, şu gibi alanlarda Microsoft tarafından tanımlanan gereksinimleri kullanarak ürünlerinin güvenlik duruşlarını sürekli değerlendirmesi ve geliştirmesi gerçekleştirmesini sağlar:

  - Bellenim güvenli önyükleme
  - Bellenim güvenli kurtarma
  - Bellenim güvenli güncelleştirmesi
  - Bellenim şifrelemesi
  - Kilitli donanım
  - Ayrıntılı hata ayıklama telemetrisi
  - Ölçülen önyüklemeyi etkinleştirmek için TPM 2,0 donanımı için sistem desteği

- Belirtimlerin geliştirilmesi sayesinde [Açık Işlem projesi (OCP)](https://www.opencompute.org/wiki/Security) güvenlik projesine ilgi ve katkıda bulunma. Özellikler, ekosistemde güvenli tasarım ve mimari için tutarlılık ve açıklık yükseltir.

   > [!NOTE]
   > OCP güvenlik projesine katkımız bir örnek olarak [donanım güvenli önyükleme](https://docs.google.com/document/d/1Se1Dd-raIZhl_xV3MnECeuu_I0nF-keg4kqXyK4k4Wc/edit#heading=h.5z2d7x9gbhk0) belirtimidir.

## <a name="securing-hardware-and-firmware-supply-chains"></a>Donanım ve bellenim tedarik zincirlerinin güvenliğini sağlama
Azure için bulut Donanım tedarikçileri ve satıcıları, Microsoft tarafından geliştirilen zincir güvenlik işlemlerine ve gereksinimlerine bağlı kalmak için de gereklidir. Donanım ve bellenim geliştirme ve dağıtım süreçleri, aşağıdaki gibi Microsoft [güvenlik geliştirme yaşam döngüsü](https://www.microsoft.com/securityengineering/sdl) (SDL) süreçlerini izlemek için gereklidir:

- Tehdit modelleme
- Güvenli tasarım incelemeleri
- Bellenim İncelemeleri ve sızma testi
- Güvenli derleme ve test ortamları
- Güvenlik güvenlik açığı yönetimi ve olay yanıtı

## <a name="next-steps"></a>Sonraki adımlar
Platform bütünlüğünü ve güvenliğini sağlamak için yaptığımız hakkında daha fazla bilgi edinmek için bkz.:

- [Güvenli önyükleme](secure-boot.md)
- [Ölçülen önyükleme ve konak kanıtlama](measured-boot-host-attestation.md)
- [Proje Cerberus](project-cerberus.md)
- [Bekleme sırasında şifreleme](encryption-atrest.md)
- [Hiper yönetici güvenliği](hypervisor.md)