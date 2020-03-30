---
title: 'Öğretici: Bir başarısız gruba tek bir veritabanı ekleme'
description: Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak bir başarısız gruba Azure SQL Veritabanı tek veritabanı ekleyin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 06/19/2019
ms.openlocfilehash: c5ce6a1c2f231d372a2a8113eb9043a236090388
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061703"
---
# <a name="tutorial-add-an-azure-sql-database-single-database-to-a-failover-group"></a>Öğretici: Bir başarısız gruba Azure SQL Veritabanı tek veritabanı ekleme

[Başarısız grup,](sql-database-auto-failover-group.md) mulitple coğrafi olarak çoğaltılan veritabanlarını gruplandırmanızı sağlayan bildirimsel bir soyutlama katmanıdır. Azure portalı, PowerShell veya Azure CLI'yi kullanarak bir Azure SQL Veritabanı tek veritabanı için bir başarısız lık grubunu yapılandırmayı ve başarısız olmayı test etmeyi öğrenin.  Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Azure SQL Veritabanı tek bir veritabanı oluşturun.
> - İki mantıksal SQL sunucusu arasında tek bir veritabanı için bir başarısız lık grubu oluşturun.
> - Test başarısız.

## <a name="prerequisites"></a>Ön koşullar

# <a name="portal"></a>[Portal](#tab/azure-portal)
Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun: 

- Azure aboneliği. Zaten hesabınız yoksa [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)


# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
Öğreticiyi tamamlamak için aşağıdaki öğelere sahip olduğunuzdan emin olun:

