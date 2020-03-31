---
title: Azure Cosmos DB hesabındaki dizüstü bilgisayarları etkinleştirme (önizleme)
description: Azure Cosmos DB'nin yerleşik dizüstü bilgisayarları, Verilerinizi Portal içinden analiz etmenizi ve görselleştirmenizi sağlar. Bu makalede, Cosmos hesapları için bu özelliğietkinleştirmek için nasıl açıklanmaktadır.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/22/2019
ms.author: dech
ms.openlocfilehash: dcec310db43baa513b2d574d03f3f35dee3f773b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76768013"
---
# <a name="enable-notebooks-for-azure-cosmos-db-accounts-preview"></a>Azure Cosmos DB hesapları için not defterlerini etkinleştirme (önizleme)

> [!IMPORTANT]
> Azure Cosmos DB için yerleşik dizüstü bilgisayarlar şu anda aşağıdaki Azure bölgelerinde kullanılabilir: Avustralya Doğu, Doğu ABD, Doğu ABD 2, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, İngiltere Güney, Batı Avrupa ve Batı ABD 2. Not defterlerini kullanmak [için, not defterleriyle yeni bir hesap oluşturun](#enable-notebooks-in-a-new-cosmos-account) veya bu bölgelerden birinde [varolan bir hesapta not defterlerini etkinleştirin.](#enable-notebooks-in-an-existing-cosmos-account)

Azure Cosmos DB'deki yerleşik Jupyter dizüstü bilgisayarlar, Verilerinizi Azure portalından analiz etmenizi ve görselleştirmenizi sağlar. Bu makale, Azure Cosmos DB hesabınız için bu özelliği etkinleştirmeyi açıklar.

## <a name="enable-notebooks-in-a-new-cosmos-account"></a>Yeni bir Cosmos hesabında ki dizüstü bilgisayarları etkinleştirme
1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. **Kaynak** > **Oluştur Veritabanları** > **Azure Cosmos DB'yi**seçin.
1. Azure **Cosmos DB Hesabı Oluştur** sayfasında **Not Defterleri'ni**seçin. 
 
    ![Azure Cosmos DB Create blade'de not defterleri seçeneğini seçin](media/enable-notebooks/create-new-account-with-notebooks.png)
1. **İncele ve oluştur**’u seçin. **Ağ** ve **Etiketler** seçeneğini atlayabilirsiniz. 
1. Hesap ayarlarını gözden geçirip **Oluştur** seçeneğini belirleyin. Hesabın oluşturulması birkaç dakika sürer. Portal sayfasında **Dağıtımınız tamamlandı** iletisinin görüntülenmesini bekleyin. 

    ![Azure portalındaki Bildirimler bölmesi](media/enable-notebooks/create-new-account-with-notebooks-complete.png)
1. Azure Cosmos DB hesabı sayfasına gitmek için **Kaynağa git** seçeneğini belirleyin. 

    ![Azure Cosmos DB hesabı sayfası](../../includes/media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-3.png)

1. **Veri Gezgini** bölmesine gidin. Artık not defterlerinizin çalışma alanını görmelisiniz.

    ![Yeni Azure Cosmos DB dizüstü bilgisayarlar çalışma alanı](media/enable-notebooks/new-notebooks-workspace.png)

## <a name="enable-notebooks-in-an-existing-cosmos-account"></a>Varolan bir Cosmos hesabındaki not defterlerini etkinleştirme
Varolan hesaplardaki not defterlerini de etkinleştirebilirsiniz. Bu adımın hesap başına yalnızca bir kez yapılması gerekir.

1. Cosmos hesabınızdaki **Veri Gezgini** bölmesine gidin.
1. **Not Defterlerini Etkinleştir'i**seçin.

    ![Veri Gezgini'nde yeni bir dizüstü bilgisayar çalışma alanı oluşturma](media/enable-notebooks/enable-notebooks-workspace.png)
1. Bu, yeni bir dizüstü bilgisayar çalışma alanı oluşturmanızı ister. **Kurulumu Tamamla'yı seçin.**
1. Hesabınız artık not defterlerini kullanmak için etkinleştirildi!

## <a name="create-and-run-your-first-notebook"></a>İlk not defterinizi oluşturun ve çalıştırın

Not defterlerini kullanabileceğinizi doğrulamak için Örnek Not Defterleri'nin altındaki not defterlerinden birini seçin. Bu işlem, not defterinin bir kopyasını çalışma alanınıza kaydeder ve açar.

Bu örnekte, **GettingStarted.ipynb'yi**kullanacağız. 

![GettingStarted.ipynb not defterini görüntüle](media/enable-notebooks/select-getting-started-notebook.png)

Not defterini çalıştırmak için:
1. Python kodunu içeren ilk kod hücresini seçin. 
1. Hücreyi çalıştırmak için **Çalıştır'ı** seçin. Hücreyi çalıştırmak için **Shift + Enter'u** da kullanabilirsiniz.
1. Oluşturulan veritabanını ve kapsayıcıyı görmek için kaynak bölmesini yenileyin.

    ![Çalıştırma başlangıç not defteri](media/enable-notebooks/run-first-notebook-cell.png)

Ayrıca, Yeni bir not defteri oluşturmak veya Varolan bir not defteri (.ipynb) dosyasını yükleyerek **Not Defterlerim** menüsünden **Dosya Yükle'yi** seçerek Yeni **Not Defteri'ni** seçebilirsiniz. 

![Yeni bir not defteri oluşturma veya yükleme](media/enable-notebooks/create-or-upload-new-notebook.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB Jupyter dizüstü bilgisayarların](cosmosdb-jupyter-notebooks.md) avantajları hakkında bilgi edinin
