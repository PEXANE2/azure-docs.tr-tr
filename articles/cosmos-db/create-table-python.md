---
title: "Hızlı Başlangıç: Tablo API'si ile Python - Azure Cosmos DB"
description: Bu hızlı başlangıçta Azure portalı ve Python ile uygulama oluşturmak için Azure Cosmos DB Tablo API’sinin nasıl kullanılacağı gösterilmektedir
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: python
ms.topic: quickstart
ms.date: 04/10/2018
ms.author: sngun
ms.custom: seo-python-october2019
ms.openlocfilehash: 9de9739efce13fc96bf550759eb0ef68d732af1e
ms.sourcegitcommit: 0eb0673e7dd9ca21525001a1cab6ad1c54f2e929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77212631"
---
# <a name="quickstart-build-a-table-api-app-with-python-and-azure-cosmos-db"></a>Hızlı Başlangıç: Python ve Azure Cosmos DB ile Tablo API’si uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](create-table-dotnet.md)
> * [Java](create-table-java.md)
> * [Node.js](create-table-nodejs.md)
> * [Python](create-table-python.md)
> 

Bu hızlı başlangıçta, Azure portal ve Visual Studio 'dan GitHub 'dan kopyalanmış bir Python uygulamasıyla Azure Cosmos DB Tablo API'si hesabı oluşturup yönetirsiniz. Azure Cosmos DB, genel dağıtım ve yatay ölçeklendirme özellikleri ile belge, tablo, anahtar değer ve grafik veritabanlarını hızlıca oluşturmanıza ve sorgulamanızı sağlayan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Veya Azure aboneliği olmadan [ücretsiz Azure Cosmos DB deneyin](https://azure.microsoft.com/try/cosmosdb/) . [Azure Cosmos DB öykünücüsü](https://aka.ms/cosmosdb-emulator) ' nü BIR `https://localhost:8081` URI 'siyle ve anahtar `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`de kullanabilirsiniz.
- [Visual Studio 2019](https://www.visualstudio.com/downloads/), **Azure geliştirme** ve **Python geliştirme** iş yükleri kurulum sırasında seçilidir. 
- [Git](https://git-scm.com/downloads).

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

Şimdi GitHub'dan bir Tablo uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

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

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır. 

1. [Azure portal](https://portal.azure.com/)Azure Cosmos DB hesabınızda **bağlantı dizesi**' ni seçin. 

    ![Bağlantı dizesi ayarlarındaki bağlantı DIZESINI görüntüleyin ve kopyalayın](./media/create-table-python/view-and-copy-connection-string-in-connection-string-settings.png)

2. Sağ taraftaki düğmeyi kullanarak ACCOUNT NAME’i kopyalayın.

3. *Config.py* dosyasını açın ve portaldan hesap adını, 19. satırdaki STORAGE_ACCOUNT_NAME değerine yapıştırın.

4. Portala geri dönüp PRIMARY KEY’i kopyalayın.

5. Portaldan PRIMARY KEY değerini 20. satırdaki STORAGE_ACCOUNT_KEY değerine yapıştırın.

6. *Config.py* dosyasını kaydedin.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Visual Studio 'da **Çözüm Gezgini**projeye sağ tıklayın.

2. Geçerli Python ortamını seçin ve sağ tıklayın.

2. **Python paketini yükler**' i seçin ve ardından *Azure-Storage-Table*girin.

3. Uygulamayı çalıştırmak için F5'e basın. Uygulamanız tarayıcınızda görüntülenir. 

Şimdi veri Gezgini'ne dönün ve bakın, sorgu, değiştirebilir ve bu yeni verilerle çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini kullanarak tablo oluşturmayı ve tablo verileri eklemek için Visual Studio 'da bir Python uygulaması çalıştırmayı öğrendiniz.  Şimdi Tablo API'sini kullanarak verilerinizi sorgulayabilirsiniz.  

> [!div class="nextstepaction"]
> [Tablo verilerini Tablo API’sine aktarma](table-import.md)
