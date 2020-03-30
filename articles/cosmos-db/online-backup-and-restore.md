---
title: Azure Cosmos DB'de çevrimiçi yedekleme ve isteğe bağlı veri geri yüklemesi
description: Bu makalede, Azure Cosmos DB'de otomatik, çevrimiçi yedekleme ve isteğe bağlı veri geri yüklemenin nasıl çalıştığı açıklanmaktadır.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: f5cc4339d7d6dce6d49c8d3eb744fca7fa5774d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240428"
---
# <a name="online-backup-and-on-demand-data-restore-in-azure-cosmos-db"></a>Azure Cosmos DB'de çevrimiçi yedekleme ve isteğe bağlı veri geri yüklemesi

Azure Cosmos DB, verilerinizin yedeklerini düzenli aralıklarla otomatik olarak alır. Otomatik yedeklemeler, veritabanı işlemlerinin performansını veya kullanılabilirliğini etkilemeden alınır. Tüm yedeklemeler bir depolama hizmetinde ayrı olarak depolanır ve bu yedeklemeler bölgesel felaketlere karşı dayanıklılık için küresel olarak çoğaltılır. Otomatik yedeklemeler, Azure Cosmos hesabınızı, veritabanınızı veya kapsayıcınızı yanlışlıkla sildiğinizde veya güncellediğinizde ve daha sonra veri kurtarmayı gerektiren senaryolarda yararlıdır.

## <a name="automatic-and-online-backups"></a>Otomatik ve çevrimiçi yedeklemeler

Azure Cosmos DB ile yalnızca verileriniz değil, verilerinizin yedekleri de son derece gereksiz dir ve bölgesel felaketlere karşı dayanıklıdır. Aşağıdaki adımlar Azure Cosmos DB'nin veri yedeklemeyi nasıl gerçekleştirdiğini gösterir:

* Azure Cosmos DB her 4 saatte bir otomatik olarak veritabanınızın yedeğini alır ve herhangi bir anda yalnızca 2 yedekleme depolanır. Bununla birlikte kapsayıcı veya veritabanı silinirse Azure Cosmos DB bu kapsayıcı veya veritabanının mevcut anlık görüntülerini 30 gün boyunca saklar.

* Azure Cosmos DB bu yedeklemeleri Azure Blob depolama alanında saklarken, gerçek veriler Azure Cosmos DB içinde yerel olarak bulunur.

*  Düşük gecikme süresini garanti etmek için, yedeklemenizin anlık görüntüsü Azure Cosmos veritabanı hesabınızın geçerli yazma bölgesiyle (veya çok yapılı yapılandırmanız olması durumunda yazma bölgelerinden biriyle) aynı bölgede Azure Blob depolama alanında depolanır. Bölgesel felaketlere karşı dayanıklılık için, Azure Blob depolamasındaki yedekleme verilerinin her anlık görüntüsü, coğrafi yedek depolama (GRS) aracılığıyla yeniden başka bir bölgeye çoğaltılır. Yedeklemenin çoğaltıldığı bölge, kaynak bölgenize ve kaynak bölgeyle ilişkili bölgesel çifti temel almaktadır. Daha fazla bilgi edinmek için, [Azure bölgeleri makalesinin coğrafi yedekli çiftleri listesine](../best-practices-availability-paired-regions.md) bakın. Bu yedeklemeye doğrudan erişemezsiniz. Azure Cosmos DB bu yedeklemeyi yalnızca yedekleme geri yüklemesi başlatılırsa kullanır.

* Yedeklemeler, uygulamanızın performansını veya kullanılabilirliğini etkilemeden alınır. Azure Cosmos DB, ek bir veri veri (RUs) tüketmeden veya veritabanınızın performansını ve kullanılabilirliğini etkilemeden arka planda veri yedekleme gerçekleştirir.

