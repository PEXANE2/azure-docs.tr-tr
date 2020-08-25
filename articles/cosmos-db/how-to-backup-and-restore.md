---
title: Azure Cosmos DB verilerini yedekten geri yükleme
description: Bu makalede bir yedekten Azure Cosmos DB verilerinin nasıl geri yükleneceği, verilerin geri yüklenmesi için Azure desteği ile iletişim kurma, veriler geri yüklendikten sonra gerçekleştirilecek adımlar açıklanmaktadır.
author: kanshiG
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/24/2020
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 9956ca0ca9c0957557e7ee74883a75c074ff22f8
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88797981"
---
# <a name="restore-data-from-a-backup-in-azure-cosmos-db"></a>Azure Cosmos DB bir yedekten veri geri yükleme

Veritabanınızı veya bir kapsayıcıyı yanlışlıkla silerseniz, verileri otomatik çevrimiçi yedeklemelerden geri yüklemek için [bir destek bileti dosyası](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veya [Azure desteği çağırabilirsiniz](https://azure.microsoft.com/support/options/) . Azure desteği, yalnızca **Standart**, **Geliştirici**ve planlardan daha yüksek planlar gibi seçili planlar için kullanılabilir. Azure desteği, **temel** plan ile kullanılamaz. Farklı destek planları hakkında bilgi edinmek için bkz. [Azure destek planları](https://azure.microsoft.com/support/plans/) sayfası.

Yedeklemenin belirli bir anlık görüntüsünü geri yüklemek için Azure Cosmos DB, verilerin söz konusu anlık görüntüye ait yedekleme döngüsünün süresi boyunca kullanılabilir olmasını gerektirir.

## <a name="request-a-restore"></a>Geri yükleme iste

Geri yükleme isteğinde bulunulmadan önce aşağıdaki ayrıntılara sahip olmanız gerekir:

* Abonelik KIMLIĞINIZI hazırlayın.

* Verilerinizin yanlışlıkla silinme veya değiştirilme şeklini temel alarak, ek bilgilere sahip olmak için hazırlık yapmanız gerekir. Daha iyi bir süre hassas durumda olabilecek geri ve geriye doğru bir şekilde en aza indirmek için bilgilerin ileride kullanılabilir olması önerilir.

* Azure Cosmos DB hesabının tamamı silinirse, silinen hesabın adını belirtmeniz gerekir. Silinen hesapla aynı ada sahip başka bir hesap oluşturursanız, bunu seçmek için doğru hesabı belirlemesine yardımcı olduğundan destek ekibi ile paylaşabilirsiniz. Geri yükleme durumu için karışıklıkları en aza indirecek için, silinen her bir hesap için farklı destek biletleri dosyası yapmanız önerilir.

* Bir veya daha fazla veritabanı silinirse Azure Cosmos hesabının yanı sıra Azure Cosmos veritabanı adlarını da sağlamanız ve aynı ada sahip yeni bir veritabanının mevcut olup olmadığını belirtmeniz gerekir.

* Bir veya daha fazla kapsayıcı silinirse, Azure Cosmos hesap adını, veritabanı adlarını ve kapsayıcı adlarını sağlamanız gerekir. Ve aynı ada sahip bir kapsayıcının var olup olmadığını belirtin.

* Verilerinizi yanlışlıkla silmiş veya bozdıysanız, Azure Cosmos DB ekibin verileri yedeklerden geri yüklemenize yardımcı olması için, 8 saat içinde [Azure desteği](https://azure.microsoft.com/support/options/) 'ne başvurmalısınız. **Verileri geri yüklemek için bir destek isteği oluşturmadan önce, hesabınız için [yedekleme bekletmesini](online-backup-and-restore.md) en az yedi güne artırdığınızdan emin olun. Bu olayın 8 saat içinde bekletmenin artırılması en iyisidir.** Bu şekilde Azure Cosmos DB destek ekibinin hesabınızı geri yüklemek için yeterli zamanı olacaktır.

Azure Cosmos hesap adına, veritabanı adlarına, kapsayıcı adlarına ek olarak, verilerin geri yüklenebileceği zaman noktasını belirtmeniz gerekir. Bu anda en iyi kullanılabilir yedeklemeleri belirlememize yardımcı olmak için mümkün olduğunca kesin olması önemlidir. **Saati UTC olarak belirtmek de önemlidir.**

Aşağıdaki ekran görüntüsünde, Azure portal kullanarak verileri geri yüklemek için bir kapsayıcı (koleksiyon/grafik/tablo) için bir destek isteği oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir. İsteğin önceliklendirmemize yardımcı olması için veri türü, geri yükleme amacı, verilerin silindiği zaman gibi ek ayrıntılar sağlayın.

:::image type="content" source="./media/how-to-backup-and-restore/backup-support-request-portal.png" alt-text="Azure portal kullanarak bir yedekleme destek isteği oluşturun":::

## <a name="post-restore-actions"></a>Geri yükleme sonrası eylemler

Verileri geri yükledikten sonra, yeni hesabın adı (genellikle biçimde `<original-name>-restored1` ) ve hesabın geri yüklendiği saat hakkında bir bildirim alırsınız. Geri yüklenen hesap, aynı sağlanmış işleme, dizin oluşturma ilkelerine sahip olacak ve özgün hesapla aynı bölgede. Abonelik Yöneticisi veya ortak yönetici olan bir Kullanıcı geri yüklenen hesabı görebilir.

Veriler geri yüklendikten sonra, geri yüklenen hesaptaki verileri inceleyip doğrulamanız ve beklediğiniz sürümü içerdiğinden emin olmanız gerekir. Her şey iyi görünüyorsa, [Azure Cosmos DB değişiklik akışını](change-feed.md) veya [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)kullanarak verileri özgün hesabınıza geri geçirmeniz gerekir.

Verileri geçirdikten hemen sonra kapsayıcıyı veya veritabanını silmeniz önerilir. Geri yüklenen veritabanlarını veya kapsayıcıları silmezseniz, bunlar istek birimleri, depolama ve çıkış maliyeti olur.

## <a name="next-steps"></a>Sonraki adımlar

Ardından, aşağıdaki makaleleri kullanarak verileri özgün hesabınıza geri geçirme hakkında bilgi edinebilirsiniz:

* Geri yükleme isteği oluşturmak için Azure desteğine başvurun, [Azure Portal bir bilet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) oluşturun
* Verileri Azure Cosmos DB taşımak için [Cosmos DB değişiklik akışını kullanın](change-feed.md) .

* Verileri Azure Cosmos DB taşımak için [Azure Data Factory kullanın](../data-factory/connector-azure-cosmos-db.md) .
