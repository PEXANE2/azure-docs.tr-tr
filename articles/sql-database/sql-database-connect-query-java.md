---
title: Veritabanını sorgulamak için Java'yı kullanma
description: Azure SQL veritabanına bağlanan bir program oluşturmak ve T-SQL deyimlerini kullanarak sorgulamak için Java'yı nasıl kullanacağınızı gösterir.
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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76768654"
---
# <a name="quickstart-use-java-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Java kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlatmada, Bir Azure SQL veritabanına bağlanmak için Java'yı ve verileri sorgulamak için T-SQL deyimlerini kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Azure SQL veritabanı](sql-database-single-database-get-started.md)
- [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)ile ilgili yazılım

  # <a name="macos"></a>[Macos](#tab/macos)

  Homebrew ve Java'yı yükleyin, ardından [macOS'ta SQL Server'ı kullanarak Java uygulamaları oluşturun'da](https://www.microsoft.com/sql-server/developer-get-started/java/mac/) **1.2** ve **1.3** adımlarını kullanarak Maven'i yükleyin.

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Java'yı, Java Geliştirme Kiti'ni yükleyin, ardından [Ubuntu'da SQL Server'ı kullanarak Java uygulamaları oluşturun'da](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/) **1.2,** **1.3**ve **1.4** adımlarını kullanarak Maven'i yükleyin.

  # <a name="windows"></a>[Windows](#tab/windows)

  Java'yı yükleyin, ardından [Windows'ta SQL Server'ı kullanarak Java uygulamaları oluşturun'da](https://www.microsoft.com/sql-server/developer-get-started/java/windows/) **1.2** ve **1.3** adımlarını kullanarak Maven'i yükleyin.

  ---

> [!IMPORTANT]
> Bu makaledeki komut dosyaları Adventure **Works** veritabanını kullanmak için yazılmıştır.

> [!NOTE]
> İsteğe bağlı olarak Azure SQL yönetilen bir örneği kullanmayı seçebilirsiniz.
>
> Oluşturmak ve yapılandırmak için [Azure Portalını,](sql-database-managed-instance-get-started.md) [PowerShell'i](scripts/sql-database-create-configure-managed-instance-powershell.md)veya [CLI'yi,](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)ardından [yerinde](sql-database-managed-instance-configure-p2s.md) veya [VM](sql-database-managed-instance-configure-vm.md) bağlantısını kurun.
>
> Verileri yüklemek için, [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) [dosyasıyla BACPAC ile geri yükleme'ye](sql-database-import.md) bakın veya [Geniş Dünya İthalatçılar veritabanını geri yükleyin.](sql-database-managed-instance-get-started-restore.md)

## <a name="get-sql-server-connection-information"></a>SQL sunucu bağlantı bilgilerini alın

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Gelecek yordamlar için tam nitelikli sunucu adı veya ana bilgisayar adı, veritabanı adı ve giriş bilgilerine ihtiyacınız vardır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. **SQL veritabanlarını** seçin veya **SQL yönetilen örnekler** sayfasını açın.

3. Genel **Bakış** sayfasında, tek bir veritabanı için **Sunucu adının** yanındaki tam nitelikli sunucu adını veya yönetilen bir örnek için **Host'un** yanındaki tam nitelikli sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine tıklayın ve **Kopyasimgesini** seçin. 

## <a name="create-the-project"></a>Proje oluşturma

1. Komut isteminden *sqltest*adında yeni bir Maven projesi oluşturun.

    ```bash
    mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0" --batch-mode
    ```

1. Klasörü *sqltest* olarak değiştirin ve favori metin düzenleyicisiyle *pom.xml'i* açın. Aşağıdaki kodu kullanarak projenizin bağımlılıklarına **SQL Server için Microsoft JDBC Sürücüsü'nü** ekleyin.

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

1. Maven projenizde *App.java* adında bir dosyanız olmalıdır:

   *.. \sqltest\src\main\java\com\sqldbsamples\App.java*

1. Dosyayı açıp içeriğini aşağıdaki kodla değiştirin. Ardından sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

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

1. En iyi 20 satırın döndürüldünden doğrulayın ve uygulama penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)  

- [SQL Server için Microsoft JDBC sürücüsü](https://github.com/microsoft/mssql-jdbc)  

- [Sorun bildirin/soru sorun](https://github.com/microsoft/mssql-jdbc/issues)  
