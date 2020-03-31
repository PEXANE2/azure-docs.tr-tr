---
title: 'Azure VPN Ağ Geçidi: Ağ geçitlerini birden çok şirket içi ilke tabanlı VPN aygıtına bağlama'
description: Azure Kaynak Yöneticisi ve PowerShell kullanarak birden çok ilke tabanlı VPN aygıtına Azure rota tabanlı BIR VPN ağ geçidi yapılandırın.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: yushwang
ms.openlocfilehash: 687c33e50a986cf8af08d0201fe0159a79cf02a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80123314"
---
# <a name="connect-azure-vpn-gateways-to-multiple-on-premises-policy-based-vpn-devices-using-powershell"></a>PowerShell'i kullanarak Azure VPN ağ geçitlerini şirket içi ilke tabanlı birden çok VPN aygıtına bağlayın

Bu makale, S2S VPN bağlantılarında özel IPsec/IKE ilkelerinden yararlanan birden çok şirket içi ilke tabanlı VPN aygıtına bağlanmak için Azure rota tabanlı bir VPN ağ geçidi ni yapılandırmanıza yardımcı olur.

## <a name="about-policy-based-and-route-based-vpn-gateways"></a><a name="about"></a>İlke tabanlı ve rota tabanlı VPN ağ geçitleri hakkında

İlke tabanlı *vs.* rota tabanlı VPN aygıtları, IPsec trafik seçicilerinin bir bağlantıda nasıl ayarlanır:

* **İlke tabanlı** VPN aygıtları, trafiğin IPsec tünelleri aracılığıyla nasıl şifrelenerek/şifresinin çözülür olduğunu tanımlamak için her iki ağdaki önekkombinasyonlarını kullanır. Genellikle paket filtreleme gerçekleştiren güvenlik duvarı aygıtları üzerine inşa edilmiştir. Paket filtreleme ve işleme altyapısına IPsec tünel şifreleme ve şifre çözme özellikleri eklenir.
* **Rota tabanlı** VPN aygıtları any-to-any (joker) trafik seçicileri kullanır ve yönlendirme/yönlendirme tablolarının trafiği farklı IPsec tünellerine yönlendirmesine izin verir. Genellikle her IPsec tünel bir ağ arabirimi veya VTI (sanal tünel arabirimi) olarak modellenir yönlendirici platformlarda inşa edilmiştir.

Aşağıdaki diyagramlar iki modeli vurgular:

