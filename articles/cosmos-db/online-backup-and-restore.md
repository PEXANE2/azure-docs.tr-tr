---
title: Azure Cosmos DB çevrimiçi yedekleme ve isteğe bağlı veri yükleme
description: Bu makalede otomatik yedekleme, isteğe bağlı veri geri yükleme işlemlerinin nasıl çalıştığı, Azure Cosmos DB ' de yedekleme aralığını ve bekletmenin nasıl yapılandırılacağı açıklanır.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/24/2020
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: f8ec215458e8ebfafb87209516f167d628e98389
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89047637"
---
# <a name="online-backup-and-on-demand-data-restore-in-azure-cosmos-db"></a>Azure Cosmos DB çevrimiçi yedekleme ve isteğe bağlı veri yükleme

Azure Cosmos DB, düzenli aralıklarla verilerinizin yedeklerini otomatik olarak alır. Otomatik yedeklemeler, veritabanı işlemlerinin performansını veya kullanılabilirliğini etkilemeden alınır. Tüm yedeklemeler bir depolama hizmetinde ayrı olarak depolanır ve bu yedeklemeler, bölgesel felate karşı dayanıklılık açısından küresel olarak çoğaltılır. Azure Cosmos hesabınızı, veritabanınızı veya kapsayıcısını yanlışlıkla sildikten veya güncelleştirdiğinizde ve daha sonra veri kurtarmayı gerektirdiğinde, otomatik yedeklemeler senaryolarda yardımcı olur.

## <a name="automatic-and-online-backups"></a>Otomatik ve çevrimiçi yedeklemeler

Yalnızca verilerinizin değil, Azure Cosmos DB ile verilerinizin yedekleri çok fazla yedekli ve bölgesel felaketlere dayanıklı olabilir. Aşağıdaki adımlarda Azure Cosmos DB veri yedeklemesini nasıl gerçekleştirdiği gösterilmektedir:

* Azure Cosmos DB, veritabanınızın bir yedeğini otomatik olarak 4 saatte bir ve istediğiniz zaman alır. varsayılan olarak yalnızca en son iki yedek saklanır. Varsayılan aralıklar iş yükleriniz için yeterli değilse, yedekleme aralığını ve bekletme süresini Azure portal değiştirebilirsiniz. Azure Cosmos hesabı oluşturulduktan sonra veya sonrasında yedekleme yapılandırmasını değiştirebilirsiniz. Kapsayıcı veya veritabanı silinirse, Azure Cosmos DB belirli bir kapsayıcının veya veritabanının mevcut anlık görüntülerini 30 gün boyunca tutar.

* Azure Cosmos DB, bu yedeklemeleri Azure Blob depolamada depolar, ancak gerçek veriler yerel olarak Azure Cosmos DB içinde bulunur.

* Düşük gecikme süresini garantilemek için, yedeğinizin anlık görüntüsü, mevcut yazma bölgesiyle aynı bölgedeki (veya çok yöneticili bir yapılandırmaya sahip olması durumunda, yazma bölgelerinden **biri** ) Azure Blob depolama alanında depolanır. Bölgesel olağanüstü duruma karşı dayanıklılık için, Azure Blob depolamada bulunan yedekleme verilerinin her bir anlık görüntüsü, coğrafi olarak yedekli depolama (GRS) aracılığıyla başka bir bölgeye çoğaltılır. Yedeklemenin çoğaltılacağı bölge, kaynak bölgeinizi ve kaynak bölgeyle ilişkili bölgesel çiftin temel alır. Daha fazla bilgi edinmek için [coğrafi olarak yedekli Azure bölgeleri çiftlerinin listesine](../best-practices-availability-paired-regions.md) bakın. Bu yedeklemeye doğrudan erişemezsiniz. Azure Cosmos DB takım, bir destek isteğiyle istek yaptığınızda yedeklemenizi geri yükler.

   Aşağıdaki görüntüde, Batı ABD ' deki üç birincil fiziksel bölüm içeren bir Azure Cosmos kapsayıcısının Batı ABD ' de uzak Azure Blob depolama hesabında nasıl yedeklenebileceği ve ardından Doğu ABD ' e nasıl çoğaltıldığı gösterilmektedir:

  :::image type="content" source="./media/online-backup-and-restore/automatic-backup.png" alt-text="GRS Azure depolama alanındaki tüm Cosmos DB varlıkların düzenli aralıklarla tam yedeklemeleri" border="false":::

* Yedeklemeler, uygulamanızın performansını veya kullanılabilirliğini etkilemeden alınır. Azure Cosmos DB, ek sağlanmış üretilen iş (ru) olmadan veya veritabanınızın performansını ve kullanılabilirliğini etkilemeden arka planda veri yedekleme gerçekleştirir.

## <a name="options-to-manage-your-own-backups"></a>Kendi yedeklemelerinizi yönetme seçenekleri

SQL API hesaplarıyla Azure Cosmos DB, aşağıdaki yaklaşımlardan birini kullanarak kendi yedeklemelerinizi de koruyabilirsiniz:

