---
title: Kapsayıcılar için Azure Izleyicisini etkinleştirme | Microsoft Docs
description: Bu makalede, kapsayıcının nasıl çalıştığını ve performansla ilgili sorunları nasıl tanımladığınızı anlayabilmeniz için kapsayıcılar için Azure Izleyicisini etkinleştirme ve yapılandırma açıklanmaktadır.
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: d85dd4f1eb89ddba96ec012acb7fb7550800ce7f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800639"
---
# <a name="enable-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyicisini etkinleştirme

Bu makalede, Kubernetes ortamlarına dağıtılan ve üzerinde barındırılan iş yüklerinin performansını izlemek üzere kapsayıcılar için Azure Izleyicisini ayarlamaya yönelik seçeneklere genel bir bakış sunulmaktadır:

- [Azure Kubernetes Hizmeti (AKS)](https://docs.microsoft.com/azure/aks/)  
- [Azure Red Hat OpenShift](../../openshift/intro-openshift.md) sürümleri 3. x ve 4. x  
- [Red Hat OpenShift](https://docs.openshift.com/container-platform/4.3/welcome/index.html) sürüm 4. x  
- [Yay etkin bir Kubernetes kümesi](../../azure-arc/kubernetes/overview.md)

Ayrıca, üzerinde barındırılan kendi kendine yönetilen Kubernetes kümelerine dağıtılan iş yüklerinin performansını izleyebilirsiniz:
- Azure, [aks altyapısını](https://github.com/Azure/aks-engine) kullanarak
- AKS altyapısını kullanarak [Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1910) veya şirket içinde.

Aşağıdaki desteklenen yöntemlerden birini kullanarak yeni bir dağıtım veya bir veya daha fazla mevcut Kubernetes dağıtımı için Azure Izleyicisini etkinleştirebilirsiniz:

- Azure portal
- Azure PowerShell
- Azure CLI
- [Terrayform ve AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdaki gereksinimleri karşıladığınızdan emin olun:

- Log Analytics bir çalışma alanınız var.

   Kapsayıcılar için Azure Izleyici, [bölgeye göre kullanılabilir ürünlerde](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor)listelenen bölgelerde Log Analytics çalışma alanını destekler.

   Yeni AKS kümeniz için izlemeyi etkinleştirdiğinizde bir çalışma alanı oluşturabilir veya ekleme deneyiminin AKS kümesi aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturmasına izin verebilirsiniz. 
   
   Çalışma alanını kendiniz oluşturmayı seçerseniz, şunu kullanarak oluşturabilirsiniz: 
   - [Azure Resource Manager](../platform/template-workspace-configuration.md)
   - [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)
   - [Azure portal](../learn/quick-create-workspace.md) 
   
   Varsayılan çalışma alanı için kullanılmak üzere desteklenen eşleme çiftlerinin bir listesi için bkz. [kapsayıcılar Için Azure izleyici Için bölge eşleme](container-insights-region-mapping.md).

- Kapsayıcı izlemeyi etkinleştirmek için *Log Analytics katkıda* bulunan grubunun bir üyesisiniz. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../platform/manage-access.md).

- AKS küme kaynağında [ *sahip* grubunun](../../role-based-access-control/built-in-roles.md#owner) bir üyesisiniz.

   [!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

- İzleme verilerini görüntülemek için, kapsayıcılar için Azure Izleyici ile yapılandırılmış Log Analytics çalışma alanında [*Log Analytics okuyucu*](../platform/manage-access.md#manage-access-using-azure-permissions) rolüne sahip olmanız gerekir.

- Prometheus ölçümleri varsayılan olarak toplanmaz. Bir aracıyı ölçümleri toplayacak şekilde [yapılandırmadan](container-insights-prometheus-integration.md) önce, hangi verilerin hangi verileri desteklebileceğini ve hangi yöntemlerin desteklendiğini anlamak Için [Prometheus belgelerini](https://prometheus.io/) gözden geçirmeniz önemlidir.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

Kapsayıcılar için Azure Izleyici, resmi olarak aşağıdaki konfigürasyonları destekler:

- Ortamlar: Azure Red Hat OpenShift, Kubernetes, şirket içi ve Azure 'da AKS motoru ve Azure Stack. Daha fazla bilgi için [Azure Stack üzerindeki AKS altyapısına](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908)bakın.
- Kubernetes ve destek ilkesi sürümleri, [Azure Kubernetes hizmeti (AKS) ' de desteklenenlerle](../../aks/supported-kubernetes-versions.md)aynıdır. 

## <a name="network-firewall-requirements"></a>Ağ güvenlik duvarı gereksinimleri

Aşağıdaki tabloda Kapsayıcılı aracının kapsayıcılar için Azure Izleyici ile iletişim kurması için gereken proxy ve güvenlik duvarı yapılandırma bilgileri listelenmektedir. Aracıdan gelen tüm ağ trafiği Azure Izleyici 'ye giden.

|Aracı kaynağı|Bağlantı noktası |
|--------------|------|
| `*.ods.opinsights.azure.com` | 443 |
| `*.oms.opinsights.azure.com` | 443 |
| `dc.services.visualstudio.com` | 443 |
| `*.monitoring.azure.com` | 443 |
| `login.microsoftonline.com` | 443 |

Aşağıdaki tabloda, Azure Çin 21Vianet için proxy ve güvenlik duvarı yapılandırma bilgileri listelenmektedir:

|Aracı kaynağı|Bağlantı noktası |Açıklama | 
|--------------|------|-------------|
| `*.ods.opinsights.azure.cn` | 443 | Veri alımı |
| `*.oms.opinsights.azure.cn` | 443 | OMS ekleme |
| `dc.services.visualstudio.com` | 443 | Azure genel bulutu kullanan aracı telemetrisi için Application Insights |

Aşağıdaki tabloda, Azure ABD kamu için proxy ve güvenlik duvarı yapılandırma bilgileri listelenmektedir:

|Aracı kaynağı|Bağlantı noktası |Açıklama | 
|--------------|------|-------------|
| `*.ods.opinsights.azure.us` | 443 | Veri alımı |
| `*.oms.opinsights.azure.us` | 443 | OMS ekleme |
| `dc.services.visualstudio.com` | 443 | Azure genel bulutu kullanan aracı telemetrisi için Application Insights |

## <a name="components"></a>Bileşenler

Performansı izleme özelliği, özellikle kapsayıcılar için Azure Izleyici için geliştirilmiş, Linux için kapsayıcılı bir Log Analytics aracısına dayanır. Bu özelleştirilmiş aracı kümedeki tüm düğümlerden performans ve olay verilerini toplar ve Aracı, dağıtım sırasında belirtilen Log Analytics çalışma alanıyla otomatik olarak dağıtılır ve kaydedilir. 

Aracı sürümü Microsoft/OMS: ciprod04202018 veya üzeri ve aşağıdaki biçimde bir tarih ile temsil edilir: *mmddyyyy*.

>[!NOTE]
>AKS için Windows Server desteğinin genel kullanıma sunulmasıyla birlikte, Windows Server düğümlerine sahip bir AKS kümesi, günlükleri toplamak ve Log Analytics iletmek için her bir Windows Server düğümünde daemonset Pod olarak yüklenen bir önizleme aracısına sahiptir. Performans ölçümleri için, standart dağıtımın bir parçası olarak kümede otomatik olarak dağıtılan bir Linux düğümü, kümedeki tüm Windows düğümleri adına verileri toplar ve Azure Izleyici 'ye iletir.

Aracının yeni bir sürümü yayınlandığında, Azure Kubernetes Service (AKS) üzerinde barındırılan yönetilen Kubernetes kümelerinizde otomatik olarak yükseltilir. Hangi sürümlerin Yayınlanma izleneceğini izlemek için bkz. [Aracı Sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

> [!NOTE]
> Zaten bir AKS kümesi dağıttıysanız, bu makalede daha sonra gösterildiği gibi Azure CLı veya sağlanan bir Azure Resource Manager şablonu kullanarak izlemeyi etkinleştirdiniz. `kubectl`Aracıyı yükseltmek, silmek, yeniden dağıtmak veya dağıtmak için kullanamazsınız.
>
> Şablonun, kümeyle aynı kaynak grubunda dağıtılması gerekir.

Kapsayıcılar için Azure Izleyicisini etkinleştirmek üzere aşağıdaki tabloda açıklanan yöntemlerden birini kullanın:

| Dağıtım durumu | Yöntem | Açıklama |
|------------------|--------|-------------|
| Yeni Kubernetes kümesi | [Azure CLı kullanarak AKS kümesi oluşturma](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Azure CLı kullanarak oluşturduğunuz yeni bir AKS kümesi için izlemeyi etkinleştirebilirsiniz. |
| | [Terrayform kullanarak AKS kümesi oluşturma](container-insights-enable-new-cluster.md#enable-using-terraform)| Açık kaynak aracı Tertuform kullanarak oluşturduğunuz yeni bir AKS kümesi için izlemeyi etkinleştirebilirsiniz. |
| | [Azure Resource Manager şablonu kullanarak bir OpenShift kümesi oluşturma](container-insights-azure-redhat-setup.md#enable-for-a-new-cluster-using-an-azure-resource-manager-template) | Önceden yapılandırılmış bir Azure Resource Manager şablonu kullanarak oluşturduğunuz yeni bir OpenShift kümesi için izlemeyi etkinleştirebilirsiniz. |
| | [Azure CLı kullanarak bir OpenShift kümesi oluşturma](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create) | Azure CLı kullanarak yeni bir OpenShift kümesi dağıtırken izlemeyi etkinleştirebilirsiniz. |
| Mevcut Kubernetes kümesi | [Azure CLı kullanarak bir AKS kümesinin izlenmesini etkinleştirme](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Azure CLı kullanılarak zaten dağıtılmış bir AKS kümesi için izlemeyi etkinleştirebilirsiniz. |
| |[Tertuform kullanarak AKS kümesi için etkinleştirme](container-insights-enable-existing-clusters.md#enable-using-terraform) | Açık kaynak aracı Tertuform kullanılarak zaten dağıtılmış olan bir AKS kümesi için izlemeyi etkinleştirebilirsiniz. |
| | [Azure Izleyici 'den AKS kümesi için etkinleştirme](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Azure Izleyici 'de çok küme sayfasından zaten dağıtılmış olan bir veya daha fazla AKS kümesi için izlemeyi etkinleştirebilirsiniz. |
| | [AKS kümesinden etkinleştir](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| İzlemeyi, Azure portal doğrudan bir AKS kümesinden etkinleştirebilirsiniz. |
| | [Azure Resource Manager şablonu kullanarak AKS kümesi için etkinleştirme](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| Önceden yapılandırılmış bir Azure Resource Manager şablonu kullanarak bir AKS kümesi için izlemeyi etkinleştirebilirsiniz. |
| | [Karma Kubernetes kümesi için etkinleştir](container-insights-hybrid-setup.md) | Azure Stack veya şirket içinde barındırılan bir Kubernetes kümesi için barındırılan AKS motoru için izlemeyi etkinleştirebilirsiniz. |
| | [Yay etkin Kubernetes kümesi Için etkinleştirin](container-insights-enable-arc-enabled-clusters.md). | Azure dışında barındırılan ve Azure Arc ile etkinleştirilen Kubernetes kümeleriniz için izlemeyi etkinleştirebilirsiniz. |
| | [Azure Resource Manager şablonu kullanarak OpenShift kümesi için etkinleştirme](container-insights-azure-redhat-setup.md#enable-using-an-azure-resource-manager-template) | Önceden yapılandırılmış bir Azure Resource Manager şablonu kullanarak var olan bir OpenShift kümesi için izlemeyi etkinleştirebilirsiniz. |
| | [Azure Izleyici 'den OpenShift kümesi için etkinleştirme](container-insights-azure-redhat-setup.md#from-the-azure-portal) | Azure Izleyici 'de birden çok Luster sayfasından zaten dağıtılmış olan bir veya daha fazla OpenShift kümesi için izlemeyi etkinleştirebilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar

İzlemeyi etkinleştirmiş olduğunuza göre, Azure Kubernetes Service (AKS), Azure Stack veya başka bir ortamda barındırılan Kubernetes kümelerinin performansını çözümlemeye başlayabilirsiniz. Kapsayıcılar için Azure Izleyici 'yi nasıl kullanacağınızı öğrenmek için bkz. [Kubernetes küme performansını görüntüleme](container-insights-analyze.md).
