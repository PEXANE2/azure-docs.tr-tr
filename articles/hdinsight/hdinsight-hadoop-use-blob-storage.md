---
title: HDFS uyumlu Azure Depolama'da veri sorgulama - Azure HDInsight
description: Azure depolama 'dan verileri sorgulamayı ve analizin sonuçlarını depolamak için Azure Data Lake Storage öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: 7941748f7f917847e551b0cf5cd0a7bf926d31a9
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086985"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile Azure Depolama'yı kullanma

Verileri [Azure depolama](../storage/common/storage-introduction.md), [Azure Data Lake Storage gen 1](../data-lake-store/data-lake-store-overview.md)veya [Azure Data Lake Storage Gen 2](../storage/blobs/data-lake-storage-introduction.md)' de saklayabilirsiniz. Ya da bu seçeneklerin bir birleşimi. Bu depolama seçenekleri, hesaplama için kullanılan HDInsight kümelerini Kullanıcı verilerini kaybetmeden güvenle silmenizi sağlar.

Apache Hadoop, varsayılan dosya sisteminin bir kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight kümesi oluşturma işlemi sırasında Azure depolama 'da varsayılan dosya sistemi olarak bir blob kapsayıcısı belirtebilirsiniz. Ya da HDInsight 3,6 ile, birkaç özel durum dışında Azure Storage veya Azure Data Lake Storage Gen 1/Azure Data Lake Storage Gen 2 ' yi varsayılan dosya sistemi olarak seçebilirsiniz. Hem varsayılan hem de bağlı depolama alanı olarak Data Lake Storage Gen 1 kullanmanın desteklenebilirliği için bkz. [HDInsight kümesi Için kullanılabilirlik](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

Bu makalede Azure Depolama'nın HDInsight kümeleri ile nasıl çalıştığı hakkında bilgi edinebilirsiniz. Data Lake Storage Gen 1 ' in HDInsight kümeleri ile nasıl çalıştığını öğrenmek için bkz. [Azure HDInsight kümeleri ile Azure Data Lake Storage kullanma](hdinsight-hadoop-use-data-lake-store.md). HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz. [HDInsight 'ta Apache Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

> [!IMPORTANT]  
> Depolama hesabı türü **blobstorage** yalnızca HDInsight kümeleri için ikincil depolama alanı olarak kullanılabilir.

| Depolama hesabı türü | Desteklenen hizmetler | Desteklenen performans katmanları |Desteklenmeyen performans katmanları| Desteklenen erişim katmanları |
|----------------------|--------------------|-----------------------------|---|------------------------|
| StorageV2 (genel amaçlı v2)  | Blob     | Standart                    |Premium| Sık erişimli, seyrek erişimli Arşiv\*   |
| Depolama (genel amaçlı v1)   | Blob     | Standart                    |Premium| YOK                    |
| BlobStorage                    | Blob     | Standart                    |Premium| Sık erişimli, seyrek erişimli Arşiv\*   |

İş verilerini depolamak için varsayılan blob kapsayıcısını kullanmanızı önermiyoruz. Depolama maliyetini azaltmak için blob kapsayıcısının her kullanımdan sonra silinmesi iyi bir uygulamadır. Varsayılan kapsayıcı, uygulama ve sistem günlükleri içerir. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Birden çok küme için varsayılan dosya sistemi olarak bir blob kapsayıcısının paylaşılması desteklenmez.

> [!NOTE]  
> Arşiv erişim katmanı, birkaç saat alma gecikmesi olan ve HDInsight ile kullanım için önerilmeyen bir çevrimdışı katmandır. Daha fazla bilgi için bkz. [Arşiv erişim katmanı](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

## <a name="access-files-from-within-cluster"></a>Küme içinden dosyalara erişme

HDInsight kümesinden Data Lake Storage dosyalara erişmek için çeşitli yollar vardır. URI şeması, şifrelenmemiş erişim (yani, ile birlikte *:* ön ek ile) ve TLS şifreli erişimi ( *GB*'ler ile) sağlar. Azure’da aynı bölgede bulunan verilere erişirken dahi mümkün olduğunda *wasbs* kullanmanızı öneririz.

* **Tam adı kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın tam yolunu girersiniz.

    ```
    wasb://<containername>@<accountname>.blob.core.windows.net/<file.path>/
    wasbs://<containername>@<accountname>.blob.core.windows.net/<file.path>/
    ```

* **Kısaltılmış yol biçimi kullanarak**. Bu yaklaşımda, yolu küme köküne kadar değiştirirsiniz:

    ```
    wasb:///<file.path>/
    wasbs:///<file.path>/
    ```

* **Göreli yolu kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın yalnızca göreli yolunu girersiniz.

    ```
    /<file.path>/
    ```

### <a name="data-access-examples"></a>Veri erişimi örnekleri

Örnekler, kümenin baş düğümüne bir [ssh bağlantısını](./hdinsight-hadoop-linux-use-ssh-unix.md) temel alır. Örneklerde, üç URI şeması da kullanılır. `CONTAINERNAME`Ve `STORAGEACCOUNT` değerlerini ilgili değerlerle değiştirin

#### <a name="a-few-hdfs-commands"></a>Birkaç bir bu komut

1. Yerel depolamada bir dosya oluşturun.

    ```bash
    touch testFile.txt
    ```

1. Küme depolamada dizinler oluşturun.

    ```bash
    hdfs dfs -mkdir wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -mkdir wasbs:///sampledata2/
    hdfs dfs -mkdir /sampledata3/
    ```

1. Yerel depolamadan küme depolamaya veri kopyalama.

    ```bash
    hdfs dfs -copyFromLocal testFile.txt  wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -copyFromLocal testFile.txt  wasbs:///sampledata2/
    hdfs dfs -copyFromLocal testFile.txt  /sampledata3/
    ```

1. Küme depolamada dizin içeriğini listeleyin.

    ```bash
    hdfs dfs -ls wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -ls wasbs:///sampledata2/
    hdfs dfs -ls /sampledata3/
    ```

> [!NOTE]  
> HDInsight dışındaki blob'larla çalışırken, yardımcı programların çoğu WASB biçimini tanımaz ve bunun yerine, `example/jars/hadoop-mapreduce-examples.jar` gibi temel yol biçimi gibi bekler.

#### <a name="creating-a-hive-table"></a>Hive tablosu oluşturma

Üç dosya konumu tanım amacıyla gösterilmiştir. Gerçek yürütme için girdilerden yalnızca birini kullanın `LOCATION` .

```hql
DROP TABLE myTable;
CREATE EXTERNAL TABLE myTable (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE
LOCATION 'wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/example/data/';
LOCATION 'wasbs:///example/data/';
LOCATION '/example/data/';
```

## <a name="access-files-from-outside-cluster"></a>Dış kümeden dosyalara erişme

Microsoft, Azure depolama ile çalışmak için aşağıdaki araçları sağlar:

| Araç | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure portalındaki](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure CLI](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |

## <a name="identify-storage-path-from-ambari"></a>Ambarı 'ndan depolama yolunu tanımla

* Yapılandırılmış varsayılan deponun tüm yolunu belirlemek için şuraya gidin:

    **HDFS** Bu  >  **Ve sonra** `fs.defaultFS` filtre giriş kutusuna girin.

* Bellek deposunun ikincil depolama olarak yapılandırılıp yapılandırılmadığını denetlemek için şuraya gidin:

    **HDFS** Bu  >  **Ve sonra** `blob.core.windows.net` filtre giriş kutusuna girin.

Ambarı REST API kullanarak yolu elde etmek için bkz. [varsayılan depolamayı alma](./hdinsight-hadoop-manage-ambari-rest-api.md#get-the-default-storage).

## <a name="blob-containers"></a>Blob kapsayıcıları

Blob'ları kullanmak için önce bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md) oluşturursunuz. Bu adımın bir parçası olarak, depolama hesabının oluşturulduğu bir Azure bölgesi belirtirsiniz. Küme ve depolama hesabının aynı bölgede barındırılması gerekir. Hive meta veri deposu SQL Server veritabanı ve Apache Oozie metaser SQL Server veritabanı aynı bölgede bulunmalıdır.

Nerede olursa olsun, oluşturduğunuz her blob Azure Storage hesabınızdaki bir kapsayıcıya aittir. Bu kapsayıcı, HDInsight dışında oluşturulmuş mevcut bir BLOB olabilir. Ya da bir HDInsight kümesi için oluşturulmuş bir kapsayıcı olabilir.

Varsayılan Blob kapsayıcısı iş geçmişi ve iş günlükleri gibi kümeye özel bilgileri depolar. Varsayılan Blob kapsayıcısını birden çok HDInsight kümesiyle paylaşmayın. Bu eylem, iş geçmişini bozmayabilir. Her küme için farklı bir kapsayıcı kullanmanız önerilir. Paylaşılan verileri, varsayılan depolama hesabı yerine tüm ilgili kümeler için belirtilen bağlantılı bir depolama hesabına yerleştirin. Bağlantılı Depolama hesaplarını yapılandırma hakkında daha fazla bilgi için bkz: [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md). Ancak özgün HDInsight kümesi silindikten sonra varsayılan depolama kapsayıcısını yeniden kullanabilirsiniz. HBase kümeleri için, silinmiş bir HBase kümesi tarafından kullanılan varsayılan blob kapsayıcısını kullanarak yeni bir HBase kümesi oluşturarak HBase tablo şemasını ve verilerini koruyabilirsiniz

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

## <a name="use-additional-storage-accounts"></a>Ek depolama hesaplarını kullanma

HDInsight kümesi oluştururken ilişkilendirmek istediğiniz Azure Depolama hesabını belirtirsiniz. Ayrıca, oluşturma işlemi sırasında veya bir küme oluşturulduktan sonra aynı Azure aboneliğinden veya farklı Azure aboneliklerinden ek depolama hesapları ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede HDFS ile uyumlu Azure Depolama'yı HDInsight ile nasıl kullanacağınızı öğrendiniz. Bu depolama, uyarlamalı tablo oluşturmanıza, uzun süreli, veri alma çözümlerini arşivlemeyi ve depolanan yapılandırılmış ve yapılandırılmamış veriler içindeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure HDInsight 'ı kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure Data Lake Storage kullanmaya başlayın](../data-lake-store/data-lake-store-get-started-portal.md)
* [HDInsight'a veri yükleme](hdinsight-upload-data.md)
* [HDInsight ile verilere erişimi kısıtlamak için Azure Depolama Paylaşılan Erişim İmzaları kullanma](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Azure HDInsight kümeleriyle Azure Data Lake Storage 2. Nesil hizmetini kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)
* [Öğretici: Azure HDInsight 'ta etkileşimli sorgu kullanarak verileri ayıklama, dönüştürme ve yükleme](./interactive-query/interactive-query-tutorial-analyze-flight-data.md)
