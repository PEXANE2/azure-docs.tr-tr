---
title: Azure HDInsight kümelerini izlemek için Azure Monitör günlüklerini kullanma
description: BIR HDInsight kümesinde çalışan işleri izlemek için Azure Monitor günlüklerini nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/06/2020
ms.openlocfilehash: e4b33e132e660fba7d06ff33c7db06c7727dd26c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162795"
---
# <a name="use-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Azure İzleyici günlüklerini kullanarak HDInsight kümelerini izleme

HDInsight'ta Hadoop küme işlemlerini izlemek için Azure Monitor günlüklerini nasıl etkinleştirizleyeceğinizi ve HDInsight izleme çözümünü nasıl ekleyeceğinizi öğrenin.

[Azure Monitor günlükleri,](../log-analytics/log-analytics-overview.md) kullanılabilirliklerini ve performanslarını korumak için bulutve şirket içi ortamlarınızı izleyen Azure Monitor'daki bir hizmettir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

* Log Analytics çalışma alanı. Bu çalışma alanını, kendi veri deposu, veri kaynakları ve çözümleriyle benzersiz bir Azure Monitor günlükleri ortamı olarak düşünebilirsiniz. Talimatlar için [bkz.](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace)

* Bir Azure HDInsight kümesi. Şu anda, aşağıdaki HDInsight küme türleri ile Azure Monitor günlüklerini kullanabilirsiniz:

  * Hadoop
  * HBase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  HDInsight kümesinin nasıl oluşturulacağına ilişkin talimatlar için Azure [HDInsight ile başlayın'](hadoop/apache-hadoop-linux-tutorial-get-started.md)a bakın.  

* Azure PowerShell Az modülü.  Bkz. [Yeni Azure PowerShell Az modülü tanıtımı.](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) En son sürüme sahip olduğundan emin olun. Gerekirse, çalıştırın. `Update-Module -Name Az`

> [!NOTE]  
> Daha iyi performans için hem HDInsight kümesini hem de Log Analytics çalışma alanını aynı bölgeye yerleştirmen önerilir. Azure Monitor günlükleri tüm Azure bölgelerinde kullanılamaz.

## <a name="enable-azure-monitor-logs-by-using-the-portal"></a>Portalı kullanarak Azure Monitör günlüklerini etkinleştirme

Bu bölümde, varolan bir HDInsight Hadoop kümesini işleri, hata ayıklama günlüklerini vb. izlemek için bir Azure Log Analytics çalışma alanı kullanacak şekilde yapılandırırsınız.

1. Azure [portalından](https://portal.azure.com/)kümenizi seçin.  Bkz. Yönergeler için [liste ve gösteri kümeleri.](./hdinsight-administer-use-portal-linux.md#showClusters) Küme yeni bir portal sayfasında açılır.

1. Soldan, **İzleme**altında Azure **Monitör'ü**seçin.

1. Ana görünümden, **Azure Monitör Tümleştirmesi**altında **Etkinleştir'i**seçin.

1. Çalışma **alanı** açılır listesini seç'ten varolan bir Günlük Analizi çalışma alanını seçin.

1. **Kaydet'i**seçin.  Ayarı kaydetmek birkaç dakika sürer.

    ![HDInsight kümeleri için izlemeyi etkinleştirme](./media/hdinsight-hadoop-oms-log-analytics-tutorial/azure-portal-monitoring.png "HDInsight kümeleri için izlemeyi etkinleştirme")

## <a name="enable-azure-monitor-logs-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure Monitör günlüklerini etkinleştirme

Azure PowerShell Az modülünü [etkinleştir-AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/enable-azhdinsightmonitoring) cmdlet'i kullanarak Azure Monitor günlüklerini etkinleştirebilirsiniz.

```powershell
# Enter user information
$resourceGroup = "<your-resource-group>"
$cluster = "<your-cluster>"
$LAW = "<your-Log-Analytics-workspace>"
# End of user input

# obtain workspace id for defined Log Analytics workspace
$WorkspaceId = (Get-AzOperationalInsightsWorkspace `
                    -ResourceGroupName $resourceGroup `
                    -Name $LAW).CustomerId

# obtain primary key for defined Log Analytics workspace
$PrimaryKey = (Get-AzOperationalInsightsWorkspace `
                    -ResourceGroupName $resourceGroup `
                    -Name $LAW | Get-AzOperationalInsightsWorkspaceSharedKeys).PrimarySharedKey

# Enables monitoring and relevant logs will be sent to the specified workspace.
Enable-AzHDInsightMonitoring `
    -ResourceGroupName $resourceGroup `
    -Name $cluster `
    -WorkspaceId $WorkspaceId `
    -PrimaryKey $PrimaryKey

# Gets the status of monitoring installation on the cluster.
Get-AzHDInsightMonitoring `
    -ResourceGroupName $resourceGroup `
    -Name $cluster
```

Devre dışı bırakmak için [Devre Dışı Bırakma-AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightmonitoring) cmdlet'ini kullanın:

```powershell
Disable-AzHDInsightMonitoring -Name "<your-cluster>"
```

## <a name="install-hdinsight-cluster-management-solutions"></a>HDInsight küme yönetimi çözümlerini yükleyin

HDInsight, Azure Monitor günlükleri için ekleyebileceğiniz kümeye özgü yönetim çözümleri sağlar. [Yönetim çözümleri,](../log-analytics/log-analytics-add-solutions.md) ek veri ve analiz araçları sağlayarak Azure Monitor günlüklerine işlevsellik ekler. Bu çözümler, HDInsight kümelerinizden önemli performans ölçümleri toplar ve ölçümleri aramak için araçlar sağlar. Bu çözümler ayrıca HDInsight'ta desteklenen küme türlerinin çoğu için görselleştirmeler ve panolar da sağlar. Çözümle birlikte topladığınız ölçümleri kullanarak, özel izleme kuralları ve uyarıları oluşturabilirsiniz.

Mevcut HDInsight çözümleri şunlardır:

* HDInsight Hadoop İzleme
* HDInsight HBase İzleme
* HDInsight İnteraktif Sorgu İzleme
* HDInsight Kafka İzleme
* HDInsight Kıvılcım İzleme
* HDInsight Fırtına İzleme

Bir yönetim çözümü yüklemek için verilen talimatlar için [Azure'da Yönetim çözümlerine](../azure-monitor/insights/solutions.md#install-a-monitoring-solution)bakın. Deneme yapmak için bir HDInsight Hadoop İzleme çözümü yükleyin. Bittiğinde, **Özet**altında listelenen bir **HDInsightHadoop** karosu görürsünüz. **HDInsightHadoop** karosu seçin. HDInsightHadoop çözümü aşağıdaki gibi görünür:

![HDInsight izleme çözüm görünümü](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Küme yepyeni bir küme olduğundan, rapor herhangi bir etkinlik göstermez.

## <a name="configuring-performance-counters"></a>Performans sayaçlarını yapılandırma

Azure monitörü, kümenizdeki düğümler için performans ölçümlerinin toplanıp çözümlenmesini de destekler. Bu özelliği etkinleştirme ve yapılandırma hakkında daha fazla bilgi için [Azure Monitor'daki Linux performans veri kaynaklarına](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-performance-counters#linux-performance-counters)bakın.

## <a name="cluster-auditing"></a>Küme denetimi

HDInsight, aşağıdaki günlük türlerini içe aktararak Azure Monitor günlükleriyle küme denetimini destekler:

* `log_gateway_audit_CL`- Bu tablo, başarılı ve başarısız oturum açma denemeleri gösteren küme ağ geçidi düğümlerinden denetim günlükleri sağlar.
* `log_auth_CL`- Bu tablo, SSH günlüklerine başarılı ve başarısız giriş denemeleri sağlar.
* `log_ambari_audit_CL`- Bu tablo Ambari'den denetim günlükleri sağlar.
* `log_ranger_audti_CL`- Bu tablo ESP kümeleri üzerinde Apache Ranger denetim günlükleri sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümelerini izlemek için Azure Monitor günlüklerini sorgula](hdinsight-hadoop-oms-log-analytics-use-queries.md)
