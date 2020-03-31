---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 01/22/2020
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 8f7a69b81430d964d1aade26ed179354171e4164
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77134717"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.

2. Sol menüde **kaynak oluştur'u**seçin.
   
   ![Azure portalında kaynak oluşturma](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-0.png)
   
3. **Yeni** sayfada, **Veritabanları** > **Azure Cosmos DB'yi**seçin.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)
   
3. Azure **Cosmos DB Hesabı Oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>Ardından Hesap Adı ile aynı adı girin|**Yeni oluştur**’u seçin. Ardından hesabınız için yeni bir kaynak grubu adı girin. Basitlik için Azure Cosmos hesap adınız ile aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad girin|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin. Uri hesabınız, benzersiz hesap adınıza cassandra.cosmos.azure.com *eklenir.*<br><br>Hesap adı yalnızca küçük harfleri, sayıları ve tireleri (-) kullanabilir ve 3 ile 31 karakter uzunluğunda olmalıdır.
    API|Cassandra|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB, belge veritabanları için beş API sağlar: Belge veritabanları için Çekirdek (SQL), grafik veritabanları için Gremlin, belge veritabanları için MongoDB, Azure Tablo ve Cassandra. Her API için ayrı bir hesap oluşturmanız gerekir. <br><br>**Cassandra'yı**seçin, çünkü bu hızlı başlangıçta Cassandra API ile çalışan bir tablo oluşturuyorsunuz. <br><br>[Cassandra API hakkında daha fazla bilgi edinin](../articles/cosmos-db/cassandra-introduction.md).|
    Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.

    **Gözden Geçir+Oluştur'u**seçin. **Ağ** ve **Etiketler** bölümünü atlayabilirsiniz. 

    ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-cassandra/azure-cosmos-db-create-new-account.png)

4. Hesabın oluşturulması birkaç dakika sürer. Tebrikler diyerek sayfagörüntülemek için portal **bekleyin! Azure Cosmos DB hesabınız oluşturuldu.**

