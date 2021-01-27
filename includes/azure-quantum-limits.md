---
title: include dosyası
description: include dosyası
author: danielstocker
ms.service: azure-quantum
ms.topic: include
ms.date: 01/08/2021
ms.author: dasto
ms.openlocfilehash: c312ee63f9f00e4eef726924fc01f2862ba2884f
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98920484"
---
### <a name="provider-limits--quota"></a>Sağlayıcı sınır & kotası

Azure hisse hizmeti, birinci ve üçüncü taraf hizmet sağlayıcılarını destekler. Üçüncü taraf sağlayıcılarının sınırları ve kotaları vardır. Kullanıcılar, sağlayıcı dikey penceresinde üçüncü taraf sağlayıcıları yapılandırırken Azure portal teklifleri ve limitleri görüntüleyebilirler. 

Microsoft 'un ilk taraf Iyileştirme çözümleri sağlayıcısı için yayımlanmış kota sınırlarını bulabilirsiniz. 

#### <a name="learn--develop-sku"></a>SKU geliştirme & öğrenin

| Kaynak | Sınır |
| --- | --- |
| CPU tabanlı eşzamanlı işler | en fazla 5 eşzamanlı iş |
| FPGA tabanlı eşzamanlı işler | 2 adede kadar eşzamanlı iş |
| CPU tabanlı çözücü saatleri | ayda 20 saat  |
| FPGA tabanlı çözücü saatleri | ayda 1 saat  |

Öğrenme & geliştirme SKU 'SU kullanıyorsanız, kota sınırlarınıza bir artış **isteyemezsiniz** . Bunun yerine, ölçek SKU 'sunda performansa geçmeniz gerekir.

#### <a name="performance-at-scale-sku"></a>Ölçek SKU 'sunda performans

| Kaynak | Sınır |
| --- | --- |
| CPU tabanlı eşzamanlı işler | 100 adede kadar eşzamanlı iş |
| FPGA tabanlı eşzamanlı işler | 10 adede kadar eşzamanlı iş |
| Çözücü saati | ayda 50.000 saat  |

Sınır artışı istemeniz gerekiyorsa lütfen Azure desteğine ulaşın. 

Daha fazla bilgi için lütfen [Azure hisse fiyatlandırma sayfasını](https://aka.ms/AQ/Pricing)gözden geçirin.
Üçüncü taraf teklifleri hakkında daha fazla bilgi için lütfen Azure portal ilgili sağlayıcı sayfasını gözden geçirin.
