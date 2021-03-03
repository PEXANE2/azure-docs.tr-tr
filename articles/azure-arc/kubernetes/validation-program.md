---
title: Azure Arc etkin Kubernetes doğrulama programı
services: azure-arc
ms.service: azure-arc
ms.date: 03/02/2021
ms.topic: article
author: shashankbarsin
ms.author: shasb
description: Kubernetes dağıtımları için Arc doğrulama programını açıklar
keywords: Kubernetes, yay, Azure, K8s, doğrulama
ms.openlocfilehash: 0d7756f407a3a176c8913d99bdb45a5026300569
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101689224"
---
# <a name="azure-arc-validation-program"></a>Azure Arc doğrulama programı

Azure Arc etkin Kubernetes, tüm bulut Yerel Bilgi Işlem altyapısı (CNCF) sertifikalı Kubernetes kümeleriyle birlikte çalışmaktadır. Azure Arc ekibi, Kubernetes dağıtımlarıyla birlikte Azure Arc etkin Kubernetes 'i doğrulamak için temel sektör Kubernetes hizmet sağlayıcıları ile de çalıştık. Bu sağlayıcılar tarafından yayınlanan Kubernetes dağıtımlarının gelecekteki büyük ve küçük sürümleri, Azure Arc etkinleştirilmiş Kubernetes ile uyumluluk açısından doğrulanacak.

## <a name="validated-distributions"></a>Doğrulanan dağıtımlar

Aşağıdaki Microsoft tarafından sağlanan Kubernetes dağıtımları ve altyapı sağlayıcıları, Azure Arc etkin Kubernetes için uygunluk testlerini başarıyla geçti:

| Dağıtım ve altyapı sağlayıcısı | Sürüm |
| ---------------------------------------- | ------- |
| Azure 'da küme API sağlayıcısı            | Yayın sürümü: [0.4.12](https://github.com/kubernetes-sigs/cluster-api-provider-azure/releases/tag/v0.4.12); Kubernetes sürümü: [1.18.2](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.2) |
| Azure Stack HIN üzerinde AKS                   | Yayın sürümü: [aralık 2020 güncelleştirme](https://github.com/Azure/aks-hci/releases/tag/AKS-HCI-2012); Kubernetes sürümü: [1.18.8](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.8) |

Aşağıdaki sağlayıcılar ve ilgili Kubernetes dağıtımları, Azure Arc etkin Kubernetes için uyumluluk testlerini başarıyla geçti:

| Sağlayıcı adı | Dağıtım adı | Sürüm |
| ------------ | ----------------- | ------- |
| RedHat       | [OpenShift Kapsayıcı Platformu](https://www.openshift.com/products/container-platform) | [4,5](https://docs.openshift.com/container-platform/4.5/release_notes/ocp-4-5-release-notes.html), [4,6](https://docs.openshift.com/container-platform/4.6/release_notes/ocp-4-6-release-notes.html), [4,7](https://docs.openshift.com/container-platform/4.7/release_notes/ocp-4-7-release-notes.html) |
| VMware       | [Tanzu Kubernetes Kılavuzu](https://tanzu.vmware.com/kubernetes-grid) | Kubernetes sürümü: v 1.17.5 |
| Canonical    | [Charmed Kubernetes](https://ubuntu.com/kubernetes) | [1,19](https://ubuntu.com/kubernetes/docs/1.19/components) |
| SUSE Ranrahi      | [Ranhi Kubernetes altyapısı](https://rancher.com/products/rke/) | RKE CLı sürümü: [v 1.2.4](https://github.com/rancher/rke/releases/tag/v1.2.4); Kubernetes sürümleri: [1.19.6](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.6)), [1.18.14](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.14)), [1.17.16](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.16))  |
| Nutanx      | [Karbon](https://www.nutanix.com/products/karbon)    | Sürüm 2.2.1 |

Azure Arc ekibi, uyumluluk testlerini ve doğrulanan Azure Arc etkin Kubernetes senaryolarını aşağıdaki genel bulut sağlayıcıları üzerinde de çalıştırdı:

| Genel bulut sağlayıcısı adı | Dağıtım adı | Sürüm |
| -------------------------- | ----------------- | ------- |
| Amazon Web Hizmetleri        | Elastik Kubernetes hizmeti (EKS) | v 1.18.9  |
| Google Cloud Platform      | Google Kubernetes Altyapısı (GKE) | v 1.17.15 |

## <a name="scenarios-validated"></a>Doğrulanan senaryolar

Uyumluluk sınamaları, Azure Arc etkin Kubernetes doğrulamasının bir parçası olarak çalışır ve aşağıdaki senaryolar ele alınmaktadır:

1. Kubernetes kümelerini Azure yaya bağlama: 
    * Küme üzerinde Azure Arc etkinleştirilmiş Kubernetes aracı HELI grafiğini dağıtın.
    * Kümede yönetilen sistem kimliği (MSI) sertifikasını ayarlayın.
    * Aracılar, Azure 'a küme meta verileri gönderir.

2. Yapılandırma: 
    * Azure Arc etkin Kubernetes kaynağı üzerinde yapılandırma oluşturun.
    * Gilar iş akışını ayarlamak için gereken [Flox](https://docs.fluxcd.io/), kümede dağıtılır.
    * Flox, demo git deposundan bildirimleri ve Held grafiklerini çeker ve kümeye dağıtır.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir kümeyi Azure yaya bağlama](./connect-cluster.md)
* [Arc etkin Kubernetes kümesinde yapılandırma oluşturma](./use-gitops-connected-cluster.md)
* [Uygun ölçekte yapılandırma uygulamak için Azure Ilkesini kullanma](./use-azure-policy.md)
