---
title: 'Sanal ağ ağ geçidini silme: Azure klasik'
description: Klasik dağıtım modelinde PowerShell'i kullanarak sanal ağ ağ ağ geçidini silin.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: cherylmc
ms.openlocfilehash: e7283f5e28edc6f7beaad3a2743aa155f6ea6e14
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77198658"
---
# <a name="delete-a-virtual-network-gateway-using-powershell-classic"></a>PowerShell 'i kullanarak sanal ağ ağ ağ geçidini silme (klasik)

> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi - Azure portalı](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [Klasik - PowerShell](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>

Bu makale, PowerShell kullanarak klasik dağıtım modelinde bir VPN ağ geçidini silmenize yardımcı olur. Sanal ağ ağ geçidi silindikten sonra, artık kullanmadığınız öğeleri kaldırmak için ağ yapılandırma dosyasını değiştirin.

## <a name="step-1-connect-to-azure"></a><a name="connect"></a>Adım 1: Azure'a bağlanma

### <a name="1-install-the-latest-powershell-cmdlets"></a>1. En son PowerShell cmdlets yükleyin.

[!INCLUDE [vpn-gateway-classic-powershell](../../includes/vpn-gateway-powershell-classic-locally.md)]

### <a name="2-connect-to-your-azure-account"></a>2. Azure hesabınıza bağlanın.

PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

1. PowerShell konsolunuzu yüksek haklarla açın. Hizmet yönetimine geçmek için şu komutu kullanın:

   ```powershell
   azure config mode asm
   ```
2. Hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

   ```powershell
   Add-AzureAccount
   ```

## <a name="step-2-export-and-view-the-network-configuration-file"></a><a name="export"></a>Adım 2: Ağ yapılandırma dosyasını dışa aktarma ve görüntüleme

Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Bu dosyayı hem geçerli yapılandırma bilgilerini görüntülemek hem de ağ yapılandırmasını değiştirmek için kullanırsınız.

Bu örnekte, ağ yapılandırma dosyası C:\AzureNet dizinine aktarılır.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Dosyayı bir metin düzenleyicisi ile açın ve klasik VNet'inizin adını görüntüleyin. Azure portalında bir VNet oluşturduğunuzda, Azure'un kullandığı tam ad portalda görünmez. Örneğin, Azure portalında 'ClassicVNet1' olarak adlandırılmış gibi görünen bir VNet'in ağ yapılandırma dosyasında çok daha uzun bir adı olabilir. Adı gibi bir şey görünebilir: 'Grup ClassicRG1 ClassicVNet1'. Sanal ağ adları **'VirtualNetworkSite adı ='** olarak listelenir. PowerShell cmdlets'inizi çalıştırırken ağ yapılandırma dosyasındaki adları kullanın.

## <a name="step-3-delete-the-virtual-network-gateway"></a><a name="delete"></a>Adım 3: Sanal ağ ağ geçidini silme

Sanal ağ ağ ağ geçidini sildiğinizde, ağ geçidi üzerinden VNet'e olan tüm bağlantıların bağlantısı kesilir. VNet'e bağlı P2S istemciniz varsa, uyarı yapılmadan bağlantı kesilir.

Bu örnek, sanal ağ ağ ağ geçidini siler. Ağ yapılandırma dosyasından sanal ağın tam adını kullandığınızdan emin olun.

```powershell
Remove-AzureVNetGateway -VNetName "Group ClassicRG1 ClassicVNet1"
```

Başarılı olursa, dönüş gösterir:

```
Status : Successful
```

## <a name="step-4-modify-the-network-configuration-file"></a><a name="modify"></a>Adım 4: Ağ yapılandırma dosyasını değiştirme

Sanal ağ ağ ağ geçidini sildiğinizde, cmdlet ağ yapılandırma dosyasını değiştirmez. Artık kullanılmayan öğeleri kaldırmak için dosyayı değiştirmeniz gerekir. Aşağıdaki bölümler, karşıdan yüklediğiniz ağ yapılandırma dosyasını değiştirmenize yardımcı olur.

### <a name="local-network-site-references"></a><a name="lnsref"></a>Yerel Ağ Sitesi Referansları

Site başvuru bilgilerini kaldırmak için **ConnectionsToLocalNetwork/LocalNetworkSiteRef'te**yapılandırma değişiklikleri yapın. Yerel bir site başvurusu kaldırmak, bir tüneli silmek için Azure'u tetikler. Oluşturduğunuz yapılandırmaya bağlı olarak, bir **LocalNetworkSiteRef** listelenmemiş olabilir.

```
<Gateway>
   <ConnectionsToLocalNetwork>
     <LocalNetworkSiteRef name="D1BFC9CB_Site2">
       <Connection type="IPsec" />
     </LocalNetworkSiteRef>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

Örnek:

```
<Gateway>
   <ConnectionsToLocalNetwork>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

### <a name="local-network-sites"></a><a name="lns"></a>Yerel Ağ Siteleri

Artık kullanmadığınız yerel siteleri kaldırın. Oluşturduğunuz yapılandırmaya bağlı olarak, listelenen bir **LocalNetworkSite'niz** yok olabilir.

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
  <LocalNetworkSite name="Site3">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>57.179.18.164</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

Bu örnekte, yalnızca Site3 kaldırıldı.

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

### <a name="client-addresspool"></a><a name="clientaddresss"></a>Müşteri Adresi Havuzu

VNet'inize P2S bağlantınız varsa **VPNClientAddressPool'unuz**olacaktır. Sildiğiniz sanal ağ ağ ağ geçidine karşılık gelen istemci adresi havuzlarını kaldırın.

```
<Gateway>
    <VPNClientAddressPool>
      <AddressPrefix>10.1.0.0/24</AddressPrefix>
    </VPNClientAddressPool>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

Örnek:

```
<Gateway>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

### <a name="gatewaysubnet"></a><a name="gwsub"></a>GatewaySubnet

VNet'e karşılık gelen **GatewaySubnet'i** silin.

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
   <Subnet name="GatewaySubnet">
     <AddressPrefix>10.11.1.0/29</AddressPrefix>
   </Subnet>
 </Subnets>
```

Örnek:

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
 </Subnets>
```

## <a name="step-5-upload-the-network-configuration-file"></a><a name="upload"></a>Adım 5: Ağ yapılandırma dosyasını yükleme

Değişikliklerinizi kaydedin ve ağ yapılandırma dosyasını Azure'a yükleyin. Ortamınız için gerekli olan dosya yolunu değiştirdiğinizden emin olun.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Başarılı olursa, dönüş bu örneğe benzer bir şey gösterir:

```
OperationDescription        OperationId                      OperationStatus                                                
--------------------        -----------                      ---------------                                           
Set-AzureVNetConfig         e0ee6e66-9167-cfa7-a746-7casb9   Succeeded
```
