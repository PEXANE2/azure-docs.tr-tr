---
title: VNet uç noktaları ve tek ve havuzlu veritabanları için kurallar için PowerShell
description: Azure SQL Veritabanınız ve Azure Synapse'niz için Sanal Hizmet bitiş noktaları oluşturmak ve yönetmek için PowerShell komut dosyaları sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: PowerShell
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: genemi, vanto
ms.date: 03/12/2019
tags: azure-synapse
ms.openlocfilehash: 1e8ec394eab1df0aebe394b8acebc74c7ed49e9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124697"
---
# <a name="powershell--create-a-virtual-service-endpoint-and-vnet-rule-for-sql"></a>PowerShell: SQL için Sanal Hizmet bitiş noktası ve VNet kuralı oluşturun

*Sanal ağ kuralları,* Azure [SQL Veritabanı'ndaki](sql-database-technical-overview.md) tek veritabanlarınız ve elastik havuzunuz için veya [Azure Synapse'deki](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) veritabanlarınızın sanal ağlardaki belirli alt ağlardan gönderilen iletişimleri kabul edip etmediğini kontrol eden bir güvenlik duvarı güvenlik özelliğidir.

> [!IMPORTANT]
> Bu makale, Azure SQL sunucusu ve Azure Synapse'de oluşturulan SQL Veritabanı ve veri ambarı için geçerlidir. Basitlik için, SQL Veritabanı hem SQL Veritabanı hem de Azure Synapse'ye atıfta bulunularak kullanılır. Bu makale, Azure SQL Veritabanı'nda yönetilen bir **örnek** dağıtımı için geçerli *değildir,* çünkü bununla ilişkili bir hizmet bitiş noktası yoktur.

Bu makalede, aşağıdaki eylemleri alan bir PowerShell komut dosyası sağlar ve açıklar:

1. Alt ağınızda bir Microsoft Azure *Sanal Hizmet bitiş noktası* oluşturur.
2. *Sanal ağ kuralı*oluşturmak için Azure SQL Veritabanı sunucunuzun güvenlik duvarına bitiş noktası ekler.

Bir kural oluşturma motivasyonlarınız şu şekilde açıklanmıştır: [Azure SQL Veritabanı için Sanal Hizmet bitiş noktaları.][sql-db-vnet-service-endpoint-rule-overview-735r]

