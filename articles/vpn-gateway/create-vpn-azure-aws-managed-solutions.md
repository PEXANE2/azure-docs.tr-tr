---
title: Yönetilen çözümleri kullanarak Azure ile AWS arasında bir VPN oluşturma
description: VM 'Ler veya gereçler yerine, Yönetilen çözümleri kullanarak Azure ile AWS arasında bir VPN bağlantısı oluşturma.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: ricmmartins
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 01/22/2021
ms.author: ricmart
ms.openlocfilehash: a0655ce1d2e9939981bb4fd3280af80e359ea1e1
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98737753"
---
# <a name="create-a-vpn-connection-between-azure-and-aws-using-managed-solutions"></a>Yönetilen çözümleri kullanarak Azure ile AWS arasında bir VPN bağlantısı oluşturma

Yönetilen çözümleri kullanarak Azure ile AWS arasında bir bağlantı kurabilirsiniz. Daha önce, Yanıtlayıcı görevi gören bir gereç veya VM kullanmanız gerekiyordu. Artık AWS sanal özel ağ geçidini, sanal makineler gibi IaaS kaynaklarını yönetme konusunda endişelenmenize gerek kalmadan doğrudan Azure VPN Gateway bağlayabilirsiniz. Bu makale, yalnızca yönetilen çözümleri kullanarak Azure ile AWS arasında bir VPN bağlantısı oluşturmanıza yardımcı olur.

:::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/diagram.png" alt-text="Mimari diyagramı":::

## <a name="configure-azure"></a>Azure 'ı yapılandırma

### <a name="configure-a-virtual-network"></a>Sanal ağ yapılandırma

Bir sanal ağ yapılandırın. Yönergeler için bkz. [sanal ağ hızlı](../virtual-network/quick-create-portal.md)başlangıcı.

Bu makalede aşağıdaki örnek değerler kullanılır:

* **Kaynak grubu:** RG-Azure-AWS
* **Bölge:** Doğu ABD
* **Sanal ağ adı:** VNET-Azure
* **IPv4 adres alanı:** 172.10.0.0/16
* **Alt ağ adı:** alt ağ-01
* **Alt ağ adres aralığı:** 172.10.1.0/24

### <a name="create-a-vpn-gateway"></a>VPN ağ geçidi oluşturma

Sanal ağınız için bir VPN Ağ Geçidi oluşturun. Yönergeler için bkz. [öğretici: BIR VPN ağ geçidi oluşturma ve yönetme](tutorial-create-gateway-portal.md).

Bu makalede aşağıdaki örnek değerler ve ayarlar kullanılır:

* **Ağ geçidi adı:** VPN-Azure-AWS
* **Bölge:** Doğu ABD
* **Ağ geçidi türü:** VPN
* **VPN türü:** Rota tabanlı
* **SKU:** VpnGw1
* **Oluşturma:** 1. nesil
* **Sanal ağ:** İçin ağ geçidini oluşturmak istediğiniz VNet olmalıdır.
* **Ağ geçidi alt ağ adres aralığı:** 172.10.0.0/27
* **Genel IP adresi:** Yeni oluştur
* **Genel IP adresi adı: PIP** -VPN-Azure-AWS
* **Etkin-etkin modunu etkinleştir:** Dıı
* **BGP 'Yi yapılandırma:** Dıı

Örnek:

:::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/summary.png" alt-text="Sanal ağ geçidi Özeti":::

## <a name="configure-aws"></a>AWS 'yi yapılandırma

1. Sanal özel bulutu (VPC) oluşturun.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-vpc.png" alt-text="VPC bilgisi oluştur":::

1. VPC (sanal ağ) içinde bir alt ağ oluşturun.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-subnet-vpc.png" alt-text="Alt ağ oluşturma":::

1. Azure VPN Gateway genel IP adresini işaret eden bir müşteri Ağ Geçidi oluşturun. **Müşteri ağ geçidi** , bu durumda Azure VPN Gateway olan müşteri ağ geçidi cihazı hakkında AWS hakkında bilgi içeren bir AWS kaynağıdır.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-customer-gw.png" alt-text="Müşteri ağ geçidi oluştur":::

