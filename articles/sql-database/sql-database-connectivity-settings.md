---
title: Azure SQL veritabanı ve veri ambarı için bağlantı ayarları
description: Bu belgede, Azure SQL için TLS sürüm seçimi ve proxy ile yeniden yönlendirme ayarı açıklanmaktadır
services: sql-database
ms.service: sql-database
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 03/09/2020
ms.openlocfilehash: ef846b23bf6c9da2b7dd64927eac7cc1a1704dae
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684940"
---
# <a name="azure-sql-connectivity-settings"></a>Azure SQL Bağlantı Ayarları
> [!NOTE]
> Bu makale, Azure 'da mantıksal sunucuda oluşturulan hem Azure SQL veritabanı hem de SQL veri ambarı veritabanları için kullanılan mantıksal SQL Server için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

> [!IMPORTANT]
> Bu makale **Azure SQL veritabanı yönetilen örneği** *için uygulanmıyor*

Bu makalede, sunucu düzeyinde Azure SQL veritabanı bağlantısını denetleyen ayarlar açıklanır. Bu ayarlar, sunucuyla ilişkili **Tüm** SQL VERITABANı ve SQL veri ambarı veritabanları için geçerlidir.

> [!NOTE]
> Bu ayarlar uygulandıktan sonra, **hemen etkili olur** ve her bir ayar için gereksinimleri karşılamayan istemcileriniz için bağlantı kaybına neden olabilir.

Bağlantı ayarlarına aşağıdaki ekran görüntüsünde gösterildiği gibi **güvenlik duvarları ve sanal ağlar** ekranından erişilebilir:

 ![Bağlantı ayarlarının ekran görüntüsü][1]


## <a name="deny-public-network-access"></a>Ortak ağ erişimini reddetme

Müşteriler, genel uç noktaları (IP tabanlı güvenlik duvarı kuralları, VNET tabanlı güvenlik duvarı kuralları) veya özel uç noktalar (özel bağlantı kullanarak) [ağ erişimine genel bakış](sql-database-networkaccess-overview.md)bölümünde açıklandığı gıbı, SQL veritabanı 'na bağlanabilir. 

**Ortak ağ erişimini reddet** ayarı **Evet**olarak ayarlandığında, yalnızca özel uç noktalar aracılığıyla bağlantılara izin verilir ve genel uç noktalar aracılığıyla tüm bağlantılar şuna benzer bir hata iletisiyle reddedilir:  

```output
Error 47073
An instance-specific error occurred while establishing a connection to SQL Server. 
The public network interface on this server is not accessible. 
To connect to this server, use the Private Endpoint from inside your virtual network.
```

## <a name="change-public-network-access-via-powershell"></a>PowerShell aracılığıyla genel ağ erişimini değiştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Aşağıdaki betik [Azure PowerShell modülünü](/powershell/azure/install-az-ps)gerektiriyor.

Aşağıdaki PowerShell betiği, `Get` `Set` mantıksal sunucu düzeyinde nasıl ve **genel ağ erişim** özelliğinin nasıl yapılacağını gösterir:

```powershell
#Get the Public Network Access property
(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).PublicNetworkAccess

# Update Public Network Access to Disabled
$SecureString = ConvertTo-SecureString "password" -AsPlainText -Force

Set-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group -SqlAdministratorPassword $SecureString -PublicNetworkAccess "Enabled" 
```

## <a name="change-public-network-access-via-cli"></a>CLı aracılığıyla ortak ağ erişimini değiştirme

> [!IMPORTANT]
> Bu bölümdeki tüm betikler [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)gerektirir.

### <a name="azure-cli-in-a-bash-shell"></a>Bash kabuğu 'nda Azure CLı

Aşağıdaki CLı betiği, bir bash kabuğu 'ndaki **genel ağ erişiminin** nasıl değiştirileceğini göstermektedir:

```azurecli-interactive

# Get current setting for Public Network Access
az sql server show -n sql-server-name -g sql-server-group --query "publicNetworkAccess"

# Update setting for Public Network Access
az sql server update -n sql-server-name -g sql-server-group --set publicNetworkAccess="Disabled"

```

## <a name="minimal-tls-version"></a>En az TLS sürümü 

Minimum [Aktarım Katmanı Güvenliği (TLS)](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server) sürümü ayarı, MÜŞTERILERIN Azure SQL veritabanı tarafından kullanılan TLS sürümünü denetlemesine olanak tanır.

Şu anda TLS 1,0, 1,1 ve 1,2 destekliyoruz. Minimum TLS sürümü ayarlandığında, daha yeni olan TLS sürümlerinin desteklenmesini sağlar. Örneğin, örneğin, 1,1 'den büyük bir TLS sürümü seçme. Yalnızca TLS 1,1 ve 1,2 arasındaki bağlantıların kabul edildiği ve TLS 1,0 reddedildiği anlamına gelir. Uygulamalarınızın bu uygulamayı desteklediğini doğrulamak için test ettikten sonra, önceki sürümlerde bulunan güvenlik açıklarına yönelik düzeltmeler içerdiğinden ve Azure SQL veritabanı 'nda desteklenen en yüksek TLS sürümü olduğundan, en düşük TLS sürümünü 1,2 olarak ayarlamayı öneririz.

