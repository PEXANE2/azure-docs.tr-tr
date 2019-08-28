---
author: MashaMSFT
ms.service: sql-database
ms.subservice: single-database
ms.topic: include
ms.date: 07/31/2019
ms.author: mathoma
ms.openlocfilehash: d4c426c5fe31f8fc2bfaf4697c05456124cafcb1
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70099078"
---
Bu adımda, bir Azure SQL veritabanı tek veritabanı oluşturacaksınız. 

> [!IMPORTANT]
> Bu makaleyi tamamladıktan sonra, kullanmakta olduğunuz bilgisayarın genel IP adresini kullanmak için güvenlik duvarı kuralları ayarladığınızdan emin olun.
>
> Bilgi için, bkz. [veritabanı düzeyinde güvenlik duvarı kuralı oluşturma](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) veya bilgisayarınızın sunucu düzeyinde güvenlik duvarı kuralı IÇIN kullanılan IP adresini belirleme bkz. [sunucu düzeyinde güvenlik duvarı oluşturma](../sql-database-server-level-firewall-rule.md).  

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak kaynak grubunuzu ve tek veritabanınızı oluşturun.

1. [Azure Portal](https://portal.azure.com)sol taraftaki menüden **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve arama kutusuna *Azure SQL* yazın. Seçim **Azure SQL** ' in yanındaki yıldızı seçerek bunu sık kullanılanlara ekleyin ve sol gezinti bölmesinde bir öğe olarak ekleyin. 
2. **+ Ekle** ' yı seçerek **SQL dağıtım seçeneğini seçin** sayfasını açın. **Veritabanları** kutucuğunda **Ayrıntıları göster** ' i seçerek farklı veritabanları hakkındaki ek bilgileri görüntüleyebilirsiniz.
3. **Oluştur**' u seçin.

   ![Tek veritabanı oluşturma](../media/sql-database-get-started-portal/create-single-database.png)

3. **Temel bilgiler** sekmesinde, **proje ayrıntıları** bölümünde aşağıdaki değerleri yazın veya seçin:

   - **Abonelik**: Açılır ve görünmüyorsa doğru aboneliği seçin.
   - **Kaynak grubu**: **Yeni oluştur**' u seçin `myResourceGroup`, yazın ve **Tamam**' ı seçin.

     ![Yeni SQL veritabanı-temel sekmesi](../media/sql-database-get-started-portal/new-sql-database-basics.png)

4. **Veritabanı ayrıntıları** bölümünde, aşağıdaki değerleri yazın veya seçin:

   - **Veritabanı adı**: `mySampleDatabase` yazın.
   - **Sunucu**: **Yeni oluştur**' u seçin, aşağıdaki değerleri girin ve ardından **Seç**' i seçin.
       - **Sunucu adı**: Benzersizlik `mysqlserver`için bazı sayılarla birlikte yazın.
       - **Sunucu Yöneticisi oturum açma**: `azureuser`yazın.
       - **Parola**: Parola gereksinimlerini karşılayan karmaşık bir parola yazın.
       - **Konum**: Açılan kutudan, `West US`gibi bir konum seçin.

         ![Yeni sunucu](../media/sql-database-get-started-portal/new-server.png)

      > [!IMPORTANT]
      > Bu ve diğer hızlı başlangıçlara yönelik sunucu ve veritabanlarında oturum açabilmek için Sunucu Yöneticisi oturum açma bilgilerini ve parolayı kaydetmeyi unutmayın. Oturum açma veya parolayı unutursanız **SQL Server** sayfasında oturum açma adını alabilir veya parolayı sıfırlayabilirsiniz. **SQL Server** sayfasını açmak için veritabanı oluşturulduktan sonra veritabanına **genel bakış** sayfasında sunucu adını seçin.

   - **SQL elastik havuzu kullanmak istiyor**: **Hayır** seçeneğini belirleyin.
   - **İşlem + depolama**: **Veritabanını yapılandır**' ı seçin. 

     ![SQL veritabanı ayrıntıları](../media/sql-database-get-started-portal/sql-db-basic-db-details.png)

   - **Sağlanan** ve **5. nesil**öğesini seçin.

     ![Sağlanan 4. nesil](../media/sql-database-get-started-portal/create-database-provisioned.png)

   - **Maksimum sanal çekirdekler**, **Minimum sanal çekirdek**, **oto duraklatma gecikmesi**ve **veri boyutu üst**sınırı için ayarları gözden geçirin. Bunları istediğiniz şekilde değiştirin.
   - Önizleme koşullarını kabul edin ve **Tamam**' a tıklayın.
   - **Uygula**’yı seçin.

5. **Ek ayarlar** sekmesini seçin. 
6. **Veri kaynağı** bölümünde **var olan verileri kullan**altında öğesini seçin `Sample`.

   ![Ek SQL VERITABANı ayarları](../media/sql-database-get-started-portal/create-sql-database-additional-settings.png)

   > [!IMPORTANT]
   > Bu verileri kullanan kolayca bu ve diğer Azure SQL veritabanı hızlı başlangıçlarını izleyebilmeniz için **örnek (AdventureWorksLT)** verilerini seçtiğinizden emin olun.

7. Kalan değerleri varsayılan olarak bırakın ve formun altındaki **gözden geçir + oluştur** ' u seçin.
8. Son ayarları gözden geçirin ve **Oluştur**' u seçin.

9. Kaynak grubunu, sunucuyu ve veritabanını dağıtmak ve sağlamak için **SQL veritabanı** formunda **Oluştur** ' u seçin.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

PowerShell kullanarak kaynak grubunuzu ve tek bir veritabanınızı oluşturun.

   ```powershell-interactive
   # Set variables for your server and database
   $subscriptionId = '<SubscriptionID>'
   $resourceGroupName = "myResourceGroup-$(Get-Random)"
   $location = "West US"
   $adminLogin = "azureuser"
   $password = "PWD27!"+(New-Guid).Guid
   $serverName = "mysqlserver-$(Get-Random)"
   $databaseName = "mySampleDatabase"

   # The ip address range that you want to allow to access your server 
   # (leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   $startIp = "0.0.0.0"
   $endIp = "0.0.0.0"

   # Show randomized variables
   Write-host "Resource group name is" $resourceGroupName 
   Write-host "Password is" $password  
   Write-host "Server name is" $serverName 

   # Connect to Azure
   Connect-AzAccount

   # Set subscription ID
   Set-AzContext -SubscriptionId $subscriptionId 

   # Create a resource group
   Write-host "Creating resource group..."
   $resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location -Tag @{Owner="SQLDB-Samples"}
   $resourceGroup

   # Create a server with a system wide unique server name
   Write-host "Creating primary logical server..."
   $server = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -Location $location `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
      -ArgumentList $adminLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $server

   # Create a server firewall rule that allows access from the specified IP range
   Write-host "Configuring firewall for primary logical server..."
   $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   $serverFirewallRule

   # Create General Purpose Gen4 database with 1 vCore
   Write-host "Creating a gen5 2 vCore database..."
   $database = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName `
      -Edition GeneralPurpose `
      -VCore 2 `
      -ComputeGeneration Gen5 `
      -MinimumCapacity 2 `
      -SampleName "AdventureWorksLT"
   $database
   ```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

AZ CLı kullanarak kaynak grubunuzu ve tek veritabanınızı oluşturun.

   ```azurecli-interactive
   #!/bin/bash
   # Set variables
   subscriptionID=<SubscriptionID>
   resourceGroupName=myResourceGroup-$RANDOM
   location=SouthCentralUS
   adminLogin=azureuser
   password="PWD27!"+`openssl rand -base64 18`
   serverName=mysqlserver-$RANDOM
   databaseName=mySampleDatabase
   drLocation=NorthEurope
   drServerName=mysqlsecondary-$RANDOM
   failoverGroupName=failovergrouptutorial-$RANDOM

   # The ip address range that you want to allow to access your DB. 
   # Leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB
   startip=0.0.0.0
   endip=0.0.0.0
  
   # Connect to Azure
   az login

   # Set the subscription context for the Azure account
   az account set -s $subscriptionID

   # Create a resource group
   echo "Creating resource group..."
   az group create \
      --name $resourceGroupName \
      --location $location \
      --tags Owner[=SQLDB-Samples]

   # Create a logical server in the resource group
   echo "Creating primary logical server..."
   az sql server create \
      --name $serverName \
      --resource-group $resourceGroupName \
      --location $location  \
      --admin-user $adminLogin \
      --admin-password $password

   # Configure a firewall rule for the server
   echo "Configuring firewall..."
   az sql server firewall-rule create \
      --resource-group $resourceGroupName \
      --server $serverName \
      -n AllowYourIp \
      --start-ip-address $startip \
      --end-ip-address $endip

   # Create a gen5 1vCore database in the server 
   echo "Creating a gen5 2 vCore database..."
   az sql db create \
      --resource-group $resourceGroupName \
      --server $serverName \
      --name $databaseName \
      --sample-name AdventureWorksLT \
      --edition GeneralPurpose \
      --family Gen5 \
      --capacity 2
   ```

---
