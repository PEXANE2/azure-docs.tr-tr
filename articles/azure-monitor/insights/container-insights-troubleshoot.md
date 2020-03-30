---
title: Kapsayıcılar için Azure Monitör'ün Sorun Giderme Sorunu | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor ile sorunları nasıl giderebileceğiniz ve çözebileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 17a2817b320599b2aa2c331c354d316b9d864a32
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75403375"
---
# <a name="troubleshooting-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitörü Sorun Giderme

Azure Kubernetes Hizmet (AKS) kümenizin denetimini kapsayıcılar için Azure Monitor ile yapılandırdığınızda, veri toplamayı veya raporlama durumunu engelleyen bir sorunla karşılaşabilirsiniz. Bu makalede, bazı sık karşılaşılan sorunlar ve sorun giderme adımları ayrıntıları.

## <a name="authorization-error-during-onboarding-or-update-operation"></a>Onboarding veya güncelleştirme işlemi sırasında yetkilendirme hatası
Kapsayıcılar için Azure Monitor'u etkinleştirirken veya bir kümeyi ölçüm toplamayı desteklemek üzere güncellerken, aşağıdakilere benzer bir hata alabilirsiniz - *Kullanıcı <kimlik>' nesne kimliği ile '<kullanıcının objectId>' kapsamında 'Microsoft.Authorization/roleAssignments/write' eylemini gerçekleştirme yetkisi yoktur*

Onboarding veya güncelleştirme işlemi sırasında, Küme kaynağıüzerinde **İzleme Ölçümleri Yayımcısı** rol ataması verilir. Kapsayıcılar için Azure Monitor'u etkinleştirmek için işlemi başlatan veya ölçümlerin toplanmasını desteklemek için güncelleştirmeyi başlatan kullanıcının, AKS küme kaynak kapsamında **Microsoft.Authorization/roleAssignments/write** iznine erişimi olmalıdır. Yalnızca **Sahip** ve **Kullanıcı Erişimi Yöneticisi** yerleşik rolleri üyelerine bu izne erişim izni verilir. Güvenlik ilkeleriniz parçalı düzey izinleri atamasını gerektiriyorsa, [özel rolleri](../../role-based-access-control/custom-roles.md) görüntülemenizi ve bunu isteyen kullanıcılara atamanızı öneririz. 

Ayrıca, aşağıdaki adımları gerçekleştirerek bu rolü Azure portalından el ile verebilirsiniz:

