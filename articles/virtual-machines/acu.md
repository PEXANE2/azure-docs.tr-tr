---
title: Azure İşlem Birimine Genel Bakış | Microsoft Dokümanlar
description: Azure bilgi işlem birimleri kavramına genel bakış. ACU, Azure SNU'lar arasında CPU performansını karşılaştırmanın bir yolunu sağlar.
services: virtual-machines
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager,azure-service-management
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: mimckitt
ms.openlocfilehash: e344d09497a30dec546dfaedd3d78f30c7d214d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535706"
---
# <a name="azure-compute-unit-acu"></a>Azure işlem birimi (ACU)

Azure İşlem Birimi (ACU) kavramı, Azure SK'leri genelinde bilgi işlem (CPU) performansını karşılaştırmanın bir yolunu sağlar. Bu birim, performans ihtiyaçlarınızı karşılayabilecek SKU'yu kolayca belirlemenize yardımcı olacak. ACU şu anda Küçük (Standard_A1) VM'de 100 olarak standart haline getirilmiş ve diğer tüm SKU'lar, standart bir karşılaştırmalı testte sunabilecekleri yaklaşık hıza göre derecelendirilmiştir.

> [!IMPORTANT]
> ACU yalnızca rehberlik etme amacı taşımaktadır. İş yükünüzle aldığınız sonuçlar farklılık gösterebilir.
<br>

| SKU Ailesi | ACU \ vCPU | vCPU: Çekirdek |
| --- | --- |---|
| [A0](sizes-previous-gen.md) |50 | 1:1 |
| [A1 - A4](sizes-previous-gen.md) |100 | 1:1 |
| [A5 - A7](sizes-previous-gen.md) |100 | 1:1 |
| [A1_v2 - A8_v2](sizes-general.md) |100 | 1:1 |
| [A2m_v2 - A8m_v2](sizes-general.md) |100 | 1:1 |
| [A8 - A11](sizes-previous-gen.md) |225* | 1:1 |
| [D1 - D14](sizes-previous-gen.md) |160 - 250 | 1:1 |
| [D1_v2 - D15_v2](dv2-dsv2-series.md) |210-250* | 1:1 |
| [DS1 - DS14](sizes-previous-gen.md) |160 - 250 | 1:1 |
| [DS1_v2 - DS15_v2](dv2-dsv2-series.md) |210-250* | 1:1 |
| [D_v3](dv3-dsv3-series.md) |160 - 190* | 2:1\*\*\* |
| [Ds_v3](dv3-dsv3-series.md) |160 - 190* | 2:1\*\*\* |
| [E_v3](ev3-esv3-series.md) |160 - 190* | 2:1\*\*\*|
| [Es_v3](ev3-esv3-series.md) |160 - 190* | 2:1\*\*\* |
| [F2s_v2 - F72s_v2](fsv2-series.md) |195 - 210* | 2:1\*\*\* |
| [F1 - F16](sizes-previous-gen.md) |210-250* | 1:1 |
| [F1s - F16s](sizes-previous-gen.md) |210-250* | 1:1 |
| [G1 - G5](sizes-previous-gen.md) |180-240* | 1:1 |
| [GS1 - GS5](sizes-previous-gen.md) |180-240* | 1:1 |
| [H](h-series.md) |290-300* | 1:1 |
| [HB](hb-series.md) |199 - 216** | 1:1 |
| [Hc](hc-series.md) |297 - 315* | 1:1 |
| [L4s - L32s](sizes-previous-gen.md) |180-240* | 1:1 |
| [L8s_v2 - L80s_v2](lsv2-series.md) |150 - 175** | 2:1 |
| [M](m-series.md) | 160 - 180 | 2:1\*\*\* |

*AcUs, CPU sıklığını artırmak ve performans artışı sağlamak için Intel® Turbo teknolojisini kullanır.  Performans artışı miktarı, aynı ana bilgisayarda çalışan VM boyutuna, iş yüküne ve diğer iş yüklerine bağlı olarak değişebilir.
**ACUs CPU sıklığını artırmak ve bir performans artışı sağlamak için AMD® Boost teknolojisini kullanın.  Performans artışı miktarı, aynı ana bilgisayarda çalışan VM boyutuna, iş yüküne ve diğer iş yüklerine bağlı olarak değişebilir.
Hiper iş parçacığı ve iç içe sanallaştırma çalıştırma yeteneğine sahip

Burada farklı boyutları hakkında daha fazla bilgi için bağlantılar şunlardır:

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
