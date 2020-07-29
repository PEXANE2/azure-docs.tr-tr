---
title: 'Şirket içi ağları bir sanal ağa bağlama: siteden siteye VPN: CLı'
description: Şirket içi ağınız ile bir Azure sanal ağı arasında genel İnternet üzerinden bir IPSec bağlantısı oluşturma adımları. Bu adımlar CLI kullanarak Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı oluşturmanıza yardımcı olur.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/18/2018
ms.author: cherylmc
ms.openlocfilehash: 28d0fa3a0fdc2f8ac65fc3545abab0a273051e37
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84987508"
---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>CLI kullanarak Siteden Siteye VPN bağlantısı olan bir sanal ağ oluşturma

Bu makalede, Azure CLI kullanarak şirket içi ağınızdan VNet’e Siteden Siteye VPN ağ geçidi bağlantısı oluşturma işlemi gösterilir. Bu makaledeki adımlar Resource Manager dağıtım modeli için geçerlidir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:<br>

> [!div class="op_single_selector"]
> * [Azure portalındaki](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure portal (klasik)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>


![Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı diyagramı](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-diagram.png)

Siteden Siteye VPN ağ geçidi bağlantısı, şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) tüneli üzerinden Azure sanal ağına bağlamak için kullanılır. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN ağ geçidi hakkında](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek birinin bulunduğundan emin olun. Uyumlu VPN cihazları ve cihaz yapılandırması hakkında daha fazla bilgi için bkz.[VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md).
* VPN cihazınız için dışarıya dönük genel bir IPv4 adresi olduğunu doğrulayın.
* Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir. Bu yapılandırmayı oluşturduğunuzda, Azure’un şirket içi konumunuza yönlendireceği IP adres aralığı ön eklerini oluşturmanız gerekir. Şirket içi ağınızın alt ağlarından hiçbiri, bağlanmak istediğiniz sanal ağ alt ağlarıyla çakışamaz.
* Azure Cloud Shell, CLı komutlarınızı çalıştırmak için kullanabilirsiniz (aşağıdaki yönergeler). Ancak, komutlarınızı yerel olarak çalıştırmayı tercih ediyorsanız, CLı komutlarının en son sürümünü (2,0 veya üzeri) yüklediğinizden emin olun. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli) ve [Azure CLI’yi Kullanmaya Başlama](/cli/azure/get-started-with-azure-cli). 
 
  [!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="example-values"></a><a name="example"></a>Örnek değerler

Aşağıdaki değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakabilirsiniz:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.0.0/24 
GatewaySubnet           = 10.11.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="1-connect-to-your-subscription"></a><a name="Login"></a>1. aboneliğinize bağlanın

CLı 'yi yerel olarak çalıştırmayı seçerseniz aboneliğinize bağlanın. Tarayıcıda Azure Cloud Shell kullanıyorsanız, aboneliğinize bağlanmanız gerekmez. Azure Cloud Shell ' de otomatik olarak bağlanacaksınız. Ancak, bağlandıktan sonra doğru aboneliği kullandığınızı doğrulamak isteyebilirsiniz.

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="2-create-a-resource-group"></a><a name="rg"></a>2. kaynak grubu oluşturma

Aşağıdaki örnekte, 'eastus' konumunda 'TestRG1' adlı bir kaynak grubu oluşturulur. VNet’inizi oluşturmak istediğiniz bölgede zaten bir kaynak grubunuz varsa, bunun yerine onu da kullanabilirsiniz.

```azurecli-interactive
az group create --name TestRG1 --location eastus
```

## <a name="3-create-a-virtual-network"></a><a name="VNet"></a>3. sanal ağ oluşturma

Henüz bir sanal ağınız yoksa, [az network vnet create](/cli/azure/network/vnet) komutunu kullanarak bir tane oluşturun. Sanal ağ oluştururken, belirlediğiniz adres alanlarının şirket içi ağınızdaki adres alanlarından herhangi biriyle çakışmadığından emin olun.

>[!NOTE]
>Bu VNet’in bir şirket içi konuma bağlanması için şirket içi ağ yöneticinizle bu ağ için özellikle kullanabileceğiniz bir IP adresi aralığı ayırma işlemini koordine etmeniz gerekir. VPN bağlantısının her iki tarafında bir yinelenen adres aralığı varsa, trafik beklediğiniz şekilde yönlendirilmez. Ayrıca, bu sanal ağı başka bir sanal ağa bağlamak isterseniz adres alanı diğer sanal ağ ile örtüşemez. Ağ yapılandırmanızı uygun şekilde planlamaya dikkat edin.
>
>

Aşağıdaki örnekte, 'TestVNet1' adlı bir sanal ağ ve 'Subnet1' adlı alt ağ oluşturulur.

```azurecli-interactive
az network vnet create --name TestVNet1 --resource-group TestRG1 --address-prefix 10.11.0.0/16 --location eastus --subnet-name Subnet1 --subnet-prefix 10.11.0.0/24
```

## <a name="4-create-the-gateway-subnet"></a>4. <a name="gwsub"></a> ağ geçidi alt ağını oluşturma


[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

Ağ geçidi alt ağını oluşturmak için [az network vnet subnet create](/cli/azure/network/vnet/subnet) komutunu kullanın.

```azurecli-interactive
az network vnet subnet create --address-prefix 10.11.255.0/27 --name GatewaySubnet --resource-group TestRG1 --vnet-name TestVNet1
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="5-create-the-local-network-gateway"></a><a name="localnet"></a>5. yerel ağ geçidini oluşturma

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Siteye Azure’un başvuruda bulunmak için kullanabileceği bir ad verir, ardından bağlantı oluşturacağınız şirket içi VPN cihazının IP adresini belirtirsiniz. Ayrıca, VPN ağ geçidi üzerinden VPN cihazına yönlendirilecek IP adresi ön eklerini de belirtirsiniz. Belirttiğiniz adres ön ekleri, şirket içi adresinizde yer alan ön eklerdir. Şirket içi ağınız değişirse, ön ekleri kolayca güncelleştirebilirsiniz.

Aşağıdaki değerleri kullanın:

* *--gateway-ip-address* şirket içi VPN cihazınızın IP adresidir.
* *--local-address-prefixes* şirket içi adres alanlarınızdır.

Birden fazla adres ön ekine sahip bir yerel ağ geçidi eklemek için [az network local-gateway create](/cli/azure/network/local-gateway) komutunu kullanın:

```azurecli-interactive
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --resource-group TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="6-request-a-public-ip-address"></a><a name="PublicIP"></a>6. genel IP adresi isteyin

Bir VPN ağ geçidinin genel bir IP adresi olmalıdır. İlk olarak IP adresi kaynağını istemeniz, sonra sanal ağ geçidinizi oluştururken bu kaynağa başvurmanız gerekir. VPN ağ geçidi oluşturulurken, IP adresi kaynağa dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Statik bir Genel IP adresi ataması isteğinde bulunamazsınız. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

Dinamik Genel IP adresi istemek için [az network public-ip create](/cli/azure/network/public-ip) komutunu kullanın.

```azurecli-interactive
az network public-ip create --name VNet1GWIP --resource-group TestRG1 --allocation-method Dynamic
```

## <a name="7-create-the-vpn-gateway"></a><a name="CreateGateway"></a>7. VPN Gateway oluşturma

Sanal ağ VPN ağ geçidini oluşturun. VPN ağ geçidi oluşturma işleminin tamamlanması 45 dakika veya daha uzun sürebilir.

Aşağıdaki değerleri kullanın:

* Siteden Siteye bir yapılandırma için *--gateway-type* değeri *Vpn* olmalıdır. Ağ geçidi türü her zaman, uygulamakta olduğunuz yapılandırmaya özeldir. Daha fazla bilgi için bkz. [ağ geçidi türleri](vpn-gateway-about-vpn-gateway-settings.md#gwtype).
* *--VPN türü* , *routebased* (bazı belgelerde dinamik ağ geçidi olarak adlandırılır) veya *Policybased* (bazı belgelerde statik ağ geçidi olarak adlandırılır) olabilir. Ayar, bağlamakta olduğunuz cihazın gereksinimlerine özgüdür. VPN ağ geçidi türleri hakkında daha fazla bilgi için bkz. [VPN Gateway yapılandırma ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md#vpntype).
* Kullanmak istediğiniz Ağ Geçidi SKU'sunu seçin. Bazı SKU’larda yapılandırma sınırlamaları vardır. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

[az network vnet-gateway create](/cli/azure/network/vnet-gateway) komutunu kullanarak VPN ağ geçidini oluşturun. Bu komutu '--no-wait' parametresiyle çalıştırırsanız herhangi bir geri bildirim veya çıktı görmezsiniz. Bu parametre, ağ geçidinin arka planda oluşturulmasına olanak tanır. Ağ geçidinin oluşturulması 45 dakika kadar sürebilir.

```azurecli-interactive
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWIP --resource-group TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait 
```

## <a name="8-configure-your-vpn-device"></a><a name="VPNDevice"></a>8. VPN cihazınızı yapılandırın

Bir şirket içi ağı ile Siteden Siteye bağlantılar için VPN cihazı gerekir. Bu adımda VPN cihazınızı yapılandıracaksınız. VPN cihazınızı yapılandırırken şunlar gerekir:

- Paylaşılan bir anahtar. Siteden Siteye VPN bağlantınızı oluştururken belirttiğiniz paylaşılan anahtarın aynısıdır. Bu örneklerde temel bir paylaşılan anahtar kullanılır. Kullanmak için daha karmaşık bir anahtar oluşturmanız önerilir.
- Sanal ağ geçidinizin Genel IP adresi. Azure Portal, PowerShell veya CLI kullanarak genel IP adresini görüntüleyebilirsiniz. Sanal ağ geçidinizin genel IP adresini bulmak için [az network public-ip list](/cli/azure/network/public-ip) komutunu kullanın. Kolay okunması için, çıkış genel IP’lerin tablo biçiminde gösterileceği şekilde biçimlendirilmiştir.

  ```azurecli-interactive
  az network public-ip list --resource-group TestRG1 --output table
  ```


[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <a name="9-create-the-vpn-connection"></a><a name="CreateConnection"></a>9. VPN bağlantısını oluşturun

Sanal ağ geçidiniz ile şirket içi VPN cihazınız arasında Siteden Siteye VPN bağlantısı oluşturun. Paylaşılan anahtar değerine özellikle dikkat edin; bu değer VPN cihazınız için yapılandırılmış paylaşılan anahtar değeriyle eşleşmelidir.

[az network vpn-connection create](/cli/azure/network/vpn-connection) komutunu kullanarak bağlantıyı oluşturun.

```azurecli-interactive
az network vpn-connection create --name VNet1toSite2 --resource-group TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Kısa bir süre içerisinde bağlantı kurulur.

## <a name="10-verify-the-vpn-connection"></a><a name="toverify"></a>10. VPN bağlantısını doğrulama

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

Bağlantınızı doğrulamak için başka bir yöntem kullanmak istiyorsanız, bkz. [VPN Gateway bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).

## <a name="to-connect-to-a-virtual-machine"></a><a name="connectVM"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="common-tasks"></a><a name="tasks"></a>Genel görevler

Bu bölüm, siteden siteye yapılandırmalarla çalışırken yararlı olan genel komutları içerir. CLI ağ komutlarının tam listesi için bkz. [Azure CLI - Ağ](/cli/azure/network).

[!INCLUDE [local network gateway common tasks](../../includes/vpn-gateway-common-tasks-cli-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/).
* BGP hakkında bilgi edinmek için [BGP’ye Genel Bakış](vpn-gateway-bgp-overview.md) ve [BGP’yi yapılandırma](vpn-gateway-bgp-resource-manager-ps.md) makalelerine bakın.
* Zorlamalı tünel oluşturma hakkında bilgi için bkz. [Zorlamalı tünel oluşturma](vpn-gateway-forced-tunneling-rm.md).
* Yüksek Oranda Kullanılabilir Etkin-Etkin bağlantılar hakkında bilgi için bkz. [Yüksek Oranda Kullanılabilir Şirket İçi ve Dışı ile Sanal Ağdan Sanal Ağa Bağlantı](vpn-gateway-highlyavailable.md).
* Ağ Azure CLI komutlarının listesi için bkz. [Azure CLI](https://docs.microsoft.com/cli/azure/network).
* Azure Resource Manager şablonunu kullanarak siteden siteye VPN bağlantısı oluşturma hakkında bilgi için bkz. [Siteden Siteye VPN Bağlantısı Oluşturma](https://azure.microsoft.com/resources/templates/101-site-to-site-vpn-create/).
* Azure Resource Manager şablonunu kullanarak sanal ağlar arası VPN bağlantısı oluşturma hakkında bilgi için bkz. [HBase coğrafi çoğaltmayı dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-geo/).
