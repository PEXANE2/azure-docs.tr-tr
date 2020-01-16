---
title: Azure Dev Spaces hakkında sık sorulan sorular
services: azure-dev-spaces
ms.date: 09/25/2019
ms.topic: conceptual
description: Azure Dev Spaces hakkında bazı yaygın soruların yanıtlarını bulun
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: d5ab56edfe4799d51fb7f08642aad9e2ee01db05
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76044975"
---
# <a name="frequently-asked-questions-about-azure-dev-spaces"></a>Azure Dev Spaces hakkında sık sorulan sorular

Bu, Azure Dev Spaces hakkında sıkça sorulan soruları ele alınmaktadır.

## <a name="which-azure-regions-currently-provide-azure-dev-spaces"></a>Şu anda hangi Azure bölgeleri Azure Dev Spaces sağlıyor?

Kullanılabilir bölgelerin tüm listesi için bkz. [Desteklenen bölgeler][supported-regions] .

## <a name="can-i-use-azure-dev-spaces-without-a-public-ip-address"></a>Genel IP adresi olmadan Azure Dev Spaces kullanabilir miyim?

Hayır, genel IP olmadan AKS kümesinde Azure Dev Spaces sağlayamazsınız. [Yönlendirme için Azure dev Spaces][dev-spaces-routing]genel bir IP gerekir.

## <a name="can-i-use-my-own-ingress-with-azure-dev-spaces"></a>Azure Dev Spaces ile kendi giriş sayfamı kullanabilir miyim?

Evet, bir Azure Dev Spaces oluşturduğu bir yandan kendi giriş hesabınızı yapılandırabilirsiniz. Örneğin, [traefik][ingress-traefik]kullanabilirsiniz.

## <a name="can-i-use-https-with-azure-dev-spaces"></a>Azure Dev Spaces ile HTTPS kullanabilir miyim?

Evet, [traefik][ingress-https-traefik]kullanarak kendi GIRIŞ hesabınızı https ile yapılandırabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-on-a-cluster-that-uses-cni-rather-than-kubenet"></a>Azure Dev Spaces, kubenet yerine CNı kullanan bir kümede kullanabilir miyim? 

Evet, ağ için CNı kullanan bir AKS kümesinde Azure Dev Spaces kullanabilirsiniz. Örneğin, bir AKS kümesinde Azure Dev Spaces, ağ için CNı kullanan [mevcut Windows kapsayıcılarıyla][windows-containers]birlikte kullanabilirsiniz. Azure Dev Spaces ile ağ için CNı kullanma hakkında daha fazla bilgiye [buradan](configure-networking.md#using-azure-cni)ulaşabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-with-windows-containers"></a>Windows kapsayıcılarıyla Azure Dev Spaces kullanabilir miyim?

Şu anda Azure Dev Spaces yalnızca Linux düğüm ve düğümleri üzerinde çalışmak üzere tasarlanmıştır, ancak [mevcut Windows kapsayıcılarıyla][windows-containers]bir aks kümesinde Azure dev Spaces çalıştırabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-api-server-authorized-ip-address-ranges-enabled"></a>API sunucusu yetkilendirilmiş IP adresi aralıkları etkin olan AKS kümelerinde Azure Dev Spaces kullanabilir miyim?

Evet, [API sunucusu YETKILENDIRILMIŞ IP adresi aralıkları][aks-auth-range] etkin olan aks kümelerinde Azure dev Spaces kullanabilirsiniz. Azure Dev Spaces ile etkinleştirilmiş bir AKS kümesi ile ilgili daha fazla bilgi için, ile etkinleştirilen IP adresi aralıklarını [burada](configure-networking.md#using-api-server-authorized-ip-ranges)bulabilirsiniz.

### <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-restricted-egress-traffic-for-cluster-nodes"></a>Küme düğümleri için kısıtlanmış çıkış trafiği ile AKS kümelerinde Azure Dev Spaces kullanabilir miyim?

Evet, doğru FQDN 'Lere izin verildiğinde, [küme düğümleri için kısıtlanmış çıkış trafiği][aks-restrict-egress-traffic] olan aks kümelerinde Azure dev Spaces kullanabilirsiniz. Azure Dev Spaces ile etkin küme düğümleri için kısıtlanmış çıkış trafiği ile AKS kümelerini kullanma hakkında daha fazla [bilgi edinebilirsiniz.](configure-networking.md#ingress-and-egress-network-traffic-requirements)

[aks-auth-range]: ../aks/api-server-authorized-ip-ranges.md
[aks-auth-range-create]: ../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled
[aks-auth-range-ranges]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[aks-auth-range-update]: ../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges
[aks-restrict-egress-traffic]: ../aks/limit-egress-traffic.md
[dev-spaces-routing]: how-dev-spaces-works.md#how-routing-works
[ingress-traefik]: how-to/ingress-https-traefik.md#configure-a-custom-traefik-ingress-controller
[ingress-https-traefik]: how-to/ingress-https-traefik.md#configure-the-traefik-ingress-controller-to-use-https
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[windows-containers]: how-to/run-dev-spaces-windows-containers.md