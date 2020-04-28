---
title: 'VPN Gateway: ağ geçidi IP adresi ayarlarını değiştir: Azure portal'
description: Bu makalede, Azure portal kullanarak yerel ağ geçidinizin IP adresi öneklerini değiştirme işlemi adım adım açıklanmaktadır.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: fdb98242cede36f818604a7a5d713f5f7c75daad
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75864035"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Azure portal kullanarak yerel ağ geçidi ayarlarını değiştirme

Bazen yerel ağ geçidi Adresispredüzeltmesini veya Gatewayıpaddress değişikliği ayarları değişir. Bu makalede yerel ağ geçidi ayarlarınızı nasıl değiştireceğiniz gösterilir. Ayrıca, aşağıdaki listeden farklı bir seçenek belirleyerek bu ayarları farklı bir yöntem kullanarak da değiştirebilirsiniz:

Bağlantıyı silmeden önce, tanımlanan PSK 'yi almak için bağlanan cihazlarınız için yapılandırmayı indirmek isteyebilirsiniz. Bu şekilde, diğer tarafta yeniden tanımlamanız gerekmez.

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>IP adresi öneklerini değiştirme

IP adresi öneklerini değiştirirken, izleyeceğiniz adımlar Yerel Ağ geçidinizde bir bağlantı olup olmadığına bağlıdır.

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Ağ geçidi IP adresini değiştirme

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Genel IP adresini değiştirdiğinizde, izleyeceğiniz adımlar yerel ağ geçidinize bir bağlantı olup olmadığına bağlıdır.

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ Geçidi bağlantınızı doğrulayabilirsiniz. Bkz. [ağ geçidi bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).
