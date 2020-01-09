---
title: Azure Stream Analytics aramalar için başvuru verilerini kullanma
description: Bu makalede, bir Azure Stream Analytics işinin sorgu tasarımında verileri aramak veya ilişkilendirmek için başvuru verilerinin nasıl kullanılacağı açıklanır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/8/2019
ms.openlocfilehash: b3808524706b13761dd8eccffa301c602d08f481
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75369573"
---
# <a name="using-reference-data-for-lookups-in-stream-analytics"></a>Stream Analytics aramalar için başvuru verilerini kullanma

Başvuru verileri (arama tablosu olarak da bilinir), bir arama gerçekleştirmek veya veri akışlarınızı genişletmek için kullanılan, statik veya yavaş değişen, sınırlı bir veri kümesidir. Örneğin, bir IoT senaryosunda, sensörlerle ilgili meta verileri (sık değişmeyin) başvuru verilerinde saklayabilir ve gerçek zamanlı IoT veri akışları ile birleştirebilirsiniz. Azure Stream Analytics düşük gecikmeli akış işleme elde etmek için belleğe başvuru verilerini yükler. Azure Stream Analytics işinizdeki başvuru verilerini kullanmak için, genellikle sorgunuzda bir [başvuru verisi katılımı](https://docs.microsoft.com/stream-analytics-query/reference-data-join-azure-stream-analytics) kullanacaksınız. 

Stream Analytics, Azure Blob depolama ve Azure SQL veritabanını başvuru verileri için depolama katmanı olarak destekler. Ayrıca, [bulut tabanlı ve şirket içi veri mağazalarını](../data-factory/copy-activity-overview.md)kullanmak için Azure Data Factory ' den BLOB depolama alanına başvuru verileri dönüştürebilir ve/veya kopyalayabilirsiniz.

## <a name="azure-blob-storage"></a>Azure Blob depolama

Başvuru verileri, blob adında belirtilen tarih/saatin artan sırada bir blob dizisi (giriş yapılandırmasında tanımlanır) olarak modellenir. **Yalnızca** dizideki son blob tarafından belirtilenden **büyük** bir tarih/saat kullanarak sıranın sonuna eklenmesini destekler.

### <a name="configure-blob-reference-data"></a>Blob başvuru verilerini yapılandırma

Başvuru verilerinizi yapılandırmak için önce **başvuru verileri**türünde bir giriş oluşturmanız gerekir. Aşağıdaki tabloda, başvuru veri girişini açıklaması ile oluştururken sağlamanız gereken her özellik açıklanmaktadır:

|**Özellik adı**  |**Açıklama**  |
|---------|---------|
|Girdi Diğer Adı   | Bu girişe başvurmak için iş sorgusunda kullanılacak kolay bir ad.   |
|Depolama Hesabı   | Bloblarınızın bulunduğu depolama hesabının adı. Stream Analytics Işiniz ile aynı abonelikte yer alıyorsa, açılır listeden seçim yapabilirsiniz.   |
|Depolama Hesabı Anahtarı   | Depolama hesabı ile ilişkili gizli anahtar. Depolama hesabı Stream Analytics işle aynı abonelikte ise bu otomatik olarak doldurulur.   |
|Depolama kapsayıcısı   | Kapsayıcıları Microsoft Azure Blob hizmetinde depolanan bloblar için mantıksal bir gruplandırmasını sağlar. Blob hizmeti için bir blob karşıya yüklediğinizde, bu blob kapsayıcısı belirtmeniz gerekir.   |
|Yol Deseni   | Belirtilen kapsayıcı içindeki bloblarınızı bulmak için kullanılan yol. Yol içinde, aşağıdaki 2 değişkenlerin bir veya daha fazla örneğini belirtmeyi seçebilirsiniz:<BR>{date} {time}<BR>Örnek 1: Ürünler/{Date}/{Time}/Product-List. csv<BR>Örnek 2: Ürünler/{Date}/Product-List. csv<BR>Örnek 3: Product-List. csv<BR><br> Blob belirtilen yolda yoksa, blob 'un kullanılabilir olması için Stream Analytics işi süresiz olarak bekler.   |
|Tarih biçimi [isteğe bağlı]   | Belirttiğiniz yol deseninin içinde {Date} kullandıysanız, bloblarınızın desteklenen biçimlerin açılan listesinden düzenlendiği tarih biçimini seçebilirsiniz...<BR>Örnek: YYYY/AA/GG, AA/GG/YYYY vb.   |
|Saat biçimi [isteğe bağlı]   | Belirttiğiniz yol deseninin içinde {Time} kullandıysanız, bloblarınızın desteklenen biçimlerin açılan listesinden düzenlendiği zaman biçimini seçebilirsiniz...<BR>Örnek: HH, HH/mm veya HH-mm.  |
|Olay serileştirme biçimi   | Sorgularınızın istediğiniz şekilde çalıştığından emin olmak için, Stream Analytics gelen veri akışları için hangi serileştirme biçimini kullandığınızı bilmeniz gerekir. Başvuru verileri için desteklenen biçimler CSV ve JSON ' tır.  |
|Encoding   | Şu anda desteklenen tek kodlama biçimi UTF-8'dir.  |

### <a name="static-reference-data"></a>Statik başvuru verileri

Başvuru verilerinizin değiştirilmesi beklenmiyorsa, giriş yapılandırmasında statik bir yol belirtilerek statik başvuru verileri desteği etkinleştirilir. Azure Stream Analytics belirtilen yoldan blobu seçer. {Date} ve {Time} değiştirme belirteçleri gerekli değildir. Başvuru verileri Stream Analytics sabit olduğundan, statik başvuru veri blobunun üzerine yazılması önerilmez.

### <a name="generate-reference-data-on-a-schedule"></a>Bir zamanlamaya göre başvuru verileri oluşturma

Başvuru verileriniz yavaş değişen bir veri kümesi ise, başvuru verilerinin yenilenmesi desteği, giriş yapılandırmasında {Date} ve {Time} değiştirme belirteçlerini kullanarak bir yol kalıbı belirtilerek etkinleştirilir. Stream Analytics, bu yol düzenine göre güncelleştirilmiş başvuru verileri tanımlarını seçer. Örneğin, tarih biçimi **"yyyy-aa-gg"** ve saat biçimi **"hh-mm"** olan bir `sample/{date}/{time}/products.csv` bir deseninin, 16 Nisan 2015 UTC saat diliminde 5:30: ' de güncelleştirilmiş blob `sample/2015-04-16/17-30/products.csv` seçmesini Stream Analytics söyler.

Azure Stream Analytics, yenilenen başvuru verileri bloblarını bir dakikalık aralıklarla otomatik olarak tarar. Zaman damgası 10:30:00 olan bir blob küçük bir gecikmeyle karşıya yüklenirse (örneğin, 10:30:30), bu bloba başvuran Stream Analytics işinde küçük bir gecikme fark edeceksiniz. Bu senaryolara engel olmak için, blob 'u bellek içinde bulup yüklemek ve işlemleri gerçekleştirmek için yeterli zaman Stream Analytics izin vermek üzere hedef geçerlilik zamanından (Bu örnekteki 10:30:00) daha önce blob yüklemeniz önerilir. 

> [!NOTE]
> Şu anda Stream Analytics işleri yalnızca makine süresi blob adında kodlanan zamana ilerlediği zaman blob yenilemesini arar. Örneğin, iş en kısa sürede `sample/2015-04-16/17-30/products.csv`, ancak 16 Nisan 2015 UTC saat diliminde 5:30 PM 'tan önceki bir süre içinde görünür. Bir blob, bulunan son değerden daha önce kodlanmış bir saat ile *hiçbir zaman* aramaz.
> 
> Örneğin, iş blobu bulduktan sonra `sample/2015-04-16/17-30/products.csv` 12 Nisan 5:30 2015 ' den önceki kodlanmış bir tarih içeren tüm dosyaları yok sayar. bu nedenle, bir geç gelen `sample/2015-04-16/17-25/products.csv` blobu aynı kapsayıcıda oluşturulduysa iş onu kullanmaz.
> 
> Benzer şekilde, `sample/2015-04-16/17-30/products.csv` yalnızca 16 Nisan 2015 ' de 10:03 ' de üretilediyse, kapsayıcıda daha önceki bir tarih içeren bir blob yoksa, iş bu 10:03 dosyayı 16 Nisan, 2015 ve önceki başvuru verilerini kullanacak şekilde kullanır.
> 
> Bunun bir özel durumu, işin, verileri zamanında yeniden işlemesi veya işin ilk başlatılışında olması gerekir. Başlangıç zamanında iş, belirtilen iş başlangıç zamanından önce üretilen en son blobu arıyor. Bu, iş başladığında **boş olmayan** bir başvuru veri kümesi olduğundan emin olmak için yapılır. Bir tane bulunamazsa, iş şu tanılamayı görüntüler: `Initializing input without a valid reference data blob for UTC time <start time>`.

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) , başvuru veri tanımlarını güncelleştirmek için Stream Analytics gereken güncelleştirilmiş blob 'ların oluşturulması görevini düzenlemek üzere kullanılabilir. Veri Fabrikası, veri taşımayı ve dönüştürmeyi düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Data Factory, [çok sayıda bulut tabanlı ve şirket içi veri depolarına bağlanmayı](../data-factory/copy-activity-overview.md) ve verileri belirttiğiniz düzenli bir zamanlamaya göre kolayca taşımayı destekler. Daha fazla bilgi ve bir Data Factory işlem hattının, önceden tanımlanmış bir zamanlamaya göre yenileyen Stream Analytics için başvuru verileri oluşturmak üzere nasıl ayarlanacağı hakkında adım adım yönergeler için bu [GitHub örneğine](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/ReferenceDataRefreshForASAJobs)göz atın.