> [!TIP]
> Tek ihtiyacınız olan SQL Veritabanı için Sanal Hizmet uç nokta *türü adını* alt ağınıza eklemekse, daha [doğrudan PowerShell komut dosyamıza](#a-verify-subnet-is-endpoint-ps-100)geçebilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

## <a name="major-cmdlets"></a>Büyük cmdlets

Bu makalede, Azure SQL Veritabanı sunucunuzun erişim denetim listesine (ACL) alt uç noktasını ekleyen ve böylece bir kural oluşturan **Yeni-AzSqlServerVirtualNetworkRule** cmdlet vurgulamaktadır.

Aşağıdaki **liste, New-AzSqlServerVirtualNetworkRule'e**çağrınıza hazırlanmak için çalıştırmanız gereken diğer *büyük* cmdletdizisini gösterir. Bu makalede, bu çağrılar [komut dosyası 3 "Sanal ağ kuralı"](#a-script-30)oluşur:

1. [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig): Bir alt ağ nesnesi oluşturur.
2. [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork): Sanal ağınızı oluşturur ve alt ağı verir.
3. [Set-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/Set-azVirtualNetworkSubnetConfig): Alt ağınıza sanal hizmet bitiş noktası atar.
4. [Set-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/Set-azVirtualNetwork): Sanal ağınızda yapılan güncellemeleri devam eder.
5. [New-AzSqlServerVirtualNetworkRule](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlservervirtualnetworkrule): Alt ağınız bir bitiş noktası olduktan sonra, alt ağınızı sanal ağ kuralı olarak Azure SQL Veritabanı sunucunuzun ACL'sine ekler.
   - Bu cmdlet Azure RM PowerShell Modülü sürüm 5.1.1'den başlayarak **-IgnoreMissingVNetServiceEndpoint**- parametresunar.

## <a name="prerequisites-for-running-powershell"></a>PowerShell çalıştırmak için ön koşullar

- [Azure portalı][http-azure-portal-link-ref-477t]gibi Azure'da oturum açabilirsiniz.
- PowerShell komut dosyalarını zaten çalıştırabilirsiniz.

> [!NOTE]
> Lütfen Sunucunuza eklemek istediğiniz VNet/Subnet için hizmet bitiş noktalarının açık olduğundan emin olun, aksi takdirde VNet Güvenlik Duvarı Kuralı'nın oluşturulması başarısız olur.

## <a name="one-script-divided-into-four-chunks"></a>Dört parçaya bölünmüş bir komut dosyası

Bizim gösteri PowerShell komut küçük komut bir dizi ayrılmıştır. Bölüm öğrenmeyi kolaylaştırır ve esneklik sağlar. Komut dosyaları belirtilen sırayla çalıştırılmalıdır. Komut dosyalarını çalıştırmak için şimdi zamanın yoksa, gerçek test çıktımız komut dosyası 4'ten sonra görüntülenir.

<a name="a-script-10" />

### <a name="script-1-variables"></a>Komut Dosyası 1: Değişkenler

Bu ilk PowerShell komut dosyası değişkenlere değer atar. Sonraki komut dosyaları bu değişkenlere bağlıdır.

> [!IMPORTANT]
> Bu komut dosyasını çalıştırmadan önce, isterseniz değerleri de yapabilirsiniz. Örneğin, zaten bir kaynak grubunuz varsa, kaynak grup adınızı atanan değer olarak yeniden atamak isteyebilirsiniz.
>
> Abonelik adınız komut dosyasında düzenlenmelidir.

### <a name="powershell-script-1-source-code"></a>PowerShell komut dosyası 1 kaynak kodu

```powershell
######### Script 1 ########################################
##   LOG into to your Azure account.                     ##
##   (Needed only one time per powershell.exe session.)  ##
###########################################################

$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]';
if ('yes' -eq $yesno) { Connect-AzAccount; }

###########################################################
##  Assignments to variables used by the later scripts.  ##
###########################################################

# You can edit these values, if necessary.
$SubscriptionName = 'yourSubscriptionName';
Select-AzSubscription -SubscriptionName $SubscriptionName;

$ResourceGroupName = 'RG-YourNameHere';
$Region            = 'westcentralus';

$VNetName            = 'myVNet';
$SubnetName          = 'mySubnet';
$VNetAddressPrefix   = '10.1.0.0/16';
$SubnetAddressPrefix = '10.1.1.0/24';
$VNetRuleName        = 'myFirstVNetRule-ForAcl';

$SqlDbServerName         = 'mysqldbserver-forvnet';
$SqlDbAdminLoginName     = 'ServerAdmin';
$SqlDbAdminLoginPassword = 'ChangeYourAdminPassword1';

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql';  # Official type name.

Write-Host 'Completed script 1, the "Variables".';
```

<a name="a-script-20" />

### <a name="script-2-prerequisites"></a>Komut Dosyası 2: Ön koşullar

Bu komut dosyası, bitiş noktası eyleminin olduğu bir sonraki komut dosyası için hazırlanır. Bu komut dosyası, yalnızca zaten yoksa, sizin için aşağıdaki listelenen öğeleri oluşturur. Bu öğelerin zaten var olduğundan eminseniz komut dosyası 2'yi atlayabilirsiniz:

- Azure kaynak grubu
- Azure SQL Veritabanı sunucusu

### <a name="powershell-script-2-source-code"></a>PowerShell komut dosyası 2 kaynak kodu

```powershell
######### Script 2 ########################################
##   Ensure your Resource Group already exists.          ##
###########################################################

Write-Host "Check whether your Resource Group already exists.";

$gottenResourceGroup = $null;

$gottenResourceGroup = Get-AzResourceGroup `
  -Name        $ResourceGroupName `
  -ErrorAction SilentlyContinue;

if ($null -eq $gottenResourceGroup)
{
    Write-Host "Creating your missing Resource Group - $ResourceGroupName.";

    $gottenResourceGroup = New-AzResourceGroup `
      -Name $ResourceGroupName `
      -Location $Region;

    $gottenResourceGroup;
}
else { Write-Host "Good, your Resource Group already exists - $ResourceGroupName."; }

$gottenResourceGroup = $null;

###########################################################
## Ensure your Azure SQL Database server already exists. ##
###########################################################

Write-Host "Check whether your Azure SQL Database server already exists.";

$sqlDbServer = $null;

$sqlDbServer = Get-AzSqlServer `
  -ResourceGroupName $ResourceGroupName `
  -ServerName        $SqlDbServerName `
  -ErrorAction       SilentlyContinue;

if ($null -eq $sqlDbServer) {
    Write-Host "Creating the missing Azure SQL Database server - $SqlDbServerName.";
    Write-Host "Gather the credentials necessary to next create an Azure SQL Database server.";

    $sqlAdministratorCredentials = New-Object `
      -TypeName     System.Management.Automation.PSCredential `
      -ArgumentList `
        $SqlDbAdminLoginName, `
        $(ConvertTo-SecureString `
            -String      $SqlDbAdminLoginPassword `
            -AsPlainText `
            -Force `
         );

    if ($null -eq $sqlAdministratorCredentials) {
        Write-Host "ERROR, unable to create SQL administrator credentials.  Now ending.";
        return;
    }

    Write-Host "Create your Azure SQL Database server.";

    $sqlDbServer = New-AzSqlServer `
      -ResourceGroupName $ResourceGroupName `
      -ServerName        $SqlDbServerName `
      -Location          $Region `
      -SqlAdministratorCredentials $sqlAdministratorCredentials;

    $sqlDbServer;
}
else {
    Write-Host "Good, your Azure SQL Database server already exists - $SqlDbServerName."; 
}

$sqlAdministratorCredentials = $null;
$sqlDbServer                 = $null;

Write-Host 'Completed script 2, the "Prerequisites".';
```

<a name="a-script-30" />

## <a name="script-3-create-an-endpoint-and-a-rule"></a>Komut Dosyası 3: Bir bitiş noktası ve kural oluşturma

Bu komut dosyası, bir alt ağ ile sanal ağ oluşturur. Ardından komut **dosyası, microsoft.sql** uç noktası türünü alt ağınıza atar. Son olarak komut dosyası, alt ağınızı SQL Veritabanı sunucunuzun erişim denetim listesine (ACL) ekleyerek bir kural oluşturur.

### <a name="powershell-script-3-source-code"></a>PowerShell komut dosyası 3 kaynak kodu

```powershell
######### Script 3 ########################################
##   Create your virtual network, and give it a subnet.  ##
###########################################################

Write-Host "Define a subnet '$SubnetName', to be given soon to a virtual network.";

$subnet = New-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

Write-Host "Create a virtual network '$VNetName'." `
  "  Give the subnet to the virtual network that we created.";

$vnet = New-AzVirtualNetwork `
  -Name              $VNetName `
  -AddressPrefix     $VNetAddressPrefix `
  -Subnet            $subnet `
  -ResourceGroupName $ResourceGroupName `
  -Location          $Region;

###########################################################
##   Create a Virtual Service endpoint on the subnet.    ##
###########################################################

Write-Host "Assign a Virtual Service endpoint 'Microsoft.Sql' to the subnet.";

$vnet = Set-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -VirtualNetwork  $vnet `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

Write-Host "Persist the updates made to the virtual network > subnet.";

$vnet = Set-AzVirtualNetwork `
  -VirtualNetwork $vnet;

$vnet.Subnets[0].ServiceEndpoints;  # Display the first endpoint.

###########################################################
##   Add the Virtual Service endpoint Id as a rule,      ##
##   into SQL Database ACLs.                             ##
###########################################################

Write-Host "Get the subnet object.";

$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

$subnet = Get-AzVirtualNetworkSubnetConfig `
  -Name           $SubnetName `
  -VirtualNetwork $vnet;

Write-Host "Add the subnet .Id as a rule, into the ACLs for your Azure SQL Database server.";

$vnetRuleObject1 = New-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName `
  -VirtualNetworkSubnetId $subnet.Id;

$vnetRuleObject1;

Write-Host "Verify that the rule is in the SQL DB ACL.";

$vnetRuleObject2 = Get-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName;

$vnetRuleObject2;

Write-Host 'Completed script 3, the "Virtual-Network-Rule".';
```

<a name="a-script-40" />

## <a name="script-4-clean-up"></a>Komut Dosyası 4: Temizleme

Bu son komut dosyası, önceki komut dosyalarının gösteri için oluşturduğu kaynakları siler. Ancak, komut dosyası aşağıdakileri silmeden önce onay ister:

- Azure SQL Veritabanı sunucusu
- Azure Kaynak Grubu

Komut dosyası 4'u komut dosyası 1 tamamlandıktan sonra istediğiniz zaman çalıştırabilirsiniz.

### <a name="powershell-script-4-source-code"></a>PowerShell komut dosyası 4 kaynak kodu

```powershell
######### Script 4 ########################################
##   Clean-up phase A:  Unconditional deletes.           ##
##                                                       ##
##   1. The test rule is deleted from SQL DB ACL.        ##
##   2. The test endpoint is deleted from the subnet.    ##
##   3. The test virtual network is deleted.             ##
###########################################################

Write-Host "Delete the rule from the SQL DB ACL.";

Remove-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName `
  -ErrorAction            SilentlyContinue;

Write-Host "Delete the endpoint from the subnet.";

$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

Remove-AzVirtualNetworkSubnetConfig `
  -Name $SubnetName `
  -VirtualNetwork $vnet;

Write-Host "Delete the virtual network (thus also deletes the subnet).";

Remove-AzVirtualNetwork `
  -Name              $VNetName `
  -ResourceGroupName $ResourceGroupName `
  -ErrorAction       SilentlyContinue;

###########################################################
##   Clean-up phase B:  Conditional deletes.             ##
##                                                       ##
##   These might have already existed, so user might     ##
##   want to keep.                                       ##
##                                                       ##
##   1. Azure SQL Database server                        ##
##   2. Azure resource group                             ##
###########################################################

$yesno = Read-Host 'CAUTION !: Do you want to DELETE your Azure SQL Database server AND your Resource Group?  [yes/no]';
if ('yes' -eq $yesno) {
    Write-Host "Remove the Azure SQL DB server.";

    Remove-AzSqlServer `
      -ServerName        $SqlDbServerName `
      -ResourceGroupName $ResourceGroupName `
      -ErrorAction       SilentlyContinue;

    Write-Host "Remove the Azure Resource Group.";

    Remove-AzResourceGroup `
      -Name        $ResourceGroupName `
      -ErrorAction SilentlyContinue;
}
else {
    Write-Host "Skipped over the DELETE of SQL Database and resource group.";
}

