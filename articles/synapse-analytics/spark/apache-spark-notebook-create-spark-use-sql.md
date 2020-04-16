---
title: "Hızlı başlangıç: Azure Synapse Analytics'te Apache Spark havuzu (önizleme) oluşturma"
description: Bu hızlı başlangıç, Azure Synapse Analytics'te Bir Apache Spark havuzu (önizleme) oluşturmak ve bir Spark SQL sorgusu çalıştırmak için web araçlarının nasıl kullanılacağını gösterir.
services: synapse-analytics
author: euangMS
ms.author: euang
ms.reviewer: jrasnick, carlrab
ms.service: synapse-analytics
ms.subservice: ''
ms.topic: quickstart
ms.date: 04/15/2020
ms.openlocfilehash: 5762c074b825c1282959c509c2c72d232f0a238c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424303"
---
# <a name="quickstart-create-an-apache-spark-pool-preview-in-synapse-analytics-using-web-tools"></a>Hızlı başlangıç: Web araçlarını kullanarak Synapse Analytics'te Apache Spark havuzu (önizleme) oluşturma

Bu hızlı başlangıçta, web araçlarını kullanarak Azure Synapse Analytics'te Apache Spark havuzu (önizleme) oluşturmayı öğrenirsiniz. Daha sonra Apache Spark havuzuna bağlanmayı ve Sql sorgularını dosya ve tablolara karşı çalıştırmayı öğrenirsiniz. Apache Spark, bellek içi işleme kullanarak hızlı veri analizi ve küme hesaplama sağlar. Spark on Synapse Analytics hakkında daha fazla bilgi için bkz: [Azure Synapse Analytics'te Apache Spark.](apache-spark-overview.md)

> [!IMPORTANT]
> Spark örnekleri için faturalandırma, bunları kullanıyor olun veya kullanmasanız da dakika başına eşit olarak eşitlenir. Spark örneğini kullanmayı bitirdikten sonra kapattığınızdan veya kısa bir zaman aranızdan ayarladığınızdan emin olun. Daha fazla bilgi için bu makalenin **Kaynakları temizleme** bölümüne bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="create-an-apache-spark-pool"></a>Apache Spark havuzu oluşturma

Bu makalede, web araçlarını kullanarak yeni bir Apache Spark havuzu nasıl oluşturulacağını gösterir.

## <a name="create-a-notebook"></a>Not defteri oluşturma

Not defteri, çeşitli programlama dillerini destekleyen etkileşimli bir ortamdır. Not defteri, verilerinizle etkileşimkurmanızı, kodu işaretleme, metin ve basit görselleştirmelerle birleştirmenizi sağlar.

1. Kullanmak istediğiniz Synapse Analytics çalışma alanı için Azure portal görünümünden **Synapse Studio'yu başlat'ı**seçin.
2. Synapse Analytics Studio açıldıktan sonra **Geliştir'i**seçin. Sonra, **Notebooks** girişinin üzerine gezin. Elipsleri seçin (**...**).
3. Buradan Yeni **dizüstü bilgisayar**seçeneğini belirleyin. Yeni bir not defteri oluşturulur ve otomatik olarak oluşturulan bir adla açılır.
  ![Yeni not defteri](./media/apache-spark-notebook-create-spark-use-sql/spark-get-started-new-notebook.png "Yeni not defteri")

4. **Özellikler** penceresinde, not defteri için bir ad sağlayın.
5. Araç çubuğunda **Yayımla'yı**tıklatın.
6. Çalışma alanınızda yalnızca bir Apache Spark havuzu varsa, varsayılan olarak seçilir. Hiçbiri seçili değilse doğru Apache Spark havuzunu seçmek için açılır açılır alanı kullanın.
7. **Kod Ekle'yi**tıklatın. Varsayılan `Pyspark`dil. Pyspark ve Spark SQL'in bir karışımını kullanacaksınız, bu nedenle varsayılan seçenek iyi.
8. Daha sonra işlemek için basit bir Spark DataFrame nesnesi oluşturun. Bu durumda, koddan oluşturursunuz. Üç satır ve üç sütun vardır:

   ```python
   new_rows = [('CA',22, 45000),("WA",35,65000) ,("WA",50,85000)]
   demo_df = spark.createDataFrame(new_rows, ['state', 'age', 'salary'])
   demo_df.show()
   ```

