---
title: dosya dahil etme
description: dosya dahil etme
services: cosmos-db
author: LuisBosquez
ms.service: cosmos-db
ms.topic: include
ms.date: 04/15/2020
ms.author: lbosq
ms.custom: include file
ms.openlocfilehash: 8d12645bd302a7ea808f13c07e2a72f66846bad7
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2020
ms.locfileid: "85121453"
---
Artık, MongoDB veritabanı ve kapsayıcısı için bir Azure Cosmos DB API 'SI oluşturmak üzere Azure portal Veri Gezgini aracı 'nı kullanabilirsiniz. 

1. Yeni **Veri Gezgini**  >  **kapsayıcı**seçin. 
    
    **Kapsayıcı Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    :::image type="content" source="./media/cosmos-db-create-collection/azure-cosmosdb-mongodb-data-explorer.png" alt-text="Azure portalındaki Veri Gezgini, Kapsayıcı Ekle bölmesi":::

2. **Kapsayıcı Ekle** sayfasında, yeni kapsayıcının ayarlarını girin.

    |Ayar|Önerilen değer|Description
    |---|---|---|
    |**Veritabanı Kimliği**|veritabanı|Yeni veritabanının adı olarak *DB* girin. Veritabanı adları 1 ila 255 karakterden oluşmalıdır ve `/, \\, #, ?` boşluk içeremez veya sonunda boşluk olamaz. Veritabanı **Işleme sağlama** seçeneğini kontrol edin, veritabanı içindeki tüm kapsayıcılar üzerinde veritabanı için sağlanan aktarım hızını paylaşmanıza olanak sağlar. Bu seçenek maliyet tasarruflarıyla de yardımcı olur. |
    |**Aktarım hızı**|400|Aktarım hızını saniyede 400 istek birimi (RU/s) olarak bırakın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz. Ayrıca, gerektiğinde dinamik olarak arttırmayı ve azaltmanızı sağlayan bir RU/sn aralığı sağlayacak [Otomatik ölçeklendirme modunu](https://docs.microsoft.com/azure/cosmos-db/provision-throughput-autoscale)da seçebilirsiniz.| 
    |**Koleksiyon KIMLIĞI**|Coll|`coll`Yeni kapsayıcının adı olarak girin. Kapsayıcı kimliklerinin karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır.|
    |**Depolama kapasitesi**|Sabit (10 GB)|Bu uygulama için *sabit (10 GB)* girin. *Sınırsız*seçeneğini belirlerseniz, `Shard Key` tüm ekli öğelerin gerekli olacağı bir oluşturmanız gerekecektir.|
    |**Parça anahtarı**| /_id| Bu makalede açıklanan örnek bir parça anahtarı kullanmaz, bu nedenle */_id* olarak ayarlamak, otomatik olarak üretilen kimliği alanını parça anahtarı olarak kullanır. Bölümlendirme olarak da bilinen parça hakkında daha fazla bilgi için [Azure Cosmos DB Bölümlemede](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview)|
        
    **Tamam**’ı seçin. Veri Gezgini, yeni veritabanını ve kapsayıcıyı görüntüler.
