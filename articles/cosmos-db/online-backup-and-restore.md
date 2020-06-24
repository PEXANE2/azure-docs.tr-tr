---
title: Azure Cosmos DB çevrimiçi yedekleme ve isteğe bağlı veri yükleme
description: Bu makalede, Azure Cosmos DB ' de otomatik, çevrimiçi yedekleme ve isteğe bağlı veri geri yükleme işlemlerinin nasıl çalıştığı açıklanır.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 8ed9e23b178b8eeefbd3c3a690491124e6901180
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2020
ms.locfileid: "85112931"
---
# <a name="online-backup-and-on-demand-data-restore-in-azure-cosmos-db"></a>Azure Cosmos DB çevrimiçi yedekleme ve isteğe bağlı veri yükleme

Azure Cosmos DB, düzenli aralıklarla verilerinizin yedeklerini otomatik olarak alır. Otomatik yedeklemeler, veritabanı işlemlerinin performansını veya kullanılabilirliğini etkilemeden alınır. Tüm yedeklemeler bir depolama hizmetinde ayrı olarak depolanır ve bu yedeklemeler, bölgesel felate karşı dayanıklılık açısından küresel olarak çoğaltılır. Azure Cosmos hesabınızı, veritabanınızı veya kapsayıcısını yanlışlıkla sildikten veya güncelleştirdiğinizde ve daha sonra veri kurtarmayı gerektirdiğinde, otomatik yedeklemeler senaryolarda yardımcı olur.

## <a name="automatic-and-online-backups"></a>Otomatik ve çevrimiçi yedeklemeler

Yalnızca verilerinizin değil, Azure Cosmos DB ile verilerinizin yedekleri çok fazla yedekli ve bölgesel felaketlere dayanıklı olabilir. Aşağıdaki adımlarda Azure Cosmos DB veri yedeklemesini nasıl gerçekleştirdiği gösterilmektedir:

* Azure Cosmos DB her 4 saatte bir otomatik olarak veritabanınızın yedeğini alır ve herhangi bir anda yalnızca 2 yedekleme depolanır. Bununla birlikte kapsayıcı veya veritabanı silinirse Azure Cosmos DB bu kapsayıcı veya veritabanının mevcut anlık görüntülerini 30 gün boyunca saklar.

* Azure Cosmos DB, bu yedeklemeleri Azure Blob depolamada depolar, ancak gerçek veriler yerel olarak Azure Cosmos DB içinde bulunur.

*  Düşük gecikme süresini garantilemek için, yedeğinizin anlık görüntüsü Azure Blob depolama alanında geçerli yazma bölgesiyle (veya çok yöneticili bir yapılandırmaya sahip olması durumunda, yazma bölgelerinden biri) Azure Cosmos veritabanı hesabınızın bulunduğu bölgede depolanır. Bölgesel olağanüstü duruma karşı dayanıklılık için, Azure Blob depolamada bulunan yedekleme verilerinin her bir anlık görüntüsü, coğrafi olarak yedekli depolama (GRS) aracılığıyla başka bir bölgeye çoğaltılır. Yedeklemenin çoğaltılacağı bölge, kaynak bölgeinizi ve kaynak bölgeyle ilişkili bölgesel çiftin temel alır. Daha fazla bilgi edinmek için [coğrafi olarak yedekli Azure bölgeleri çiftlerinin listesine](../best-practices-availability-paired-regions.md) bakın. Bu yedeklemeye doğrudan erişemezsiniz. Azure Cosmos DB, bu yedeklemeyi yalnızca bir yedekleme geri yüklemesi başlatılmışsa kullanacaktır.

* Yedeklemeler, uygulamanızın performansını veya kullanılabilirliğini etkilemeden alınır. Azure Cosmos DB, ek sağlanmış üretilen iş (ru) olmadan veya veritabanınızın performansını ve kullanılabilirliğini etkilemeden arka planda veri yedekleme gerçekleştirir.

