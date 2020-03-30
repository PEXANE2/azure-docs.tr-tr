---
title: include dosyası
description: include dosyası
services: cosmos-db
ms.custom: include file
ms.openlocfilehash: ecf45a692f83689ed56c03bec13f291781508474
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77061733"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.

2. Sol menüde **kaynak oluştur'u**seçin.
   
   ![Azure portalında kaynak oluşturma](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-0.png)
   
3. **Yeni** sayfada, **Veritabanları** > **Azure Cosmos DB'yi**seçin.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)
   
3. Azure **Cosmos DB Hesabı Oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>Ardından Hesap Adı ile aynı adı girin|**Yeni oluştur**’u seçin. Ardından hesabınız için yeni bir kaynak grubu adı girin. Basitlik için Azure Cosmos DB hesap adınız ile aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad girin|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin. Uri *hesabınız,* benzersiz hesap adınıza mongo.cosmos.azure.com eklenir.<br><br>Hesap adı yalnızca küçük harfleri, sayıları ve tireleri (-) kullanabilir ve 3 ile 31 karakter uzunluğunda olmalıdır.
    API|Mongo DB API için Azure Cosmos DB|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB, belge veritabanları için beş API sağlar: Belge veritabanları için Çekirdek (SQL), grafik veritabanları için Gremlin, belge veritabanları için Mongo DB API için Azure Cosmos DB, Azure Tablo ve Cassandra. Şu anda, her API için farklı bir hesap oluşturmanız gerekir. <br><br>**Mongo DB API için Azure Cosmos DB'yi** seçin, çünkü bu hızlı başlangıçta MongoDB ile çalışan bir koleksiyon oluşturuyorsunuz.<br><br>[MongoDB API için Azure Cosmos DB hakkında daha fazla bilgi edinin.](../articles/cosmos-db/mongodb-introduction.md)|
    Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.|

    **Gözden Geçir+Oluştur'u**seçin. **Ağ** ve **Etiketler** bölümünü atlayabilirsiniz. 

    ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png)

4. Hesabın oluşturulması birkaç dakika sürer. Tebrikler görüntülemek için portal **bekleyin! Mongo DB API hesabınız için Azure Cosmos DB'niz hazır sayfadır.**

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png)
