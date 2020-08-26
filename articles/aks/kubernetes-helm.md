---
title: AKS 'de Held ile mevcut uygulamaları yükler
description: Azure Kubernetes Service (AKS) kümesinde kapsayıcılar dağıtmak için Helmpaketleme aracının nasıl kullanılacağını öğrenin
services: container-service
author: zr-msft
ms.topic: article
ms.date: 06/24/2020
ms.author: zarhoads
ms.openlocfilehash: d05d0166724e586fa79e58e2e74fb583b45d0cc6
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88852867"
---
# <a name="install-existing-applications-with-helm-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service 'te (AKS) Held ile mevcut uygulamaları kurma

[Helk][helm] , Kubernetes uygulamalarının yaşam döngüsünü yüklemenize ve yönetmenize yardımcı olan bir açık kaynaklı paketleme aracıdır. *Apt* ve *yum*gibi Linux paket yöneticilerine benzer şekilde, Helm, önceden yapılandırılmış Kubernetes kaynakları paketleri olan Kubernetes grafiklerini yönetmek için kullanılır.

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

Resmi Held kararlı grafiklerini ve giriş *-NGINX* depolarını eklemek için [helmrepo][helm-repo-add] komutunu kullanın.

```console
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

### <a name="find-helm-charts"></a>Held grafiklerini bulma

Hele grafikleri, uygulamaları bir Kubernetes kümesine dağıtmak için kullanılır. Önceden oluşturulmuş Held grafiklerini aramak için [hele Search][helm-search] komutunu kullanın:

```console
helm search repo stable
```

Aşağıdaki sıkıştırılmış örnek çıktıda, kullanılabilecek bazı HELI grafikleri gösterilmektedir:

```console
$ helm search repo stable

NAME                                    CHART VERSION   APP VERSION                     DESCRIPTION                                       
stable/acs-engine-autoscaler            2.2.2           2.1.1                           DEPRECATED Scales worker nodes within agent pools 
stable/aerospike                        0.3.1           v4.5.0.5                        A Helm chart for Aerospike in Kubernetes          
stable/airflow                          4.10.0          1.10.4                          Airflow is a platform to programmatically autho...
stable/ambassador                       4.4.7           0.85.0                          A Helm chart for Datawire Ambassador              
stable/anchore-engine                   1.3.7           0.5.2                           Anchore container analysis and policy evaluatio...
stable/apm-server                       2.1.5           7.0.0                           The server receives data from the Elastic APM a...
stable/ark                              4.2.2           0.10.2                          DEPRECATED A Helm chart for ark                   
stable/artifactory                      7.3.1           6.1.0                           DEPRECATED Universal Repository Manager support...
stable/artifactory-ha                   0.4.1           6.2.0                           DEPRECATED Universal Repository Manager support...
stable/atlantis                         3.8.4           v0.8.2                          A Helm chart for Atlantis https://www.runatlant...
stable/auditbeat                        1.1.0           6.7.0                           A lightweight shipper to audit the activities o...
stable/aws-cluster-autoscaler           0.3.3                                           Scales worker nodes within autoscaling groups.    
stable/aws-iam-authenticator            0.1.1           1.0                             A Helm chart for aws-iam-authenticator            
stable/bitcoind                         0.2.2           0.17.1                          Bitcoin is an innovative payment network and a ...
stable/bookstack                        1.1.2           0.27.4-1                        BookStack is a simple, self-hosted, easy-to-use...
stable/buildkite                        0.2.4           3                               DEPRECATED Agent for Buildkite                    
stable/burrow                           1.5.2           0.29.0                          Burrow is a permissionable smart contract machine 
stable/centrifugo                       3.1.0           2.1.0                           Centrifugo is a real-time messaging server.       
stable/cerebro                          1.3.1           0.8.5                           A Helm chart for Cerebro - a web admin tool tha...
stable/cert-manager                     v0.6.7          v0.6.2                          A Helm chart for cert-manager                     
stable/chaoskube                        3.1.2           0.14.0                          Chaoskube periodically kills random pods in you...
stable/chartmuseum                      2.4.0           0.8.2                           Host your own Helm Chart Repository               
stable/chronograf                       1.1.0           1.7.12                          Open-source web application written in Go and R...
stable/clamav                           1.0.4           1.6                             An Open-Source antivirus engine for detecting t...
stable/cloudserver                      1.0.3           8.1.5                           An open-source Node.js implementation of the Am...
stable/cluster-autoscaler               6.2.0           1.14.6                          Scales worker nodes within autoscaling groups.    
stable/cluster-overprovisioner          0.2.6           1.0                             Installs the a deployment that overprovisions t...
stable/cockroachdb                      2.1.16          19.1.5                          CockroachDB is a scalable, survivable, strongly...
stable/collabora-code                   1.0.5           4.0.3.1                         A Helm chart for Collabora Office - CODE-Edition  
stable/concourse                        8.2.7           5.6.0                           Concourse is a simple and scalable CI system.     
stable/consul                           3.9.2           1.5.3                           Highly available and distributed service discov...
stable/contour                          0.1.0           v0.15.0                         Contour Ingress controller for Kubernetes         
stable/coredns                          1.7.4           1.6.4                           CoreDNS is a DNS server that chains plugins and...
stable/cosbench                         1.0.1           0.0.6                           A benchmark tool for cloud object storage services
stable/coscale                          1.0.0           3.16.0                          CoScale Agent                                     
stable/couchbase-operator               1.0.1           1.2.1                           A Helm chart to deploy the Couchbase Autonomous...
stable/couchdb                          2.3.0           2.3.1                           DEPRECATED A database featuring seamless multi-...
stable/dask                             3.1.0           1.1.5                           Distributed computation in Python with task sch...
stable/dask-distributed                 2.0.2                                           DEPRECATED: Distributed computation in Python     
stable/datadog                          1.38.3          6.14.0                          DataDog Agent 
...
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
...Successfully got an update from the "stable" chart repository
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
> [Hela belgeleri][helm-documentation]

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
