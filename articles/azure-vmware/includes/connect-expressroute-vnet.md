---
title: ExpressRoute 'ı sanal ağ geçidine bağlama
description: ExpressRoute 'ı sanal ağ geçidine bağlama adımları.
ms.topic: include
ms.date: 12/08/2020
ms.openlocfilehash: 5f9a565a7662041dbd85e61388129496fa376962
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96861531"
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

   :::image type="content" source="../media/expressroute-global-reach/open-cloud-shell.png" alt-text="Bağlantı Ekle sayfasında, alanlar için değerler sağlayın ve Tamam ' ı seçin." border="true" lightbox="../media/expressroute-global-reach/open-cloud-shell.png":::

ExpressRoute bağlantı hattı ve sanal ağınız arasındaki bağlantı oluşturulur.