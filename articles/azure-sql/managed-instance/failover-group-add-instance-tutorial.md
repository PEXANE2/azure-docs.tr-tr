---
title: 'Öğretici: yük devretme grubuna SQL yönetilen örneği ekleme'
titleSuffix: Azure SQL Managed Instance
description: Bu öğreticide, birincil ve ikincil Azure SQL yönetilen örneği arasında bir yük devretme grubu oluşturmayı öğrenin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: high-availability
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sashan, carlrab
ms.date: 08/27/2019
ms.openlocfilehash: 47f33d8b1a7792487491cbe7f2ddb5c7f5b087af
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88002995"
---
# <a name="tutorial-add-sql-managed-instance-to-a-failover-group"></a>Öğretici: yük devretme grubuna SQL yönetilen örneği ekleme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneği 'nin yönetilen örneklerini bir yük devretme grubuna ekleyin. Bu makalede şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> - Birincil yönetilen örnek oluşturun.
> - Bir [Yük devretme grubunun](../database/auto-failover-group-overview.md)parçası olarak ikincil bir yönetilen örnek oluşturun. 
> - Yük devretme testi.

  > [!NOTE]
  > - Bu öğreticide, kaynaklarınızı [SQL yönetilen örneği için yük devretme grupları ayarlamaya yönelik önkoşullara](../database/auto-failover-group-overview.md#enabling-geo-replication-between-managed-instances-and-their-vnets)göre yapılandırırken emin olun. 
  > - Yönetilen bir örnek oluşturmak, önemli miktarda zaman alabilir. Sonuç olarak, Bu öğreticinin tamamlanması birkaç saat sürebilir. Sağlama süreleri hakkında daha fazla bilgi için bkz. [SQL yönetilen örnek yönetimi işlemleri](sql-managed-instance-paas-overview.md#management-operations). 
  > - Yük devretme grubuna katılan yönetilen örnekler için [Azure ExpressRoute](../../expressroute/expressroute-howto-circuit-portal-resource-manager.md) veya ıkı bağlı VPN ağ geçidi gerekir. Küresel VNet eşlemesi desteklenmiyor. Bu öğretici, VPN ağ geçitleri oluşturmak ve bağlamak için gereken adımları sağlar. ExpressRoute zaten yapılandırılmışsa bu adımları atlayın. 


## <a name="prerequisites"></a>Önkoşullar

# <a name="portal"></a>[Portal](#tab/azure-portal)
Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun: 

- Azure aboneliği. Henüz bir [hesabınız yoksa ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Öğreticiyi tamamlayabilmeniz için aşağıdaki öğelerin bulunduğundan emin olun:

- Azure aboneliği. Henüz bir [hesabınız yoksa ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .
- [Azure PowerShell](/powershell/azure/)

---


## <a name="1---create-a-resource-group-and-primary-managed-instance"></a>1-kaynak grubu ve birincil yönetilen örnek oluşturma

Bu adımda, Azure portal veya PowerShell 'i kullanarak yük devretme grubunuz için kaynak grubunu ve birincil yönetilen örneği oluşturacaksınız. 


# <a name="portal"></a>[Portal](#tab/azure-portal) 

Azure portal kullanarak kaynak grubunu ve birincil yönetilen örneğinizi oluşturun. 

1. Azure portal sol taraftaki menüden **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve `Azure SQL` Arama kutusuna yazın. Seçim **Azure SQL** ' in yanındaki yıldızı seçerek bunu sık kullanılanlara ekleyin ve sol gezinti bölmesinde bir öğe olarak ekleyin. 
1. **+ Ekle** ' yı seçerek **SQL dağıtım seçeneğini seçin** sayfasını açın. **Veritabanları** kutucuğunda **Ayrıntıları göster** ' i seçerek farklı veritabanları hakkındaki ek bilgileri görüntüleyebilirsiniz.
1. **SQL yönetilen örnekler** kutucuğunda **Oluştur** ' u seçin. 

    ![SQL yönetilen örneği Seç](./media/failover-group-add-instance-tutorial/select-managed-instance.png)

1. **Azure SQL yönetilen örneği oluştur** sayfasında, **temel bilgiler** sekmesinde:
    1. **Proje ayrıntıları**' nın altında, açılır listeden **aboneliğinizi** seçin ve ardından yeni kaynak grubu **oluşturmayı** seçin. Kaynak grubunuz için gibi bir ad yazın `myResourceGroup` . 
    1. **SQL yönetilen örnek ayrıntıları**' nın altında, yönetilen örneğinizin adını ve yönetilen örneğinizi dağıtmak istediğiniz bölgeyi girin. **İşlem + depolama alanını** varsayılan değerlerle bırakın. 
    1. **Yönetici hesabı**altında, gibi bir yönetici oturum açma `azureuser` ve karmaşık yönetici parolası sağlayın. 

    ![Birincil yönetilen örnek oluştur](./media/failover-group-add-instance-tutorial/primary-sql-mi-values.png)

1. Ayarları varsayılan değerlerinde bırakın ve SQL yönetilen örnek ayarlarınızı gözden geçirmek için **gözden geçir + oluştur** ' u seçin. 
1. Birincil yönetilen örneğinizi oluşturmak için **Oluştur** ' u seçin. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell kullanarak kaynak grubunuzu ve birincil yönetilen örneği oluşturun. 

   ```powershell-interactive
   # Connect-AzAccount
   # The SubscriptionId in which to create these objects
   $SubscriptionId = '<Subscription-ID>'
   # Create a random identifier to use as subscript for the different resource names
   $randomIdentifier = $(Get-Random)
   # Set the resource group name and location for SQL Managed Instance
   $resourceGroupName = "myResourceGroup-$randomIdentifier"
   $location = "eastus"
   $drLocation = "eastus2"
   
   # Set the networking values for your primary managed instance
   $primaryVNet = "primaryVNet-$randomIdentifier"
   $primaryAddressPrefix = "10.0.0.0/16"
   $primaryDefaultSubnet = "primaryDefaultSubnet-$randomIdentifier"
   $primaryDefaultSubnetAddress = "10.0.0.0/24"
   $primaryMiSubnetName = "primaryMISubnet-$randomIdentifier"
   $primaryMiSubnetAddress = "10.0.0.0/24"
   $primaryMiGwSubnetAddress = "10.0.255.0/27"
   $primaryGWName = "primaryGateway-$randomIdentifier"
   $primaryGWPublicIPAddress = $primaryGWName + "-ip"
   $primaryGWIPConfig = $primaryGWName + "-ipc"
   $primaryGWAsn = 61000
   $primaryGWConnection = $primaryGWName + "-connection"
   
   
   # Set the networking values for your secondary managed instance
   $secondaryVNet = "secondaryVNet-$randomIdentifier"
   $secondaryAddressPrefix = "10.128.0.0/16"
   $secondaryDefaultSubnet = "secondaryDefaultSubnet-$randomIdentifier"
   $secondaryDefaultSubnetAddress = "10.128.0.0/24"
   $secondaryMiSubnetName = "secondaryMISubnet-$randomIdentifier"
   $secondaryMiSubnetAddress = "10.128.0.0/24"
   $secondaryMiGwSubnetAddress = "10.128.255.0/27"
   $secondaryGWName = "secondaryGateway-$randomIdentifier"
   $secondaryGWPublicIPAddress = $secondaryGWName + "-IP"
   $secondaryGWIPConfig = $secondaryGWName + "-ipc"
   $secondaryGWAsn = 62000
   $secondaryGWConnection = $secondaryGWName + "-connection"
   
   
   
   # Set the SQL Managed Instance name for the new managed instances
   $primaryInstance = "primary-mi-$randomIdentifier"
   $secondaryInstance = "secondary-mi-$randomIdentifier"
   
   # Set the admin login and password for SQL Managed Instance
   $secpasswd = "PWD27!"+(New-Guid).Guid | ConvertTo-SecureString -AsPlainText -Force
   $mycreds = New-Object System.Management.Automation.PSCredential ("azureuser", $secpasswd)
   
   
   # Set the SQL Managed Instance service tier, compute level, and license mode
   $edition = "General Purpose"
   $vCores = 8
   $maxStorage = 256
   $computeGeneration = "Gen5"
   $license = "LicenseIncluded" #"BasePrice" or LicenseIncluded if you have don't have SQL Server license that can be used for AHB discount
   
   # Set failover group details
   $vpnSharedKey = "mi1mi2psk"
   $failoverGroupName = "failovergroup-$randomIdentifier"
   
   # Show randomized variables
   Write-host "Resource group name is" $resourceGroupName
   Write-host "Password is" $secpasswd
   Write-host "Primary Virtual Network name is" $primaryVNet
   Write-host "Primary default subnet name is" $primaryDefaultSubnet
   Write-host "Primary SQL Managed Instance subnet name is" $primaryMiSubnetName
   Write-host "Secondary Virtual Network name is" $secondaryVNet
   Write-host "Secondary default subnet name is" $secondaryDefaultSubnet
   Write-host "Secondary SQL Managed Instance subnet name is" $secondaryMiSubnetName
   Write-host "Primary SQL Managed Instance name is" $primaryInstance
   Write-host "Secondary SQL Managed Instance name is" $secondaryInstance
   Write-host "Failover group name is" $failoverGroupName
   
   # Suppress networking breaking changes warning (https://aka.ms/azps-changewarnings
   Set-Item Env:\SuppressAzurePowerShellBreakingChangeWarnings "true"
   
   # Set the subscription context
   Set-AzContext -SubscriptionId $subscriptionId 
   
   # Create the resource group
   Write-host "Creating resource group..."
   $resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location -Tag @{Owner="SQLDB-Samples"}
   $resourceGroup
   
   # Configure the primary virtual network
   Write-host "Creating primary virtual network..."
   $primaryVirtualNetwork = New-AzVirtualNetwork `
                         -ResourceGroupName $resourceGroupName `
                         -Location $location `
                         -Name $primaryVNet `
                         -AddressPrefix $primaryAddressPrefix
   
                     Add-AzVirtualNetworkSubnetConfig `
                         -Name $primaryMiSubnetName `
                         -VirtualNetwork $primaryVirtualNetwork `
                         -AddressPrefix $PrimaryMiSubnetAddress `
                     | Set-AzVirtualNetwork
   $primaryVirtualNetwork
   
   
   # Configure the primary managed instance subnet
   Write-host "Configuring primary MI subnet..."
   $primaryVirtualNetwork = Get-AzVirtualNetwork -Name $primaryVNet -ResourceGroupName $resourceGroupName
   
   
   $primaryMiSubnetConfig = Get-AzVirtualNetworkSubnetConfig `
                           -Name $primaryMiSubnetName `
                           -VirtualNetwork $primaryVirtualNetwork
   $primaryMiSubnetConfig
   
   # Configure the network security group management service
   Write-host "Configuring primary MI subnet..."
   
   $primaryMiSubnetConfigId = $primaryMiSubnetConfig.Id
   
   $primaryNSGMiManagementService = New-AzNetworkSecurityGroup `
                         -Name 'primaryNSGMiManagementService' `
                         -ResourceGroupName $resourceGroupName `
                         -location $location
   $primaryNSGMiManagementService
   
   # Configure the route table management service
   Write-host "Configuring primary MI route table management service..."
   
   $primaryRouteTableMiManagementService = New-AzRouteTable `
                         -Name 'primaryRouteTableMiManagementService' `
                         -ResourceGroupName $resourceGroupName `
                         -location $location
   $primaryRouteTableMiManagementService
   
   # Configure the primary network security group
   Write-host "Configuring primary network security group..."
   Set-AzVirtualNetworkSubnetConfig `
                         -VirtualNetwork $primaryVirtualNetwork `
                         -Name $primaryMiSubnetName `
                         -AddressPrefix $PrimaryMiSubnetAddress `
                         -NetworkSecurityGroup $primaryNSGMiManagementService `
                         -RouteTable $primaryRouteTableMiManagementService | `
                       Set-AzVirtualNetwork
   
   Get-AzNetworkSecurityGroup `
                         -ResourceGroupName $resourceGroupName `
                         -Name "primaryNSGMiManagementService" `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 100 `
                         -Name "allow_management_inbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix * `
                         -DestinationPortRange 9000,9003,1438,1440,1452 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 200 `
                         -Name "allow_misubnet_inbound" `
                         -Access Allow `
                         -Protocol * `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix $PrimaryMiSubnetAddress `
                         -DestinationPortRange * `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 300 `
                         -Name "allow_health_probe_inbound" `
                         -Access Allow `
                         -Protocol * `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix AzureLoadBalancer `
                         -DestinationPortRange * `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 1000 `
                         -Name "allow_tds_inbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix VirtualNetwork `
                         -DestinationPortRange 1433 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 1100 `
                         -Name "allow_redirect_inbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix VirtualNetwork `
                         -DestinationPortRange 11000-11999 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 1200 `
                         -Name "allow_geodr_inbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix VirtualNetwork `
                         -DestinationPortRange 5022 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 4096 `
                         -Name "deny_all_inbound" `
                         -Access Deny `
                         -Protocol * `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix * `
                         -DestinationPortRange * `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 100 `
                         -Name "allow_management_outbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Outbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix * `
                         -DestinationPortRange 80,443,12000 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 200 `
                         -Name "allow_misubnet_outbound" `
                         -Access Allow `
                         -Protocol * `
                         -Direction Outbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix * `
                         -DestinationPortRange * `
                         -DestinationAddressPrefix $PrimaryMiSubnetAddress `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 1100 `
                         -Name "allow_redirect_outbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Outbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix VirtualNetwork `
                         -DestinationPortRange 11000-11999 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 1200 `
                         -Name "allow_geodr_outbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Outbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix VirtualNetwork `
                         -DestinationPortRange 5022 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 4096 `
                         -Name "deny_all_outbound" `
                         -Access Deny `
                         -Protocol * `
                         -Direction Outbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix * `
                         -DestinationPortRange * `
                         -DestinationAddressPrefix * `
                       | Set-AzNetworkSecurityGroup
   Write-host "Primary network security group configured successfully."
   
   
   Get-AzRouteTable `
                         -ResourceGroupName $resourceGroupName `
                         -Name "primaryRouteTableMiManagementService" `
                       | Add-AzRouteConfig `
                         -Name "primaryToMIManagementService" `
                         -AddressPrefix 0.0.0.0/0 `
                         -NextHopType Internet `
                       | Add-AzRouteConfig `
                         -Name "ToLocalClusterNode" `
                         -AddressPrefix $PrimaryMiSubnetAddress `
                         -NextHopType VnetLocal `
                       | Set-AzRouteTable
   Write-host "Primary network route table configured successfully."
   
   
   # Create the primary managed instance
   
   Write-host "Creating primary SQL Managed Instance..."
   Write-host "This will take some time, see https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations or more information."
   New-AzSqlInstance -Name $primaryInstance `
                         -ResourceGroupName $resourceGroupName `
                         -Location $location `
                         -SubnetId $primaryMiSubnetConfigId `
                         -AdministratorCredential $mycreds `
                         -StorageSizeInGB $maxStorage `
                         -VCore $vCores `
                         -Edition $edition `
                         -ComputeGeneration $computeGeneration `
                         -LicenseType $license
   Write-host "Primary SQL Managed Instance created successfully."
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Bir Azure Kaynak grubu oluşturur.  |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur.  |
| [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Bir sanal ağa alt ağ yapılandırması ekler. | 
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. | 
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Bir sanal ağ içindeki bir alt ağı alır. | 
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Bir ağ güvenlik grubu oluşturur. | 
| [New-AzRouteTable](/powershell/module/az.network/new-azroutetable) | Bir yol tablosu oluşturur. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | Bir sanal ağ için alt ağ yapılandırmasını güncelleştirir.  |
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Bir sanal ağı güncelleştirir.  |
| [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup) | Bir ağ güvenlik grubu alır. |
| [Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Ağ güvenlik grubuna ağ güvenlik kuralı yapılandırması ekler. |
| [Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup) | Bir ağ güvenlik grubunu güncelleştirir.  | 
| [Add-AzRouteConfig](/powershell/module/az.network/add-azrouteconfig) | Rota tablosuna bir yol ekler. |
| [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable) | Bir yol tablosunu güncelleştirir.  |
| [New-Azsqlınstance](/powershell/module/az.sql/new-azsqlinstance) | Yönetilen bir örnek oluşturur.  |

---

## <a name="2---create-secondary-virtual-network"></a>2-ikincil sanal ağ oluşturma

Yönetilen örneğinizi oluşturmak için Azure portal kullanıyorsanız, birincil ve ikincil yönetilen örnek alt ağının çakışan aralıklar olmadığından, sanal ağı ayrı olarak oluşturmanız gerekecektir. Yönetilen örneğinizi yapılandırmak için PowerShell kullanıyorsanız, adım 3 ' e atlayın. 

# <a name="portal"></a>[Portal](#tab/azure-portal) 

Birincil sanal ağınızın alt ağ aralığını doğrulamak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com), kaynak grubunuza gidin ve birincil örneğiniz için sanal ağı seçin.  
2. **Ayarlar** altında **alt ağlar** ' ı seçin ve **adres aralığını**aklınızda edin. İkincil yönetilen örnek için sanal ağın alt ağ adres aralığı bu ile çakışamaz. 


   ![Birincil alt ağ](./media/failover-group-add-instance-tutorial/verify-primary-subnet-range.png)

Bir sanal ağ oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com), **kaynak oluştur** ve *sanal ağ*ara ' yı seçin. 
1. Microsoft tarafından yayımlanan **sanal ağ** seçeneğini seçin ve ardından sonraki sayfada **Oluştur** ' u seçin. 
1. İkincil yönetilen örneğinizin sanal ağını yapılandırmak için gerekli alanları doldurun ve **Oluştur**' u seçin. 

   Aşağıdaki tabloda, ikincil sanal ağ için gereken değerler gösterilmektedir:

    | **Alan** | Değer |
    | --- | --- |
    | **Ad** |  İkincil yönetilen örnek tarafından kullanılacak sanal ağın adı (gibi) `vnet-sql-mi-secondary` . |
    | **Adres alanı** | Sanal ağınız için gibi adres alanı `10.128.0.0/16` . | 
    | **Abonelik** | Birincil yönetilen örneğinizin ve kaynak grubunuzun bulunduğu abonelik. |
    | **Bölge** | İkincil yönetilen örneğinizi dağıtacağınız konum. |
    | **Alt ağ** | Alt ağınızın adı. `default`Varsayılan olarak sizin için sağlanır. |
    | **Adres aralığı**| Alt ağınızın adres aralığı. Bunun gibi, birincil yönetilen örneğinizin sanal ağı tarafından kullanılan alt ağ adres aralığından farklı olması gerekir `10.128.0.0/24` .  |
    | &nbsp; | &nbsp; |

    ![İkincil sanal ağ değerleri](./media/failover-group-add-instance-tutorial/secondary-virtual-network.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Bu adım yalnızca SQL yönetilen örneği dağıtmak için Azure portal kullanıyorsanız gereklidir. PowerShell kullanıyorsanız, adım 3 ' e atlayın. 

---

## <a name="3---create-a-secondary-managed-instance"></a>3-ikincil bir yönetilen örnek oluşturma
Bu adımda, Azure portal bir ikincil yönetilen örnek oluşturacaksınız ve bu, iki yönetilen örnek arasında ağ iletişimini de yapılandıracaksınız. 

İkinci yönetilen örneğiniz şunları sağlamalıdır:
- Boş olmalıdır. 
- Birincil yönetilen örnekten farklı bir alt ağa ve IP aralığına sahip olmak. 

# <a name="portal"></a>[Portal](#tab/azure-portal) 

Azure portal kullanarak ikincil yönetilen örnek oluşturun. 

1. Azure portal sol taraftaki menüden **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve `Azure SQL` Arama kutusuna yazın. Seçim **Azure SQL** ' in yanındaki yıldızı seçerek bunu sık kullanılanlara ekleyin ve sol gezinti bölmesinde bir öğe olarak ekleyin. 
1. **+ Ekle** ' yı seçerek **SQL dağıtım seçeneğini seçin** sayfasını açın. **Veritabanları** kutucuğunda **Ayrıntıları göster** ' i seçerek farklı veritabanları hakkındaki ek bilgileri görüntüleyebilirsiniz.
1. **SQL yönetilen örnekler** kutucuğunda **Oluştur** ' u seçin. 

    ![SQL yönetilen örneği Seç](./media/failover-group-add-instance-tutorial/select-managed-instance.png)

1. **Azure SQL yönetilen örnek oluştur** sayfasının **temel bilgiler** sekmesinde, ikincil yönetilen örneğinizi yapılandırmak için gerekli alanları doldurun. 

   Aşağıdaki tabloda, ikincil yönetilen örnek için gereken değerler gösterilmektedir:
 
    | **Alan** | Değer |
    | --- | --- |
    | **Abonelik** |  Birincil yönetilen örneğinizin bulunduğu abonelik. |
    | **Kaynak grubu**| Birincil yönetilen örneğinizin bulunduğu kaynak grubu. |
    | **SQL yönetilen örnek adı** | Yeni ikincil yönetilen örneğinizin adı, örneğin `sql-mi-secondary` .  | 
    | **Bölge**| İkincil yönetilen örneğinizin konumu.  |
    | **SQL yönetilen örnek Yöneticisi oturum açma** | Yeni ikincil yönetilen örneğiniz için kullanmak istediğiniz oturum açma gibi `azureuser` . |
    | **Parola** | Yeni ikincil yönetilen örnek için yönetici oturumu tarafından kullanılacak karmaşık bir parola.  |
    | &nbsp; | &nbsp; |

1. **Ağ** sekmesinde, **sanal ağ**için, açılan listeden ikincil yönetilen örnek için oluşturduğunuz sanal ağı seçin.

   ![İkincil mı ağı](./media/failover-group-add-instance-tutorial/networking-settings-for-secondary-mi.png)

1. **Ek ayarlar** sekmesinde, **coğrafi çoğaltma**için, _Yük devretme Ikincili olarak kullanmak_için **Evet** ' i seçin. Açılan listeden birincil yönetilen örneği seçin. 
    
   Harmanlama ve saat diliminin, birincil yönetilen örnekle eşleştiğinden emin olun. Bu öğreticide oluşturulan birincil yönetilen örnek, `SQL_Latin1_General_CP1_CI_AS` harmanlama ve `(UTC) Coordinated Universal Time` saat dilimi varsayılanını kullanır. 

   ![İkincil yönetilen örnek ağı](./media/failover-group-add-instance-tutorial/secondary-mi-failover.png)

1. İkincil yönetilen örneğinizin ayarlarını gözden geçirmek için **gözden geçir + oluştur** ' u seçin. 
1. İkincil yönetilen örneğinizi oluşturmak için **Oluştur** ' u seçin. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell kullanarak ikincil yönetilen örnek oluşturun. 

   ```powershell-interactive
   # Configure the secondary virtual network
   Write-host "Configuring secondary virtual network..."
   
   $SecondaryVirtualNetwork = New-AzVirtualNetwork `
                         -ResourceGroupName $resourceGroupName `
                         -Location $drlocation `
                         -Name $secondaryVNet `
                         -AddressPrefix $secondaryAddressPrefix
   
   Add-AzVirtualNetworkSubnetConfig `
                         -Name $secondaryMiSubnetName `
                         -VirtualNetwork $SecondaryVirtualNetwork `
                         -AddressPrefix $secondaryMiSubnetAddress `
                       | Set-AzVirtualNetwork
   $SecondaryVirtualNetwork
   
   # Configure the secondary managed instance subnet
   Write-host "Configuring secondary MI subnet..."
   
   $SecondaryVirtualNetwork = Get-AzVirtualNetwork -Name $secondaryVNet `
                                   -ResourceGroupName $resourceGroupName
   
   $secondaryMiSubnetConfig = Get-AzVirtualNetworkSubnetConfig `
                           -Name $secondaryMiSubnetName `
                           -VirtualNetwork $SecondaryVirtualNetwork
   $secondaryMiSubnetConfig
   
   # Configure the secondary network security group management service
   Write-host "Configuring secondary network security group management service..."
   
   $secondaryMiSubnetConfigId = $secondaryMiSubnetConfig.Id
   
   $secondaryNSGMiManagementService = New-AzNetworkSecurityGroup `
                         -Name 'secondaryToMIManagementService' `
                         -ResourceGroupName $resourceGroupName `
                         -location $drlocation
   $secondaryNSGMiManagementService
   
   # Configure the secondary route table MI management service
   Write-host "Configuring secondary route table MI management service..."
   
   $secondaryRouteTableMiManagementService = New-AzRouteTable `
                         -Name 'secondaryRouteTableMiManagementService' `
                         -ResourceGroupName $resourceGroupName `
                         -location $drlocation
   $secondaryRouteTableMiManagementService
   
   # Configure the secondary network security group
   Write-host "Configuring secondary network security group..."
   
   Set-AzVirtualNetworkSubnetConfig `
                         -VirtualNetwork $SecondaryVirtualNetwork `
                         -Name $secondaryMiSubnetName `
                         -AddressPrefix $secondaryMiSubnetAddress `
                         -NetworkSecurityGroup $secondaryNSGMiManagementService `
                         -RouteTable $secondaryRouteTableMiManagementService `
                       | Set-AzVirtualNetwork
   
   Get-AzNetworkSecurityGroup `
                         -ResourceGroupName $resourceGroupName `
                         -Name "secondaryToMIManagementService" `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 100 `
                         -Name "allow_management_inbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix * `
                         -DestinationPortRange 9000,9003,1438,1440,1452 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 200 `
                         -Name "allow_misubnet_inbound" `
                         -Access Allow `
                         -Protocol * `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix $secondaryMiSubnetAddress `
                         -DestinationPortRange * `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 300 `
                         -Name "allow_health_probe_inbound" `
                         -Access Allow `
                         -Protocol * `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix AzureLoadBalancer `
                         -DestinationPortRange * `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 1000 `
                         -Name "allow_tds_inbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix VirtualNetwork `
                         -DestinationPortRange 1433 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 1100 `
                         -Name "allow_redirect_inbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix VirtualNetwork `
                         -DestinationPortRange 11000-11999 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 1200 `
                         -Name "allow_geodr_inbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix VirtualNetwork `
                         -DestinationPortRange 5022 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 4096 `
                         -Name "deny_all_inbound" `
                         -Access Deny `
                         -Protocol * `
                         -Direction Inbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix * `
                         -DestinationPortRange * `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 100 `
                         -Name "allow_management_outbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Outbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix * `
                         -DestinationPortRange 80,443,12000 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 200 `
                         -Name "allow_misubnet_outbound" `
                         -Access Allow `
                         -Protocol * `
                         -Direction Outbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix * `
                         -DestinationPortRange * `
                         -DestinationAddressPrefix $secondaryMiSubnetAddress `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 1100 `
                         -Name "allow_redirect_outbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Outbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix VirtualNetwork `
                         -DestinationPortRange 11000-11999 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 1200 `
                         -Name "allow_geodr_outbound" `
                         -Access Allow `
                         -Protocol Tcp `
                         -Direction Outbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix VirtualNetwork `
                         -DestinationPortRange 5022 `
                         -DestinationAddressPrefix * `
                       | Add-AzNetworkSecurityRuleConfig `
                         -Priority 4096 `
                         -Name "deny_all_outbound" `
                         -Access Deny `
                         -Protocol * `
                         -Direction Outbound `
                         -SourcePortRange * `
                         -SourceAddressPrefix * `
                         -DestinationPortRange * `
                         -DestinationAddressPrefix * `
                       | Set-AzNetworkSecurityGroup
   
   
   Get-AzRouteTable `
                         -ResourceGroupName $resourceGroupName `
                         -Name "secondaryRouteTableMiManagementService" `
                       | Add-AzRouteConfig `
                         -Name "secondaryToMIManagementService" `
                         -AddressPrefix 0.0.0.0/0 `
                         -NextHopType Internet `
                       | Add-AzRouteConfig `
                         -Name "ToLocalClusterNode" `
                         -AddressPrefix $secondaryMiSubnetAddress `
                         -NextHopType VnetLocal `
                       | Set-AzRouteTable
   Write-host "Secondary network security group configured successfully."
   
   # Create the secondary managed instance
   
   $primaryManagedInstanceId = Get-AzSqlInstance -Name $primaryInstance -ResourceGroupName $resourceGroupName | Select-Object Id
   
   
   Write-host "Creating secondary SQL Managed Instance..."
   Write-host "This will take some time, see https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations or more information."
   New-AzSqlInstance -Name $secondaryInstance `
                     -ResourceGroupName $resourceGroupName `
                     -Location $drLocation `
                     -SubnetId $secondaryMiSubnetConfigId `
                     -AdministratorCredential $mycreds `
                     -StorageSizeInGB $maxStorage `
                     -VCore $vCores `
                     -Edition $edition `
                     -ComputeGeneration $computeGeneration `
                     -LicenseType $license `
                     -DnsZonePartner $primaryManagedInstanceId.Id
   Write-host "Secondary SQL Managed Instance created successfully."
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Bir Azure Kaynak grubu oluşturur.  |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur.  |
| [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Bir sanal ağa alt ağ yapılandırması ekler. | 
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. | 
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Bir sanal ağ içindeki bir alt ağı alır. | 
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Bir ağ güvenlik grubu oluşturur. | 
| [New-AzRouteTable](/powershell/module/az.network/new-azroutetable) | Bir yol tablosu oluşturur. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | Bir sanal ağ için alt ağ yapılandırmasını güncelleştirir.  |
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Bir sanal ağı güncelleştirir.  |
| [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup) | Bir ağ güvenlik grubu alır. |
| [Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Ağ güvenlik grubuna ağ güvenlik kuralı yapılandırması ekler. |
| [Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup) | Bir ağ güvenlik grubunu güncelleştirir.  | 
| [Add-AzRouteConfig](/powershell/module/az.network/add-azrouteconfig) | Rota tablosuna bir yol ekler. |
| [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable) | Bir yol tablosunu güncelleştirir.  |
| [New-Azsqlınstance](/powershell/module/az.sql/new-azsqlinstance) | Yönetilen bir örnek oluşturur.  |

---

## <a name="4---create-a-primary-gateway"></a>4-birincil ağ geçidi oluşturma 

Yük devretme grubuna katılacak iki yönetilen örnek için, ağ iletişimine izin vermek üzere iki yönetilen örneğin sanal ağları arasında bir ExpressRoute ya da bir ağ geçidi olmalıdır. İki VPN ağ geçidini bağlamak yerine [ExpressRoute](../../expressroute/expressroute-howto-circuit-portal-resource-manager.md) 'u yapılandırmayı seçerseniz, [adım 7](#7---create-a-failover-group)' ye atlayın.  

Bu makalede, iki VPN ağ geçidini oluşturma ve bunları bağlama adımları sağlanır, ancak bunun yerine ExpressRoute 'u yapılandırdıysanız yük devretme grubunu oluşturmaya devam edebilirsiniz. 


# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak, birincil yönetilen örneğinizin sanal ağı için ağ geçidini oluşturun. 


1. [Azure Portal](https://portal.azure.com), kaynak grubunuza gidin ve birincil yönetilen örneğiniz için **sanal ağ** kaynağını seçin. 
1. **Ayarlar** altında **alt ağlar** ' ı seçin ve ardından yeni bir **ağ geçidi alt ağı**eklemeyi seçin. Varsayılan değerleri bırakın. 

   ![Birincil yönetilen örnek için ağ geçidi ekle](./media/failover-group-add-instance-tutorial/add-subnet-gateway-primary-vnet.png)

1. Alt ağ geçidi oluşturulduktan sonra sol gezinti bölmesinden **kaynak oluştur** ' u seçin ve `Virtual network gateway` Arama kutusuna yazın. **Microsoft**tarafından yayınlanan **sanal ağ geçidi** kaynağını seçin. 

   ![Yeni bir sanal ağ geçidi oluştur](./media/failover-group-add-instance-tutorial/create-virtual-network-gateway.png)

1. Birincil yönetilen örneğiniz için ağ geçidini yapılandırmak üzere gerekli alanları doldurun. 

   Aşağıdaki tabloda, birincil yönetilen örnek için ağ geçidi için gereken değerler gösterilmektedir:
 
    | **Alan** | Değer |
    | --- | --- |
    | **Abonelik** |  Birincil yönetilen örneğinizin bulunduğu abonelik. |
    | **Ad** | Sanal ağ geçidinizin adı, örneğin `primary-mi-gateway` . | 
    | **Bölge** | Birincil yönetilen örneğinizin bulunduğu bölge. |
    | **Ağ geçidi türü** | **VPN**' yi seçin. |
    | **VPN türü** | **Rota tabanlı**' ı seçin. |
    | **SKU**| Varsayılan bırakın `VpnGw1` . |
    | **Konum**| Birincil yönetilen örneğinizin ve birincil sanal ağınızın bulunduğu konum.   |
    | **Sanal ağ**| 2. bölümde oluşturulan sanal ağı seçin `vnet-sql-mi-primary` . |
    | **Genel IP adresi**| **Yeni oluştur**’u seçin. |
    | **Genel IP adresi adı**| IP adresiniz için gibi bir ad girin `primary-gateway-IP` . |
    | &nbsp; | &nbsp; |

1. Diğer değerleri varsayılan olarak bırakın ve sonra sanal ağ geçidinizin ayarlarını gözden geçirmek için **gözden geçir + oluştur** ' u seçin.

   ![Birincil ağ geçidi ayarları](./media/failover-group-add-instance-tutorial/settings-for-primary-gateway.png)

1. Yeni sanal ağ geçidinizi oluşturmak için **Oluştur** ' u seçin. 


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell kullanarak birincil yönetilen örneğinizin sanal ağı için ağ geçidini oluşturun. 

   ```powershell-interactive
   # Create the primary gateway
   Write-host "Adding GatewaySubnet to primary VNet..."
   Get-AzVirtualNetwork `
                     -Name $primaryVNet `
                     -ResourceGroupName $resourceGroupName `
                   | Add-AzVirtualNetworkSubnetConfig `
                     -Name "GatewaySubnet" `
                     -AddressPrefix $primaryMiGwSubnetAddress `
                   | Set-AzVirtualNetwork
   
   $primaryVirtualNetwork  = Get-AzVirtualNetwork `
                     -Name $primaryVNet `
                     -ResourceGroupName $resourceGroupName
   $primaryGatewaySubnet = Get-AzVirtualNetworkSubnetConfig `
                     -Name "GatewaySubnet" `
                     -VirtualNetwork $primaryVirtualNetwork
   
   Write-host "Creating primary gateway..."
   Write-host "This will take some time."
   $primaryGWPublicIP = New-AzPublicIpAddress -Name $primaryGWPublicIPAddress -ResourceGroupName $resourceGroupName `
            -Location $location -AllocationMethod Dynamic
   $primaryGatewayIPConfig = New-AzVirtualNetworkGatewayIpConfig -Name $primaryGWIPConfig `
            -Subnet $primaryGatewaySubnet -PublicIpAddress $primaryGWPublicIP
   
   $primaryGateway = New-AzVirtualNetworkGateway -Name $primaryGWName -ResourceGroupName $resourceGroupName `
       -Location $location -IpConfigurations $primaryGatewayIPConfig -GatewayType Vpn `
       -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $true -Asn $primaryGWAsn
   $primaryGateway
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. |
| [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Bir sanal ağa alt ağ yapılandırması ekler. | 
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Bir sanal ağı güncelleştirir.  |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Bir sanal ağ içindeki bir alt ağı alır. |
| [New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur.  | 
| [New-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig) | Bir sanal ağ geçidi için IP yapılandırması oluşturur. |
| [New-AzVirtualNetworkGateway](/powershell/module/az.network/new-azvirtualnetworkgateway) | Bir sanal ağ geçidi oluşturur. |


---


## <a name="5---create-secondary-gateway"></a>5-ikincil ağ geçidi oluşturma 
Bu adımda, Azure portal kullanarak ikincil yönetilen örneğinizin sanal ağı için ağ geçidini oluşturun. 


# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak, ikincil yönetilen örnek için sanal ağ alt ağını ve ağ geçidini oluşturmak üzere önceki bölümdeki adımları yineleyin. İkincil yönetilen örneğiniz için ağ geçidini yapılandırmak üzere gerekli alanları doldurun. 

   Aşağıdaki tabloda, ikincil yönetilen örnek için ağ geçidi için gereken değerler gösterilmektedir:

   | **Alan** | Değer |
   | --- | --- |
   | **Abonelik** |  İkincil yönetilen örneğinizin olduğu abonelik. |
   | **Ad** | Sanal ağ geçidinizin adı, örneğin `secondary-mi-gateway` . | 
   | **Bölge** | İkincil yönetilen örneğinizin bulunduğu bölge. |
   | **Ağ geçidi türü** | **VPN**' yi seçin. |
   | **VPN türü** | **Rota tabanlı**' ı seçin. |
   | **SKU**| Varsayılan bırakın `VpnGw1` . |
   | **Konum**| İkincil yönetilen örneğinizin ve ikincil sanal ağınızın bulunduğu konum.   |
   | **Sanal ağ**| 2. bölümde oluşturulan sanal ağı seçin `vnet-sql-mi-secondary` . |
   | **Genel IP adresi**| **Yeni oluştur**’u seçin. |
   | **Genel IP adresi adı**| IP adresiniz için gibi bir ad girin `secondary-gateway-IP` . |
   | &nbsp; | &nbsp; |

   ![İkincil ağ geçidi ayarları](./media/failover-group-add-instance-tutorial/settings-for-secondary-gateway.png)


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell kullanarak ikincil yönetilen örneğin sanal ağı için ağ geçidini oluşturun. 

   ```powershell-interactive
   # Create the secondary gateway
   Write-host "Creating secondary gateway..."
   
   Write-host "Adding GatewaySubnet to secondary VNet..."
   Get-AzVirtualNetwork `
                     -Name $secondaryVNet `
                     -ResourceGroupName $resourceGroupName `
                   | Add-AzVirtualNetworkSubnetConfig `
                     -Name "GatewaySubnet" `
                     -AddressPrefix $secondaryMiGwSubnetAddress `
                   | Set-AzVirtualNetwork
   
   $secondaryVirtualNetwork  = Get-AzVirtualNetwork `
                     -Name $secondaryVNet `
                     -ResourceGroupName $resourceGroupName
   $secondaryGatewaySubnet = Get-AzVirtualNetworkSubnetConfig `
                     -Name "GatewaySubnet" `
                     -VirtualNetwork $secondaryVirtualNetwork
   $drLocation = $secondaryVirtualNetwork.Location
   
   Write-host "Creating primary gateway..."
   Write-host "This will take some time."
   $secondaryGWPublicIP = New-AzPublicIpAddress -Name $secondaryGWPublicIPAddress -ResourceGroupName $resourceGroupName `
            -Location $drLocation -AllocationMethod Dynamic
   $secondaryGatewayIPConfig = New-AzVirtualNetworkGatewayIpConfig -Name $secondaryGWIPConfig `
            -Subnet $secondaryGatewaySubnet -PublicIpAddress $secondaryGWPublicIP
   
   $secondaryGateway = New-AzVirtualNetworkGateway -Name $secondaryGWName -ResourceGroupName $resourceGroupName `
       -Location $drLocation -IpConfigurations $secondaryGatewayIPConfig -GatewayType Vpn `
       -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $true -Asn $secondaryGWAsn
   $secondaryGateway
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. |
| [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Bir sanal ağa alt ağ yapılandırması ekler. | 
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Bir sanal ağı güncelleştirir.  |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Bir sanal ağ içindeki bir alt ağı alır. |
| [New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur.  | 
| [New-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig) | Bir sanal ağ geçidi için IP yapılandırması oluşturur. |
| [New-AzVirtualNetworkGateway](/powershell/module/az.network/new-azvirtualnetworkgateway) | Bir sanal ağ geçidi oluşturur. |

---


## <a name="6---connect-the-gateways"></a>6-ağ geçitlerini bağlama
Bu adımda iki sanal ağın iki ağ geçidi arasında çift yönlü bir bağlantı oluşturun. 


# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak iki ağ geçidini bağlayın. 


1. Azure portal **kaynak oluştur** ' u seçin [Azure portal](https://portal.azure.com).
1. Arama `connection` kutusuna yazın ve ardından arama yapmak için ENTER tuşuna basın. Bu, sizi Microsoft tarafından yayımlanan **bağlantı** kaynağına götürür.
1. Bağlantınızı oluşturmak için **Oluştur** ' u seçin. 
1. **Temel bilgiler** sekmesinde, aşağıdaki değerleri seçip **Tamam**' ı seçin. 
    1. `VNet-to-VNet` **Bağlantı türü**için seçin. 
    1. Açılan listeden aboneliğinizi seçin. 
    1. Açılan kutuda SQL yönetilen örneği için kaynak grubunu seçin. 
    1. Açılan listeden birincil yönetilen örneğinizin konumunu seçin. 
1. **Ayarlar** sekmesinde, aşağıdaki değerleri seçin veya girin ve sonra **Tamam**' ı seçin:
    1. **İlk sanal ağ geçidi**için, gibi birincil ağ geçidini seçin `Primary-Gateway` .  
    1. **İkinci sanal ağ geçidi**için ikincil ağ geçidini (gibi) seçin `Secondary-Gateway` . 
    1. **Çift yönlü bağlantı oluştur**' un yanındaki onay kutusunu işaretleyin. 
    1. Varsayılan birincil bağlantı adını bırakın ya da seçtiğiniz bir değerle yeniden adlandırın. 
    1. Bağlantı için, gibi bir **paylaşılan anahtar (PSK)** sağlayın `mi1m2psk` . 

   ![Ağ Geçidi bağlantısı oluştur](./media/failover-group-add-instance-tutorial/create-gateway-connection.png)

1. **Özet** sekmesinde, çift yönlü bağlantınızın ayarlarını gözden geçirin ve ardından bağlantıyı oluşturmak için **Tamam** ' ı seçin. 


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell kullanarak iki ağ geçidini bağlayın. 

   ```powershell-interactive
   # Connect the primary to secondary gateway
   Write-host "Connecting the primary gateway to secondary gateway..."
   New-AzVirtualNetworkGatewayConnection -Name $primaryGWConnection -ResourceGroupName $resourceGroupName `
       -VirtualNetworkGateway1 $primaryGateway -VirtualNetworkGateway2 $secondaryGateway -Location $location `
       -ConnectionType Vnet2Vnet -SharedKey $vpnSharedKey -EnableBgp $true
   $primaryGWConnection
   
   # Connect the secondary to primary gateway
   Write-host "Connecting the secondary gateway to primary gateway..."
   
   New-AzVirtualNetworkGatewayConnection -Name $secondaryGWConnection -ResourceGroupName $resourceGroupName `
       -VirtualNetworkGateway1 $secondaryGateway -VirtualNetworkGateway2 $primaryGateway -Location $drLocation `
       -ConnectionType Vnet2Vnet -SharedKey $vpnSharedKey -EnableBgp $true
   $secondaryGWConnection
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'ini kullanır:

| Komut | Notlar |
|---|---|
| [New-AzVirtualNetworkGatewayConnection](/powershell/module/az.network/new-azvirtualnetworkgatewayconnection) | İki sanal ağ geçidi arasında bir bağlantı oluşturur.   |

---


## <a name="7---create-a-failover-group"></a>7-yük devretme grubu oluşturma
Bu adımda, yük devretme grubunu oluşturacak ve yönetilen örneklerin her ikisini de ekleyecek. 


# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portal kullanarak yük devretme grubunu oluşturun. 


1. [Azure Portal](https://portal.azure.com)sol taraftaki menüden **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve `Azure SQL` Arama kutusuna yazın. Seçim **Azure SQL** ' in yanındaki yıldızı seçerek bunu sık kullanılanlara ekleyin ve sol gezinti bölmesinde bir öğe olarak ekleyin. 
1. İlk bölümde oluşturduğunuz birincil yönetilen örneği (örneğin,) seçin `sql-mi-primary` . 
1. **Ayarlar**altında, örnek yük **devretme grupları** ' na gidin ve sonra **örnek yük devretme grubu** sayfasını açmak için **Grup Ekle** ' yi seçin. 

   ![Yük devretme grubu ekleme](./media/failover-group-add-instance-tutorial/add-failover-group.png)

1. **Örnek yük devretme grubu** sayfasında, gibi yük devretme grubunuzun adını yazın `failovergrouptutorial` . Sonra, açılan listeden, gibi ikincil yönetilen örneği seçin `sql-mi-secondary` . Yük devretme grubunuzu oluşturmak için **Oluştur** ' u seçin. 

   ![Yük devretme grubu oluştur](./media/failover-group-add-instance-tutorial/create-failover-group.png)

1. Yük devretme grubu dağıtımı tamamlandıktan sonra, **Yük devretme grubu** sayfasına geri yönlendirilirsiniz. 


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
PowerShell kullanarak yük devretme grubunu oluşturun. 

   ```powershell-interactive
   Write-host "Creating the failover group..."
   $failoverGroup = New-AzSqlDatabaseInstanceFailoverGroup -Name $failoverGroupName `
        -Location $location -ResourceGroupName $resourceGroupName -PrimaryManagedInstanceName $primaryInstance `
        -PartnerRegion $drLocation -PartnerManagedInstanceName $secondaryInstance `
        -FailoverPolicy Automatic -GracePeriodWithDataLossHours 1
   $failoverGroup
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'ini kullanır:

| Komut | Notlar |
|---|---|
| [New-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/new-azsqldatabaseinstancefailovergroup)| Yeni bir Azure SQL yönetilen örnek yük devretme grubu oluşturur.  |


---


## <a name="8---test-failover"></a>8-yük devretme testi
Bu adımda, yük devretme grubunuzu ikincil sunucuya devreder ve sonra Azure portal kullanarak yeniden başarısız olursunuz. 


# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portal kullanarak yük devretmeyi test edin. 


1. [Azure Portal](https://portal.azure.com) içinde _İkincil_ yönetilen örneğinize gidin ve ayarlar altında **örnek yük devretme grupları** ' nı seçin. 
1. Hangi yönetilen örnek birincil olduğunu ve hangi yönetilen örnek ikincil olduğunu gözden geçirin. 
1. **Yük devretme** ' yı seçin ve sonra kesilmekte olan tds oturumlarının uyarısında **Evet** ' i seçin. 

   ![Yük devretme grubu yükünü devreder](./media/failover-group-add-instance-tutorial/failover-mi-failover-group.png)

1. Hangi yönetilen örnek birincil olduğunu ve hangi yönetilen örnek ikincil olduğunu gözden geçirin. Yük devretme başarılı olursa, iki örnek anahtarlamalı rollere sahip olmalıdır. 

   ![Yönetilen örnekler, yük devretmeden sonra rolleri değiştirdi](./media/failover-group-add-instance-tutorial/mi-switched-after-failover.png)

1. Yeni _İkincil_ yönetilen örneğe gidin ve birincil örnek birincil role geri dönmek Için **Yük devretmeyi** bir kez daha seçin. 


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
PowerShell kullanarak yük devretmeyi test etme. 

   ```powershell-interactive
    
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
       -Location $location -Name $failoverGroupName
   
   # Fail over the primary managed instance to the secondary role
   Write-host "Failing primary over to the secondary location"
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
       -Location $drLocation -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
   Write-host "Successfully failed failover group to secondary location"
   ```


Yük devretme grubunu birincil sunucuya geri döndürür:

   ```powershell-interactive
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
       -Location $drLocation -Name $failoverGroupName
   
   # Fail the primary managed instance back to the primary role
   Write-host "Failing primary back to primary role"
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
       -Location $location -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
   Write-host "Successfully failed failover group to primary location"
   
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
       -Location $location -Name $failoverGroupName
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [Get-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/get-azsqldatabaseinstancefailovergroup) | SQL yönetilen örnek yük devretme gruplarını alır veya listeler.| 
| [Switch-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/switch-azsqldatabaseinstancefailovergroup) | SQL yönetilen örnek yük devretme grubunun yük devretmesini yürütür. | 

---



## <a name="clean-up-resources"></a>Kaynakları temizleme
Önce yönetilen örnekleri, ardından sanal kümeyi, ardından kalan kaynakları ve son olarak kaynak grubunu silerek kaynakları temizleyin. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
1. [Azure Portal](https://portal.azure.com)kaynak grubunuza gidin. 
1. Yönetilen örnekleri seçin ve ardından **Sil**' i seçin. `yes`Kaynağı silmek istediğinizi onaylamak için metin kutusunu yazın ve ardından **Sil**' i seçin. Bu işlemin tamamlanması biraz zaman alabilir ve tamamlanana kadar *sanal kümeyi* ya da başka herhangi bir bağımlı kaynağı silemeyeceksiniz. Yönetilen örneğinizin silindiğini onaylamak için **etkinlik** sekmesindeki silme işlemini izleyin. 
1. Yönetilen örnek silindikten sonra, *sanal kümeyi* kaynak grubunuzda seçip **Sil**' i seçerek silin. `yes`Kaynağı silmek istediğinizi onaylamak için metin kutusunu yazın ve ardından **Sil**' i seçin. 
1. Kalan kaynakları silin. `yes`Kaynağı silmek istediğinizi onaylamak için metin kutusunu yazın ve ardından **Sil**' i seçin. 
1. Kaynak grubunu Sil ' i seçerek kaynak **grubunu Sil ' i seçin,** kaynak grubunun adını yazın ve Sil ' i `myResourceGroup` seçin. **Delete** 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Kaynak grubunu iki kez kaldırmanız gerekecektir. Kaynak grubunun ilk kez kaldırılması yönetilen örnekleri ve sanal kümeleri kaldırır, ancak hata iletisiyle başarısız olur `Remove-AzResourceGroup : Long running operation failed with status 'Conflict'` . Kalan kaynakları ve kaynak grubunu kaldırmak için Remove-AzResourceGroup komutunu ikinci kez çalıştırın.

```powershell-interactive
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
Write-host "Removing SQL Managed Instance and virtual cluster..."
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
Write-host "Removing residual resources and resource group..."
```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'ini kullanır:

| Komut | Notlar |
|---|---|
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu kaldırır. |

---

## <a name="full-script"></a>Tam betik

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/failover-groups/add-managed-instance-to-failover-group-az-ps.ps1 "Add SQL Managed Instance to a failover group")]

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Bir Azure Kaynak grubu oluşturur.  |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur.  |
| [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Bir sanal ağa alt ağ yapılandırması ekler. | 
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. | 
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Bir sanal ağ içindeki bir alt ağı alır. | 
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Bir ağ güvenlik grubu oluşturur. | 
| [New-AzRouteTable](/powershell/module/az.network/new-azroutetable) | Bir yol tablosu oluşturur. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | Bir sanal ağ için alt ağ yapılandırmasını güncelleştirir.  |
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Bir sanal ağı güncelleştirir.  |
| [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup) | Bir ağ güvenlik grubu alır. |
| [Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Ağ güvenlik grubuna ağ güvenlik kuralı yapılandırması ekler. |
| [Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup) | Bir ağ güvenlik grubunu güncelleştirir.  | 
| [Add-AzRouteConfig](/powershell/module/az.network/add-azrouteconfig) | Rota tablosuna bir yol ekler. |
| [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable) | Bir yol tablosunu güncelleştirir.  |
| [New-Azsqlınstance](/powershell/module/az.sql/new-azsqlinstance) | Yönetilen bir örnek oluşturur.  |
| [Get-Azsqlınstance](/powershell/module/az.sql/get-azsqlinstance)| Azure SQL yönetilen örneği hakkında bilgi döndürür. |
| [New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur.  | 
| [New-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig) | Bir sanal ağ geçidi için IP yapılandırması oluşturur. |
| [New-AzVirtualNetworkGateway](/powershell/module/az.network/new-azvirtualnetworkgateway) | Bir sanal ağ geçidi oluşturur. |
| [New-AzVirtualNetworkGatewayConnection](/powershell/module/az.network/new-azvirtualnetworkgatewayconnection) | İki sanal ağ geçidi arasında bir bağlantı oluşturur.   |
| [New-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/new-azsqldatabaseinstancefailovergroup)| Yeni bir SQL yönetilen örnek yük devretme grubu oluşturur.  |
| [Get-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/get-azsqldatabaseinstancefailovergroup) | SQL yönetilen örnek yük devretme gruplarını alır veya listeler.| 
| [Switch-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/switch-azsqldatabaseinstancefailovergroup) | SQL yönetilen örnek yük devretme grubunun yük devretmesini yürütür. | 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu kaldırır. | 

# <a name="portal"></a>[Portal](#tab/azure-portal) 

Azure portal için kullanılabilir komut yok.

---

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, iki yönetilen örnek arasında bir yük devretme grubu yapılandırdınız. Şunları öğrendiniz:

> [!div class="checklist"]
> - Birincil yönetilen örnek oluşturun.
> - Bir [Yük devretme grubunun](../database/auto-failover-group-overview.md)parçası olarak ikincil bir yönetilen örnek oluşturun. 
> - Yük devretme testi.

SQL yönetilen örneğine bağlanma ve bir veritabanını SQL yönetilen örneğine geri yükleme hakkında sonraki hızlı başlangıca ilerleyin: 

> [!div class="nextstepaction"]
> [SQL yönetilen örneğine Bağlan](connect-vm-instance-configure.md) 
>  [VERITABANıNı SQL yönetilen örneğine geri yükleme](restore-sample-database-quickstart.md)


