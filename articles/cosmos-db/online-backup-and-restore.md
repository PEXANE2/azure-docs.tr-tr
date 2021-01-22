---
title: Azure Cosmos DB çevrimiçi yedekleme ve isteğe bağlı veri yükleme
description: Bu makalede otomatik yedekleme, isteğe bağlı veri geri yükleme 'nin nasıl çalıştığı, yedekleme aralığı ve saklama 'nın nasıl yapılandırılacağı, Azure Cosmos DB ' de veri geri yükleme desteği ile nasıl iletişim kurulacağı açıklanmaktadır.
author: kanshiG
ms.service: cosmos-db
ms.topic: how-to
ms.date: 10/13/2020
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: ba66013e37c196c58291a6bcd979be7fb5fa0130
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98684585"
---
# <a name="online-backup-and-on-demand-data-restore-in-azure-cosmos-db"></a>Azure Cosmos DB çevrimiçi yedekleme ve isteğe bağlı veri yükleme
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB, düzenli aralıklarla verilerinizin yedeklerini otomatik olarak alır. Otomatik yedeklemeler, veritabanı işlemlerinin performansını veya kullanılabilirliğini etkilemeden alınır. Tüm yedeklemeler bir depolama hizmetinde ayrı olarak depolanır ve bu yedeklemeler, bölgesel felate karşı dayanıklılık açısından küresel olarak çoğaltılır. Azure Cosmos hesabınızı, veritabanınızı veya kapsayıcısını yanlışlıkla sildikten veya güncelleştirdiğinizde ve daha sonra veri kurtarmayı gerektirdiğinde, otomatik yedeklemeler senaryolarda yardımcı olur.

## <a name="automatic-and-online-backups"></a>Otomatik ve çevrimiçi yedeklemeler

Yalnızca verilerinizin değil, Azure Cosmos DB ile verilerinizin yedekleri çok fazla yedekli ve bölgesel felaketlere dayanıklı olabilir. Aşağıdaki adımlarda Azure Cosmos DB veri yedeklemesini nasıl gerçekleştirdiği gösterilmektedir:

* Azure Cosmos DB, veritabanınızın her 4 saatte bir tam yedeklemesini otomatik olarak alır, her zaman yalnızca en son iki yedek varsayılan olarak saklanır. Varsayılan aralıklar iş yükleriniz için yeterli değilse, yedekleme aralığını ve bekletme süresini Azure portal değiştirebilirsiniz. Azure Cosmos hesabı oluşturulduktan sonra veya sonrasında yedekleme yapılandırmasını değiştirebilirsiniz. Kapsayıcı veya veritabanı silinirse, Azure Cosmos DB belirli bir kapsayıcının veya veritabanının mevcut anlık görüntülerini 30 gün boyunca tutar.

* Azure Cosmos DB, bu yedeklemeleri Azure Blob depolamada depolar, ancak gerçek veriler yerel olarak Azure Cosmos DB içinde bulunur.

* Düşük gecikme süresini garantilemek için, yedeğinizin anlık görüntüsü, mevcut yazma bölgesiyle (veya çok bölgeli bir yazma yapılandırmasına sahip olmanız durumunda, yazma bölgelerinden **biri** ) aynı bölgedeki Azure Blob depolama alanında depolanır. Bölgesel olağanüstü durumlara dayanıklı olması için Azure Blob depolamadaki yedek verilerin her anlık görüntüsü coğrafi olarak yedekli depolama (GRS) aracılığıyla başka bir bölgeye yeniden çoğaltılır. Yedeklemenin çoğaltıldığı bölge, kaynak bölgenize ve kaynak bölgeyle ilişkilendirilmiş bölge çiftine bağlıdır. Daha fazla bilgi edinmek için [coğrafi olarak yedekli Azure bölgeleri çiftlerinin listesine](../best-practices-availability-paired-regions.md) bakın. Bu yedeklemeye doğrudan erişemezsiniz. Destek isteği aracılığıyla istekte bulunduğunuzda Azure Cosmos DB ekibi yedeklemenizi geri yükleyecektir.

   Aşağıdaki görüntüde, Batı ABD ' deki üç birincil fiziksel bölüm içeren bir Azure Cosmos kapsayıcısının Batı ABD ' de uzak Azure Blob depolama hesabında nasıl yedeklenebileceği ve ardından Doğu ABD ' e nasıl çoğaltıldığı gösterilmektedir:

  :::image type="content" source="./media/online-backup-and-restore/automatic-backup.png" alt-text="GRS Azure depolama alanındaki tüm Cosmos DB varlıkların düzenli aralıklarla tam yedeklemeleri" border="false":::

