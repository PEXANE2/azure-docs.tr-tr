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
ms.openlocfilehash: 8f8b8384b0c570e2b4925b0e84480b19a632187e
ms.sourcegitcommit: 02160a2c64a5b8cb2fb661a087db5c2b4815ec04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75752161"
---
1. Bir Azure Cosmos DB hesabı oluşturmak için [Azure Portal](https://portal.azure.com/) gidin. Arama yapın ve **Azure Cosmos DB**seçin.

   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

1. **Add (Ekle)** seçeneğini belirleyin.
1. **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos hesabı için temel ayarları girin. 

    |Ayar|Değer|Açıklama |
    |---|---|---|
    |Abonelik|Abonelik adı|Bu Azure Cosmos hesabı için kullanmak istediğiniz Azure aboneliğini seçin. |
    |Kaynak Grubu|Kaynak grubu adı|Bir kaynak grubu seçin veya **Yeni oluştur**' u seçip yeni kaynak grubu için benzersiz bir ad girin. |
    |Hesap Adı|Benzersiz bir ad|Azure Cosmos hesabınızı tanımlamak için bir ad girin. *Documents.Azure.com* , URI 'nizi oluşturmak için sağladığınız ada eklendiği için benzersiz bir ad kullanın.<br><br>Ad yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3-31 karakter uzunluğunda olmalıdır.|
    |eklentisi|Oluşturulacak hesabın türü|SQL sözdizimini kullanarak bir belge veritabanı ve sorgu oluşturmak için **Core (SQL)** seçeneğini belirleyin. <br><br>API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: belge verileri için çekirdek (SQL) ve MongoDB, Graf verileri için Gremlin, Azure tablosu ve Cassandra. Şu anda, her bir API için ayrı bir hesap oluşturmanız gerekir. <br><br>[SQL API'si hakkında daha fazla bilgi](../articles/cosmos-db/documentdb-introduction.md).|
    |Konum|Kullanıcılarınıza en yakın bölge|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişim sağlamak için kullanıcılarınıza en yakın konumu kullanın.|

   ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-account.png)

1. **İncele ve oluştur**’u seçin. **Ağ** ve **Etiketler** bölümlerini atlayabilirsiniz.

1. Hesap ayarlarını gözden geçirin ve ardından **Oluştur**' u seçin. Hesabın oluşturulması birkaç dakika sürer. Portalın, **dağıtımınızın**tamamlanmasını bekleyin. 

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Azure Cosmos DB hesap sayfasına gitmek için **Kaynağa Git** ' i seçin. 

    ![Azure Cosmos DB hesabı sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
