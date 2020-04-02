---
title: Quickstart - Azure portalından Azure Cosmos DB kaynakları oluşturun
description: Bu hızlı başlangıç, Azure portalını kullanarak bir Azure Cosmos veritabanı, kapsayıcı ve öğeleri nasıl oluşturulacak larını gösterir.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 01/05/2020
ms.openlocfilehash: 79deb2f33a11e8ccb6f059bde7590b7cc0fe20c0
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521120"
---
# <a name="quickstart-create-an-azure-cosmos-account-database-container-and-items-from-the-azure-portal"></a>Hızlı başlatma: Azure portalından bir Azure Cosmos hesabı, veritabanı, kapsayıcı ve öğeler oluşturun

> [!div class="op_single_selector"]
> * [Azure portalı](create-cosmosdb-resources-portal.md)
> * [.NET](create-sql-api-dotnet.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)
>  

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB'nin çekirdeğindeki küresel dağıtım ve yatay ölçek özelliklerinden yararlanan anahtar/değer veritabanlarını, belge veritabanlarını ve grafik veritabanlarını hızla oluşturmak ve sorgulamak için Azure Cosmos DB'yi kullanabilirsiniz. 

Bu hızlı başlangıç, Azure Cosmos DB [SQL API](sql-api-introduction.md) hesabı oluşturmak, bir belge veritabanı ve kapsayıcı oluşturmak ve kapsayıcıya veri eklemek için Azure portalının nasıl kullanılacağını gösterir. 

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliği veya ücretsiz Azure Cosmos DB deneme hesabı
- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

<a id="create-account"></a>
## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Azure Cosmos DB hesabı oluşturmak için [Azure portalına](https://portal.azure.com/) gidin. **Azure Cosmos DB**’yi arayın ve seçin.

   ![Azure portalındaki Veritabanları bölmesi](./media/create-cosmosdb-resources-portal/find-nosql-cosmosdb-marketplace.png)

