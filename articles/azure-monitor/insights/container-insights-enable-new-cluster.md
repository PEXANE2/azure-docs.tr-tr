---
title: Yeni bir Azure Kubernetes hizmeti (AKS) kümesini izleme | Microsoft Docs
description: Kapsayıcılar için Azure Izleyici aboneliğine sahip yeni bir Azure Kubernetes hizmeti (AKS) kümesi için izlemeyi nasıl etkinleştireceğinizi öğrenin.
ms.topic: conceptual
ms.date: 04/25/2019
ms.openlocfilehash: c731826f2780c45358730f9ce20d6a6151f6f259
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78385498"
---
# <a name="enable-monitoring-of-a-new-azure-kubernetes-service-aks-cluster"></a>Yeni bir Azure Kubernetes hizmeti (AKS) kümesini izlemeyi etkinleştir

Bu makalede, aboneliğinizden dağıtmaya hazırlanmakta olduğunuz [Azure Kubernetes hizmetinde](https://docs.microsoft.com/azure/aks/) barındırılan yönetilen Kubernetes kümesini izlemek için kapsayıcılar Için Azure izleyici 'nin nasıl ayarlanacağı açıklanır.

Desteklenen yöntemlerden birini kullanarak bir AKS kümesinin izlenmesini etkinleştirebilirsiniz:

* Azure CLI
* Terraform

## <a name="enable-using-azure-cli"></a>Azure CLI kullanarak etkinleştirin

Azure CLı ile oluşturulan yeni bir AKS kümesinin izlenmesini etkinleştirmek için, [aks kümesi oluşturma](../../aks/kubernetes-walkthrough.md#create-aks-cluster)bölümünde yer alarak hızlı başlangıç makalesindeki adımları izleyin.  

>[!NOTE]
>Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLı sürüm 2.0.74 veya üstünü çalıştırıyor olmanız gerekir. Sürümünüzü belirlemek için `az --version`çalıştırın. Azure CLı 'yi yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 'Yı yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli). Aks-Preview CLı uzantısı sürümü 0.4.12 veya üstünü yüklediyseniz, AKS Önizleme özellikleri Azure US Governmnet bulutu 'nda kullanılamadığından, önizleme uzantısını etkinleştirmek için yaptığınız tüm değişiklikleri kaldırın.

## <a name="enable-using-terraform"></a>Terraform kullanarak etkinleştirin

[Teresform kullanarak yeni bir AKS kümesi dağıtıyorsanız](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md), mevcut bir tane belirtmeyi seçmediyseniz [Log Analytics bir çalışma alanı oluşturmak için](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_workspace.html) profilde gereken bağımsız değişkenleri belirtirsiniz. 

>[!NOTE]
>Terraform kullanmayı seçerseniz, Terraform'u Azure RM sağlayıcısı sürümü 1.17.0 çalıştırmalıdır veya üzeri.

Çalışma alanına kapsayıcılar için Azure Izleyici eklemek için, [**addon_profile**](https://www.terraform.io/docs/providers/azurerm/r/kubernetes_cluster.html#addon_profile) dahil ederek [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) ve **oms_agent**belirterek profili doldurun. 

İzleme etkin ve tüm yapılandırma görevleri başarıyla tamamlandıktan sonra iki yöntemden biriyle kümenizin performansı izleyebilirsiniz:

* Sol bölmedeki **sistem durumu** ' nu seçerek doğrudan aks kümesinde.
* Seçili küme için AKS kümesi sayfasında bulunan **kapsayıcı öngörülerini izle** kutucuğunu seçin. Azure Izleyici 'de, sol bölmede **sistem durumu**' nu seçin. 

  ![AKS, kapsayıcılar için Azure İzleyici seçmek için seçenekleri](./media/container-insights-onboard/kubernetes-select-monitoring-01.png)

İzleme etkinleştirdikten sonra küme için sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. 

## <a name="verify-agent-and-solution-deployment"></a>Aracı ve çözüm dağıtımı doğrulama
Aracı sürümü *06072018* veya sonraki sürümlerde, hem aracının hem de çözümün başarıyla dağıtıldığını doğrulayabilirsiniz. Aracıyı önceki sürümleriyle birlikte, yalnızca aracı dağıtımı doğrulayabilirsiniz.

### <a name="agent-version-06072018-or-later"></a>Aracı 06072018 veya sonraki bir sürümü
Aracı başarıyla dağıtıldığını doğrulamak için aşağıdaki komutu çalıştırın. 

```
kubectl get ds omsagent --namespace=kube-system
```

Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Çözümün dağıtımı doğrulamak için aşağıdaki komutu çalıştırın:

```
kubectl get deployment omsagent-rs -n=kube-system
```

Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Aracı sürümü 06072018 öncesi

*06072018* ' den önce yayınlanan Log Analytics Agent sürümünün düzgün şekilde dağıtılmadığını doğrulamak için şu komutu çalıştırın:  

```
kubectl get ds omsagent --namespace=kube-system
```

Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>CLI ile yapılandırmayı görüntüle
Çözüm etkin değil veya Not, Log Analytics çalışma alanı RESOURCEID nedir ve kümeyle ilgili Özet ayrıntılar gibi ayrıntıları almak için `aks show` komutunu kullanın.  

```azurecli
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
```

Birkaç dakika sonra komut tamamlanır ve JSON biçimli çözümü hakkında bilgi döndürür.  Komutun sonuçlarını izleme eklenti profilini göstermesi gerekir ve aşağıdaki örnek çıktıya benzer:

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```

## <a name="next-steps"></a>Sonraki adımlar

* Çözümü oluşturmaya çalışırken sorunlarla karşılaşırsanız, [sorun giderme kılavuzunu](container-insights-troubleshoot.md) gözden geçirin

* İzleme etkinken, AKS kümeniz ve üzerinde çalışan iş yüklerinizin sistem durumunu ve kaynak kullanımını toplayıp, kapsayıcılar için Azure Izleyicisini [nasıl kullanacağınızı](container-insights-analyze.md) öğrenin.
