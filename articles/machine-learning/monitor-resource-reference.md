---
title: Azure Machine Learning veri başvurusunu izleme | Microsoft Docs
description: İzleme Azure Machine Learning için başvuru belgeleri. Azure Izleyici 'de toplanan ve sunulan veri & kaynakları hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.custom: subject-monitoring
ms.date: 10/02/2020
ms.openlocfilehash: f130fc0c65c49c33c838812fc2758619e0d1bca0
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102521348"
---
# <a name="monitoring-azure-machine-learning-data-reference"></a>Azure Machine Learning veri başvurusunu izleme

Azure Machine Learning çalışma alanınızdan Azure Izleyici tarafından toplanan veriler ve kaynaklar hakkında bilgi edinin. İzleme verilerini toplama ve çözümleme hakkında ayrıntılar için bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md) .

## <a name="metrics"></a>Ölçümler

Bu bölümde, Azure Machine Learning için toplanan otomatik olarak toplanan tüm platform ölçümleri listelenmektedir. Bu ölçümler için kaynak sağlayıcısı, [Microsoft. MachineLearningServices/Workspaces](../azure-monitor/essentials/metrics-supported.md#microsoftmachinelearningservicesworkspaces).

**Modelleme**

| Metric | Birim | Description |
| ----- | ----- | ----- |
| Model dağıtılamadı | Count | Başarısız olan model dağıtımı sayısı. |
| Model dağıtımı başlatıldı | Count | Başlatılan model dağıtımı sayısı. |
| Model dağıtımı başarılı oldu | Count | Başarılı olan model dağıtımı sayısı. |
| Model kaydı başarısız oldu | Count | Başarısız olan model kaydı sayısı. |
| Model kaydı başarılı oldu | Count | Başarılı olan model kaydı sayısı. |

**Kota**

Kota bilgileri yalnızca Azure Machine Learning işlem amaçlıdır.

| Metric | Birim | Description |
| ----- | ----- | ----- |
| Etkin çekirdekler | Count | Etkin işlem çekirdekleri sayısı. |
| Etkin düğümler | Count | Etkin düğümlerin sayısı. |
| Boştaki çekirdekler | Count | Boşta işlem çekirdekleri sayısı. |
| Boştaki düğümler | Count | Boşta işlem düğümlerinin sayısı. |
| Çekirdekleri bırakma | Count | Çekirdekleri bırakma sayısı. |
| Düğümlerden çıkılıyor | Count | Düğüm sayısı. |
| Önden çekirdekler | Count | Yok edilecek çekirdek sayısı. |
| Önden düğümler | Count | Yok edilecek düğüm sayısı. |
| Kota kullanım yüzdesi | Yüzde | Kullanılan Kota yüzdesi. |
| Toplam çekirdek | Count | Toplam çekirdek sayısı. |
| Toplam düğüm sayısı | Count | Toplam düğüm sayısı. |
| Kullanılamayan çekirdekler | Count | Kullanılamayan çekirdek sayısı. |
| Kullanılamayan düğümler | Count | Kullanılamayan düğüm sayısı. |

**Kaynak**

| Metric | Birim | Description |
| ----- | ----- | ----- |
| Cpukullanımı | Yüzde | Bir çalıştırma/iş sırasında belirli bir düğüm için CPU 'nun yüzde ne kadarı kullanıldı. Bu ölçüm yalnızca bir düğüm üzerinde bir iş çalışırken yayımlanır. Bir iş, bir veya daha fazla düğüm kullanabilir. Bu ölçüm düğüm başına yayımlanır. |
| Gpukullanım | Yüzde | Bir çalıştırma/iş sırasında belirli bir düğüm için GPU 'nun ne kadarının kullanıldığı. Bir düğümde bir veya daha fazla GPU bulunabilir. Bu ölçüm, düğüm başına GPU başına yayımlanır. |

**Çalıştır**

Eğitim çalıştırmaları hakkında bilgiler.

| Metric | Birim | Description |
| ----- | ----- | ----- |
| Tamamlanan çalıştırmalar | Count | Tamamlanan çalıştırma sayısı. |
| Başarısız çalıştırmalar | Count | Başarısız çalıştırmaların sayısı. |
| Başlatılan çalıştırmalar | Count | Başlatılan çalıştırma sayısı. |

## <a name="metric-dimensions"></a>Ölçüm boyutları

Ölçüm boyutları hakkında daha fazla bilgi için bkz. [çok boyutlu ölçümler](../azure-monitor/essentials/data-platform-metrics.md#multi-dimensional-metrics).

Azure Machine Learning, ölçümleriyle ilişkili aşağıdaki boyutlara sahiptir.

| Boyut | Description |
| ---- | ---- |
| Küme Adı | İşlem kümesi kaynağının adı. Tüm Kota ölçümleri için kullanılabilir. |
| VM ailesi adı | Küme tarafından kullanılan VM ailesi adı. Kota kullanımı yüzdesi için kullanılabilir. |
| VM önceliği | VM 'nin önceliği. Kota kullanımı yüzdesi için kullanılabilir.
| CreatedTime | Yalnızca Cpukullanımı ve Gpukullanımı için kullanılabilir. |
| DeviceId | Cihazın KIMLIĞI (GPU). Yalnızca Gpukullanım için kullanılabilir. |
| NodeId | İşin çalıştığı yerde oluşturulan düğümün KIMLIĞI. Yalnızca Cpukullanımı ve Gpukullanımı için kullanılabilir. |
| RunId | Çalışma/iş KIMLIĞI. Yalnızca Cpukullanımı ve Gpukullanımı için kullanılabilir. |
| ComputeType | Çalıştırmanın kullandığı işlem türü. Yalnızca tamamlanan çalıştırmalar, başarısız çalıştırmalar ve başlatılan çalıştırmalar için kullanılabilir. |
| PipelineStepType | Çalıştırmada kullanılan [ardışık düzen Inestep](/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinestep) türü. Yalnızca tamamlanan çalıştırmalar, başarısız çalıştırmalar ve başlatılan çalıştırmalar için kullanılabilir. |
| Publishedpipelineıd | Çalıştırmada kullanılan yayınlanan işlem hattının KIMLIĞI. Yalnızca tamamlanan çalıştırmalar, başarısız çalıştırmalar ve başlatılan çalıştırmalar için kullanılabilir. |
| RunType | Çalıştırma türü. Yalnızca tamamlanan çalıştırmalar, başarısız çalıştırmalar ve başlatılan çalıştırmalar için kullanılabilir. |

RunType boyutu için geçerli değerler şunlardır:

| Değer | Açıklama |
| ----- | ----- |
| Deneme | İşlem hattı olmayan çalıştırmalar. |
| Ardışık düzen eylemsizlik | Bir StepRun 'un üst öğesi olan bir işlem hattı çalıştırması. |
| StepRun | İşlem hattı adımı için bir çalıştırma. |
| ReusedStepRun | Önceki bir çalıştırmayı yeniden kullanan bir işlem hattı adımı için bir çalıştırma. |

## <a name="activity-log"></a>Etkinlik günlüğü

Aşağıdaki tabloda, etkinlik günlüğünde oluşturulabilecek Azure Machine Learning ilişkili işlemler listelenmiştir.

| İşlem | Description |
|:---|:---|
| Machine Learning çalışma alanı oluşturur veya güncelleştirir | Çalışma alanı oluşturuldu veya güncelleştirildi |
| Checkcomputenameavaılabılıty | İşlem adının zaten kullanımda olup olmadığını denetleyin |
| İşlem kaynaklarını oluşturur veya güncelleştirir | İşlem kaynağı oluşturuldu veya güncelleştirildi |
| İşlem kaynaklarını siler | Bir işlem kaynağı silindi |
| Gizli dizileri listeleme | Bir Machine Learning çalışma alanı için, işlem tarafından listelenen gizli dizileri |

## <a name="resource-logs"></a>Kaynak günlükleri

Bu bölümde, Azure Machine Learning çalışma alanı için toplayacağınız kaynak günlüklerinin türleri listelenmektedir.

Kaynak sağlayıcısı ve tür: [Microsoft. MachineLearningServices/Workspace](../azure-monitor/essentials/resource-logs-categories.md#microsoftmachinelearningservicesworkspaces).

| Kategori | Görünen Ad |
| ----- | ----- |
| AmlComputeClusterEvent | AmlComputeClusterEvent |
| AmlComputeClusterNodeEvent | AmlComputeClusterNodeEvent |
| AmlComputeCpuGpuUtilization | AmlComputeCpuGpuUtilization |
| AmlComputeJobEvent | AmlComputeJobEvent |
| AmlRunStatusChangedEvent | AmlRunStatusChangedEvent |

## <a name="schemas"></a>Şemalar

Aşağıdaki şemalar Azure Machine Learning tarafından kullanılıyor

### <a name="amlcomputejobevents-table"></a>AmlComputeJobEvents tablosu

| Özellik | Açıklama |
|:--- |:---|
| TimeGenerated | Günlük girişinin oluşturulduğu saat |
| OperationName | Günlük olayı ile ilişkili işlemin adı |
| Kategori | Günlük olayının adı, AmlComputeClusterNodeEvent |
| JobId | Gönderilen Işin KIMLIĞI |
| ExperimentId | Deneme KIMLIĞI |
| ExperimentName | Deneme adı |
| CustomerSubscriptionId | Deneme ve Işin gönderildiği abonelik |
| WorkspaceName | Machine Learning çalışma alanının adı |
| ClusterName | Kümenin adı |
| ProvisioningState | Iş gönderimi durumu |
| ResourceGroupName | Kaynak grubunun adı |
| JobName | Işin adı |
| Lclusterıd | Kümenin KIMLIĞI |
| Olay türü | Iş olayının türü. Örneğin, Jobgönderildi, JobRunning, JobFailed, JobSucceeded. |
| ExecutionState | İşin durumu (çalıştırma). Örneğin, sıraya alınmış, çalışıyor, başarılı, başarısız |
| ErrorDetails | İş hatası ayrıntıları |
| CreationApiVersion | İşi oluşturmak için kullanılan API sürümü |
| ClusterResourceGroupName | Kümenin kaynak grubu adı |
| TFWorkerCount | TF Worker sayısı |
| TFParameterServerCount | TF Parameter Server sayısı |
| Araç türü | Kullanılan araç türü |
| Runıncontainer | İşin bir kapsayıcı içinde çalıştırılması gerekip gerekmediğini açıklayan bayrak |
| JobErrorMessage | Iş hatası ayrıntılı iletisi |
| NodeId | İşin çalıştığı, oluşturulan düğümün KIMLIĞI |

### <a name="amlcomputeclusterevents-table"></a>AmlComputeClusterEvents tablosu

| Özellik | Açıklama |
|:--- |:--- |
| TimeGenerated | Günlük girişinin oluşturulduğu saat |
| OperationName | Günlük olayı ile ilişkili işlemin adı |
| Kategori | Günlük olayının adı, AmlComputeClusterNodeEvent |
| ProvisioningState | Kümenin sağlama durumu |
| ClusterName | Kümenin adı |
| ClusterType | Kümenin türü |
| CreatedBy | Kümeyi oluşturan Kullanıcı |
| CoreCount | Kümedeki çekirdekler sayısı |
| VmSize | Kümenin VM boyutu |
| VmPriority | Adanmış/LowPriority kümesi içinde oluşturulan düğümlerin önceliği |
| ScalingType | El ile veya otomatik olarak küme ölçeklendirme türü |
| Initialnodecount | Kümenin ilk düğüm sayısı |
| MinimumNodeCount | Kümenin en düşük düğüm sayısı |
| MaximumNodeCount | Kümenin en fazla düğüm sayısı |
| Nodeanlaşlocationoption | Düğümün nasıl serbest bırakılmasının gerektiği |
| Publisher | Küme türü yayımcısı |
| Sunduğu | Kümenin oluşturulduğu teklif |
| Sku | Küme içinde oluşturulan düğüm/VM SKU 'su |
| Sürüm | Düğüm/VM oluşturulurken kullanılan görüntünün sürümü |
| SubnetID | Kümenin alt NetID |
| AllocationState | Küme ayırma durumu |
| CurrentNodeCount | Kümenin geçerli düğüm sayısı |
| TargetNodeCount | Ölçek artırma/azaltma sırasında kümenin hedef düğüm sayısı |
| Olay türü | Küme oluşturma sırasında olay türü. |
| NodeIdleTimeSecondsBeforeScaleDown | Kümenin aşağı ölçeklendirilmesi için saniye cinsinden boş süre |
| PreemptedNodeCount | Kümenin küme dışı düğüm sayısı |
| Iresizegrow | Kümenin ölçeğini belirten bayrak |
| VmFamilyName | Küme içinde oluşturulabilecek düğümlerin VM ailesi adı |
| LeavingNodeCount | Kümenin düğüm sayısını bırakma |
| UnusableNodeCount | Kümenin kullanılamayan düğüm sayısı |
| Idıdnodecount | Kümenin boşta düğüm sayısı |
| RunningNodeCount | Kümenin çalışan düğüm sayısı |
| Hazırlık Ingnodecount | Kümenin düğüm sayısı hazırlanıyor |
| Kota ayrılmış | Kümeye ayrılan kota |
| Kota kullanıldı | Küme kotası kullanıldı |
| Allocationstategeçişli saati | Bir durumdan diğerine geçiş süresi |
| ClusterErrorCodes | Küme oluşturma veya ölçeklendirme sırasında hata kodu alındı |
| CreationApiVersion | Küme oluşturulurken kullanılan API sürümü |

### <a name="amlcomputeclusternodeevents-table"></a>AmlComputeClusterNodeEvents tablosu

| Özellik | Açıklama |
|:--- |:--- |
| TimeGenerated | Günlük girişinin oluşturulduğu saat |
| OperationName | Günlük olayı ile ilişkili işlemin adı |
| Kategori | Günlük olayının adı, AmlComputeClusterNodeEvent |
| ClusterName | Kümenin adı |
| NodeId | Oluşturulan küme düğümünün KIMLIĞI |
| VmSize | Düğümün VM boyutu |
| VmFamilyName | Düğümün ait olduğu VM ailesi |
| VmPriority | Oluşturulan düğümün önceliği/LowPriority |
| Publisher | VM görüntüsünün yayımcısı. Örneğin, Microsoft-dsvm |
| Sunduğu | VM oluşturma ile ilişkili teklif |
| Sku | Oluşturulan düğüm/VM SKU 'su |
| Sürüm | Düğüm/VM oluşturulurken kullanılan görüntünün sürümü |
| ClusterCreationTime | Kümenin oluşturulduğu zaman |
| ResizeStartTime | Küme ölçeği artırma/azaltma süresi |
| Resizebitişsaati | Küme ölçeği artırma/azaltma süresi sonu |
| NodeAllocationTime | Düğümün ayrıldığı saat |
| NodeBootTime | Düğümün önyüklendiği zaman |
| StartTaskStartTime | Görevin bir düğüme atandığı ve başlatıldığı zaman |
| StartTaskEndTime | Bir düğüme atanan görevin sona erdiği saat |
| TotalE2ETimeInSeconds | Toplam süre düğümü etkin |


## <a name="see-also"></a>Ayrıca bkz.

- İzleme Azure Machine Learning açıklaması için bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md) .
- Azure kaynaklarını izleme hakkında ayrıntılı bilgi için bkz. Azure [izleyici ile Azure kaynaklarını izleme](../azure-monitor/essentials/monitor-azure-resource.md) .
