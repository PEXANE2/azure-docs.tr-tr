---
title: AKS 'de Held ile mevcut uygulamaları yükler
description: Azure Kubernetes Service (AKS) kümesinde kapsayıcılar dağıtmak için Helmpaketleme aracının nasıl kullanılacağını öğrenin
services: container-service
author: zr-msft
ms.topic: article
ms.date: 12/07/2020
ms.author: zarhoads
ms.openlocfilehash: f12dffe0b538738a8f6dd00cd3d87d44da828f21
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96779176"
---
# <a name="install-existing-applications-with-helm-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service 'te (AKS) Held ile mevcut uygulamaları kurma

[Helk][helm] , Kubernetes uygulamalarının yaşam döngüsünü yüklemenize ve yönetmenize yardımcı olan bir açık kaynaklı paketleme aracıdır. *Apt* ve *yum* gibi Linux paket yöneticilerine benzer şekilde, Helm, önceden yapılandırılmış Kubernetes kaynakları paketleri olan Kubernetes grafiklerini yönetmek için kullanılır.

Bu makalede, AKS 'teki bir Kubernetes kümesinde Held 'yi yapılandırma ve kullanma işlemlerinin nasıl yapılacağı gösterilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, mevcut bir AKS kümeniz olduğunu varsaymaktadır. AKS kümesine ihtiyacınız varsa bkz. [Azure CLI kullanarak][aks-quickstart-cli] aks hızlı başlangıç veya [Azure Portal kullanımı][aks-quickstart-portal].

Ayrıca, geliştirme sisteminizde çalışan istemci olan Held CLı 'nin yüklü olması gerekir. Bu sayede, Held ile uygulamaları başlatabilir, durdurabilir ve yönetebilirsiniz. Azure Cloud Shell kullanıyorsanız, HELI CLı zaten yüklüdür. Yerel platformunuzun yükleme yönergeleri için bkz. [Held 'Yi yükleme][helm-install].

> [!IMPORTANT]
> Held, Linux düğümlerinde çalışmak üzere tasarlanmıştır. Kümenizde Windows Server düğümleri varsa, Held Pod 'nin yalnızca Linux düğümlerinde çalışacak şekilde zamanlandığından emin olmanız gerekir. Ayrıca yüklediğiniz herhangi bir Held grafiğinin da doğru düğümlerde çalışacak şekilde zamanlandığından emin olmanız gerekir. Bu makaledeki komutlar, KADS 'nin doğru düğümlere zamanlandığından emin olmak için [Node-seçiciler] [k8s-node-Selector] kullanır, ancak tüm HELI grafikleri bir düğüm seçici kullanıma sunmayabilir. Ayrıca, kümenizde yer alan diğer seçenekleri de kullanabilirsiniz (örneğin, [litre][taints]).

## <a name="verify-your-version-of-helm"></a>Held sürümünüzü doğrulayın

Şu `helm version` komutu kullanarak helb 3 ' ün yüklü olduğunu doğrulayın:

```console
helm version
```

Aşağıdaki örnek, Held sürüm 3.0.0 yüklü olduğunu gösterir:

```console
$ helm version

version.BuildInfo{Version:"v3.0.0", GitCommit:"e29ce2a54e96cd02ccfce88bee4f58bb6e2a28b6", GitTreeState:"clean", GoVersion:"go1.13.4"}
```

## <a name="install-an-application-with-helm-v3"></a>Held v3 ile uygulama yükler

### <a name="add-helm-repositories"></a>Held depoları ekleme

*Inress-NGINX* deposunu eklemek için [helmrepo][helm-repo-add] komutunu kullanın.

```console
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

### <a name="find-helm-charts"></a>Held grafiklerini bulma

Hele grafikleri, uygulamaları bir Kubernetes kümesine dağıtmak için kullanılır. Önceden oluşturulmuş Held grafiklerini aramak için [hele Search][helm-search] komutunu kullanın:

```console
helm search repo ingress-nginx
```

Aşağıdaki sıkıştırılmış örnek çıktıda, kullanılabilecek bazı HELI grafikleri gösterilmektedir:

```console
$ helm search repo ingress-nginx

