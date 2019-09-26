---
title: Azure Dev Spaces hakkında sık sorulan sorular
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 09/25/2019
ms.topic: conceptual
description: Azure Dev Spaces hakkında bazı yaygın soruların yanıtlarını bulun
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: de44bf323b300a4467dc8b30555798f557898494
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71305983"
---
# <a name="frequently-asked-questions-about-azure-dev-spaces"></a>Azure Dev Spaces hakkında sık sorulan sorular

Bu, Azure Dev Spaces hakkında sıkça sorulan soruları ele alınmaktadır.

## <a name="which-azure-regions-currently-provide-azure-dev-spaces"></a>Şu anda hangi Azure bölgeleri Azure Dev Spaces sağlıyor?

Kullanılabilir bölgelerin tüm listesi için bkz. [Desteklenen bölgeler ve Konfigürasyonlar][supported-regions].

## <a name="can-i-use-azure-dev-spaces-without-a-public-ip-address"></a>Genel IP adresi olmadan Azure Dev Spaces kullanabilir miyim?

Hayır, genel IP olmadan AKS kümesinde Azure Dev Spaces sağlayamazsınız. [Yönlendirme için Azure dev Spaces][dev-spaces-routing]genel bir IP gerekir.

## <a name="can-i-use-my-own-ingress-with-azure-dev-spaces"></a>Azure Dev Spaces ile kendi giriş sayfamı kullanabilir miyim?

Evet, bir Azure Dev Spaces oluşturduğu bir yandan kendi giriş hesabınızı yapılandırabilirsiniz. Örneğin, [traefik][ingress-traefik]kullanabilirsiniz.

## <a name="can-i-use-https-with-azure-dev-spaces"></a>Azure Dev Spaces ile HTTPS kullanabilir miyim?

Evet, [traefik][ingress-https-traefik]kullanarak kendi GIRIŞ hesabınızı https ile yapılandırabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-on-a-cluster-that-uses-cni-rather-than-kubenet"></a>Azure Dev Spaces, kubenet yerine CNı kullanan bir kümede kullanabilir miyim? 

Evet, ağ için CNı kullanan bir AKS kümesinde Azure Dev Spaces kullanabilirsiniz. Örneğin, bir AKS kümesinde Azure Dev Spaces, ağ için CNı kullanan [mevcut Windows kapsayıcılarıyla][windows-containers]birlikte kullanabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-with-windows-containers"></a>Windows kapsayıcılarıyla Azure Dev Spaces kullanabilir miyim?

Şu anda Azure Dev Spaces yalnızca Linux düğüm ve düğümleri üzerinde çalışmak üzere tasarlanmıştır, ancak [mevcut Windows kapsayıcılarıyla][windows-containers]bir aks kümesinde Azure dev Spaces çalıştırabilirsiniz.


[dev-spaces-routing]: how-dev-spaces-works.md#how-routing-works
[ingress-traefik]: how-to/ingress-https-traefik.md#configure-a-custom-traefik-ingress-controller
[ingress-https-traefik]: how-to/ingress-https-traefik.md#configure-the-traefik-ingress-controller-to-use-https
[supported-regions]: about.md#supported-regions-and-configurations
[windows-containers]: how-to/run-dev-spaces-windows-containers.md