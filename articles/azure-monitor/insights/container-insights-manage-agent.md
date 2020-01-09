---
title: Azure İzleyici kapsayıcı Aracısı'nı yönetme | Microsoft Docs
description: Bu makalede, kapsayıcılar için Azure İzleyici tarafından kullanılan kapsayıcılı Log Analytics aracısını ile en sık kullanılan bakım görevlerini yönetmek açıklar.
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: 5bd3af7787ee38011c52224f5830d8b719031db8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75404257"
---
# <a name="how-to-manage-the-azure-monitor-for-containers-agent"></a>Azure İzleyici kapsayıcı Aracısı'nı yönetme
Kapsayıcılar için Azure İzleyici, Linux için Log Analytics aracısını kapsayıcı bir sürümünü kullanır. İlk dağıtımdan sonra yordamı veya yaşam döngüsü sırasında gerçekleştirmeniz gerekebilir isteğe bağlı görevleri vardır. Bu makale hakkında ayrıntılı bilgi el ile aracı yükseltme ve belirli bir kapsayıcıda ortam değişkenlerinin koleksiyonunu devre dışı bırakın. 

## <a name="how-to-upgrade-the-azure-monitor-for-containers-agent"></a>Azure İzleyici kapsayıcı Aracısı yükseltme
Kapsayıcılar için Azure İzleyici, Linux için Log Analytics aracısını kapsayıcı bir sürümünü kullanır. Aracıyı yeni bir sürümü yayımlandığında, aracıyı Azure Kubernetes Service (AKS) barındırılan yönetilen Kubernetes kümeleri üzerinde otomatik olarak yükseltilir.  

Bu makalede, aracı yükseltme başarısız olursa, el ile aracı yükseltme işlemi açıklanmaktadır. Yayımlanan sürümleri takip etmek için bkz: [Aracı sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).   

### <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>İzlenen bir Kubernetes kümesinde aracı yükseltme
Aracı yükseltme işlemi, iki anlaşılır adımdan oluşur. İlk adım, Azure İzleyici ile Azure CLI kullanarak kapsayıcıları için izleme devre dışı bırakmaktır.  İçinde açıklanan adımları izleyin [izlemeyi devre dışı](container-insights-optout.md?#azure-cli) makalesi. Azure CLI kullanarak çözüm ve çalışma alanında depolanan karşılık gelen verileri etkilemeden Kümedeki düğümlerden aracıyı kaldırmak sağlıyor. 

>[!NOTE]
>Bu bakım etkinliği gerçekleştiriyorsanız, kümedeki düğümler, toplanan verileri ilettiğiniz değil ve performans görünümlerine veri gösterilmez arasındaki zaman aracıyı kaldırın ve ardından yeni sürümü yükleyin. 
>

Aracının yeni sürümünü yüklemek için, bu işlemi gerçekleştirmek için [Azure CLI kullanarak izlemeyi etkinleştirme](container-insights-enable-new-cluster.md#enable-using-azure-cli)bölümünde açıklanan adımları izleyin.  

İzleme yeniden etkinleştirdikten sonra bu küme için güncelleştirilmiş sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. Aracı başarıyla yükseltildi doğrulamak için komutu çalıştırın: `kubectl logs omsagent-484hw --namespace=kube-system`

Durum, aşağıdaki örneğe benzemelidir burada değeri *OMI* ve *omsagent* belirtilen en son sürümü ile eşleşmelidir [Aracı sürüm geçmişi](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).  

    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.5
    omsagent 1.6.0-163
    docker-cimprov 1.0.0.31

## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>Bir kapsayıcı ortam değişkeni koleksiyonunu devre dışı bırakma
Kapsayıcılar için Azure İzleyici, bir pod içinde çalışan kapsayıcılar ortam değişkenlerini toplar ve bunları seçilen kapsayıcı içindeki özellik bölmesinde sunar **kapsayıcıları** görünümü. Bu davranışı, AKS kümesinin dağıtımı sırasında veya *AZMON_COLLECT_ENV*ortam değişkenini ayarlayarak, belirli bir kapsayıcı için koleksiyonu devre dışı bırakarak denetleyebilirsiniz. Bu özellik, aracı sürümü – ciprod11292018 kullanılabilir ve daha yüksek.  

Ortam değişkenlerinin koleksiyonunu yeni veya mevcut bir kapsayıcıda devre dışı bırakmak için, **AZMON_COLLECT_ENV** değişkenini Kubernetes Deployment YAML yapılandırma dosyanızda **false** değeriyle ayarlayın.   

```  
- name: AZMON_COLLECT_ENV  
  value: "False"  
```  

AKS kapsayıcı değişikliği uygulamak için aşağıdaki komutu çalıştırın: `kubectl apply -f  <path to yaml file>`.

Yapılandırma değişikliğinin geçerli olduğunu doğrulamak için kapsayıcılar için Azure Izleyici 'de **kapsayıcılar** görünümünde bir kapsayıcı seçin ve özellik panelinde **ortam değişkenleri**' ni genişletin.  Bölüm yalnızca daha önce oluşturulan **AZMON_COLLECT_ENV = false**değişkenini göstermelidir. Diğer tüm kapsayıcılar için ortam değişkenlerini bölümünde bulunan tüm ortam değişkenlerini listelemelisiniz.   

Ortam değişkenlerinin bulunmasını yeniden etkinleştirmek için, aynı işlemi daha önce uygulayın ve değeri **false** iken **true**olarak değiştirin ve ardından kapsayıcıyı güncelleştirmek için `kubectl` komutunu yeniden çalıştırın.  

```  
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>Sonraki adımlar
Aracı yükseltme sırasında sorunlarla karşılaşırsanız, gözden [sorun giderme kılavuzu](container-insights-troubleshoot.md) desteği.
