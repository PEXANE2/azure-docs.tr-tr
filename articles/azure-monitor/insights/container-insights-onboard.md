---
title: Kapsayıcılar için Azure Monitörü nasıl etkinleştirilir | Microsoft Dokümanlar
description: Bu makalede, kapsayıcınızın nasıl performans gösterdiğini ve performansla ilgili hangi sorunların tanımlandığını anlayabilmeniz için kapsayıcılar için Azure Monitor'u nasıl etkinleştirdiğinizi ve yapılandırdığınız açıklanmaktadır.
ms.topic: conceptual
ms.date: 11/18/2019
ms.openlocfilehash: 7aad7e7dd5ec2569377f9276c2e4793c7afd631a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275314"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitörü etkinleştirme

Bu makalede, Kubernetes ortamlarına dağıtılan ve barındırılan iş yüklerinin performansını izlemek için kapsayıcılar için Azure Monitor kurulumu için kullanılabilen seçeneklere genel bir bakış sağlanmıştır:

- [Azure Kubernetes Servisi](https://docs.microsoft.com/azure/aks/) (AKS)

- [AKS Engine](https://github.com/Azure/aks-engine)kullanarak Azure'da barındırılan kendi kendini yöneten Kubernetes kümeleri.

- [Azure Yığını'nda](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1910) veya AKS Engine kullanarak şirket içinde barındırılan kendi kendini yöneten Kubernetes kümeleri.

- [Azure Red Hat OpenShift](../../openshift/intro-openshift.md)

Kapsayıcılar için Azure Monitörü, desteklenen yöntemler kullanılarak Kubernetes'in yeni veya bir veya daha fazla varolan dağıtımı için etkinleştirilebilir:

- Azure portalı Azure PowerShell'den veya Azure CLI ile

- [Terraform ve AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md) kullanma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakileri yaptığınızdan emin olun:

- **Log Analytics çalışma alanı.**

    Kapsayıcılar için Azure Monitörü, bölgeye göre Azure [Ürünleri'nde](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor)listelenen bölgelerde ki Günlük Analizi çalışma alanını destekler.

    Yeni AKS kümenizin izlenmesini etkinleştirdiğinizde veya onboarding deneyiminin AKS küme aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturmasına izin verdiğinde bir çalışma alanı oluşturabilirsiniz. Kendiniz oluşturmayı seçtiyseniz, Azure Kaynak [Yöneticisi](../platform/template-workspace-configuration.md)aracılığıyla , [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)aracılığıyla veya [Azure portalında](../learn/quick-create-workspace.md)oluşturabilirsiniz. Varsayılan çalışma alanı için kullanılan desteklenen eşleme çiftlerinin listesi [için kapsayıcılar için Azure Monitörü için Bölge eşleme bölümüne](container-insights-region-mapping.md)bakın.

- Konteyner izlemeyi etkinleştirmek için **Log Analytics katılımcısı rolünün** bir üyesisiniz. Günlük Analizi çalışma alanına erişimi nasıl denetleyecekleri hakkında daha fazla bilgi için [bkz.](../platform/manage-access.md)

- AKS küme kaynağındaki **[Sahip](../../role-based-access-control/built-in-roles.md#owner)** rolünün bir üyesisiniz.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

* Prometheus ölçümleri varsayılan olarak toplanmaz. Aracıyı bunları toplamak için [yapılandırmadan](container-insights-prometheus-integration.md) önce, neyin kazınabileceğini ve desteklenen yöntemleri anlamak için Prometheus [belgelerini](https://prometheus.io/) gözden geçirmeniz önemlidir.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

Aşağıdakiler kapsayıcılar için Azure Monitor ile resmi olarak desteklenir.

- Ortamlar: Azure Red Hat OpenShift, Kubernetes şirket içi ve Azure ve Azure Yığını'nda AKS Motoru. Daha fazla bilgi için [Azure Yığını'ndaki AKS Engine'e](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908)bakın.
- Kubernetes sürümleri ve destek ilkesi [AKS desteklenen](../../aks/supported-kubernetes-versions.md)sürümleri ile aynıdır. 

## <a name="network-firewall-requirements"></a>Ağ güvenlik duvarı gereksinimleri

Aşağıdaki tablodaki bilgiler, kapsayıcı aracının kapsayıcılar için Azure Monitor ile iletişim kurması için gereken proxy ve güvenlik duvarı yapılandırma bilgilerini listeler. Aracıdan gelen tüm ağ trafiği Azure Monitor'a giden.

|Aracı Kaynağı|Bağlantı Noktaları |
|--------------|------|
| *.ods.opinsights.azure.com | 443 |  
| *.oms.opinsights.azure.com | 443 | 
| *.blob.core.windows.net | 443 |
| dc.services.visualstudio.com | 443 |
| *.microsoftonline.com | 443 |
| *.monitoring.azure.com | 443 |
| login.microsoftonline.com | 443 |

Aşağıdaki tablodaki bilgiler, Azure Çin'in proxy ve güvenlik duvarı yapılandırma bilgilerini listeler.

|Aracı Kaynağı|Bağlantı Noktaları |Açıklama | 
|--------------|------|-------------|
| *.ods.opinsights.azure.cn | 443 | Veri alımı |
| *.oms.opinsights.azure.cn | 443 | OMS onboarding |
| *.blob.core.windows.net | 443 | Giden bağlantının izlenmesi için kullanılır. |
| microsoft.com | 80 | Ağ bağlantısı için kullanılır. Bu yalnızca aracı görüntü sürümü ciprod09262019 veya daha önce yse gereklidir. |
| dc.services.visualstudio.com | 443 | Azure Genel Bulut Uygulama Öngörüleri'ni kullanarak aracı telemetrisi için. |

Aşağıdaki tablodaki bilgiler, Azure ABD Hükümeti için proxy ve güvenlik duvarı yapılandırma bilgilerini listeler.

|Aracı Kaynağı|Bağlantı Noktaları |Açıklama | 
|--------------|------|-------------|
| *.ods.opinsights.azure.us | 443 | Veri alımı |
| *.oms.opinsights.azure.us | 443 | OMS onboarding |
| *.blob.core.windows.net | 443 | Giden bağlantının izlenmesi için kullanılır. |
| microsoft.com | 80 | Ağ bağlantısı için kullanılır. Bu yalnızca aracı görüntü sürümü ciprod09262019 veya daha önce yse gereklidir. |
| dc.services.visualstudio.com | 443 | Azure Genel Bulut Uygulama Öngörülerini kullanarak aracı telemetrisi için. |

## <a name="components"></a>Bileşenler

Performansı izleme yeteneğiniz, kapsayıcılar için Azure Monitor için özel olarak geliştirilmiş linux için konteynerleştirilmiş bir Log Analytics aracısına dayanır. Bu özel aracı kümedeki tüm düğümlerden performans ve olay verileri toplar ve aracı dağıtım sırasında otomatik olarak dağıtılır ve belirtilen Log Analytics çalışma alanına kaydedilir. Aracı sürümü microsoft/oms:ciprod04202018 veya daha sonraki bir tarih le temsil edilir: *mmddyyyy*.

>[!NOTE]
>AKS için Windows Server desteğinin önizleme sürümüyle, Windows Server düğümlerine sahip bir AKS kümesinde veri toplamak ve Azure Monitor'a iletmek için yüklü bir aracı yoktur. Bunun yerine, standart dağıtımın bir parçası olarak kümede otomatik olarak dağıtılan bir Linux düğümü, kümedeki tüm Windows düğümleri adına verileri toplar ve Azure Monitor'a iletir.  
>

Aracının yeni bir sürümü yayımlandığında, Azure Kubernetes Hizmeti'nde (AKS) barındırılan yönetilen Kubernetes kümelerinde otomatik olarak yükseltilir. Yayımlanan sürümleri takip etmek [için, aracı sürüm duyurularına](https://github.com/microsoft/docker-provider/tree/ci_feature_prod)bakın.

>[!NOTE]
>Bir AKS kümesini zaten dağıttıysanız, bu makalede daha sonra gösterildiği gibi Azure CLI veya sağlanan Azure Kaynak Yöneticisi şablonu kullanarak izlemeyi etkinleştirin. Aracıyı `kubectl` yükseltmek, silmek, yeniden dağıtmak veya dağıtmak için kullanamazsınız.
>Şablonun kümeyle aynı kaynak grubunda dağıtılması gerekir.

Aşağıdaki tabloda açıklanan yöntemlerden birini kullanarak kapsayıcılar için Azure Monitor'u etkinleştirirsiniz.

| Dağıtım Durumu | Yöntem | Açıklama |
|------------------|--------|-------------|
| Yeni AKS Kubernetes kümesi | [Azure CLI'yi kullanarak AKS kümesi oluşturma](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Azure CLI ile oluşturduğunuz yeni bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Terraform kullanarak AKS kümesi oluşturma](container-insights-enable-new-cluster.md#enable-using-terraform)| Açık kaynak aracı Terraform kullanarak oluşturduğunuz yeni bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Azure Kaynak Yöneticisi şablonu kullanarak OpenShift kümesi oluşturma](container-insights-azure-redhat-setup.md#enable-for-a-new-cluster-using-an-azure-resource-manager-template) | Önceden yapılandırılmış bir Azure Kaynak Yöneticisi şablonuyla oluşturduğunuz yeni bir OpenShift kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Azure CLI'yi kullanarak OpenShift kümesi oluşturma](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create) | Azure CLI'yi kullanarak yeni bir OpenShift kümesini dağıtırken izlemeyi etkinleştirebilirsiniz. |
| Mevcut AKS Kubernetes kümesi | [Azure CLI kullanarak AKS kümesi için etkinleştirme](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Azure CLI kullanarak zaten dağıtılan bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| |[Terraform kullanarak AKS kümesi için etkinleştirin](container-insights-enable-existing-clusters.md#enable-using-terraform) | Açık kaynak aracı Terraform'u kullanarak zaten dağıtılan bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Azure Monitor'dan AKS kümesi için etkinleştirme](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Azure Monitor'daki çoklu küme sayfasından zaten dağıtılmış bir veya daha fazla AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [AKS kümesinden etkinleştirme](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| Azure portalındaki bir AKS kümesinden doğrudan izleme yi etkinleştirebilirsiniz. |
| | [Azure Kaynak Yöneticisi şablonu kullanarak AKS kümesi ni etkinleştirme](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| Önceden yapılandırılmış Bir Azure Kaynak Yöneticisi şablonuyla bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Hibrit Kubernetes kümesi için etkinleştirme](container-insights-hybrid-setup.md) | Azure Yığını'nda veya şirket içinde barındırılan Kubernetes için barındırılan bir AKS Motorunun izlenmesini etkinleştirebilirsiniz. |
| | [Azure Kaynak Yöneticisi şablonu kullanarak OpenShift kümesini etkinleştirme](container-insights-azure-redhat-setup.md#enable-using-an-azure-resource-manager-template) | Önceden yapılandırılmış bir Azure Kaynak Yöneticisi şablonuyla varolan bir OpenShift kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Azure Monitör'den OpenShift kümesi ni etkinleştirme](container-insights-azure-redhat-setup.md#from-the-azure-portal) | Azure Monitor'daki çok kümeli sayfadan zaten dağıtılmış bir veya daha fazla OpenShift kümesinin izlenmesini etkinleştirebilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar

- İzleme etkinleştirildiğinde, Azure Kubernetes Hizmeti (AKS), Azure Yığını veya diğer ortamda barındırılan Kubernetes kümelerinizin performansını analiz etmeye başlayabilirsiniz. Kapsayıcılar için Azure Monitörünü nasıl kullanacağınızı öğrenmek için [Bkz. Kubernetes küme performansını görüntüle.](container-insights-analyze.md)
