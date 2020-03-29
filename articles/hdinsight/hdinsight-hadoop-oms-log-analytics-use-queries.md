---
title: Azure HDInsight kümelerini izlemek için Azure Monitor günlüklerini sorgula
description: HDInsight kümesinde çalışan işleri izlemek için Azure Monitor günlüklerinde sorguları nasıl çalıştırılacizleyeceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/02/2019
ms.openlocfilehash: 65e85548420116bdfcab87fe9f81a20e66226beb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74803846"
---
# <a name="query-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>HDInsight kümelerini izlemek için Azure Monitor günlüklerini sorgula

Azure HDInsight kümelerini izlemek için Azure Monitor günlüklerinin nasıl kullanılacağına ilişkin bazı temel senaryoları öğrenin:

* [HDInsight küme ölçümlerini analiz edin](#analyze-hdinsight-cluster-metrics)
* [Olay uyarıları oluşturma](#create-alerts-for-tracking-events)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure Monitor günlüklerini kullanmak için bir HDInsight kümesini yapılandırmış ve çalışma alanına HDInsight kümeye özgü Azure Monitor günlükleri izleme çözümlerini eklemiş olmalısınız. Talimatlar için bkz. [HDInsight kümeleriyle Azure Monitörü günlüklerini kullan.](hdinsight-hadoop-oms-log-analytics-tutorial.md)

## <a name="analyze-hdinsight-cluster-metrics"></a>HDInsight küme ölçümlerini analiz edin

HDInsight kümeniz için belirli ölçümleri nasıl arayacağınızı öğrenin.

1. Azure portalından HDInsight kümenizle ilişkili Günlük Analizi çalışma alanını açın.
1. **Genel**altında, **Günlükleri**seçin.
1. Azure Monitor günlüklerini kullanacak şekilde yapılandırılan tüm HDInsight kümeleri için tüm kullanılabilir ölçümleri aramak için arama kutusuna aşağıdaki sorguyu yazın ve ardından **Çalıştır'ı**seçin. Sonuçları gözden geçirin.

    ```kusto
    search *
    ```

    ![Apache Ambari analitiği tüm ölçümleri ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "Tüm ölçümleri ara")

1. Sol menüden **Filtre** sekmesini seçin.

1. **Türü**altında, **Kalp Atışı'nı**seçin. Ardından **Çalıştır'& Uygula'yı**seçin.

    ![günlük analitik arama belirli ölçümler](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "Belirli ölçümleri arama")

1. Metin kutusundaki sorgunun aşağıdakilere göre değiştiğine dikkat edin:

    ```kusto
    search *
    | where Type == "Heartbeat"
    ```

1. Sol menüdeki seçenekleri kullanarak daha derine inebilirsiniz. Örnek:

    - Belirli bir düğümdeki günlükleri görmek için:

        ![Belirli hatalar için arama çıktısı1](./media/hdinsight-hadoop-oms-log-analytics-use-queries/log-analytics-specific-node.png "Belirli hatalar için arama çıktısı1")

    - Belirli zamanlarda günlükleri görmek için:

        ![Belirli hatalar çıktısı için arama2](./media/hdinsight-hadoop-oms-log-analytics-use-queries/log-analytics-specific-time.png "Belirli hatalar çıktısı için arama2")

1. **Çalıştır'& uygula'yı** seçin ve sonuçları gözden geçirin. Ayrıca, sorgunun şu şekilde güncelleştirilene de dikkat edin:

    ```kusto
    search *
    | where Type == "Heartbeat"
    | where (Computer == "zk2-myhado") and (TimeGenerated == "2019-12-02T23:15:02.69Z" or TimeGenerated == "2019-12-02T23:15:08.07Z" or TimeGenerated == "2019-12-02T21:09:34.787Z")
    ```

### <a name="additional-sample-queries"></a>Ek örnek sorgular

Küme adına göre sınıflandırılan, 10 dakikalık bir aralıkta kullanılan kaynakların ortalamasını temel alan örnek bir sorgu:

```kusto
search in (metrics_resourcemanager_queue_root_default_CL) * 
| summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)
```

Kullanılan kaynakların ortalamasını temel alan rafine etmek yerine, 10 dakikalık bir süre içinde maksimum kaynakların ne zaman kullanıldığına (yanı sıra 90 ve 95. yüzdelik dilimi) göre sonuçları hassaslaştırmak için aşağıdaki sorguyu kullanabilirsiniz:

```kusto
search in (metrics_resourcemanager_queue_root_default_CL) * 
| summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)
```

## <a name="create-alerts-for-tracking-events"></a>Olayları izlemek için uyarılar oluşturma

Bir uyarı oluşturmak için ilk adım, uyarının tetiklendiği bir sorguya ulaşmaktır. Uyarı oluşturmak istediğiniz herhangi bir sorguyı kullanabilirsiniz.

1. Azure portalından HDInsight kümenizle ilişkili Günlük Analizi çalışma alanını açın.
1. **Genel**altında, **Günlükleri**seçin.
1. Bir uyarı oluşturmak istediğiniz aşağıdaki sorguyu çalıştırın ve ardından **Çalıştır'ı**seçin.

    ```kusto
    metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0
    ```

    Sorgu, HDInsight kümelerinde çalışan başarısız uygulamaların listesini sağlar.

1. Sayfanın üst kısmında **Yeni uyarı kuralını** seçin.

    ![Bir uyarı oluşturmak için sorgu girin1](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Bir uyarı oluşturmak için sorgu girin1")

1. Oluştur **kuralı** penceresinde, bir uyarı oluşturmak için sorguyu ve diğer ayrıntıları girin ve ardından **uyarı kuralı oluştur'u**seçin.

    ![Bir uyarı oluşturmak için sorgu girin2](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Bir uyarı oluşturmak için sorgu girin2")

### <a name="edit-or-delete-an-existing-alert"></a>Varolan bir uyarıyı düzenle veya silme

1. Azure portalından Log Analytics çalışma alanını açın.

1. Sol menüden, **İzleme**altında **Uyarılar'ı**seçin.

1. En üste doğru, **uyarı kurallarını yönet'i**seçin.

1. Düzenleveya silmek istediğiniz uyarıyı seçin.

1. Aşağıdaki seçeneklere sahipsiniz: **Kaydet,** **At,** **Devre Dışı ve** **Sil**.

    ![HDInsight Azure Monitor uyarı silme düzenle günlükleri](media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png)

Daha fazla bilgi için bkz: [Azure Monitor kullanarak metrik uyarıları oluştur, görüntüle ve yönet.](../azure-monitor/platform/alerts-metric.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Monitor'da günlük sorgularına başlayın](../azure-monitor/log-query/get-started-queries.md)
* [Azure Monitör'de Görünüm Tasarımcısı'nı kullanarak özel görünümler oluşturun](../azure-monitor/platform/view-designer.md)
