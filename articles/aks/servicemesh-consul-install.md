---
title: Azure Kubernetes Hizmetine Konsolos Yükle (AKS)
description: Azure Kubernetes Hizmeti (AKS) kümesinde hizmet ağı oluşturmak için Konsolos'u nasıl yükleyip kullanacağınızı öğrenin
author: dstrebel
ms.topic: article
ms.date: 10/09/2019
ms.author: dastrebe
zone_pivot_groups: client-operating-system
ms.openlocfilehash: 1601ab6d81b888fd2247e95f22c58e1fc91df698
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273731"
---
# <a name="install-and-use-consul-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Konsolos'u yükleyin ve kullanın

[Konsolos,][consul-github] Bir Kubernetes kümesindeki mikro hizmetler de önemli bir işlevsellik kümesi sağlayan açık kaynak kodlu bir hizmet kafesidir. Bu özellikler arasında hizmet bulma, sistem durumu denetimi, hizmet bölümleme ve gözlemlenebilirlik yer almaktadır. Konsolos hakkında daha fazla bilgi için resmi [Konsolos nedir?][consul-docs-concepts]

Bu makalede, Konsolos'u nasıl yükleyebilirsiniz gösterilmektedir. Konsolos bileşenleri AKS'deki bir Kubernetes kümesine yüklenir.

> [!NOTE]
> Bu talimatlar referans `1.6.0`Konsolos sürümü , `2.14.2`ve en az Helm sürümü kullanın .
>
> Konsolos `1.6.x` bültenleri Kubernetes sürümleri `1.13+`karşı çalıştırılabilir. [GitHub][consul-github-releases] ek Konsolos sürümleri bulabilirsiniz - Konsolos Bültenleri ve [Konsolos-Yayın Notları][consul-release-notes]de bültenleri her biri hakkında bilgi .

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Konsolos bileşenlerini AKS'ye yükleyin
> * Konsolos yüklemesini doğrulayın
> * AKS'den Konsolosu Kaldır

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede ayrıntılı adımlar, bir AKS kümesi (Kubernetes `1.13` ve yukarıda, RBAC etkin) oluşturduğunuzve küme ile bir `kubectl` bağlantı kurduk varsayalım. Bu öğelerden herhangi biriyle ilgili yardıma ihtiyacınız varsa, [AKS quickstart'ına][aks-quickstart]bakın. Kümenizin Linux düğümü havuzunda en az 3 düğüm olduğundan emin olun.

[Helm'in][helm] bu talimatları izlemesi ve Konsolos'u kurması gerekecek. Kümenizde en son kararlı sürümün doğru şekilde yüklenmesi ve yapılandırılması önerilir. Helm'i yükleme konusunda yardıma ihtiyacınız varsa, [AKS Helm kurulum kılavuzuna][helm-install]bakın. Tüm Konsolos bölmeleri de Linux düğümleri üzerinde çalışacak şekilde zamanlanmalıdır.

Bu makale, Konsolos yükleme kılavuzunu birkaç ayrı adıma ayırır. Sonuçta resmi Konsolos kurulum [kılavuzu][consul-install-k8]olarak yapı olarak aynıdır.

### <a name="install-the-consul-components-on-aks"></a>Konsolos bileşenlerini AKS'ye yükleyin

Konsolos Helm'in sürümünü `v0.10.0` indirerek başlayacağız. Grafiğin bu sürümü Konsolos `1.6.0`sürümü içerir.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download](includes/servicemesh/consul/download-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [macOS - download](includes/servicemesh/consul/download-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download](includes/servicemesh/consul/download-powershell.md)]

::: zone-end

Konsül bileşenlerini `consul-helm` AKS kümenizdeki `consul` ad alanına yüklemek için Helm ve indirilen grafiği kullanın. 

> [!NOTE]
> **Yükleme seçenekleri**
> 
> Kurulumuzun bir parçası olarak aşağıdaki seçenekleri kullanıyoruz:
> - `connectInject.enabled=true`- vekillerin bölmelere enjekte edilmesini sağlar
> - `client.enabled=true`- Konsolos müşterilerinin her düğümüzerinde çalışmasını sağlamak
> - `client.grpc=true`- connectInject için gRPC dinleyicisini etkinleştirin
> - `syncCatalog.enabled=true`- senkronize Kubernetes ve Konsolos hizmetleri
>
> **Düğüm seçicileri**
>
> Konsolos şu anda Linux düğümleri üzerinde çalışacak şekilde zamanlanmış olmalıdır. Kümenizde Windows Server düğümleri varsa, Konsolos bölmelerinin yalnızca Linux düğümlerinde çalışacak şekilde zamanlandığını sağlamalısınız. Bölmelerin doğru düğümlere zamanlanmış olduğundan emin olmak için [düğüm seçicileri][kubernetes-node-selectors] kullanırız.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - install Istio components](includes/servicemesh/consul/install-components-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - install Istio components](includes/servicemesh/consul/install-components-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - install Istio components](includes/servicemesh/consul/install-components-powershell.md)]

