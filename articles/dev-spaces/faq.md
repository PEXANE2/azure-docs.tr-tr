---
title: Azure Dev Spaces hakkında sık sorulan sorular
services: azure-dev-spaces
ms.date: 09/25/2019
ms.topic: conceptual
description: Azure Dev Spaces hakkında bazı yaygın soruların yanıtlarını bulun
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: 2baab0812061bec7dcf08d35056804313d873889
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74482303"
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

## <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-api-server-authorized-ip-address-ranges-enabled"></a>API sunucusu yetkilendirilmiş IP adresi aralıkları etkin olan AKS kümelerinde Azure Dev Spaces kullanabilir miyim?

Evet, [API sunucusu YETKILENDIRILMIŞ IP adresi aralıkları][aks-auth-range] etkin olan aks kümelerinde Azure dev Spaces kullanabilirsiniz. Kümenizi [oluştururken][aks-auth-range-create] , [bölgeniz temelinde ek aralıklara izin vermeniz][aks-auth-range-ranges]gerekir. Ayrıca, var olan bir kümeyi bu ek aralıklara izin verecek şekilde [güncelleştirebilirsiniz][aks-auth-range-update] .

### <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-restricted-egress-traffic-for-cluster-nodes"></a>Küme düğümleri için kısıtlanmış çıkış trafiği ile AKS kümelerinde Azure Dev Spaces kullanabilir miyim?

Evet, aşağıdaki FQDN 'Lere izin verildiğinde, [küme düğümleri Için kısıtlanmış çıkış trafiği][aks-restrict-egress-traffic] olan aks kümelerinde Azure dev Spaces kullanabilirsiniz:

| FQDN                                    | Bağlantı Noktası      | Bir yönetim grubuna bağlanmak veya bağlı bir yönetim grubunun özelliklerini düzenlemek için Yönetim çalışma alanında      |
|-----------------------------------------|-----------|----------|
| cloudflare.docker.com | HTTPS: 443 | Linux alp ve diğer Azure Dev Spaces görüntülerini çekmek için |
| gcr.io | HTTP: 443 | Held/Tiller görüntülerini çekmek için |
| storage.googleapis.com | HTTP: 443 | Held/Tiller görüntülerini çekmek için |
| azds-<guid>.<location>.azds.io | HTTPS: 443 | Denetleyicinize yönelik Azure Dev Spaces arka uç hizmetleriyle iletişim kurmak için. % USERPROFILE%\.azds\settings.JSON içindeki "dataplaneFqdn" içinde tam FQDN bulunabilir |


[aks-auth-range]: ../aks/api-server-authorized-ip-ranges.md
[aks-auth-range-create]: ../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled
[aks-auth-range-ranges]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[aks-auth-range-update]: ../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges
[aks-restrict-egress-traffic]: ../aks/limit-egress-traffic.md
[dev-spaces-routing]: how-dev-spaces-works.md#how-routing-works
[ingress-traefik]: how-to/ingress-https-traefik.md#configure-a-custom-traefik-ingress-controller
[ingress-https-traefik]: how-to/ingress-https-traefik.md#configure-the-traefik-ingress-controller-to-use-https
[supported-regions]: about.md#supported-regions-and-configurations
[windows-containers]: how-to/run-dev-spaces-windows-containers.md