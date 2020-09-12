---
title: 'VPN Gateway: ağ geçidi IP adresi ayarlarını değiştirme: Azure CLı'
description: Bu makalede, Azure CLı kullanarak yerel ağ geçidinizin IP adresi öneklerini değiştirme işlemi adım adım açıklanmaktadır.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: cherylmc
ms.openlocfilehash: d5656b60b3c94720ad0a5952f8f6524f90dc6c17
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89392638"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a>Azure CLı kullanarak yerel ağ geçidi ayarlarını değiştirme

Bazen yerel ağ geçidi adres ön eki veya ağ geçidi IP adresi değişiklerinizin ayarları değişir. Bu makalede yerel ağ geçidi ayarlarınızı nasıl değiştireceğiniz gösterilir. Ayrıca, aşağıdaki listeden farklı bir seçenek belirleyerek bu ayarları farklı bir yöntem kullanarak da değiştirebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before-you-begin"></a><a name="before"></a>Başlamadan önce

CLı komutlarının en son sürümünü (2,0 veya üzeri) yükler. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>IP adresi öneklerini değiştirme

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Ağ geçidi IP adresini değiştirme

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ Geçidi bağlantınızı doğrulayabilirsiniz. Bkz. [ağ geçidi bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).

