---
title: Azure ağ gidiş dönüş gecikme süresi istatistikleri | Microsoft Docs
description: Azure bölgeleri arasında gidiş dönüş gecikme süresi istatistikleri hakkında bilgi edinin.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 08/05/2020
ms.author: kumud
ms.openlocfilehash: a1ae6386ddbf9e8bc226598b0b9218ba20669356
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91336368"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Azure ağ gidiş dönüş gecikme süresi istatistikleri

Azure, iç izleme araçlarının yanı sıra üçüncü taraf yapay bir izleme hizmeti olan [ThousandEyes](https://thousandeyes.com)tarafından toplanan ölçümleri kullanarak ağının temel alanlarının gecikme süresini (hızını) sürekli olarak izler.

## <a name="how-are-the-measurements-collected"></a>Ölçümler nasıl toplanır?

Gecikme ölçümleri, dünya çapındaki Azure bulut bölgelerinde barındırılan ThousandEyes aracılarından toplanır ve bunlar arasında sürekli olarak 1 dakikalık aralıklarla ağ araştırmaları gönderilir. Aylık gecikme istatistikleri, bu ay için toplanan örneklerin ortalarından ortalaması alınır.

## <a name="august-2020-round-trip-latency-figures"></a>Ağustos 2020, gidiş dönüş gecikme rakamları

Son 31 gün boyunca Azure bölgeleri arasındaki aylık ortalama gidiş dönüş süreleri (31 Ağustos 2020 tarihinde sona eriyor) aşağıda gösterilmiştir. Aşağıdaki ölçümler [ThousandEyes](https://thousandeyes.com)tarafından desteklenmektedir.

[![Azure bölgeler arası gecikme istatistikleri](media/azure-network-latency/azure-network-latency.png)](media/azure-network-latency/azure-network-latency.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

[Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında bilgi edinin.
