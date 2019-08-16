---
author: MashaMSFT
ms.service: sql-database
ms.subservice: single-database
ms.topic: include
ms.date: 06/19/2019
ms.author: mathoma
ms.openlocfilehash: eff121cfaf4473607110de4553a9bb8021990caf
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "68444688"
---
Bu adımda, kaynak grubunuzu ve tek bir Azure SQL veritabanı veritabanını oluşturacaksınız. 

> [!IMPORTANT]
> Bu makaledeki adımları gerçekleştirdiğiniz bilgisayarın genel IP adresini kullanmak için güvenlik duvarı kuralları ayarladığınızdan emin olun. Veritabanı düzeyinde güvenlik duvarı kuralları otomatik olarak ikincil sunucuya çoğaltılır.
>
> Bilgi için bkz. [veritabanı düzeyinde güvenlik duvarı kuralı oluşturma](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) veya bilgisayarınızın sunucu düzeyinde güvenlik duvarı kuralı IÇIN kullanılan IP adresini belirleme bkz. [sunucu düzeyinde güvenlik duvarı oluşturma](../articles/sql-database/sql-database-server-level-firewall-rule.md).  

# <a name="azure-portaltabazure-portal"></a>[Azure portal](#tab/azure-portal)
Azure portal kullanarak kaynak grubunuzu ve tek veritabanınızı oluşturun. 

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
2. **Veritabanları** ' nı seçin ve SQL VERITABANı ' nı seçerek **SQL veritabanı oluştur** sayfasını açın.

   ![Tek veritabanı oluşturma](../articles/sql-database/media/sql-database-get-started-portal/create-database-1.png)

3. **Temel bilgiler** sekmesinde, **proje ayrıntıları** bölümünde aşağıdaki değerleri yazın veya seçin:

   - **Abonelik**: Açılır ve görünmüyorsa doğru aboneliği seçin.
   - **Kaynak grubu**: **Yeni oluştur**' u seçin `myResourceGroup`, yazın ve **Tamam**' ı seçin.

     ![Yeni SQL veritabanı-temel sekmesi](../articles/sql-database/media/sql-database-get-started-portal/new-sql-database-basics.png)

4. **Veritabanı ayrıntıları** bölümünde, aşağıdaki değerleri yazın veya seçin:

   - **Veritabanı adı**: `mySampleDatabase` yazın.
   - **Sunucu**: **Yeni oluştur** ' u seçin ve aşağıdaki değerleri girip **Seç**' i seçin.
       - **Sunucu adı**: Benzersizlik `mysqlserver`için bazı sayılarla birlikte yazın.
       - **Sunucu Yöneticisi oturum açma**: `azureuser`yazın.
       - **Parola**: Parola gereksinimlerini karşılayan karmaşık bir parola yazın.
       - **Konum**: Açılan kutudan, `West US 2`gibi bir konum seçin.

         ![Yeni sunucu](../articles/sql-database/media/sql-database-get-started-portal/new-server.png)

      > [!IMPORTANT]
      > Bu ve diğer hızlı başlangıçlara yönelik sunucu ve veritabanlarında oturum açabilmek için Sunucu Yöneticisi oturum açma bilgilerini ve parolayı kaydetmeyi unutmayın. Oturum açma veya parolayı unutursanız **SQL Server** sayfasında oturum açma adını alabilir veya parolayı sıfırlayabilirsiniz. **SQL Server** sayfasını açmak için veritabanı oluşturulduktan sonra veritabanına **genel bakış** sayfasında sunucu adını seçin.

        ![SQL veritabanı ayrıntıları](../articles/sql-database/media/sql-database-get-started-portal/sql-db-basic-db-details.png)

   - **SQL elastik havuzu kullanmak istiyor**: **Hayır** seçeneğini belirleyin.
   - **İşlem + depolama**: **Veritabanını yapılandır** ' ı seçin ve bu hızlı başlangıç Için, **sanal çekirdek tabanlı satın alma seçeneklerini** belirleyin.

     ![vCore temelli satın alma seçenekleri](../articles/sql-database/media/sql-database-get-started-portal/create-database-vcore.png)

   - **Sağlanan** ve **4. nesil**öğesini seçin.

     ![Sunucusuz işlem katmanı](../articles/sql-database/media/sql-database-get-started-portal/create-database-serverless.png)

   - **Maksimum sanal çekirdekler**, **Minimum sanal çekirdek**, **Otomatik duraklatma gecikmesi**ve **veri boyutu üst**sınırı için ayarları gözden geçirin. Bunları istediğiniz şekilde değiştirin.
   - Önizleme koşullarını kabul edin ve **Tamam**' a tıklayın.
   - **Uygula**’yı seçin.