- Azure aboneliği. Zaten hesabınız yoksa [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
- [Azure PowerShell](/powershell/azureps-cmdlets-docs)


# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Öğreticiyi tamamlamak için aşağıdaki öğelere sahip olduğunuzdan emin olun:

- Azure aboneliği. Zaten hesabınız yoksa [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
- [Azure CLI'nin](/cli/azure/install-azure-cli?view=azure-cli-latest)en son sürümü. 

---

## <a name="1---create-a-single-database"></a>1 - Tek bir veritabanı oluşturma 

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="2---create-the-failover-group"></a>2 - Başarısız grup oluşturma 
Bu adımda, varolan bir Azure SQL sunucusu yla başka bir bölgedeki yeni bir Azure SQL sunucusu arasında bir başarısız lık [grubu](sql-database-auto-failover-group.md) oluşturursunuz. Ardından örnek veritabanını failover grubuna ekleyin. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalını kullanarak başarısız grubunuzu oluşturun ve tek veritabanınızı bu gruba ekleyin. 

1. [Azure portalının](https://portal.azure.com)sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. Bölüm 1'de oluşturulan tek veritabanını seçin, örneğin. `mySampleDatabase` 
1. Failover grupları sunucu düzeyinde yapılandırılabilir. **Sunucunun** ayarlarını açmak için Sunucu adı altında sunucunun adını seçin.

   ![Tek db için açık sunucu](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. **Ayarlar** bölmesinin altındaki **Failover gruplarını** seçin ve ardından yeni bir başarısız grup oluşturmak için **Grup Ekle'yi** seçin. 

    ![Yeni başarısız grup ekleme](media/sql-database-single-database-failover-group-tutorial/sqldb-add-new-failover-group.png)

1. **Failover Grubu** sayfasında aşağıdaki değerleri girin veya seçin ve sonra **Oluştur'u**seçin:
    - **Failover grup adı**: Benzersiz bir failover `failovergrouptutorial`grup adı yazın, gibi . 
    - **İkincil sunucu**: *Gerekli ayarları yapılandırma* seçeneğini seçin ve ardından yeni bir sunucu **oluşturmayı**seçin. Alternatif olarak, ikincil sunucu olarak zaten varolan bir sunucu seçebilirsiniz. Aşağıdaki değerleri girdikten sonra **Seç'i**seçin. 
        - **Sunucu adı**: İkincil sunucu için benzersiz `mysqlsecondary`bir ad yazın, gibi . 
        - **Sunucu admin giriş**: Türü`azureuser`
        - **Parola**: Parola gereksinimlerini karşılayan karmaşık bir parola yazın.
        - **Konum**: Açılan konumdan bir konum `East US`seçin, örneğin. Bu konum birincil sunucunuzla aynı konumda olamaz.

    > [!NOTE]
    > Sunucu girişi ve güvenlik duvarı ayarları birincil sunucunuzunkiyle eşleşmelidir. 
    
      ![Failover grubu için ikincil bir sunucu oluşturma](media/sql-database-single-database-failover-group-tutorial/create-secondary-failover-server.png)

   - **Grup içindeki veritabanları**: İkincil bir sunucu seçildikten sonra bu seçeneğin kilidi açılır. Eklemek için **veritabanlarını seçin** ve bölüm 1'de oluşturduğunuz veritabanını seçin. Veritabanının başarısız gruba eklenmesi, coğrafi çoğaltma işlemini otomatik olarak başlatır. 
        
    ![Başarısız gruba SQL DB ekleme](media/sql-database-single-database-failover-group-tutorial/add-sqldb-to-failover-group.png)
        

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
PowerShell'i kullanarak başarısız grubunuzu oluşturun ve tek veritabanınızı buna ekleyin. 

   > [!NOTE]
   > Sunucu girişi ve güvenlik duvarı ayarları birincil sunucunuzunkiyle eşleşmelidir. 

   ```powershell-interactive
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "West US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $databaseName = "mySampleDatabase"
   $drLocation = "East US"
   $drServerName = "mysqlsecondary-$(Get-Random)"
   $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # The ip address range that you want to allow to access your server 
   # (leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   $startIp = "0.0.0.0"
   $endIp = "0.0.0.0"

   # Show randomized variables
   Write-host "DR Server name is" $drServerName 
   Write-host "Failover group name is" $failoverGroupName
   
   # Create a secondary server in the failover region
   Write-host "Creating a secondary logical server in the failover region..."
   $drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $drServer

   # Create a server firewall rule that allows access from the specified IP range
   Write-host "Configuring firewall for secondary logical server..."
   $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   $serverFirewallRule   
   
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

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [Yeni-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek veritabanları ve elastik havuzlar barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Mantıksal bir sunucu için bir güvenlik duvarı kuralı oluşturur. | 
| [Yeni-AzSqlVeritabanı](/powershell/module/az.sql/new-azsqldatabase) | Yeni bir Azure SQL Veritabanı tek veritabanı oluşturur. | 
| [Yeni-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Yeni bir başarısız grup oluşturur. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Bir veya daha fazla SQL veritabanı alır. |
| [Ekle-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Bir başarısız gruba bir veya daha fazla Azure SQL Veritabanı ekler. |

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Başarısız grupunuzu oluşturun ve AZ CLI'yi kullanarak tek veritabanınızı ekleyin. 

   > [!NOTE]
   > Sunucu girişi ve güvenlik duvarı ayarları birincil sunucunuzunkiyle eşleşmelidir. 

   ```azurecli-interactive
   #!/bin/bash
   # set variables
   $failoverLocation = "West US"
   $failoverServer = "failoverServer-$randomIdentifier"
   $failoverGroup = "failoverGroup-$randomIdentifier"

   echo "Creating a secondary logical server in the DR region..."
   az sql server create --name $failoverServer --resource-group $resourceGroup --location $failoverLocation --admin-user $login --admin-password $password
   
   echo "Creating a failover group between the two servers..."
   az sql failover-group create --name $failoverGroup --partner-server $failoverServer --resource-group $resourceGroup --server $server --add-db $database --failover-policy Automatic
   ```

Öğreticinin bu bölümünde aşağıdaki Az CLI cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Tek veritabanları ve elastik havuzlar barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule) | Sunucunun güvenlik duvarı kurallarını oluşturur. | 
| [az sql failover-grup oluşturma](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | Bir başarısız grup oluşturur. | 

---

## <a name="3---test-failover"></a>3 - Test başarısız 
Bu adımda, başarısız olan grubunuzun ikincil sunucuya devredilemeyeceğiniz ve Azure portalını kullanarak geri dönmeceksiniz. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalını kullanarak başarısız olun. 

1. [Azure portalının](https://portal.azure.com)sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. Bölüm 2'de `mySampleDatbase`oluşturulan tek veritabanını seçin. 
1. **Sunucunun** ayarlarını açmak için Sunucu adı altında sunucunun adını seçin.

   ![Tek db için açık sunucu](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. **Ayarlar** bölmesinin altındaki **Failover gruplarını** seçin ve ardından bölüm 2'de oluşturduğunuz başarısız grubu seçin. 
  
   ![Portaldan başarısız grubu seçin](media/sql-database-single-database-failover-group-tutorial/select-failover-group.png)

1. Hangi sunucunun birincil ve hangi sunucunun ikincil olduğunu gözden geçirin. 
1. Örnek tek veritabanınızı içeren failover grubunuzun üzerinden başarısız olmak için görev bölmesinden **Failover'ı** seçin. 
1. TDS oturumlarının bağlantısının kesilmeyeceğini size belirten uyarıda **Evet'i** seçin. 

   ![SQL veritabanınızı içeren başarısız grubunuzun üzerinde başarısız olun](media/sql-database-single-database-failover-group-tutorial/failover-sql-db.png)

1. Hangi sunucunun artık birincil ve hangi sunucunun ikincil olduğunu gözden geçirin. Başarısız başarısız olursa, iki sunucu rolleri değiş tokuş etmiş olmalıdır. 
1. Sunucuları asıl rollerine geri getirmek için **Failover'ı** yeniden seçin. 

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
PowerShell kullanarak başarısız lık testi yapın. 


İkincil yinelemenin rolünü denetleyin: 

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
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
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully to" $drServerName 
   ```

Failover grubunu birincil sunucuya geri döndürün:

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully back to" $serverName
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Azure SQL Veritabanı başarısız grupları alır veya listeler. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Azure SQL Veritabanı failover grubunun başarısız olduğunu yürüter. |



# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
AZ CLI kullanarak test başarısız. 

Hangi sunucunun ikincil sunucu olduğunu doğrulayın:

   
   ```azurecli-interactive
   echo "Verifying which server is in the secondary role..."
   az sql failover-group list --server $server --resource-group $resourceGroup
   ```

İkincil sunucuya başarısız olabilir: 

   ```azurecli-interactive
   echo "Failing over group to the secondary server..."
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $failoverServer
   echo "Successfully failed failover group over to" $failoverServer
   ```

Failover grubunu birincil sunucuya geri döndürün:

   ```azurecli-interactive
   echo "Failing over group back to the primary server..."
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $server
   echo "Successfully failed failover group back to" $server
   ```

Öğreticinin bu bölümünde aşağıdaki Az CLI cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [az sql failover-grup listesi](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | Bir sunucudaki başarısız grupları listeler. |
| [az sql failover-group set-primary](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | Geçerli birincil sunucudaki tüm veritabanları üzerinde başarısız olarak failover grubunun birincil ayarlayın. | 

---

## <a name="clean-up-resources"></a>Kaynakları temizleme 
Kaynak grubunu silerek kaynakları temizleyin. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalını kullanarak kaynak grubunu silin. 

1. [Azure portalındaki](https://portal.azure.com)kaynak grubunuza gidin.
1. Gruptaki tüm kaynakları ve kaynak grubunun kendisini silmek için **kaynak grubunu** sil'i seçin. 
1. Kaynak grubunun adını textbox'a `myResourceGroup`yazın ve kaynak grubunu silmek için **Sil'i** seçin.  

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak kaynak grubunu silin. 

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"

   # Remove the resource group
   Write-host "Removing resource group..."
   Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
   Write-host "Resource group removed =" $resourceGroupName
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu kaldırır | 

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

AZ CLI kullanarak kaynak grubunu silin. 


   ```azurecli-interactive
   echo "Cleaning up resources by removing the resource group..."
   az group delete --name $resourceGroup
   echo "Successfully removed resource group" $resourceGroup
   ```

Öğreticinin bu bölümünde aşağıdaki Az CLI cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

---


> [!IMPORTANT]
> Kaynak grubunu tutmak ancak ikincil veritabanını silmek istiyorsanız, silmeden önce başarısız gruptan kaldırın. Başarısız gruptan kaldırılmadan önce ikincil bir veritabanının silinmesi öngörülemeyen davranışlara neden olabilir. 


## <a name="full-scripts"></a>Tam komut dosyaları

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

[!code-powershell-interactive[main](../../powershell_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-ps.ps1 "Add single database to a failover group")]

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek veritabanları ve elastik havuzlar barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Mantıksal bir sunucu için bir güvenlik duvarı kuralı oluşturur. | 
| [Yeni-AzSqlVeritabanı](/powershell/module/az.sql/new-azsqldatabase) | Yeni bir Azure SQL Veritabanı tek veritabanı oluşturur. | 
| [Yeni-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Yeni bir başarısız grup oluşturur. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Bir veya daha fazla SQL veritabanı alır. |
| [Ekle-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Bir başarısız gruba bir veya daha fazla Azure SQL Veritabanı ekler. |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Azure SQL Veritabanı başarısız grupları alır veya listeler. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Azure SQL Veritabanı failover grubunun başarısız olduğunu yürüter. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu kaldırır | 

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!code-azurecli-interactive[main](../../cli_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-cli.sh "Add single database to a failover group")]

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az hesap seti](/cli/azure/account?view=azure-cli-latest#az-account-set) | Aboneliği geçerli etkin abonelik olarak ayarlar. | 
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Tek veritabanları ve elastik havuzlar barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule) | Sunucunun güvenlik duvarı kurallarını oluşturur. | 
| [az sql db create](/cli/azure/sql/db?view=azure-cli-latest) | Bir veritabanı oluşturur. | 
| [az sql failover-grup oluşturma](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | Bir başarısız grup oluşturur. | 
| [az sql failover-grup listesi](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | Bir sunucudaki başarısız grupları listeler. |
| [az sql failover-group set-primary](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | Geçerli birincil sunucudaki tüm veritabanları üzerinde başarısız olarak failover grubunun birincil ayarlayın. | 
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalı için kullanılabilir komut dosyası yok. 
 
---

Diğer Azure SQL Veritabanı komut dosyalarını burada bulabilirsiniz: [Azure PowerShell](sql-database-powershell-samples.md) ve [Azure CLI.](sql-database-cli-samples.md) 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir başarısız gruba azure SQL Veritabanı tek veritabanı eklediniz ve başarısız olmayı sınadın. Şunları öğrendiniz: 

> [!div class="checklist"]
> - Azure SQL Veritabanı tek bir veritabanı oluşturun. 
> - İki mantıksal SQL sunucusu arasında tek bir veritabanı için bir başarısız lık [grubu](sql-database-auto-failover-group.md) oluşturun.
> - Test başarısız.

Elastik havuzunuzu bir başarısız gruba nasıl ekleyeceğiniz le ilgili bir sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Öğretici: Bir başarısız gruba Azure SQL Veritabanı elastik havuzu ekleme](sql-database-elastic-pool-failover-group-tutorial.md)
