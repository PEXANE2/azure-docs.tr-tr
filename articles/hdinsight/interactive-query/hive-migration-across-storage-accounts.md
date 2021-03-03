---
title: Azure Storage 'da yeni hesaba Hive iş yükü geçişi
description: Azure Storage 'da yeni hesaba Hive iş yükü geçişi
author: kevxmsft
ms.author: kevx
ms.reviewer: ''
ms.service: hdinsight
ms.topic: how-to
ms.date: 12/11/2020
ms.openlocfilehash: 0d62bebddb7751c168ba2e487b2391a40bbc6e67
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101747224"
---
# <a name="hive-workload-migration-to-new-account-in-azure-storage"></a>Azure Storage 'da yeni hesaba Hive iş yükü geçişi

HDInsight 'ta depolama hesapları arasında Hive tablolarını kopyalamak için betik eylemlerinin nasıl kullanılacağını öğrenin. Bu, öğesine geçiş yaparken yararlı olabilir [`Azure Data Lake Storage Gen2`](../hdinsight-hadoop-use-data-lake-storage-gen2.md) .

HDInsight 4,0 ' de tek bir Hive tablosunu el ile kopyalamak için bkz. [Hive dışarı/içeri aktarma](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ImportExport).

## <a name="prerequisites"></a>Önkoşullar

* Aşağıdaki yapılandırmalara sahip yeni bir HDInsight kümesi:
  * Varsayılan dosya sistemi, hedef depolama hesabıdır. Bkz. Azure [HDInsight kümeleri Ile Azure depolama kullanma](../hdinsight-hadoop-use-blob-storage.md).
  * Küme sürümü, kaynak kümenin ile eşleşmelidir.
  * Yeni bir dış Hive meta veri deposu DB kullanır. Bkz. [dış meta veri depoları kullanma](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation).
