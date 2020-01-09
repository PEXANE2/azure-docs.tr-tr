---
title: Azure Dev Spaces ile kubectl kullanma
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure Dev Spaces etkin olan bir Azure Kubernetes hizmet kümesinde kubectl komutlarını bir geliştirme alanı içinde kullanmayı öğrenin
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: 7530cde68b2ce8d06fb7b16a9a147f0f0bc8645c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75438356"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Azure dev Space ile kubectl kullanma

Kubernetes kümesine bir Azure dev alanı içinde erişebilir ve `kubectl`gibi mevcut Kubernetes araçlarını kullanabilirsiniz.

`az aks use-dev-spaces` komutu çalıştırmak sizin için otomatik olarak bir `kubectl` yapılandırma bağlamını ekler. bu nedenle, kubectl 'nin Azure Dev Spaces Kubernetes kümenize zaten bağlı olması gerekir. Örnekler:
- Geçerli bağlamı onaylayın: `kubectl config current-context`
- Tüm kullanılabilir bağlamları Listele: `kubectl config get-contexts`. 
- Değiştirme bağlamı: `kubectl config use-context <context-name>`
- Kubernetes panosunu görüntüleyin: `kubectl proxy`çalıştırın, ardından tarayıcınızı bu komutun yaydığı adrese açın (Kubernetes panosuna gitmek için URL 'ye `/ui` ekleyin).
- Çalışan hizmetleri varsayılan Azure Dev Spaces *alanı varsayılan olarak listeleyin: `kubectl get services --namespace=default`*

