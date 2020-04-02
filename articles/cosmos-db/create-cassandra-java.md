---
title: Azure Cosmos DB Cassandra API ile bir Java uygulaması oluşturun
description: Bu hızlı başlangıçta Azure portalı ve Java ile profil uygulaması oluşturmak için Azure Cosmos DB Cassandra API’sinin nasıl kullanılacağı gösterilmektedir
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.subservice: cosmosdb-cassandra
ms.devlang: java
ms.topic: quickstart
ms.date: 09/24/2018
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 4c93ffa7828710c26200408666c03a28fd7f3d3c
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549283"
---
# <a name="quickstart-build-a-java-app-to-manage-azure-cosmos-db-cassandra-api-data"></a>Hızlı başlangıç: Azure Cosmos DB Cassandra API verilerini yönetmek için bir Java uygulaması oluşturun

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [Java](create-cassandra-java.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
>  

Bu hızlı başlangıçta, bir Azure Cosmos DB Cassandra API hesabı oluşturursunuz ve Bir Cassandra veritabanı ve kapsayıcı oluşturmak için GitHub'dan klonlanmış bir Cassandra Java uygulaması kullanırsınız. Azure Cosmos DB, belge, tablo, anahtar değeri ve grafik veritabanlarını küresel dağıtım ve yatay ölçek özelliklerine sahip hızlı bir şekilde oluşturmanıza ve sorgulamanıza olanak tanıyan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Veya Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz olarak deneyin.](https://azure.microsoft.com/try/cosmosdb/)
- [Java Geliştirme Kiti (JDK) 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts&architecture=x86-64-bit&package=jdk). Ortam `JAVA_HOME` değişkeninizi JDK'nın yüklü olduğu klasöre yönlendirin.
- [Bir Maven ikili arşiv](https://maven.apache.org/download.cgi). Ubuntu'da, `apt-get install maven` Maven'i yüklemek için koş.
- [Git.](https://www.git-scm.com/downloads) Ubuntu'da `sudo apt-get install git` Git'i yüklemek için koş.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir belge veritabanı oluşturmadan önce Azure Cosmos DB ile bir Cassandra hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi kod ile çalışmaya geçelim. GitHub’dan bir Cassandra uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Bir komut istemi açın. `git-samples` adlı yeni bir klasör oluşturun. Ardından, komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere yeni bir klasör olarak değiştirmek için `cd` komutunu kullanın.

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Kodun veritabanı kaynaklarını nasıl oluşturduğunu öğrenmek istiyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. Bu parçacıklar *src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java* dosyasından alınmıştır.  

* Cassandra ana bilgisayar, bağlantı noktası, kullanıcı adı, parola ve TLS/SSL seçenekleri ayarlanır. Bağlantı dizesi bilgileri Azure portalındaki bağlantı dizesi sayfasından gelir.

   ```java
   cluster = Cluster.builder().addContactPoint(cassandraHost).withPort(cassandraPort).withCredentials(cassandraUsername, cassandraPassword).withSSL(sslOptions).build();
   ```

* `cluster`, Azure Cosmos DB Cassandra API’sine bağlanır ve erişilecek bir oturum döndürür.

    ```java
    return cluster.connect();
    ```

Aşağıdaki parçacıklar *src/main/java/com/azure/cosmosdb/cassandra/repository/UserRepository.java* dosyasından dır.

* Yeni bir anahtar alanı oluşturun.

    ```java
    public void createKeyspace() {
        final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '3' } ";
        session.execute(query);
        LOGGER.info("Created keyspace 'uprofile'");
    }
    ```

* Yeni bir tablo oluşturun.

   ```java
   public void createTable() {
        final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)";
        session.execute(query);
        LOGGER.info("Created table 'user'");
   }
   ```

* Hazırlanmış bir deyim nesnesini kullanarak kullanıcı varlıkları ekleyin.

    ```java
    public PreparedStatement prepareInsertStatement() {
        final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
        return session.prepare(insertStatement);
    }

    public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
    }
    ```

* Tüm kullanıcı bilgilerini almak için sorgulayın.

    ```java
   public void selectAllUsers() {
        final String query = "SELECT * FROM uprofile.user";
        List<Row> rows = session.execute(query).all();

        for (Row row : rows) {
            LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
        }
    }
    ```

* Tek bir kullanıcının bilgilerini almak için sorgulayın.

    ```java
    public void selectUser(int id) {
        final String query = "SELECT * FROM uprofile.user where user_id = 3";
        Row row = session.execute(query).one();

        LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
    }
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bağlantı dizesi ayrıntıları, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır.

1. [Azure portalındaki](https://portal.azure.com/)Azure Cosmos DB hesabınızda **Bağlantı Dizesini**seçin. 

    ![Azure portalı, Bağlantı Dizesi sayfasından kullanıcı adını görüntüleme ve kopyalama](./media/create-cassandra-java/copy-username-connection-string-azure-portal.png)

2. En üstteki USERNAME değerini kopyalamak için ekranın sağ tarafındaki ![Kopyala düğmesini](./media/create-cassandra-java/copy-button-azure-portal.png) kullanın.

3. *C:\git-samples\azure-cosmosdb-cassandra-java-getting-started\java-examples\src\main\resources* klasöründen *config.properties* dosyasını açın. 

3. Portaldan CONTACT POINT değerini 2. satırdaki `<Cassandra endpoint host>` üzerine yapıştırın.

    *Config.properties* satır 2 şimdi benzer görünmelidir 

    `cassandra_host=cosmos-db-quickstart.cassandra.cosmosdb.azure.com`

3. Portala geri dön ve USERNAME değerini kopyalayın. Portaldan USERNAME değerini 4. satırdaki `<cassandra endpoint username>` üzerine yapıştırın.

    *Config.properties'in 4.* 

    `cassandra_username=cosmos-db-quickstart`

4. Portala geri dön ve PAROLA değerini kopyala. Portaldan PASSWORD değerini 5. satırdaki `<cassandra endpoint password>` üzerine yapıştırın.

    *Config.properties* Satır 5 şimdi benzer görünmelidir 

    `cassandra_password=2Ggkr662ifxz2Mg...==`

5. Satır 6'da, belirli bir TLS/SSL sertifikası kullanmak `<SSL key store file location>` istiyorsanız, TLS/SSL sertifikasının konumuyla değiştirin. Bir değer sağlanmamışsa, <JAVA_HOME>/jre/lib/security/cacerts konumuna yüklenen JDK sertifikası kullanılır. 

6. Belirli bir TLS/SSL sertifikasını kullanmak için 6 satırını değiştirdiyseniz, söz konusu sertifikanın parolasını kullanmak için satır 7'yi güncelleyin. 

7. *config.properties* dosyasını kaydedin.

## <a name="run-the-java-app"></a>Java uygulamasını çalıştırma

1. Git terminal penceresinde, `azure-cosmosdb-cassandra-java-getting-started\java-examples` klasörüne `cd`.

    ```git
    cd "C:\git-samples\azure-cosmosdb-cassandra-java-getting-started\java-examples"
    ```

2. Git terminal penceresinde, `cosmosdb-cassandra-examples.jar` dosyasını oluşturmak için aşağıdaki komutları kullanın.

    ```git
    mvn clean install
    ```

3. Git terminal penceresinde, Java uygulamasını başlatmak için aşağıdaki komutu çalıştırın.

    ```git
    java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
    ```

    Terminal penceresinde anahtar alanı ve tablonun oluşturulduğuna yönelik bildirimler gösterilir. Daha sonra tablodaki tüm kullanıcıları seçip döndürür ve çıktıyı gösterir, ardından kimliğe göre bir satır seçip değeri gösterir.  

    Programın yürütülmesini durdurmak ve konsol penceresini kapatmak için Ctrl+C tuşuna basın.

4. Azure portalında bu yeni verileri sorgulamak, değiştirmek ve birlikte çalışmak için **Veri Gezgini**'ni açın. 

    ![Verileri Data Explorer'da görüntüleme - Azure Cosmos DB](./media/create-cassandra-java/view-data-explorer-java-app.png)

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Cassandra API ile azure cosmos DB hesabı oluşturmayı ve Cassandra veritabanı ve kapsayıcı oluşturan bir Cassandra Java uygulamasını çalıştırmayı öğrendiniz. Artık Azure Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Cassandra verilerini Azure Cosmos DB’ye aktarma](cassandra-import-data.md)
