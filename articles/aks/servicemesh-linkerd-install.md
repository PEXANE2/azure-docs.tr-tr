---
title: Azure Kubernetes Hizmetinde (AKS) Linkerd'i Yükleyin
description: Azure Kubernetes Hizmeti (AKS) kümesinde hizmet ağı oluşturmak için Linkerd'i nasıl yükleyip kullanacağınızı öğrenin
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
zone_pivot_groups: client-operating-system
ms.openlocfilehash: 419b61527b68299c82dec4f2f5da6b0220859cc1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593759"
---
# <a name="install-linkerd-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Linkerd'i Yükleyin

[Linkerd][linkerd-github] bir açık kaynak hizmet örgü ve [CNCF kuluçka projesidir.][linkerd-cncf] Linkerd, trafik yönetimi, hizmet kimliği ve güvenliği, güvenilirlik ve gözlemlenebilirlik gibi özellikler sağlayan ultra hafif bir servis kafesidir. Linkerd hakkında daha fazla bilgi için resmi [Linkerd SSS][linkerd-faq] ve [Linkerd Architecture][linkerd-architecture] belgelerine bakın.

Bu makalede, Linkerd nasıl yüklenir gösterir. Linkerd `linkerd` istemci ikilisi istemci makinenize yüklenir ve Linkerd bileşenleri AKS'deki bir Kubernetes kümesine yüklenir.

> [!NOTE]
> Bu talimatlar referans Linkerd sürümü `stable-2.6.0`.
>
> Linkerd `stable-2.6.x` Kubernetes sürümleri `1.13+`karşı çalıştırılabilir. GitHub ek kararlı ve kenar Linkerd sürümleri bulabilirsiniz [- Linkerd Sürümleri][linkerd-github-releases].

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * İndirin ve Linkerd linkerd istemci ikili yükleyin
> * Linkerd'i AKS'ye Yükleyin
> * Linkerd yüklemesini doğrulama
> * Panoya Erişin
> * Aks'tan Linkerd'i Kaldır

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede ayrıntılı adımlar, bir AKS kümesi (Kubernetes `1.13` ve yukarıda, RBAC etkin) oluşturduğunuzve küme ile bir `kubectl` bağlantı kurduk varsayalım. Bu öğelerden herhangi biriyle ilgili yardıma ihtiyacınız varsa, [AKS quickstart'ına][aks-quickstart]bakın.

Tüm Linkerd bölmeleri Linux düğümleri üzerinde çalışacak şekilde zamanlanmalıdır - bu kurulum aşağıda ayrıntılı yükleme yönteminde varsayılandır ve ek yapılandırma gerektirmez.

Bu makalede, Linkerd yükleme kılavuzunu birkaç ayrı adıma ayırır. Sonuç resmi Linkerd [rehberlik][linkerd-getting-started]başlarken olarak yapı olarak aynıdır.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download and install client binary](includes/servicemesh/linkerd/install-client-binary-linux.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [MacOS - download and install client binary](includes/servicemesh/linkerd/install-client-binary-macos.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download and install client binary](includes/servicemesh/linkerd/install-client-binary-windows.md)]

::: zone-end

## <a name="install-linkerd-on-aks"></a>Linkerd'i AKS'ye Yükleyin

Linkerd'i yüklemeden önce, denetim düzleminin AKS kümemize yüklenip yüklenmeyebileceğini belirlemek için ön yükleme denetimleri çalıştırırız:

```console
linkerd check --pre
```

AKS kümenizin Linkerd için geçerli bir yükleme hedefi olduğunu belirtmek için aşağıdaki gibi bir şey görmeniz gerekir:

```console
kubernetes-api
--------------
√ can initialize the client
√ can query the Kubernetes API

kubernetes-version
------------------
√ is running the minimum Kubernetes API version
√ is running the minimum kubectl version

pre-kubernetes-setup
--------------------
√ control plane namespace does not already exist
√ can create Namespaces
√ can create ClusterRoles
√ can create ClusterRoleBindings
√ can create CustomResourceDefinitions
√ can create PodSecurityPolicies
√ can create ServiceAccounts
√ can create Services
√ can create Deployments
√ can create CronJobs
√ can create ConfigMaps
√ no clock skew detected

pre-kubernetes-capability
-------------------------
√ has NET_ADMIN capability
√ has NET_RAW capability

pre-linkerd-global-resources
----------------------------
√ no ClusterRoles exist
√ no ClusterRoleBindings exist
√ no CustomResourceDefinitions exist
√ no MutatingWebhookConfigurations exist
√ no ValidatingWebhookConfigurations exist
√ no PodSecurityPolicies exist

linkerd-version
---------------
√ can determine the latest version
√ cli is up-to-date

Status check results are √
```

