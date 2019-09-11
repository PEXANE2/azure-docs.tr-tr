---
title: Azure Kubernetes Service (AKS) ' de Hashıcorp Tüketil 'yi yükler
description: Azure Kubernetes Service (AKS) kümesinde hizmet ağı oluşturmak için Tüketil 'yi yüklemeyi ve kullanmayı öğrenin
services: container-service
author: dstrebel
ms.service: container-service
ms.topic: article
ms.date: 06/19/2019
ms.author: dastrebe
ms.openlocfilehash: 7acd2533079499091427c7e63741b9c587ee29e5
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70189230"
---
# <a name="install-and-use-consul-connect-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ' de Tüketil Connect 'i yükleyip kullanma

[Tüketil][consul-github] , bir Kubernetes kümesindeki mikro hizmetlerde anahtar bir işlevsellik kümesi sağlayan açık kaynaklı bir hizmet kafesidir. Bu özellikler hizmet bulma, sistem durumu denetimi, hizmet segmentleme ve Observability içerir. Tüketil hakkında daha fazla bilgi için resmi [nedir?][consul-docs-concepts] belgelerine bakın.

Bu makalede, Tüketil 'nin nasıl yükleneceği gösterilmektedir. Tüketil bileşenleri AKS 'teki bir Kubernetes kümesine yüklenir.

> [!NOTE]
> Bu yönergeler, Tüketil sürümüne `1.5`başvurur.
>
> Tüketil `1.5.x` yayınları, Kubernetes `1.14`sürümleriyle `1.12`,,,,,, `1.14`. [GitHub-Tüketil yayınlarına][consul-github-releases] ek tüketil sürümleri ve her bir yayın hakkında bilgi edinmek için, her zaman [Tüketim l-sürüm notları][consul-release-notes]bulabilirsiniz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * AKS 'e Tüketil bileşenlerini yükler
> * Tüketil yüklemesini doğrulama
> * AKS 'ten Tüketil 'yi kaldırma

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede açıklanan adımlarda bir aks kümesi (RBAC etkinleştirilmiş Kubernetes `1.11` ve üzeri) oluşturduğunuz ve kümeyle bir `kubectl` bağlantı oluşturmuş olduğunuz varsayılır. Bu öğelerin herhangi biriyle ilgili yardıma ihtiyacınız varsa, [aks hızlı başlangıç][aks-quickstart]bölümüne bakın.

Bu yönergeleri izlemek ve Tüketil 'yi yüklemek için [Helm][helm] gerekir. Kümenizde doğru bir sürüm `2.12.2` veya daha sonra yüklenip yapılandırılmış olması önerilir. Held 'yi yüklemeyle ilgili yardıma ihtiyacınız varsa bkz. [aks helk Yükleme Kılavuzu][helm-install]. Tüm Tüketil 'lerin da Linux düğümlerinde çalışacak şekilde zamanlanması gerekir.

Bu makale, Tüketil yükleme kılavuzunu çeşitli ayrı adımlara ayırır. Nihai sonuç, resmi sarf l yükleme [kılavuzundaki][consul-install-k8]yapıda aynıdır.

### <a name="install-the-consul-components-on-aks"></a>AKS 'e Tüketil bileşenlerini yükler

İlk olarak Kubernetes GitHub depositesinde resmi HashiCorp Tüketil 'yi klonlarız.

```bash
git clone https://github.com/hashicorp/consul-helm.git
cd consul-helm
```

Daha sonra, Tüketil yüklemesi için özel bir Helm değer dosyası oluşturmanız gerekir. Tüketil 'yi yüklemeden önce aşağıdaki özel değerler dosyasını oluşturun.

```bash
vim consul-values.yaml
```

Ardından aşağıdaki değerleri, tüketil-values. YAML dosyasına kopyalayıp yapıştırın.

```yaml
# Sets datacenter name and version Consul to use
global:
  datacenter: dc1
  image: "consul:1.5.2"

# Enables proxies to be injected into pods
connectInject:
  enabled: true

# Enables UI on ClusterIP
ui:
  service:
    type: "ClusterIP"

# Enables GRCP that is required for connectInject
client:
  enabled: true
  grpc: true

# Sets replicase to 3 for HA
server:
  replicas: 3
  bootstrapExpect: 1
  disruptionBudget:
    enabled: true
    maxUnavailable: 1

# Syncs Kubernetes service discovery with Consul
syncCatalog:
  enabled: true
```

Artık özel değerler dosyasına sahip olduğumuz AKS kümenize Tüketil 'yi yükleyebiliriz

Bash

```bash
helm install -f consul-values.yaml --name consul --namespace consul .
```
`Consul` Helb grafiği bir dizi nesne dağıtır. Yukarıdaki `helm install` komutun çıktısından listesini görebilirsiniz. Tüketil bileşenlerinin dağıtımı, küme ortamınıza bağlı olarak 4 ila 5 dakika sürebilir.

> [!NOTE]
> Tüm Tüketil 'lerin Linux düğümlerinde çalışması için zamanlanması gerekir. Kümenizde Linux düğüm havuzlarının yanı sıra Windows Server düğüm havuzlarınız varsa, tüm Tüketil 'lerin Linux düğümlerinde çalışacak şekilde zamanlandığını doğrulayın.

Bu noktada, AKS kümenize Tüketil 'yi dağıttık. Tüketil 'nin başarılı bir şekilde dağıtımına sahip olduğunuzdan emin olmak için, tüketim l yüklemesini doğrulamak üzere bir sonraki bölüme geçeceğiz.

