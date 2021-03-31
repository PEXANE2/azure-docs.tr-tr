---
title: 'Öğretici-VPN Gateway oluşturma ve yönetme: Azure portal'
description: Portalı kullanarak Azure VPN Gateway oluşturma, dağıtma ve yönetme hakkında bilgi edinmek için bu öğreticiyi izleyin
author: cherylmc
ms.author: cherylmc
ms.service: vpn-gateway
ms.topic: tutorial
ms.date: 12/01/2020
ms.openlocfilehash: 820482a268af038737557c517ccda086cd65d943
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98880567"
---
# <a name="tutorial-create-and-manage-a-vpn-gateway-using-azure-portal"></a>Öğretici: Azure portal kullanarak bir VPN ağ geçidi oluşturma ve yönetme

Azure VPN ağ geçitleri, müşterinin iş yeri ile Azure arasında konumlar arası bağlantı sağlar. Bu öğretici, bir VPN ağ geçidi oluşturma ve yönetme gibi temel Azure VPN ağ geçidi dağıtım öğelerini kapsar. [Azure CLI](create-routebased-vpn-gateway-cli.md) veya [Azure PowerShell](create-routebased-vpn-gateway-powershell.md)kullanarak da ağ geçidi oluşturabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Sanal ağ oluşturma
> * VPN ağ geçidi oluşturma
> * Ağ Geçidi genel IP adresini görüntüleme
> * VPN ağ geçidini yeniden boyutlandırma (SKU 'YU yeniden boyutlandır)
> * Bir VPN ağ geçidini sıfırlama

Aşağıdaki diyagramda, bu öğreticinin bir parçası olarak oluşturulan sanal ağ ve VPN ağ geçidi gösterilmiştir.

:::image type="content" source="./media/tutorial-create-gateway-portal/gateway-diagram.png" alt-text="VNet ve VPN Gateway diyagramı":::

## <a name="prerequisites"></a>Önkoşullar

Etkin aboneliği olan bir Azure hesabı. Yoksa, [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="create-a-virtual-network"></a><a name="CreatVNet"></a>Sanal ağ oluşturma

Aşağıdaki değerleri kullanarak bir VNet oluşturun:

* **Kaynak grubu:** TestRG1
* **Ad:** VNet1
* **Bölge:** (US) Doğu ABD
* **IPv4 adres alanı:** 10.1.0.0/16
* **Alt ağ adı:** Uçta
* **Alt ağ adres alanı:** 10.1.0.0/24

[!INCLUDE [Create a virtual network](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="create-a-vpn-gateway"></a><a name="VNetGateway"></a>VPN ağ geçidi oluşturma

Bu adımda sanal ağınız için sanal ağ geçidi oluşturacaksınız. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

Aşağıdaki değerleri kullanarak bir sanal ağ geçidi oluşturun:

* **Ad:** VNet1GW
* **Bölge:** Doğu ABD
* **Ağ geçidi türü:** VPN
* **VPN türü:** Rota tabanlı
* **SKU:** VpnGw1
* **Oluşturma:** Generation1
* **Sanal ağ:** VNet1
* **Ağ geçidi alt ağ adres aralığı:** 10.1.255.0/27
* **Genel IP adresi:** Yeni oluştur
* **Genel IP adresi adı:** VNet1GWpip
* **Etkin-etkin modunu etkinleştir:** Devre dışı
* **BGP 'Yi yapılandırma:** Devre dışı

[!INCLUDE [Create a vpn gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

[!INCLUDE [NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="view-the-public-ip-address"></a><a name="view"></a>Genel IP adresini görüntüle

Ağ Geçidi genel IP adresini ağ geçidinizin **genel bakış** sayfasında görüntüleyebilirsiniz.

:::image type="content" source="./media/tutorial-create-gateway-portal/address.png" alt-text="Genel bakış sayfası":::

Genel IP adresi nesnesiyle ilgili ek bilgileri görmek için **genel IP adresi**' nin yanındaki ad/IP adresi bağlantısına tıklayın.

## <a name="resize-a-gateway-sku"></a><a name="resize"></a>Ağ Geçidi SKU 'sunu yeniden boyutlandırma

Yeniden boyutlandırma ile ilgili belirli kurallar vardır. bir ağ geçidi SKU 'SU değiştiriliyor. Bu bölümde, SKU 'YU yeniden boyutlandıracağız. Daha fazla bilgi için bkz. [Ağ Geçidi ayarları-SKU 'ları yeniden boyutlandırma ve değiştirme](vpn-gateway-about-vpn-gateway-settings.md#resizechange).

[!INCLUDE [resize a gateway](../../includes/vpn-gateway-resize-gw-portal-include.md)]

## <a name="reset-a-gateway"></a><a name="reset"></a>Ağ geçidini sıfırlama

[!INCLUDE [reset a gateway](../../includes/vpn-gateway-reset-gw-portal-include.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam edemeyecekti veya sonraki öğreticiye gitmek için aşağıdaki adımları kullanarak bu kaynakları silin:

1. Portalın üst kısmındaki **arama** kutusuna kaynak grubunuzun adını girip arama sonuçlarından seçin.

1. **Kaynak grubunu sil**'i seçin.

1. Kaynak **grubunun adını yazıp** **Sil**' i seçerek kaynak grubunuzu girin.

## <a name="next-steps"></a>Sonraki adımlar

VPN ağ geçidiniz olduktan sonra bağlantıları yapılandırabilirsiniz. Aşağıdaki makaleler en yaygın yapılandırmaların çoğunu oluşturmanıza yardımcı olur:

> [!div class="nextstepaction"]
> [Siteden siteye VPN bağlantıları](./tutorial-site-to-site-portal.md)

> [!div class="nextstepaction"]
> [Noktadan siteye VPN bağlantıları](vpn-gateway-howto-point-to-site-resource-manager-portal.md)