Şimdi Linkerd bileşenlerini yükleme zamanı. Linkerd bileşenlerini `linkerd` AKS kümenize yüklemek için ikili ve `kubectl` ikili leri kullanın. Ad `linkerd` alanı otomatik olarak oluşturulur ve bileşenler bu ad alanına yüklenir.

```console
linkerd install | kubectl apply -f -
```

Linkerd bir dizi nesne dağıtıyor. Listeyi yukarıdaki komutunuzun `linkerd install` çıktısından görürsünüz. Bağlayıcı bileşenlerinin dağıtımının tamamlanması, küme ortamınıza bağlı olarak yaklaşık 1 dakika sürer.

Bu noktada, Linkerd'i AKS kümenize dağıttınız. Linkerd'in başarılı bir şekilde dağıtılmasını sağlamak için, [Linkerd yüklemesini doğrulamak](#validate-the-linkerd-installation)için bir sonraki bölüme geçelim.

## <a name="validate-the-linkerd-installation"></a>Linkerd yüklemesini doğrulama

Kaynakların başarıyla oluşturulduğunu doğrulayın. Linkerd bileşenlerikomutu `linkerd` tarafından `linkerd install` yüklenen ad alanını sorgulamak için [kubectl get svc][kubectl-get] ve [kubectl get pod][kubectl-get] komutlarını kullanın:

```console
kubectl get svc --namespace linkerd --output wide
kubectl get pod --namespace linkerd --output wide
```

Aşağıdaki örnek çıktı, şu anda çalışıyor olması gereken hizmetleri ve bölmeleri (Linux düğümlerinde zamanlanmış) gösterir:

```console
NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)             AGE  SELECTOR
linkerd-controller-api   ClusterIP   10.0.110.67    <none>        8085/TCP            66s  linkerd.io/control-plane-component=controller
linkerd-destination      ClusterIP   10.0.224.29    <none>        8086/TCP            66s  linkerd.io/control-plane-component=controller
linkerd-dst              ClusterIP   10.0.225.148   <none>        8086/TCP            66s  linkerd.io/control-plane-component=destination
linkerd-grafana          ClusterIP   10.0.61.124    <none>        3000/TCP            65s  linkerd.io/control-plane-component=grafana
linkerd-identity         ClusterIP   10.0.6.104     <none>        8080/TCP            67s  linkerd.io/control-plane-component=identity
linkerd-prometheus       ClusterIP   10.0.27.168    <none>        9090/TCP            65s  linkerd.io/control-plane-component=prometheus
linkerd-proxy-injector   ClusterIP   10.0.100.133   <none>        443/TCP             64s  linkerd.io/control-plane-component=proxy-injector
linkerd-sp-validator     ClusterIP   10.0.221.5     <none>        443/TCP             64s  linkerd.io/control-plane-component=sp-validator
linkerd-tap              ClusterIP   10.0.18.14     <none>        8088/TCP,443/TCP    64s  linkerd.io/control-plane-component=tap
linkerd-web              ClusterIP   10.0.37.108    <none>        8084/TCP,9994/TCP   66s  linkerd.io/control-plane-component=web

NAME                                      READY   STATUS    RESTARTS   AGE   IP            NODE                            NOMINATED NODE   READINESS GATES
linkerd-controller-66ddc9f94f-cm9kt       3/3     Running   0          66s   10.240.0.50   aks-linux-16165125-vmss000001   <none>           <none>
linkerd-destination-c94bc454-qpkng        2/2     Running   0          66s   10.240.0.78   aks-linux-16165125-vmss000002   <none>           <none>
linkerd-grafana-6868fdcb66-4cmq2          2/2     Running   0          65s   10.240.0.69   aks-linux-16165125-vmss000002   <none>           <none>
linkerd-identity-74d8df4b85-tqq8f         2/2     Running   0          66s   10.240.0.48   aks-linux-16165125-vmss000001   <none>           <none>
linkerd-prometheus-699587cf8-k8ghg        2/2     Running   0          65s   10.240.0.41   aks-linux-16165125-vmss000001   <none>           <none>
linkerd-proxy-injector-6556447f64-n29wr   2/2     Running   0          64s   10.240.0.32   aks-linux-16165125-vmss000000   <none>           <none>
linkerd-sp-validator-56745cd567-v4x7h     2/2     Running   0          64s   10.240.0.6    aks-linux-16165125-vmss000000   <none>           <none>
linkerd-tap-5cd9fc566-ct988               2/2     Running   0          64s   10.240.0.15   aks-linux-16165125-vmss000000   <none>           <none>
linkerd-web-774c79b6d5-dhhwf              2/2     Running   0          65s   10.240.0.70   aks-linux-16165125-vmss000002   <none>           <none>
```

Linkerd, Bağlantı denetim `linkerd` düzleminin başarıyla yüklenmiş ve yapılandırıldığını doğrulamak için istemci ikilisi aracılığıyla bir komut sağlar.

```console
linkerd check
```

Yüklemenizin başarılı olduğunu belirtmek için aşağıdaki gibi bir şey görmeniz gerekir:

```console
kubernetes-api
--------------
√ can initialize the client
√ can query the Kubernetes API

kubernetes-version
------------------
√ is running the minimum Kubernetes API version
√ is running the minimum kubectl version

linkerd-config
--------------
√ control plane Namespace exists
√ control plane ClusterRoles exist
√ control plane ClusterRoleBindings exist
√ control plane ServiceAccounts exist
√ control plane CustomResourceDefinitions exist
√ control plane MutatingWebhookConfigurations exist
√ control plane ValidatingWebhookConfigurations exist
√ control plane PodSecurityPolicies exist

linkerd-existence
-----------------
√ 'linkerd-config' config map exists
√ heartbeat ServiceAccount exist
√ control plane replica sets are ready
√ no unschedulable pods
√ controller pod is running
√ can initialize the client
√ can query the control plane API

linkerd-api
-----------
√ control plane pods are ready
√ control plane self-check
√ [kubernetes] control plane can talk to Kubernetes
√ [prometheus] control plane can talk to Prometheus
√ no invalid service profiles

linkerd-version
---------------
√ can determine the latest version
√ cli is up-to-date

control-plane-version
---------------------
√ control plane is up-to-date
√ control plane and cli versions match

Status check results are √
```

## <a name="access-the-dashboard"></a>Panoya erişin

Linkerd, hizmet örgüsü ve iş yükleri hakkında bilgi sağlayan bir panoyla birlikte gelir. Panoya erişmek için `linkerd dashboard` komutu kullanın. Bu komut, istemci makineniz ile AKS kümenizdeki ilgili bölmeler arasında güvenli bir bağlantı oluşturmak için [kubectl port ileriden][kubectl-port-forward] yararlanır. Daha sonra varsayılan tarayıcınızdaki panoyu otomatik olarak açar.

```console
linkerd dashboard
```

Komut ayrıca bir bağlantı noktası oluşturacak ve Grafana panoları için bir bağlantı döndürür.

```console
Linkerd dashboard available at:
http://127.0.0.1:50750
Grafana dashboard available at:
http://127.0.0.1:50750/grafana
Opening Linkerd dashboard in the default browser
```

## <a name="uninstall-linkerd-from-aks"></a>Aks'tan Linkerd'i Kaldır

> [!WARNING]
> Linkerd'in çalışan bir sistemden silinmesi, hizmetleriniz arasında trafikle ilgili sorunlara neden olabilir. İşleme devam etmeden önce Linkerd olmadan sisteminizin hala doğru çalışması için hükümler yaptığınızdan emin olun.

Önce veri düzlemi vekilleri kaldırmanız gerekir. İş yükü ad alanlarından herhangi bir Otomatik Proxy Enjeksiyon [ek açıklamalarını][linkerd-automatic-proxy-injection] kaldırın ve iş yükü dağıtımlarınızı dağıtın. İş yüklerinizin artık ilişkili veri düzlemi bileşenleri olmamalıdır.

Son olarak, kontrol düzlemini aşağıdaki gibi kaldırın:

```console
linkerd install --ignore-cluster | kubectl delete -f -
```

## <a name="next-steps"></a>Sonraki adımlar

Linkerd için daha fazla yükleme ve yapılandırma seçeneğini keşfetmek için aşağıdaki resmi Linkerd kılavuzuna bakın:

- [Linkerd - Miğfer kurulumu][linkerd-install-with-helm]
- [Linkerd - Rol ayrıcalıklarını karşılamak için çok aşamalı kurulum][linkerd-multi-stage-installation]

Ayrıca aşağıdakileri kullanarak ek senaryolar da izleyebilirsiniz:

- [Linkerd emojivoto demo][linkerd-demo-emojivoto]
- [Linkerd kitap demo][linkerd-demo-books]

<!-- LINKS - external -->

[linkerd]: https://linkerd.io/
[linkerd-cncf]: https://landscape.cncf.io/selected=linkerd
[linkerd-faq]: https://linkerd.io/2/faq/
[linkerd-architecture]: https://linkerd.io/2/reference/architecture/
[linkerd-getting-started]: https://linkerd.io/2/getting-started/
[linkerd-overview]: https://linkerd.io/2/overview/
[linkerd-github]: https://github.com/linkerd/linkerd2
[linkerd-github-releases]: https://github.com/linkerd/linkerd2/releases/

[linkerd-install-with-helm]: https://linkerd.io/2/tasks/install-helm/
[linkerd-multi-stage-installation]: https://linkerd.io/2/tasks/install/#multi-stage-install
[linkerd-automatic-proxy-injection]: https://linkerd.io/2/features/proxy-injection/

[linkerd-demo-emojivoto]: https://linkerd.io/2/getting-started/#step-5-install-the-demo-app
[linkerd-demo-books]: https://linkerd.io/2/tasks/books/

[helm]: https://helm.sh

[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