Write-Host 'Completed script 4, the "Clean-Up".';
```

<a name="a-actual-output" />

<a name="a-verify-subnet-is-endpoint-ps-100" />

## <a name="verify-your-subnet-is-an-endpoint"></a>Alt ağınızın bir bitiş noktası olduğunu doğrulayın

**Microsoft.Sql** türü adı zaten atanmış bir alt ağınız olabilir, yani zaten sanal hizmet bitiş noktasıdır. Son noktadan bir sanal ağ kuralı oluşturmak için [Azure portalını][http-azure-portal-link-ref-477t] kullanabilirsiniz.

Veya, alt ağınızın **Microsoft.Sql** türü adı olup olmadığından emin olabilirsiniz. Aşağıdaki eylemleri yapmak için aşağıdaki PowerShell komut dosyasını çalıştırabilirsiniz:

1. Alt ağınızın **Microsoft.Sql** türü adı olup olmadığını sapta.
2. İsteğe bağlı olarak, yoksa tür adını atayın.
    - Komut dosyası, yok olan tür adını uygulamadan önce *onaylamanızı*ister.

### <a name="phases-of-the-script"></a>Komut dosyasının aşamaları

PowerShell komut dosyasının aşamaları şunlardır:

1. PS oturumu başına yalnızca bir kez gereken Azure hesabınıza giriş yapın.  Değişkenleri atayın.
2. Sanal ağınızı ve ardından alt ağınızı arayın.
3. Alt ağınız **Microsoft.Sql** uç noktası sunucu türü olarak etiketlendi mi?
4. Alt netinize **Microsoft.Sql**türü adının Sanal Hizmet bitiş noktası ekleyin.

> [!IMPORTANT]
> Bu komut dosyasını çalıştırmadan önce, komut dosyasının üst kısmında ki $değişkenlerine atanan değerleri yeniden atamalısınız.

### <a name="direct-powershell-source-code"></a>Doğrudan PowerShell kaynak kodu

Bu PowerShell komut dosyası, onay için sizden sorulması halinde evet yanıtını vermedikçe hiçbir şeyi güncellemez. Komut dosyası, alt ağınıza **Microsoft.Sql** türü adını ekleyebilir. Ancak komut dosyası, yalnızca alt ağınızda tür adı yoksa eklemeyi dener.

```powershell
### 1. LOG into to your Azure account, needed only once per PS session.  Assign variables.
$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]';
if ('yes' -eq $yesno) { Connect-AzAccount; }