NAME                            CHART VERSION   APP VERSION     DESCRIPTION                                       
ingress-nginx/ingress-nginx     2.12.0          0.34.1          Ingress controller for Kubernetes using NGINX a...
```

Grafik listesini güncelleştirmek için [helmrepo Update][helm-repo-update] komutunu kullanın.

```console
helm repo update
```

Aşağıdaki örnekte başarılı bir depo güncelleştirmesi gösterilmektedir:

```console
$ helm repo update

Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "ingress-nginx" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

### <a name="run-helm-charts"></a>Held grafiklerini çalıştırma

Held ile grafik yüklemek için [HELI install][helm-install-command] komutunu kullanın ve bir sürüm adı ve yüklenecek grafiğin adını belirtin. Bir Helm grafiğinin yükleme işlemini görmek için Helm grafiğini kullanarak temel bir NGINX dağıtımı yükleyelim.

```console
helm install my-nginx-ingress ingress-nginx/ingress-nginx \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Aşağıdaki sıkıştırılmış örnek çıktısı, helk grafiği tarafından oluşturulan Kubernetes kaynaklarının dağıtım durumunu gösterir:

```console
$ helm install my-nginx-ingress ingress-nginx/ingress-nginx \
>     --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
>     --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux

NAME: my-nginx-ingress
LAST DEPLOYED: Fri Nov 22 10:08:06 2019
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The nginx-ingress controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace default get services -o wide -w my-nginx-ingress-ingress-nginx-controller'
...
```

`kubectl get services`Hizmetinizin *dış IP* 'sini almak için komutunu kullanın.

```console
kubectl --namespace default get services -o wide -w my-nginx-ingress-ingress-nginx-controller
```

Örneğin, aşağıdaki komut *My-NGINX-ingress-ingress-NGINX-Controller* hizmeti IÇIN *dış IP* 'yi gösterir:

```console
$ kubectl --namespace default get services -o wide -w my-nginx-ingress-ingress-nginx-controller

NAME                                        TYPE           CLUSTER-IP   EXTERNAL-IP      PORT(S)                      AGE   SELECTOR
my-nginx-ingress-ingress-nginx-controller   LoadBalancer   10.0.2.237   <EXTERNAL-IP>    80:31380/TCP,443:32239/TCP   72s   app.kubernetes.io/component=controller,app.kubernetes.io/instance=my-nginx-ingress,app.kubernetes.io/name=ingress-nginx
```

### <a name="list-releases"></a>Sürümleri Listele

Kümenizde yüklü olan sürümlerin listesini görmek için `helm list` komutunu kullanın.

```console
helm list
```

Aşağıdaki örnekte, önceki adımda dağıtılan *My-NGINX-ingress* sürümü gösterilmektedir:

```console
$ helm list

NAME                NAMESPACE   REVISION    UPDATED                                 STATUS      CHART                   APP VERSION
my-nginx-ingress    default     1           2019-11-22 10:08:06.048477 -0600 CST    deployed    nginx-ingress-1.25.0    0.26.1 
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bir helk grafiği dağıttığınızda, bir dizi Kubernetes kaynağı oluşturulur. Bu kaynaklar, pods, dağıtımlar ve hizmetleri içerir. Bu kaynakları temizlemek için [HELI Uninstall][helm-cleanup] komutunu kullanın ve önceki komutta bulunan yayın adınızı belirtin `helm list` .

```console
helm uninstall my-nginx-ingress
```

Aşağıdaki örnek, *My-NGINX-ingress* adlı sürümü kaldırılmıştır:

```console
$ helm uninstall my-nginx-ingress

release "my-nginx-ingress" uninstalled
```

## <a name="next-steps"></a>Sonraki adımlar

Held ile Kubernetes uygulama dağıtımlarını yönetme hakkında daha fazla bilgi için helk belgelerine bakın.

> [!div class="nextstepaction"]
> [Helm belgeleri][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-cleanup]: https://helm.sh/docs/intro/using_helm/#helm-uninstall-uninstalling-a-release
[helm-documentation]: https://helm.sh/docs/
[helm-install]: https://helm.sh/docs/intro/install/
[helm-install-command]: https://helm.sh/docs/intro/using_helm/#helm-install-installing-a-package
[helm-repo-add]: https://helm.sh/docs/intro/quickstart/#initialize-a-helm-chart-repository
[helm-search]: https://helm.sh/docs/intro/using_helm/#helm-search-finding-charts
[helm-repo-update]: https://helm.sh/docs/intro/using_helm/#helm-repo-working-with-repositories
            
<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[taints]: operator-best-practices-advanced-scheduler.md
