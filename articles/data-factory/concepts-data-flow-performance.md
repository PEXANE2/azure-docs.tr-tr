---
title: Veri akışı performansını ve atoklama kılavuzunu eşleme
description: Azure Veri Fabrikası'nda veri akışlarının eşleme performansını etkileyen önemli etkenler hakkında bilgi edinin.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.custom: seo-lt-2019
ms.date: 03/11/2020
ms.openlocfilehash: 4baf7974bdb0a5efe4cb556e820e9d13aeac5d8a
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409853"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Veri akışlarını eşleme performansı ve atoklama kılavuzu

Azure Veri Fabrikası'ndaki Veri Akışlarını Eşleme, veri dönüşümlerini ölçekte tasarlamak, dağıtmak ve düzenlemek için kodsuz bir arabirim sağlar. Veri akışlarını eşleme konusunda aşina değilseniz, [Veri Akışına Genel Bakış Eşleme'ye](concepts-data-flow-overview.md)bakın.

ADF UX'den Veri Akışları tasarlarken ve test ederken, bir kümenin ısınmasını beklemeden veri akışlarınızı gerçek zamanlı olarak yürütmek için hata ayıklama modunu açtığınızdan emin olun. Daha fazla bilgi için [Hata Ayıklama Modu'na](concepts-data-flow-debug-mode.md)bakın.

Bu video, veri akışları ile veri dönüştüren bazı örnek zamanlamaları gösterir:
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4rNxM]

## <a name="monitoring-data-flow-performance"></a>Veri akışı performansını izleme

Eşleme veri akışları tasarlarken, yapılandırma panelindeki veri önizleme sekmesine tıklayarak her dönüşümü birim olarak test edebilirsiniz. Mantığınızı doğruladıktan sonra, veri akışınızı bir ardışık işlem olarak uçuça test edin. Veri Akışını Yürüt etkinliği ekleyin ve veri akışınızın performansını test etmek için Hata Ayıklama düğmesini kullanın. Veri akışınızın yürütme planını ve performans profilini açmak için, ardınızdaki çıkış sekmesindeki 'eylemler' altındaki gözlük simgesine tıklayın.

![Veri Akış Monitörü](media/data-flow/mon002.png "Veri Akış Monitörü 2")

 Bu bilgileri, veri akışınızın farklı boyutlu veri kaynaklarına karşı performansını tahmin etmek için kullanabilirsiniz. Daha fazla bilgi için [bkz.](concepts-data-flow-monitoring.md)

![Veri Akışını İzleme](media/data-flow/mon003.png "Veri Akış Monitörü 3")

 Ardışık hata ayıklama çalıştırmaları için, genel performans hesaplamalarınızda yaklaşık bir dakikalık küme kurulum süresi, sıcak bir küme için gereklidir. Varsayılan Azure Tümleştirme Çalışma Süresini başlangıç olarak başlıyorsanız, döndürme süresi yaklaşık 5 dakika sürebilir.

## <a name="increasing-compute-size-in-azure-integration-runtime"></a>Azure Tümleştirme Çalışma Süresi'nde işlem boyutunu artırma

Daha fazla çekirdekiçeren Tümleştirme Çalışma Süresi, Spark bilgi işlem ortamlarında düğüm sayısını artırır ve verilerinizi okumak, yazmak ve dönüştürmek için daha fazla işlem gücü sağlar.
* İşlem hızınızın giriş hızınızdan daha yüksek olmasını **istiyorsanız, Bilgi İşlem En İyi Duruma Getirilmiş** kümeyi deneyin.
* Bellekte daha fazla veri önbelleğe almak istiyorsanız **Bellek En İyi Duruma Getirilmiş** kümeyi deneyin. Bellek optimize compute Optimize daha çekirdek başına daha yüksek bir fiyat puanı vardır, ancak büyük olasılıkla daha hızlı dönüşüm hızları neden olur.

![Yeni IR](media/data-flow/ir-new.png "Yeni IR")