### <a name="tips-on-refreshing-blob-reference-data"></a>Blob başvuru verilerini yenileme ipuçları

1. Başvuru verileri bloblarını sabit oldukları için üzerine yazmaz.
2. Başvuru verilerini yenilemek için önerilen yol şu şekilde yapılır:
    * Yol modelinde {Date}/{Time} kullanın
    * İş girişinde tanımlanan aynı kapsayıcıyı ve yol modelini kullanarak yeni bir blob ekleyin
    * Sıradaki son blob tarafından belirtilenden **büyük** bir tarih/saat kullanın.
3. Başvuru verileri Blobları, Blobun "son değiştirilme" zamanına **göre sıralanır,** ancak yalnızca {Date} ve {Time} değiştirmeler kullanılarak blob adında belirtilen saat ve Tarih ile belirtilir.
3. Çok sayıda blob listelemeyi önlemek için, işleme artık yapılmayacak olan çok eski blob 'ları silmeyi göz önünde bulundurun. Lütfen ASA 'nin yeniden başlatma gibi bazı senaryolarda küçük bir miktarı yeniden işlemesi gerekebileceğini unutmayın.

## <a name="azure-sql-database"></a>Azure SQL Veritabanı

Azure SQL veritabanı başvuru verileri Stream Analytics işiniz tarafından alınır ve işlenmek üzere bellekte bir anlık görüntü olarak depolanır. Başvuru verilerinizin anlık görüntüsü Ayrıca yapılandırma ayarlarında belirttiğiniz depolama hesabındaki bir kapsayıcıda depolanır. İş başladığında kapsayıcı otomatik olarak oluşturulur. İş durdurulmuşsa veya başarısız durumuna girerse, iş yeniden başlatıldığında otomatik oluşturulan kapsayıcılar silinir.  

