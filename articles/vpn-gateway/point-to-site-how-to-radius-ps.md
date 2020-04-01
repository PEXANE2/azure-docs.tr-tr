---
title: "Bir bilgisayarı, Site'ye Noktadan Noktaya ve RADIUS kimlik doğrulamasını kullanarak sanal ağa bağlayın: PowerShell | Azure"
description: Windows ve Mac OS X istemcilerini P2S ve RADIUS kimlik doğrulamasını kullanarak sanal ağa güvenli bir şekilde bağlayın.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: cherylmc
ms.openlocfilehash: cb9a02532c3651aca544ed946f40bdcff9e9be83
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411767"
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-radius-authentication-powershell"></a>RADIUS kimlik doğrulaması kullanarak Bir VNet'e Noktadan Siteye bağlantı yapılandırma: PowerShell

Bu makalede, RADIUS kimlik doğrulaması kullanan Bir Noktaya Sayfa bağlantısı ile bir VNet oluşturmak için nasıl gösterir. Bu yapılandırma yalnızca Kaynak Yöneticisi dağıtım modeli için kullanılabilir.

Noktadan Siteye (P2S) VPN ağ geçidi, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. VNet'inize evden veya konferanstan evden veya konferansla yaptığınız gibi uzak bir konumdan bağlanmak istediğinizde, Site'ye noktadan siteye VPN bağlantıları yararlıdır. P2S VPN ayrıca, bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda Siteden Siteye VPN yerine kullanabileceğiniz yararlı bir çözümüdür.

Windows ve Mac cihazlardan bir P2S VPN bağlantısı başlatılır. Bağlanma istemcileri aşağıdaki kimlik doğrulama yöntemlerini kullanabilir: 

* RADIUS sunucusu
* VPN Ağ Geçidi yerel sertifika kimlik doğrulaması