Tümleştirme Çalışma Zamanı'nı nasıl oluşturabilirsiniz hakkında daha fazla bilgi için [Azure Veri Fabrikası'nda Tümleştirme Çalışma Zamanı'na](concepts-integration-runtime.md)bakın.

### <a name="increase-the-size-of-your-debug-cluster"></a>Hata ayıklama kümenizin boyutunu artırma

Varsayılan olarak, hata ayıklama yı açmak, her veri fabrikası için otomatik olarak oluşturulan varsayılan Azure Tümleştirme çalışma süresini kullanır. Bu varsayılan Azure IR, Genel İşlem özelliklerini kullanarak sekiz çekirdek, dört sürücü düğümü ve dört işçi düğümü için ayarlanır. Daha büyük verilerle test ederken, hata ayıklama kümenizin boyutunu daha büyük yapılandırmalara sahip bir Azure IR oluşturarak artırabilir ve hata ayıklama'yı açtığınızda bu yeni Azure IR'sini seçebilirsiniz. Bu, ADF'ye veri önizlemesi ve veri akışlarıyla birlikte veri hata ayıklama için bu Azure IR'sini kullanmasını emredecektir.

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Azure SQL Veritabanı ve Azure SQL Veri Ambarı için optimizasyon

### <a name="partitioning-on-source"></a>Kaynakta bölümleme

1. **Optimize et** sekmesine gidin ve **Bölümlemeyi Ayarla'yı** seçin
1. **Kaynak'ı**seçin.
1. **Bölüm Sayısı'na**göre, Azure SQL DB'nize en fazla bağlantı sayısını ayarlayın. Veritabanınıza paralel bağlantılar elde etmek için daha yüksek bir ayar deneyebilirsiniz. Ancak, bazı durumlarda sınırlı sayıda bağlantı yla daha hızlı performans neden olabilir.
1. Belirli bir tablo sütununa mı yoksa sorguyla mı bölümleştirilmeyeceğini seçin.
1. **Sütun'u**seçtiyseniz, bölüm sütununa seçin.
1. **Sorgu'yu**seçtiyseniz, veritabanı tablonuzun bölümleme düzenine uyan bir sorgu girin. Bu sorgu, kaynak veritabanı altyapısının bölüm elemeden yararlanmasını sağlar. Kaynak veritabanı tablolarınızın bölümlenmiş olması gerekmez. Kaynağınız zaten bölümlenmemişse, ADF Kaynak dönüşümünde seçtiğiniz anahtara bağlı olarak Spark dönüşüm ortamında veri bölümleme kullanmaya devam eder.

![Kaynak Bölüm](media/data-flow/sourcepart3.png "Kaynak Bölüm")

> [!NOTE]
> Kaynağınız için bölüm sayısını seçmenize yardımcı olacak iyi bir kılavuz, Azure Tümleştirme Çalışma süreniz için ayarladığınız çekirdek sayısını temel alınve bu sayıyı beşle çarpar. Örneğin, ADLS klasörlerinizde bir dizi dosyayı dönüştürüyorsanız ve 32 çekirdekli bir Azure IR kullanacaksanız, hedeflediğiniz bölüm sayısı 32 x 5 = 160 bölümdür.

### <a name="source-batch-size-input-and-isolation-level"></a>Kaynak toplu iş boyutu, giriş ve yalıtım düzeyi

Kaynak dönüşümünde **Kaynak Seçenekleri** altında, aşağıdaki ayarlar performansı etkileyebilir:

* Toplu iş boyutu, ADF'ye verileri satır satır yerine Spark belleğinde kümelerde depolamasını bildirir. Toplu iş boyutu isteğe bağlı bir ayardır ve düzgün boyutlandırılamıyorlarsa, işlem düğümlerinde kaynaklar tükenebilir. Bu özelliği ayarlamamak, Spark önbelleğe alma toplu varsayılankullanır.
* Sorgu ayarlama, işlem için Veri Akışı'na gelmeden önce satırlarını kaynaktaki filtrelere filtrelemenize olanak sağlar. Bu, ilk veri toplamayı hızlandırabilir. Bir sorgu kullanıyorsanız, Azure SQL DB'niz için READ UNCOMMITTED gibi isteğe bağlı sorgu ipuçları ekleyebilirsiniz.
* İşlenmemiş okuma, Kaynak dönüşümünde daha hızlı sorgu sonuçları sağlar

