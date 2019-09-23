---
title: Azure Data Factory veri akışı performansını ve ayarlama kılavuzunu eşleme | Microsoft Docs
description: Veri akışlarını eşleme kullandığınızda Azure Data Factory veri akışlarının performansını etkileyen anahtar faktörleri hakkında bilgi edinin.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 09/22/2019
ms.openlocfilehash: e4b3e08c0cc7fc1ead2aed551c228c6a1165c3b6
ms.sourcegitcommit: a19bee057c57cd2c2cd23126ac862bd8f89f50f5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71180850"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Veri akışlarını eşleme performansı ve ayarlama Kılavuzu

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory eşleme veri akışları, veri dönüştürmelerini ölçeklendirerek tasarlamak, dağıtmak ve düzenlemek için kod içermeyen bir tarayıcı arabirimi sağlar.

> [!NOTE]
> ADF eşleme veri akışlarını genel olarak bilmiyorsanız, bu makaleyi okumadan önce [veri akışlarına genel bakış](concepts-data-flow-overview.md) bölümüne bakın.
>

> [!NOTE]
> ADF kullanıcı arabiriminden veri akışları tasarlarken ve test ederken, bir kümenin ısınma süresini beklemeden veri akışlarınızı gerçek zamanlı olarak yürütebilmeniz için hata ayıklama anahtarını etkinleştirdiğinizden emin olun.
>

![Hata ayıklama düğmesi](media/data-flow/debugb1.png "Hata Ayıkla")

## <a name="monitor-data-flow-performance"></a>Veri akışı performansını izleme

Eşleme veri akışlarınızı tarayıcıda tasarlarken, her dönüşüm için alt ayarlar bölmesindeki veri önizleme sekmesine tıklayarak her bir dönüştürmeyi her bir dönüşüme göre test edebilirsiniz. Bir sonraki adım işlem hattı tasarımcısında veri akışınızı uçtan uca test etmeniz gerekir. Bir veri akışı yürütme etkinliği ekleyin ve veri akışınızın performansını test etmek için Hata Ayıkla düğmesini kullanın. İşlem hattı penceresinin alt bölmesinde, "eylemler" altında bir eyecam simgesi görürsünüz:

![Veri akışı izleyicisi](media/data-flow/mon002.png "Veri akışı izleyicisi 2")

Bu simgeye tıkladığınızda, veri akışınız için yürütme planı ve sonraki performans profili görüntülenir. Bu bilgileri, veri akışlarınızın performansını farklı boyutlardaki veri kaynaklarına karşı tahmin etmek için kullanabilirsiniz. Genel performans hesaplamalarınızda 1 dakikalık küme işi yürütme süresi olduğunu varsayabilir ve varsayılan Azure Integration Runtime kullanıyorsanız, 5 dakikalık küme döndürme süresi eklemeniz gerekebilir.

![Veri akışı izleme](media/data-flow/mon003.png "Veri akışı izleyicisi 3")

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Azure SQL veritabanı ve Azure SQL veri ambarı için iyileştirme

![Kaynak bölüm](media/data-flow/sourcepart3.png "Kaynak bölüm")

### <a name="partition-your-source-data"></a>Kaynak verilerinizi bölümleme

* "Iyileştirin" seçeneğine gidin ve "kaynak" seçeneğini belirleyin. Belirli bir tablo sütununu veya bir sorgudaki türü ayarlayın.
* "Sütun" seçeneğini belirlediyseniz bölüm sütununu seçin.
* Ayrıca, Azure SQL DB 'nize yönelik bağlantı sayısı üst sınırını ayarlayın. Veritabanınıza paralel bağlantı kazanmak için daha yüksek bir ayar deneyebilirsiniz. Ancak bazı durumlar, sınırlı sayıda bağlantıyla daha hızlı performans elde edebilir.
* Kaynak veritabanı tablolarınızın bölümlenmiş olması gerekmez.
* Kaynak dönüşümünüzün veritabanı tablonuzun bölümleme şeması ile eşleşen bir sorgu ayarlanması, kaynak veritabanı altyapısının Bölüm eleme özelliğinden yararlanmasını sağlar.
* Kaynağınız zaten bölümlenmemişse, ADF, kaynak dönüşümünde seçtiğiniz anahtara bağlı olarak Spark dönüştürme ortamında veri bölümleme işlemi kullanır.

### <a name="set-batch-size-and-query-on-source"></a>Kaynak üzerinde toplu iş boyutu ve sorgu ayarla

