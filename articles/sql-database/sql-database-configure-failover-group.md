---
title: Bir başarısız grubu yapılandırma
description: Azure portalı, Az CLI ve PowerShell'i kullanarak Azure SQL Veritabanı tek veritabanı, elastik havuz ve yönetilen örnek için otomatik hata grubu nasıl yapılandırılabildiğini öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 08/14/2019
ms.openlocfilehash: 3b423a25b6b13ad543ef4a74bc0335ce19f5766d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461824"
---
# <a name="configure-a-failover-group-for-azure-sql-database"></a>Azure SQL Veritabanı için bir başarısız grup yapılandırma

Bu konu, Azure portalını veya PowerShell'i kullanarak bir Azure SQL Veritabanı tek veritabanı, elastik havuz ve yönetilen örnek için [otomatik hata grubunu](sql-database-auto-failover-group.md) nasıl yapılandırabileceğinizi öğretir. 

## <a name="single-database"></a>Tek veritabanı
Azure portalını veya PowerShell'i kullanarak başarısız grup oluşturun ve veritabanına tek bir veritabanı ekleyin.

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki ön koşulları göz önünde bulundurun:

- İkincil sunucunun sunucu girişi ve güvenlik duvarı ayarları birincil sunucunuzunkiyle eşleşmelidir. 

### <a name="create-failover-group"></a>Başarısız grup oluşturma

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalını kullanarak başarısız grubunuzu oluşturun ve tek veritabanınızı bu gruba ekleyin.