::: zone-end

`Consul` Miğfer grafiği bir dizi nesne dağıtıyor. Listeyi yukarıdaki komutunuzun `helm install` çıktısından görebilirsiniz. Konsül bileşenlerinin dağıtımının tamamlanması, küme ortamınıza bağlı olarak yaklaşık 3 dakika sürebilir.

Bu noktada, AKS kümenize Konsolos'u görevlendirdiniz. Konsolosu başarılı bir şekilde konuşlandırmamız için Konsolos kurulumunu doğrulamak için bir sonraki bölüme geçelim.

## <a name="validate-the-consul-installation"></a>Konsolos yüklemesini doğrulayın

Kaynakların başarıyla oluşturulduğunu doğrulayın. [Kubectl get svc][kubectl-get] ve [kubectl get pod][kubectl-get] `consul` komutlarını kullanarak Konsül bileşenlerinin `helm install` komut tarafından yüklendiği ad alanını sorgulayabilirsiniz:

```console
kubectl get svc --namespace consul --output wide
kubectl get pod --namespace consul --output wide
```

Aşağıdaki örnek çıktı, şu anda çalışıyor olması gereken hizmetleri ve bölmeleri (Linux düğümlerinde zamanlanmış) gösterir:

```output
NAME                                 TYPE           CLUSTER-IP    EXTERNAL-IP             PORT(S)                                                                   AGE     SELECTOR
consul                               ExternalName   <none>        consul.service.consul   <none>                                                                    38s     <none>
consul-consul-connect-injector-svc   ClusterIP      10.0.98.102   <none>                  443/TCP                                                                   3m26s   app=consul,component=connect-injector,release=consul
consul-consul-dns                    ClusterIP      10.0.46.194   <none>                  53/TCP,53/UDP                                                             3m26s   app=consul,hasDNS=true,release=consul
consul-consul-server                 ClusterIP      None          <none>                  8500/TCP,8301/TCP,8301/UDP,8302/TCP,8302/UDP,8300/TCP,8600/TCP,8600/UDP   3m26s   app=consul,component=server,release=consul
consul-consul-ui                     ClusterIP      10.0.50.188   <none>                  80/TCP                                                                    3m26s   app=consul,component=server,release=consul

NAME                                                              READY   STATUS    RESTARTS   AGE    IP            NODE                            NOMINATED NODE   READINESS GATES
consul-consul-connect-injector-webhook-deployment-99f74fdbcr5zj   1/1     Running   0          3m9s   10.240.0.68   aks-linux-92468653-vmss000002   <none>           <none>
consul-consul-jbksc                                               1/1     Running   0          3m9s   10.240.0.44   aks-linux-92468653-vmss000001   <none>           <none>
consul-consul-jkwtq                                               1/1     Running   0          3m9s   10.240.0.70   aks-linux-92468653-vmss000002   <none>           <none>
consul-consul-server-0                                            1/1     Running   0          3m9s   10.240.0.91   aks-linux-92468653-vmss000002   <none>           <none>
consul-consul-server-1                                            1/1     Running   0          3m9s   10.240.0.38   aks-linux-92468653-vmss000001   <none>           <none>
consul-consul-server-2                                            1/1     Running   0          3m9s   10.240.0.10   aks-linux-92468653-vmss000000   <none>           <none>
consul-consul-sync-catalog-d846b79c-8ssr8                         1/1     Running   2          3m9s   10.240.0.94   aks-linux-92468653-vmss000002   <none>           <none>
consul-consul-tz2t5                                               1/1     Running   0          3m9s   10.240.0.12   aks-linux-92468653-vmss000000   <none>           <none>
```

