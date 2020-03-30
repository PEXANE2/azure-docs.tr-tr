---
title: Veri eğriltme çözümle - Visual Studio için Azure Veri Gölü Araçları
description: Visual Studio için Azure Veri Gölü Araçlarını kullanarak veri eğriliği sorunları için olası çözümleri sorun giderme.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 12/16/2016
ms.openlocfilehash: 9ff7ba5f04a8c1862f8ef136f8f3f6900f00a431
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71802546"
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a>Visual Studio için Azure Data Lake Araçları’nı kullanarak veri eğimi sorunlarını çözme

## <a name="what-is-data-skew"></a>Veri eğriltme nedir?

Kısaca belirtildiği gibi, veri çarpıklık aşırı temsil edilen bir değerdir. Vergi beyannamelerini denetlemek için 50 vergi incelemesi atadığınızı düşünün, her ABD eyaleti için bir sınav görevlisi. Wyoming müfettişi, çünkü oradaki nüfus çok az, yapacak çok az şeyi var. Kaliforniya'da, ancak, sınav eyaletin büyük nüfusu nedeniyle çok meşgul tutulur.
    ![Veri eğriltme sorunu örneği](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)

Bizim senaryomuzda, veriler tüm vergi incelemecilerine eşit olmayan bir şekilde dağıtılır, bu da bazı sınav görevlilerinin diğerlerinden daha fazla çalışması gerektiği anlamına gelir. Kendi işinizde, sık sık burada vergi inceleme örneği gibi durumlar la karşılaşırsınız. Daha teknik açıdan, bir tepe noktası akranlarından çok daha fazla veri alır, tepe noktasının diğerlerinden daha fazla çalışmasını sağlayan ve sonunda tüm işi yavaşlatan bir durum. Daha da kötüsü, iş başarısız olabilir, çünkü vertices olabilir, örneğin, 5 saatlik çalışma süresi sınırlaması ve 6 GB bellek sınırlaması.

## <a name="resolving-data-skew-problems"></a>Veri eğriltme sorunlarını çözme

Visual Studio için Azure Veri Gölü Araçları, işinizde veri eğriliği sorunu olup olmadığını algılamaya yardımcı olabilir. Bir sorun varsa, bu bölümdeki çözümleri deneyerek çözebilirsiniz.

## <a name="solution-1-improve-table-partitioning"></a>Çözüm 1: Tablo bölümleme geliştirin

### <a name="option-1-filter-the-skewed-key-value-in-advance"></a>Seçenek 1: Çarpık anahtar değerini önceden filtreleyin

İş mantığınızı etkilemiyorsa, yüksek frekans değerlerini önceden filtreleyebilirsiniz. Örneğin, GUID sütununda 000-000-000 çok fazla varsa, bu değeri toplamak istemeyebilirsiniz. Toplamdan önce, yüksek frekans değerini filtrelemek için "WHERE GUID != "000-000-000"" yazabilirsiniz.

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a>Seçenek 2: Farklı bir bölüm veya dağıtım anahtarı seçin

Önceki örnekte, yalnızca ülke/bölge üzerindeki vergi denetimi iş yükünü denetlemek istiyorsanız, anahtar olarak kimlik numarasını seçerek veri dağıtımını geliştirebilirsiniz. Farklı bir bölüm veya dağıtım anahtarı seçmek bazen verileri daha eşit dağıtabilir, ancak bu seçimin işletme mantığınızı etkilemediğinden emin olmanız gerekir. Örneğin, her durum için vergi toplamını hesaplamak _için, Bölüm_ anahtarı olarak Durumu belirtmek isteyebilirsiniz. Bu sorunla yaşamaya devam ederseniz, Seçenek 3'u kullanmayı deneyin.

### <a name="option-3-add-more-partition-or-distribution-keys"></a>Seçenek 3: Daha fazla bölüm veya dağıtım anahtarı ekleme

Yalnızca _Durum'u_ bölüm anahtarı olarak kullanmak yerine, bölümleme için birden fazla anahtar kullanabilirsiniz. Örneğin, veri bölme boyutlarını azaltmak ve verileri daha eşit dağıtmak için _posta kodu_ eklemeyi ek bir bölüm anahtarı olarak eklemeyi düşünün.

### <a name="option-4-use-round-robin-distribution"></a>Seçenek 4: Round-robin dağılımını kullanın

