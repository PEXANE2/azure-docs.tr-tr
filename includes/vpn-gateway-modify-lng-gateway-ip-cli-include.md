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
ms.openlocfilehash: 069d7af9cee0bee673c8f0b32659ce362fe4dd70
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96026495"
---
### <a name="to-modify-the-local-network-gateway-gatewayipaddress"></a>Yerel ağ geçidini değiştirmek için 'gatewayIpAddress'

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Ağ geçidi IP adresi, mevcut bir VPN ağ geçidi bağlantısı (varsa) kaldırılmadan değiştirilebilir. Ağ geçidi IP adresini değiştirmek için 'Site2' ve 'TestRG1' değerlerini [az network local-gateway update](/cli/azure/network/local-gateway) komutunu kullanarak istediğiniz değerlerle değiştirin.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

Çıktıda IP adresinin doğru olduğundan emin olun:

```
"gatewayIpAddress": "23.99.222.170",
```