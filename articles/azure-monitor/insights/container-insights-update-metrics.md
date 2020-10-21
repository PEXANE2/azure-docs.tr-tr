---
title: Ölçümler için Azure Izleyicisini güncelleştirme | Microsoft Docs
description: Bu makalede, toplu ölçümler üzerinde araştırmayı ve uyarı vermeyi destekleyen özel ölçümler özelliğini etkinleştirmek üzere kapsayıcılar için Azure Izleyicisini nasıl güncelleştireceğinizi açıklanmaktadır.
ms.topic: conceptual
ms.date: 10/09/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 2a94f250c83fbd2779620376087a83b8851e583e
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92309444"
---
# <a name="how-to-update-azure-monitor-for-containers-to-enable-metrics"></a>Kapsayıcılar için Azure İzleyici'yi ölçümleri etkinleştirecek şekilde güncelleştirme

Kapsayıcılar için Azure Izleyici, Azure Kubernetes Services (AKS) ve Azure Arc etkin Kubernetes kümeleri düğümleri ve yığınlarından ve bunları Azure Izleyici ölçümleri deposuna yazarken ölçüm toplama desteğiyle tanışın. Bu değişiklik, Performans grafiklerinde toplam hesaplamalar (Ort, say, Max, min, Sum) sunarken, performans grafiklerini Azure portal panolara sabitlemeyi destekledikleri ve ölçüm uyarılarını destekledikleri zaman, gelişmiş zaman çizelgesi sunmaya yöneliktir.

>[!NOTE]
>Bu özellik şu anda Azure Red Hat OpenShift kümelerini desteklemez.
>

Bu özelliğin bir parçası olarak aşağıdaki ölçümler etkinleştirilmiştir:

| Ölçüm ad alanı | Ölçüm | Açıklama |
|------------------|--------|-------------|
| Öngörüler. kapsayıcı/düğümler | Cpuusagemiliçekirdekler, cpuUsagePercentage, memoryRssBytes, memoryRssPercentage, memoryWorkingSetBytes, memoryWorkingSetPercentage, nodesCount, diskUsedPercentage, | *Düğüm* ölçümleri olarak *ana bilgisayarı* bir boyut olarak içerirler. Ayrıca şunları içerir<br> *ana bilgisayar* boyutu için değer olarak düğümün adı. |
| Öngörüler. kapsayıcı/pods | Pod Count, completedJobsCount, restartingContainerCount, oomKilledContainerCount, Pod Readypercentage | *Pod* ölçümleri olarak, Boyutlar-ControllerName, Kubernetes ad alanı, ad, aşama olarak aşağıdakileri içerirler. |
| Öngörüler. kapsayıcı/kapsayıcılar | cpuExceededPercentage, memoryRssExceededPercentage, memoryWorkingSetExceededPercentage | |
| Öngörüler. kapsayıcı/persistentvolumes | pvUsageExceededPercentage | |

Bu yeni özellikleri desteklemek için sürüme yeni bir kapsayıcı aracı eklenmiştir. **Microsoft/OMS: ciprod05262020** for aks ve Version **Microsoft/OMS: Ciprod09252020** for Azure Arc, Kubernetes kümeleri için. Yeni AKS dağıtımları otomatik olarak bu yapılandırma değişikliğini ve yeteneklerini içerir. Kümenizi bu özelliği destekleyecek şekilde güncelleştirmek Azure portal, Azure PowerShell veya Azure CLı ile gerçekleştirilebilir. Azure PowerShell ve CLı ile. Bu küme başına veya aboneliğinizdeki tüm kümeler için etkinleştirebilirsiniz.

Her iki işlem de, aracı tarafından toplanan verilerin kümeler kaynağına yayımlanabilmesi için izleme **ölçümleri yayımcı** rolünü kümenin hizmet sorumlusuna veya izleme eklentisi için Kullanıcı tarafından atanmış MSI 'ye atar. Ölçümleri izlemek, yalnızca ölçümleri kaynağa iletmek için izne sahiptir, herhangi bir durumu değiştiremez, kaynağı güncelleştiremez veya herhangi bir veriyi okuyabilir. Rol hakkında daha fazla bilgi için bkz. [ölçümleri Izleme yayımcısı rolü](../../role-based-access-control/built-in-roles.md#monitoring-metrics-publisher). Izleme ölçümleri Yayımcı rolü gereksinimi, Azure Arc etkinleştirilmiş Kubernetes kümeleri için geçerli değildir.

