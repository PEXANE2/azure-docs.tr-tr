---
title: Azure Dev Spaces hakkında sık sorulan sorular
services: azure-dev-spaces
ms.date: 01/28/2020
ms.topic: conceptual
description: Azure Dev Spaces hakkında bazı yaygın soruların yanıtlarını bulun
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: 6328515232b257e01b9b453de4ca0aca820c4038
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84267503"
---
# <a name="frequently-asked-questions-about-azure-dev-spaces"></a>Azure Dev Spaces hakkında sık sorulan sorular

Bu, Azure Dev Spaces hakkında sıkça sorulan soruları ele alınmaktadır.

## <a name="what-versions-of-kubernetes-are-supported-for-azure-dev-spaces"></a>Azure Dev Spaces için Kubernetes sürümleri destekleniyor mu?

Azure Dev Spaces, [AKS 'Deki Kubernetes 'in Şu anda desteklenen tüm genel kullanılabilirlik (GA) sürümlerini][aks-supported-k8s]destekler.

## <a name="which-azure-regions-currently-provide-azure-dev-spaces"></a>Şu anda hangi Azure bölgeleri Azure Dev Spaces sağlıyor?

Kullanılabilir bölgelerin tüm listesi için bkz. [Desteklenen bölgeler][supported-regions] .

## <a name="can-i-migrate-my-aks-cluster-with-azure-dev-spaces-to-another-region"></a>AKS kümemi başka bir bölgeye Azure Dev Spaces geçirebilir miyim?

Evet, AKS kümenizi Azure Dev Spaces başka bir [desteklenen bölgeye][supported-regions]taşımak istiyorsanız, diğer bölgede yeni bir küme oluşturmanız ve ardından Azure dev Spaces yükleyip yapılandırmanız ve kaynak ve uygulamalarınızı yeni kümenize dağıtmanız önerilir. AKS 'leri geçirme hakkında daha fazla bilgi için bkz. [Azure Kubernetes hizmetine geçiş (aks)][aks-migration].

## <a name="can-i-use-azure-dev-spaces-with-existing-dockerfiles-or-helm-charts"></a>Azure Dev Spaces, mevcut Dockerfiles veya Helm grafikleriyle kullanabilir miyim?

Evet, projenizde zaten bir Dockerfile veya Held grafiği varsa, bu dosyaları Azure Dev Spaces kullanabilirsiniz. Çalıştırdığınızda `azds prep` , `--chart` parametresini kullanın ve grafiğin konumunu belirtin. Azure Dev Spaces, *azds. YAML* ve *dockerfile. geliþme* dosyası oluşturmaya devam eder, ancak mevcut bir Dockerfile veya Helu grafiğini değiştirmez ya da değiştirmeyecektir. *Azds. YAML* ve *dockerfile.* çalıştırırken, her şeyin mevcut uygulamanızla doğru şekilde çalışması için dosyaları geliştirmeniz `azds up` gerekebilir.

Kendi Dockerfile veya Held grafiğinizi kullanırken aşağıdaki sınırlamalar vardır:
* Yalnızca bir Dockerfile kullanılıyorsa, dil SDK 'Sı gibi yalnızca çalışma zamanı değil, geliştirme senaryolarını etkinleştirmek için ihtiyaç duyduğunuz her şeyi içermesi gerekir. Azure Dev Spaces için bir Dockerfile. geliştirme gibi ayrı bir Dockerfile kullanılıyorsa, geliştirme senaryolarını etkinleştirmek için ihtiyacınız olan her şey, bu Dockerfile 'a eklenmelidir.
* Held grafiğiniz, tüm görüntü etiketinin bir kısmını veya tamamını *values. YAML*değeri olarak geçirmeyi desteklemelidir.
* Giriş ile herhangi bir şeyi değiştiriyorsanız, Azure Dev Spaces tarafından sunulan giriş çözümünü kullanmak için Helm grafiğinizi da güncelleştirebilirsiniz.
* [Azure dev Spaces tarafından sunulan yönlendirme yeteneklerini][dev-spaces-routing]kullanmak istiyorsanız, tek bir proje için tüm hizmetlerin tek bir Kubernetes ad alanı içine sığması ve basit adlandırma ile dağıtılması gerekir. Örneğin, *hizmet-a*. Standart HELI grafiklerinde, bu adlandırma güncelleştirmesi *Fullnameoverride* özelliği için bir değer belirtilerek yapılabilir.

Kendi Dockerfile veya hele grafiğinizi Azure Dev Spaces ile birlikte kullanılan mevcut bir sürümle karşılaştırmak için [hızlı][quickstart-cli]başlangıçta oluşturulan dosyaları gözden geçirin.


