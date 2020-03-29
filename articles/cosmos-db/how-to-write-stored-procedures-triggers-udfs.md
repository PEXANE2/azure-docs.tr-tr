---
title: Azure Cosmos DB'de depolanan yordamları, tetikleyicileri ve UDF'leri yazma
description: Azure Cosmos DB'de depolanan yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri nasıl tanımlarız öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: mjbrown
ms.openlocfilehash: 4dee017323bda5fc08598a9b24cadd11516807cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75441725"
---
# <a name="how-to-write-stored-procedures-triggers-and-user-defined-functions-in-azure-cosmos-db"></a>Azure Cosmos DB'de depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler nasıl yazılır?

Azure Cosmos DB, javascript'in dil tümleşik, işlemsel yürütmesini sağlayarak **depolanmış yordamları,** **tetikleyicileri**ve **kullanıcı tanımlı işlevleri (UDFs)** yazmanızı sağlar. Azure Cosmos DB'de SQL API'sini kullanırken, JavaScript dilinde depolanan yordamları, tetikleyicileri ve UDF'leri tanımlayabilirsiniz. Mantığınızı JavaScript'e yazabilir ve veritabanı altyapısının içinde çalıştırabilirsiniz. [Azure portalını,](https://portal.azure.com/) [Azure Cosmos DB'deki JavaScript dili tümleşik sorgu API'sini](javascript-query-api.md) ve [Cosmos DB SQL API istemci SDK'larını](sql-api-dotnet-samples.md)kullanarak tetikleyiciler, depolanmış yordamlar ve UDF'ler oluşturabilir ve yürütebilirsiniz. 

