---
title: MongoDB için Azure Cosmos DB'nin API'sini kullanarak bir Python Flask web uygulaması oluşturun
description: Azure Cosmos DB'nin MongoDB için API'sini kullanarak bağlanmak ve sorgulamak için kullanabileceğiniz bir Python Flask kodu örneği sunar.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: python
ms.topic: quickstart
ms.date: 12/26/2018
ms.openlocfilehash: 43f4cf7e4008aa01a26c48a8e99f7465eeeb234b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77061752"
---
# <a name="quickstart-build-a-python-app-using-azure-cosmos-dbs-api-for-mongodb"></a>Hızlı başlangıç: MongoDB için Azure Cosmos DB'nin API'sini kullanarak bir Python uygulaması oluşturun

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Bu hızlı başlangıçta, GitHub'dan klonlanmış bir Python Flask To-Do web uygulamasını çalıştırmak için Mongo DB API hesabı veya Azure Cosmos DB Emulator için bir Azure Cosmos DB kullanırsınız. Azure Cosmos DB, belge, tablo, anahtar değeri ve grafik veritabanlarını küresel dağıtım ve yatay ölçek özelliklerine sahip hızlı bir şekilde oluşturmanıza ve sorgulamanıza olanak tanıyan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Veya Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz olarak deneyin.](https://azure.microsoft.com/try/cosmosdb/) Veya [Azure Cosmos DB Emülatörü'ni](local-emulator.md)kullanabilirsiniz. 
- [Python 3.6+](https://www.python.org/downloads/)
- [Python Uzantılı](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python) [Görsel Stüdyo Kodu](https://code.visualstudio.com/Download) .

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Flask-MongoDB uygulamasını klonlayalım, bağlantı dizesini ayarlayalım ve çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu görüyorsunuz.

1. Bir komut istemini açın, git-samples adlı yeni bir klasör oluşturun ve komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere yeni bir klasör olarak değiştirmek için `cd` komutunu kullanın.

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git
    ```
3. Python modüllerini yüklemek için aşağıdaki komutu çalıştırın.

    ```bash 
    pip install -r .\requirements.txt
    ```
4. Visual Studio Code’da klasörü açın.

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız, aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi takdirde, [Web uygulamasını çalıştırma](#run-the-web-app) konusuna atlayabilirsiniz. 

Aşağıdaki parçacıkların tümü *app.py* dosyasından alınır ve yerel Azure Cosmos DB Emulator'un bağlantı dizesini kullanır. Başka türlü ayrıştırılamayan ileri eğik çizgiler için aşağıda görüldüğü gibi parolanın bölünmesi gerekir.

* MongoDB istemcisini başlatın, veritabanını alın ve kimlik doğrulaması yapın.

    ```python
    client = MongoClient("mongodb://127.0.0.1:10250/?ssl=true") #host uri
    db = client.test    #Select the database
    db.authenticate(name="localhost",password='C2y6yDjf5' + r'/R' + '+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw' + r'/Jw==')
    ```

* Koleksiyonu alın veya henüz yoksa bir tane oluşturun.

    ```python
    todos = db.todo #Select the collection
    ```

* Uygulama oluşturma

    ```Python
    app = Flask(__name__)
    title = "TODO with Flask"
    heading = "ToDo Reminder"
    ```
    
## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

1. Azure Cosmos DB Emulator’ın çalışır durumda olduğundan emin olun.

2. Bir terminal penceresi açın ve uygulamanın kayıtlı olduğu dizine `cd` uygulayın.

3. Ardından, PowerShell editörleri `set FLASK_APP=app.py` `$env:FLASK_APP = app.py` için veya `export FLASK_APP=app.py` Mac kullanıyorsanız, Flask uygulaması için ortam değişkenini ayarlayın. 

4. Uygulamayı çalıştırın `flask run` ve *http:\//127.0.0.1:5000/* ile göz atın.

5. Görevleri ekleyip kaldırın ve koleksiyona eklenip kaldırıldığına dikkat edin.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Kodu canlı bir Azure Cosmos DB hesabına karşı sınamak istiyorsanız, bir hesap oluşturmak için Azure portalına gidin.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Kodu canlı Azure Cosmos DB hesabına karşı test etmek için bağlantı dize bilgilerinizi alın. Ardından uygulamaya kopyalayın.

1. Azure portalındaki Azure Cosmos DB hesabınızda, sol daki gezinmede **Bağlantı Dizesini**seçin ve ardından **Oku-Yaz Tuşlarını**seçin. Kullanıcı adını, bağlantı dizesini ve parolayı kopyalamak için ekranın sağ tarafındaki kopyalama düğmelerini kullanırsınız. 

2. Kök dizinde *app.py* dosyasını açın.

3. **username** değerini portaldan kopyalayın (kopyalama düğmesini kullanarak) ve *app.py* dosyasına **name** değeri olarak yapıştırın.

4. Ardından **bağlantı dizenizi** portaldan kopyalayın ve *app.py* dosyanızdaki **MongoClient** değeri haline getirin.

5. Son olarak, **password** değerini portaldan kopyalayın ve *app.py* dosyanıza **password** değeri olarak yapıştırın.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. Öncekiyle aynı şekilde çalıştırabilirsiniz.

## <a name="deploy-to-azure"></a>Azure’a Dağıt

Bu uygulamayı dağıtmak için Azure'da yeni bir web uygulaması oluşturabilir ve bu GitHub repo'nun çatalıyla sürekli dağıtımı etkinleştirebilirsiniz. Azure'da GitHub ile sürekli dağıtım ayarlamak için bu [öğreticiyi](https://docs.microsoft.com/azure/app-service/deploy-continuous-deployment) izleyin.

Azure'a dağıtırken uygulama anahtarlarınızı kaldırmanız ve aşağıdaki bölümün açıklama satırı yapılmadığından emin olmanız gerekir:

```python
    client = MongoClient(os.getenv("MONGOURL"))
    db = client.test    #Select the database
    db.authenticate(name=os.getenv("MONGO_USERNAME"),password=os.getenv("MONGO_PASSWORD"))
```

Daha sonra MONGOURL, MONGO_PASSWORD ve MONGO_USERNAME değerlerinizi uygulama ayarlarına eklemeniz gerekir. Azure Web Apps’te Uygulama Ayarları hakkında daha fazla bilgi almak için bu [öğreticiyi](https://docs.microsoft.com/azure/app-service/configure-common#configure-app-settings) izleyebilirsiniz.

Bu repo'nun bir çatalını oluşturmak istemiyorsanız, aşağıdaki **Azure'a Dağıt** düğmesini de seçebilirsiniz. Daha sonra Azure'a gitmeli ve Azure Cosmos DB hesap bilgilerinizle uygulama ayarlarını ayarlamalısınız.

<a href="https://deploy.azure.com/?repository=https://github.com/heatherbshapiro/To-Do-List---Flask-MongoDB-Example" target="_blank">
<img src="https://azuredeploy.net/deploybutton.png" alt="Click to Deploy to Azure">
</a>

> [!NOTE]
> Kodunuzu GitHub'da veya diğer kaynak denetim seçeneklerinde depolamayı planlıyorsanız, lütfen bağlantı dizelerinizi koddan kaldırdığınızdan emin olun. Bağlantı dizeleriniz, bunun yerine web uygulamasının uygulama ayarlarıyla ayarlanabilir.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Mongo DB API hesabı için bir Azure Cosmos DB oluşturmayı ve GitHub'dan klonlanmış bir Python Flask To-Do web uygulamasını çalıştırmak için Azure Cosmos DB Emulator'u kullanmayı öğrendiniz. Şimdi Azure Cosmos DB hesabınıza ek veriler aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)
