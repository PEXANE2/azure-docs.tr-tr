---
title: Azure Kubernetes Service (AKS) içinde sertifikaları döndürme
description: Sertifikalarınızı bir Azure Kubernetes hizmeti (AKS) kümesinde döndürme hakkında bilgi edinin.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 11/15/2019
ms.author: zarhoads
ms.openlocfilehash: 02bfdbc840065558003b249e1e3ea52f46ec64d6
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77596276"
---
# <a name="rotate-certificates-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içinde sertifikaları döndürme

Azure Kubernetes hizmeti (AKS), birçok bileşeni ile kimlik doğrulaması için sertifikalar kullanır. Düzenli olarak, güvenlik veya ilke nedenleriyle bu sertifikaları döndürmeniz gerekebilir. Örneğin, tüm sertifikalarınızı her 90 günde bir döndürebilmeniz için bir ilkeniz olabilir.

Bu makalede, AKS kümenizdeki sertifikaların nasıl döndürübir gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure CLı sürüm 2.0.77 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

## <a name="aks-certificates-certificate-authorities-and-service-accounts"></a>AKS sertifikaları, sertifika yetkilileri ve hizmet hesapları

AKS aşağıdaki sertifikaları, sertifika yetkililerini ve hizmet hesaplarını oluşturur ve kullanır:

* AKS API sunucusu, küme CA 'sı adlı bir sertifika yetkilisi (CA) oluşturur.
* API sunucusunda, API sunucusundan kubeyalar 'a tek yönlü iletişim için sertifikalar imzalayan bir küme CA 'sı vardır.
* Her kubelet aynı zamanda, API sunucusuna kubelet 'ten iletişim sağlamak için, küme CA 'sı tarafından imzalanan bir sertifika Imzalama Isteği (CSR) oluşturur.
* Etcd anahtar değer deposunda, etcd 'den API sunucusuna iletişim için küme CA 'sı tarafından imzalanmış bir sertifika vardır.
* Etcd anahtar değeri deposu, AKS kümesindeki etcd çoğaltmaları arasında veri çoğaltmasına kimlik doğrulaması ve yetkilendirme için sertifikaları imzalayan bir CA oluşturur.
* API toplayıcısı, Azure için açık Hizmet Aracısı gibi diğer API 'lerle iletişim için sertifikalar vermek üzere Küme CA 'sını kullanır. API toplayıcısı 'nın bu sertifikaları vermek için kendi CA 'sı da olabilir, ancak şu anda küme CA 'sını kullanır.
* Her düğüm, küme CA 'sı tarafından imzalanmış bir hizmet hesabı (SA) belirteci kullanır.
* `kubectl` istemcisinde AKS kümesiyle iletişim kurmak için bir sertifika vardır.

> [!NOTE]
> Mart 2019 ' den önce oluşturulan AKS kümelerinde iki yıl sonra süre sonu olan sertifikalar vardır. Mart 2019 ' den sonra oluşturulan herhangi bir küme veya sertifikalarının döndürüldüğü herhangi bir küme 30 yıl sonra süresi dolacak olan sertifikalardır. Kümenizin oluşturulduğunu doğrulamak için, düğüm havuzlarınızın *yaşını* görmek üzere `kubectl get nodes` kullanın.
> 
> Ayrıca, kümenizin sertifikasının sona erme tarihini kontrol edebilirsiniz. Örneğin, aşağıdaki komut, *Myakscluster* kümesi için sertifika ayrıntılarını görüntüler.
> ```console
> kubectl config view --raw -o jsonpath="{.clusters[?(@.name == 'myAKSCluster')].cluster.certificate-authority-data}" | base64 -d > my-cert.crt
> openssl x509 -in my-cert.crt -text
> ```

## <a name="rotate-your-cluster-certificates"></a>Küme sertifikalarınızı döndürme

> [!WARNING]
> `az aks rotate-certs` kullanarak sertifikalarınızı döndürme, AKS kümeniz için 30 dakikalık kapalı kalma süresine kadar bir süre olabilir.

AKS kümenizde oturum açmak için [az aks Get-Credentials][az-aks-get-credentials] kullanın. Bu komut ayrıca yerel makinenizde `kubectl` istemci sertifikasını indirir ve yapılandırır.

```console
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

Kümenizdeki tüm sertifikaları, CA 'Ları ve SAs 'yi döndürmek için `az aks rotate-certs` kullanın.

```console
az aks rotate-certs -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

> [!IMPORTANT]
> `az aks rotate-certs` tamamlanması 30 dakika kadar sürebilir. Komut tamamlanmadan önce başarısız olursa, kümenin durumunun *sertifika döndürme*olduğunu doğrulamak için `az aks show` kullanın. Küme başarısız durumdaysa, sertifikalarınızı yeniden döndürmek için `az aks rotate-certs` yeniden çalıştırın.

`kubectl` komutu çalıştırarak eski sertifikaların artık geçerli olmadığını doğrulayın. `kubectl`tarafından kullanılan sertifikaları güncelleştirmediyseniz bir hata görürsünüz.  Örnek:

```console
$ kubectl get no
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "ca")
```

`az aks get-credentials`çalıştırarak `kubectl` tarafından kullanılan sertifikayı güncelleştirin.

```console
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME --overwrite-existing
```

`kubectl` komutu çalıştırılarak sertifikaların güncelleştirildiğini doğrulayın, bu, şimdi başarılı olur. Örnek:

```console
kubectl get no
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kümenizin sertifikalarını, CA 'Ları ve SAs 'yi otomatik olarak döndürme konusu gösterilmektedir. AKS güvenlik en iyi uygulamaları hakkında daha fazla bilgi için [Azure Kubernetes Service 'te (aks) küme güvenliği ve yükseltmeleri Için en iyi uygulamaları][aks-best-practices-security-upgrades] görebilirsiniz.


[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[aks-best-practices-security-upgrades]: operator-best-practices-cluster-security.md