1. Sanal özel ağ geçidini oluşturun.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-vpg.png" alt-text="Sanal özel ağ geçidi oluştur":::

1. Sanal özel ağ geçidini VPC 'ye bağlayın.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attach-vpg.png" alt-text="VPC 'ye VPG iliştirme":::

1. VPC 'yi seçin.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attaching-vpg.png" alt-text="İliştir":::

1. Siteden siteye VPN bağlantısı oluşturun.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-vpn-connection.png" alt-text="VPN bağlantısı oluştur":::

1. Yönlendirme seçeneğini **statik** olarak ayarlayın ve Azure alt ağı-01 önekini **(172.10.1.0/24)** işaret edin.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/set-static-route.png" alt-text="Statik yol ayarlama":::

1. Seçenekleri doldurduktan sonra bağlantı **oluşturun** .

1. Yapılandırma dosyasını indirin. Doğru yapılandırmayı indirmek için, Azure geçerli bir seçenek olmadığından satıcıyı, platformu ve yazılımı **genel**' e değiştirin.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/download-config.png" alt-text="Yapılandırmayı indir":::

1. Yapılandırma dosyası AWS tarafından oluşturulan iki ıpsec tünelinin her biri için önceden paylaşılan anahtarı ve genel IP adresini içerir.

   **Tünel 1**

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-1.png" alt-text="Tünel 1":::

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-1-config.png" alt-text="Tünel 1 yapılandırması":::

   **Tünel 2**

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-2.png" alt-text="Tünel 2":::

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-2-config.png" alt-text="Tünel 2 yapılandırması":::

1. Tüneller oluşturulduktan sonra, bu örneğe benzer bir durum görürsünüz.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-connection-details.png" alt-text="AWS VPN bağlantısı ayrıntıları":::

## <a name="create-local-network-gateway"></a>Yerel ağ geçidi oluştur

Azure 'da yerel ağ geçidi, genellikle şirket içi bir konumu temsil eden bir Azure kaynağıdır. Şirket içi VPN cihazına bağlanmak için kullanılan bilgilerle doldurulur. Bununla birlikte, bu yapılandırmada yerel ağ geçidi oluşturulur ve AWS sanal özel ağ geçidi bağlantı bilgileriyle doldurulur. Azure yerel ağ geçitleri hakkında daha fazla bilgi için bkz. [azure VPN Gateway ayarları](vpn-gateway-about-vpn-gateway-settings.md#lng).

Azure 'da yerel ağ geçidi oluşturun. Adımlar için bkz. [yerel ağ geçidi oluşturma](tutorial-site-to-site-portal.md#LocalNetworkGateway).

Aşağıdaki değerleri belirtin:

* **Ad:** Örnekte, LNG-Azure-AWS kullanırız.
* **Uç nokta:** IP adresi
* **IP adresi:** AWS sanal özel ağ geçidi ve VPC CıDR ön ekinin genel IP adresi. Genel IP adresini, daha önce indirdiğiniz yapılandırma dosyasında bulabilirsiniz.

AWS, yüksek kullanılabilirlik amaçlarıyla iki IPSec tüneli oluşturur. Aşağıdaki örnek, IPSec tüneli #1 genel IP adresini gösterir.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/local-network-gateway.png" alt-text="Yerel ağ geçidi":::

## <a name="create-vpn-connection"></a>VPN bağlantısı oluştur

Bu bölümde, Azure sanal ağ geçidi ve AWS ağ geçidi arasında VPN bağlantısı oluşturursunuz.

