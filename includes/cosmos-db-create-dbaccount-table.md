---
title: dosya dahil etme
description: dosya dahil etme
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 01/24/2020
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 5015371afc67574a214097f8d3eef661df29c22f
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2020
ms.locfileid: "85115056"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.

2. Sol taraftaki menüde **kaynak oluştur**' u seçin.
   
   :::image type="content" source="./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-0.png" alt-text="Azure portal bir kaynak oluşturun":::
   
3. **Yeni** sayfada **veritabanları**  >  **Azure Cosmos DB**' nı seçin.
   
   :::image type="content" source="./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png" alt-text="Azure portalındaki Veritabanları bölmesi":::
   
3. **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Description
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|**Yeni**, sonra hesap adı oluştur|**Yeni oluştur**’u seçin. Ardından, hesabınız için yeni bir kaynak grubu adı girin. Basitlik için Azure Cosmos DB hesap adınızla aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin.<br><br>Hesap adı yalnızca küçük harf, sayı ve kısa çizgi (-) kullanabilir ve 3 ila 31 karakter uzunluğunda olmalıdır.
    API|Tablo|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: belge veritabanları için çekirdek (SQL), grafik veritabanları için Gremlin, belge veritabanları için MongoDB, Azure tablosu ve Cassandra. Her API için ayrı bir hesap oluşturmanız gerekir. <br><br>**Azure tablosu**' nu seçin, çünkü bu hızlı başlangıçta tablo API'si ile birlikte çalışacak bir tablo oluşturuyorsunuz. <br><br>[Tablo API'si hakkında daha fazla bilgi edinin](../articles/cosmos-db/table-introduction.md).|
    Konum|Kullanıcılarınıza en yakın bölge|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.

    Ek ücretlerden kaçınmak için, **coğrafi artıklık** ve **çok bölgeli yazma** seçeneklerini **devre dışı** bırakmak ve **ağ** ve **Etiketler** bölümlerini atlamak için kullanabilirsiniz.

5. **Gözden geçir + oluştur**' u seçin. Doğrulama tamamlandıktan sonra, hesabı oluşturmak için **Oluştur** ' u seçin. 
 
   :::image type="content" source="./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-create-new-account.png" alt-text="Azure Cosmos DB için yeni hesap sayfası":::

6. Hesabın oluşturulması birkaç dakika sürer. **Dağıtımınızın devam**ettiği bir ileti görürsünüz. Dağıtımın bitmesini bekleyin ve sonra **Kaynağa Git**' i seçin.

    :::image type="content" source="./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-account-created.png" alt-text="Azure portal bildirimleri bölmesi":::

