---
title: Yeni bir Azure Kubernetes Hizmeti (AKS) kümesini izleyin | Microsoft Dokümanlar
description: Kapsayıcı aboneliği için Azure Monitor ile yeni bir Azure Kubernetes Hizmeti (AKS) kümesiiçin izlemeyi nasıl etkinleştirebilirsiniz öğrenin.
ms.topic: conceptual
ms.date: 04/25/2019
ms.openlocfilehash: c731826f2780c45358730f9ce20d6a6151f6f259
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275444"
---
# <a name="enable-monitoring-of-a-new-azure-kubernetes-service-aks-cluster"></a>Yeni bir Azure Kubernetes Hizmeti (AKS) kümesinin izlenmesini etkinleştirin

Bu makalede, aboneliğinizde dağıtmaya hazırlandığınız [Azure Kubernetes Hizmetinde](https://docs.microsoft.com/azure/aks/) barındırılan yönetilen Kubernetes kümesini izlemek için kapsayıcılar için Azure Monitörü nasıl ayarlanacağı açıklanmaktadır.

Desteklenen yöntemlerden birini kullanarak bir AKS kümesinin izlenmesini etkinleştirebilirsiniz:

* Azure CLI
* Terraform

## <a name="enable-using-azure-cli"></a>Azure CLI'yi kullanarak etkinleştirme

Azure CLI ile oluşturulan yeni bir AKS kümesinin izlenmesini etkinleştirmek için, [AKS küme oluştur](../../aks/kubernetes-walkthrough.md#create-aks-cluster)bölümünün altındaki hızlı başlangıç makalesindeki adımı izleyin.  

>[!NOTE]
>Azure CLI'yi kullanmayı seçerseniz, öncelikle CLI'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI sürümünü 2.0.74 veya sonraki sürümde çalışıyor olmalısınız. Sürümünüzü tanımlamak için `az --version`çalıştırın. Azure CLI'yi yüklemeniz veya yükseltmeniz gerekiyorsa, [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli) AKS Preview cli uzantısı sürümünü 0.4.12 veya daha yüksek bir şekilde yüklediyseniz, AKS Preview özellikleri Azure US Governmnet bulutunda kullanılamadığıiçin varsayılan Azure CLI davranışını geçersiz kılabilir ken, önizleme uzantısını etkinleştirmek için yaptığınız değişiklikleri kaldırın.

## <a name="enable-using-terraform"></a>Terraform kullanarak etkinleştirme

[Terraform kullanarak yeni bir AKS kümesi dağıtıyorsanız,](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)varolan bir küme belirtmeyi seçmezseniz, bir [Log Analytics çalışma alanı oluşturmak için](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_workspace.html) profilde gereken bağımsız değişkenleri belirtirsiniz. 

>[!NOTE]
>Terraform'u kullanmayı seçerseniz, Terraform Azure RM Sağlayıcısı sürümünü 1.17.0 veya üzeri olarak çalıştırıyor olmalısınız.

Çalışma alanına kapsayıcılar için Azure Monitörü eklemek [için, azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) bakın ve [**addon_profile**](https://www.terraform.io/docs/providers/azurerm/r/kubernetes_cluster.html#addon_profile) ekleyerek profili tamamlayın ve **oms_agent**belirtin. 

İzlemeyi etkinleştirdikten ve tüm yapılandırma görevleri başarıyla tamamlandıktan sonra kümenizin performansını iki şekilde izleyebilirsiniz:

* Doğrudan AKS kümesinde sol bölmede **Sağlık** seçerek.
* Seçili küme için AKS küme sayfasındaki **Monitör Kapsayıcısı istatistiklerini** seçerek. Azure Monitor'da, sol bölmede **Sistem Durumu'nu**seçin. 

  ![AKS'deki kapsayıcılar için Azure Monitörünü seçme seçenekleri](./media/container-insights-onboard/kubernetes-select-monitoring-01.png)

İzlemeyi etkinleştirdikten sonra, kümenin sistem durumu ölçümlerini görüntülemeniz yaklaşık 15 dakika sürebilir. 

## <a name="verify-agent-and-solution-deployment"></a>Aracıyı ve çözüm dağıtımını doğrula
Aracı sürümü *06072018* veya daha sonra, hem aracının hem de çözümün başarıyla dağıtıldığını doğrulayabilirsiniz. Aracının önceki sürümlerinde yalnızca aracı dağıtımını doğrulayabilirsiniz.

### <a name="agent-version-06072018-or-later"></a>Aracı sürümü 06072018 veya sonrası
Aracının başarıyla dağıtılmış olduğunu doğrulamak için aşağıdaki komutu çalıştırın. 

```
kubectl get ds omsagent --namespace=kube-system
```

Çıktı, düzgün dağıtıldığını gösteren aşağıdakilere benzemelidir:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Çözümün dağıtımını doğrulamak için aşağıdaki komutu çalıştırın:

```
kubectl get deployment omsagent-rs -n=kube-system
```

Çıktı, düzgün dağıtıldığını gösteren aşağıdakilere benzemelidir:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>06072018'den önceki aracı sürümü

*06072018'den* önce yayımlanan Log Analytics aracısı sürümünün düzgün bir şekilde dağıtılmış olduğunu doğrulamak için aşağıdaki komutu çalıştırın:  

```
kubectl get ds omsagent --namespace=kube-system
```

Çıktı, düzgün dağıtıldığını gösteren aşağıdakilere benzemelidir:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>CLI ile yapılandırmayı görüntüleme
Çözüm `aks show` etkin olup olmadığı, Log Analytics çalışma alanı kaynağı kimliği ve küme yle ilgili özet ayrıntıları gibi ayrıntıları almak için komutu kullanın.  

```azurecli
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
```

Birkaç dakika sonra komut tamamlar ve çözüm hakkında JSON biçimlendirilmiş bilgileri döndürür.  Komutun sonuçları izleme eklentiprofilini göstermeli ve aşağıdaki örnek çıktıya benzer:

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

* Çözüme binmeye çalışırken sorunlarla karşılaşırsanız, [sorun giderme kılavuzunu](container-insights-troubleshoot.md) gözden geçirin

* AKS kümenizin sistem durumu ve kaynak kullanımını ve üzerlerinde çalışan iş yüklerini toplamak için etkinleştirilen izleme sayesinde, kapsayıcılar için Azure Monitor'u [nasıl kullanacağınızı](container-insights-analyze.md) öğrenin.