Bu makale, RADIUS sunucusu kullanarak kimlik doğrulaması ile bir P2S yapılandırma yapılandırmayardımcı olur. Bunun yerine oluşturulan sertifikaları ve VPN ağ geçidi yerel sertifika kimlik doğrulamasını kullanarak kimlik doğrulaması yapmak istiyorsanız, [VPN ağ geçidi yerel sertifika kimlik doğrulaması kullanarak bir VNet'e Nokta-To-Site bağlantısını Yapılandırma](vpn-gateway-howto-point-to-site-rm-ps.md)konusuna bakın.

![Bağlantı diyagramı - RADIUS](./media/point-to-site-how-to-radius-ps/p2sradius.png)

Noktadan Siteye bağlantılar için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. P2S, SSTP (Secure Socket Tunneling Protocol), OpenVPN veya IKEv2 üzerinden VPN bağlantısı oluşturur.

* SSTP, yalnızca Windows istemci platformlarında desteklenen TLS tabanlı bir VPN tünelidir. Güvenlik duvarlarından geçebildiği için, herhangi bir yerden Azure’a bağlanmak için ideal bir seçenektir. Sunucu tarafında 1.0, 1.1 ve 1.2 SSTP sürümlerini destekliyoruz. Kullanılacak sürüm, istemci tarafından belirlenir. Windows 8.1 ve sonraki sürümlerinde, SSTP'de varsayılan olarak 1.2 kullanılır.

* OpenVPN® Protokolü, SSL/TLS tabanlı VPN protokolü. Bir TLS VPN çözümü güvenlik duvarlarına nüfuz edebilir, çünkü çoğu güvenlik duvarı TCP bağlantı noktası 443 giden ilerlediği ve TLS'nin kullandığı açık olduğundan. OpenVPN, Android, iOS (11.0 ve üzeri sürümler), Windows, Linux ve Mac aygıtlarından (OSX sürümleri 10.13 ve üzeri) bağlanmak için kullanılabilir.

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir.

P2S bağlantıları aşağıdakileri gerektirir:

* RouteBased VPN ağ geçidi. 
* Kullanıcı kimlik doğrulamasını işlemek için bir RADIUS sunucusu. RADIUS sunucusu şirket içinde veya Azure VNet'te dağıtılabilir.
* VNet'e bağlanacak Windows aygıtları için VPN istemcisi yapılandırma paketi. VPN istemcisi yapılandırma paketi, bir VPN istemcisinin P2S üzerinden bağlanması için gereken ayarları sağlar.

## <a name="about-active-directory-ad-domain-authentication-for-p2s-vpns"></a><a name="aboutad"></a>P2S VPN'leri için Active Directory (AD) Etki Alanı Kimlik Doğrulaması Hakkında

AD Etki Alanı kimlik doğrulaması, kullanıcıların kuruluş etki alanı kimlik bilgilerini kullanarak Azure'da oturum açmalarına olanak tanır. AD sunucusuyla tümleşen bir RADIUS sunucusu gerektirir. Kuruluşlar, mevcut RADIUS dağıtımından da yararlanabilirler.
 
RADIUS sunucusu şirket içinde veya Azure VNet'inizde bulunabilir. Kimlik doğrulama sırasında VPN ağ geçidi geçiş görevi görür ve radius sunucusu ile bağlantı aygıtı arasında kimlik doğrulama iletilerini ileri geri iletir. VPN ağ geçidinin RADIUS sunucusuna ulaşabilmesi önemlidir. RADIUS sunucusu şirket içinde bulunuyorsa, Azure'dan şirket içi siteye VPN Site-Site bağlantısı gereklidir.

Active Directory dışında, radius sunucusu diğer dış kimlik sistemleriyle de tümleşebilir. Bu, MFA seçenekleri de dahil olmak üzere, Site'den Siteye Puanlı VPN'ler için birçok kimlik doğrulama seçeneği açar. Entegre olduğu kimlik sistemlerinin listesini almak için RADIUS sunucu satıcı belgelerinizi kontrol edin.

![Bağlantı diyagramı - RADIUS](./media/point-to-site-how-to-radius-ps/radiusimage.png)

> [!IMPORTANT]
>Bir RADIUS sunucusuna şirket içinde bağlanmak için yalnızca VPN Siteden Siteye bağlantı kullanılabilir. ExpressRoute bağlantısı kullanılamaz.
>
>

## <a name="before-beginning"></a><a name="before"></a>Başlamadan önce

Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

### <a name="example-values"></a><a name="example"></a>Örnek değerler

Örnek değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakabilirsiniz. İzlenecek yol olarak adımları kullanıp değerleri değiştirmeden uygulayabilir veya ortamınızı yansıtacak şekilde değiştirebilirsiniz.

* **Ad: VNet1**
* **Adres alanı: 192.168.0.0/16** ve **10.254.0.0/16**<br>Bu örnekte, bu yapılandırmanın birden çok adres alanıyla çalıştığını göstermek için birden fazla adres alanı kullanıyoruz. Ancak, bu yapılandırma için birden çok adres alanı gerekli değildir.
* **Alt ağ adı: FrontEnd**
  * **Alt ağ adres aralığı: 192.168.1.0/24**
* **Alt ağ adı: BackEnd**
  * **Alt ağ adres aralığı: 10.254.1.0/24**
* **Alt ağ adı: GatewaySubnet**<br>VPN ağ geçidinin çalışması için Alt Ağ adı olarak *GatewaySubnet*'in kullanılması zorunludur.
  * **Ağ Geçidi Alt Ağ adres aralığı: 192.168.200.0/24** 
* **VPN istemcisi adres havuzu: 172.16.201.0/24**<br>Sanal ağa, bu Noktadan Siteye bağlantıyı kullanarak bağlanan VPN istemcileri, VPN istemci adresi havuzundan bir IP adresi alır.
* **Abonelik:** Birden fazla aboneliğiniz varsa doğru aboneliği kullandığınızdan emin olun.
* **Kaynak Grubu: TestRG**
* **Yer: Doğu ABD**
* **DNS Server:** VNet'iniz için ad çözümü için kullanmak istediğiniz DNS sunucusunun IP adresi. (isteğe bağlı)
* **Ağ Geçidi Adı: Vnet1GW**
* **Ortak IP adı: VNet1GWPIP**
* **VpnType: RouteBased**

## <a name="1-set-the-variables"></a><a name="signin"></a>1. Değişkenleri ayarlama

Kullanmak istediğiniz değişkenleri bildirin. Aşağıdaki örneği kullanın ve gerektiğinde, değerleri kendi değerlerinizle değiştirin. Egzersiz sırasında PowerShell/Cloud Shell oturumunuzu herhangi bir noktada kapatırsanız, değişkenleri yeniden bildirmek için değerleri kopyalayıp yapıştırmanız gerekir.

  ```azurepowershell-interactive
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <a name="2-create-the-resource-group-vnet-and-public-ip-address"></a>2. <a name="vnet"> </a>Kaynak grubu, VNet ve Genel IP adresi oluşturma

Aşağıdaki adımlar, kaynak grubunda üç alt ağ içeren bir kaynak grubu ve sanal ağ oluşturur. Değerleri yerine alırken, ağ geçidi alt ağınıza her zaman özel olarak 'GatewaySubnet' adını verirken önemlidir. Başka bir isim verirsen, ağ geçidi oluşturma başarısız olur;

1. Bir kaynak grubu oluşturun.

   ```azurepowershell-interactive
   New-AzResourceGroup -Name "TestRG" -Location "East US"
   ```
2. Sanal ağ için alt ağ yapılandırmalarını oluşturup *FrontEnd*, *BackEnd* ve *GatewaySubnet* olarak adlandırın. Bu ön ekler bildirdiğiniz sanal adres alanının parçası olmalıdır.

   ```azurepowershell-interactive
   $fesub = New-AzVirtualNetworkSubnetConfig -Name "FrontEnd" -AddressPrefix "192.168.1.0/24"  
   $besub = New-AzVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.254.1.0/24"  
   $gwsub = New-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "192.168.200.0/24"
   ```
3. Sanal ağı oluşturun.

   Bu örnekte, -DnsServer sunucu parametresi isteğe bağlıdır. Bir değer belirtildiğinde yeni bir DNS sunucusu oluşturulmaz. Belirttiğiniz DNS sunucusu IP adresi, sanal ağınızdan bağlandığınız kaynakların adlarını çözümleyebilen bir DNS sunucusu olmalıdır. Bu örnek için özel bir IP adresi kullandık, ancak DNS sunucunuzun IP adresi muhtemelen bu değildir. Kendi değerlerinizi kullandığınızdan emin olun. Belirttiğiniz değer, P2S bağlantısı tarafından değil, VNet'e dağıttığınız kaynaklar tarafından kullanılır.

   ```azurepowershell-interactive
   New-AzVirtualNetwork -Name "VNet1" -ResourceGroupName "TestRG" -Location "East US" -AddressPrefix "192.168.0.0/16","10.254.0.0/16" -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
   ```
4. Bir VPN ağ geçidinin genel bir IP adresi olmalıdır. İlk olarak IP adresi kaynağını istemeniz, sonra sanal ağ geçidinizi oluştururken bu kaynağa başvurmanız gerekir. VPN ağ geçidi oluşturulurken, IP adresi kaynağa dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Statik bir Genel IP adresi ataması isteğinde bulunamazsınız. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

   Dinamik olarak atanmış genel bir IP adresi istemek için değişkenleri belirtin.

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name "VNet1" -ResourceGroupName "TestRG"  
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet 
   $pip = New-AzPublicIpAddress -Name "VNet1GWPIP" -ResourceGroupName "TestRG" -Location "East US" -AllocationMethod Dynamic 
   $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name "gwipconf" -Subnet $subnet -PublicIpAddress $pip
   ```

## <a name="3-set-up-your-radius-server"></a>3. <a name="radius"> </a>RADIUS sunucunuzu ayarlama

Sanal ağ ağ ağ geçidini oluşturmadan ve yapılandırmadan önce RADIUS sunucunuzun kimlik doğrulaması için doğru şekilde yapılandırılması gerekir.

1. Dağıtılmış bir RADIUS sunucunuz yoksa, bir tane dağıtın. Dağıtım adımları için RADIUS satıcınız tarafından sağlanan kurulum kılavuzuna bakın.  
2. VPN ağ geçidini RADIUS istemcisi olarak yapılandırın. Bu RADIUS istemcisini eklerken, oluşturduğunuz sanal ağ GatewaySubnet'i belirtin. 
3. RADIUS sunucusu kurulduktan sonra RADIUS sunucusunun IP adresini ve RADIUS istemcilerinin RADIUS sunucusuyla konuşmak için kullanması gereken paylaşılan sırrı alın. RADIUS sunucusu Azure VNet'teyse, RADIUS sunucusu VM'nin CA IP'sini kullanın.

[Ağ İlkesi Sunucusu (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) makalesi, AD etki alanı kimlik doğrulaması için bir Windows RADIUS sunucusunu (NPS) yapılandırma hakkında kılavuz sağlar.

## <a name="4-create-the-vpn-gateway"></a>4. <a name="creategw"> </a>VPN ağ geçidini oluşturun

VNet'iniz için VPN ağ geçidini yapılandırın ve oluşturun.

* -GatewayType 'Vpn' ve -VpnType 'RouteBased' olmalıdır.
* VPN ağ geçidinin tamamlanması, seçtiğiniz [SKU](vpn-gateway-about-vpn-gateway-settings.md#gwsku) ağ geçidine bağlı olarak tamamlanması 45 dakika kadar sürebilir.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1
```

## <a name="5-add-the-radius-server-and-client-address-pool"></a>5. <a name="addradius"> </a>RADIUS sunucu ve istemci adresi havuzu nu ekleyin
 
* -RadiusServer adıyla veya IP adresiyle belirtilebilir. Adı belirtirseniz ve sunucu şirket içinde bulunursa, VPN ağ geçidi adı çözümlemeyebilir. Bu durumda, sunucunun IP adresini belirtmek daha iyidir. 
* -RadiusSecret RADIUS sunucunuzda yapılandırılanla eşleşmelidir.
* -VpnClientAddressPool, bağlanan VPN istemcilerinin IP adresi aldığı aralıktır.Bağlantıyı kuracağınız şirket içi konum veya bağlanmak istediğiniz sanal ağ ile çakışmayan özel bir IP adresi aralığı kullanın. Yapılandırılan yeterince büyük bir adres havuzuna sahip olduğundan emin olun.  

1. RADIUS sırrı için güvenli bir dize oluşturun.

   ```azurepowershell-interactive
   $Secure_Secret=Read-Host -AsSecureString -Prompt "RadiusSecret"
   ```

2. RADIUS sırrına girmeniz istenir. Girdiğiniz karakterler görüntülenmez ve bunun yerine "*" karakteri değiştirilir.

   ```azurepowershell-interactive
   RadiusSecret:***
   ```
3. VPN istemci adres havuzunu ve RADIUS sunucu bilgilerini ekleyin.

   SSTP yapılandırmaları için:

    ```azurepowershell-interactive
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol "SSTP" `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

   OpenVPN® yapılandırmaları için:

    ```azurepowershell-interactive
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientRootCertificates @()
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol "OpenVPN" `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```


   IKEv2 yapılandırmaları için:

    ```azurepowershell-interactive
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol "IKEv2" `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

   SSTP + IKEv2 için

    ```azurepowershell-interactive
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol @( "SSTP", "IkeV2" ) `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

## <a name="6-download-the-vpn-client-configuration-package-and-set-up-the-vpn-client"></a>6. <a name="vpnclient"> </a>VPN istemcisi yapılandırma paketini indirin ve VPN istemcisini ayarlayın

VPN istemcisi yapılandırması, cihazların P2S bağlantısı üzerinden VNet'e bağlanmasını sağlar.VPN istemcisi yapılandırma paketi oluşturmak ve VPN istemcisini ayarlamak [için](point-to-site-vpn-client-configuration-radius.md)bkz.

## <a name="7-connect-to-azure"></a><a name="connect"></a>7. Azure'a bağlanın

### <a name="to-connect-from-a-windows-vpn-client"></a>Windows VPN istemcisinden bağlanmak için

1. İstemci bilgisayarda sanal ağınıza bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. Etki alanı kimlik bilgilerinizi girin ve 'Bağlan'ı tıklatın. Yükseltilmiş haklar isteyen bir açılır ileti görüntülenir. Kabul edin ve kimlik bilgilerini girin.

   ![VPN istemcisinin Azure’a bağlanması](./media/point-to-site-how-to-radius-ps/client.png)
2. Bağlantınız kurulur.

   ![Bağlantı kuruldu](./media/point-to-site-how-to-radius-ps/connected.png)

### <a name="connect-from-a-mac-vpn-client"></a>Mac VPN istemcisinden bağlanma

Ağ iletişim kutusunda kullanmak istediğiniz istemci profilini bulup **Bağlan**’a tıklayın.

  ![Mac bağlantısı](./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png)

## <a name="to-verify-your-connection"></a><a name="verify"></a>Bağlantınızı doğrulamak için

1. VPN bağlantınızın etkin olduğunu doğrulamak için, yükseltilmiş bir komut istemi açın ve *ipconfig/all* komutunu çalıştırın.
2. Sonuçlara bakın. Aldığınız IP adresinin, yapılandırmanızda belirttiğiniz Noktadan Siteye VPN İstemcisi Adres Havuzu'ndaki adreslerden biri olduğuna dikkat edin. Sonuçları şu örneğe benzer:

   ```
   PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
   ```

Bir P2S bağlantısını gidermek için, [Azure nokta-site bağlantılarıyla sorun giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)konusuna bakın.

## <a name="to-connect-to-a-virtual-machine"></a><a name="connectVM"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="faq"></a><a name="faq"></a>SSS

Bu SSS RADIUS kimlik doğrulaması kullanarak P2S için geçerlidir

[!INCLUDE [Point-to-Site RADIUS FAQ](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/). Ağ ve sanal makineler hakkında daha fazla bilgi edinmek için, bkz. [Azure ve Linux VM ağına genel bakış](../virtual-machines/linux/azure-vm-network-overview.md).
