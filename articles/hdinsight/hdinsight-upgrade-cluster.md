---
title: HDInsight kümesini daha yeni bir sürüme yükseltme-Azure
description: Azure HDInsight kümenizi daha yeni bir sürüme yükseltmeye yönelik yönergeleri öğrenin.
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: 1a1d4a71786ebb1e68f59084086b3256a1c1ea40
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951165"
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>HDInsight kümesini daha yeni bir sürüme yükseltme

En son HDInsight özelliklerinden yararlanmak için HDInsight kümelerinin en son sürüme yükseltilmesi önerilir. HDInsight küme sürümlerinizi yükseltmek için aşağıdaki yönergeleri izleyin.

> [!NOTE]  
> HDInsight 'ın desteklenen sürümleri hakkında bilgi için bkz. [HDInsight bileşen sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions).

## <a name="upgrade-tasks"></a>Yükseltme görevleri

HDInsight kümesini yükseltmek için iş akışı aşağıdaki gibidir.
HDInsight yükseltme iş akışı diyagramını ![](./media/hdinsight-upgrade-cluster/upgrade-workflow-diagram.png)

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

## <a name="next-steps"></a>Sonraki Adımlar

* [Linux tabanlı HDInsight kümeleri oluşturmayı öğrenin](hdinsight-hadoop-provision-linux-clusters.md)
* [SSH kullanarak HDInsight’a bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Apache ambarı 'nı kullanarak Linux tabanlı bir kümeyi yönetme](hdinsight-hadoop-manage-ambari.md)
* [HDInsight sürüm notları](./hdinsight-version-release.md)
