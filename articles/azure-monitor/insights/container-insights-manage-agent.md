---
title: Kapsayıcılar Aracısı için Azure Izleyicisini yönetme | Microsoft Docs
description: Bu makalede, kapsayıcılar için Azure Izleyici tarafından kullanılan Kapsayıcılı Log Analytics aracısıyla en yaygın bakım görevlerinin yönetilmesi açıklanmaktadır.
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: ca0fa88cf27db15d45a2c855a1af351764c48fde
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84887509"
---
# <a name="how-to-manage-the-azure-monitor-for-containers-agent"></a>Kapsayıcılar aracısının Azure Izleyicisini yönetme

Kapsayıcılar için Azure Izleyici, Linux için Log Analytics aracısının kapsayıcılı bir sürümünü kullanır. İlk dağıtımdan sonra, yaşam döngüsü boyunca gerçekleştirmeniz gerekebilecek rutin veya isteğe bağlı görevler vardır. Bu makalede, aracıyı el ile yükseltme ve belirli bir kapsayıcıdan ortam değişkenlerinin toplanmasını devre dışı bırakma hakkında ayrıntılı bilgiler yer alır. 

## <a name="how-to-upgrade-the-azure-monitor-for-containers-agent"></a>Kapsayıcılar aracısının Azure Izleyicisini yükseltme

Kapsayıcılar için Azure Izleyici, Linux için Log Analytics aracısının kapsayıcılı bir sürümünü kullanır. Aracının yeni bir sürümü bırakıldığında, aracı Azure Kubernetes Service (AKS) ve Azure Red Hat OpenShift sürüm 3. x üzerinde barındırılan yönetilen Kubernetes kümelerinizde otomatik olarak yükseltilir. Karma bir [Kubernetes kümesi](container-insights-hybrid-setup.md) ve Azure Red Hat OpenShift sürüm 4. x için, aracı yönetilmez ve aracıyı el ile yükseltmeniz gerekir.

