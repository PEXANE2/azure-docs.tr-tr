---
title: Gocql istemcisini kullanarak Azure Cosmos DB Cassandra API ile Go uygulaması oluşturma
description: Bu hızlı başlangıçta Azure Cosmos DB ile etkileşim kurmak için bir go istemcisinin nasıl kullanılacağı gösterilmektedir Cassandra API
ms.service: cosmos-db
author: abhirockzz
ms.author: abhishgu
ms.subservice: cosmosdb-cassandra
ms.devlang: go
ms.topic: quickstart
ms.date: 07/14/2020
ms.openlocfilehash: ba53fb786b1d1f61535168cda2152049a12dfb99
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86535843"
---
# <a name="quickstart-build-a-go-app-with-the-gocql-client-to-manage-azure-cosmos-db-cassandra-api-data"></a>Hızlı başlangıç: `gocql` Azure Cosmos DB Cassandra API verileri yönetmek için istemciyle bir go uygulaması derleme

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [.NET Core](create-cassandra-dotnet-core.md)
> * [Java v3](create-cassandra-java.md)
> * [Java v4](create-cassandra-java-v4.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
> * [Golang](create-cassandra-go.md)
>

Azure Cosmos DB, genel dağıtım ve yatay ölçeklendirme özellikleri ile belge, tablo, anahtar değer ve grafik veritabanlarını hızlıca oluşturmanıza ve sorgulamanızı sağlayan çok modelli bir veritabanı hizmetidir. Bu hızlı başlangıçta Azure Cosmos DB bir Cassandra API hesabı oluşturarak başlayacaksınız. Daha sonra Cassandra keyspace ve tablo oluşturmak için bir go uygulaması çalıştıracak ve birkaç işlem yürütmeniz gerekir. Bu Go uygulaması, go dili için Cassandra istemcisi olan [gocql](https://github.com/gocql/gocql)kullanır. 

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=cassandrago-docs-abhishgu). Veya Azure aboneliği olmadan [ücretsiz Azure Cosmos DB deneyin](https://azure.microsoft.com/try/cosmosdb/?WT.mc_id=cassandrago-docs-abhishgu) .
- Bilgisayarınızda yüklü ve çalışır bir go hakkında bilgi sahibi [olun](https://golang.org/) .
- [Git](https://git-scm.com/downloads).

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir veritabanı oluşturabilmeniz için önce Azure Cosmos DB bir Cassandra hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Uygulamayı GitHub ' dan kopyalayarak başlayın.

1. Bir komut istemi açın ve adlı yeni bir klasör oluşturun `git-samples` .

    ```bash
    md "C:\git-samples"
    ```

2. Git bash gibi bir git terminal penceresi açın. `cd`Yeni klasöre geçmek ve örnek uygulamayı yüklemek için komutunu kullanın.

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-go-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Kodun veritabanı kaynaklarını nasıl oluşturduğunu öğrenmek isterseniz, aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi takdirde, [Uygulamayı çalıştırmaya](#run-the-application) devam edebilirsiniz

`GetSession`İşlevi (parçası `utils\utils.go` ) [`*gocql.Session`](https://godoc.org/github.com/gocql/gocql#Session) ekleme, bulma vb. gibi küme işlemlerini yürütmek için kullanılan bir döndürür.

```go
func GetSession(cosmosCassandraContactPoint, cosmosCassandraPort, cosmosCassandraUser, cosmosCassandraPassword string) *gocql.Session {
    clusterConfig := gocql.NewCluster(cosmosCassandraContactPoint)
    port, err := strconv.Atoi(cosmosCassandraPort)
    
    clusterConfig.Authenticator = gocql.PasswordAuthenticator{Username: cosmosCassandraUser, Password: cosmosCassandraPassword}
    clusterConfig.Port = port
    clusterConfig.SslOpts = &gocql.SslOptions{Config: &tls.Config{MinVersion: tls.VersionTLS12}}
    clusterConfig.ProtoVersion = 4
    
    session, err := clusterConfig.CreateSession()
    ...
    return session
}
```

[`gocql.NewCluster`](https://godoc.org/github.com/gocql/gocql#NewCluster) [`*gocql.ClusterConfig`](https://godoc.org/github.com/gocql/gocql#ClusterConfig) Kullanıcı adı, parola, bağlantı noktası ve uygun TLS sürümünü ([HTTPS/SSL/TLS şifreleme güvenlik gereksinimi](https://docs.microsoft.com/azure/cosmos-db/database-security?WT.mc_id=cassandrago-docs-abhishgu#how-does-azure-cosmos-db-secure-my-database)) kullanmak üzere yapılandırılmış bir yapı almak için cassandra Konağı Azure Cosmos DB işleve geçirilir

`GetSession`İşlev daha sonra `main` () işlevinden çağırılır `main.go` .

```go
func main() {
    session := utils.GetSession(cosmosCassandraContactPoint, cosmosCassandraPort, cosmosCassandraUser, cosmosCassandraPassword)
    defer session.Close()
    ...
}
```

Bağlantı bilgileri ve kimlik bilgileri, ortam değişkenleri biçiminde kabul edilir ( `init` yönteminde çözümlendi)

```go
func init() {
    cosmosCassandraContactPoint = os.Getenv("COSMOSDB_CASSANDRA_CONTACT_POINT")
    cosmosCassandraPort = os.Getenv("COSMOSDB_CASSANDRA_PORT")
    cosmosCassandraUser = os.Getenv("COSMOSDB_CASSANDRA_USER")
    cosmosCassandraPassword = os.Getenv("COSMOSDB_CASSANDRA_PASSWORD")

    if cosmosCassandraContactPoint == "" || cosmosCassandraUser == "" || cosmosCassandraPassword == "" {
        log.Fatal("missing mandatory environment variables")
    }
}
```

Daha sonra, `operations\setup.go` ve oluşturma ile başlayıp Azure Cosmos DB çeşitli işlemleri (parçası) yürütmek için kullanılır `keyspace` `table` .

Adından da anlaşılacağı gibi, `DropKeySpaceIfExists` işlev varsa, işlevi `keyspace` onu bırakır.

```go
const dropKeyspace = "DROP KEYSPACE IF EXISTS %s"

func DropKeySpaceIfExists(keyspace string, session *gocql.Session) {
    err := utils.ExecuteQuery(fmt.Sprintf(dropKeyspace, keyspace), session)
    if err != nil {
        log.Fatal("Failed to drop keyspace", err)
    }
    log.Println("Keyspace dropped")
}
```

`CreateKeySpace`işlevi `keyspace` () oluşturmak için kullanılır `user_profile`

```go
const createKeyspace = "CREATE KEYSPACE %s WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1 }"

func CreateKeySpace(keyspace string, session *gocql.Session) {
    err := utils.ExecuteQuery(fmt.Sprintf(createKeyspace, keyspace), session)
    if err != nil {
        log.Fatal("Failed to create keyspace", err)
    }
    log.Println("Keyspace created")
}
```

Bu, `user` işlevin ilgilendikleri tablo oluşturma () ile izlenir `CreateUserTable`

```go
const createTable = "CREATE TABLE %s.%s (user_id int PRIMARY KEY, user_name text, user_bcity text)"

func CreateUserTable(keyspace, table string, session *gocql.Session) {
    err := session.Query(fmt.Sprintf(createTable, keyspace, table)).Exec()
    if err != nil {
        log.Fatal("failed to create table ", err)
    }
    log.Println("Table created")
}
```

Anahtar alanı ve Table oluşturulduktan sonra CRUD işlemlerini (parçasını) çağıracağız `operations\crud.go` . 

`InsertUser`, oluşturmak için kullanılır `User` . Kullanarak Kullanıcı bilgilerini (KIMLIK, ad ve şehir) sorgu bağımsız değişkenleri olarak ayarlar[`Bind`](https://godoc.org/github.com/gocql/gocql#Query.Bind)

```go
const createQuery = "INSERT INTO %s.%s (user_id, user_name , user_bcity) VALUES (?,?,?)"

func InsertUser(keyspace, table string, session *gocql.Session, user model.User) {
    err := session.Query(fmt.Sprintf(createQuery, keyspace, table)).Bind(user.ID, user.Name, user.City).Exec()
    if err != nil {
        log.Fatal("Failed to create user", err)
    }
    log.Println("User created")
}
```

`FindUser`, `model\user.go` [`Scan`](https://godoc.org/github.com/gocql/gocql#Iter.Scan) Kullanıcı özniteliklerini (Cassandra tarafından döndürülen) bağımsız değişkenlere (,,) bağlalarken (,,) bir kullanıcı kimliğini kullanarak bir kullanıcıyı () aramak için kullanılır. `userid` `name` `city` arama sorgu sonucu olarak elde edilen sonucu kullanmanın bir yolu vardır

```go
const selectQuery = "SELECT * FROM %s.%s where user_id = ?"

func FindUser(keyspace, table string, id int, session *gocql.Session) model.User {
    var userid int
    var name, city string
    err := session.Query(fmt.Sprintf(selectQuery, keyspace, table)).Bind(id).Scan(&userid, &name, &city)

    if err != nil {
        if err == gocql.ErrNotFound {
            log.Printf("User with id %v does not exist\n", id)
        } else {
            log.Printf("Failed to find user with id %v - %v\n", id, err)
        }
    }
    return model.User{ID: userid, Name: name, City: city}
}
```

`FindAllUsers`tüm kullanıcıları getirmek için kullanılır. [`SliceMap`](https://godoc.org/github.com/gocql/gocql#Iter.SliceMap), tüm Kullanıcı bilgilerini bir dizi dilimi biçiminde almak için bir toplu değer olarak kullanılır `map` . Her birini `map` , sütun adının (örneğin, `user_id` ) ilgili değeriyle birlikte anahtar-değer çiftleri olarak düşünün.

```go
const findAllUsersQuery = "SELECT * FROM %s.%s"

func FindAllUsers(keyspace, table string, session *gocql.Session) []model.User {
    var users []model.User
    results, _ := session.Query(fmt.Sprintf(findAllUsersQuery, keyspace, table)).Iter().SliceMap()

    for _, u := range results {
        users = append(users, mapToUser(u))
    }
    return users
}
```

`map`Kullanıcı bilgilerinin her biri, `User` `mapToUser` değeri ilgili sütundan çıkaran bir using işlevine dönüştürülür ve yapının bir örneğini oluşturmak için onu kullanır `User`

```go
func mapToUser(m map[string]interface{}) model.User {
    id, _ := m["user_id"].(int)
    name, _ := m["user_name"].(string)
    city, _ := m["user_bcity"].(string)

    return model.User{ID: id, Name: name, City: city}
}
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Daha önce belirtildiği gibi, uygulama, ortam değişkenleri biçiminde bağlantı ve kimlik bilgilerini kabul eder. 

1. [Azure portal](https://portal.azure.com/)Azure Cosmos DB hesabınızda **bağlantı dizesi**' ni seçin. 

    :::image type="content" source="./media/create-cassandra-go/copy-username-connection-string-azure-portal.png" alt-text="Azure portal bağlantı dizesi sayfasından ayrıntıları görüntüleyin ve kopyalayın":::

Aşağıdaki özniteliklerin değerlerini kopyalayın ( `CONTACT POINT` , `PORT` `USERNAME` ve `PRIMARY PASSWORD` ) ve bunları ilgili ortam değişkenlerine ayarlayın

```shell
set COSMOSDB_CASSANDRA_CONTACT_POINT=<value for "CONTACT POINT">
set COSMOSDB_CASSANDRA_PORT=<value for "PORT">
set COSMOSDB_CASSANDRA_USER=<value for "USERNAME">
set COSMOSDB_CASSANDRA_PASSWORD=<value for "PRIMARY PASSWORD">
```

Terminal penceresinde, doğru klasöre geçin. Örneğin:

```shell
cd "C:\git-samples\azure-cosmosdb-cassandra-go-getting-started"
```

2. Terminalde, uygulamayı başlatmak için aşağıdaki komutu çalıştırın.

```shell
go run main.go
```

3. Terminal penceresi, anahtar alanı ve tablo kurulumu, Kullanıcı oluşturma vb. gibi çeşitli işlemlere yönelik bildirimleri görüntüler.

4. Azure portalında bu yeni verileri sorgulamak, değiştirmek ve birlikte çalışmak için **Veri Gezgini**'ni açın. 

    :::image type="content" source="./media/create-cassandra-go/view-data-explorer-go-app.png" alt-text="Veri Gezgini Azure Cosmos DB verileri görüntüleme":::

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Cassandra API sahip bir Azure Cosmos DB hesabı oluşturmayı ve Cassandra veritabanı ve kapsayıcısı oluşturan bir Go uygulamasını çalıştırmayı öğrendiniz. Artık Azure Cosmos DB hesabınıza daha fazla veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Cassandra verilerini Azure Cosmos DB’ye aktarma](cassandra-import-data.md)
