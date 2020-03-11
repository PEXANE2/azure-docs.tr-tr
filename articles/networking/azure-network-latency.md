---
title: Azure ağ gidiş dönüş gecikme süresi istatistikleri | Microsoft Docs
description: Azure bölgeleri arasındaki gidiş dönüş gecikme süresi istatistikleri hakkında bilgi edinin.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 03/10/2020
ms.author: kumud
ms.openlocfilehash: 3469daa8b81b20d5d0052a23ce3236fc4fde75de
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79082956"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Azure ağ gidiş dönüş gecikme süresi istatistikleri

Azure, iç izleme araçlarının yanı sıra üçüncü taraf yapay bir izleme hizmeti olan [ThousandEyes](https://thousandeyes.com)tarafından toplanan ölçümleri kullanarak ağının temel alanlarının gecikme süresini (hızını) sürekli olarak izler.

## <a name="how-are-the-measurements-collected"></a>Ölçümler nasıl toplanır?

Gecikme ölçümleri, dünya çapındaki Azure bulut bölgelerinde barındırılan ThousandEyes aracılarından toplanır ve bunlar arasında sürekli olarak 1 dakikalık aralıklarla ağ araştırmaları gönderilir. Aylık gecikme istatistikleri, bu ay için toplanan örneklerin ortalarından ortalaması alınır.

## <a name="february-2020-round-trip-latency-figures"></a>Şubat 2020 gidiş dönüş gecikme süresi rakamları

Son 29 güne ait Azure bölgeleri arasındaki aylık ortalama gidiş dönüş süreleri (29 Şubat 2020 ' de sona eriyor) aşağıda gösterilmiştir. Aşağıdaki ölçümler [ThousandEyes](https://thousandeyes.com)tarafından desteklenmektedir.

[Azure bölgeler arası gecikme istatistikleri ![](media/azure-network-latency/azure-network-latency.png)](media/azure-network-latency/azure-network-latency.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

[Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında bilgi edinin.
