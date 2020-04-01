---
title: "Öğretici: Azure HDInsight'ta Bir Spark bilgi işlem bağlamında R'yi kullanın"
description: Öğretici - Azure HDInsight Machine Learning hizmet kümesinde R ve Spark ile başlayın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/21/2019
ms.openlocfilehash: 73ca0d089ab758fb13e69d341337139d79194cc5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71121931"
---
# <a name="tutorial-use-r-in-a-spark-compute-context-in-azure-hdinsight"></a>Öğretici: Azure HDInsight'ta Bir Spark bilgi işlem bağlamında R'yi kullanın

Bu öğretici, Bir Azure HDInsight Machine Learning hizmetleri kümesinde çalışan Apache Spark'ta R işlevlerini kullanmaya adım adım giriş sağlar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Örnek verileri yerel depolama alanına indirin
> * Verileri varsayılan depolama alanına kopyalama
> * Veri kümesi ayarlama
> * Veri kaynakları oluşturma
> * Kıvılcım için bilgi işlem bağlamı oluşturma
> * Doğrusal bir model sığdır
> * Bileşik XDF dosyalarını kullanma
> * XDF'yi CSV'ye dönüştürün

## <a name="prerequisites"></a>Ön koşullar

* Azure HDInsight Machine Learning hizmetleri kümesi. Azure [portalını kullanarak Apache Hadoop kümeleri oluşturma'ya](../hdinsight-hadoop-create-linux-clusters-portal.md) gidin ve Küme türü için **ML Hizmetleri'ni**seçin. **Cluster type**

## <a name="connect-to-rstudio-server"></a>RStudio Server’a bağlanma

RStudio Server kümenin kenar düğümünde çalışır. Aşağıdaki siteye gidin (URL'deki *CLUSTERNAME'nin* oluşturduğunuz HDInsight Machine Learning hizmetleri kümesinin adı olduğu yer):

```
https://CLUSTERNAME.azurehdinsight.net/rstudio/
```

İlk oturum açğınızda, iki kez kimlik doğrulaması yapıyorsun. İlk kimlik doğrulama isteminde, küme yöneticisi kullanıcı adı ve parola (varsayılan *yöneticidir)* girin. İkinci kimlik doğrulama isteminde, SSH kullanıcı adı ve parolasını sağlayın (varsayılan değer *sshuser'dir).* Sonraki oturum açmalar yalnızca SSH kimlik bilgilerini gerektirir.

## <a name="download-the-sample-data-to-local-storage"></a>Örnek verileri yerel depolama alanına indirin

*Havayolu 2012 Zamanında Veri Seti, 2012* yılı için ABD'deki tüm ticari uçuşlar için uçuş varış ve gidiş bilgilerini içeren virgülden ayrılmış 12 dosyadan oluşmaktadır. Bu veri kümesi 6 milyondan fazla gözlemle büyüktür.

1. Birkaç ortam değişkenini başlangıç olarak. RStudio Server konsoluna aşağıdaki kodu girin:

    ```R
    bigDataDirRoot <- "/tutorial/data" # root directory on cluster default storage
    localDir <- "/tmp/AirOnTimeCSV2012" # directory on edge node
    remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012" # location of data
    ```

1. Sağ bölmede **Çevre** sekmesini seçin. Değişkenler **Değerler**altında görüntülenir.

    ![HDInsight R stüdyo web konsolu](./media/ml-services-tutorial-spark-compute/hdinsight-rstudio-image.png)

1. Yerel bir dizin oluşturun ve örnek verileri indirin. RStudio'da aşağıdaki kodu girin:

    ```R
    # Create local directory
    dir.create(localDir)
    
    # Download data to the tmp folder(local)
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(localDir, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(localDir, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(localDir, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(localDir, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(localDir, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(localDir, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(localDir, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(localDir, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(localDir, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(localDir, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(localDir, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(localDir, "airOT201212.csv"))
    ```

    İndirme işlemi yaklaşık 9,5 dakika içinde tamamlanmalıdır.

## <a name="copy-the-data-to-default-storage"></a>Verileri varsayılan depolama alanına kopyalama

Hadoop Dağıtılmış Dosya Sistemi (HDFS) `airDataDir` konumu değişkenle birlikte belirtilir. RStudio'da aşağıdaki kodu girin:

```R
# Set directory in bigDataDirRoot to load the data into
airDataDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

# Create directory (default storage)
rxHadoopMakeDir(airDataDir)

# Copy data from local storage to default storage
rxHadoopCopyFromLocal(localDir, bigDataDirRoot)
    
# Optional. Verify files
rxHadoopListFiles(airDataDir)
```

Adım yaklaşık 10 saniye içinde tamamlanmalıdır.

## <a name="set-up-a-dataset"></a>Veri kümesi ayarlama

1. Varsayılan değerleri kullanan bir dosya sistemi nesnesi oluşturun. RStudio'da aşağıdaki kodu girin:

    ```R
    # Define the HDFS (WASB) file system
    hdfsFS <- RxHdfsFileSystem()
    ```

1. Orijinal CSV dosyaları oldukça hantal değişken adları olduğundan, onları daha yönetilebilir hale getirmek için bir *colInfo* listesi kaynağı. RStudio'da aşağıdaki kodu girin:

    ```R
    airlineColInfo <- list(
         MONTH = list(newName = "Month", type = "integer"),
        DAY_OF_WEEK = list(newName = "DayOfWeek", type = "factor",
            levels = as.character(1:7),
            newLevels = c("Mon", "Tues", "Wed", "Thur", "Fri", "Sat",
                          "Sun")),
        UNIQUE_CARRIER = list(newName = "UniqueCarrier", type =
                                "factor"),
        ORIGIN = list(newName = "Origin", type = "factor"),
        DEST = list(newName = "Dest", type = "factor"),
        CRS_DEP_TIME = list(newName = "CRSDepTime", type = "integer"),
        DEP_TIME = list(newName = "DepTime", type = "integer"),
        DEP_DELAY = list(newName = "DepDelay", type = "integer"),
        DEP_DELAY_NEW = list(newName = "DepDelayMinutes", type =
                             "integer"),
        DEP_DEL15 = list(newName = "DepDel15", type = "logical"),
        DEP_DELAY_GROUP = list(newName = "DepDelayGroups", type =
                               "factor",
           levels = as.character(-2:12),
           newLevels = c("< -15", "-15 to -1","0 to 14", "15 to 29",
                         "30 to 44", "45 to 59", "60 to 74",
                         "75 to 89", "90 to 104", "105 to 119",
                         "120 to 134", "135 to 149", "150 to 164",
                         "165 to 179", ">= 180")),
        ARR_DELAY = list(newName = "ArrDelay", type = "integer"),
        ARR_DELAY_NEW = list(newName = "ArrDelayMinutes", type =
                             "integer"),  
        ARR_DEL15 = list(newName = "ArrDel15", type = "logical"),
        AIR_TIME = list(newName = "AirTime", type =  "integer"),
        DISTANCE = list(newName = "Distance", type = "integer"),
        DISTANCE_GROUP = list(newName = "DistanceGroup", type =
                             "factor",
         levels = as.character(1:11),
         newLevels = c("< 250", "250-499", "500-749", "750-999",
             "1000-1249", "1250-1499", "1500-1749", "1750-1999",
             "2000-2249", "2250-2499", ">= 2500")))
    
    varNames <- names(airlineColInfo)
    ```

## <a name="create-data-sources"></a>Veri kaynakları oluşturma

Bir Spark bilgi işlem bağlamında, aşağıdaki işlevleri kullanarak veri kaynakları oluşturabilirsiniz:

|İşlev | Açıklama |
|---------|-------------|
|`RxTextData` | Virgülle sınırlandırılmış metin veri kaynağı. |
|`RxXdfData` | XDF veri dosyası biçimindeki veriler. RevoScaleR'da, XDF dosya biçimi Hadoop'un verileri tek bir dosya yerine bileşik bir dosya kümesinde depolaması için değiştirilir. |
|`RxHiveData` | Bir Kovan Veri Kaynağı nesnesi oluşturur.|
|`RxParquetData` | Parke Veri Kaynağı nesnesi oluşturur.|
|`RxOrcData` | Bir Ork Veri Kaynağı nesnesi oluşturur.|

HDFS'ye kopyaladığınız dosyaları kullanarak bir [RxTextData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxtextdata) nesnesi oluşturun. RStudio'da aşağıdaki kodu girin:

```R
airDS <- RxTextData( airDataDir,
                        colInfo = airlineColInfo,
                        varsToKeep = varNames,
                        fileSystem = hdfsFS ) 
```

## <a name="create-a-compute-context-for-spark"></a>Kıvılcım için bilgi işlem bağlamı oluşturma

Veri yüklemek ve alt düğümler üzerinde analizleri çalıştırmak için, komut dosyanızdaki işlem bağlamını [RxSpark](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxspark)olarak ayarlarsınız. Bu bağlamda, R işlevleri iş yükünü tüm işçi düğümlerine otomatik olarak dağıtır ve işleri veya sırayı yönetmek için yerleşik bir gereksinim yoktur. Spark bilgi işlem bağlamı, `RxSpark` Spark bilgi işlem bağlamı aracılığıyla veya `rxSparkConnect()` `rxSparkDisconnect()` oluşturmak için kurulur ve yerel bir bilgi işlem bağlamına dönmek için kullanır. RStudio'da aşağıdaki kodu girin:

```R
# Define the Spark compute context
mySparkCluster <- RxSpark()

# Set the compute context
rxSetComputeContext(mySparkCluster)
```

## <a name="fit-a-linear-model"></a>Doğrusal bir model sığdır

1. Veri kaynağınızı `airDS` kullanarak doğrusal bir modele uyacak [şekilde rxLinMod](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlinmod) işlevini kullanın. RStudio'da aşağıdaki kodu girin:

    ```R
    system.time(
         delayArr <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDS,
              cube = TRUE)
    )
    ```
    
    Bu adım 2-3 dakika içinde tamamlanmalıdır.

1. Sonuçlara bakın. RStudio'da aşağıdaki kodu girin:

    ```R
    summary(delayArr)
    ```

    Aşağıdaki sonuçları görmeniz gerekir:

    ```output
    Call:
    rxLinMod(formula = ArrDelay ~ DayOfWeek, data = airDS, cube = TRUE)
    
    Cube Linear Regression Results for: ArrDelay ~ DayOfWeek
    Data: airDataXdf (RxXdfData Data Source)
    File name: /tutorial/data/AirOnTimeCSV2012
    Dependent variable(s): ArrDelay
    Total independent variables: 7 
    Number of valid observations: 6005381
    Number of missing observations: 91381 
     
    Coefficients:
                   Estimate Std. Error t value Pr(>|t|)     | Counts
    DayOfWeek=Mon   3.54210    0.03736   94.80 2.22e-16 *** | 901592
    DayOfWeek=Tues  1.80696    0.03835   47.12 2.22e-16 *** | 855805
    DayOfWeek=Wed   2.19424    0.03807   57.64 2.22e-16 *** | 868505
    DayOfWeek=Thur  4.65502    0.03757  123.90 2.22e-16 *** | 891674
    DayOfWeek=Fri   5.64402    0.03747  150.62 2.22e-16 *** | 896495
    DayOfWeek=Sat   0.91008    0.04144   21.96 2.22e-16 *** | 732944
    DayOfWeek=Sun   2.82780    0.03829   73.84 2.22e-16 *** | 858366
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
    Residual standard error: 35.48 on 6005374 degrees of freedom
    Multiple R-squared: 0.001827 (as if intercept included)
    Adjusted R-squared: 0.001826 
    F-statistic:  1832 on 6 and 6005374 DF,  p-value: < 2.2e-16 
    Condition number: 1 
    ```

    Sonuçlar, belirtilen dizindeki tüm CSV dosyalarını kullanarak tüm verileri, 6 milyon gözlemi işlediğinizi gösterir. Belirttiğiniz `cube = TRUE`için, haftanın her günü için tahmini katsayınız vardır (ve kesişme için değil).

## <a name="use-composite-xdf-files"></a>Bileşik XDF dosyalarını kullanma

Gördüğünüz gibi, Hadoop'taki R ile CSV dosyalarını doğrudan analiz edebilirsiniz. Ancak, verileri daha verimli bir biçimde depolarsanız, çözümlemesi daha hızlı yapabilirsiniz. R XDF dosya biçimi verimlidir, ancak tek tek dosyaların tek bir HDFS bloğu içinde kalması için HDFS için biraz değiştirilir. (HDFS blok boyutu kurulumdan kuruluma değişir, ancak genellikle 64 MB veya 128 MB'dır.) 

Bir bileşik XDF dosyası kümesi oluşturmak için Hadoop'ta [rxImport'u](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) `AirDS` kullandığınızda, inData `RxXdfData` gibi bir `RxTextData` veri kaynağı ve fileSystem'i outFile bağımsız değişkeni olarak HDFS dosya sistemine ayarlanmış bir veri kaynağı belirtirsiniz. Daha sonra sonraki `RxXdfData` R analizlerinde veri bağımsız değişkeni olarak nesneyi kullanabilirsiniz.

