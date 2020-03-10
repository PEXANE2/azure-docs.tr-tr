---
title: 'Azure VPN Gateway: ağ geçitlerini birden çok şirket içi ilke tabanlı VPN cihazına bağlama'
description: Azure Resource Manager ve PowerShell kullanarak birden çok ilke tabanlı VPN cihazına Azure rota tabanlı bir VPN ağ geçidi yapılandırın.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: yushwang
ms.openlocfilehash: 028ed1a632016fcbdf29bb47ab81a36f659785da
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78363985"
---
# <a name="connect-azure-vpn-gateways-to-multiple-on-premises-policy-based-vpn-devices-using-powershell"></a>PowerShell kullanarak Azure VPN ağ geçitlerini birden çok şirket içi ilke tabanlı VPN cihazına bağlama

Bu makale, Azure rota tabanlı bir VPN ağ geçidini, S2S VPN bağlantılarında özel IPSec/ıKE ilkeleri kullanan birden çok şirket içi ilke tabanlı VPN cihazına bağlanmak üzere yapılandırmanıza yardımcı olur.

## <a name="about"></a>İlke tabanlı ve rota tabanlı VPN ağ geçitleri hakkında

İlke tabanlı ve rota tabanlı VPN cihazları, IPSec trafiği seçicilerin bir bağlantıda nasıl *ayarlananlara* göre farklılık gösterir:

* **İlke tabanlı** VPN cihazları, trafiğin IPsec tünelleriyle nasıl şifrelendiğini/çözülemediğini tanımlamak için her iki ağdan gelen ön eklerin birleşimini kullanır. Genellikle paket filtrelemesi gerçekleştiren güvenlik duvarı cihazlarında oluşturulur. IPSec tünel şifreleme ve şifre çözme, paket filtreleme ve işleme motoruna eklenir.
* **Rota tabanlı** VPN cihazları herhangi bir ile herhangi bir (joker karakter) trafik seçicileri kullanır ve tabloları yönlendirme/iletme tablolarının farklı IPSec tünellerinin doğrudan trafiğine izin verir. Genellikle her ıpsec tünelinin bir ağ arabirimi veya VTı (sanal tünel arabirimi) olarak modellendiği yönlendirici platformları üzerine kurulmuştur.

Aşağıdaki diyagramlar iki modeli vurgulayacaktır:

