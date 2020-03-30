---
title: MySQL için Java - Azure Veritabanı'nı kullanarak bağlanın
description: Bu hızlı başlatma, MySQL veritabanı için bir Azure Veritabanından veri bağlamak ve sorgulamak için kullanabileceğiniz bir Java kodu örneği sağlar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc, devcenter, seo-java-july2019, seo-java-august2019
ms.topic: quickstart
ms.devlang: java
ms.date: 3/18/2020
ms.openlocfilehash: 0a41dfbdbfb1a0f588c1a334aefeb4fb5043e813
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80067956"
---
# <a name="quickstart-use-java-to-connect-to-and-query-data-in-azure-database-for-mysql"></a>Quickstart: MySQL için Azure Veritabanı'ndaki verilere bağlanmak ve bunları sorgulamak için Java'yı kullanın

Bu hızlı başlangıçta, bir Java uygulaması ve JDBC sürücüsü MariaDB Konektörü/J kullanarak MySQL için bir Azure Veritabanına bağlanırsınız. Daha sonra, veritabanındaki verileri Mac, Ubuntu Linux ve Windows platformlarından sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini kullanırsınız. 

Bu konu, Java'yı geliştirmeye aşina olduğunuzu varsayar, ancak MySQL için Azure Veritabanı ile çalışmaya yeni başlıyorsunuz.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- MySQL sunucusu için bir Azure Veritabanı. [Azure portalı kullanarak MySQL sunucusu için bir Azure Veritabanı oluşturun](quickstart-create-mysql-server-database-using-azure-portal.md) veya Azure [CLI'yi kullanarak MySQL sunucusu için bir Azure Veritabanı oluşturun.](quickstart-create-mysql-server-database-using-azure-cli.md)
- MySQL bağlantı güvenliği için Azure Veritabanı, açılan güvenlik duvarı ve uygulamanız için yapılandırılan SSL bağlantı ayarlarıyla yapılandırılır.

## <a name="obtain-the-mariadb-connector"></a>MariaDB konektörünü edinin

