---
title: AKS'de Helm ile mevcut uygulamaları yükleyin
description: Bir Azure Kubernetes Hizmeti (AKS) kümesinde kapsayıcıları dağıtmak için Miğfer paketleme aracını nasıl kullanacağınızı öğrenin
services: container-service
author: zr-msft
ms.topic: article
ms.date: 11/22/2019
ms.author: zarhoads
ms.openlocfilehash: e46bed5fc9fd83a907f8c9e716317a54548c58cc
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870244"
---
# <a name="install-existing-applications-with-helm-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Helm ile mevcut uygulamaları yükleme

[Helm,][helm] Kubernetes uygulamalarının yaşam döngüsünü yüklemenize ve yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır. *APT* ve *Yum*gibi Linux paket yöneticilerine benzer şekilde Helm, önceden yapılandırılmış Kubernetes kaynaklarının paketleri olan Kubernetes grafiklerini yönetmek için kullanılır.

Bu makalede, AKS'deki bir Kubernetes kümesinde Helm'i nasıl yapılandırabileceğinizve kullanacağınız gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Ayrıca, geliştirme sisteminizde çalışan istemci olan Helm CLI'nin yüklü olması gerekir. Helm ile uygulamaları başlatmanızı, durdurmanızı ve yönetmeniz için izin verir. Azure Bulut Kabuğu'nu kullanıyorsanız, Miğfer CLI'si zaten yüklenmiş. Yerel platformunuzdaki yükleme yönergeleri için [Bkz.][helm-install]

> [!IMPORTANT]
> Helm Linux düğümleri üzerinde çalıştırmak için tasarlanmıştır. Kümenizde Windows Server düğümleri varsa, Helm bölmelerinin yalnızca Linux düğümlerinde çalışacak şekilde zamanlandığını sağlamalısınız. Ayrıca, yüklediğiniz tüm Miğfer grafiklerinin de doğru düğümlerde çalışacak şekilde zamanlandığını da sağlamanız gerekir. Bu makaledeki komutlar, bölmelerin doğru düğümlere zamanlanmış olduğundan emin olmak için [düğüm seçiciler][k8s-node-selector] kullanır, ancak tüm Miğfer grafikleri bir düğüm seçiciyi ortaya çıkaramaz. Kümenizde [leke gibi][taints]diğer seçenekleri de kullanmayı düşünebilirsiniz.

## <a name="verify-your-version-of-helm"></a>Miğfer sürümünüzü doğrulayın

Yüklediğiniz `helm version` Helm sürümünü doğrulamak için komutu kullanın:

```console
helm version
```

Aşağıdaki örnekte Helm sürüm 3.0.0 yüklü gösterir:

```console
$ helm version

version.BuildInfo{Version:"v3.0.0", GitCommit:"e29ce2a54e96cd02ccfce88bee4f58bb6e2a28b6", GitTreeState:"clean", GoVersion:"go1.13.4"}
```

