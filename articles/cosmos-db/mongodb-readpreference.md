---
title: MongoDB için Azure Cosmos DB'nin API'si ile Okuma tercihini kullanma
description: MongoDB için Azure Cosmos DB'nin API'si ile MongoDB Okuma Tercihini nasıl kullanacağınızı öğrenin
author: sivethe
ms.author: sivethe
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: 10e6ed556abe8f8c438e5436fbb93c1b70b85d2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445170"
---
# <a name="how-to-globally-distribute-reads-using-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB'nin API'sini kullanarak okumaları genel olarak dağıtma

Bu makalede, Azure Cosmos DB'nin MongoDB için API'sini kullanarak [MongoDB Okuma Tercihi](https://docs.mongodb.com/manual/core/read-preference/) ayarlarıyla okuma işlemlerinin genel olarak nasıl dağıtılanılabildiğini gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar 
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

Küresel dağıtıma sahip bir Cosmos hesabı oluşturmak ve sonra ona bağlanmak için Azure portalını kullanma yla ilgili talimatlar için bu [Quickstart](tutorial-global-distribution-mongodb.md) makalesine bakın.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizinine gidin.  

Örnek depoyu kopyalamak için aşağıdaki komutları çalıştırın. İlgi alanınıza bağlı olarak, aşağıdaki örnek depolardan birini kullanın:

1. [.NET örnek uygulaması](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference)
2. [NodeJS örnek uygulaması]( https://github.com/Azure-Samples/azure-cosmos-db-mongodb-node-geo-readpreference)
3. [Mongoose örnek uygulama](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-mongoose-geo-readpreference)
4. [Java örnek uygulaması](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference)
5. [SpringBoot örnek uygulaması](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-spring)


```bash
git clone <sample repo url>
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kullanılan platforma bağlı olarak, gerekli paketleri yükleyin ve uygulamayı başlatın. Bağımlılıkları yüklemek için örnek uygulama deposunda yer alan README'yi izleyin. Örneğin, NodeJS örnek uygulamasında, gerekli paketleri yüklemek ve uygulamayı başlatmak için aşağıdaki komutları kullanın.

```bash
cd mean
npm install
node index.js
```
Uygulama bir MongoDB kaynağına bağlanmaya çalışır ve bağlantı dizesi geçersiz olduğundan başarısız olur. Bağlantı dizesini `url`güncelleştirmek için README'deki adımları izleyin. Ayrıca, Cosmos hesabınızdaki okuma bölgesini `readFromRegion` güncelleştirin. Aşağıdaki talimatlar NodeJS örneğinden dir:

```
* Next, substitute the `url`, `readFromRegion` in App.Config with your Cosmos account's values. 
```

Bu adımları izleyerek, örnek uygulama çalışır ve aşağıdaki çıktıyı üretir:

```
connected!
readDefaultfunc query completed!
readFromNearestfunc query completed!
readFromRegionfunc query completed!
readDefaultfunc query completed!
readFromNearestfunc query completed!
readFromRegionfunc query completed!
readDefaultfunc query completed!
readFromSecondaryfunc query completed!
```

## <a name="read-using-read-preference-mode"></a>Okuma Tercihi modunu kullanarak okuma

MongoDB protokolü, istemcilerin kullanması için aşağıdaki Okuma Tercihi modlarını sağlar:

1. Birincil
2. PRIMARY_PREFERRED
3. Ikincil
4. SECONDARY_PREFERRED
5. Yakın

Bu okuma tercih içim modlarının her birinin davranışı hakkında ayrıntılı bilgi için ayrıntılı [MongoDB Okuma Tercihi](https://docs.mongodb.com/manual/core/read-preference-mechanics/#replica-set-read-preference-behavior) davranışbelgelerine bakın. Cosmos DB'de, WRITE bölgesi ve ikincil haritalar için READ bölgesiiçin birincil haritalar.

Sık karşılaşılan senaryolara dayanarak aşağıdaki ayarları kullanmanızı öneririz:

1. **Düşük gecikme gecikmesi okuması** gerekiyorsa, **ENYakın** okuma tercih modunu kullanın. Bu ayar, okuma işlemlerini kullanılabilir en yakın bölgeye yönlendirir. En yakın bölge WRITE bölgesiyse, bu işlemlerin o bölgeye yönlendirilir.
2. **Okumaların yüksek kullanılabilirliği ve coğrafi dağılımı** gerekiyorsa (gecikme bir kısıtlama değildir), sonra **IKINCIL TERCIHli** okuma tercihi modunu kullanın. Bu ayar, okuma işlemlerini kullanılabilir bir READ bölgesine yönlendirir. READ bölgesi yoksa, istekler WRITE bölgesine yönlendirilir.

Örnek uygulamadan aşağıdaki parçacık, NodeJS'te EN YAKıN Okuma Tercihinin nasıl yapılandırılabildiğini gösterir:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.NEAREST});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromNearestfunc query completed!");
  });
```

Benzer şekilde, aşağıdaki parçacık, NodeJS'te okuma tercihinin SECONDARY_PREFERRED nasıl yapılandırılabildiğini gösterir:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.SECONDARY_PREFERRED});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromSecondaryPreferredfunc query completed!");
  });
