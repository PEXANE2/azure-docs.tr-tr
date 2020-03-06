---
title: Farklı ağ topolojilerinde Azure Dev Spaces için ağ yapılandırma
services: azure-dev-spaces
ms.date: 01/10/2020
ms.topic: conceptual
description: Azure Kubernetes hizmetlerinde Azure Dev Spaces çalıştırmaya yönelik ağ gereksinimlerini açıklar
keywords: Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, CNı, kubenet, SDN, ağ
ms.openlocfilehash: 9e32e3b65451dceefaeeaf7faed7c8337797e0b8
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78389648"
---
# <a name="configure-networking-for-azure-dev-spaces-in-different-network-topologies"></a>Farklı ağ topolojilerinde Azure Dev Spaces için ağ yapılandırma

Azure Dev Spaces, varsayılan ağ yapılandırmasıyla Azure Kubernetes Service (AKS) kümelerinde çalışır. Kümeyi bir güvenlik duvarının arkasına yerleştirme, ağ güvenlik gruplarını kullanma veya ağ ilkeleri kullanma gibi AKS kümenizin ağ yapılandırmasını değiştirmek istiyorsanız, Azure Dev Spaces çalıştırmak için ek hususlar eklemeniz gerekir.

![Sanal ağ yapılandırması](media/configure-networking/virtual-network-clusters.svg)

## <a name="virtual-network-or-subnet-configurations"></a>Sanal ağ veya alt ağ yapılandırması

AKS kümeniz, AKS kümeniz için giriş veya çıkış trafiğini kısıtlamak üzere farklı bir sanal ağ veya alt ağ yapılandırmasına sahip olabilir. Örneğin, kümeniz Azure Güvenlik Duvarı gibi bir güvenlik duvarının arkasında veya ağ trafiğini kısıtlamak için ağ güvenlik grupları veya özel roller kullanıyor olabilir.

Azure Dev Spaces, giriş *ve çıkış* ağ trafiği için belirli gereksinimlere ve *yalnızca* giriş trafiğini içerir. Aks kümeniz için trafiği kısıtlayan bir sanal ağ veya alt ağ yapılandırmasına sahip bir AKS kümesinde Azure Dev Spaces kullanıyorsanız, Azure Dev Spaces için yalnızca aşağıdaki giriş ve çıkış trafiği gereksinimlerini izlemeniz gerekir düzgün şekilde çalışır.

### <a name="ingress-and-egress-network-traffic-requirements"></a>Giriş ve çıkış ağ trafiği gereksinimleri

Azure Dev Spaces aşağıdaki FQDN 'Ler için giriş ve çıkış trafiğine ihtiyaç duyuyor:

| FQDN                       | Bağlantı noktası       | Kullanım      |
|----------------------------|------------|----------|
| cloudflare.docker.com      | HTTPS: 443 | Azure Dev Spaces için Docker görüntülerini çekmek için |
| gcr.io                     | HTTPS: 443 | Azure Dev Spaces için hele görüntülerini çekmek için |
| storage.googleapis.com     | HTTPS: 443 | Azure Dev Spaces için hele görüntülerini çekmek için |
| azds-*. AZD. IO             | HTTPS: 443 | Azure Dev Spaces denetleyicisi için Azure Dev Spaces arka uç hizmetleriyle iletişim kurun. Tam FQDN, `USERPROFILE\.azds\settings.json` *Dataplanefqdn* içinde bulunabilir |

Güvenlik duvarınızı veya güvenlik yapılandırmanızı, yukarıdaki FQDN 'lerden gelen ve giden ağ trafiğine izin verecek şekilde güncelleştirin. Örneğin, ağınızı güvenli hale getirmek için bir güvenlik duvarı kullanıyorsanız, bu etki alanlarına gelen ve giden trafiğe izin vermek için yukarıdaki FQDN 'Ler güvenlik duvarının uygulama kuralına eklenmelidir.

### <a name="ingress-only-network-traffic-requirements"></a>Yalnızca ağ trafiği gereksinimlerini giriş

Azure Dev Spaces, Kubernetes ad alanı düzeyinde yönlendirme sağlar ve kendi FQDN 'sini kullanarak hizmetlere genel erişim sağlar. Bu özelliklerin her ikisinin de çalışması için, güvenlik duvarınızı veya ağ yapılandırmanızı, kümenizdeki Azure Dev Spaces giriş denetleyicisinin dış IP adresine genel giriş sağlamak üzere güncelleştirin. Alternatif olarak, güvenlik duvarınızın genel IP 'sini iç yük dengeleyicinizin IP 'si ile dönüştürmek için bir [iç yük dengeleyici][aks-internal-lb] oluşturup güvenlik DUVARıNıZA bir NAT kuralı ekleyebilirsiniz. Özel bir giriş denetleyicisi oluşturmak için [traefik][traefik-ingress] veya [NGINX][nginx-ingress] de kullanabilirsiniz.

## <a name="aks-cluster-network-requirements"></a>AKS kümesi ağ gereksinimleri

AKS, bir kümede yer alan ve bir pod 'tan çıkış trafiği arasındaki giriş ve çıkış trafiğini denetlemek için [ağ ilkelerini][aks-network-policies] kullanmanıza olanak sağlar. Azure Dev Spaces, giriş *ve çıkış* ağ trafiği için belirli gereksinimlere ve *yalnızca* giriş trafiğini içerir. Aks ağ ilkeleriyle bir AKS kümesinde Azure Dev Spaces kullanıyorsanız, Azure Dev Spaces düzgün şekilde çalışması için yalnızca aşağıdaki giriş ve çıkış trafiği gereksinimlerini izlemeniz gerekir.