## <a name="can-i-modify-the-files-generated-by-azure-dev-spaces"></a>Azure Dev Spaces tarafından oluşturulan dosyaları değiştirebilir miyim?

Evet, [projenizi hazırlarken Azure dev Spaces tarafından oluşturulan][dev-spaces-prep] *azds. YAML* dosyasını, Dockerfile ve Held grafiğini değiştirebilirsiniz. Bu dosyalar değiştirildiğinde projenin nasıl oluşturulduğu ve çalıştırıldığı değişir.

## <a name="can-i-use-azure-dev-spaces-without-a-public-ip-address"></a>Genel IP adresi olmadan Azure Dev Spaces kullanabilir miyim?

Hayır, genel IP olmadan AKS kümesinde Azure Dev Spaces sağlayamazsınız. [Yönlendirme için Azure dev Spaces][dev-spaces-routing]genel bir IP gerekir.

## <a name="can-i-use-my-own-ingress-with-azure-dev-spaces"></a>Azure Dev Spaces ile kendi giriş sayfamı kullanabilir miyim?

Evet, giriş Azure Dev Spaces oluştururken kendi giriş bilgilerinizi yapılandırabilirsiniz. Örneğin, [traefik][ingress-traefik] veya [NGINX][ingress-nginx]kullanabilirsiniz.

## <a name="can-i-use-https-with-azure-dev-spaces"></a>Azure Dev Spaces ile HTTPS kullanabilir miyim?

Evet, [traefik][ingress-https-traefik] veya [NGINX][ingress-https-nginx]kullanarak kendi giriş hesabınızı https ile yapılandırabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-on-a-cluster-that-uses-cni-rather-than-kubenet"></a>Azure Dev Spaces, kubenet yerine CNı kullanan bir kümede kullanabilir miyim? 

