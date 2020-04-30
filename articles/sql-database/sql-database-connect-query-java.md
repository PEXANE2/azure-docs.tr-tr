---
title: Veritabanını sorgulamak için Java kullanma
description: Java kullanarak bir Azure SQL veritabanına bağlanan ve T-SQL deyimlerini kullanarak sorgulayan bir program oluşturmayı gösterir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: java
ms.topic: quickstart
author: ajlam
ms.author: andrela
ms.reviewer: v-masebo
ms.date: 03/25/2019
ms.custom: seo-java-july2019. seo-java-august2019
ms.openlocfilehash: 034f92ca3b7552373ae69148d09d58d3a5dd166a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76768654"
---
# <a name="quickstart-use-java-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Java kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlangıçta, Java kullanarak Azure SQL veritabanına bağlanın ve T-SQL deyimlerini kullanarak verileri sorgulayın.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure SQL veritabanı](sql-database-single-database-get-started.md)
- [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)ile ilgili yazılımlar

  # <a name="macos"></a>[Mac OS](#tab/macos)

  Homebrew ve Java 'yı yükledikten sonra [macOS üzerinde SQL Server kullanarak Java uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/java/mac/)bölümünde **1,2** ve **1,3** adımlarını kullanarak Maven 'yi oluşturun.

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Java Geliştirme Seti ' ni, sonra da [Ubuntu üzerinde SQL Server kullanarak Java uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/)bölümünde **1,2**, **1,3**ve **1,4** adımlarını kullanarak Maven 'i yüklersiniz.

  # <a name="windows"></a>[Windows](#tab/windows)

  Java 'yı yükleyip [Windows üzerinde SQL Server kullanarak Java uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/java/windows/)bölümünde **1,2** ve **1,3** adımlarını kullanarak Maven 'yi oluşturun.

  ---

> [!IMPORTANT]
> Bu makaledeki betikler, **Adventure Works** veritabanını kullanmak için yazılmıştır.

> [!NOTE]
> İsteğe bağlı olarak bir Azure SQL yönetilen örneği kullanmayı seçebilirsiniz.
>
> Oluşturmak ve yapılandırmak için [Azure portalını](sql-database-managed-instance-get-started.md), [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md)'i veya [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)'Yi kullanın, sonra [yerinde veya](sql-database-managed-instance-configure-p2s.md) [VM](sql-database-managed-instance-configure-vm.md) bağlantısı kurun.
>
> Verileri yüklemek için bkz. [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) dosyası Ile [bacpac ile geri yükleme](sql-database-import.md) veya bkz. [Wide World Importers veritabanını geri yükleme](sql-database-managed-instance-get-started-restore.md).

## <a name="get-sql-server-connection-information"></a>SQL Server bağlantı bilgilerini al

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. **SQL veritabanlarını** seçin veya **SQL yönetilen örnekler** sayfasını açın.

3. **Genel bakış** sayfasında, tek bir veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya yönetilen örnek Için **ana bilgisayar ' ın** yanındaki tam sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin. 

## <a name="create-the-project"></a>Proje oluşturma

1. Komut isteminden *SQLtest*adlı yeni bir Maven projesi oluşturun.

    ```bash
    mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0" --batch-mode
    ```

1. Klasörü *SQLtest* olarak değiştirin ve en sevdiğiniz metin düzenleyicinizle *Pod. xml* dosyasını açın. Aşağıdaki kodu kullanarak projenizin bağımlılıklarına **SQL Server Için MICROSOFT JDBC sürücüsü** ekleyin.

    ```xml
    <dependency>
        <groupId>com.microsoft.sqlserver</groupId>
        <artifactId>mssql-jdbc</artifactId>
        <version>7.0.0.jre8</version>
    </dependency>
    ```

1. Yine *pom.xml* dosyasında, aşağıdaki özellikleri projenize ekleyin. Özellikler bölümünüz yoksa, bağımlılıklardan sonra ekleyebilirsiniz.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. *Pom.xml* dosyasını kaydedin ve kapatın.

## <a name="add-code-to-query-database"></a>Sorgu veritabanına kod ekleme

1. Şu adreste bulunan Maven projenizde *app. Java* adlı bir dosyanız zaten olmalıdır:

   *.. \Sqltest\src\mainjava\com\sqldbsamples\app.exe*

1. Dosyayı açıp içeriğini aşağıdaki kodla değiştirin. Daha sonra sunucunuz, veritabanınız, Kullanıcı ve parolanız için uygun değerleri ekleyin.

    ```java
    package com.sqldbsamples;

    import java.sql.Connection;
    import java.sql.Statement;
    import java.sql.PreparedStatement;
    import java.sql.ResultSet;
    import java.sql.DriverManager;

    public class App {

        public static void main(String[] args) {

            // Connect to database
            String hostName = "your_server.database.windows.net"; // update me
            String dbName = "your_database"; // update me
            String user = "your_username"; // update me
            String password = "your_password"; // update me
            String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;"
                + "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
            Connection connection = null;

            try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Query data example:");
                System.out.println("=========================================");

                // Create and execute a SELECT SQL statement.
                String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName "
                    + "FROM [SalesLT].[ProductCategory] pc "  
                    + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";

                try (Statement statement = connection.createStatement();
                ResultSet resultSet = statement.executeQuery(selectSql)) {

                    // Print results from select statement
                    System.out.println("Top 20 categories:");
                    while (resultSet.next())
                    {
                        System.out.println(resultSet.getString(1) + " "
                            + resultSet.getString(2));
                    }
                    connection.close();
                }
            }
            catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    ```

   > [!NOTE]
   > Kod örneği, Azure SQL için **AdventureWorksLT** örnek veritabanını kullanır.

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde uygulamayı çalıştırın.

    ```bash
    mvn package -DskipTests
    mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
    ```

1. İlk 20 satırın döndürüldüğünden emin olun ve uygulama penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)  

- [SQL Server için Microsoft JDBC sürücüsü](https://github.com/microsoft/mssql-jdbc)  

- [Sorun bildirin/soru sorun](https://github.com/microsoft/mssql-jdbc/issues)  