1. Azure bağlantısı oluşturun. Bağlantı oluşturma adımları için bkz. [VPN bağlantısı oluşturma](tutorial-site-to-site-portal.md#CreateConnection).

   Aşağıdaki örnekte, paylaşılan anahtar daha önce indirdiğiniz yapılandırma dosyasından alındı. Bu örnekte, AWS tarafından oluşturulan ve yapılandırma dosyasında açıklanan IPSec tünel #1 değerlerini kullanırız.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-connection.png" alt-text="Azure bağlantı nesnesi":::

1. Bağlantıyı görüntüleyin. Birkaç dakika sonra bağlantı oluşturulur.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/connection-established.png" alt-text="Çalışma bağlantısı":::

1. AWS IPSec tüneli **#1 olduğunu doğrulayın.**

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-connection-established.png" alt-text="AWS tünelinin çalışır olduğunu doğrulayın":::

1. VPC ile ilişkili yol tablosunu düzenleyin.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/edit-aws-route.png" alt-text="Rotayı düzenleme":::

1. Azure alt ağına yol ekleyin. Bu yol, VPC ağ geçidiyle gezinecek.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/save-aws-route.png" alt-text="Rota yapılandırmasını kaydetme":::

## <a name="configure-second-connection"></a>İkinci bağlantıyı Yapılandır

Bu bölümde, yüksek kullanılabilirlik sağlamak için ikinci bir bağlantı oluşturursunuz.

1. AWS üzerinde IPSec tüneli #2 genel IP adresini işaret eden başka bir yerel ağ geçidi oluşturun. Bu, bekleme geçidindir.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-lng-standby.png" alt-text="Yerel ağ geçidini oluşturma":::

1. Azure 'dan AWS 'ye ikinci VPN bağlantısını oluşturun.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-connection-standby.png" alt-text="Bekleme yerel ağ geçidi bağlantısı oluşturma":::

1. Azure VPN ağ geçidi bağlantılarını görüntüleyin.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-tunnels.png" alt-text="Azure bağlantı durumu":::

1. AWS bağlantılarını görüntüleyin. Bu örnekte, bağlantıların artık Kuruladığına bakabilirsiniz.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-tunnels.png" alt-text="AWS bağlantı durumu":::

## <a name="to-test-connections"></a>Bağlantıları test etmek için

1. AWS 'de VPC 'ye bir **İnternet ağ geçidi** ekleyin. İnternet ağ geçidi, bir Amazon VPN ve Internet arasındaki mantıksal bir bağlantıdır. Bu kaynak, Internet üzerinden AWS genel IP 'sinden test sanal makinesi üzerinden bağlanmanızı sağlar. Bu kaynak, VPN bağlantısı için gerekli değildir. Yalnızca test etmek için kullanıyoruz.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-igw.png" alt-text="Internet ağ geçidini oluşturma":::

1. **VPC 'ye Ekle**' yi seçin.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attach-igw.png" alt-text="Internet ağ geçidini VPC 'ye iliştirme":::

1. Bir VPC seçin ve **İnternet ağ geçidi ekleyin**.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attach-igw-2.png" alt-text="Ağ geçidini iliştirme":::

1. İnternet ağ geçidi üzerinden **0.0.0.0/0** (Internet) bağlantılarına izin vermek için bir yol oluşturun.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/allow-internet-igw.png" alt-text="Ağ Geçidi aracılığıyla rotayı yapılandırma":::

1. Azure 'da yol otomatik olarak oluşturulur. Azure VM 'nin yolunu, **sanal makine > ağ > ağ arabirimi > etkili rotalar ' a** seçerek denetleyebilirsiniz. Bağlantı başına 2 yol, 1 rota görürsünüz.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-effective-routes.png" alt-text="Geçerli rotaları denetleyin":::

1. Bunu Azure 'da bir Linux VM 'den test edebilirsiniz. Sonuç, aşağıdaki örneğe benzer şekilde görünür.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-overview.png" alt-text="Linux VM 'den Azure 'a genel bakış":::

1. Bunu ayrıca AWS 'deki bir Linux VM 'sinden test edebilirsiniz. Sonuç, aşağıdaki örneğe benzer şekilde görünür.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-overview.png" alt-text="Linux VM 'den AWS 'ye Genel Bakış":::

1. Azure VM 'den bağlantıyı test edin.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-ping.png" alt-text="Azure 'dan ping testi":::

1. AWS VM 'sinden bağlantıyı test edin.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-ping.png" alt-text="AWS 'den ping testi":::

## <a name="next-steps"></a>Sonraki adımlar

Ikev2 için AWS desteği hakkında daha fazla bilgi için [AWS makalesine](https://aws.amazon.com/about-aws/whats-new/2019/02/aws-site-to-site-vpn-now-supports-ikev2/)bakın.
