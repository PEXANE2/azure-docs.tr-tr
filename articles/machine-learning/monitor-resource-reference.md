---
title: Veri referansı izleme | Microsoft Dokümanlar
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için toplanan ve Azure Monitor'da kullanılabilen veriler ve kaynaklar hakkında bilgi edinin. Azure Monitor, Azure Machine Learning çalışma alanınız hakkında veri toplar ve yüzeylere sahip çıkar ve ölçümleri görüntülemenize, uyarıları ayarlamanıza ve günlüğe kaydedilmiş verileri çözümlemenize olanak tanır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 03/06/2020
ms.openlocfilehash: 958794cda60d0ce1b0d223b9b5a6c03283022a6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78927561"
---
# <a name="azure-machine-learning-monitoring-data-reference"></a>Azure makine öğrenimi izleme veri başvurusu

Azure Machine Learning çalışma alanınızdan Azure Monitor tarafından toplanan veriler ve kaynaklar hakkında bilgi edinin. İzleme verilerinin toplanması ve analiz için ayrıntılı bilgi için [Azure Machine Learning'](monitor-azure-machine-learning.md) i izleme önüne bakın.

## <a name="resource-logs"></a>Kaynak günlükleri

Aşağıdaki tabloda, Azure Monitor Günlükleri veya Azure Depolama'da toplandığında Azure Machine Learning kaynak günlüklerinin özellikleri listelenir.

### <a name="amlcomputejobevents-table"></a>AmlComputeJobEvents tablosu

| Özellik | Açıklama |
|:--- |:---|
| TimeGenerated | Günlük girişinin oluşturulduğu saat |
| ThrottledRequests | Günlük olayıyla ilişkili işlemin adı |
| Kategori | Günlük olayının adı, AmlComputeClusterNodeEvent |
| JobId | Gönderilen İş Kimliği |
| Deney Id | Deneyin Kimliği |
| Deneme Adı | Deneyin Adı |
| MüşteriSubscriptionId | Deneme ve İş'in gönderildiği Yer Aboneliği |
| Workspacename | Makine öğrenimi çalışma alanının adı |
| ClusterName | Kümenin Adı |
| ProvisioningState | İş Teslim Durumu |
| ResourceGroupName | Kaynak grubunun adı |
| İş Adı | İşin Adı |
| ClusterId | Kümenin kimliği |
| Olay türü | İş olayının türü, örneğin, JobSubmitted, JobRunning, JobFailed, JobSucceeded, vb. |
| ExecutionState | İşin durumu (Run), örneğin, Sıraya Alındı, Çalıştırıldı, Başarılı, Başarısız |
| ErrorDetails | İş hatasının ayrıntıları |
| CreationApiVersion | İşi oluşturmak için kullanılan Api sürümü |
| ClusterResourceGroupName | Kümenin kaynak grubu adı |
| TFİşçi Sayısı | TF çalışanlarının sayısı |
| TFParameterServerCount | TF parametre sunucusu sayısı |
| Araç Türü | Kullanılan aletin türü |
| RuninContainer | İşbir kapsayıcının içinde çalıştırılmalı mı açıklayan bayrak |
| İşHatası Mesajı | İş hatası ayrıntılı mesaj |
| Düğüm | İş çalışırken oluşturulan düğümün kimliği |

### <a name="amlcomputeclusterevents-table"></a>AmlComputeClusterEtkinlikler tablosu

| Özellik | Açıklama |
|:--- |:--- |
| TimeGenerated | Günlük girişinin oluşturulduğu saat |
| ThrottledRequests | Günlük olayıyla ilişkili işlemin adı |
| Kategori | Günlük olayının adı, AmlComputeClusterNodeEvent |
| ProvisioningState | Kümenin sağlama durumu |
| ClusterName | Kümenin adı |
| Küme Tipi | Kümenin türü |
| Createdby | Kümeyi oluşturan kullanıcı |
| Çekirdek Sayısı | Kümedeki çekirdek sayısı |
| VmSize | Kümenin Vm boyutu |
| VmÖncelikli | Adanmış/Düşük Öncelikli bir küme içinde oluşturulan düğümlerin önceliği |
| Ölçekleme Türü | Küme ölçekleme kılavuzu/otomatik türü |
| InitialNodeCount | Kümenin ilk düğüm sayısı |
| MinimumNodeCount | Kümenin minimum düğüm sayısı |
| MaximumNodeCount | Kümenin maksimum düğüm sayısı |
| DüğümDeallocationOption | Düğüm nasıl ayrılır? |
| Yayımcı | Küme türünün yayımcısı |
| Sunduğu | Kümenin oluşturulduğu teklif |
| Sku | Küme içinde oluşturulan Düğümün/VM'nin Sku'su |
| Sürüm | Düğüm/VM oluşturulurken kullanılan görüntünün sürümü |
| SubnetId | Kümenin SubnetId'i |
| TahsisDurumu | Küme ayırma durumu |
| CurrentNodeCount | Kümenin geçerli düğüm sayısı |
| TargetNodeCount | Yukarı/aşağı ölçekleme sırasında kümenin hedef düğüm sayısı |
| Olay türü | Küme oluşturma sırasında olay türü. |
| NodeIdleTimeSecondsBeforeScaleDown | Küme küçültilmeden saniyeler içinde boşta kalma süresi |
| PreemptedNodeCount | Kümenin preempted düğüm sayısı |
| IsResizeGrow | Kümenin ölçeklediğini belirten bayrak |
| VmFamilyName | Küme içinde oluşturulabilen düğümlerin VM ailesinin adı |
| LeavingNodeCount | Kümenin düğüm sayısını bırakma |
| KullanılamazSayı | Kümenin kullanılamaz düğüm sayısı |
| IdleNodeCount | Kümenin boşta düğüm sayısı |
| RunningNodeCount | Kümenin çalışan düğüm sayısı |
| PreparingNodeCount | Kümenin düğüm sayısını hazırlama |
| Kota Tahsis edildi | Kümeye ayrılan kota |
| KotaKullanıldı | Kümenin kullanılan kotası |
| TahsisDurumuGeçiş Zamanı | Bir durumdan diğerine geçiş zamanı |
| Kümehata Kodları | Küme oluşturma veya ölçekleme sırasında alınan hata kodu |
| CreationApiVersion | Küme oluşturulurken kullanılan Api sürümü |

### <a name="amlcomputeclusternodeevents-table"></a>AmlComputeClusterNodeEvents tablosu

| Özellik | Açıklama |
|:--- |:--- |
| TimeGenerated | Günlük girişinin oluşturulduğu saat |
| ThrottledRequests | Günlük olayıyla ilişkili işlemin adı |
| Kategori | Günlük olayının adı, AmlComputeClusterNodeEvent |
| ClusterName | Kümenin adı |
| Düğüm | Oluşturulan küme düğümünün kimliği |
| VmSize | Düğümün Vm boyutu |
| VmFamilyName | Düğümün ait olduğu Vm ailesi |
| VmÖncelikli | Özel/Düşük Öncelikli oluşturulan düğümün önceliği |
| Yayımcı | VM resminin yayımcısı, örneğin, microsoft-dsvm |
| Sunduğu | VM oluşturma ile ilişkili teklif |
| Sku | Oluşturulan Düğümün/VM'nin Sku'su |
| Sürüm | Düğüm/VM oluşturulurken kullanılan görüntünün sürümü |
| ClusterCreationTime | Kümenin oluşturulduğu zaman |
| Yeniden BoyutlandırBaşlangıç Zamanı | Küme ölçeğinin yukarı/aşağı başladığı zaman |
| Yeniden BoyutlandırmaSon Saat | Küme ölçeğinin yukarı/aşağı sona erdirildiğinde |
| NodeAllocationTime | Düğümün tahsis edildiği saat |
| DüğümBootTime | Düğümün başlatıldırış zamanı |
| Başlangıç GöreviBaşlangıç Saati | Görevin bir düğüme atandığı ve başlatıldıkları zaman |
| BaşlangıçTaskEndTime | Düğüme atanan görevin sona erdirildiğinde |
| Totale2ETimeinSeconds | Toplam zaman düğümü etkindi |

### <a name="metrics"></a>Ölçümler

Aşağıdaki tablolarda Azure Machine Learning için toplanan platform ölçümleri listelenir Tüm ölçümler namespace **Azure Machine Learning Workspace'te**depolanır.

**Model**

| Ölçüm | Birim | Açıklama |
| ----- | ----- | ----- |
| Model dağıtımı başarısız oldu | Sayı | Başarısız olan model dağıtımlarının sayısı. |
| Model dağıtımı başladı | Sayı | Model dağıtımlarının sayısı başladı. |
| Model dağıtımı başarılı | Sayı | Başarılı model dağıtımlarının sayısı. |
| Model kaydı başarısız oldu | Sayı | Başarısız olan model kayıtlarının sayısı. |
| Model kaydı başarılı oldu | Sayı | Başarılı model kayıtlarının sayısı. |

**Kota**

Kota bilgileri yalnızca Azure Machine Learning bilgi işlem içindir.

| Ölçüm | Birim | Açıklama |
| ----- | ----- | ----- |
| Aktif çekirdekler | Sayı | Etkin işlem çekirdeklerinin sayısı. |
| Etkin düğümler | Sayı | Etkin düğüm sayısı. |
| Boş çekirdekler | Sayı | Boşta işlem çekirdeklerinin sayısı. |
| Boşta düğümler | Sayı | Boşta işlem düğümlerinin sayısı. |
| Çekirdekbırakma | Sayı | Ayrılan çekirdek lerin sayısı. |
| Düğümleri bırakma | Sayı | Ayrılan düğümlerin sayısı. |
| Preempted çekirdekler | Sayı | Preempted çekirdek sayısı. |
| Preempted düğümler | Sayı | Önceden engellenen düğümlerin sayısı. |
| Kota kullanım yüzdesi | Yüzde | Kullanılan kota yüzdesi. |
| Toplam çekirdek | Sayı | Toplam çekirdek. |
| Toplam düğümler | Sayı | Toplam düğümler. |
| Kullanılamaz çekirdekler | Sayı | Kullanışsız çekirdek sayısı. |
| Kullanılamaz düğümler | Sayı | Kullanılamaz düğüm sayısı. |

Kota ölçümlerini filtrelemek için kullanılabilecek boyutlar şunlardır:

| Boyut | Metrik(ler) ile kullanılabilir | Açıklama |
| ---- | ---- | ---- |
| Küme Adı | Tüm kota ölçümleri | İşlem örneğinin adı. |
| Vm Aile Adı | Kota kullanım yüzdesi | Küme tarafından kullanılan VM ailesinin adı. |
| Vm Önceliği | Kota kullanım yüzdesi | VM'nin önceliği.

**Çalıştırmak**

Eğitim koşuları hakkında bilgi.

| Ölçüm | Birim | Açıklama |
| ----- | ----- | ----- |
| Tamamlanan çalıştırmalar | Sayı | Tamamlanan çalıştırma sayısı. |
| Başarısız çalıştırmalar | Sayı | Başarısız çalıştırma sayısı. |
| Başlatılan çalıştırmalar | Sayı | Başlatılan çalıştırma ların sayısı. |

Çalıştırma ölçümlerini filtrelemek için kullanılabilecek boyutlar şunlardır:

| Boyut | Açıklama |
| ---- | ---- |
| ComputeType | Çalıştırmanın kullandığı işlem türü. |
| PipelineStepType | Çalıştırmada kullanılan [PipelineStep](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinestep?view=azure-ml-py) türü. |
| Yayınlanan PipelineId | Çalıştırmada kullanılan yayımlanmış ardışık boru hattının kimliği. |
| RunType | Çalışma türü. |

RunType boyutu için geçerli değerler şunlardır:

| Değer | Açıklama |
| ----- | ----- |
| Deneme | Boru hattı olmayan çalışır. |
| PipelineRun | Bir StepRun'un ana hattı olan bir ardışık hat lar çalışır. |
| StepRun | Bir boru hattı adımı için bir çalışma. |
| Yeniden UsedStepRun | Önceki çalıştırmayı yeniden kullanan bir ardışık adadım için çalışma. |

## <a name="see-also"></a>Ayrıca Bkz.

- Azure Machine Learning'i izleme açıklaması için [Azure Machine Learning'i](monitor-azure-machine-learning.md) izleme bölümüne bakın.
- Azure kaynaklarını izleme yle ilgili ayrıntılar için [Azure Kaynaklarını Azure Monitörü ile izleme](/azure/azure-monitor/insights/monitor-azure-resource) bilgisine bakın.