9. Şimdi aşağıdaki yöntemlerden birini kullanarak hücreçalıştırın:

   - **SHIFT + ENTER**tuşuna basın.
   - Hücrenin solundaki mavi oynatma simgesini seçin.
   - Araç çubuğundaki **Tümünü Çalıştır** düğmesini seçin.

   ![Veri çerçevesi nesnesi oluşturma](./media/apache-spark-notebook-create-spark-use-sql/spark-get-started-create-data-frame-object.png "Kıvılcım işinden çıktı")

10. Apache Spark havuzu örneği zaten çalışmıyorsa, otomatik olarak başlatılır. Çalıştırmakta olduğunuz hücrenin altındaki Apache Spark havuzu örneğinin durumunu ve not defterinin altındaki durum panelinde görebilirsiniz. Havuzun büyüklüğüne bağlı olarak, başlangıç 2-5 dakika sürmelidir. Kod çalışma tamamlandıktan sonra, hücrenin altındaki bilgiler, yürütmenin ne kadar sürdüğünü ve yürütülmesini gösterir. Çıkış hücresinde çıktıyı görürsünüz.

    ![Hücreyi çalıştıran çıktı](./media/apache-spark-notebook-create-spark-use-sql/run-cell-with-output.png "Kıvılcım işinden çıktı")

11. Veriler artık bir DataFrame'de var, oradan verileri çok farklı şekillerde kullanabilirsiniz. Bu quickstart geri kalanı için farklı biçimlerde ihtiyacınız olacak.
12. Aşağıdaki kodu başka bir hücreye girin ve çalıştırın, bu bir Kıvılcım tablosu, bir CSV ve tüm verilerin kopyaları ile bir Parke dosyası oluşturur:

    ```python
     demo_df.createOrReplaceTempView('demo_df')
     demo_df.write.csv('demo_df', mode='overwrite')
     demo_df.write.parquet('abfss://<<TheNameOfAStorageAccountFileSystem>>@<<TheNameOfAStorageAccount>>.dfs.core.windows.net/demodata/demo_df', mode='overwrite')
    ```

    Depolama gezgini kullanıyorsanız, yukarıda kullanılan bir dosyayı yazmanın iki farklı yolunun etkisini görmek mümkündür. Hiçbir dosya sistemi belirtildiğinde varsayılan olarak kullanılır, `default>user>trusted-service-user>demo_df`bu durumda . Veriler belirtilen dosya sisteminin konumuna kaydedilir.

    Hem "csv" hem de "parke" biçimlerinde dikkat edin, yazma işlemleri birçok bölümlü dosya ile bir dizini oluşturulur.

    ![Çıktının depolama gezgini görünümü](./media/apache-spark-notebook-create-spark-use-sql/spark-get-started-default-storage.png "Çıktının depolama gezgini görünümü")

    ![Çıktının depolama gezgini görünümü](./media/apache-spark-notebook-create-spark-use-sql/spark-get-started-default-storage2.png "Çıktının depolama gezgini görünümü")

## <a name="run-spark-sql-statements"></a>Spark SQL deyimleri çalıştırma

SQL (Yapılandırılmış Sorgu Dili), veri sorgulama ve tanımlama için en çok kullanılan dildir. Bilinen SQL söz dizimini kullanan Spark SQL, yapısal verileri işleyen bir Apache Spark uzantısı olarak çalışır.

