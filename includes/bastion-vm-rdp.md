---
title: dosya dahil etme
description: dosya dahil etme
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 10/12/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b079ede0845de3794507691bc3c252930e2a6604
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978156"
---
1. [Azure portalını](https://portal.azure.com) açın. Bağlanmak istediğiniz sanal makineye gidin ve sonra **Bağlan**' ı seçin. Açılan **listeden savunma** ' yı seçin.

   :::image type="content" source="./media/bastion-vm-rdp/connect-vm.png" alt-text="Savunma seçin" border="false":::

1. Açılan listeden atlama ' yi seçtikten sonra, üç sekmeye sahip olan bir yan çubuk görüntülenir: RDP, SSH ve savunma. Sanal ağ için savunma sağlanmakta olduğundan, savunma sekmesi varsayılan olarak etkindir. **Kullanım kullanımı**' nı seçin.

   :::image type="content" source="./media/bastion-vm-rdp/use-bastion.png" alt-text="Savunma seçin" border="false":::

1. Azure savunma **kullanarak bağlan** sayfasında, sanal makinenizin Kullanıcı adını ve parolasını girin ve ardından **Bağlan**' ı seçin.

   :::image type="content" source="./media/bastion-vm-rdp/host.png" alt-text="Savunma seçin" border="false":::

1. Bu sanal makineyle savunma aracılığıyla RDP bağlantısı, bağlantı noktası 443 ve savunma hizmeti kullanılarak doğrudan Azure portal (HTML5 üzerinden) açılır.

   :::image type="content" source="./media/bastion-vm-rdp/connection.png" alt-text="Savunma seçin" border="false":::
