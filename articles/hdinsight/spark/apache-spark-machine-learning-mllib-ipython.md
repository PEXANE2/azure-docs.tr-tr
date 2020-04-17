---
title: HDInsight'ta Spark MLlib ile makine öğrenimi örneği - Azure
description: Lojistik regresyon yoluyla sınıflandırmayı kullanarak bir veri kümesini analiz eden bir makine öğrenme uygulaması oluşturmak için Spark MLlib'i nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 04/16/2020
ms.openlocfilehash: 26695df299ba5d0f50c8f271b5da99284a8d6764
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81531142"
---
# <a name="use-apache-spark-mllib-to-build-a-machine-learning-application-and-analyze-a-dataset"></a>Bir makine öğrenme uygulaması oluşturmak ve bir veri kümesini analiz etmek için Apache Spark MLlib'i kullanın

Bir makine öğrenme uygulaması oluşturmak için Apache Spark [MLlib'i](https://spark.apache.org/mllib/) nasıl kullanacağınızı öğrenin. Uygulama, açık bir veri kümesi üzerinde tahmine dayalı analizler yapar. Bu örnek, Spark'ın yerleşik makine öğrenimi kitaplıklarından lojistik regresyon yoluyla *sınıflandırmayı* kullanır.

MLlib, makine öğrenimi görevleri için yararlı olan birçok yardımcı program sağlayan temel bir Spark kitaplığıdır:

* Sınıflandırma
* Regresyon
* Kümeleme
* Modelleme
* Tekil değer ayrıştırma (SVD) ve ana bileşen analizi (PCA)
* Hipotez testi ve örneklem istatistiklerinin hesaplanması

## <a name="understand-classification-and-logistic-regression"></a>Sınıflandırmayı ve lojistik gerilemeyi anlama

Popüler bir makine öğrenimi görevi olan *sınıflandırma,* giriş verilerini kategorilere ayırma işlemidir. Sağladığınız giriş verilerine "etiketler" nasıl atayacağınızı bulmak bir sınıflandırma algoritmasının işidir. Örneğin, stok bilgilerini giriş olarak kabul eden bir makine öğrenme algoritması düşünebilirsiniz. Daha sonra hisse senetlerini iki kategoriye ayırır: satmanız gereken hisse senetleri ve tutmanız gereken hisse senetleri.

Lojistik regresyon sınıflandırma için kullandığınız algoritmadır. Spark'ın lojistik regresyon *API'si ikili sınıflandırma*veya girdi verilerini iki gruptan birine sınıflandırmak için yararlıdır. Lojistik gerilemeler hakkında daha fazla bilgi için [Vikipedi'ye](https://en.wikipedia.org/wiki/Logistic_regression)bakın.

Özetle, lojistik regresyon süreci *bir lojistik işlevi*üretir. Bir giriş vektörü bir gruba veya diğerine ait olma olasılığını tahmin etmek için işlevi kullanın.  

## <a name="predictive-analysis-example-on-food-inspection-data"></a>Gıda denetimi verilerinde tahmine dayalı analiz örneği

Bu örnekte, gıda denetim verileri **(Food_Inspections1.csv)** üzerinde bazı tahmine dayalı analizler yapmak için Spark'ı kullanırsınız. Chicago Şehri veri portalı ndan elde edilen [veriler.](https://data.cityofchicago.org/) Bu veri seti, Chicago'da yapılan gıda kuruluşu denetimleri hakkında bilgi içerir. Her kuruluş hakkında bilgi, bulunan ihlalleri (varsa) ve denetim sonuçları da dahil olmak üzere. CSV veri dosyası kümeyle ilişkili depolama hesabında **zaten mevcuttur /HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv.**

Aşağıdaki adımlarda, bir gıda denetiminden geçmek veya başarısız olmak için neler gerektiğini görmek için bir model geliştirirsiniz.

## <a name="create-an-apache-spark-mllib-machine-learning-app"></a>Apache Spark MLlib makine öğrenme uygulaması oluşturun

1. PySpark çekirdeği kullanarak bir Jupyter not defteri oluşturun. Yönergeler için bkz. [Jupyter not defteri oluşturma](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).

2. Bu uygulama için gereken türleri içeri aktarın. Aşağıdaki kodu boş bir hücreye kopyalayıp yapıştırın ve ardından **SHIFT + ENTER**tuşuna basın.

    ```PySpark
    from pyspark.ml import Pipeline
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml.feature import HashingTF, Tokenizer
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    ```

    PySpark çekirdeği nedeniyle, herhangi bir bağlam ı açıkça oluşturmanız gerekmez. İlk kod hücresini çalıştırdığınızda Kıvılcım ve Hive bağlamları otomatik olarak oluşturulur.

## <a name="construct-the-input-dataframe"></a>Giriş veri çerçevesini oluşturma

Ham CSV verilerini yapılandırılmamış metin olarak belleğe çekmek için Spark bağlamını kullanın. Ardından, verilerin her satırını ayrışdırmak için Python'un CSV kitaplığını kullanın.

1. Giriş verilerini içe aktarıp ayrıştırarak Esnek Dağıtılmış Veri Kümesi (RDD) oluşturmak için aşağıdaki satırları çalıştırın.

    ```PySpark
    def csvParse(s):
        import csv
        from StringIO import StringIO
        sio = StringIO(s)
        value = csv.reader(sio).next()
        sio.close()
        return value

    inspections = sc.textFile('/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                    .map(csvParse)
    ```

2. RDD'den bir satır almak için aşağıdaki kodu çalıştırın, böylece veri şemasına göz atabilirsiniz:

    ```PySpark
    inspections.take(1)
    ```

    Çıkış şöyle olur:

    ```
    [['413707',
        'LUNA PARK INC',
        'LUNA PARK  DAY CARE',
        '2049789',
        "Children's Services Facility",
        'Risk 1 (High)',
        '3250 W FOSTER AVE ',
        'CHICAGO',
        'IL',
        '60625',
        '09/21/2010',
        'License-Task Force',
        'Fail',
        '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
        '41.97583445690982',
        '-87.7107455232781',
        '(41.97583445690982, -87.7107455232781)']]
    ```

    Çıktı, giriş dosyasının şeması hakkında bir fikir verir. Her kuruluşun adını ve kuruluş türünü içerir. Ayrıca, adres, denetimlerin verileri ve konumu, diğer şeylerin yanı sıra.

3. Tahmine dayalı çözümleme için yararlı olan birkaç sütuniçeren bir veri çerçevesi (*df*) ve geçici bir tablo *(CountResults)* oluşturmak için aşağıdaki kodu çalıştırın. `sqlContext`yapılandırılmış veriler üzerinde dönüşümler yapmak için kullanılır.

    ```PySpark
    schema = StructType([
    StructField("id", IntegerType(), False),
    StructField("name", StringType(), False),
    StructField("results", StringType(), False),
    StructField("violations", StringType(), True)])

    df = spark.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
    df.registerTempTable('CountResults')
    ```

    Veri çerçevesi ilgi dört sütun **kimlik,** **isim**, **sonuçlar**ve **ihlalleri**vardır.

4. Verilerin küçük bir örneğini almak için aşağıdaki kodu çalıştırın:

    ```PySpark
    df.show(5)
    ```

    Çıkış şöyle olur:

    ```
    +------+--------------------+-------+--------------------+
    |    id|                name|results|          violations|
    +------+--------------------+-------+--------------------+
    |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
    |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
    |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
    |413708|BENCHMARK HOSPITA...|   Pass|                    |
    |413722|           JJ BURGER|   Pass|                    |
    +------+--------------------+-------+--------------------+
    ```

## <a name="understand-the-data"></a>Verileri anlama

Veri kümesinin neler içerdiğini anlamaya başlayalım. 

1. **Sonuçlar** sütunundaki farklı değerleri göstermek için aşağıdaki kodu çalıştırın:

    ```PySpark
    df.select('results').distinct().show()
    ```

    Çıkış şöyle olur:

    ```
    +--------------------+
    |             results|
    +--------------------+
    |                Fail|
    |Business Not Located|
    |                Pass|
    |  Pass w/ Conditions|
    |     Out of Business|
    +--------------------+
    ```

2. Bu sonuçların dağılımını görselleştirmek için aşağıdaki kodu çalıştırın:

    ```PySpark
    %%sql -o countResultsdf
    SELECT COUNT(results) AS cnt, results FROM CountResults GROUP BY results
    ```

    Ardından `%%sql` gelen `-o countResultsdf` büyü, sorguçıktısının Jupyter sunucusunda (genellikle kümenin baş düğümü) yerel olarak kalıcı olmasını sağlar. Çıktı belirtilen ad **sayımıResultsdf**ile [bir Pandas](https://pandas.pydata.org/) veri çerçevesi olarak devam etti. `%%sql` PySpark çekirdeği ile birlikte mevcut olan büyü ler ve diğer büyüler hakkında daha fazla bilgi [için, Apache Spark HDInsight kümelerine sahip Jupyter dizüstü bilgisayarlarda bulunan Çekirdekler'e](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic)bakın.

    Çıkış şöyle olur:

    ![SQL sorgu çıktısı](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL sorgu çıktısı")

3. Ayrıca, bir çizim oluşturmak için, verilerin görselleştirme oluşturmak için kullanılan bir kitaplık [matplotlib](https://en.wikipedia.org/wiki/Matplotlib)kullanabilirsiniz. Çizim yerel olarak kalıcı **countResultsdf** veri çerçevesinden oluşturulması gerektiğinden, kod parçacığı `%%local` sihirle başlamalıdır. Bu eylem, kodun Jupyter sunucusunda yerel olarak çalıştırılmasını sağlar.

    ```PySpark
    %%local
    %matplotlib inline
    import matplotlib.pyplot as plt

    labels = countResultsdf['results']
    sizes = countResultsdf['cnt']
    colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
    plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
    plt.axis('equal')
    ```

    Bir gıda denetim sonucu tahmin etmek için, ihlalleri dayalı bir model geliştirmeniz gerekir. Lojistik regresyon ikili sınıflandırma yöntemi olduğundan, sonuç verilerini iki kategoriye ayırmak mantıklıdır: **Başarısız** ve **Geç**:

   - Geçirmek
       - Geçirmek
       - Pass w/ koşulları
   - Başarısız
       - Başarısız
   - Vazgeç
       - İş yeri bulunmuyor
       - İş Dışı

     Diğer sonuçlarla birlikte veriler ("İşletme Bulunamadı" veya "İş Dışı") yararlı değildir ve yine de sonuçların küçük bir yüzdesini bulurlar.

4. Varolan veri çerçevesini,`df`her denetimin etiket ihlali çifti olarak temsil edildiği yeni bir veri çerçevesine dönüştürmek için aşağıdaki kodu çalıştırın. Bu durumda, bir `0.0` başarısızlığı temsil eden bir `1.0` etiket, bir başarıyı `-1.0` temsil eden bir etiket ve bu iki sonuç dışında bazı sonuçları temsil eden bir etiket.

    ```PySpark
    def labelForResults(s):
        if s == 'Fail':
            return 0.0
        elif s == 'Pass w/ Conditions' or s == 'Pass':
            return 1.0
        else:
            return -1.0
    label = UserDefinedFunction(labelForResults, DoubleType())
    labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')
    ```

5. Etiketli verilerin bir satırını göstermek için aşağıdaki kodu çalıştırın:

    ```PySpark
    labeledData.take(1)
    ```

    Çıkış şöyle olur:

    ```
    [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]
    ```

## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Giriş veri çerçevesinden bir lojistik regresyon modeli oluşturma

Son görev, etiketli verileri dönüştürmektir. Verileri lojistik regresyon ile analiz edilebilen bir biçime dönüştürün. Bir lojistik regresyon algoritması girişi *etiket özelliği vektör çiftleri*kümesi gerekir. "Özellik vektörü"nün giriş noktasını temsil eden sayıların vektörü olduğu yer. Yani, yarı yapılandırılmış ve serbest metin birçok yorum içeren "ihlalleri" sütun, dönüştürmek gerekir. Sütunu, bir makinenin kolayca anlayabileceği bir dizi gerçek sayıya dönüştürün.

Doğal dili işlemek için standart bir makine öğrenimi yaklaşımı, her bir kelimeye bir "dizin" atamaktır. Sonra makine öğrenme algoritmasına bir vektör geçirin. Her dizinin değeri metin dizesinde bu sözcüğün göreli sıklığını içerir.

MLlib bu işlemi yapmak için kolay bir yol sağlar. İlk olarak, "tokenize" her dize tek tek sözcükleri almak için her ihlalleri dize. Ardından, her `HashingTF` belirteç kümesini, bir model oluşturmak için lojistik regresyon algoritmasına geçirilebilen bir özellik vektörüne dönüştürmek için a'yı kullanın. Tüm bu adımları bir "ardışık" kullanarak sırayla gerçekleştirin.

```PySpark
tokenizer = Tokenizer(inputCol="violations", outputCol="words")
hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
lr = LogisticRegression(maxIter=10, regParam=0.01)
pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

model = pipeline.fit(labeledData)
```

## <a name="evaluate-the-model-using-another-dataset"></a>Modeli başka bir veri kümesi ni kullanarak değerlendirme

Yeni denetimlerin sonuçlarının ne olacağını *tahmin* etmek için daha önce oluşturduğunuz modeli kullanabilirsiniz. Tahminler, gözlemlenen ihlallere dayanıyor. Bu modeli dataset **Food_Inspections1.csv**üzerinde eğittiniz. Yeni veriler üzerinde bu modelin gücünü *değerlendirmek* için ikinci bir veri kümesi, **Food_Inspections2.csv**kullanabilirsiniz. Bu ikinci veri kümesi **(Food_Inspections2.csv)** kümeyle ilişkili varsayılan depolama kapsayıcısındadır.

1. Yeni bir veri çerçevesi oluşturmak için aşağıdaki kodu çalıştırın, model tarafından oluşturulan tahmin içeren **öngörülerDf.** Parçacık, veri çerçevesine dayalı **Öngörüler** adlı geçici bir tablo da oluşturur.

    ```PySpark
    testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                .map(csvParse) \
                .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
    testDf = spark.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
    predictionsDf = model.transform(testDf)
    predictionsDf.registerTempTable('Predictions')
    predictionsDf.columns
    ```

    Aşağıdaki metin gibi bir çıktı görmeniz gerekir:

    ```
    ['id',
        'name',
        'results',
        'violations',
        'words',
        'features',
        'rawPrediction',
        'probability',
        'prediction']
    ```

1. Tahminlere bak. Bu parçacığı çalıştırın:

    ```PySpark
    predictionsDf.take(1)
    ```

   Test veri kümesine ilk giriş için bir tahmin var.

1. Yöntem, `model.transform()` aynı şemaya sahip tüm yeni verilere aynı dönüşümü uygular ve verilerin nasıl sınıflandırılalalalalaaçıklaştırılabildiğini tahmin eder. Tahminlerin nasıl olduğunu anlamak için bazı istatistikler yapabilirsiniz:

    ```PySpark
    numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                            (prediction = 1 AND (results = 'Pass' OR
                                                                results = 'Pass w/ Conditions'))""").count()
    numInspections = predictionsDf.count()

    print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
    print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"
    ```

    Çıktı aşağıdaki metne benzer:

    ```
    There were 9315 inspections and there were 8087 successful predictions
    This is a 86.8169618894% success rate
    ```

    Spark ile lojistik regresyon kullanarak İngilizce ihlalleri açıklamaları arasındaki ilişkinin bir model verir. Ve belirli bir iş geçmek ya da bir gıda denetim başarısız olup olmadığını.

## <a name="create-a-visual-representation-of-the-prediction"></a>Tahminin görsel bir temsilini oluşturma

Artık bu testin sonuçları hakkında bilgi vermek için son bir görselleştirme oluşturabilirsiniz.

1. Daha önce oluşturulan **Öngörüler** geçici tablosundan farklı tahminleri ve sonuçları ayıklayarak başlarsınız. Aşağıdaki sorgular çıktıyı *true_positive,* *false_positive,* *true_negative*ve *false_negative*olarak ayırır. Aşağıdaki sorgularda, görselleştirmeyi kullanarak `-q` kapatın ve çıktıyı (kullanarak) `-o`daha sonra `%%local` sihirle kullanılabilecek veri çerçevesi olarak kaydedebilirsiniz.

    ```PySpark
    %%sql -q -o true_positive
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'
    ```

    ```PySpark
    %%sql -q -o false_positive
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
    ```

    ```PySpark
    %%sql -q -o true_negative
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'
    ```

    ```PySpark
    %%sql -q -o false_negative
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
    ```

1. Son olarak, **Matplotlib**kullanarak arsa oluşturmak için aşağıdaki snippet kullanın.

    ```PySpark
    %%local
    %matplotlib inline
    import matplotlib.pyplot as plt

    labels = ['True positive', 'False positive', 'True negative', 'False negative']
    sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
    colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
    plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
    plt.axis('equal')
    ```

    Aşağıdaki çıktıyı görmeniz gerekir:

    ![Spark makine öğrenme uygulama çıktısı - başarısız gıda denetimleri pasta grafik yüzdeleri.](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Kıvılcım makine öğrenme sonucu çıktı")

    Bu grafikte, "pozitif" bir sonuç başarısız gıda denetimi anlamına gelirken, negatif sonuç geçirilen bir denetim anlamına gelir.

## <a name="shut-down-the-notebook"></a>Not defterini kapatma

Uygulamayı çalıştırmayı bitirdikten sonra, kaynakları serbest bırakmak için not defterini kapatmanız gerekir. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’u seçin. Bu eylem, not defterini kapatır.

## <a name="next-steps"></a>Sonraki adımlar

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ IDEA için HDInsight Araçları Eklentisini kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
