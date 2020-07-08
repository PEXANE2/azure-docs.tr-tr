---
title: Azure PowerShell kullanarak bir Azure özel uç noktası oluşturma | Microsoft Docs
description: Azure özel bağlantısı hakkında bilgi edinin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: how-to
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 0c6fc36be101679cea3a770f311005f63c3f0d66
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84737385"
---
# <a name="create-a-private-endpoint-using-azure-powershell"></a>Azure PowerShell kullanarak özel uç nokta oluşturma
Özel uç nokta, Azure 'da özel bağlantı için temel yapı taşdır. Sanal makineler (VM) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasına olanak sağlar. 

Bu hızlı başlangıçta, Azure PowerShell kullanarak Azure özel uç noktası olan bir Azure sanal ağında bir sanal makıne oluşturmayı öğreneceksiniz. Daha sonra, VM 'den SQL veritabanına güvenli bir şekilde erişebilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynaklarınızı oluşturabilmeniz için önce, [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)Ile sanal ağı ve özel uç noktayı barındıran bir kaynak grubu oluşturmanız gerekir. Aşağıdaki örnek *WestUS* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur:

```azurepowershell

New-AzResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus
```

## <a name="create-a-virtual-network"></a>Sanal Ağ Oluşturma
Bu bölümde, bir sanal ağ ve bir alt ağ oluşturacaksınız. Sonra, alt ağı sanal ağınızla ilişkilendirirsiniz.

### <a name="create-a-virtual-network"></a>Sanal Ağ Oluşturma

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile özel uç noktanız Için bir sanal ağ oluşturun. Aşağıdaki örnek, *MyVirtualNetwork*adlı bir sanal ağ oluşturur:
 
```azurepowershell

$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Alt ağ ekleme

Azure, kaynakları bir sanal ağ içindeki bir alt ağa dağıtır, bu nedenle bir alt ağ oluşturmanız gerekir. [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig)Ile *mysubnet* adlı bir alt ağ yapılandırması oluşturun. Aşağıdaki örnek, Özel uç nokta ağ ilkesi bayrağı **devre dışı**olarak ayarlanan *mysubnet* adlı bir alt ağ oluşturur.

```azurepowershell
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 10.0.0.0/24 `
  -PrivateEndpointNetworkPoliciesFlag "Disabled" `
  -VirtualNetwork $virtualNetwork
```

> [!CAUTION]
> `PrivateEndpointNetworkPoliciesFlag`Parametreyi `PrivateLinkServiceNetworkPoliciesFlag` hem uzun hem de benzer görünüşler oldukları için, başka bir kullanılabilir bayrağıyla karıştırmayı kolay hale gelir.  Doğru olanı kullandığınızdan emin olun `PrivateEndpointNetworkPoliciesFlag` .

### <a name="associate-the-subnet-to-the-virtual-network"></a>Alt ağı sanal ağla ilişkilendir

Alt ağ yapılandırmasını [set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork)Ile sanal ağa yazabilirsiniz. Bu komut alt ağ oluşturur:

```azurepowershell
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[New-azvm](/powershell/module/az.compute/new-azvm)Ile sanal ağda bir VM oluşturun. Sonraki komutu çalıştırdığınızda kimlik bilgileri istenir. VM için bir Kullanıcı adı ve parola girin:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "westcentralus" `
    -VirtualNetworkName "MyVirtualNetwork" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80,3389 `
    -AsJob  
```

`-AsJob`Seçeneği, sanal makineyi arka planda oluşturur. Sonraki adıma devam edebilirsiniz.

Azure, arka planda VM oluşturmaya başladığında, bu geri doğru bir şey alacaksınız:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

## <a name="create-a-logical-sql-server"></a>Mantıksal SQL Server oluştur 

New-AzSqlServer komutunu kullanarak mantıksal bir SQL Server oluşturun. Sunucunuzun adının Azure genelinde benzersiz olması gerektiğini unutmayın, bu nedenle yer tutucu değerini köşeli ayraç içinde kendi benzersiz değer ile değiştirin:

```azurepowershell-interactive
$adminSqlLogin = "SqlAdmin"
$password = "ChangeYourAdminPassword1"

$server = New-AzSqlServer -ResourceGroupName "myResourceGroup" `
    -ServerName "myserver" `
    -Location "WestCentralUS" `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

New-AzSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName "myserver"`
    -DatabaseName "myda"`
    -RequestedServiceObjectiveName "S0" `
    -SampleName "AdventureWorksLT"