* Verileri düzenli aralıklarla seçtiğiniz bir depoya taşımak için [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) kullanın.

* Tam yedeklemeler veya artımlı değişiklikler için verileri düzenli aralıklarla okumak üzere Azure Cosmos DB [değişiklik akışını](change-feed.md) kullanın ve kendi depolama alanınızı saklayın.

## <a name="modify-the-backup-interval-and-retention-period"></a>Yedekleme aralığını ve bekletme süresini değiştirme

Azure Cosmos DB her 4 saatte bir ve herhangi bir zamanda verilerinizin bir yedeğini otomatik olarak alır, en son iki yedek saklanır. Bu yapılandırma varsayılan seçenektir ve ek bir maliyet olmadan sunulur. Azure Cosmos hesap oluşturma sırasında veya hesap oluşturulduktan sonra varsayılan yedekleme aralığını ve saklama süresini değiştirebilirsiniz. Yedekleme yapılandırması Azure Cosmos hesap düzeyinde ayarlanır ve her hesapta yapılandırmanız gerekir. Bir hesap için yedekleme seçeneklerini yapılandırdıktan sonra, bu hesap içindeki tüm kapsayıcılara uygulanır. Şu anda bu yedekleme seçeneklerini yalnızca Azure portal ' dan değiştirebilirsiniz.

Verilerinizi yanlışlıkla silmiş veya bozdıysanız, **verileri geri yüklemek için bir destek isteği oluşturmadan önce, hesabınız için yedekleme bekletmesini en az yedi güne artırdığınızdan emin olun. Bu olayın 8 saat içinde bekletmenin artırılması en iyisidir.** Bu şekilde, Azure Cosmos DB ekibin hesabınızı geri yüklemek için yeterli zaman vardır.

Mevcut bir Azure Cosmos hesabının varsayılan yedekleme seçeneklerini değiştirmek için aşağıdaki adımları kullanın:

