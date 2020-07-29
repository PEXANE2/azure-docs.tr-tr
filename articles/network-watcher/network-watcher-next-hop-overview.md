---
title: Azure ağ Izleyicisi 'nde sonraki atlamaya giriş | Microsoft Docs
description: Bu makalede, ağ Izleyicisi sonraki atlama özelliğine bir genel bakış sunulmaktadır.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76844067"
---
# <a name="use-next-hop-to-diagnose-virtual-machine-routing-problems"></a>Sanal makine yönlendirme sorunlarını tanılamak için sonraki atlamayı kullanın

Bir sanal makineden (VM) gelen trafik, bir ağ arabirimi (NIC) ile ilişkili etkin yollara göre bir hedefe gönderilir. Sonraki atlama, belirli bir VM ve NIC 'den bir paketin sonraki atlama türünü ve IP adresini alır. Sonraki atlamanın bilinmesi, trafiğin hedeflenen hedefe yönlendirilip yönlendirilmediğini veya trafiğin ne yerde gönderilip gönderilmediğini belirlemenize yardımcı olur. Trafiğin şirket içi bir konuma veya sanal bir gereci yönlendirildiği, bağlantı sorunlarına yol açabilecek yolların yanlış yapılandırılması. Sonraki atlama Ayrıca sonraki atlama ile ilişkili yol tablosunu da döndürür. Yol, Kullanıcı tanımlı bir yol olarak tanımlanmışsa, bu yol döndürülür. Aksi halde, sonraki atlama **sistem yolu**döndürür.

![sonraki atlamaya genel bakış](./media/network-watcher-next-hop-overview/figure1.png)

Sonraki atlama özelliği tarafından döndürülebilecek sonraki atlamalar aşağıdaki gibidir:

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VirtualNetwork
* VirtualNetworkPeering
* VirtualNetworkServiceEndpoint 
* MicrosoftEdge
* Hiçbiri

Sonraki atlama türleri hakkında daha fazla bilgi edinmek için bkz. [yönlendirmeye genel bakış](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

VM ağı yönlendirme sorunlarını tanılamak için sonraki atlamayı nasıl kullanacağınızı öğrenmek için bkz. [Azure Portal](diagnose-vm-network-routing-problem.md), [POWERSHELL](diagnose-vm-network-routing-problem-powershell.md)veya [Azure CLI](diagnose-vm-network-routing-problem-cli.md)kullanarak VM ağı yönlendirme sorunlarını tanılama.
