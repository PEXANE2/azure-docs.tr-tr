---
title: Veri akışı performansını ve ayarlama kılavuzunu eşleme
description: Azure Data Factory veri akışlarını eşleme performansını etkileyen anahtar faktörleri hakkında bilgi edinin.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.custom: seo-lt-2019
ms.date: 03/11/2020
ms.openlocfilehash: 1a6b50456a5dc3ff89fe7b513f406dc68bd2401e
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79246298"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Veri akışlarını eşleme performansı ve ayarlama Kılavuzu

Azure Data Factory veri akışlarını eşleme, veri dönüştürmelerinin ölçeklendirilmesi, dağıtılması ve organize olması için kod içermeyen bir arabirim sağlar. Veri akışlarını eşleme konusunda bilgi sahibi değilseniz, [eşleme veri akışına genel bakış](concepts-data-flow-overview.md)bölümüne bakın.

ADF UX 'den veri akışları tasarlarken ve test ederken, bir kümenin ısınma süresini beklemeden veri akışlarınızı gerçek zamanlı olarak yürütmek için hata ayıklama modunda geçiş yaptığınızdan emin olun. Daha fazla bilgi için bkz. [hata ayıklama modu](concepts-data-flow-debug-mode.md).

Bu videoda veri akışları ile verileri dönüştüren bazı örnek zamanlamalar gösterilmektedir:
> [!VIDEO https://www.youtube.com/watch?v=6CSbWm4lRhw]

## <a name="monitoring-data-flow-performance"></a>Veri akışı performansını izleme

Eşleme veri akışları tasarlarken, yapılandırma panelinde veri önizleme sekmesine tıklayarak her bir dönüşüme birim test edebilirsiniz. Mantığınızı doğruladıktan sonra, bir işlem hattındaki etkinlik olarak veri akışınızı uçtan uca test edin. Bir veri akışı yürütme etkinliği ekleyin ve veri akışınızın performansını test etmek için Hata Ayıkla düğmesini kullanın. Veri akışınızın yürütme planını ve performans profilini açmak için, işlem hattınızdaki çıkış sekmesinde ' Eylemler ' altında bulunan göz gözlük simgesine tıklayın.

![Veri akışı Izleyicisi](media/data-flow/mon002.png "Veri akışı Izleyicisi 2")

 Bu bilgileri, veri akışınızın farklı boyutlardaki veri kaynaklarına karşı performansını tahmin etmek için kullanabilirsiniz. Daha fazla bilgi için bkz. [eşleme veri akışlarını izleme](concepts-data-flow-monitoring.md).

![Veri Akışını İzleme](media/data-flow/mon003.png "Veri akışı Izleyicisi 3")

 İşlem hattı hata ayıklama çalıştırmaları için, bir ısınma kümesi için genel performans hesaplamalarınızda bir dakikalık küme ayarlama zamanı gereklidir. Varsayılan Azure Integration Runtime başladıysanız, başlatma süresi yaklaşık 5 dakika sürebilir.

## <a name="increasing-compute-size-in-azure-integration-runtime"></a>Azure Integration Runtime işlem boyutunu artırma

Daha fazla çekirdeğe sahip bir Integration Runtime Spark işlem ortamlarındaki düğümlerin sayısını artırır ve verilerinizi okumak, yazmak ve dönüştürmek için daha fazla işlem gücü sağlar.
* İşlem hızlarınızın giriş oranından daha yüksek olmasını istiyorsanız, **işlem Için iyileştirilmiş** bir küme deneyin.
* Bellekte daha fazla veri önbelleğe almak istiyorsanız **bellek Için iyileştirilmiş** bir küme deneyin. Bellek için iyileştirilmiş, Işlem Iyileştirilenden çekirdek başına daha yüksek bir fiyat noktasına sahiptir, ancak büyük olasılıkla daha hızlı dönüştürme hızına neden olur.

![Yeni IR](media/data-flow/ir-new.png "Yeni IR")

Integration Runtime oluşturma hakkında daha fazla bilgi için bkz. [Azure Data Factory Integration Runtime](concepts-integration-runtime.md).

### <a name="increase-the-size-of-your-debug-cluster"></a>Hata ayıklama kümenizin boyutunu artırın

Varsayılan olarak, hata ayıklamayı açmak her bir veri fabrikası için otomatik olarak oluşturulan varsayılan Azure tümleştirme çalışma zamanını kullanır. Bu varsayılan Azure IR, genel Işlem özellikleri kullanılarak, bir sürücü düğümü için dört ve bir çalışan düğümü için dört adet olmak üzere sekiz çekirdek için ayarlanır. Daha büyük verilerle test ettiğinizde, daha büyük yapılandırmalara sahip bir Azure IR oluşturarak hata ayıklama kümenizin boyutunu artırabilir ve hata ayıklama sırasında bu yeni Azure IR seçebilirsiniz. Bu, ADF 'yi veri önizleme ve veri akışları ile ardışık düzen hata ayıklaması için bu Azure IR kullanmasını ister.

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Azure SQL veritabanı ve Azure SQL veri ambarı için iyileştirme

### <a name="partitioning-on-source"></a>Kaynakta bölümlendirme

1. **Optimizasyon** sekmesine gidin ve **bölümleme ayarla** ' yı seçin.
1. **Kaynak**seçin.
1. **Bölüm sayısı**altında, Azure SQL DB 'nize yönelik bağlantı sayısı üst sınırını ayarlayın. Veritabanınıza paralel bağlantı kazanmak için daha yüksek bir ayar deneyebilirsiniz. Ancak bazı durumlar, sınırlı sayıda bağlantıyla daha hızlı performans elde edebilir.
1. Belirli bir tablo sütunuyla veya bir sorgu ile bölümleyip bölümlendirmeyeceğinizi seçin.
1. **Sütun**' ı seçtiyseniz, Bölüm sütununu seçin.
1. **Sorgu**' yı seçtiyseniz, veritabanı tablonuzun bölümleme düzeniyle eşleşen bir sorgu girin. Bu sorgu, kaynak veritabanı altyapısının Bölüm eleme özelliğinden yararlanmasını sağlar. Kaynak veritabanı tablolarınızın bölümlenmiş olması gerekmez. Kaynağınız zaten bölümlenmemişse ADF, kaynak dönüşümünde seçtiğiniz anahtara göre Spark dönüştürme ortamında veri bölümleme kullanır.

![Kaynak bölüm](media/data-flow/sourcepart3.png "Kaynak bölüm")

> [!NOTE]
> Kaynağınız için bölüm sayısını seçmenize yardımcı olacak iyi bir kılavuz, Azure Integration Runtime için ayarladığınız çekirdek sayısına bağlıdır ve bu sayıyı beş ile çarpın. Bu nedenle, örneğin, ADLS klasörlerinizdeki bir dizi dosyayı dönüştürüyorsanız ve 32 çekirdekli Azure IR kullanacaksanız, hedeflediğiniz bölüm sayısı 32 x 5 = 160 bölümlerdir.

### <a name="source-batch-size-input-and-isolation-level"></a>Kaynak toplu iş boyutu, giriş ve yalıtım düzeyi

Kaynak dönüşümünde **kaynak seçenekleri** altında aşağıdaki ayarlar performansı etkileyebilir:

* Toplu iş boyutu, ADF 'yi satır satır yerine bellekte kümeler halinde depolamasını ister. Toplu iş boyutu isteğe bağlı bir ayardır ve doğru boyutlandıralınmazlarsa, işlem düğümlerinde kaynakları çalıştırabilirsiniz.
* Bir sorgu ayarlandığında, işleme için veri akışına geçmeden önce kaynakta satırları filtrelemenize izin verebilir. Bu, ilk veri alımını daha hızlı hale getirir. Bir sorgu kullanıyorsanız, Azure SQL DB 'niz için READ UNCOMMıTTED gibi isteğe bağlı sorgu ipuçları ekleyebilirsiniz.
* Read UNCOMMITTED, kaynak dönüşümünde daha hızlı sorgu sonuçları sağlar

![Kaynak](media/data-flow/source4.png "Kaynak")

### <a name="sink-batch-size"></a>Havuz toplu iş boyutu

Veri akışlarınızın satır içi işlenmesini önlemek için, Azure SQL VERITABANı ve Azure SQL DW havuzları için ayarlar sekmesinde **toplu iş boyutunu** ayarlayın. Toplu iş boyutu ayarlandıysa, ADF, belirtilen boyuta göre veritabanı yazmaları toplu işlemlere işler.

![Ev](media/data-flow/sink4.png "Havuz")

### <a name="partitioning-on-sink"></a>Havuzda bölümlendirme

Verileriniz hedef Tablolarınızda bölümlenmemiş olsanız bile, verilerinizin havuz dönüşümünde bölümlenmesi önerilir. Bölümlenmiş veriler genellikle tüm bağlantıların tek bir düğüm/bölüm kullanmasına zorlanarak çok daha hızlı yüklenmesine neden olur. Havuzunuzda En Iyi duruma getirin sekmesine gidin ve havuzunuzda yazılacak ideal bölüm sayısını seçmek için bir *kez deneme* bölümlendirme seçeneğini belirleyin.

### <a name="disable-indexes-on-write"></a>Yazma sırasında dizinleri devre dışı bırak

İşlem hattınızda, Havuzınızdan yazılmış hedef tablolarınızdaki dizinleri devre dışı bırakan veri akışı etkinlikinizden önce bir [saklı yordam etkinliği](transform-data-using-stored-procedure.md) ekleyin. Veri akışı etkinliğinizi tamamladıktan sonra, bu dizinleri sağlayan başka bir saklı yordam etkinliği ekleyin. Ya da bir veritabanı havuzunda ön işleme ve işlem sonrası betikleri kullanın.

### <a name="increase-the-size-of-your-azure-sql-db-and-dw"></a>Azure SQL DB ve DW 'nizin boyutunu artırın

İşlem hattınızı artırmak ve DTU sınırlarına ulaştığınızda Azure azaltmayı en aza indirmek için, kaynağınızı yeniden boyutlandırmayı zamanlayın ve Azure SQL DB ve DW 'yi iş hattınızda çalıştırın. İşlem hattı yürütme tamamlandıktan sonra veritabanlarınızı normal çalışma hızına geri doğru yeniden boyutlandırın.

* Tek bir türetilmiş sütun dönüştürmesi olan SQL DB kaynak tablosu, 887k satırları ve 74 sütunları olan SQL DB tablosu, bellek için iyileştirilmiş 80-Core hata ayıklama Azure IRS kullanarak 3 dakikalık uçtan uca sürer.

### <a name="azure-synapse-sql-dw-only-use-staging-to-load-data-in-bulk-via-polybase"></a>[Yalnızca Azure SYNAPSE SQL DW] Polybase aracılığıyla verileri toplu olarak yüklemek için hazırlama kullanma

DW 'nize satır satır ekleme yapmaktan kaçınmak için, ADF 'nin [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)kullanabilmesi için havuz ayarlarınızda **hazırlama özelliğini etkinleştirin** ' i işaretleyin. PolyBase, ADF 'nin verileri toplu olarak yüklemesine olanak tanır.
* Veri akışı etkinliğinizi bir işlem hattından yürüttüğünüzde, toplu yükleme sırasında verilerinizi hazırlamak için bir blob veya ADLS 2. depolama konumu seçmeniz gerekir.

* 74 sütunlu bir Synapse tablosuna ve tek bir türetilmiş sütun dönüşümüne sahip 421Mb dosyasının dosya kaynağı, bellek için iyileştirilmiş 80-Core hata ayıklama Azure IRS kullanarak 4 dakikalık uçtan uca kadar sürer.

## <a name="optimizing-for-files"></a>Dosyalar için iyileştirme

Her dönüşümde, Data Factory 'nin en Iyileştirme sekmesinde kullanmasını istediğiniz bölümlendirme şemasını ayarlayabilirsiniz. İlk olarak dosya tabanlı havuzları test etmek, varsayılan bölümlendirme ve iyileştirmeleri korumak iyi bir uygulamadır.

* Daha küçük dosyalar için, daha az bölüm seçmeyi fark edebilirsiniz.
* Kaynak verileriniz hakkında yeterli bilgiye sahip değilseniz, *hepsini bir kez deneme* bölümlendirme ve bölüm sayısını ayarlama ' yı seçin.
* Verilerinizde iyi bir karma anahtar olabilecek sütunlar varsa *karma bölümlendirme*' yi seçin.

* 74 sütunlu ve tek bir türetilmiş sütun dönüşümünde, 421Mb 'Lık bir dosyanın dosya havuzuna sahip dosya kaynağı, bellek için iyileştirilmiş 80-Core hata ayıklama Azure IRS kullanılarak yaklaşık 2 dakikalık uçtan uca sürer.

Veri önizleme ve ardışık düzen Hata ayıklamasında hata ayıklarken, dosya tabanlı kaynak veri kümeleri için sınır ve örnekleme boyutları, okunan satır sayısı için yalnızca döndürülen satır sayısı için geçerlidir. Bu, hata ayıklama yürütmelerinin performansını etkileyebilir ve muhtemelen akışın başarısız olmasına neden olabilir.
* Hata ayıklama kümeleri, varsayılan olarak tek düğümlü küçük kümelerdir ve hata ayıklama için örnek küçük dosyaları kullanmanızı öneririz. Hata ayıklama ayarları ' na gidin ve geçici bir dosya kullanarak verilerinizin küçük bir alt kümesini işaret edin.

    ![Hata ayıklama ayarları](media/data-flow/debugsettings3.png "Hata ayıklama ayarları")

### <a name="file-naming-options"></a>Dosya adlandırma seçenekleri

Blob veya ADLS dosya deposu yazma veri akışları eşleme içinde dönüştürülmüş verileri yazmanın en yaygın yolu. Havuzınızda, adlandırılmış bir dosya değil, bir kapsayıcıya veya klasöre işaret eden bir veri kümesi seçmelisiniz. Eşleme veri akışı, yürütme için Spark kullandığından, çıktılarınız bölümlendirme düzeninize göre birden fazla dosyanın üzerine bölünür.

Ortak bölümlendirme şeması, tüm çıkış bölüm dosyalarını havuzinizdeki tek bir dosyada birleştiren _tek bir dosyaya çıkış_' ı seçmedir. Bu işlem, çıktının tek bir küme düğümünde tek bir bölüme azalttığını gerektirir. Birçok büyük kaynak dosyasını tek bir çıkış dosyasına birleştiriyorsanız, küme düğüm kaynakları ' nı çalıştırabilirsiniz.

İşlem düğümü kaynaklarını tüketmenin önüne geçmek için, varsayılan, en iyi duruma getirilmiş şemayı veri akışında tutun ve işlem hattınızda, tüm bölüm dosyalarını çıkış klasöründen yeni tek bir dosyaya birleştiren bir kopyalama etkinliği ekleyin. Bu teknik, dönüştürme eylemini dosya birleştirmeden ayırır ve _çıktıyı tek bir dosyaya_ayarla ile aynı sonuca erişir.

### <a name="looping-through-file-lists"></a>Dosya listeleri aracılığıyla döngü

Kaynak dönüştürme her etkinlik Için kullanarak döngü yerine birden çok dosya üzerinde yineleme yaparken, bir eşleme veri akışı daha iyi yürütülür. Kaynak dönüşümünüze joker karakter veya dosya listesi kullanmanızı öneririz. Veri akışı işlemi, bu döngünün Spark kümesi içinde oluşmasına izin vererek daha hızlı yürütülür. Daha fazla bilgi için bkz. [kaynak dönüşümünde joker karakter](connector-azure-data-lake-storage.md#mapping-data-flow-properties)kullanımı.

Örneğin, 2019 Temmuz 'dan BLOB depolama alanındaki bir klasörde işlemek istediğiniz veri dosyaları listeniz varsa, aşağıdaki kaynak dönüşümünüze kullanabileceğiniz bir joker karakterdir.

```DateFiles/*_201907*.txt```

Joker karakter kullanımı ' nı kullanarak, işlem hattınız yalnızca bir veri akışı etkinliği içerir. Bu, içinde bir veri akışı yürütme etkinliği içeren bir ForEach kullanarak tüm eşleşen dosyalarda yineleme yapan blob deposuna karşı bir aramanın daha iyi bir şekilde gerçekleştirilir.

### <a name="optimizing-for-cosmosdb"></a>CosmosDB için iyileştiriliyor

CosmosDB havuzları üzerinde üretilen iş ve Batch özelliklerinin ayarlanması yalnızca bu veri akışının işlem hattı veri akışı etkinliğinden yürütülmesi sırasında etkili olur. Özgün koleksiyon ayarları, veri akışı yürütmeden sonra CosmosDB tarafından kabul edilir.

* Toplu iş boyutu: verilerinizin kaba satır boyutunu hesaplayın ve rowSize * toplu iş boyutunun 2.000.000 ' den küçük olduğundan emin olun. Varsa, daha iyi aktarım hızı sağlamak için toplu iş boyutunu artırın
* Aktarım hızı: belgelerin CosmosDB 'ye daha hızlı yazmasını sağlamak için burada daha yüksek bir verimlilik ayarı ayarlayın. Lütfen yüksek bir verimlilik ayarına göre daha yüksek RU maliyetlerine göz önünde bulundurun.
*   Yazma aktarım hızı bütçesi: dakikada toplam ru 'dan küçük olan bir değer kullanın. Çok sayıda Spark bölümünün bulunduğu bir veri akışınız varsa, bir bütçe üretilen işi ayarlandığında, bu bölümlerde daha fazla dengelemek olur.

## <a name="join-performance"></a>Performansa katılarak

Veri akışınızdaki birleşimlerin performansını yönetmek, veri dönüştürmelerinizin yaşam döngüsü boyunca gerçekleştirdiğiniz çok yaygın bir işlemdir. ADF 'de, bu işlemler Spark 'ta Karma birleşimler olarak gerçekleştirilirken veri akışları verilerin birleşimlerden önce sıralanmasını gerektirmez. Bununla birlikte, "yayın" JOIN iyileştirmesi sayesinde geliştirilmiş performans avantajlarından yararlanabilirsiniz. Bu, JOIN ilişkiniz her iki tarafının içeriğini Spark düğümüne ileterek karışık bir şekilde kaçınacaktır. Bu, başvuru aramaları için kullanılan daha küçük tablolar için iyi bir sonuç verir. Düğümün belleğine sığamayacak olabilecek daha büyük tablolar yayın iyileştirmesi için iyi aday değildir.

Başka bir birleştirme iyileştirmesi, birleşimlerinizi çapraz birleştirmeleri uygulamak için Spark 'un kullanım zamanlarını önleyecağından bu şekilde derlemenize olanak sağlar. Örneğin, JOIN koşullarınıza sabit değer değerleri dahil ettiğinizde Spark, önce tam bir Kartezyen ürün gerçekleştirmeye yönelik bir gereksinim olarak görebilir, sonra da birleştirilmiş değerleri filtreleyebiliriz. Ancak, birleştirme koşullarınızın her iki tarafında da sütun değerlerine sahip olduğunuzdan emin olduğunuzda, bu Spark kaynaklı Kartezyen ürünü kullanmaktan kaçınabilir ve birleşimlerinizin ve veri akışlarınızın performansını geliştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Performansla ilgili diğer veri akışı makalelerine bakın:

- [Veri akışı Iyileştirme sekmesi](concepts-data-flow-overview.md#optimize)
- [Veri akışı etkinliği](control-flow-execute-data-flow-activity.md)
- [Veri akışı performansını izleme](concepts-data-flow-monitoring.md)
