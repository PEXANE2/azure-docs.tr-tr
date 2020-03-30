---
title: Azure VPN Ağ Geçidi'nde BGP'yi yapılandırın:CLI
description: Bu makale, Azure Kaynak Yöneticisi ve CLI'yi kullanarak BGP'yi Azure VPN ağ geçidiyle yapılandırmanız için size yol açar.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/25/2018
ms.author: yushwang
ms.openlocfilehash: 42a07ac00fd8a26918164f6547bf57c2b021d14c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75863623"
---
# <a name="how-to-configure-bgp-on-an-azure-vpn-gateway-by-using-cli"></a>CLI kullanarak Bir Azure VPN ağ geçidinde BGP yapılandırma

Bu makale, Azure Kaynak Yöneticisi dağıtım modelini ve Azure CLI'yi kullanarak BGP'yi tesisler arası Site-to-Site (S2S) VPN bağlantısı ve VNet'ten VNet'e (diğer bir şekilde sanal ağlar arasında bağlantı) etkinleştirmenize yardımcı olur.

## <a name="about-bgp"></a>BGP hakkında

BGP, iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini değiştirmek için internette yaygın olarak kullanılan standart yönlendirme protokolüdür. BGP, VPN ağ geçitlerini ve BGP eşleri veya komşuları olarak adlandırılan şirket içi VPN aygıtlarınızın rota değişimini sağlar. Rotalar, her iki ağ geçidini de öneklerin ağ geçitlerinden veya ilgili yönlendiricilerden geçmesi için kullanılabilirlik ve ulaşılabilirlik hakkında bilgilendirir. BGP ayrıca, bir BGP ağ geçidinin bir BGP eşlerinden öğrendiği yolları diğer tüm BGP eşlerine yayırak olarak birden çok ağ arasında geçiş yönlendirmesini de etkinleştirebilir.

BGP'nin yararları hakkında daha fazla bilgi ve BGP kullanmanın teknik gereksinimlerini ve dikkatlerini anlamak için [Azure VPN ağ geçitleriyle BGP'ye Genel Bakış](vpn-gateway-bgp-overview.md)bölümüne bakın.

Bu makale, aşağıdaki görevlerde size yardımcı olur:

