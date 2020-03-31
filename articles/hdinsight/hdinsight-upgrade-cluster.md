---
title: Kümeyi daha yeni bir sürüme geçir
titleSuffix: Azure HDInsight
description: Azure HDInsight kümenizi daha yeni bir sürüme geçirmek için yönergeler öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/31/2020
ms.openlocfilehash: f7198aeff5e9ef6d37e29c2336dc38e4eec0dda1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023982"
---
# <a name="migrate-hdinsight-cluster-to-a-newer-version"></a>HDInsight kümesini daha yeni bir sürüme geçirin

En son HDInsight özelliklerinden yararlanmak için HDInsight kümelerinin düzenli olarak en son sürüme geçirilmelerini öneririz. HDInsight, varolan bir kümenin daha yeni bir bileşen sürümüne yükseltildiği yerinde yükseltmeleri desteklemez. İstenilen bileşen ve platform sürümüyle yeni bir küme oluşturmanız ve ardından yeni kümeyi kullanmak için uygulamalarınızı geçirmeniz gerekir. HDInsight küme sürümlerinizi geçirmek için aşağıdaki yönergeleri izleyin.

> [!NOTE]  
> HDInsight'ın desteklenen sürümleri hakkında daha fazla bilgi için [HDInsight bileşen sürümlerine](hdinsight-component-versioning.md#supported-hdinsight-versions)bakın.

## <a name="migration-tasks"></a>Geçiş görevleri

HDInsight Cluster'ı yükseltmek için iş akışı aşağıdaki gibidir.
![HDInsight yükseltme iş akışı diyagramı](./media/hdinsight-upgrade-cluster/upgrade-workflow-diagram.png)

1. HDInsight kümenizi yükseltirken gerekli olabilecek değişiklikleri anlamak için bu belgenin her bölümünü okuyun.
2. Test/kalite güvence ortamı olarak bir küme oluşturun. Küme oluşturma hakkında daha fazla bilgi için [bkz.](hdinsight-hadoop-provision-linux-clusters.md)
3. Varolan işleri, veri kaynaklarını ve lavaboları yeni ortama kopyalayın.
4. İşlerinizin yeni kümede beklendiği gibi çalıştığından emin olmak için doğrulama sınamasını gerçekleştirin.

Her şeyin beklendiği gibi çalıştığını doğruladıktan sonra, geçiş için kapalı kalma süresini zamanlayın. Bu kapalı kalma süresi boyunca aşağıdaki eylemleri yapın:

1. Küme düğümlerinde yerel olarak depolanan geçici verileri yedekleyin. Örneğin, doğrudan bir baş düğümünde depolanan verileriniz varsa.
1. [Varolan kümeyi silin.](./hdinsight-delete-cluster.md)
1. Önceki kümenin kullandığı varsayılan veri deposunu kullanarak en son (veya desteklenen) HDI sürümüyle aynı VNET alt netinde bir küme oluşturun. Bu, yeni kümenin varolan üretim verilerinize karşı çalışmaya devam etmesini sağlar.
1. Yedeklediğiniz geçici verileri içe aktarın.
1. Yeni kümeyi kullanarak işleri başlatın/işleme devam edin.

## <a name="workload-specific-guidance"></a>İş yüküne özel kılavuz

Aşağıdaki belgeler, belirli iş yüklerinin nasıl geçirilen hakkında rehberlik sağlar:

* [Göç HBase](./hbase/apache-hbase-migrate-new-version.md)
* [Kafka'ya göç edin](./kafka/migrate-versions.md)
* [Geçiş Hive/İnteraktif Sorgu](./interactive-query/apache-hive-migrate-workloads.md)

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Veritabanı yedekleme ve geri yükleme hakkında daha fazla bilgi için, [otomatik veritabanı yedeklemelerini kullanarak bir Azure SQL veritabanını](../sql-database/sql-database-recovery-using-backups.md)kurtarın'a bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Linux tabanlı HDInsight kümelerini nasıl oluşturabilirsiniz öğrenin](hdinsight-hadoop-provision-linux-clusters.md)
* [SSH kullanarak HDInsight’a bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Apache Ambari'yi kullanarak Linux tabanlı bir küme yi yönetme](hdinsight-hadoop-manage-ambari.md)
* [HDInsight sürüm notları](./hdinsight-version-release.md)
