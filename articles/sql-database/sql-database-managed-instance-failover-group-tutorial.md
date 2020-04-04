---
title: 'Öğretici: Yönetilen bir örneği başarısız bir gruba ekleme'
description: Azure SQL Veritabanı yönetilen örneğiniz için bir başarısız grup yapılandırmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sashan, carlrab
manager: jroth
ms.date: 08/27/2019
ms.openlocfilehash: 0f1a56fa6ea38acd8061180407eb47fe416b61e9
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631709"
---
# <a name="tutorial-add-a-sql-database-managed-instance-to-a-failover-group"></a>Öğretici: Başarısız bir gruba SQL Veritabanı yönetilen örnek ekleme

Başarısız bir gruba SQL Veritabanı yönetilen bir örnek ekleyin. Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
> - Birincil yönetilen örnek oluşturma
> - Bir [başarısız lık grubunun](sql-database-auto-failover-group.md)parçası olarak ikincil yönetilen bir örnek oluşturun. 
> - Yük devretme testi

  > [!NOTE]
  > - Bu öğreticiden geçerken, yönetilen örneğin başarısız [grupları ayarlamak için kaynaklarınızı ön koşullarla](sql-database-auto-failover-group.md#enabling-geo-replication-between-managed-instances-and-their-vnets)yapılandırdığınızdan emin olun. 
  > - Yönetilen bir örnek oluşturmak önemli miktarda zaman alabilir. Sonuç olarak, bu öğreticinin tamamlanması birkaç saat sürebilir. Sağlama süreleri hakkında daha fazla bilgi için [yönetilen örnek yönetim işlemlerine](sql-database-managed-instance.md#managed-instance-management-operations)bakın. 
  > - Bir başarısız gruba katılan yönetilen örnekler [expressroute](../expressroute/expressroute-howto-circuit-portal-resource-manager.md) veya iki bağlı VPN ağ geçidi gerektirir. Bu öğretici, VPN ağ geçitlerini oluşturmak ve bağlamak için adımlar sağlar. ExpressRoute'u zaten yapılandırmışsanız bu adımları atlayın. 


## <a name="prerequisites"></a>Ön koşullar

# <a name="portal"></a>[Portal](#tab/azure-portal)
Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun: 

- Azure aboneliği. Zaten hesabınız yoksa [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Öğreticiyi tamamlamak için aşağıdaki öğelere sahip olduğunuzdan emin olun:

- Azure aboneliği. Zaten hesabınız yoksa [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
- [Azure PowerShell](/powershell/azureps-cmdlets-docs)

---


## <a name="1---create-resource-group-and-primary-managed-instance"></a>1 - Kaynak grubu ve birincil yönetilen örnek oluşturma
Bu adımda, Azure portalını veya PowerShell'i kullanarak başarısız grubunuz için kaynak grubunu ve birincil yönetilen örneği oluşturursunuz. 


# <a name="portal"></a>[Portal](#tab/azure-portal) 

Azure portalını kullanarak kaynak grubunu ve birincil yönetilen örneğini oluşturun. 

1. Azure portalının sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. SELECT SQL dağıtım **seçeneği** sayfasını açmak için **+ Ekle'yi** seçin. Veritabanları döşemesindeki ayrıntıları göster'i seçerek farklı veritabanları hakkında ek bilgileri görüntüleyebilirsiniz.
1. SQL yönetilen **örnekler** döşemesi üzerinde **Oluştur'u** seçin. 

    ![Yönetilen örneği seçin](media/sql-database-managed-instance-failover-group-tutorial/select-managed-instance.png)

1. Azure **SQL Veritabanı Yönetilen Örnek Oluştur** sayfasında, Temel **Bilgiler** sekmesinde
    1. **Proje Ayrıntıları**altında, açılan sistemden **Aboneliğinizi** seçin ve ardından Yeni kaynak grubu **oluşturmayı** seçin. Kaynak grubunuz için bir ad `myResourceGroup`yazın, örneğin. 
    1. **Yönetilen Örnek Ayrıntıları**altında, yönetilen örneğinizin adını ve yönetilen örneğinizin dağıtmak istediğiniz bölgeyi sağlayın. İşlem **+ depolamayı** varsayılan değerlerde bırakın. 
    1. **Yönetici Hesabı**altında, bir yönetici giriş, gibi `azureuser`bir yönetici giriş ve karmaşık bir yönetici parolası sağlar. 

    ![Birincil MI oluşturma](media/sql-database-managed-instance-failover-group-tutorial/primary-sql-mi-values.png)

1. Ayarların geri kalanını varsayılan değerlere bırakın ve yönetilen örnek ayarlarınızı gözden geçirmek için **Gözden Geçir + oluştur'u** seçin. 
1. Birincil yönetilen örneğini oluşturmak için **Oluştur'u** seçin. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell'i kullanarak kaynak grubunuzu ve birincil yönetilen örneği oluşturun. 

   ```powershell-interactive
   # Connect-AzAccount
   # The SubscriptionId in which to create these objects
   $SubscriptionId = '<Subscription-ID>'
   # Create a random identifier to use as subscript for the different resource names
   $randomIdentifier = $(Get-Random)
   # Set the resource group name and location for your managed instance
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
   
   
   
   # Set the managed instance name for the new managed instances
   $primaryInstance = "primary-mi-$randomIdentifier"
   $secondaryInstance = "secondary-mi-$randomIdentifier"
   
   # Set the admin login and password for your managed instance
   $secpasswd = "PWD27!"+(New-Guid).Guid | ConvertTo-SecureString -AsPlainText -Force
   $mycreds = New-Object System.Management.Automation.PSCredential ("azureuser", $secpasswd)
   
   
   # Set the managed instance service tier, compute level, and license mode
   $edition = "General Purpose"
   $vCores = 8
   $maxStorage = 256
   $computeGeneration = "Gen5"
   $license = "LicenseIncluded" #"BasePrice" or LicenseIncluded if you have don't have SQL Server licence that can be used for AHB discount
   
   # Set failover group details
   $vpnSharedKey = "mi1mi2psk"
   $failoverGroupName = "failovergroup-$randomIdentifier"
   
   # Show randomized variables
   Write-host "Resource group name is" $resourceGroupName
   Write-host "Password is" $secpasswd
   Write-host "Primary Virtual Network name is" $primaryVNet
   Write-host "Primary default subnet name is" $primaryDefaultSubnet
   Write-host "Primary managed instance subnet name is" $primaryMiSubnetName
   Write-host "Secondary Virtual Network name is" $secondaryVNet
   Write-host "Secondary default subnet name is" $secondaryDefaultSubnet
   Write-host "Secondary managed instance subnet name is" $secondaryMiSubnetName
   Write-host "Primary managed instance name is" $primaryInstance
   Write-host "Secondary managed instance name is" $secondaryInstance
   Write-host "Failover group name is" $failoverGroupName
   
   # Suppress networking breaking changes warning (https://aka.ms/azps-changewarnings
   Set-Item Env:\SuppressAzurePowerShellBreakingChangeWarnings "true"
   
   # Set subscription context
   Set-AzContext -SubscriptionId $subscriptionId 
   
   # Create a resource group
   Write-host "Creating resource group..."
   $resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location -Tag @{Owner="SQLDB-Samples"}
   $resourceGroup
   
   # Configure primary virtual network
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
   
   
   # Configure primary MI subnet
   Write-host "Configuring primary MI subnet..."
   $primaryVirtualNetwork = Get-AzVirtualNetwork -Name $primaryVNet -ResourceGroupName $resourceGroupName
   
   
   $primaryMiSubnetConfig = Get-AzVirtualNetworkSubnetConfig `
                           -Name $primaryMiSubnetName `
                           -VirtualNetwork $primaryVirtualNetwork
   $primaryMiSubnetConfig
   
   # Configure network security group management service
   Write-host "Configuring primary MI subnet..."
   
   $primaryMiSubnetConfigId = $primaryMiSubnetConfig.Id
   
   $primaryNSGMiManagementService = New-AzNetworkSecurityGroup `
                         -Name 'primaryNSGMiManagementService' `
                         -ResourceGroupName $resourceGroupName `
                         -location $location
   $primaryNSGMiManagementService
   
   # Configure route table management service
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
   
   
   # Create primary managed instance
   
   Write-host "Creating primary managed instance..."
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
   Write-host "Primary managed instance created successfully."
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Bir Azure kaynak grubu oluşturur.  |
| [Yeni-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur.  |
| [Ekle-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Sanal ağa bir alt ağ yapılandırması ekler. | 
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. | 
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Sanal ağda bir alt ağ alır. | 
| [Yeni-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Ağ güvenlik grubu oluşturur. | 
| [Yeni-AzRouteTable](/powershell/module/az.network/new-azroutetable) | Bir rota tablosu oluşturur. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | Sanal ağ için bir alt ağ yapılandırması güncelleştirir.  |
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Sanal ağı güncelleştirir.  |
| [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup) | Bir ağ güvenlik grubu alır. |
| [Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Ağ güvenlik grubuna ağ güvenliği kuralı yapılandırması ekler. |
| [Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup) | Ağ güvenlik grubunu güncelleştirir.  | 
| [Ekle-AzRouteConfig](/powershell/module/az.network/add-azrouteconfig) | Rota tablosuna bir rota ekler. |
| [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable) | Rota tablosunu güncelleştirir.  |
| [Yeni-AzSqlInstance](/powershell/module/az.sql/new-azsqlinstance) | Azure SQL Veritabanı yönetilen bir örnek oluşturur.  |

---

## <a name="2---create-secondary-virtual-network"></a>2 - İkincil sanal ağ oluşturma
Yönetilen örneğini oluşturmak için Azure portalını kullanıyorsanız, birincil ve ikincil yönetilen örneğin alt ağının çakışan aralıklara sahip olmaması için sanal ağı ayrı ayrı oluşturmanız gerekir. Yönetilen örneğini yapılandırmak için PowerShell kullanıyorsanız, 3. 

# <a name="portal"></a>[Portal](#tab/azure-portal) 
Birincil sanal ağınızın alt ağ aralığını doğrulamak için aşağıdaki adımları izleyin:
1. Azure [portalında,](https://portal.azure.com)kaynak grubunuza gidin ve birincil örneğiniz için sanal ağı seçin. 
1. **Ayarlar'ın** **altındaki Alt Ağları** seçin ve **Adres aralığına**dikkat edin. İkincil yönetilen örnek için sanal ağın alt ağ adresi aralığı bu çakışmaz. 


   ![Birincil alt ağ](media/sql-database-managed-instance-failover-group-tutorial/verify-primary-subnet-range.png)

Sanal ağ oluşturmak için aşağıdaki adımları izleyin:

1. Azure [portalında](https://portal.azure.com) **kaynak oluştur'u** ve *sanal ağ*aramasını seçin. 
1. Microsoft tarafından yayınlanan **Sanal Ağ** seçeneğini seçin ve ardından bir sonraki sayfada **Oluştur'u** seçin. 
1. İkincil yönetilen örneğiniz için sanal ağı yapılandırmak için gerekli alanları doldurun ve ardından **Oluştur'u**seçin. 

   Aşağıdaki tablo ikincil sanal ağ için gerekli değerleri gösterir:

    | **Alan** | Değer |
    | --- | --- |
    | **Adı** |  Sanal ağın adı, ikincil yönetilen örnek tarafından kullanılacak, örneğin. `vnet-sql-mi-secondary` |
    | **Adres alanı** | Sanal ağınızın adres alanı, `10.128.0.0/16`örneğin. | 
    | **Abonelik** | Birincil yönetilen örnek ve kaynak grubunuzun bulunduğu abonelik. |
    | **Bölge** | İkincil yönetilen örneğini dağıtacağınız konum. |
    | **Alt ağ** | Alt ağınızın adı. `default`varsayılan olarak sizin için sağlanır. |
    | **Adres aralığı**| Alt ağınızın adres aralığı. Bu, birincil yönetilen örneğinizin sanal ağı tarafından kullanılan alt net adres `10.128.0.0/24`aralığından farklı olmalıdır, örneğin.  |
    | &nbsp; | &nbsp; |

    ![İkincil sanal ağ değerleri](media/sql-database-managed-instance-failover-group-tutorial/secondary-virtual-network.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Bu adım yalnızca yönetilen örneğini dağıtmak için Azure portalını kullanıyorsanız gereklidir. PowerShell kullanıyorsanız adım 3'e geçin. 

---

## <a name="3---create-a-secondary-managed-instance"></a>3 - İkincil yönetilen bir örnek oluşturma
Bu adımda, Azure portalında, yönetilen iki örnek arasındaki ağı da yapılandıracak ikincil yönetilen bir örnek oluşturursunuz. 

İkinci yönetilen örneğiniz şunları yapmalı:
- Boş ol. 
- Birincil yönetilen örnekten farklı bir alt net ve IP aralığına sahip. 

# <a name="portal"></a>[Portal](#tab/azure-portal) 

Azure portalını kullanarak ikincil yönetilen örneği oluşturun. 

1. Azure portalının sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. SELECT SQL dağıtım **seçeneği** sayfasını açmak için **+ Ekle'yi** seçin. Veritabanları döşemesindeki ayrıntıları göster'i seçerek farklı veritabanları hakkında ek bilgileri görüntüleyebilirsiniz.
1. SQL yönetilen **örnekler** döşemesi üzerinde **Oluştur'u** seçin. 

    ![Yönetilen örneği seçin](media/sql-database-managed-instance-failover-group-tutorial/select-managed-instance.png)

1. **Azure SQL Veritabanı Yönetilen Örnek Oluştur** sayfasının Temel **ler** sekmesinde, ikincil yönetilen örneğini yapılandırmak için gerekli alanları doldurun. 

   Aşağıdaki tablo ikincil yönetilen örnek için gerekli değerleri gösterir:
 
    | **Alan** | Değer |
    | --- | --- |
    | **Abonelik** |  Birincil yönetilen örneğinizin olduğu abonelik. |
    | **Kaynak grubu**| Birincil yönetilen örneğinizin olduğu kaynak grubu. |
    | **Yönetilen örnek adı** | Yeni ikincil yönetilen örneğinizin adı, örneğin`sql-mi-secondary`  | 
    | **Bölge**| İkincil yönetilen örneğinizin konumu.  |
    | **Yönetilen örnek yöneticisi oturum açma** | Yeni ikincil yönetilen örneğiniz için kullanmak istediğiniz oturum `azureuser`açma, örneğin. |
    | **Parola** | Yeni ikincil yönetilen örnek için yönetici girişi tarafından kullanılacak karmaşık bir parola.  |
    | &nbsp; | &nbsp; |

1. **Ağ** sekmesi altında, **Sanal Ağ**için, açılan alttan ikincil yönetilen örnek için oluşturduğunuz sanal ağı seçin.

   ![İkincil MI ağ](media/sql-database-managed-instance-failover-group-tutorial/networking-settings-for-secondary-mi.png)

1. Ek **ayarlar** sekmesi altında, **Coğrafi Çoğaltma**için, _ikincil üzerinden başarısız olarak Kullanmak_için **Evet'i** seçin. Açılan açılır dan birincil yönetilen örneği seçin. 
    1. Harmanlama ve saat diliminin birincil yönetilen örnekle eşleştiğinden emin olun. Bu öğreticide oluşturulan birincil yönetilen örnek, harmanlama varsayılanını `SQL_Latin1_General_CP1_CI_AS` ve saat dilimini `(UTC) Coordinated Universal Time` kullandı. 

   ![İkincil MI ağ](media/sql-database-managed-instance-failover-group-tutorial/secondary-mi-failover.png)

1. İkincil yönetilen örneğinizin ayarlarını incelemek için **Gözden Geçir + oluştur'u** seçin. 
1. İkincil yönetilen örneğini oluşturmak için **Oluştur'u** seçin. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell'i kullanarak ikincil yönetilen örneği oluşturun. 

   ```powershell-interactive
   # Configure secondary virtual network
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
   
   # Configure secondary managed instance subnet
   Write-host "Configuring secondary MI subnet..."
   
   $SecondaryVirtualNetwork = Get-AzVirtualNetwork -Name $secondaryVNet `
                                   -ResourceGroupName $resourceGroupName
   
   $secondaryMiSubnetConfig = Get-AzVirtualNetworkSubnetConfig `
                           -Name $secondaryMiSubnetName `
                           -VirtualNetwork $SecondaryVirtualNetwork
   $secondaryMiSubnetConfig
   
   # Configure secondary network security group management service
   Write-host "Configuring secondary network security group management service..."
   
   $secondaryMiSubnetConfigId = $secondaryMiSubnetConfig.Id
   
   $secondaryNSGMiManagementService = New-AzNetworkSecurityGroup `
                         -Name 'secondaryToMIManagementService' `
                         -ResourceGroupName $resourceGroupName `
                         -location $drlocation
   $secondaryNSGMiManagementService
   
   # Configure secondary route table MI management service
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
   
   # Create secondary managed instance
   
   $primaryManagedInstanceId = Get-AzSqlInstance -Name $primaryInstance -ResourceGroupName $resourceGroupName | Select-Object Id
   
   
   Write-host "Creating secondary managed instance..."
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
   Write-host "Secondary managed instance created successfully."
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Bir Azure kaynak grubu oluşturur.  |
| [Yeni-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur.  |
| [Ekle-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Sanal ağa bir alt ağ yapılandırması ekler. | 
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. | 
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Sanal ağda bir alt ağ alır. | 
| [Yeni-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Ağ güvenlik grubu oluşturur. | 
| [Yeni-AzRouteTable](/powershell/module/az.network/new-azroutetable) | Bir rota tablosu oluşturur. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | Sanal ağ için bir alt ağ yapılandırması güncelleştirir.  |
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Sanal ağı güncelleştirir.  |
| [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup) | Bir ağ güvenlik grubu alır. |
| [Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Ağ güvenlik grubuna ağ güvenliği kuralı yapılandırması ekler. |
| [Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup) | Ağ güvenlik grubunu güncelleştirir.  | 
| [Ekle-AzRouteConfig](/powershell/module/az.network/add-azrouteconfig) | Rota tablosuna bir rota ekler. |
| [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable) | Rota tablosunu güncelleştirir.  |
| [Yeni-AzSqlInstance](/powershell/module/az.sql/new-azsqlinstance) | Azure SQL Veritabanı yönetilen bir örnek oluşturur.  |

---

## <a name="4---create-primary-gateway"></a>4 - Birincil ağ geçidi oluşturma 
Bir başarısız gruba katılmak için yönetilen iki örnek için, ağ iletişimine izin vermek için ExpressRoute veya yönetilen iki örneğin sanal ağları arasında yapılandırılan bir ağ geçidi olması gerekir. [ExpressRoute'u](../expressroute/expressroute-howto-circuit-portal-resource-manager.md) iki VPN ağ geçidine bağlanmak yerine yapılandırmayı seçerseniz, [Adım 7'ye](#7---create-a-failover-group)geçin.  

Bu makalede, iki VPN ağ geçidi oluşturmak ve bunları bağlamak için adımlar sağlar, ancak expressroute yerine yapılandırıldıysanız, başarısız grup oluşturmak için ileri atlayabilirsiniz. 


# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak birincil yönetilen örneğinizin sanal ağı için ağ geçidi oluşturun. 


1. Azure [portalında,](https://portal.azure.com)kaynak grubunuza gidin ve birincil yönetilen örneğiniz için **Sanal ağ** kaynağını seçin. 
1. **Ayarlar'ın** altındaki **Alt Ağları** seçin ve ardından yeni bir Ağ Geçidi **alt ağı**eklemek için seçin. Varsayılan değerleri bırakın. 

   ![Birincil yönetilen örnek için ağ geçidi ekleme](media/sql-database-managed-instance-failover-group-tutorial/add-subnet-gateway-primary-vnet.png)

1. Alt ağ geçidi oluşturulduktan sonra, sol gezinti bölmesinden kaynak `Virtual network gateway` **oluştur'u** seçin ve ardından arama kutusuna yazın. **Microsoft**tarafından yayınlanan **Sanal ağ ağ ağ** geçidi kaynağını seçin. 

   ![Yeni bir sanal ağ ağ geçidi oluşturma](media/sql-database-managed-instance-failover-group-tutorial/create-virtual-network-gateway.png)

1. Ağ geçidini yapılandırmak için gereken alanları birincil yönetilen örneğini doldurun. 

   Aşağıdaki tablo, birincil yönetilen örnek için ağ geçidi için gerekli değerleri gösterir:
 
    | **Alan** | Değer |
    | --- | --- |
    | **Abonelik** |  Birincil yönetilen örneğinizin olduğu abonelik. |
    | **Adı** | Sanal ağ ağ ağ geçidinizin adı, örneğin. `primary-mi-gateway` | 
    | **Bölge** | Birincil yönetilen örneğinizin olduğu bölge. |
    | **Ağ geçidi türü** | **VPN'i**seçin. |
    | **VPN Türü** | **Rota Tabanlı'yı** seçin |
    | **Sku**| Varsayılan ı `VpnGw1`bırakın. |
    | **Konum**| Birincil yönetilen örneğinizin ve birincil sanal ağınızın olduğu konum.   |
    | **Sanal ağ**| Bölüm 2'de oluşturulan sanal ağı seçin. `vnet-sql-mi-primary` |
    | **Genel IP adresi**| **Yeni oluştur**’u seçin. |
    | **Genel IP adresi adı**| IP adresiniz için bir ad `primary-gateway-IP`girin, örneğin. |
    | &nbsp; | &nbsp; |

1. Diğer değerleri varsayılan olarak bırakın ve ardından sanal ağ ağ ağ geçidinizin ayarlarını gözden geçirmek için **Gözden Geçir + oluştur'u** seçin.

   ![Birincil ağ geçidi ayarları](media/sql-database-managed-instance-failover-group-tutorial/settings-for-primary-gateway.png)

1. Yeni sanal ağ ağ ağınızı oluşturmak için **Oluştur'u** seçin. 


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell'i kullanarak birincil yönetilen örneğinizin sanal ağı için ağ geçidi oluşturun. 

   ```powershell-interactive
   # Create primary gateway
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

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. |
| [Ekle-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Sanal ağa bir alt ağ yapılandırması ekler. | 
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Sanal ağı güncelleştirir.  |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Sanal ağda bir alt ağ alır. |
| [Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur.  | 
| [Yeni-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig) | Sanal Ağ Ağ Geçidi için IP Yapılandırması Oluşturur |
| [Yeni-AzVirtualNetworkAğ Ağ Geçidi](/powershell/module/az.network/new-azvirtualnetworkgateway) | Sanal Ağ Ağ Geçidi Oluşturur |


---


## <a name="5---create-secondary-gateway"></a>5 - İkincil ağ geçidi oluşturma 
Bu adımda, Azure portalını kullanarak ikincil yönetilen örneğinizin sanal ağı için ağ geçidi oluşturun, 


# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak, ikincil yönetilen örnek için sanal ağ alt ağı ve ağ geçidi oluşturmak için önceki bölümdeki adımları yineleyin. İkincil yönetilen örneğiniz için ağ geçidini yapılandırmak için gerekli alanları doldurun. 

   Aşağıdaki tablo, ikincil yönetilen örnek için ağ geçidi için gerekli değerleri gösterir:

   | **Alan** | Değer |
   | --- | --- |
   | **Abonelik** |  İkincil yönetilen örneğinizin olduğu abonelik. |
   | **Adı** | Sanal ağ ağ ağ geçidinizin adı, örneğin. `secondary-mi-gateway` | 
   | **Bölge** | İkincil yönetilen örneğinizin olduğu bölge. |
   | **Ağ geçidi türü** | **VPN'i**seçin. |
   | **VPN Türü** | **Rota Tabanlı'yı** seçin |
   | **Sku**| Varsayılan ı `VpnGw1`bırakın. |
   | **Konum**| İkincil yönetilen örneğinizin ve ikincil sanal ağınızın olduğu konum.   |
   | **Sanal ağ**| Bölüm 2'de oluşturulan sanal ağı seçin. `vnet-sql-mi-secondary` |
   | **Genel IP adresi**| **Yeni oluştur**’u seçin. |
   | **Genel IP adresi adı**| IP adresiniz için bir ad `secondary-gateway-IP`girin, örneğin. |
   | &nbsp; | &nbsp; |

   ![İkincil ağ geçidi ayarları](media/sql-database-managed-instance-failover-group-tutorial/settings-for-secondary-gateway.png)


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell'i kullanarak ikincil yönetilen örneğin sanal ağı için ağ geçidi oluşturun. 

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

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. |
| [Ekle-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Sanal ağa bir alt ağ yapılandırması ekler. | 
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Sanal ağı güncelleştirir.  |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Sanal ağda bir alt ağ alır. |
| [Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur.  | 
| [Yeni-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig) | Sanal Ağ Ağ Geçidi için IP Yapılandırması Oluşturur |
| [Yeni-AzVirtualNetworkAğ Ağ Geçidi](/powershell/module/az.network/new-azvirtualnetworkgateway) | Sanal Ağ Ağ Geçidi Oluşturur |

---


## <a name="6---connect-the-gateways"></a>6 - Ağ geçitlerini bağlayın
Bu adımda, iki sanal anın iki ağ geçidi arasında çift yönlü bir bağlantı oluşturun. 


# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak iki ağ geçidini bağlayın. 


1. [Azure portalından](https://portal.azure.com) **kaynak oluştur'u** seçin.
1. Arama `connection` kutusuna yazın ve ardından arama için enter tuşuna basın, bu da sizi Microsoft tarafından yayınlanan **Bağlantı** kaynağına götürür.
1. Bağlantınızı oluşturmak için **Oluştur'u** seçin. 
1. Temel **bilgiler** sekmesinde, aşağıdaki değerleri seçin ve sonra **Tamam'ı**seçin. 
    1. Bağlantı `VNet-to-VNet` **türü**için seçin. 
    1. Açılan listeden aboneliğinizi seçin. 
    1. Açılan yolda yönetilen örneğiniz için kaynak grubunu seçin. 
    1. Açılır noktadan birincil yönetilen örneğinizin konumunu seçin 
1. **Ayarlar** sekmesinde aşağıdaki değerleri seçin veya girin ve sonra **Tamam'ı**seçin:
    1. İlk sanal ağ ağ ağ geçidi için `Primary-Gateway`birincil ağ ağ ağ **geçidini**seçin (  
    1. İkinci sanal ağ ağ geçidi için ikincil `Secondary-Gateway`ağ ağ ağ **geçidini**seçin ( 
    1. Çift yönlü bağlantı **oluştur'un**yanındaki onay kutusunu seçin. 
    1. Varsayılan birincil bağlantı adını bırakın veya seçtiğiniz bir değerle yeniden adlandırın. 
    1. Bağlantı için paylaşılan bir **anahtar (PSK)** sağlayın, örneğin. `mi1m2psk` 

   ![Ağ geçidi bağlantısı oluşturma](media/sql-database-managed-instance-failover-group-tutorial/create-gateway-connection.png)

1. **Özet** sekmesinde, çift yönlü bağlantınızın ayarlarını gözden geçirin ve ardından bağlantınızı oluşturmak için **Tamam'ı** seçin. 


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell'i kullanarak iki ağ geçidini bağlayın. 

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

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet kullanır:

| Komut | Notlar |
|---|---|
| [New-AzVirtualNetworkGatewayConnection](/powershell/module/az.network/new-azvirtualnetworkgatewayconnection) | İki sanal ağ ağ geçidi arasında bir bağlantı oluşturur.   |

---


## <a name="7---create-a-failover-group"></a>7 - Bir başarısız grup oluşturma
Bu adımda, başarısız grup oluşturacak ve her iki yönetilen örnekleri de ona eklersiniz. 


# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalını kullanarak başarısız grubu oluşturun. 


1. [Azure portalının](https://portal.azure.com)sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. İlk bölümde oluşturduğunuz birincil yönetilen örneği seçin, örneğin. `sql-mi-primary` 
1. **Ayarlar** **altında, Örnek Failover Grupları'na** gidin ve ardından **Örnek Failover Grubu** sayfasını açmak için grup **eklemeyi** seçin. 

   ![Bir başarısız grup ekleme](media/sql-database-managed-instance-failover-group-tutorial/add-failover-group.png)

1. Instance **Failover Group** sayfasında, başarısız grubunuzun adını yazın `failovergrouptutorial` ve açılan bırakma gibi `sql-mi-secondary` ikincil yönetilen örneği seçin. Başarısız grupoluşturmak için **Oluştur'u** seçin. 

   ![Başarısız grup oluşturma](media/sql-database-managed-instance-failover-group-tutorial/create-failover-group.png)

1. Başarısız grup dağıtımı tamamlandıktan sonra **Failover grup** sayfasına geri götürülür. 


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
PowerShell'i kullanarak başarısız grubu oluşturun. 

   ```powershell-interactive
   Write-host "Creating the failover group..."
   $failoverGroup = New-AzSqlDatabaseInstanceFailoverGroup -Name $failoverGroupName `
        -Location $location -ResourceGroupName $resourceGroupName -PrimaryManagedInstanceName $primaryInstance `
        -PartnerRegion $drLocation -PartnerManagedInstanceName $secondaryInstance `
        -FailoverPolicy Automatic -GracePeriodWithDataLossHours 1
   $failoverGroup
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet kullanır:

| Komut | Notlar |
|---|---|
| [Yeni-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/new-azsqldatabaseinstancefailovergroup)| Yeni bir Azure SQL Veritabanı yönetilen örnek failover grubu oluşturur.  |


---


## <a name="8---test-failover"></a>8 - Test başarısız
Bu adımda, başarısız olan grubunuzun ikincil sunucuya devredilemeyeceğiniz ve Azure portalını kullanarak geri dönmeceksiniz. 


# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalını kullanarak başarısız olun. 


1. [Azure portalındaki](https://portal.azure.com) _ikincil_ yönetilen örneğinize gidin ve ayarlar altında **Örnek Failover Grupları'nı** seçin. 
1. Yönetilen örneğin birincil, yönetilen örneğin ikincil olduğu gözden geçirin. 
1. **Failover'ı** seçin ve ardından TDS oturumlarının bağlantısının kesildiğine dair uyarıda **Evet'i** seçin. 

   ![Başarısız grup üzerinde başarısız](media/sql-database-managed-instance-failover-group-tutorial/failover-mi-failover-group.png)

1. Hangi manged örneğin birincil ve hangi örnek ikincil olduğunu gözden geçirin. Başarısız başarılı olsaydı, iki örnek rolleri değiştirmiş olmalıdır. 

   ![Yönetilen örnekler, başarısız olduktan sonra rolleri değiştirdi](media/sql-database-managed-instance-failover-group-tutorial/mi-switched-after-failover.png)

1. Yeni _ikincil_ yönetilen örneğe gidin ve birincil örneği birincil role geri başarısız olmak için **failover'ı** bir kez daha seçin. 


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
PowerShell kullanarak başarısız lık testi yapın. 

   ```powershell-interactive
    
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
       -Location $location -Name $failoverGroupName
   
   # Failover the primary managed instance to the secondary role
   Write-host "Failing primary over to the secondary location"
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
       -Location $drLocation -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
   Write-host "Successfully failed failover group to secondary location"
   ```


Failover grubunu birincil sunucuya geri döndürün:

   ```powershell-interactive
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
       -Location $drLocation -Name $failoverGroupName
   
   # Fail primary managed instance back to primary role
   Write-host "Failing primary back to primary role"
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
       -Location $location -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
   Write-host "Successfully failed failover group to primary location"
   
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
       -Location $location -Name $failoverGroupName
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [Get-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/get-azsqldatabaseinstancefailovergroup) | Yönetilen örnek failover gruplarını alır veya listeler.| 
| [Switch-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/switch-azsqldatabaseinstancefailovergroup) | Yönetilen bir örnek failover grubunun başarısız bir yürütmesini yürütür. | 

---



## <a name="clean-up-resources"></a>Kaynakları temizleme
Önce yönetilen örneği, ardından sanal kümeyi, sonra kalan kaynakları ve son olarak kaynak grubunu silerek kaynakları temizleyin. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
1. [Azure portalındaki](https://portal.azure.com)kaynak grubunuza gidin. 
1. Yönetilen örnek(ler)'i seçin ve sonra **Sil'i**seçin. Kaynağı `yes` silmek istediğinizi doğrulamak için metin kutusuna yazın ve sonra **Sil'i**seçin. Bu işlemin arka planda tamamlanması biraz zaman alabilir ve tamamlanana kadar *Sanal kümeyi* veya diğer bağımlı kaynakları silemezsiniz. Yönetilen örneğinizin silindiğini doğrulamak için Etkinlik sekmesindeki silmeyi izleyin. 
1. Yönetilen örnek silindikten sonra, kaynak grubunuzda seçerek *Sanal kümeyi* silin ve sonra **Sil'i**seçin. Kaynağı `yes` silmek istediğinizi doğrulamak için metin kutusuna yazın ve sonra **Sil'i**seçin. 
1. Kalan kaynakları silin. Kaynağı `yes` silmek istediğinizi doğrulamak için metin kutusuna yazın ve sonra **Sil'i**seçin. 
1. **Kaynak grubunu sil'i**seçerek kaynak grubunu sil, kaynak `myResourceGroup`grubunun adını yazarak ve sonra **Sil'i**seçerek kaynak grubunu silin. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Kaynak grubunu iki kez kaldırmanız gerekir. Kaynak grubunu ilk kez kaldırma yönetilen örnek ve sanal kümeleri kaldırır, ancak `Remove-AzResourceGroup : Long running operation failed with status 'Conflict'.`daha sonra hata iletisi ile başarısız olur. Kaynak grubunun yanı sıra kalan kaynakları kaldırmak için Remove-AzResourceGroup komutunu ikinci kez çalıştırın.

```powershell-interactive
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
Write-host "Removing managed instance and virtual cluster..."
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
Write-host "Removing residual resources and resouce group..."
```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet kullanır:

| Komut | Notlar |
|---|---|
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu kaldırır. |

---

## <a name="full-script"></a>Tam betik

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
[!code-powershell-interactive[main](../../powershell_scripts/sql-database/failover-groups/add-managed-instance-to-failover-group-az-ps.ps1 "Add managed instance to a failover group")]

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Bir Azure kaynak grubu oluşturur.  |
| [Yeni-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur.  |
| [Ekle-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Sanal ağa bir alt ağ yapılandırması ekler. | 
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. | 
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Sanal ağda bir alt ağ alır. | 
| [Yeni-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Ağ güvenlik grubu oluşturur. | 
| [Yeni-AzRouteTable](/powershell/module/az.network/new-azroutetable) | Bir rota tablosu oluşturur. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | Sanal ağ için bir alt ağ yapılandırması güncelleştirir.  |
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Sanal ağı güncelleştirir.  |
| [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup) | Bir ağ güvenlik grubu alır. |
| [Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Ağ güvenlik grubuna ağ güvenliği kuralı yapılandırması ekler. |
| [Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup) | Ağ güvenlik grubunu güncelleştirir.  | 
| [Ekle-AzRouteConfig](/powershell/module/az.network/add-azrouteconfig) | Rota tablosuna bir rota ekler. |
| [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable) | Rota tablosunu güncelleştirir.  |
| [Yeni-AzSqlInstance](/powershell/module/az.sql/new-azsqlinstance) | Azure SQL Veritabanı yönetilen bir örnek oluşturur.  |
| [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance)| Azure SQL Yönetilen Veritabanı Örneği hakkındaki bilgileri verir. |
| [Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur.  | 
| [Yeni-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig) | Sanal Ağ Ağ Geçidi için IP Yapılandırması Oluşturur |
| [Yeni-AzVirtualNetworkAğ Ağ Geçidi](/powershell/module/az.network/new-azvirtualnetworkgateway) | Sanal Ağ Ağ Geçidi Oluşturur |
| [New-AzVirtualNetworkGatewayConnection](/powershell/module/az.network/new-azvirtualnetworkgatewayconnection) | İki sanal ağ ağ geçidi arasında bir bağlantı oluşturur.   |
| [Yeni-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/new-azsqldatabaseinstancefailovergroup)| Yeni bir Azure SQL Veritabanı yönetilen örnek failover grubu oluşturur.  |
| [Get-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/get-azsqldatabaseinstancefailovergroup) | Yönetilen örnek failover gruplarını alır veya listeler.| 
| [Switch-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/switch-azsqldatabaseinstancefailovergroup) | Yönetilen bir örnek failover grubunun başarısız bir yürütmesini yürütür. | 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu kaldırır. | 

# <a name="portal"></a>[Portal](#tab/azure-portal) 

Azure portalı için kullanılabilir komut dosyası yok.

---

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, yönetilen iki örnek arasında bir başarısız lık grubu yapılandırıldınız. Şunları öğrendiniz:

> [!div class="checklist"]
> - Birincil yönetilen örnek oluşturma
> - Bir [başarısız lık grubunun](sql-database-auto-failover-group.md)parçası olarak ikincil yönetilen bir örnek oluşturun. 
> - Yük devretme testi

Yönetilen örneğinize nasıl bağlanılacağınızda ve yönetilen örneğinize veritabanını nasıl geri yükleyebilirsiniz: 

> [!div class="nextstepaction"]
> [Yönetilen örneğinize](sql-database-managed-instance-configure-vm.md)
> bağlanma[Veritabanını yönetilen örneğe geri yükleme](sql-database-managed-instance-get-started-restore.md)


