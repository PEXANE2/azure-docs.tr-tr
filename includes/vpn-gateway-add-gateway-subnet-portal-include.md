---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 52ab0413dffeee59cb9d34c6276a0c806a4d0322
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780239"
---
Ağ geçidi alt ağı, sanal ağ geçidi hizmetlerinin kullandığı ayrılmış IP adreslerini içerir. Ağ geçidi alt ağı oluşturun.

1. Portalda, sanal ağ geçidini oluşturmak istediğiniz sanal ağa gidin.
2. Sanal ağ sayfanızda **VNet1-alt ağlar** sayfasını genişletmek Için **alt ağlar** ' a tıklayın.
3. **Alt ağ ekle** sayfasını açmak için üst kısımdaki **+ Gateway alt ağına** tıklayın.

   ![Ağ geçidi alt ağını ekleme](./media/vpn-gateway-add-gateway-subnet-portal-include/gateway-subnet.png "Ağ geçidi alt ağını ekleme")
4. Alt ağınızın **adı** , gereken ' gatewaysubnet ' değeri ile otomatik olarak doldurulur. Otomatik doldurulmuş **adres aralığını (CIDR bloğu)** aşağıdaki değerle eşleşecek şekilde ayarlayın:

   **Adres aralığı (CIDR bloğu)** : 10.1.255.0/27

   ![Ağ geçidi alt ağını ekleme](./media/vpn-gateway-add-gateway-subnet-portal-include/add-gateway-subnet1.png "Ağ geçidi alt ağını ekleme")
5. Ayarların geri kalanını **none** veya **0 seçili**olarak bırakın. Sonra, ağ geçidi alt ağını oluşturmak için **Tamam** ' ı tıklatın.