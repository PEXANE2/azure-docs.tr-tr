---
title: 'Öğretici: Başarısız bir gruba elastik bir havuz ekleme'
description: Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak bir başarısız gruba Azure SQL Veritabanı elastik havuzu ekleyin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 08/27/2019
ms.openlocfilehash: c57f9eed2147504dd7b3313d58468fb76ab40caa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268983"
---
# <a name="tutorial-add-an-azure-sql-database-elastic-pool-to-a-failover-group"></a>Öğretici: Bir başarısız gruba Azure SQL Veritabanı elastik havuzu ekleme

Azure portalını kullanarak bir Azure SQL Veritabanı elastik havuzu için bir başarısız lık grubunu yapılandırın ve başarısızlığı test edin.  Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Azure SQL Veritabanı tek bir veritabanı oluşturun.
> - Tek veritabanını elastik bir havuza ekleyin. 
> - İki mantıksal SQL sunucusu arasında iki elastik havuz için bir başarısız lık [grubu](sql-database-auto-failover-group.md) oluşturun.
> - Test başarısız.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun: 

- Azure aboneliği. Zaten hesabınız yoksa [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)


## <a name="1---create-a-single-database"></a>1 - Tek bir veritabanı oluşturma 

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="2---add-single-database-to-elastic-pool"></a>2 - Elastik havuza tek veritabanı ekleme
Bu adımda, elastik bir havuz oluşturur ve tek veritabanınızı buna eklersiniz. 


# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak esnek havuzunuzu oluşturun. 


1. Azure portalının sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. SELECT SQL dağıtım **seçeneği** sayfasını açmak için **+ Ekle'yi** seçin. Veritabanları döşemesindeki ayrıntıları göster'i seçerek farklı veritabanları hakkında ek bilgileri görüntüleyebilirsiniz.
1. **SQL Veritabanları** döşemesinde **Kaynak türü** açılır dilinden **Elastik havuzu** seçin. Elastik havuzunuzu oluşturmak için **Oluştur'u** seçin. 

    ![Elastik havuzu seçin](media/sql-database-elastic-pool-failover-group-tutorial/select-azure-sql-elastic-pool.png)

1. Elastik havuzunuzu aşağıdaki değerlerle yapılandırın:
   - **Adı**: Elastik havuzunuz için benzersiz `myElasticPool`bir ad sağlayın, örneğin. 
   - **Abonelik**: Aboneliğinizi açılır yerden seçin.
   - **ResourceGroup**: `myResourceGroup` Bölüm 1'de oluşturduğunuz kaynak grubu açılır kaynak grubundan seçin. 
   - **Sunucu**: Bölüm 1'de oluşturduğunuz sunucuyu açılır dosyadan seçin.  

       ![Esnek havuz için yeni sunucu oluşturma](media/sql-database-elastic-pool-failover-group-tutorial/use-existing-server-for-elastic-pool.png)

   - **İşlem + depolama**: İşleminizi, depolamanızı yapılandırmak ve tek veritabanınızı elastik havuzunuza eklemek için **yapılandırılan esnek havuzu** seçin. Havuz **Ayarları** sekmesinde, 2 vCores ve 32gb ile Gen5 varsayılan bırakın. 

1. **Yapılartır** **sayfasında, Veritabanları** sekmesini seçin ve ardından **veritabanı ekle'yi**seçin. Bölüm 1'de oluşturduğunuz veritabanını seçin ve ardından elastik havuzunuza eklemek için **Uygula'yı** seçin. Esnek havuz ayarlarınızı uygulamak ve **Yapıl'ı kapatmak** için yeniden **Uygula'yı** seçin. 

    ![Elastik havuza SQL DB ekleme](media/sql-database-elastic-pool-failover-group-tutorial/add-database-to-elastic-pool.png)

1. Elastik havuz ayarlarınızı gözden geçirmek için **Gözden Geçir + oluştur'u** seçin ve ardından elastik havuzunuzu oluşturmak için **Oluştur'u** seçin. 


# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
PowerShell'i kullanarak elastik havuzlarınızı ve ikincil sunucunuzu oluşturun. 

   ```powershell-interactive
   # Set variables for your server and database
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "East US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   $poolName = "myElasticPool"
   $databaseName = "mySampleDatabase"
   $drLocation = "West US"
   $drServerName = "mysqlsecondary-$(Get-Random)"
   $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # The ip address range that you want to allow to access your server 
   # Leaving at 0.0.0.0 will prevent outside-of-azure connections
   # $startIp = "0.0.0.0"
   # $endIp = "0.0.0.0"
   
   # Show randomized variables
   Write-host "DR Server name is" $drServerName 
   Write-host "Failover group name is" $failoverGroupName
   
   # Create primary Gen5 elastic 2 vCore pool
   Write-host "Creating elastic pool..."
   $elasticPool = New-AzSqlElasticPool -ResourceGroupName $resourceGroupName `
       -ServerName $serverName `
       -ElasticPoolName $poolName `
       -Edition "GeneralPurpose" `
       -vCore 2 `
       -ComputeGeneration Gen5
   $elasticPool
   
   # Add single db into elastic pool
   Write-host "Creating elastic pool..."
   $addDatabase = Set-AzSqlDatabase -ResourceGroupName $resourceGroupName `
       -ServerName $serverName `
       -DatabaseName $databaseName `
       -ElasticPoolName $poolName
   $addDatabase
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [Yeni-Azsqlelasticpool](/powershell/module/az.sql/new-azsqlelasticpool) | Azure SQL Veritabanı için esnek bir veritabanı havuzu oluşturur.| 
| [Set-AzSqlVeritabanı](/powershell/module/az.sql/set-azsqldatabase) | Veritabanı nın özelliklerini ayarlar veya varolan bir veritabanını elastik bir havuza taşır. | 

---

## <a name="3---create-the-failover-group"></a>3 - Başarısız grup oluşturma 
Bu adımda, varolan bir Azure SQL sunucusu yla başka bir bölgedeki yeni bir Azure SQL sunucusu arasında bir başarısız lık [grubu](sql-database-auto-failover-group.md) oluşturursunuz. Sonra failover grubuna elastik havuzu ekleyin. 


# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak başarısız grubunuzu oluşturun. 

1. [Azure portalının](https://portal.azure.com)sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. Önceki bölümde oluşturulan elastik havuzu seçin, örneğin. `myElasticPool` 
1. Genel **Bakış** bölmesindeki sunucunun ayarlarını açmak için Sunucu adı altında **sunucunun** adını seçin.
  
    ![Elastik havuz için açık sunucu](media/sql-database-elastic-pool-failover-group-tutorial/server-for-elastic-pool.png)

1. **Ayarlar** bölmesinin altındaki **Failover gruplarını** seçin ve ardından yeni bir başarısız grup oluşturmak için **Grup Ekle'yi** seçin. 

    ![Yeni başarısız grup ekleme](media/sql-database-elastic-pool-failover-group-tutorial/elastic-pool-failover-group.png)

1. **Failover Grubu** sayfasında aşağıdaki değerleri girin veya seçin ve sonra **Oluştur'u**seçin:
    - **Failover grup adı**: Benzersiz bir failover `failovergrouptutorial`grup adı yazın, gibi . 
    - **İkincil sunucu**: *Gerekli ayarları yapılandırma* seçeneğini seçin ve ardından yeni bir sunucu **oluşturmayı**seçin. Alternatif olarak, ikincil sunucu olarak zaten varolan bir sunucu seçebilirsiniz. Yeni ikincil sunucunuz için aşağıdaki değerleri girdikten sonra **Seç'i**seçin. 
        - **Sunucu adı**: İkincil sunucu için benzersiz `mysqlsecondary`bir ad yazın, gibi . 
        - **Sunucu admin giriş**: Türü`azureuser`
        - **Parola**: Parola gereksinimlerini karşılayan karmaşık bir parola yazın.
        - **Konum**: Açılan konumdan bir konum `East US`seçin, örneğin. Bu konum birincil sunucunuzla aynı konumda olamaz.

       > [!NOTE]
       > Sunucu girişi ve güvenlik duvarı ayarları birincil sunucunuzunkiyle eşleşmelidir. 
    
       ![Failover grubu için ikincil bir sunucu oluşturma](media/sql-database-elastic-pool-failover-group-tutorial/create-secondary-failover-server.png)

1. **Grup içindeki Veritabanları'nı** seçin ve ardından bölüm 2'de oluşturduğunuz elastik havuzu seçin. İkincil sunucuda elastik bir havuz oluşturmanızı isteyen bir uyarı görünmelidir. Uyarıyı seçin ve ardından ikincil sunucuda elastik havuz oluşturmak için **Tamam'ı** seçin. 
        
    ![Başarısız gruba elastik havuz ekleme](media/sql-database-elastic-pool-failover-group-tutorial/add-elastic-pool-to-failover-group.png)
        
1. Elastik havuz ayarlarınızı başarısız gruba uygulamak için **Seç'i** seçin ve ardından başarısız grubunuzu oluşturmak için **Oluştur'u** seçin. Başarısız gruba elastik havuz eklemek, coğrafi çoğaltma işlemini otomatik olarak başlatır.


# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak başarısız grubunuzu oluşturun. 

   ```powershell-interactive
   # Set variables for your server and database
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "East US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $poolName = "myElasticPool"
   # $databaseName = "mySampleDatabase"
   # $drLocation = "West US"
   # $drServerName = "mysqlsecondary-$(Get-Random)"
   $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # Create a secondary server in the failover region
   Write-host "Creating a secondary logical server in the failover region..."
   New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   Write-host "Secondary logical server =" $drServerName
   
   # Create a server firewall rule that allows access from the specified IP range
   Write-host "Configuring firewall for secondary logical server..."
   New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   Write-host "Firewall configured" 
   
   # Create secondary Gen5 elastic 2 vCore pool
   Write-host "Creating secondary elastic pool..."
   $elasticPool = New-AzSqlElasticPool -ResourceGroupName $resourceGroupName `
       -ServerName $drServerName `
       -ElasticPoolName $poolName `
       -Edition "GeneralPurpose" `
       -vCore 2 `
       -ComputeGeneration Gen5
   $elasticPool
   
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
   $failoverGroup
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [Yeni-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek veritabanları ve elastik havuzlar barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Mantıksal bir sunucu için bir güvenlik duvarı kuralı oluşturur. | 
| [Yeni-Azsqlelasticpool](/powershell/module/az.sql/new-azsqlelasticpool) | Azure SQL Veritabanı için esnek bir veritabanı havuzu oluşturur.| 
| [Yeni-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Yeni bir başarısız grup oluşturur. |
| [Ekle-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Bir başarısız gruba bir veya daha fazla Azure SQL Veritabanı ekler. |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Azure SQL Veritabanı başarısız grupları alır veya listeler. |

---


## <a name="4---test-failover"></a>4 - Test başarısız 
Bu adımda, başarısız olan grubunuzun ikincil sunucuya devredilemeyeceğiniz ve Azure portalını kullanarak geri dönmeceksiniz. 


# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak başarısız grubunuzun başarısız olduğunu test edin. 

1. [Azure portalının](https://portal.azure.com)sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. Önceki bölümde oluşturulan elastik havuzu seçin, örneğin. `myElasticPool` 
1. **Sunucunun** ayarlarını açmak için Sunucu adı altında sunucunun adını seçin.

    ![Elastik havuz için açık sunucu](media/sql-database-elastic-pool-failover-group-tutorial/server-for-elastic-pool.png)

1. **Ayarlar** bölmesinin altındaki **Failover gruplarını** seçin ve ardından bölüm 2'de oluşturduğunuz başarısız grubu seçin. 
  
   ![Portaldan başarısız grubu seçin](media/sql-database-elastic-pool-failover-group-tutorial/select-failover-group.png)

1. Hangi sunucunun birincil, hangi sunucunun ikincil olduğunu gözden geçirin. 
1. Elastik havuzunuzu içeren failover grubunuzda başarısız olmak için görev bölmesinden **Failover'ı** seçin. 
1. TDS oturumlarının bağlantısının kesilmeyeceğini size belirten uyarıda **Evet'i** seçin. 

   ![SQL veritabanınızı içeren başarısız grubunuzun üzerinde başarısız olun](media/sql-database-elastic-pool-failover-group-tutorial/failover-sql-db.png)

1. Hangi sunucunun birincil, hangi sunucunun ikincil olduğunu gözden geçirin. Failover başarılı olduysa, iki sunucu rolleri değiş tokuş etmiş olmalıdır. 
1. Failover grubunu orijinal ayarlara geri getirmek için **failover'ı** yeniden seçin. 


# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak başarısız grubunuzun başarısız olduğunu test edin. 

   ```powershell-interactive
   # Set variables for your server and database
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "East US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $poolName = "myElasticPool"
   # $databaseName = "mySampleDatabase"
   # $drLocation = "West US"
   # $drServerName = "mysqlsecondary-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Check role of secondary replica
   Write-host "Confirming the secondary server is secondary...." 
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   
   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failover group failed over to" $drServerName 
   ```

Başarısız grupunuzu ikincil sunucuya aktarın ve PowerShell'i kullanarak başarısız olur. 

   ```powershell-interactive
   # Set variables for your server and database
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "East US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $poolName = "myElasticPool"
   # $databaseName = "mySampleDatabase"
   # $drLocation = "West US"
   # $drServerName = "mysqlsecondary-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # Check role of secondary replica
   Write-host "Confirming the secondary server is now primary" 
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   
   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failover group failed over to" $serverName 
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Azure SQL Veritabanı başarısız grupları alır veya listeler. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Azure SQL Veritabanı failover grubunun başarısız olduğunu yürüter. |


