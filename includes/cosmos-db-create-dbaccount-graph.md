---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 01/27/2020
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: a6cfafd41d1632d99b7ca03bce2316ec679d7579
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77061861"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.

2. Sol taraftaki menüde **kaynak oluştur**' u seçin.
   
   ![Azure portal bir kaynak oluşturun](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-0.png)
   
3. **Yeni** sayfasında **veritabanları** > **Azure Cosmos DB**' ni seçin.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)
   
3. **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>Daha sonra hesap adı ile aynı adı girin|**Yeni oluştur**’u seçin. Ardından, hesabınız için yeni bir kaynak grubu adı girin. Basitlik için Azure Cosmos DB hesap adınızla aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad girin|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin. Hesap URI 'niz, benzersiz hesap adınızın *Gremlin.Azure.com* eklenir.<br><br>Hesap adı yalnızca küçük harf, sayı ve kısa çizgi (-) kullanabilir ve 3 ila 31 karakter uzunluğunda olmalıdır.
    API|Gremlin (grafik)|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: belge veritabanları için çekirdek (SQL), grafik veritabanları için Gremlin, belge veritabanları için MongoDB, Azure tablosu ve Cassandra. Her API için ayrı bir hesap oluşturmanız gerekir. <br><br>**Gremlin (Graf)** seçeneğini belirleyin. bu hızlı başlangıçta GREMLIN API 'siyle birlikte çalışarak bir tablo oluşturuyorsunuz. <br><br>[Gremlin API 'si hakkında daha fazla bilgi edinin](../articles/cosmos-db/graph-introduction.md).|
    Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.

    **Gözden geçir + oluştur**' u seçin. **Ağ** ve **Etiketler** bölümünü atlayabilirsiniz. 

    ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-create-new-account.png)

4. Hesabın oluşturulması birkaç dakika sürer. Portalın Tebrikler görüntülemesini bekleyin **! Azure Cosmos DB hesabınız oluşturuldu** sayfası.
   
   ![Azure Cosmos DB hesabı oluşturuldu sayfası](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-graph-created.png)

