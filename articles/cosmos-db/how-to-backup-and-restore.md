---
title: Azure Cosmos DB verilerini yedeklemeden geri yükleme
description: Bu makalede, azure cosmos DB verilerinin bir yedeklemeden nasıl geri yüklenir, verileri geri yüklemek için Azure desteğine nasıl başvurulur, veriler geri yüklendikten sonra atılacak adımlar açıklanmaktadır.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 19ca835ca8211202cd358ac2ec3695675183a372
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70240774"
---
# <a name="restore-data-from-a-backup-in-azure-cosmos-db"></a>Azure Cosmos DB'deki bir yedeklemeden veri geri yükleme 

Veritabanınızı veya bir kapsayıcıyı yanlışlıkla silerseniz, otomatik çevrimiçi yedeklemelerden verileri geri yüklemek için [bir destek bileti açabilir]( https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veya Azure desteğini [arayabilirsiniz.]( https://azure.microsoft.com/support/options/) Azure desteği yalnızca **Standart**, **Geliştirici**ve onlardan daha yüksek planlar gibi seçili planlar için kullanılabilir. **Temel** planla Azure desteği kullanılamıyor. Farklı destek planları hakkında bilgi edinmek için [Azure destek planları](https://azure.microsoft.com/support/plans/) sayfasına bakın. 

Yedeklemenin belirli bir anlık görüntüsünü geri yüklemek için Azure Cosmos DB, verilerin bu anlık görüntü için yedekleme döngüsü süresince kullanılabilir olmasını gerektirir.

## <a name="request-a-restore"></a>Geri yükleme isteğinde bulunun

Geri yükleme isteğinde bulunmadan önce aşağıdaki ayrıntılara sahip olmalısınız:

* Abonelik kimliğinizi hazırlayın.

* Verilerinizin yanlışlıkla silinme veya değiştirilme şekline bağlı olarak, ek bilgilere sahip olmaya hazırlanmanız gerekir. Bir süre hassas durumlarda zararlı olabilecek ileri geri bilgileri en aza indirmek için önceden mevcut bilgilere sahip olduğunuz önerilir.

* Azure Cosmos DB hesabının tamamı silinirse, silinen hesabın adını sağlamanız gerekir. Silinen hesapla aynı ada sahip başka bir hesap oluşturursanız, bunu destek ekibiyle paylaşın, çünkü bu hesap seçilecek doğru hesabı belirlemeye yardımcı olur. Geri yükleme durumunun karışıklığını en aza indirdiğinden, silinen her hesap için farklı destek biletleri dosyalamak önerilir.

* Bir veya daha fazla veritabanı silinirse, Azure Cosmos hesabının yanı sıra Azure Cosmos veritabanı adlarını sağlamalı ve aynı ada sahip yeni bir veritabanı olup olmadığını belirtmelisiniz.

* Bir veya daha fazla kapsayıcı silinirse, Azure Cosmos hesap adını, veritabanı adlarını ve kapsayıcı adlarını sağlamanız gerekir. Ve aynı ada sahip bir kapsayıcı olup olmadığını belirtin.

* Verilerinizi yanlışlıkla sildiyseniz veya bozduysanız, Azure Cosmos DB ekibinin yedeklemelerden verileri geri yüklemenize yardımcı olması için 8 saat içinde [Azure desteğine](https://azure.microsoft.com/support/options/) başvurmalısınız.
  
  * Veritabanınızı veya kapsayıcınızı yanlışlıkla sildiyseniz, bir Sev B veya Sev C Azure destek kılıfı açın. 
  * Kapsayıcıdaki bazı belgeleri yanlışlıkla sildiyseniz veya bozduysanız, bir Sev A destek kılıfı açın. 

Veri bozulması oluştuğunda ve kapsayıcı içindeki belgeler değiştirildiğinde veya silinirse, **kapsayıcıyı mümkün olan en kısa sürede silin.** Kapsayıcıyı silerek Azure Cosmos DB'nin yedeklemelere fazla yazmasını önleyebilirsiniz. Herhangi bir nedenle silme mümkün değilse, en kısa sürede bir bilet dosyası gerekir. Azure Cosmos hesap adı, veritabanı adları, kapsayıcı adları ek olarak, verilerin geri yüklenebileceği noktayı da belirtmeniz gerekir. O anda mevcut en iyi yedeklemeleri belirlememize yardımcı olmak mümkün olduğunca hassas olmak önemlidir. UTC'de zamanı belirtmek de önemlidir. 

Aşağıdaki ekran görüntüsü, Azure portalını kullanarak verileri geri yüklemek için bir kapsayıcı (koleksiyon/grafik/tablo) için bir destek isteğinin nasıl oluşturuluğa gösterilmektedir. İsteğe öncelik vermemize yardımcı olmak için veri türü, geri yüklemenin amacı, verilerin silindiği zaman gibi ek ayrıntılar sağlayın.

![Azure portalını kullanarak yedek destek isteği oluşturma](./media/how-to-backup-and-restore/backup-support-request-portal.png)

## <a name="post-restore-actions"></a>Geri yükleme sonrası eylemler

Verileri geri yükledikten sonra, yeni hesabın adı (genellikle biçimdedir) `<original-name>-restored1`ve hesabın geri yüklenme zamanı hakkında bir bildirim alırsınız. Geri yüklenen hesap, sağlanan iş, dizine ilişkin ilkelere sahip olacak ve orijinal hesapla aynı bölgede dir. Abonelik yöneticisi veya yardımcı yönetici olan bir kullanıcı geri yüklenen hesabı görebilir.

Veriler geri yüklendikten sonra, geri yüklenen hesaptaki verileri denetlemeli ve doğrulamalı ve beklediğiniz sürümü içerdiğinden emin olmalısınız. Her şey yolunda görünüyorsa, [Azure Cosmos DB değişiklik akışı](change-feed.md) veya [Azure Veri Fabrikası'nı](../data-factory/connector-azure-cosmos-db.md)kullanarak verileri orijinal hesabınıza geri aktarmalısınız.

Verileri geçişten hemen sonra kapsayıcıyı veya veritabanını silmeniz önerilir. Geri yüklenen veritabanlarını veya kapsayıcıları silmezseniz, istek birimleri, depolama ve çıkış için maliyet eve girer.

## <a name="next-steps"></a>Sonraki adımlar

Ardından, aşağıdaki makaleleri kullanarak verileri orijinal hesabınıza nasıl geri geçirebileceğinizi öğrenebilirsiniz:

* Geri yükleme isteği nde bulunmak için Azure Desteği'ne başvurun ve [Azure portalından bilet gönderin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
* Verileri Azure Cosmos DB'ye taşımak için [Cosmos DB değiştirme akışını kullanın.](change-feed.md)

* Verileri Azure Cosmos DB'ye taşımak için [Azure Veri Fabrikası'nı kullanın.](../data-factory/connector-azure-cosmos-db.md)
