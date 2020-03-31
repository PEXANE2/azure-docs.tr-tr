---
title: Azure Dev Alanları için ağ geliştirmeyi farklı ağ topolojilerinde yapılandırın
services: azure-dev-spaces
ms.date: 03/17/2020
ms.topic: conceptual
description: Azure Kubernetes Hizmetlerinde Azure Dev Alanları çalıştırmak için ağ gereksinimlerini açıklar
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, CNI, kubenet, SDN, ağ
ms.openlocfilehash: 82d046aa36fe9caf6337aa7f58ca0db525062283
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240565"
---
# <a name="configure-networking-for-azure-dev-spaces-in-different-network-topologies"></a>Azure Dev Alanları için ağ geliştirmeyi farklı ağ topolojilerinde yapılandırın

Azure Geliştirme Alanları, varsayılan ağ yapılandırmasıyla Azure Kubernetes Hizmeti (AKS) kümelerinde çalışır. Kümeyi bir güvenlik duvarının arkasına koymak, ağ güvenlik gruplarını kullanmak veya ağ ilkelerini kullanmak gibi AKS kümenizin ağ yapılandırmasını değiştirmek istiyorsanız, Azure Dev Alanları çalıştırmak için ek hususlar da dahil etmeniz gerekir.

![Sanal ağ yapılandırması](media/configure-networking/virtual-network-clusters.svg)

## <a name="virtual-network-or-subnet-configurations"></a>Sanal ağ veya alt ağ yapılandırmaları

AKS kümenizde, AKS kümeniziçin girişi veya çıkış trafiğini kısıtlamak için farklı bir sanal ağ veya alt ağ yapılandırması olabilir. Örneğin, kümeniz Azure Güvenlik Duvarı gibi bir güvenlik duvarının arkasında olabilir veya ağ trafiğini kısıtlamak için Ağ Güvenlik Grupları veya özel roller kullanabilirsiniz.

Azure Dev Spaces'in *Giriş ve Çıkış* ağ trafiği nin yanı sıra Yalnızca *Giriş* trafiği için belirli gereksinimleri vardır. AKS kümenizin trafiğini kısıtlayan sanal ağ veya alt ağ yapılandırması olan bir AKS kümesinde Azure Dev Spaces kullanıyorsanız, Azure Dev Spaces'in düzgün bir şekilde çalışır.

### <a name="ingress-and-egress-network-traffic-requirements"></a>Giriş ve çıkış ağ trafik gereksinimleri

Azure Dev Spaces aşağıdaki FQDN'ler için giriş ve çıkış trafiğine ihtiyaç duyar:

| FQDN                       | Bağlantı noktası       | Kullanım      |
|----------------------------|------------|----------|
| cloudflare.docker.com      | HTTPS: 443 | Azure Geliştirme Alanları için docker görüntüleri çekmek için |
| gcr.io                     | HTTPS: 443 | Azure Dev Spaces için dümen görüntülerini çekmek için |
| storage.googleapis.com     | HTTPS: 443 | Azure Dev Spaces için dümen görüntülerini çekmek için |
| azds-*.azds.io             | HTTPS: 443 | Azure Dev Spaces denetleyicisi için Azure Dev Spaces arka uç hizmetleriyle iletişim kurmak için. Tam FQDN *dataplaneFqdn* bulunabilir`USERPROFILE\.azds\settings.json` |

Yukarıdaki FQDN'lerin tümüne ağ trafiğine izin vermek için güvenlik duvarınızı veya güvenlik yapılandırmanızı güncelleştirin. Örneğin, ağınızı güvenli hale getirmek için bir güvenlik duvarı kullanıyorsanız, bu etki alanlarından gelen trafiğin sağlanması için yukarıdaki FQDN'ler güvenlik duvarının uygulama kuralına eklenmelidir.

### <a name="ingress-only-network-traffic-requirements"></a>Yalnızca ağ trafiği gereksinimlerini girin