![Kaynak](media/data-flow/source4.png "Kaynak")

### <a name="sink-batch-size"></a>Lavabo toplu boyutu

Veri akışlarınızın satır satır işlenmesini önlemek için, Azure SQL DB ve Azure SQL DW lavabolarının Ayarlar sekmesinde **Toplu Iş boyutu** ayarlayın. Toplu iş boyutu ayarlanırsa, ADF veritabanı sağlanan boyuta bağlı olarak toplu olarak yazar. Bu özelliği ayarlamamak, Spark önbelleğe alma toplu varsayılankullanır.

![Havuz](media/data-flow/sink4.png "Havuz")

### <a name="partitioning-on-sink"></a>Lavaboda bölümleme

Hedef tablolarınızda verilerinizi bölümlememiş olsanız bile, verilerinizin lavabo dönüşümünde bölümletilen önerilir. Bölümlenmiş veriler genellikle tüm bağlantıları tek bir düğüm/bölüm kullanmaya zorlamanın üzerinden çok daha hızlı yüklemeyle sonuçlanır. Lavabonuzun Optimize sekmesine gidin ve lavabonuza yazacak ideal bölüm sayısını seçmek için *Round Robin* bölümlemeyi seçin.

### <a name="disable-indexes-on-write"></a>Yazma dizinlerini devre dışı

Ardışık bilgisayarınızda, Veri Akışı etkinliğinizden önce, Lavabonuzdan yazılan hedef tablolarınızdaki dizinleri devre dışı bırakan bir [Saklı Yordam etkinliği](transform-data-using-stored-procedure.md) ekleyin. Veri Akışı etkinliğininizden sonra, bu dizinleri etkinleştiren başka bir Saklı Yordam etkinliği ekleyin. Veya bir veritabanı lavaboön işleme ve post-processing komut dosyaları kullanmak.

### <a name="increase-the-size-of-your-azure-sql-db-and-dw"></a>Azure SQL DB ve DW'nizin boyutunu artırma

Kaynaklarınızın yeniden boyutlandırılmasını zamanlayın ve dtu sınırlarına ulaştığınızda iş hacmini artırmak ve Azure azaltmayı en aza indirmek için boru hattınız çalışmadan önce Azure SQL DB ve DW'yi batırın. Ardışık hatlar yürütmeniz tamamlandıktan sonra, veritabanlarınızı normal çalışma hızına geri boyutlandırın.

* Sql DB kaynak tablosu 887k satır ve 74 sütun ile SQL DB tabloya tek bir türetilmiş sütun dönüşümü ile bellek optimize 80 çekirdekli hata ayıklama Azure IRs kullanarak yaklaşık 3 dakika uçtan uca alır.

### <a name="azure-synapse-sql-dw-only-use-staging-to-load-data-in-bulk-via-polybase"></a>[Yalnızca Azure Synapse SQL DW] Polybase üzerinden verileri toplu olarak yüklemek için hazırlamayı kullanın

