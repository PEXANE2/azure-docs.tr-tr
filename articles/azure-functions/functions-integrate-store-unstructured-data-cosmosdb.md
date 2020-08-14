---
title: Azure Cosmos DB ve Işlevleri kullanarak yapılandırılmamış verileri depolama
description: Azure İşlevleri ve Cosmos DB’yi kullanarak yapılandırılmamış verileri depolama
ms.topic: quickstart
ms.date: 04/14/2020
ms.custom: devx-track-csharp, mvc
ms.openlocfilehash: a242f1ffc0a7738ce7cdf33aeeef214fb4f63e61
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88210883"
---
# <a name="store-unstructured-data-using-azure-functions-and-azure-cosmos-db"></a>Azure İşlevleri ve Azure Cosmos DB’yi kullanarak yapılandırılmamış verileri depolama

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) YAPıLANDıRıLMAMıŞ ve JSON verilerini depolamanın harika bir yoludur. Cosmos DB, Azure İşlevleri ile birlikte kullanıldığında verilerin ilişkisel bir veritabanında depolanmasına göre çok daha az kodla verileri hızlı ve kolay bir şekilde depolar.

> [!NOTE]
> Şu anda Azure Cosmos DB tetikleyicisi, giriş bağlamaları ve çıkış bağlamaları yalnızca SQL API ve Graph API hesaplarıyla çalışır.

Azure İşlevleri’nde giriş ve çıkış bağlamaları, işlevinizden dış hizmet verilerine bağlanmanın bildirim temelli bir yöntemini sağlar. Bu makalede, yapılandırılmamış verileri bir Azure Cosmos DB belgesinde depolayan bir çıktı bağlaması eklemek için var olan bir işlevi güncelleştirme hakkında bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Çıkış bağlamasını oluşturabilmek için SQL API'sini kullanan bir Azure Cosmos DB hesabınızın olması gerekir.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

1. Azure portal ' a gidin ve daha önce oluşturduğunuz işlev uygulamasını seçin.

1. **İşlevler**' i seçin ve ardından httptrigger işlevini seçin.

    :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-select-http-function.png" alt-text="Azure portal http işlevinizi seçin." border="true":::

1. **Tümleştirme** ' i ve **Çıkış Ekle**' yi seçin.

     :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-add-output-binding.png" alt-text="Azure Cosmos DB çıkış bağlaması ekleyin." border="true":::

1. Tabloda belirtilen **çıkış oluştur** ayarlarını kullanın:

     :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-configure-cosmosdb-binding.png" alt-text="Azure Cosmos DB çıkış bağlamayı yapılandırın." border="true":::

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Bağlama türü** | Azure Cosmos DB | Azure Cosmos DB çıkış bağlamasını oluşturmak için seçilecek bağlama türünün adı. |
    | **Belge parametre adı** | taskDocument | Kodda Cosmos DB nesnesine başvuran ad. |
    | **Veritabanı adı** | taskDatabase | Belgelerin kaydedileceği veritabanının adı. |
    | **Koleksiyon adı** | taskCollection | Veritabanı koleksiyonunun adı. |
    | **True ise, Cosmos DB veritabanı ve koleksiyonunu oluşturur** | Evet | Koleksiyon henüz mevcut değil, bu yüzden oluşturun. |
    | **Cosmos DB hesabı bağlantısı** | Yeni ayar | **Yeni**' yi ve ardından daha önce oluşturduğunuz **Azure Cosmos DB hesabı** ve **veritabanı hesabı** ' nı seçin ve ardından **Tamam**' ı seçin. Hesap bağlantınız için bir uygulama ayarı oluşturulur. Bu ayar bağlama tarafından veritabanı bağlantısı için kullanılır. |

1. Bağlamayı oluşturmak için **Tamam ' ı** seçin.

## <a name="update-the-function-code"></a>İşlev kodunu güncelleştirme

Mevcut işlev kodunu aşağıdan seçtiğiniz dildeki kodla değiştirin:

# <a name="c"></a>[C#](#tab/csharp)

Mevcut C# işlevini aşağıdaki kodla değiştirin:

```csharp
#r "Newtonsoft.Json"

using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;

public static IActionResult Run(HttpRequest req, out object taskDocument, ILogger log)
{
    string name = req.Query["name"];
    string task = req.Query["task"];
    string duedate = req.Query["duedate"];

    // We need both name and task parameters.
    if (!string.IsNullOrEmpty(name) && !string.IsNullOrEmpty(task))
    {
        taskDocument = new
        {
            name,
            duedate,
            task
        };

        return (ActionResult)new OkResult();
    }
    else
    {
        taskDocument = null;
        return (ActionResult)new BadRequestResult();
    }
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Mevcut JavaScript işlevini aşağıdaki kodla değiştirin:

```js
module.exports = async function (context, req) {

    // We need both name and task parameters.
    if (req.query.name && req.query.task) {

        // Set the output binding data from the query object.
        context.bindings.taskDocument = req.query;

        // Success.
        context.res = {
            status: 200
        };
    }
    else {
        context.res = {
            status: 400,
            body: "The query options 'name' and 'task' are required."
        };
    }
};
```
---

Bu kod örneği, HTTP İsteği sorgu dizelerini okur ve `taskDocument` nesnesindeki alanlara atar. `taskDocument` bağlaması bu bağlama parametresindeki nesne verilerini, bağlanan belge veritabanında depolanmak üzere gönderir. Veritabanı, işlev ilk kez çalıştırıldığında oluşturulur.

## <a name="test-the-function-and-database"></a>İşlevi ve veritabanını test etme

1. **Test**'i seçin. **Sorgu**altında **+ parametre Ekle** ' yi seçin ve sorgu dizesine aşağıdaki parametreleri ekleyin:

    + `name`
    + `task`
    + `duedate`

    :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png" alt-text="İşlevi test edin." border="true":::


1. **Çalıştır** ' ı seçin ve 200 durumunun döndürüldüğünü doğrulayın.

    :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function-output.png" alt-text="İşlevi test edin." border="true":::


1. Azure portal, araması yapın ve **Azure Cosmos DB**seçin.

    :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-search-cosmos-db.png" alt-text="Cosmos DB hizmeti için arama yapın." border="true":::

1. Azure Cosmos DB hesabınızı seçin ve  **Veri Gezgini**' ı seçin.

1. **TaskCollection** düğümlerini genişletin, yeni belge ' yi seçin ve belgenin sorgu dizesi değerlerinizi içerdiğinden, bazı ek meta verilerle birlikte emin olun.

    :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-data-explorer-check-document.png" alt-text="Belgenizdeki dize değerlerini doğrulayın." border="true":::

Yapılandırılmamış verileri bir Azure Cosmos DB’de depolayan HTTP tetikleyicinize başarıyla bir bağlama eklediniz.

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB veritabanına bağlama hakkında daha fazla bilgi için bkz. [Azure İşlevleri Cosmos DB bağlamaları](functions-bindings-cosmosdb.md).

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps-2.md)]
