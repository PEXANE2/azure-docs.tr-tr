---
title: Azure PowerShell örnekleri-ağ
description: Azure PowerShell Örnekleri
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/24/2017
ms.author: gwallace
ms.openlocfilehash: ca6ac145db0536d3cf7e5bcc72a58d72101ab12a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81459126"
---
# <a name="azure-powershell-samples-for-networking"></a>Ağ için Azure PowerShell örnekleri

Aşağıdaki tabloda Azure PowerShell kullanılarak oluşturulan betiklerin bağlantıları yer almaktadır.

| | |
|-|-|
|**Azure kaynakları arasında bağlantı**||
| [Çok katmanlı uygulamalar için sanal ağ oluşturma](./scripts/virtual-network-powershell-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Ön uç ve arka uç alt ağları ile sanal ağ oluşturur. 1433 numaralı bağlantı noktası için, ön uç alt ağına giden trafik HTTP ile sınırlıyken, arka uç alt ağına giden trafik SQL ile sınırlıdır. |
| [İki sanal ağı eşleme](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Aynı bölgede iki sanal ağ oluşturur ve bunları bağlar. |
| [Bir ağ sanal gereci yoluyla trafiği yönlendirme](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | İki alt ağ arasında trafiği yönlendirebilen bir sanal makine ve ön uç ve arka uç alt ağları içeren bir sanal ağ oluşturur. |
| [Gelen ve giden sanal makine ağ trafiğini filtreleme](./scripts/virtual-network-powershell-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Ön uç ve arka uç alt ağları ile sanal ağ oluşturur. Ön uç alt ağına gelen ağ trafiği, HTTP ve HTTPS ile sınırlıdır. Arka uç alt ağından Internet 'e giden trafiğe izin verilmez. |
|**Yük Dengeleme ve trafik yönü**||
| [Yüksek kullanılabilirlik için VM 'lere Yük Dengeleme trafiği](./scripts/load-balancer-windows-powershell-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | Yüksek oranda kullanılabilir ve yük dengeli bir yapılandırmada birkaç sanal makine oluşturur. |
| [Yüksek uygulama kullanılabilirliği için birden çok bölge genelinde trafiği yönlendirme](./scripts/traffic-manager-powershell-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  İki App Service planı, iki Web uygulaması, bir Traffic Manager profili ve iki Traffic Manager uç noktası oluşturur. |
| | |
