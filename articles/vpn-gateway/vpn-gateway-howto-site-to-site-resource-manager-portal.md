---
title: 'Şirket içi ağı Azure sanal ağına bağlama: siteden siteye VPN: Portal'
description: Şirket içi ağınızdan, portalı kullanarak genel İnternet üzerinden bir Azure sanal ağı arasında bir IPSec siteden siteye VPN Gateway bağlantısı oluşturun.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/22/2020
ms.author: cherylmc
ms.openlocfilehash: 069add9a7e5dd6b19810b8b36a4fca49818bfb02
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92479629"
---
# <a name="create-a-site-to-site-connection-in-the-azure-portal"></a>Azure portalında Siteden Siteye bağlantı oluşturma

Bu makalede, Azure portalını kullanarak şirket içi ağınızdan VNet’e Siteden Siteye VPN ağ geçidi bağlantısı oluşturma işlemi gösterilir. Bu makaledeki adımlar Resource Manager dağıtım modeli için geçerlidir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure portal (klasik)](vpn-gateway-howto-site-to-site-classic-portal.md)
>

Siteden Siteye VPN ağ geçidi bağlantısı, şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) tüneli üzerinden Azure sanal ağına bağlamak için kullanılır. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN ağ geçidi hakkında](vpn-gateway-about-vpngateways.md).

:::image type="content" source="./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png" alt-text="Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı diyagramı":::

