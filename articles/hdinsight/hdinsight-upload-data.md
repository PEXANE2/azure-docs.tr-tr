---
title: HDInsight'ta Apache Hadoop işleri için veri yükleyin
description: HDInsight'ta Apache Hadoop işleri için verileri nasıl yükleyeceğinizi ve bu işe nasıl erişeceğinizi öğrenin. Azure klasik CLI, Azure Depolama Gezgini, Azure PowerShell, Hadoop komut satırı veya Sqoop'u kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdiseo17may2017
ms.date: 04/07/2020
ms.openlocfilehash: c862633245e75613f9e4f9956486f872b96239f8
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875019"
---
# <a name="upload-data-for-apache-hadoop-jobs-in-hdinsight"></a>HDInsight'ta Apache Hadoop işleri için veri yükleyin

HDInsight, Azure Depolama üzerinden Hadoop dağıtılmış dosya sistemi (HDFS) ve Azure Veri Gölü Depolama sağlar. Bu depolama Gen1 ve Gen2 içerir. Azure Depolama ve Veri Gölü Depolama Gen1 ve Gen2 HDFS uzantıları olarak tasarlanmıştır. Hadoop ortamındaki tüm bileşenlerin doğrudan yönettiği veriler üzerinde çalışmasını sağlarlar. Azure Depolama, Veri Gölü Depolama Gen1 ve Gen2 farklı dosya sistemleridir. Sistemler, bu verilerdeki verilerin ve hesaplamaların saklanması için optimize edilmiştir. Azure Depolama'yı kullanmanın yararları hakkında bilgi için [bkz.](hdinsight-hadoop-use-blob-storage.md) Ayrıca bakınız, [HDInsight ile Veri Gölü Depolama Gen1 kullanın](hdinsight-hadoop-use-data-lake-store.md)ve [HDInsight ile Veri Gölü Depolama Gen2 kullanın.](hdinsight-hadoop-use-data-lake-storage-gen2.md)

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdaki gereksinimleri not edin:

* Bir Azure HDInsight kümesi. Talimatlar için bkz: [Azure HDInsight ile başlayın](hadoop/apache-hadoop-linux-tutorial-get-started.md) veya [HDInsight kümeleri oluşturun.](hdinsight-hadoop-provision-linux-clusters.md)
* Aşağıdaki makalelerin bilgisi:
    * [HDInsight ile Azure Depolama'yı kullanma](hdinsight-hadoop-use-blob-storage.md)
    * [HDInsight ile Veri Gölü Depolama Gen1'i kullanma](hdinsight-hadoop-use-data-lake-store.md)
    * [HDInsight ile Veri Gölü Depolama Gen2'yi Kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)  

## <a name="upload-data-to-azure-storage"></a>Azure Depolama'ya veri yükleme

## <a name="utilities"></a>Yardımcı Programlar

Microsoft, Azure Depolama ile çalışmak için aşağıdaki yardımcı programları sağlar:

| Araç | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure portalı](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure CLI](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [Azcopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |
| [Hadoop komutu](#hadoop-command-line) |✔ |✔ |✔ |

> [!NOTE]  
> Hadoop komutu yalnızca HDInsight kümesinde kullanılabilir. Komut yalnızca yerel dosya sisteminden gelen verilerin Azure Depolama'ya yüklenmesine izin verir.  

## <a name="hadoop-command-line"></a>Hadoop komut satırı

Hadoop komut satırı, yalnızca veri küme başlığı düğümünde zaten mevcut olduğunda Azure depolama blob'una veri depolamak için kullanışlıdır.

Hadoop komutunu kullanmak için önce [SSH veya PuTTY](hdinsight-hadoop-linux-use-ssh-unix.md)kullanarak kafa düğümüne bağlanmanız gerekir.

Bağlandıktan sonra, bir dosyayı depolama alanına yüklemek için aşağıdaki sözdizimini kullanabilirsiniz.

```bash
hadoop fs -copyFromLocal <localFilePath> <storageFilePath>
```

Örneğin, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

HDInsight için varsayılan dosya sistemi Azure Depolama'da olduğundan, /example/data/data.txt aslında Azure Depolama'dadır. Dosyaya şu şekilde de başvurabilirsiniz:

    wasbs:///example/data/data.txt

or

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Dosyalarla çalışan diğer Hadoop komutlarının listesi için bkz.[https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]  
> Apache HBase kümelerinde, veri yazarken kullanılan varsayılan blok boyutu 256 KB'dir. Bu, HBase API'leri veya REST API'leri kullanırken iyi çalışırken, ~12 GB'dan daha büyük verileri yazmak için `hadoop` veya `hdfs dfs` komutları kullanarak bir hatayla sonuçlanır. Daha fazla bilgi için, bu makaledeki [blob bölümüne yazmak için depolama özel durumu](#storage-exception-for-write-on-blob) bakın.

## <a name="graphical-clients"></a>Grafik istemcileri

Azure Depolama ile çalışmak için grafik arabirimi sağlayan çeşitli uygulamalar da vardır. Aşağıdaki tablo, bu uygulamalardan birkaçı listesidir:

| İstemci | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [HDInsight için Microsoft Visual Studio Araçları](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources) |✔ |✔ |✔ |
| [Azure Depolama Gezgini](../storage/blobs/storage-quickstart-blobs-storage-explorer.md) |✔ |✔ |✔ |
| [`Cerulea`](https://www.cerebrata.com/products/cerulean/features/azure-storage) | | |✔ |
| [CloudXplorer](https://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Microsoft Azure için CloudBerry Explorer](https://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

## <a name="mount-azure-storage-as-local-drive"></a>Azure Depolamayı Yerel Sürücü Olarak Dağıla

[Azure Depolama Dağı'nı Yerel Sürücü olarak](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx)görün.

## <a name="upload-using-services"></a>Hizmetleri kullanarak yükleme

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Veri Fabrikası hizmeti, veri oluşturmak için tam olarak yönetilen bir hizmettir: depolama, işleme ve hareket hizmetleri, kolaylaştırılmış, uyarlanabilir ve güvenilir veri üretim boru hatlarına.

|Depolama türü|Belgeler|
|----|----|
|Azure Blob depolama|[Azure Veri Fabrikası'nı kullanarak verileri Azure Blob depolamasına veya Azure Blob depolamasından kopyalama](../data-factory/connector-azure-blob-storage.md)|
|Azure Data Lake Storage Gen1|[Azure Veri Fabrikası'nı kullanarak verileri Azure Veri Gölü Depolama Gen1'e veya Azure Veri Gölü Deposu Gen1'den kopyalama](../data-factory/connector-azure-data-lake-store.md)|
|Azure Data Lake Storage Gen2 |[Azure Veri Fabrikası ile Verileri Azure Veri Gölü Depolama Gen2'ye yükleyin](../data-factory/load-azure-data-lake-storage-gen2.md)|

### <a name="apache-sqoop"></a>Apaçi Sqoop

Sqoop Hadoop ve ilişkisel veritabanları arasında veri aktarmak için tasarlanmış bir araçtır. SQL Server, MySQL veya Oracle gibi ilişkisel veritabanı yönetim sisteminden (RDBMS) veri almak için kullanın. Sonra Hadoop dağıtılan dosya sistemi (HDFS) içine. MapReduce veya Hive ile Hadoop'taki verileri dönüştürün ve verileri bir RDBMS'ye geri aktarın.

Daha fazla bilgi için [HDInsight ile Sqoop'u Kullanın'a](hadoop/hdinsight-use-sqoop.md)bakın.

### <a name="development-sdks"></a>Geliştirme SDK'ları

Azure Depolama'ya aşağıdaki programlama dillerinden bir Azure SDK kullanılarak da erişilebilir:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Azure SDK'larını yükleme hakkında daha fazla bilgi için [Azure indirmelerine](https://azure.microsoft.com/downloads/) bakın

## <a name="troubleshooting"></a>Sorun giderme

### <a name="storage-exception-for-write-on-blob"></a>Blob yazmak için depolama özel durumu

**Belirtiler**: Bir `hadoop` HBase kümesinde ~12 GB veya daha büyük olan dosyaları yazmak için veya `hdfs dfs` komutları kullanırken aşağıdaki hatayla karşılaşabilirsiniz:

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**Neden**: HDInsight kümelerinde HBase, Azure depolamaalanına yazarken varsayılan olarak 256 KB blok boyutuna bağlıdır. HBase API'leri veya REST API'leri için çalışırken, `hadoop` `hdfs dfs` komut satırı yardımcı hizmetlerini kullanırken bir hataya neden olur.

**Çözünürlük**: `fs.azure.write.request.size` Daha büyük bir blok boyutunu belirtmek için kullanın. Bu değişikliği `-D` parametreyi kullanarak kullanım başına yapabilirsiniz. Aşağıdaki `hadoop` komut, komutu ile bu parametre kullanarak bir örnektir:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

Ayrıca Apache Ambari kullanarak `fs.azure.write.request.size` küresel değerini artırabilir. The following steps can be used to change the value in the Ambari Web UI:

1. Tarayıcınızda, kümeniz için Ambari Web UI'ye gidin. `https://CLUSTERNAME.azurehdinsight.net`URL, kümenizin `CLUSTERNAME` adıdır. İstendiğinde, kümenin yönetici adını ve parolasını girin.
2. Ekranın sol tarafından **HDFS'yi**seçin ve ardından **Configs** sekmesini seçin.
3. **Filtre...** alanına girin. `fs.azure.write.request.size`
4. Değeri 262144'ten (256 KB) yeni değere değiştirin. Örneğin, 4194304 (4 MB).

    ![Ambari Web UI ile değeri değiştirme görüntüsü](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Ambari'yi kullanma hakkında daha fazla bilgi için [Apache Ambari Web UI'yi kullanarak HDInsight kümelerini yönet'e](hdinsight-hadoop-manage-ambari.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Artık HDInsight'a nasıl veri alacağınızı anladığınıziçin, analiz öğrenmek için aşağıdaki makaleleri okuyun:

* [Azure HDInsight ile başlayın](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Apache Hadoop işlerini programlı olarak gönderin](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [HDInsight ile Apache Hive'ı kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Apache Pig'i kullanma](./use-pig.md)
