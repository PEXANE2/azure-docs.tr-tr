---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: include
ms.date: 04/08/2019
ms.custom: include file
ms.openlocfilehash: bfce10b0c9c55012c3ba2fb25121b28157f203b8
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78672054"
---
1. Azure Cosmos DB hesabı oluşturmak için [Azure portalına](https://portal.azure.com/) gidin. **Azure Cosmos DB**’yi arayın ve seçin.

   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

1. **Add (Ekle)** seçeneğini belirleyin.
1. **Azure Cosmos DB Hesabı Oluştur** sayfasında, yeni Azure Cosmos hesabına yönelik temel ayarları girin. 

    |Ayar|Değer|Açıklama |
    |---|---|---|
    |Abonelik|Abonelik adı|Bu Azure Cosmos hesabı için kullanmak istediğiniz Azure aboneliğini seçin. |
    |Kaynak Grubu|Kaynak grubu adı|Bir kaynak grubu seçin veya **Yeni oluştur** seçeneğini belirleyin ve yeni kaynak grubu için benzersiz bir ad girin. |
    |Hesap Adı|Benzersiz bir ad|Azure Cosmos hesabınızı tanımlayan bir ad girin. URI’nizi oluşturmak için sağladığınız ada *documents.azure.com* ekleneceği için benzersiz bir ad kullanın.<br><br>Ad yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. Ad, 3-31 karakter arası uzunlukta olmalıdır.|
    |API|Oluşturulacak hesap türü|SQL söz dizimini kullanarak belge veritabanı ve sorgusu oluşturmak için **Cekirdek (SQL)** seçeneğini belirleyin. <br><br>API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: Belge verileri için Çekirdek (SQL) ve MongoDB, Graf verileri için Gremlin, Azure Tablosu ve Cassandra. Şu anda, her API için farklı bir hesap oluşturmanız gerekir. <br><br>[SQL API’si hakkında daha fazla bilgi edinin](../articles/cosmos-db/documentdb-introduction.md).|
    |Ücretsiz Katman İndirimi uygulama|Uygula veya Uygulama|Azure Cosmos DB ücretsiz katmanı ile hesabınızda ilk 400 RU/sn ve 5 GB depolama alanını ücretsiz elde edersiniz. [Ücretsiz katman](https://azure.microsoft.com/pricing/details/cosmos-db/) hakkında daha fazla bilgi edinin.|
    |Konum|Kullanıcılarınıza en yakın bölge|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.|
    |Hesap Türü|Üretim veya Üretim dışı|Hesap bir üretim iş yükü için kullanılacaksa **Üretim** seçeneğini belirleyin. Hesap üretim dışı bir amaç doğrultusunda kullanılacaksa (geliştirme, test etme, kalite denetimi veya hazırlama gibi) **Üretim dışı** seçeneğini belirleyin. Bu, Portal deneyiminde hassas ayarlamalar yapan ancak temel alınan Azure Cosmos DB hesabını olumsuz etkilemeyen bir Azure kaynak etiketi ayarıdır. Bu değeri istediğiniz zaman değiştirebilirsiniz.|


> [!NOTE]
> Azure aboneliği başına bir ücretsiz katman Azure Cosmos DB hesabına sahip olabilirsiniz. Bunu hesabı oluştururken kabul etmeniz gerekir. Ücretsiz katman indirimini uygulama seçeneğinin gözükmemesi abonelikteki farklı bir hesabın ücretsiz katman ile etkinleştirildiğini belirtir.
   
   ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-detail.png)

1. **İncele ve oluştur**’u seçin. **Ağ** ve **Etiketler** bölümlerini atlayabilirsiniz.

1. Hesap ayarlarını gözden geçirip **Oluştur** seçeneğini belirleyin. Hesabın oluşturulması birkaç dakika sürer. Portal sayfasında **Dağıtımınız tamamlandı** iletisinin görüntülenmesini bekleyin. 

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Azure Cosmos DB hesabı sayfasına gitmek için **Kaynağa git** seçeneğini belirleyin. 

    ![Azure Cosmos DB hesabı sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