## <a name="validate-the-consul-installation"></a>Tüketil yüklemesini doğrulama

Önce beklenen hizmetlerin oluşturulduğunu onaylayın. Çalışan hizmetleri görüntülemek için [kubectl Get svc][kubectl-get] komutunu kullanın. Tüketil bileşenlerinin `consul` Helm grafiği tarafından yüklendiği adalanınısorgulayın:`consul`

```console
kubectl get svc --namespace consul
```

Aşağıdaki örnek çıktıda Şu anda çalışıyor olması gereken hizmetler gösterilmektedir:

```console
NAME                                 TYPE           CLUSTER-IP     EXTERNAL-IP             PORT(S)                                                                   AGE
consul                               ExternalName   <none>         consul.service.consul   <none>                                                                    38m
consul-consul-connect-injector-svc   ClusterIP      10.0.89.113    <none>                  443/TCP                                                                   40m
consul-consul-dns                    ClusterIP      10.0.166.82    <none>                  53/TCP,53/UDP                                                             40m
consul-consul-server                 ClusterIP      None           <none>                  8500/TCP,8301/TCP,8301/UDP,8302/TCP,8302/UDP,8300/TCP,8600/TCP,8600/UDP   40m
consul-consul-ui                     ClusterIP      10.0.117.164   <none>                  80/TCP                                                                    40m
```

Ardından, gerekli yığınların oluşturulduğunu onaylayın. [Kubectl Get Pod][kubectl-get] komutunu kullanın ve `consul` ad alanını yeniden sorgulayın:

```console
kubectl get pods --namespace consul
```

Aşağıdaki örnek çıktı, çalıştıran Pod 'leri göstermektedir:

```console
NAME                                                              READY   STATUS    RESTARTS   AGE
consul-consul-7cc9v                                               1/1     Running   0          37m
consul-consul-7klg7                                               1/1     Running   0          37m
consul-consul-connect-injector-webhook-deployment-57f88df8hgmfs   1/1     Running   0          37m
consul-consul-lq8qb                                               1/1     Running   0          37m
consul-consul-server-0                                            1/1     Running   0          37m
consul-consul-server-1                                            1/1     Running   0          36m
consul-consul-server-2                                            1/1     Running   0          36m
consul-consul-sync-catalog-7cf7d5bfff-jjbjv                       1/1     Running   2          37m
```

 Tüm FID 'ler durumunu `Running`göstermelidir. Ayırımlarınızın bu durumları yoksa, tamamlanana kadar bir dakika veya iki tane bekleyin. Herhangi bir pod bir sorun bildirirse, çıktısını ve durumlarını gözden geçirmek için [kubectl 'yi bir pod betimleyen][kubectl-describe] komutunu kullanın.

## <a name="accessing-the-consul-ui"></a>Tüketil Kullanıcı arabirimine erişme

Tüketil Kullanıcı arabirimi, yukarıdaki kurulumumuza, Tüketil için Kullanıcı arabirimi tabanlı yapılandırma sağlayan bir şekilde yüklendi. Tüketil için Kullanıcı arabirimi, bir dış IP adresi aracılığıyla herkese açık bir şekilde gösterilmez. Eklenti kullanıcı arabirimlerine erişmek için [kubectl Port-Forward][kubectl-port-forward] komutunu kullanın. Bu komut, istemci makineniz ile AKS kümenizdeki ilgili Pod arasında güvenli bir bağlantı oluşturur.

```bash
kubectl port-forward -n consul svc/consul-consul-ui 8080:80
```
Artık bir tarayıcı açabilir ve bunu `http://localhost:8080/ui` , tüketil Kullanıcı arabirimini açmak için üzerine yazabilirsiniz. Kullanıcı arabirimini açtığınızda şunları görmeniz gerekir:

![Tüketil Kullanıcı arabirimi](./media/consul/consul-ui.png)

## <a name="uninstall-consul-from-aks"></a>AKS 'ten Tüketil 'yi kaldırma

> [!WARNING]
> Çalışan bir sistemden Tüketil 'nin silinmesi, hizmetleriniz arasında trafik ile ilgili sorunlar oluşmasına neden olabilir. Devam etmeden önce sisteminizin Tüketil olmadan doğru şekilde çalışmaya yönelik bir sağlama gerçekleştirdiğinizden emin olun.

### <a name="remove-consul-components-and-namespace"></a>Tüketil bileşenlerini ve ad alanını kaldır

AKS kümenizdeki Tüketil 'yi kaldırmak için aşağıdaki komutları kullanın. Komutlar grafiği kaldırır ve`kubectl delete ns` komut `consul` ad alanını kaldırır. `consul` `helm delete`

```bash
helm delete --purge consul
kubectl delete ns consul
```

## <a name="next-steps"></a>Sonraki adımlar

Tüketil için daha fazla yükleme ve yapılandırma seçeneği araştırmak için aşağıdaki resmi tüketim ve makaleleri inceleyin:

- [Tüketil-Helm Yükleme Kılavuzu][consul-install-k8]
- [Tüketil-Helm yükleme seçenekleri][consul-install-helm-options]

Ayrıca, [Tüketil örnek uygulamasını][consul-app-example]kullanarak ek senaryolar da izleyebilirsiniz.

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

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[consul-scenario-mtls]: ./consul-mtls.md
[helm-install]: ./kubernetes-helm.md
