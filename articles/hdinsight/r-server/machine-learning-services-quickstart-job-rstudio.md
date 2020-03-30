---
title: 'Quickstart: RStudio Server & R için ML Hizmetleri - Azure HDInsight'
description: Hızlı başlangıç ta, RStudio Server'ı kullanarak Azure HDInsight'ta ML Hizmetleri kümesinde bir R komut dosyası çalıştırın.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 06/19/2019
ms.author: hrasheed
ms.custom: mvc
ms.openlocfilehash: 8a6a204ee5080e3acf99c13ecba1e1c7664d68b4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73241892"
---
# <a name="quickstart-execute-an-r-script-on-an-ml-services-cluster-in-azure-hdinsight-using-rstudio-server"></a>Hızlı başlatma: RStudio Server'ı kullanarak Azure HDInsight'ta ML Hizmetleri kümesinde Bir R komut dosyası çalıştırma

Azure HDInsight'taki ML Hizmetleri, Dağıtılmış hesaplamaları çalıştırmak için R komut dosyalarının Apache Spark ve Apache Hadoop MapReduce'i kullanmasına olanak tanır. ML Hizmetleri, bilgi işlem bağlamını ayarlayarak çağrıların nasıl yürütüldedildiğini denetler. Bir kümenin kenar düğümü kümeye bağlanmak ve R komut dosyalarınızı çalıştırmak için uygun bir yer sağlar. Kenar düğümü ile, RevoScaler'ın paralelleştirilmiş dağıtılmış işlevlerini kenar düğümü sunucusunun çekirdekleri arasında çalıştırma seçeneğiniz vardır. RevoScaleR'ın Hadoop Harita Azaltma veya Apache Spark bilgi işlem bağlamlarını kullanarak kümenin düğümleri üzerinde de çalıştırabilirsiniz.

Bu hızlı başlatmada, dağıtılmış R hesaplamaları için Spark'ı kullanarak gösteren RStudio Server ile nasıl çalıştırılabileceğinizi öğrenirsiniz. Bir kenar düğümünde yerel olarak hesaplamalar gerçekleştirmek için bir bilgi işlem bağlamı tanımlarsınız ve yine HDInsight kümesindeki düğümler arasında dağıtılır.

## <a name="prerequisite"></a>Önkoşul

HDInsight'ta bir ML Hizmetleri kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun](../hdinsight-hadoop-create-linux-clusters-portal.md) ve Küme türü için **ML Hizmetleri'ni** seçin. **Cluster type**

## <a name="connect-to-rstudio-server"></a>RStudio Server’a bağlanma

RStudio Server kümenin kenar düğümünde çalışır. Oluşturduğunuz ML Hizmetleri `CLUSTERNAME` kümesinin adı aşağıdaki URL'ye gidin:

```
https://CLUSTERNAME.azurehdinsight.net/rstudio/
```

İlk oturum açğınızda iki kez kimlik doğrulamanız gerekir. İlk kimlik doğrulama istemi için, küme Admin giriş ve `admin`şifre sağlamak, varsayılan . İkinci kimlik doğrulama istemi için, SSH giriş ve `sshuser`parola sağlamak, varsayılan . Sonraki oturum açmalar yalnızca SSH kimlik bilgilerini gerektirir.

Bağlandıktan sonra ekranınız aşağıdaki ekran görüntüsüne benzemelidir:

![R stüdyo web konsolu genel bakış](./media/ml-services-quickstart-job-rstudio/connect-to-r-studio1.png)

## <a name="use-a-compute-context"></a>İşlem bağlamı kullanma

1. RStudio Server'dan, HDInsight için varsayılan depolama alanına örnek verileri yüklemek için aşağıdaki kodu kullanın:

    ```RStudio
    # Set the HDFS (WASB) location of example data
     bigDataDirRoot <- "/example/data"
    
     # create a local folder for storing data temporarily
     source <- "/tmp/AirOnTimeCSV2012"
     dir.create(source)
    
     # Download data to the tmp folder
     remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
     download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
     download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
     download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
     download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
     download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
     download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
     download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
     download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
     download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
     download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
     download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
     download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))
    
     # Set directory in bigDataDirRoot to load the data into
     inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")
    
     # Make the directory
     rxHadoopMakeDir(inputDir)
    
     # Copy the data from source to input
     rxHadoopCopyFromLocal(source, bigDataDirRoot)
    ```

    Bu adımın tamamlanması yaklaşık 8 dakika sürebilir.

