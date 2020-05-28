---
title: Kümeyi daha yeni bir sürüme geçir
titleSuffix: Azure HDInsight
description: Azure HDInsight kümenizi daha yeni bir sürüme geçirmeye yönelik yönergeleri öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/31/2020
ms.openlocfilehash: 79cd3ddd21af700fd36d0e565b9f5a1be1e56343
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84015140"
---
# <a name="migrate-hdinsight-cluster-to-a-newer-version"></a>HDInsight kümesini daha yeni bir sürüme geçir

En son HDInsight özelliklerinden yararlanmak için HDInsight kümelerinin düzenli olarak en son sürüme geçirilmesi önerilir. HDInsight, var olan bir kümenin daha yeni bir bileşen sürümüne yükseltildiği yerinde yükseltmeleri desteklemez. İstenen bileşen ve platform sürümü ile yeni bir küme oluşturmanız ve ardından yeni kümeyi kullanmak için uygulamalarınızı geçirmeniz gerekir. HDInsight küme sürümlerinizi geçirmek için aşağıdaki yönergeleri izleyin.

> [!NOTE]  
> HDInsight 'ın desteklenen sürümleri hakkında bilgi için bkz. [HDInsight bileşen sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions).

## <a name="migration-tasks"></a>Geçiş görevleri

HDInsight kümesini yükseltmek için iş akışı aşağıdaki gibidir.
![HDInsight yükseltme iş akışı diyagramı](./media/hdinsight-upgrade-cluster/upgrade-workflow-diagram.png)

1. HDInsight kümenizi yükseltirken gerekebilecek değişiklikleri anlamak için bu belgenin her bölümünü okuyun.
2. Test/kalite güvencesi ortamı olarak bir küme oluşturun. Küme oluşturma hakkında daha fazla bilgi için bkz. [Linux tabanlı HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md) hakkında bilgi edinin
3. Mevcut işleri, veri kaynaklarını ve havuzları yeni ortama kopyalayın.
4. İşlerinizin yeni kümede beklendiği gibi çalıştığından emin olmak için doğrulama testi gerçekleştirin.

Her şeyin beklendiği gibi çalıştığından emin olduktan sonra geçiş için kapalı kalma süresi zamanlayın. Bu kapalı kalma süresi boyunca aşağıdaki işlemleri yapın:

1. Küme düğümlerinde yerel olarak depolanan tüm geçici verileri yedekleyin. Örneğin, doğrudan bir baş düğümde depolanan verileriniz varsa.
1. [Mevcut kümeyi silin](./hdinsight-delete-cluster.md).
1. Önceki kümenin kullandığı varsayılan veri deposunu kullanarak en son (veya desteklenen) HDI sürümü ile aynı VNET alt ağında bir küme oluşturun. Bu, yeni kümenin mevcut üretim verilerinize karşı çalışmaya devam etmesine olanak tanır.
1. Yedeklediğiniz tüm geçici verileri içeri aktarın.
1. Yeni kümeyi kullanarak işleri başlatın/işleme devam edin.

## <a name="workload-specific-guidance"></a>İş yüküne özgü rehberlik

Aşağıdaki belgeler belirli iş yüklerini geçirme hakkında rehberlik sağlar:

* [HBase 'i geçir](./hbase/apache-hbase-migrate-new-version.md)
* [Kafka geçir](./kafka/migrate-versions.md)
* [Hive/etkileşimli sorguyu geçirme](./interactive-query/apache-hive-migrate-workloads.md)

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Veritabanı yedekleme ve geri yükleme hakkında daha fazla bilgi için bkz. [otomatik veritabanı yedeklemeleri kullanarak Azure SQL veritabanını kurtarma](../azure-sql/database/recovery-using-backups.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Linux tabanlı HDInsight kümeleri oluşturmayı öğrenin](hdinsight-hadoop-provision-linux-clusters.md)
* [SSH kullanarak HDInsight’a bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Apache ambarı 'nı kullanarak Linux tabanlı bir kümeyi yönetme](hdinsight-hadoop-manage-ambari.md)
* [HDInsight sürüm notları](./hdinsight-version-release.md)