### <a name="ingress-and-egress-network-traffic-requirements"></a>Giriş ve çıkış ağ trafiği gereksinimleri

Azure Dev Spaces, hata ayıklama için kümenizdeki bir geliştirme alanındaki bir pod ile doğrudan iletişim kurmanızı sağlar. Bu özelliğin çalışması için, [bölgeye göre farklılık][dev-spaces-ip-auth-range-regions]gösteren Azure dev Spaces altyapısının IP adreslerine giriş ve çıkış iletişimine izin veren bir ağ ilkesi ekleyin.

### <a name="ingress-only-network-traffic-requirements"></a>Yalnızca ağ trafiği gereksinimlerini giriş

Azure Dev Spaces ad alanları arasında oluşan yönlendirme sağlar. Örneğin, Azure Dev Spaces etkin olan ad alanları üst/alt ilişkisine sahip olabilir ve bu, üst ve alt ad alanları arasında ağ trafiğinin yönlendirilmesine olanak tanır. Bu özelliğin çalışması için, üst/alt ad alanları gibi ağ trafiğinin yönlendirildiği ad alanları arasında trafiğe izin veren bir ağ ilkesi ekleyin. Ayrıca, giriş denetleyicisi *azds* ad alanına dağıtılmışsa, giriş denetleyicisinin farklı bir ad alanındaki Azure dev alanı tarafından işaretlenmiş olan Pod ile iletişim kurması gerekir. Giriş denetleyicisinin düzgün çalışması için, *azds* ad alanından, izlenen Pod 'nin çalıştığı ad alanı arasında ağ trafiğine izin verilmelidir.

## <a name="using-azure-cni"></a>Azure CNı kullanma

Varsayılan olarak, aks kümeleri, Azure dev Spaces ile birlikte çalışarak ağ için [Kubernetes kullanan][aks-kubenet] kullanacak şekilde yapılandırılmıştır. AKS kümenizi [Azure Container Networking Interface (CNI)][aks-cni]kullanacak şekilde de yapılandırabilirsiniz. Aks kümenizde Azure CNI ile Azure dev Spaces kullanmak için, sanal ağ ve alt ağ adres alanları, Azure dev Spaces tarafından dağıtılan Pod 'ler için 10 özel IP adresine sahip olacak şekilde izin verir. Özel IP adreslerine izin verme hakkında daha fazla ayrıntı, [aks Azure CNI belgelerinde][aks-cni-ip-planning]bulunabilir.

## <a name="using-api-server-authorized-ip-ranges"></a>API sunucusu yetkilendirilmiş IP aralıklarını kullanma

AKS kümeleri, özel sanal ağlar veya [YETKILENDIRILMIŞ IP aralıklarını kullanarak API sunucusuna erişimin güvenliğini sağlamak][aks-ip-auth-ranges]gibi kümelerinizle etkileşime girebilen ek güvenliği yapılandırmanıza olanak tanır. Kümenizi [oluştururken][aks-ip-auth-range-create] bu ek güvenliği kullanırken Azure dev Spaces kullanmak için [bölgenize göre ek aralıklara izin vermeniz][dev-spaces-ip-auth-range-regions]gerekir. Ayrıca, var olan bir kümeyi bu ek aralıklara izin verecek şekilde [güncelleştirebilirsiniz][aks-ip-auth-range-update] . Ayrıca, API sunucunuza bağlanmak üzere hata ayıklama için AKS kümenize bağlanan herhangi bir geliştirme makinesi IP adresine izin vermeniz gerekir.

## <a name="using-aks-private-clusters"></a>AKS özel kümelerini kullanma

Şu anda, [aks özel kümeleriyle][aks-private-clusters]Azure dev Spaces desteklenmez.

## <a name="client-requirements"></a>İstemci gereksinimleri

Azure Dev Spaces, hata ayıklama için AKS kümeniz ile iletişim kurmak için Azure Dev Spaces CLı uzantısı, Visual Studio Code uzantısı ve Visual Studio uzantısı gibi istemci tarafı araçları kullanır. Azure Dev Spaces istemci tarafı araçları 'nı kullanmak için, geliştirme makinelerinden *azds-\*. azds.io* etki alanına giden trafiğe izin verin. Tam FQDN için `USERPROFILE\.azds\settings.json` *Dataplanefqdn* bölümüne bakın. [API sunucusu YETKILENDIRILMIŞ IP aralıklarını][auth-range-section]KULLANıYORSANıZ, API sunucunuza bağlanmak için hata ayıklama için aks kümenize bağlanan herhangi bir GELIŞTIRME makinesi IP adresine de izin vermeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces birden çok kapsayıcı genelinde daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda kodunuzun farklı sürümleriyle veya dallarıyla çalışarak işbirliğine dayalı geliştirmeyi nasıl kolaylaştırabileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Dev Spaces 'de takım geliştirme][team-quickstart]

[aks-cni]: ../aks/configure-azure-cni.md
[aks-cni-ip-planning]: ../aks/configure-azure-cni.md#plan-ip-addressing-for-your-cluster
[aks-kubenet]: ../aks/configure-kubenet.md
[aks-internal-lb]: ../aks/internal-lb.md
[aks-ip-auth-ranges]: ../aks/api-server-authorized-ip-ranges.md
[aks-ip-auth-range-create]: ../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled
[aks-ip-auth-range-update]: ../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges
[aks-network-policies]: ../aks/use-network-policies.md
[aks-private-clusters]: ../aks/private-clusters.md
[auth-range-section]: #using-api-server-authorized-ip-ranges
[dev-spaces-ip-auth-range-regions]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[traefik-ingress]: how-to/ingress-https-traefik.md
[nginx-ingress]: how-to/ingress-https-nginx.md
[team-quickstart]: quickstart-team-development.md