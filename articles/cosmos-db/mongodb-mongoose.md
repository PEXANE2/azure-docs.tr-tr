---
title: Bir Düğüm.js Mongoose uygulamasını Azure Cosmos DB'ye bağlayın
description: Azure Cosmos DB'deki verileri depolamak ve yönetmek için Mongoose Framework'ü nasıl kullanacağınızı öğrenin.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 03/20/2020
author: timsander1
ms.author: tisande
ms.custom: seodec18
ms.openlocfilehash: 7f4d955583b82b224e3c963431c234ef4690198a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063745"
---
# <a name="connect-a-nodejs-mongoose-application-to-azure-cosmos-db"></a>Bir Düğüm.js Mongoose uygulamasını Azure Cosmos DB'ye bağlayın

Bu öğretici, Cosmos DB'de veri depolarken [Mongoose Framework'ün](https://mongoosejs.com/) nasıl kullanılacağını göstermektedir. Bu iziçin Azure Cosmos DB'nin MongoDB api'sini kullanıyoruz. Henüz tanımayanlar için, Mongoose MongoDB için Node.js'de bir nesne modelleme çerçevesidir ve uygulama verilerinizi modellemeniz için rahat, şema tabanlı bir çözüm sağlar.

Cosmos DB, Microsoft'un dünya çapında dağıtılan çok modelli veritabanı hizmetidir. Cosmos DB'nin çekirdeğindeki küresel dağıtım ve yatay ölçek özelliklerinden yararlanan belge, anahtar/değer ve grafik veritabanlarını hızla oluşturabilir ve sorgulayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) sürüm v0.10.29 veya üzeri.

## <a name="create-a-cosmos-account"></a>Cosmos hesabı oluşturma

