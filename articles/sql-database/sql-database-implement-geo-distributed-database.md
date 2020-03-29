---
title: Coğrafi olarak dağıtılmış bir çözüm uygulayın
description: Azure SQL veritabanınızı ve uygulamanızı çoğaltılmış bir veritabanında başarısız olmak üzere yapılandırmayı öğrenin ve başarısız olmayı test edin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 58d5bd4a7f3087e11056354f7534c3c9dbebca3c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067299"
---
# <a name="tutorial-implement-a-geo-distributed-database"></a>Öğretici: Coğrafi olarak dağıtılmış bir veritabanı uygulama

Bir Azure SQL veritabanını ve uygulamayı uzak bir bölgeye geçmemek üzere yapılandırın ve bir başarısız lık planını test edin. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> - Bir [başarısız grup](sql-database-auto-failover-group.md) oluşturma
> - Azure SQL veritabanını sorgulamak için Java uygulaması çalıştırma
> - Yük devretme testi

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Öğreticiyi tamamlamak için aşağıdaki öğeleri yüklediğinizden emin olun:

- [Azure PowerShell](/powershell/azureps-cmdlets-docs)
- Azure SQL Veritabanı'nda tek bir veritabanı. Tek bir kullanım oluşturmak için,
  - [Portal](sql-database-single-database-get-started.md)
  - [CLI](sql-database-cli-samples.md)
  - [Powershell](sql-database-powershell-samples.md)

  > [!NOTE]
  > Öğretici *AdventureWorksLT* örnek veritabanını kullanır.

- Java ve Maven, [bkz.](https://www.microsoft.com/sql-server/developer-get-started/) **Java**

> [!IMPORTANT]
> Bu öğreticideki adımları gerçekleştirdiğiniz bilgisayarın genel IP adresini kullanmak için güvenlik duvarı kurallarını ayarladığınızdan emin olun. Veritabanı düzeyinde güvenlik duvarı kuralları ikincil sunucuya otomatik olarak çoğalır.
>
> Bilgi için [bkz. veritabanı düzeyinde güvenlik duvarı kuralı oluştur](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) veya bilgisayarınız için sunucu düzeyinde güvenlik duvarı kuralı için kullanılan IP adresini belirlemek için bkz. [Create a server-level firewall](sql-database-server-level-firewall-rule.md)  

## <a name="create-a-failover-group"></a>Bir başarısız grup oluşturma

Azure PowerShell'i kullanarak, varolan bir Azure SQL sunucusu yla başka bir bölgedeki yeni bir Azure SQL sunucusu arasında [hata grupları](sql-database-auto-failover-group.md) oluşturun. Ardından örnek veritabanını failover grubuna ekleyin.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

> [!IMPORTANT]
> [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]

Bir başarısız grup oluşturmak için aşağıdaki komut dosyasını çalıştırın:

```powershell
$admin = "<adminName>"
$password = "<password>"
$resourceGroup = "<resourceGroupName>"
$location = "<resourceGroupLocation>"
$server = "<serverName>"
$database = "<databaseName>"
$drLocation = "<disasterRecoveryLocation>"
$drServer = "<disasterRecoveryServerName>"
$failoverGroup = "<globallyUniqueFailoverGroupName>"

# create a backup server in the failover region
New-AzSqlServer -ResourceGroupName $resourceGroup -ServerName $drServer `
    -Location $drLocation -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
    -ArgumentList $admin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# create a failover group between the servers
New-AzSqlDatabaseFailoverGroup –ResourceGroupName $resourceGroup -ServerName $server `
    -PartnerServerName $drServer –FailoverGroupName $failoverGroup –FailoverPolicy Automatic -GracePeriodWithDataLossHours 2

# add the database to the failover group
Get-AzSqlDatabase -ResourceGroupName $resourceGroup -ServerName $server -DatabaseName $database | `
    Add-AzSqlDatabaseToFailoverGroup -ResourceGroupName $resourceGroup -ServerName $server -FailoverGroupName $failoverGroup
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!IMPORTANT]
> Azure'da oturum açacak şekilde çalıştırın. `az login`

```azurecli
$admin = "<adminName>"
$password = "<password>"
$resourceGroup = "<resourceGroupName>"
$location = "<resourceGroupLocation>"
$server = "<serverName>"
$database = "<databaseName>"
$drLocation = "<disasterRecoveryLocation>" # must be different then $location
$drServer = "<disasterRecoveryServerName>"
$failoverGroup = "<globallyUniqueFailoverGroupName>"

# create a backup server in the failover region
az sql server create --admin-password $password --admin-user $admin `
    --name $drServer --resource-group $resourceGroup --location $drLocation

# create a failover group between the servers
az sql failover-group create --name $failoverGroup --partner-server $drServer `
    --resource-group $resourceGroup --server $server --add-db $database `
    --failover-policy Automatic --grace-period 2