> [!IMPORTANT]
> Gerekli en düşük aracı sürümüne sahip olduklarından, Azure Arc etkin Kubernetes kümeleri için yükseltme gerekli değildir.

## <a name="prerequisites"></a>Ön koşullar

Kümenizi güncelleştirmeden önce aşağıdakileri onaylayın:

* Özel ölçümler yalnızca Azure bölgelerinin bir alt kümesinde kullanılabilir. Desteklenen bölgelerin listesi [burada](../platform/metrics-custom-overview.md#supported-regions)belgelenmiştir.

* Düğüm koleksiyonunu ve pod özel performans ölçümlerini etkinleştirmek için AKS küme kaynağında **[sahip](../../role-based-access-control/built-in-roles.md#owner)** rolünün bir üyesisiniz. Bu gereksinim, Azure Arc etkin Kubernetes kümeleri için geçerlidir.

Azure CLı 'yı kullanmayı seçerseniz, önce CLı 'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLı sürüm 2.0.59 veya üstünü çalıştırıyor olmanız gerekir. Sürümünüzü tanımlamak için öğesini çalıştırın `az --version` . Azure CLı 'yi yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli).

## <a name="upgrade-a-cluster-from-the-azure-portal"></a>Azure portal bir kümeyi yükseltme

Kapsayıcılar için Azure Izleyici tarafından izlenen mevcut AKS kümelerinde, Azure Izleyici 'de birden çok küme görünümündeki veya sol bölmedeki **Öngörüler** ' i seçerek doğrudan kümeden gelen sistem durumunu görüntülemek üzere kümeyi seçtikten sonra portalın en üstünde bir başlık görmeniz gerekir.

![Azure portal AKS küme başlığını yükselt](./media/container-insights-update-metrics/portal-banner-enable-01.png)

**Etkinleştir** ' e tıkladığınızda Kümeyi yükseltme işlemi başlatılır. Bu işlemin tamamlanması birkaç saniye sürebilir ve ilerleme durumunu menüdeki bildirimler ' in altından izleyebilirsiniz.

## <a name="upgrade-all-clusters-using-bash-in-azure-command-shell"></a>Azure komut kabuğu 'nda Bash kullanarak tüm kümeleri yükseltme

Azure komut kabuğu 'nda Bash kullanarak aboneliğinizdeki tüm kümeleri güncelleştirmek için aşağıdaki adımları gerçekleştirin.

1. Azure CLı kullanarak aşağıdaki komutu çalıştırın.  AKS kümesi için **aks genel bakış** sayfasındaki değeri kullanarak **SubscriptionID** değerini düzenleyin.

    ```azurecli
    az login
    az account set --subscription "Subscription Name"
    curl -sL https://aka.ms/ci-md-onboard-atscale | bash -s subscriptionId   
    ```

    Yapılandırma değişikliğinin tamamlanması birkaç saniye sürebilir. Bu tamamlandığında, aşağıdakine benzer bir ileti görüntülenir ve sonuç şunu içerir:

    ```azurecli
    completed role assignments for all AKS clusters in subscription: <subscriptionId>
    ```

## <a name="upgrade-per-cluster-using-azure-cli"></a>Azure CLı kullanarak küme başına yükseltme

Azure CLı kullanarak aboneliğinizdeki belirli bir kümeyi güncelleştirmek için aşağıdaki adımları gerçekleştirin.

1. Azure CLı kullanarak aşağıdaki komutu çalıştırın. AKS kümesi için **aks genel bakış** sayfasındaki değerleri kullanarak **SubscriptionID**, **resourcegroupname**ve **clusterName** değerlerini düzenleyin.  **Clienentidofspn**değerini almak için `az aks show` Aşağıdaki örnekte gösterildiği gibi komutunu çalıştırdığınızda döndürülür.

    ```azurecli
    az login
    az account set --subscription "<subscriptionName>"
    az aks show -g <resourceGroupName> -n <clusterName> 
    az role assignment create --assignee <clientIdOfSPN> --scope <clusterResourceId> --role "Monitoring Metrics Publisher" 
    ```

    **ClientIdOfSPNOrMsi**değerini almak için `az aks show` Aşağıdaki örnekte gösterildiği gibi komutu çalıştırabilirsiniz. **Serviceprincipalprofile** nesnesinin geçerli bir *ClientID* değeri varsa bunu kullanabilirsiniz. Aksi halde, *MSI*olarak ayarlandıysa ClientID ' yi öğesinden geçirmeniz gerekir `addonProfiles.omsagent.identity.clientId` .

    ```azurecli
    az login
    az account set --subscription "<subscriptionName>"
    az aks show -g <resourceGroupName> -n <clusterName> 
    az role assignment create --assignee <clientIdOfSPNOrMsi> --scope <clusterResourceId> --role "Monitoring Metrics Publisher"
    ```

## <a name="upgrade-all-clusters-using-azure-powershell"></a>Azure PowerShell kullanarak tüm kümeleri yükseltme

Azure PowerShell kullanarak aboneliğinizdeki tüm kümeleri güncelleştirmek için aşağıdaki adımları gerçekleştirin.

1. **mdm_onboarding_atscale.ps1** betiğini [indirip](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/docs/aks/mdmonboarding/mdm_onboarding_atscale.ps1) GitHub deponuzdaki yerel bir klasöre kaydedin.
2. Azure PowerShell kullanarak aşağıdaki komutu çalıştırın.  AKS kümesi için **aks genel bakış** sayfasındaki değeri kullanarak **SubscriptionID** değerini düzenleyin.

    ```powershell
    .\mdm_onboarding_atscale.ps1 subscriptionId
    ```
    Yapılandırma değişikliğinin tamamlanması birkaç saniye sürebilir. Bu tamamlandığında, aşağıdakine benzer bir ileti görüntülenir ve sonuç şunu içerir:

    ```powershell
    Completed adding role assignment for the aks clusters in subscriptionId :<subscriptionId>
    ```

## <a name="upgrade-per-cluster-using-azure-powershell"></a>Azure PowerShell kullanarak küme başına yükseltme

Azure PowerShell kullanarak belirli bir kümeyi güncelleştirmek için aşağıdaki adımları gerçekleştirin.

1. **mdm_onboarding.ps1** betiğini [indirip](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/docs/aks/mdmonboarding/mdm_onboarding.ps1) GitHub deponuzdaki yerel bir klasöre kaydedin.

2. Azure PowerShell kullanarak aşağıdaki komutu çalıştırın. AKS kümesi için **aks genel bakış** sayfasındaki değerleri kullanarak **SubscriptionID**, **resourcegroupname**ve **clusterName** değerlerini düzenleyin.

    ```powershell
    .\mdm_onboarding.ps1 subscriptionId <subscriptionId> resourceGroupName <resourceGroupName> clusterName <clusterName>
    ```

    Yapılandırma değişikliğinin tamamlanması birkaç saniye sürebilir. Bu tamamlandığında, aşağıdakine benzer bir ileti görüntülenir ve sonuç şunu içerir:

    ```powershell
    Successfully added Monitoring Metrics Publisher role assignment to cluster : <clusterName>
    ```

## <a name="verify-update"></a>Güncelleştirmeyi doğrula

Daha önce açıklanan yöntemlerden birini kullanarak güncelleştirmeyi başlattıktan sonra, Azure Izleyici ölçümleri Gezginini kullanabilir ve **Öngörüler** tarafından listelenen **ölçüm ad** alanından emin olabilirsiniz. Varsa, devam edebilir, [ölçüm uyarılarını](../platform/alerts-metric.md) ayarlamaya veya grafiklerinizi [panolara](../../azure-portal/azure-portal-dashboards.md)sabitlemeye başlayabilirsiniz.  
