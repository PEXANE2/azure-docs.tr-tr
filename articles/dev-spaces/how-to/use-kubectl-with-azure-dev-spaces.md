---
title: Azure Dev Spaces ile kubectl nasıl kullanılır?
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure Geliştirme Alanları etkinleştirilmiş bir Azure Kubernetes Hizmet kümesinde dev alanı içinde kubectl komutlarının nasıl kullanılacağını öğrenin
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s '
ms.openlocfilehash: 7530cde68b2ce8d06fb7b16a9a147f0f0bc8645c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75438356"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Kubectl'i Azure Geliştirme Alanı ile kullanma

Azure Geliştirme Alanı'ndaki Kubernetes kümesine erişebilir ve `kubectl`mevcut Kubernetes araçlarını kullanabilirsiniz.

Çalıştırma `az aks use-dev-spaces` komutu sizin `kubectl` için otomatik olarak bir yapılandırma bağlamı ekler, bu nedenle kubectl zaten Azure Dev Spaces Kubernetes kümenize bağlanmış olmalıdır. Örnekler:
- Geçerli bağlamı onaylayın:`kubectl config current-context`
- Kullanılabilir tüm bağlamları `kubectl config get-contexts`listele: . 
- Bağlamı değiştir:`kubectl config use-context <context-name>`
- Kubernetes panosunu görüntüleyin: çalıştırın, `kubectl proxy`ardından tarayıcınızı bu komutun `/ui` yadığı adrese açın (Kubernetes panosuna gitmek için URL'ye ek.
- Varsayılan olarak adlandırılan varsayılan Azure Dev Spaces alanında çalışan hizmetleri *listele:*`kubectl get services --namespace=default`