Bölüm ve dağıtım için uygun bir anahtar bulamazsanız, round-robin dağıtım kullanmayı deneyebilirsiniz. Round-robin dağılımı tüm satırları eşit olarak sıralar ve rasgele karşılık gelen kovalar içine koyar davranır. Veriler eşit olarak dağıtılır, ancak bazı işlemler için iş performansını da azaltabilecek bir dezavantaj olan yerellik bilgilerini kaybeder. Ayrıca, yine de çarpık anahtar için toplama yapıyorsanız, veri çarpıklık sorunu devam edecektir. Round-robin dağıtımı hakkında daha fazla bilgi edinmek için [CREATE TABLE (U-SQL): Şema ile tablo oluşturma](/u-sql/ddl/tables/create/managed/create-table-u-sql-creating-a-table-with-schema#dis_sch)bölümündeu-SQL Tablo Dağılımları bölümüne bakın.

## <a name="solution-2-improve-the-query-plan"></a>Çözüm 2: Sorgu planını geliştirin

### <a name="option-1-use-the-create-statistics-statement"></a>Seçenek 1: CREATE STATISTICS deyimini kullanın

U-SQL tablolarda CREATE Statistics deyimini sağlar. Bu deyim, bir tabloda depolanan değer dağılımı gibi veri özellikleri hakkında sorgu optimize ediciye daha fazla bilgi verir. Çoğu sorgu için sorgu optimize edici zaten yüksek kaliteli bir sorgu planı için gerekli istatistikleri oluşturur. Bazen, CREATE STATISTICS ile ek istatistikler oluşturarak veya sorgu tasarımını değiştirerek sorgu performansını artırmanız gerekebilir. Daha fazla bilgi için [CREATE STATISTICS (U-SQL)](/u-sql/ddl/statistics/create-statistics) sayfasına bakın.

Kod örneği:

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
>İstatistik bilgileri otomatik olarak güncelleştirilmeyecek. İstatistikleri yeniden oluşturmadan tablodaki verileri güncellerseniz, sorgu performansı düşebilir.

### <a name="option-2-use-skewfactor"></a>Seçenek 2: SKEWFACTOR kullanın

Her durum için vergiyi toplamamak istiyorsanız, veri eğriltme sorununu önlemeyen bir yaklaşım olan GROUP BY state'i kullanmanız gerekir. Ancak, en iyi duruma getiricinin sizin için bir yürütme planı hazırlayabilmesi için anahtarlarda veri eğriliğini belirlemek için sorgunuzda bir veri ipucu sağlayabilirsiniz.

Genellikle, 0,5 ve 1 olarak parametre ayarlayabilirsiniz, 0,5 çok çarpık ve 1 ağır çarpık anlamına gelir. İpucu, geçerli deyim ve tüm alt akış ifadeleri için yürütme planı optimizasyonunu etkilediğinden, olası çarpık anahtar akıllıca toplamadan önce ipucunu eklediğinizden emin olun.

    SKEWFACTOR (columns) = x

    Provides a hint that the given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

Kod örneği:

    //Add a SKEWFACTOR hint.
    @Impressions =
        SELECT * FROM
        searchDM.SML.PageView(@start, @end) AS PageView
        OPTION(SKEWFACTOR(Query)=0.5)
        ;

    //Query 1 for key: Query, ClientId
    @Sessions =
        SELECT
            ClientId,
            Query,
            SUM(PageClicks) AS Clicks
        FROM
            @Impressions
        GROUP BY
            Query, ClientId
        ;

    //Query 2 for Key: Query
    @Display =
        SELECT * FROM @Sessions
            INNER JOIN @Campaigns
                ON @Sessions.Query == @Campaigns.Query
        ;   

### <a name="option-3-use-rowcount"></a>Seçenek 3: ROWCOUNT'u kullanın  
SKEWFACTOR'e ek olarak, belirli çarpık anahtar birleştirme örnekleri için, diğer birleştirilmiş satır kümesinin küçük olduğunu biliyorsanız, JOIN'ten önce U-SQL deyimine bir ROWCOUNT ipucu ekleyerek en iyi duruma getiriciyi söyleyebilirsiniz. Bu şekilde, optimize edici performansı artırmaya yardımcı olmak için bir yayın birleştirme stratejisi seçebilir. ROWCOUNT'un veri eğriliği sorununu çözmediğini, ancak bazı ek yardımlar sunabileceğini unutmayın.

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

Kod örneği:

    //Unstructured (24-hour daily log impressions)
    @Huge   = EXTRACT ClientId int, ...
                FROM @"wasb://ads@wcentralus/2015/10/30/{*}.nif"
                ;

    //Small subset (that is, ForgetMe opt out)
    @Small  = SELECT * FROM @Huge
                WHERE Bing.ForgetMe(x,y,z)
                OPTION(ROWCOUNT=500)
                ;

    //Result (not enough information to determine simple broadcast JOIN)
    @Remove = SELECT * FROM Bing.Sessions
                INNER JOIN @Small ON Sessions.Client == @Small.Client
                ;

## <a name="solution-3-improve-the-user-defined-reducer-and-combiner"></a>Çözüm 3: Kullanıcı tanımlı azaltıcıyı ve birleşimciyi geliştirin

Bazen karmaşık işlem mantığıyla başa çıkmak için kullanıcı tanımlı bir işleç yazabilirsiniz ve iyi yazılmış bir azaltıcı ve birleşimci bazı durumlarda veri eğriliği sorununu azaltabilir.

### <a name="option-1-use-a-recursive-reducer-if-possible"></a>Seçenek 1: Mümkünse özyinelemeli bir redüktör kullanın

Varsayılan olarak, kullanıcı tanımlı bir indirgeme, özyinelemeli olmayan modda çalışır, bu da bir anahtarın çalışmasını azaltmanın tek bir tepe ye dağıtıldığı anlamına gelir. Ancak verileriniz çarpıksa, büyük veri kümeleri tek bir tepe de işlenebilir ve uzun süre çalıştırılabilir.

Performansı artırmak için, özyinelemeli modda çalışacak azaltıcıyı tanımlamak için kodunuza bir öznitelik ekleyebilirsiniz. Daha sonra, büyük veri kümeleri birden çok vertices dağıtılabilir ve işinizi hızlandırır paralel olarak çalıştırın.

Özyinelemeli olmayan bir azaltıcıyı özyinelemeli olarak değiştirmek için algoritmanızın birleştirici olduğundan emin olmanız gerekir. Örneğin, toplam birleştiricidir ve ortanca değildir. Ayrıca, redüktör için giriş ve çıktının aynı şemayı tuttuğundan emin olmanız gerekir.

Özyinelemeli indirgeyicinin özniteliği:

    [SqlUserDefinedReducer(IsRecursive = true)]

Kod örneği:

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a>Seçenek 2: Mümkünse satır düzeyinde birleşim modunu kullanın

Belirli çarpık anahtar birleştirme servis talepleri için ROWCOUNT ipucuna benzer şekilde, birleşim modu, çalışmanın aynı anda yürütülebilmeleri için büyük çarpık anahtar değer kümelerini birden çok vertices'e dağıtmaya çalışır. Birleşim modu veri eğriliği sorunlarını çözemez, ancak büyük çarpık anahtar değer kümeleri için bazı ek yardım sunabilir.

Varsayılan olarak, birleşim modu Tam'dır, bu da sol satır kümesi ve sağ satır kümesinin ayrılamayacağı anlamına gelir. Modu Sol/Sağ/İç olarak ayarlamak satır düzeyinde birleştirme sağlar. Sistem, karşılık gelen satır kümelerini ayırır ve bunları paralel olarak çalışan birden çok vertices dağıtır. Ancak, birleşim modunu yapılandırmadan önce, ilgili satır kümelerinin ayrıladığından emin olmak için dikkatli olun.

Aşağıdaki örnek, ayrılmış bir sol satır kümesini gösterir. Her çıktı satırı soldan tek bir giriş satırına bağlıdır ve potansiyel olarak aynı anahtar değerine sahip sağdaki tüm satırlara bağlıdır. Birleşim modunu sol olarak ayarlarsanız, sistem büyük sol satır kümesini küçük kümelere ayırır ve bunları birden çok tepe ucuna atar.

![Birleşim modu çizimi](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
>Yanlış birleşim modunu ayarlarsanız, kombinasyon daha az verimlidir ve sonuçlar yanlış olabilir.

Birleşim modunun öznitelikleri:

- SqlUserDefinedCombiner(Mode=CombinerMode.Full): Her çıkış satırı potansiyel olarak aynı anahtar değerine sahip soldan ve sağdaki tüm giriş satırlarına bağlıdır.

- SqlUserDefinedCombiner(Mode=CombinerMode.Left): Her çıkış satırı soldan tek bir giriş satırına bağlıdır (ve potansiyel olarak sağdan aynı anahtar değerine sahip tüm satırlar).

- qlUserDefinedCombiner(Mode=CombinerMode.Right): Her çıkış satırı sağdan tek bir giriş satırına bağlıdır (ve potansiyel olarak soldan aynı anahtar değerine sahip tüm satırlar).

- SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Her çıkış satırı, aynı değere sahip soldan ve sağdan tek bir giriş satırına bağlıdır.

Kod örneği:

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }
