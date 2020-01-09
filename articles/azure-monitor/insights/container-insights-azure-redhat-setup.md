---
title: Azure Red Hat OpenShift kümelerini kapsayıcılar için Azure Izleyici ile yapılandırma | Microsoft Docs
description: Bu makalede, Azure Red Hat OpenShift 'te barındırılan Kubernetes kümelerini izlemek üzere kapsayıcılar için Azure Izleyicisini nasıl yapılandırabileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: 6922cb7b143989ba329df972a06825629c4c5020
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75405567"
---
# <a name="configure-azure-red-hat-openshift-clusters-with-azure-monitor-for-containers"></a>Azure Red Hat OpenShift kümelerini kapsayıcılar için Azure Izleyici ile yapılandırma

Kapsayıcılar için Azure Izleyici, Azure Kubernetes hizmeti (AKS) ve AKS motoru kümeleri için zengin izleme deneyimi sağlar. Bu makalede, benzer bir izleme deneyimi elde etmek için [Azure Red Hat OpenShift](../../openshift/intro-openshift.md) üzerinde barındırılan Kubernetes kümelerinin izlenmesini nasıl etkinleştireceğinizi açıklar.

>[!NOTE]
>Azure Red Hat OpenShift desteği şu anda genel önizlemede bir özelliktir.
>

Kapsayıcılar için Azure Izleyici, yeni bir veya daha fazla Azure Red Hat OpenShift dağıtımı için aşağıdaki desteklenen yöntemleri kullanarak etkinleştirilebilir:

- Azure portal veya Azure Resource Manager şablonu kullanarak var olan bir küme için
- Azure Resource Manager şablonu kullanan yeni bir küme için 

## <a name="supported-and-unsupported-features"></a>Desteklenen ve desteklenmeyen özellikler

Kapsayıcılar için Azure Izleyici, [genel bakış](container-insights-overview.md) makalesinde açıklandığı şekilde Azure Red Hat OpenShift 'i izlemeyi destekler, ancak aşağıdaki özellikler hariç:

- Canlı veriler
- Prometheus ölçümleri scraping
- Küme düğümlerinden ve yığınlardan [ölçümler toplayın](container-insights-update-metrics.md) ve bunları Azure izleyici ölçümleri veritabanında depolarsınız
- Sistem durumu özelliği

## <a name="prerequisites"></a>Ön koşullar

- Kapsayıcılar için Azure Izleyici 'deki özellikleri etkinleştirmek ve bu özelliklere erişmek için, en azından Azure aboneliğindeki Azure *katkıda* bulunan rolünün bir üyesi olmanız ve kapsayıcılar Için Azure izleyici ile yapılandırılmış Log Analytics çalışma alanının [*Log Analytics katkıda*](../platform/manage-access.md#manage-access-using-azure-permissions) bulunan rolünün bir üyesi olmanız gerekir.

- İzleme verilerini görüntülemek için, kapsayıcılar için Azure Izleyici ile yapılandırılmış Log Analytics çalışma alanına [*Log Analytics okuyucu*](../platform/manage-access.md#manage-access-using-azure-permissions) rolü izninin bir üyesi olursunuz.

## <a name="enable-for-a-new-cluster-using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak yeni küme için etkinleştirme

İzleme özelliği etkinken bir Azure Red Hat OpenShift kümesi dağıtmak için aşağıdaki adımları gerçekleştirin. Devam etmeden önce, ortamınızın doğru şekilde ayarlanabilmesi için yapılandırmanız gereken bağımlılıkları anlamak üzere [Azure Red Hat OpenShift kümesi oluşturma](../../openshift/tutorial-create-cluster.md#prerequisites) öğreticisini gözden geçirin.

Bu yöntem, iki JSON şablonları içerir. Bir şablon, izleme etkin olan kümeyi dağıtmaya yönelik yapılandırmayı belirtir ve diğeri aşağıdakileri belirtmek için yapılandırdığınız parametre değerlerini içerir:

- Azure Red Hat OpenShift kümesi kaynak KIMLIĞI. 

- Kümenin dağıtıldığı kaynak grubu.

- Azure Active Directory bir veya zaten oluşturulmuş bir oluşturma adımları gerçekleştirildikten sonra belirtilen [KIRACı kimliği](../../openshift/howto-create-tenant.md#create-a-new-azure-ad-tenant) .

- Azure Active Directory bir veya zaten oluşturulmuş bir oluşturma adımları gerçekleştirildikten sonra belirtilen [istemci uygulama kimliği](../../openshift/howto-aad-app-configuration.md#create-an-azure-ad-app-registration) .

- Bir veya zaten oluşturulmuş bir oluşturma adımları gerçekleştirildikten sonra belirtilen [istemci parolası Azure Active Directory](../../openshift/howto-aad-app-configuration.md#create-a-client-secret) .

- [Azure AD güvenlik grubu](../../openshift/howto-aad-app-configuration.md#create-an-azure-ad-security-group) , bir veya zaten oluşturulmuş bir oluşturma adımları gerçekleştirildikten sonra belirtilmiştir.

- Mevcut bir Log Analytics çalışma alanının kaynak KIMLIĞI.

- Kümede oluşturulacak ana düğüm sayısı.

- Aracı havuzu profilindeki işlem düğümlerinin sayısı.

- Aracı havuzu profilindeki altyapı düğümlerinin sayısı. 

Bir şablon kullanarak kaynakları dağıtma kavramıyla alışkın değilseniz, bkz:

- [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)

- [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLı sürüm 2.0.65 veya üstünü çalıştırıyor olmanız gerekir. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

Azure PowerShell veya CLı kullanılarak izlemeyi etkinleştirmeden önce Log Analytics çalışma alanı oluşturulmalıdır. Çalışma alanı oluşturmak için bunu aracılığıyla ayarlayabilirsiniz [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)temellidir [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../../azure-monitor/learn/quick-create-workspace.md).

1. Aşağıdaki komutları kullanarak izleme eklentisi ile bir küme oluşturmak için bir yerel klasöre, Azure Resource Manager şablonu ve parametre dosyasına indirip kaydedin:

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aro/enable_monitoring_to_new_cluster/newClusterWithMonitoring.json`

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aro/enable_monitoring_to_new_cluster/newClusterWithMonitoringParam.json` 

2. Azure'da oturum açın 

    ```azurecli
    az login    
    ```
    
    Birden çok aboneliğe erişiminiz varsa, `{subscription ID}` yerine kullanmak istediğiniz abonelikle değiştirin `az account set -s {subscription ID}` çalıştırın.
 
3. Henüz bir tane yoksa kümeniz için bir kaynak grubu oluşturun. Azure üzerinde OpenShift 'i destekleyen Azure bölgelerinin listesi için bkz. [Desteklenen bölgeler](../../openshift/supported-resources.md#azure-regions). 

    ```azurecli
    az group create -g <clusterResourceGroup> -l <location> 
    ```

4. **Newclusterwithmonitoringparam. JSON** JSON parametre dosyasını düzenleyin ve aşağıdaki değerleri güncelleştirin:

    - *konum*
    - *:/*
    - *Aadtenantıd*
    - *Aadclientıd*
    - *aadClientSecret* 
    - *Aadcustomeradmingroupıd* 
    - *workspaceResourceId*
    - *masterNodeCount*
    - *computeNodeCount*
    - *Deniz Ranodecount*

5. Aşağıdaki adım, Azure CLı 'yı kullanarak kümeyi etkin izleme ile dağıtır. 

    ```azurecli
    az group deployment create --resource-group <ClusterResourceGroupName> --template-file ./newClusterWithMonitoring.json --parameters @./newClusterWithMonitoringParam.json 
    ```
 
    Çıktı aşağıdakine benzer:

    ```azurecli
    provisioningState       : Succeeded
    ```

## <a name="enable-for-an-existing-cluster"></a>Var olan bir küme için etkinleştir

Azure 'da dağıtılan bir Azure Red Hat OpenShift kümesinin izlenmesini etkinleştirmek için aşağıdaki adımları gerçekleştirin. Bunu Azure portal veya belirtilen şablonları kullanarak gerçekleştirebilirsiniz.

### <a name="from-the-azure-portal"></a>Azure portalından
 
1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. Azure portal menüsünde veya giriş sayfasından **Azure izleyici**' yi seçin. Altında **Insights** bölümünden **kapsayıcıları**. 

3. Üzerinde **İzleyicisi - kapsayıcıları** sayfasında **olmayan izlenen kümeleri**.

4. İzlenmeyen kümeler listesinden, listeden kümeyi bulun ve **Etkinleştir**' e tıklayın. Sütun **KÜMESI türü**altında, **Aro** değerini arayarak listedeki sonuçları belirleyebilirsiniz.

5. Üzerinde **kapsayıcılar için Azure İzleyici ekleme** sayfasında, mevcut bir Log Analytics varsa küme ile aynı abonelikte çalışma alanı, aşağı açılan listeden seçin.  
    Liste, varsayılan çalışma alanını ve kümenin abonelikte dağıtıldığı konumu önceden seçer. 

    ![İzlenmeyen kümeler için izlemeyi etkinleştir](./media/container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Küme izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md). Çalışma alanını, RedHat OpenShift kümesinin dağıtıldığı abonelikte oluşturmayı unutmayın. 
 
İzleme etkinleştirdikten sonra küme için sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. 

### <a name="enable-using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak etkinleştir

Bu yöntem, iki JSON şablonları içerir. Yapılandırmayı, izlemeyi etkinleştirmek için bir şablon belirtir ve diğer aşağıdaki belirtmek için yapılandırdığınız parametre değerlerini içerir:

- Azure RedHat OpenShift kümesi kaynak KIMLIĞI. 

- Kümenin dağıtıldığı kaynak grubu.

- Log Analytics çalışma alanı.

Bir şablon kullanarak kaynakları dağıtma kavramıyla alışkın değilseniz, bkz:

- [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)

- [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLı sürüm 2.0.65 veya üstünü çalıştırıyor olmanız gerekir. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

Azure PowerShell veya CLı kullanılarak izlemeyi etkinleştirmeden önce Log Analytics çalışma alanı oluşturulmalıdır. Çalışma alanı oluşturmak için bunu aracılığıyla ayarlayabilirsiniz [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)temellidir [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../../azure-monitor/learn/quick-create-workspace.md).

1. Aşağıdaki komutları kullanarak kümenizi izleme eklentisi ile güncelleştirmek için şablon ve parametre dosyasını indirin:

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aro/enable_monitoring_to_existing_cluster/existingClusterOnboarding.json`

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aro/enable_monitoring_to_existing_cluster/existingClusterParam.json` 

2. Azure'da oturum açın 

    ```azurecli
    az login    
    ```

    Birden çok aboneliğe erişiminiz varsa, `{subscription ID}` yerine kullanmak istediğiniz abonelikle değiştirin `az account set -s {subscription ID}` çalıştırın.

3. Azure RedHat OpenShift kümesi aboneliğini belirtin.

    ```azurecli
    az account set --subscription "Subscription Name"  
    ```

4. Küme konumunu ve kaynak KIMLIĞINI belirlemek için aşağıdaki komutu çalıştırın:

    ```azurecli
    az openshift show -g <clusterResourceGroup> -n <clusterName> 
    ```

5. **Existingclusterparam. JSON** JSON parametre dosyasını düzenleyin ve *Araresourceıd* ve *Araresorucelocation*değerlerini güncelleştirin. Değeri **workspaceResourceId** çalışma alanı adı içerir, Log Analytics çalışma alanının tam kaynak kimliği. 

6. Azure CLı ile dağıtmak için aşağıdaki komutları çalıştırın: 

    ```azurecli
    az group deployment create --resource-group <ClusterResourceGroupName> --template-file ./ExistingClusterOnboarding.json --parameters @./existingClusterParam.json 
    ```

    Çıktı aşağıdakine benzer:

    ```azurecli
    provisioningState       : Succeeded
    ```

## <a name="next-steps"></a>Sonraki adımlar

- RedHat OpenShift kümeniz ve üzerinde çalışan iş yüklerinizin sistem durumunu ve kaynak kullanımını toplamaya yönelik izleme özelliği, kapsayıcılar için Azure Izleyicisini [nasıl](container-insights-analyze.md) kullanacağınızı öğrenin.

- Azure Izleyici kapsayıcılar ile kümenizi izlemeyi durdurmayı öğrenmek için bkz. [Azure Red Hat OpenShift kümenizi Izlemeyi durdurma](container-insights-optout-openshift.md).