### <a name="policy-based-vpn-example"></a>İlke tabanlı VPN örneği
![ilke tabanlı](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a>Rota tabanlı VPN örneği
![rota tabanlı](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a>İlke tabanlı VPN için Azure desteği
Şu anda Azure, her iki VPN ağ geçidi modunu destekler: rota tabanlı VPN ağ geçitleri ve ilke tabanlı VPN ağ geçitleri. Farklı belirtimlerle sonuçlanan farklı iç platformlar üzerinde oluşturulmuştur:

|                          | **PolicyBased VPN Gateway** | **RouteBased VPN Gateway**       |**RouteBased VPN Gateway**                          |
| ---                      | ---                         | ---                              |---                                                 |
| **Azure ağ geçidi SKU 'SU**    | Temel                       | Temel                            | Standart, HighPerformance, VpnGw1, VpnGw2, VpnGw3  |
| **IKE sürümü**          | IKEv1                       | IKEv2                            | IKEv1 ve IKEv2                                    |
| **Biçimlendir. S2S bağlantıları** | **1**                       | 10                               |Standart: 10<br> Diğer SKU 'Lar: 30                     |
|                          |                             |                                  |                                                    |

Özel IPSec/ıKE ilkesiyle artık Azure rota tabanlı VPN ağ geçitlerini, şirket içi ilke tabanlı VPN cihazlarına bağlanmak için "**PolicyBasedTrafficSelectors**" seçeneği ile önek tabanlı trafik seçicileri kullanacak şekilde yapılandırabilirsiniz. Bu özellik, Azure sanal ağ ve VPN Gateway 'den birden çok şirket içi ilke tabanlı VPN/güvenlik duvarı cihazına bağlanmanızı sağlar ve tek bağlantı sınırını geçerli Azure ilke tabanlı VPN ağ geçitlerinden kaldırır.

> [!IMPORTANT]
> 1. Bu bağlantıyı etkinleştirmek için, şirket içi ilke tabanlı VPN cihazlarınızın Azure rota tabanlı VPN ağ geçitlerine bağlanmak için **Ikev2** 'yi desteklemesi gerekir. VPN cihazı belirtimlerinizi kontrol edin.
> 2. Bu mekanizmaya sahip ilke tabanlı VPN cihazları aracılığıyla bağlanan şirket içi ağlar yalnızca Azure sanal ağına bağlanabilir; **aynı Azure VPN ağ geçidi üzerinden diğer şirket içi ağlara veya sanal ağlara geçiş yapılamaz**.
> 3. Yapılandırma seçeneği, Özel IPSec/ıKE bağlantı ilkesinin bir parçasıdır. İlke tabanlı trafik Seçicisi seçeneğini etkinleştirirseniz, tam ilkeyi (IPSec/ıKE şifreleme ve bütünlük algoritmaları, anahtar güçleri ve SA yaşam süreleri) belirtmeniz gerekir.

Aşağıdaki diyagramda Azure VPN Gateway aracılığıyla geçiş yönlendirmenin neden ilke tabanlı seçenekle çalışmadığına ilişkin bir sorun gösterilmektedir:

![ilke tabanlı Aktarım](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

Diyagramda gösterildiği gibi, Azure VPN ağ geçidi, çapraz bağlantı öneklerini değil, sanal ağdan her bir şirket içi ağ öneklerine yönelik trafik seçicileri içerir. Örneğin, şirket içi site 2, site 3 ve site 4 her biri sırasıyla VNet1 ile iletişim kurabilir, ancak Azure VPN ağ geçidi üzerinden birbirlerine bağlanamaz. Diyagramda bu yapılandırma altındaki Azure VPN Gateway 'de kullanılamayan çapraz bağlantı trafik seçicileri gösterilmektedir.

## <a name="workflow"></a>Akışıyla

Bu makaledeki yönergeler, S2S VPN bağlantısı kurmak üzere [S2S veya VNET-VNET bağlantıları Için IPSec/IKE Ilkesini yapılandırma](vpn-gateway-ipsecikepolicy-rm-powershell.md) bölümünde açıklananla aynı örneği izler. Bu, aşağıdaki diyagramda gösterilmiştir:

![S2S-ilke](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

Bu bağlantıyı etkinleştirmek için iş akışı:
1. Şirket içi bağlantınız için sanal ağ, VPN Gateway ve yerel ağ geçidi oluşturun.
2. IPSec/ıKE ilkesi oluşturun.
3. Bir S2S veya VNet-VNet bağlantısı oluştururken ilkeyi uygulayın ve bağlantıda **ilke tabanlı trafik seçicileri etkinleştirin** .
4. Bağlantı zaten oluşturulduysa, var olan bir bağlantıya ilke uygulayabilir veya ilkeyi güncelleştirebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

* [!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="enablepolicybased"></a>İlke tabanlı trafik seçicileri etkinleştirme

Bu bölümde, bağlantı üzerinde ilke tabanlı trafik seçicilerin nasıl etkinleştirileceği gösterilmektedir. [IPSec/IKE Ilkesini yapılandırma makalesinin 3. kısmını](vpn-gateway-ipsecikepolicy-rm-powershell.md)tamamladığınızdan emin olun. Bu makaledeki adımlar aynı parametreleri kullanır.

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>1\. adım-sanal ağ, VPN Gateway ve yerel ağ geçidi oluşturma

#### <a name="connect-to-your-subscription-and-declare-your-variables"></a>Aboneliğinize bağlanın ve değişkenlerinizi bildirin

1. Bilgisayarınızda PowerShell 'i yerel olarak çalıştırıyorsanız, *Connect-AzAccount* cmdlet 'ini kullanarak oturum açın. Bunun yerine, tarayıcınızda Azure Cloud Shell kullanın.

2. Değişkenlerinizi bildirin. Bu alıştırma için aşağıdaki değişkenleri kullanırız:

   ```azurepowershell-interactive
   $Sub1          = "<YourSubscriptionName>"
   $RG1           = "TestPolicyRG1"
   $Location1     = "East US 2"
   $VNetName1     = "TestVNet1"
   $FESubName1    = "FrontEnd"
   $BESubName1    = "Backend"
   $GWSubName1    = "GatewaySubnet"
   $VNetPrefix11  = "10.11.0.0/16"
   $VNetPrefix12  = "10.12.0.0/16"
   $FESubPrefix1  = "10.11.0.0/24"
   $BESubPrefix1  = "10.12.0.0/24"
   $GWSubPrefix1  = "10.12.255.0/27"
   $DNS1          = "8.8.8.8"
   $GWName1       = "VNet1GW"
   $GW1IPName1    = "VNet1GWIP1"
   $GW1IPconf1    = "gw1ipconf1"
   $Connection16  = "VNet1toSite6"
   $LNGName6      = "Site6"
   $LNGPrefix61   = "10.61.0.0/16"
   $LNGPrefix62   = "10.62.0.0/16"
   $LNGIP6        = "131.107.72.22"
   ```

#### <a name="create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>Sanal ağ, VPN Gateway ve yerel ağ geçidi oluşturma

1. Bir kaynak grubu oluşturun.

   ```azurepowershell-interactive
   New-AzResourceGroup -Name $RG1 -Location $Location1
   ```
2. Üç alt ağ ve VPN ağ geçidi ile sanal ağ TestVNet1 oluşturmak için aşağıdaki örneği kullanın. Değerleri yerine koymak istiyorsanız, ağ geçidi alt ağınızın özel olarak ' GatewaySubnet ' olarak adlandırılmalıdır. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

    ```azurepowershell-interactive
    $fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
    
    New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
    
    $gw1pip1    = New-AzPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $vnet1      = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    
    New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
    
    New-AzLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
    ```

### <a name="step-2---create-an-s2s-vpn-connection-with-an-ipsecike-policy"></a>2\. adım-IPSec/ıKE ilkesiyle bir S2S VPN bağlantısı oluşturma

1. IPSec/ıKE ilkesi oluşturun.

   > [!IMPORTANT]
   > Bağlantıda "UsePolicyBasedTrafficSelectors" seçeneğini etkinleştirmek için bir IPSec/ıKE ilkesi oluşturmanız gerekir.

   Aşağıdaki örnek, bu algoritmalara ve parametrelere sahip bir IPSec/ıKE ilkesi oluşturur:
    * Ikev2: AES256, SHA384, DHGroup24
    * IPSec: AES256, SHA256, PFS yok, SA yaşam süresi 14400 saniye & 102400000KB

   ```azurepowershell-interactive
   $ipsecpolicy6 = New-AzIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
   ```
1. İlke tabanlı trafik seçicileri ve IPSec/ıKE ilkesiyle S2S VPN bağlantısını oluşturun ve önceki adımda oluşturulan IPSec/ıKE ilkesini uygulayın. Bağlantıda ilke tabanlı trafik seçicileri sağlayan "-UsePolicyBasedTrafficSelectors $True" ek parametresine dikkat edin.

   ```azurepowershell-interactive
   $vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
   $lng6 = Get-AzLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

   New-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
   ```
1. Adımları tamamladıktan sonra, S2S VPN bağlantısı tanımlanan IPsec/ıKE ilkesini kullanır ve bağlantıda ilke tabanlı trafik seçicileri etkinleştirir. Aynı Azure VPN ağ geçidinden şirket içi ilke tabanlı ek VPN cihazlarına daha fazla bağlantı eklemek için aynı adımları tekrarlayabilirsiniz.

## <a name="update"></a>İlke tabanlı trafik seçicileri güncelleştirmek için
Bu bölümde, mevcut bir S2S VPN bağlantısı için ilke tabanlı trafik seçicileri seçeneğinin nasıl güncelleşyapılacağı gösterilmektedir.

1. Bağlantı kaynağını alın.

   ```azurepowershell-interactive
   $RG1          = "TestPolicyRG1"
   $Connection16 = "VNet1toSite6"
   $connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
   ```
1. İlke tabanlı trafik seçicileri seçeneğini görüntüleyin.
Aşağıdaki satırda, bağlantı için ilke tabanlı trafik seçicileri kullanılıp kullanılmayacağını gösterilmektedir:

   ```azurepowershell-interactive
   $connection6.UsePolicyBasedTrafficSelectors
   ```

   Satır "**true**" döndürürse, bağlantı üzerinde ilke tabanlı trafik seçicileri yapılandırılır; Aksi takdirde "**false**" döndürür.
1. Bağlantı kaynağını edindikten sonra bir bağlantıda ilke tabanlı trafik seçicileri etkinleştirebilir veya devre dışı bırakabilirsiniz.

   - Etkinleştirmek için

      Aşağıdaki örnek, ilke tabanlı trafik seçicileri seçeneğini sunar, ancak IPSec/ıKE ilkesini değişmeden bırakır:

      ```azurepowershell-interactive
      $RG1          = "TestPolicyRG1"
      $Connection16 = "VNet1toSite6"
      $connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

      Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
      ```

   - Devre dışı bırakmak için

      Aşağıdaki örnek, ilke tabanlı trafik seçicileri seçeneğini devre dışı bırakır, ancak IPSec/ıKE ilkesini değişmeden bırakır:

      ```azurepowershell-interactive
      $RG1          = "TestPolicyRG1"
      $Connection16 = "VNet1toSite6"
      $connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

      Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
      ```

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Özel IPSec/ıKE ilkeleri hakkında daha fazla bilgi için [bkz. S2S VPN veya VNET-VNET bağlantıları Için IPSec/IKE Ilkesini yapılandırma](vpn-gateway-ipsecikepolicy-rm-powershell.md) .