DW'nize satır satır eklemelerden kaçınmak için, ADF'nin [PolyBase'i](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)kullanabilmesi için Lavabo ayarlarınızda **hazırlamayı etkinleştir'i** işaretleyin. PolyBase, ADF'nin verileri toplu olarak yüklemesine olanak tanır.
* Veri akışı etkinliğinizi bir ardışık kaynaktan yürüttüğünüzde, toplu yükleme sırasında verilerinizi aşamalı olarak bir Blob veya ADLS Gen2 depolama konumu seçmeniz gerekir.

* Bir Synapse tablosuna 74 sütun ve tek türetilen sütun dönüşümü ile 421Mb dosya kaynağı bellek optimize 80 çekirdekli hata ayıklama Azure IRs kullanarak yaklaşık 4 dakika uçtan uca alır.

## <a name="optimizing-for-files"></a>Dosyaları en iyi duruma alma

Her dönüşümde, veri fabrikasının Optimize etme sekmesinde kullanmasını istediğiniz bölümleme düzenini ayarlayabilirsiniz. İlk test dosya tabanlı lavabolar varsayılan bölümleme ve optimizasyonlar tutmak için iyi bir uygulamadır.

* Daha küçük dosyalar için, daha az bölüm seçmenin bazen Spark'tan küçük dosyalarınızı bölmesini istemekten daha iyi ve daha hızlı çalışabileceğini görebilirsiniz.
* Kaynak verileriniz hakkında yeterli bilgiye sahip değilseniz, *Round Robin* bölümleme'yi seçin ve bölüm sayısını ayarlayın.
* Verilerinizin iyi karma tuşları olabilecek sütunları varsa, *Karma bölümleme'yi*seçin.

* 74 sütunlu 421Mb dosya nın dosya batması ve tek bir türetilmiş sütun dönüşümü, en iyi duruma getirilmiş 80 çekirdekli hata ayıklama Azure IRs bellekkullanarak yaklaşık 2 dakika uçtan uca alır.

Veri önizleme ve satır hata ayıklama hata ayıklama, dosya tabanlı kaynak veri kümeleri için sınır ve örnekleme boyutları yalnızca satır sayısı değil, döndürülen satır sayısı için geçerlidir. Bu, hata ayıklama yürütmelerinizin performansını etkileyebilir ve büyük olasılıkla akışın başarısız lığa neden olabilir.
* Hata ayıklama kümeleri varsayılan olarak küçük tek düğümlü kümelerdir ve hata ayıklama için örnek küçük dosyaları kullanmanızı öneririz. Hata Ayıklama Ayarları'na gidin ve geçici bir dosya kullanarak verilerinizin küçük bir alt kümesini işaret edin.

    ![Hata Ayıklama Ayarları](media/data-flow/debugsettings3.png "Hata Ayıklama Ayarları")

### <a name="file-naming-options"></a>Dosya adlandırma seçenekleri

Blob veya ADLS dosya deposu yazma veri akışları eşleme dönüştürülmüş veri yazmak için en yaygın yolu. Lavabonuzda, adlandırılmış bir dosyayı değil, bir kapsayıcıyı veya klasörü işaret eden bir veri kümesi seçmeniz gerekir. Veri akışının eşlemesi yürütme için Spark'ı kullandığından, çıktınız bölümleme düzeninize bağlı olarak birden çok dosyaya bölünür.

Ortak bir bölümleme düzeni, tüm çıktı PART dosyalarını lavabonuzdaki tek bir dosyada birleştiren _çıktıyı tek_bir dosyaya seçmektir. Bu işlem, çıktının tek bir küme düğümünde tek bir bölüme inmesini gerektirir. Birçok büyük kaynak dosyayı tek bir çıktı dosyasında birleştiriyorsanız küme düğümü kaynaklarıtünuzun tükenebilir.

Bilgi işlem düğümü kaynaklarını tüketmekten kaçınmak için, varsayılan, en iyi duruma getirilmiş düzeni veri akışında tutun ve çıktı klasöründeki tüm PART dosyalarını yeni bir dosyayla birleştiren bir Kopyalama Etkinliği ekleyin. Bu teknik, dönüştürme eylemini dosya birleştirme den ayırır ve _Çıktı'yı tek bir dosyaya_ayarlarken aynı sonuca ulaşır.

### <a name="looping-through-file-lists"></a>Dosya listelerinde döngü oluşturma

Kaynak dönüştürme, Her Etkinlik Için üzerinden döngü yerine birden çok dosya üzerinde yinelendiğinde eşleme liveri akışı daha iyi yürütülür. Kaynak dönüşümde joker karakterler veya dosya listeleri kullanmanızı öneririz. Veri Akışı işlemi, döngünün Kıvılcım kümesi içinde oluşmasına izin vererek daha hızlı yürütülür. Daha fazla bilgi için [Kaynak Dönüşümünde Joker Karakter'e](connector-azure-data-lake-storage.md#mapping-data-flow-properties)bakın.

Örneğin, Blob Depolama'daki bir klasörde işlemek istediğiniz Temmuz 2019'a ait veri dosyalarının bir listesi varsa, kaynak dönüşümünüzde kullanabileceğiniz bir joker karakter aşağıdadır.

```DateFiles/*_201907*.txt```

Joker karakter kullanarak, ardışık hattınız yalnızca bir Veri Akışı etkinliği içerir. Bu, blob mağazasına karşı yapılan bir aramadan daha iyi performans gösterir ve daha sonra içinde Veri Akışı Gerçekleştirme etkinliği olan bir ForEach kullanarak eşleşen tüm dosyaları yineler.

### <a name="optimizing-for-cosmosdb"></a>CosmosDB için optimizasyon

CosmosDB lavabolarında iş ve toplu özelliklerin ayarlanması yalnızca bir boru hattı veri akışı etkinliğinden gelen veri akışının yürütülmesi sırasında etkili olur. Orijinal toplama ayarları, veri akışı yürütmenizden sonra CosmosDB tarafından onurlandırılacaktır.

* Toplu iş boyutu: Verilerinizin kaba satır boyutunu hesaplayın ve satır Boyutu * toplu iş boyutunun iki milyondan az olduğundan emin olun. Eğer varsa, daha iyi iş elde etmek için toplu iş boyutunu artırın
* İşlem: Belgelerin CosmosDB'ye daha hızlı yazabilmesi için burada daha yüksek bir iş ortamı ayarını ayarlayın. Lütfen yüksek iş elde ayarına dayalı yüksek RU maliyetlerini aklınızda bulundurun.
*   İş İbzül Bütçesini Yaz: Dakikada toplam RUS'tan daha küçük bir değer kullanın. Çok sayıda Kıvılcım bölümüne sahip bir veri akışınız varsa, bütçe verisi ayarlamak bu bölümler arasında daha fazla denge sağlar.

## <a name="join-performance"></a>Performansa katılma

Veri akışınızdaki birleştirmelerin performansını yönetmek, veri dönüşümlerinizin yaşam döngüsü boyunca gerçekleştireceğiniz çok yaygın bir işlemdir. ADF'de, bu işlemler Spark'ta karma birleştirme olarak gerçekleştirildiği için veri akışlarının birleştirmelerden önce sıralanmalarını gerektirmez. Ancak, "Yayın" Join optimizasyonu ile geliştirilmiş performanstan yararlanabilirsiniz. Bu, birleştirme ilişkinizin her iki tarafının içeriğini Kıvılcım düğümüne iterek karışıklığı önler. Bu, başvuru aramaları için kullanılan daha küçük tablolar için iyi çalışır. Düğümün belleğine sığmayan daha büyük tablolar yayın optimizasyonu için iyi adaylar değildir.

Başka bir Join optimizasyonu, spark'ın çapraz birleştirmeleri uygulama eğilimini önleyecek şekilde birleştirmelerinizi oluşturmaktır. Örneğin, birleştirme koşullarınıza gerçek değerleri eklediğinizde, Spark bunu önce tam kartezyen bir ürünü gerçekleştirmek için bir gereklilik olarak görebilir, ardından birleştirilmiş değerleri filtreleyebilir. Ancak, birleştirme koşulunuzun her iki tarafında da sütun değerlerinin olduğundan emin olursanız, bu Kıvılcım kaynaklı kartezyen üründen kaçınabilir ve birleşimlerinizin ve veri akışlarınızın performansını artırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Performansla ilgili diğer Veri Akışı makalelerine bakın:

- [Veri Akışı Optimize Sekmesi](concepts-data-flow-overview.md#optimize)
- [Veri Akışı etkinliği](control-flow-execute-data-flow-activity.md)
- [Veri Akışı performansını izleyin](concepts-data-flow-monitoring.md)