Depolanan yordamı, tetikleyiciyi ve kullanıcı tanımlı işlevi çağırmak için bunu kaydetmeniz gerekir. Daha fazla bilgi için Azure [Cosmos DB'de depolanan yordamlar, tetikleyiciler, kullanıcı tanımlı işlevlerle nasıl çalışılabilenişler](how-to-use-stored-procedures-triggers-udfs.md)ebakın.

> [!NOTE]
> Bölümlenmiş kapsayıcılar için, depolanan yordamı yürürken, istek seçeneklerinde bir bölüm anahtarı değeri sağlanmalıdır. Depolanan yordamlar her zaman bir bölüm anahtarına kapsamlıdır. Farklı bir bölüm anahtar değerine sahip öğeler depolanan yordam tarafından görünür olmayacaktır. Bu da tetikleyiciler için de uygulanır.

> [!Tip]
> Cosmos, depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevlerle kapsayıcıların dağıtılmasını destekler. Daha fazla bilgi için [bkz: Sunucu tarafı işlevselliği olan bir Azure Cosmos DB kapsayıcısı oluşturun.](manage-sql-with-resource-manager.md#create-sproc)

## <a name="how-to-write-stored-procedures"></a><a id="stored-procedures"></a>Depolanan yordamlar nasıl yazılır?

Depolanan yordamlar JavaScript kullanılarak yazılır, azure cosmos kapsayıcısı içinde öğeler oluşturabilir, güncelleyebilir, okuyabilir, sorgulayabilir ve silebilir. Depolanan yordamlar koleksiyon başına kaydedilir ve bu koleksiyonda bulunan herhangi bir belge veya ek üzerinde çalışabilir.

**Örnek**

Burada bir "Merhaba Dünya" yanıtı döndürür basit bir depolanmış yordamdır.

```javascript
var helloWorldStoredProc = {
    id: "helloWorld",
    serverScript: function () {
        var context = getContext();
        var response = context.getResponse();

        response.setBody("Hello, World");
    }
}
```

Bağlam nesnesi, Azure Cosmos DB'de gerçekleştirilebilen tüm işlemlere ve istek ve yanıt nesnelerine erişim sağlar. Bu durumda, yanıt nesnesini istemciye geri gönderilmek üzere yanıtın gövdesini ayarlamak için kullanırsınız.

Bir kez yazıldıktan sonra, saklanan yordam bir koleksiyona kaydedilmelidir. Daha fazla bilgi edinmek için Azure [Cosmos DB makalesinde depolanan yordamları nasıl kullanacağınızı](how-to-use-stored-procedures-triggers-udfs.md#stored-procedures) öğrenin.

### <a name="create-an-item-using-stored-procedure"></a><a id="create-an-item"></a>Depolanan yordamı kullanarak öğe oluşturma

Depolanan yordamı kullanarak bir öğe oluşturduğunuzda, öğe Azure Cosmos kapsayıcısına eklenir ve yeni oluşturulan öğenin kimliği döndürülür. Öğe oluşturma, eşzamanlı bir işlemdir ve JavaScript geri çağırma işlevlerine bağlıdır. Geri arama işlevinin iki parametresi vardır : biri işlemin başarısız olması durumunda hata nesnesi için, diğeri ise geri dönüş değeri için; bu durumda, oluşturulan nesne. Geri aramanın içinde, özel durumu işleyebilir veya bir hata atabilirsiniz. Geri arama sağlanamaz ve bir hata varsa, Azure Cosmos DB çalışma süresi bir hata oluşturur. 

Depolanan yordam da açıklama ayarlamak için bir parametre içerir, bir boolean değeri. Parametre doğru ayarlandığında ve açıklama eksik olduğunda, depolanan yordam bir özel durum oluşturur. Aksi takdirde, depolanan yordamın geri kalanı çalıştırmaya devam eder.

Aşağıdaki örnek, depolanan yordam, yeni bir Azure Cosmos öğesini giriş olarak alır, Azure Cosmos kapsayıcısına ekler ve yeni oluşturulan öğenin kimliğini döndürür. Bu örnekte, [Quickstart .NET SQL API'den](create-sql-api-dotnet.md) ToDoList örneğinden yararlanıyoruz

```javascript
function createToDoItem(itemToCreate) {

    var context = getContext();
    var container = context.getCollection();

    var accepted = container.createDocument(container.getSelfLink(),
        itemToCreate,
        function (err, itemCreated) {
            if (err) throw new Error('Error' + err.message);
            context.getResponse().setBody(itemCreated.id)
        });
    if (!accepted) return;
}
```

### <a name="arrays-as-input-parameters-for-stored-procedures"></a>Depolanan yordamlar için giriş parametreleri olarak diziler 

Azure portalında depolanan bir yordam tanımlanırken, giriş parametreleri her zaman depolanan yordama dize olarak gönderilir. Bir dizi dizeyi giriş olarak geçirseniz bile, dizi dize dönüştürülür ve depolanan yordamı gönderir. Bunu aşmak için, dizeyi bir dizi olarak ayrıştmak için depolanan yordamınız içinde bir işlev tanımlayabilirsiniz. Aşağıdaki kod, dize giriş parametresini dizi olarak ayrışturmanın nasıl yapılacağını gösterir:

```javascript
function sample(arr) {
    if (typeof arr === "string") arr = JSON.parse(arr);

    arr.forEach(function(a) {
        // do something here
        console.log(a);
    });
}
```

### <a name="transactions-within-stored-procedures"></a><a id="transactions"></a>Depolanan yordamlar içindeki işlemler

Depolanan bir yordamı kullanarak kapsayıcı içindeki maddelerüzerindeki hareketleri uygulayabilirsiniz. Aşağıdaki örnek, tek bir işlemde iki takım arasında oyuncu ticareti yapmak için bir fantezi futbol oyun uygulaması içindeki işlemleri kullanır. Depolanan yordam, her biri bağımsız değişken olarak geçirilen oyuncu işlikilerine tekabif eden iki Azure Cosmos ürününü okumaya çağRısında Her iki oyuncu da bulunursa, depolanan yordam takımlarını değiştirerek öğeleri güncelleştirir. Yol boyunca herhangi bir hatayla karşılaşılırsa, depolanan yordam, hareketi dolaylı olarak iptal eden bir JavaScript özel durumu oluşturur.

```javascript
// JavaScript source code
function tradePlayers(playerId1, playerId2) {
    var context = getContext();
    var container = context.getCollection();
    var response = context.getResponse();

    var player1Document, player2Document;

    // query for players
    var filterQuery = 
    {     
        'query' : 'SELECT * FROM Players p where p.id = @playerId1',
        'parameters' : [{'name':'@playerId1', 'value':playerId1}] 
    };
            
    var accept = container.queryDocuments(container.getSelfLink(), filterQuery, {},
        function (err, items, responseOptions) {
            if (err) throw new Error("Error" + err.message);

            if (items.length != 1) throw "Unable to find both names";
            player1Item = items[0];

            var filterQuery2 = 
            {     
                'query' : 'SELECT * FROM Players p where p.id = @playerId2',
                'parameters' : [{'name':'@playerId2', 'value':playerId2}] 
            };
            var accept2 = container.queryDocuments(container.getSelfLink(), filterQuery2, {},
                function (err2, items2, responseOptions2) {
                    if (err2) throw new Error("Error" + err2.message);
                    if (items2.length != 1) throw "Unable to find both names";
                    player2Item = items2[0];
                    swapTeams(player1Item, player2Item);
                    return;
                });
            if (!accept2) throw "Unable to read player details, abort ";
        });

    if (!accept) throw "Unable to read player details, abort ";

    // swap the two players’ teams
    function swapTeams(player1, player2) {
        var player2NewTeam = player1.team;
        player1.team = player2.team;
        player2.team = player2NewTeam;

        var accept = container.replaceDocument(player1._self, player1,
            function (err, itemReplaced) {
                if (err) throw "Unable to update player 1, abort ";

                var accept2 = container.replaceDocument(player2._self, player2,
                    function (err2, itemReplaced2) {
                        if (err) throw "Unable to update player 2, abort"
                    });

                if (!accept2) throw "Unable to update player 2, abort";
            });

        if (!accept) throw "Unable to update player 1, abort";
    }
}
```

### <a name="bounded-execution-within-stored-procedures"></a><a id="bounded-execution"></a>Saklanan yordamlar içinde sınırlı yürütme

Aşağıda, öğeleri bir Azure Cosmos kapsayıcısına toplu olarak içe aktarma yapan depolanmış yordam örneği verilmiştir. Depolanan yordam, boolean iade değerini `createDocument`kontrol ederek sınırlı yürütmeyi işler ve ardından toplu işler arasında ilerlemeyi izlemek ve devam ettirmek için depolanan yordamın her davetine eklenen öğelerin sayısını kullanır.

```javascript
function bulkImport(items) {
    var container = getContext().getCollection();
    var containerLink = container.getSelfLink();

    // The count of imported items, also used as current item index.
    var count = 0;

    // Validate input.
    if (!items) throw new Error("The array is undefined or null.");

    var itemsLength = items.length;
    if (itemsLength == 0) {
        getContext().getResponse().setBody(0);
    }

    // Call the create API to create an item.
    tryCreate(items[count], callback);

    // Note that there are 2 exit conditions:
    // 1) The createDocument request was not accepted.
    //    In this case the callback will not be called, we just call setBody and we are done.
    // 2) The callback was called items.length times.
    //    In this case all items were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
    function tryCreate(item, callback) {
        var isAccepted = container.createDocument(containerLink, item, callback);

        // If the request was accepted, callback will be called.
        // Otherwise report current count back to the client,
        // which will call the script again with remaining set of items.
        if (!isAccepted) getContext().getResponse().setBody(count);
    }

    // This is called when container.createDocument is done in order to process the result.
    function callback(err, item, options) {
        if (err) throw err;

        // One more item has been inserted, increment the count.
        count++;

        if (count >= itemsLength) {
            // If we created all items, we are done. Just set the response.
            getContext().getResponse().setBody(count);
        } else {
            // Create next document.
            tryCreate(items[count], callback);
        }
    }
}
```

## <a name="how-to-write-triggers"></a><a id="triggers"></a>Tetikleyiciler nasıl yazılır?

Azure Cosmos DB ön tetikleyicileri ve tetikleyicisonrası tetikleyicileri destekler. Ön tetikleyiciler bir veritabanı öğesini değiştirmeden önce yürütülür ve bir veritabanı öğesi değiştirinden sonra post-tetikleyiciler yürütülür.

### <a name="pre-triggers"></a><a id="pre-triggers"></a>Ön tetikleyiciler

Aşağıdaki örnek, oluşturulan bir Azure Cosmos öğesinin özelliklerini doğrulamak için ön tetikleyicinin nasıl kullanıldığını gösterir. Bu örnekte, [Quickstart .NET SQL API'den](create-sql-api-dotnet.md)ToDoList örneğinden yararlanarak yeni eklenen bir öğeye bir zaman damgası özelliği ekliyoruz.

```javascript
function validateToDoItemTimestamp() {
    var context = getContext();
    var request = context.getRequest();

    // item to be created in the current operation
    var itemToCreate = request.getBody();

    // validate properties
    if (!("timestamp" in itemToCreate)) {
        var ts = new Date();
        itemToCreate["timestamp"] = ts.getTime();
    }

    // update the item that will be created
    request.setBody(itemToCreate);
}
```

Ön tetikleyicilerin hiçbir giriş parametresi olamaz. Tetikleyicideki istek nesnesi, işlemle ilişkili istek iletisini işlemek için kullanılır. Önceki örnekte, bir Azure Cosmos öğesi oluşturulurken ön tetikleyici çalıştırılır ve istek iletisi gövdesi JSON biçiminde oluşturulacak öğeyi içerir.

Tetikleyiciler kaydedildiğinde, çalıştırabileceği işlemleri belirtebilirsiniz. Bu `TriggerOperation` `TriggerOperation.Create`tetikleyici, aşağıdaki kodda gösterildiği gibi bir değiştirme işleminde tetikleyicinin kullanılmasına izin verilmeyen bir değerle oluşturulmalıdır.

Ön tetikleyiciyi kaydetme ve başlatma gibi örnekler [için, ön tetikleyiciler](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) ve [tetikleyiciler](how-to-use-stored-procedures-triggers-udfs.md#post-triggers) makaleleri görün. 

### <a name="post-triggers"></a><a id="post-triggers"></a>Son tetikleyiciler

Aşağıdaki örnekte bir tetikleyici sonrası gösterilmektedir. Bu, meta veri öğesi için sorguları tetikler ve yeni oluşturulan öğe yle ilgili ayrıntılarla güncelleştirir.


```javascript
function updateMetadata() {
var context = getContext();
var container = context.getCollection();
var response = context.getResponse();

// item that was created
var createdItem = response.getBody();

// query for metadata document
var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
var accept = container.queryDocuments(container.getSelfLink(), filterQuery,
    updateMetadataCallback);
if(!accept) throw "Unable to update metadata, abort";

function updateMetadataCallback(err, items, responseOptions) {
    if(err) throw new Error("Error" + err.message);
        if(items.length != 1) throw 'Unable to find metadata document';

        var metadataItem = items[0];

        // update metadata
        metadataItem.createdItems += 1;
        metadataItem.createdNames += " " + createdItem.id;
        var accept = container.replaceDocument(metadataItem._self,
            metadataItem, function(err, itemReplaced) {
                    if(err) throw "Unable to update metadata, abort";
            });
        if(!accept) throw "Unable to update metadata, abort";
        return;
}
```

Unutulmaması gereken önemli bir nokta, Azure Cosmos DB'deki tetikleyicilerin işlemsel olarak yürütülmesidir. Tetikleyici sonrası, temel öğenin kendisi için aynı işlemin bir parçası olarak çalışır. Tetikleyici sonrası yürütme sırasında bir özel durum tüm işlem başarısız olur. Taahhüt edilen her şey geri alınır ve bir istisna döndürülür.

Ön tetikleyiciyi kaydetme ve başlatma gibi örnekler [için, ön tetikleyiciler](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) ve [tetikleyiciler](how-to-use-stored-procedures-triggers-udfs.md#post-triggers) makaleleri görün. 

## <a name="how-to-write-user-defined-functions"></a><a id="udfs"></a>Kullanıcı tanımlı işlevler nasıl yazılır?

Aşağıdaki örnek, çeşitli gelir dilimleri için gelir vergisini hesaplamak için bir UDF oluşturur. Bu kullanıcı tanımlı işlev daha sonra bir sorgu içinde kullanılır. Bu örnek amaçları için aşağıdaki gibi özellikleri ile "Gelirler" adlı bir kapsayıcı olduğunu varsayalım:

```json
{
   "name": "Satya Nadella",
   "country": "USA",
   "income": 70000
}
```

Aşağıdaki çeşitli gelir dilimleri için gelir vergisi hesaplamak için bir işlev tanımı:

```javascript
function tax(income) {

        if(income == undefined)
            throw 'no input';

        if (income < 1000)
            return income * 0.1;
        else if (income < 10000)
            return income * 0.2;
        else
            return income * 0.4;
    }
```

Kullanıcı tanımlı bir işlevin nasıl kaydedilip kullanılacağına ilişkin örnekler için Azure [Cosmos DB makalesinde kullanıcı tanımlı işlevlerin nasıl](how-to-use-stored-procedures-triggers-udfs.md#udfs) kullanılacağına bakın.

## <a name="logging"></a>Günlüğe Kaydetme 

Depolanan yordamı, tetikleyicileri veya kullanıcı tanımlı işlevleri kullanırken, komutu kullanarak `console.log()` adımları günlüğe kaydedebilirsiniz. Bu komut, aşağıdaki örnekte gösterildiği `EnableScriptLogging` gibi doğru ayarlandığında hata ayıklama için bir dize konsantre olacaktır:

```javascript
var response = await client.ExecuteStoredProcedureAsync(
document.SelfLink,
new RequestOptions { EnableScriptLogging = true } );
Console.WriteLine(response.ScriptLog);
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de depolanmış yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri nasıl yazkullanılacağını veya nasıl kullanılacağını öğrenin:

* [Azure Cosmos DB'de saklı yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri kaydetme ve kullanma](how-to-use-stored-procedures-triggers-udfs.md)

* [Azure Cosmos DB'de Javascript Query API kullanarak depolanan yordamlar ve tetikleyiciler nasıl yazılır?](how-to-write-javascript-query-api.md)

* [Azure Cosmos DB'de Azure Cosmos DB depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevlerle çalışma](stored-procedures-triggers-udfs.md)

* [Azure Cosmos DB'de JavaScript dili tümleşik sorgu API'si ile çalışma](javascript-query-api.md)
