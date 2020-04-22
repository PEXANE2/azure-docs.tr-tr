---
author: MashaMSFT
ms.service: sql-database
ms.subservice: single-database
ms.topic: include
ms.date: 03/10/2020
ms.author: mathoma
ms.reviewer: vanto
ms.openlocfilehash: 563ac8748e9d4f8a254a151814491113b9c816e6
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685158"
---
Bu adımda, bir Azure SQL Veritabanı sunucusu ve AdventureWorksLT örnek verilerini kullanan tek bir veritabanı oluşturursunuz. Veritabanını Azure portal menülerini ve ekranlarını kullanarak veya Azure Bulut BulutU'nda Bir Azure CLI veya PowerShell komut dosyası kullanarak oluşturabilirsiniz.

Tüm yöntemler, sunucuya erişmek için kullandığınız bilgisayarın genel IP adresine izin vermek için sunucu düzeyinde bir güvenlik duvarı kuralı ayarlamayı içerir. Sunucu güvenlik duvarı kuralları oluşturma hakkında daha fazla bilgi [için](../sql-database-server-level-firewall-rule.md)bkz. Veritabanı düzeyinde güvenlik duvarı kuralları da ayarlayabilirsiniz. Bkz. [Veritabanı düzeyinde güvenlik duvarı kuralı oluşturun.](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) 

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalında bir kaynak grubu, SQL sunucusu ve tek bir veritabanı oluşturmak için:

1. [Portalda](https://portal.azure.com)oturum aç.
1. Arama çubuğundan **Azure SQL'i**arayın ve seçin.
1. Azure **SQL** sayfasında **Ekle'yi**seçin. 
   
   ![Azure SQL'e Ekle](../media/sql-database-single-database-get-started/sqldbportal.png)
   
1. SELECT **SQL dağıtım seçeneği** sayfasında, **Kaynak türü**altında **Tek veritabanı** olan SQL **veritabanları** döşemesini seçin. **Ayrıntıları Göster'i**seçerek farklı veritabanları hakkında daha fazla bilgi görüntüleyebilirsiniz.
1. **Oluştur**’u seçin.
   
   ![Tek veritabanı oluşturma](../media/sql-database-single-database-get-started/create-single-database.png)
   
1. **SQL veritabanı** oluştur formunun **Temeller** sekmesinde, **Project ayrıntıları**altında, henüz seçilmemişse doğru Azure **Aboneliği'ni** seçin.
1. **Kaynak grubu**altında, **yeni oluştur'u**seçin , *myResourceGroup'u*girin ve **Tamam'ı**seçin.
1. **Veritabanı ayrıntıları**altında, **Veritabanı adı** için *mySampleDatabase*girin.
1. **Sunucu**için **yeni oluştur'u**seçin ve **Yeni sunucu** formunu aşağıdaki gibi doldurun:
   - **Sunucu adı**: *mysqlserver*girin ve benzersizlik için bazı karakterler.
   - **Server admin giriş**: *Azureuser*girin .
   - **Parola**: Gereksinimleri karşılayan bir parola girin ve **parolayı onayla** alanına tekrar girin.
   - **Konum**: Aşağı inin ve **(ABD) Doğu ABD**gibi bir yer seçin.
   
   **Tamam'ı**seçin.
   
   ![Yeni sunucu](../media/sql-database-single-database-get-started/new-server.png)
   
   Sunucu ve veritabanlarında oturum açabilmeniz için sunucu yöneticisi giriş ve parolasını kaydedin. Oturum açma veya parolanızı unutursanız, veritabanı oluşturulduktan sonra giriş adını alabilir veya **SQL server** sayfasındaki parolayı sıfırlayabilirsiniz. **SQL sunucu** sayfasını açmak için **veritabanına Genel Bakış** sayfasındaki sunucu adını seçin.
   
1. **Hesaplama + depolama**altında, varsayılanları yeniden yapılandırmak istiyorsanız, **veritabanını yapılandır'ı**seçin.
   
   **Yapılandırılan** sayfada isteğe bağlı olarak şunları yapabilirsiniz:
   - İşlem **katmanını** **Provisioned'den** **Serverless'a**değiştirin.
   - **vCores** ve Veri maksimum boyutu ayarlarını gözden geçirin ve **değiştirin.**
   - Donanım oluşturmayı değiştirmek için **yapılandırmayı değiştir'i** seçin.
   
   Herhangi bir değişiklik yaptıktan sonra **Uygula'yı**seçin.
   
1. **Sonraki'ni seçin:** Sayfanın altındaki ağ.
   
   ![Yeni SQL veritabanı - Temel sekme](../media/sql-database-single-database-get-started/new-sql-database-basics.png)
   
1. **Ağ** sekmesinde, **Bağlantı yöntemi** **altında, Ortak bitiş noktasını**seçin. 
1. **Güvenlik Duvarı kuralları altında,** geçerli istemci IP adresini **Evet'e** **ekle'yi** ayarlayın.
1. **Sonraki'ni seçin:** Sayfanın altındaki ek ayarlar.
   
   ![Ağ sekmesi](../media/sql-database-single-database-get-started/networking.png)
   
   Güvenlik duvarı ayarları hakkında daha fazla bilgi için azure [hizmetlerine ve kaynaklarına bu sunucuya erişmeye izin](../sql-database-networkaccess-overview.md) ver ve özel bir bitiş noktası [ekleyin'e](../../private-link/private-endpoint-overview.md)bakın.
   
1. Ek **ayarlar** sekmesinde, **Veri kaynağı** bölümünde, **varolan verileri kullanmak**için **Örnek'i**seçin.
1. Sayfanın altındaki **Gözden Geçir + seçeneğini** belirleyin.
   
   ![Ek ayarlar sekmesi](../media/sql-database-single-database-get-started/additional-settings.png)
   
1. Ayarları inceledikten sonra **Oluştur'u**seçin.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure komut satırı arabirimini (Azure CLI) kullanarak bir Azure kaynak grubu, SQL sunucusu ve tek bir veritabanı oluşturabilirsiniz. Azure Bulut Kabuğu'nu kullanmak istemiyorsanız, bilgisayarınıza [Azure CLI yükleyin.](/cli/azure/install-azure-cli)

Azure Bulut Su Şurası'nda aşağıdaki kod örneğini çalıştırmak için kod örneği başlık çubuğunda **deneyin'i** seçin. Bulut Kabuğu açıldığında, **Copy** kod örneği başlık çubuğunda Kopyala'yı seçin ve kod örneğini Bulut Kabuğu penceresine yapıştırın. Kodda, Azure `<Subscription ID>` Abonelik Kimliğinizle ve `$startip` `$endip`kullandığınız `0.0.0.0` bilgisayarın genel IP adresiyle değiştirin.

Azure'da oturum açma ve kodu çalıştırma istemini izleyin. 

Üst teki Bulut Kabuğu simgesini seçerek Azure portalından Azure Bulut Kabuğu'nu da kullanabilirsiniz. 
   
   ![Azure Cloud Shell](../media/sql-database-single-database-get-started/cloudshell.png)
   
Portalda Cloud Shell'i ilk kez **kullandığınızda, Hoş Geldiniz** iletişim kutusunda **Bash'i** seçin. Sonraki oturumlarda Bir Bash ortamında Azure CLI kullanır veya Bulut Kabuğu denetim çubuğundan **Bash'i** seçebilirsiniz. 

Aşağıdaki Azure CLI kodu, sunucuya erişmek için bir Azure kaynak grubu, SQL sunucusu, tek veritabanı ve güvenlik duvarı kuralı oluşturur. Bu kaynakları daha sonra yönetebilmek için oluşturulan kaynak grubunu ve sunucu adlarını kaydettiğinden emin olun.

```azurecli-interactive
#!/bin/bash

# Sign in to Azure and set execution context (if necessary)
az login
az account set --subscription <Subscription ID>

# Set the resource group name and location for your server
resourceGroupName=myResourceGroup-$RANDOM
location=westus2

# Set an admin login and password for your database
adminlogin=azureuser
password=Azure1234567

# Set a logical server name that is unique to Azure DNS (<server_name>.database.windows.net)
servername=server-$RANDOM

# Set the ip address range that can access your database
startip=0.0.0.0
endip=0.0.0.0

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a logical server in the resource group
az sql server create \
    --name $servername \
    --resource-group $resourceGroupName \
    --location $location  \
    --admin-user $adminlogin \
    --admin-password $password

# Configure a firewall rule for the server
az sql server firewall-rule create \
    --resource-group $resourceGroupName \
    --server $servername \
    -n AllowYourIp \
    --start-ip-address $startip \
    --end-ip-address $endip

# Create a gen5 2 vCore database in the server
az sql db create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name mySampleDatabase \
    --sample-name AdventureWorksLT \
    --edition GeneralPurpose \
    --family Gen5 \
    --capacity 2 \
```

Önceki kod şu Azure CLI komutlarını kullanır:

| Komut | Açıklama |
|---|---|
| [az hesap seti](/cli/azure/account?view=azure-cli-latest#az-account-set) | Aboneliği geçerli etkin abonelik olarak ayarlar. | 
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Tek veritabanları ve elastik havuzlar barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-create) | Sunucunun güvenlik duvarı kurallarını oluşturur. | 
| [az sql db create](/cli/azure/sql/db#az-sql-db-create?view=azure-cli-latest) | Bir veritabanı oluşturur. | 

Daha fazla Azure SQL Veritabanı Azure CLI örnekleri için [Azure CLI örneklerine](../sql-database-cli-samples.md)bakın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Windows PowerShell'i kullanarak bir Azure kaynak grubu, SQL sunucusu ve tek bir veritabanı oluşturabilirsiniz. Azure Bulut Kabuğu'nu kullanmak istemiyorsanız, [Azure PowerShell modüllerini yükleyin.](/powershell/azure/install-az-ps)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Bulut Kabuğu'nda aşağıdaki kod örneğini çalıştırmak için kod başlığı çubuğunda **deneyin'i** seçin. Bulut Kabuğu açıldığında, **Copy** kod örneği başlık çubuğunda Kopyala'yı seçin ve kod örneğini Bulut Kabuğu penceresine yapıştırın. Kodda, Azure `<Subscription ID>` Abonelik Kimliğinizle ve `$startIp` `$endIp`kullandığınız `0.0.0.0` bilgisayarın genel IP adresiyle değiştirin. 

Azure'da oturum açma ve kodu çalıştırma istemini izleyin. 

Üst teki Bulut Kabuğu simgesini seçerek Azure portalından Azure Bulut Su Şunları Topünü de kullanabilirsiniz. 
   
   ![Azure Cloud Shell](../media/sql-database-single-database-get-started/cloudshell.png)
   
Portaldan Cloud Shell'i ilk kez **kullandığınızda, Hoş Geldiniz** iletişim kutusunda **PowerShell'i** seçin. Sonraki oturumlarda PowerShell kullanır veya Cloud Shell denetim çubuğundan seçebilirsiniz. 

Aşağıdaki PowerShell kodu, sunucuya erişmek için bir Azure kaynak grubu, SQL sunucusu, tek veritabanı ve güvenlik duvarı kuralı oluşturur. Bu kaynakları daha sonra yönetebilmek için oluşturulan kaynak grubunu ve sunucu adlarını kaydettiğinden emin olun.

   ```powershell-interactive
   # Set variables for your server and database
   $subscriptionId = '<SubscriptionID>'
   $resourceGroupName = "myResourceGroup-$(Get-Random)"
   $location = "West US"
   $adminLogin = "azureuser"
   $password = "Azure1234567"
   $serverName = "mysqlserver-$(Get-Random)"
   $databaseName = "mySampleDatabase"

   # The ip address range that you want to allow to access your server 
   $startIp = "0.0.0.0"
   $endIp = "0.0.0.0"

   # Show randomized variables
   Write-host "Resource group name is" $resourceGroupName 
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

Önceki kod bu PowerShell cmdlets kullanır:

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek veritabanları ve elastik havuzlar barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Mantıksal bir sunucu için bir güvenlik duvarı kuralı oluşturur. | 
| [Yeni-AzSqlVeritabanı](/powershell/module/az.sql/new-azsqldatabase) | Azure SQL Veritabanı tek bir veritabanı oluşturur. | 

Daha fazla Azure SQL Veritabanı PowerShell örneği için [Azure PowerShell örneklerine](../sql-database-powershell-samples.md)bakın.

---
