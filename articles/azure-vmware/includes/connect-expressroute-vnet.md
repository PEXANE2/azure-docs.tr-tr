---
title: ExpressRoute 'ı sanal ağ geçidine bağlama
description: ExpressRoute 'ı sanal ağ geçidine bağlama adımları.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 214ef9c01193b238c8e456ef2809f7a2edbdb6c7
ms.sourcegitcommit: ffa7a269177ea3c9dcefd1dea18ccb6a87c03b70
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91598163"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-configure-networking.md -->

1. Önceki öğreticide oluşturduğunuz özel buluta gidin ve **Yönet**' ın altındaki **bağlantı** ' yı seçin, **ExpressRoute** sekmesini seçin.

1. Yetkilendirme anahtarını kopyalayın. Bir yetkilendirme anahtarı yoksa, oluşturmanız gerekir ve **Yetkilendirme anahtarı iste**' yi seçin.

   :::image type="content" source="../media/expressroute-global-reach/start-request-auth-key.png" alt-text="Yetkilendirme anahtarını kopyalayın. Bir yetkilendirme anahtarı yoksa, oluşturmanız gerekir ve yetkilendirme anahtarı ıste ' yi seçin." border="true" lightbox="../media/expressroute-global-reach/start-request-auth-key.png":::

1. Önceki adımda oluşturduğunuz sanal ağ geçidine gidin ve **Ayarlar**altında **Bağlantılar**' ı seçin. **Bağlantılar** sayfasında **+ Ekle**' yi seçin.

1. **Bağlantı ekle** sayfasında, alanlar için değerler sağlayın ve **Tamam**' ı seçin. 

   | Alan | Değer |
   | --- | --- |
   | **Ad**  | Bağlantı için bir ad girin.  |
   | **Bağlantı türü**  | **ExpressRoute**' ı seçin.  |
   | **Yetkilendirmeyi kullanma**  | Bu kutunun seçili olduğundan emin olun.  |
   | **Sanal ağ geçidi** | Daha önce oluşturduğunuz sanal ağ geçidi.  |
   | **Yetkilendirme anahtarı**  | Yetkilendirme anahtarını kopyalayın ve kaynak grubunuzun ExpressRoute sekmesinden yapıştırın. |
   | **Eş devre URI 'SI**  | ExpressRoute KIMLIĞINI kopyalayın ve kaynak grubunuzun ExpressRoute sekmesinden yapıştırın.  |

   :::image type="content" source="../media/expressroute-global-reach/open-cloud-shell.png" alt-text="Yetkilendirme anahtarını kopyalayın. Bir yetkilendirme anahtarı yoksa, oluşturmanız gerekir ve yetkilendirme anahtarı ıste ' yi seçin." border="true" lightbox="../media/expressroute-global-reach/open-cloud-shell.png":::

ExpressRoute bağlantı hattı ve sanal ağınız arasındaki bağlantı oluşturulur.