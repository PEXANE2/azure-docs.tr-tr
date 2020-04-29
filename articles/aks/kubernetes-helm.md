---
title: AKS 'de Held ile mevcut uygulamaları yükler
description: Azure Kubernetes Service (AKS) kümesinde kapsayıcılar dağıtmak için Helmpaketleme aracının nasıl kullanılacağını öğrenin
services: container-service
author: zr-msft
ms.topic: article
ms.date: 11/22/2019
ms.author: zarhoads
ms.openlocfilehash: e46bed5fc9fd83a907f8c9e716317a54548c58cc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81870244"
---
# <a name="install-existing-applications-with-helm-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service 'te (AKS) Held ile mevcut uygulamaları kurma

[Helk][helm] , Kubernetes uygulamalarının yaşam döngüsünü yüklemenize ve yönetmenize yardımcı olan bir açık kaynaklı paketleme aracıdır. *Apt* ve *yum*gibi Linux paket yöneticilerine benzer şekilde, Helm, önceden yapılandırılmış Kubernetes kaynakları paketleri olan Kubernetes grafiklerini yönetmek için kullanılır.

Bu makalede, AKS 'teki bir Kubernetes kümesinde Held 'yi yapılandırma ve kullanma işlemlerinin nasıl yapılacağı gösterilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, mevcut bir AKS kümeniz olduğunu varsaymaktadır. AKS kümesine ihtiyacınız varsa bkz. [Azure CLI kullanarak][aks-quickstart-cli] aks hızlı başlangıç veya [Azure Portal kullanımı][aks-quickstart-portal].

Ayrıca, geliştirme sisteminizde çalışan istemci olan Held CLı 'nin yüklü olması gerekir. Bu sayede, Held ile uygulamaları başlatabilir, durdurabilir ve yönetebilirsiniz. Azure Cloud Shell kullanıyorsanız, HELI CLı zaten yüklüdür. Yerel platformunuzun yükleme yönergeleri için bkz. [Held 'Yi yükleme][helm-install].

> [!IMPORTANT]
> Held, Linux düğümlerinde çalışmak üzere tasarlanmıştır. Kümenizde Windows Server düğümleri varsa, Held Pod 'nin yalnızca Linux düğümlerinde çalışacak şekilde zamanlandığından emin olmanız gerekir. Ayrıca yüklediğiniz herhangi bir Held grafiğinin da doğru düğümlerde çalışacak şekilde zamanlandığından emin olmanız gerekir. Bu makaledeki komutlar, düğümlerin doğru düğümlere zamanlandığından emin olmak için [düğüm seçicileri][k8s-node-selector] kullanır, ancak tüm HELI grafikleri bir düğüm seçici kullanıma sunmayabilir. Ayrıca, kümenizde yer alan diğer seçenekleri de kullanabilirsiniz (örneğin, [litre][taints]).

## <a name="verify-your-version-of-helm"></a>Held sürümünüzü doğrulayın

Yüklediğiniz Held sürümünü doğrulamak için `helm version` komutunu kullanın:

```console
helm version
```

Aşağıdaki örnek, Held sürüm 3.0.0 yüklü olduğunu gösterir:

```console
$ helm version

version.BuildInfo{Version:"v3.0.0", GitCommit:"e29ce2a54e96cd02ccfce88bee4f58bb6e2a28b6", GitTreeState:"clean", GoVersion:"go1.13.4"}
```