Başvuru verileriniz yavaş değişen bir veri kümesi ise, işte kullanılan anlık görüntüyü düzenli olarak yenilemeniz gerekir. Stream Analytics, Azure SQL veritabanı giriş bağlantınızı yapılandırırken bir yenileme hızı ayarlamanıza olanak sağlar. Stream Analytics çalışma zamanı, Azure SQL veritabanınızı yenileme hızı tarafından belirtilen aralıkta sorgular. Desteklenen en hızlı yenileme hızı dakikada bir kez yapılır. Her yenileme için, Stream Analytics, belirtilen depolama hesabında yeni bir anlık görüntü depolar.

Stream Analytics, Azure SQL veritabanınızı sorgulamak için iki seçenek sunar. Bir anlık görüntü sorgusu zorunludur ve her işe eklenmelidir. Stream Analytics, yenileme aralığı temelinde anlık görüntü sorgusunu düzenli aralıklarla çalıştırır ve sorgu sonucunu (snapshot) başvuru veri kümesi olarak kullanır. Anlık görüntü sorgusunun çoğu senaryoya uyması gerekir, ancak büyük veri kümeleri ve hızlı yenileme ücretleri ile performans sorunlarıyla karşılaşırsanız, Delta sorgu seçeneğini kullanabilirsiniz. Başvuru veri kümesini döndürmek için 60 saniyeden daha fazla geçen sorgular zaman aşımı ile sonuçlanır.

