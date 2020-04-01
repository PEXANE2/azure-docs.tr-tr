---
title: 'Öğretici: Apache Spark ile sorguları çalıştırmak & veri yükleyin - Azure HDInsight'
description: Öğretici - Azure HDInsight'ta Spark kümelerinde verileri nasıl yükleyip etkileşimli sorgular çalıştırışta bulundurun öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive,mvc
ms.date: 02/12/2020
ms.openlocfilehash: 5eb6788a558e4429296731f1693edd18bf92f98f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77198897"
---
# <a name="tutorial-load-data-and-run-queries-on-an-apache-spark-cluster-in-azure-hdinsight"></a>Öğretici: Azure HDInsight içindeki bir Apache Spark kümesinde veri yükleme ve sorgular çalıştırma

Bu eğitimde, bir csv dosyasından nasıl bir veri çerçevesi oluşturabileceğinizi ve Azure HDInsight'ta bir [Apache Spark](https://spark.apache.org/) kümesine karşı etkileşimli Spark SQL sorgularını nasıl çalıştırabileceğinizi öğrenirsiniz. Spark’ta dataframe, adlandırılmış sütunlar halinde düzenlenmiş, dağıtılmış bir veri koleksiyonudur. Dataframe kavramsal olarak, ilişkisel bir veritabanındaki tabloya veya R/Python’daki veri çerçevesine eşdeğerdir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Bir csv dosyasından dataframe oluşturma
> * Dataframe üzerinde sorgular çalıştırma

## <a name="prerequisites"></a>Ön koşullar

HDInsight üzerinde bir Apache Spark kümesi. Bkz. [Bir Apache Spark kümesi oluştur.](./apache-spark-jupyter-spark-sql-use-portal.md)

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

Jupyter Notebook, çeşitli programlama dillerini destekleyen etkileşimli bir not defteri ortamıdır. Not defteri, verilerle etkileşim kurmanıza, kodu markdown metniyle birleştirmenize ve basit görselleştirmeler gerçekleştirmenize olanak sağlar.

1. Kıvılcım kümenizin adını `SPARKCLUSTER` değiştirerek URL'yi `https://SPARKCLUSTER.azurehdinsight.net/jupyter` düzenleme. Ardından düzenlenen URL'yi bir web tarayıcısı girin. İstendiğinde, küme için küme oturum açma kimlik bilgilerini girin.

2. Jupyter web sayfasından, bir dizüstü bilgisayar oluşturmak için **Yeni** > **PySpark'ı** seçin.

   ![Etkileşimli Spark SQL sorgusunu çalıştırmak için bir Jupyter Notebook oluşturma](./media/apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusunu çalıştırmak için bir Jupyter Notebook oluşturma")

   İsimsiz adıyla yeni bir not defteri`Untitled.ipynb`oluşturulur ve açılır.

    > [!NOTE]  
    > PySpark çekirdeği kullanılarak not defteri oluşturmak için, ilk kod hücresini çalıştırdığınızda sizin için otomatik olarak `spark` oturumu oluşturulur. Belirtik şekilde bir oturum oluşturmanız gerekmez.

## <a name="create-a-dataframe-from-a-csv-file"></a>Bir csv dosyasından dataframe oluşturma

Uygulamalar, azure depolama veya Azure Veri Gölü Depolama gibi uzak depolama daki dosya veya klasörlerden doğrudan veri çerçeveleri oluşturabilir; bir Hive tablosundan; veya Cosmos DB, Azure SQL DB, DW gibi Spark tarafından desteklenen diğer veri kaynaklarından. Aşağıdaki ekran görüntüsünde, bu öğreticide kullanılan HVAC.csv dosyasının bir anlık görüntü gösterilmektedir. Csv dosyası, tüm HDInsight Spark kümeleriyle birlikte gelir. Veriler, bazı binaların sıcaklık varyasyonlarını yakalar.

![Etkileşimli Spark SQL sorgusu için veri anlık görüntüsü](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusu için veri anlık görüntüsü")

1. Jupyter not defterinin boş bir hücresine aşağıdaki kodu yapıştırın ve kodu çalıştırmak için **SHIFT + ENTER** tuşuna basın. Kod, bu senaryo için gerekli olan türleri içeri aktarır:

    ```python
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```

    Jupyter’de etkileşimli bir sorgu çalıştırılırken web tarayıcısı penceresinde veya sekme açıklamalı alt yazısında not defteri başlığıyla birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.

    ![Etkileşimli Spark SQL sorgusunun durumu](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Etkileşimli Spark SQL sorgusunun durumu")

1. Oturum kimliğinin döndürüldüğüne dikkat edin. Yukarıdaki resimden oturum kimliği 0'dır. İstenirseniz, CLUSTERNAME'nin Spark `https://CLUSTERNAME.azurehdinsight.net/livy/sessions/ID/statements` kümenizin adı ve kimliğin oturum kimlik numaranız olduğu yere gezinerek oturum ayrıntılarını alabilirsiniz.

1. Aşağıdaki kodu çalıştırarak bir dataframe ve geçici bir tablo (**hvac**) oluşturun.

    ```python
    # Create a dataframe and table from sample data
    csvFile = spark.read.csv('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv', header=True, inferSchema=True)
    csvFile.write.saveAsTable("hvac")
    ```

## <a name="run-queries-on-the-dataframe"></a>Dataframe üzerinde sorgular çalıştırma

Tablo oluşturulduktan sonra veriler üzerinde etkileşimli bir sorgu çalıştırabilirsiniz.

1. Not defterinin boş bir hücresinde aşağıdaki kodu çalıştırın:

    ```sql
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Aşağıdaki tablo çıktısı görüntülenir.

     ![Etkileşimli Spark sorgu sonucunun tablo çıktısı](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Etkileşimli Spark sorgu sonucunun tablo çıktısı")

2. Sonuçları diğer görselleştirmelerde de görebilirsiniz. Aynı çıktı için bir alan grafiği görmek için **Alan**’ı seçin ve sonra gösterildiği gibi diğer değerleri ayarlayın.

    ![Etkileşimli Kıvılcım sorgu sonucunun alan grafiği](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Etkileşimli Kıvılcım sorgu sonucunun alan grafiği")

3. Not defteri menü çubuğundan **Dosya** > **Kaydet ve Denetim Noktası'na**gidin.

4. [Sonraki öğreticiyi](apache-spark-use-bi-tools.md) şimdi başlatıyorsanız, not defterini açık bırakın. Değilse, küme kaynaklarını serbest bırakmak için not defterini kapatın: not defteri menü çubuğundan **Dosya** >  **Kapat ve Durdur'a**gidin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

HDInsight ile verileriniz ve Jupyter dizüstü bilgisayarlarınız Azure Depolama veya Azure Veri Gölü Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır. Sonraki öğretici üzerinde hemen çalışmayı planlıyorsanız, kümeyi tutmak isteyebilirsiniz.

Azure portalında kümeyi açıp **Sil**’i seçin.

![HDInsight kümesini silme](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek hem HDInsight Spark kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir csv dosyasından nasıl bir veri çerçevesi oluşturacağınız ve Azure HDInsight'ta bir Apache Spark kümesine karşı etkileşimli Spark SQL sorgularını çalıştırmayı öğrendiniz. Apache Spark'a kaydettiğiniz verilerin Power BI gibi bir BI analiz aracına nasıl çekilebildiğini görmek için bir sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [BI araçlarını kullanarak verileri analiz etme](apache-spark-use-bi-tools.md)