Held v3 için [helmv3 bölümündeki](#install-an-application-with-helm-v3)adımları izleyin. Held v2 için, [HELI v2 bölümündeki](#install-an-application-with-helm-v2) adımları izleyin.

## <a name="install-an-application-with-helm-v3"></a>Held v3 ile uygulama yükler

### <a name="add-the-official-helm-stable-charts-repository"></a>Resmi Held kararlı grafikler deposunu ekleme

Resmi Held kararlı grafikler deposunu eklemek için [helmrepo][helm-repo-add] komutunu kullanın.

```console
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
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

Grafik listesini güncelleştirmek için [helmrepo Update][helm-repo-update] komutunu kullanın. Aşağıdaki örnekte başarılı bir depo güncelleştirmesi gösterilmektedir:

```console
$ helm repo update

Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

### <a name="run-helm-charts"></a>Held grafiklerini çalıştırma

Held ile grafik yüklemek için [HELI install][helm-install-command] komutunu kullanın ve bir sürüm adı ve yüklenecek grafiğin adını belirtin. Bir Helm grafiğinin yükleme işlemini görmek için Helm grafiğini kullanarak temel bir NGINX dağıtımı yükleyelim.

```console
helm install my-nginx-ingress stable/nginx-ingress \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Aşağıdaki sıkıştırılmış örnek çıktısı, helk grafiği tarafından oluşturulan Kubernetes kaynaklarının dağıtım durumunu gösterir:

```console
$ helm install my-nginx-ingress stable/nginx-ingress \
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
You can watch the status by running 'kubectl --namespace default get services -o wide -w my-nginx-ingress-controller'
...
```

Hizmetinizin `kubectl get services` *dış IP* 'sini almak için komutunu kullanın. Örneğin, aşağıdaki komut *My-NGINX-ingress-Controller* hizmeti IÇIN *dış IP* 'yi gösterir:

```console
$ kubectl --namespace default get services -o wide -w my-nginx-ingress-controller

NAME                          TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE   SELECTOR
my-nginx-ingress-controller   LoadBalancer   10.0.123.1     <EXTERNAL-IP>   80:31301/TCP,443:31623/TCP   96s   app=nginx-ingress,component=controller,release=my-nginx-ingress
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

Bir helk grafiği dağıttığınızda, bir dizi Kubernetes kaynağı oluşturulur. Bu kaynaklar, pods, dağıtımlar ve hizmetleri içerir. Bu kaynakları temizlemek için [HELI Uninstall][helm-cleanup] komutunu kullanın ve önceki `helm list` komutta bulunan yayın adınızı belirtin.

```console
helm uninstall my-nginx-ingress
```

Aşağıdaki örnek, *My-NGINX-ingress* adlı sürümü kaldırılmıştır:

```console
$ helm uninstall my-nginx-ingress

release "my-nginx-ingress" uninstalled
```

## <a name="install-an-application-with-helm-v2"></a>Held v2 ile uygulama yükler

### <a name="create-a-service-account"></a>Hizmet hesabı oluşturma

Helm 'yi RBAC özellikli bir AKS kümesinde dağıtabilmeniz için, Tiller hizmeti için bir hizmet hesabı ve rol bağlaması olması gerekir. RBAC etkin bir kümede Held/Tiller güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Tiller, namespaces ve RBAC][tiller-rbac]. AKS kümeniz RBAC etkinleştirilmemişse, bu adımı atlayın.

Aşağıdaki YAML 'de `helm-rbac.yaml` adlı bir dosya oluşturun ve kopyalayın:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

`kubectl apply` Komutuyla hizmet hesabı ve rol bağlamayı oluşturun:

```console
kubectl apply -f helm-rbac.yaml
```

### <a name="secure-tiller-and-helm"></a>Güvenli Tiller ve Held

Helk istemcisi ve Tiller hizmeti, TLS/SSL kullanarak birbirleriyle kimlik doğrular ve birbirleriyle iletişim kurar. Bu kimlik doğrulama yöntemi, Kubernetes kümesinin güvenliğini sağlamaya ve hangi hizmetlerin dağıtılabileceklerini kolaylaştırır. Güvenliği artırmak için, kendi imzalı sertifikalarınızı oluşturabilirsiniz. Her helk Kullanıcı kendi istemci sertifikasını alır ve Tiller sertifikalar uygulanmış Kubernetes kümesinde başlatılır. Daha fazla bilgi için bkz. [helk ve Tiller arasında TLS/SSL kullanma][helm2-ssl].

RBAC özellikli bir Kubernetes kümesi ile, Tiller erişim düzeyini kümeye göre kontrol edebilirsiniz. Tiller ' nin dağıtıldığı Kubernetes ad alanını tanımlayabilir ve Tiller ad alanlarını daha sonra kaynaklarını dağıtabileceğini kısıtlayabilirsiniz. Bu yaklaşım, farklı ad alanlarında Tiller örnekleri oluşturmanıza ve dağıtım sınırlarını sınırlandırmanıza ve hele istemcisi kullanıcılarını belirli ad alanlarına aktarmanıza olanak tanır. Daha fazla bilgi için bkz. [Helm rol tabanlı erişim denetimleri][helm2-rbac].

### <a name="configure-helm"></a>Held 'yi yapılandırma

Bir AKS kümesine temel bir Tiller dağıtmak için [helk init][helm2-init] komutunu kullanın. Kümeniz RBAC etkinleştirilmemişse, `--service-account` bağımsız değişkeni ve değeri kaldırın. Aşağıdaki örneklerde Ayrıca [History-Max][helm2-history-max] değeri 200 olarak ayarlanır.

Tiller ve Held için TLS/SSL yapılandırdıysanız, bu temel başlatma adımını atlayın ve bunun yerine, sonraki örnekte gösterildiği `--tiller-tls-` gibi gerekli olan adımları belirtin.

```console
helm init --history-max 200 --service-account tiller --node-selectors "beta.kubernetes.io/os=linux"
```

Helk ve Tiller arasında TLS/SSL yapılandırdıysanız, aşağıdaki örnekte gösterildiği `--tiller-tls-*` gibi kendi sertifikalarınızın parametrelerini ve adlarını belirtin:

```console
helm init \
    --tiller-tls \
    --tiller-tls-cert tiller.cert.pem \
    --tiller-tls-key tiller.key.pem \
    --tiller-tls-verify \
    --tls-ca-cert ca.cert.pem \
    --history-max 200 \
    --service-account tiller \
    --node-selectors "beta.kubernetes.io/os=linux"
```

### <a name="find-helm-charts"></a>Held grafiklerini bulma

Hele grafikleri, uygulamaları bir Kubernetes kümesine dağıtmak için kullanılır. Önceden oluşturulmuş Held grafiklerini aramak için [hele Search][helm2-search] komutunu kullanın:

```console
helm search
```

Aşağıdaki sıkıştırılmış örnek çıktıda, kullanılabilecek bazı HELI grafikleri gösterilmektedir:

```
$ helm search

NAME                           CHART VERSION    APP VERSION  DESCRIPTION
stable/aerospike               0.1.7            v3.14.1.2    A Helm chart for Aerospike in Kubernetes
stable/anchore-engine          0.1.7            0.1.10       Anchore container analysis and policy evaluatio...
stable/apm-server              0.1.0            6.2.4        The server receives data from the Elastic APM a...
stable/ark                     1.0.1            0.8.2        A Helm chart for ark
stable/artifactory             7.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/artifactory-ha          0.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/auditbeat               0.1.0            6.2.4        A lightweight shipper to audit the activities o...
stable/aws-cluster-autoscaler  0.3.3                         Scales worker nodes within autoscaling groups.
stable/bitcoind                0.1.3            0.15.1       Bitcoin is an innovative payment network and a ...
stable/buildkite               0.2.3            3            Agent for Buildkite
stable/burrow                  0.4.4            0.17.1       Burrow is a permissionable smart contract machine
stable/centrifugo              2.0.1            1.7.3        Centrifugo is a real-time messaging server.
stable/cerebro                 0.1.0            0.7.3        A Helm chart for Cerebro - a web admin tool tha...
stable/cert-manager            v0.3.3           v0.3.1       A Helm chart for cert-manager
stable/chaoskube               0.7.0            0.8.0        Chaoskube periodically kills random pods in you...
stable/chartmuseum             1.5.0            0.7.0        Helm Chart Repository with support for Amazon S...
stable/chronograf              0.4.5            1.3          Open-source web application written in Go and R...
stable/cluster-autoscaler      0.6.4            1.2.2        Scales worker nodes within autoscaling groups.
stable/cockroachdb             1.1.1            2.0.0        CockroachDB is a scalable, survivable, strongly...
stable/concourse               1.10.1           3.14.1       Concourse is a simple and scalable CI system.
stable/consul                  3.2.0            1.0.0        Highly available and distributed service discov...
stable/coredns                 0.9.0            1.0.6        CoreDNS is a DNS server that chains plugins and...
stable/coscale                 0.2.1            3.9.1        CoScale Agent
stable/dask                    1.0.4            0.17.4       Distributed computation in Python with task sch...
stable/dask-distributed        2.0.2                         DEPRECATED: Distributed computation in Python
stable/datadog                 0.18.0           6.3.0        DataDog Agent
...
```

Grafik listesini güncelleştirmek için [helmrepo Update][helm2-repo-update] komutunu kullanın. Aşağıdaki örnekte başarılı bir depo güncelleştirmesi gösterilmektedir:

```console
$ helm repo update

Hold tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete.
```

### <a name="run-helm-charts"></a>Held grafiklerini çalıştırma

Held ile grafik yüklemek için [HELI install][helm2-install-command] komutunu kullanın ve yüklenecek grafiğin adını belirtin. Bir Helm grafiğinin yükleme işlemini görmek için Helm grafiğini kullanarak temel bir NGINX dağıtımı yükleyelim. TLS/SSL yapılandırdıysanız, hele İstemci sertifikanızı `--tls` kullanmak için parametresini ekleyin.

```console
helm install stable/nginx-ingress \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Aşağıdaki sıkıştırılmış örnek çıktısı, helk grafiği tarafından oluşturulan Kubernetes kaynaklarının dağıtım durumunu gösterir:

```
$ helm install stable/nginx-ingress --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux

NAME:   flailing-alpaca
LAST DEPLOYED: Thu May 23 12:55:21 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                      DATA  AGE
flailing-alpaca-nginx-ingress-controller  1     0s

==> v1/Pod(related)
NAME                                                            READY  STATUS             RESTARTS  AGE
flailing-alpaca-nginx-ingress-controller-56666dfd9f-bq4cl       0/1    ContainerCreating  0         0s
flailing-alpaca-nginx-ingress-default-backend-66bc89dc44-m87bp  0/1    ContainerCreating  0         0s

==> v1/Service
NAME                                           TYPE          CLUSTER-IP  EXTERNAL-IP  PORT(S)                     AGE
flailing-alpaca-nginx-ingress-controller       LoadBalancer  10.0.109.7  <pending>    80:31219/TCP,443:32421/TCP  0s
flailing-alpaca-nginx-ingress-default-backend  ClusterIP     10.0.44.97  <none>       80/TCP                      0s
...
```

NGINX-ingress-Controller hizmetinin *dış IP* adresinin doldurulması ve bir Web tarayıcısı ile erişmenize izin vermek için bir dakika veya iki zaman alır.

### <a name="list-helm-releases"></a>Held yayınları listeleme

Kümenizde yüklü olan sürümlerin listesini görmek için [HELI List][helm2-list] komutunu kullanın. Aşağıdaki örnekte, önceki adımda dağıtılan NGINX-ingress sürümü gösterilmektedir. TLS/SSL yapılandırdıysanız, hele İstemci sertifikanızı `--tls` kullanmak için parametresini ekleyin.

```console
$ helm list

NAME                REVISION    UPDATED                     STATUS      CHART                 APP VERSION   NAMESPACE
flailing-alpaca   1         Thu May 23 12:55:21 2019    DEPLOYED    nginx-ingress-1.6.13    0.24.1      default
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bir helk grafiği dağıttığınızda, bir dizi Kubernetes kaynağı oluşturulur. Bu kaynaklar, pods, dağıtımlar ve hizmetleri içerir. Bu kaynakları temizlemek için, `helm delete` komutunu kullanın ve önceki `helm list` komutta bulunduğu şekilde sürüm adınızı belirtin. Aşağıdaki örnek, *flamı-Alpaca*adlı yayını siler:

```console
$ helm delete flailing-alpaca

release "flailing-alpaca" deleted
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
[helm2-init]: https://v2.helm.sh/docs/helm/#helm-init
[helm2-install-command]: https://v2.helm.sh/docs/helm/#helm-install
[helm2-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm2-list]: https://v2.helm.sh/docs/helm/#helm-list
[helm2-history-max]: https://v2.helm.sh/docs/using_helm/#initialize-helm-and-install-tiller
[helm2-rbac]: https://v2.helm.sh/docs/using_helm/#role-based-access-control
[helm2-repo-update]: https://v2.helm.sh/docs/helm/#helm-repo-update
[helm2-search]: https://v2.helm.sh/docs/helm/#helm-search
[tiller-rbac]: https://v2.helm.sh/docs/using_helm/#tiller-namespaces-and-rbac
[helm2-ssl]: https://v2.helm.sh/docs/using_helm/#using-ssl-between-helm-and-tiller
            
<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors
[taints]: operator-best-practices-advanced-scheduler.md