* Verilerinizi yanlışlıkla sildiyseniz veya bozduysanız, Azure Cosmos DB ekibinin yedeklemelerden verileri geri yüklemenize yardımcı olması için 8 saat içinde [Azure desteğine](https://azure.microsoft.com/support/options/) başvurmalısınız.

Aşağıdaki resim, Batı ABD'deki üç ana fiziksel bölüme sahip bir Azure Cosmos kapsayıcısının Batı ABD'deki uzak bir Azure Blob Depolama hesabında nasıl yedeklenip Doğu ABD'ye çoğaltıldığını gösterir:

![GRS Azure Depolama'daki tüm Cosmos DB varlıklarının periyodik tam yedekleri](./media/online-backup-and-restore/automatic-backup.png)

## <a name="options-to-manage-your-own-backups"></a>Kendi yedeklemelerinizi yönetme seçenekleri

Azure Cosmos DB SQL API hesapları ile aşağıdaki yaklaşımlardan birini kullanarak kendi yedeklemelerinizi de koruyabilirsiniz:

* Verileri periyodik olarak seçtiğiniz bir depolama alanına taşımak için [Azure Veri Fabrikası'nı](../data-factory/connector-azure-cosmos-db.md) kullanın.

* Verileri tam yedeklemelerin yanı sıra artımlı değişiklikler için düzenli aralıklarla okumak ve kendi depolama alanınızda depolamak için Azure Cosmos DB [değiştirme akışını](change-feed.md) kullanın.

## <a name="backup-retention-period"></a>Yedekleme bekletme süresi

Azure Cosmos DB, verilerinizin anlık görüntülerini her dört saatte bir alır. Herhangi bir zamanda, yalnızca son iki anlık görüntü korunur. Bununla birlikte kapsayıcı veya veritabanı silinirse Azure Cosmos DB bu kapsayıcı veya veritabanının mevcut anlık görüntülerini 30 gün boyunca saklar.

## <a name="restoring-data-from-online-backups"></a>Çevrimiçi yedeklemelerden gelen verileri geri alma

Verilerin yanlışlıkla silinmesi veya değiştirilmesi aşağıdaki senaryolardan birinde gerçekleşebilir:  

* Azure Cosmos hesabının tamamı silinir

* Bir veya daha fazla Azure Cosmos veritabanı silinir

* Bir veya daha fazla Azure Cosmos kapsayıcısı silinir

* Bir kapsayıcıdaki Azure Cosmos öğeleri (örneğin, belgeler) silinir veya değiştirilir. Bu özel durum genellikle "veri bozulması" olarak adlandırılır.

* Paylaşılan teklif veritabanı veya paylaşılan teklif veritabanı içindeki kapsayıcılar silinir veya bozulur

Azure Cosmos DB, yukarıdaki senaryoların tümlerinde verileri geri yükleyebilir. Geri yükleme işlemi, geri yüklenen verileri barındırmak için her zaman yeni bir Azure Cosmos hesabı oluşturur. Yeni hesabın adı belirtilmemişse, biçime `<Azure_Cosmos_account_original_name>-restored1`sahip olacaktır. Birden çok geri yükleme denenmişse, son basamak artımlı. Verileri önceden oluşturulmuş bir Azure Cosmos hesabına geri yükleyemezsiniz.

Bir Azure Cosmos hesabı silindiğinde, hesap adının kullanılmaması koşuluyla verileri aynı ada sahip bir hesaba geri yükleyebiliriz. Bu gibi durumlarda, yalnızca geri yüklenen verilerin aynı adı kullanmasını engellemekle kalmaması, aynı zamanda daha zor bir şekilde geri yüklemek için doğru hesabı keşfetmeyi de zorlaştırdığından, silindikten sonra hesabı yeniden oluşturmamaönerilir. 

Bir Azure Cosmos veritabanı silindiğinde, veritabanının tamamını veya bu veritabanı içindeki kapsayıcıların bir alt kümesini geri yüklemek mümkündür. Veritabanları arasında kapsayıcıları seçmek ve bunları geri yüklemek de mümkündür ve geri yüklenen tüm veriler yeni bir Azure Cosmos hesabına yerleştirilir.

Bir kapsayıcıdaki bir veya daha fazla öğe yanlışlıkla silindiğinde veya değiştirildiğinde (veri bozulması durumu), geri yükleme nin saatini belirtmeniz gerekir. Bu dava için zaman çok önemli. Kapsayıcı canlı olduğundan, yedekleme hala çalışıyor, bu nedenle bekletme süresinin ötesine beklerseniz (varsayılan değer sekiz saattir) yedeklemeler üzerine yazılır. Silme durumunda, verileriniz artık depolanmıyor, çünkü yedekleme döngüsü tarafından üzerine yazılmayacaklar. Silinen veritabanları veya kapsayıcılar için yedeklemeler 30 gün boyunca kaydedilir.

Veri tabanı düzeyinde iş elde etme işlemi sağlarsanız (diğer bir deyişle, bir kapsayıcı kümesinin sağlanan iş girdisini paylaştığı durumlarda), bu durumda yedekleme ve geri yükleme işlemi tek tek kapsayıcılar düzeyinde değil, tüm veritabanı düzeyinde gerçekleşir. Bu gibi durumlarda, geri yüklemek için kapsayıcıların bir alt kümesini seçmek bir seçenek değildir.

## <a name="migrating-data-to-the-original-account"></a>Verileri özgün hesaba geçirme

Veri geri yüklemesinin birincil amacı, yanlışlıkla sildiğiniz veya değiştirdiğiniz verileri kurtarmak için bir yol sağlamaktır. Bu nedenle, öncelikle kurtarılan verilerin içeriğini inceleyerek beklediğiniz verileri içerdiğinden emin olmamızı öneririz. Ardından, verileri birincil hesaba geri geçirme üzerinde çalışın. Geri yüklenen hesabı canlı hesap olarak kullanmak mümkün olsa da, üretim iş yükleri varsa önerilen bir seçenek değildir.  

Verileri özgün Azure Cosmos hesabına geçirmenin farklı yolları şunlardır:

* [Cosmos DB Veri Geçiş Aracını](import-data.md) Kullanma
* [Azure Veri Fabrikasını]( ../data-factory/connector-azure-cosmos-db.md) Kullanma
* Azure Cosmos DB'de [değişiklik akışını](change-feed.md) kullanma 
* Özel kod yazma

Geri yüklenen hesapları, devam eden ücretlere maruz kalacaklarından, geçiş ilerler bitmez silin.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra bir Azure Cosmos hesabından verileri nasıl geri yükleyebileceğinizi veya verileri azure cosmos hesabına nasıl geçirebileceğinizi öğrenebilirsiniz

* Geri yükleme isteği nde bulunmak için Azure Desteği'ne başvurun ve [Azure portalından bilet gönderin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
* [Azure Cosmos hesabındaki verileri geri yükleme](how-to-backup-and-restore.md)
* Verileri Azure Cosmos DB'ye taşımak için [Cosmos DB değiştirme akışını kullanın.](change-feed.md)
* Verileri Azure Cosmos DB'ye taşımak için [Azure Veri Fabrikası'nı kullanın.](../data-factory/connector-azure-cosmos-db.md)