Bir Cosmos hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa Node.js uygulamanızı ayarlama adımına atlayabilirsiniz. Azure Cosmos DB Emülatörü kullanıyorsanız, emülatörkurmak için [Azure Cosmos DB Emulator'daki](local-emulator.md) adımları izleyin ve Düğüm.js uygulamanızı ayarlamak için ileri ye atın.

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Node.js uygulamanızı ayarlama

>[!Note]
> Uygulamanın kendisini ayarlamak yerine yalnızca örnek kodun üzerinden geçmek istiyorsanız, bu öğreticide kullanılan [örneği](https://github.com/Azure-Samples/Mongoose_CosmosDB) kopyalayın ve Azure Cosmos DB'de Node.js Mongoose uygulamanızı oluşturun.

1. İstediğiniz klasörde bir Node.js uygulaması oluşturmak için, düğümün komut isteminde aşağıdaki komutu çalıştırın.

    ```npm init```

    Soruları yanıtladığınızda projeniz kullanıma hazır olacaktır.

1. Klasöre yeni bir dosya ekleyin ve bu dosyayı ```index.js``` olarak adlandırın.
1. ```npm install``` seçeneklerinden birini kullanarak gerekli paketleri yükleyin:
   * Mongoose: ```npm install mongoose@5 --save```

     > [!Note]
     > Aşağıdaki Mongoose örnek bağlantısı, önceki sürümlerden beri değişen Mongoose 5+'a dayanmaktadır.
    
   * Dotenv (gizli dizilerinizi bir .env dosyasında yüklemek isterseniz): ```npm install dotenv --save```

     >[!Note]
     > ```--save``` bayrağı package.json dosyasına bağımlılık ekler.

1. Bağımlılıkları index.js dosyanıza aktarın.

    ```JavaScript
   var mongoose = require('mongoose');
   var env = require('dotenv').config();   //Use the .env file to load the variables
    ```

1. Cosmos DB bağlantı dizenizi ve Cosmos DB Adını ```.env``` dosyasına ekleyin. Yer tutucular {cosmos-account-name} ve {dbname} yer tutucularını parantez sembolleri olmadan kendi Cosmos hesap adınız ve veritabanı adınız ile değiştirin.

    ```JavaScript
   # You can get the following connection details from the Azure portal. You can find the details on the Connection string pane of your Azure Cosmos account.

   COSMODDB_USER = "<Azure Cosmos account's user name>"
   COSMOSDB_PASSWORD = "<Azure Cosmos account passowrd>"
   COSMOSDB_DBNAME = "<Azure Cosmos database name>"
   COSMOSDB_HOST= "<Azure Cosmos Host name>"
   COSMOSDB_PORT=10255
    ```

1. Index.js sonuna aşağıdaki kodu ekleyerek Mongoose çerçevesini kullanarak Cosmos DB'ye bağlanın.
    ```JavaScript
   mongoose.connect("mongodb://"+process.env.COSMOSDB_HOST+":"+process.env.COSMOSDB_PORT+"/"+process.env.COSMOSDB_DBNAME+"?ssl=true&replicaSet=globaldb", {
      auth: {
        user: process.env.COSMODDB_USER,
        password: process.env.COSMOSDB_PASSWORD
      }
    })
    .then(() => console.log('Connection to CosmosDB successful'))
    .catch((err) => console.error(err));
    ```
    >[!Note]
    > Burada, 'dotenv' npm paketiyle process.env.{variableName} kullanılarak ortam değişkenleri yüklenir.

    Azure Cosmos DB'ye bağlandıktan sonra, artık Mongoose'da nesne modellerini ayarlamaya başlayabilirsiniz.

## <a name="caveats-to-using-mongoose-with-cosmos-db"></a>Cosmos DB ile Mongoose kullanma uyarılar

Oluşturduğunuz her model için, Mongoose yeni bir koleksiyon oluşturur. Ancak, Cosmos DB'nin koleksiyon başına faturalandırma modeli göz önüne alındığında, seyrek nüfuslu birden çok nesne modeli varsa, en uygun maliyetli yol olmayabilir.

Bu yönergeler her iki modeli de kapsar. Önce koleksiyon başına tek veri türünü depolama yönergelerini vereceğiz. Mongoose'un genel davranışı budur.

Mongoose'da ayrıca [Ayırıcı](https://mongoosejs.com/docs/discriminators.html) (Discriminator) adlı bir kavram vardır. Ayırıcılar bir şema devralma mekanizmasıdır. Bunlar aynı temel MongoDB koleksiyonu üzerinde örtüşen şemalar içeren birden çok modelinizin olmasına olanak tanır.

Çeşitli veri modellerini aynı koleksiyonda depolayabilir ve ardından sorgu zamanında bir filtre yan tümcesi kullanarak yalnızca gerekli verileri aşağı çekebilirsiniz.

### <a name="one-collection-per-object-model"></a>Nesne modeli başına bir koleksiyon

Varsayılan Mongoose davranışı, her Nesne modeli oluşturduğunuzda bir MongoDB koleksiyonu oluşturmaktır. Bu bölümde, Azure Cosmos DB'nin MongoDB için api'si ile bunu nasıl başarılalıştırılanınca incelenilir. Bu yöntem, büyük miktarda veri içeren nesne modelleriniz olduğunda önerilir. Bu, Mongoose için varsayılan çalışma modelidir; dolayısıyla Mongoose'u tanıyorsanız bu yöntemi de biliyor olabilirsiniz.

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
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Son olarak, nesneyi Cosmos DB'ye kaydedelim. Bu işlem, perde arkasında bir koleksiyon oluşturur.

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

1. Şimdi, Azure portalına girerken, Cosmos DB'de oluşturulan iki koleksiyonu fark esiniz.

    ![Node.js öğretici - Azure portalının ekran görüntüsü, azure cosmos DB hesabı nı gösteren, birden çok koleksiyon adı vurgulanmış - Düğüm veritabanı][multiple-coll]

1. Son olarak, Cosmos DB'den gelen verileri okuyalım. Varsayılan Mongoose çalışma modelini kullandığımızdan, okuma işlemleri Mongoose'la yapılan diğer okuma işlemleriyle aynıdır.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Mongoose ayırıcılarını kullanarak verileri tek koleksiyonda depolama

Bu yöntemde, her koleksiyonun maliyetlerini optimize etmeye yardımcı olmak için [Mongoose Ayırıcılar](https://mongoosejs.com/docs/discriminators.html) kullanıyoruz. Ayırıcılar, farklı nesne modellerinde depolama, ayırt etme ve filtreleme işlemleri yapmanıza olanak tanıyan ayırt edici bir 'Key' (Anahtar) tanımlamanızı sağlar.

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
           { givenName: "Blackie" }
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

    ![Node.js öğretici - Azure portalının ekran görüntüsü, bir Azure Cosmos DB hesabı nı gösteren, koleksiyon adı vurgulanmış - Düğüm veritabanı][alldata]

1. Ayrıca, iki farklı nesne modeli arasında ayırt etmenize yardımcı olmak üzere her nesnenin ```__type``` olarak adlandırılan başka bir özniteliği vardır.

1. Son olarak, Azure Cosmos DB'de depolanan verileri okuyalım. Mongoose model temelinde verileri filtreleme işlemini üstlenir. Dolayısıyla, verileri okurken farklı bir şey yapmanız gerekmez. Modelinizi belirtmeniz yeterlidir (bu örnekte ```Family_common```); Mongoose 'DiscriminatorKey' ile filtrelemeyi yapar.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Sizin de görebileceğiniz gibi, Mongoose ayırıcılarıyla çalışmak kolaydır. Bu nedenle, Mongoose çerçevesini kullanan bir uygulamanız varsa, bu öğretici, çok fazla değişiklik gerektirmeden Azure Cosmos'un MongoDB apisini kullanarak uygulamanızı çalışır hale getirebilmenizin bir yoludur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Azure Cosmos DB'nin MongoDB için API'si ile [Studio 3T'yi](mongodb-mongochef.md) nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB'nin API'si ile [Robo 3T'yi](mongodb-robomongo.md) nasıl kullanacağınızı öğrenin.
- Azure Cosmos DB'nin MongoDB için API'si ile MongoDB [örneklerini](mongodb-samples.md) keşfedin.

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[multiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png