* Hem özgün hem de yeni kümeler tarafından erişilebilen bir depolama hesabı. İzin verilen ikincil depolama türleri için HDInsight ve [Depolama türleri ve özelliklerine](../hdinsight-hadoop-compare-storage-options.md#storage-types-and-features) [ek depolama hesapları ekleme](../hdinsight-hadoop-add-storage.md) bölümüne bakın.

    Bazı seçenekler şunlardır:
  * Hedef depolama hesabını özgün kümeye ekleyin.
  * Özgün depolama hesabını yeni kümeye ekleyin.
  * Hem özgün hem de yeni kümelere bir ara depolama hesabı ekleyin.

## <a name="how-it-works"></a>Nasıl çalışır?

Hive tablolarını orijinal kümeden belirtilen bir IBU dizine aktarmak için bir betik eylemi çalıştıracağız. [Çalışan bir küme Için betik eylemi](../hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster)' ne bakın.

Ardından, yeni kümede Hive tablolarını, IBU dizinden içeri aktarmak için başka bir betik eylemi çalıştıracağız.

Komut dosyası, yeni kümenin varsayılan dosya sistemiyle tabloları yeniden oluşturur. Yerel tablolar da verileri depolama alanına kopyalayacaktır. Yerel olmayan tablolar yalnızca tanımına göre kopyalanacak. Yerel olmayan tablolar hakkında ayrıntılar için bkz. [Hive depolama işleyicileri](https://cwiki.apache.org/confluence/display/Hive/StorageHandlers) .

Hive ambar dizininde bulunmayan dış tabloların yolu korunacaktır. Diğer tablolar hedef kümenin varsayılan Hive yoluna kopyalayacaktır. Bkz. Hive özellikleri `hive.metastore.warehouse.external.dir` ve `hive.metastore.warehouse.dir` .

Betikler, hedef kümede özel dosya izinlerini korumayacaktır.

> [!NOTE]
>
> Bu kılavuz, Hive veritabanları, tabloları ve bölümleri ile ilgili meta veri nesnelerinin kopyalanmasını destekler. Diğer meta veri nesnelerinin el ile yeniden oluşturulması gerekir.
>
> * İçin `Views` Hive, `SHOW VIEWS` HDInsight 4,0 üzerinde Hive 2.2.0 olarak komutunu destekler. `SHOW CREATE TABLE`Görünüm tanımı için kullanın. Hive 'nin önceki sürümlerinde, görünümleri göstermek için meta veri deposu SQL DB 'yi sorgulayın.
> * İçin, `Materialized Views` ve komutlarını `SHOW MATERIALIZED VIEWS` kullanın `DESCRIBE FORMATTED` `CREATE MATERIALIZED VIEW` . Ayrıntılar için bkz. [gerçekleştirilmiş görünümler](https://cwiki.apache.org/confluence/display/Hive/Materialized+views) .
> * İçin `Constraints` , hdınsight 4,0 üzerinde Hive 2.1.0 itibariyle desteklenir, `DESCRIBE EXTENDED` bir tabloya yönelik kısıtlamaları listelemek için kullanın ve `ALTER TABLE` kısıtlama eklemek için kullanın. Ayrıntılar için bkz. [alter table kısıtlamaları](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-AlterTableConstraints) .

## <a name="copy-hive-tables"></a>Hive tablolarını Kopyala

1. Özgün kümede "dışarı aktar" betik eylemini aşağıdaki alanlarla uygulayın.

    Bu işlem, ara Hive betikleri oluşturur ve yürütür. Bunlar, belirtilen öğesine kaydedilir `<hdfs-export-path>` .

    İsteğe bağlı olarak, `--run-script=false` bunları el ile yürütmeden önce özelleştirmek için kullanın.

    |Özellik | Değer |
    |---|---|
    |Bash betiği URI 'SI|`https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/export-hive-data-v01.sh`|
    |Düğüm türleri|Head|
    |Parametreler|`<hdfs-export-path>` `--run-script`|

    ```sh
    usage: generate Hive export and import scripts and export Hive data to specified HDFS path
           [--run-script={true,false}]
           hdfs-export-path

    positional arguments:

        hdfs-export-path      remote HDFS directory to write export data to

    optional arguments:
        --run-script={true,false}
                            whether to execute the generated Hive export script
                            (default: true)
    ```

2. Dışarı aktarma işleminin başarıyla tamamlanmasından sonra, yeni kümedeki "içeri aktar" betik eylemini aşağıdaki alanlarla uygulayın.

    |Özellik | Değer |
    |---|---|
    |Bash betiği URI 'SI|`https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/import-hive-data-v01.sh`|
    |Düğüm türleri|Head|
    |Parametreler|`<hdfs-export-path>`|

    ```sh
    usage: download Hive import script from specified HDFS path and execute it
           hdfs-export-path

    positional arguments:

      hdfs-export-path      remote HDFS directory to download Hive import script from

    ```

## <a name="verification"></a>Doğrulama

Betiği her kümenin birincil düğümünde kök kullanıcı olarak indirip çalıştırın [`hive_contents.sh`](https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/hive_contents.sh) ve çıkış dosyasının içeriğini karşılaştırın `/tmp/hive_contents.out` . Bkz. [SSH kullanarak HDInsight 'A bağlanma (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="cleanup-additional-storage-usage"></a>Ek depolama kullanımını temizle

Depolama geçişi tamamlandıktan ve doğrulandıktan sonra, belirtilen bir. dışarı aktarma yolundaki verileri silebilirsiniz.

Seçeneği,,,, bu komutu kullanma `hdfs dfs -rm -R` .

## <a name="option-reduce-additional-storage-usage"></a>Seçenek: ek depolama kullanımını azaltma

Betiği dışarı aktar eylemi, Hive nedeniyle depolama kullanımını büyük olasılıkla iki katına çıkarır. Ancak, aynı anda el ile, bir veritabanı veya tablo geçirerek ek depolama kullanımını sınırlamak mümkündür.

1. `--run-script=false`Oluşturulan Hive betiğinin yürütülmesini atlamak için belirtin. Hive dışa aktarma ve içeri aktarma betikleri yine de dışa aktarma yoluna kaydedilir.

2. Her geçirilmiş veritabanı veya tablodan sonra dışarı aktarma yolunu el ile silerek, Hive 'nin kod parçacıklarını ve dışarı aktarma betiklerini yürütün.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Storage 2. Nesil](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
* [Dış meta veri depolarını kullanma](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation)
* [Depolama türleri ve özellikleri](../hdinsight-hadoop-compare-storage-options.md#storage-types-and-features)
* [Çalışan bir kümeye betik eylemi](../hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster)