Tüm bölmeler bir durum `Running`göstermelidir. Kapsüllerinizde bu durum yoksa, bunu öğrenene kadar bir iki dakika bekleyin. Herhangi bir bölme bir sorun bildiriyorsa, çıktılarını ve durumlarını gözden geçirmek için [kubectl describe pod][kubectl-describe] komutunu kullanın.

## <a name="accessing-the-consul-ui"></a>Konsolos UI'ye erişim

Konsolos UI yukarıdaki kurulumda yüklü ve Konsolos için UI tabanlı yapılandırma sağlar. Konsolos için UI harici bir ip adresi üzerinden kamuya açık değildir. Konsolos kullanıcı arabirimine erişmek için [kubectl port-forward][kubectl-port-forward] komutunu kullanın. Bu komut, istemci makineniz ile AKS kümenizdeki ilgili bölme arasında güvenli bir bağlantı oluşturur.

```console
kubectl port-forward -n consul svc/consul-consul-ui 8080:80
```

Artık bir tarayıcı yı açıp `http://localhost:8080/ui` Konsolos UI'yi açmaya yönlendirebilirsiniz. UI'yi açtığınızda aşağıdakileri görmeniz gerekir:

![Konsolos UI](./media/servicemesh/consul/consul-ui.png)

## <a name="uninstall-consul-from-aks"></a>AKS'den Konsolosu Kaldır

> [!WARNING]
> Konsolos'u çalışan bir sistemden silmesi, hizmetleriniz arasında trafikle ilgili sorunlara neden olabilir. Devam etmeden önce Konsolos olmadan sisteminizin hala doğru çalışması için hükümler yaptığınızdan emin olun.

### <a name="remove-consul-components-and-namespace"></a>Konsolos bileşenlerini ve ad alanını kaldırma

Konsolos'u AKS kümenizden kaldırmak için aşağıdaki komutları kullanın. Komutlar `helm delete` `consul` grafiği kaldırır ve `kubectl delete namespace` komut `consul` ad alanını kaldırır.

```console
helm delete --purge consul
kubectl delete namespace consul
```

## <a name="next-steps"></a>Sonraki adımlar

Konsolos için daha fazla kurulum ve yapılandırma seçeneğini keşfetmek için aşağıdaki resmi Konsolos makalelerine bakın:

- [Konsolos - Miğfer kurulum kılavuzu][consul-install-k8]
- [Konsolos - Dümen kurulum seçenekleri][consul-install-helm-options]

Ayrıca aşağıdakileri kullanarak ek senaryolar da izleyebilirsiniz:

- [Konsolos Örnek Uygulaması][consul-app-example]

<!-- LINKS - external -->
[Hashicorp]: https://hashicorp.com
[cosul-github]: https://github.com/hashicorp/consul
[helm]: https://helm.sh

[consul-docs-concepts]: https://www.consul.io/docs/platform/k8s/index.html
[consul-github]: https://github.com/hashicorp/consul
[consul-github-releases]: https://github.com/hashicorp/consul/releases
[consul-release-notes]: https://github.com/hashicorp/consul/blob/master/CHANGELOG.md
[consul-install-download]: https://www.consul.io/downloads.html
[consul-install-k8]: https://www.consul.io/docs/platform/k8s/run.html
[consul-install-helm-options]: https://www.consul.io/docs/platform/k8s/helm.html#configuration-values-
[consul-app-example]: https://github.com/hashicorp/demo-consul-101/tree/master/k8s
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward
[kubernetes-node-selectors]: https://docs.microsoft.com/azure/aks/concepts-clusters-workloads#node-selectors

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[consul-scenario-mtls]: ./consul-mtls.md
[helm-install]: ./kubernetes-helm.md
