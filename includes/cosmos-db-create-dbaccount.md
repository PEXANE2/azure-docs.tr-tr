---
title: include dosyası
description: include dosyası
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.topic: include
ms.date: 04/08/2019
ms.author: rimman
ms.custom: include file
ms.openlocfilehash: 22df1e2251de618a0131e5d18cd72c752b00a16d
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75663093"
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
    |Konum|Kullanıcılarınıza en yakın bölge|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.|

   ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-account.png)

1. **İncele ve oluştur**’u seçin. **Ağ** ve **Etiketler** bölümlerini atlayabilirsiniz.

1. Hesap ayarlarını gözden geçirip **Oluştur** seçeneğini belirleyin. Hesabın oluşturulması birkaç dakika sürer. Portal sayfasında **Dağıtımınız tamamlandı** iletisinin görüntülenmesini bekleyin. 

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Azure Cosmos DB hesabı sayfasına gitmek için **Kaynağa git** seçeneğini belirleyin. 

    ![Azure Cosmos DB hesabı sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
