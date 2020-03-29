---
title: Create an Azure Private Endpoint using Azure PowerShell| Microsoft Dokümanlar
description: Azure Özel Bağlantısı hakkında bilgi edinin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 60032677594537f1e7791b7108eebd5d4cfad5b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430334"
---
# <a name="create-a-private-endpoint-using-azure-powershell"></a>Azure PowerShell'i kullanarak özel bir bitiş noktası oluşturma
Özel Bitiş Noktası, Azure'daki özel bağlantının temel yapı taşıdır. Sanal Makineler (VM' ler) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasını sağlar. 

Bu Quickstart'ta, Azure PowerShell'i kullanarak Azure özel bitiş noktasına sahip bir SQL Veritabanı Sunucusu olan Azure Sanal Ağı'nda VM nasıl oluşturulacağı öğrenilir. Ardından, VM'den SQL Database Server'a güvenli bir şekilde erişebilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynaklarınızı oluşturmadan önce, Sanal Ağı ve Özel bitiş noktasını [Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile barındıran bir kaynak grubu oluşturmanız gerekir. Aşağıdaki örnek, *WestUS* konumunda *myResourceGroup* adında bir kaynak grubu oluşturur:

```azurepowershell

New-AzResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus
```

## <a name="create-a-virtual-network"></a>Sanal Ağ Oluşturma
Bu bölümde, bir sanal ağ ve bir alt ağ oluşturursunuz. Ardından, alt ağı Sanal Ağınızla ilişkilendirin.

### <a name="create-a-virtual-network"></a>Sanal Ağ Oluşturma

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile özel bitiş noktanız için sanal ağ oluşturun. Aşağıdaki örnek *MyVirtualNetwork*adlı bir Sanal Ağ oluşturur:
 
```azurepowershell

$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Alt ağ ekleme

Azure kaynakları Sanal Ağ içindeki bir alt ağa dağıtır, bu nedenle bir alt ağ oluşturmanız gerekir. [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig)ile *mySubnet* adlı bir alt net yapılandırması oluşturun. Aşağıdaki örnek, **devre dışı bırakılmış**özel uç nokta ağ ilkesi bayrağı ayarlanmış *mySubnet* adlı bir alt ağ oluşturur.

```azurepowershell
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 10.0.0.0/24 `
  -PrivateEndpointNetworkPoliciesFlag "Disabled" `
  -VirtualNetwork $virtualNetwork
```

> [!CAUTION]
> Parametreyi `PrivateEndpointNetworkPoliciesFlag` kullanılabilir başka bir bayrakla karıştırmak kolaydır, `PrivateLinkServiceNetworkPoliciesFlag`çünkü her ikisi de uzun sözcüklerdir ve benzer görünüme sahiptirler.  Doğru olanı kullandığınızdan emin `PrivateEndpointNetworkPoliciesFlag`olun.

### <a name="associate-the-subnet-to-the-virtual-network"></a>Subnet'i Sanal Ağla Ilişkilendir

[Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork)ile Sanal Ağa alt ağ yapılandırması yazabilirsiniz. Bu komut alt ağı oluşturur:

```azurepowershell
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="create-a-virtual-machine"></a>Sanal Makine Oluşturma

[New-AzVM](/powershell/module/az.compute/new-azvm)ile Sanal Ağ'da VM oluşturun. Bir sonraki komutu çalıştırdığınızda, kimlik bilgileri için istenirsiniz. VM için bir kullanıcı adı ve parola girin:

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

Seçenek `-AsJob` arka planda VM oluşturur. Bir sonraki adıma devam edebilirsiniz.

Azure arka planda VM oluşturmaya başladığında, böyle bir şeyi geri alırsınız:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

## <a name="create-a-sql-database-server"></a>SQL Veritabanı Sunucusu Oluşturma 

New-AzSqlServer komutunu kullanarak bir SQL Veritabanı Sunucusu oluşturun. SQL Veritabanı sunucunuzun adının Azure'da benzersiz olması gerektiğini unutmayın, bu nedenle parantez içinde yer tutucu değerini kendi benzersiz değerinizle değiştirin:

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