1. **Ekle'yi**seçin.
1. **Azure Cosmos DB Hesabı Oluştur** sayfasında, yeni Azure Cosmos hesabına yönelik temel ayarları girin. 

    |Ayar|Değer|Açıklama |
    |---|---|---|
    |Abonelik|Abonelik adı|Bu Azure Cosmos hesabı için kullanmak istediğiniz Azure aboneliğini seçin. |
    |Kaynak Grubu|Kaynak grubu adı|Bir kaynak grubu seçin veya **Yeni oluştur** seçeneğini belirleyin ve yeni kaynak grubu için benzersiz bir ad girin. |
    |Hesap Adı|Benzersiz bir ad|Azure Cosmos hesabınızı tanımlayan bir ad girin. URI’nizi oluşturmak için sağladığınız ada *documents.azure.com* ekleneceği için benzersiz bir ad kullanın.<br><br>Ad yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. Ad, 3-31 karakter arası uzunlukta olmalıdır.|
    |API|Oluşturulacak hesap türü|SQL söz dizimini kullanarak belge veritabanı ve sorgusu oluşturmak için **Cekirdek (SQL)** seçeneğini belirleyin. <br><br>API, oluşturulacak hesap türünü belirler. Azure Cosmos DB, belge verileri için çekirdek (SQL) ve MongoDB, grafik verileri için Gremlin, Azure Tablosu ve Cassandra olmak üzere beş API sağlar. Şu anda, her API için farklı bir hesap oluşturmanız gerekir. <br><br>[SQL API’si hakkında daha fazla bilgi edinin](introduction.md).|
    |Ücretsiz Katman İndirimi Uygulayın|Uygula veya Uygula|Azure Cosmos DB ücretsiz katmanı ile ilk 400 RU/s ve 5 GB depolama alanını bir hesapta ücretsiz olarak alırsınız. [Ücretsiz katman](https://azure.microsoft.com/pricing/details/cosmos-db/)hakkında daha fazla bilgi edinin.|
    |Konum|Kullanıcılarınıza en yakın bölge|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.|
    |Hesap Türü|Üretim veya Üretim Dışı|Hesap üretim iş yükü için kullanılacaksa **Üretim'i** seçin. Hesabın üretim dışı olarak kullanılması, örneğin geliştirme, test, QA veya evreleme için kullanılacaksa **Üretim Dışı'yı** seçin. Bu, Portal deneyimini etkileyen ancak temel Azure Cosmos DB hesabını etkilemeyen bir Azure kaynak etiketi ayarıdır. Bu değeri istediğiniz zaman değiştirebilirsiniz.|


> [!NOTE]
> Azure aboneliği başına en fazla bir ücretsiz katman Azure Cosmos DB hesabınız olabilir ve hesabı oluştururken kabul etmeniz gerekir. Ücretsiz katman indirimini uygulama seçeneğini görmüyorsanız, bu abonelikteki başka bir hesabın zaten ücretsiz katmanla etkinleştirilmiş olduğu anlamına gelir.
   
   ![Azure Cosmos DB için yeni hesap sayfası](./media/create-cosmosdb-resources-portal/azure-cosmos-db-create-new-account-detail.png)

1. **İncele ve oluştur**’u seçin. **Ağ** ve **Etiketler** bölümlerini atlayabilirsiniz.

1. Hesap ayarlarını gözden geçirip **Oluştur** seçeneğini belirleyin. Hesabın oluşturulması birkaç dakika sürer. Portal sayfasında **Dağıtımınız tamamlandı** iletisinin görüntülenmesini bekleyin. 

    ![Azure portalındaki Bildirimler bölmesi](./media/create-cosmosdb-resources-portal/azure-cosmos-db-account-deployment-successful.png)

1. Azure Cosmos DB hesabı sayfasına gitmek için **Kaynağa git** seçeneğini belirleyin. 

    ![Azure Cosmos DB hesabı sayfası](./media/create-cosmosdb-resources-portal/azure-cosmos-db-account-quickstart-pane.png)

<a id="create-container-database"></a>
## <a name="add-a-database-and-a-container"></a>Veritabanı ve kapsayıcı ekleme 

Bir veritabanı ve kapsayıcı oluşturmak için Azure portalındaki Veri Gezgini'ni kullanabilirsiniz. 

1.  Azure Cosmos DB hesap sayfanızdaki sol gezintiden **Veri Gezgini'ni** seçin ve ardından Yeni **Kapsayıcı'yı**seçin. 
    
    **Kapsayıcı Ekle** penceresini görmek için sağa kaydırmanız gerekebilir.
    
    ![Azure portalındaki Veri Gezgini, Kapsayıcı Ekle bölmesi](./media/create-sql-api-dotnet/azure-cosmosdb-data-explorer-dotnet.png)
    
1.  Ekle **kapsayıcı** bölmesine, yeni kapsayıcının ayarlarını girin.
    
    |Ayar|Önerilen değer|Açıklama
    |---|---|---|
    |**Veritabanı Kimliği**|ToDoList|*ToDoList'i* yeni veritabanının adı olarak girin. Veritabanı adları 1 ile 255 arasında karakter içermelidir ve bunlar veya bir iz alanı içeremez. `/, \\, #, ?` Tedarik **veritabanı iş verme** seçeneğini denetleyin, veritabanına sağlanan iş veri tabanına sağlanan iş veri tabanı içindeki tüm kapsayıcılar arasında paylaşmanızı sağlar. Bu seçenek aynı zamanda maliyet tasarrufu ile yardımcı olur. |
    |**Aktarım hızı**|400|Çıktıyı saniyede 400 istek biriminde (RU/s) bırakın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.| 
    |**Konteyner Kimliği**|Öğeler|Yeni kapsayıcınızın adı olarak *Öğeleri* girin. Kapsayıcı kimliklerinin karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır.|
    |**Bölüm anahtarı**| /kategori| Bu makalede açıklanan örnek, */kategoriyi* bölüm anahtarı olarak kullanır.|

    
    Bu örnek için **Benzersiz tuşları** eklemeyin. Benzersiz anahtarlar, bölüm anahtarı başına bir veya daha fazla değerin benzersizliğini sağlayarak veritabanına bir veri bütünlüğü katmanı eklemenize izin verir. Daha fazla bilgi için [Azure Cosmos DB'deki Benzersiz anahtarlara](unique-keys.md)bakın.
    
1.  **Tamam'ı**seçin. Veri Gezgini yeni veritabanını ve oluşturduğunuz kapsayıcıyı görüntüler.

## <a name="add-data-to-your-database"></a>Veritabanınıza veri ekleme

Data Explorer'ı kullanarak yeni veritabanınıza veri ekleyin.

1. **Veri Gezgini'nde,** **ToDoList** veritabanını genişletin ve **Öğeler** kapsayıcısını genişletin. Ardından, **Öğeler'i**seçin ve ardından **Yeni Öğe'yi**seçin. 
   
   ![Azure portalındaki Veri Gezgini'nde yeni belge oluşturma](./media/create-sql-api-dotnet/azure-cosmosdb-new-document.png)
   
1. **Belgeler** bölmesinin sağ tarafındaki belgeye aşağıdaki yapıyı ekleyin:

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

1. **Kaydet'i**seçin.
   
   ![Json verilerini kopyalayın ve Azure portalında Veri Gezgininde Kaydet'i seçin](./media/create-sql-api-dotnet/azure-cosmosdb-save-document.png)
   
1. **Yeni Belge'yi** yeniden seçin ve benzersiz `id`ve diğer özellikleri ve değerleri içeren başka bir belge oluşturun ve kaydedin. Azure Cosmos DB verilerinize herhangi bir şema uygulamadığından, belgelerinizin herhangi bir yapısı olabilir.

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)] 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

Gelecekte sadece veritabanını silmek ve Azure Cosmos hesabını kullanmak isterseniz, veritabanını aşağıdaki adımlarla silebilirsiniz:

* Azure Cosmos hesabınıza geçtiniz.
* **Veri Gezgini'ni**Aç , silmek istediğiniz veritabanına sağ tıklayın ve **Veritabanını Sil'i**seçin.
* Silme işlemini onaylamak için Veritabanı Kimliği/veritabanı adını girin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Veri Gezgini'ni kullanarak bir Azure Cosmos DB hesabı oluşturmayı, veritabanı oluşturmayı ve kapsayıcıyı nasıl oluşturabileceğinizi öğrendiniz. Şimdi Azure Cosmos DB hesabınıza ek veriler aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)
