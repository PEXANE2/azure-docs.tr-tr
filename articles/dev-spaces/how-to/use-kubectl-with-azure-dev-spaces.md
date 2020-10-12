---
title: Azure Dev Spaces ile kubectl kullanma
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure Dev Spaces etkin olan bir Azure Kubernetes hizmet kümesinde kubectl komutlarını bir geliştirme alanı içinde kullanmayı öğrenin
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: 7530cde68b2ce8d06fb7b16a9a147f0f0bc8645c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "75438356"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Azure dev Space ile kubectl kullanma

Kubernetes kümesine bir Azure dev alanı içinde erişebilir ve gibi mevcut Kubernetes araçlarını kullanabilirsiniz `kubectl` .

Çalışan `az aks use-dev-spaces` komut sizin için otomatik olarak bir `kubectl` yapılandırma bağlamı ekler. bu nedenle, kubectl Azure dev Spaces Kubernetes kümenize zaten bağlanmalıdır. Örnekler:
- Geçerli bağlamı onaylayın: `kubectl config current-context`
- Tüm kullanılabilir bağlamları Listele: `kubectl config get-contexts` . 
- Bağlamı Değiştir: `kubectl config use-context <context-name>`
- Kubernetes panosunu görüntüleyin: çalıştırın `kubectl proxy` , ardından tarayıcınızı bu komutun yaydığı adrese açın ( `/ui` Kubernetes panosuna gıtmek için URL 'ye ekleyin).
- Çalışan *Hizmetleri varsayılan Azure dev Spaces alanında varsayılan adı listeleyin*: `kubectl get services --namespace=default`

