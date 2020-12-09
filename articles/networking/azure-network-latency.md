---
title: Azure ağ gidiş dönüş gecikme süresi istatistikleri | Microsoft Docs
description: Azure bölgeleri arasında gidiş dönüş gecikme süresi istatistikleri hakkında bilgi edinin.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 12/07/2020
ms.author: kumud
ms.openlocfilehash: fb828d239266691766f55c1b156831afab7cc5bc
ms.sourcegitcommit: 48cb2b7d4022a85175309cf3573e72c4e67288f5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96854687"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Azure ağ gidiş dönüş gecikme süresi istatistikleri

Azure, iç izleme araçlarının yanı sıra üçüncü taraf yapay bir izleme hizmeti olan [ThousandEyes](https://thousandeyes.com)tarafından toplanan ölçümleri kullanarak ağının temel alanlarının gecikme süresini (hızını) sürekli olarak izler.

## <a name="how-are-the-measurements-collected"></a>Ölçümler nasıl toplanır?

Gecikme ölçümleri, dünya çapındaki Azure bulut bölgelerinde barındırılan ThousandEyes aracılarından toplanır ve bunlar arasında sürekli olarak 1 dakikalık aralıklarla ağ araştırmaları gönderilir. Aylık gecikme istatistikleri, bu ay için toplanan örneklerin ortalarından ortalaması alınır.

## <a name="november-2020-round-trip-latency-figures"></a>Kasım 2020 gidiş dönüş gecikme rakamları

Son 30 güne ait Azure bölgeleri arasındaki aylık ortalama gidiş dönüş süreleri (30 Kasım 2020 ' de sona eriyor) aşağıda gösterilmiştir. Aşağıdaki ölçümler [ThousandEyes](https://thousandeyes.com)tarafından desteklenmektedir.

[![Azure bölgeler arası gecikme istatistikleri](media/azure-network-latency/azure-network-latency.png)](media/azure-network-latency/azure-network-latency.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

[Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında bilgi edinin.