1. [Azure portalının](https://portal.azure.com)sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. Failover grubuna eklemek istediğiniz tek veritabanını seçin. 
1. **Sunucunun** ayarlarını açmak için Sunucu adı altında sunucunun adını seçin.

   ![Tek db için açık sunucu](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. **Ayarlar** bölmesinin altındaki **Failover gruplarını** seçin ve ardından yeni bir başarısız grup oluşturmak için **Grup Ekle'yi** seçin. 

    ![Yeni başarısız grup ekleme](media/sql-database-single-database-failover-group-tutorial/sqldb-add-new-failover-group.png)

1. **Failover Grubu** sayfasında, gerekli değerleri girin veya seçin ve sonra **Oluştur'u**seçin.

   - **Grup içindeki veritabanları**: Failover grubuna eklemek istediğiniz veritabanını seçin. Veritabanının başarısız gruba eklenmesi, coğrafi çoğaltma işlemini otomatik olarak başlatır. 
        
    ![Başarısız gruba SQL DB ekleme](media/sql-database-single-database-failover-group-tutorial/add-sqldb-to-failover-group.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
PowerShell'i kullanarak başarısız grubunuzu oluşturun ve tek veritabanınızı buna ekleyin. 

   ```powershell-interactive
   $subscriptionId = "<SubscriptionID>"
   $resourceGroupName = "<Resource-Group-Name>"
   $location = "<Region>"
   $adminLogin = "<Admin-Login>"
   $password = "<Complex-Password>"
   $serverName = "<Primary-Server-Name>"
   $databaseName = "<Database-Name>"
   $drLocation = "<DR-Region>"
   $drServerName = "<Secondary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Create a secondary server in the failover region
   Write-host "Creating a secondary logical server in the failover region..."
   $drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $drServer
   
   # Create a failover group between the servers
   $failovergroup = Write-host "Creating a failover group between the primary and secondary server..."
   New-AzSqlDatabaseFailoverGroup `
      –ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -PartnerServerName $drServerName  `
      –FailoverGroupName $failoverGroupName `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $failovergroup
   
   # Add the database to the failover group
   Write-host "Adding the database to the failover group..." 
   Get-AzSqlDatabase `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName | `
   Add-AzSqlDatabaseToFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Successfully added the database to the failover group..." 
   ```

---

### <a name="test-failover"></a>Yük devretme testi 

Azure portalını veya PowerShell'i kullanarak başarısız grubunuzun başarısız olup bittiğini test edin. 

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak başarısız grubunuzun başarısız olduğunu test edin. 

1. [Azure portalının](https://portal.azure.com)sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. Failover grubuna eklemek istediğiniz tek veritabanını seçin. 

   ![Tek db için açık sunucu](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. **Ayarlar** bölmesinin altındaki **Failover gruplarını** seçin ve ardından yeni oluşturduğunuz başarısız grubu seçin. 
  
   ![Portaldan başarısız grubu seçin](media/sql-database-single-database-failover-group-tutorial/select-failover-group.png)

1. Hangi sunucunun birincil ve hangi sunucunun ikincil olduğunu gözden geçirin. 
1. Tek veritabanınızı içeren failover grubunuzun üzerinde başarısız olmak için görev bölmesinden **Failover'ı** seçin. 
1. TDS oturumlarının bağlantısının kesilmeyeceğini size belirten uyarıda **Evet'i** seçin. 

   ![SQL veritabanınızı içeren başarısız grubunuzun üzerinde başarısız olun](media/sql-database-single-database-failover-group-tutorial/failover-sql-db.png)

1. Hangi sunucunun artık birincil ve hangi sunucunun ikincil olduğunu gözden geçirin. Failover başarılı olduysa, iki sunucu rolleri değiş tokuş etmiş olmalıdır. 
1. Sunucuları asıl rollerine geri getirmek için **Failover'ı** yeniden seçin. 

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak başarısız grubunuzun başarısız olduğunu test edin.  

İkincil yinelemenin rolünü denetleyin: 

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"
   
   # Check role of secondary replica
   Write-host "Confirming the secondary replica is secondary...." 
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   ```
İkincil sunucuya başarısız olabilir: 

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"
   
   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group to successfully to" $drServerName 
   ```

Failover grubunu birincil sunucuya geri döndürün:

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"
   
   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully to back to" $serverName
   ```

---

> [!IMPORTANT]
> İkincil veritabanını silmeniz gerekiyorsa, silmeden önce veritabanını failover grubundan kaldırın. Başarısız gruptan kaldırılmadan önce ikincil bir veritabanının silinmesi öngörülemeyen davranışlara neden olabilir. 

## <a name="elastic-pool"></a>Elastik havuz
Azure portalını veya PowerShell'i kullanarak başarısız grubu oluşturun ve ona elastik bir havuz ekleyin.  

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki ön koşulları göz önünde bulundurun:

- İkincil sunucunun sunucu girişi ve güvenlik duvarı ayarları birincil sunucunuzunkiyle eşleşmelidir. 

### <a name="create-the-failover-group"></a>Başarısız grup oluşturma 

Azure portalını veya PowerShell'i kullanarak esnek havuzunuz için başarısız grup oluşturun. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalını kullanarak başarısız grubunuzu oluşturun ve elastik havuzunuzu ekleyin.

1. [Azure portalının](https://portal.azure.com)sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. Başarısız gruba eklemek istediğiniz elastik havuzu seçin. 
1. Genel **Bakış** bölmesindeki sunucunun ayarlarını açmak için Sunucu adı altında **sunucunun** adını seçin.
  
    ![Elastik havuz için açık sunucu](media/sql-database-elastic-pool-failover-group-tutorial/server-for-elastic-pool.png)

1. **Ayarlar** bölmesinin altındaki **Failover gruplarını** seçin ve ardından yeni bir başarısız grup oluşturmak için **Grup Ekle'yi** seçin. 

    ![Yeni başarısız grup ekleme](media/sql-database-single-database-failover-group-tutorial/sqldb-add-new-failover-group.png)

1. **Failover Grubu** sayfasında, gerekli değerleri girin veya seçin ve sonra **Oluştur'u**seçin. Yeni bir ikincil sunucu oluşturun veya varolan bir ikincil sunucu seçin. 

1. **Grup içindeki Veritabanları'nı** seçin ve ardından failover grubuna eklemek istediğiniz esnek havuzu seçin. İkincil sunucuda zaten elastik bir havuz yoksa, ikincil sunucuda elastik bir havuz oluşturmanızı isteyen bir uyarı görüntülenir. Uyarıyı seçin ve ardından ikincil sunucuda elastik havuz oluşturmak için **Tamam'ı** seçin. 
        
    ![Başarısız gruba elastik havuz ekleme](media/sql-database-elastic-pool-failover-group-tutorial/add-elastic-pool-to-failover-group.png)
        
1. Elastik havuz ayarlarınızı başarısız gruba uygulamak için **Seç'i** seçin ve ardından başarısız grubunuzu oluşturmak için **Oluştur'u** seçin. Başarısız gruba elastik havuz eklemek, coğrafi çoğaltma işlemini otomatik olarak başlatır. 

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak başarısız grubunuzu oluşturun ve elastik havuzunuzu ekleyin. 

   ```powershell-interactive
   $subscriptionId = "<SubscriptionID>"
   $resourceGroupName = "<Resource-Group-Name>"
   $location = "<Region>"
   $adminLogin = "<Admin-Login>"
   $password = "<Complex-Password>"
   $serverName = "<Primary-Server-Name>"
   $databaseName = "<Database-Name>"
   $poolName = "myElasticPool"
   $drLocation = "<DR-Region>"
   $drServerName = "<Secondary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Create a failover group between the servers
   Write-host "Creating failover group..." 
   New-AzSqlDatabaseFailoverGroup `
       –ResourceGroupName $resourceGroupName `
       -ServerName $serverName `
       -PartnerServerName $drServerName  `
       –FailoverGroupName $failoverGroupName `
       –FailoverPolicy Automatic `
       -GracePeriodWithDataLossHours 2
   Write-host "Failover group created successfully." 

   # Add elastic pool to the failover group
   Write-host "Enumerating databases in elastic pool...." 
   $FailoverGroup = Get-AzSqlDatabaseFailoverGroup `
                    -ResourceGroupName $resourceGroupName `
                    -ServerName $serverName `
                    -FailoverGroupName $failoverGroupName
   $databases = Get-AzSqlElasticPoolDatabase `
               -ResourceGroupName $resourceGroupName `
               -ServerName $serverName `
               -ElasticPoolName $poolName
   Write-host "Adding databases to failover group..." 
   $failoverGroup = $failoverGroup | Add-AzSqlDatabaseToFailoverGroup `
                                     -Database $databases 
   Write-host "Databases added to failover group successfully." 
  ```

---

### <a name="test-failover"></a>Yük devretme testi

Azure portalını veya PowerShell'i kullanarak elastik havuzunuzun başarısız olup olmamasını test edin. 

# <a name="portal"></a>[Portal](#tab/azure-portal)

Başarısız grupunuzu ikincil sunucuya getirin ve Azure portalını kullanarak başarısız olun. 

1. [Azure portalının](https://portal.azure.com)sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. Başarısız gruba eklemek istediğiniz elastik havuzu seçin. 
1. Genel **Bakış** bölmesindeki sunucunun ayarlarını açmak için Sunucu adı altında **sunucunun** adını seçin.
  
    ![Elastik havuz için açık sunucu](media/sql-database-elastic-pool-failover-group-tutorial/server-for-elastic-pool.png)
1. **Ayarlar** bölmesinin altındaki **Failover gruplarını** seçin ve ardından bölüm 2'de oluşturduğunuz başarısız grubu seçin. 
  
   ![Portaldan başarısız grubu seçin](media/sql-database-single-database-failover-group-tutorial/select-failover-group.png)

1. Hangi sunucunun birincil, hangi sunucunun ikincil olduğunu gözden geçirin. 
1. Elastik havuzunuzu içeren failover grubunuzda başarısız olmak için görev bölmesinden **Failover'ı** seçin. 
1. TDS oturumlarının bağlantısının kesilmeyeceğini size belirten uyarıda **Evet'i** seçin. 

   ![SQL veritabanınızı içeren başarısız grubunuzun üzerinde başarısız olun](media/sql-database-single-database-failover-group-tutorial/failover-sql-db.png)

1. Hangi sunucunun birincil, hangi sunucunun ikincil olduğunu gözden geçirin. Failover başarılı olduysa, iki sunucu rolleri değiş tokuş etmiş olmalıdır. 
1. Failover grubunu orijinal ayarlara geri getirmek için **failover'ı** yeniden seçin. 

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak başarısız grubunuzun başarısız olduğunu test edin.

İkincil yinelemenin rolünü denetleyin: 

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"
   
   # Check role of secondary replica
   Write-host "Confirming the secondary replica is secondary...." 
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   ```

İkincil sunucuya başarısız olabilir: 

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"
   
   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group to successfully to" $drServerName 
   ```

---

> [!IMPORTANT]
> İkincil veritabanını silmeniz gerekiyorsa, silmeden önce veritabanını failover grubundan kaldırın. Başarısız gruptan kaldırılmadan önce ikincil bir veritabanının silinmesi öngörülemeyen davranışlara neden olabilir. 

## <a name="managed-instance"></a>Yönetilen örnek

Azure portalını veya PowerShell'i kullanarak yönetilen iki örnek arasında bir başarısız lık grubu oluşturun. 

[ExpressRoute'u](../expressroute/expressroute-howto-circuit-portal-resource-manager.md) yapılandırmanız veya yönetilen her örneğin sanal ağı için bir ağ geçidi oluşturmanız, iki ağ geçidini bağlamanız ve ardından başarısız grup oluşturmanız gerekir. 

### <a name="prerequisites"></a>Ön koşullar
Aşağıdaki ön koşulları göz önünde bulundurun:

- İkincil yönetilen örnek boş olmalıdır.
- İkincil sanal ağ için alt ağ aralığı birincil sanal ağın alt ağ aralığıyla örtüşmemelidir. 
- İkincil örneğin harmanlama ve saat dilimi birincil örneğinkiyle eşleşmelidir. 
- İki ağ geçidini bağlarken, **Paylaşılan Anahtar** her iki bağlantı için de aynı olmalıdır. 

### <a name="create-primary-virtual-network-gateway"></a>Birincil sanal ağ ağ geçidi oluşturma 

[ExpressRoute'u](../expressroute/expressroute-howto-circuit-portal-resource-manager.md)yapılandırmadıysanız, Azure portalı veya PowerShell ile birincil sanal ağ ağ geçidini oluşturabilirsiniz. 

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak birincil sanal ağ ağ ağ geçidini oluşturun. 

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
    | **Adı** | Sanal ağ ağ geçidinizin adı. | 
    | **Bölge** | İkincil yönetilen örneğinizin olduğu bölge. |
    | **Ağ geçidi türü** | **VPN'i**seçin. |
    | **VPN Türü** | **Rota Tabanlı'yı** seçin |
    | **Sku**| Varsayılan ı `VpnGw1`bırakın. |
    | **Konum**| İkincil yönetilen örneğinizin ve ikincil sanal ağınızın olduğu konum.   |
    | **Sanal ağ**| İkincil yönetilen örneğiniz için sanal ağı seçin. |
    | **Genel IP adresi**| **Yeni oluştur**’u seçin. |
    | **Genel IP adresi adı**| IP adresiniz için bir ad girin. |
    | &nbsp; | &nbsp; |

1. Diğer değerleri varsayılan olarak bırakın ve ardından sanal ağ ağ ağ geçidinizin ayarlarını gözden geçirmek için **Gözden Geçir + oluştur'u** seçin.

   ![Birincil ağ geçidi ayarları](media/sql-database-managed-instance-failover-group-tutorial/settings-for-primary-gateway.png)

1. Yeni sanal ağ ağ ağınızı oluşturmak için **Oluştur'u** seçin. 

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak birincil sanal ağ ağ ağ geçidini oluşturun. 

   ```powershell-interactive
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $primaryVnetName = "<Primary-Virtual-Network-Name>"
   $primaryGWName = "<Primary-Gateway-Name>"
   $primaryGWPublicIPAddress = $primaryGWName + "-ip"
   $primaryGWIPConfig = $primaryGWName + "-ipc"
   $primaryGWAsn = 61000
   
   # Get the primary virtual network
   $vnet1 = Get-AzVirtualNetwork -Name $primaryVnetName -ResourceGroupName $primaryResourceGroupName
   $primaryLocation = $vnet1.Location
   
   # Create primary gateway
   Write-host "Creating primary gateway..."
   $subnet1 = Get-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet1
   $gwpip1= New-AzPublicIpAddress -Name $primaryGWPublicIPAddress -ResourceGroupName $primaryResourceGroupName `
            -Location $primaryLocation -AllocationMethod Dynamic
   $gwipconfig1 = New-AzVirtualNetworkGatewayIpConfig -Name $primaryGWIPConfig `
            -SubnetId $subnet1.Id -PublicIpAddressId $gwpip1.Id
    
   $gw1 = New-AzVirtualNetworkGateway -Name $primaryGWName -ResourceGroupName $primaryResourceGroupName `
       -Location $primaryLocation -IpConfigurations $gwipconfig1 -GatewayType Vpn `
       -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $true -Asn $primaryGWAsn
   $gw1
   ```

---

### <a name="create-secondary-virtual-network-gateway"></a>İkincil sanal ağ ağ geçidi oluşturma

Azure portalını veya PowerShell'i kullanarak ikincil sanal ağ ağ ağ geçidini oluşturun. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
İkincil yönetilen örnek için sanal ağ alt ağı ve ağ geçidi oluşturmak için önceki bölümdeki adımları yineleyin. İkincil yönetilen örneğiniz için ağ geçidini yapılandırmak için gerekli alanları doldurun. 

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

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak ikincil sanal ağ ağ ağ geçidini oluşturun. 

   ```powershell-interactive
   $secondaryResourceGroupName = "<Secondary-Resource-Group>"
   $secondaryVnetName = "<Secondary-Virtual-Network-Name>"
   $secondaryGWName = "<Secondary-Gateway-Name>"
   $secondaryGWPublicIPAddress = $secondaryGWName + "-IP"
   $secondaryGWIPConfig = $secondaryGWName + "-ipc"
   $secondaryGWAsn = 62000
   
   # Get the secondary virtual network
   $vnet2 = Get-AzVirtualNetwork -Name $secondaryVnetName -ResourceGroupName $secondaryResourceGroupName
   $secondaryLocation = $vnet2.Location
    
   # Create the secondary gateway
   Write-host "Creating secondary gateway..."
   $subnet2 = Get-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet2
   $gwpip2= New-AzPublicIpAddress -Name $secondaryGWPublicIPAddress -ResourceGroupName $secondaryResourceGroupName `
            -Location $secondaryLocation -AllocationMethod Dynamic
   $gwipconfig2 = New-AzVirtualNetworkGatewayIpConfig -Name $secondaryGWIPConfig `
            -SubnetId $subnet2.Id -PublicIpAddressId $gwpip2.Id
    
   $gw2 = New-AzVirtualNetworkGateway -Name $secondaryGWName -ResourceGroupName $secondaryResourceGroupName `
       -Location $secondaryLocation -IpConfigurations $gwipconfig2 -GatewayType Vpn `
       -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $true -Asn $secondaryGWAsn
   
   $gw2
   ```

---


### <a name="connect-the-gateways"></a>Ağ geçitlerini bağlama 
Azure portalını veya PowerShell'i kullanarak iki ağ geçidi arasında bağlantılar oluşturun. 

İki bağlantı oluşturulması gerekir : birincil ağ geçidinden ikincil ağ geçidine bağlantı ve ardından ikincil ağ geçidinden birincil ağ geçidine bağlantı. 

Her iki bağlantı için de kullanılan paylaşılan anahtar her bağlantı için aynı olmalıdır. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalını kullanarak iki ağ geçidi arasında bağlantılar oluşturun. 

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

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak iki ağ geçidi arasında bağlantılar oluşturun. 

   ```powershell-interactive
   $vpnSharedKey = "mi1mi2psk"
    
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $primaryGWConnection = "<Primary-connection-name>"
   $primaryLocation = "<Primary-Region>"
    
   $secondaryResourceGroupName = "<Secondary-Resource-Group>"
   $secondaryGWConnection = "<Secondary-connection-name>"
   $secondaryLocation = "<Secondary-Region>"
  
   # Connect the primary to secondary gateway
   Write-host "Connecting the primary gateway"
   New-AzVirtualNetworkGatewayConnection -Name $primaryGWConnection -ResourceGroupName $primaryResourceGroupName `
       -VirtualNetworkGateway1 $gw1 -VirtualNetworkGateway2 $gw2 -Location $primaryLocation `
       -ConnectionType Vnet2Vnet -SharedKey $vpnSharedKey -EnableBgp $true
   $primaryGWConnection
   
   # Connect the secondary to primary gateway
   Write-host "Connecting the secondary gateway"
   
   New-AzVirtualNetworkGatewayConnection -Name $secondaryGWConnection -ResourceGroupName $secondaryResourceGroupName `
       -VirtualNetworkGateway1 $gw2 -VirtualNetworkGateway2 $gw1 -Location $secondaryLocation `
       -ConnectionType Vnet2Vnet -SharedKey $vpnSharedKey -EnableBgp $true
   $secondaryGWConnection
   ```

---

### <a name="create-the-failover-group"></a>Başarısız grup oluşturma 
Azure portalını veya PowerShell'i kullanarak yönetilen örnekleriniz için başarısız lık grubu oluşturun. 

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak yönetilen örnekleriniz için başarısız lık grubu oluşturun. 

1. [Azure portalının](https://portal.azure.com)sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. Başarısız gruba eklemek istediğiniz birincil yönetilen örneği seçin.  
1. **Ayarlar** **altında, Örnek Failover Grupları'na** gidin ve ardından **Örnek Failover Grubu** sayfasını açmak için grup **eklemeyi** seçin. 

   ![Bir başarısız grup ekleme](media/sql-database-managed-instance-failover-group-tutorial/add-failover-group.png)

1. Instance **Failover Group** sayfasında, başarısız grubunuzun adını yazın ve ardından açılan alttan ikincil yönetilen örneği seçin. Başarısız grupoluşturmak için **Oluştur'u** seçin. 

   ![Başarısız grup oluşturma](media/sql-database-managed-instance-failover-group-tutorial/create-failover-group.png)

1. Başarısız grup dağıtımı tamamlandıktan sonra **Failover grup** sayfasına geri götürülür. 

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak yönetilen örnekleriniz için başarısız grup oluşturun. 

   ```powershell-interactive
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $failoverGroupName = "<Failover-Group-Name>"
   $primaryLocation = "<Primary-Region>"
   $secondaryLocation = "<Secondary-Region>"
   $primaryManagedInstance = "<Primary-Managed-Instance-Name>"
   $secondaryManagedInstance = "<Secondary-Managed-Instance-Name>"
   
   # Create failover group
   Write-host "Creating the failover group..."
   $failoverGroup = New-AzSqlDatabaseInstanceFailoverGroup -Name $failoverGroupName `
        -Location $primaryLocation -ResourceGroupName $primaryResourceGroupName -PrimaryManagedInstanceName $primaryManagedInstance `
        -PartnerRegion $secondaryLocation -PartnerManagedInstanceName $secondaryManagedInstance `
        -FailoverPolicy Automatic -GracePeriodWithDataLossHours 1
   $failoverGroup
   ```
---

### <a name="test-failover"></a>Yük devretme testi

Azure portalını veya PowerShell'i kullanarak başarısız grubunuzun başarısız olup bittiğini test edin. 

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak başarısız grubunuzun başarısız olduğunu test edin. 

1. [Azure portalındaki](https://portal.azure.com) _ikincil_ yönetilen örneğinize gidin ve ayarlar altında **Örnek Failover Grupları'nı** seçin. 
1. Yönetilen örneğin birincil, yönetilen örneğin ikincil olduğu gözden geçirin. 
1. **Failover'ı** seçin ve ardından TDS oturumlarının bağlantısının kesildiğine dair uyarıda **Evet'i** seçin. 

   ![Başarısız grup üzerinde başarısız](media/sql-database-managed-instance-failover-group-tutorial/failover-mi-failover-group.png)

1. Hangi manged örneğin birincil ve hangi örnek ikincil olduğunu gözden geçirin. Başarısız başarılı olduysa, iki örnek rolleri değiştirmiş olmalıdır. 

   ![Yönetilen örnekler, başarısız olduktan sonra rolleri değiştirdi](media/sql-database-managed-instance-failover-group-tutorial/mi-switched-after-failover.png)

1. Yeni _ikincil_ yönetilen örneğe gidin ve birincil örneği birincil role geri başarısız olmak için **failover'ı** bir kez daha seçin. 

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak başarısız grubunuzun başarısız olduğunu test edin. 

   ```powershell-interactive
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $secondaryResourceGroupName = "<Secondary-Resource-Group>"
   $failoverGroupName = "<Failover-Group-Name>"
   $primaryLocation = "<Primary-Region>"
   $secondaryLocation = "<Secondary-Region>"
   $primaryManagedInstance = "<Primary-Managed-Instance-Name>"
   $secondaryManagedInstance = "<Secondary-Managed-Instance-Name>"
   
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName
   
   # Failover the primary managed instance to the secondary role
   Write-host "Failing primary over to the secondary location"
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $secondaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
   Write-host "Successfully failed failover group to secondary location"
   
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName
   
   # Fail primary managed instance back to primary role
   Write-host "Failing primary back to primary role"
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $primaryLocation -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
   Write-host "Successfully failed failover group to primary location"
   
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName
   ```

---

## <a name="locate-listener-endpoint"></a>Dinleyici bitiş noktasını bulma

Başarısız grubunuzun yapılandırılmasından sonra, uygulamanızın bağlantı dizesini dinleyici bitiş noktasına güncelleyin. Bu, uygulamanızı birincil veritabanı, elastik havuz veya yönetilen örnek yerine failover grubu dinleyicisine bağlı tutar. Bu şekilde, Azure SQL veritabanı varlığınız her başarısız olduğunda bağlantı dizesini el ile güncelleştirmeniz gerekemez ve trafik şu anda birincil olan varlık hangisine yönlendirilir. 

Dinleyici bitiş noktası `fog-name.database.windows.net`, ' şeklindedir ve başarısız grubu görüntülerken Azure portalında görülebilir:

![Failover grubu bağlantı dizesi](media/sql-database-configure-failover-group/find-failover-group-connection-string.png)

## <a name="remarks"></a>Açıklamalar

- Tek veya havuza girilen bir veritabanı için bir hata grubu kaldırmak çoğaltmayı durdurmaz ve çoğaltılan veritabanını silmez. Tek bir veya havuzlu veritabanıkaldırıldıktan sonra bir başarısız gruba geri eklemek istiyorsanız, coğrafi çoğaltmayı el ile durdurmanız ve veritabanını ikincil sunucudan silmeniz gerekir. Her iki şeyi de yapmamak, `The operation cannot be performed due to multiple errors` veritabanını başarısız gruba eklemeye çalışırken benzer bir hataya neden olabilir. 


## <a name="next-steps"></a>Sonraki adımlar

Bir başarısız grubu yapılandıran ayrıntılı adımlar için aşağıdaki öğreticilere bakın:
- [Bir başarısız gruba tek bir veritabanı ekleme](sql-database-single-database-failover-group-tutorial.md)
- [Yük devretme grubuna bir elastik havuz ekleme](sql-database-elastic-pool-failover-group-tutorial.md)
- [Yönetilen örnekleri bir başarısız gruba ekleme](sql-database-managed-instance-failover-group-tutorial.md)
 
Azure SQL Veritabanı yüksek kullanılabilirlik seçeneklerine genel bakış [için, coğrafi çoğaltma](sql-database-active-geo-replication.md) ve [otomatik hata gruplarına](sql-database-auto-failover-group.md)bakın. 
