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
ms.openlocfilehash: 2001d0621a340cbdb04c0bb5eea1166ce8b88eab
ms.sourcegitcommit: 0eb0673e7dd9ca21525001a1cab6ad1c54f2e929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77212679"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.

2. Sol taraftaki menüde **kaynak oluştur**' u seçin.
   
   ![Azure portal bir kaynak oluşturun](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-0.png)
   
3. **Yeni** sayfasında **veritabanları** > **Azure Cosmos DB**' ni seçin.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png)
   
3. **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|**Yeni**, sonra hesap adı oluştur|**Yeni oluştur**’u seçin. Ardından, hesabınız için yeni bir kaynak grubu adı girin. Basitlik için Azure Cosmos DB hesap adınızla aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin.<br><br>Hesap adı yalnızca küçük harf, sayı ve kısa çizgi (-) kullanabilir ve 3 ila 31 karakter uzunluğunda olmalıdır.
    API|Tablo|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: belge veritabanları için çekirdek (SQL), grafik veritabanları için Gremlin, belge veritabanları için MongoDB, Azure tablosu ve Cassandra. Her API için ayrı bir hesap oluşturmanız gerekir. <br><br>**Azure tablosu**' nu seçin, çünkü bu hızlı başlangıçta tablo API'si ile birlikte çalışacak bir tablo oluşturuyorsunuz. <br><br>[Tablo API'si hakkında daha fazla bilgi edinin](../articles/cosmos-db/table-introduction.md).|
    Konum|Kullanıcılarınıza en yakın bölge|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.

    Ek ücretlerden kaçınmak için, **coğrafi artıklık** ve **çok bölgeli yazma** seçeneklerini **devre dışı** bırakmak ve **ağ** ve **Etiketler** bölümlerini atlamak için kullanabilirsiniz.

5. **Gözden geçir + oluştur**' u seçin. Doğrulama tamamlandıktan sonra, hesabı oluşturmak için **Oluştur** ' u seçin. 
 
   ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-create-new-account.png)

6. Hesabın oluşturulması birkaç dakika sürer. **Dağıtımınızın devam**ettiği bir ileti görürsünüz. Dağıtımın bitmesini bekleyin ve sonra **Kaynağa Git**' i seçin.

    ![Azure portal bildirimleri bölmesi](./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-account-created.png)