AKS veya Azure Red Hat OpenShift sürüm 3. x üzerinde barındırılan bir kümede aracı yükseltmesi başarısız olursa, bu makalede aracıyı el ile yükseltme işlemi de açıklanmaktadır. Yayınlanan sürümleri izlemek için bkz. [Aracı Sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

### <a name="upgrade-agent-on-aks-cluster"></a>AKS kümesinde aracıyı yükseltme

AKS kümelerinde aracıyı yükseltme işlemi iki düz ileri doğru adımdan oluşur. İlk adım, Azure CLı kullanan kapsayıcılar için Azure Izleyici ile izlemeyi devre dışı bırakmlarıdır. [Izlemeyi devre dışı bırakma](container-insights-optout.md?#azure-cli) makalesinde açıklanan adımları izleyin. Azure CLı kullanmak, çözümü ve çalışma alanında depolanan ilgili verileri etkilemeden aracıyı kümedeki düğümlerden kaldırmamızı sağlar. 

>[!NOTE]
>Bu bakım etkinliğini gerçekleştirirken, kümedeki düğümler toplanan verileri iletmez ve performans görünümleri aracıyı kaldırdığınız zamanla yeni sürümü yüklerken verileri göstermez. 
>

Aracının yeni sürümünü yüklemek için, bu işlemi gerçekleştirmek için [Azure CLI kullanarak izlemeyi etkinleştirme](container-insights-enable-new-cluster.md#enable-using-azure-cli)bölümünde açıklanan adımları izleyin.  

İzlemeyi yeniden etkinleştirdikten sonra, küme için güncelleştirilmiş sistem durumu ölçümlerini görüntüleyebilmeniz yaklaşık 15 dakika sürebilir. Aracının başarıyla yükseltildiğini doğrulamak için şu komutu çalıştırın:`kubectl logs omsagent-484hw --namespace=kube-system`

Bu durum, *OMI* ve *omsagent* değerinin [Aracı yayın geçmişinde](https://github.com/microsoft/docker-provider/tree/ci_feature_prod)belirtilen en son sürümle eşleşmesi gereken örneğe benzer.  

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

### <a name="upgrade-agent-on-hybrid-kubernetes-cluster"></a>Karma Kubernetes kümesinde aracıyı yükseltme

Üzerinde çalışan bir Kubernetes kümesinde aracıyı yükseltmek için aşağıdaki adımları gerçekleştirin:

* AKS altyapısı kullanılarak Azure üzerinde barındırılan, kendi kendine yönetilen Kubernetes kümeleri.
* AKS altyapısını kullanarak Azure Stack veya şirket içinde barındırılan, kendi kendine yönetilen Kubernetes kümeleri.
* Red Hat OpenShift sürüm 4. x.

Log Analytics çalışma alanı ticari Azure 'da ise aşağıdaki komutu çalıştırın:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<my_prod_cluster> incubator/azuremonitor-containers
```

Log Analytics çalışma alanı Azure Çin 21Vianet ' de ise aşağıdaki komutu çalıştırın:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.cn,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

Log Analytics çalışma alanı Azure ABD kamu 'da ise aşağıdaki komutu çalıştırın:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.us,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

### <a name="upgrade-agent-on-azure-red-hat-openshift-v4"></a>Azure Red Hat OpenShift v4 üzerinde aracıyı yükseltme

Azure Red Hat OpenShift sürüm 4. x üzerinde çalışan bir Kubernetes kümesinde aracıyı yükseltmek için aşağıdaki adımları gerçekleştirin. 

>[!NOTE]
>Azure Red Hat OpenShift sürüm 4. x yalnızca Azure ticari bulutunda çalışmayı destekler.
>

```
$ helm upgrade --name myrelease-1 \
--set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterId=<azureAroV4ResourceId> incubator/azuremonitor-containers
```

### <a name="upgrade-agent-on-azure-arc-enabled-kubernetes"></a>Azure Arc etkin Kubernetes üzerinde aracıyı yükseltme

Azure Arc etkin bir Kubernetes kümesindeki aracıyı proxy uç noktası olmadan yükseltmek için aşağıdaki komutu gerçekleştirin.

```
$ helm upgrade --install azmon-containers-release-1  –set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterId=<resourceIdOfAzureArcK8sCluster>
```

Bir proxy uç noktası belirtildiğinde aracıyı yükseltmek için aşağıdaki komutu gerçekleştirin. Proxy uç noktası hakkında daha fazla bilgi için bkz. [proxy uç noktasını yapılandırma](container-insights-enable-arc-enabled-clusters.md#configure-proxy-endpoint).

```
helm upgrade –name azmon-containers-release-1 –set omsagent.proxy=<proxyEndpoint>,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterId=<resourceIdOfAzureArcK8sCluster>
```

## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>Bir kapsayıcıda ortam değişkeni toplamayı devre dışı bırakma

Kapsayıcılar için Azure Izleyici, Pod içinde çalışan kapsayıcılardan ortam değişkenlerini toplar ve bunları **kapsayıcılar** görünümünde seçilen kapsayıcının Özellik bölmesinde sunar. Bu davranışı, Kubernetes kümesinin dağıtımı sırasında veya *AZMON_COLLECT_ENV*ortam değişkenini ayarlayarak, belirli bir kapsayıcı için koleksiyonu devre dışı bırakarak denetleyebilirsiniz. Bu özellik, aracı sürümü – ciprod11292018 ve üzeri sürümlerde kullanılabilir.  

Ortam değişkenlerinin koleksiyonunu yeni veya mevcut bir kapsayıcıda devre dışı bırakmak için, **AZMON_COLLECT_ENV** değişkenini Kubernetes Deployment YAML yapılandırma dosyanızda **false** değeriyle ayarlayın. 

```  
- name: AZMON_COLLECT_ENV  
  value: "False"  
```  

Değişikliği Azure Red Hat Openshıft dışındaki Kubernetes kümelerine uygulamak için aşağıdaki komutu çalıştırın: `kubectl apply -f  <path to yaml file>` . ConfigMap 'i düzenlemek ve bu değişikliği Azure Red Hat OpenShift kümeleri için uygulamak üzere şu komutu çalıştırın:

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

Bu, varsayılan metin düzenleyicinizi açar. Değişkeni ayarladıktan sonra dosyayı düzenleyicide kaydedin.

Yapılandırma değişikliğinin geçerli olduğunu doğrulamak için kapsayıcılar için Azure Izleyici 'de **kapsayıcılar** görünümünde bir kapsayıcı seçin ve özellik panelinde **ortam değişkenleri**' ni genişletin.  Bölüm yalnızca daha önce oluşturulan **AZMON_COLLECT_ENV = false**değişkenini göstermelidir. Diğer tüm kapsayıcılar için, ortam değişkenleri bölümü bulunan tüm ortam değişkenlerini listelemelidir.

Ortam değişkenlerinin bulunmasını yeniden etkinleştirmek için, aynı işlemi daha önce uygulayın ve değeri **false** iken **true**olarak değiştirin ve ardından `kubectl` kapsayıcıyı güncelleştirmek için komutu yeniden çalıştırın.  

```  
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>Sonraki adımlar

Aracıyı yükseltirken sorunlarla karşılaşırsanız, destek için [sorun giderme kılavuzunu](container-insights-troubleshoot.md) gözden geçirin.
