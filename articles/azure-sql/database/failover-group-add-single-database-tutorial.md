---
title: 'Öğretici: bir yük devretme grubuna veritabanı ekleme'
description: Azure SQL veritabanı 'nda Azure portal, PowerShell veya Azure CLı kullanarak bir oto yük devretme grubuna veritabanı ekleme.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 06/19/2019
ms.openlocfilehash: 53645b6ba9f1463eac14ea974a17c356c1791db6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85255322"
---
# <a name="tutorial-add-an-azure-sql-database-to-an-autofailover-group"></a>Öğretici: bir Azure SQL veritabanını bir oto yük devretme grubuna ekleme
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

[Yük devretme grubu](auto-failover-group-overview.md) , birden çok coğrafi çoğaltılan veritabanını gruplamayı sağlayan, bildirime dayalı bir soyutlama katmanıdır. Azure SQL veritabanı için bir yük devretme grubu yapılandırmayı ve Azure portal, PowerShell veya Azure CLı kullanarak yük devretmeyi test yapmayı öğrenin.  Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> - Azure SQL veritabanı 'nda veritabanı oluşturma
> - İki sunucu arasında veritabanı için bir yük devretme grubu oluşturun.
> - Yük devretme testi.

## <a name="prerequisites"></a>Ön koşullar

# <a name="the-portal"></a>[Portal](#tab/azure-portal)

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