[Yeni-AzPrivateLinkServiceConnection](/powershell/module/az.network/New-AzPrivateLinkServiceConnection)ile Sanal Ağınızda SQL Veritabanı Sunucusu için Özel Bitiş Noktası: 

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
  -Subnet  $subnet`
  -PrivateLinkServiceConnection $privateEndpointConnection
``` 

## <a name="configure-the-private-dns-zone"></a>Özel DNS Bölgesini Yapılandırma 
SQL Database Server etki alanı için özel bir DNS bölgesi oluşturun ve sanal ağla bir ilişkilendirme bağlantısı oluşturun: 

```azurepowershell

$zone = New-AzPrivateDnsZone -ResourceGroupName "myResourceGroup" `
  -Name "privatelink.database.windows.net" 
 
$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName "myResourceGroup" `
  -ZoneName "privatelink.database.windows.net"`
  -Name "mylink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$networkInterface = Get-AzResource -ResourceId $privateEndpoint.NetworkInterfaces[0].Id -ApiVersion "2019-04-01" 
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName -RecordType A -ZoneName "privatelink.database.windows.net"  `
-ResourceGroupName "myResourceGroup" -Ttl 600 `
-PrivateDnsRecords (New-AzPrivateDnsRecordConfig -IPv4Address $ipconfig.properties.privateIPAddress)  
} 
} 
``` 
  
## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Bir VM'nin genel IP adresini döndürmek için [Get-AzPublicIpAddress'i](/powershell/module/az.network/Get-AzPublicIpAddress) kullanın. Bu *örnek, myVM VM'nin* genel IP adresini döndürür:

```azurepowershell
Get-AzPublicIpAddress `
  -Name myPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress 
```  
Yerel bilgisayarınızda bir komut istemi açın. MSTSC komutunu çalıştırın. Son <publicIpAddress> adımdan döndürülen genel IP adresiyle değiştirin: 


> [!NOTE]
> Bu komutları yerel bilgisayarınızdaki bir PowerShell komut isteminden çalıştırıyorsanız ve Az PowerShell modül sürüm 1.0 veya daha sonrakullanıyorsanız, bu arabirimde devam edebilirsiniz.
```
mstsc /v:<publicIpAddress>
```

1. İstendiğinde **Bağlan**’ı seçin. 
2. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.
  > [!NOTE]
  > VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için farklı bir hesap > daha fazla seçenek seçmeniz gerekebilir. 
  
3. **Tamam'ı**seçin. 
4. Bir sertifika uyarısı alabilirsiniz. Bunu **yaparsanız, Evet** veya **Devam'ı**seçin. 

## <a name="access-sql-database-server-privately-from-the-vm"></a>SQL Database Server'a VM'den özel olarak erişin

1. myVM'in Uzak Masaüstü'nde PowerShell'i açın.
2. `nslookup myserver.database.windows.net` yazın. 

    Buna benzer bir ileti alırsınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
    ```
3. SQL Server Management Studio'yı yükleyin
4. Sunucuya Bağlan'da şu bilgileri girin veya seçin: Değer Sunucusu türünü Ayarla Veritabanı Altyapısını Seçin.
      Sunucu adı Select myserver.database.windows.net Kullanıcı Adı Oluşturma sırasında sağlanan bir kullanıcı adı girin.
      Parola Oluşturma sırasında sağlanan bir parola girin.
      Parolayı hatırla Evet Seç.
5. Bağlan’ı seçin.
6. Sol menüden Veritabanlarına göz atın. 
7. (İsteğe bağlı olarak) Veritabanımdan bilgi oluşturma veya sorgulama
8. Uzak masaüstü bağlantısını *myVM'e*kapatın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 
Özel bitiş noktası, SQL Veritabanı sunucusu ve VM'yi kullandığınızda, kaynak grubunu ve sahip olduğu tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup) kullanın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Özel Bağlantı](private-link-overview.md) hakkında daha fazla bilgi edinin
