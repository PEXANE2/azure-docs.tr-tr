---
title: Azure HDInsight 'ta sorun giderme
description: Azure HDInsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/27/2020
ms.custom: seodec18
ms.openlocfilehash: 6de9e31c3e79f6d704ef8b4749d41329dcc0bddb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82190698"
---
# <a name="troubleshoot-apache-hadoop-hdfs-by-using-azure-hdinsight"></a>Azure HDInsight kullanarak Apache Hadoop HDFS sorunlarını giderme

Hadoop Dağıtılmış Dosya Sistemi (bir) ile çalışırken popüler sorunlar ve çözümleri öğrenin. Komutların tam listesi için, bkz... [komut Kılavuzu](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HDFSCommands.html) ve [dosya sistemi kabuğu Kılavuzu](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html).

## <a name="how-do-i-access-the-local-hdfs-from-inside-a-cluster"></a><a name="how-do-i-access-local-hdfs-from-inside-a-cluster"></a>Yerel olarak bir küme içinden erişim Nasıl yaparım? mi?

### <a name="issue"></a>Sorun

Azure Blob depolama veya HDInsight kümesinin içinden Azure Data Lake Storage yerine komut satırı ve uygulama kodundan yerel olarak erişin.

### <a name="resolution-steps"></a>Çözüm adımları

1. Komut isteminde, aşağıdaki komutta olduğu `hdfs dfs -D "fs.default.name=hdfs://mycluster/" ...` gibi, harfine kullanın:

    ```output
    hdfs dfs -D "fs.default.name=hdfs://mycluster/" -ls /
    Found 3 items
    drwxr-xr-x   - hdiuser hdfs          0 2017-03-24 14:12 /EventCheckpoint-30-8-24-11102016-01
    drwx-wx-wx   - hive    hdfs          0 2016-11-10 18:42 /tmp
    drwx------   - hdiuser hdfs          0 2016-11-10 22:22 /user
    ```

2. Kaynak koddan aşağıdaki örnek uygulamada olduğu gibi `hdfs://mycluster/` , URI 'yi tam olarak kullanın:

    ```Java
    import java.io.IOException;
    import java.net.URI;
    import org.apache.commons.io.IOUtils;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.*;

    public class JavaUnitTests {

        public static void main(String[] args) throws Exception {

            Configuration conf = new Configuration();
            String hdfsUri = "hdfs://mycluster/";
            conf.set("fs.defaultFS", hdfsUri);
            FileSystem fileSystem = FileSystem.get(URI.create(hdfsUri), conf);
            RemoteIterator<LocatedFileStatus> fileStatusIterator = fileSystem.listFiles(new Path("/tmp"), true);
            while(fileStatusIterator.hasNext()) {
                System.out.println(fileStatusIterator.next().getPath().toString());
            }
        }
    }
    ```

3. Aşağıdaki komutla, HDInsight kümesinde derlenen. jar dosyasını (örneğin, adlı `java-unit-tests-1.0.jar`bir dosya) çalıştırın:

    ```apache
    hadoop jar java-unit-tests-1.0.jar JavaUnitTests
    hdfs://mycluster/tmp/hive/hive/5d9cf301-2503-48c7-9963-923fb5ef79a7/inuse.info
    hdfs://mycluster/tmp/hive/hive/5d9cf301-2503-48c7-9963-923fb5ef79a7/inuse.lck
    hdfs://mycluster/tmp/hive/hive/a0be04ea-ae01-4cc4-b56d-f263baf2e314/inuse.info
    hdfs://mycluster/tmp/hive/hive/a0be04ea-ae01-4cc4-b56d-f263baf2e314/inuse.lck
    ```

## <a name="storage-exception-for-write-on-blob"></a>Blob üzerinde yazma için depolama özel durumu

### <a name="issue"></a>Sorun

Bir HBase `hadoop` kümesinde `hdfs dfs` ~ 12 GB veya daha büyük olan dosyaları yazmak için veya komutlarını kullanırken, şu hata boyunca karşılaşabilirsiniz:

```error
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
```

### <a name="cause"></a>Nedeni

HDInsight kümelerindeki HBase, Azure depolama 'ya yazarken varsayılan olarak 256 KB 'lik bir blok boyutuna sahiptir. HBase API 'Leri veya REST API 'Leri için çalışırken, `hadoop` veya `hdfs dfs` komut satırı yardımcı programları kullanılırken bir hatayla sonuçlanır.

### <a name="resolution"></a>Çözüm

Daha `fs.azure.write.request.size` büyük bir blok boyutunu belirtmek için kullanın. Bu değişikliği, `-D` parametresini kullanarak kullanım başına temelinde yapabilirsiniz. Aşağıdaki komut, `hadoop` komutuyla bu parametreyi kullanan bir örnektir:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

Ayrıca, Apache ambarı 'nı kullanarak `fs.azure.write.request.size` genel değeri de artırabilirsiniz. Aşağıdaki adımlar, ambarı Web Kullanıcı arabirimindeki değeri değiştirmek için kullanılabilir:

1. Tarayıcınızda kümeniz için ambarı Web Kullanıcı arabirimine gidin. URL `https://CLUSTERNAME.azurehdinsight.net`, burada `CLUSTERNAME` kümenizin adıdır. İstendiğinde, kümenin yönetici adını ve parolasını girin.
2. Ekranın sol tarafında, bu ' ı seçin ve ardından **configs** **sekmesini seçin.**
3. **Filter...** alanına, girin `fs.azure.write.request.size`.
4. 262144 (256 KB) değerini yeni değerle değiştirin. Örneğin, 4194304 (4 MB).

    ![Ambarı Web Kullanıcı arabirimi üzerinden değerin değiştirilmesinin görüntüsü](./media/hdinsight-troubleshoot-hdfs/hbase-change-block-write-size.png)

Ambarı kullanma hakkında daha fazla bilgi için bkz. [Apache ambarı Web Kullanıcı arabirimini kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md).

## <a name="du"></a>du

Komut [`-du`](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html#du) , belirli bir dizinde bulunan dosya ve dizinlerin boyutlarını veya yalnızca bir dosya olması durumunda bir dosyanın uzunluğunu görüntüler.

Seçeneği `-s` , görüntülenmekte olan dosya uzunluklarının toplam özetini oluşturur.  
`-h` Seçeneği, dosya boyutlarını biçimlendirir.

Örnek:

```bash
hdfs dfs -du -s -h hdfs://mycluster/
hdfs dfs -du -s -h hdfs://mycluster/tmp
```

## <a name="rm"></a>'yi

[-RM](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html#rm) komutu bağımsız değişken olarak belirtilen dosyaları siler.

Örnek:

```bash
hdfs dfs -rm hdfs://mycluster/tmp/testfile
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
