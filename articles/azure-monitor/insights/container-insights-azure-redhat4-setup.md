---
title: Kapsayıcılar için Azure Izleyici ile Azure Red Hat OpenShift v4. x yapılandırma | Microsoft Docs
description: Bu makalede, Azure Red Hat OpenShift sürüm 4 ve üzeri sürümlerde barındırılan Azure Izleyici ile bir Kubernetes kümesi izlemenin nasıl yapılandırılacağı açıklanır.
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: 4b827524845874dabaabe535163d99c408f77a60
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82196302"
---
# <a name="configure-azure-red-hat-openshift-v4x-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici ile Azure Red Hat OpenShift v4. x yapılandırma

Kapsayıcılar için Azure Izleyici, Azure Kubernetes hizmeti (AKS) ve AKS motoru kümeleri için zengin izleme deneyimi sağlar. Bu makalede, benzer bir izleme deneyimi elde etmek için [Azure Red Hat OpenShift](../../openshift/intro-openshift.md) sürüm 4. x üzerinde barındırılan Kubernetes kümelerinin izlenmesini nasıl etkinleştireceğinizi açıklar.

>[!NOTE]
>Azure Red Hat OpenShift desteği şu anda genel önizlemede bir özelliktir.
>

Azure Red Hat OpenShift v4. x ' in bir veya daha fazla mevcut dağıtımı için Azure Izleyici, aşağıdaki desteklenen yöntemleri kullanarak etkinleştirilebilir:

- Mevcut bir küme için, belirtilen Bash betiğini kullanarak [Azure CLI](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create)'de çalışır.

## <a name="supported-and-unsupported-features"></a>Desteklenen ve desteklenmeyen özellikler

Kapsayıcılar için Azure Izleyici, [genel bakış](container-insights-overview.md) makalesinde açıklandığı şekilde Azure Red Hat OpenShift v4. x ' i izlemeyi destekler, ancak aşağıdaki özellikler hariç:

- Canlı veriler (Önizleme)
- Küme düğümlerinden ve yığınlardan [ölçümler toplayın](container-insights-update-metrics.md) ve bunları Azure izleyici ölçümleri veritabanında depolarsınız

## <a name="prerequisites"></a>Ön koşullar

- Azure CLı sürüm 2.0.72 veya üzeri

