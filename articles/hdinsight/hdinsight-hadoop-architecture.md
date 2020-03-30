---
title: Apache Hadoop mimarisi - Azure HDInsight
description: Azure HDInsight kümelerinde Apache Hadoop depolama ve işleme yi açıklar.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/07/2020
ms.openlocfilehash: 3feacd94558ba275c81469827993aef106ae633c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162217"
---
# <a name="apache-hadoop-architecture-in-hdinsight"></a>HDInsight’ta Apache Hadoop mimarisi

[Apache Hadoop](https://hadoop.apache.org/) iki temel bileşeniçerir: depolama sağlayan [Apache Hadoop Dağıtılmış Dosya Sistemi (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) ve işleme sağlayan [Apache Hadoop Yet Another Resource Negotiator (YARN).](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) Depolama ve işleme yetenekleri yle, bir küme istenilen veri işlemeyi gerçekleştirmek için [MapReduce](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html) programlarını çalıştırabilme yeteneğine sahip olur.

> [!NOTE]  
> HdFS genellikle HDInsight kümesi içinde depolama sağlamak için dağıtılanmaz. Bunun yerine, Hadoop bileşenleri tarafından HDFS uyumlu bir arabirim katmanı kullanılır. Gerçek depolama özelliği Azure Depolama veya Azure Veri Gölü Depolama tarafından sağlanır. Hadoop için MapReduce, HDInsight kümesinde çalışan işleri hdfs varmış gibi çalıştırın ve bu nedenle depolama gereksinimlerini desteklemek için değişiklik gerektirmez. HDInsight'taki Hadoop'ta depolama dış kaynaklıdır, ancak İPLik işleme temel bileşen olmaya devam etmektedir. Daha fazla bilgi için azure [HDInsight'a Giriş 'e](hadoop/apache-hadoop-introduction.md)bakın.

Bu makalede, IPN ve nasıl HDInsight uygulamaların yürütülmesini koordine tanıttı.

## <a name="apache-hadoop-yarn-basics"></a>Apache Hadoop İplik temelleri

HADOOP'ta veri işlemeyi İPLik yönetir ve yönetir. YARN kümedeki düğümler üzerinde işlem olarak çalışan iki temel hizmete sahiptir:

* Resourcemanager
* DüğümYöneticisi

ResourceManager, MapReduce işleri gibi uygulamalara küme hesaplama kaynakları verir. ResourceManager bu kaynakları kapsayıcı olarak verir ve her kapsayıcı CPU çekirdekleri ve RAM belleği tahsisinden oluşur. Bir kümede bulunan tüm kaynakları birleştirdiyseniz ve çekirdekleri ve belleği bloklar halinde dağıttıysanız, her kaynak bloğu bir kapsayıcıdır. Kümedeki her düğüm belirli sayıda kapsayıcı için kapasiteye sahiptir, bu nedenle kümenin kullanılabilir kapsayıcı sayısında sabit bir sınırı vardır. Bir kapsayıcıdaki kaynakların tahsisi yapılandırılabilir.

MapReduce uygulaması bir küme üzerinde çalıştığında, ResourceManager uygulamaya yürütülecek kapsayıcıları sağlar. ResourceManager çalışan uygulamaların durumunu, kullanılabilir küme kapasitesini izler ve uygulamaları tamamlayıp kaynaklarını serbest bırakarak izler.

ResourceManager ayrıca uygulamaların durumunu izlemek için bir web kullanıcı arabirimi sağlayan bir web sunucusu işlemi çalıştırın.

Bir kullanıcı kümeüzerinde çalışması için mapreduce uygulaması gönderdiğinde, uygulama ResourceManager'a gönderilir. Buna karşılık, Kaynak Yöneticisi kullanılabilir NodeManager düğümlerine bir kapsayıcı ayırır. Düğüm Yöneticisi düğümleri, uygulamanın gerçekte yürütüldüğü yerdir. Ayrılan ilk kapsayıcı ApplicationMaster adı verilen özel bir uygulama çalıştırılır. Bu ApplicationMaster, gönderilen başvuruyu çalıştırmak için gereken sonraki kapsayıcılar şeklinde kaynak elde etmekle yükümlüdür. ApplicationMaster, harita aşaması ve aşamayı azaltma gibi uygulamaaşamalarını ve ne kadar verinin işlenmesi gerektiğine ilişkin faktörleri inceler. ApplicationMaster daha sonra kaynak yöneticisinden uygulama adına kaynakları talep eder *(görüşür).* Kaynak Yöneticisi de kümedeki NodeManager'lardan uygulamayı yürütmede kullanması için ApplicationMaster'a kaynak sağlar.

NodeManagers, uygulamayı oluşturan görevleri çalıştırır, ardından ilerleme lerini ve durumlarını ApplicationMaster'a geri bildirir. ApplicationMaster sırayla uygulamanın durumunu Kaynak Yöneticisi'ne geri bildirir. Kaynak Yöneticisi tüm sonuçları istemciye döndürür.

## <a name="yarn-on-hdinsight"></a>HDInsight üzerinde İplik

Tüm HDInsight küme türleri IPLIK dağıtın. ResourceManager, küme içindeki birinci ve ikinci kafa düğümlerinde sırasıyla çalışan birincil ve ikincil bir örnekle yüksek kullanılabilirlik için dağıtılır. Kaynak Yöneticisi'nin tek örneği aynı anda etkindir. NodeManager örnekleri kümedeki kullanılabilir alt düğümler arasında çalışır.

![Azure HDInsight'ta Apache YARN](./media/hdinsight-hadoop-architecture/apache-yarn-on-hdinsight.png)

## <a name="soft-delete"></a>Yumuşak silme

Depolama Hesabınızdaki bir dosyayı silmek için bkz:

### <a name="azure-storage"></a>Azure Storage

* [Azure Depolama blobları için geçici silme](../storage/blobs/storage-blob-soft-delete.md)
* [Undelete Blob](https://docs.microsoft.com/rest/api/storageservices/undelete-blob)

### <a name="azure-data-lake-storage-gen-1"></a>Azure Veri Gölü Depolama Gen 1

[Geri Yükleme-AzDataLakeStoreDeletedItem](https://docs.microsoft.com/powershell/module/az.datalakestore/restore-azdatalakestoredeleteditem)

### <a name="azure-data-lake-storage-gen-2"></a>Azure Data Lake Storage 2. Nesil

[Azure Veri Gölü Depolama Gen2 ile bilinen sorunlar](../storage/blobs/data-lake-storage-known-issues.md)

## <a name="trash-purging"></a>Çöp temizleme

Yerel `fs.trash.interval` dosya sisteminde herhangi bir veri depolamamanız `0` gerektiğinden, **HDFS** > Gelişmiş çekirdek**sitesindeki** özellik varsayılan değerde kalmalıdır. Bu değer uzak depolama hesaplarını etkilemez (WASB, ADLS GEN1, ABFS)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Hadoop ile MapReduce'u kullanma](hadoop/hdinsight-use-mapreduce.md)
* [Azure HDInsight'a Giriş](hadoop/apache-hadoop-introduction.md)
