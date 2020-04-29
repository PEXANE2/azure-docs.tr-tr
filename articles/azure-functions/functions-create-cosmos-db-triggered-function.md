---
title: Azure Cosmos DB tarafından tetiklenen bir işlev oluşturma
description: Azure İşlevleri kullanarak Azure Cosmos DB’de bir veritabanına veri eklendiğinde çağrılan sunucusuz bir işlev oluşturun.
ms.assetid: bc497d71-75e7-47b1-babd-a060a664adca
ms.topic: how-to
ms.date: 10/02/2018
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 6045c61dc9837667bfaf01c685f687fcf5816e4c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80754202"
---
# <a name="create-a-function-triggered-by-azure-cosmos-db"></a>Azure Cosmos DB tarafından tetiklenen bir işlev oluşturma

Azure Cosmos DB’de veri eklendiğinde veya değiştirildiğinde tetiklenen bir işlev oluşturmayı öğrenin. Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB: Azure İşlevleri ile sunucusuz veritabanı işlemleri](../cosmos-db/serverless-computing-database.md).

![Günlüklerde iletiyi görüntüleyin.](./media/functions-create-cosmos-db-triggered-function/quickstart-completed.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

+ Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> [!INCLUDE [SQL API support only](../../includes/functions-cosmosdb-sqlapi-note.md)]

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Tetikleyiciyi oluşturabilmek için SQL API'sini kullanan bir Azure Cosmos DB hesabınızın olması gerekir.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

<a name="create-function"></a>

## <a name="create-azure-cosmos-db-trigger"></a>Azure Cosmos DB tetikleyicisi oluşturma

1. İşlev uygulamanızı genişletin ve **işlevler**' in **+** yanındaki düğmesine tıklayın. Bu, işlev uygulamanızdaki ilk işlevse **Portalda**'yı ve ardından **Devam**'ı seçin. Aksi takdirde üçüncü adıma geçin.

   ![Azure portalındaki İşlevler hızlı başlangıç sayfası](./media/functions-create-cosmos-db-triggered-function/function-app-quickstart-choose-portal.png)

1. **Diğer şablonlar**'ı ve ardından **Sonlandır ve şablonları görüntüle**'yi seçin.

    ![İşlevler hızlı başlangıcı diğer şablonlar](./media/functions-create-cosmos-db-triggered-function/add-first-function.png)

1. Arama alanına `cosmos` yazıp **Azure Cosmos DB tetikleyicisi** şablonunu seçin.

1. İstenirse, işlev uygulamasına Azure Cosmos DB uzantısını yüklemek için, **yüklensin** ' i seçin. Yükleme başarılı olduktan sonra **Devam**'ı seçin.

    ![Bağlama uzantılarını yükleme](./media/functions-create-cosmos-db-triggered-function/functions-create-cosmos-db-trigger-portal.png)

1. Yeni tetikleyiciyi resmin altındaki tabloda belirtilen ayarlarla yapılandırın.

    ![Azure Cosmos DB tarafından tetiklenen işlevi oluşturma](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-settings.png)

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Adı** | Varsayılan | Şablonun önerdiği varsayılan işlev adını kullanın.|
    | **Azure Cosmos DB hesabı bağlantısı** | Yeni ayar | **Yeni**'yi ve ardından **Aboneliğinizi**, önceden oluşturduğunuz **Veritabanı hesabını** ve **Seç**'i belirtin. Bunu yaptığınızda hesap bağlantınız için bir uygulama ayarı oluşturulur. Bu ayar bağlama tarafından veritabanı bağlantısı için kullanılır. |
    | **Kapsayıcı adı** | Öğeler | İzlenecek kapsayıcının adı. |
    | **Mevcut değilse kira kapsayıcısı oluştur** | İşaretli | Kapsayıcı zaten mevcut değil, oluşturun. |
    | **Veritabanı adı** | Görevler | İzlenecek kapsayıcının bulunduğu veritabanının adı. |

1. **Oluştur**’a tıklayarak Azure Cosmos DB tarafından tetiklenen işlevinizi oluşturun. İşlev oluşturulduktan sonra şablon temelli işlev kodu görüntülenir.  

    ![C# dilinde Cosmos DB işlev şablonu](./media/functions-create-cosmos-db-triggered-function/function-cosmosdb-template.png)

    Bu işlev şablonu, günlüklere belge sayısını ve ilk belgenin kimliğini yazar.

Sonra, Azure Cosmos DB hesabınıza bağlanıp `Items` kapsayıcıyı `Tasks` veritabanında oluşturursunuz.

## <a name="create-the-items-container"></a>Öğe kapsayıcısını oluşturma

1. Tarayıcıdaki yeni bir sekmede [Azure portalının](https://portal.azure.com) ikinci bir örneğini açın.

1. Portalın sol tarafındaki simge çubuğunu genişletin, arama alanına `cosmos` yazıp **Azure Cosmos DB**’yi seçin.

    ![Azure Cosmos DB hizmetini arama](./media/functions-create-cosmos-db-triggered-function/functions-search-cosmos-db.png)

1. Azure Cosmos DB hesabınızı seçin ve ardından **Veri Gezgini**’ni seçin. 

1. **SQL API 'si**altında **Görevler** veritabanı ' nı seçin ve **yeni kapsayıcı**' yı seçin.

    ![Bir kapsayıcı oluşturma](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-container.png)

1. **Kapsayıcı Ekle**' de, görüntünün altındaki tabloda gösterilen ayarları kullanın. 

    ![Görevler kapsayıcısını tanımlama](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-container2.png)

    | Ayar|Önerilen değer|Açıklama |
    | ---|---|--- |
    | **Veritabanı Kimliği** | Görevler |Yeni veritabanınızın adı. Bu, işlev bağlamanızda tanımlanan adla eşleşmelidir. |
    | **Kapsayıcı KIMLIĞI** | Öğeler | Yeni kapsayıcının adı. Bu, işlev bağlamanızda tanımlanan adla eşleşmelidir.  |
    | **[Bölüm anahtarı](../cosmos-db/partition-data.md)** | /kategori|Verileri her bölüme eşit şekilde dağıtan bir bölüm anahtarı. Doğru bölüm anahtarının seçilmesi, bir performanslı kapsayıcı oluşturmak için önemlidir. | 
    | **Aktarım hızı** |400 RU| Varsayılan değeri kullanın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz. |    

1. Öğeler kapsayıcısını oluşturmak için **Tamam** ' ı tıklatın. Kapsayıcının oluşturulması kısa bir zaman alabilir.

İşlev bağlamasında belirtilen kapsayıcı varsa, bu yeni kapsayıcıya öğe ekleyerek işlevi test edebilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Veri Gezgini yeni **öğeler** kapsayıcısını genişletin, **öğeler**' i seçin ve sonra **Yeni öğe**' yi seçin.

    ![Öğeler kapsayıcısında bir öğe oluşturma](./media/functions-create-cosmos-db-triggered-function/create-item-in-container.png)

1. Yeni öğenin içeriğini aşağıdaki içerikle değiştirin ve ardından **Kaydet**' i seçin.

        {
            "id": "task1",
            "category": "general",
            "description": "some task"
        }

1. Portalda işlevinizi içeren ilk tarayıcı sekmesine geçin. İşlev günlüklerini genişletin ve yeni belgenin işlevi tetiklediğini doğrulayın. `task1` belge kimliğinin günlüklere yazılıp yazılmadığına bakın. 

    ![Günlüklerde iletiyi görüntüleyin.](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-view-logs.png)

1. (İsteğe bağlı) Belgenize dönerek bir değişiklik yapın ve **Güncelleştir**’e tıklayın. Sonra işlev günlüklerine dönerek güncelleştirmenin işlevi de tetiklediğini doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB’nizde bir belge eklendiğinde ya da değiştirildiğinde çalışan bir işlev oluşturdunuz. Azure Cosmos DB tetikleyicileri hakkında daha fazla bilgi için bkz. [Azure İşlevleri için Azure Cosmos DB bağlamaları](functions-bindings-cosmosdb.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
