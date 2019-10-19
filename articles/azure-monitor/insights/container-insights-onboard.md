---
title: Kapsayıcılar için Azure Izleyicisini etkinleştirme | Microsoft Docs
description: Bu makalede, kapsayıcının nasıl çalıştığını ve performansla ilgili sorunları nasıl tanımladığınızı anlayabilmeniz için kapsayıcılar için Azure Izleyicisini etkinleştirme ve yapılandırma açıklanmaktadır.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 07/12/2019
ms.openlocfilehash: 44cdc2d6b93ac9a62f96875ca6c679fbb97d85a9
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555391"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyicisini etkinleştirme

Bu makalede, Kubernetes ortamlarına dağıtılan ve [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/)'nde barındırılan iş yüklerinin performansını izlemek üzere kapsayıcılar Için Azure izleyici 'yi ayarlama için kullanılabilen seçeneklere bir genel bakış sunulmaktadır.

Kapsayıcılar için Azure Izleyici, aşağıdaki desteklenen yöntemleri kullanarak yeni veya bir veya daha fazla AKS dağıtımı için etkinleştirilebilir:

* Azure portal, Azure PowerShell veya Azure CLı ile
* [Terrayform ve AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md) kullanma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce, aşağıdakilere sahip olduğunuzdan emin olun:

* **Log Analytics çalışma alanı.**

    Kapsayıcılar için Azure Izleyici, [bölgeye göre Azure ürünlerinde](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor)listelenen bölgelerde bir Log Analytics çalışma alanını destekler.

    Yeni AKS kümenizi izlemeyi etkinleştirdiğinizde bir çalışma alanı oluşturabilir veya ekleme deneyiminin AKS küme aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturmasına izin verin. Kendiniz oluşturmayı seçerseniz, [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)aracılığıyla veya [Azure Portal](../learn/quick-create-workspace.md) [Azure Resource Manager](../platform/template-workspace-configuration.md)aracılığıyla oluşturabilirsiniz. Varsayılan çalışma alanı için kullanılan desteklenen eşleme çiftlerinin bir listesi için bkz. [kapsayıcılar Için Azure izleyici Için bölge eşleme](container-insights-region-mapping.md).

* Kapsayıcı izlemeyi etkinleştirmek için **Log Analytics katkıda bulunan rolünün** bir üyesisiniz. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../platform/manage-access.md).

* AKS küme kaynağında **[sahip](../../role-based-access-control/built-in-roles.md#owner)** rolünün bir üyesisiniz.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

* Prometheus ölçümleri varsayılan olarak toplanmaz. Aracıyı toplayacak şekilde [yapılandırmadan](container-insights-agent-config.md) önce, neleri tanımlayabileceğinizi anlamak Için Prometheus [belgelerini](https://prometheus.io/) gözden geçirmeniz önemlidir.

## <a name="components"></a>Bileşenler

Performansı izleme özelliği, özellikle kapsayıcılar için Azure Izleyici için geliştirilmiş, Linux için kapsayıcılı bir Log Analytics aracısına dayanır. Bu özelleştirilmiş aracı kümedeki tüm düğümlerden performans ve olay verilerini toplar ve Aracı, dağıtım sırasında belirtilen Log Analytics çalışma alanıyla otomatik olarak dağıtılır ve kaydedilir. Aracı sürümü Microsoft/OMS: ciprod04202018 veya üzeri olur ve şu biçimdeki bir tarihle temsil edilir: *mmddyyyy*.

>[!NOTE]
>AKS için Windows Server desteğinin önizleme sürümü sayesinde, Windows Server düğümlerine sahip bir AKS kümesi, veri toplamak ve Azure Izleyici 'ye iletmek için bir aracı yüklü değildir. Bunun yerine, standart dağıtımın bir parçası olarak kümede otomatik olarak dağıtılan bir Linux düğümü, verileri kümedeki tüm Windows düğümleri adına toplayıp Azure Izleyici 'ye iletir.  
>

Aracının yeni bir sürümü yayınlandığında, Azure Kubernetes Service (AKS) üzerinde barındırılan yönetilen Kubernetes kümelerinizde otomatik olarak yükseltilir. Yayınlanan sürümleri izlemek için bkz. [Aracı Sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

>[!NOTE]
>Zaten bir AKS kümesi dağıttıysanız, bu makalede daha sonra gösterildiği gibi Azure CLı veya sağlanmış Azure Resource Manager şablonu kullanarak izlemeyi etkinleştirirsiniz. Aracıyı yükseltmek, silmek, yeniden dağıtmak veya dağıtmak için `kubectl` kullanamazsınız.
>Şablonun, kümeyle aynı kaynak grubunda dağıtılması gerekir.

Aşağıdaki tabloda açıklanan aşağıdaki yöntemlerden birini kullanarak kapsayıcılar için Azure Izleyicisini etkinleştirirsiniz.

| Dağıtım durumu | Yöntem | Açıklama |
|------------------|--------|-------------|
| Yeni AKS kümesi | [Azure CLı kullanarak küme oluşturma](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Azure CLı ile oluşturduğunuz yeni bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Terrayform kullanarak küme oluşturma](container-insights-enable-new-cluster.md#enable-using-terraform)| Açık kaynak aracı Tertuform kullanarak oluşturduğunuz yeni bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| Mevcut AKS kümesi | [Azure CLı kullanarak etkinleştirme](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Azure CLı kullanarak zaten dağıtılmış bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| |[Terrayform kullanarak etkinleştir](container-insights-enable-existing-clusters.md#enable-using-terraform) | Açık kaynaklı araç Tertuform kullanarak zaten dağıtılmış bir AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [Azure Izleyici 'den etkinleştir](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Azure Izleyici 'de AKS Multi-Cluster sayfasından zaten dağıtılmış bir veya daha fazla AKS kümesinin izlenmesini etkinleştirebilirsiniz. |
| | [AKS kümesinden etkinleştir](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| İzlemeyi, Azure portal doğrudan bir AKS kümesinden etkinleştirebilirsiniz. |
| | [Azure Resource Manager şablonu kullanarak etkinleştir](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| Bir AKS kümesinin önceden yapılandırılmış Azure Resource Manager şablonuyla izlenmesini etkinleştirebilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar

* Hem AKS küme düğümleri hem de pods için sistem durumu ölçümlerini yakalamak üzere izleme etkinken, bu durum ölçümleri Azure portal kullanılabilir. Kapsayıcılar için Azure Izleyici 'yi nasıl kullanacağınızı öğrenmek için bkz. [Azure Kubernetes hizmet durumunu görüntüleme](container-insights-analyze.md).