1. [Azure portalında](https://portal.azure.com)oturum açın. 
2. Azure portalının sol alt köşesinde bulunan **Tüm hizmetler**’e tıklayın. Kaynaklar listesinde **Kubernetes**yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Azure Kubernetes'i**seçin.
3. Kubernetes kümeleri listesinde, listeden birini seçin.
2. Sol menüden **Access denetimi'ni (IAM)** tıklatın.
3. Rol ataması eklemek için **+ Ekle'yi** seçin ve abonelikte tanımlanan kümeler hizmet ilkelerindeki sonuçları filtrelemek için **İzleme Ölçümleri Yayımcısı** rolünü ve **Select** kutusu türü **AKS'nin** altında n'yi seçin. Bu kümeye özgü listeden birini seçin.
4. Rolü atamayı bitirmek için **Kaydet'i** seçin. 

## <a name="azure-monitor-for-containers-is-enabled-but-not-reporting-any-information"></a>Kapsayıcılar için Azure Monitörü etkindir, ancak herhangi bir bilgi bildirmiyor
Kapsayıcılar için Azure Monitor başarıyla etkinleştirilmiş ve yapılandırılan, ancak durum bilgilerini görüntüleyemiyorsanız veya günlük sorgusundan hiçbir sonuç döndürülemiyorsa, aşağıdaki adımları izleyerek sorunu tanılarsınız: 

1. Komutu çalıştırarak aracının durumunu kontrol edin: 

    `kubectl get ds omsagent --namespace=kube-system`

    Çıktı, düzgün dağıtıldığını gösteren aşağıdakilere benzemelidir:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Aracı sürümü *06072018* veya daha sonra komutu kullanarak dağıtım durumunu kontrol edin:

    `kubectl get deployment omsagent-rs -n=kube-system`

    Çıktı, düzgün dağıtıldığını gösteren aşağıdaki örneğe benzemelidir:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Komutu kullanarak çalıştığını doğrulamak için bölmenin durumunu denetleyin:`kubectl get pods --namespace=kube-system`

    Çıktı, omsagent için *Çalışan* durumu ile aşağıdaki örnek benzer olmalıdır:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Aracı kayıtlarını kontrol et. Kapsayıcı aracı dağıtıldığında, OMI komutlarını çalıştırarak hızlı bir denetim çalıştırır ve aracı ve sağlayıcının sürümünü görüntüler. 

5. Aracının başarıyla dağıtıldığını doğrulamak için komutu çalıştırın:`kubectl logs omsagent-484hw --namespace=kube-system`

    Durum aşağıdaki örneğe benzemelidir:

    ```
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
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="error-messages"></a>Hata iletileri

Aşağıdaki tablo, kapsayıcılar için Azure Monitor kullanırken karşılaşabileceğiniz bilinen hataları özetleyebilir.

| Hata iletileri  | Eylem |  
| ---- | --- |  
| Hata İletisi`No data for selected filters`  | Yeni oluşturulan kümelerin veri akışını izlemek için bir süre beklemeniz gerekebilir. Kümeniz için verilerin görünmesi için en az 10 ila 15 dakika bekleyin. |   
| Hata İletisi`Error retrieving data` | Azure Kubernetes Hizmet kümesi sistem durumu ve performans izleme için ayarlanırken, küme ile Azure Log Analytics çalışma alanı arasında bir bağlantı kurulur. Log Analytics çalışma alanı, kümeniz için tüm izleme verilerini depolamak için kullanılır. Bu hata, Günlük Analizi çalışma alanınız silindiğinde oluşabilir. Çalışma alanının silinip silinip silinmediğinden ve silinip silinmeyeceğini kontrol edin, kapsayıcılar için Azure Monitor ile kümenizin izlenmesini yeniden etkinleştirmeniz ve varolan bir çalışma alanı belirtmeniz veya yeni bir çalışma alanı oluşturmanız gerekir. Yeniden etkinleştirmek için kümenin izlemeyi [devre dışı bırakıp](container-insights-optout.md) kapsayıcılar için Azure [Monitor'u](container-insights-enable-new-cluster.md) yeniden etkinleştirmeniz gerekir. |  
| `Error retrieving data`az aks cli aracılığıyla kapsayıcılar için Azure Monitor ekledikten sonra | İzlemeyi etkinleştirildiğinde, `az aks cli`kapsayıcılar için Azure Monitörü düzgün dağıtılamayabilir. Çözümün dağıtılıp dağıtılmadığını denetleyin. Bunu yapmak için Log Analytics çalışma alanınıza gidin ve sol taraftaki bölmeden **Çözümler** seçilerek çözümün kullanılabilir olup olmadığını görün. Bu sorunu gidermek [için, kapsayıcılar için Azure Monitörü'nün nasıl dağıtılanacacağına](container-insights-onboard.md) ilişkin yönergeleri izleyerek çözümü yeniden dağıtmanız gerekir |  

Sorunu tanılamaya yardımcı olmak [için, burada](https://github.com/Microsoft/OMS-docker/tree/ci_feature_prod/Troubleshoot#troubleshooting-script)kullanılabilen bir sorun giderme komut dosyası sağladık.

## <a name="azure-monitor-for-containers-agent-replicaset-pods-are-not-scheduled-on-non-azure-kubernetes-cluster"></a>Kapsayıcılar için Azure Monitörü aracısı ReplicaSet Pods, Azure Olmayan Kubernetes kümesinde zamanlanmıyor

Kapsayıcılar aracısı ReplicaSet Pods için Azure Monitor, zamanlama için alt (veya aracı) düğümlerinde aşağıdaki düğüm seçicilerine bağımlıdır:

```
nodeSelector:
  beta.kubernetes.io/os: Linux
  kubernetes.io/role: agent
```

İşçi düğümlerinizde düğüm etiketleri yoksa, aracı ReplicaSet Pods zamanlanmaz. Kubernetes'e bakın, etiketin nasıl eklendiğinizle ilgili talimatlar için [etiket seçicileri atayın.](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)

## <a name="performance-charts-dont-show-cpu-or-memory-of-nodes-and-containers-on-a-non-azure-cluster"></a>Performans grafikleri, Azure olmayan bir kümede CPU veya düğüm lerin ve kapsayıcıların belleği göstermez

Kapsayıcılar için Azure Monitor aracısı Pods, performans ölçümlerini toplamak için düğüm aracısı üzerindeki cAdvisor bitiş noktasını kullanır. Performans ölçümlerini toplamak için kümedeki tüm `cAdvisor port: 10255` düğümlerde açılmasına izin verecek şekilde yapılandırılan düğümdeki kapsayıcı aracının doğrulandığını doğrulayın.

## <a name="non-azure-kubernetes-cluster-are-not-showing-in-azure-monitor-for-containers"></a>Azure Olmayan Kubernetes kümesi kapsayıcılar için Azure Monitörü'nde görünmüyor

Kapsayıcılar için Azure Monitor'da Azure Olmayan Kubernetes kümesini görüntülemek için, Bu Öngörüyü destekleyen Log Analytics çalışma alanında ve Container Insights çözüm kaynağı **ContainerInsights'da *(çalışma alanı)*** Azure Kubernetes olmayan kümeyi görüntülemek için erişim gereklidir.

## <a name="next-steps"></a>Sonraki adımlar

Hem AKS küme düğümleri hem de bölmeler için sistem durumu ölçümlerini yakalama özelliğine sahip izleme sayesinde, bu sistem durumu ölçümleri Azure portalında kullanılabilir. Kapsayıcılar için Azure Monitörünü nasıl kullanacağınızı öğrenmek için [bkz.](container-insights-analyze.md)