- Azure aboneliği. Henüz bir [hesabınız yoksa ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Öğreticiyi tamamlayabilmeniz için aşağıdaki öğelerin bulunduğundan emin olun:

- Azure aboneliği. Henüz bir [hesabınız yoksa ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .
- [Azure PowerShell](/powershell/azureps-cmdlets-docs)

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

Öğreticiyi tamamlayabilmeniz için aşağıdaki öğelerin bulunduğundan emin olun:

- Azure aboneliği. Henüz bir [hesabınız yoksa ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)'nın en son sürümü.

---

## <a name="1---create-a-database"></a>1-veritabanı oluşturma

[!INCLUDE [sql-database-create-single-database](../includes/sql-database-create-single-database.md)]

## <a name="2---create-the-failover-group"></a>2-yük devretme grubu oluşturma

Bu adımda, var olan bir sunucu ile başka bir bölgedeki yeni bir sunucu arasında bir [Yük devretme grubu](auto-failover-group-overview.md) oluşturacaksınız. Ardından örnek veritabanını yük devretme grubuna ekleyin.

# <a name="the-portal"></a>[Portal](#tab/azure-portal)

Yük devretme grubunuzu oluşturun ve Azure portal kullanarak veritabanınızı veritabanına ekleyin.

1. [Azure Portal](https://portal.azure.com)sol taraftaki menüden **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve arama kutusuna Azure SQL yazın. Seçim **Azure SQL** ' in yanındaki yıldızı seçerek bunu sık kullanılanlara ekleyin ve sol gezinti bölmesinde bir öğe olarak ekleyin.
1. Bölüm 1 ' de oluşturulan veritabanını (gibi) seçin `mySampleDatabase` .
1. Yük devretme grupları sunucu düzeyinde yapılandırılabilir. Sunucu ayarlarını açmak için sunucu **adı** altında sunucunun adını seçin.

   ![Veritabanı için açık sunucu](./media/failover-group-add-single-database-tutorial/open-sql-db-server.png)

1. **Ayarlar** bölmesinde **Yük devretme grupları** ' nı seçin ve sonra yeni bir yük devretme grubu oluşturmak için **Grup Ekle** ' yi seçin.

   ![Yeni Yük devretme grubu Ekle](./media/failover-group-add-single-database-tutorial/sqldb-add-new-failover-group.png)

1. **Yük devretme grubu** sayfasında, aşağıdaki değerleri girin veya seçin ve ardından **Oluştur**' u seçin:

   - **Yük devretme grubu adı**: gibi benzersiz bir yük devretme grubu adı yazın `failovergrouptutorial` .
   - **İkincil sunucu**: *gerekli ayarları yapılandırma* seçeneğini belirleyin ve ardından **Yeni bir sunucu oluşturmayı**seçin. Alternatif olarak, zaten var olan bir sunucuyu ikincil sunucu olarak seçebilirsiniz. Aşağıdaki değerleri girdikten sonra **Seç**' i seçin.
      - **Sunucu adı**: ikincil sunucu için, gibi benzersiz bir ad yazın `mysqlsecondary` .
      - **Sunucu Yöneticisi oturum açma**: tür`azureuser`
      - **Parola**: parola gereksinimlerini karşılayan karmaşık bir parola yazın.
      - **Konum**: açılan listeden, gibi bir konum seçin `East US` . Bu konum, birincil sunucunuz ile aynı konumda olamaz.

     > [!NOTE]
     > Sunucu oturum açma ve güvenlik duvarı ayarları, birincil sunucunuzun bilgileriyle eşleşmelidir.

     ![Yük devretme grubu için ikincil sunucu oluşturma](./media/failover-group-add-single-database-tutorial/create-secondary-failover-server.png)

   - **Grup Içindeki veritabanları**: ikincil bir sunucu seçildikten sonra, bu seçenek kilidi açılmış olur. **Eklenecek veritabanlarını seçmek** ve sonra Bölüm 1 ' de oluşturduğunuz veritabanını seçmek için bu seçeneği belirleyin. Veritabanını yük devretme grubuna eklemek, coğrafi çoğaltma işlemini otomatik olarak başlatır.

   ![Yük devretme grubuna SQL veritabanı Ekle](./media/failover-group-add-single-database-tutorial/add-sqldb-to-failover-group.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Yük devretme grubunuzu oluşturun ve PowerShell 'i kullanarak veritabanınızı veritabanına ekleyin.

   > [!NOTE]
   > Sunucu oturum açma ve güvenlik duvarı ayarları, birincil sunucunuzun bilgileriyle eşleşmelidir.

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
   Write-host "Creating a secondary server in the failover region..."
   $drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $drServer

   # Create a server firewall rule that allows access from the specified IP range
   Write-host "Configuring firewall for secondary server..."
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

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Azure SQL veritabanı 'nda tek veritabanları ve elastik havuzlar barındıran bir sunucu oluşturur. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Azure SQL veritabanı 'nda bir sunucu için güvenlik duvarı kuralı oluşturur. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Azure SQL veritabanı 'nda yeni bir tek veritabanı oluşturur. |
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Azure SQL veritabanı 'nda yeni bir yük devretme grubu oluşturur. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Azure SQL veritabanı 'nda bir veya daha fazla veritabanını alır. |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Azure SQL veritabanı 'nda bir yük devretme grubuna bir veya daha fazla veritabanı ekler. |

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

Yük devretme grubunuzu oluşturun ve Azure CLı kullanarak veritabanınızı veritabanına ekleyin.

   > [!NOTE]
   > Sunucu oturum açma ve güvenlik duvarı ayarları, birincil sunucunuzun bilgileriyle eşleşmelidir.

   ```azurecli-interactive
   #!/bin/bash
   # set variables
   $failoverLocation = "West US"
   $failoverServer = "failoverServer-$randomIdentifier"
   $failoverGroup = "failoverGroup-$randomIdentifier"

   echo "Creating a secondary server in the DR region..."
   az sql server create --name $failoverServer --resource-group $resourceGroup --location $failoverLocation --admin-user $login --admin-password $password

   echo "Creating a failover group between the two servers..."
   az sql failover-group create --name $failoverGroup --partner-server $failoverServer --resource-group $resourceGroup --server $server --add-db $database --failover-policy Automatic
   ```

Öğreticinin bu bölümü aşağıdaki Azure CLı cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Veritabanları ve elastik havuzlar barındıran bir sunucu oluşturur. |
| [az SQL Server Firewall-Rule Create](/cli/azure/sql/server/firewall-rule) | Sunucunun güvenlik duvarı kurallarını oluşturur. |
| [az SQL yük devretme-Grup oluşturma](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | Bir yük devretme grubu oluşturur. |

---

## <a name="3---test-failover"></a>3-yük devretme testi

Bu adımda, yük devretme grubunuzu ikincil sunucuya devreder ve sonra Azure portal kullanarak yeniden başarısız olursunuz.

# <a name="the-portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak yük devretmeyi test edin.

1. [Azure Portal](https://portal.azure.com)sol taraftaki menüden **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve arama kutusuna Azure SQL yazın. Seçim **Azure SQL** ' in yanındaki yıldızı seçerek bunu sık kullanılanlara ekleyin ve sol gezinti bölmesinde bir öğe olarak ekleyin.
1. 2. bölümde oluşturulan veritabanını (gibi) seçin `mySampleDatbase` .
1. Sunucu ayarlarını açmak için sunucu **adı** altında sunucunun adını seçin.

   ![Veritabanı için açık sunucu](./media/failover-group-add-single-database-tutorial/open-sql-db-server.png)

1. **Ayarlar** bölmesinde **Yük devretme grupları** ' nı seçin ve ardından Bölüm 2 ' de oluşturduğunuz yük devretme grubunu seçin.
  
   ![Portaldan yük devretme grubunu seçin](./media/failover-group-add-single-database-tutorial/select-failover-group.png)

1. Hangi sunucunun birincil olduğunu ve hangi sunucunun ikincil olduğunu gözden geçirin.
1. Örnek veritabanınızı içeren yük devretme grubunuzun yükünü devretmek için görev bölmesinden **Yük devretmeyi** seçin.
1. TDS oturumlarının kesileceğini bildiren uyarıda **Evet** ' i seçin.

   ![Veritabanınızı içeren yük devretme grubunuzun yükünü devreder](./media/failover-group-add-single-database-tutorial/failover-sql-db.png)

1. Hangi sunucunun artık birincil olduğunu ve hangi sunucunun ikincil olduğunu gözden geçirin. Yük devretme başarılı olursa iki sunucu, bulunan rolleri değiştirmiş olmalıdır.
1. Sunucuları özgün rollerine geri dönmek için **Yük devretmeyi** yeniden seçin.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell kullanarak yük devretmeyi test etme.

İkincil çoğaltmanın rolünü kontrol edin:

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

İkincil sunucuya Yük devret:

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

Yük devretme grubunu birincil sunucuya geri çevir:

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

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Azure SQL veritabanı yük devretme gruplarını alır veya listeler. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Bir Azure SQL veritabanı yük devretme grubunun yük devretmesini yürütür. |

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanarak yük devretmeyi test edin.

Hangi sunucunun ikincil olduğunu doğrulayın:

   ```azurecli-interactive
   echo "Verifying which server is in the secondary role..."
   az sql failover-group list --server $server --resource-group $resourceGroup
   ```

İkincil sunucuya Yük devret:

   ```azurecli-interactive
   echo "Failing over group to the secondary server..."
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $failoverServer
   echo "Successfully failed failover group over to" $failoverServer
   ```

Yük devretme grubunu birincil sunucuya geri çevir:

   ```azurecli-interactive
   echo "Failing over group back to the primary server..."
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $server
   echo "Successfully failed failover group back to" $server
   ```

Öğreticinin bu bölümü aşağıdaki Azure CLı cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [az SQL yük devretme-Grup listesi](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | Bir sunucudaki yük devretme gruplarını listeler. |
| [az SQL yük devretme-grup kümesi-birincil](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | Geçerli birincil sunucudaki tüm veritabanlarının yükünü devretmek için yük devretme grubunun birincil kısmını ayarlayın. |

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu silerek kaynakları temizleyin.

# <a name="the-portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak kaynak grubunu silin.

1. [Azure Portal](https://portal.azure.com)kaynak grubunuza gidin.
1. Gruptaki tüm kaynakların yanı sıra kaynak grubunun kendisini silmek için **kaynak grubunu sil** ' i seçin.
1. Kaynak grubunun adını, `myResourceGroup` metin kutusuna yazın ve ardından **Sil** ' i seçerek kaynak grubunu silin.  

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell kullanarak kaynak grubunu silin.

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"

   # Remove the resource group
   Write-host "Removing resource group..."
   Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
   Write-host "Resource group removed =" $resourceGroupName
   ```

Öğreticinin bu bölümü aşağıdaki PowerShell cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu kaldırır |

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanarak kaynak grubunu silin.

   ```azurecli-interactive
   echo "Cleaning up resources by removing the resource group..."
   az group delete --name $resourceGroup
   echo "Successfully removed resource group" $resourceGroup
   ```

Öğreticinin bu bölümü aşağıdaki Azure CLı cmdlet 'lerini kullanır:

| Komut | Notlar |
|---|---|
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

---

> [!IMPORTANT]
> Kaynak grubunu korumak, ancak ikincil veritabanını silmek istiyorsanız, onu silmeden önce yük devretme grubundan kaldırın. İkincil bir veritabanının yük devretme grubundan kaldırılmadan önce silinmesi öngörülemeyen davranışlara neden olabilir.

## <a name="full-scripts"></a>Tam betikler

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-ps.ps1 "Add database to a failover group")]

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Azure SQL veritabanı 'nda tek veritabanlarını ve elastik havuzları barındıran bir sunucu oluşturur. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Azure SQL veritabanı 'nda bir sunucu için güvenlik duvarı kuralı oluşturur. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Azure SQL veritabanı 'nda yeni bir veritabanı oluşturur. |
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Azure SQL veritabanı 'nda yeni bir yük devretme grubu oluşturur. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Azure SQL veritabanı 'nda bir veya daha fazla veritabanını alır. |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Azure SQL veritabanı 'nda bir yük devretme grubuna bir veya daha fazla veritabanı ekler. |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Azure SQL veritabanı 'nda yük devretme gruplarını alır veya listeler. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Azure SQL veritabanında bir yük devretme grubunun yük devretmesini yürütür. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Azure SQL veritabanında bir kaynak grubunu kaldırır.|

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-cli.sh "Add database to a failover group")]

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az Account set](/cli/azure/account?view=azure-cli-latest#az-account-set) | Aboneliği geçerli etkin abonelik olacak şekilde ayarlar. |
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Azure SQL veritabanı 'nda tek veritabanlarını ve elastik havuzları barındıran bir sunucu oluşturur. |
| [az SQL Server Firewall-Rule Create](/cli/azure/sql/server/firewall-rule) | Azure SQL veritabanı 'nda sunucu düzeyinde IP güvenlik duvarı kuralları oluşturur. |
| [az sql db create](/cli/azure/sql/db?view=azure-cli-latest) | Azure SQL veritabanı 'nda bir veritabanı oluşturur. |
| [az SQL yük devretme-Grup oluşturma](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | Azure SQL veritabanı 'nda bir yük devretme grubu oluşturur. |
| [az SQL yük devretme-Grup listesi](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | Yük devretme gruplarını Azure SQL veritabanı 'ndaki bir sunucuda listeler. |
| [az SQL yük devretme-grup kümesi-birincil](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | Geçerli birincil sunucudaki tüm veritabanlarının yükünü devretmek için yük devretme grubunun birincil kısmını ayarlayın. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

# <a name="the-portal"></a>[Portal](#tab/azure-portal)

Azure portal için kullanılabilir komut yok.

---

Diğer Azure SQL veritabanı betiklerini buradan bulabilirsiniz: [Azure PowerShell](powershell-script-content-guide.md) ve [Azure CLI](az-cli-script-samples-content-guide.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure SQL veritabanı 'nda bir yük devretme grubuna bir veritabanı eklediniz ve yük devretme işlemi test edildi. Şunları öğrendiniz:

> [!div class="checklist"]
>
> - Azure SQL veritabanı 'nda veritabanı oluşturma
> - İki sunucu arasında veritabanı için bir yük devretme grubu oluşturun.
> - Yük devretme testi.

Esnek havuzunuzu bir yük devretme grubuna ekleme hakkında sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: bir yük devretme grubuna Azure SQL veritabanı elastik havuzu ekleme](failover-group-add-elastic-pool-tutorial.md)
