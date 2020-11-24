---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 923620c1c9719d857cc848507a4c5507f528d34e
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95555499"
---
### <a name="to-view-local-network-gateways"></a>Yerel ağ geçitlerini görüntülemek için

Yerel ağ geçitlerinin bir listesini görüntülemek için [az network local gateway list](/cli/azure/network/local-gateway) komutunu kullanın.

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="to-verify-the-shared-key-values"></a>Paylaşılan anahtar değerlerini doğrulamak için

Paylaşılan anahtar değerinin, VPN cihaz yapılandırmanızda kullandığınız değerle aynı olduğunu doğrulayın. Aynı değilse, cihazdan alınan değeri kullanarak bağlantıyı yeniden çalıştırın veya cihazı dönüş değeriyle güncelleştirin. Değerler eşleşmelidir. Paylaşılan anahtarı görüntülemek için [az network vpn-connection-list](/cli/azure/network/vpn-connection) komutunu kullanın.

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="to-view-the-vpn-gateway-public-ip-address"></a>VPN ağ geçidi Genel IP Adresini görüntülemek için

Sanal ağ geçidinizin genel IP adresini bulmak için [az network public-ip list](/cli/azure/network/public-ip) komutunu kullanın. Kolay okunması için, bu örneğin çıktısı genel IP’lerin tablo biçiminde gösterileceği şekilde biçimlendirilmiştir.

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```