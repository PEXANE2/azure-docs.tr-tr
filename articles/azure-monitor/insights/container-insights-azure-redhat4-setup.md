---
title: Kapsayıcılar için Azure Izleyici ile Azure Red Hat OpenShift v4. x yapılandırma | Microsoft Docs
description: Bu makalede, Azure Red Hat OpenShift sürüm 4 veya sonraki sürümlerde barındırılan Azure Izleyici ile bir Kubernetes kümesi için izlemenin nasıl yapılandırılacağı açıklanır.
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 49097d96ecf58d7c5bf7d1a60ff01fc7182587c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85801487"
---
# <a name="configure-azure-red-hat-openshift-v4x-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici ile Azure Red Hat OpenShift v4. x yapılandırma

Kapsayıcılar için Azure Izleyici, Azure Kubernetes hizmeti (AKS) ve AKS motoru kümelerine yönelik zengin bir izleme deneyimi sağlar. Bu makalede, [Azure Red Hat OpenShift](../../openshift/intro-openshift.md) sürüm 4. x üzerinde barındırılan Kubernetes kümelerinin izlenmesini etkinleştirerek benzer bir izleme deneyiminin nasıl elde edileceğini açıklar.

>[!NOTE]
>Azure Red Hat OpenShift desteği şu anda genel önizlemede bir özelliktir.
>

Bu makalede açıklanan desteklenen yöntemleri kullanarak bir veya daha fazla mevcut Azure Red Hat OpenShift v4. x dağıtımına yönelik kapsayıcılar için Azure Izleyicisini etkinleştirebilirsiniz.

