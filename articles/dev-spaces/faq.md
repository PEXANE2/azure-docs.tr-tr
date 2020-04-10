---
title: Azure Geliştirme Alanları hakkında sık sorulan sorular
services: azure-dev-spaces
ms.date: 01/28/2020
ms.topic: conceptual
description: Azure Geliştirme Alanları ile ilgili sık sorulan bazı soruların yanıtlarını bulun
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s '
ms.openlocfilehash: f3ac6ec3c3ddbe8ff508befba2eb4a8423e66f07
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998725"
---
# <a name="frequently-asked-questions-about-azure-dev-spaces"></a>Azure Geliştirme Alanları hakkında sık sorulan sorular

Bu, Azure Dev Alanları hakkında sık sorulan soruları gideriyor.

## <a name="which-azure-regions-currently-provide-azure-dev-spaces"></a>Şu anda hangi Azure bölgeleri Azure Geliştirme Alanları sağlar?

Kullanılabilir bölgelerin tam listesi için [desteklenen bölgelere][supported-regions] bakın.

## <a name="can-i-migrate-my-aks-cluster-with-azure-dev-spaces-to-another-region"></a>AKS kümemi Azure Dev Spaces ile başka bir bölgeye geçirebilir miyim?

Evet, AKS kümenizi Azure Dev Spaces ile başka bir [desteklenen bölgeye][supported-regions]taşımak istiyorsanız, diğer bölgede yeni bir küme oluşturmanızı ve ardından Azure Dev Alanları'nı yüklemenizi ve yapılandırmanızı ve kaynaklarınızı ve uygulamalarınızı yeni kümenize dağıtmanızı öneririz. AKS'ye geçiş hakkında daha fazla bilgi için [bkz.][aks-migration]

## <a name="can-i-use-azure-dev-spaces-with-existing-dockerfiles-or-helm-charts"></a>Mevcut Dockerfiles veya Helm grafikleriyle Azure Dev Spaces'i kullanabilir miyim?

Evet, projenizde zaten bir Dockerfile veya Miğfer grafiği varsa, bu dosyaları Azure Geliştirme Alanları ile kullanabilirsiniz. Çalıştırdığınızda, `azds prep`parametreyi `--chart` kullanın ve grafiğin konumunu belirtin. Azure Dev Spaces yine de *bir azds.yaml* ve *Dockerfile.develop* dosyası oluşturur, ancak varolan bir Dockerfile veya Helm grafiğini değiştirmez veya değiştirmez. Çalışırken her şeyin mevcut uygulamanızla doğru çalışması için *azds.yaml* ve *Dockerfile'i* değiştirmeniz `azds up`gerekebilir.

Kendi Dockerfile veya Helm grafiğinizi kullanırken aşağıdaki sınırlamalar vardır:
* Yalnızca bir Dockerfile kullanıyorsanız, yalnızca çalışma zamanı değil, SDK dili gibi geliştirme senaryolarını etkinleştirmek için ihtiyacınız olan her şeyi içermelidir. Dockerfile gibi Azure Dev Spaces için ayrı bir Dockerfile kullanıyorsanız, geliştirme senaryolarını etkinleştirmek için ihtiyacınız olan her şeyin söz konusu Dockerfile'a dahil edilmesi gerekir.
* Miğfer grafiğiniz, görüntü etiketinin bir kısmını veya tamamını *values.yaml'den*bir değer olarak geçirmeyi desteklemelidir.
* Giriş le herhangi bir şeyi değiştiriyorsanız, Azure Dev Spaces tarafından sağlanan giriş çözümlerini kullanmak için Miğfer grafiğinizi de güncelleyebilirsiniz.
* Azure Dev Spaces [tarafından sağlanan yönlendirme özelliklerini][dev-spaces-routing]kullanmak istiyorsanız, tek bir proje için tüm hizmetler tek bir Kubernetes ad alanına sığmalı ve basit bir adlandırma ile dağıtılmalı, örneğin *service-a*. Standart Miğfer grafiklerinde, bu adlandırma güncelleştirmesi *tam nameOverride* özelliği için bir değer belirterek yapılabilir.