* Verilerinizi yanlışlıkla silmiş veya bozdıysanız, Azure Cosmos DB ekibin verileri yedeklerden geri yüklemenize yardımcı olması için, 8 saat içinde [Azure desteği](https://azure.microsoft.com/support/options/) 'ne başvurmalısınız.

Aşağıdaki görüntüde, Batı ABD ' deki üç birincil fiziksel bölüm içeren bir Azure Cosmos kapsayıcısının Batı ABD ' de uzak Azure Blob depolama hesabında nasıl yedeklenebileceği ve ardından Doğu ABD ' e nasıl çoğaltıldığı gösterilmektedir:

:::image type="content" source="./media/online-backup-and-restore/automatic-backup.png" alt-text="GRS Azure depolama alanındaki tüm Cosmos DB varlıkların düzenli aralıklarla tam yedeklemeleri" border="false":::

## <a name="options-to-manage-your-own-backups"></a>Kendi yedeklemelerinizi yönetme seçenekleri

SQL API hesaplarıyla Azure Cosmos DB, aşağıdaki yaklaşımlardan birini kullanarak kendi yedeklemelerinizi de koruyabilirsiniz:

* Verileri düzenli aralıklarla seçtiğiniz bir depoya taşımak için [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) kullanın.

* Tam yedeklemeler için verileri düzenli aralıklarla okumak için Azure Cosmos DB [değişiklik akışını](change-feed.md) , artımlı değişiklikler için de kullanın ve kendi depolama ortamınızda saklayın.

## <a name="backup-retention-period"></a>Yedekleme bekletme süresi

Azure Cosmos DB, verilerinizin her dört saatte bir anlık görüntüsünü alır. Belirli bir zamanda yalnızca son iki anlık görüntü tutulur. Bununla birlikte kapsayıcı veya veritabanı silinirse Azure Cosmos DB bu kapsayıcı veya veritabanının mevcut anlık görüntülerini 30 gün boyunca saklar.

## <a name="restoring-data-from-online-backups"></a>Çevrimiçi yedeklemelerdeki verileri geri yükleme

Verileri yanlışlıkla silme veya değiştirme, aşağıdaki senaryolardan birinde oluşabilir:  

* Tüm Azure Cosmos hesabı silindi

* Bir veya daha fazla Azure Cosmos veritabanı silindi

* Bir veya daha fazla Azure Cosmos kapsayıcısı silindi

* Bir kapsayıcı içindeki Azure Cosmos öğeleri (örneğin, belgeler) silinir veya değiştirilir. Bu özel durum genellikle "veri bozulması" olarak adlandırılır.

* Paylaşılan bir teklif veritabanı içindeki paylaşılan bir teklif veritabanı veya kapsayıcılar silinir veya bozuktur

Azure Cosmos DB yukarıdaki tüm senaryolarda verileri geri yükleyebilir. Geri yükleme işlemi, geri yüklenen verileri barındırmak için her zaman yeni bir Azure Cosmos hesabı oluşturur. Yeni hesabın adı belirtilmezse, biçimi olur `<Azure_Cosmos_account_original_name>-restored1` . Birden fazla geri yükleme denendiğinde, son basamak artırılır. Önceden oluşturulmuş bir Azure Cosmos hesabına veri geri yükleme yapamazsınız.

Bir Azure Cosmos hesabı silindiğinde, hesap adının kullanımda olmaması kaydıyla, verileri aynı ada sahip bir hesaba geri yükleme işlemi yapılabilir. Bu gibi durumlarda, yalnızca geri yüklenen verilerin aynı adı kullanmasına engel olmadığından ve ayrıca geri yüklenecek doğru hesabı daha zor bir şekilde keşfettiğinden, silme işleminden sonra hesabı yeniden oluşturmanız önerilir. 

Bir Azure Cosmos veritabanı silindiğinde, veritabanının tamamını veya bu veritabanı içindeki kapsayıcıların bir alt kümesini geri yüklemek mümkündür. Veritabanları genelinde kapsayıcı seçmek ve bunları geri yüklemek de mümkündür ve geri yüklenen tüm veriler yeni bir Azure Cosmos hesabına yerleştirilir.

Bir kapsayıcı içindeki bir veya daha fazla öğe yanlışlıkla silindiğinde veya değiştirildiğinde (veri bozulması durumu), geri yükleme zamanını belirtmeniz gerekecektir. Bu durum için saat özbir değer. Kapsayıcı canlı olduğundan, yedekleme hala çalışıyor, bu nedenle bekletme döneminin ötesine (varsayılan sekiz saat) kadar beklerseniz yedeklemelerin üzerine yazılır. Silme durumunda verileriniz artık yedekleme döngüsüyle üzerine yazılmayacağından depolanmaz. Silinen veritabanları veya kapsayıcılar için yedeklemeler 30 gün boyunca kaydedilir.

Veritabanı düzeyinde aktarım hızı sağlarsanız (yani, bir kapsayıcı kümesi sağlanan aktarım hızını paylaşıyorsa), bu durumda yedekleme ve geri yükleme işlemi tüm veritabanı düzeyinde gerçekleşir ve tek tek kapsayıcılar düzeyinde değildir. Bu gibi durumlarda, geri yüklenecek kapsayıcıların bir alt kümesini seçmek bir seçenek değildir.

## <a name="migrating-data-to-the-original-account"></a>Verileri özgün hesaba geçirme

Veri geri yükleme 'nin birincil amacı, yanlışlıkla sildiğiniz veya değiştirdiğiniz verileri kurtarmak için bir yol sağlamaktır. Bu nedenle, önce beklediğiniz verileri içerdiğinden emin olmak için kurtarılan verilerin içeriğini incelemenizi öneririz. Ardından verileri birincil hesaba geri geçirmeye çalışın. Geri yüklenen hesabı canlı hesap olarak kullanmak mümkün olsa da, üretim iş yükleriniz varsa önerilen bir seçenek değildir.  

Aşağıda, verileri özgün Azure Cosmos hesabına geri geçirmenin farklı yolları verilmiştir:

* [Cosmos DB veri geçiş aracını](import-data.md) kullanma
* [Azure Data Factory]( ../data-factory/connector-azure-cosmos-db.md) kullanma
* Azure Cosmos DB [değişiklik akışını](change-feed.md) kullanma 
* Özel kod yazma

Geri yüklenen hesapları, geçiş işlemi tamamlandıktan hemen sonra silin, çünkü bunlar devam eden ücretlere tabi olur.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, verileri bir Azure Cosmos hesabından geri yükleme veya Azure Cosmos hesabına nasıl veri geçirebileceğiniz hakkında bilgi edinebilirsiniz.

* Geri yükleme isteği oluşturmak için Azure desteğine başvurun, [Azure Portal bir bilet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) oluşturun
* [Azure Cosmos hesabından verileri geri yükleme](how-to-backup-and-restore.md)
* Verileri Azure Cosmos DB taşımak için [Cosmos DB değişiklik akışını kullanın](change-feed.md) .
* Verileri Azure Cosmos DB taşımak için [Azure Data Factory kullanın](../data-factory/connector-azure-cosmos-db.md) .

