---
title: include dosyası
description: include dosyası
services: cosmos-db
ms.custom: include file
ms.openlocfilehash: b1dcf395c55e91a98b237f6e3866c97d74b7a97c
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942639"
---
1. Yeni bir pencerede [Azure portalında](https://portal.azure.com/) oturum açın.
2. Sol taraftaki menüde **kaynak oluştur**' u seçin, **veritabanları**' nı seçin ve ardından **Azure Cosmos DB**altında **Oluştur**' u seçin.
   
   ![Azure portal ekran görüntüsü, daha fazla hizmet vurgulama ve Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. İçinde **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>Ardından Kimlikte sağlanan benzersiz adın aynısını girin|**Yeni oluştur**’u seçin. Ardından hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için kimliğinizle aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad girin|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin. *Mongo.Cosmos.Azure.com* , URI 'nizi oluşturmak IÇIN sağladığınız kimliğe eklendiği için BENZERSIZ bir kimlik kullanın.<br><br>Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini kullanabilirsiniz. 3 ila 31 karakter uzunluğunda olmalıdır.
    eklentisi|MongoDB için Azure Cosmos DB API'si|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: belge veritabanları için çekirdek (SQL), grafik veritabanları için Gremlin, belge veritabanları, Azure tablosu ve Cassandra için API MongoDB Azure Cosmos DB. Şu anda, her bir API için ayrı bir hesap oluşturmanız gerekir. <br><br>**MongoDB** ' yi seçin. bu hızlı başlangıçta MongoDB ile birlikte çalışarak bir koleksiyon oluşturuyorsunuz.|
    Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.
    Sürüm|3.6|MongoDB tel Protokolü sürüm 3,6 ' yı seçin veya geriye dönük uyumluluk için 3,2 ' i seçin.

    Seçin **gözden geçir + Oluştur**. Atlayabilirsiniz **ağ** ve **etiketleri** bölümü. 

    ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png)

4. Hesabın oluşturulması birkaç dakika sürer. Portalın Tebrikler görüntülemesini bekleyin **! MongoDB için tel protokol uyumluluğuna sahip Cosmos hesabınız hazırlanıyor** .

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png)
