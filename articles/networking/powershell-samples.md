---
title: Azure PowerShell örnekleri-ağ
description: Çok katmanlı uygulamalar için sanal ağ oluşturma örneği de dahil olmak üzere ağ için Azure PowerShell örnekler hakkında bilgi edinin.
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
ms.openlocfilehash: aa3304cbabb183368f0e47415e64120854f43535
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87282084"
---
# <a name="azure-powershell-samples-for-networking"></a>Ağ için Azure PowerShell örnekleri

Aşağıdaki tabloda Azure PowerShell kullanılarak oluşturulan betiklerin bağlantıları yer almaktadır.

| Komut Dosyası | Açıklama |
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