TLS 'nin eski sürümlerini kullanan uygulamalar için, uygulamalarınızın gereksinimlerine göre en düşük TLS sürümünü ayarlamayı öneririz. Şifrelenmemiş bir bağlantı kullanarak bağlanmak için uygulamalara bağlı olan müşteriler için, en az TLS sürümü ayarlamamız önerilir. 

Daha fazla bilgi için bkz. [SQL veritabanı bağlantısı Için TLS konuları](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

En düşük TLS sürümünü ayarladıktan sonra, sunucunun en düşük TLS sürümünü kullanarak bir TLS sürümü kullanan istemcilerden gelen oturum açma girişimleri aşağıdaki hatayla başarısız olur:

```output
Error 47072
Login failed with invalid TLS version
```

## <a name="set-minimal-tls-version-via-powershell"></a>PowerShell ile en az TLS sürümü ayarla

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Aşağıdaki betik [Azure PowerShell modülünü](/powershell/azure/install-az-ps)gerektiriyor.

Aşağıdaki PowerShell betiği, `Get` `Set` mantıksal sunucu düzeyinde nasıl ve **en az TLS sürümü** özelliğinin olduğunu gösterir:

```powershell
#Get the Public Network Access property
(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).PublicNetworkAccess

# Update Public Network Access to Disabled
$SecureString = ConvertTo-SecureString "password" -AsPlainText -Force

Set-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group -SqlAdministratorPassword $SecureString  -MinimalTlsVersion "1.2"
```

## <a name="set-minimal-tls-version-via-azure-cli"></a>Azure CLı ile en az TLS sürümü ayarlama

> [!IMPORTANT]
> Bu bölümdeki tüm betikler [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)gerektirir.

### <a name="azure-cli-in-a-bash-shell"></a>Bash kabuğu 'nda Azure CLı

Aşağıdaki CLı betiği, bir bash kabuğunda **En düşük TLS sürümü** ayarının nasıl değiştirileceğini göstermektedir:

```azurecli-interactive
# Get current setting for Minimal TLS Version
az sql server show -n sql-server-name -g sql-server-group --query "minimalTlsVersion"

# Update setting for Minimal TLS Version
az sql server update -n sql-server-name -g sql-server-group --set minimalTlsVersion="1.2"
```

## <a name="connection-policy"></a>Bağlantı ilkesi

[Bağlantı ilkesi](sql-database-connectivity-architecture.md#connection-policy) , istemcilerin Azure SQL Server nasıl bağlanacağını belirler. 

## <a name="change-connection-policy-via-powershell"></a>Bağlantı ilkesini PowerShell aracılığıyla değiştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Aşağıdaki betik [Azure PowerShell modülünü](/powershell/azure/install-az-ps)gerektiriyor.

Aşağıdaki PowerShell betiği, PowerShell kullanılarak bağlantı ilkesinin nasıl değiştirileceğini göstermektedir:

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

## <a name="change-connection-policy-via-azure-cli"></a>Azure CLı aracılığıyla bağlantı ilkesini değiştirme

> [!IMPORTANT]
> Bu bölümdeki tüm betikler [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)gerektirir.

### <a name="azure-cli-in-a-bash-shell"></a>Bash kabuğu 'nda Azure CLı
Aşağıdaki CLı betiği, bir bash kabuğu 'nda bağlantı ilkesinin nasıl değiştirileceğini göstermektedir: 

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

### <a name="azure-cli-from-a-windows-command-prompt"></a>Windows komut isteminden Azure CLı

Aşağıdaki CLı betiği, bir Windows komut isteminden (Azure CLı yüklü) bağlantı ilkesinin nasıl değiştirileceğini gösterir.

```azurecli
# Get SQL Server ID and set URI
FOR /F "tokens=*" %g IN ('az sql server show --resource-group myResourceGroup-571418053 --name server-538465606 --query "id" -o tsv') do (SET sqlserverid=%g/connectionPolicies/Default)

# Get current connection policy
az resource show --ids %sqlserverid%

# Update connection policy
az resource update --ids %sqlserverid% --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı 'nda bağlantının nasıl çalıştığına ilişkin bir genel bakış için [Azure SQL bağlantı mimarisi](sql-database-connectivity-architecture.md) ' ne bakın.
- Azure SQL veritabanı sunucusu için Azure SQL veritabanı bağlantı ilkesini değiştirme hakkında daha fazla bilgi için bkz. [Conn-Policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/manage-connectivity-settings.png
