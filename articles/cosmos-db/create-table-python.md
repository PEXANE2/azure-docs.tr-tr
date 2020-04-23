---
title: 'Hızlı başlangıç: Python ile Tablo API - Azure Cosmos DB'
description: Bu hızlı başlangıçta Azure portalı ve Python ile uygulama oluşturmak için Azure Cosmos DB Tablo API’sinin nasıl kullanılacağı gösterilmektedir
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: python
ms.topic: quickstart
ms.date: 04/10/2018
ms.author: sngun
ms.custom: seo-python-october2019
ms.openlocfilehash: 673155706c3b7d44b41562c2dc8ce24a94ac6a58
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82083640"
---
# <a name="quickstart-build-a-table-api-app-with-python-and-azure-cosmos-db"></a>Hızlı Başlangıç: Python ve Azure Cosmos DB ile Tablo API’si uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](create-table-dotnet.md)
> * [Java](create-table-java.md)
> * [Node.js](create-table-nodejs.md)
> * [Python](create-table-python.md)
> 

Bu hızlı başlangıçta, Azure portalından ve Visual Studio'dan GitHub'dan klonlanmış bir Python uygulamasıyla bir Azure Cosmos DB Table API hesabı oluşturur ve yönetirsiniz. Azure Cosmos DB, belge, tablo, anahtar değeri ve grafik veritabanlarını küresel dağıtım ve yatay ölçek özelliklerine sahip hızlı bir şekilde oluşturmanıza ve sorgulamanıza olanak tanıyan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Veya Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz olarak deneyin.](https://azure.microsoft.com/try/cosmosdb/) Ayrıca Bir URI `https://localhost:8081` ve anahtarı `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`ile Azure [Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator) kullanabilirsiniz.
- [Visual Studio 2019,](https://www.visualstudio.com/downloads/)kurulum sırasında seçilen **Azure geliştirme** ve **Python geliştirme** iş yükleriyle birlikte. 
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
    git clone https://github.com/Azure-Samples/storage-python-getting-started.git
    ```

3. Ardından çözüm dosyasını Visual Studio'da açın. 

> ! [İpucu] Benzer kodun daha ayrıntılı bir gözden geçirme için [Cosmos DB Tablo API örnek](table-storage-how-to-use-python.md) makalesine bakın.

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır. 

1. [Azure portalındaki](https://portal.azure.com/)Azure Cosmos DB hesabınızda **Bağlantı Dizesini**seçin. 

    ![Bağlantı String ayarlarında BAĞLANTI STRING'i görüntüleme ve kopyalama](./media/create-table-python/view-and-copy-connection-string-in-connection-string-settings.png)

2. Sağ taraftaki düğmeyi kullanarak ACCOUNT NAME’i kopyalayın.

3. *config.py* dosyasını açın ve portaldan HESAP Adını satır 19'daki STORAGE_ACCOUNT_NAME değerine yapıştırın.

4. Portala geri dönüp PRIMARY KEY’i kopyalayın.

5. Portaldan PRIMARY KEY değerini 20. satırdaki STORAGE_ACCOUNT_KEY değerine yapıştırın.

6. *config.py* dosyasını kaydedin.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Visual Studio'da **Solution Explorer'daki**projeye sağ tıklayın.

2. Geçerli Python ortamını seçin ve sağ tıklatın.

2. **Python Paketini Yükle'yi**seçin, ardından *azure-depolama tablosunu*girin.

3. Uygulamayı çalıştırmak için F5'e basın. Uygulamanız tarayıcınızda görüntülenir. 

Artık Veri Gezgini'ne geri dönüp bu yeni verileri görebilir, sorgulayabilir, değiştirebilir ve çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak bir tablo oluşturmayı ve tablo verilerini eklemek için Visual Studio'da bir Python uygulamasını çalıştırmayı öğrendiniz.  Şimdi Tablo API'sini kullanarak verilerinizi sorgulayabilirsiniz.  

> [!div class="nextstepaction"]
> [Tablo verilerini Tablo API’sine içeri aktarma](table-import.md)
