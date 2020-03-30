---
title: Kapsayıcılar aracısı için Azure Monitörü nasıl yönetilir | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor tarafından kullanılan kapsayıcılaştırılmış Log Analytics aracısıyla en yaygın bakım görevlerinin yönetilmesi açıklanmaktadır.
ms.topic: conceptual
ms.date: 01/24/2020
ms.openlocfilehash: 1a1f8d690979a846dbf5041999180221752acc0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275327"
---
# <a name="how-to-manage-the-azure-monitor-for-containers-agent"></a>Kapsayıcılar aracısı için Azure Monitörü nasıl yönetilir?

Kapsayıcılar için Azure Monitor, Linux için Log Analytics aracısının kapsayıcılaştırılmış bir sürümünü kullanır. İlk dağıtımdan sonra, yaşam döngüsü sırasında gerçekleştirmeniz gereken rutin veya isteğe bağlı görevler vardır. Bu makalede, aracının el ile yükseltilen ve belirli bir kapsayıcıdan çevresel değişkenlerin toplama devre dışı nasıl ayrıntıları. 

## <a name="how-to-upgrade-the-azure-monitor-for-containers-agent"></a>Kapsayıcılar aracısı için Azure Monitörü nasıl yükseltilir?

Kapsayıcılar için Azure Monitor, Linux için Log Analytics aracısının kapsayıcılaştırılmış bir sürümünü kullanır. Aracının yeni bir sürümü yayımlandığında, aracı, Azure Kubernetes Hizmeti (AKS) ve Azure Red Hat OpenShift'te barındırılan yönetilen Kubernetes kümelerinizde otomatik olarak yükseltilir. Karma bir [Kubernetes kümesi](container-insights-hybrid-setup.md) için aracı yönetilmez ve aracıyı el ile yükseltmeniz gerekir.

Aracı yükseltme aks barındırılan bir küme için başarısız olursa, bu makalede aracıyı el ile yükseltme işlemi de açıklanır. Yayımlanan sürümleri takip etmek [için, aracı sürüm duyurularına](https://github.com/microsoft/docker-provider/tree/ci_feature_prod)bakın.

### <a name="upgrade-agent-on-monitored-kubernetes-cluster"></a>İzlenen Kubernetes kümesinde yükseltme aracısı

Azure Red Hat OpenShift dışındaki kümeler üzerindeki aracıyı yükseltme işlemi, iki düz ileri adımdan oluşur. İlk adım, Azure CLI kullanan kapsayıcılar için Azure Monitor ile izlemeyi devre dışı katmaktır. [İzlemeyi Devre Dışı Bırak](container-insights-optout.md?#azure-cli) makalesinde açıklanan adımları izleyin. Azure CLI'yi kullanmak, çözüme ve çalışma alanında depolanan ilgili verileri etkilemeden aracıyı kümedeki düğümlerden kaldırmamıza olanak tanır. 

>[!NOTE]
>Bu bakım etkinliğini gerçekleştirirken, kümedeki düğümler toplanan verileri iletmiyor ve performans görünümleri aracıyı kaldırdığınızda ve yeni sürümü yüklediğiniz zaman arasında veri göstermez. 
>

Aracının yeni sürümünü yüklemek için, bu işlemi tamamlamak için [Azure CLI kullanarak izlemeyi etkinleştirme](container-insights-enable-new-cluster.md#enable-using-azure-cli)de açıklanan adımları izleyin.  

İzlemeyi yeniden etkinleştirdikten sonra, kümeiçin güncelleştirilmiş sistem durumu ölçümlerini görüntülemeniz yaklaşık 15 dakika sürebilir. Aracının başarıyla yükseltileni doğrulamak için komutu çalıştırın:`kubectl logs omsagent-484hw --namespace=kube-system`

Durum, *omi* ve *omsagent* değerinin [aracı sürüm geçmişinde](https://github.com/microsoft/docker-provider/tree/ci_feature_prod)belirtilen en son sürümle eşleşmesi gereken aşağıdaki örneğe benzemelidir.  

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

## <a name="upgrade-agent-on-hybrid-kubernetes-cluster"></a>Hibrit Kubernetes kümesinde yükseltme aracısı

Şirket içinde barındırılan bir Kubernetes kümesindeki aracıyı yükseltme işlemi, Azure'daki AKS Engine ve Azure Yığını aşağıdaki komutu çalıştırarak tamamlanabilir:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<my_prod_cluster> incubator/azuremonitor-containers
```

Log Analytics çalışma alanı Azure Çin'deyse aşağıdaki komutu çalıştırın:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.cn,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

Log Analytics çalışma alanı Azure ABD Hükümeti'ndeyse aşağıdaki komutu çalıştırın:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.us,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>Bir kapsayıcıda ortam değişkeni koleksiyonunu devre dışı etme

Kapsayıcılar için Azure Monitörü, bir bölmede çalışan kapsayıcılardan çevresel değişkenleri toplar ve **bunları Kapsayıcılar** görünümünde seçili kapsayıcının özellik bölmesinde sunar. Bu davranışı, Kubernetes kümesinin dağıtımı sırasında veya ortam değişkenini AZMON_COLLECT_ENV ayarladıktan sonra *AZMON_COLLECT_ENV*belirli bir kapsayıcı için koleksiyonu devre dışı bırakarak denetleyebilirsiniz. Bu özellik ajan sürümünden temin edilebilir – ciprod11292018 ve daha yüksek.  

Yeni veya varolan bir kapsayıcıda çevresel değişkenlerin toplanmasını devre dışı düşürmek için, Kubernetes dağıtım yaml yapılandırma dosyanızda **False** değeriyle değişken **AZMON_COLLECT_ENV** ayarlayın. 

```  
- name: AZMON_COLLECT_ENV  
  value: "False"  
```  

Değişikliği Azure Red Hat OpenShift dışındaki Kubernetes kümelerine uygulamak `kubectl apply -f  <path to yaml file>`için aşağıdaki komutu çalıştırın: . ConfigMap'i yeniden oluşturmak ve bu değişikliği Azure Red Hat OpenShift kümeleri için uygulamak için aşağıdaki komutu çalıştırın:

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

Bu, varsayılan metin düzenleyicinizi açar. Değişkeni ayarladıktan sonra dosyayı düzenleyiciye kaydedin.

Yapılandırma değişikliğinin etkili olduğunu doğrulamak için, kapsayıcılar için Azure Monitor'da **Kapsayıcılar** görünümünde ve özellik panelinde **Çevre Değişkenleri'ni**genişleten bir kapsayıcı seçin.  Bölüm sadece daha önce oluşturulan değişkeni göstermelidir - **AZMON_COLLECT_ENV=FALSE**. Diğer tüm kapsayıcılar için Çevre Değişkenleri bölümü keşfedilen tüm ortam değişkenlerini listelemelidir.

Çevresel değişkenlerin keşfini yeniden etkinleştirmek için, aynı işlemi daha önce uygulayın ve değeri `kubectl` **False'dan** **True'ya**değiştirin ve ardından kapsayıcıyı güncelleştirmek için komutu yeniden çalıştırın.  

```  
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>Sonraki adımlar

Aracıyı yükseltirken sorunlarla karşılaşırsanız, destek için [sorun giderme kılavuzunu](container-insights-troubleshoot.md) inceleyin.