1. Bir `RxXdfData` nesne tanımlayın. RStudio'da aşağıdaki kodu girin:

    ```R
    airDataXdfDir <- file.path(bigDataDirRoot,"AirOnTimeXDF2012")
    
    airDataXdf <- RxXdfData( airDataXdfDir,
                            fileSystem = hdfsFS )
    ```

1. 250.000 satırlık bir blok boyutu ayarlayın ve tüm verileri okuduğumuzdan belirtin. RStudio'da aşağıdaki kodu girin:

    ```R
    blockSize <- 250000
    numRowsToRead = -1
    ```

1. Kullanarak verileri `rxImport`alma. RStudio'da aşağıdaki kodu girin:

    ```R
    rxImport(inData = airDS,
             outFile = airDataXdf,
             rowsPerRead = blockSize,
             overwrite = TRUE,
             numRows = numRowsToRead )
    ```
    
    Bu adım birkaç dakika içinde tamamlanmalıdır.

1. Yeni, daha hızlı veri kaynağını kullanarak aynı doğrusal modeli yeniden tahmin edin. RStudio'da aşağıdaki kodu girin:

    ```R
    system.time(
         delayArr <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDataXdf,
              cube = TRUE)
    )
    ```
    
    Adım bir dakikadan kısa sürede tamamlanmalıdır.