1. Bazı veri bilgileri oluşturun ve iki veri kaynağı tanımlayın. RStudio'ya aşağıdaki kodu girin:

    ```RStudio
    # Define the HDFS (WASB) file system
     hdfsFS <- RxHdfsFileSystem()
    
     # Create info list for the airline data
     airlineColInfo <- list(
          DAY_OF_WEEK = list(type = "factor"),
          ORIGIN = list(type = "factor"),
          DEST = list(type = "factor"),
          DEP_TIME = list(type = "integer"),
          ARR_DEL15 = list(type = "logical"))
    
     # get all the column names
     varNames <- names(airlineColInfo)
    
     # Define the text data source in hdfs
     airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)
    
     # Define the text data source in local system
     airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)
    
     # formula to use
     formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"
    ```

1. **Yerel** bilgi işlem bağlamını kullanarak veriler üzerinde lojistik bir gerileme çalıştırın. RStudio'ya aşağıdaki kodu girin:

    ```RStudio
    # Set a local compute context
     rxSetComputeContext("local")
    
     # Run a logistic regression
     system.time(
        modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
     )
    
     # Display a summary
     summary(modelLocal)
    ```

    Hesaplamalar yaklaşık 7 dakika içinde tamamlanmalıdır. Aşağıdaki parçacıka benzer çizgilerle biten çıktıyı görmeniz gerekir:

    ```output
    Data: airOnTimeDataLocal (RxTextData Data Source)
     File name: /tmp/AirOnTimeCSV2012
     Dependent variable(s): ARR_DEL15
     Total independent variables: 634 (Including number dropped: 3)
     Number of valid observations: 6005381
     Number of missing observations: 91381
     -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)
    
     Coefficients:
                      Estimate Std. Error z value Pr(>|z|)
      (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
      ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
      ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
      ......
      DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
      DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
      DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
      DEST=BPT         Dropped    Dropped Dropped  Dropped
    
      ---
    
      Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
      Condition number of final variance-covariance matrix: 11904202
      Number of iterations: 7
    ```

1. **Kıvılcım** bağlamını kullanarak aynı lojistik regresyonu çalıştırın. Spark bağlamı, işlemi HDInsight kümesindeki tüm çalışan düğümlerine dağıtır. RStudio'ya aşağıdaki kodu girin:

    ```RStudio
    # Define the Spark compute context
     mySparkCluster <- RxSpark()
    
     # Set the compute context
     rxSetComputeContext(mySparkCluster)
    
     # Run a logistic regression
     system.time(  
        modelSpark <- rxLogit(formula, data = airOnTimeData)
     )
    
     # Display a summary
     summary(modelSpark)
    ```

    Hesaplamalar yaklaşık 5 dakika içinde tamamlanmalıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [Tarayıcınızı, PowerShell'i veya Azure CLI'yi kullanarak bir HDInsight kümesini sil.](../hdinsight-delete-cluster.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dağıtılmış R hesaplamaları için Spark'ı kullanarak gösterildiği RStudio Server ile nasıl çalıştırılabileceğinizi öğrendiniz.  Yürütmenin kenar düğümüveya HDInsight kümesinin çekirdekleri arasında paralelleştirilip paralelleştirilmediğini belirtmek için kullanılabilen seçenekleri öğrenmek için bir sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[HDInsight'ta ML Hizmetleri için bilgi işlem bağlam ı seçenekleri](./r-server-compute-contexts.md)

> [!NOTE]
> Bu sayfada RStudio yazılımının özellikleri açıklanmaktadır. Microsoft Azure HDInsight, RStudio, Inc.'e bağlı değildir.
