---
title: Azure SQL veritabanı ve veri ambarı için bağlantı ayarları
description: Bu belgede, Azure SQL veritabanı ve Azure SYNAPSE Analytics için Aktarım Katmanı Güvenliği (TLS) sürüm seçimi ve ara sunucu ile yeniden yönlendirme ayarı açıklanmaktadır
services: sql-database
ms.service: sql-database
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 07/06/2020
ms.openlocfilehash: 6297f7797758069ee75fe14960d72f9c497b0fe6
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87132985"
---
# <a name="azure-sql-connectivity-settings"></a>Azure SQL Bağlantı Ayarları
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

Bu makalede, Azure SQL veritabanı ve Azure SYNAPSE Analytics için sunucuya bağlantıyı denetleyen ayarlar açıklanır. Bu ayarlar, sunucuyla ilişkili **Tüm** SQL veritabanı ve Azure SYNAPSE veritabanları için geçerlidir.

> [!IMPORTANT]
> Bu makale, **Azure SQL yönetilen örneği** *için uygulanmıyor*

Bağlantı ayarlarına aşağıdaki ekran görüntüsünde gösterildiği gibi **güvenlik duvarları ve sanal ağlar** ekranından erişilebilir:

 ![Bağlantı ayarlarının ekran görüntüsü][1]

> [!NOTE]
> Bu ayarlar uygulandıktan sonra, **hemen etkili olur** ve her bir ayar için gereksinimleri karşılamayan istemcileriniz için bağlantı kaybına neden olabilir.

## <a name="deny-public-network-access"></a>Ortak ağ erişimini reddetme

**Ortak ağ erişimini reddet** ayarı **Evet**olarak ayarlandığında yalnızca özel uç noktalar aracılığıyla bağlantılara izin verilir. Bu ayar **Hayır** (varsayılan) olarak ayarlandığında, istemciler genel uç noktaları (IP tabanlı güvenlik duvarı KURALLARı, VNET tabanlı güvenlik duvarı kuralları) veya özel uç noktalar (özel bağlantı kullanarak) [ağ erişimi genel bakış](network-access-controls-overview.md)bölümünde özetlenen şekilde bağlanabilir. 

 ![Ortak ağ erişimini reddetme ile bağlantının ekran görüntüsü][2]

Mantıksal sunucuda var olan özel uç noktaları olmadan **ortak ağ erişimini reddet** ayarını **Evet** olarak ayarlama girişimleri şuna benzer bir hata iletisiyle başarısız olur:  

```output
Error 42102
Unable to set Deny Public Network Access to Yes since there is no private endpoint enabled to access the server. 
Please set up private endpoints and retry the operation. 
```

**Ortak ağ erişimini reddet** ayarı **Evet**olarak ayarlandığında, yalnızca özel uç noktalar aracılığıyla bağlantılara izin verilir ve genel uç noktalar aracılığıyla tüm bağlantılar şuna benzer bir hata iletisiyle reddedilir:  

```output
Error 47073
An instance-specific error occurred while establishing a connection to SQL Server. 
The public network interface on this server is not accessible. 
To connect to this server, use the Private Endpoint from inside your virtual network.
```

**Ortak ağ erişimini reddet** ayarı **Evet**olarak ayarlandığında, güvenlik duvarı kuralları ekleme veya güncelleştirme girişimleri aşağıdakine benzer bir hata iletisiyle reddedilir:

```output
Error 42101
Unable to create or modify firewall rules when public network interface for the server is disabled. 
To manage server or database level firewall rules, please enable the public network interface.
```

## <a name="change-public-network-access-via-powershell"></a>PowerShell aracılığıyla genel ağ erişimini değiştirme

> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Aşağıdaki betik [Azure PowerShell modülünü](/powershell/azure/install-az-ps)gerektiriyor.

Aşağıdaki PowerShell betiği, `Get` sunucu düzeyinde nasıl ve `Set` **genel ağ erişim** özelliğinin nasıl yapılacağını gösterir:

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

Mevcut olduğunda, TLS 1,0, 1,1 ve 1,2 destekliyoruz. Minimum TLS sürümü ayarlandığında, daha yeni olan TLS sürümlerinin desteklenmesini sağlar. Örneğin, 1,1 'den büyük bir TLS sürümü seçme. Yalnızca TLS 1,1 ve 1,2 arasındaki bağlantıların kabul edildiği ve TLS 1,0 reddedildiği anlamına gelir. Uygulamalarınızın bu uygulamayı desteklediğini doğrulamak için test ettikten sonra, önceki sürümlerde bulunan güvenlik açıklarına yönelik düzeltmeler içerdiğinden ve Azure SQL veritabanı 'nda desteklenen en yüksek TLS sürümü olduğundan, en düşük TLS sürümünü 1,2 olarak ayarlamayı öneririz.

> [!IMPORTANT]
> En az TLS sürümü için varsayılan değer tüm sürümlere izin verdir. Ancak, bir TLS sürümünü zorladıktan sonra, varsayılana dönmek mümkün değildir.

TLS 'nin eski sürümlerini kullanan uygulamalar için, uygulamalarınızın gereksinimlerine göre en düşük TLS sürümünü ayarlamayı öneririz. Şifrelenmemiş bir bağlantı kullanarak bağlanmak için uygulamalara bağlı olan müşteriler için, en az TLS sürümü ayarlamamız önerilir.

Daha fazla bilgi için bkz. [SQL veritabanı bağlantısı Için TLS konuları](connect-query-content-reference-guide.md#tls-considerations-for-database-connectivity).

En düşük TLS sürümünü ayarladıktan sonra, sunucunun en düşük TLS sürümünü kullanarak bir TLS sürümü kullanan istemcilerden gelen oturum açma girişimleri aşağıdaki hatayla başarısız olur:

```output
Error 47072
Login failed with invalid TLS version
```

## <a name="set-minimal-tls-version-via-powershell"></a>PowerShell ile en az TLS sürümü ayarla

> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Aşağıdaki betik [Azure PowerShell modülünü](/powershell/azure/install-az-ps)gerektiriyor.

Aşağıdaki PowerShell betiği, `Get` `Set` mantıksal sunucu düzeyinde nasıl ve **en az TLS sürümü** özelliğinin olduğunu gösterir:

```powershell
#Get the Minimal TLS Version property
(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).MinimalTlsVersion

# # Update Minimal TLS Version to 1.2
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


## <a name="change-connection-policy"></a>Bağlantı ilkesini değiştirme

[Bağlantı ilkesi](connectivity-architecture.md#connection-policy) , ISTEMCILERIN Azure SQL veritabanına nasıl bağlanacağını belirler.


## <a name="change-connection-policy-via-powershell"></a>Bağlantı ilkesini PowerShell aracılığıyla değiştirme

> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Aşağıdaki betik [Azure PowerShell modülünü](/powershell/azure/install-az-ps)gerektiriyor.

Aşağıdaki PowerShell betiği, PowerShell kullanılarak bağlantı ilkesinin nasıl değiştirileceğini göstermektedir:

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id -ApiVersion 2014-04-01 -Verbose).Properties.ConnectionType

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

- Azure SQL veritabanı 'nda bağlantının nasıl çalıştığına ilişkin genel bilgiler için, [bağlantı mimarisi](connectivity-architecture.md) ' ne bakın
- Sunucu için bağlantı ilkesini değiştirme hakkında daha fazla bilgi için bkz. [Conn-Policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).

<!--Image references-->
[1]: media/single-database-create-quickstart/manage-connectivity-settings.png
[2]: media/single-database-create-quickstart/manage-connectivity-flowchart.png
