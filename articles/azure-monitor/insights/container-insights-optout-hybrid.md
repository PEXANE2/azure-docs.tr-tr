---
title: Karma Kubernetes kümenizi izlemeyi durdurma | Microsoft Docs
description: Bu makalede, kapsayıcı için Azure Izleyici ile karma Kubernetes kümenizi izlemeyi nasıl durdurulabileceğinizi açıklanmaktadır.
ms.topic: conceptual
ms.date: 06/16/2020
ms.openlocfilehash: 86a774737d5269d77c4053ad61ab870b13288aa7
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84885864"
---
# <a name="how-to-stop-monitoring-your-hybrid-cluster"></a>Karma kümenizi izlemeyi durdurma

Kubernetes kümenizi izlemeyi etkinleştirdikten sonra, artık izlemek istememeye karar verirseniz, kapsayıcıyı kapsayıcılar için Azure Izleyici ile izlemeyi durdurabilirsiniz. Bu makalede, aşağıdaki ortamlar için bunun nasıl yapılacağı gösterilmektedir:

- Azure ve Azure Stack AKS altyapısı
- OpenShift sürüm 4 ve üzeri
- Azure Arc özellikli Kubernetes (önizleme)

## <a name="how-to-stop-monitoring-using-helm"></a>Held kullanarak izlemeyi durdurma

Aşağıdaki adımlar aşağıdaki ortamlar için geçerlidir:

- Azure ve Azure Stack AKS altyapısı
- OpenShift sürüm 4 ve üzeri

1. Kümenizde yüklü olan kapsayıcıların Helm grafik sürümünün Azure Izleyicisini belirlemek için aşağıdaki Helm komutunu çalıştırın.

    ```
    helm list
    ```

    Çıktı aşağıdakine benzeyecektir:

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    azmon-containers-release-1      default         3               2020-04-21 15:27:24.1201959 -0700 PDT   deployed        azuremonitor-containers-2.7.0   7.0.0-1
    ```

    *Azmon-kapsayıcılar-Release-1* kapsayıcılar Için Azure izleyici için Helm grafik sürümünü temsil eder.

2. Grafik sürümünü silmek için aşağıdaki Held komutunu çalıştırın.

    `helm delete <releaseName>`

    Örnek:

    `helm delete azmon-containers-release-1`

    Bu işlem, yayını kümeden kaldırır. Komutunu çalıştırarak doğrulayabilirsiniz `helm list` :

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    ```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Held, sildikten sonra bile yayınlarınızı izliyorsa, bir kümenin geçmişini denetleyebilir ve hatta ile bir yayını geri alabilirsiniz `helm rollback` .

## <a name="how-to-stop-monitoring-on-arc-enabled-kubernetes"></a>Yay etkin Kubernetes üzerinde izlemeyi durdurma

### <a name="using-powershell"></a>PowerShell’i kullanma

1. Aşağıdaki komutları kullanarak betiği, izleme eklentisi ile kümenizi yapılandıran yerel bir klasöre yükleyin ve kaydedin:

    ```powershell
    wget https://aka.ms/disable-monitoring-powershell-script -OutFile disable-monitoring.ps1
    ```

2. `$azureArcClusterResourceId`İçin karşılık gelen değerleri ayarlayarak `subscriptionId` `resourceGroupName` ve `clusterName` Azure Arc özellikli Kubernetes küme kaynağınızın kaynak kimliğini temsil ederek değişkeni yapılandırın.

    ```powershell
    $azureArcClusterResourceId = "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
    ```

3. `$kubeContext`Komutunu çalıştırarak değişkeni, kümenizin **kuin bağlamı** ile yapılandırın `kubectl config get-contexts` . Geçerli bağlamı kullanmak istiyorsanız, değerini olarak ayarlayın `""` .

    ```powershell
    $kubeContext = "<kubeContext name of your k8s cluster>"
    ```

4. Kümeyi izlemeyi durdurmak için aşağıdaki komutu çalıştırın.

    ```powershell
    .\disable-monitoring.ps1 -clusterResourceId $azureArcClusterResourceId -kubeContext $kubeContext
    ```

### <a name="using-bash"></a>Bash kullanma

1. Aşağıdaki komutları kullanarak betiği, izleme eklentisi ile kümenizi yapılandıran yerel bir klasöre yükleyin ve kaydedin:

    ```bash
    curl -o disable-monitoring.sh -L https://aka.ms/disable-monitoring-bash-script
    ```

2. `azureArcClusterResourceId`İçin karşılık gelen değerleri ayarlayarak `subscriptionId` `resourceGroupName` ve `clusterName` Azure Arc özellikli Kubernetes küme kaynağınızın kaynak kimliğini temsil ederek değişkeni yapılandırın.

    ```bash
    export azureArcClusterResourceId="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
    ```

3. `kubeContext`Komutunu çalıştırarak değişkeni, kümenizin **kuin bağlamı** ile yapılandırın `kubectl config get-contexts` .

    ```bash
    export kubeContext="<kubeContext name of your k8s cluster>"
    ```

4. Kümenizi izlemeyi durdurmak için, dağıtım senaryonuz temelinde sunulan farklı komutlar vardır.

    Geçerli bağlamı kullanarak kümeyi izlemeyi durdurmak için aşağıdaki komutu çalıştırın.

    ```bash
    bash disable-monitoring.sh --resource-id $azureArcClusterResourceId
    ```

    Bir bağlam belirterek kümeyi izlemeyi durdurmak için aşağıdaki komutu çalıştırın

    ```bash
    bash disable-monitoring.sh --resource-id $azureArcClusterResourceId --kube-context $kubeContext
    ```

## <a name="next-steps"></a>Sonraki adımlar

Log Analytics çalışma alanı yalnızca kümeyi izlemeyi desteklemek için oluşturulduysa ve artık gerekmiyorsa, el ile silmeniz gerekir. Bir çalışma alanının nasıl silineceği konusunda bilgi sahibi değilseniz bkz. [Azure Log Analytics çalışma alanını silme](../../log-analytics/log-analytics-manage-del-workspace.md).