```

Okuma Tercihi, bağlantı dizesi URI seçeneklerinde parametre olarak geçilerek `readPreference` de ayarlanabilir:

```javascript
const MongoClient = require('mongodb').MongoClient;
const assert = require('assert');

// Connection URL
const url = 'mongodb://localhost:27017?ssl=true&replicaSet=globaldb&readPreference=nearest';

// Database Name
const dbName = 'myproject';

// Use connect method to connect to the Server
MongoClient.connect(url, function(err, client) {
  console.log("Connected correctly to server");

  const db = client.db(dbName);

  client.close();
});
```

[.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) ve [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference)gibi diğer platformlar için ilgili örnek uygulama repos bakın.

## <a name="read-using-tags"></a>Etiketleri kullanarak okuma

Okuma Tercihi moduna ek olarak, MongoDB protokolü etiketlerin doğrudan okuma işlemleri için kullanılmasına izin verir. Cosmos DB'nin MongoDB api'sinde, `region` etiket varsayılan olarak yanıtın `isMaster` bir parçası olarak dahildir:

```json
"tags": {
         "region": "West US"
      }
```

Bu nedenle, MongoClient `region` belirli bölgelere okuma işlemleri doğrudan bölge adı ile birlikte etiketi kullanabilirsiniz. Cosmos hesapları için bölge adları, genel olarak Ayarlar >Çoğaltma verileri altında soldaki Azure **portalında**bulunabilir. Bu ayar, okuma **yalıtımı** elde etmek için yararlıdır - istemci uygulamasının okuma işlemlerini yalnızca belirli bir bölgeye yönlendirmek istediği durumlar. Bu ayar, arka planda çalışan ve üretim açısından kritik hizmetler olmayan üretim dışı/analitik türü senaryoları için idealdir.

Örnek uygulamadan elde edilen aşağıdaki parçacık, Okuma Tercihinin NodeJS'teki etiketlerle nasıl yapılandırılabildiğini gösterir:

```javascript
 var query = {};
  var readcoll = client.db('regionDB').collection('regionTest',{readPreference: new ReadPreference(ReadPreference.SECONDARY_PREFERRED, {"region": "West US"})});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromRegionfunc query completed!");
  });
```

[.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) ve [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference)gibi diğer platformlar için ilgili örnek uygulama repos bakın.

Bu makalede, Azure Cosmos DB'nin MongoDB için API'si ile Okuma Tercihi'ni kullanarak okuma işlemlerini genel olarak nasıl dağıtabileceğinizi öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, Azure portalında bu makalede oluşturulan tüm kaynakları aşağıdaki adımları kullanarak silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)
* [MongoDB için Azure Cosmos DB'nin API'si ile küresel olarak dağıtılmış bir veritabanı oluşturma](tutorial-global-distribution-mongodb.md)
* [Azure Cosmos DB emülatörü ile yerel olarak geliştirin](local-emulator.md)