Evet, ağ için CNı kullanan bir AKS kümesinde Azure Dev Spaces kullanabilirsiniz. Örneğin, bir AKS kümesinde Azure Dev Spaces, ağ için CNı kullanan [mevcut Windows kapsayıcılarıyla][windows-containers]birlikte kullanabilirsiniz. Azure Dev Spaces ile ağ için CNı kullanma hakkında daha fazla bilgiye [buradan](configure-networking.md#using-azure-cni)ulaşabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-with-windows-containers"></a>Windows kapsayıcılarıyla Azure Dev Spaces kullanabilir miyim?

Şu anda Azure Dev Spaces yalnızca Linux düğüm ve düğümleri üzerinde çalışmak üzere tasarlanmıştır, ancak [mevcut Windows kapsayıcılarıyla][windows-containers]bir aks kümesinde Azure dev Spaces çalıştırabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-api-server-authorized-ip-address-ranges-enabled"></a>API sunucusu yetkilendirilmiş IP adresi aralıkları etkin olan AKS kümelerinde Azure Dev Spaces kullanabilir miyim?

Evet, [API sunucusu YETKILENDIRILMIŞ IP adresi aralıkları][aks-auth-range] etkin olan aks kümelerinde Azure dev Spaces kullanabilirsiniz. Azure Dev Spaces ile etkinleştirilmiş bir AKS kümesi ile ilgili daha fazla bilgi için, ile etkinleştirilen IP adresi aralıklarını [burada](configure-networking.md#using-api-server-authorized-ip-ranges)bulabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-restricted-egress-traffic-for-cluster-nodes"></a>Küme düğümleri için kısıtlanmış çıkış trafiği ile AKS kümelerinde Azure Dev Spaces kullanabilir miyim?

Evet, doğru FQDN 'Lere izin verildiğinde, [küme düğümleri için kısıtlanmış çıkış trafiği][aks-restrict-egress-traffic] olan aks kümelerinde Azure dev Spaces kullanabilirsiniz. Azure Dev Spaces ile etkin küme düğümleri için kısıtlanmış çıkış trafiği ile AKS kümelerini kullanma hakkında daha fazla [bilgi edinebilirsiniz.](configure-networking.md#ingress-and-egress-network-traffic-requirements)

## <a name="can-i-use-azure-dev-spaces-on-rbac-enabled-aks-clusters"></a>RBAC özellikli AKS kümelerinde Azure Dev Spaces kullanabilir miyim?

Evet, AKS kümelerindeki Azure Dev Spaces RBAC etkinleştirilmiş veya olmadan kullanabilirsiniz.

## <a name="what-happens-when-i-enable-ingress-for-project-in-visual-studio"></a>Visual Studio 'da Project için giriş 'i etkinleştirdiğimde ne olur?

Projenizi hazırlamak için Visual Studio kullanırken hizmetiniz için giriş etkinleştirme seçeneğiniz vardır. Inress 'nin etkinleştirilmesi, AKS kümenizde çalışırken hizmetinize erişmek için genel bir uç nokta oluşturur, bu isteğe bağlıdır. Girişi etkinleştirmezseniz hizmetinize yalnızca AKS kümeniz içinden erişilebilir.

## <a name="can-i-use-pod-managed-identities-with-azure-dev-spaces"></a>Azure Dev Spaces Pod tarafından yönetilen kimlikleri kullanabilir miyim?

Evet, AKS kümelerinde [Pod yönetilen kimliklerini][aks-pod-managed-id] Azure dev Spaces etkinleştirilmiş olarak kullanabilirsiniz, ancak kümenizde Pod yönetilen kimliklerle Azure dev Spaces etkinleştirdikten sonra [ek yapılandırma adımları][dev-spaces-pod-managed-id-steps] vardır. Pod yönetilen kimlikleri yüklüyse ve kaldırmak istiyorsanız, [kaldırma notlarında][aks-pod-managed-id-uninstall]daha fazla ayrıntı bulabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-with-multiple-microservices-in-an-application"></a>Bir uygulamada birden fazla mikro hizmet ile Azure Dev Spaces kullanabilir miyim?

Evet, birden fazla mikro hizmet içeren bir uygulamada Azure Dev Spaces kullanabilirsiniz, ancak tek tek mikro hizmetleri köklerinde hazırlamanız ve çalıştırmanız gerekir. Azure Dev Spaces CLı, Azure Dev Spaces VS Code uzantısı ve Visual Studio Azure geliştirme iş yükü, çalıştırılabilmesi ve hata ayıklaması için *azds. YAML* dosyasının mikro hizmetin kökünde olmasını bekler. Tek bir uygulamada birden fazla mikro hizmet örneği için bkz. [Bisiklet paylaşma örnek uygulaması][bike-sharing] .

Visual Studio Code, [tek bir çalışma alanında ayrı projeler açmak][vs-code-multi-root-workspaces] ve Azure dev Spaces aracılığıyla ayrı olarak hata ayıklamak mümkündür. Projelerin her biri, Azure Dev Spaces için kendi kendine dahil ve hazırlanmalıdır.

Visual Studio 'da, Azure Dev Spaces aracılığıyla hata ayıklama için .NET Core çözümlerini yapılandırmak mümkündür.

## <a name="can-i-use-azure-dev-spaces-with-a-service-mesh"></a>Hizmet ağı ile Azure Dev Spaces kullanabilir miyim?

Şu anda, [Istio][istio] veya [linkerd][linkerd]gibi hizmet kafesleri ile Azure dev Spaces kullanamazsınız. Azure Dev Spaces ve bir hizmet ağı aynı AKS kümesinde çalıştırılabilir, ancak aynı ad alanında hem Azure Dev Spaces hem de bir hizmet ağı etkinleştirilmemiş olabilir.

[aks-auth-range]: ../aks/api-server-authorized-ip-ranges.md
[aks-auth-range-create]: ../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled
[aks-auth-range-update]: ../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges
[aks-migration]: ../aks/aks-migration.md
[aks-pod-managed-id]: ../aks/developer-best-practices-pod-security.md#use-pod-managed-identities
[aks-pod-managed-id-uninstall]: https://github.com/Azure/aad-pod-identity#uninstall-notes
[aks-restrict-egress-traffic]: ../aks/limit-egress-traffic.md
[aks-supported-k8s]: ../aks/supported-kubernetes-versions.md#list-currently-supported-versions
[bike-sharing]: https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp
[dev-spaces-pod-managed-id-steps]: troubleshooting.md#error-no-azureassignedidentity-found-for-podazdsazds-webhook-deployment-id-in-assigned-state
[dev-spaces-prep]: how-dev-spaces-works-prep.md
[dev-spaces-routing]: how-dev-spaces-works-routing.md#how-routing-works
[ingress-nginx]: how-to/ingress-https-nginx.md#configure-a-custom-nginx-ingress-controller
[ingress-traefik]: how-to/ingress-https-traefik.md#configure-a-custom-traefik-ingress-controller
[ingress-https-nginx]: how-to/ingress-https-nginx.md#configure-the-nginx-ingress-controller-to-use-https
[ingress-https-traefik]: how-to/ingress-https-traefik.md#configure-the-traefik-ingress-controller-to-use-https
[istio]: https://istio.io/
[linkerd]: https://linkerd.io/
[quickstart-cli]: quickstart-cli.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[vs-code-multi-root-workspaces]: https://code.visualstudio.com/docs/editor/multi-root-workspaces
[windows-containers]: how-to/run-dev-spaces-windows-containers.md