---
title: MySQL için Azure veritabanı ile Java ve JDBC kullanma
description: Bir MySQL veritabanı için Azure veritabanı ile Java ve JDBC kullanmayı öğrenin.
author: jdubois
ms.author: judubois
ms.service: mysql
ms.custom: mvc, devcenter
ms.topic: quickstart
ms.devlang: java
ms.date: 08/17/2020
ms.openlocfilehash: a54e950286a37c207d902090f015b3732e0ff10b
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88517591"
---
# <a name="use-java-and-jdbc-with-azure-database-for-mysql"></a>MySQL için Azure veritabanı ile Java ve JDBC kullanma

Bu konu başlığı altında, [MySQL Için Azure veritabanı](https://docs.microsoft.com/azure/mysql/)'nda bilgi depolamak ve almak üzere Java ve [JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity) kullanan bir örnek uygulama oluşturma gösterilmektedir.

JDBC, geleneksel ilişkisel veritabanlarına bağlanmak için standart Java API 'sidir.

## <a name="prerequisites"></a>Ön koşullar

- Azure hesabı. Hiç kimse yoksa [ücretsiz deneme sürümü alın](https://azure.microsoft.com/free/).
- [Azure Cloud Shell](/azure/cloud-shell/quickstart) veya [Azure CLI](/cli/azure/install-azure-cli). Otomatik olarak oturum açabileceksiniz ve ihtiyacınız olan tüm araçlara erişebilmek için Azure Cloud Shell önerilir.
- Desteklenen bir [Java Development Kit](https://aka.ms/azure-jdks), sürüm 8 (Azure Cloud Shell içinde bulunur).
- [Apache Maven](https://maven.apache.org/) derleme aracı.

## <a name="prepare-the-working-environment"></a>Çalışma ortamını hazırlama

Yazma hatalarını sınırlamak ve özel gereksinimleriniz için aşağıdaki yapılandırmayı özelleştirmenizi kolaylaştırmak için ortam değişkenlerini kullanacağız.

Aşağıdaki komutları kullanarak bu ortam değişkenlerini ayarlayın:

```bash
AZ_RESOURCE_GROUP=database-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_MYSQL_USERNAME=demo
AZ_MYSQL_PASSWORD=<YOUR_MYSQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

Yer tutucuları, bu makale boyunca kullanılan aşağıdaki değerlerle değiştirin:

- `<YOUR_DATABASE_NAME>`: MySQL sunucunuzun adı. Azure genelinde benzersiz olmalıdır.
- `<YOUR_AZURE_REGION>`: Kullanacağınız Azure bölgesi. `eastus`Varsayılan olarak kullanabilirsiniz, ancak bir bölgeyi yaşadığınız yere yakın bir yerde yapılandırmanızı öneririz. ' İ girerek kullanılabilir bölgelerin tam listesini alabilirsiniz `az account list-locations` .
- `<YOUR_MYSQL_PASSWORD>`: MySQL veritabanı sunucunuzun parolası. Bu parola en az sekiz karakter uzunluğunda olmalıdır. Karakterler şu kategorilerden üçünde olmalıdır: Ingilizce büyük harfler, Ingilizce küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #,%, vb.).
- `<YOUR_LOCAL_IP_ADDRESS>`: Java uygulamanızı çalıştıracağınız yerel bilgisayarınızın IP adresi. Bunu bulmanın kolay bir yolu, tarayıcınızı [whatismyip.Akamai.com](http://whatismyip.akamai.com/)'e işaret kullanmaktır.

Sonra, bir kaynak grubu oluşturun:

```azurecli
az group create \
    --name $AZ_RESOURCE_GROUP \
    --location $AZ_LOCATION \
  	| jq
```

> [!NOTE]
> `jq`JSON verilerini göstermek ve daha okunabilir hale getirmek için [Azure Cloud Shell](https://shell.azure.com/) varsayılan olarak yüklenen yardımcı programı kullanıyoruz.
> Bu yardımcı programı beğenmezseniz, `| jq` kullanacağımız tüm komutların bölümünü güvenle kaldırabilirsiniz.

## <a name="create-an-azure-database-for-mysql-instance"></a>MySQL için Azure veritabanı örneği oluşturma

Oluşturacağız ilk şey, yönetilen bir MySQL sunucusudur.

> [!NOTE]
> Azure portal kullanarak MySQL sunucusu oluşturma hakkında daha ayrıntılı bilgi edinmek için bkz. [MySQL Için Azure veritabanı sunucusu oluşturma](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).

[Azure Cloud Shell](https://shell.azure.com/), aşağıdaki betiği çalıştırın:

```azurecli
az mysql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --sku-name B_Gen5_1 \
    --storage-size 5120 \
    --admin-user $AZ_MYSQL_USERNAME \
    --admin-password $AZ_MYSQL_PASSWORD \
  	| jq
```

Bu komut, küçük bir MySQL sunucusu oluşturur.

### <a name="configure-a-firewall-rule-for-your-mysql-server"></a>MySQL sunucunuz için bir güvenlik duvarı kuralı yapılandırma

MySQL için Azure veritabanı örnekleri varsayılan olarak güvenli hale getirilir. Herhangi bir gelen bağlantıya izin veren bir güvenlik duvarı vardır. Veritabanınızı kullanabilmeniz için, yerel IP adresinin veritabanı sunucusuna erişmesine imkan sağlayacak bir güvenlik duvarı kuralı eklemeniz gerekir.

Bu makalenin başlangıcında yerel IP adresimizi yapılandırdığınız için, şunu çalıştırarak sunucunun güvenlik duvarını açabilirsiniz:

```azurecli
az mysql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
  	| jq
```

### <a name="configure-a-mysql-database"></a>MySQL veritabanı yapılandırma

Daha önce oluşturduğunuz MySQL sunucusu boştur. Java uygulamasıyla kullanabileceğiniz herhangi bir veritabanı yoktur. Adlı yeni bir veritabanı oluşturun `demo` :

```azurecli
az mysql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name demo \
    --server-name $AZ_DATABASE_NAME \
  	| jq
```

### <a name="create-a-new-java-project"></a>Yeni bir Java projesi oluşturma

En sevdiğiniz IDE 'yi kullanarak yeni bir Java projesi oluşturun ve `pom.xml` kök dizinine bir dosya ekleyin:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo</name>

    <properties>
        <java.version>1.8</java.version>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.20</version>
        </dependency>
    </dependencies>
</project>
```

Bu dosya, projemizi kullanacak şekilde yapılandıran bir [Apache Maven](https://maven.apache.org/) :

- Java 8
- Java için son MySQL sürücüsü

### <a name="prepare-a-configuration-file-to-connect-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'na bağlanmak üzere bir yapılandırma dosyası hazırlama

*Src/Main/Resources/Application. Properties* dosyası oluşturun ve şunları ekleyin:

```properties
url=jdbc:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/demo?serverTimezone=UTC
user=demo@$AZ_DATABASE_NAME
password=$AZ_MYSQL_PASSWORD
```

- İki değişkeni, `$AZ_DATABASE_NAME` Bu makalenin başlangıcında yapılandırdığınız değerle değiştirin.
- Değişkenini, `$AZ_MYSQL_PASSWORD` Bu makalenin başlangıcında yapılandırdığınız değerle değiştirin.

> [!NOTE]
> `?serverTimezone=UTC` `url` VERITABANıNA bağlanırken JDBC sürücüsüne UTC tarih biçimini (veya Eşgüdümlü Evrensel Saat) kullanmasını söylemek için yapılandırma özelliğine ekledik. Aksi halde, Java sunucusu veritabanıyla aynı tarih biçimini kullanmaz ve bu da hataya yol açabilir.

### <a name="create-an-sql-file-to-generate-the-database-schema"></a>Veritabanı şemasını oluşturmak için bir SQL dosyası oluşturma

Veritabanı şeması oluşturmak için bir *src/Main/kaynaklar `schema.sql` /* dosya kullanacağız. Bu dosyayı aşağıdaki içerikle oluşturun:

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

## <a name="code-the-application"></a>Uygulamayı kodlayın

### <a name="connect-to-the-database"></a>Veritabanına bağlanın

Ardından, MySQL sunucunuzdaki verileri depolamak ve almak için JDBC kullanacak Java kodunu ekleyin.

Şunu içeren bir *src/Main/Java/DemoApplication. Java* dosyası oluşturun:

```java
package com.example.demo;

import com.mysql.cj.jdbc.AbandonedConnectionCleanupThread;

import java.sql.*;
import java.util.*;
import java.util.logging.Logger;

public class DemoApplication {

    private static final Logger log;

    static {
        System.setProperty("java.util.logging.SimpleFormatter.format", "[%4$-7s] %5$s %n");
        log =Logger.getLogger(DemoApplication.class.getName());
    }

    public static void main(String[] args) throws Exception {
        log.info("Loading application properties");
        Properties properties = new Properties();
        properties.load(DemoApplication.class.getClassLoader().getResourceAsStream("application.properties"));

        log.info("Connecting to the database");
        Connection connection = DriverManager.getConnection(properties.getProperty("url"), properties);
        log.info("Database connection test: " + connection.getCatalog());

        log.info("Create database schema");
        Scanner scanner = new Scanner(DemoApplication.class.getClassLoader().getResourceAsStream("schema.sql"));
        Statement statement = connection.createStatement();
        while (scanner.hasNextLine()) {
            statement.execute(scanner.nextLine());
        }

        /*
        Todo todo = new Todo(1L, "configuration", "congratulations, you have set up JDBC correctly!", true);
        insertData(todo, connection);
        todo = readData(connection);
        todo.setDetails("congratulations, you have updated data!");
        updateData(todo, connection);
        deleteData(todo, connection);
        */

        log.info("Closing database connection");
        connection.close();
        AbandonedConnectionCleanupThread.uncheckedShutdown();
    }
}
```

Bu Java kodu, MySQL sunucusuna bağlanmak ve verilerimizi depolayacak bir şema oluşturmak için daha önce oluşturduğumuz *Application. Properties* ve *Schema. SQL* dosyalarını kullanacaktır.

Bu dosyada, verileri ekleme, okuma, güncelleştirme ve silmeye yönelik yöntemler hakkında yorum yaptık. Bu makalenin geri kalanında bu yöntemleri kodlarız ve bunların her birinin açıklamalarını oluşturabileceksiniz.

> [!NOTE]
> Veritabanı kimlik bilgileri, *Application. Properties* dosyasının *Kullanıcı* ve *parola* özelliklerinde depolanır. `DriverManager.getConnection(properties.getProperty("url"), properties);`Özellikler dosyası bir bağımsız değişken olarak geçirildiğinden, bu kimlik bilgileri çalıştırıldığında kullanılır.

> [!NOTE]
> `AbandonedConnectionCleanupThread.uncheckedShutdown();`Sonundaki çizgi, uygulamayı kapatırken bir iç iş parçacığını yok etmek Için MySQL sürücüye özgü bir komuttur.
> Güvenle yoksayılabilir. 

Artık bu ana sınıfı en sevdiğiniz aracı ile çalıştırabilirsiniz:

- IDE 'nizi kullanarak *Demoapplication* sınıfına sağ tıklayıp yürütebilmelisiniz.
- Maven kullanarak uygulamayı çalıştırarak çalıştırabilirsiniz: `mvn exec:java -Dexec.mainClass="com.example.demo.DemoApplication"` .

Uygulama, MySQL için Azure veritabanı 'na bağlanmalıdır, bir veritabanı şeması oluşturun ve konsol günlüklerinde gördüğünüz gibi bağlantıyı kapatır:

```
[INFO   ] Loading application properties 
[INFO   ] Connecting to the database 
[INFO   ] Database connection test: demo 
[INFO   ] Create database schema 
[INFO   ] Closing database connection 
```

### <a name="create-a-domain-class"></a>Etki alanı sınıfı oluşturma

`Todo`Sınıfının yanında yeni bir Java sınıfı oluşturun `DemoApplication` ve aşağıdaki kodu ekleyin:

```java
package com.example.demo;

public class Todo {

    private Long id;
    private String description;
    private String details;
    private boolean done;

    public Todo() {
    }

    public Todo(Long id, String description, String details, boolean done) {
        this.id = id;
        this.description = description;
        this.details = details;
        this.done = done;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }

    public boolean isDone() {
        return done;
    }

    public void setDone(boolean done) {
        this.done = done;
    }

    @Override
    public String toString() {
        return "Todo{" +
                "id=" + id +
                ", description='" + description + '\'' +
                ", details='" + details + '\'' +
                ", done=" + done +
                '}';
    }
}
```

Bu sınıf, `todo` *şema. SQL* betiği yürütürken oluşturduğunuz tabloda eşlenmiş bir etki alanı modelidir.

### <a name="insert-data-into-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'na veri ekleme

*Src/Main/Java/DemoApplication. Java* dosyasında, Main yönteminden sonra veritabanına veri eklemek için aşağıdaki yöntemi ekleyin:

```java
private static void insertData(Todo todo, Connection connection) throws SQLException {
    log.info("Insert data");
    PreparedStatement insertStatement = connection
            .prepareStatement("INSERT INTO todo (id, description, details, done) VALUES (?, ?, ?, ?);");

    insertStatement.setLong(1, todo.getId());
    insertStatement.setString(2, todo.getDescription());
    insertStatement.setString(3, todo.getDetails());
    insertStatement.setBoolean(4, todo.isDone());
    insertStatement.executeUpdate();
}
```

Artık yönteminde aşağıdaki iki satırın açıklamasını bulabilirsiniz `main` :

```java
Todo todo = new Todo(1L, "configuration", "congratulations, you have set up JDBC correctly!", true);
insertData(todo, connection);
```

Ana sınıfın yürütülmesi şu çıktıyı üretmelidir:

```
[INFO   ] Loading application properties 
[INFO   ] Connecting to the database 
[INFO   ] Database connection test: demo 
[INFO   ] Create database schema 
[INFO   ] Insert data 
[INFO   ] Closing database connection
```

### <a name="reading-data-from-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'ndan veri okuma

Kodunuzun doğru şekilde çalışıp çalışmadığını doğrulamak için daha önce eklenen verileri okuyalim.

*Src/Main/Java/DemoApplication. Java* dosyasında, yönteminden sonra, `insertData` veritabanından veri okumak için aşağıdaki yöntemi ekleyin:

```java
private static Todo readData(Connection connection) throws SQLException {
    log.info("Read data");
    PreparedStatement readStatement = connection.prepareStatement("SELECT * FROM todo;");
    ResultSet resultSet = readStatement.executeQuery();
    if (!resultSet.next()) {
        log.info("There is no data in the database!");
        return null;
    }
    Todo todo = new Todo();
    todo.setId(resultSet.getLong("id"));
    todo.setDescription(resultSet.getString("description"));
    todo.setDetails(resultSet.getString("details"));
    todo.setDone(resultSet.getBoolean("done"));
    log.info("Data read from the database: " + todo.toString());
    return todo;
}
```

Artık yönteminde aşağıdaki satırı açıklama ekleyebilirsiniz `main` :

```java
todo = readData(connection);
```

Ana sınıfın yürütülmesi şu çıktıyı üretmelidir:

```
[INFO   ] Loading application properties 
[INFO   ] Connecting to the database 
[INFO   ] Database connection test: demo 
[INFO   ] Create database schema 
[INFO   ] Insert data 
[INFO   ] Read data 
[INFO   ] Data read from the database: Todo{id=1, description='configuration', details='congratulations, you have set up JDBC correctly!', done=true} 
[INFO   ] Closing database connection 
```

### <a name="updating-data-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda veri güncelleştirme

Daha önce eklediğimiz verileri güncelleştirelim.

Hala *src/Main/Java/DemoApplication. Java* dosyasında, yönteminden sonra, `readData` veritabanı içindeki verileri güncelleştirmek için aşağıdaki yöntemi ekleyin:

```java
private static void updateData(Todo todo, Connection connection) throws SQLException {
    log.info("Update data");
    PreparedStatement updateStatement = connection
            .prepareStatement("UPDATE todo SET description = ?, details = ?, done = ? WHERE id = ?;");

    updateStatement.setString(1, todo.getDescription());
    updateStatement.setString(2, todo.getDetails());
    updateStatement.setBoolean(3, todo.isDone());
    updateStatement.setLong(4, todo.getId());
    updateStatement.executeUpdate();
    readData(connection);
}
```

Artık yönteminde aşağıdaki iki satırın açıklamasını bulabilirsiniz `main` :

```java
todo.setDetails("congratulations, you have updated data!");
updateData(todo, connection);
```

Ana sınıfın yürütülmesi şu çıktıyı üretmelidir:

```
[INFO   ] Loading application properties 
[INFO   ] Connecting to the database 
[INFO   ] Database connection test: demo 
[INFO   ] Create database schema 
[INFO   ] Insert data 
[INFO   ] Read data 
[INFO   ] Data read from the database: Todo{id=1, description='configuration', details='congratulations, you have set up JDBC correctly!', done=true} 
[INFO   ] Update data 
[INFO   ] Read data 
[INFO   ] Data read from the database: Todo{id=1, description='configuration', details='congratulations, you have updated data!', done=true} 
[INFO   ] Closing database connection 
```

### <a name="deleting-data-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda verileri silme

Son olarak, daha önce eklediğimiz verileri silelim.

Hala *src/Main/Java/DemoApplication. Java* dosyasında, yönteminden sonra, `updateData` veritabanı içindeki verileri silmek için aşağıdaki yöntemi ekleyin:

```java
private static void deleteData(Todo todo, Connection connection) throws SQLException {
    log.info("Delete data");
    PreparedStatement deleteStatement = connection.prepareStatement("DELETE FROM todo WHERE id = ?;");
    deleteStatement.setLong(1, todo.getId());
    deleteStatement.executeUpdate();
    readData(connection);
}
```

Artık yönteminde aşağıdaki satırı açıklama ekleyebilirsiniz `main` :

```java
deleteData(todo, connection);
```

Ana sınıfın yürütülmesi şu çıktıyı üretmelidir:

```
[INFO   ] Loading application properties 
[INFO   ] Connecting to the database 
[INFO   ] Database connection test: demo 
[INFO   ] Create database schema 
[INFO   ] Insert data 
[INFO   ] Read data 
[INFO   ] Data read from the database: Todo{id=1, description='configuration', details='congratulations, you have set up JDBC correctly!', done=true} 
[INFO   ] Update data 
[INFO   ] Read data 
[INFO   ] Data read from the database: Todo{id=1, description='configuration', details='congratulations, you have updated data!', done=true} 
[INFO   ] Delete data 
[INFO   ] Read data 
[INFO   ] There is no data in the database! 
[INFO   ] Closing database connection 
```

## <a name="conclusion-and-resources-clean-up"></a>Sonuç ve kaynak Temizleme

Tebrikler! MySQL için Azure veritabanı 'ndan veri depolamak ve almak için JDBC kullanan bir Java uygulaması oluşturdunuz.

Bu hızlı başlangıç sırasında kullanılan tüm kaynakları temizlemek için, aşağıdaki komutu kullanarak kaynak grubunu silin:

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Döküm alma ve geri yükleme işlemlerini kullanarak MySQL veritabanınızı MySQL için Azure Veritabanı'na geçirme](concepts-migrate-dump-restore.md)
