---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: include
ms.date: 04/10/2020
ms.custom: include file
ms.openlocfilehash: 4c6e45eaa816499c7e5969e302a036368bea0937
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81422331"
---
1. Azure Cosmos DB hesabı oluşturmak için [Azure portalına](https://portal.azure.com/) gidin. Giriş sayfanızda **Azure hizmetleri** panelinden **kaynak oluştur** ' u seçin.

   ![Kaynak oluşturma](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-resource-1.png)

1. **Azure Cosmos DB**’yi arayın ve seçin.

   ![Azure portal kaynakları açılır](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-updated-1.png)

1. **Oluştur**’u seçin.

   ![Azure Cosmos DB kaynağı oluşturma](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-account-1.png)

1. **Azure Cosmos DB Hesabı Oluştur** sayfasında, yeni Azure Cosmos hesabına yönelik temel ayarları girin. 

    |Ayar|Değer|Açıklama |
    |---|---|---|
    |Abonelik|Abonelik adı|Bu Azure Cosmos hesabı için kullanmak istediğiniz Azure aboneliğini seçin. |
    |Kaynak Grubu|Kaynak grubu adı|Bir kaynak grubu seçin veya **Yeni oluştur** seçeneğini belirleyin ve yeni kaynak grubu için benzersiz bir ad girin. |
    |Hesap Adı|Benzersiz bir ad|Azure Cosmos hesabınızı tanımlayan bir ad girin. URI’nizi oluşturmak için sağladığınız ada *documents.azure.com* ekleneceği için benzersiz bir ad kullanın.<br><br>Ad yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. Ad, 3-31 karakter arası uzunlukta olmalıdır.|
    |API|Oluşturulacak hesap türü|SQL söz dizimini kullanarak belge veritabanı ve sorgusu oluşturmak için **Cekirdek (SQL)** seçeneğini belirleyin. <br><br>API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: belge verileri için çekirdek (SQL) ve MongoDB, Graf verileri için Gremlin, Azure tablosu ve Cassandra. Şu anda, her API için farklı bir hesap oluşturmanız gerekir. <br><br>[SQL API’si hakkında daha fazla bilgi edinin](../articles/cosmos-db/documentdb-introduction.md).|
    |Ücretsiz katman Indirimi Uygula|Uygula veya Uygula|Azure Cosmos DB ücretsiz katman sayesinde ilk 400 RU/sn ve 5 GB depolama alanını hesapta ücretsiz olarak alırsınız. [Ücretsiz katman](https://azure.microsoft.com/pricing/details/cosmos-db/)hakkında daha fazla bilgi edinin.|
    |Konum|Kullanıcılarınıza en yakın bölge|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.|
    |Hesap türü|Üretim veya üretim dışı|Hesap bir üretim iş yükü için kullanılacaksa, **Üretim** ' ı seçin. Hesap üretim dışı, örneğin geliştirme, test, QA veya hazırlama için kullanılacaksa, **Üretim dışı** seçeneğini belirleyin. Bu, Portal deneyimini denetleyen ancak temel alınan Azure Cosmos DB hesabını etkilemeyen bir Azure Kaynak etiketi ayarıdır. Bu değeri dilediğiniz zaman değiştirebilirsiniz.|


    > [!NOTE]
    > Azure aboneliği başına en fazla bir ücretsiz katman Azure Cosmos DB hesabınız olabilir ve hesabı oluştururken kabul etmeniz gerekir. Ücretsiz katman indirimi uygulama seçeneğini görmüyorsanız bu, abonelikteki başka bir hesabın ücretsiz katmanla zaten etkinleştirildiği anlamına gelir.
   
   ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-detail.png)

1. **İncele ve oluştur**’u seçin. **Ağ** ve **Etiketler** bölümlerini atlayabilirsiniz.

1. Hesap ayarlarını gözden geçirip **Oluştur** seçeneğini belirleyin. Hesabın oluşturulması birkaç dakika sürer. Portal sayfasında **Dağıtımınız tamamlandı** iletisinin görüntülenmesini bekleyin. 

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Azure Cosmos DB hesabı sayfasına gitmek için **Kaynağa git** seçeneğini belirleyin. 

    ![Azure Cosmos DB hesabı sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