1. Azure portal oturum açın [.](https://portal.azure.com/)
1. Azure Cosmos hesabınıza gidin ve **yedekleme & geri yükleme** Bölmesi ' ni açın. Yedekleme aralığını ve yedekleme saklama süresini gereken şekilde güncelleştirin.

   * **Yedekleme aralığı** -Azure Cosmos DB, verilerinizin yedeğini alma girişiminde bulunan zaman aralığıdır. Yedekleme, sıfır olmayan bir süre sürer ve bazı durumlarda aşağı akış bağımlılıkları nedeniyle başarısız olabilir. Azure Cosmos DB, yapılandırılan aralıkta yedeklemeyi en iyi şekilde dener, ancak yedeklemenin bu zaman aralığında tamamlanmasını garanti etmez. Bu değeri saat veya dakika olarak yapılandırabilirsiniz. Yedekleme aralığı 1 saatten az ve 24 saatten fazla olamaz. Bu aralığı değiştirdiğinizde, yeni Aralık son yedeklemenin alındığı zamandan itibaren yürürlüğe girer.

   * **Yedekleme bekletme** -bu, her yedeklemenin saklanacağı dönemi temsil eder. Saati saat veya gün olarak yapılandırabilirsiniz. En düşük saklama süresi, yedekleme aralığının (saat cinsinden) iki katından az olamaz ve 720 saatten büyük olamaz.

   * **Korunan verilerin kopyaları** -varsayılan olarak, verilerinizin iki yedek kopyası ücretsiz olarak sunulur. İkiden fazla kopyaya ihtiyacınız varsa ek bir ücret vardır. Ek kopyaların tam fiyatını öğrenmek için [fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/cosmos-db/) tüketilen depolama bölümüne bakın.

   :::image type="content" source="./media/online-backup-and-restore/configure-backup-interval-retention.png" alt-text="Mevcut bir Azure Cosmos hesabı için yedekleme aralığını ve bekletmeyi yapılandırın" border="true":::

Hesap oluşturma sırasında yedekleme seçeneklerini yapılandırırsanız, her bir **düzenli** veya **sürekli**olan **yedekleme ilkesini**yapılandırabilirsiniz. Düzenli ilke, yedekleme aralığını ve yedekleme saklama süresini yapılandırmanıza olanak tanır. Sürekli ilke şu anda yalnızca kaydolma tarafından kullanılabilir. Azure Cosmos DB ekibi, iş yükünüzü değerlendirecek ve isteğinizi onaylacaktır.

:::image type="content" source="./media/online-backup-and-restore/configure-periodic-continuous-backup-policy.png" alt-text="Yeni Azure Cosmos hesapları için dönemsel veya sürekli yedekleme ilkesini yapılandırma" border="true":::

## <a name="restore-data-from-an-online-backup"></a>Çevrimiçi bir yedeklemeden verileri geri yükleme

Verilerinizi aşağıdaki senaryolardan birinde yanlışlıkla silebilir veya değiştirebilirsiniz:  

* Tüm Azure Cosmos hesabını silin.

* Bir veya daha fazla Azure Cosmos veritabanını silin.

* Bir veya daha fazla Azure Cosmos kapsayıcısı silin.

* Bir kapsayıcı içindeki Azure Cosmos öğelerini (örneğin, belgeler) silin veya değiştirin. Bu özel durum genellikle veri bozulması olarak adlandırılır.

* Paylaşılan bir teklif veritabanı içindeki paylaşılan bir teklif veritabanı veya kapsayıcılar silinir veya bozuktur.

Azure Cosmos DB yukarıdaki tüm senaryolarda verileri geri yükleyebilir. Geri yüklenirken, geri yüklenen verileri tutmak için yeni bir Azure Cosmos hesabı oluşturulur. Yeni hesabın adı belirtilmemişse, biçimi olur `<Azure_Cosmos_account_original_name>-restored1` . Birden fazla geri yükleme denendiğinde, son basamak artırılır. Önceden oluşturulmuş bir Azure Cosmos hesabına veri geri yükleme yapamazsınız.

Bir Azure Cosmos hesabını yanlışlıkla sildiğinizde, hesap adının kullanımda olmaması halinde verileri aynı ada sahip yeni bir hesaba geri yükleme işlemi yapabilirsiniz. Bu nedenle, hesabı sildikten sonra yeniden oluşturmanız önerilir. Yalnızca geri yüklenen verilerin aynı adı kullanmasına engel olmadığı için, ancak aynı zamanda geri yüklenecek doğru hesabı bulmaktan de zorlaştırıyor.

Bir Azure Cosmos veritabanını yanlışlıkla sildiğinizde, veritabanının tamamını veya bu veritabanı içindeki kapsayıcıların bir alt kümesini geri yükleyebilirsiniz. Veritabanları genelinde belirli kapsayıcıları seçmek ve bunları yeni bir Azure Cosmos hesabına geri yüklemek de mümkündür.

Bir kapsayıcı içindeki bir veya daha fazla öğeyi yanlışlıkla sildiğinizde veya değiştirdiğinizde (veri bozulması durumu), geri yükleme zamanını belirtmeniz gerekir. Veri bozulması durumunda zaman önemlidir. Kapsayıcı canlı olduğu için yedekleme hala çalışıyor, bu nedenle bekletme döneminin ötesine (varsayılan sekiz saat) kadar beklerseniz yedeklemelerin üzerine yazılır. **Yedeklemenin üzerine yazılmasını engellemek için, hesabınız için yedekleme bekletmesini en az yedi güne yükseltin. Veri bozulmasından 8 saat içinde bekletmenin artırılması en iyisidir.**

Verilerinizi yanlışlıkla silmiş veya bozdıysanız, Azure Cosmos DB ekibin verileri yedeklerden geri yüklemenize yardımcı olması için, 8 saat içinde [Azure desteği](https://azure.microsoft.com/support/options/) 'ne başvurmalısınız. Bu şekilde Azure Cosmos DB destek ekibinin hesabınızı geri yüklemek için yeterli zamanı olacaktır.

Veritabanı düzeyinde üretilen iş sağlamak istiyorsanız, bu durumda yedekleme ve geri yükleme işlemi, tek tek kapsayıcılar düzeyinde değil, tüm veritabanı düzeyinde gerçekleşir. Bu gibi durumlarda, geri yüklenecek kapsayıcıların bir alt kümesini seçemezsiniz.

## <a name="migrate-data-to-the-original-account"></a>Verileri özgün hesaba geçirme

Veri geri yükleme 'nin birincil hedefi, yanlışlıkla sildiğiniz veya değiştirdiğiniz verileri kurtarmaktır. Bu nedenle, önce beklediğiniz verileri içerdiğinden emin olmak için kurtarılan verilerin içeriğini incelemenizi öneririz. Daha sonra verileri birincil hesaba geri geçirebilirsiniz. Geri yüklenen hesabı yeni etkin hesabınız olarak kullanmak mümkün olsa da, üretim iş yükleriniz varsa önerilen bir seçenek değildir.  

Aşağıda, verileri özgün Azure Cosmos hesabına geri geçirmenin farklı yolları verilmiştir:

* [Azure Cosmos DB veri geçiş aracını](import-data.md)kullanın.
* [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)kullanın.
* Azure Cosmos DB [değişiklik akışını](change-feed.md) kullanın.
* Kendi özel kodunuzu yazabilirsiniz.

Geri yüklenen hesapları, verilerinizi geçirdiğiniz anda sildikten sonra, devam eden ücretler oluşturdıklarından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, verileri bir Azure Cosmos hesabından geri yükleme veya Azure Cosmos hesabına nasıl veri geçirebileceğiniz hakkında bilgi edinebilirsiniz.

* Geri yükleme isteği oluşturmak için Azure desteğine başvurun, [Azure Portal bir bilet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) oluşturun
* [Azure Cosmos hesabından verileri geri yükleme](how-to-backup-and-restore.md)
* Verileri Azure Cosmos DB taşımak için [Cosmos DB değişiklik akışını kullanın](change-feed.md) .
* Verileri Azure Cosmos DB taşımak için [Azure Data Factory kullanın](../data-factory/connector-azure-cosmos-db.md) .

