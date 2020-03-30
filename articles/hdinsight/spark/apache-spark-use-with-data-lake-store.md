---
title: HDInsight Apache Spark ile Azure Veri Gölü Depolama Gen1'i analiz edin
description: Azure Veri Gölü Depolama Gen1'de depolanan verileri analiz etmek için Apache Spark işlerini çalıştırın
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: f7a6ab954aff1bcc2e3dae3fc035db4b136ccbbe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73818175"
---
# <a name="use-hdinsight-spark-cluster-to-analyze-data-in-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'deki verileri analiz etmek için HDInsight Spark kümesini kullanın

Bu makalede, Bir Veri Gölü Depolama hesabından veri okuyan bir işi çalıştırmak için HDInsight Spark kümeleri ile kullanılabilen [Jupyter Notebook'u](https://jupyter.org/) kullanıyorsunuz.

## <a name="prerequisites"></a>Ön koşullar

* Azure Veri Gölü Depolama Gen1 hesabı. [Azure portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın'daki](../../data-lake-store/data-lake-store-get-started-portal.md)yönergeleri izleyin.

* Azure HDInsight Spark kümesi, Veri Gölü Depolama Gen1'i depolama alanı olarak sunar. Quickstart'taki yönergeleri [izleyin: HDInsight'ta kümeler ayarlayın.](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)

## <a name="prepare-the-data"></a>Verileri hazırlama

> [!NOTE]  
> Varsayılan depolama alanı olarak Veri Gölü Depolama ile HDInsight kümesi oluşturduysanız bu adımı gerçekleştirmeniz gerekmez. Küme oluşturma işlemi, küme oluştururken belirttiğiniz Veri Gölü Depolama hesabına bazı örnek veriler ekler. Veri Gölü Depolama ile HDInsight Spark kümesini kullan bölümüne atlayın.

Ek depolama alanı olarak Veri Gölü Depolama'sı ve varsayılan depolama alanı olarak Azure Depolama Blob içeren bir HDInsight kümesi oluşturduysanız, önce bazı örnek verileri Veri Gölü Depolama hesabına kopyalamanız gerekir. HDInsight kümesiyle ilişkili Azure Depolama Blob'undaki örnek verileri kullanabilirsiniz. Bunu yapmak için [ADLCopy aracını](https://www.microsoft.com/download/details.aspx?id=50358) kullanabilirsiniz. Bağlantıyı indirin ve yükleyin.

1. Bir komut istemi açın ve AdlCopy'nin yüklendiği dizine gidin, genellikle `%HOMEPATH%\Documents\adlcopy`.

2. Kaynak kapsayıcıdan Veri Gölü Depolamasına belirli bir blob kopyalamak için aşağıdaki komutu çalıştırın:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    **HVAC.csv** örnek veri dosyasını **/HdiSamples/HdiSamples/SensorSampleData/hvac/** adresindeki Azure Veri Gölü Depolama hesabına kopyalayın. Kod parçacığı aşağıdaki gibi görünmelidir:

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]  
   > Dosya ve yol adlarının uygun büyük harf kullandığından emin olun.

3. Veri Gölü Depolama hesabınız altında bulunan Azure aboneliğiiçin kimlik bilgilerini girmeniz istenir. Aşağıdaki kod parçacığına benzer bir çıkış görürsünüz:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    Veri dosyası **(HVAC.csv)** Veri Gölü Depolama hesabında bir klasör **/ hvac** altında kopyalanır.

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-storage-gen1"></a>Data Lake Storage Gen1 ile HDInsight Spark kümesini kullanma

1. Başlangıç panosundan [Azure portalından,](https://portal.azure.com/)Apache Spark kümenizin döşemesini tıklatın (başlangıç panosuna sabitlediyseniz). Ayrıca Tüm > **HDInsight Kümelerine** **Gözat**altında kümenize gidebilirsiniz.

2. Spark kümesi dikey penceresinden **Hızlı Bağlantılar**’a ve sonra **Küme Panosu** dikey penceresinden **Jupyter Not Defteri**’ne tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   > [!NOTE]  
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

3. Yeni bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.

    ![Yeni bir Jupyter not defteri oluşturma](./media/apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Yeni bir Jupyter not defteri oluşturma")

4. PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. Bu senaryo için gereken türleri içeri aktararak işleme başlayabilirsiniz. Bunu yapmak için aşağıdaki kod parçacığını bir hücreye yapıştırın ve **SHIFT + ENTER** tuşuna basın.

        from pyspark.sql.types import *

    Jupyter’de bir işi her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.

     ![Jupyter not defteri işinin durumu](./media/apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Jupyter not defteri işinin durumu")

5. Veri Gölü Depolama Gen1 hesabına kopyaladığınız **HVAC.csv** dosyasını kullanarak örnek verileri geçici bir tabloya yükleyin. Aşağıdaki URL deseni kullanarak Veri Gölü Depolama hesabındaki verilere erişebilirsiniz.

   * Varsayılan depolama alanı olarak Veri Gölü Depolama Gen1 varsa, HVAC.csv aşağıdaki URL'ye benzer yolda olacaktır:

           adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

       Veya, aşağıdakigibi kısaltılmış bir biçim de kullanabilirsiniz:

           adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

   * Ek depolama alanı olarak Veri Gölü Depolama'nız varsa, HVAC.csv, aşağıdakiler gibi kopyaladiğiniz konumda olacaktır:

           adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     Boş bir hücrede, aşağıdaki kod örneğini yapıştırın, **MYDATALAKESTORE'u** Veri Gölü Depolama hesap adınız ile değiştirin ve **SHIFT + ENTER tuşuna**basın. Bu kod örneği, verileri **hvac** adlı geçici bir tabloya kaydeder.

           # Load the data. The path below assumes Data Lake Storage is default storage for the Spark cluster
           hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

           # Create the schema
           hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

           # Parse the data in hvacText
           hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

           # Create a data frame
           hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

           # Register the data fram as a table to run queries against
           hvacdf.registerTempTable("hvac")

6. Bir PySpark çekirdeği kullandığınız için `%%sql` sihrini kullanarak yeni oluşturduğunuz **hvac** geçici tablosunda bundan böyle bir SQL sorgusunu doğrudan çalıştırabilirsiniz. `%%sql` Büyü hakkında daha fazla bilgi için, yanı sıra PySpark çekirdek ile mevcut diğer büyüler, [Çekirdekler Apache Spark HDInsight kümeleri ile Jupyter dizüstü bilgisayarlarda kullanılabilir](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic)bakın.

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. İş başarıyla tamamlandıktan sonra varsayılan olarak aşağıdaki tablo çıktısı görüntülenir.

      ![Sorgu sonucunun tablo çıktısı](./media/apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Sorgu sonucunun tablo çıktısı")

     Sonuçları diğer görselleştirmelerde de görebilirsiniz. Örneğin, aynı çıktı için bir alan grafiği aşağıdaki gibi görünür.

     ![Sorgu sonucunun alan grafiği](./media/apache-spark-use-with-data-lake-store/jupyter-area-output1.png "Sorgu sonucunun alan grafiği")

8. Uygulamayı çalıştırmayı tamamladıktan sonra kaynakları serbest bırakmak için not defterini kapatmanız gerekir. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Bunun yapılması not defterini kapatır.


## <a name="next-steps"></a>Sonraki adımlar

* [Apache Spark kümesinde çalışmak için bağımsız bir Scala uygulaması oluşturun](apache-spark-create-standalone-application.md)
* [HDInsight Spark Linux kümesi için Apache Spark uygulamaları oluşturmak için IntelliJ için Azure Araç Setinde HDInsight Araçlarını kullanın](apache-spark-intellij-tool-plugin.md)
* [HDInsight Spark Linux kümesi için Apache Spark uygulamaları oluşturmak için Eclipse için Azure Toolkit'te HDInsight Araçlarını kullanın](apache-spark-eclipse-tool-plugin.md)
* [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
