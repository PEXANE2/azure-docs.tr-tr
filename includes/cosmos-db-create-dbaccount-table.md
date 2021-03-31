---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 01/24/2020
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 941e2ee7b8eba957d970998324156c50a86439af
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94503925"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.

2. Sol taraftaki menüde **kaynak oluştur**' u seçin.
   
   :::image type="content" source="./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-0.png" alt-text="Azure portalında kaynak oluşturma":::
   
3. **Yeni** sayfada **veritabanları**  >  **Azure Cosmos DB**' nı seçin.
   
   :::image type="content" source="./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png" alt-text="Azure portalındaki Veritabanları bölmesi":::
   
3. **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|**Yeni**, sonra hesap adı oluştur|**Yeni oluştur**’u seçin. Ardından, hesabınız için yeni bir kaynak grubu adı girin. Basitlik için Azure Cosmos DB hesap adınızla aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin.<br><br>Hesap adı yalnızca küçük harf, sayı ve kısa çizgi (-) kullanabilir ve 3 ila 31 karakter uzunluğunda olmalıdır.
    API|Tablo|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: belge veritabanları için çekirdek (SQL), grafik veritabanları için Gremlin, belge veritabanları için MongoDB, Azure tablosu ve Cassandra. Her API için ayrı bir hesap oluşturmanız gerekir. <br><br>**Azure tablosu**' nu seçin, çünkü bu hızlı başlangıçta tablo API'si ile birlikte çalışacak bir tablo oluşturuyorsunuz. <br><br>[Tablo API'si hakkında daha fazla bilgi edinin](../articles/cosmos-db/table-introduction.md).|
    Konum|Kullanıcılarınıza en yakın bölge|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.
    Kapasite modu|Sağlanan aktarım hızı veya sunucusuz|[Sağlanan aktarım](../articles/cosmos-db/set-throughput.md) hızı modunda bir hesap oluşturmak için **sağlanan aktarım hızı** ' nı seçin. [Sunucusuz modda bir](../articles/cosmos-db/serverless.md) hesap oluşturmak Için **sunucusuz** ' ı seçin.

    Ek ücretlerden kaçınmak için, **coğrafi artıklık** ve **çok bölgeli yazma** seçeneklerini **devre dışı** bırakmak ve **ağ** ve **Etiketler** bölümlerini atlamak için kullanabilirsiniz.

5. **Gözden geçir + oluştur**' u seçin. Doğrulama tamamlandıktan sonra, hesabı oluşturmak için **Oluştur** ' u seçin. 
 
   :::image type="content" source="./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-create-new-account.png" alt-text="Azure Cosmos DB için yeni hesap sayfası":::

6. Hesabın oluşturulması birkaç dakika sürer. **Dağıtımınızın devam** ettiği bir ileti görürsünüz. Dağıtımın bitmesini bekleyin ve sonra **Kaynağa Git**' i seçin.

    :::image type="content" source="./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-account-created.png" alt-text="Azure portal bildirimleri bölmesi":::