Helm v3 için [Helm v3 bölümündeki](#install-an-application-with-helm-v3)adımları izleyin. Helm v2 için [Helm v2 bölümündeki](#install-an-application-with-helm-v2) adımları izleyin

## <a name="install-an-application-with-helm-v3"></a>Helm v3 ile bir uygulama yükleyin

### <a name="add-the-official-helm-stable-charts-repository"></a>Resmi Helm kararlı grafik deposunu ekleyin

Resmi Helm kararlı grafik deposunu eklemek için [dümen repo][helm-repo-add] komutunu kullanın.

```console
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

### <a name="find-helm-charts"></a>Miğfer grafiklerini bul

Miğfer grafikleri, uygulamaları bir Kubernetes kümesine dağıtmak için kullanılır. Önceden oluşturulmuş Miğfer grafiklerini aramak için [dümen arama][helm-search] komutunu kullanın:

```console
helm search repo stable
```

Aşağıdaki yoğunlaştırılmış örnek çıktı, kullanılabilir Miğfer grafiklerinden bazılarını gösterir:


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

Grafik listesini güncelleştirmek için [dümen repo güncelleştirme][helm-repo-update] komutunu kullanın. Aşağıdaki örnekte başarılı bir repo güncelleştirmesi gösterilmektedir:

```console
$ helm repo update

Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

### <a name="run-helm-charts"></a>Miğfer grafiklerini çalıştır

Helm ile grafikler yüklemek [için, dümen yükleme][helm-install-command] komutunu kullanın ve bir sürüm adı ve yüklemek için grafiğin adını belirtin. Bir Miğfer grafiğinin iş başında yüklenmesini görmek için, Helm grafiği ni kullanarak temel bir nginx dağıtımı yükleyelim.

```console
helm install my-nginx-ingress stable/nginx-ingress \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Aşağıdaki yoğunlaştırılmış örnek çıktı, Miğfer grafiği tarafından oluşturulan Kubernetes kaynaklarının dağıtım durumunu gösterir:

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

Hizmetinizin `kubectl get services` *EXTERNAL-IP'sini* almak için komutu kullanın. Örneğin, aşağıdaki komut *my-nginx-ingress-controller* hizmeti için *EXTERNAL-IP* gösterir:

```console
$ kubectl --namespace default get services -o wide -w my-nginx-ingress-controller

NAME                          TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE   SELECTOR
my-nginx-ingress-controller   LoadBalancer   10.0.123.1     <EXTERNAL-IP>   80:31301/TCP,443:31623/TCP   96s   app=nginx-ingress,component=controller,release=my-nginx-ingress
```

### <a name="list-releases"></a>Liste bültenleri

Kümenizde yüklü sürümlistesini görmek için komutu `helm list` kullanın.

```console
helm list
```

Aşağıdaki örnek, önceki adımda dağıtılan *my-nginx-ingress* sürümü gösterir:

```console
$ helm list

NAME                NAMESPACE   REVISION    UPDATED                                 STATUS      CHART                   APP VERSION
my-nginx-ingress    default     1           2019-11-22 10:08:06.048477 -0600 CST    deployed    nginx-ingress-1.25.0    0.26.1 
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bir Miğfer grafiği dağıttığınızda, bir dizi Kubernetes kaynağı oluşturulur. Bu kaynaklar bölmeleri, dağıtımları ve hizmetleri içerir. Bu kaynakları temizlemek [için, dümen kaldırma][helm-cleanup] komutunu kullanın ve önceki `helm list` komutta bulunduğu gibi sürüm adınızı belirtin.

```console
helm uninstall my-nginx-ingress
```

Aşağıdaki örnek, *my-nginx-ingress* adlı sürümün kaldırıldığını gösterir:

```console
$ helm uninstall my-nginx-ingress

release "my-nginx-ingress" uninstalled
```

## <a name="install-an-application-with-helm-v2"></a>Helm v2 ile bir uygulama yükleme

### <a name="create-a-service-account"></a>Hizmet hesabı oluşturma

Helm'i RBAC özellikli bir AKS kümesine dağıtmadan önce, Tiller hizmeti için bir hizmet hesabı ve rol bağlama gerekir. Bir RBAC etkin kümede Helm / Tiller güvenliğini sağlama hakkında daha fazla bilgi için Bkz. [Tiller, Namespaces ve RBAC.][tiller-rbac] AKS kümeniz RBAC etkin değilse, bu adımı atlayın.

Adlandırılmış `helm-rbac.yaml` bir dosya oluşturun ve aşağıdaki YAML'de kopyalayın:

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

Hizmet hesabını oluşturun ve komutla rol bağlama: `kubectl apply`

```console
kubectl apply -f helm-rbac.yaml
```

### <a name="secure-tiller-and-helm"></a>Güvenli Çapa Makinesi ve Miğfer

Helm istemcisi ve Tiller hizmeti TLS/SSL kullanarak birbiriyle iletişim kurar ve kimlik doğrular. Bu kimlik doğrulama yöntemi, Kubernetes kümesinin ve hangi hizmetlerin dağıtılanabileceğinin güvenliğini sağlamaya yardımcı olur. Güvenliği artırmak için kendi imzalı sertifikalarınızı oluşturabilirsiniz. Her Helm kullanıcısı kendi istemci sertifikasını alır ve Tiller kubernetes kümesinde sertifikalar uygulanarak başharfe alınır. Daha fazla bilgi için Helm [ve Tiller arasında TLS/SSL kullanma][helm2-ssl]bilgisine bakın.

RBAC özellikli Bir Kubernetes kümesi yle, Tiller'ın kümeye sahip olduğu erişim düzeyini denetleyebilirsiniz. Tiller'ın dağıtılan Kubernetes ad alanını tanımlayabilir ve Tiller'ın kaynakları dağıtabileceği ad alanlarını kısıtlayabilirsiniz. Bu yaklaşım, farklı ad alanlarında Tiller örnekleri oluşturmanıza ve dağıtım sınırlarını sınırlamanıza ve Miğfer istemcisinin kullanıcılarını belirli ad boşluklarıyla kapsamanıza olanak tanır. Daha fazla bilgi için [Helm rol tabanlı erişim denetimlerine][helm2-rbac]bakın.

### <a name="configure-helm"></a>Helm'i Yapılandır

Temel bir Çapa Makinesi'ni bir AKS kümesine dağıtmak için [dümen init][helm2-init] komutunu kullanın. Kümeniz RBAC etkin değilse, `--service-account` bağımsız değişkeni ve değeri kaldırın. Aşağıdaki örnekler de [200 tarih-max][helm2-history-max] ayarlayın.

Tiller ve Helm için TLS/SSL yapılandırıldıysanız, bu temel başlatma `--tiller-tls-` adımını atlayın ve bunun yerine bir sonraki örnekte gösterildiği gibi gerekli olanları sağlayın.

```console
helm init --history-max 200 --service-account tiller --node-selectors "beta.kubernetes.io/os=linux"
```

TlS/SSL'yi Helm ve Tiller arasında `--tiller-tls-*` yapılandırmışsanız, aşağıdaki örnekte gösterildiği gibi kendi sertifikalarınızın parametrelerini ve adlarını sağlar:

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

### <a name="find-helm-charts"></a>Miğfer grafiklerini bul

Miğfer grafikleri, uygulamaları bir Kubernetes kümesine dağıtmak için kullanılır. Önceden oluşturulmuş Miğfer grafiklerini aramak için [dümen arama][helm2-search] komutunu kullanın:

```console
helm search
```

Aşağıdaki yoğunlaştırılmış örnek çıktı, kullanılabilir Miğfer grafiklerinden bazılarını gösterir:

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

Grafik listesini güncelleştirmek için [dümen repo güncelleştirme][helm2-repo-update] komutunu kullanın. Aşağıdaki örnekte başarılı bir repo güncelleştirmesi gösterilmektedir:

```console
$ helm repo update

Hold tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete.
```

### <a name="run-helm-charts"></a>Miğfer grafiklerini çalıştır

Mik ile grafikler yüklemek [için, dümen yükleme][helm2-install-command] komutunu kullanın ve yüklemek için grafiğin adını belirtin. Bir Miğfer grafiğinin iş başında yüklenmesini görmek için, Helm grafiği ni kullanarak temel bir nginx dağıtımı yükleyelim. TLS/SSL yapılandırıldıysanız, Helm `--tls` istemci sertifikanızı kullanmak için parametreyi ekleyin.

```console
helm install stable/nginx-ingress \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Aşağıdaki yoğunlaştırılmış örnek çıktı, Miğfer grafiği tarafından oluşturulan Kubernetes kaynaklarının dağıtım durumunu gösterir:

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

Nginx-ingress-controller hizmetinin *external-IP* adresinin doldurulması ve bir web tarayıcısı ile erişmenize olanak sağlaması bir veya iki dakika sürer.

### <a name="list-helm-releases"></a>Helm bültenlerini listele

Kümenizde yüklü sürümlistesini görmek için [dümen listesi][helm2-list] komutunu kullanın. Aşağıdaki örnek, önceki adımda dağıtılan nginx-ingress serbest gösterir. TLS/SSL yapılandırıldıysanız, Helm `--tls` istemci sertifikanızı kullanmak için parametreyi ekleyin.

```console
$ helm list

NAME                REVISION    UPDATED                     STATUS      CHART                 APP VERSION   NAMESPACE
flailing-alpaca   1         Thu May 23 12:55:21 2019    DEPLOYED    nginx-ingress-1.6.13    0.24.1      default
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bir Miğfer grafiği dağıttığınızda, bir dizi Kubernetes kaynağı oluşturulur. Bu kaynaklar bölmeleri, dağıtımları ve hizmetleri içerir. Bu kaynakları temizlemek için `helm delete` komutu kullanın ve önceki `helm list` komutta bulunduğu gibi sürüm adınızı belirtin. Aşağıdaki *örnek, sallanan-alpaka*adlı sürümü siler:

```console
$ helm delete flailing-alpaca

release "flailing-alpaca" deleted
```

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes uygulama dağıtımlarını Helm ile yönetme hakkında daha fazla bilgi için Helm belgelerine bakın.

> [!div class="nextstepaction"]
> [Miğfer belgeleri][helm-documentation]

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