Aşağıdaki yaklaşımlardan birini kullanarak [MariaDB Konektörü/J](https://mariadb.com/kb/en/library/mariadb-connector-j/) konektörünü edinin:
   - Projeniz için POM dosyasına [mariadb-java-istemci bağımlılığını](https://mvnrepository.com/artifact/org.mariadb.jdbc/mariadb-java-client) eklemek için Maven paketini [mariadb-java-istemcisini](https://search.maven.org/search?q=a:mariadb-java-client) kullanın.
   - JDBC sürücüsü [MariaDB Bağlayıcı/J'yi](https://downloads.mariadb.org/connector-java/) indirin ve JDBC jar dosyasını (örneğin mariadb-java-client-2.4.3.jar) uygulama sınıfı yolunuza ekleyin. [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) veya [Java SE](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html) gibi sınıf yolu özellikleri için ortamınızın belgelerine başvurun

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

MySQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure portalına](https://portal.azure.com/)giriş yapın.
2. Azure portalındaki sol menüden **Tüm kaynakları**seçin ve ardından oluşturduğunuz sunucuyu **(mydemoserver**gibi) arayın.
3. Sunucu adını seçin.
4. Sunucunun **Genel Bakış** panelinden **Sunucu adı** ile **Sunucu yöneticisi oturum açma adı**’nı not alın. Parolanızı unutursanız, bu panelden parolayı da sıfırlayabilirsiniz.
 ![MySQL için Azure Veritabanı sunucu adı](./media/connect-java/azure-database-mysql-server-name.png)

## <a name="connect-create-table-and-insert-data"></a>Bağlanma, tablo oluşturma ve veri ekleme

Bağlanmak ve **INSERT** SQL deyimiyle birlikte işlevi kullanarak verileri yüklemek için aşağıdaki kodu kullanın. [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager) yöntemi MySQL'e bağlanmak için kullanılır. [createStatement()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#creating-a-table-on-a-mariadb-or-mysql-server) ve execute() yöntemleri tabloyu bırakmak ve oluşturmak için kullanılır. Parametre değerlerini bağlamak için kullanılan setString() ve setInt() ile birlikte ekleme komutlarını oluşturmak için prepareStatement nesnesi kullanılır. executeUpdate() yöntemi, değerleri eklemek üzere her parametre kümesi için komutu çalıştırır. 

host, database, user ve password parametrelerini kendi sunucunuzu ve veritabanınızı oluştururken belirttiğiniz değerlerle değiştirin.

```java
import java.sql.*;
import java.util.Properties;

public class CreateTableInsertRows {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("org.mariadb.jdbc");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MariaDB JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MariaDB JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Drop previous table of same name if one exists.
                Statement statement = connection.createStatement();
                statement.execute("DROP TABLE IF EXISTS inventory;");
                System.out.println("Finished dropping table (if existed).");
    
                // Create table.
                statement.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
                System.out.println("Created table.");
                
                // Insert some data into table.
                int nRowsInserted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO inventory (name, quantity) VALUES (?, ?);");
                preparedStatement.setString(1, "banana");
                preparedStatement.setInt(2, 150);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "orange");
                preparedStatement.setInt(2, 154);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "apple");
                preparedStatement.setInt(2, 100);
                nRowsInserted += preparedStatement.executeUpdate();
                System.out.println(String.format("Inserted %d row(s) of data.", nRowsInserted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
    
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}

```

## <a name="read-data"></a>Verileri okuma

**SELECT** SQL deyimiyle verileri okumak için aşağıdaki kodu kullanın. [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager) yöntemi MySQL'e bağlanmak için kullanılır. Select deyimini bağlamak ve çalıştırmak için [Statement()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#creating-a-table-on-a-mariadb-or-mysql-server) ve executeQuery() yöntemleri kullanılır. Sonuçlar ResultSet nesnesi kullanılarak işlenir. 

host, database, user ve password parametrelerini kendi sunucunuzu ve veritabanınızı oluştururken belirttiğiniz değerlerle değiştirin.

```java
import java.sql.*;
import java.util.Properties;

public class ReadTable {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("org.mariadb.jdbc");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MariaDB JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MariaDB JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
    
                Statement statement = connection.createStatement();
                ResultSet results = statement.executeQuery("SELECT * from inventory;");
                while (results.next())
                {
                    String outputString = 
                        String.format(
                            "Data row = (%s, %s, %s)",
                            results.getString(1),
                            results.getString(2),
                            results.getString(3));
                    System.out.println(outputString);
                }
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database."); 
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="update-data"></a>Verileri güncelleştirme

**UPDATE** SQL deyimiyle verileri değiştirmek için aşağıdaki kodu kullanın. [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager) yöntemi MySQL'e bağlanmak için kullanılır. UPDATE deyimini hazırlamak ve çalıştırmak için [prepareStatement()](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) ve executeUpdate() yöntemleri kullanılır. 

host, database, user ve password parametrelerini kendi sunucunuzu ve veritabanınızı oluştururken belirttiğiniz değerlerle değiştirin.

```java
import java.sql.*;
import java.util.Properties;

public class UpdateTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("org.mariadb.jdbc");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MariaDB JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MariaDB JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Modify some data in table.
                int nRowsUpdated = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?;");
                preparedStatement.setInt(1, 200);
                preparedStatement.setString(2, "banana");
                nRowsUpdated += preparedStatement.executeUpdate();
                System.out.println(String.format("Updated %d row(s) of data.", nRowsUpdated));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="delete-data"></a>Verileri silme

**DELETE** SQL deyimiyle verileri kaldırmak için aşağıdaki kodu kullanın. [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager) yöntemi MySQL'e bağlanmak için kullanılır.  Delete deyimini hazırlamak ve çalıştırmak için [Statement()](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) ve executeUpdate() yöntemleri kullanılır. 

host, database, user ve password parametrelerini kendi sunucunuzu ve veritabanınızı oluştururken belirttiğiniz değerlerle değiştirin.

```java
import java.sql.*;
import java.util.Properties;

public class DeleteTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";
        
        // check that the driver is installed
        try
        {
            Class.forName("org.mariadb.jdbc");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MariaDB JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MariaDB JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Delete some data from table.
                int nRowsDeleted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM inventory WHERE name = ?;");
                preparedStatement.setString(1, "orange");
                nRowsDeleted += preparedStatement.executeUpdate();
                System.out.println(String.format("Deleted %d row(s) of data.", nRowsDeleted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Döküm alma ve geri yükleme işlemlerini kullanarak MySQL veritabanınızı MySQL için Azure Veritabanı'na geçirme](concepts-migrate-dump-restore.md)