* Yedeklemeler, uygulamanızın performansını veya kullanılabilirliğini etkilemeden alınır. Azure Cosmos DB, ek sağlanmış üretilen iş (ru) olmadan veya veritabanınızın performansını ve kullanılabilirliğini etkilemeden arka planda veri yedekleme gerçekleştirir.

## <a name="modify-the-backup-interval-and-retention-period"></a><a id="configure-backup-interval-retention"></a>Yedekleme aralığını ve bekletme süresini değiştirme

Azure Cosmos DB her 4 saatte bir ve herhangi bir zamanda verilerinizin tam yedeklemesini otomatik olarak alır, en son iki yedek saklanır. Bu yapılandırma varsayılan seçenektir ve ek bir maliyet olmadan sunulur. Azure Cosmos hesap oluşturma sırasında veya hesap oluşturulduktan sonra varsayılan yedekleme aralığını ve saklama süresini değiştirebilirsiniz. Yedekleme yapılandırması Azure Cosmos hesabı düzeyinde ayarlanır ve bunu her hesapta yapılandırmanız gerekir. Bir hesap için yedekleme seçeneklerini yapılandırdıktan sonra, bu hesap içindeki tüm kapsayıcılara uygulanır. Şu anda yedekleme seçeneklerini yalnızca Azure portaldan değiştirebilirsiniz.

Verilerinizi yanlışlıkla silmiş veya bozdıysanız, **verileri geri yüklemek için bir destek isteği oluşturmadan önce, hesabınız için yedekleme bekletmesini en az yedi güne artırdığınızdan emin olun. Bu olayın 8 saat içinde bekletmenin artırılması en iyisidir.** Bu şekilde Azure Cosmos DB ekibinin hesabınızı geri yüklemek için yeterli zamanı olur.

Mevcut bir Azure Cosmos hesabının varsayılan yedekleme seçeneklerini değiştirmek için aşağıdaki adımları kullanın:

1. Azure portal oturum açın [.](https://portal.azure.com/)
1. Azure Cosmos hesabınıza gidin ve **yedekleme & geri yükleme** Bölmesi ' ni açın. Yedekleme aralığını ve yedekleme saklama süresini gereken şekilde güncelleştirin.

   * **Yedekleme aralığı** -Azure Cosmos DB, verilerinizin yedeğini alma girişiminde bulunan zaman aralığıdır. Yedekleme, sıfır olmayan bir süre sürer ve bazı durumlarda aşağı akış bağımlılıkları nedeniyle başarısız olabilir. Azure Cosmos DB, yapılandırılan aralıkta yedeklemeyi en iyi şekilde dener, ancak yedeklemenin bu zaman aralığında tamamlanmasını garanti etmez. Bu değeri saat veya dakika olarak yapılandırabilirsiniz. Yedekleme aralığı 1 saatten az ve 24 saatten fazla olamaz. Bu aralığı değiştirdiğinizde, yeni Aralık son yedeklemenin alındığı zamandan itibaren yürürlüğe girer.

   * **Yedekleme bekletme** -bu, her yedeklemenin saklanacağı dönemi temsil eder. Saati saat veya gün olarak yapılandırabilirsiniz. En düşük saklama süresi, yedekleme aralığının (saat cinsinden) iki katından az olamaz ve 720 saatten büyük olamaz.

   * **Korunan verilerin kopyaları** -varsayılan olarak, verilerinizin iki yedek kopyası ücretsiz olarak sunulur. İkiden fazla kopyaya ihtiyacınız varsa ek bir ücret vardır. Ek kopyaların tam fiyatını öğrenmek için [Fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/cosmos-db/) Kullanılan Depolama bölümüne bakın.

   :::image type="content" source="./media/online-backup-and-restore/configure-backup-interval-retention.png" alt-text="Mevcut bir Azure Cosmos hesabı için yedekleme aralığını ve bekletmeyi yapılandırın" border="true":::

Hesap oluşturma sırasında yedekleme seçeneklerini yapılandırırsanız, her bir **düzenli** veya **sürekli** olan **yedekleme ilkesini** yapılandırabilirsiniz. Düzenli ilke, yedekleme aralığını ve yedekleme saklama süresini yapılandırmanıza olanak tanır. Sürekli ilke şu anda yalnızca kaydolma tarafından kullanılabilir. Azure Cosmos DB ekibi, iş yükünüzü değerlendirecek ve isteğinizi onaylacaktır.

:::image type="content" source="./media/online-backup-and-restore/configure-periodic-continuous-backup-policy.png" alt-text="Yeni Azure Cosmos hesapları için dönemsel veya sürekli yedekleme ilkesini yapılandırma" border="true":::

## <a name="request-data-restore-from-a-backup"></a>Yedekten veri geri yükleme isteği

Veritabanınızı veya bir kapsayıcıyı yanlışlıkla silerseniz, verileri otomatik çevrimiçi yedeklemelerden geri yüklemek için [bir destek bileti dosyası](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veya [Azure desteği çağırabilirsiniz](https://azure.microsoft.com/support/options/) . Azure desteği, yalnızca **Standart**, **Geliştirici** ve planlardan daha yüksek planlar gibi seçili planlar için kullanılabilir. Azure desteği, **temel** plan ile kullanılamaz. Farklı destek planları hakkında bilgi edinmek için bkz. [Azure destek planları](https://azure.microsoft.com/support/plans/) sayfası.

Yedeklemenin belirli bir anlık görüntüsünü geri yüklemek için Azure Cosmos DB, verilerin söz konusu anlık görüntüye ait yedekleme döngüsünün süresi boyunca kullanılabilir olmasını gerektirir.
Geri yükleme isteğinde bulunulmadan önce aşağıdaki ayrıntılara sahip olmanız gerekir:

* Abonelik KIMLIĞINIZI hazırlayın.

* Verilerinizin yanlışlıkla silinme veya değiştirilme şeklini temel alarak, ek bilgilere sahip olmak için hazırlık yapmanız gerekir. Daha iyi bir süre hassas durumda olabilecek geri ve geriye doğru bir şekilde en aza indirmek için bilgilerin ileride kullanılabilir olması önerilir.

* Azure Cosmos DB hesabının tamamı silinirse, silinen hesabın adını belirtmeniz gerekir. Silinen hesapla aynı ada sahip başka bir hesap oluşturursanız, bunu seçmek için doğru hesabı belirlemesine yardımcı olduğundan destek ekibi ile paylaşabilirsiniz. Geri yükleme durumu için karışıklıkları en aza indirecek için, silinen her bir hesap için farklı destek biletleri dosyası yapmanız önerilir.

* Bir veya daha fazla veritabanı silinirse Azure Cosmos hesabının yanı sıra Azure Cosmos veritabanı adlarını da sağlamanız ve aynı ada sahip yeni bir veritabanının mevcut olup olmadığını belirtmeniz gerekir.

* Bir veya daha fazla kapsayıcı silinirse, Azure Cosmos hesap adını, veritabanı adlarını ve kapsayıcı adlarını sağlamanız gerekir. Ve aynı ada sahip bir kapsayıcının var olup olmadığını belirtin.

* Verilerinizi yanlışlıkla silmiş veya bozdıysanız, Azure Cosmos DB ekibin verileri yedeklerden geri yüklemenize yardımcı olması için, 8 saat içinde [Azure desteği](https://azure.microsoft.com/support/options/) 'ne başvurmalısınız. **Verileri geri yüklemek için bir destek isteği oluşturmadan önce, hesabınız için [yedekleme bekletmesini](#configure-backup-interval-retention) en az yedi güne artırdığınızdan emin olun. Bu olayın 8 saat içinde bekletmenin artırılması en iyisidir.** Bu şekilde Azure Cosmos DB destek ekibinin hesabınızı geri yüklemek için yeterli zamanı olacaktır.

Azure Cosmos hesap adına, veritabanı adlarına, kapsayıcı adlarına ek olarak, verilerin geri yüklenebileceği zaman noktasını belirtmeniz gerekir. Bu anda en iyi kullanılabilir yedeklemeleri belirlememize yardımcı olmak için mümkün olduğunca kesin olması önemlidir. **Saati UTC olarak belirtmek de önemlidir.**

Aşağıdaki ekran görüntüsünde, Azure portal kullanarak verileri geri yüklemek için bir kapsayıcı (koleksiyon/grafik/tablo) için bir destek isteği oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir. İsteğin önceliklendirmemize yardımcı olması için veri türü, geri yükleme amacı, verilerin silindiği zaman gibi ek ayrıntılar sağlayın.

:::image type="content" source="./media/online-backup-and-restore/backup-support-request-portal.png" alt-text="Azure portal kullanarak bir yedekleme destek isteği oluşturun":::

## <a name="considerations-for-restoring-the-data-from-a-backup"></a>Verileri bir yedekten geri yükleme konuları

Verilerinizi aşağıdaki senaryolardan birinde yanlışlıkla silebilir veya değiştirebilirsiniz:  

* Tüm Azure Cosmos hesabını silin.

* Bir veya daha fazla Azure Cosmos veritabanını silin.

* Bir veya daha fazla Azure Cosmos kapsayıcısı silin.

* Bir kapsayıcı içindeki Azure Cosmos öğelerini (örneğin, belgeler) silin veya değiştirin. Bu özel durum genellikle veri bozulması olarak adlandırılır.

* Paylaşılan bir teklif veritabanı içindeki paylaşılan bir teklif veritabanı veya kapsayıcılar silinir veya bozuktur.

Azure Cosmos DB yukarıdaki tüm senaryolarda verileri geri yükleyebilir. Geri yüklenirken, geri yüklenen verileri tutmak için yeni bir Azure Cosmos hesabı oluşturulur. Yeni hesabın adı belirtilmemişse, biçimi olur `<Azure_Cosmos_account_original_name>-restored1` . Birden fazla geri yükleme denendiğinde, son basamak artırılır. Önceden oluşturulmuş bir Azure Cosmos hesabına veri geri yükleme yapamazsınız.

Bir Azure Cosmos hesabını yanlışlıkla sildiğinizde, hesap adının kullanımda olmaması halinde verileri aynı ada sahip yeni bir hesaba geri yükleme işlemi yapabilirsiniz. Bu nedenle, hesabı sildikten sonra yeniden oluşturmanız önerilir. Yalnızca geri yüklenen verilerin aynı adı kullanmasına engel olmadığı için, ancak aynı zamanda geri yüklenecek doğru hesabı bulmaktan de zorlaştırıyor.

Bir Azure Cosmos veritabanını yanlışlıkla sildiğinizde, veritabanının tamamını veya bu veritabanı içindeki kapsayıcıların bir alt kümesini geri yükleyebilirsiniz. Veritabanları genelinde belirli kapsayıcıları seçmek ve bunları yeni bir Azure Cosmos hesabına geri yüklemek de mümkündür.

Bir kapsayıcı içindeki bir veya daha fazla öğeyi yanlışlıkla sildiğinizde veya değiştirdiğinizde (veri bozulması durumu), geri yükleme zamanını belirtmeniz gerekir. Veri bozulması durumunda zaman önemlidir. Kapsayıcı canlı olduğu için yedekleme hala çalışıyor, bu nedenle bekletme döneminin ötesine (varsayılan sekiz saat) kadar beklerseniz yedeklemelerin üzerine yazılır. Yedeklemenin üzerine yazılmasını engellemek için, hesabınız için yedekleme bekletmesini en az yedi güne yükseltin. Veri bozulmasından 8 saat içinde bekletmenin artırılması en iyisidir.

Verilerinizi yanlışlıkla silmiş veya bozdıysanız, Azure Cosmos DB ekibin verileri yedeklerden geri yüklemenize yardımcı olması için, 8 saat içinde [Azure desteği](https://azure.microsoft.com/support/options/) 'ne başvurmalısınız. Bu şekilde Azure Cosmos DB destek ekibinin hesabınızı geri yüklemek için yeterli zamanı olacaktır.

> [!NOTE]
> Verileri geri yükledikten sonra, tüm kaynak özellikleri veya ayarları geri yüklenen hesaba uygulanmaz. Aşağıdaki ayarlar yeni hesaba taşınmaz:
> * VNET erişim denetim listeleri
> * Saklı yordamlar, Tetikleyiciler ve Kullanıcı tanımlı işlevler
> * Çok bölgeli ayarlar  

Veritabanı düzeyinde üretilen iş sağlamak istiyorsanız, bu durumda yedekleme ve geri yükleme işlemi, tek tek kapsayıcılar düzeyinde değil, tüm veritabanı düzeyinde gerçekleşir. Bu gibi durumlarda, geri yüklenecek kapsayıcıların bir alt kümesini seçemezsiniz.

## <a name="required-permissions-to-change-retention-or-restore-from-the-portal"></a>Portaldan saklama veya geri yükleme yapmak için gerekli izinler
[Cosmosdbbackupoperator](../role-based-access-control/built-in-roles.md#cosmosbackupoperator), Owner veya katkıda bulunan rolünün bir parçası olan sorumlular geri yükleme istemesine veya saklama süresini değiştirmesine izin verilir.

## <a name="understanding-costs-of-extra-backups"></a>Ekstra yedeklemelerin maliyetlerini anlama
2 yedek sağlanır ve ek yedeklemeler, [Yedekleme Depolama fiyatlandırması](https://azure.microsoft.com/en-us/pricing/details/cosmos-db/)bölümünde açıklanan yedekleme depolaması için bölge tabanlı fiyatlandırmaya göre ücretlendirilir. Örneğin, yedekleme bekletme 240 saat, 10 gün ve yedekleme aralığı 24 saat olarak yapılandırılmışsa. Bu, yedekleme verilerinin 10 kopyasını gerektirir. Batı ABD 2 1 TB veri varsayıldığında, belirli bir ayda yedekleme depolaması için 1000 * 0,12 ~ $120 olur. 


## <a name="options-to-manage-your-own-backups"></a>Kendi yedeklemelerinizi yönetme seçenekleri

SQL API hesaplarıyla Azure Cosmos DB, aşağıdaki yaklaşımlardan birini kullanarak kendi yedeklemelerinizi de koruyabilirsiniz:

* Verileri düzenli aralıklarla seçtiğiniz bir depoya taşımak için [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) kullanın.

* Tam yedeklemeler veya artımlı değişiklikler için verileri düzenli aralıklarla okumak üzere Azure Cosmos DB [değişiklik akışını](change-feed.md) kullanın ve kendi depolama alanınızı saklayın.

## <a name="post-restore-actions"></a>Geri yükleme sonrası eylemler

Veri geri yükleme 'nin birincil hedefi, yanlışlıkla sildiğiniz veya değiştirdiğiniz verileri kurtarmaktır. Bu nedenle, önce beklediğiniz verileri içerdiğinden emin olmak için kurtarılan verilerin içeriğini incelemenizi öneririz. Her şey iyi görünüyorsa, verileri birincil hesaba geri geçirebilirsiniz. Geri yüklenen hesabı yeni etkin hesabınız olarak kullanmak mümkün olsa da, üretim iş yükleriniz varsa önerilen bir seçenek değildir. 

Verileri geri yükledikten sonra, yeni hesabın adı (genellikle biçimde `<original-name>-restored1` ) ve hesabın geri yüklendiği saat hakkında bir bildirim alırsınız. Geri yüklenen hesap, aynı sağlanmış işleme, dizin oluşturma ilkelerine sahip olacak ve özgün hesapla aynı bölgede. Abonelik Yöneticisi veya ortak yönetici olan bir Kullanıcı geri yüklenen hesabı görebilir.

### <a name="migrate-data-to-the-original-account"></a>Verileri özgün hesaba geçirme

Aşağıda, verileri özgün hesaba geri geçirmenin farklı yolları verilmiştir:

* [Azure Cosmos DB veri geçiş aracını](import-data.md)kullanın.
* [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)kullanın.
* Azure Cosmos DB [değişiklik akışını](change-feed.md) kullanın.
* Kendi özel kodunuzu yazabilirsiniz.

Verileri geçirdikten hemen sonra kapsayıcıyı veya veritabanını silmeniz önerilir. Geri yüklenen veritabanlarını veya kapsayıcıları silmezseniz, bunlar istek birimleri, depolama ve çıkış maliyeti olur.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, verileri bir Azure Cosmos hesabından geri yükleme veya Azure Cosmos hesabına nasıl veri geçirebileceğiniz hakkında bilgi edinebilirsiniz.

* Geri yükleme isteği oluşturmak için Azure desteğine başvurun, [Azure Portal bir bilet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) oluşturun
* Verileri Azure Cosmos DB taşımak için [Cosmos DB değişiklik akışını kullanın](change-feed.md) .
* Verileri Azure Cosmos DB taşımak için [Azure Data Factory kullanın](../data-factory/connector-azure-cosmos-db.md) .
