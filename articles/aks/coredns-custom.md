---
title: Azure Kubernetes Hizmeti (AKS) için CoreDNS'i özelleştirin
description: Azure Kubernetes Hizmeti 'ni (AKS) kullanarak alt etki alanları eklemek veya özel DNS uç noktalarını genişletmek için CoreDNS'yi nasıl özelleştirebilirsiniz öğrenin
services: container-service
author: jnoller
ms.topic: article
ms.date: 03/15/2019
ms.author: jenoller
ms.openlocfilehash: 78132a53313f4a8ee5c10af340c8dab08c3e42c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77595833"
---
# <a name="customize-coredns-with-azure-kubernetes-service"></a>Azure Kubernetes Service ile CoreDNS hizmetini özelleştirme

Azure Kubernetes Service (AKS), *1.12.x* ve daha yüksek kümelerle küme DNS yönetimi ve çözümü için [CoreDNS][coredns] projesini kullanır. Daha önce kube-dns projesi kullanılmıştır. Bu kube-dns projesi artık küçümsül. CoreDNS özelleştirme ve Kubernetes hakkında daha fazla bilgi [için, resmi upstream belgelerine][corednsk8s]bakın.

AKS yönetilen bir hizmet olduğundan, CoreDNS *(CoreFile)* için ana yapılandırmayı değiştiremezsiniz. Bunun yerine, varsayılan ayarları geçersiz kılmak için bir Kubernetes *ConfigMap* kullanırsınız. Varsayılan AKS CoreDNS ConfigMaps'i `kubectl get configmaps --namespace=kube-system coredns -o yaml` görmek için komutu kullanın.

Bu makalede, AKS CoreDNS temel özelleştirme seçenekleri için ConfigMaps nasıl kullanılacağını gösterir. Bu yaklaşım, CoreFile'ı kullanmak gibi diğer bağlamlarda CoreDNS yapılandırmaktan farklıdır. Yapılandırma değerleri sürümler arasında değişebileceğinden, çalıştırdığınız CoreDNS sürümünü doğrulayın.

> [!NOTE]
> `kube-dns`bir Kubernetes config harita üzerinden farklı [özelleştirme seçenekleri][kubednsblog] sundu. CoreDNS geriye doğru kube-dns ile uyumlu **değildir.** Daha önce kullandığınız özelleştirmeler CoreDNS ile kullanılmak üzere güncelleştirilmelidir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

## <a name="what-is-supportedunsupported"></a>Desteklenen/desteklenmeyen ler

Tüm yerleşik CoreDNS eklentileri desteklenir. Hiçbir eklenti/üçüncü taraf eklentisi desteklenmez.

## <a name="rewrite-dns"></a>DNS'yi yeniden yazın

Sahip olduğunuz senaryolardan biri, anında DNS adının yeniden yazmasını gerçekleştirmektir. Aşağıdaki örnekte, `<domain to be written>` kendi tam nitelikli alan adınızı değiştirin. Adlandırılmış `corednsms.yaml` bir dosya oluşturun ve aşağıdaki örnek yapılandırmayı yapıştırın:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    <domain to be rewritten>.com:53 {
        errors
        cache 30
        rewrite name substring <domain to be rewritten>.com default.svc.cluster.local
        forward .  /etc/resolv.conf # you can redirect this to a specific DNS server such as 10.0.0.10
    }
```

[Kubectl uygula configmap][kubectl-apply] komutunu kullanarak ConfigMap'i oluşturun ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f corednsms.yaml
```

Özelleştirmelerin uygulandığını doğrulamak [için, kubectl configmaps almak][kubectl-get] kullanın ve *coredns-özel* ConfigMap belirtin:

```
kubectl get configmaps --namespace=kube-system coredns-custom -o yaml
```

Şimdi ConfigMap yeniden CoreDNS zorlar. [Kubectl delete pod][kubectl delete] komutu yıkıcı değildir ve zaman adabına neden olmaz. Bölmeler `kube-dns` silinir ve Kubernetes Zamanlayıcısı bunları yeniden oluşturur. Bu yeni bölmeler TTL değerindeki değişimi içerir.

```console
kubectl delete pod --namespace kube-system -l k8s-app=kube-dns
```

> [!Note]
> Yukarıdaki komut doğrudur. Biz değiştirirken, `coredns`dağıtım **kube-dns** adı altında.

## <a name="custom-forward-server"></a>Özel ileri sunucu

Ağ trafiğiniz için bir ileri sunucu belirtmeniz gerekiyorsa, DNS'yi özelleştirmek için bir ConfigMap oluşturabilirsiniz. Aşağıdaki örnekte, `forward` adı ve adresi kendi ortamınız için değerlerle güncelleştirin. Adlandırılmış `corednsms.yaml` bir dosya oluşturun ve aşağıdaki örnek yapılandırmayı yapıştırın:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    <domain to be rewritten>.com:53 {
        forward foo.com 1.1.1.1
    }
