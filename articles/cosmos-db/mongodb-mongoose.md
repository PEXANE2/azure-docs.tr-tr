---
title: Node.js Mongoose uygulamasını Azure Cosmos DB 'a bağlama
description: Azure Cosmos DB verileri depolamak ve yönetmek için Mongoose çerçevesini nasıl kullanacağınızı öğrenin.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: how-to
ms.date: 03/20/2020
author: timsander1
ms.author: tisande
ms.custom: seodec18
ms.openlocfilehash: 6332e74ec46454a4df70c40f4b4dfbbc6260bc39
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281438"
---
# <a name="connect-a-nodejs-mongoose-application-to-azure-cosmos-db"></a>Node.js Mongoose uygulamasını Azure Cosmos DB 'a bağlama

Bu öğreticide, Cosmos DB veri depolarken [Mongoose çerçevesinin](https://mongoosejs.com/) nasıl kullanılacağı gösterilmektedir. Bu izlenecek yol için MongoDB için Azure Cosmos DB API 'sini kullanıyoruz. Henüz tanımayanlar için, Mongoose MongoDB için Node.js'de bir nesne modelleme çerçevesidir ve uygulama verilerinizi modellemeniz için rahat, şema tabanlı bir çözüm sağlar.

Cosmos DB, Microsoft 'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Cosmos DB temel dağıtım ve yatay ölçek özelliğinden faydalanabilir ve her şey belge, anahtar/değer ve grafik veritabanlarını hızlıca oluşturup sorgulayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) sürüm v0.10.29 veya üzeri.

## <a name="create-a-cosmos-account"></a>Cosmos hesabı oluşturma

Bir Cosmos hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa Node.js uygulamanızı ayarlama adımına atlayabilirsiniz. Azure Cosmos DB öykünücüsü kullanıyorsanız, öykünücüyü ayarlamak için [Azure Cosmos DB öykünücüsü](local-emulator.md) ' nde bulunan adımları izleyin ve Node.js uygulamanızı ayarlamaya devam edin.

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

### <a name="create-a-database"></a>Veritabanı oluşturma 
Bu uygulamada Azure Cosmos DB koleksiyon oluşturmanın iki yolu ele alınacaktır: 
- **Her nesne modelini ayrı bir koleksiyonda depolama**: [adanmış aktarım hızına sahip bir veritabanı oluşturmanızı](set-throughput.md#set-throughput-on-a-database)öneririz. Bu kapasite modelinin kullanılması, size daha iyi maliyet verimliliği sağlayacaktır.

    :::image type="content" source="./media/mongodb-mongoose/db-level-throughput.png" alt-text="Node.js öğreticisi-Azure portal, Mongoose node modülüyle kullanılmak üzere bir Azure Cosmos DB hesabı için Veri Gezgini bir veritabanı oluşturmayı gösteren ekran görüntüsü":::

- **Tüm nesne modellerini tek bir Cosmos DB koleksiyonunda**depolama: tüm modelleri tek bir koleksiyonda depolamayı tercih ediyorsanız, sağlama üretilen iş seçeneğini seçmeden yalnızca yeni bir veritabanı oluşturabilirsiniz. Bu kapasite modelinin kullanılması her bir koleksiyonu her nesne modeli için kendi üretilen iş kapasitesine sahip olacak şekilde oluşturacaktır.

Veritabanını oluşturduktan sonra, `COSMOSDB_DBNAME` Aşağıdaki ortam değişkeninde adı kullanacaksınız.

## <a name="set-up-your-nodejs-application"></a>Node.js uygulamanızı ayarlama

>[!Note]
> Uygulamanın kendisini ayarlamak yerine yalnızca örnek kodun üzerinden geçmek istiyorsanız, bu öğreticide kullanılan [örneği](https://github.com/Azure-Samples/Mongoose_CosmosDB) kopyalayın ve Azure Cosmos DB'de Node.js Mongoose uygulamanızı oluşturun.

1. İstediğiniz klasörde bir Node.js uygulaması oluşturmak için, düğümün komut isteminde aşağıdaki komutu çalıştırın.

    ```npm init```

    Soruları yanıtladığınızda projeniz kullanıma hazır olacaktır.

2. Klasöre yeni bir dosya ekleyin ve bu dosyayı ```index.js``` olarak adlandırın.
3. ```npm install``` seçeneklerinden birini kullanarak gerekli paketleri yükleyin:
   * Mongoose: ```npm install mongoose@5 --save```

     > [!Note]
     > Aşağıdaki Mongoose örnek bağlantısı, önceki sürümlerden beri değiştirilen Mongoose 5 ' i temel alır.
    
   * Dotenv (gizli dizilerinizi bir .env dosyasında yüklemek isterseniz): ```npm install dotenv --save```

     >[!Note]
     > ```--save``` bayrağı package.json dosyasına bağımlılık ekler.

4. Bağımlılıkları index.js dosyanıza aktarın.

    ```JavaScript
   var mongoose = require('mongoose');
   var env = require('dotenv').config();   //Use the .env file to load the variables
    ```

5. Cosmos DB bağlantı dizenizi ve Cosmos DB Adını ```.env``` dosyasına ekleyin. {Cosmos-Account-Name} ve {dbname} yertutucuları, küme ayracı sembolleri olmadan kendi Cosmos hesap adınız ve veritabanı adınızla değiştirin.

    ```JavaScript
   # You can get the following connection details from the Azure portal. You can find the details on the Connection string pane of your Azure Cosmos account.

   COSMODDB_USER = "<Azure Cosmos account's user name, usually the database account name>"
   COSMOSDB_PASSWORD = "<Azure Cosmos account password, this is one of the keys specified in your account>"
   COSMOSDB_DBNAME = "<Azure Cosmos database name>"
   COSMOSDB_HOST= "<Azure Cosmos Host name>"
   COSMOSDB_PORT=10255
    ```

6. Aşağıdaki kodu index.js sonuna ekleyerek Mongoose çerçevesini kullanarak Cosmos DB bağlanın.
    ```JavaScript
   mongoose.connect("mongodb://"+process.env.COSMOSDB_HOST+":"+process.env.COSMOSDB_PORT+"/"+process.env.COSMOSDB_DBNAME+"?ssl=true&replicaSet=globaldb", {
      auth: {
        user: process.env.COSMODDB_USER,
        password: process.env.COSMOSDB_PASSWORD
      },
    useNewUrlParser: true,
    useUnifiedTopology: true,
    retryWrites: false
    })
    .then(() => console.log('Connection to CosmosDB successful'))
    .catch((err) => console.error(err));
    ```
    >[!Note]
    > Burada, 'dotenv' npm paketiyle process.env.{variableName} kullanılarak ortam değişkenleri yüklenir.

    Azure Cosmos DB'ye bağlandıktan sonra, artık Mongoose'da nesne modellerini ayarlamaya başlayabilirsiniz.

## <a name="best-practices-for-using-mongoose-with-cosmos-db"></a>Cosmos DB ile Mongoose kullanmaya yönelik en iyi uygulamalar

Oluşturduğunuz her model için Mongoose yeni bir koleksiyon oluşturur. Bu, daha önce açıklanan [veritabanı düzeyi aktarım hızı seçeneği](set-throughput.md#set-throughput-on-a-database)kullanılarak en iyi şekilde karşılanır. Tek bir koleksiyon kullanmak için Mongoose [ayrılmış Minators](https://mongoosejs.com/docs/discriminators.html)'ı kullanmanız gerekir. Ayırıcılar bir şema devralma mekanizmasıdır. Bunlar aynı temel MongoDB koleksiyonu üzerinde örtüşen şemalar içeren birden çok modelinizin olmasına olanak tanır.

Çeşitli veri modellerini aynı koleksiyonda depolayabilir ve ardından sorgu zamanında bir filtre yan tümcesi kullanarak yalnızca gerekli verileri aşağı çekebilirsiniz. Modellerden her birini inceleyelim.

### <a name="one-collection-per-object-model"></a>Nesne modeli başına bir koleksiyon

Bu bölümde, MongoDB için Azure Cosmos DB API 'SI ile nasıl elde edilecek anlatılmaktadır. Bu yöntem, maliyet ve kapasiteyi denetlemenize izin verdiğinden, önerilen yaklaşımımız. Sonuç olarak, veritabanındaki Istek birimlerinin miktarı nesne modellerinin sayısına bağlı değildir. Bu, Mongoose için varsayılan işletim modelidir. bu nedenle, bunu biliyor olabilirsiniz.

1. ```index.js``` dosyanızı yeniden açın.

1. 'Family' (Aile) için şema tanımını oluşturun.

    ```JavaScript
    const Family = mongoose.model('Family', new mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
            gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }));
    ```

1. 'Family' için bir nesne oluşturun.

    ```JavaScript
    const family = new Family({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Buddy" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Son olarak, nesneyi Cosmos DB kaydedelim. Bu işlem, perde arkasında bir koleksiyon oluşturur.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. Şimdi de, başka bir şema ve nesne oluşturalım. Bu kez, oluşturduğumuz nesne ailelerin ilgisini çekebilecek 'Vacation Destinations' (Tatil Yerleri) için olsun.
   1. Aynı geçen seferki gibi, şemayı oluşturalım
      ```JavaScript
      const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
       name: String,
       country: String
      }));
      ```

   1. Örnek bir nesne oluşturun (bu şemaya birden çok nesne ekleyebilirsiniz) ve kaydedin.
      ```JavaScript
      const vacaySpot = new VacationDestinations({
       name: "Honolulu",
       country: "USA"
      });

      vacaySpot.save((err, saveVacay) => {
       console.log(JSON.stringify(saveVacay));
      });
      ```

1. Şimdi de Azure portal giderek Cosmos DB iki koleksiyonun oluşturulduğunu fark edersiniz.

   :::image type="content" source="./media/mongodb-mongoose/mongo-mutliple-collections.png" alt-text="Node.js öğreticisi-birden çok koleksiyon adı vurgulanmış bir Azure Cosmos DB hesabı gösteren Azure portal ekran görüntüsü":::

1. Son olarak, Cosmos DB verileri okuyalim. Varsayılan Mongoose çalışma modelini kullandığımızdan, okuma işlemleri Mongoose'la yapılan diğer okuma işlemleriyle aynıdır.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Mongoose ayırıcılarını kullanarak verileri tek koleksiyonda depolama

Bu yöntemde, her koleksiyonun maliyetlerini iyileştirmenize yardımcı olmak için [Mongoose ayrımlarını](https://mongoosejs.com/docs/discriminators.html) kullanırız. Ayırıcılar, farklı nesne modellerinde depolama, ayırt etme ve filtreleme işlemleri yapmanıza olanak tanıyan ayırt edici bir 'Key' (Anahtar) tanımlamanızı sağlar.

Burada temel bir nesne modeli oluşturuyor, ayırt edici anahtar tanımlıyor ve temel modele 'Family' ile 'VacationDestinations' nesneleri uzantı olarak ekliyoruz.

1. Şimdi temel yapılandırmayı ayarlayalım ve ayırıcı anahtarı tanımlayalım.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Ardından da ortak nesne modelini tanımlayalım

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. Şimdi 'Family' modelini tanımlıyoruz. Burada ```mongoose.model``` yerine ```commonModel.discriminator``` kullandığımıza dikkat edin. Ayrıca, mongoose şemasına temel yapılandırmayı da ekliyoruz. Dolayısıyla, burada discriminatorKey değeri ```FamilyType``` oluyor.

    ```JavaScript
    const Family_common = commonModel.discriminator('FamilyType', new     mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
           gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }, baseConfig));
    ```

1. Benzer biçimde, şimdi bir şema daha, bu kez 'VacationDestinations' ekleyelim. Burada, DiscriminatorKey değeri ```VacationDestinationsType``` oluyor.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Son olarak da model için nesneleri oluşturalım ve kaydedelim.
   1. 'Family' modeline nesneleri ekleyelim.
      ```JavaScript
      const family_common = new Family_common({
       lastName: "Volum",
       parents: [
           { firstName: "Thomas" },
           { firstName: "Mary Kay" }
       ],
       children: [
           { firstName: "Ryan", gender: "male", grade: 8 },
           { firstName: "Patrick", gender: "male", grade: 7 }
       ],
       pets: [
           { givenName: "Buddy" }
       ],
       address: { country: "USA", state: "WA", city: "Seattle" }
      });

      family_common.save((err, saveFamily) => {
       console.log("Saved: " + JSON.stringify(saveFamily));
      });
      ```

   1. Ardından, 'VacationDestinations' modeline nesneleri ekleyelim ve kaydedelim.
      ```JavaScript
      const vacay_common = new Vacation_common({
       name: "Honolulu",
       country: "USA"
      });

      vacay_common.save((err, saveVacay) => {
       console.log("Saved: " + JSON.stringify(saveVacay));
      });
      ```

1. Artık Azure Portal'a dönerseniz, hem 'Family' hem de 'VacationDestinations' verilerini içeren ```alldata``` adlı tek bir koleksiyonunuz olduğunu görürsünüz.

   :::image type="content" source="./media/mongodb-mongoose/mongo-collections-alldata.png" alt-text="Node.js öğreticisi-koleksiyon adı vurgulanmış düğüm veritabanı ile bir Azure Cosmos DB hesabı gösteren Azure portal ekran görüntüsü":::

1. Ayrıca, iki farklı nesne modeli arasında ayırt etmenize yardımcı olmak üzere her nesnenin ```__type``` olarak adlandırılan başka bir özniteliği vardır.

1. Son olarak, Azure Cosmos DB'de depolanan verileri okuyalım. Mongoose model temelinde verileri filtreleme işlemini üstlenir. Dolayısıyla, verileri okurken farklı bir şey yapmanız gerekmez. Modelinizi belirtmeniz yeterlidir (bu örnekte ```Family_common```); Mongoose 'DiscriminatorKey' ile filtrelemeyi yapar.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Sizin de görebileceğiniz gibi, Mongoose ayırıcılarıyla çalışmak kolaydır. Bu nedenle, Mongoose çerçevesini kullanan bir uygulamanız varsa, bu öğretici, çok fazla değişiklik gerektirmeden Azure Cosmos 'nin MongoDB API 'sini kullanarak uygulamanızı çalışır duruma getirmenin bir yoludur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Studio 3T](mongodb-mongochef.md) 'ı Azure Cosmos DB MongoDB IÇIN API 'si ile nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB API 'SI ile [Robo 3T kullanmayı](mongodb-robomongo.md) öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.

[dbleveltp]: ./media/mongodb-mongoose/db-level-throughput.png