1. Sonuçlara bakın. Sonuçlar CSV dosyaları ile aynı olmalıdır. RStudio'da aşağıdaki kodu girin:

    ```R
    summary(delayArr)
    ```

## <a name="convert-xdf-to-csv"></a>XDF'yi CSV'ye dönüştürün

### <a name="in-a-spark-context"></a>Kıvılcım bağlamında

Analizleri çalıştırırken daha fazla verimlilik için CSV dosyalarınızı XDF dosya biçimine dönüştürdüyseniz, ancak şimdi verilerinizi CSV'ye geri dönüştürmek istiyorsanız, bunu [rxDataStep'i](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep)kullanarak yapabilirsiniz.

CSV dosyalarının bir klasörünü oluşturmak `RxTextData` için, önce dosya bağımsız değişkeni olarak bir dizin adı kullanarak bir nesne oluşturun. Bu nesne, CSV dosyalarını oluşturmak için klasörü temsil eder. Bu dizin, `rxDataStep`'yi çalıştırdığınızda oluşturulur. Daha sonra, `RxTextData` `outFile` bu nesneye işaret `rxDataStep`argümanı . Oluşturulan her CSV dizin adına göre adlandırılır ve ardından bir sayı.

Yeni CSV dosyalarının öngörülen değerleri ve artıkları içermesini `airDataXdf` sağlamak için, lojistik regresyon ve tahmin gerçekleştirmenizden sonra bileşik XDF'nizden HDFS'deki CSV dosyalarının bir klasörünü yazmak istediğinizi varsayalım. RStudio'da aşağıdaki kodu girin:

