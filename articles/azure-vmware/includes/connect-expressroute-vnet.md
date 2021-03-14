---
title: ExpressRoute 'ı sanal ağ geçidine bağlama
description: ExpressRoute 'ı sanal ağ geçidine bağlama adımları.
ms.topic: include
ms.date: 12/08/2020
ms.openlocfilehash: cea07f9a75e91b2b8ff444c9dda4a59d421ca37c
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462380"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-configure-networking.md -->

1. [Azure 'Da vSphere kümesi dağıtma](../tutorial-create-private-cloud.md) öğreticisinde oluşturduğunuz özel buluta gidin. **Yönet** altında **bağlantı** ' yı seçin, **ExpressRoute** sekmesini seçin.

1. Yetkilendirme anahtarını kopyalayın. Bir yetkilendirme anahtarı yoksa, oluşturmanız gerekir ve **Yetkilendirme anahtarı iste**' yi seçin.

   :::image type="content" source="../media/expressroute-global-reach/start-request-auth-key.png" alt-text="Yetkilendirme anahtarını kopyalayın. Bir yetkilendirme anahtarı yoksa, oluşturmanız gerekir ve yetkilendirme anahtarı ıste ' yi seçin." border="true" lightbox="../media/expressroute-global-reach/start-request-auth-key.png":::

1. Önceki adımda oluşturduğunuz sanal ağ geçidine gidin ve **Ayarlar** altında **Bağlantılar**' ı seçin. **Bağlantılar** sayfasında **+ Ekle**' yi seçin.

1. **Bağlantı ekle** sayfasında, alanlar için değerler sağlayın ve **Tamam**' ı seçin. 

   | Alan | Değer |
   | --- | --- |
   | **Ad**  | Bağlantı için bir ad girin.  |
   | **Bağlantı türü**  | **ExpressRoute**' ı seçin.  |
   | **Yetkilendirmeyi kullanma**  | Bu kutunun seçili olduğundan emin olun.  |
   | **Sanal ağ geçidi** | Daha önce oluşturduğunuz sanal ağ geçidi.  |
   | **Yetkilendirme anahtarı**  | Yetkilendirme anahtarını kopyalayın ve kaynak grubunuzun ExpressRoute sekmesinden yapıştırın. |
   | **Eş devre URI 'SI**  | ExpressRoute KIMLIĞINI kopyalayın ve kaynak grubunuzun ExpressRoute sekmesinden yapıştırın.  |

   :::image type="content" source="../media/expressroute-global-reach/expressroute-add-connection.png" alt-text="ExpressRoute 'ı sanal ağ geçidine bağlamak için bağlantı ekle sayfasının ekran görüntüsü.":::

ExpressRoute bağlantı hattı ve sanal ağınız arasındaki bağlantı oluşturulur.

:::image type="content" source="../media/expressroute-global-reach/virtual-network-gateway-connections.png" alt-text="Sanal ağ geçidi bağlantılarının ekran görüntüsü.":::