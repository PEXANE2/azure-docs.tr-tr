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
ms.openlocfilehash: 5d57d7e18befba175a5a8a825494ce512644b5a2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "67188124"
---
1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Kaynak oluştur** > **Veritabanları** > **Azure Cosmos DB** seçeneğini belirleyin.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

1. **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos hesabı için temel ayarları girin. 
 
    |Ayar|Değer|Açıklama |
    |---|---|---|
    |Subscription|Abonelik adı|Bu Azure Cosmos hesabı için kullanmak istediğiniz Azure aboneliğini seçin. |
    |Kaynak Grubu|Kaynak grubu adı|Bir kaynak grubu seçin veya **Yeni oluştur**' u seçip yeni kaynak grubu için benzersiz bir ad girin. |
    | Hesap Adı|Benzersiz bir ad girin|Azure Cosmos hesabınızı tanımlamak için bir ad girin. Girdiğiniz kimliğe *documents.azure.com* eklenerek URI'niz oluşturulacağından benzersiz bir kimlik kullanın.<br><br>KIMLIK yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3-31 karakter uzunluğunda olmalıdır.|
    | API|Çekirdek (SQL)|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: Belge verileri için çekirdek (SQL) ve MongoDB, Graf verileri, Azure tablosu ve Cassandra için Gremlin. Şu anda, her bir API için ayrı bir hesap oluşturmanız gerekir. <br><br>SQL sözdizimini kullanarak bir belge veritabanı ve sorgu oluşturmak için **Core (SQL)** seçeneğini belirleyin. <br><br>[SQL API'si hakkında daha fazla bilgi](../articles/cosmos-db/documentdb-introduction.md).|
    | Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişim sağlamak için kullanıcılarınıza en yakın konumu kullanın.|
   
   ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account.png)

1. **İncele ve oluştur**’u seçin. **Ağ** ve **Etiketler** bölümlerini atlayabilirsiniz. 

1. Hesap ayarlarını gözden geçirin ve ardından **Oluştur**' u seçin. Hesabın oluşturulması birkaç dakika sürer. Portalın, **dağıtımınızın**tamamlanmasını bekleyin. 

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Azure Cosmos DB hesap sayfasına gitmek için **Kaynağa Git** ' i seçin. 

    ![Azure Cosmos DB hesabı sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