```R
airDataCsvDir <- file.path(bigDataDirRoot,"AirDataCSV2012")
airDataCsvDS <- RxTextData(airDataCsvDir,fileSystem=hdfsFS)
rxDataStep(inData=airDataXdf, outFile=airDataCsvDS)
```

Bu adım yaklaşık 2,5 dakika içinde tamamlanmalıdır.

Giriş `rxDataStep` bileşik XDF dosyasında her XDFD dosyası için bir CSV dosyası yazdı. Bu, bilgi işlem bağlamı ayarlandığında bileşik XDF dosyalarından HDFS'ye CSV dosyaları yazmak için `RxSpark`varsayılan davranıştır.

### <a name="in-a-local-context"></a>Yerel bağlamda

Alternatif olarak, analizlerinizi gerçekleştirmeyi bitirdiğinizde, CSV dosyalarını `local` HDFS'ye yazarken `RxTextData` size biraz daha fazla denetim sağlayan iki bağımsız değişkenden `createFileSet` yararlanmak `rowsPerOutFile`için bilgi işlem bağlamınızı yeniden değiştirebilirsiniz: ve . `TRUE`Ayarladığınızda, `createFileSet` csv dosyalarının bir klasörü belirttiğiniz dizine yazılır. Ayarlarken `createFileSet` `FALSE`, tek bir CSV dosyası yazılır. İkinci bağımsız değişkeni, `rowsPerOutFile`her CSV dosyasına `createFileSet` kaç satır yazılması gerektiğini belirtmek için `TRUE`bir arameme ayarlayabilirsiniz.

