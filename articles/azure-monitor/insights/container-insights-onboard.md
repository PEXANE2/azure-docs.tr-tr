---
title: Kapsayıcılar için Azure Izleyicisini etkinleştirme | Microsoft Docs
description: Bu makalede, kapsayıcının nasıl çalıştığını ve performansla ilgili sorunları nasıl tanımladığınızı anlayabilmeniz için kapsayıcılar için Azure Izleyicisini etkinleştirme ve yapılandırma açıklanmaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: 25be8f166fec8a311fdc2ed1fa3fca6339185e94
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67867526"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyicisini etkinleştirme

Bu makalede, Kubernetes ortamlarına dağıtılan ve [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/)'nde barındırılan iş yüklerinin performansını izlemek üzere kapsayıcılar Için Azure izleyici 'yi ayarlama için kullanılabilen seçeneklere bir genel bakış sunulmaktadır.

Kapsayıcılar için Azure İzleyici yeni etkinleştirilebilir ya da bir veya daha fazla var olan dağıtımları AKS aşağıdakileri kullanarak desteklenen yöntemleri:

* Azure portal, Azure PowerShell veya Azure CLı ile
* Kullanarak [Terraform ve AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

* **Log Analytics çalışma alanı.**

    Kapsayıcılar için Azure Izleyici, bölge **US gov Virginia**hariç olmak üzere [bölgeye göre Azure ürünlerinde](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor)listelenen bölgelerde bir Log Analytics çalışma alanını destekler.

    Yeni AKS kümenizi izlemeyi etkinleştirdiğinizde bir çalışma alanı oluşturabilir veya ekleme deneyiminin AKS küme aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturmasına izin verin. Kendiniz oluşturmayı seçerseniz, üzerinden oluşturabilirsiniz [Azure Resource Manager](../platform/template-workspace-configuration.md)temellidir [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../learn/quick-create-workspace.md). Varsayılan çalışma alanı için kullanılan desteklenen eşleme çiftlerinin bir listesi için bkz. [kapsayıcılar Için Azure izleyici Için bölge eşleme](container-insights-region-mapping.md).

* Kapsayıcı izlemeyi etkinleştirmek için **Log Analytics katkıda bulunan rolünün** bir üyesisiniz. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../platform/manage-access.md).

* AKS küme kaynağında **[sahip](../../role-based-access-control/built-in-roles.md#owner)** rolünün bir üyesisiniz.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

* Prometheus ölçümleri varsayılan olarak toplanmaz. Aracıyı toplayacak şekilde [yapılandırmadan](container-insights-agent-config.md) önce, neleri tanımlayabileceğinizi anlamak Için Prometheus [belgelerini](https://prometheus.io/) gözden geçirmeniz önemlidir.

## <a name="components"></a>Bileşenler

Yeteneğinizi performansını izlemek için Azure İzleyici kapsayıcılar için özel olarak geliştirilmiş bir Linux kapsayıcı bir Log Analytics aracısını kullanır. Bu özel aracı kümedeki tüm düğümlerden performans ve olay verilerini toplar ve aracıyı otomatik olarak dağıtılır ve dağıtım sırasında belirtilen Log Analytics çalışma alanı ile kayıtlı. Aracı sürümü microsoft, / oms:ciprod04202018 ya da sonraki ve bir tarihi şu biçimde temsil edilir: *mmddyyyy*.

>[!NOTE]
>AKS için Windows Server desteğinin önizleme sürümü sayesinde, Windows Server düğümlerine sahip bir AKS kümesi, veri toplamak ve Azure Izleyici 'ye iletmek için bir aracı yüklü değildir. Bunun yerine, standart dağıtımın bir parçası olarak kümede otomatik olarak dağıtılan bir Linux düğümü, verileri kümedeki tüm Windows düğümleri adına toplayıp Azure Izleyici 'ye iletir.  
>

Aracıyı yeni bir sürümü yayımlandığında, Azure Kubernetes Service (AKS) barındırılan yönetilen Kubernetes kümeleri üzerinde otomatik olarak yükseltilir. Yayımlanan sürümleri takip etmek için bkz: [Aracı sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

>[!NOTE]
>Bir AKS kümesi zaten dağıttıysanız, bu makalenin sonraki bölümlerinde gösterildiği gibi Azure CLI'yı veya sağlanan bir Azure Resource Manager şablonu kullanarak izlemeyi etkinleştirin. Kullanamazsınız `kubectl` yükseltmek için silmek, yeniden dağıtın veya aracıyı dağıtın.
>Şablon, aynı kaynak grubunda kümesi olarak dağıtılması gerekiyor.

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

* AKS kümesi düğümlerin ve pod'ları için sistem durumu ölçümleri yakalamak için etkinleştirilmiş izleme ile bu sistem durumu ölçümleri Azure portalında kullanılabilir. Kapsayıcılar için Azure İzleyicisi'ni kullanma konusunda bilgi almak için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).
