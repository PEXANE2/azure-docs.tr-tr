---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 9bc5be37f3892186233fac197c08066dbfacb43f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77780770"
---
Artık bir veritabanı ve kapsayıcı oluşturmak için Azure portalındaki Veri Gezgini aracını kullanabilirsiniz. 

1. Veri Gezgini > **Yeni** **Kapsayıcı'yı**seçin. 
    
    **Kapsayıcı Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    ![Azure portalındaki Veri Gezgini, Kapsayıcı Ekle bölmesi](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. Kapsayıcı **Ekle** sayfasına yeni kapsayıcının ayarlarını girin.

    |Ayar|Önerilen değer|Açıklama
    |---|---|---|
    |**Veritabanı Kimliği**|Görevler|Yeni veritabanınızın adı olarak *Görevler* girin. Veritabanı adları 1 ile 255 arasında karakter içermelidir ve bunlar veya bir iz alanı içeremez. `/, \\, #, ?` Tedarik **veritabanı iş verme** seçeneğini denetleyin, veritabanına sağlanan iş veri tabanına sağlanan iş veri tabanı içindeki tüm kapsayıcılar arasında paylaşmanızı sağlar. Bu seçenek aynı zamanda maliyet tasarrufu ile yardımcı olur. |
    |**Aktarım hızı**|400|Çıktıyı saniyede 400 istek biriminde (RU/s) bırakın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.| 
    |**Konteyner Kimliği**|Öğeler|Yeni kapsayıcınızın adı olarak *Öğeleri* girin. Kapsayıcı kimliklerinin karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır.|
    |**Bölüm anahtarı**| /kategori| Bu makalede açıklanan örnek, */kategoriyi* bölüm anahtarı olarak kullanır.|
    
    Önceki ayarlara ek olarak, isteğe bağlı olarak kapsayıcı için **Benzersiz tuşları** ekleyebilirsiniz. Bu örnekte bu alanı boş bırakalım. Benzersiz anahtarlar sayesinde geliştiriciler veritabanına bir veri bütünlüğü katmanı ekleyebilir. Kapsayıcı oluştururken benzersiz bir anahtar ilkesi oluşturarak, bölüm anahtarı başına bir veya daha fazla değerin benzersizliğini sağlarsınız. Daha fazla bilgi edinmek için [Azure Cosmos DB'de benzersiz anahtarlar](../articles/cosmos-db/unique-keys.md) makalesine bakın.
    
    **Tamam'ı**seçin. Veri Gezgini, yeni veritabanını ve kapsayıcıyı görüntüler.