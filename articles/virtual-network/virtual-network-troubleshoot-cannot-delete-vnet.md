---
title: Azure'da sanal ağ silinemez | Microsoft Dokümanlar
description: Azure'da sanal ağı silemeyeceğiniz sorunu nasıl gidermeyeceğinizi öğrenin.
services: virtual-network
documentationcenter: na
author: chadmath
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 2d427a8b40fcb537801ce76aae6bc32fcda3a307
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71056936"
---
# <a name="troubleshooting-failed-to-delete-a-virtual-network-in-azure"></a>Sorun giderme: Azure'da sanal bir ağı silmek için başarısız oldu

Microsoft Azure'daki bir sanal ağı silmeye çalıştığınızda hatalar alabilirsiniz. Bu makalede, bu sorunu çözmenize yardımcı olacak sorun giderme adımları sağlar. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Sorun giderme rehberi 

1. [Sanal ağ ağ geçidinin sanal ağda çalışıp çalışmadığını denetleyin.](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network)
2. [Sanal ağda bir uygulama ağ geçidinin çalışıp çalışmadığını denetleyin.](#check-whether-an-application-gateway-is-running-in-the-virtual-network)
3. [Azure Active Directory Etki Alanı Hizmetinin sanal ağda etkin olup olmadığını kontrol edin.](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network)
4. [Sanal ağın diğer kaynağa bağlı olup olmadığını denetleyin.](#check-whether-the-virtual-network-is-connected-to-other-resource)
5. [Sanal bir makinenin sanal ağda çalışmaya devam edip etmediğini kontrol edin.](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network)
6. [Sanal ağın geçişte takılıp takılmadığını denetleyin.](#check-whether-the-virtual-network-is-stuck-in-migration)

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

### <a name="check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network"></a>Sanal ağ ağ geçidinin sanal ağda çalışıp çalışmadığını denetleme

Sanal ağı kaldırmak için öncelikle sanal ağ ağ ağ geçidini kaldırmanız gerekir.

Klasik sanal ağlar için Azure portalındaki klasik sanal ağın **Genel Bakış** sayfasına gidin. VPN **bağlantıları** bölümünde, ağ geçidi sanal ağda çalışıyorsa, ağ geçidinin IP adresini görürsünüz. 

![Ağ geçidinin çalışıp çalışmadığını denetleme](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Sanal ağlar için sanal ağın **Genel Bakış** sayfasına gidin. Sanal ağ ağ geçidi için **Bağlı aygıtları** denetleyin.

![Bağlı cihazı kontrol edin](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Ağ geçidini kaldırmadan önce, ağ geçidindeki **bağlantı** nesnelerini kaldır. 

### <a name="check-whether-an-application-gateway-is-running-in-the-virtual-network"></a>Sanal ağda bir uygulama ağ geçidinin çalışıp çalışmadığını denetleme

Sanal ağın **Genel Bakış** sayfasına gidin. Uygulama ağ geçidi için **Bağlı aygıtları** denetleyin.

![Bağlı cihazı kontrol edin](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Bir uygulama ağ geçidi varsa, sanal ağı silmeden önce bu ağ kaldırmanız gerekir.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network"></a>Azure Active Directory Etki Alanı Hizmetinin sanal ağda etkin olup olmadığını kontrol edin

Active Directory Etki Alanı Hizmeti etkinleştirilmiş ve sanal ağa bağlıysa, bu sanal ağı silemezsiniz. 

![Bağlı cihazı kontrol edin](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

Hizmeti devre dışı kalmak için Azure [portalını kullanarak Azure Etkin Dizin Etki Alanı Hizmetlerini Devre Dışı Kınama](../active-directory-domain-services/delete-aadds.md)'ya bakın.

### <a name="check-whether-the-virtual-network-is-connected-to-other-resource"></a>Sanal ağın diğer kaynağa bağlı olup olmadığını denetleme

Devre Bağlantıları, bağlantılar ve sanal ağ eşlemelerini denetleyin. Bunlardan herhangi biri, sanal ağ silme işleminin başarısız lığa neden olabilir. 

Önerilen silme sırası aşağıdaki gibidir:

1. Ağ geçidi bağlantıları
2. Ağ geçitleri
3. ıps
4. Sanal ağ eşlemeleri
5. Uygulama Hizmet Ortamı (ASE)

### <a name="check-whether-a-virtual-machine-is-still-running-in-the-virtual-network"></a>Sanal ağda sanal bir makinenin hala çalışıp çalışmadığını kontrol edin

Sanal ağda sanal makine olmadığından emin olun.

### <a name="check-whether-the-virtual-network-is-stuck-in-migration"></a>Sanal ağın geçişte takılıp takılmadığını denetleme

Sanal ağ geçiş durumunda sıkışmışsa, silinemez. Geçişi iptal etmek için aşağıdaki komutu çalıştırın ve ardından sanal ağı silin.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Sanal Ağ](virtual-networks-overview.md)
- [Azure Sanal Ağ hakkında sık sorulan sorular (SSS)](virtual-networks-faq.md)