![Kaynak](media/data-flow/source4.png "Kaynak")

* Toplu iş boyutunu ayarlama, ADF 'yi satır satır yerine bellekte kümeler halinde depolamasını ister. Bu, isteğe bağlı bir ayardır ve doğru şekilde boyutlandıralınmazlarsa, işlem düğümlerinde kaynakları tükenmeyebilirsiniz.
* Bir sorgu ayarlamak, işleme için veri akışı için geldiklerinde bile satırları doğrudan filtrelemenize izin verebilir, bu da ilk veri alımını daha hızlı yapabilir.
* Bir sorgu kullanıyorsanız, Azure SQL VERITABANı için isteğe bağlı sorgu ipuçları ekleyebilirsiniz, yani READ UNCOMMıTTED

### <a name="set-isolation-level-on-source-transformation-settings-for-sql-datasets"></a>SQL veri kümeleri için kaynak dönüştürme ayarlarında yalıtım düzeyi ayarla

* Read UNCOMMITTED, kaynak dönüşümünde daha hızlı sorgu sonuçları sağlar

![Yalıtım düzeyi](media/data-flow/isolationlevel.png "Yalıtım düzeyi")

### <a name="set-sink-batch-size"></a>Havuz toplu iş boyutunu ayarla

![Havuz](media/data-flow/sink4.png "Havuz")

* Veri akışlarınızın satır içi işlemesini önlemek için, Azure SQL VERITABANı için havuz ayarları ' nda "toplu Iş boyutu" nı ayarlayın. Bu, ADF 'nin, belirtilen boyuta göre yığınlardaki veritabanı yazmaları işlemesini söyler.

### <a name="set-partitioning-options-on-your-sink"></a>Havuzunuzu bölümlendirme seçeneklerini ayarlama

* Verileriniz hedef Azure SQL DB tablolarında bölümlenmemiş olsanız bile, optimizasyon sekmesine gidin ve bölümlendirme ayarlayın.
* Çoğu kez, ADF 'nin Spark yürütme kümelerinde hepsini bir kez deneme için Bölümlendirmeyi kullanması, tek bir düğümden/bölümden tüm bağlantıları zorlamak yerine çok daha hızlı veri yüklemeye neden olur.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>İşlem altyapılarınızın boyutunu Azure Integration Runtime artırın

![Yenı IR](media/data-flow/ir-new.png "Yenı IR")

* Düğüm sayısını arttırabileceğiniz çekirdek sayısını artırın ve Azure SQL DB 'nize sorgu ve yazma için daha fazla işlem gücü sağlar.
* İşlem düğümlerinizde daha fazla kaynak uygulamak için "Işlem için Iyileştirilmiş" ve "bellek için Iyileştirilmiş" seçeneklerini deneyin.

### <a name="unit-test-and-performance-test-with-debug"></a>Hata ayıklama ile birim testi ve performans testi

* Birim testi verileri akışlarında, "veri akışı hata ayıklama" düğmesini açık olarak ayarlayın.
* Veri akışı Tasarımcısı ' nın içinde, dönüştürme mantığınızın sonuçlarını görüntülemek için dönüşümlerdeki veri önizleme sekmesini kullanın.
* İşlem hattı tasarım tuvaline bir veri akışı etkinliği yerleştirerek ve test etmek için "hata ayıkla" düğmesini kullanarak, veri hattı tasarımcısından alınan birim testi.
* Hata ayıklama modunda test etmek, tam zamanında küme dönmesi beklenmeden gerçek zamanlı bir küme ortamına karşı çalışır.
* Veri akışı Tasarımcısı deneyiminin içinde veri önizleme hata ayıklaması sırasında, veri akışı Tasarımcısı Kullanıcı arabirimindeki hata ayıklama ayarları bağlantısından satır sınırını ayarlayarak her bir kaynak için test ettiğiniz veri miktarını sınırlayabilirsiniz. Lütfen önce hata ayıklama modunu açmanız gerektiğini unutmayın.

![Hata ayıklama ayarları](media/data-flow/debug-settings.png "Hata ayıklama ayarları")

* Veri akışlarınızı işlem hattı hata ayıklama yürütmeden sınarken, kaynaklarınızın her birinde örnekleme boyutunu ayarlayarak test için kullanılan satır sayısını sınırlayabilirsiniz. İşlem hatlarınızı düzenli bir işlem zamanlamaya göre zamanlarken örnekleme 'yi devre dışı bıraktığınızdan emin olun.