```

Önceki örneklerde olduğu gibi, [kubectl configmap][kubectl-apply] komutunu kullanarak ConfigMap'i oluşturun ve YAML manifestonuzun adını belirtin. Daha sonra CoreDNS'i, Kubernetes Zamanlayıcısı'nın [kubectl silme bölmesini][kubectl delete] kullanarak ConfigMap'i yeniden yüklemeye zorlayarak yeniden oluşturabilirsiniz:

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --selector k8s-app=kube-dns
```

## <a name="use-custom-domains"></a>Özel etki alanlarını kullanma

Yalnızca dahili olarak çözülebilecek özel etki alanlarını yapılandırmak isteyebilirsiniz. Örneğin, geçerli bir üst düzey etki alanı olmayan özel *puglife.local*etki alanını çözmek isteyebilirsiniz. Özel bir etki alanı ConfigMap olmadan, AKS kümesi adresi çözemez.

Aşağıdaki örnekte, trafiği kendi ortamınız için değerlerle yönlendirmek için özel etki alanını ve IP adresini güncelleştirin. Adlandırılmış `corednsms.yaml` bir dosya oluşturun ve aşağıdaki örnek yapılandırmayı yapıştırın:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  puglife.server: |
    puglife.local:53 {
        errors
        cache 30
        forward . 192.11.0.1  # this is my test/dev DNS server
    }
```

Önceki örneklerde olduğu gibi, [kubectl configmap][kubectl-apply] komutunu kullanarak ConfigMap'i oluşturun ve YAML manifestonuzun adını belirtin. Daha sonra CoreDNS'i, Kubernetes Zamanlayıcısı'nın [kubectl silme bölmesini][kubectl delete] kullanarak ConfigMap'i yeniden yüklemeye zorlayarak yeniden oluşturabilirsiniz:

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --selector k8s-app=kube-dns
```

## <a name="stub-domains"></a>Saplama etki alanları

CoreDNS, saplama etki alanlarını yapılandırmak için de kullanılabilir. Aşağıdaki örnekte, özel etki alanlarını ve IP adreslerini kendi ortamınız için değerlerle güncelleştirin. Adlandırılmış `corednsms.yaml` bir dosya oluşturun ve aşağıdaki örnek yapılandırmayı yapıştırın:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    abc.com:53 {
        errors
        cache 30
        forward . 1.2.3.4
    }
    my.cluster.local:53 {
        errors
        cache 30
        forward . 2.3.4.5
    }

```

Önceki örneklerde olduğu gibi, [kubectl configmap][kubectl-apply] komutunu kullanarak ConfigMap'i oluşturun ve YAML manifestonuzun adını belirtin. Daha sonra CoreDNS'i, Kubernetes Zamanlayıcısı'nın [kubectl silme bölmesini][kubectl delete] kullanarak ConfigMap'i yeniden yüklemeye zorlayarak yeniden oluşturabilirsiniz:

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --selector k8s-app=kube-dns
```

## <a name="hosts-plugin"></a>Hosts eklentisi

Tüm yerleşik eklentiler desteklendikçe, CoreDNS [Hosts][coredns hosts] eklentisinin de özelleştirilebilen bir eklentisi vardır:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom # this is the name of the configmap you can overwrite with your changes
  namespace: kube-system
data:
    test.override: |
          hosts example.hosts example.org { # example.hosts must be a file
              10.0.0.1 example.org
              fallthrough
          }
```

## <a name="enable-logging-for-dns-query-debugging"></a>DNS sorgu hata ayıklama için günlüğe kaydetmeyi etkinleştirme 

DNS sorgu günlüğe kaydetmeyi etkinleştirmek için, coredns-custom ConfigMap'inizde aşağıdaki yapılandırmayı uygulayın:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  log.override: |
        log
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, CoreDNS özelleştirme için bazı örnek senaryolar gösterilmiş. CoreDNS projesi hakkında daha fazla bilgi için [CoreDNS yukarı akış proje sayfasına][coredns]bakın.

Temel ağ kavramları hakkında daha fazla bilgi edinmek [için AKS'deki uygulamalar için Ağ kavramlarına][concepts-network]bakın.

<!-- LINKS - external -->
[kubednsblog]: https://www.danielstechblog.io/using-custom-dns-server-for-domain-specific-name-resolution-with-azure-kubernetes-service/
[coredns]: https://coredns.io/
[corednsk8s]: https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns
[dnscache]: https://coredns.io/plugins/cache/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[coredns hosts]: https://coredns.io/plugins/hosts/

<!-- LINKS - internal -->
[concepts-network]: concepts-network.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
