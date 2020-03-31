---
title: Kapsayıcılar için Azure Red Hat OpenShift kümelerini Azure Monitor ile yapılandırın | Microsoft Dokümanlar
description: Bu makalede, Azure Red Hat OpenShift'te barındırılan Azure Monitor ile bir Kubernetes kümesinin izleme denetiminin nasıl yapılandırılacağı açıklanmaktadır.
ms.topic: conceptual
ms.date: 02/12/2020
ms.openlocfilehash: c2fd3568be2c51296bb1377e91031ebfb7ca6ee3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275522"
---
# <a name="configure-azure-red-hat-openshift-clusters-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Red Hat OpenShift kümelerini Azure Monitörü ile yapılandırma

Kapsayıcılar için Azure Monitör, Azure Kubernetes Hizmeti (AKS) ve AKS Engine kümeleri için zengin izleme deneyimi sağlar. Bu makalede, benzer bir izleme deneyimi elde etmek için [Azure Red Hat OpenShift'te](../../openshift/intro-openshift.md) barındırılan Kubernetes kümelerinin izlenmesinin nasıl etkinleştirilen olduğu açıklanmaktadır.

>[!NOTE]
>Azure Red Hat OpenShift desteği, şu anda genel önizlemede bir özelliktir.
>

Kapsayıcılar için Azure Monitörü, desteklenen yöntemler kullanılarak Azure Red Hat OpenShift'in yeni veya bir veya daha fazla varolan dağıtımı için etkinleştirilebilir:

- Azure portalından veya Azure Kaynak Yöneticisi şablonundan varolan bir küme için.
- Azure Kaynak Yöneticisi şablonu kullanarak veya [Azure CLI'yi](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create)kullanarak yeni bir küme oluştururken yeni bir küme için.

## <a name="supported-and-unsupported-features"></a>Desteklenen ve desteklenmeyen özellikler

Kapsayıcılar için Azure Monitor, aşağıdaki özellikler dışında [Genel Bakış](container-insights-overview.md) makalesinde açıklandığı gibi Azure Red Hat OpenShift'in izlenmesini destekler:

- Canlı Veri (önizleme)
- Küme düğümleri ve [bölmelerden ölçümler toplayın](container-insights-update-metrics.md) ve bunları Azure Monitor ölçümleri veritabanında depolama

## <a name="prerequisites"></a>Ön koşullar

- Kapsayıcılar için Azure Monitor'daki özellikleri etkinleştirmek ve erişmek için, en azından Azure aboneliğinde Azure *Katılımcısı* rolünün bir üyesi ve kapsayıcılar için Azure Monitor ile yapılandırılan Log Analytics çalışma alanının [*Log Analytics Katılımcısıfatı*](../platform/manage-access.md#manage-access-using-azure-permissions) rolünün bir üyesi olmanız gerekir.

- İzleme verilerini görüntülemek için, kapsayıcılar için Azure Monitor ile yapılandırılan Log Analytics çalışma alanıyla [*Log Analytics okuyucu*](../platform/manage-access.md#manage-access-using-azure-permissions) rol izninin bir üyesisiniz.

## <a name="enable-for-a-new-cluster-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak yeni bir küme için etkinleştirme

İzleme etkinleştirilmiş bir Azure Red Hat OpenShift kümesini dağıtmak için aşağıdaki adımları gerçekleştirin. Devam etmeden önce, ortamınızın doğru şekilde ayarlanması için yapılandırmanız gereken bağımlılıkları anlamak için bir [Azure Kırmızı Şapka OpenShift kümesi oluştur](../../openshift/tutorial-create-cluster.md#prerequisites) sertifikasını gözden geçirin.

Bu yöntem iki JSON şablonu içerir. Şablonlardan biri, izlemeyi etkinleştirerek kümeyi dağıtmak için yapılandırmayı belirtir ve diğeri aşağıdakileri belirtmek üzere yapılandırdığınız parametre değerlerini içerir:

- Azure Red Hat OpenShift küme kaynak kimliği.

- Kümenin dağıtılan kaynak grubu.

- [Azure Active Directory kiracı kimliği,](../../openshift/howto-create-tenant.md#create-a-new-azure-ad-tenant) zaten oluşturulmuş bir veya bir tane oluşturmak için adımları gerçekleştirdikten sonra kaydetti.

- [Azure Active Directory istemci uygulaması Kimliği,](../../openshift/howto-aad-app-configuration.md#create-an-azure-ad-app-registration) zaten oluşturulmuş bir veya bir tane oluşturmak için adımları gerçekleştirdikten sonra kaydetti.

- [Azure Active Directory Client secret,](../../openshift/howto-aad-app-configuration.md#create-a-client-secret) zaten oluşturulmuş bir veya bir tane oluşturmak için adımları gerçekleştirdikten sonra kaydetti.

- [Azure AD güvenlik grubu,](../../openshift/howto-aad-app-configuration.md#create-an-azure-ad-security-group) zaten oluşturulmuş bir veya bir tane oluşturmak için adımları gerçekleştirdikten sonra kaydetti.

- Varolan bir Log Analytics çalışma alanının kaynak kimliği.

- Kümede oluşturulacak ana düğüm sayısı.

- Aracı havuzu profilindeki işlem düğümlerinin sayısı.

- Aracı havuzu profilindeki altyapı düğümlerinin sayısı.

Bir şablon kullanarak kaynakları dağıtma kavramına aşina değilseniz, bkz:

- [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)

- [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Azure CLI'yi kullanmayı seçerseniz, öncelikle CLI'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI sürümünü 2.0.65 veya sonraki sürümde çalışıyor olmalısınız. Sürümünüzü tanımlamak için `az --version`çalıştırın. Azure CLI'yi yüklemeniz veya yükseltmeniz gerekiyorsa, [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

Azure PowerShell veya CLI kullanarak izlemeyi etkinleştirmeden önce Log Analytics çalışma alanının oluşturulması gerekir. Çalışma alanını oluşturmak için, azure [kaynak yöneticisi](../../azure-monitor/platform/template-workspace-configuration.md)aracılığıyla , [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)üzerinden veya [Azure portalında](../../azure-monitor/learn/quick-create-workspace.md)ayarlayabilirsiniz.

1. Aşağıdaki komutları kullanarak izleme eklentisi ile bir küme oluşturmak için yerel bir klasöre, Azure Kaynak Yöneticisi şablonu ve parametre dosyasına indirin ve kaydedin:

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aro/enable_monitoring_to_new_cluster/newClusterWithMonitoring.json`

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aro/enable_monitoring_to_new_cluster/newClusterWithMonitoringParam.json`

2. Azure'da oturum açma

    ```azurecli
    az login    
    ```

    Birden çok aboneye erişiminiz `az account set -s {subscription ID}` varsa, `{subscription ID}` kullanmak istediğiniz abonelikle değiştirin.

3. Kümeniz için zaten bir kaynak grubunuz yoksa bir kaynak grubu oluşturun. Azure'da OpenShift'i destekleyen Azure bölgelerinin listesi için [bkz.](../../openshift/supported-resources.md#azure-regions)

    ```azurecli
    az group create -g <clusterResourceGroup> -l <location>
    ```

4. JSON parametre **dosyasını yeniClusterWithMonitoringParam.json'u** güncelleyin ve aşağıdaki değerleri güncelleyin:

    - *Konum*
    - *clusterName*
    - *aadTenantId*
    - *aadClientId*
    - *aadClientSecret*
    - *aadCustomerAdminGroupId*
    - *çalışma alanıResourceId*
    - *masterNodeCount*
    - *computeNodeCount*
    - *infraNodeCount*

5. Aşağıdaki adım, Azure CLI'yi kullanarak izlemeyi etkinleştirerek kümeyi dağıtıyor.

    ```azurecli
    az group deployment create --resource-group <ClusterResourceGroupName> --template-file ./newClusterWithMonitoring.json --parameters @./newClusterWithMonitoringParam.json
    ```

    Çıktı aşağıdakilere benzer:

    ```output
    provisioningState       : Succeeded
    ```

## <a name="enable-for-an-existing-cluster"></a>Varolan bir küme için etkinleştirme

Azure'da dağıtılan bir Azure Red Hat OpenShift kümesinin izlenmesini etkinleştirmek için aşağıdaki adımları gerçekleştirin. Bunu Azure portalından veya sağlanan şablonları kullanarak gerçekleştirebilirsiniz.

### <a name="from-the-azure-portal"></a>Azure portalından

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Azure portalı menüsünde veya Giriş **sayfasından Azure Monitör'ü**seçin. İstatistikler bölümünde **Kapsayıcılar'ı** seçin. **Containers**

3. **Monitörde - kapsayıcılar** sayfasında, **izlenmeyen kümeleri**seçin.

4. İzlenmeyen kümeler listesinden, listedeki kümeyi bulun ve **Etkinleştir'i**tıklatın. Cluster **TYPE**sütunu altındaki **ARO** değerini arayarak listedeki sonuçları tanımlayabilirsiniz.

5. Kapsayıcılar için **Onboarding** to Azure Monitor sayfasında, kümeyle aynı abonelikte mevcut bir Log Analytics çalışma alanınız varsa, açılan listeden seçin.  
    Liste, kümenin abonelikte dağıtılan varsayılan çalışma alanı ve konumunu önceden seçer.

    ![İzlenmeyen kümeler için izlemeyi etkinleştirme](./media/container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >İzleme verilerini kümeden depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız, [Log Analytics çalışma alanı oluştur'daki](../../azure-monitor/learn/quick-create-workspace.md)yönergeleri izleyin. RedHat OpenShift kümesinin dağıtıldığı aynı abonelikte çalışma alanını oluşturduğunuzdan emin olun.

İzlemeyi etkinleştirdikten sonra, kümenin sistem durumu ölçümlerini görüntülemeniz yaklaşık 15 dakika sürebilir.

### <a name="enable-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak etkinleştirme

Bu yöntem iki JSON şablonu içerir. Şablonlardan biri izlemeyi etkinleştirmek için yapılandırmayı belirtir ve diğeri aşağıdakileri belirtmek üzere yapılandırdığınız parametre değerlerini içerir:

- Azure Redhat OpenShift küme kaynak kimliği.

- Kümenin dağıtılan kaynak grubu.

- Log Analytics çalışma alanı.

Bir şablon kullanarak kaynakları dağıtma kavramına aşina değilseniz, bkz:

- [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)

- [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Azure CLI'yi kullanmayı seçerseniz, öncelikle CLI'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI sürümünü 2.0.65 veya sonraki sürümde çalışıyor olmalısınız. Sürümünüzü tanımlamak için `az --version`çalıştırın. Azure CLI'yi yüklemeniz veya yükseltmeniz gerekiyorsa, [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

Azure PowerShell veya CLI kullanarak izlemeyi etkinleştirmeden önce Log Analytics çalışma alanının oluşturulması gerekir. Çalışma alanını oluşturmak için, azure [kaynak yöneticisi](../../azure-monitor/platform/template-workspace-configuration.md)aracılığıyla , [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)üzerinden veya [Azure portalında](../../azure-monitor/learn/quick-create-workspace.md)ayarlayabilirsiniz.

1. Aşağıdaki komutları kullanarak izleme eklentisi ile kümenizi güncelleştirmek için şablon ve parametre dosyasını indirin:

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aro/enable_monitoring_to_existing_cluster/existingClusterOnboarding.json`

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aro/enable_monitoring_to_existing_cluster/existingClusterParam.json`

2. Azure'da oturum açma

    ```azurecli
    az login    
    ```

    Birden çok aboneye erişiminiz `az account set -s {subscription ID}` varsa, `{subscription ID}` kullanmak istediğiniz abonelikle değiştirin.

3. Azure RedHat OpenShift kümesinin aboneliğini belirtin.

    ```azurecli
    az account set --subscription "Subscription Name"  
    ```

4. Küme konumunu ve kaynak kimliğini tanımlamak için aşağıdaki komutu çalıştırın:

    ```azurecli
    az openshift show -g <clusterResourceGroup> -n <clusterName>
    ```

5. **MevcutClusterParam.json** JSON parametre dosyasını düzenleyin ve *araResourceId* ve *araResoruceLocation değerlerini güncelleyin.* **WorkspaceResourceId** için değer, çalışma alanı adını içeren Log Analytics çalışma alanınızın tam kaynak kimliğidir.

6. Azure CLI ile dağıtmak için aşağıdaki komutları çalıştırın:

    ```azurecli
    az group deployment create --resource-group <ClusterResourceGroupName> --template-file ./ExistingClusterOnboarding.json --parameters @./existingClusterParam.json
    ```

    Çıktı aşağıdakilere benzer:

    ```output
    provisioningState       : Succeeded
    ```

## <a name="next-steps"></a>Sonraki adımlar

- RedHat OpenShift kümenizin sistem durumu ve kaynak kullanımını ve üzerlerinde çalışan iş yüklerini toplamak için etkinleştirilen izleme sayesinde, kapsayıcılar için Azure [Monitörü'nü nasıl kullanacağınızı](container-insights-analyze.md) öğrenin.

- Kapsayıcılar için Azure Monitor ile kümenizi izlemeyi nasıl durdurup durduracaklarınız hakkında bilgi için [Azure Red Hat OpenShift kümenizi izlemeyi nasıl durdurana](container-insights-optout-openshift.md)bakın.