---

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Kaynak grubunu silerek kaynakları temizleyin. 


# <a name="portal"></a>[Portal](#tab/azure-portal)


1. [Azure portalındaki](https://portal.azure.com)kaynak grubunuza gidin.
1. Gruptaki tüm kaynakları ve kaynak grubunun kendisini silmek için **kaynak grubunu** sil'i seçin. 
1. Kaynak grubunun adını textbox'a `myResourceGroup`yazın ve kaynak grubunu silmek için **Sil'i** seçin. 


# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak kaynaklarınızı temizleyin. 

   ```powershell-interactive
   # Set variables for your server and database
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   
   # Clean up resources by removing the resource group
   Write-host "Removing resource group..."
   Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
   Write-host "Resource group removed =" $resourceGroupName
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet kullanır:

| Komut | Notlar |
|---|---|
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu kaldırır | 

---

> [!IMPORTANT]
> Kaynak grubunu tutmak ancak ikincil veritabanını silmek istiyorsanız, silmeden önce başarısız gruptan kaldırın. Başarısız gruptan kaldırılmadan önce ikincil bir veritabanının silinmesi öngörülemeyen davranışlara neden olabilir. 

## <a name="full-script"></a>Tam betik

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

[!code-powershell-interactive[main](../../powershell_scripts/sql-database/failover-groups/add-elastic-pool-to-failover-group-az-ps.ps1 "Add elastic pool to a failover group")]

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek veritabanları ve elastik havuzlar barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Mantıksal bir sunucu için bir güvenlik duvarı kuralı oluşturur. | 
| [Yeni-AzSqlVeritabanı](/powershell/module/az.sql/new-azsqldatabase) | Yeni bir Azure SQL Veritabanı tek veritabanı oluşturur. | 
| [Yeni-Azsqlelasticpool](/powershell/module/az.sql/new-azsqlelasticpool) | Azure SQL Veritabanı için esnek bir veritabanı havuzu oluşturur.| 
| [Set-AzSqlVeritabanı](/powershell/module/az.sql/set-azsqldatabase) | Veritabanı nın özelliklerini ayarlar veya varolan bir veritabanını elastik bir havuza taşır. | 
| [Yeni-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Yeni bir başarısız grup oluşturur. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Bir veya daha fazla SQL veritabanı alır. |
| [Ekle-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Bir başarısız gruba bir veya daha fazla Azure SQL Veritabanı ekler. |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Azure SQL Veritabanı başarısız grupları alır veya listeler. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Azure SQL Veritabanı failover grubunun başarısız olduğunu yürüter. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu kaldırır | 


# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalı için kullanılabilir komut dosyası yok.

---

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir başarısız gruba bir Azure SQL Veritabanı elastik havuzu eklediniz ve başarısız lığı sınadın. Şunları öğrendiniz:

> [!div class="checklist"]
> - Azure SQL Veritabanı tek bir veritabanı oluşturun.
> - Tek veritabanını elastik bir havuza ekleyin. 
> - İki mantıksal SQL sunucusu arasında iki elastik havuz için bir başarısız lık [grubu](sql-database-auto-failover-group.md) oluşturun.
> - Test başarısız.

DMS kullanarak nasıl geçirilir bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: SQL Server'ı DMS kullanarak havuzlu bir veritabanına geçirin](../dms/tutorial-sql-server-to-azure-sql.md?toc=/azure/sql-database/toc.json)
