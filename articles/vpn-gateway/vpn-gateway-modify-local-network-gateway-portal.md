---
title: 'VPN Ağ Geçidi: Ağ geçidi IP adresi ayarlarını değiştirin: Azure portalı'
description: Bu makale, Azure portalını kullanarak yerel ağ ağ ağ açınızın IP adresi önekleri değiştirmede size yol açar.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: fdb98242cede36f818604a7a5d713f5f7c75daad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864035"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Azure portalını kullanarak yerel ağ ağ ağ geçidi ayarlarını değiştirme

Bazen yerel ağ ağ geçidinizin ayarları AdresPrefix veya GatewayIPAddress değişir. This article shows you how to modify your local network gateway settings. Aşağıdaki listeden farklı bir seçenek seçerek bu ayarları farklı bir yöntem kullanarak da değiştirebilirsiniz:

Bağlantıyı silmeden önce, tanımlanan PSK'yi almak için bağlantı aygıtlarınızın yapılandırmasını indirmek isteyebilirsiniz. Böylece diğer tarafta yeniden tanımlamana gerek yok.

> [!div class="op_single_selector"]
> * [Azure portalında](vpn-gateway-modify-local-network-gateway-portal.md)
> * [Powershell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>IP adresi öneklerini değiştirme

IP adresi öneklerini değiştirdiğinizde, izdiğiniz adımlar yerel ağ ağ ağ geçidinizin bağlantısı olup olmadığına bağlıdır.

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Ağ geçidi IP adresini değiştirme

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Genel IP adresini değiştirdiğinizde, izdiğiniz adımlar yerel ağ ağ ağ geçidinizin bağlantısı olup olmadığına bağlıdır.

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi bağlantınızı doğrulayabilirsiniz. Bkz. [Ağ geçidi bağlantısını doğrula.](vpn-gateway-verify-connection-resource-manager.md)
