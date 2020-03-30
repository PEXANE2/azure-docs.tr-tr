---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d36d2be59010c47348a8e196b28d87e5b967868e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188242"
---
### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>Adım 1: Sanal ağ ağ geçidine gidin

1. Azure [portalında](https://portal.azure.com)Tüm **kaynaklara**gidin. 
2. Sanal ağ ağ geçidi sayfasını açmak için, silmek istediğiniz sanal ağ ağ geçidine gidin ve tıklatın.

### <a name="step-2-delete-connections"></a>Adım 2: Bağlantıları silme

1. Sanal ağ ağ ağ geçidinizin sayfasında, ağ geçidine giden tüm bağlantıları görüntülemek için **Bağlantılar'ı** tıklatın.
2. Bağlantının adının satırındaki **'...'** seçeneğini tıklatın ve ardından açılır yerden **Sil'i** seçin.
3. Bağlantıyı silmek istediğinizi doğrulamak için **Evet'i** tıklatın. Birden çok bağlantınız varsa, her bağlantıyı silin.

### <a name="step-3-delete-the-virtual-network-gateway"></a>Adım 3: Sanal ağ ağ geçidini silme

S2S yapılandırmanıza ek olarak bu VNet'te bir P2S yapılandırmanız varsa, sanal ağ ağ ağ geçidini silmenin uyarı yapmadan tüm P2S istemcilerinin bağlantısını otomatik olarak keseceğini unutmayın.

1. Sanal ağ ağ geçidi sayfasında **Genel Bakış'ı**tıklatın.
2. Genel **Bakış** sayfasında, ağ geçidini silmek için **Sil'i** tıklatın.
