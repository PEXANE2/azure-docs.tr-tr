---
title: HDFS uyumlu Azure Depolama'da veri sorgulama - Azure HDInsight
description: Analizsonuçlarını depolamak için Azure depolama ve Azure Veri Gölü Depolama'dan verileri nasıl sorgulayınız öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: cd6ba50cf81b93da887134e89d75313acb6bd936
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869851"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile Azure Depolama'yı kullanma

Verileri [Azure Depolama,](../storage/common/storage-introduction.md) [Azure Veri Gölü Depolama Gen 1](../data-lake-store/data-lake-store-overview.md)veya Azure Veri Gölü Depolama Gen [2'de](../storage/blobs/data-lake-storage-introduction.md)depolayabilirsiniz. Ya da bu seçeneklerin bir kombinasyonu. Bu depolama seçenekleri, kullanıcı verilerini kaybetmeden hesaplama için kullanılan HDInsight kümelerini güvenli bir şekilde silmenizi sağlar.

Apache Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight küme oluşturma işlemi sırasında, Varsayılan dosya sistemi olarak Azure Depolama'da bir blob kapsayıcısı belirtebilirsiniz. Veya HDInsight 3.6 ile Azure Depolama veya Azure Veri Gölü Depolama Gen 1/ Azure Veri Gölü Depolama Gen 2'yi birkaç istisna dışında varsayılan dosya sistemi olarak seçebilirsiniz. Veri Gölü Depolama Gen 1'i hem varsayılan hem de bağlantılı depolama alanı olarak kullanmanın desteklenebilirliği [için HDInsight kümesi için Kullanılabilirlik bölümüne](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters)bakın.

Bu makalede Azure Depolama'nın HDInsight kümeleri ile nasıl çalıştığı hakkında bilgi edinebilirsiniz. Data Lake Storage Gen 1'in HDInsight kümeleriyle nasıl çalıştığını öğrenmek için Azure [HDInsight kümeleriyle Azure Veri Gölü Depolamasını Kullan'a](hdinsight-hadoop-use-data-lake-store.md)bakın. HDInsight kümesi oluşturma hakkında daha fazla bilgi için, [HDInsight'ta Apache Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)bilgisine bakın.

> [!IMPORTANT]  
> Depolama hesabı türü **BlobStorage** yalnızca HDInsight kümeleri için ikincil depolama alanı olarak kullanılabilir.

| Depolama hesabı türü | Desteklenen hizmetler | Desteklenen performans katmanları |Desteklenmeyen performans katmanları| Desteklenen erişim katmanları |
|----------------------|--------------------|-----------------------------|---|------------------------|
| StorageV2 (genel amaçlı v2)  | Blob     | Standart                    |Premium| Sıcak, Serin, Arşiv\*   |
| Depolama (genel amaçlı v1)   | Blob     | Standart                    |Premium| Yok                    |
| BlobDepolama                    | Blob     | Standart                    |Premium| Sıcak, Serin, Arşiv\*   |

İş verilerini depolamak için varsayılan blob kapsayıcısını kullanmanızı önermiyoruz. Depolama maliyetini azaltmak için blob kapsayıcısının her kullanımdan sonra silinmesi iyi bir uygulamadır. Varsayılan kapsayıcı uygulama ve sistem günlükleri içerir. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Birden çok küme için varsayılan dosya sistemi desteklenmez olarak bir blob kapsayıcı paylaşımı.

> [!NOTE]  
> Arşiv erişim katmanı, birkaç saatlik alma gecikmesi olan ve HDInsight ile kullanılması önerilmez çevrimdışı bir katmandır. Daha fazla bilgi için [Arşiv erişim katmanına](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier)bakın.

## <a name="access-files-from-within-cluster"></a>Küme içinden dosyalara eriş

Veri Gölü Depolama'daki dosyalara hdinsight kümesinden erişmenin birkaç yolu vardır. URI şeması şifrelenmemiş erişim *(wasb* ile: önek) ve TLS şifreli erişim *(wasbs*ile) sağlar. Azure’da aynı bölgede bulunan verilere erişirken dahi mümkün olduğunda *wasbs* kullanmanızı öneririz.

* **Tam adı kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın tam yolunu girersiniz.

    ```
    wasb://<containername>@<accountname>.blob.core.windows.net/<file.path>/
    wasbs://<containername>@<accountname>.blob.core.windows.net/<file.path>/
    ```

* **Kısaltılmış yol biçimi kullanarak**. Bu yaklaşımla, küme köküne giden yolu aşağıdakilerle değiştirirsiniz:

    ```
    wasb:///<file.path>/
    wasbs:///<file.path>/
    ```

* **Göreli yolu kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın yalnızca göreli yolunu girersiniz.

    ```
    /<file.path>/
    ```

### <a name="data-access-examples"></a>Veri erişim örnekleri

Örnekler kümenin baş düğümüne [bir ssh bağlantısına](./hdinsight-hadoop-linux-use-ssh-unix.md) dayanır. Örnekler de üç URI düzenini kullanır. Değiştir `CONTAINERNAME` `STORAGEACCOUNT` ve ilgili değerlerle

#### <a name="a-few-hdfs-commands"></a>Birkaç hdfs komutları

1. Yerel depolama alanı yla ilgili bir dosya oluşturun.

    ```bash
    touch testFile.txt
    ```

1. Küme depolama dizinleri oluşturun.

    ```bash
    hdfs dfs -mkdir wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -mkdir wasbs:///sampledata2/
    hdfs dfs -mkdir /sampledata3/
    ```

1. Verileri yerel depolamadan küme depolamasına kopyalayın.

    ```bash
    hdfs dfs -copyFromLocal testFile.txt  wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -copyFromLocal testFile.txt  wasbs:///sampledata2/
    hdfs dfs -copyFromLocal testFile.txt  /sampledata3/
    ```

1. Küme depolama da dizin içeriğini liste.

    ```bash
    hdfs dfs -ls wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -ls wasbs:///sampledata2/
    hdfs dfs -ls /sampledata3/
    ```

> [!NOTE]  
> HDInsight dışındaki blob'larla çalışırken, yardımcı programların çoğu WASB biçimini tanımaz ve bunun yerine, `example/jars/hadoop-mapreduce-examples.jar` gibi temel yol biçimi gibi bekler.

#### <a name="creating-a-hive-table"></a>Kovan tablosu oluşturma

Açıklayıcı amaçlarla üç dosya konumu gösterilir. Gerçek yürütme için `LOCATION` girişlerden yalnızca birini kullanın.

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

## <a name="access-files-from-outside-cluster"></a>Dış kümeden dosyalara eriş

Microsoft, Azure Depolama ile çalışmak için aşağıdaki araçları sağlar:

| Araç | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure CLI](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [Azcopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |

## <a name="identify-storage-path-from-ambari"></a>Ambari'den depolama yolunu belirleme

* Yapılandırılan varsayılan depoya giden tam yolu tanımlamak için şu şekilde gidin:

    **HDFS** > **Configs** `fs.defaultFS` ve filtre giriş kutusuna girin.

* Wasb deposunun ikincil depolama alanı olarak yapılandırıp yapılandırmayıp yapılmadı sını kontrol etmek için şu

    **HDFS** > **Configs** `blob.core.windows.net` ve filtre giriş kutusuna girin.

Ambari REST API'yi kullanarak yolu elde etmek için [bkz.](./hdinsight-hadoop-manage-ambari-rest-api.md#get-the-default-storage)

## <a name="blob-containers"></a>Blob kaplar

Blob'ları kullanmak için önce bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md) oluşturursunuz. Bu adımın bir parçası olarak, depolama hesabının oluşturulduğu bir Azure bölgesi belirtirsiniz. Küme ve depolama hesabının aynı bölgede barındırılması gerekir. Hive metastore SQL Server veritabanı ve Apache Oozie metastore SQL Server veritabanı aynı bölgede bulunmalıdır.

Nerede olursa olsun, oluşturduğunuz her blob Azure Storage hesabınızdaki bir kapsayıcıya aittir. Bu kapsayıcı HDInsight dışında oluşturulan mevcut bir leke olabilir. Veya bir HDInsight kümesi için oluşturulan bir kapsayıcı olabilir.

Varsayılan Blob kapsayıcısı iş geçmişi ve iş günlükleri gibi kümeye özel bilgileri depolar. Varsayılan Blob kapsayıcısını birden çok HDInsight kümesiyle paylaşmayın. Bu eylem iş geçmişini bozabilir. Her küme için farklı bir kapsayıcı kullanılması önerilir. Paylaşılan verileri varsayılan depolama hesabı yerine ilgili tüm kümeler için belirtilen bağlantılı bir depolama hesabına koyun. Bağlantılı Depolama hesaplarını yapılandırma hakkında daha fazla bilgi için bkz: [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md). Ancak özgün HDInsight kümesi silindikten sonra varsayılan depolama kapsayıcısını yeniden kullanabilirsiniz. HBase kümeleri için, silinmiş bir HBase kümesi tarafından kullanılan varsayılan blob kapsayıcısını kullanarak yeni bir HBase kümesi oluşturarak HBase tablo şemasını ve verilerini tutabilirsiniz

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

## <a name="use-additional-storage-accounts"></a>Ek depolama hesaplarını kullanma

HDInsight kümesi oluştururken ilişkilendirmek istediğiniz Azure Depolama hesabını belirtirsiniz. Ayrıca, oluşturma işlemi sırasında aynı Azure aboneliğinden veya farklı Azure aboneliklerinden ek depolama hesapları ekleyebilirsiniz. Veya bir küme oluşturulduktan sonra. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede HDFS ile uyumlu Azure Depolama'yı HDInsight ile nasıl kullanacağınızı öğrendiniz. Bu depolama, uyarlanabilir, uzun vadeli, arşivleme veri toplama çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış verilerdeki bilgilerin kilidini açmak için HDInsight'ı kullanmanıza olanak tanır.

Daha fazla bilgi için bkz.

* [Azure HDInsight ile başlayın](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure Veri Gölü Depolama'ya başlayın](../data-lake-store/data-lake-store-get-started-portal.md)
* [HDInsight'a veri yükleme](hdinsight-upload-data.md)
* [HDInsight ile verilere erişimi kısıtlamak için Azure Depolama Paylaşılan Erişim İmzaları kullanma](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)
* [Öğretici: Azure HDInsight'ta Etkileşimli Sorgu'u kullanarak verileri ayıklayın, dönüştürün ve yükleyin](./interactive-query/interactive-query-tutorial-analyze-flight-data.md)
