---
title: Kapsayıcılar için Azure Izleyici ile Azure Red Hat OpenShift v4. x yapılandırma | Microsoft Docs
description: Bu makalede, Azure Red Hat OpenShift sürüm 4 veya sonraki sürümlerde barındırılan Azure Izleyici ile bir Kubernetes kümesi için izlemenin nasıl yapılandırılacağı açıklanır.
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: 7eee7ba6ba01679f72d1249058e4101b38d8461d
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85508019"
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

- Kapsayıcılar için Azure Izleyici 'deki özellikleri etkinleştirmek ve bu özelliklere erişmek için Azure aboneliğindeki bir Azure *katılımcısı* rolüne ve kapsayıcılar Için Azure izleyici ile yapılandırılmış Log Analytics çalışma alanında bir [*Log Analytics katılımcısı*](../platform/manage-access.md#manage-access-using-azure-permissions) rolüne sahip olmanız gerekir.

- İzleme verilerini görüntülemek için, kapsayıcılar için Azure Izleyici ile yapılandırılmış Log Analytics çalışma alanında [*Log Analytics okuyucu*](../platform/manage-access.md#manage-access-using-azure-permissions) rolüne sahip olmanız gerekir.

## <a name="enable-monitoring-for-an-existing-cluster"></a>Var olan bir küme için izlemeyi etkinleştir

Azure Red Hat OpenShift sürüm 4 veya sonraki bir küme için izlemeyi etkinleştirmek üzere, sunulan Bash betiği kullanılarak Azure 'da dağıtılan bir küme için izlemeyi etkinleştirmek üzere şunları yapın:

1. Aşağıdaki komutu çalıştırarak Azure'da oturum açın:

    ```azurecli
    az login
    ```

1. Aşağıdaki komutu çalıştırarak kümenizi izleme eklentisi ile yapılandıran betiği yerel bir klasöre indirip kaydedin:

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aroV4/onboarding_azuremonitor_for_containers.sh.`

1. Kümenizin *kuin bağlamını* belirlemek için, kümenizde başarılı bir *OC oturum* açtıktan sonra aşağıdaki komutu çalıştırın:

    `kubectl config current-context`
    
1. Daha sonra kullanmak için değeri kopyalayın.

### <a name="integrate-with-an-existing-workspace"></a>Mevcut bir çalışma alanıyla tümleştirin

Bu bölümde, daha önce indirdiğiniz Bash betiğini kullanarak kümenizin izlenmesini etkinleştirirsiniz. Mevcut bir Log Analytics çalışma alanıyla tümleştirmek için, parametresi için gereken Log Analytics çalışma alanınızın tam kaynak KIMLIĞINI tanımlayarak başlayın `workspaceResourceId` ve ardından İzleme eklentisini belirtilen çalışma alanına karşı etkinleştirmek için komutunu çalıştırın. 

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

1. İzlemeyi etkinleştirmek için aşağıdaki komutu çalıştırın. `azureAroV4ResourceId`Ve parametrelerinin değerlerini değiştirin `workspaceResourceId` . 

    `bash onboarding_azuremonitor_for_containers.sh <kube-context> <azureAroV4ResourceId> <workspaceResourceId>`

    Örnek:

    `bash onboarding_azuremonitor_for_containers.sh MyK8sTestCluster /subscriptions/0fb60ef2-03cc-4290-b595-e71108e8f4ce/resourceGroups/test-aro-v4-rg/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/test-aro-v4 /subscriptions/0fb60ef2-03cc-4290-b595-e71108e8f4ce/resourcegroups/test-la-workspace-rg/providers/microsoft.operationalinsights/workspaces/test-la-workspace`

İzlemeyi etkinleştirdikten sonra, küme için sistem durumu ölçümlerini görüntüleyebilmeniz yaklaşık 15 dakika sürebilir.

### <a name="integrate-with-the-default-workspace"></a>Varsayılan çalışma alanıyla tümleştirin

Bu bölümde, indirdiğiniz Bash betiğini kullanarak Azure Red Hat OpenShift v4. x kümeniz için izlemeyi etkinleştirirsiniz. 

Bu örnekte, var olan bir çalışma alanını önceden oluşturmanız veya belirtmeniz gerekmez. Bu komut, bölge içinde zaten mevcut değilse, küme aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturarak işlemi sizin için basitleştirir. 

Oluşturulan varsayılan çalışma alanı *defaultworkspace- \<GUID> - \<Region> *biçimindedir.  

`bash onboarding_azuremonitor_for_containers.sh <kube-context> <azureAroV4ResourceId>`

Örneğin:

`bash onboarding_azuremonitor_for_containers.sh MyK8sTestCluster /subscriptions/0fb60ef2-03cc-4290-b595-e71108e8f4ce/resourceGroups/test-aro-v4-rg/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/test-aro-v4`

İzlemeyi etkinleştirdikten sonra, küme için sistem durumu ölçümlerini görüntüleyebilmeniz yaklaşık 15 dakika sürebilir.

### <a name="from-the-azure-portal"></a>Azure portalından

Kapsayıcılar için Azure Izleyici 'de birden çok küme görünümü, **izlenen kümeler** sekmesinde izleme etkinleştirilmemiş olan Azure Red Hat OpenShift kümelerinizi vurgular. Kümenizin yanındaki **Etkinleştir** seçeneği, portaldan izleme ekleme işlemini başlatmaz. Bu makalenin önceki kısımlarında açıklanan adımları izleyerek el ile izlemeyi etkinleştirmek için bu makaleye yönlendirilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmedeki veya giriş sayfasından **Azure izleyici**' yi seçin. 

1. **Öngörüler** bölümünde **kapsayıcılar**' ı seçin.

1. **İzleyici-kapsayıcılar** sayfasında, **izlenmeyen kümeler**' ı seçin.

1. İzlenmeyen kümeler listesinde kümeyi seçin ve ardından **Etkinleştir**' i seçin. 

    **Küme türü** sütununda, **Aro** değerini arayarak listedeki sonuçları belirleyebilirsiniz. **Etkinleştir**' i seçtikten sonra bu makaleye yönlendirilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Artık, RedHat OpenShift sürüm 4. x kümenizin ve üzerinde çalışan iş yüklerinizin sistem durumunu ve kaynak kullanımını toplamaya yönelik izlemeyi etkinleştirdiniz. kapsayıcılar için Azure Izleyicisini [nasıl](container-insights-analyze.md) kullanacağınızı öğrenin.

- Varsayılan olarak, Kapsayıcılı Aracı, Kuto-System hariç tüm ad alanlarında çalışan tüm kapsayıcıların *stdout* ve *stderr* kapsayıcı günlüklerini toplar. Belirli bir ad alanı veya ad alanına özgü bir kapsayıcı günlüğü koleksiyonunu yapılandırmak için, *Configmap* yapılandırma dosyanız için istediğiniz veri koleksiyonu ayarlarını yapılandırmak üzere [kapsayıcı öngörüleri Aracısı yapılandırmasını](container-insights-agent-config.md) gözden geçirin.

- Kümelediğiniz Prometheus ölçümlerini hurdaya almak ve analiz etmek için bkz. [Prometheus ölçümleri koruması](container-insights-prometheus-integration.md)'nı inceleyin.

- Kapsayıcılar için Azure Izleyici 'yi kullanarak kümenizi izlemeyi durdurmayı öğrenmek için bkz. [Azure Red Hat OpenShift kümenizi izlemeyi durdurma](container-insights-optout-openshift.md).
