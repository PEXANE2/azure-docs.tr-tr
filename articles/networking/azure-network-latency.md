---
title: Azure ağ gidiş dönüş gecikme süresi istatistikleri | Microsoft Docs
description: Azure bölgeleri arasında gidiş dönüş gecikme süresi istatistikleri hakkında bilgi edinin.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 12/04/2019
ms.author: mnayak
ms.openlocfilehash: 3947df81b67d5aefc1b628b6ddaf8275152a4cd3
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74893103"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Azure ağ gidiş dönüş gecikme süresi istatistikleri

Azure, iç izleme araçlarının yanı sıra üçüncü taraf yapay bir izleme hizmeti olan [ThousandEyes](https://thousandeyes.com)tarafından toplanan ölçümleri kullanarak ağının temel alanlarının gecikme süresini (hızını) sürekli olarak izler.

## <a name="how-are-the-measurements-collected"></a>Ölçümler nasıl toplanır?

Gecikme ölçümleri, dünya çapındaki Azure bulut bölgelerinde barındırılan ThousandEyes aracılarından toplanır ve bunlar arasında sürekli olarak 1 dakikalık aralıklarla ağ araştırmaları gönderilir. Aylık gecikme istatistikleri, bu ay için toplanan örneklerin ortalarından ortalaması alınır.

## <a name="november-2019-latency-figures"></a>Kasım 2019 gecikme rakamları

**Kasım güncelleştirmesi:** 3 bölge eklendi.

* Norveç Doğu
* Norveç Batı
* Avustralya

Son 30 güne ait Azure bölgeleri arasındaki aylık ortalama gidiş dönüş süreleri (30 Kasım 2019 ' de sona eriyor) aşağıda gösterilmiştir. Aşağıdaki ölçümler [ThousandEyes](https://thousandeyes.com)tarafından desteklenmektedir.

![Azure bölgeler arası gecikme istatistikleri](media/azure-network-latency/latency-nov-2019.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında bilgi edinin.