Azure Dev Spaces, Kubernetes ad alanı düzeyinde yönlendirmenin yanı sıra kendi FQDN'sini kullanarak hizmetlere genel erişim sağlar. Bu özelliklerin her ikisinin de çalışması için, kümenizdeki Azure Dev Spaces giriş denetleyicisinin dış IP adresine ortak girişe izin vermek için güvenlik duvarınızı veya ağ yapılandırmanızı güncelleyin. Alternatif olarak, bir [dahili yük dengeleyicisi][aks-internal-lb] oluşturabilir ve güvenlik duvarınızın genel IP'sini dahili yük dengeleyicinizin IP'sine çevirmek için güvenlik duvarınıza bir NAT kuralı ekleyebilirsiniz. Özel bir giriş denetleyicisi oluşturmak için [traefik][traefik-ingress] veya [NGINX'i][nginx-ingress] de kullanabilirsiniz.

## <a name="aks-cluster-network-requirements"></a>AKS küme ağ gereksinimleri

AKS, kümedeki bölmeler arasındaki girişi ve çıkış trafiğini kontrol etmek ve bölmeden çıkış trafiğini kontrol etmek için [ağ ilkelerini][aks-network-policies] kullanmanıza olanak tanır. Azure Dev Spaces'in *Giriş ve Çıkış* ağ trafiği nin yanı sıra Yalnızca *Giriş* trafiği için belirli gereksinimleri vardır. AKS ağ ilkeleriyle bir AKS kümesinde Azure Dev Spaces kullanıyorsanız, Azure Dev Spaces'in düzgün çalışması için yalnızca aşağıdaki giriş ve giriş ve çıkış trafiğini izlemeniz gerekir.

### <a name="ingress-and-egress-network-traffic-requirements"></a>Giriş ve çıkış ağ trafik gereksinimleri

Azure Dev Spaces, hata ayıklama için kümenizdeki dev alandaki bir bölmeyle doğrudan iletişim kurmanızı sağlar. Bu özelliğin çalışması için, [bölgeye göre değişen][dev-spaces-ip-auth-range-regions]Azure Dev Spaces altyapısının IP adreslerine giriş ve çıkış iletişimine izin veren bir ağ ilkesi ekleyin.

### <a name="ingress-only-network-traffic-requirements"></a>Yalnızca ağ trafiği gereksinimlerini girin

Azure Dev Spaces, ad alanlarında bölmeler arasında yönlendirme sağlar. Örneğin, Azure Geliştirme Alanları etkinleştirilmiş ad alanlarında, ağ trafiğinin üst ve alt ad alanları arasında bölmeler arasında yönlendirilmesine olanak tanıyan bir üst/alt ilişkisi olabilir. Azure Dev Spaces, kendi FQDN'sini kullanarak hizmet uç noktalarını da ortaya çıkarır. Hizmetleri farklı şekilde yapılandırmak ve ad alanı düzeyi yönlendirmesini nasıl etkilediğini yapılandırmak için [bkz.][endpoint-options]

## <a name="using-azure-cni"></a>Azure CNI'yi kullanma

