---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/16/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b8a61586acd888301350277d924f3d4fe176b4c8
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148198"
---
### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>1. Adım: sanal ağ geçidine gidin

1. [Azure Portal](https://portal.azure.com) **tüm kaynaklar**' a gidin. 
2. Sanal ağ geçidi sayfasını açmak için, silmek istediğiniz sanal ağ geçidine gidin ve tıklayın.

### <a name="step-2-delete-connections"></a>2. Adım: bağlantıları silme

1. Sanal ağ geçidinizin sayfasında, ağ geçidine yönelik tüm bağlantıları görüntülemek için **Bağlantılar** ' a tıklayın.
2. Bağlantı adının satırındaki **'... '** düğmesine tıklayın ve ardından açılan menüden **Sil** ' i seçin.
3. Bağlantıyı silmek istediğinizi onaylamak için **Evet** ' e tıklayın. Birden çok bağlantınız varsa, her bir bağlantıyı silin.

### <a name="step-3-delete-the-virtual-network-gateway"></a>3. Adım: sanal ağ geçidini silme

S2S yapılandırmanıza ek olarak bu VNet 'e yönelik bir P2S yapılandırmasına sahipseniz, sanal ağ geçidinin silinmesi uyarı vermeden tüm P2S istemcilerinin bağlantısını otomatik olarak keser.

1. Sanal ağ geçidi sayfasında **Genel Bakış ' a**tıklayın.
2. **Genel bakış** sayfasında, ağ geçidini silmek için **Sil** ' e tıklayın.
