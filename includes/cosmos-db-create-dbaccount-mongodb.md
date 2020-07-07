---
title: dosya dahil etme
description: dosya dahil etme
services: cosmos-db
ms.custom: include file
ms.openlocfilehash: ecf45a692f83689ed56c03bec13f291781508474
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77061733"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.

2. Sol taraftaki menüde **kaynak oluştur**' u seçin.
   
   ![Azure portal bir kaynak oluşturun](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-0.png)
   
3. **Yeni** sayfada **veritabanları**  >  **Azure Cosmos DB**' nı seçin.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)
   
3. **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Description
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>Daha sonra hesap adı ile aynı adı girin|**Yeni oluştur**’u seçin. Ardından, hesabınız için yeni bir kaynak grubu adı girin. Basitlik için Azure Cosmos DB hesap adınızla aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad girin|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin. Hesap URI 'niz, benzersiz hesap adınızın *Mongo.Cosmos.Azure.com* eklenir.<br><br>Hesap adı yalnızca küçük harf, sayı ve kısa çizgi (-) kullanabilir ve 3 ila 31 karakter uzunluğunda olmalıdır.
    API|Mongo DB API 'SI için Azure Cosmos DB|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: belge veritabanları için çekirdek (SQL), grafik veritabanları için Gremlin, belge veritabanları, Azure tablosu ve Cassandra için Mongo DB API 'SI için Azure Cosmos DB. Şu anda, her API için farklı bir hesap oluşturmanız gerekir. <br><br>**Mongo DB API 'si için Azure Cosmos DB** seçin çünkü bu hızlı başlangıçta MongoDB ile birlikte çalışarak bir koleksiyon oluşturuyorsunuz.<br><br>[MongoDB API 'si için Azure Cosmos DB hakkında daha fazla bilgi edinin](../articles/cosmos-db/mongodb-introduction.md).|
    Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.|

    **Gözden geçir + oluştur**' u seçin. **Ağ** ve **Etiketler** bölümünü atlayabilirsiniz. 

    ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png)

4. Hesabın oluşturulması birkaç dakika sürer. Portalın Tebrikler görüntülemesini bekleyin **! Mongo DB API hesabı için Azure Cosmos DB, sayfada hazırlanıyor** .

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png)