## <a name="prerequisites"></a>Önkoşullar

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Etkin aboneliği olan bir Azure hesabı. Yoksa, [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek birinin bulunduğundan emin olun. Uyumlu VPN cihazları ve cihaz yapılandırması hakkında daha fazla bilgi için bkz.[VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md).
* VPN cihazınız için dışarıya dönük genel bir IPv4 adresi olduğunu doğrulayın.
* Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir. Bu yapılandırmayı oluşturduğunuzda, Azure’un şirket içi konumunuza yönlendireceği IP adres aralığı ön eklerini oluşturmanız gerekir. Şirket içi ağınızın alt ağlarından hiçbiri, bağlanmak istediğiniz sanal ağ alt ağlarıyla çakışamaz.

## <a name="create-a-virtual-network"></a><a name="CreatVNet"></a>Sanal ağ oluşturma

Aşağıdaki değerleri kullanarak bir sanal ağ (VNet) oluşturun:

* **Kaynak grubu:** TestRG1
* **Ad:** VNet1
* **Bölge:** (US) Doğu ABD
* **IPv4 adres alanı:** 10.1.0.0/16
* **Alt ağ adı:** Uçta
* **Alt ağ adres alanı:** 10.1.0.0/24

[!INCLUDE [About cross-premises addresses](../../includes/vpn-gateway-cross-premises.md)]

[!INCLUDE [Create a virtual network](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="create-a-vpn-gateway"></a><a name="VNetGateway"></a>VPN ağ geçidi oluşturma

Bu adımda sanal ağınız için sanal ağ geçidi oluşturacaksınız. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

### <a name="about-the-gateway-subnet"></a>Ağ geçidi alt ağı hakkında

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

### <a name="create-the-gateway"></a>Ağ geçidini oluşturma

Aşağıdaki değerleri kullanarak bir VPN Ağ Geçidi oluşturun:

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

## <a name="create-a-local-network-gateway"></a><a name="LocalNetworkGateway"></a>Yerel ağ geçidi oluşturma

Yerel ağ geçidi, yönlendirme amacıyla Şirket içi konumunuzu (site) temsil eden belirli bir nesnedir. Siteye Azure’un başvuruda bulunmak için kullanabileceği bir ad verir, ardından bağlantı oluşturacağınız şirket içi VPN cihazının IP adresini belirtirsiniz. Ayrıca, VPN ağ geçidi üzerinden VPN cihazına yönlendirilecek IP adresi ön eklerini de belirtirsiniz. Belirttiğiniz adres ön ekleri, şirket içi adresinizde yer alan ön eklerdir. Şirket içi ağınız değişirse veya VPN cihazının genel IP adresini değiştirmeniz gerekirse değerleri daha sonra kolayca güncelleştirebilirsiniz.

Aşağıdaki değerleri kullanarak bir yerel ağ geçidi oluşturun:

* **Ad:** Site1
* **Kaynak Grubu:** TestRG1
* **Konum:** Doğu ABD

[!INCLUDE [Add a local network gateway](../../includes/vpn-gateway-add-local-network-gateway-portal-include.md)]

## <a name="configure-your-vpn-device"></a><a name="VPNDevice"></a>VPN cihazınızı yapılandırma

Bir şirket içi ağı ile Siteden Siteye bağlantılar için VPN cihazı gerekir. Bu adımda VPN cihazınızı yapılandıracaksınız. VPN cihazınızı yapılandırırken aşağıdaki değerlere sahip olmanız gerekir:

* Paylaşılan bir anahtar. Siteden Siteye VPN bağlantınızı oluştururken belirttiğiniz paylaşılan anahtarın aynısıdır. Bu örneklerde temel bir paylaşılan anahtar kullanılır. Kullanmak için daha karmaşık bir anahtar oluşturmanız önerilir.
* Sanal ağ geçidinizin Genel IP adresi. Azure Portal, PowerShell veya CLI kullanarak genel IP adresini görüntüleyebilirsiniz. Azure portal kullanarak VPN ağ geçidinizin genel IP adresini bulmak için **sanal ağ geçitleri**' ne gidin ve ağ geçidinizin adını seçin.

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-include.md)]

## <a name="create-a-vpn-connection"></a><a name="CreateConnection"></a>VPN bağlantısı oluşturma

Sanal ağ geçidiniz ile şirket içi VPN cihazınız arasında Siteden Siteye VPN bağlantısı oluşturun.

Aşağıdaki değerleri kullanarak bir bağlantı oluşturun:

* **Yerel ağ geçidi adı:** Site1
* **Bağlantı adı:** VNet1toSite1
* **Ortak anahtar:** Bu örnekte abc123 kullanılır. Ancak, VPN donanımınızla uyumlu herhangi bir seçeneği kullanabilirsiniz. Önemli olan, değerin bağlantının her iki tarafında eşleşmesidir.

[!INCLUDE [Add a site-to-site connection](../../includes/vpn-gateway-add-site-to-site-connection-portal-include.md)]

## <a name="verify-the-vpn-connection"></a><a name="VerifyConnection"></a>VPN bağlantısını doğrulama

[!INCLUDE [Verify the connection](../../includes/vpn-gateway-verify-connection-portal-include.md)]

## <a name="how-to-connect-to-a-virtual-machine"></a><a name="connectVM"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="how-to-reset-a-vpn-gateway"></a><a name="reset"></a>VPN ağ geçidini sıfırlama

Bir veya daha fazla Siteden Siteye VPN tünelinde şirketler arası VPN bağlantısını kaybederseniz bir Azure VPN ağ geçidinin sıfırlanması yararlıdır. Bu durumda şirket içi VPN cihazlarınızın tümü düzgün çalışır, ancak Azure VPN ağ geçitleriyle IPsec tünelleri kuramaz. Adımlar için bkz. [VPN ağ geçidini sıfırlama](reset-gateway.md).

## <a name="how-to-change-a-gateway-sku-resize-a-gateway"></a><a name="resize"></a>Ağ geçidi SKU’sunu değiştirme (ağ geçidini yeniden boyutlandırma)

Bir ağ geçidi SKU'sunu değiştirme adımları için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

## <a name="how-to-add-an-additional-connection-to-a-vpn-gateway"></a><a name="addconnect"></a>Bir VPN ağ geçidine başka bağlantı ekleme

Hiçbir adres alanının bağlantılar arasında çakışmaması şartıyla başka bağlantılar ekleyebilirsiniz.

1. Ek bir bağlantı eklemek için VPN ağ geçidine gidin ve bağlantılar sayfasını açmak için **Bağlantılar** ' ı seçin.
1. Bağlantınızı eklemek için **+ Ekle** ' yi seçin. Bağlantı türünü VNet-VNet (başka bir VNet ağ geçidine bağlanılıyorsa) veya Site-site ayarını yansıtacak şekilde ayarlayın.
1. Site-site ile bağlantı kuruyorsanız ve bağlanmak istediğiniz site için henüz bir yerel ağ geçidi oluşturmadıysanız yeni bir tane oluşturabilirsiniz.
1. Kullanmak istediğiniz paylaşılan anahtarı belirtin, ardından bağlantıyı oluşturmak için **Tamam** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* BGP hakkında bilgi edinmek için [BGP’ye Genel Bakış](vpn-gateway-bgp-overview.md) ve [BGP’yi yapılandırma](vpn-gateway-bgp-resource-manager-ps.md) makalelerine bakın.
* Zorlamalı tünel oluşturma hakkında bilgi için bkz. [Zorlamalı tünel oluşturma hakkında](vpn-gateway-forced-tunneling-rm.md).
* Yüksek Oranda Kullanılabilir Etkin-Etkin bağlantılar hakkında bilgi için bkz. [Yüksek Oranda Kullanılabilir Şirket İçi ve Dışı ile Sanal Ağdan Sanal Ağa Bağlantı](vpn-gateway-highlyavailable.md).
* Ağ trafiğini bir sanal ağ içindeki kaynaklarla sınırlama hakkında bilgi için bkz. [Ağ Güvenliği](../virtual-network/security-overview.md).
* Azure, şirket içi ve İnternet kaynakları arasındaki trafiğin Azure tarafından nasıl yönlendirdiği hakkında bilgi için bkz. [Sanal ağ trafiği yönlendirme](../virtual-network/virtual-networks-udr-overview.md).
* Azure Resource Manager şablonu kullanarak siteden siteye VPN bağlantısı oluşturma hakkında bilgi için bkz. [siteden sıteye VPN bağlantısı oluşturma](https://azure.microsoft.com/resources/templates/101-site-to-site-vpn-create/).
* Azure Resource Manager şablonu kullanarak VNet-VNet VPN bağlantısı oluşturma hakkında bilgi için bkz. [HBase Coğrafi çoğaltmayı dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-geo/).
