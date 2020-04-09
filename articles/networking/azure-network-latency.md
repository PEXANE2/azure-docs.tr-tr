---
title: Azure ağ gidiş-dönüş gecikme istatistikleri | Microsoft Dokümanlar
description: Azure bölgeleri arasındaki gidiş-dönüş gecikme istatistikleri hakkında bilgi edinin.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 03/10/2020
ms.author: kumud
ms.openlocfilehash: d9cae04499f046749e504bcab89b893fcc31a81c
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886967"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Azure ağ turu gecikme sonu istatistikleri

Azure, dahili izleme araçlarının yanı sıra üçüncü taraf sentetik izleme hizmeti [ThousandEyes](https://thousandeyes.com)tarafından toplanan ölçümleri kullanarak ağının temel alanlarının gecikmesini (hızını) sürekli olarak izler.

## <a name="how-are-the-measurements-collected"></a>Ölçümler nasıl toplanır?

Gecikme süreleri, dünya çapında Azure bulut bölgelerinde barındırılan ve 1 dakikalık aralıklarla ağ sondalarını sürekli olarak kendi aralarında gönderen ThousandEyes aracılarından toplanır. Aylık gecikme istatistikleri, toplanan ayın ortalama örneklerinden elde edilir.

## <a name="march-2020-round-trip-latency-figures"></a>Mart 2020 gidiş-dönüş gecikme rakamları

Azure bölgeleri arasında son 31 gün (31 Mart 2020'de sona erecek) aylık ortalama gidiş-dönüş süreleri aşağıda gösterilmiştir. Aşağıdaki ölçümler [ThousandEyes](https://thousandeyes.com)tarafından desteklenmektedir.

[![Azure bölgeler arası gecikme istatistikleri](media/azure-network-latency/azure-network-latency.png)](media/azure-network-latency/azure-network-latency.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Azure [bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında bilgi edinin.
