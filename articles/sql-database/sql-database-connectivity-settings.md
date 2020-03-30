---
title: Azure SQL Veritabanı ve Veri Ambarı için bağlantı ayarları
description: Bu belge, TLS sürüm seçimini ve Azure SQL için Proxy ve Yönlendirme ayarını açıklar
services: sql-database
ms.service: sql-database
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 03/09/2020
ms.openlocfilehash: d18fdee85bd0fbabe68fe9890c4a2dc74366041d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366096"
---
# <a name="azure-sql-connectivity-settings"></a>Azure SQL Bağlantı Ayarları
> [!NOTE]
> Bu makale, Azure SQL sunucusu ve Azure SQL sunucusunda oluşturulan SQL Veritabanı ve SQL Veri Ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

> [!IMPORTANT]
> Bu makale, **Azure SQL Veritabanı Yönetilen Örnek** için geçerli *değildir*

Bu makalede, sunucu düzeyinde Azure SQL Veritabanına bağlantıyı denetleyen ayarlar tanıtılmaktadır. Bu ayarlar, sunucuyla ilişkili **tüm** SQL Veritabanı ve SQL Veri Ambarı veritabanları için geçerlidir.

> [!NOTE]
> Bu ayarlar uygulandıktan sonra, **hemen etkili olurlar** ve her ayar için gereksinimleri karşılamadıkları takdirde müşterileriniz için bağlantı kaybına neden olabilir.

Bağlantı ayarlarına, aşağıdaki ekran görüntüsünde gösterildiği gibi **Güvenlik Duvarları ve sanal ağlar** bıçağından erişilebilir:

 ![Bağlantı ayarlarının ekran görüntüsü][1]


## <a name="deny-public-network-access"></a>Ortak ağ erişimini reddetme
Azure portalında, **Genel Ağ erişimini Reddet** ayarı **Evet**olarak ayarlandığında, yalnızca özel uç noktalar üzerinden bağlantılara izin verilir. Bu ayar **Hayır**olarak ayarlandığında, istemciler özel veya genel bitiş noktasını kullanarak bağlanabilir.

Ortak **ağ erişimini** **Evet'e**ayarı yaptıktan sonra, ortak bitiş noktasını kullanan istemcilerden giriş denemeleri aşağıdaki hatayla başarısız olur:

```output
Error 47073
An instance-specific error occurred while establishing a connection to SQL Server. The public network interface on this server is not accessible. To connect to this server, use the Private Endpoint from inside your virtual network.
```

## <a name="change-public-network-access-via-powershell"></a>PowerShell ile Ortak Ağ Erişimini Değiştirme
[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Aşağıdaki komut dosyası [Azure PowerShell modülgerektirir.](/powershell/azure/install-az-ps)

Aşağıdaki PowerShell komut dosyası, `Set` mantıksal sunucu düzeyinde Public Network Access özelliğinin nasıl yapılacağını ve **Public Network Access** özelliğinin nasıl yapılacağını `Get` gösterir:

```powershell
#Get the Public Network Access property
(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).PublicNetworkAccess

# Update Public Network Access to Disabled
$SecureString = ConvertTo-SecureString "password" -AsPlainText -Force

Set-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group -SqlAdministratorPassword $SecureString -PublicNetworkAccess "Enabled" 
```

## <a name="change-public-network-access-via-cli"></a>CLI ile Ortak Ağ Erişimini Değiştirme
> [!IMPORTANT]
> Bu bölümdeki tüm komut dosyaları [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)gerektirir.

### <a name="azure-cli-in-a-bash-shell"></a>Bir bash kabuk azure CLI
Aşağıdaki CLI komut dosyası, **Genel Ağ Erişiminin** bash kabuğunda nasıl değiştirilebildiğini gösterir:

```azurecli-interactive

# Get current setting for Public Network Access
az sql server show -n sql-server-name -g sql-server-group --query "publicNetworkAccess"

# Update setting for Public Network Access
az sql server update -n sql-server-name -g sql-server-group --set publicNetworkAccess="Disabled"

```


## <a name="connection-policy"></a>Bağlantı ilkesi
[Bağlantı ilkesi,](sql-database-connectivity-architecture.md#connection-policy) istemcilerin Azure SQL Server'a nasıl bağlandığını belirler. 

## <a name="change-connection-policy-via-powershell"></a>PowerShell ile Bağlantı politikasını değiştir
[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Aşağıdaki komut dosyası [Azure PowerShell modülgerektirir.](/powershell/azure/install-az-ps)

Aşağıdaki PowerShell komut dosyası, PowerShell'i kullanarak bağlantı ilkesinin nasıl değiştirilebildiğini gösterir:

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="change-connection-policy-via-azure-cli"></a>Azure CLI ile Bağlantı ilkesini değiştirme
> [!IMPORTANT]
> Bu bölümdeki tüm komut dosyaları [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)gerektirir.

### <a name="azure-cli-in-a-bash-shell"></a>Bir bash kabuk azure CLI
Aşağıdaki CLI komut dosyası, bir bash kabuğundaki bağlantı ilkesinin nasıl değiştirilebildiğini gösterir: 

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
ids="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $ids

# Update connection policy
az resource update --ids $ids --set properties.connectionType=Proxy
```

### <a name="azure-cli-from-a-windows-command-prompt"></a>Windows komut isteminden Azure CLI
Aşağıdaki CLI komut dosyası, bağlantı ilkesinin Windows komut isteminden (Azure CLI yüklü olarak) nasıl değiştirilebildiğini gösterir.

```azurecli
# Get SQL Server ID and set URI
FOR /F "tokens=*" %g IN ('az sql server show --resource-group myResourceGroup-571418053 --name server-538465606 --query "id" -o tsv') do (SET sqlserverid=%g/connectionPolicies/Default)

# Get current connection policy
az resource show --ids %sqlserverid%

# Update connection policy
az resource update --ids %sqlserverid% --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Sonraki adımlar
- Azure SQL Veritabanı'nda bağlantının nasıl çalıştığına genel bir bakış için [Azure SQL Bağlantı Mimarisi'ne](sql-database-connectivity-architecture.md) bakın
- Azure SQL Veritabanı sunucusu için Azure SQL Veritabanı bağlantı ilkesini nasıl değiştireceğiniz hakkında bilgi için [conn-policy'ye](https://docs.microsoft.com/cli/azure/sql/server/conn-policy)bakın.

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/manage-connectivity-settings.png
