---
title: Azure Akış Analizi'nde aramalar için başvuru verilerini kullanma
description: Bu makalede, bir Azure Akış Analizi işinin sorgu tasarımında verileri aramak veya ilişkilendirmek için başvuru verilerinin nasıl kullanılacağı açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/8/2019
ms.openlocfilehash: b3808524706b13761dd8eccffa301c602d08f481
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267293"
---
# <a name="using-reference-data-for-lookups-in-stream-analytics"></a>Akış Analizi'nde aramalar için referans verilerini kullanma

Başvuru verileri (arama tablosu olarak da bilinir), statik veya doğada yavaş yavaş değişen, arama yapmak veya veri akışlarınızı artırmak için kullanılan sonlu bir veri kümesidir. Örneğin, bir IoT senaryosunda, başvuru verilerinde sensörlerle ilgili meta verileri (sık sık değişmeyen) depolayabilir ve gerçek zamanlı IoT veri akışlarıyla birleştirebilirsiniz. Azure Akış Analizi, düşük gecikmeli akış işleme sayılmak için referans verilerini belleğe yükler. Azure Akış Analizi işinizde referans verilerini kullanmak için, genellikle sorgunuzda bir [Referans Veri Birleştirme](https://docs.microsoft.com/stream-analytics-query/reference-data-join-azure-stream-analytics) kullanırsınız. 

Akış Analizi, Başvuru Verileri için depolama katmanı olarak Azure Blob depolamasını ve Azure SQL Veritabanını destekler. Ayrıca, [herhangi bir sayıda bulut tabanlı](../data-factory/copy-activity-overview.md)ve şirket içi veri deposu kullanmak için başvuru verilerini Azure Veri Fabrikası'ndan Blob depolamasına dönüştürebilir ve/veya kopyalayabilirsiniz.

## <a name="azure-blob-storage"></a>Azure Blob depolama

Başvuru verileri, blob adında belirtilen tarih/saatin artan sırasına göre blob dizisi (giriş yapılandırmasında tanımlanan) olarak modellenir. Yalnızca **only** dizideki son blob tarafından belirtilenden **daha büyük** bir tarih/saat kullanarak dizinin sonuna eklemeyi destekler.

### <a name="configure-blob-reference-data"></a>Blob başvuru verilerini yapılandırma

Başvuru verilerinizi yapılandırmak için öncelikle **Başvuru Verileri**türüne ait bir giriş oluşturmanız gerekir. Aşağıdaki tablo, açıklamasıyla birlikte referans veri girişi oluştururken sağlamanız gereken her özelliği açıklar:

|**Özellik Adı**  |**Açıklama**  |
|---------|---------|
|Girdi Diğer Adı   | Bu girişe başvurmak için iş sorgusunda kullanılacak kolay bir ad.   |
|Depolama Hesabı   | Lekelerinizin bulunduğu depolama hesabının adı. Akış Analizi İşinizle aynı abonelikteyse, açılır yerden seçebilirsiniz.   |
|Depolama Hesap Anahtarı   | Depolama hesabıyla ilişkili gizli anahtar. Depolama hesabı Stream Analytics işinizle aynı abonelikteyse, bu işlem otomatik olarak doldurulur.   |
|Depolama Konteyneri   | Kapsayıcılar, Microsoft Azure Blob hizmetinde depolanan blob'lar için mantıksal bir gruplandırma sağlar. Blob hizmetine bir blob yüklediğinizde, bu blob için bir kapsayıcı belirtmeniz gerekir.   |
|Yol Deseni   | Lekelerinizi belirtilen kapsayıcının içinde bulmak için kullanılan yol. Yol içinde, aşağıdaki 2 değişkenden birini veya daha fazla örneğini belirtmeyi seçebilirsiniz:<BR>{tarih}, {zaman}<BR>Örnek 1: ürünler/{tarih}/{time}/product-list.csv<BR>Örnek 2: ürünler/{tarih}/ürün-list.csv<BR>Örnek 3: product-list.csv<BR><br> Blob belirtilen yolda yoksa, Stream Analytics işi blobun kullanılabilir hale gelmesi için süresiz olarak bekler.   |
|Tarih Biçimi [isteğe bağlı]   | Belirlediğiniz Yol Deseni içinde {tarih} kullandıysanız, desteklenen biçimlerin açılır tarafından blobs düzenlendiği tarih biçimini seçebilirsiniz.<BR>Örnek: YYYY/MM/DD, MM/DD/YYYY, vb.   |
|Zaman Biçimi [isteğe bağlı]   | Belirlediğiniz Yol Deseni içinde {zaman} kullandıysanız, desteklenen biçimlerin açılır tarafından blobs düzenlendiği zaman biçimini seçebilirsiniz.<BR>Örnek: HH, HH/mm veya HH-mm.  |
|Olay Serileştirme Biçimi   | Akış Analizi, sorgularınızın beklediğiniz gibi çalıştığından emin olmak için, gelen veri akışları için hangi serileştirme biçimini kullandığınızı bilmesi gerekir. Başvuru Verileri için desteklenen biçimler CSV ve JSON'dur.  |
|Encoding   | UTF-8 şu anda desteklenen tek kodlama biçimidir.  |

### <a name="static-reference-data"></a>Statik referans verileri

Başvuru verilerinizin değişmesi beklenmiyorsa, giriş yapılandırmasında statik bir yol belirtilerek statik başvuru verileri desteği etkinleştirilir. Azure Akış Analizi, blob'u belirtilen yoldan alır. {date} ve {time} değiştirme belirteçleri gerekmez. Akış Analizi'nde başvuru verileri değişmez olduğundan, statik bir başvuru verisi blob'unüzerine yazmak önerilmez.

### <a name="generate-reference-data-on-a-schedule"></a>Zamanlamada referans verioluşturma

Başvuru verileriniz yavaş yavaş değişen bir veri kümesiyse, giriş yapılandırmasında {date} ve {time} değiştirme belirteçlerini kullanarak bir yol deseni belirterek başvuru verilerini yenileme desteği etkinleştirilir. Akış Analizi, bu yol desenine göre güncelleştirilmiş başvuru veri tanımlarını alır. Örneğin, **"YYYY-MM-DD"** tarih biçimi ve **"HH-mm"** zaman `sample/{date}/{time}/products.csv` biçimiiçeren bir desen, Stream Analytics'e `sample/2015-04-16/17-30/products.csv` 16 Nisan 2015 tarihinde saat 17:30'da güncellenen blob'u almasını bildirir.

Azure Akış Analizi, bir dakikalık aralıkla yenilenmiş başvuru veri lekelerini otomatik olarak tarar. Zaman damgası 10:30:00'lı bir blob küçük bir gecikmeyle yüklenirse (örneğin, 10:30:30), Stream Analytics işinde bu blob'a atıfta bulunan küçük bir gecikme fark esiniz. Bu tür senaryoları önlemek için, Stream Analytics'in bellekte bulmak ve yüklemek ve işlemleri gerçekleştirmek için yeterli zaman almasına izin vermek için blob'un hedef etkili çalışma süresinden (bu örnekte 10:30:00) daha önce yüklenmesi önerilir. 

> [!NOTE]
> Şu anda Stream Analytics işleri, blob yenilemesini yalnızca makine zamanı blob adında kodlanmış zamana ilerlediğinde arar. Örneğin, iş en kısa `sample/2015-04-16/17-30/products.csv` zamanda ancak en erken 16 Nisan 2015 tarihinde saat 17:30'da arayacaktır. Keşfedilen son zaman daha erken bir kodlanmış zaman ile bir blob aramak *asla.*
> 
> Örneğin, iş blob `sample/2015-04-16/17-30/products.csv` bulduğunda, kodlanmış tarihi 16 Nisan 2015'ten önce 17:30'dan önce olan dosyaları `sample/2015-04-16/17-25/products.csv` yok sayacaktır, böylece aynı kapsayıcıda geç gelen bir blob oluşturulursa iş onu kullanmaz.
> 
> Aynı şekilde, 16 Nisan 2015 saat 22:03'te `sample/2015-04-16/17-30/products.csv` üretiliyorsa ancak konteynerde daha erken bir tarih olan bir leke yoksa, iş bu dosyayı 16 Nisan 2015 saat 10:03'ten itibaren kullanacak ve o zamana kadar önceki başvuru verilerini kullanacaktır.
> 
> Bunun bir istisnası, işin verileri zamanında yeniden işlemesi gerektiğinde veya işin ilk başlatıldığında olmasıdır. Başlangıç saatinde iş belirtilen iş başlangıç saatinden önce üretilen en son blob arıyor. Bu, iş başladığında boş **olmayan** bir başvuru veri kümesi olduğundan emin olmak için yapılır. Bir bulunamıyorsa, iş aşağıdaki tanıyı `Initializing input without a valid reference data blob for UTC time <start time>`görüntüler: .

[Azure Veri Fabrikası,](https://azure.microsoft.com/documentation/services/data-factory/) akış analizinin başvuru veri tanımlarını güncelleştirmek için gerektirdiği güncelleştirilmiş lekeler oluşturma görevini düzenlemek için kullanılabilir. Data Factory, verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Veri Fabrikası, [çok sayıda bulut tabanlı ve şirket içi veri deposuna bağlanmayı ve](../data-factory/copy-activity-overview.md) verileri belirttiğiniz normal bir zamanlamayla kolayca taşımayı destekler. Önceden tanımlanmış bir zamanlamada yenilenen Stream Analytics için referans verileri oluşturmak için bir Veri Fabrikası ardışık hattının nasıl kurulacağı yla ilgili daha fazla bilgi ve adım adım kılavuz için bu [GitHub örneğine](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/ReferenceDataRefreshForASAJobs)göz atın.

### <a name="tips-on-refreshing-blob-reference-data"></a>Blob başvuru verilerini yenileme küçme ipuçları

1. Değişmez oldukları için referans veri bloblarının üzerine yazmayın.
2. Başvuru verilerini yenilemenin önerilen yolu:
    * Yol deseninde {date}/{time} kullanın
    * İş girişinde tanımlanan aynı kapsayıcı ve yol deseni kullanarak yeni bir leke ekleme
    * Dizideki son blob tarafından belirtilenden **daha büyük** bir tarih/saat kullanın.
3. Başvuru veri blobs blob's "Son Modifiye" zaman değil, sadece {date} ve {time} ikamekullanarak blob adında belirtilen zaman ve tarih **tarafından** sıralanır.
3. Çok sayıda blob listelemek zorunda kalmamak için, işleme artık yapılacak olan ço eski lekeleri silmeyi düşünün. ASA'nın yeniden başlatma gibi bazı senaryolarda küçük bir miktarı yeniden işlemesi gerekebileceğini lütfen unutmayın.

## <a name="azure-sql-database"></a>Azure SQL Veritabanı

Azure SQL Veritabanı başvuru verileri, Akış Analizi işiniz tarafından alınır ve işleme için bellekte anlık görüntü olarak depolanır. Başvuru verilerinizin anlık görüntüsü, yapılandırma ayarlarında belirttiğiniz bir depolama hesabındaki bir kapsayıcıda da depolanır. Kapsayıcı, iş başladığında otomatik olarak oluşturulur. İş durdurulursa veya başarısız bir duruma girerse, iş yeniden başlatıldığında otomatik oluşturulan kapsayıcılar silinir.  

Başvuru verileriniz yavaş yavaş değişen bir veri kümesiyse, işinizde kullanılan anlık görüntünün düzenli olarak yenilenmesi gerekir. Akış Analizi, Azure SQL Veritabanı giriş bağlantınızı yapılandırDığınızda bir yenileme hızı belirlemenize olanak tanır. Akış Analizi çalışma süresi, Azure SQL Veritabanınızı yenileme hızıyla belirtilen aralıkta sorgular. Desteklenen en hızlı yenileme hızı dakikada bir kez dir. Her yenileme için Stream Analytics, sağlanan depolama hesabında yeni bir anlık görüntü depolar.

Akış Analizi, Azure SQL Veritabanınızı sorgulamak için iki seçenek sunar. Anlık görüntü sorgusu zorunludur ve her işe dahil edilmelidir. Akış Analizi anlık görüntü sorgusunu yenileme aralığınızı temel alınarak düzenli aralıklarla çalıştırın ve başvuru veri kümesi olarak sorgunun sonucunu (anlık görüntü) kullanır. Anlık görüntü sorgusu çoğu senaryoya uymalıdır, ancak büyük veri kümeleri ve hızlı yenileme hızlarıyla performans sorunlarıyla karşınıza çıkarsa, delta sorgusu seçeneğini kullanabilirsiniz. Başvuru veri kümesini döndürmek 60 saniyeden uzun süren sorgular bir zaman alacaktır.

Delta sorgu seçeneğiyle, Akış Analizi temel başvuru veri kümesini almak için başlangıçta anlık görüntü sorgusunu çalıştırın. Sonra, Akış Analizi, artımlı değişiklikleri almak için yenileme aralığınızı temel alan delta sorgusunu düzenli aralıklarla çalıştırın. Bu artımlı değişiklikler, güncel liğini korumak için başvuru veri kümesine sürekli olarak uygulanır. Delta sorgusunun kullanılması depolama maliyetini ve ağ G/Ç işlemlerini azaltmaya yardımcı olabilir.

### <a name="configure-sql-database-reference"></a>SQL Veritabanı başvuruyapılandırma

SQL Veritabanı başvuru verilerinizi yapılandırmak için öncelikle **Referans Veri** girişi oluşturmanız gerekir. Aşağıdaki tablo, açıklamasıyla birlikte referans veri girişi oluştururken sağlamanız gereken her özelliği açıklar. Daha fazla bilgi için bkz: [Azure Akışı Analizi işi için bir SQL Veritabanından başvuru verilerini kullan.](sql-reference-data.md)

[Azure SQL Veritabanı Yönetilen Örnek'i](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) başvuru veri girişi olarak kullanabilirsiniz. [Azure SQL Veritabanı Yönetilen Örneği'nde ortak bitiş noktasını yapılandırmanız](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure) ve ardından Azure Akış Analizi'nde aşağıdaki ayarları el ile yapılandırmanız gerekir. SQL Server'ı bir veritabanı ekli çalıştıran Azure sanal makine, aşağıdaki ayarları el ile yapılandırarak da desteklenir.

|**Özellik Adı**|**Açıklama**  |
|---------|---------|
|Girdi diğer adı|Bu girişe başvurmak için iş sorgusunda kullanılacak kolay bir ad.|
|Abonelik|Aboneliğinizi seçin|
|Database|Başvuru verilerinizi içeren Azure SQL Veritabanı. Azure SQL Veritabanı Yönetilen Örnek için bağlantı noktası 3342'yi belirtmeniz gerekir. Örneğin, *sampleserver.public.database.windows.net,3342*|
|Kullanıcı adı|Azure SQL Veritabanınızla ilişkili kullanıcı adı.|
|Parola|Azure SQL Veritabanınızla ilişkili parola.|
|Periyodik olarak yenileyin|Bu seçenek, yenileme oranı seçmenize olanak tanır. "Açık"ı seçmek, DD:HH:MM'de yenileme oranını belirtmenizi sağlar.|
|Anlık görüntü sorgusu|Bu, BAŞVURU VERILERINI SQL Veritabanınızdan alan varsayılan sorgu seçeneğidir.|
|Delta sorgusu|Büyük veri kümeleri ve kısa yenileme hızına sahip gelişmiş senaryolar için bir delta sorgusu eklemeyi seçin.|

## <a name="size-limitation"></a>Boyut sınırlaması

Stream **Analytics, en fazla 300 MB boyutuyla**referans verilerini destekler. Başvuru verilerinin maksimum boyutu 300 MB sınırı yalnızca basit sorgularla elde edilebilir. Sorgukarmaşıklığı, pencereli agregalar, zamansal birleştirmeler ve zamansal analitik işlevler gibi durum lu işlemeyi içerecek şekilde arttıkça, başvuru verilerinin desteklenen maksimum boyutunun azalması beklenir. Azure Akış Analizi başvuru verilerini yükleyemiyor ve karmaşık işlemler gerçekleştiremiyorsa, işin belleği biter ve başarısız olur. Bu gibi durumlarda, SU % Kullanım ölçümü %100'e ulaşacaktır.    

|**Akış Birimi Sayısı**  |**Yaklaşık Max Boyut Destekli (MB' de)**  |
|---------|---------|
|1   |50   |
|3   |150   |
|6 ve sonrası   |300   |

Bir işin Akış Birimi sayısının 6'nın üzerinde artırılması, desteklenen en büyük başvuru verisi boyutunu artırmaz.

Başvuru verileri için sıkıştırma desteği kullanılamıyor. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