# Assignments to variables used by the later scripts.
# You can EDIT these values, if necessary.

$SubscriptionName  = 'yourSubscriptionName';
Select-AzSubscription -SubscriptionName "$SubscriptionName";

$ResourceGroupName   = 'yourRGName';
$VNetName            = 'yourVNetName';
$SubnetName          = 'yourSubnetName';
$SubnetAddressPrefix = 'Obtain this value from the Azure portal.'; # Looks roughly like: '10.0.0.0/24'

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql';  # Do NOT edit. Is official value.

### 2. Search for your virtual network, and then for your subnet.
# Search for the virtual network.
$vnet = $null;
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

if ($vnet -eq $null) {
    Write-Host "Caution: No virtual network found by the name '$VNetName'.";
    Return;
}

$subnet = $null;
for ($nn=0; $nn -lt $vnet.Subnets.Count; $nn++) {
    $subnet = $vnet.Subnets[$nn];
    if ($subnet.Name -eq $SubnetName)
    { break; }
    $subnet = $null;
}

if ($subnet -eq $null) {
    Write-Host "Caution: No subnet found by the name '$SubnetName'";
    Return;
}

### 3. Is your subnet tagged as 'Microsoft.Sql' endpoint server type?
$endpointMsSql = $null;
for ($nn=0; $nn -lt $subnet.ServiceEndpoints.Count; $nn++) {
    $endpointMsSql = $subnet.ServiceEndpoints[$nn];
    if ($endpointMsSql.Service -eq $ServiceEndpointTypeName_SqlDb) {
        $endpointMsSql;
        break;
    }
    $endpointMsSql = $null;
}

if ($endpointMsSql -ne $null) {
    Write-Host "Good: Subnet found, and is already tagged as an endpoint of type '$ServiceEndpointTypeName_SqlDb'.";
    Return;
}
else {
    Write-Host "Caution: Subnet found, but not yet tagged as an endpoint of type '$ServiceEndpointTypeName_SqlDb'.";

    # Ask the user for confirmation.
    $yesno = Read-Host 'Do you want the PS script to apply the endpoint type name to your subnet?  [yes/no]';
    if ('no' -eq $yesno) { Return; }
}

### 4. Add a Virtual Service endpoint of type name 'Microsoft.Sql', on your subnet.
$vnet = Set-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -VirtualNetwork  $vnet `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

# Persist the subnet update.
$vnet = Set-AzVirtualNetwork `
  -VirtualNetwork $vnet;

for ($nn=0; $nn -lt $vnet.Subnets.Count; $nn++) {
    $vnet.Subnets[0].ServiceEndpoints; }  # Display.
```

<!-- Link references: -->
[sql-db-vnet-service-endpoint-rule-overview-735r]: sql-database-vnet-service-endpoint-rule-overview.md
[http-azure-portal-link-ref-477t]: https://portal.azure.com/