* [VPN ağ geçidiniz için BGP'yi etkinleştirin](#enablebgp) (gereklidir)

  Daha sonra aşağıdaki bölümlerden birini veya her ikisini de tamamlayabilirsiniz:

* [BGP ile bir çapraz bağlantı kurun](#crossprembgp)
* [BGP ile VNet-vNet bağlantısı kurun](#v2vbgp)

Bu üç bölümün her biri, ağ bağlantınızda BGP'yi etkinleştirmek için temel bir yapı taşı oluşturur. Üç bölümü de tamamlarsanız, aşağıdaki diyagramda gösterildiği gibi topolojiyi oluşturursunuz:

![BGP topolojisi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

İhtiyaçlarınızı karşılayan daha karmaşık bir multihop geçiş ağı oluşturmak için bu bölümleri birleştirebilirsiniz.

## <a name="enable-bgp-for-your-vpn-gateway"></a><a name ="enablebgp"></a>VPN ağ geçidiniz için BGP'yi etkinleştirme

Bu bölüm, diğer iki yapılandırma bölümündeki adımlardan herhangi birini gerçekleştirmeden önce gereklidir. Aşağıdaki yapılandırma adımları, aşağıdaki diyagramda gösterildiği gibi Azure VPN ağ geçidinin BGP parametrelerini ayarlar:

![BGP ağ geçidi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Başlamadan önce

CLI komutlarının (2.0 veya sonraki) en son sürümünü yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli) ve [Azure CLI’yi Kullanmaya Başlama](/cli/azure/get-started-with-azure-cli).

### <a name="step-1-create-and-configure-testvnet1"></a>Adım 1: TestVNet1 oluşturma ve yapılandırma

#### <a name="1-connect-to-your-subscription"></a><a name="Login"></a>1. Aboneliğinize bağlanın

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

#### <a name="2-create-a-resource-group"></a>2. Kaynak grubu oluşturma

Aşağıdaki örnekte, "eastus" konumunda TestRG1 adlı bir kaynak grubu oluşturulur. Sanal ağınızı oluşturmak istediğiniz bölgede zaten bir kaynak grubunuz varsa, bunun yerine bunu kullanabilirsiniz.

```azurecli
az group create --name TestBGPRG1 --location eastus
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 oluştur

Aşağıdaki örnek, TestVNet1 adında bir sanal ağ ve üç alt ağ oluşturur: GatewaySubnet, FrontEnd ve BackEnd. Değerleri yerine alırken, ağ geçidi alt ağınıza her zaman özel GatewaySubnet adını verirken önemlidir. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

İlk komut, ön uç adres alanını ve FrontEnd alt netini oluşturur. İkinci komut, BackEnd alt ağı için ek bir adres alanı oluşturur. Üçüncü ve dördüncü komutlar BackEnd alt ağı ve GatewaySubnet'i oluşturur.

```azurecli
az network vnet create -n TestVNet1 -g TestBGPRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24 
 
az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestBGPRG1 
 
az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestBGPRG1 --address-prefix 10.12.0.0/24 
 
az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestBGPRG1 --address-prefix 10.12.255.0/27 
```

### <a name="step-2-create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Adım 2: BGP parametreleri ile TestVNet1 için VPN ağ geçidi oluşturma

#### <a name="1-create-the-public-ip-address"></a>1. Genel IP adresini oluşturma

Genel bir IP adresi isteyin. Genel IP adresi, sanal ağınız için oluşturduğunuz VPN ağ geçidine tahsis edilir.

```azurecli
az network public-ip create -n GWPubIP -g TestBGPRG1 --allocation-method Dynamic 
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. AS numarası ile VPN ağ geçidi oluşturma

TestVNet1 için sanal ağ geçidini oluşturun. BGP bir Rota Tabanlı VPN ağ geçidi gerektirir. Ayrıca TestVNet1 `-Asn` için özerk sistem numarasını (ASN) ayarlamak için ek parametre gerekir. Ağ geçidi oluşturmanın tamamlanması bir süre (45 dakika veya daha fazla) sürebilir. 

Bu komutu `--no-wait` parametreyi kullanarak çalıştırın, herhangi bir geri bildirim veya çıktı görmezsiniz. Parametre ağ `--no-wait` geçidinin arka planda oluşturulmasına izin verir. Bu VPN ağ geçidi hemen oluşturulur anlamına gelmez.

```azurecli
az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address GWPubIP -g TestBGPRG1 --vnet TestVNet1 --gateway-type Vpn --sku HighPerformance --vpn-type RouteBased --asn 65010 --no-wait
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Azure BGP eş IP adresini edinin

Ağ geçidi oluşturulduktan sonra, Azure VPN ağ geçidindeki BGP eş IP adresini almanız gerekir. Bu adres, şirket içi VPN aygıtlarınız için VPN ağ geçidini BGP eş olarak yapılandırmak için gereklidir.

Aşağıdaki komutu çalıştırın `bgpSettings` ve çıktının üst kısmındaki bölümü kontrol edin:

```azurecli
az network vnet-gateway list -g TestBGPRG1 
 
  
"bgpSettings": { 
      "asn": 65010, 
      "bgpPeeringAddress": "10.12.255.30", 
      "peerWeight": 0 
    }
```

Ağ geçidi oluşturulduktan sonra, bu ağ geçidini, bgp ile bir şirket arası bağlantı veya VNet-vNet bağlantısı kurmak için kullanabilirsiniz.

## <a name="establish-a-cross-premises-connection-with-bgp"></a><a name ="crossprembgp"></a>BGP ile bir çapraz bağlantı kurun

Bir şirket içi bağlantı kurmak için, şirket içi VPN aygıtınızı temsil edecek yerel bir ağ ağ geçidi oluşturmanız gerekir. Ardından Azure VPN ağ geçidini yerel ağ ağ geçidine bağlarsınız. Bu adımlar diğer bağlantılar oluşturmaya benzer olsa da, BGP yapılandırma parametrelerini belirtmek için gereken ek özellikleri içerir.

![Tesisler arası için BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)


### <a name="step-1-create-and-configure-the-local-network-gateway"></a>Adım 1: Yerel ağ ağ geçidini oluşturma ve yapılandırma

Bu alıştırma, diyagramda gösterilen yapılandırmayı oluşturmaya devam eder. Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun. Yerel ağ ağ geçitleriyle çalışırken aşağıdaki leri aklınızda bulundurun:

* Yerel ağ ağ geçidi VPN ağ geçidiyle aynı konum ve kaynak grubunda olabilir veya farklı bir konum ve kaynak grubunda olabilir. Bu örnek, farklı konumlardaki farklı kaynak gruplarındaki ağ geçitlerini gösterir.
* Yerel ağ ağ geçidi için bildirmeniz gereken minimum önek, VPN aygıtınızdaki BGP eş IP adresinizin ana bilgisayarıadresidir. Bu durumda, 10.51.255.254/32 bir /32 öneki's.
* Anımsatıcı olarak, şirket içi ağlarınız ve Azure sanal ağı nız arasında farklı BGP ASN'ler kullanmanız gerekir. Bunlar aynıysa, şirket içi VPN aygıtlarınız diğer BGP komşularıyla eşlemek için ASN'yi zaten kullanıyorsa VNet ASN'nizi değiştirmeniz gerekir.

Devam etmeden önce, bu alıştırmanın [VPN ağ geçidi bölümünüz için BGP'yi etkinleştir'i](#enablebgp) tamamladığınızdan ve hala Abonelik 1'e bağlı olduğunuzdan emin olun. Bu örnekte, yeni bir kaynak grubu oluşturduğunuza dikkat edin. Ayrıca, yerel ağ ağ geçidi için `Asn` iki `BgpPeerAddress`ek parametre ye dikkat edin: ve.

```azurecli
az group create -n TestBGPRG5 -l eastus2 
 
az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site5 -g TestBGPRG5 --local-address-prefixes 10.51.255.254/32 --asn 65050 --bgp-peering-address 10.51.255.254
```

### <a name="step-2-connect-the-vnet-gateway-and-local-network-gateway"></a>Adım 2: VNet ağ geçidini ve yerel ağ ağ geçidini bağlayın

Bu adımda, TestVNet1'den Site5'e bağlantı oluşturursunuz. Bu bağlantı `--enable-bgp` için BGP'yi etkinleştirmek için parametreyi belirtmeniz gerekir. 

Bu örnekte, sanal ağ ağ ağ geçidi ve yerel ağ ağ geçidi farklı kaynak gruplarındadır. Ağ geçitleri farklı kaynak gruplarında olduğunda, sanal ağlar arasında bağlantı kurmak için iki ağ geçidinin tüm kaynak kimliğini belirtmeniz gerekir.

#### <a name="1-get-the-resource-id-of-vnet1gw"></a>1. VNet1GW kaynak kimliğini alın

VNet1GW kaynak kimliğini almak için aşağıdaki komuttan çıktı kullanın:

```azurecli
az network vnet-gateway show -n VNet1GW -g TestBGPRG1
```

`"id":` Çıktıda, satırı bulun. Bir sonraki bölümde bağlantı oluşturmak için tırnak işaretleri içindeki değerlere ihtiyacınız var.

Örnek çıktı:

```
{ 
  "activeActive": false, 
  "bgpSettings": { 
    "asn": 65010, 
    "bgpPeeringAddress": "10.12.255.30", 
    "peerWeight": 0 
  }, 
  "enableBgp": true, 
  "etag": "W/\"<your etag number>\"", 
  "gatewayDefaultSite": null, 
  "gatewayType": "Vpn", 
  "id": "/subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW",
```

Bağlantıyı oluştururken `"id":` kolayca yapıştırabilmeniz için değerleri Not Defteri gibi bir metin düzenleyicisine kopyalayın. 

```
"id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
```

#### <a name="2-get-the-resource-id-of-site5"></a>2. Site5'in kaynak kimliğini alın

Site5'in kaynak kimliğini çıktıdan almak için aşağıdaki komutu kullanın:

```azurecli
az network local-gateway show -n Site5 -g TestBGPRG5
```

#### <a name="3-create-the-testvnet1-to-site5-connection"></a>3. TestVNet1-Site5 bağlantısını oluşturun

Bu adımda, TestVNet1'den Site5'e bağlantı oluşturursunuz. Daha önce de belirtildiği gibi, aynı Azure VPN ağ geçidi için hem BGP hem de BGP olmayan bağlantılara sahip olmak mümkündür. Bağlantı özelliğinde BGP etkinleştirilmediği sürece, BGP parametreleri her iki ağ geçidinde de yapılandırılmış olsa da Azure bu bağlantı için BGP'yi etkinleştirmez. Abonelik tünalarını kendi nizle değiştirin.

```azurecli
az network vpn-connection create -n VNet1ToSite5 -g TestBGPRG1 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW --enable-bgp -l eastus --shared-key "abc123" --local-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG5/providers/Microsoft.Network/localNetworkGateways/Site5 --no-wait
```

Bu alıştırma için aşağıdaki örnek, şirket içi VPN aygıtınızın BGP yapılandırma bölümüne girilen parametreleri listeler:

```
Site5 ASN            : 65050
Site5 BGP IP         : 10.52.255.254
Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
Azure VNet ASN       : 65010
Azure VNet BGP IP    : 10.12.255.30
Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Bağlantı birkaç dakika içerisinde kurulacaktır. BGP eşleme oturumu, IPsec bağlantısı kurulduktan sonra başlar.

## <a name="establish-a-vnet-to-vnet-connection-with-bgp"></a><a name ="v2vbgp"></a>BGP ile VNet-vNet bağlantısı kurun

Bu bölümde, aşağıdaki diyagramda gösterildiği gibi BGP ile VNet-to-VNet bağlantısı ekler: 

![VNet-to-VNet için BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Aşağıdaki yönergeler önceki bölümlerdeki adımlardan devam edin. TestVNet1 ve BGP ile VPN ağ geçidi oluşturmak ve yapılandırmak [için VPN ağ geçidi bölümünüz için BGP'yi etkinleştir'i tamamlamanız](#enablebgp) gerekir.

### <a name="step-1-create-testvnet2-and-the-vpn-gateway"></a>Adım 1: TestVNet2 ve VPN ağ geçidi oluşturma

Yeni sanal ağ TestVNet2'nin IP adres alanının VNet aralıklarınızın hiçbiriyle örtüşmediğinden emin olmak önemlidir.

Bu örnekte, sanal ağlar aynı aboneye aittir. Farklı abonelikler arasında VNet-vNet bağlantıları ayarlayabilirsiniz. Daha fazla bilgi için [vnet-to-VNet bağlantısını yapılandırın'](vpn-gateway-howto-vnet-vnet-cli.md)a bakın. BGP'yi `-EnableBgp $True` etkinleştirmek için bağlantıları oluştururken eklediğinizden emin olun.

#### <a name="1-create-a-new-resource-group"></a>1. Yeni bir kaynak grubu oluşturma

```azurecli
az group create -n TestBGPRG2 -l westus
```

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Yeni kaynak grubunda TestVNet2 oluşturma

İlk komut, ön uç adres alanını ve FrontEnd alt netini oluşturur. İkinci komut, BackEnd alt ağı için ek bir adres alanı oluşturur. Üçüncü ve dördüncü komutlar BackEnd alt ağı ve GatewaySubnet'i oluşturur.

```azurecli
az network vnet create -n TestVNet2 -g TestBGPRG2 --address-prefix 10.21.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.21.0.0/24 
 
az network vnet update -n TestVNet2 --address-prefixes 10.21.0.0/16 10.22.0.0/16 -g TestBGPRG2 
 
az network vnet subnet create --vnet-name TestVNet2 -n BackEnd -g TestBGPRG2 --address-prefix 10.22.0.0/24 
 
az network vnet subnet create --vnet-name TestVNet2 -n GatewaySubnet -g TestBGPRG2 --address-prefix 10.22.255.0/27
```

#### <a name="3-create-the-public-ip-address"></a>3. Genel IP adresini oluşturma

Genel bir IP adresi isteyin. Genel IP adresi, sanal ağınız için oluşturduğunuz VPN ağ geçidine tahsis edilir.

```azurecli
az network public-ip create -n GWPubIP2 -g TestBGPRG2 --allocation-method Dynamic
```

#### <a name="4-create-the-vpn-gateway-with-the-as-number"></a>4. AS numarası ile VPN ağ geçidi oluşturun

TestVNet2 için sanal ağ ağ ağ geçidi oluşturun. Azure VPN ağ geçitlerinizde varsayılan ASN'yi geçersiz kılmanız gerekir. BGP ve transit yönlendirmeyi etkinleştirmek için bağlı sanal ağların ASN'leri farklı olmalıdır.
 
```azurecli
az network vnet-gateway create -n VNet2GW -l westus --public-ip-address GWPubIP2 -g TestBGPRG2 --vnet TestVNet2 --gateway-type Vpn --sku Standard --vpn-type RouteBased --asn 65020 --no-wait
```

### <a name="step-2-connect-the-testvnet1-and-testvnet2-gateways"></a>Adım 2: TestVNet1 ve TestVNet2 ağ geçitlerini bağlayın

Bu adımda, TestVNet1'den Site5'e bağlantı oluşturursunuz. Bu bağlantı için BGP'yi etkinleştirmek için parametreyi `--enable-bgp` belirtmeniz gerekir.

Aşağıdaki örnekte, sanal ağ ağ ağ geçidi ve yerel ağ ağ geçidi farklı kaynak gruplarındadır. Ağ geçitleri farklı kaynak gruplarında olduğunda, sanal ağlar arasında bağlantı kurmak için iki ağ geçidinin tüm kaynak kimliğini belirtmeniz gerekir. 

#### <a name="1-get-the-resource-id-of-vnet1gw"></a>1. VNet1GW kaynak kimliğini alın 

Aşağıdaki komutun çıktısından VNet1GW kaynak kimliğini alın:

```azurecli
az network vnet-gateway show -n VNet1GW -g TestBGPRG1
```

#### <a name="2-get-the-resource-id-of-vnet2gw"></a>2. VNet2GW kaynak kimliğini alın

Aşağıdaki komutun çıktısından VNet2GW'nin kaynak kimliğini alın:

```azurecli
az network vnet-gateway show -n VNet2GW -g TestBGPRG2
```

#### <a name="3-create-the-connections"></a>3. Bağlantıları oluşturma

TestVNet1'den TestVNet2'ye, TestVNet2'den TestVNet1'e bağlantı oluşturun. Abonelik tünalarını kendi nizle değiştirin.

```azurecli
az network vpn-connection create -n VNet1ToVNet2 -g TestBGPRG1 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW --enable-bgp -l eastus --shared-key "efg456" --vnet-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG2/providers/Microsoft.Network/virtualNetworkGateways/VNet2GW
```

```azurecli
az network vpn-connection create -n VNet2ToVNet1 -g TestBGPRG2 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG2/providers/Microsoft.Network/virtualNetworkGateways/VNet2GW --enable-bgp -l westus --shared-key "efg456" --vnet-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
```

> [!IMPORTANT]
> *Her iki* bağlantı için de BGP'yi etkinleştirin.
> 
> 

Bu adımları tamamladıktan sonra, bağlantı birkaç dakika içinde kurulacaktır. VNet-vNet bağlantısı tamamlandıktan sonra BGP eşleme oturumu açılır.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için [bkz.](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
