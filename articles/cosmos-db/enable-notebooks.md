---
title: Azure Cosmos DB hesabındaki not defterlerini etkinleştir (Önizleme)
description: Azure Cosmos DB yerleşik Not defterleri, portaldaki verilerinizi analiz etmenize ve görselleştirmenize olanak tanır. Bu makalede, Cosmos hesapları için bu özelliğin nasıl etkinleştirileceği açıklanır.
author: deborahc
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/22/2019
ms.author: dech
ms.openlocfilehash: 4c485bf6b9eb34e68e399c24e51286428f47586f
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85261911"
---
# <a name="enable-notebooks-for-azure-cosmos-db-accounts-preview"></a>Azure Cosmos DB hesapları için not defterlerini etkinleştirme (Önizleme)

> [!IMPORTANT]
> Azure Cosmos DB için yerleşik Not defterleri Şu anda şu Azure bölgelerinde sunulmaktadır: Avustralya Doğu, Doğu ABD, Doğu ABD 2, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, UK Güney, Batı Avrupa ve Batı ABD 2. Not defterlerini kullanmak için, bu bölgelerden birindeki mevcut bir hesapta not defterleri [ile yeni bir hesap oluşturun](#enable-notebooks-in-a-new-cosmos-account) veya Not [defterlerini etkinleştirin](#enable-notebooks-in-an-existing-cosmos-account) .

Azure Cosmos DB 'daki yerleşik jupi Not defterleri, Azure portal verilerinizi analiz etmenize ve görselleştirmenize olanak tanır. Bu makale, Azure Cosmos DB hesabınız için bu özelliği etkinleştirmeyi açıklar.

## <a name="enable-notebooks-in-a-new-cosmos-account"></a>Yeni bir Cosmos hesabındaki not defterlerini etkinleştir

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. Azure Cosmos DB **kaynak veritabanları oluştur**' u seçin  >  **Databases**  >  **Azure Cosmos DB**.
1. **Azure Cosmos DB hesabı oluştur** sayfasında, **Not defterleri**' ni seçin. 
 
    :::image type="content" source="media/enable-notebooks/create-new-account-with-notebooks.png" alt-text="Azure Cosmos DB dikey pencere Oluştur bölümünde Not defterleri seçeneğini belirleyin":::

1. **İncele ve oluştur**’u seçin. **Ağ** ve **Etiketler** seçeneğini atlayabilirsiniz. 
1. Hesap ayarlarını gözden geçirip **Oluştur** seçeneğini belirleyin. Hesabın oluşturulması birkaç dakika sürer. Portal sayfasında **Dağıtımınız tamamlandı** iletisinin görüntülenmesini bekleyin. 

   :::image type="content" source="media/enable-notebooks/create-new-account-with-notebooks-complete.png" alt-text="Azure portalındaki Bildirimler bölmesi":::

1. Azure Cosmos DB hesabı sayfasına gitmek için **Kaynağa git** seçeneğini belirleyin.

   :::image type="content" source="../../includes/media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-3.png" alt-text="Azure Cosmos DB hesabı sayfası":::

1. **Veri Gezgini** bölmesine gidin. Artık Not defteri çalışma alanınızı görmeniz gerekir.

    :::image type="content" source="media/enable-notebooks/new-notebooks-workspace.png" alt-text="Yeni Azure Cosmos DB Not defteri çalışma alanı":::

## <a name="enable-notebooks-in-an-existing-cosmos-account"></a>Mevcut Cosmos hesabındaki not defterlerini etkinleştir

Ayrıca, mevcut hesaplarda not defterlerini etkinleştirebilirsiniz. Bu adımın hesap başına yalnızca bir kez yapılması gerekir.

1. Cosmos hesabınızda **Veri Gezgini** bölmesine gidin.
1. **Not defterlerini etkinleştir**' i seçin.

    :::image type="content" source="media/enable-notebooks/enable-notebooks-workspace.png" alt-text="Veri Gezgini yeni bir not defteri çalışma alanı oluştur":::

1. Bu, yeni bir not defteri çalışma alanı oluşturmanızı ister. **Kurulumu Tamam** ' ı seçin.
1. Hesabınız artık not defterlerini kullanacak şekilde etkinleştirilmiştir!

## <a name="create-and-run-your-first-notebook"></a>İlk not defterinizi oluşturma ve çalıştırma

Not defterlerini kullanacağınızı doğrulamak için örnek Not defterleri altındaki not defterlerinden birini seçin. Bu işlem, Not defterinin bir kopyasını çalışma alanınıza kaydeder ve açar.

Bu örnekte, **gettingstarted. ipynb**' yi kullanacağız. 

:::image type="content" source="media/enable-notebooks/select-getting-started-notebook.png" alt-text="GettingStarted. ipynb Not defterini görüntüle":::

Not defterini çalıştırmak için:
1. Python kodunu içeren ilk kod hücresini seçin. 
1. Hücreyi çalıştırmak için **Çalıştır** ' ı seçin. Hücreyi çalıştırmak için **SHIFT + enter** ' i de kullanabilirsiniz.
1. Oluşturulan veritabanını ve kapsayıcıyı görmek için kaynak bölmesini yenileyin.

    :::image type="content" source="media/enable-notebooks/run-first-notebook-cell.png" alt-text="Başlarken Not defterini çalıştır":::

Yeni bir not defteri oluşturmak için **Yeni Not** defteri ' ni **seçip not defterim menüsünden** **dosya karşıya yükle** ' yi seçerek var olan bir not defteri (. ipynb) dosyasını karşıya yükleyebilirsiniz. 

:::image type="content" source="media/enable-notebooks/create-or-upload-new-notebook.png" alt-text="Yeni bir not defteri oluşturun veya karşıya yükleyin":::

## <a name="next-steps"></a>Sonraki adımlar

- [Jupyıter Not defterlerinin Azure Cosmos DB](cosmosdb-jupyter-notebooks.md) avantajları hakkında bilgi edinin
