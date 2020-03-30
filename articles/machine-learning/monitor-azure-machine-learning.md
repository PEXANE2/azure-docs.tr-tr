---
title: Azure Makine Öğrenimini İzleme | Microsoft Dokümanlar
description: Azure Machine Learning'in ölçümleri hakkında uyarıları görüntülemek, analiz etmek ve oluşturmak için Azure Monitor'u nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 03/05/2020
ms.openlocfilehash: eb4f46322bec57fb4412d3ddebb345640556ca5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399098"
---
# <a name="monitoring-azure-machine-learning"></a>Azure Makine Öğrenimini İzleme

Bu makalede, Azure Machine Learning tarafından oluşturulan izleme verileri açıklanmaktadır. Ayrıca, verilerinizi çözümlemek ve uyarıları tanımlamak için Azure Monitörünü nasıl kullanabileceğinizi de açıklar.

> [!TIP]
> Azure Machine Learning'in izlemesini açıkladığı için, bu belgedeki bilgiler öncelikle yöneticiler içindir. Veri uzmanı veya geliştiriciyseniz ve model eğitim çalıştırmalarınıza özel bilgileri izlemek istiyorsanız aşağıdaki belgelere bakın:
>
> * [Eğitim çalıştırmalarını başlatma, izleme ve iptal etme](how-to-manage-runs.md)
> * [Eğitim çalıştırmaları için günlük ölçümleri](how-to-track-experiments.md)
> * [MLflow ile denemeleri izleme](how-to-use-mlflow.md)
> * [TensorBoard ile çalıştırmaları görselleştirme](how-to-monitor-tensorboard.md)

## <a name="azure-monitor"></a>Azure İzleyici

Azure Machine Learning, Azure'da tam bir yığın izleme hizmeti olan Azure Monitor'u kullanarak izleme verilerini kaydeder. Azure Monitor, Azure kaynaklarınızı izlemek için eksiksiz bir özellik kümesi sağlar. Ayrıca diğer bulutlarve şirket içi kaynakları izleyebilirsiniz.

İzleme yeteneklerine genel bir bakış sağlayan Azure Monitor makalesine genel [bakışla](/azure/azure-monitor/overview)başlayın. Aşağıdaki bölümler, Azure Machine Learning ile Azure Monitor kullanmanın ayrıntılarını sağlayarak bu bilgileri temel alar.

Azure Monitor ile ilişkili maliyetleri anlamak için [Kullanım ve tahmini maliyetleri](/azure/azure-monitor/platform/usage-estimated-costs)görün. Verilerinizin Azure Monitor'da görünmesi için gereken süreyi anlamak için veri [giriş saatine](/azure/azure-monitor/platform/data-ingestion-time)bakın.

## <a name="monitoring-data-from-azure-machine-learning"></a>Azure Machine Learning'den verileri izleme