- [Helb 3](https://helm.sh/docs/intro/install/) CLı aracı

- [Bash sürüm 4](https://www.gnu.org/software/bash/)

- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) komut satırı aracı

- Kapsayıcılar için Azure Izleyici 'deki özellikleri etkinleştirmek ve bu özelliklere erişmek için, en azından Azure aboneliğindeki Azure *katkıda* bulunan rolünün bir üyesi olmanız ve kapsayıcılar Için Azure izleyici ile yapılandırılmış Log Analytics çalışma alanının [*Log Analytics katkıda*](../platform/manage-access.md#manage-access-using-azure-permissions) bulunan rolünün bir üyesi olmanız gerekir.

- İzleme verilerini görüntülemek için, kapsayıcılar için Azure Izleyici ile yapılandırılmış Log Analytics çalışma alanına [*Log Analytics okuyucu*](../platform/manage-access.md#manage-access-using-azure-permissions) rolü izninin bir üyesi olursunuz.

## <a name="enable-for-an-existing-cluster"></a>Var olan bir küme için etkinleştir

Azure Red Hat OpenShift sürüm 4 ' ün ve daha yüksek bir kümenin, Azure 'da dağıtılan Bash betiği kullanılarak izlenmesini sağlamak için aşağıdaki adımları gerçekleştirin.

1. Azure’da oturum açma

    ```azurecli
    az login
    ```

2. Aşağıdaki komutları kullanarak betiği, izleme eklentisi ile kümenizi yapılandıran yerel bir klasöre yükleyin ve kaydedin:

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aroV4/onboarding_azuremonitor_for_containers.sh.`

3. Kümenizin **kuin bağlamını** belirlemek için, kümenizde başarılı `oc login` olduktan sonra komutunu `kubectl config current-context` çalıştırın ve değeri kopyalayın.

### <a name="integrate-with-an-existing-workspace"></a>Mevcut bir çalışma alanıyla tümleştirin

Aşağıdaki adım, daha önce indirdiğiniz Bash betiğini kullanarak kümenizin izlenmesine izin vermez. Mevcut bir Log Analytics çalışma alanıyla tümleştirebilmek için, `workspaceResourceId` parametre için gereken Log Analytics çalışma alanınızın tam kaynak kimliğini belirlemek üzere aşağıdaki adımları uygulayın ve ardından, belirtilen çalışma alanına karşı izleme eklentisini etkinleştirmek için komutunu çalıştırın. Belirtmek için bir çalışma alanınız yoksa, 5. adıma atlayabilir ve betiğin sizin için yeni bir çalışma alanı oluşturmasını sağlayabilirsiniz.

1. Aşağıdaki komutu kullanarak erişiminiz olan tüm abonelikleri listeleyin:

    ```azurecli
    az account list --all -o table
    ```

    Çıktı aşağıdakine benzeyecektir:

    ```azurecli
    Name                                  CloudName    SubscriptionId                        State    IsDefault
    ------------------------------------  -----------  ------------------------------------  -------  -----------
    Microsoft Azure                       AzureCloud   68627f8c-91fO-4905-z48q-b032a81f8vy0  Enabled  True
    ```

    **SubscriptionID**değerini kopyalayın.

2. Aşağıdaki komutu kullanarak Log Analytics çalışma alanını barındıran aboneliğe geçin:

    ```azurecli
    az account set -s <subscriptionId of the workspace>
    ```

3. Aşağıdaki örnek, aboneliklerinizdeki çalışma alanlarının listesini varsayılan JSON biçiminde görüntüler.

    ```
    az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
    ```

    Çıktıda, çalışma alanı adını bulun ve alan **kimliği**altında bu Log Analytics çalışma alanının tam kaynak kimliğini kopyalayın.

4. `workspaceResourceId` Parametresinin değerini değiştirerek izlemeyi etkinleştirmek için aşağıdaki komutu çalıştırın: 

    `bash onboarding_azuremonitor_for_containers.sh <kube-context> <azureAroV4ResourceId> <LogAnayticsWorkspaceResourceId>`

    Örnek:

    `bash onboarding_azuremonitor_for_containers.sh MyK8sTestCluster /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/test-aro-v4-rg/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/test-aro-v4  /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourcegroups/test-la-workspace-rg/providers/microsoft.operationalinsights/workspaces/test-la-workspace`

İzlemeyi etkinleştirdikten sonra, küme için sistem durumu ölçümlerini görüntüleyebilmeniz yaklaşık 15 dakika sürebilir.

### <a name="integrate-with-default-workspace"></a>Varsayılan çalışma alanıyla tümleştirin

Aşağıdaki adım, indirdiğiniz Bash betiğini kullanarak Azure Red Hat OpenShift v4. x kümenizi izlemeye izin vermez. Bu örnekte, oluşturma veya mevcut bir çalışma alanı belirtmeniz gerekmez. Bu komut, bölgede zaten mevcut değilse, küme aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturarak işlemi sizin için basitleştirir. Oluşturulan varsayılan çalışma alanı, *Defaultworkspace-\<GUID>-\<Region>* biçimine benzer.  

    `bash onboarding_azuremonitor_for_containers.sh <kube-context> <azureAroV4ResourceId>`

    For example:

    `bash onboarding_azuremonitor_for_containers.sh MyK8sTestCluster /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/test-aro-v4-rg/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/test-aro-v4`

İzlemeyi etkinleştirdikten sonra, küme için sistem durumu ölçümlerini görüntüleyebilmeniz yaklaşık 15 dakika sürebilir.

### <a name="from-the-azure-portal"></a>Azure portalından

Kapsayıcılar için Azure Izleyici 'de birden çok küme görünümü, **izlenen kümeler** sekmesinde izleme etkinleştirilmemiş olan Azure Red Hat OpenShift kümelerinizi vurgular. Kümenizin yanındaki **Etkinleştir** seçeneği, portaldan izleme eklemeyi başlatmaz. Bu makalede daha önce açıklanan adımları izleyerek izlemeyi el ile etkinleştirmek için bu makaleye yönlendirilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Azure portal menüsünde veya giriş sayfasından **Azure izleyici**' yi seçin. **Öngörüler** bölümünde **kapsayıcılar**' ı seçin.

3. **İzleyici-kapsayıcılar** sayfasında, **izlenmeyen kümeler**' ı seçin.

4. İzlenmeyen kümeler listesinden, listeden kümeyi bulun ve **Etkinleştir**' e tıklayın. Sütun **KÜMESI türü**altında, **Aro** değerini arayarak listedeki sonuçları belirleyebilirsiniz. **Etkinleştir**' e tıkladıktan sonra bu makaleye yönlendirilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- RedHat OpenShift sürüm 4. x kümeniz ve üzerinde çalışan iş yüklerinizin sistem durumunu ve kaynak kullanımını toplamaya yönelik izleme özelliği, kapsayıcılar için Azure Izleyicisini [nasıl](container-insights-analyze.md) kullanacağınızı öğrenin.

- Varsayılan olarak, Kapsayıcılı Aracı, Kuto-System hariç tüm ad alanlarında çalışan tüm kapsayıcıların stdout/stderr kapsayıcı günlüklerini toplar. Belirli bir ad alanı veya ad alanına özgü kapsayıcı günlüğü koleksiyonunu yapılandırmak için, istenen veri koleksiyonu ayarlarını ConfigMap yapılandırmaları dosyanıza yapılandırmak üzere [kapsayıcı öngörüleri aracı yapılandırmasını](container-insights-agent-config.md) gözden geçirin.

- Kümelediğiniz Prometheus ölçümlerini hurdaya almak ve analiz etmek için bkz. [Prometheus ölçümleri koruması](container-insights-prometheus-integration.md) 'nı inceleyin

- Azure Izleyici kapsayıcılar ile kümenizi izlemeyi durdurmayı öğrenmek için bkz. [Azure Red Hat OpenShift kümenizi Izlemeyi durdurma](container-insights-optout-openshift.md).