```

## <a name="create-a-private-endpoint"></a>Özel Uç Nokta oluşturma

[New-AzPrivateLinkServiceConnection](/powershell/module/az.network/New-AzPrivateLinkServiceConnection)Ile sanal ağınızdaki sunucu Için özel uç nokta: 

```azurepowershell

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnection" `
  -PrivateLinkServiceId $server.ResourceId `
  -GroupId "sqlServer" 
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  "myResourceGroup" -Name "MyVirtualNetwork"  
 
$subnet = $virtualNetwork `
  | Select -ExpandProperty subnets `
  | Where-Object  {$_.Name -eq 'mysubnet'}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName "myResourceGroup" `
  -Name "myPrivateEndpoint" `
  -Location "westcentralus" `
  -Subnet  $subnet `
  -PrivateLinkServiceConnection $privateEndpointConnection
``` 

## <a name="configure-the-private-dns-zone"></a>Özel DNS bölgesini yapılandırma 
SQL veritabanı etki alanı için bir Özel DNS bölgesi oluşturun, sanal ağla bir ilişki bağlantısı oluşturun ve özel uç noktasını Özel DNS bölgesi ile ilişkilendirmek için bir DNS bölge grubu oluşturun.

```azurepowershell

$zone = New-AzPrivateDnsZone -ResourceGroupName "myResourceGroup" `
  -Name "privatelink.database.windows.net" 
 
$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName "myResourceGroup" `
  -ZoneName "privatelink.database.windows.net"`
  -Name "mylink" `
  -VirtualNetworkId $virtualNetwork.Id  

$config = New-AzPrivateDnsZoneConfig -Name "privatelink.database.windows.net" -PrivateDnsZoneId $zone.ResourceId

$privateDnsZoneGroup = New-AzPrivateDnsZoneGroup -ResourceGroupName "myResourceGroup" `
 -PrivateEndpointName "myPrivateEndpoint" -name "MyZoneGroup" -PrivateDnsZoneConfig $config
``` 
  
## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Bir sanal makinenin genel IP adresini döndürmek için [Get-Azpublicıpaddress](/powershell/module/az.network/Get-AzPublicIpAddress) komutunu kullanın. Bu örnek, *Myvm* VM 'nın genel IP adresini döndürür:

```azurepowershell
Get-AzPublicIpAddress `
  -Name myPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress 
```  
Yerel bilgisayarınızda bir komut istemi açın. Mstsc komutunu çalıştırın. <publicIpAddress>Son adımdan döndürülen genel IP adresiyle değiştirin: 


> [!NOTE]
> Bu komutları yerel bilgisayarınızdaki bir PowerShell isteminden çalıştırıyorsanız ve az PowerShell modülü 1,0 veya sonraki bir sürümü kullanıyorsanız, bu arabirime devam edebilirsiniz.
```
mstsc /v:<publicIpAddress>
```

1. İstendiğinde **Bağlan**’ı seçin. 
2. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.
  > [!NOTE]
  > VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için farklı bir hesap kullanmak > daha fazla seçenek belirlemeniz gerekebilir. 
  
3. **Tamam**’ı seçin. 
4. Bir sertifika uyarısı alabilirsiniz. Bunu yaparsanız **Evet** ' i veya **devam et**' i seçin. 

## <a name="access-sql-database-privately-from-the-vm"></a>SQL veritabanına özel olarak VM 'den erişin

1. MyVM uzak masaüstünde PowerShell ' i açın.
2. `nslookup myserver.database.windows.net` yazın. `myserver`SQL Server adınızla değiştirmeyi unutmayın.

    Şuna benzer bir ileti alacaksınız:
    
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
    ```
    
3. [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)'i yükler.
4. **Sunucuya Bağlan**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | --- | --- |
    | Sunucu türü | Veritabanı Altyapısı |
    | Sunucu adı | myserver.database.windows.net |
    | Kullanıcı adı | Oluşturma sırasında belirtilen kullanıcı adını girin |
    | Parola | Oluşturma sırasında girilen parolayı girin |
    | Parolayı anımsa | Evet |
    
5. **Bağlan**'ı seçin.
6. Sol menüden **veritabanlarına** gözatamazsınız. 
7. I MyDatabase 'ten bilgi oluşturun veya sorgulayın.
8. *Myvm*ile uzak masaüstü bağlantısını kapatın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 
Özel uç nokta, SQL veritabanı ve VM 'yi kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure özel bağlantısı](private-link-overview.md) hakkında daha fazla bilgi edinin
