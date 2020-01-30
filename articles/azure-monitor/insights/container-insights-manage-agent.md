---
title: Azure İzleyici kapsayıcı Aracısı'nı yönetme | Microsoft Docs
description: Bu makalede, kapsayıcılar için Azure İzleyici tarafından kullanılan kapsayıcılı Log Analytics aracısını ile en sık kullanılan bakım görevlerini yönetmek açıklar.
ms.topic: conceptual
ms.date: 01/24/2020
ms.openlocfilehash: 1a1f8d690979a846dbf5041999180221752acc0b
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843965"
---
# <a name="how-to-manage-the-azure-monitor-for-containers-agent"></a>Azure İzleyici kapsayıcı Aracısı'nı yönetme

Kapsayıcılar için Azure İzleyici, Linux için Log Analytics aracısını kapsayıcı bir sürümünü kullanır. İlk dağıtımdan sonra yordamı veya yaşam döngüsü sırasında gerçekleştirmeniz gerekebilir isteğe bağlı görevleri vardır. Bu makale hakkında ayrıntılı bilgi el ile aracı yükseltme ve belirli bir kapsayıcıda ortam değişkenlerinin koleksiyonunu devre dışı bırakın. 

## <a name="how-to-upgrade-the-azure-monitor-for-containers-agent"></a>Azure İzleyici kapsayıcı Aracısı yükseltme

Kapsayıcılar için Azure İzleyici, Linux için Log Analytics aracısını kapsayıcı bir sürümünü kullanır. Aracının yeni bir sürümü bırakıldığında, aracı Azure Kubernetes hizmeti (AKS) ve Azure Red Hat OpenShift üzerinde barındırılan yönetilen Kubernetes kümelerinizde otomatik olarak yükseltilir. Karma bir [Kubernetes kümesi](container-insights-hybrid-setup.md) için aracı yönetilmez ve aracıyı el ile yükseltmeniz gerekir.

AKS üzerinde barındırılan bir küme için aracı yükseltmesi başarısız olursa, bu makale aracıyı el ile yükseltme işlemini de açıklar. Yayımlanan sürümleri takip etmek için bkz: [Aracı sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

### <a name="upgrade-agent-on-monitored-kubernetes-cluster"></a>İzlenen Kubernetes kümesinde aracıyı yükselt

Azure Red Hat OpenShift dışındaki kümeler üzerinde aracıyı yükseltme işlemi, iki düz ileri doğru adımdan oluşur. İlk adım, Azure İzleyici ile Azure CLI kullanarak kapsayıcıları için izleme devre dışı bırakmaktır. İçinde açıklanan adımları izleyin [izlemeyi devre dışı](container-insights-optout.md?#azure-cli) makalesi. Azure CLI kullanarak çözüm ve çalışma alanında depolanan karşılık gelen verileri etkilemeden Kümedeki düğümlerden aracıyı kaldırmak sağlıyor. 

>[!NOTE]
>Bu bakım etkinliği gerçekleştiriyorsanız, kümedeki düğümler, toplanan verileri ilettiğiniz değil ve performans görünümlerine veri gösterilmez arasındaki zaman aracıyı kaldırın ve ardından yeni sürümü yükleyin. 
>

Aracının yeni sürümünü yüklemek için, bu işlemi gerçekleştirmek için [Azure CLI kullanarak izlemeyi etkinleştirme](container-insights-enable-new-cluster.md#enable-using-azure-cli)bölümünde açıklanan adımları izleyin.  

İzlemeyi yeniden etkinleştirdikten sonra, küme için güncelleştirilmiş sistem durumu ölçümlerini görüntüleyebilmeniz yaklaşık 15 dakika sürebilir. Aracı başarıyla yükseltildi doğrulamak için komutu çalıştırın: `kubectl logs omsagent-484hw --namespace=kube-system`

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

## <a name="upgrade-agent-on-hybrid-kubernetes-cluster"></a>Karma Kubernetes kümesinde aracıyı yükseltme

Şirket içinde barındırılan bir Kubernetes kümesinde aracıyı yükseltme işlemi, Azure 'da AKS motoru ve Azure Stack aşağıdaki komut çalıştırılarak tamamlanabilir:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<my_prod_cluster> incubator/azuremonitor-containers
```

Log Analytics çalışma alanı Azure Çin 'de ise aşağıdaki komutu çalıştırın:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.cn,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

Log Analytics çalışma alanı Azure ABD kamu 'da ise aşağıdaki komutu çalıştırın:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.us,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>Bir kapsayıcı ortam değişkeni koleksiyonunu devre dışı bırakma

Kapsayıcılar için Azure İzleyici, bir pod içinde çalışan kapsayıcılar ortam değişkenlerini toplar ve bunları seçilen kapsayıcı içindeki özellik bölmesinde sunar **kapsayıcıları** görünümü. Bu davranışı, Kubernetes kümesinin dağıtımı sırasında veya *AZMON_COLLECT_ENV*ortam değişkenini ayarlayarak, belirli bir kapsayıcı için koleksiyonu devre dışı bırakarak denetleyebilirsiniz. Bu özellik, aracı sürümü – ciprod11292018 kullanılabilir ve daha yüksek.  

Ortam değişkenlerinin koleksiyonunu yeni veya mevcut bir kapsayıcıda devre dışı bırakmak için, **AZMON_COLLECT_ENV** değişkenini Kubernetes Deployment YAML yapılandırma dosyanızda **false** değeriyle ayarlayın. 

```  
- name: AZMON_COLLECT_ENV  
  value: "False"  
```  

Değişikliği Azure Red Hat Openshıft dışındaki Kubernetes kümelerine uygulamak için aşağıdaki komutu çalıştırın: `kubectl apply -f  <path to yaml file>`. ConfigMap 'i düzenlemek ve bu değişikliği Azure Red Hat OpenShift kümeleri için uygulamak üzere şu komutu çalıştırın:

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

Bu, varsayılan metin düzenleyicinizi açar. Değişkeni ayarladıktan sonra dosyayı düzenleyicide kaydedin.

Yapılandırma değişikliğinin geçerli olduğunu doğrulamak için kapsayıcılar için Azure Izleyici 'de **kapsayıcılar** görünümünde bir kapsayıcı seçin ve özellik panelinde **ortam değişkenleri**' ni genişletin.  Bölüm yalnızca daha önce oluşturulan **AZMON_COLLECT_ENV = false**değişkenini göstermelidir. Diğer tüm kapsayıcılar için ortam değişkenlerini bölümünde bulunan tüm ortam değişkenlerini listelemelisiniz.

Ortam değişkenlerinin bulunmasını yeniden etkinleştirmek için, aynı işlemi daha önce uygulayın ve değeri **false** iken **true**olarak değiştirin ve ardından kapsayıcıyı güncelleştirmek için `kubectl` komutunu yeniden çalıştırın.  

```  
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>Sonraki adımlar

Aracı yükseltme sırasında sorunlarla karşılaşırsanız, gözden [sorun giderme kılavuzu](container-insights-troubleshoot.md) desteği.