### <a name="policy-based-vpn-example"></a>İlke tabanlı VPN örneği
![politika tabanlı](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a>Rota tabanlı VPN örneği
![rota tabanlı](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a>İlke tabanlı VPN için Azure desteği
Şu anda Azure, VPN ağ geçitlerinin her iki modunu da destekler: rota tabanlı VPN ağ geçitleri ve ilke tabanlı VPN ağ geçitleri. Bunlar farklı iç platformlar üzerine inşa edilmiştir, hangi farklı özellikleri neden:

|                          | **Politika Tabanlı VPN Ağ Geçidi** | **RouteBased VPN Ağ Geçidi**       |**RouteBased VPN Ağ Geçidi**                          |
| ---                      | ---                         | ---                              |---                                                 |
| **Azure Ağ Geçidi SKU**    | Temel                       | Temel                            | VpnGw1, VpnGw2, VpnGw3, VpnGw4, VpnGw5  |
| **IKE sürümü**          | IKEv1                       | IKEv2                            | İkEv1 ve IKEv2                         |
| **Max. S2S bağlantıları** | **1**                       | 10                               | 30                     |
|                          |                             |                                  |                                                    |

Özel IPsec/IKE ilkesiyle, artık "**PolicyBasedTrafficSelectors**" seçeneğiyle önek tabanlı trafik seçicileri kullanacak, şirket içi ilke tabanlı VPN aygıtlarına bağlanmak için Azure rota tabanlı VPN ağ geçitlerini yapılandırabilirsiniz. Bu özellik, bir Azure sanal ağ dan VPN ağ geçidinden birden çok şirket içi ilke tabanlı VPN/güvenlik duvarı aygıtına bağlanarak geçerli Azure ilkesi tabanlı VPN ağ geçitlerinden tek bağlantı sınırını kaldırmanızı sağlar.

> [!IMPORTANT]
> 1. Bu bağlantıyı etkinleştirmek için, şirket içi ilke tabanlı VPN aygıtlarınızın Azure rota tabanlı VPN ağ geçitlerine bağlanmak için **IKEv2'yi** desteklemesi gerekir. VPN cihaz özelliklerinizi kontrol edin.
> 2. Bu mekanizmayla ilke tabanlı VPN aygıtları üzerinden bağlanan şirket içi ağlar yalnızca Azure sanal ağına bağlanabilir; **aynı Azure VPN ağ geçidi aracılığıyla diğer şirket içi ağlara veya sanal ağlara geçiş yapamazlar.**
> 3. Yapılandırma seçeneği, özel IPsec/IKE bağlantı ilkesinin bir parçasıdır. İlke tabanlı trafik seçici seçeneğini etkinleştiriseniz, tam ilkeyi (IPsec/IKE şifreleme ve bütünlük algoritmaları, anahtar güçlü yanları ve SA yaşam ömürleri) belirtmeniz gerekir.

Aşağıdaki diyagram, Azure VPN ağ geçidi üzerinden geçiş yönlendirmesinin neden ilke tabanlı seçenekle çalışmadığını gösterir:

![politika tabanlı transit](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

Diyagramda gösterildiği gibi, Azure VPN ağ geçidinde sanal ağdan şirket içi ağ önekleri için trafik seçicileri vardır, ancak çapraz bağlantı önekleri vardır. Örneğin, şirket içi site 2, site 3 ve site 4'ün her biri sırasıyla VNet1 ile iletişim kurabilir, ancak Azure VPN ağ geçidi üzerinden birbirine bağlanamaz. Diyagram, bu yapılandırma altında Azure VPN ağ geçidinde bulunmayan çapraz bağlantı trafik seçicilerini gösterir.

## <a name="workflow"></a><a name="workflow"></a>Iş akışı

Bu makaledeki yönergeler, [S2S veya VNet-to-VNet bağlantıları için S2S VPN bağlantısı kurmak için IPsec/IKE'yi Yapılandırı(IPsec/IKE" politikasını yapılandırın"](vpn-gateway-ipsecikepolicy-rm-powershell.md) ilkesinde açıklandığı gibi aynı örneği izler. Bu, aşağıdaki diyagramda gösterilmiştir:

![s2s-politikası](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

Bu bağlantıyı etkinleştirmek için iş akışı:
1. Binalar arası bağlantınız için sanal ağ, VPN ağ geçidi ve yerel ağ ağ geçidi oluşturun.
2. Bir IPsec/IKE ilkesi oluşturun.
3. Bir S2S veya VNet'ten VNet'e bağlantı oluşturduğunuzda ve bağlantıdaki **ilke tabanlı trafik seçicilerini etkinleştirdiğinizde ilkeyi** uygulayın.
4. Bağlantı zaten oluşturulmuşsa, ilkeyi varolan bir bağlantıya uygulayabilir veya güncelleştirebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

* [!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="enable-policy-based-traffic-selectors"></a><a name="enablepolicybased"></a>İlke tabanlı trafik seçicilerini etkinleştirme

Bu bölümde, bir bağlantıda ilke tabanlı trafik seçicileri nasıl etkinleştirilebilirsiniz. [Yapılandırma IPsec/IKE ilkesi makalesinin Bölüm 3'ü](vpn-gateway-ipsecikepolicy-rm-powershell.md)tamamladığınızdan emin olun. Bu makaledeki adımlar aynı parametreleri kullanır.

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>Adım 1 - Sanal ağ, VPN ağ geçidi ve yerel ağ ağ geçidi oluşturma

#### <a name="connect-to-your-subscription-and-declare-your-variables"></a>Aboneliğinize bağlanın ve değişkenlerinizi bildirin

1. PowerShell'i bilgisayarınızda yerel olarak çalıştırıyorsanız, *Connect-AzAccount* cmdlet'ini kullanarak oturum açın. Bunun yerine tarayıcınızda Azure Bulut Su Şununu kullanın.

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

#### <a name="create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>Sanal ağ, VPN ağ geçidi ve yerel ağ ağ geçidi oluşturma

1. Bir kaynak grubu oluşturun.

   ```azurepowershell-interactive
   New-AzResourceGroup -Name $RG1 -Location $Location1
   ```
2. Üç alt ağ içeren sanal ağ TestVNet1 ve VPN ağ geçidini oluşturmak için aşağıdaki örneği kullanın. Değerleri değiştirmek istiyorsanız, ağ geçidi alt ağınıza her zaman özel olarak 'GatewaySubnet' adını vermeniz önemlidir. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

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

### <a name="step-2---create-an-s2s-vpn-connection-with-an-ipsecike-policy"></a>Adım 2 - IPsec/IKE ilkesiyle S2S VPN bağlantısı oluşturma

1. Bir IPsec/IKE ilkesi oluşturun.

   > [!IMPORTANT]
   > Bağlantıda "UsePolicyBasedTrafficSelectors" seçeneğini etkinleştirmek için bir IPsec/IKE ilkesi oluşturmanız gerekir.

   Aşağıdaki örnek, bu algoritmalar ve parametrelerle bir IPsec/IKE ilkesi oluşturur:
    * İkEv2: AES256, SHA384, DHGroup24
    * IPsec: AES256, SHA256, PFS Yok, SA Ömür Boyu 14400 saniye & 10240000KB

   ```azurepowershell-interactive
   $ipsecpolicy6 = New-AzIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
   ```
1. İlke tabanlı trafik seçicileri ve IPsec/IKE ilkesiyle S2S VPN bağlantısını oluşturun ve önceki adımda oluşturulan IPsec/IKE ilkesini uygulayın. Bağlantıda ilke tabanlı trafik seçicileri sağlayan ek "-UsePolicyBasedTrafficSelectors $True" parametresine dikkat edin.

   ```azurepowershell-interactive
   $vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
   $lng6 = Get-AzLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

   New-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
   ```
1. Adımları tamamladıktan sonra, S2S VPN bağlantısı tanımlanan IPsec/IKE ilkesini kullanır ve bağlantıda ilke tabanlı trafik seçicilerini etkinleştirecektir. Aynı Azure VPN ağ geçidinden şirket içi ilke tabanlı VPN aygıtlarına daha fazla bağlantı eklemek için aynı adımları yineleyebilirsiniz.

## <a name="to-update-policy-based-traffic-selectors"></a><a name="update"></a>İlke tabanlı trafik seçicilerini güncelleştirmek için
Bu bölümde, varolan bir S2S VPN bağlantısı için ilke tabanlı trafik seçiciler seçeneğini nasıl güncelleştirebilirsiniz gösterilmektedir.

1. Bağlantı kaynağını alın.

   ```azurepowershell-interactive
   $RG1          = "TestPolicyRG1"
   $Connection16 = "VNet1toSite6"
   $connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
   ```
1. İlke tabanlı trafik seçiciler seçeneğini görüntüleyin.
Aşağıdaki satır, ilke tabanlı trafik seçicilerinin bağlantı için kullanılıp kullanılmadığını gösterir:

   ```azurepowershell-interactive
   $connection6.UsePolicyBasedTrafficSelectors
   ```

   Satır "**True**" döndürürse, ilke tabanlı trafik seçiciler bağlantıüzerinde yapılandırılır; aksi takdirde "**False**" döndürür.
1. Bağlantı kaynağını aldıktan sonra, bir bağlantıdaki ilke tabanlı trafik seçicilerini etkinleştirebilir veya devre dışı kullanabilirsiniz.

   - Etkinleştirmek için

      Aşağıdaki örnek, ilke tabanlı trafik seçiciler seçeneğini etkinleştirire, ancak IPsec/IKE ilkesini değiştirmeden bırakır:

      ```azurepowershell-interactive
      $RG1          = "TestPolicyRG1"
      $Connection16 = "VNet1toSite6"
      $connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

      Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
      ```

   - Devre Dışı kalmak için

      Aşağıdaki örnek, ilke tabanlı trafik seçiciler seçeneğini devre dışı bırakır, ancak IPsec/IKE ilkesini değiştirmeden bırakır:

      ```azurepowershell-interactive
      $RG1          = "TestPolicyRG1"
      $Connection16 = "VNet1toSite6"
      $connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

      Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
      ```

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Ayrıca, özel IPsec/IKE ilkeleri hakkında daha fazla bilgi için [S2S VPN veya VNet-to-VNet bağlantıları için IPsec/IKE politikasını yapılandırın.](vpn-gateway-ipsecikepolicy-rm-powershell.md)
