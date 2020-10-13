---
title: 'VPN Gateway: ağ geçidi IP adresi ayarlarını değiştir: Azure portal'
description: Bu makalede, Azure portal kullanarak yerel ağ geçidinizin IP adresi öneklerini değiştirme işlemi adım adım açıklanmaktadır.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/16/2020
ms.author: cherylmc
ms.openlocfilehash: af3513c4a4f3b3187e85c65de51ad2e6e2d7279c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90983194"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Azure portalını kullanarak yerel ağ geçidi ayarlarını değiştirme

Bazen yerel ağ geçidi Adresispredüzeltmesini veya Gatewayıpaddress değişikliği ayarları değişir. Bu makalede yerel ağ geçidi ayarlarınızı nasıl değiştireceğiniz gösterilir. Ayrıca, aşağıdaki listeden farklı bir seçenek belirleyerek bu ayarları farklı bir yöntem kullanarak da değiştirebilirsiniz:

Bağlantıyı silmeden önce, tanımlanan PSK 'yi almak için bağlanan cihazlarınız için yapılandırmayı indirmek isteyebilirsiniz. Bu şekilde, diğer tarafta yeniden tanımlamanız gerekmez.

> [!div class="op_single_selector"]
> * [Azure portalındaki](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="local-network-gateway-configuration"></a><a name="configure-lng"></a>Yerel ağ geçidi yapılandırması

Aşağıdaki ekran görüntüsünde, genel IP adresi uç noktası kullanılarak yerel ağ geçidi kaynağının **yapılandırma** sayfası gösterilmektedir:

:::image type="content" source="./media/vpn-gateway-modify-local-network-gateway-portal/ip-address.png" alt-text="Yerel ağ geçidi yapılandırma-IP adresi":::

Bu, bir FQDN uç noktası ile aynı yapılandırma sayfasıdır:

:::image type="content" source="./media/vpn-gateway-modify-local-network-gateway-portal/fqdn.png" alt-text="Yerel ağ geçidi yapılandırma-IP adresi":::

## <a name="modify-the-gateway-ip-address"></a><a name="ip"></a>Ağ geçidi IP adresini değiştirme

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir.

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. **IP adresi** kutusunda, IP adresini değiştirin.
3. Ayarları kaydetmek için **Kaydet**’e tıklayın.

## <a name="modify-the-gateway-fqdn"></a><a name="fqdn"></a>Ağ Geçidi FQDN 'sini değiştirme

Bağlanmak istediğiniz VPN cihazı FQDN 'sini (tam etki alanı adı) değiştirmemişse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir.

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. **FQDN** kutusunda, etki alanı adını değiştirin.
3. Ayarları kaydetmek için **Kaydet**’e tıklayın.

> ! NOTUN FQDN bitiş noktası ve IP adresi uç noktası arasında yerel ağ geçidini değiştiremezsiniz. Bu yerel ağ geçidi ile ilişkili tüm bağlantıları silmeniz, yeni uç nokta (IP adresi veya FQDN) ile yeni bir tane oluşturmanız ve ardından bağlantıları yeniden oluşturmanız gerekir.

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>IP adresi öneklerini değiştirme

### <a name="to-add-additional-address-prefixes"></a>Başka adres ön ekleri eklemek için:

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. IP adresi alanını *ek adres aralığı Ekle* kutusuna ekleyin.
3. Ayarlarınızı kaydetmek için **Kaydet** ' e tıklayın.

### <a name="to-remove-address-prefixes"></a>Adres ön eklerini kaldırmak için:

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. Kaldırmak istediğiniz öneki içeren satırdaki **'... '** düğmesine tıklayın.
3. **Kaldır**’a tıklayın.
4. Ayarlarınızı kaydetmek için **Kaydet** ' e tıklayın.

## <a name="modify-bgp-settings"></a><a name="bgp"></a>BGP ayarlarını değiştir

### <a name="to-add-or-update-bgp-settings"></a>BGP ayarlarını eklemek veya güncelleştirmek için:

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. Bu yerel ağ geçidi için BGP yapılandırmalarının görüntülenmesini veya güncelleştirilmesini sağlamak üzere **"BGP ayarlarını yapılandır"** ı seçin
3. Karşılık gelen alanlara otonom sistem numarasını veya BGP eş IP adresini ekleme veya güncelleştirme
4. Ayarlarınızı kaydetmek için **Kaydet** ' e tıklayın.

### <a name="to-remove-bgp-settings"></a>BGP ayarlarını kaldırmak için:

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. Mevcut BGP ASN ve BGP eşi IP adresini kaldırmak için **"BGP ayarlarını yapılandır"** seçimini kaldırın
3. Ayarlarınızı kaydetmek için **Kaydet** ' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Ağ Geçidi bağlantınızı doğrulayabilirsiniz. Bkz. [ağ geçidi bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).