Varsayılan olarak, AKS kümeleri, Azure Dev Alanları ile çalışan ağ için [kubenet][aks-kubenet] kullanacak şekilde yapılandırılır. AKS kümenizi [Azure Kapsayıcı Ağ Arabirimi 'ni (CNI)][aks-cni]kullanacak şekilde de yapılandırabilirsiniz. AKS kümenizde Azure CNI ile Azure Dev Alanları'nı kullanmak için, Azure Dev Spaces tarafından dağıtılan bölmeler için sanal ağınız ve alt net adresinizi 10'a kadar özel IP adresine izin verin. Özel IP adreslerine izin verme hakkında daha fazla ayrıntı [AKS Azure CNI belgelerinde][aks-cni-ip-planning]mevcuttur.

## <a name="using-api-server-authorized-ip-ranges"></a>API sunucusu yetkili IP aralıklarını kullanma

AKS kümeleri, örneğin özel sanal ağları kullanarak veya [yetkili IP aralıklarını kullanarak API sunucusuna erişimi güvence altına almak][aks-ip-auth-ranges]gibi, hangi IP adresinin kümelerinizle etkileşimkurabileceğini sınırlayan ek güvenlik yapılandırmanıza olanak sağlar. Kümenizi [oluştururken][aks-ip-auth-range-create] bu ek güvenliği kullanırken Azure Dev Alanları'nı kullanmak için [bölgenize göre ek aralıklara izin][dev-spaces-ip-auth-range-regions]vermelisiniz. Varolan bir kümeyi, bu ek aralıklara izin vermek için de [güncelleştirebilirsiniz.][aks-ip-auth-range-update] Ayrıca, API sunucunuza bağlanmak için hata ayıklama için AKS kümenize bağlanan geliştirme makinelerinin IP adresine izin verebilirsiniz.

## <a name="using-aks-private-clusters"></a>AKS özel kümelerini kullanma

Şu anda Azure Dev Spaces [AKS özel kümeleriyle][aks-private-clusters]desteklenmiyor.

## <a name="using-different-endpoint-options"></a>Farklı uç nokta seçeneklerini kullanma

Azure Dev Spaces, AKS'de çalışan hizmetleriniz için uç noktaları ortaya çıkarma seçeneğine sahiptir. Kümenizde Azure Dev Alanları'nı etkinleştirirken, kümeniz için uç nokta türünü yapılandırmak için aşağıdaki seçeneklere sahip siniz:

* Varsayılan olan *ortak* bitiş noktası, ortak IP adresine sahip bir giriş denetleyicisi dağıtır. Ortak IP adresi kümenin DNS'sine kaydedilir ve bir URL kullanarak hizmetlerinize genel erişim sağlar. Bu URL'yi `azds list-uris`.
* *Özel* bir bitiş noktası, özel IP adresine sahip bir giriş denetleyicisi dağıtLar. Özel bir IP adresiyle, kümenizin yük dengeleyicisi yalnızca kümenin sanal ağının içinden erişilebilir. Yük dengeleyicisinin özel IP adresi kümenin DNS'sinde kaydedilir, böylece kümenin sanal ağındaki hizmetlere URL kullanılarak erişilebilir. Bu URL'yi `azds list-uris`.
* Bitiş noktası seçeneği için *hiçbirinin* ayarlanması, hiçbir giriş denetleyicisinin dağıtılmasına neden olur. Giriş denetleyicisi dağıtılmadan, [Azure Dev Spaces yönlendirme özellikleri][dev-spaces-routing] çalışmaz. İsteğe bağlı olarak, yönlendirme yeteneklerinin yeniden çalışmasını sağlayacak [traefik][traefik-ingress] veya [NGINX][nginx-ingress]kullanarak kendi giriş denetleyici çözümünüzü uygulayabilirsiniz.

Uç nokta seçeneğinizi yapılandırmak için, kümenizde Azure Dev Alanları'nı etkinleştirirken *-e* veya *uç nokta* kullanın. Örnek:

> [!NOTE]
> Bitiş noktası seçeneği, Azure CLI sürüm 2.2.0 veya sonraki sürümlerini çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

```azurecli
az aks use-dev-spaces -g MyResourceGroup -n MyAKS -e private
```

## <a name="client-requirements"></a>İstemci gereksinimleri

Azure Dev Spaces, hata ayıklama için AKS kümenizle iletişim kurmak için Azure Dev Spaces CLI uzantısı, Visual Studio Code uzantısı ve Visual Studio uzantısı gibi istemci tarafı aracını kullanır. Azure Dev Spaces istemci tarafını kullanmak için, geliştirme makinelerinden *\*azds- .azds.io* etki alanına kadar trafiğe izin verin. Tam FQDN `USERPROFILE\.azds\settings.json` için *dataplaneFqdn* bakın. [API sunucusu yetkili IP aralıkları][auth-range-section]kullanıyorsanız, API sunucunuza bağlanmak için hata ayıklama için AKS kümenize bağlanan geliştirme makinelerinin IP adresine de izin verebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Geliştirme Alanları'nın birden çok kapsayıcıda daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda farklı sürümlerle veya kod dallarıyla çalışarak ortak geliştirmeyi nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Geliştirme Alanlarında ekip geliştirme][team-quickstart]

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
[azure-cli-install]: /cli/azure/install-azure-cli
[dev-spaces-ip-auth-range-regions]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[dev-spaces-routing]: how-dev-spaces-works-routing.md
[endpoint-options]: #using-different-endpoint-options
[traefik-ingress]: how-to/ingress-https-traefik.md
[nginx-ingress]: how-to/ingress-https-nginx.md
[team-quickstart]: quickstart-team-development.md