---
title: Kapsayıcılar için Azure Izleyici ile Azure yay ekleme (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Kapsayıcılar için Azure Izleyici ile Azure yayı ekleme
keywords: Kubernetes, yay, Azure, K8s, kapsayıcılar
ms.openlocfilehash: 3e1ea96a9241211b25a4e5b356723290fa647412
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680742"
---
# <a name="onboard-azure-monitor-for-containers-with-arc-preview"></a>Yay ile kapsayıcılar için Azure Izleyici 'yi ekleme (Önizleme)

Azure [izleyici etkin kapsayıcılarını](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview) Azure Arc etkin Kubernetes kümesine ekleme.

## <a name="before-you-begin"></a>Başlamadan önce

* [Kubernetes sürümleri](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions)
* Küme (Master & Worker) düğümleri için Linux 'ları-Ubuntu (18,04 LTS ve 16,04 LTS)
* Azure Arc etkin Kubernetes kümesi için Azure aboneliğinde en düşük katkıda bulunan RBAC rolü izni
* Azure Arc etkin Kubernetes kümesinin tam Azure Kaynak KIMLIĞI
* Kubernetes kümesinin kubeconfig bağlamı
* İzleme Aracısı, Kubelet üzerinde Cadvizörü ' ı güvenli bağlantı noktasında çalıştırıyor olmalıdır: 10250 veya güvenli olmayan bağlantı noktası: 10255 tüm düğümlerde performans ölçümlerini çekmek için   
* Kubelet Cadvizörü bağlantı noktasının güvenliğini güvenli hale getirmek için yapılandırmanız önerilir: 10250.
* İzleme Aracısı, izleme verilerini Azure Izleyici arka ucuna göndermek için aşağıdaki giden bağlantı noktalarını ve etki alanlarını gerektirir (proxy/güvenlik duvarı tarafından engellenmişse)
    -  *. ods.opinsights.azure.com 443
    -  *. oms.opinsights.azure.com 443
    -  *. blob.core.windows.net 443
    -  dc.services.visualstudio.com 443

## <a name="onboarding"></a>Ekleme

### <a name="using-helm-chart"></a>Held grafiğini kullanma

### <a name="option-1-using-powershell--script"></a>Seçenek 1: PowerShell betiğini kullanma

1. Ekleme betiğini indirin

    ```console
    curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/haiku/onboarding_azuremonitor_for_containers.ps1
     ```

2. PowerShell ekleme betiğini yürütmek için geliştirme makinenize [PowerShell Core](https://docs.microsoft.com/PowerShell/scripting/install/installing-PowerShell?view=PowerShell-6) 'u yükler.

3. Azure'da oturum açma

    ```console
    az login --use-device-code
    ```

4. Aşağıdaki betiği kümenizdeki Azure Arc K8s Cluster RESOURCEID ve Kubernetes kümesinin bağlamıyla yürütün

    ```console
    .\onboarding_azuremonitor_for_containers.ps1 -azureArcClusterResourceId <resourcedIdOfAzureArcCluster> -kubeContext <kube-context>

    For Example ..
    .\onboarding_azuremonitor_for_containers.ps1 -azureArcClusterResourceId /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1 -kubeContext MyK8sTestCluster
     ```

### <a name="option-2-using-bash-script"></a>Seçenek 2: Bash betiğini kullanma

> **İpucu:** Betik Bash 4 özelliklerini kullanır, bu nedenle Bash 'nizin güncel olduğundan emin olun. Geçerli sürümünüzü ile kontrol edebilirsiniz `bash --version` .

1. Ekleme betiğini indirin

    ```console
    curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/haiku/onboarding_azuremonitor_for_containers.sh
     ```

2. Aşağıdaki betiği kümenizdeki Azure Arc K8s Cluster RESOURCEID ve Kubernetes kümesinin bağlamıyla yürütün

    ```console
    bash onboarding_azuremonitor_for_containers.sh <resourcedIdOfAzureArcCluster>  <kube-context>

    For Example:
    bash onboarding_azuremonitor_for_containers.sh /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1 MyK8sTestCluster

     ```

## <a name="configure-agent-data-collection"></a>Aracı veri toplamayı yapılandır

Varsayılan olarak, aracı, kuin-System ad alanında stdout ve stderr 'in kapsayıcılarının günlüklerini toplamaz.
https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-agent-configAracı istenen veri toplama ayarlarıyla yapılandırmak için bölümüne bakın.

## <a name="configure-scraping-of-prometheus-metrics"></a>Prometheus ölçümlerinin korumasını yapılandırın

Kapsayıcılar için Azure Izleyici, Prometheus ölçümlerini ve Azure Izleyici arka ucunu alma.
https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-prometheus-integrationPrometheus scraping öğesinin nasıl yapılandırılacağı hakkında yönergeler için bkz..

## <a name="user-interface"></a>Kullanıcı arabirimi

https://aka.ms/azmon-containers-azurearcEklendi kümesini görüntülemek için bölümüne gidin.

## <a name="disable-monitoring"></a>Izlemeyi devre dışı bırak

İzlemeyi bir nedenden dolayı devre dışı bırakmak istiyorsanız, kapsayıcıların arka ucu için Azure Izleyicisine toplamayı ve izlemeyi durdurmak üzere yalnızca kapsayıcıların HELM grafiğinin Azure Izleyicisini silebilirsiniz.

    ```console
    helm del azmon-containers-release-1
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)

