---
title: 'Hızlı başlangıç: Node.js ile Tablo API - Azure Cosmos DB'
description: Bu hızlı başlangıçta Azure portalı ve Node.js ile uygulama oluşturmak için Azure Cosmos DB Tablo API’sinin nasıl kullanılacağı gösterilmektedir
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 08/06/2019
ms.author: sngun
ms.openlocfilehash: f317b7b5f3ab60f466054f2043027b13e8396abc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77212818"
---
# <a name="quickstart-build-a-table-api-app-with-nodejs-and-azure-cosmos-db"></a>Hızlı Başlangıç: Node.js ve Azure Cosmos DB ile Tablo API’si uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](create-table-dotnet.md)
> * [Java](create-table-java.md)
> * [Node.js](create-table-nodejs.md)
> * [Python](create-table-python.md)
> 

Bu hızlı başlangıçta, bir Azure Cosmos DB Tablo API hesabı oluşturursunuz ve tablo lar ve varlıklar oluşturmak için GitHub'dan klonlanmış Veri Gezgini ve Bir Düğüm.js uygulamasını kullanırsınız. Azure Cosmos DB, belge, tablo, anahtar değeri ve grafik veritabanlarını küresel dağıtım ve yatay ölçek özelliklerine sahip hızlı bir şekilde oluşturmanıza ve sorgulamanıza olanak tanıyan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Veya Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz olarak deneyin.](https://azure.microsoft.com/try/cosmosdb/) Ayrıca Bir URI `https://localhost:8081` ve anahtarı `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`ile Azure [Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator) kullanabilirsiniz.
- [Düğüm.js 0.10.29+](https://nodejs.org/) .
- [Git.](https://git-scm.com/downloads)

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

> [!IMPORTANT] 
> Genel olarak kullanılabilir Tablo API’si SDK’ları ile çalışmak için yeni bir Tablo API’si hesabı oluşturmanız gerekir. Önizleme sırasında oluşturulan Tablo API’si hesapları genel olarak kullanılabilir SDK’lar tarafından desteklenmez.
>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Tablo ekleme

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-table-add-sample-data](../../includes/cosmos-db-create-table-add-sample-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Tablo uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

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
    git clone https://github.com/Azure-Samples/storage-table-node-getting-started.git
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır. 

1. [Azure portalındaki](https://portal.azure.com/)Azure Cosmos DB hesabınızda **Bağlantı Dizesini**seçin. 

    ![Bağlantı Dizesi bölmesinde gerekli bağlantı dizesi bilgilerini görüntüleme ve kopyalama](./media/create-table-nodejs/connection-string.png)

2. Sağ taraftaki kopyalama düğmesini kullanarak BIRINCIL BAĞLANTI STRING'ini kopyalayın.

3. *app.config* dosyasını açın ve değeri üçüncü satırdaki connectionString'e yapıştırın. 

    > [!IMPORTANT]
    > Uç noktanız documents.azure.com kullanıyorsa, bir önizleme hesabınız var demektir ve genel olarak kullanılabilir Tablo API’si SDK’ları ile çalışmak için [yeni bir Tablo API’si hesabı](#create-a-database-account) oluşturmanız gerekir.
    >

3. *App.config* dosyasını kaydedin.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Git terminal penceresinde `cd` komutuyla storage-table-java-getting-started klasörüne ulaşın.

    ```
    cd "C:\git-samples\storage-table-node-getting-started"
    ```

2. [azure], [node-uuid], [nconf] ve [async] modüllerini yerel olarak yüklemek ve *paket.json* dosyasına bir giriş kaydetmek için aşağıdaki komutu çalıştırın.

   ```
   npm install azure-storage node-uuid async nconf --save
   ```

2. Git terminali penceresinde, Düğüm.js uygulamasını çalıştırmak için aşağıdaki komutları çalıştırın.

    ```
    node ./tableSample.js 
    ```

    Konsol penceresi, Azure Cosmos DB içinde yeni tablo veritabanına eklenen tablo verilerini görüntüler.

    Artık Veri Gezgini'ne geri dönüp bu yeni verileri görebilir, sorgulayabilir, değiştirebilir ve çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak bir tablo oluşturmayı ve tablo verileri eklemek için bir Düğüm.js uygulamasını çalıştırmayı öğrendiniz.  Şimdi Tablo API'sini kullanarak verilerinizi sorgulayabilirsiniz.  

> [!div class="nextstepaction"]
> [Tablo verilerini Tablo API’sine içeri aktarma](table-import.md)