![Satır örnekleme](media/data-flow/source1.png "Satır örnekleme")

### <a name="disable-indexes-on-write"></a>Yazma sırasında dizinleri devre dışı bırak
* Veri akışı etkinlikinizden önce, Havuzınızdan üzerine yazılmakta olan hedef tablolarınızdaki dizinleri devre dışı bırakan bir ADF işlem hattı saklı yordam etkinliğini kullanın.
* Veri akışı etkinliğinizi tamamladıktan sonra, bu dizinleri etkinleştirilen başka bir saklı proc etkinliği ekleyin.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Azure SQL DB 'nizin boyutunu artırın
* İşlem hattınızı artırmak ve DTU sınırlarına ulaştığınızda Azure azaltmayı en aza indirmek için, kaynağınızı yeniden boyutlandırmayı zamanlayın ve Azure SQL DB 'yi iş hattınızı çalıştırmadan önce Azure SQL DB 'yi yeniden boyutlandırın
* İşlem hattı yürütme tamamlandıktan sonra veritabanlarınızı normal çalışma hızına geri boyutlandırabilirsiniz.

## <a name="optimizing-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için iyileştirme

### <a name="use-staging-to-load-data-in-bulk-via-polybase"></a>Polybase aracılığıyla verileri toplu olarak yüklemek için hazırlama kullanma

* Veri akışlarınızın satır içi işlemesini önlemek için, alıcı sayısı ' nın "hazırlama" seçeneğini ayarlayarak, ADF 'nin satır sonu ekleme işlemini önlemek için havuz ayarlarındaki "hazırlama" seçeneğini ayarlayın. Bu, ADF 'nin PolyBase 'i kullanarak verilerin toplu olarak yüklenebilmesini sağlar.
* Veri akışı etkinliğinizi bir işlem hattından yürüttüğünüzde, hazırlama açık durumdayken, toplu yükleme için hazırlama verilerinizin BLOB depolama konumunu seçmeniz gerekir.

### <a name="increase-the-size-of-your-azure-sql-dw"></a>Azure SQL DW 'nizin boyutunu artırın

* İşlem hattınızı artırmak ve DWU limitleriyle iletişime geçmek için işlem hattınızı çalıştırmadan önce Azure SQL DW 'yi yeniden boyutlandırmayı zamanlayın ve Azure SQL DW 'yi en aza indirin.

* İşlem hattı yürütme tamamlandıktan sonra veritabanlarınızı normal çalışma hızına geri boyutlandırabilirsiniz.

## <a name="optimize-for-files"></a>Dosyalar için iyileştirin

* ADF 'nin kaç bölüm kullanacağı üzerinde denetim yapabilirsiniz. Her kaynak & havuz dönüştürmesinin yanı sıra her bir dönüştürme için de bir bölümleme düzeni ayarlayabilirsiniz. Daha küçük dosyalar için, "tek bölüm" seçimini seçtiğinizde, küçük dosyalarınızın bölümlenmesi için Spark sorulmaya kıyasla daha iyi ve hızlı bir şekilde çalışabilir.
* Kaynak verileriniz hakkında yeterli bilgiye sahip değilseniz, "hepsini bir kez deneme" ve bölüm sayısını ayarlama seçeneklerinden birini belirleyebilirsiniz.
* Verilerinizi araştırdığınızda ve iyi bir karma anahtar olabilecek sütunlarınızın olduğunu görürseniz, karma bölümlendirme seçeneğini kullanın.
* Veri önizleme ve ardışık düzen Hata ayıklamasında hata ayıklarken, dosya tabanlı kaynak veri kümeleri için sınır ve örnekleme boyutlarının, okunan satır sayısına değil, yalnızca döndürülen satır sayısına uygulanacağını unutmayın. Hata ayıklama yürütmelerinin performansını etkileyebilir ve muhtemelen akışın başarısız olmasına neden olabileceğinden, bu dikkat edilmesi önemlidir.
* Hata ayıklama kümelerinin varsayılan olarak küçük tek düğümlü kümeler olduğunu unutmayın, bu nedenle hata ayıklama için geçici küçük dosyaları kullanın. Hata ayıklama ayarları ' na gidin ve geçici bir dosya kullanarak verilerinizin küçük bir alt kümesini işaret edin.

![Hata ayıklama ayarları](media/data-flow/debugsettings3.png "Hata ayıklama ayarları")

### <a name="file-naming-options"></a>Dosya adlandırma seçenekleri