Delta sorgu seçeneği ile, Stream Analytics taban çizgisi başvuru veri kümesi almak için başlangıçta anlık görüntü sorgusunu çalıştırır. Sonra, Stream Analytics artımlı değişiklikleri almak için yenileme aralığı temelinde düzenli aralıklarla Delta sorgusu çalıştırır. Bu artımlı değişiklikler, güncel tutmak için başvuru verileri kümesine sürekli olarak uygulanır. Delta sorgusunun kullanılması, depolama maliyetini ve ağ g/ç işlemlerini azaltmaya yardımcı olabilir.

### <a name="configure-sql-database-reference"></a>SQL veritabanı başvurusunu yapılandırma

SQL veritabanı başvuru verilerinizi yapılandırmak için önce **başvuru veri** girişi oluşturmanız gerekir. Aşağıdaki tabloda, başvuru verileri girişini açıklamasıyla oluştururken sağlamanız gereken her özellik açıklanmaktadır. Daha fazla bilgi için bkz. [Azure Stream Analytics bir iş IÇIN SQL veritabanından başvuru verileri kullanma](sql-reference-data.md).

[Azure SQL veritabanı yönetilen örneğini](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) , başvuru veri girişi olarak kullanabilirsiniz. [Azure SQL veritabanı yönetilen örneği 'nde genel uç noktasını yapılandırmanız](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure) ve ardından Azure Stream Analytics aşağıdaki ayarları el ile yapılandırmanız gerekir. SQL Server çalıştıran bir veritabanı ile birlikte çalışan Azure sanal makinesi, aşağıdaki ayarları el ile yapılandırarak de desteklenir.

|**Özellik adı**|**Açıklama**  |
|---------|---------|
|Giriş diğer adı|Bu girişe başvurmak için iş sorgusunda kullanılacak kolay bir ad.|
|Abonelik|Aboneliğinizi seçin|
|Veritabanı|Başvuru verilerinizi içeren Azure SQL veritabanı. Azure SQL veritabanı yönetilen örneği için 3342 numaralı bağlantı noktasını belirtmeniz gerekir. Örneğin, *sampleserver. public. Database. Windows. net, 3342*|
|Kullanıcı adı|Azure SQL veritabanınız ile ilişkilendirilen Kullanıcı adı.|
|Parola|Azure SQL veritabanınız ile ilişkilendirilen parola.|
|Düzenli aralıklarla Yenile|Bu seçenek, yenileme ücreti seçmenizi sağlar. "Açık" seçilirse yenileme hızını gg: ss: dd olarak belirtmenize izin verirsiniz.|
|Anlık görüntü sorgusu|Bu, SQL veritabanından başvuru verilerini alan varsayılan sorgu seçeneğidir.|
|Delta sorgusu|Büyük veri kümelerine ve kısa yenileme hızına sahip gelişmiş senaryolar için, bir Delta sorgusu eklemeyi seçin.|

## <a name="size-limitation"></a>Boyut sınırlaması

Stream Analytics **en fazla 300 MB boyutlu**başvuru verilerini destekler. En büyük başvuru verisi boyutu 300 MB sınırı yalnızca basit sorgularla ulaşılabilir. Sorgu karmaşıklığı, pencereli toplamalar, zamana bağlı birleşimler ve zamana bağlı analitik işlevler gibi durum bilgisi içeren işleme dahil olmak üzere arttıkça, başvuru verilerinin en fazla desteklenen boyutunun azalttığı beklenmektedir. Azure Stream Analytics başvuru verilerini yükleyemez ve karmaşık işlemler gerçekleştirmeyebilir, iş belleği tükenme ve başarısız olur. Bu gibi durumlarda% SU kullanım ölçümü %100 ' ye ulaşacaktır.    

|**Akış birimi sayısı**  |**Yaklaşık olarak desteklenen en büyük boyut (MB)**  |
|---------|---------|
|1   |50   |
|3   |150   |
|6 ve daha fazla   |300   |

6 ' dan fazla bir işin akış birimi sayısının artırılması, en fazla desteklenen başvuru verileri boyutunu artırmaz.

Sıkıştırma desteğine başvuru verileri için kullanılabilir değil. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