Var olan bir küme için [Azure CLI 'da bu Bash betiğini](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create)çalıştırın.

## <a name="supported-and-unsupported-features"></a>Desteklenen ve desteklenmeyen özellikler

Kapsayıcılar için Azure Izleyici, Azure Red Hat OpenShift v4. x ' i [kapsayıcılara genel bakış](container-insights-overview.md)bölümünde açıklandığı gibi izlemeyi destekler, ancak aşağıdaki özellikler hariç:

- Canlı veriler (Önizleme)
- Küme düğümlerinden ve yığınlardan [ölçümler toplama](container-insights-update-metrics.md) ve bunları Azure izleyici ölçüm veritabanında depolama

## <a name="prerequisites"></a>Ön koşullar

- Azure CLı sürüm 2.0.72 veya üzeri  

- [Helb 3](https://helm.sh/docs/intro/install/) CLI aracı

- [Bash sürüm 4](https://www.gnu.org/software/bash/)

- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) komut satırı aracı

- [Log Analytics çalışma alanı](../platform/design-logs-deployment.md).

    Kapsayıcılar için Azure Izleyici, [bölgeye göre Azure ürünlerinde](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor)listelenen bölgelerde bir Log Analytics çalışma alanını destekler. Kendi çalışma alanınızı oluşturmak için [Azure Resource Manager](../platform/template-workspace-configuration.md), [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)aracılığıyla veya [Azure Portal](../learn/quick-create-workspace.md)aracılığıyla oluşturulabilir.

- Kapsayıcılar için Azure Izleyici 'deki özellikleri etkinleştirmek ve bu özelliklere erişmek için Azure aboneliğindeki bir Azure *katılımcısı* rolüne ve kapsayıcılar Için Azure izleyici ile yapılandırılmış Log Analytics çalışma alanında bir [*Log Analytics katılımcısı*](../platform/manage-access.md#manage-access-using-azure-permissions) rolüne sahip olmanız gerekir.

- İzleme verilerini görüntülemek için, kapsayıcılar için Azure Izleyici ile yapılandırılmış Log Analytics çalışma alanında [*Log Analytics okuyucu*](../platform/manage-access.md#manage-access-using-azure-permissions) rolüne sahip olmanız gerekir.

## <a name="enable-monitoring-for-an-existing-cluster"></a>Var olan bir küme için izlemeyi etkinleştir

Azure Red Hat OpenShift sürüm 4 veya sonraki bir küme için izlemeyi etkinleştirmek üzere, sunulan Bash betiği kullanılarak Azure 'da dağıtılan bir küme için izlemeyi etkinleştirmek üzere şunları yapın:

1. Aşağıdaki komutu çalıştırarak Azure'da oturum açın:

    ```azurecli
    az login
    ```

1. Aşağıdaki komutu çalıştırarak kümenizi izleme eklentisi ile yapılandıran betiği yerel bir klasöre indirip kaydedin:

    `curl -o enable-monitoring.sh -L https://aka.ms/enable-monitoring-bash-script`

1. Kümenizin *Kubebağlamını* belirlemek için aşağıdaki komutları çalıştırın

    ```
    adminUserName=$(az aro list-credentials -g $clusterResourceGroup -n $clusterName --query 'kubeadminUsername' -o tsv)
    adminPassword=$(az aro list-credentials -g $clusterResourceGroup -n $clusterName --query 'kubeadminPassword' -o tsv)
    apiServer=$(az aro show -g $clusterResourceGroup -n $clusterName --query apiserverProfile.url -o tsv)
    oc login $apiServer -u $adminUserName -p $adminPassword
    # openshift project name for azure monitor for containers
    openshiftProjectName="azure-monitor-for-containers"
    oc new-project $openshiftProjectName
    # get the kube config context
    kubeContext=$(oc config current-context)
    ```

1. Daha sonra kullanmak için değeri kopyalayın.

### <a name="integrate-with-an-existing-workspace"></a>Mevcut bir çalışma alanıyla tümleştirin

Bu bölümde, daha önce indirdiğiniz Bash betiğini kullanarak kümenizin izlenmesini etkinleştirirsiniz. Mevcut bir Log Analytics çalışma alanıyla tümleştirmek için, parametresi için gereken Log Analytics çalışma alanınızın tam kaynak KIMLIĞINI tanımlayarak başlayın `logAnalyticsWorkspaceResourceId` ve ardından İzleme eklentisini belirtilen çalışma alanına karşı etkinleştirmek için komutunu çalıştırın.

Belirtmek için bir çalışma alanınız yoksa, [varsayılan çalışma alanı Ile tümleştir](#integrate-with-the-default-workspace) bölümüne atlayabilirsiniz ve betiğin sizin için yeni bir çalışma alanı oluşturmasına izin verebilirsiniz.

1. Aşağıdaki komutu çalıştırarak erişiminiz olan tüm abonelikleri listeleyin:

    ```azurecli
    az account list --all -o table
    ```

    Çıktı aşağıdaki gibi görünür:

    ```azurecli
    Name                                  CloudName    SubscriptionId                        State    IsDefault
    ------------------------------------  -----------  ------------------------------------  -------  -----------
    Microsoft Azure                       AzureCloud   0fb60ef2-03cc-4290-b595-e71108e8f4ce  Enabled  True
    ```

1. **SubscriptionID**değerini kopyalayın.

1. Aşağıdaki komutu çalıştırarak Log Analytics çalışma alanını barındıran aboneliğe geçin:

    ```azurecli
    az account set -s <subscriptionId of the workspace>
    ```

1. Aşağıdaki komutu çalıştırarak aboneliklerinizde bulunan çalışma alanlarının listesini varsayılan JSON biçiminde görüntüleyin:

    ```
    az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
    ```

1. Çıktıda, çalışma alanı adını bulun ve alan **kimliği**altında bu Log Analytics çalışma alanının tam kaynak kimliğini kopyalayın.

1. İzlemeyi etkinleştirmek için aşağıdaki komutu çalıştırın. `azureAroV4ClusterResourceId`, `logAnalyticsWorkspaceResourceId` Ve parametrelerinin değerlerini değiştirin `kubeContext` .

    ```bash
    export azureAroV4ClusterResourceId=“/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/<clusterName>”
    export logAnalyticsWorkspaceResourceId=“/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/microsoft.operationalinsights/workspaces/<workspaceName>”
    export kubeContext="<kubeContext name of your ARO v4 cluster>"  
    ```

    Örnek:

    `bash enable-monitoring.sh --resource-id $azureAroV4ClusterResourceId --kube-context $kubeContext --workspace-id $logAnalyticsWorkspaceResourceId`

İzlemeyi etkinleştirdikten sonra, küme için sistem durumu ölçümlerini görüntüleyebilmeniz yaklaşık 15 dakika sürebilir.

### <a name="integrate-with-the-default-workspace"></a>Varsayılan çalışma alanıyla tümleştirin

Bu bölümde, indirdiğiniz Bash betiğini kullanarak Azure Red Hat OpenShift v4. x kümeniz için izlemeyi etkinleştirirsiniz.

Bu örnekte, var olan bir çalışma alanını önceden oluşturmanız veya belirtmeniz gerekmez. Bu komut, bölge içinde zaten mevcut değilse, küme aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturarak işlemi sizin için basitleştirir.

Oluşturulan varsayılan çalışma alanı *defaultworkspace- \<GUID> - \<Region> *biçimindedir.  

`azureAroV4ClusterResourceId`Ve parametrelerinin değerlerini değiştirin `kubeContext` .

```bash
export azureAroV4ClusterResourceId=“/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/<clusterName>”
export kubeContext="<kubeContext name of your ARO v4 cluster>"
```

Örneğin:

`bash enable-monitoring.sh --resource-id $azureAroV4ClusterResourceId --kube-context $kubeContext`

İzlemeyi etkinleştirdikten sonra, küme için sistem durumu ölçümlerini görüntüleyebilmeniz yaklaşık 15 dakika sürebilir.

### <a name="enable-monitoring-from-the-azure-portal"></a>Azure portal izlemeyi etkinleştir

Kapsayıcılar için Azure Izleyici 'de çok küme görünümü, **izlenmeyen kümeler** sekmesi altında izleme özelliği etkinleştirilmemiş olan Azure Red Hat OpenShift kümelerinizi vurgular. Kümenizin yanındaki **Etkinleştir** seçeneği, portaldan izleme ekleme işlemini başlatmaz. Bu makalenin önceki kısımlarında açıklanan adımları izleyerek el ile izlemeyi etkinleştirmek için bu makaleye yönlendirilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Sol bölmedeki veya giriş sayfasından **Azure izleyici**' yi seçin.

1. **Öngörüler** bölümünde **kapsayıcılar**' ı seçin.

1. **İzleme kapsayıcıları** sayfasında, **izlenmeyen kümeler**' ı seçin.

1. İzlenmeyen kümeler listesinde kümeyi seçin ve ardından **Etkinleştir**' i seçin.

    **Küme türü** sütununda, **Aro** değerini arayarak listedeki sonuçları belirleyebilirsiniz. **Etkinleştir**' i seçtikten sonra bu makaleye yönlendirilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Artık, RedHat OpenShift sürüm 4. x kümenizin ve üzerinde çalışan iş yüklerinizin sistem durumunu ve kaynak kullanımını toplamaya yönelik izlemeyi etkinleştirdiniz. kapsayıcılar için Azure Izleyicisini [nasıl](container-insights-analyze.md) kullanacağınızı öğrenin.

- Varsayılan olarak, Kapsayıcılı Aracı, Kuto-System hariç tüm ad alanlarında çalışan tüm kapsayıcıların *stdout* ve *stderr* kapsayıcı günlüklerini toplar. Belirli bir ad alanı veya ad alanına özgü bir kapsayıcı günlüğü koleksiyonunu yapılandırmak için, *Configmap* yapılandırma dosyanız için istediğiniz veri koleksiyonu ayarlarını yapılandırmak üzere [kapsayıcı öngörüleri Aracısı yapılandırmasını](container-insights-agent-config.md) gözden geçirin.

- Kümelediğiniz Prometheus ölçümlerini hurdaya almak ve analiz etmek için bkz. [Prometheus ölçümleri koruması](container-insights-prometheus-integration.md)'nı inceleyin.

- Kapsayıcılar için Azure Izleyici 'yi kullanarak kümenizi izlemeyi durdurmayı öğrenmek için bkz. [Azure Red Hat OpenShift kümenizi izlemeyi durdurma](container-insights-optout-openshift.md).
