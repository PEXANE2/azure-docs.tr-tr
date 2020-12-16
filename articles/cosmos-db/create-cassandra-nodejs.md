---
title: 'Hızlı Başlangıç: Node.js ile Cassandra API’si - Azure Cosmos DB'
description: Bu hızlı başlangıçta Node.js ile profil uygulaması oluşturmak için Azure Cosmos DB Cassandra API’sinin nasıl kullanılacağı gösterilmektedir
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 05/18/2020
ms.custom: devx-track-js
ms.openlocfilehash: b9e036df91eecadc701664a19905a92c142b7585
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97591914"
---
# <a name="quickstart-build-a-cassandra-app-with-nodejs-sdk-and-azure-cosmos-db"></a>Hızlı başlangıç: Node.js SDK ve Azure Cosmos DB ile Cassandra uygulaması derleme
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [.NET Core](create-cassandra-dotnet-core.md)
> * [Java v3](create-cassandra-java.md)
> * [Java v4](create-cassandra-java-v4.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
>  

Bu hızlı başlangıçta, bir Azure Cosmos DB Cassandra API hesabı oluşturur ve GitHub 'dan kopyalanmış olan Cassandra Node.js uygulamasını kullanarak Cassandra veritabanı ve kapsayıcısı oluşturursunuz. Azure Cosmos DB, genel dağıtım ve yatay ölçeklendirme özellikleri ile belge, tablo, anahtar değer ve grafik veritabanlarını hızlıca oluşturmanıza ve sorgulamanızı sağlayan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] Alternatif olarak, [Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/)’yi ücretsiz olarak, Azure aboneliği olmadan ve herhangi bir taahhütte bulunmadan deneyebilirsiniz.

Ayrıca, şunlar gerekir:
* [Node.js](https://nodejs.org/dist/v0.10.29/x64/node-v0.10.29-x64.msi) v 0.10.29 veya üzeri sürümü
* [Git](https://git-scm.com/)

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir belge veritabanı oluşturmadan önce Azure Cosmos DB ile bir Cassandra hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub 'dan bir Cassandra API uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu görüyorsunuz. 

1. Bir komut istemi açın. `git-samples` adlı yeni bir klasör oluşturun. Ardından, komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git bash gibi bir git terminal penceresi açın. `cd` komutunu kullanarak, örnek uygulamayı yüklemek için yeni klasöre geçiş yapın.

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Kodun veritabanı kaynaklarını nasıl oluşturduğunu öğrenmek istiyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Kod parçacıklarının tamamı `C:\git-samples\azure-cosmos-db-cassandra-nodejs-getting-started` klasöründeki `uprofile.js` dosyasından alınmıştır. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. 

* Kullanıcı adı ve parola değerleri, Azure portalındaki bağlantı dizesi sayfası kullanılarak ayarlanmıştır. `path\to\cert` bir X509 sertifikasının yolunu sağlar. 

   ```javascript
   var ssl_option = {
        cert : fs.readFileSync("path\to\cert"),
        rejectUnauthorized : true,
        secureProtocol: 'TLSv1_2_method'
        };
   const authProviderLocalCassandra = new cassandra.auth.PlainTextAuthProvider(config.username, config.password);
   ```

* `client`, contactPoint bilgileriyle başlatılır. ContactPoint, Azure portalından alınır.

    ```javascript
    const client = new cassandra.Client({contactPoints: [config.contactPoint], authProvider: authProviderLocalCassandra, sslOptions:ssl_option});
    ```

* `client`, Azure Cosmos DB Cassandra API’sine bağlanır.

    ```javascript
    client.connect(next);
    ```

* Yeni bir anahtar alanı oluşturulur.

    ```javascript
    function createKeyspace(next) {
        var query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }";
        client.execute(query, next);
        console.log("created keyspace");    
  }
    ```

* Yeni bir tablo oluşturulur.

   ```javascript
   function createTable(next) {
       var query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)";
        client.execute(query, next);
        console.log("created table");
   },
   ```

* Anahtar/değer varlıkları eklenir.

    ```javascript
        function insert(next) {
            console.log("\insert");
            const arr = ['INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (1, \'AdrianaS\', \'Seattle\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (2, \'JiriK\', \'Toronto\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (3, \'IvanH\', \'Mumbai\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (4, \'IvanH\', \'Seattle\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (5, \'IvanaV\', \'Belgaum\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (6, \'LiliyaB\', \'Seattle\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (7, \'JindrichH\', \'Buenos Aires\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (8, \'AdrianaS\', \'Seattle\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (9, \'JozefM\', \'Seattle\')'];
            arr.forEach(element => {
            client.execute(element);
            });
            next();
        },
    ```

* Tüm anahtar değerlerini almak için sorgu.

    ```javascript
        function selectAll(next) {
            console.log("\Select ALL");
            var query = 'SELECT * FROM uprofile.user';
            client.execute(query, function (err, result) {
            if (err) return next(err);
            result.rows.forEach(function(row) {
                console.log('Obtained row: %d | %s | %s ',row.user_id, row.user_name, row.user_bcity);
            }, this);
            next();
            });
        },
    ```  

* Bir anahtar-değeri almak için sorgu.

    ```javascript
        function selectById(next) {
            console.log("\Getting by id");
            var query = 'SELECT * FROM uprofile.user where user_id=1';
            client.execute(query, function (err, result) {
            if (err) return next(err);
            result.rows.forEach(function(row) {
                console.log('Obtained row: %d | %s | %s ',row.user_id, row.user_name, row.user_bcity);
            }, this);
            next();
            });
        }
    ```  

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bağlantı dizesi, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır.

1. [Azure portal](https://portal.azure.com/)Azure Cosmos DB hesabınızda **bağlantı dizesi**' ni seçin. 

1. :::image type="icon" source="./media/create-cassandra-nodejs/copy.png":::En üstteki değeri, ILETIŞIM noktasını kopyalamak için ekranın sağ tarafındaki düğmeyi kullanın.

    :::image type="content" source="./media/create-cassandra-nodejs/keys.png" alt-text="Azure portalında bağlantı dizesi sayfasından CONTACT POINT, USERNAME ve PASSWORD değerlerini görüntüleme ve kopyalama":::

1. `config.js` dosyasını açın. 

1. Portaldan CONTACT POINT değerini 4. satırda `<FillMEIN>` üzerine yapıştırın.

    Satır 4 şuna benzer şekilde görünmelidir: 

    `config.contactPoint = "cosmos-db-quickstarts.cassandra.cosmosdb.azure.com:10350"`

1. Portaldan USERNAME değerini kopyalayın ve 2. satırda `<FillMEIN>` üzerine yapıştırın.

    Satır 2 şuna benzer şekilde görünmelidir: 

    `config.username = 'cosmos-db-quickstart';`

1. Portaldan PASSWORD değerini kopyalayın ve 3. satırda `<FillMEIN>` üzerine yapıştırın.

    Satır 3 şuna benzer şekilde görünmelidir:

    `config.password = '2Ggkr662ifxz2Mg==';`

1. `config.js` dosyasını kaydedin.

## <a name="use-the-x509-certificate"></a>X509 sertifikası kullanma

1. Baltimore CyberTrust kök sertifikasını öğesinden yerel olarak indirin [https://cacert.omniroot.com/bc2025.crt](https://cacert.omniroot.com/bc2025.crt) . `.cer` dosya uzantısını kullanarak dosyayı yeniden adlandırın.

   Sertifika `02:00:00:b9` seri numarasına ve `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` SHA1 parmak izine sahiptir.

2. `uprofile.js` dosyasını açın ve `path\to\cert` değerini yeni sertifikanızı işaret edecek şekilde değiştirin.

3. `uprofile.js` dosyasını kaydedin.

> [!NOTE]
> Sonraki adımlarda sertifikayla ilgili bir hatayla karşılaşırsanız ve bir Windows makinesinde çalışıyorsanız, bir. CRT dosyasını düzgün şekilde Microsoft. cer biçimine dönüştürme sürecini izlemediğinizden emin olun.
> 
> . CRT dosyasına çift tıklayarak sertifika görüntüsüne açın. 
>
> :::image type="content" source="./media/create-cassandra-nodejs/crtcer1.gif" alt-text="Sertifika penceresini gösteren ekran görüntüsü.":::
>
> Sertifika sihirbazında Ileri ' ye basın. Base-64 kodlamalı X. 509.440 (. CER) ve ardından Ileri.
>
> :::image type="content" source="./media/create-cassandra-nodejs/crtcer2.gif" alt-text="Base-64 ile kodlanmış X. 509.440 (. CER) seçeneğini.":::
>
> Araştır ' ı seçin (bir hedef bulmak için) ve bir dosya adı yazın.
> Ileri ' yi ve ardından tamamlandı seçeneğini belirleyin.
>
> Artık düzgün şekilde biçimlendirilen bir. cer dosyanız olmalıdır. İçindeki yolun `uprofile.js` Bu dosyaya işaret ettiğini doğrulayın.

## <a name="run-the-nodejs-app"></a>Node.js uygulamasını çalıştırma

1. Git Terminal penceresinde, daha önce Klonladığınız örnek dizinde olduğunuzdan emin olun:

    ```bash
    cd azure-cosmos-db-cassandra-nodejs-getting-started
    ```

2. `npm install`Gerekli NPM modüllerini yüklemek için ' i çalıştırın.

3. Node.js uygulamanızı başlatmak için `node uprofile.js` komutunu çalıştırın.

4. Sonuçların beklendiği gibi olduğunu komut satırından kontrol edin.

    :::image type="content" source="./media/create-cassandra-nodejs/output.png" alt-text="Çıktıyı görüntüleme ve doğrulama":::

    Programın yürütülmesini durdurmak için CTRL + C tuşlarına basın ve konsol penceresini kapatın. 

5. Azure portalında bu yeni verileri sorgulamak, değiştirmek ve birlikte çalışmak için **Veri Gezgini**'ni açın. 

    :::image type="content" source="./media/create-cassandra-nodejs/data-explorer.png" alt-text="Veri Gezgini’nde verileri görüntüleme"::: 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Cassandra API sahip bir Azure Cosmos DB hesabı oluşturmayı ve Cassandra veritabanı ve kapsayıcısı oluşturan Cassandra Node.js uygulamasını çalıştırmayı öğrendiniz. Artık Azure Cosmos DB hesabınıza daha fazla veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Cassandra verilerini Azure Cosmos DB’ye aktarma](cassandra-import-data.md)