5. **Ek ayarlar** sekmesini seçin. 
6. **Veri kaynağı** bölümünde **var olan verileri kullan**altında öğesini seçin `Sample`. 

   ![Ek SQL VERITABANı ayarları](../articles/sql-database/media/sql-database-get-started-portal/create-sql-database-additional-settings.png)

   > [!IMPORTANT]
   > Bu verileri kullanan kolayca bu ve diğer Azure SQL veritabanı hızlı başlangıçlarını izleyebilmeniz için **örnek (AdventureWorksLT)** verilerini seçtiğinizden emin olun.

7. Kalan değerleri varsayılan olarak bırakın ve formun altındaki **gözden geçir + oluştur** ' u seçin.
8. Son ayarları gözden geçirin ve **Oluştur**' u seçin.

9. Kaynak grubunu, sunucuyu ve veritabanını dağıtmak ve sağlamak için **SQL veritabanı** formunda **Oluştur** ' u seçin.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

[!INCLUDE [updated-for-az](updated-for-az.md)]

PowerShell kullanarak kaynak grubunuzu ve tek bir veritabanınızı oluşturun. 

   ```powershell-interactive
   # Set variables for your server and database
   $SubscriptionId = '<Your Azure subscription ID>'
   $ResourceGroupName = "myResourceGroup" # to randomize: "myResourceGroup-$(Get-Random)"
   $Location = "westus2"
   $AdminLogin = "azureuser"
   $Password = "ChangeYourAdminPassword1"
   $ServerName = "mysqlserver" # to randomize: "mysqlserver-$(Get-Random)"
   $DatabaseName = "mySampleDatabase"
   
   
   # The ip address range that you want to allow to access your server 
   #(leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   $startIp = "0.0.0.0"
   $endIp = "0.0.0.0"
   
   # Connect to Azure
   Connect-AzAccount

   # Set subscription ID
   Set-AzContext -SubscriptionId $subscriptionId 
   
   # Create a resource group
   $resourceGroup = New-AzResourceGroup -Name $ResourceGroupName -Location $Location
   
   # Create a server with a system wide unique server name
   $server = New-AzSqlServer -ResourceGroupName $ResourceGroupName `
      -ServerName $ServerName `
      -Location $Location `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $AdminLogin, $(ConvertTo-SecureString -String $Password -AsPlainText -Force))
   
   # Create a server firewall rule that allows access from the specified IP range
   $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
      -ServerName $ServerName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   
   # Create General Purpose Gen4 database with 1 vCore
   $database = New-AzSqlDatabase  -ResourceGroupName $ResourceGroupName `
      -ServerName $ServerName `
      -DatabaseName $DatabaseName `
      -Edition GeneralPurpose `
      -VCore 1 `
      -ComputeGeneration Gen4  `
      -MinimumCapacity 1 `
      -SampleName "AdventureWorksLT"
   ```

# <a name="az-clitabbash"></a>[AZ CLI](#tab/bash)
AZ CLı kullanarak kaynak grubunuzu ve tek veritabanınızı oluşturun. 


   ```azurecli-interactive
   #!/bin/bash
   # Set variables
   export subscriptionID=<Your Subscription ID>
   export ResourceGroupName=myResourceGroup # to randomize: myResourceGroup-$RANDOM
   export Location=WestUS2
   export AdminLogin=azureuser
   export Password=ChangeYourAdminPassword1
   export ServerName="mysqlserver" # to randomize: mysqlserver-$RANDOM
   export DatabaseName=mySampleDatabase
   
   # The ip address range that you want to allow to access your DB. 
   # (leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   export startip=0.0.0.0
   export endip=0.0.0.0
  
   # Connect to Azure
   az login

   $ Set subscription ID
   az account set --subscription $SubscriptionID
   
   # Create a resource group
   az group create \
      --name $ResourceGroupName \
      --location $Location
   
   # Create a logical server in the resource group
   az sql server create \
      --name $ServerName \
      --resource-group $ResourceGroupName \
      --location $Location  \
      --admin-user $AdminLogin\
      --admin-password $Password
   
   # Configure a firewall rule for the server
   az sql server firewall-rule create \
      --resource-group $ResourceGroupName \
      --server $ServerName \
      -n AllowYourIp \
      --start-ip-address $startip \
      --end-ip-address $endip
   
   # Create a database in the server 
   az sql db create \
      --resource-group $ResourceGroupName \
      --server $ServerName \
      --name $DatabaseName \
      --sample-name AdventureWorksLT \
      --edition GeneralPurpose \
      --family Gen4 \
      --capacity 1 \
   ```

---