```

* * *

Coğrafi çoğaltma ayarları, veritabanınızı seçerek Azure portalında değiştirilebilir ve ardından**Coğrafi Çoğaltmayı** **Ayarlar.** > 

![Coğrafi çoğaltma ayarları](./media/sql-database-implement-geo-distributed-database/geo-replication.png)

## <a name="run-the-sample-project"></a>Örnek projeyi çalıştırma

1. Konsolda, aşağıdaki komutu içeren bir Maven projesi oluşturun:

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

1. **Y** yazın ve **Enter**tuşuna basın.

1. Dizinleri yeni projeyle değiştirin.

   ```bash
   cd SqlDbSample
   ```

1. En sevdiğiniz düzenleyiciyi kullanarak proje klasörünüzdeki *pom.xml* dosyasını açın.

1. Aşağıdaki `dependency` bölümü ekleyerek SQL Server bağımlılığı için Microsoft JDBC Sürücüsü ekleyin. Bağımlılık büyük `dependencies` bölüm içinde yapıştırılmalıdır.

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

1. Bölümden sonraki `properties` bölümü ekleyerek Java `dependencies` sürümünü belirtin:

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Bölümden `build` sonraki bölümü ekleyerek `properties` bildirim dosyalarını destekleyin:

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```

1. *Pom.xml* dosyasını kaydedin ve kapatın.

1. Bulunan *App.java* dosyasını açın... \SqlDbSample\src\main\java\com\sqldbsamples ve aşağıdaki kod ile içeriğini değiştirin:

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "<your failover group name>";  // add failover group name
  
      private static final String DB_NAME = "<your database>";  // add database name
      private static final String USER = "<your admin>";  // add database user
      private static final String PASSWORD = "<your password>";  // add database password

      private static final String READ_WRITE_URL = String.format("jdbc:" +
         "sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;" +
         "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", +
         FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:" +
         "sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;" +
         "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", +
         FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println("");

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " +
                   (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " +
                   (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name so we can find the product again
      String sql = "INSERT INTO SalesLT.Product " +
         "(Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL);
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query the data previously inserted into the primary database from the geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL);
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query the high water mark id stored in the table to be able to make unique inserts
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL);
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```

1. *App.java* dosyasını kaydedin ve kapatın.

1. Komut konsolunda aşağıdaki komutu çalıştırın:

   ```bash
   mvn package
   ```

1. Başarısızlık testini çalıştırmak için zaman tanıyan uygulamayı el ile durdurulana kadar yaklaşık 1 saat boyunca çalıştıracak uygulamayı başlatın.

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

   ```output
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ...
   ```

## <a name="test-failover"></a>Yük devretme testi

Bir başarısızlık simüle etmek ve uygulama sonuçlarını gözlemlemek için aşağıdaki komut dosyalarını çalıştırın. Veritabanı geçişi sırasında bazı eklerin ve seklerin nasıl başarısız olacağına dikkat edin.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Test sırasında olağanüstü durum kurtarma sunucusunun rolünü aşağıdaki komutla denetleyebilirsiniz:

```powershell
(Get-AzSqlDatabaseFailoverGroup -FailoverGroupName $failoverGroup `
    -ResourceGroupName $resourceGroup -ServerName $drServer).ReplicationRole
```

Bir başarısızı test etmek için:

1. Failover grubunun manuel olarak başarısız lığı başlatın:

   ```powershell
   Switch-AzSqlDatabaseFailoverGroup -ResourceGroupName $myresourcegroupname `
    -ServerName $drServer -FailoverGroupName $failoverGroup
   ```

1. Failover grubunu birincil sunucuya geri döndürün:

   ```powershell
   Switch-AzSqlDatabaseFailoverGroup -ResourceGroupName $resourceGroup `
    -ServerName $server -FailoverGroupName $failoverGroup
   ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Test sırasında olağanüstü durum kurtarma sunucusunun rolünü aşağıdaki komutla denetleyebilirsiniz:

```azurecli
az sql failover-group show --name $failoverGroup --resource-group $resourceGroup --server $drServer
```

Bir başarısızı test etmek için:

1. Failover grubunun manuel olarak başarısız lığı başlatın:

   ```azurecli
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $drServer
   ```

1. Failover grubunu birincil sunucuya geri döndürün:

   ```azurecli
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $server
   ```

* * *

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure SQL veritabanını ve uygulamayı uzak bir bölgeye başarısız olmak üzere yapılandırıldınız ve bir başarısızlık planını test ettin. Şunları öğrendiniz:

> [!div class="checklist"]
> - Coğrafi çoğaltma yük devretme grubu oluşturma
> - Azure SQL veritabanını sorgulamak için Java uygulaması çalıştırma
> - Yük devretme testi

DMS kullanarak nasıl geçirilir bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [SQL Server'ı DMS kullanarak Azure SQL veritabanına geçirin](../dms/tutorial-sql-server-to-managed-instance.md)
