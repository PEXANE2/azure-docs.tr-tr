---
title: 'VPN Ağ Geçidi: Ağ geçidi IP adresi ayarlarını değiştirin: Azure CLI'
description: Bu makale, Azure CLI'yi kullanarak yerel ağ ağ ağ açınızın IP adresi önekleri değiştirmede size yol açar.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 11/29/2017
ms.author: cherylmc
ms.openlocfilehash: bc051a7e0a19dc54431266cfa5f37131868bdc07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864052"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a>Azure CLI'yi kullanarak yerel ağ ağ ağ geçidi ayarlarını değiştirme

Bazen yerel ağ ağ geçidinizin ayarları Adres Önek veya Ağ Geçidi IP Adresi değişir. This article shows you how to modify your local network gateway settings. Aşağıdaki listeden farklı bir seçenek seçerek bu ayarları farklı bir yöntem kullanarak da değiştirebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portalında](vpn-gateway-modify-local-network-gateway-portal.md)
> * [Powershell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before-you-begin"></a><a name="before"></a>Başlamadan önce

CLI komutlarının (2.0 veya sonraki) en son sürümünü yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>IP adresi öneklerini değiştirme

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Ağ geçidi IP adresini değiştirme

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi bağlantınızı doğrulayabilirsiniz. Bkz. [Ağ geçidi bağlantısını doğrula.](vpn-gateway-verify-connection-resource-manager.md)

