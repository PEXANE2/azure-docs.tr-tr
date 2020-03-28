---
title: Mongoose kullanarak MongoDB için Azure Cosmos DB'nin API'sine Açısal uygulamayı bağlayın
description: Bu öğretici, Cosmos DB'de depolanan verileri yönetmek için Açısal ve Express kullanarak bir Düğüm.js uygulamasının nasıl oluşturulabildiğini açıklar. Bu bölümde, Azure Cosmos DB'ye bağlanmak için Mongoose'u kullanıyorsunuz.
author: johnpapa
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: jopapa
ms.custom: seodec18
ms.reviewer: sngun
Customer intent: As a developer, I want to build a Node.js application, so that I can manage the data stored in Cosmos DB.
ms.openlocfilehash: ba893eeb8c2560397f3524d1042566dbafee7d1b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75444705"
---
# <a name="create-an-angular-app-with-azure-cosmos-dbs-api-for-mongodb---use-mongoose-to-connect-to-cosmos-db"></a>MongoDB için Azure Cosmos DB'nin API'si ile Açısal bir uygulama oluşturun - Cosmos DB'ye bağlanmak için Mongoose'u kullanın

Bu çok parçalı öğretici Express ve Angular ile bir Düğüm.js uygulaması oluşturmak ve [Cosmos hesabınıza MongoDB için Cosmos DB's API ile yapılandırılan](mongodb-introduction.md)bağlamak nasıl gösterir. Bu makalede, bölüm 5 öğretici açıklar ve [Bölüm 4](tutorial-develop-mongodb-nodejs-part4.md)üzerine inşa eder.

Öğreticinin bu bölümünde, şunları yapacaksınız:

> [!div class="checklist"]
> * Cosmos DB'ye bağlanmak için Mongoose'u kullanın.
> * Cosmos DB bağlantı dizenizi alın.
> * Kahraman modelini oluşturun.
> * Kahraman verilerini almak için Hero hizmetini oluşturun.
> * Uygulamayı yerel olarak çalıştırın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

* Bu öğreticibaşlamadan önce, Bölüm [4](tutorial-develop-mongodb-nodejs-part4.md)adımları tamamlamak.

* Bu öğretici, Azure CLI'yi yerel olarak çalıştırmanızı gerektirir. Azure CLI 2.0 veya sonraki bir sürümünü yüklemiş olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Azure CLI'yi yüklemeniz veya yükseltmeniz gerekiyorsa, [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

* Bu öğretici adım adım uygulama oluşturmak için adımlar üzerinden size yol. Tamamlanmış projeyi indirmek isterseniz, tamamlanmış uygulamayı github'daki [angular-cosmosdb deposundan](https://github.com/Azure-Samples/angular-cosmosdb) alabilirsiniz.

## <a name="use-mongoose-to-connect"></a>Bağlanmak için Mongoose'u kullanın

Mongoose, MongoDB ve Node.js için bir nesne veri modelleme (ODM) kitaplığıdır. Azure Cosmos DB hesabınıza bağlanmak için Mongoose'u kullanabilirsiniz. Mongoose'u yüklemek ve Azure Cosmos DB'ye bağlanmak için aşağıdaki adımları kullanın:

1. MongoDB ile konuşmak için kullanılan bir API olan mongoose npm modüllerini yükleyin.

    ```bash
    npm i mongoose --save
    ```

1. **Sunucu** klasöründe, **mongo.js**adlı bir dosya oluşturun. Azure Cosmos DB hesabınızın bağlantı ayrıntılarını bu dosyaya eklersiniz.

1. Aşağıdaki kodu **mongo.js** dosyasına kopyalayın. Kod aşağıdaki işlevselliği sağlar:

   * Mongoose gerektirir.
   * Es6/ES2015 ve sonraki sürümlerde yerleşik olan temel sözü kullanma sözü Mongo'yu geçersiz kılar.
   * Evreleme, üretim veya geliştirme de olup olmadığınıza göre belirli şeyleri ayarlamanızı sağlayan bir env dosyasına yapılan aramalar. Bu dosyayı bir sonraki bölümde oluşturursunuz.
   * Env dosyasında ayarlanan MongoDB bağlantı dizesini içerir.
   * Mongoose’u çağıran bir bağlanma işlevi oluşturur.

     ```javascript
     const mongoose = require('mongoose');
     /**
     * Set to Node.js native promises
     * Per https://mongoosejs.com/docs/promises.html
     */
     mongoose.Promise = global.Promise;

     const env = require('./env/environment');

     // eslint-disable-next-line max-len
     const mongoUri = `mongodb://${env.accountName}:${env.key}@${env.accountName}.documents.azure.com:${env.port}/${env.databaseName}?ssl=true`;

     function connect() {
     mongoose.set('debug', true);
     return mongoose.connect(mongoUri, { useMongoClient: true });
     }

     module.exports = {
     connect,
     mongoose
     };
     ```
    
1. Explorer bölmesinde, **sunucu**nun altında, **ortam**adlı bir klasör oluşturun. **Ortam** klasöründe, **environment.js**adlı bir dosya oluşturun.

1. Mongo.js `dbName`dosyasından, , ve `key` `cosmosPort` parametreler için değerler eklememiz gerekir. Aşağıdaki kodu **environment.js** dosyasına kopyalayın:

    ```javascript
    // TODO: replace if yours are different
    module.exports = {
      accountName: 'your-cosmosdb-account-name-goes-here',
      databaseName: 'admin', 
      key: 'your-key-goes-here',
      port: 10255
    };
    ```

## <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Uygulamanızı Azure Cosmos DB'ye bağlamak için uygulamanın yapılandırma ayarlarını güncelleştirmeniz gerekir. Ayarları güncelleştirmek için aşağıdaki adımları kullanın: 

1. Azure portalında bağlantı noktası numarasını, Azure Cosmos DB hesap adını ve Azure Cosmos DB hesabınız için birincil anahtar değerlerini alın.

1. **environment.js** dosyasında, değerini 10255 olarak değiştirin. `port` 

    ```javascript
    const port = 10255;
    ```

1. **environment.js** dosyasında, öğreticinin `accountName` [Bölüm 4'te](tutorial-develop-mongodb-nodejs-part4.md) oluşturduğunuz Azure Cosmos DB hesabının değerini değiştirin. 

1. Terminal penceresinde aşağıdaki CLI komutunu kullanarak Azure Cosmos DB hesabı için birincil anahtarı alın: 

    ```azure-cli-interactive
    az cosmosdb list-keys --name <cosmosdb-name> -g myResourceGroup
    ```    
    
    \<cosmosdb-name>, öğreticinin [Bölüm 4'te](tutorial-develop-mongodb-nodejs-part4.md) oluşturduğunuz Azure Cosmos DB hesabının adıdır.

1. Birincil anahtarı değer olarak **environment.js** `key` dosyasına kopyalayın.

Artık uygulamanız Azure Cosmos DB'ye bağlanmak için gerekli tüm bilgilere sahiptir. 

## <a name="create-a-hero-model"></a>Hero modeli oluşturma

Ardından, bir model dosyası tanımlayarak Azure Cosmos DB'de depolayacak verilerin şemasını tanımlamanız gerekir. Verilerin şemasını tanımlayan bir _Kahraman modeli_ oluşturmak için aşağıdaki adımları kullanın:

1. Explorer bölmesinde, **sunucu** klasörü **altında, hero.model.js**adlı bir dosya oluşturun.

1. Aşağıdaki kodu **hero.model.js** dosyasına kopyalayın. Kod aşağıdaki işlevselliği sağlar:

   * Mongoose gerektirir.
   * Bir kimliği, adı ve deyişi olan yeni bir şema oluşturur.
   * Şemayı kullanarak bir model oluşturur.
   * Modeli dışarı aktarır. 
   * **Adlar** toplama Heroes **(Kahramanlar**yerine , Mongoose çoğul adlandırma kurallarına dayalı koleksiyonun varsayılan adıdır).

   ```javascript
   const mongoose = require('mongoose');

   const Schema = mongoose.Schema;

   const heroSchema = new Schema(
     {
       id: { type: Number, required: true, unique: true },
       name: String,
       saying: String
     },
     {
       collection: 'Heroes'
     }
   );

   const Hero = mongoose.model('Hero', heroSchema);

   module.exports = Hero;
   ```

## <a name="create-a-hero-service"></a>Hero hizmeti oluşturma

Kahraman modelini oluşturduktan sonra, verileri okumak ve liste gerçekleştirmek, oluşturma, silme ve güncelleştirme işlemleri için bir hizmet tanımlamanız gerekir. Azure Cosmos DB'deki verileri sorgulayan bir _Kahraman hizmeti_ oluşturmak için aşağıdaki adımları kullanın:

1. Explorer bölmesinde, **sunucu** klasörü **altında, hero.service.js**adlı bir dosya oluşturun.

1. Aşağıdaki kodu **hero.service.js** dosyasına kopyalayın. Kod aşağıdaki işlevselliği sağlar:

   * Oluşturduğunuz modeli alır.
   * Veritabanına bağlanır.
   * Tüm kahramanları `docquery` döndüren `hero.find` bir sorgu tanımlamak için yöntemi kullanan bir değişken oluşturur.
   * Yanıt durumunun `docquery.exec` 200 olduğu tüm kahramanların listesini alma sözü yle işlevi olan bir sorgu çalıştırın. 
   * Durum 500 ise hata iletisini geri gönderir.
   * Kahramanları alıyor çünkü modülleri kullanıyoruz. 

   ```javascript
   const Hero = require('./hero.model');

   require('./mongo').connect();

   function getHeroes() {
     const docquery = Hero.find({});
     docquery
       .exec()
       .then(heroes => {
         res.status(200).json(heroes);
       })
       .catch(error => {
         res.status(500).send(error);
         return;
       });
   }

   module.exports = {
     getHeroes
   };
   ```

## <a name="configure-routes"></a>Rotaları yapılandırma

Ardından, url'leri işlemek için istek almak, oluşturmak, okumak ve silmek için rotalar ayarlamanız gerekir. Yönlendirme yöntemleri geri arama işlevlerini _(işleyici işlevleri_olarak da adlandırılır) belirtir. Bu işlevler, uygulama belirtilen bitiş noktası ve HTTP yöntemi için bir istek aldığında çağrılır. Kahraman hizmetini eklemek ve rotalarınızı tanımlamak için aşağıdaki adımları kullanın:

1. Visual Studio Code'da, **routes.js** dosyasında, örnek kahraman verilerini gönderen `res.send` işlevi belirtin. `heroService.getHeroes` İşlev yerine aramak için bir satır ekleyin.

    ```javascript
    router.get('/heroes', (req, res) => {
      heroService.getHeroes(req, res);
    //  res.send(200, [
    //      {"id": 10, "name": "Starlord", "saying": "oh yeah"}
    //  ])
    });
    ```

1. **routes.js** dosyasında, `require` kahraman hizmeti:

    ```javascript
    const heroService = require('./hero.service'); 
    ```

1. **hero.service.js** dosyasında, aşağıdaki `getHeroes` gibi `req` `res` parametreleri almak için işlevi güncelleştirin:

    ```javascript
    function getHeroes(req, res) {
    ```

Önceki kodu gözden geçirip gözden geçirelim. İlk olarak, düğüm sunucusunu oluşturan index.js dosyasına giriyoruz. Rotalarınızı ayarladığını ve tanımladığına dikkat edin. Sonra, routes.js dosya kahraman hizmeti görüşmeler ve **getHeroes**gibi işlevleri almak için söyler , ve istek ve yanıt geçmek. Hero.service.js dosyası modeli alır ve Mongo'ya bağlanır. Sonra biz onu aramak **getHeroes** yürütür ve 200 bir yanıt geri döner. 

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Ardından, aşağıdaki adımları kullanarak uygulamayı çalıştırın:

1. Visual Studio Code'da tüm değişikliklerinizi kaydedin. Solda, Visual Studio **Debug** Code'da ![](./media/tutorial-develop-mongodb-nodejs-part5/debug-button.png)Hata Ayıklama düğmesini seçin ve ardından Visual ![Studio Code'daki](./media/tutorial-develop-mongodb-nodejs-part5/start-debugging-button.png) **Hata Ayıklama Düğmesi** simgesini seçin.

1. Şimdi tarayıcıya geçin. Geliştirici **araçlarını** ve **Ağ sekmesini**açın. Oraya `http://localhost:3000`git ve uygulamamızı orada gör.

    ![Azure Portal’daki yeni Azure Cosmos DB hesabı](./media/tutorial-develop-mongodb-nodejs-part5/azure-cosmos-db-heroes-app.png)

Uygulamada henüz depolanmış kahraman yok. Bu öğreticinin bir sonraki bölümünde, put, itme ve silme işlevlerini ekleyeceğiz. Daha sonra Azure Cosmos veritabanımıza Mongoose bağlantılarını kullanarak ui'den kahramanlar ekleyebilir, güncelleyebilir ve silebiliriz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynaklara artık ihtiyacınız olmadığında, kaynak grubunu, Azure Cosmos DB hesabını ve ilgili tüm kaynakları silebilirsiniz. Kaynak grubunu silmek için aşağıdaki adımları kullanın:

 1. Azure Cosmos DB hesabını oluşturduğunuz kaynak grubuna gidin.
 1. **Kaynak grubunu sil**'i seçin.
 1. Silmek için kaynak grubunun adını onaylayın ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanın Gönder, Koy ve Sil işlevlerini eklemek için öğreticinin 6.

> [!div class="nextstepaction"]
> [Bölüm 6: Uygulamaiçin Gönderi Ekle, Koy ve Sil işlevleri](tutorial-develop-mongodb-nodejs-part6.md)
