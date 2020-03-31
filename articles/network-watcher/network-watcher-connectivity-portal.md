---
title: Sorun giderme bağlantıları - Azure portalı
titleSuffix: Azure Network Watcher
description: Azure portalını kullanarak Azure Ağ İzleyicisi'nin bağlantı sorun giderme özelliğini nasıl kullanacağınızı öğrenin.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: damendo
ms.openlocfilehash: e405a91b1ea541b4ed3328fdb3bf80ca82731c93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283244"
---
# <a name="troubleshoot-connections-with-azure-network-watcher-using-the-azure-portal"></a>Azure portalını kullanarak Azure Ağ İzleyicisi ile bağlantıları giderme

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [Powershell](network-watcher-connectivity-powershell.md)
> - [Azure CLI](network-watcher-connectivity-cli.md)
> - [Azure REST API](network-watcher-connectivity-rest.md)

Sanal bir makineden belirli bir bitiş noktasına doğrudan TCP bağlantısının kurulup kurulamayacağını doğrulamak için bağlantı sorun giderini nasıl kullanacağınızı öğrenin.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aşağıdaki kaynaklara sahip varsayar:

* Bağlantıyla sorun gidermek istediğiniz bölgedeki Ağ İzleyicisi örneği.
* Bağlantıları gidermek için sanal makineler.

> [!IMPORTANT]
> Bağlantı sorun giderme, sorun giderdiğiniz VM'nin `AzureNetworkWatcherExtension` VM uzantısıyüklü olması gerekir. Uzantıyı Windows VM'de yüklemek [için Windows için Azure Network Watcher Agent sanal makine uzantısını](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ziyaret edin ve Linux VM için Linux için Azure Network [Watcher Agent sanal makine uzantısını](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)ziyaret edin. Uzantı, hedef bitiş noktasında gerekli değildir.

## <a name="check-connectivity-to-a-virtual-machine"></a>Sanal makineye bağlantıyı kontrol edin

Bu örnek, bağlantı noktası 80 üzerinden bir hedef sanal makineye bağlantı denetler.

Ağ İzleyicinize gidin ve **Bağlantı sorun giderin'i**tıklatın. Bağlantının denetimini kontrol etmek için sanal makineyi seçin. **Hedef** bölümünde **sanal bir makine seçin** ve test etmek için doğru sanal makine ve bağlantı noktasını seçin.

**Denetle'yi**tıklattığınızda, belirtilen bağlantı noktasındaki sanal makineler arasındaki bağlantı kontrol edilir. Örnekte, hedef VM erişilemez, atlamaları bir liste gösterilir.

![Sanal bir makine için bağlantı sonuçlarını denetleme][1]

## <a name="check-remote-endpoint-connectivity"></a>Uzak uç nokta bağlantısını denetleme

Uzak bir bitiş noktasına bağlantı ve gecikme süresini denetlemek için **Hedef** **bölümündeel radyo işaretle** düğmesini seçin, url'yi ve bağlantı noktasını girdiniz ve **Kontrol Et'i**tıklatın.  Bu, web siteleri ve depolama bitiş noktaları gibi uzak uç noktalar için kullanılır.

![Bir web sitesi için bağlantı sonuçlarını denetleme][2]

## <a name="next-steps"></a>Sonraki adımlar

[Uyarı tetiklenen paket yakalamayı](network-watcher-alert-triggered-packet-capture.md) görüntüleyerek Sanal makine uyarılarıyla paket yakalamaları nasıl otomatikleştirin

[KONTROL IP akışını doğrulayarak](diagnose-vm-network-traffic-filtering-problem.md) VM'nize belirli bir trafiğe izin verilip verilmediğini bulun

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png