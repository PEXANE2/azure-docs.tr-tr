---
title: Kapsayıcılar için Azure Izleyicisini etkinleştirme | Microsoft Docs
description: Bu makalede, kapsayıcının nasıl çalıştığını ve performansla ilgili sorunları nasıl tanımladığınızı anlayabilmeniz için kapsayıcılar için Azure Izleyicisini etkinleştirme ve yapılandırma açıklanmaktadır.
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: a765c601682eb594d40ba98b8b4ef1853f35fb37
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84886003"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyicisini etkinleştirme

Bu makalede, Kubernetes ortamlarına dağıtılan ve üzerinde barındırılan iş yüklerinin performansını izlemek üzere kapsayıcılar için Azure Izleyici 'yi ayarlama için kullanılabilen seçeneklere genel bir bakış sunulmaktadır:

- [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/) (aks)

- [Aks altyapısı](https://github.com/Azure/aks-engine)kullanılarak Azure üzerinde barındırılan, kendi kendine yönetilen Kubernetes kümeleri.

- AKS altyapısını kullanarak [Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1910) veya şirket içinde barındırılan, kendi kendine yönetilen Kubernetes kümeleri.

- [Azure Red Hat OpenShift](../../openshift/intro-openshift.md) sürüm 3. x ve 4. x

- [Red Hat OpenShift](https://docs.openshift.com/container-platform/4.3/welcome/index.html) sürüm 4. x

- [Yay etkin Kubernetes kümesi](../../azure-arc/kubernetes/overview.md)

Kapsayıcılar için Azure Izleyici, aşağıdaki desteklenen yöntemleri kullanarak yeni veya var olan bir veya daha fazla Kubernetes dağıtımı için etkinleştirilebilir:

- Azure portal, Azure PowerShell veya Azure CLı ile

- [Terrayform ve AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md) kullanma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdakilere sahip olduğunuzdan emin olun:

- **Log Analytics çalışma alanı.**

   Kapsayıcılar için Azure Izleyici, [bölgeye göre Azure ürünlerinde](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor)listelenen bölgelerde bir Log Analytics çalışma alanını destekler.

   Yeni AKS kümenizi izlemeyi etkinleştirdiğinizde bir çalışma alanı oluşturabilir veya ekleme deneyiminin AKS küme aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturmasına izin verin. Kendiniz oluşturmayı seçerseniz, [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)aracılığıyla veya [Azure Portal](../learn/quick-create-workspace.md) [Azure Resource Manager](../platform/template-workspace-configuration.md)aracılığıyla oluşturabilirsiniz. Varsayılan çalışma alanı için kullanılan desteklenen eşleme çiftlerinin bir listesi için bkz. [kapsayıcılar Için Azure izleyici Için bölge eşleme](container-insights-region-mapping.md).

- Kapsayıcı izlemeyi etkinleştirmek için **Log Analytics katkıda bulunan rolünün** bir üyesisiniz. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../platform/manage-access.md).

- AKS küme kaynağında **[sahip](../../role-based-access-control/built-in-roles.md#owner)** rolünün bir üyesisiniz.

   [!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

- Prometheus ölçümleri varsayılan olarak toplanmaz. Aracıyı toplamak üzere [yapılandırmadan](container-insights-prometheus-integration.md) önce, nelerin desteklenebilir olduğunu ve yöntemlerin desteklenip desteklenmediğini anlamak Için Prometheus [belgelerini](https://prometheus.io/) gözden geçirmeniz önemlidir.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

Kapsayıcılar için Azure Izleyici ile resmi olarak şunlar desteklenir.

- Ortamlar: Azure Red Hat OpenShift, Kubernetes on-premises ve AKS Engine for Azure ve Azure Stack. Daha fazla bilgi için bkz. [Azure Stack aks motoru](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908).
- Kubernetes ve destek ilkesi sürümleri [desteklenen aks](../../aks/supported-kubernetes-versions.md)sürümleriyle aynıdır. 

## <a name="network-firewall-requirements"></a>Ağ güvenlik duvarı gereksinimleri

Aşağıdaki tablodaki bilgiler Kapsayıcılı aracının kapsayıcılar için Azure Izleyici ile iletişim kurması için gereken proxy ve güvenlik duvarı yapılandırma bilgilerini listelemektedir. Aracıdan gelen tüm ağ trafiği Azure Izleyici 'ye giden.

|Aracı Kaynağı|Bağlantı noktaları |
|--------------|------|
| `*.ods.opinsights.azure.com` | 443 |  
| `*.oms.opinsights.azure.com` | 443 |
| `dc.services.visualstudio.com` | 443 |
| `*.monitoring.azure.com` | 443 |
| `login.microsoftonline.com` | 443 |

Aşağıdaki tablodaki bilgiler, Azure Çin 21Vianet için proxy ve güvenlik duvarı yapılandırma bilgilerini listelemektedir.

|Aracı Kaynağı|Bağlantı noktaları |Description | 
|--------------|------|-------------|
| `*.ods.opinsights.azure.cn` | 443 | Veri alımı |
| `*.oms.opinsights.azure.cn` | 443 | OMS ekleme |
| `dc.services.visualstudio.com` | 443 | Azure genel bulut Application Insights kullanan aracı telemetri için. |

Aşağıdaki tablodaki bilgiler, Azure ABD kamu için proxy ve güvenlik duvarı yapılandırma bilgilerini listelemektedir.

|Aracı Kaynağı|Bağlantı noktaları |Description | 
|--------------|------|-------------|
| `*.ods.opinsights.azure.us` | 443 | Veri alımı |
| `*.oms.opinsights.azure.us` | 443 | OMS ekleme |
| `dc.services.visualstudio.com` | 443 | Azure genel bulut Application Insights kullanan aracı telemetrisi için. |

## <a name="components"></a>Bileşenler

Performansı izleme özelliği, özellikle kapsayıcılar için Azure Izleyici için geliştirilmiş, Linux için kapsayıcılı bir Log Analytics aracısına dayanır. Bu özelleştirilmiş aracı kümedeki tüm düğümlerden performans ve olay verilerini toplar ve Aracı, dağıtım sırasında belirtilen Log Analytics çalışma alanıyla otomatik olarak dağıtılır ve kaydedilir. Aracı sürümü Microsoft/OMS: ciprod04202018 veya üzeri olur ve şu biçimdeki bir tarihle temsil edilir: *mmddyyyy*.

>[!NOTE]
>AKS için Windows Server desteğinin önizleme sürümü sayesinde, Windows Server düğümlerine sahip bir AKS kümesi, veri toplamak ve Azure Izleyici 'ye iletmek için bir aracı yüklü değildir. Bunun yerine, standart dağıtımın bir parçası olarak kümede otomatik olarak dağıtılan bir Linux düğümü, verileri kümedeki tüm Windows düğümleri adına toplayıp Azure Izleyici 'ye iletir.  
>

Aracının yeni bir sürümü yayınlandığında, Azure Kubernetes Service (AKS) üzerinde barındırılan yönetilen Kubernetes kümelerinizde otomatik olarak yükseltilir. Yayınlanan sürümleri izlemek için bkz. [Aracı Sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

>[!NOTE]
>Zaten bir AKS kümesi dağıttıysanız, bu makalede daha sonra gösterildiği gibi Azure CLı veya sağlanmış Azure Resource Manager şablonu kullanarak izlemeyi etkinleştirirsiniz. `kubectl`Aracıyı yükseltmek, silmek, yeniden dağıtmak veya dağıtmak için kullanamazsınız.
>Şablonun, kümeyle aynı kaynak grubunda dağıtılması gerekir.

Aşağıdaki tabloda açıklanan aşağıdaki yöntemlerden birini kullanarak kapsayıcılar için Azure Izleyicisini etkinleştirirsiniz.

| Dağıtım durumu | Yöntem | Description |
|------------------|--------|-------------|
| Yeni Kubernetes kümesi | [Azure CLı kullanarak AKS kümesi oluşturma](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Azure CLı ile oluşturduğunuz yeni bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Terrayform kullanarak AKS kümesi oluşturma](container-insights-enable-new-cluster.md#enable-using-terraform)| Açık kaynak aracı Tertuform kullanarak oluşturduğunuz yeni bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Azure Resource Manager şablonu kullanarak OpenShift kümesi oluşturma](container-insights-azure-redhat-setup.md#enable-for-a-new-cluster-using-an-azure-resource-manager-template) | Önceden yapılandırılmış bir Azure Resource Manager şablonuyla oluşturduğunuz yeni bir OpenShift kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Azure CLı kullanarak OpenShift kümesi oluşturma](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create) | Azure CLı kullanarak yeni bir OpenShift kümesi dağıttığınızda izlemeyi etkinleştirebilirsiniz. |
| Mevcut Kubernetes kümesi | [Azure CLı kullanarak AKS kümesi için etkinleştirme](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Azure CLı kullanarak zaten dağıtılmış bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| |[Tertuform kullanarak AKS kümesi için etkinleştirme](container-insights-enable-existing-clusters.md#enable-using-terraform) | Açık kaynaklı araç Tertuform kullanarak zaten dağıtılmış bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Azure Izleyici 'den AKS kümesi için etkinleştirme](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Azure Izleyici 'de çok küme sayfasından zaten dağıtılmış bir veya daha fazla AKS kümesi izlemeyi etkinleştirebilirsiniz. |
| | [AKS kümesinden etkinleştir](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| İzlemeyi, Azure portal doğrudan bir AKS kümesinden etkinleştirebilirsiniz. |
| | [Azure Resource Manager şablonu kullanarak AKS kümesi için etkinleştirme](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| Bir AKS kümesinin önceden yapılandırılmış Azure Resource Manager şablonuyla izlenmesini etkinleştirebilirsiniz. |
| | [Karma Kubernetes kümesi için etkinleştir](container-insights-hybrid-setup.md) | Azure Stack veya şirket içinde barındırılan Kubernetes için barındırılan bir AKS altyapısının izlenmesini etkinleştirebilirsiniz. |
| | [Yay etkin Kubernetes kümesi Için etkinleştirin](container-insights-enable-arc-enabled-clusters.md). | Azure 'un dışında barındırılan Kubernetes kümelerinizin izlenmesini, Azure Arc ile etkin hale getirebilirsiniz. |
| | [Azure Resource Manager şablonu kullanarak OpenShift kümesi için etkinleştirme](container-insights-azure-redhat-setup.md#enable-using-an-azure-resource-manager-template) | Mevcut bir OpenShift kümesini önceden yapılandırılmış Azure Resource Manager şablonuyla izlemeyi etkinleştirebilirsiniz. |
| | [Azure Izleyici 'den OpenShift kümesi için etkinleştirme](container-insights-azure-redhat-setup.md#from-the-azure-portal) | Azure Izleyici 'de çok küme sayfasından zaten dağıtılmış olan bir veya daha fazla OpenShift kümesi izlemeyi etkinleştirebilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar

- İzleme etkinken, Azure Kubernetes Service (AKS), Azure Stack veya başka bir ortamda barındırılan Kubernetes kümelerinin performansını çözümlemeye başlayabilirsiniz. Kapsayıcılar için Azure Izleyici 'yi nasıl kullanacağınızı öğrenmek için bkz. [Kubernetes küme performansını görüntüleme](container-insights-analyze.md).