Azure Machine Learning, [Azure kaynaklarından gelen verileri izlemede](/azure/azure-monitor/insights/monitor-azure-resource#monitoring-data)açıklanan diğer Azure kaynaklarıyla aynı izleme veritürlerini toplar. Azure Machine Learning tarafından oluşturulan günlüklerin ve ölçümlerin ayrıntılı bir başvurusu için [Azure Machine Learning izleme veri başvurusuna](monitor-resource-reference.md) bakın.

## <a name="analyzing-metric-data"></a>Metrik verileri çözümleme

**Azure Monitor** menüsünden **Ölçümleri** açarak Azure Machine Learning ölçümlerini analiz edebilirsiniz. Bu aracı kullanma yla ilgili ayrıntılar için [Azure Ölçümleri Gezgini ile başlarken](/azure/azure-monitor/platform/metrics-getting-started) bkz.

Azure Machine Learning için tüm ölçümler namespace **Machine Learning Service Workspace'te**yer alıyor.

![Machine Learning Service Workspace ile Metrics Explorer seçildi](./media/monitor-azure-machine-learning/metrics.png)

### <a name="filtering-and-splitting"></a>Filtreleme ve bölme

Boyutları destekleyen ölçümler için, bir boyut değeri kullanarak filtreler uygulayabilirsiniz. Örneğin, `cpu-cluster`Bir **Küme Adı** için Etkin **Çekirdekleri** filtreleme. 

Ayrıca, ölçümün farklı bölümlerinin birbiriyle nasıl karşılaştırın olduğunu görselleştirmek için bir ölçütü boyuta bölebilirsiniz. Örneğin, **ardışık** alanda kullanılan adım türlerinin sayısını görmek için Pipeline Step Type'ı bölme.

Filtreleme ve bölme hakkında daha fazla bilgi için [Azure Monitor'un Gelişmiş özelliklerine](/azure/azure-monitor/platform/metrics-charts)bakın.

## <a name="alerts"></a>Uyarılar

**Azure Monitor** menüsünden **Uyarıları** açarak Azure Machine Learning uyarılarına erişebilirsiniz. Bkz. Uyarı oluşturma yla ilgili ayrıntılar için [Azure Monitor'u kullanarak metrik uyarıları oluşturun, görüntüleyin ve yönetin.](/azure/azure-monitor/platform/alerts-metric)

Aşağıdaki tabloda Azure Makine Öğrenimi için yaygın ve önerilen metrik uyarı kuralları listelenebönemlidir:

| Uyarı türü | Koşul | Açıklama |
|:---|:---|:---|
| Model Dağıtma Başarısız Oldu | Toplama türü: Toplam, Operatör: Büyük, Eşik değeri: 0 | Bir veya daha fazla model dağıtımı başarısız olduğunda |
| Kota Kullanım Yüzdesi | Toplama türü: Ortalama, Operatör: Büyük, Eşik değeri: 90| Kota kullanım yüzdesi %90'dan büyükolduğunda |
| Kullanılamaz Düğümler | Toplama türü: Toplam, Operatör: Büyük, Eşik değeri: 0 | Bir veya daha fazla kullanılamaz düğüm olduğunda |

## <a name="configuration"></a>Yapılandırma

> [!IMPORTANT]
> __Azure Makine Öğrenimi ölçümleri yapılandırılması gerekmez,__ otomatik olarak toplanır ve izleme ve uyarı için Metrics Explorer'da kullanılabilir.

Aşağıdaki işlevselliği yapılandırmak için bir tanılama ayarı ekleyebilirsiniz:

* Günlük ve metrik bilgilerini bir Azure depolama hesabına arşivleyin.
* Günlük ve metrik bilgilerini bir Azure Etkinlik Hub'ına aktarın.
* Günlük ve metrik bilgilerini Azure Monitor Log Analytics'e gönderin.

Bu ayarları etkinleştirmek için ek Azure hizmetleri (depolama hesabı, etkinlik merkezi veya Log Analytics) gerektirir ve bu da maliyetinizi artırabilir. Tahmini maliyeti hesaplamak için [Azure fiyatlandırma hesaplayıcısı'nı](https://azure.microsoft.com/pricing/calculator)ziyaret edin.

Tanılama ayarı oluşturma hakkında daha fazla bilgi için [azure'da platform günlükleri ve ölçümleri toplamak için tanılama ayarı oluştur'a](/azure/azure-monitor/platform/diagnostic-settings)bakın.

Azure Machine Learning için aşağıdaki günlükleri yapılandırabilirsiniz:

| Kategori | Açıklama |
|:---|:---|
| AmlComputeClusterOlay | Azure Machine Learning bilgi işlem kümelerinden etkinlikler. |
| AmlComputeClusterNodeEvent | Bir Azure Machine Learning bilgi işlem kümesiiçindeki düğümlerden gelen olaylar. |
| AmlComputeJobOlay | Azure Machine Learning bilgi işlem işlem lerinde çalışan işlerden etkinlikler. |

> [!NOTE]
> Tanılama ayarında ölçümleri etkinleştirdiğinizde, boyut bilgileri şu anda bir depolama hesabına, olay merkezine veya günlük analitiğine gönderilen bilgilerin bir parçası olarak dahil edilmez.

## <a name="analyzing-log-data"></a>Günlük verilerini çözümleme

Azure Monitor Log Analytics'i kullanmak için bir tanı yapılandırması oluşturmanız ve Bilgi Gönder'in __Log Analytics'e gönderilmesini__sağlamanız gerekiyor. Daha fazla bilgi için [Yapılandırma](#configuration) bölümüne bakın.

Azure Monitör Günlükleri'ndeki veriler tablolarda depolanır ve her tablonun kendine özgü özellikleri vardır. Azure Machine Learning verileri aşağıdaki tablolarda depolar:

| Tablo | Açıklama |
|:---|:---|
| AmlComputeClusterOlay | Azure Machine Learning bilgi işlem kümelerinden etkinlikler. |
| AmlComputeClusterNodeEvent | Bir Azure Machine Learning bilgi işlem kümesiiçindeki düğümlerden gelen olaylar. |
| AmlComputeJobOlay | Azure Machine Learning bilgi işlem işlem lerinde çalışan işlerden etkinlikler. |

> [!IMPORTANT]
> Azure Machine Learning menüsünden **Günlükler'i** seçtiğinizde, Geçerli çalışma alanına ayarlanan sorgu kapsamıyla Log Analytics açılır. Bu, günlük sorgularının yalnızca bu kaynaktan gelen verileri içereceği anlamına gelir. Diğer veritabanlarından veya diğer Azure hizmetlerinden gelen verileri içeren bir sorgu çalıştırmak istiyorsanız, **Azure Monitörü** menüsünden **Günlükler'i** seçin. Ayrıntılar [için Azure Monitor Log Analytics'te Günlük sorgu kapsamı ve zaman aralığına](/azure/azure-monitor/log-query/scope/) bakın.

Günlükler ve ölçümler hakkında ayrıntılı bir başvuru için Azure [Machine Learning izleme veri referansına](monitor-resource-reference.md)bakın.

### <a name="sample-queries"></a>Örnek sorgular

Azure Machine Learning kaynaklarınızı izlemenize yardımcı olmak için kullanabileceğiniz sorgular şunlardır: 

+ Son beş gün içinde başarısız işler alın:

    ```Kusto
    AmlComputeJobEvent
    | where TimeGenerated > ago(5d) and EventType == "JobFailed"
    | project  TimeGenerated , ClusterId , EventType , ExecutionState , ToolType
    ```

+ Belirli bir iş adı için kayıt alın:

    ```Kusto
    AmlComputeJobEvent
    | where JobName == "automl_a9940991-dedb-4262-9763-2fd08b79d8fb_setup"
    | project  TimeGenerated , ClusterId , EventType , ExecutionState , ToolType
    ```

+ VM boyutunun Standard_D1_V2 kümeler için son beş gün içinde küme olayları alın:

    ```Kusto
    AmlComputeClusterEvent
    | where TimeGenerated > ago(4d) and VmSize == "STANDARD_D1_V2"
    | project  ClusterName , InitialNodeCount , MaximumNodeCount , QuotaAllocated , QuotaUtilized
    ```

+ Son sekiz gün içinde düğümler tahsis alın:

    ```Kusto
    AmlComputeClusterNodeEvent
    | where TimeGenerated > ago(8d) and NodeAllocationTime  > ago(8d)
    | distinct NodeId
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Günlüklere ve ölçümlere başvurmak için Azure [Machine Learning izleme veri referansına](monitor-resource-reference.md)bakın.
- Azure Machine Learning ile ilgili kotalarla çalışma hakkında daha fazla bilgi için Azure [kaynakları için kotaları yönet ve talep etme bölümüne](how-to-manage-quotas.md)bakın.
- Azure kaynaklarını izleme yle ilgili ayrıntılar için Azure [Kaynaklarını Azure Monitörü ile izleme](/azure/azure-monitor/insights/monitor-azure-resource)bilgisine bakın.
