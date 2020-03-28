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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77212679"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.

2. Sol menüde **kaynak oluştur'u**seçin.
   
   ![Azure portalında kaynak oluşturma](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-0.png)
   
3. **Yeni** sayfada, **Veritabanları** > **Azure Cosmos DB'yi**seçin.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png)
   
3. Azure **Cosmos DB Hesabı Oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|**Yeni**, sonra Hesap Adı oluşturma|**Yeni oluştur**’u seçin. Ardından hesabınız için yeni bir kaynak grubu adı girin. Basitlik için Azure Cosmos DB hesap adınız ile aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin.<br><br>Hesap adı yalnızca küçük harfleri, sayıları ve tireleri (-) kullanabilir ve 3 ile 31 karakter uzunluğunda olmalıdır.
    API|Tablo|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB, belge veritabanları için beş API sağlar: Belge veritabanları için Çekirdek (SQL), grafik veritabanları için Gremlin, belge veritabanları için MongoDB, Azure Tablo ve Cassandra. Her API için ayrı bir hesap oluşturmanız gerekir. <br><br>**Azure Tablosu'nu**seçin, çünkü bu hızlı başlangıçta Tablo API ile çalışan bir tablo oluşturuyorsunuz. <br><br>[Tablo API'si hakkında daha fazla bilgi edinin.](../articles/cosmos-db/table-introduction.md)|
    Konum|Kullanıcılarınıza en yakın bölge|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.

    Ek ücretlerden kaçınmak için **Coğrafi Artıklık** ve **Çok Bölgeli Yazma** seçeneklerini Devre **Dışı** bırakabilir ve **Ağ** ve **Etiketler** bölümlerini atlayabilirsiniz.

5. **Gözden Geçir+Oluştur'u**seçin. Doğrulama tamamlandıktan sonra, hesabı oluşturmak için **Oluştur'u** seçin. 
 
   ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-create-new-account.png)

6. Hesabın oluşturulması birkaç dakika sürer. **Dağıtımınızın devam**ettiğini belirten bir ileti görürsünüz. Dağıtımın tamamlanmasını bekleyin ve ardından **kaynağa Git'i**seçin.

    ![Azure portal bildirimleri bölmesi](./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-account-created.png)

