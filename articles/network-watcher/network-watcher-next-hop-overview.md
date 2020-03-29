---
title: Azure Ağ İzleyicisi'nde bir sonraki hop'a giriş | Microsoft Dokümanlar
description: Bu makalede, Ağ İzleyicisi sonraki atlama yeteneğine genel bir bakış sağlar.
services: network-watcher
documentationcenter: na
author: damendo
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 33c22b5b68b6677e8cf271dc185007316ec44500
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844067"
---
# <a name="use-next-hop-to-diagnose-virtual-machine-routing-problems"></a>Sanal makine yönlendirme sorunlarını tanılamak için sonraki atlamayı kullanın

Sanal makineden (VM) gelen trafik, ağ arabirimi (NIC) ile ilişkili etkili rotalara göre bir hedefe gönderilir. Sonraki atlama, belirli bir VM ve NIC'den bir paketin bir sonraki atlama türünü ve IP adresini alır. Bir sonraki atlamayı bilmek, trafiğin hedeflenen hedefe yönlendirilip yönlendirilemediğini veya trafiğin hiçbir yere gönderilmediğini belirlemenize yardımcı olur. Trafiğin şirket içi bir konuma veya sanal bir cihaza yönlendirildiği yolların uygunsuz bir yapılandırması bağlantı sorunlarına yol açabilir. Sonraki atlama da sonraki atlama ile ilişkili rota tablosu döndürür. Rota kullanıcı tanımlı bir rota olarak tanımlanırsa, bu rota döndürülür. Aksi takdirde, sonraki atlama **Sistem Rotası**döndürür.

![sonraki hop genel bakış](./media/network-watcher-next-hop-overview/figure1.png)

Sonraki atlama yeteneği tarafından döndürülebilir sonraki atlamaları aşağıdaki gibidir:

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VirtualNetwork
* VirtualNetworkPeering
* VirtualNetworkServiceEndpoint 
* MicrosoftEdge
* None

Sonraki her atlama türü hakkında daha fazla bilgi edinmek için [Yönlendirme'ye genel bakış'a](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)bakın.

## <a name="next-steps"></a>Sonraki adımlar

VM ağ yönlendirme sorunlarını tanılamak için sonraki atlamayı nasıl kullanacağınızı öğrenmek için Azure [portalı](diagnose-vm-network-routing-problem.md), [PowerShell](diagnose-vm-network-routing-problem-powershell.md)veya [Azure CLI'yi](diagnose-vm-network-routing-problem-cli.md)kullanarak VM ağ yönlendirme sorunlarını tanıla'ya bakın.
