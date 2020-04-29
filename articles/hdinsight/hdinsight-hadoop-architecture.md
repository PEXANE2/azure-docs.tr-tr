---
title: Apache Hadoop mimarisi-Azure HDInsight
description: Azure HDInsight kümelerinde Apache Hadoop depolamayı ve işlemeyi açıklar.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/07/2020
ms.openlocfilehash: 3feacd94558ba275c81469827993aef106ae633c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77162217"
---
# <a name="apache-hadoop-architecture-in-hdinsight"></a>HDInsight’ta Apache Hadoop mimarisi

[Apache Hadoop](https://hadoop.apache.org/) iki çekirdek bileşeni içerir: depolama sağlayan [Apache Hadoop Dağıtılmış dosya sistemi (bir)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) Apache Hadoop ve Işleme sağlayan [başka bir kaynak Negotiator (Yarn)](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) . Depolama ve işleme özellikleri sayesinde bir küme, [MapReduce](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html) programlarını, istenen veri işlemeyi gerçekleştirecek şekilde çalıştırabilme özelliğine sahip olur.

> [!NOTE]  
> Bir bir bir bir bir bir bir bir bir bir bir şekilde, depolama sağlamak için HDInsight kümesinde Bunun yerine, bir,, Hadoop bileşenleri tarafından bir Gerçek depolama özelliği, Azure depolama veya Azure Data Lake Storage tarafından sağlanır. Hadoop için, HDInsight kümesinde yürütülen MapReduce işleri, bir bir bir bir bir bir bir bir bir bir bir bir bir bir bir bir bir bir bir bir bir bir bir bir HDInsight 'ta Hadoop 'da, depolama alanı için kullanılır, ancak YARN işleme bir çekirdek bileşeni kalır. Daha fazla bilgi için bkz. [Azure HDInsight 'A giriş](hadoop/apache-hadoop-introduction.md).

Bu makalede YARN ve HDInsight 'ta uygulamaların yürütülmesini nasıl koordine eden açıklanır.

## <a name="apache-hadoop-yarn-basics"></a>Apache Hadoop YARN temelleri

YARN, Hadoop 'ta veri işlemeyi yönetir ve düzenler. YARN, kümedeki düğümlerde işlem olarak çalışan iki çekirdek hizmete sahiptir:

* ResourceManager
* NodeManager

ResourceManager, MapReduce işleri gibi uygulamalara küme işlem kaynakları verir. ResourceManager, bu kaynakları her kapsayıcının CPU çekirdekleri ve RAM belleği ayırmadan oluşan kapsayıcılar olarak verir. Bir kümede bulunan tüm kaynakları birleştirilemez ve sonra çekirdekleri ve belleği dağıtırsanız, her kaynak bloğu bir kapsayıcıdır. Kümedeki her düğüm belirli sayıda kapsayıcı için kapasiteye sahiptir, bu nedenle kümenin kullanılabilir kapsayıcı sayısı üzerinde sabit bir sınırı vardır. Bir kapsayıcıdaki kaynak servis birimi yapılandırılabilir.

Bir MapReduce uygulaması bir kümede çalıştığında, ResourceManager uygulamayı yürütülecek kapsayıcıları sağlar. ResourceManager, çalışan uygulamaların durumunu, kullanılabilir küme kapasitesini izler ve kaynakları tamamlayıp serbest bıraktıkları şekilde izler.

ResourceManager Ayrıca uygulamaların durumunu izlemek için bir Web Kullanıcı arabirimi sağlayan bir Web sunucusu işlemi çalıştırır.

Bir kullanıcı küme üzerinde çalışacak bir MapReduce uygulaması gönderdiğinde, uygulama ResourceManager 'ya gönderilir. Sırasıyla, ResourceManager, kullanılabilir NodeManager düğümlerinde bir kapsayıcı ayırır. NodeManager düğümleri uygulamanın aslında çalıştırıldığı yerdir. Ayrılan ilk kapsayıcı, ApplicationMaster adlı özel bir uygulama çalıştırır. Bu ApplicationMaster, gönderilen uygulamayı çalıştırmak için gereken sonraki kapsayıcılar biçiminde kaynakları edinmekten sorumludur. ApplicationMaster, eşleme aşaması ve aşamayı azaltma gibi uygulama aşamalarını ve ne kadar veri işlenmesi gerektiğine ilişkin faktörleri inceler. Daha sonra ApplicationMaster, uygulama adına kaynak üzerinde bulunan kaynakları (*anlaşma*) ister. Ayrıca ResourceManager, kümedeki NodeManager 'dan uygulamayı yürütürken kullanması için kaynak yöneticilerden kaynak sağlar.

Nodeyöneticileri, uygulamayı oluşturan görevleri çalıştırır ve sonra ilerleme durumunu ve durumlarını ApplicationMaster 'a bildirir. İçindeki ApplicationMaster, uygulamanın durumunu ResourceManager 'ya geri bildirir. ResourceManager, istemciye herhangi bir sonuç döndürür.

## <a name="yarn-on-hdinsight"></a>HDInsight üzerinde YARN

Tüm HDInsight küme türleri YARN 'yi dağıtır. ResourceManager, birincil ve ikincil örnekle, sırasıyla küme içindeki birinci ve ikinci baş düğümlerde çalışan yüksek kullanılabilirlik için dağıtılır. Tek seferde yalnızca bir ResourceManager örneği etkindir. NodeManager örnekleri, kümedeki kullanılabilir çalışan düğümleri arasında çalışır.

![Azure HDInsight 'ta Apache YARN](./media/hdinsight-hadoop-architecture/apache-yarn-on-hdinsight.png)

## <a name="soft-delete"></a>Geçici silme

Depolama hesabınızdan bir dosyayı geri almak için, bkz.:

### <a name="azure-storage"></a>Azure Storage

* [Azure Depolama blobları için geçici silme](../storage/blobs/storage-blob-soft-delete.md)
* [Blobu geri al](https://docs.microsoft.com/rest/api/storageservices/undelete-blob)

### <a name="azure-data-lake-storage-gen-1"></a>Azure Data Lake Storage Gen 1

[Restore-AzDataLakeStoreDeletedItem](https://docs.microsoft.com/powershell/module/az.datalakestore/restore-azdatalakestoredeleteditem)

### <a name="azure-data-lake-storage-gen-2"></a>Azure Data Lake Storage 2. Nesil

[Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](../storage/blobs/data-lake-storage-known-issues.md)

## <a name="trash-purging"></a>Çöp Temizleme

`fs.trash.interval` **HDFS** `0` Yerel dosya sisteminde herhangi bir veri depolamamanız gerektiğinden,,,,**Advanced core-site**  >  Bu değer, uzak depolama hesaplarını etkilemez (EB, ADLS GEN1, ABFS)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Hadoop ile MapReduce'u kullanma](hadoop/hdinsight-use-mapreduce.md)
* [Azure HDInsight 'a giriş](hadoop/apache-hadoop-introduction.md)
