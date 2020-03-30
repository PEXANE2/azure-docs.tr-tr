---
title: "Klasik sanal ağları Azure Kaynak Yöneticisi VNets: PowerShell'e bağlayın"
description: VPN Ağ Geçidi ve PowerShell'i kullanarak klasik VNet'ler ve Kaynak Yöneticisi VNets arasında VPN bağlantısı oluşturun.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/17/2018
ms.author: cherylmc
ms.openlocfilehash: 1dc0eec6178420976181b05a059e9f8b4859ec2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152015"
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>PowerShell kullanarak farklı dağıtım modellerindeki sanal ağları birbirine bağlama

Bu makale, ayrı dağıtım modellerinde bulunan kaynakların birbiriyle iletişim kurmasına izin vermek için klasik VNets'i Kaynak Yöneticisi VNet'e bağlamanıza yardımcı olur. Bu makaledeki adımlar PowerShell'i kullanır, ancak bu listeden makaleyi seçerek Azure portalını kullanarak bu yapılandırmayı da oluşturabilirsiniz.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Powershell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Klasik bir VNet'i Kaynak Yöneticisi VNet'e bağlamak, bir VNet'i şirket içi bir konuma bağlamaya benzer. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır. Farklı aboneliklerde ve farklı bölgelerde bulunan VNet'ler arasında bağlantı oluşturabilirsiniz. Ayrıca, yapılandırıldıkları ağ geçidi dinamik veya rota tabanlı olduğu sürece, şirket içi ağlara zaten bağlantısı olan VNet'leri de bağlayabilirsiniz. Sanal ağlar arası bağlantılar hakkında daha fazla bilgi için bu makalenin sonunda yer alan [Sanal ağlar arası bağlantılar hakkında SSS](#faq) bölümünü inceleyin. 

Zaten sanal bir ağ ağ ağ geçidiniz yoksa ve bir ağ oluşturmak istemiyorsanız, bunun yerine VNet'lerinizi VNet Peering kullanarak bağlamayı düşünebilirsiniz. VNet eşlemesi VPN ağ geçidini kullanmaz. Daha fazla bilgi için bkz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md).

## <a name="before-you-begin"></a><a name="before"></a>Başlamadan önce

Aşağıdaki adımlar, her VNet için dinamik veya rota tabanlı bir ağ geçidi yapılandırmak ve ağ geçitleri arasında bir VPN bağlantısı oluşturmak için gereken ayarlar arasında size yol gösterir. Bu yapılandırma statik veya ilke tabanlı ağ geçitlerini desteklemez.

### <a name="prerequisites"></a><a name="pre"></a>Ön koşullar

* Her iki VNets zaten oluşturuldu. Bir kaynak yöneticisi sanal ağ oluşturmanız gerekiyorsa, [bkz.](../virtual-network/quick-create-powershell.md#create-a-resource-group-and-a-virtual-network) Klasik bir sanal ağ oluşturmak için [bkz.](https://docs.microsoft.com/azure/virtual-network/create-virtual-network-classic)
* VNet'lerin adres aralıkları birbiriyle çakışmaz veya ağ geçitlerinin bağlanabileceği diğer bağlantılar için aralıklardan herhangi biriyle çakışmaz.
* En son PowerShell cmdlets yüklü. Daha fazla bilgi için [Azure PowerShell'i nasıl yükleyip yapılandırılabildiğini](/powershell/azure/overview) öğrenin. Hem Hizmet Yönetimi (SM) hem de Kaynak Yöneticisi (RM) cmdlets'i yüklediğinizden emin olun. 

### <a name="example-settings"></a><a name="exampleref"></a>Örnek ayarlar

Bu değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bunlara bakabilirsiniz.

**Klasik VNet ayarları**

VNet Adı = ClassicVNet <br>
Yer = Batı ABD <br>
Sanal Ağ Adresi Alanları = 10.0.0.0/24 <br>
Alt ağ-1 = 10.0.0.0/27 <br>
Ağ Geçidi Subnet = 10.0.0.32/29 <br>
Yerel Ağ Adı = RMVNetLocal <br>
GatewayType = Dinamik Routing

**Kaynak Yöneticisi VNet ayarları**

VNet Adı = RMVNet <br>
Kaynak Grubu = RG1 <br>
Sanal Ağ IP Adresi Alanları = 192.168.0.0/16 <br>
Alt net-1 = 192.168.1.0/24 <br>
Ağ Geçidi Subnet = 192.168.0.0/26 <br>
Yer = Doğu ABD <br>
Ağ geçidi genel IP adı = gwpip <br>
Yerel Ağ Ağ Geçidi = ClassicVNetLocal <br>
Sanal Ağ Ağ Geçidi adı = RMGateway <br>
Ağ geçidi IP adresleme yapılandırma = gwipconfig

## <a name="section-1---configure-the-classic-vnet"></a><a name="createsmgw"></a>Bölüm 1 - Klasik VNet'i yapılandırın
### <a name="1-download-your-network-configuration-file"></a>1. Ağ yapılandırma dosyanızı indirin
1. PowerShell konsolunda azure hesabınızda yüksek haklara sahip oturum açın. Aşağıdaki cmdlet, Azure Hesabınız için giriş kimlik bilgilerini ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir. Bu bölümde klasik Hizmet Yönetimi (SM) Azure PowerShell cmdlets kullanılır.

   ```azurepowershell
   Add-AzureAccount
   ```

   Azure aboneliğinizi alın.

   ```azurepowershell
   Get-AzureSubscription
   ```

   Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

   ```azurepowershell
   Select-AzureSubscription -SubscriptionName "Name of subscription"
   ```
2. Aşağıdaki komutu çalıştırarak Azure ağ yapılandırma dosyanızı dışa aktarın. Gerekirse dosyanın konumunu farklı bir konuma dışa aktarmak için değiştirebilirsiniz.

   ```azurepowershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
3. İndirdiğiniz .xml dosyasını açmak için açın. Ağ yapılandırma dosyasının bir örneği [için, Ağ Yapılandırma Şeması'na](https://msdn.microsoft.com/library/jj157100.aspx)bakın.

### <a name="2-verify-the-gateway-subnet"></a>2. Ağ geçidi alt netini doğrulayın
**VirtualNetworkSites** öğesinde, vnet'inize bir ağ geçidi alt ağı ekleyin. Ağ yapılandırma dosyasıyla çalışırken, ağ geçidi alt ağının adı "GatewaySubnet" olmalıdır veya Azure bunu tanıyemez ve ağ geçidi alt ağı olarak kullanamaz.

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Örnek:**

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="3-add-the-local-network-site"></a>3. Yerel ağ sitesini ekleyin
Eklediğiniz yerel ağ sitesi, bağlanmak istediğiniz RM VNet'i temsil eder. Dosyaya bir **LocalNetworkSites** öğesi ekleyin, zaten yoksa. Yapılandırmanın bu noktasında, KAYNAK Yöneticisi VNet için ağ geçidini henüz oluşturmadığımız için VPNGatewayAddress geçerli bir genel IP adresi olabilir. Ağ geçidini oluşturduktan sonra, bu yer tutucu IP adresini RM ağ geçidine atanmış doğru genel IP adresiyle değiştiririz.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="4-associate-the-vnet-with-the-local-network-site"></a>4. VNet'i yerel ağ sitesiyle ilişkilendirin
Bu bölümde, VNet'i bağlamak istediğiniz yerel ağ sitesini belirtiriz. Bu durumda, daha önce başvurulan Kaynak Yöneticisi VNet'tir. İsimlerin eşleştirdiğinden emin ol. Bu adım bir ağ geçidi oluşturmaz. Ağ geçidinin bağlanacağı yerel ağı belirtir.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="5-save-the-file-and-upload"></a>5. Dosyayı kaydedin ve yükleyin
Dosyayı kaydedin ve aşağıdaki komutu çalıştırarak Azure'a aktarın. Ortamınız için gerekli olan dosya yolunu değiştirdiğinizden emin olun.

```azurepowershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Alma nın başarılı olduğunu gösteren benzer bir sonuç görürsünüz.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="6-create-the-gateway"></a>6. Ağ geçidi oluşturma

Bu örneği çalıştırmadan önce, Azure'un görmeyi beklediği tam adlar için indirdiğiniz ağ yapılandırma dosyasına bakın. Ağ yapılandırma dosyası, klasik sanal ağlarınız için değerleri içerir. Bazen, dağıtım modellerinde ki farklılıklar nedeniyle Azure portalında klasik VNet ayarları oluşturulurken ağ yapılandırma dosyasında klasik VNet adları değiştirilir. Örneğin, Azure portalını 'Klasik VNet' adlı klasik bir VNet oluşturmak için kullandıysanız ve 'ClassicRG' adlı bir kaynak grubunda oluşturduysanız, ağ yapılandırma dosyasında bulunan ad 'Group ClassicRG Classic VNet' olarak dönüştürülür. Boşluk içeren bir VNet'in adını belirtirken, değerin etrafındaki tırnak işaretlerini kullanın.


Dinamik bir yönlendirme ağ geçidi oluşturmak için aşağıdaki örneği kullanın:

```azurepowershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

**AzureVNetAğ Geçidi** cmdlet'ini kullanarak ağ geçidinin durumunu kontrol edebilirsiniz.

## <a name="section-2---configure-the-rm-vnet-gateway"></a><a name="creatermgw"></a>Bölüm 2 - RM VNet ağ geçidini yapılandırın



Ön koşullar zaten bir RM VNet oluşturduğunuzu varsayar. Bu adımda, RM VNet için bir VPN ağ geçidi oluşturursunuz. Klasik VNet'in ağ geçidinin genel IP adresini alana kadar bu adımları başlatmayın. 

1. PowerShell konsolunda Azure hesabınızda oturum açın. Aşağıdaki cmdlet, Azure Hesabınız için giriş kimlik bilgilerini ister. Oturum açma işleminden sonra hesap ayarlarınız Azure PowerShell tarafından kullanılabilecek şekilde indirilir. Azure Cloud Shell'i tarayıcıda başlatmak için isteğe bağlı olarak "Dene" özelliğini kullanabilirsiniz.

   Azure Bulut Su Şurası kullanıyorsanız, aşağıdaki cmdlet'i atlayın:

   ```azurepowershell
   Connect-AzAccount
   ``` 
   Doğru aboneliği kullandığınızı doğrulamak için aşağıdaki cmdlet'i çalıştırın:  

   ```azurepowershell-interactive
   Get-AzSubscription
   ```
   
   Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtin.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Name of subscription"
   ```
2. Yerel ağ geçidi oluşturma. Sanal bir ağda, yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Bu durumda, yerel ağ ağ geçidi Klasik VNet'inizin anlamına gelir. Azure'un bu konuya başvurabileceği bir ad verin ve adres alanı önekini de belirtin. Azure, belirttiğiniz IP adresi ön ekini kullanarak hangi trafiğin şirket içi konumunuza gönderileceğini belirler. Ağ geçidinizi oluşturmadan önce buradaki bilgileri daha sonra ayarlamanız gerekirse, değerleri değiştirebilir ve örneği yeniden çalıştırabilirsiniz.
   
   **-Ad,** yerel ağ ağ geçidine başvurmak için atamak istediğiniz addır.<br>
   **-AddressPrefix,** klasik VNet'inizin Adres Alanıdır.<br>
   **-GatewayIpAddress** klasik VNet'in ağ geçidinin genel IP adresidir. Doğru IP adresini yansıtacak şekilde aşağıdaki örnek metin "n.n.n.n"i değiştirdiğinden emin olun.<br>

   ```azurepowershell-interactive
   New-AzLocalNetworkGateway -Name ClassicVNetLocal `
   -Location "West US" -AddressPrefix "10.0.0.0/24" `
   -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
   ```
3. Kaynak Yöneticisi VNet için sanal ağ ağ ağ geçidine tahsis edilecek ortak bir IP adresi isteyin. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi dinamik olarak sanal ağ ağ geçidine ayrılmıştır. Ancak bu, IP adresinin değiştiği anlamına gelmez. Sanal ağ ağ ağ geçidi IP adresinin yalnızca ağ geçidi silinip yeniden oluşturulduğu zamandır. Ağ geçidinin yeniden boyutlandırılması, sıfırlanması veya diğer dahili bakım/yükseltmeleri arasında değişmez.

   Bu adımda, daha sonraki bir adımda kullanılan bir değişken de belirleriz.

   ```azurepowershell-interactive
   $ipaddress = New-AzPublicIpAddress -Name gwpip `
   -ResourceGroupName RG1 -Location 'EastUS' `
   -AllocationMethod Dynamic
   ```

4. Sanal ağınızın bir ağ geçidi alt ağına sahip olduğunu doğrulayın. Ağ geçidi alt ağı yoksa, bir tane ekleyin. Ağ geçidi alt *ağıGatewaySubnet*olarak adlandırıldıemin olun.
5. Aşağıdaki komutu çalıştırarak ağ geçidi için kullanılan alt ağı alın. Bu adımda, bir sonraki adımda kullanılacak bir değişken de belirleriz.
   
   **-Adı** Kaynak Yöneticisi VNet adıdır.<br>
   **-ResourceGroupName,** VNet'in ilişkili olduğu kaynak grubudur. Ağ geçidi alt ağı bu VNet için zaten mevcut olmalı ve düzgün çalışması için *GatewaySubnet* olarak adlandırılmalıdır.<br>

   ```azurepowershell-interactive
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name GatewaySubnet `
   -VirtualNetwork (Get-AzVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
   ``` 

6. Ağ geçidi IP adresleme yapılandırmasını oluşturun. Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Ağ geçidi yapılandırmanızı oluşturmak için aşağıdaki örneği kullanın.

   Bu adımda, **-SubnetId** ve **-PublicIpAddressId** parametreleri sırasıyla kimlik özelliğini alt ağdan ve IP adresi nesnelerinden geçirilmelidir. Basit bir ip kullanamazsın. Bu değişkenler, ortak bir IP ve alt ağı almak için adım istemek için adım ayarlanır.

   ```azurepowershell-interactive
   $gwipconfig = New-AzVirtualNetworkGatewayIpConfig `
   -Name gwipconfig -SubnetId $subnet.id `
   -PublicIpAddressId $ipaddress.id
   ```
7. Aşağıdaki komutu çalıştırarak Kaynak Yöneticisi sanal ağ ağ ağ geçidi oluşturun. `-VpnType` *RouteBased*olmalıdır. Ağ geçidinin oluşturması 45 dakika veya daha uzun sürebilir.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
   -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
   -IpConfigurations $gwipconfig `
   -EnableBgp $false -VpnType RouteBased
   ```
8. VPN ağ geçidi oluşturulduktan sonra genel IP adresini kopyalayın. Klasik VNet'inizin yerel ağ ayarlarını yapılandırırken kullanırsınız. Genel IP adresini almak için aşağıdaki cmdlet'i kullanabilirsiniz. Genel IP adresi iadede *IpAddress*olarak listelenir.

   ```azurepowershell-interactive
   Get-AzPublicIpAddress -Name gwpip -ResourceGroupName RG1
   ```

## <a name="section-3---modify-the-classic-vnet-local-site-settings"></a><a name="localsite"></a>Bölüm 3 - Klasik VNet yerel site ayarlarını değiştirin

Bu bölümde, klasik VNet ile çalışırsınız. Kaynak Yöneticisi VNet ağ geçidine bağlanmak için kullanılacak yerel site ayarlarını belirtirken kullandığınız yer tutucu IP adresini değiştirirsiniz. Klasik VNet ile çalıştığınız için, Azure Cloud Shell TryIt'i değil, bilgisayarınıza yerel olarak yüklenen PowerShell'i kullanın.

1. Ağ yapılandırma dosyasını dışa aktarın.

   ```azurepowershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
2. Bir metin düzenleyicisi kullanarak VPNGatewayAddress değerini değiştirin. Yer tutucu IP adresini Kaynak Yöneticisi ağ geçidinin genel IP adresiyle değiştirin ve değişiklikleri kaydedin.

   ```
   <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
   ```
3. Değiştirilmiş ağ yapılandırma dosyasını Azure'a aktarın.

   ```azurepowershell
   Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
   ```

## <a name="section-4---create-a-connection-between-the-gateways"></a><a name="connect"></a>Bölüm 4 - Ağ geçitleri arasında bağlantı oluşturma
Ağ geçitleri arasında bağlantı oluşturmak PowerShell gerektirir. PowerShell cmdlets'in klasik sürümünü kullanmak için Azure Hesabınızı eklemeniz gerekebilir. Bunu yapmak için **Ekle-AzureHesabı'nı**kullanın.

1. PowerShell konsolunda paylaşılan anahtarınızı ayarlayın. Cmdlets'i çalıştırmadan önce, Azure'un görmeyi beklediği tam adlar için indirdiğiniz ağ yapılandırma dosyasına bakın. Boşluk içeren bir VNet'in adını belirtirken, değerin etrafında tek tırnak işaretleri kullanın.<br><br>Aşağıdaki örnekte, **-VNetName** klasik VNet'in adıdır ve **-LocalNetworkSiteName** yerel ağ sitesi için belirlediğiniz addır. **-SharedKey** oluşturduğunuz ve belirttiğiniz bir değerdir. Örnekte, 'abc123' kullandık, ancak daha karmaşık bir şey oluşturabilir ve kullanabilirsiniz. Önemli olan, burada belirttiğiniz değerin, bağlantınızı oluştururken bir sonraki adımda belirttiğiniz değerle aynı olması gerektiğidir. İade Durum **göstermelidir: Başarılı**.

   ```azurepowershell
   Set-AzureVNetGatewayKey -VNetName ClassicVNet `
   -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
   ```
2. Aşağıdaki komutları çalıştırarak VPN bağlantısını oluşturun:
   
   Değişkenleri ayarlayın.

   ```azurepowershell-interactive
   $vnet01gateway = Get-AzLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
   $vnet02gateway = Get-AzVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
   ```
   
   Bağlantıyı oluşturun. **-ConnectionType'ın** Vnet2Vnet değil, IPsec olduğuna dikkat edin.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
   -Location "East US" -VirtualNetworkGateway1 `
   $vnet02gateway -LocalNetworkGateway2 `
   $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```

## <a name="section-5---verify-your-connections"></a><a name="verify"></a>Bölüm 5 - Bağlantılarınızı doğrulayın

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Klasik VNet'inizden Kaynak Yöneticisi VNet'inize olan bağlantıyı doğrulamak için

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a>Azure portalında

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Kaynak Yöneticisi VNet'inizden klasik VNet'inize olan bağlantıyı doğrulamak için

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a>Azure portalında

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="vnet-to-vnet-faq"></a><a name="faq"></a>Sanal ağlar arası bağlantılar hakkında SSS

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]