1. Aşağıdaki kodu boş bir hücreye yapıştırın ve ardından kodu çalıştırın. Komut, havuzdaki tabloları listeler.

   ```sql
   %%sql
   SHOW TABLES
   ```

   Synapse Analytics Apache Spark havuzunuzla bir Not Defteri `sqlContext` kullandığınızda, Spark SQL kullanarak sorguları çalıştırmak için kullanabileceğiniz bir ön ayar elde elabilirsiniz. `%%sql`not defterine sorguyu `sqlContext` çalıştırmak için önceden ayarlanmışı kullanmasını söyler. Sorgu, varsayılan olarak tüm Synapse Analytics Apache Spark havuzlarıile birlikte gelen bir sistem tablosundan en iyi 10 satırı alır.

2. `demo_df` komutundaki verileri görmek için başka bir sorgu çalıştırın.

    ```sql
    %%sql
    SELECT * FROM demo_df
    ```

    Kod, biri iş görünümünü gösteren, diğeri veri sonuçları içeren iki çıktı hücresi üretir.

    Varsayılan olarak sonuç görünümü bir ızgara gösterir, ancak görünümün ızgara ve grafik görünümleri arasında geçiş yapmasına izin veren ızgaranın altında bir görünüm değiştirici vardır.

    ![Synapse Analytics Spark'ta sorgu çıktısı](./media/apache-spark-notebook-create-spark-use-sql/spark-get-started-query.png "Synapse Analytics Spark'ta sorgu çıktısı")

3. **Görünüm** değiştiricide **Grafik'i** seçin
4. Sağ taraftan **Görünüm seçeneklerini** seçin.
5. Grafik **türü** alanında "çubuk grafiği" seçeneğini belirleyin.
6. X ekseni sütun alanında "durum" seçeneğini belirleyin.
7. Y ekseni sütun alanında "maaş" seçeneğini belirleyin.
8. **Toplama** alanında "AVG" seçeneğini belirleyin.
9. **Uygula**’yı seçin.

   ![Synapse Analytics Spark'ta grafik çıktısı](./media/apache-spark-notebook-create-spark-use-sql/spark-get-started-query-chart-output.png "Synapse Analytics Spark'ta grafik çıktısı")

10. SQL'i çalıştırma deneyimi ne kadar da olsa dilleri değiştirmek zorunda kalmadan elde etmek mümkündür. Bunu yukarıdaki SQL hücresini bu PySpark hücresiile değiştirerek yapabilirsiniz, **görüntü** komutu kullanıldığından çıkış deneyimi aynıdır:

    ```python
    display(spark.sql('SELECT * FROM demo_df'))
    ```

11. Daha önce yürütülen hücrelerin her biri **Geçmiş Server** ve **İzleme**gitme seçeneğine sahipti. Bağlantıları tıklattığınızda Kullanıcı Deneyimi'nin farklı bölümlerine götürür.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Synapse Analytics verilerinizi Azure Veri Gölü Depolama'da kaydeder. Bir Kıvılcım örneğinin kullanılmadığında güvenli bir şekilde kapanmasına izin verebilirsiniz. Kullanılmamış olsa bile, çalıştığı sürece synapse Analytics Apache Spark havuzu için ücretlendirilirsiniz. Havuz ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, Spark örneklerinin kullanılmadıkları nda kapanmasına izin vermek ekonomik açıdan mantıklıdır.

Kıvılcım örneğinin kapatılmasını sağlamak için bağlı oturumları (not defterlerini) sonlandırın. Apache Spark havuzunda belirtilen **boşta zamana** ulaşıldığında havuz kapanır. Not defterinin altındaki durum çubuğundan **bitiş oturumunu** da seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, synapse Analytics Apache Spark havuzu oluşturmayı ve temel bir Spark SQL sorgusu çalıştırmayı öğrendiniz.

- [Azure Synapse Analytics](../overview-what-is.md)
- [.NET Apache Spark belgeleri için](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Apache Spark resmi belgeler](https://spark.apache.org/docs/latest/)

>[!NOTE]
> Resmi Apache Spark belgelerinden bazıları, Azure Synapse Spark'ta bulunmayan Spark konsolunun kullanımına dayanır. Bunun yerine [not defterini](../spark/apache-spark-notebook-create-spark-use-sql.md) veya [IntelliJ](../spark/intellij-tool-synapse.md) deneyimlerini kullanın.