RStudio'da aşağıdaki kodu girin:

```R
rxSetComputeContext("local")
airDataCsvRowsDir <- file.path(bigDataDirRoot,"AirDataCSVRows2012")
airDataCsvRowsDS <- RxTextData(airDataCsvRowsDir, fileSystem=hdfsFS, createFileSet=TRUE, rowsPerOutFile=1000000)
rxDataStep(inData=airDataXdf, outFile=airDataCsvRowsDS)
```

Bu adım yaklaşık 10 dakika içinde tamamlanmalıdır.

Bir `RxSpark` bilgi işlem bağlamı `createFileSet` kullandığınızda, `TRUE` `rowsPerOutFile` varsayılan olarak hiçbir etkisi yoktur. Bu nedenle, tek bir CSV oluşturmak veya dosya başına satır sayısını `rxDataStep` özelleştirmek istiyorsanız, bir `local` bilgi işlem bağlamında gerçekleştirin (veriler hala HDFS'de olabilir).

## <a name="final-steps"></a>Son adımlar

1. Verileri temizleyin. RStudio'da aşağıdaki kodu girin:

    ```R
    rxHadoopRemoveDir(airDataDir)
    rxHadoopRemoveDir(airDataXdfDir)
    rxHadoopRemoveDir(airDataCsvDir)
    rxHadoopRemoveDir(airDataCsvRowsDir)
    rxHadoopRemoveDir(bigDataDirRoot)
    ```

1. Uzak Kıvılcım uygulamasını durdurun. RStudio'da aşağıdaki kodu girin:

    ```R
    rxStopEngine(mySparkCluster)
    ```

1. R seansını bırak. RStudio'da aşağıdaki kodu girin:

    ```R
    quit()
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığı bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

Bir kümeyi silmek için bkz: [Tarayıcınızı, PowerShell'i veya Azure CLI'yi kullanarak bir HDInsight kümesini sil.](../hdinsight-delete-cluster.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, HDInsight Machine Learning hizmetleri kümesinde çalışan Apache Spark'ta R işlevlerini nasıl kullanacağınızı öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure HDInsight Machine Learning hizmetleri kümesi için içerik seçeneklerini bilgi işlem](r-server-compute-contexts.md)
* [Hadoop üzerinde Kıvılcım için R Fonksiyonları](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler-hadoop-functions)