* ADF eşleme veri akışlarında dönüştürülmüş verileri yazmanın varsayılan yapısı, blob veya ADLS bağlı hizmeti olan bir veri kümesine yazmak şeklindedir. Bu veri kümesini, adlandırılmış bir dosya değil, bir klasörü veya kapsayıcıyı işaret etmek üzere ayarlamalısınız.
* Veri akışları yürütme için Azure Databricks Spark kullanır, bu da Çıktınıza varsayılan Spark bölümlendirme veya açıkça seçtiğiniz bölümleme şemasına göre birden çok dosya üzerinde bölüneceği anlamına gelir.
* ADF veri akışlarında çok yaygın bir işlem, tüm çıkış bölümü dosyalarınızın tek bir çıktı dosyasında birlikte birleştirilmesi için "tek dosyaya çıktı" seçeneğini seçmaktır.
* Ancak bu işlem, çıktının tek bir küme düğümünde tek bir bölüme azalttığını gerektirir.
* Bu popüler seçeneği seçerken bunu aklınızda bulundurun. Birçok büyük kaynak dosyasını tek bir çıkış dosyası bölümünde birleştiriyorsanız, küme düğüm kaynakları ' nı çalıştırabilirsiniz.
* İşlem düğümü kaynaklarını tüketmeyi önlemek için, varsayılan veya açık bölümlendirme şemasını ADF 'de tutabilir ve bu sayede performansı iyileştirir ve sonra, tüm bölüm dosyalarını çıkış klasöründen yeni bir tek birleştiren işlem hattında daha sonra bir kopyalama etkinliği ekleyebilirsiniz dosyasýný. Temelde, bu teknik, dönüştürme eylemini dosya birleştirmeden ayırır ve "çıktıyı tek dosyaya Dönüştür" ayarıyla aynı sonuca ulaşır.

### <a name="looping-through-file-lists"></a>Dosya listeleri aracılığıyla döngü

Çoğu örnekte, ADF 'deki veri akışları, veri akışı kaynak dönüşümünün birden çok dosya üzerinde yineleme yapmasına izin veren bir işlem hattından daha iyi yürütülür. Diğer bir deyişle, veri akışında kaynak içinde yer alan joker karakter veya dosya listelerinin kullanılması tercih edilir. Bu, her yinelemede bir yürütme veri akışı çağırarak, işlem hattındaki bir dosya listesini, ardışık düzendeki ForEach ' i kullanarak yineleyebilirsiniz. Veri akışı işlemi, döngü veri akışı içinde oluşmasına izin vererek daha hızlı yürütülür.

Örneğin, 2019 Temmuz 'dan BLOB depolama alanındaki bir klasörde işlemek istediğim veri dosyalarının bir listesi varsa, işlem hattınızdan bir kez veri akışı yürütme etkinliği çağrısı yapmak ve kaynağınızda bunun gibi bir joker karakter kullanmanız daha iyi bir işlemdir. :

```DateFiles/*_201907*.txt```

Bu, içinde bir veri akışı yürütme etkinliği ile bir ForEach kullanarak tüm eşleşen dosyalarda yinelenen bir işlem hattındaki Blob deposunda bir aramanın daha iyi gerçekleştirilir.

### <a name="increase-the-size-of-your-debug-cluster"></a>Hata ayıklama kümenizin boyutunu artırın

Varsayılan olarak, hata ayıklamayı açmak her bir veri fabrikası için otomatik olarak oluşturulan varsayılan Azure tümleştirme çalışma zamanını kullanır. Bu varsayılan Azure IR, genel Işlem özellikleri kullanılarak 8 çekirdek, bir sürücü düğümü için 4 ve bir çalışan düğümü için 4 olarak ayarlanır. Daha büyük verilerle test ettiğinizde, daha büyük yapılandırmalara sahip yeni bir Azure IR oluşturarak hata ayıklama kümenizin boyutunu artırabilir ve hata ayıklama üzerinde geçiş yaparken bu yeni Azure IR seçebilirsiniz. Bu, ADF 'yi veri önizleme ve veri akışları ile ardışık düzen hata ayıklaması için bu Azure IR kullanmasını ister.

## <a name="next-steps"></a>Sonraki adımlar

Performansla ilgili diğer veri akışı makalelerine bakın:

- [Veri akışı Iyileştirme sekmesi](concepts-data-flow-optimize-tab.md)
- [Veri akışı etkinliği](control-flow-execute-data-flow-activity.md)
- [Veri akışı performansını izleme](concepts-data-flow-monitoring.md)