Kendi Dockerfile veya Helm grafiğinizi Azure Dev Alanları ile çalışan varolan bir sürümle karşılaştırmak [için, hızlı başlangıç'ta][quickstart-cli]oluşturulan dosyaları gözden geçirin.


## <a name="can-i-modify-the-files-generated-by-azure-dev-spaces"></a>Azure Dev Spaces tarafından oluşturulan dosyaları değiştirebilir miyim?

Evet, [projenizi hazırlarken Azure Dev Spaces tarafından oluşturulan][dev-spaces-prep] *azds.yaml* dosyasını, Dockerfile ve Helm grafiğini değiştirebilirsiniz. Bu dosyaların değiştirilmesi, projenin oluşturulma ve çalışma şeklini değiştirir.

## <a name="can-i-use-azure-dev-spaces-without-a-public-ip-address"></a>Azure Dev Spaces'i genel bir IP adresi olmadan kullanabilir miyim?

Hayır, ortak bir IP olmadan BIR AKS Kümesi'nde Azure Dev Spaces'i sağlayamaz. Yönlendirme için [Azure Dev Spaces tarafından][dev-spaces-routing]ortak bir IP gereklidir.

## <a name="can-i-use-my-own-ingress-with-azure-dev-spaces"></a>Azure Dev Spaces ile kendi girişalanımı kullanabilir miyim?

Evet, Azure Dev Spaces'in oluşturduğu girişle birlikte kendi girişinizi yapılandırabilirsiniz. Örneğin, [traefik][ingress-traefik] veya [NGINX][ingress-nginx]kullanabilirsiniz.

## <a name="can-i-use-https-with-azure-dev-spaces"></a>AZURE Dev Spaces ile HTTPS'yi kullanabilir miyim?

Evet, [traefik][ingress-https-traefik] veya [NGINX][ingress-https-nginx]kullanarak HTTPS ile kendi giriş inizi yapılandırabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-on-a-cluster-that-uses-cni-rather-than-kubenet"></a>Azure Dev Spaces'i kubenet yerine CNI kullanan bir kümede kullanabilir miyim? 

Evet, ağ oluşturmak için CNI kullanan bir AKS kümesinde Azure Dev Spaces'i kullanabilirsiniz. Örneğin, ağ için CNI kullanan [varolan Windows kapsayıcıları][windows-containers]olan bir AKS kümesinde Azure Dev Spaces'i kullanabilirsiniz. Azure Dev Spaces ile ağ oluşturmak için CNI kullanma hakkında daha fazla bilgiyi [burada](configure-networking.md#using-azure-cni)bulabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-with-windows-containers"></a>Windows Kapsayıcılı Azure Dev Alanları kullanabilir miyim?

Şu anda Azure Dev Spaces yalnızca Linux bölmelerinde ve düğümlerinde çalışacak şekilde tasarlanmıştır, ancak Azure Dev [Spaces'i varolan Windows kapsayıcıları][windows-containers]içeren bir AKS kümesinde çalıştırabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-api-server-authorized-ip-address-ranges-enabled"></a>API sunucusu yetkili IP adresi aralıkları etkinleştirilmiş AKS kümelerinde Azure Dev Spaces kullanabilir miyim?

Evet, [API sunucusu yetkili IP adresi aralıkları][aks-auth-range] etkinleştirilmiş aks kümelerinde Azure Dev Spaces kullanabilirsiniz. Azure Dev Spaces ile etkinleştirilmiş API sunucusu yetkili IP adresi aralıkları ile AKS kümeleri kullanma hakkında daha fazla bilgiyi [burada](configure-networking.md#using-api-server-authorized-ip-ranges)bulabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-restricted-egress-traffic-for-cluster-nodes"></a>Küme düğümleri için sınırlı çıkış trafiği olan AKS kümelerinde Azure Dev Spaces kullanabilir miyim?

Evet, doğru FQDN'lere izin verildikten sonra [etkinleştirilen küme düğümleri için sınırlı çıkış trafiğine][aks-restrict-egress-traffic] sahip AKS kümelerinde Azure Dev Spaces'i kullanabilirsiniz. Azure Dev Spaces ile etkin küme düğümleri için sınırlı çıkış trafiğine sahip aks kümeleri kullanma hakkında daha fazla bilgiyi [burada](configure-networking.md#ingress-and-egress-network-traffic-requirements)bulabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-on-rbac-enabled-aks-clusters"></a>RBAC özellikli AKS kümelerinde Azure Dev Spaces kullanabilir miyim?

Evet, RBAC etkinken veya olmadan AKS kümelerinde Azure Dev Spaces'i kullanabilirsiniz.

## <a name="what-happens-when-i-enable-ingress-for-project-in-visual-studio"></a>Visual Studio'da proje girişini etkinleştirdiğimde ne olur?

Projenizi hazırlamak için Visual Studio'yu kullanırken, hizmetiniz için giriş emseçeneğini zedeleme seçeneğiniz var. Girişi etkinleştirmek, isteğe bağlı olan AKS kümenizde çalışırken hizmetinize erişmek için genel bir bitiş noktası oluşturur. Girişi etkinleştirmezseniz, hizmetiniz yalnızca AKS kümenizden erişilebilir.

## <a name="can-i-use-pod-managed-identities-with-azure-dev-spaces"></a>Azure Dev Spaces ile bölme yönetilen kimlikleri kullanabilir miyim?

Şu anda Azure Dev Spaces, Azure Dev Spaces etkinken AKS kümelerinde [pod yönetilen kimlikleri][aks-pod-managed-id] kullanmayı desteklememektedir. Pod yönetilen kimlikleriniz yüklüyse ve onu kaldırmak istiyorsanız, [kaldır notları'nda][aks-pod-managed-id-uninstall]daha fazla ayrıntı bulabilirsiniz.

## <a name="can-i-use-azure-dev-spaces-with-multiple-microservices-in-an-application"></a>Azure Dev Alanları'nı bir uygulamada birden çok mikro hizmetle kullanabilir miyim?

Evet, azure dev spaces'i birden çok mikro hizmetiçeren bir uygulamada kullanabilirsiniz, ancak tek tek mikro hizmetleri kendi köklerinde hazırlamanız ve çalıştırmanız gerekir. Azure Dev Spaces CLI, Azure Dev Spaces VS Code uzantısı ve Visual Studio Azure Geliştirme iş yükü, *azds.yaml* dosyasının çalıştırmak ve hata ayıklamak için mikro hizmetin kökünde olmasını bekler. Tek bir uygulamada birden çok mikro hizmet örneği için [Bisiklet Paylaşımı örnek uygulamasına][bike-sharing] bakın.

Visual Studio [Code'da, tek bir çalışma alanında ayrı projeler açmak][vs-code-multi-root-workspaces] ve bunları Azure Dev Spaces üzerinden ayrı ayrı hata ayıklamak mümkündür. Projelerin her biri bağımsız olmalı ve Azure Dev Spaces için hazırlanmalıdır.

Visual Studio'da, Azure Dev Spaces üzerinden hata ayıklama için .NET Core çözümlerini yapılandırmak mümkündür.

[aks-auth-range]: ../aks/api-server-authorized-ip-ranges.md
[aks-auth-range-create]: ../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled
[aks-auth-range-ranges]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[aks-auth-range-update]: ../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges
[aks-migration]: ../aks/aks-migration.md
[aks-pod-managed-id]: ../aks/developer-best-practices-pod-security.md#use-pod-managed-identities
[aks-pod-managed-id-uninstall]: https://github.com/Azure/aad-pod-identity#uninstall-notes
[aks-restrict-egress-traffic]: ../aks/limit-egress-traffic.md
[bike-sharing]: https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp
[dev-spaces-prep]: how-dev-spaces-works-prep.md
[dev-spaces-routing]: how-dev-spaces-works-routing.md#how-routing-works
[ingress-nginx]: how-to/ingress-https-nginx.md#configure-a-custom-nginx-ingress-controller
[ingress-traefik]: how-to/ingress-https-traefik.md#configure-a-custom-traefik-ingress-controller
[ingress-https-nginx]: how-to/ingress-https-nginx.md#configure-the-nginx-ingress-controller-to-use-https
[ingress-https-traefik]: how-to/ingress-https-traefik.md#configure-the-traefik-ingress-controller-to-use-https
[quickstart-cli]: quickstart-cli.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[vs-code-multi-root-workspaces]: https://code.visualstudio.com/docs/editor/multi-root-workspaces
[windows-containers]: how-to/run-dev-spaces-windows-containers.md