---
title: Azure Kubernetes Hizmetinde (AKS) sertifikaları döndürme
description: Sertifikalarınızı Azure Kubernetes Hizmeti (AKS) kümesinde nasıl döndürdüğünüzü öğrenin.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 11/15/2019
ms.author: zarhoads
ms.openlocfilehash: 00dcef4ae0f04fc7f550859238ae8c7e1ad19384
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549062"
---
# <a name="rotate-certificates-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) sertifikaları döndürme

Azure Kubernetes Service (AKS), birçok bileşeniyle kimlik doğrulama için sertifikalar kullanır. Düzenli aralıklarla, bu sertifikaları güvenlik veya ilke nedenleriyle döndürmeniz gerekebilir. Örneğin, tüm sertifikalarınızı her 90 günde bir döndürecek bir ilkeniz olabilir.

Bu makalede, AKS kümenizdeki sertifikaları nasıl döndüreceklerini gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure CLI sürümünü 2.0.77 veya daha sonra çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="aks-certificates-certificate-authorities-and-service-accounts"></a>AKS sertifikaları, Sertifika Yetkilileri ve Hizmet Hesapları

AKS aşağıdaki sertifikaları, Sertifika Yetkililerini ve Hizmet Hesaplarını oluşturur ve kullanır:

* AKS API sunucusu, Küme CA adında bir Sertifika Yetkilisi (CA) oluşturur.
* API sunucusu, API sunucusundan kubelets'e tek yönlü iletişim için sertifikalar imzalayan bir Küme CA'ya sahiptir.
* Her kubelet ayrıca, kubelet'den API sunucusuna iletişim için Küme CA tarafından imzalanan bir Sertifika İmzalama İsteği (CSR) oluşturur.
* Etcd anahtar değer deposu, PD'den API sunucusuna iletişim için Cluster CA tarafından imzalanmış bir sertifikaya sahiptir.
* Etcd anahtar değer deposu, AKS kümesindeki etcd yinelemeleri arasında veri çoğaltmanın doğruluğunu doğrulamak ve yetkilendirmek için sertifikaları imzalayan bir CA oluşturur.
* API toplayıcısı, diğer API'lerle iletişim için sertifika vermek için Küme CA'sını kullanır. API toplayıcısı da bu sertifikaları vermek için kendi CA'sına sahip olabilir, ancak şu anda Küme CA'sını kullanır.
* Her düğüm, Küme CA tarafından imzalanmış bir Hizmet Hesabı (SA) belirteci kullanır.
* İstemcinin `kubectl` AKS kümesiyle iletişim kurma sertifikası vardır.

> [!NOTE]
> Mart 2019'dan önce oluşturulan AKS kümelerinin iki yıl sonra süresi dolan sertifikaları vardır. Mart 2019'dan sonra oluşturulan herhangi bir küme veya sertifikaları döndürülmüş olan herhangi bir küme, 30 yıl sonra süresi dolan Küme CA sertifikalarına sahiptir. Diğer tüm sertifikaların süresi iki yıl sonra sona erer. Kümenizin ne zaman oluşturulduğunu doğrulamak için düğüm havuzlarının `kubectl get nodes` *Yaşını* görmek için kullanın.
> 
> Ayrıca, kümenizin sertifikasının son kullanma tarihini de kontrol edebilirsiniz. Örneğin, aşağıdaki komut *myAKSCluster* kümesinin sertifika ayrıntılarını görüntüler.
> ```console
> kubectl config view --raw -o jsonpath="{.clusters[?(@.name == 'myAKSCluster')].cluster.certificate-authority-data}" | base64 -d > my-cert.crt
> openssl x509 -in my-cert.crt -text
> ```

## <a name="rotate-your-cluster-certificates"></a>Küme sertifikalarınızı döndürme

> [!WARNING]
> Sertifikalarınızı kullanarak `az aks rotate-certs` döndürmek, AKS kümeniz için 30 dakikaya kadar kapalı kalma süresine neden olabilir.

AKS kümenizde oturum açabilmek için [az aks get-credentials'ı][az-aks-get-credentials] kullanın. Bu komut ayrıca istemci sertifikasını `kubectl` yerel makinenizde indirir ve yapılandırır.

```azurecli
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

Kümenizdeki tüm sertifikaları, CA'ları ve SA'ları döndürmek için kullanın. `az aks rotate-certs`

```azurecli
az aks rotate-certs -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

> [!IMPORTANT]
> Tamamlanması 30 dakika `az aks rotate-certs` kadar sürebilir. Komut tamamlanmadan önce başarısız `az aks show` olursa, kümenin durumunu doğrulamak için kullanın *Sertifika Döndürme*. Küme başarısız durumdaysa, sertifikalarınızı `az aks rotate-certs` yeniden döndürmek için yeniden çalıştırın.

Bir `kubectl` komut çalıştırarak eski sertifikaların artık geçerli olmadığını doğrulayın. Kullandığınız `kubectl`sertifikaları güncelleştirmediğiniz için bir hata görürsünüz.  Örnek:

```console
$ kubectl get no
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "ca")
```

`kubectl` Çalıştırarak kullanılan sertifikayı `az aks get-credentials`güncelleştirin.

```azurecli
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME --overwrite-existing
```

Sertifikaların artık başarılı olacak bir `kubectl` komut çalıştırarak güncelleştirildiğini doğrulayın. Örnek:

```console
kubectl get no
```

> [!NOTE]
> [Azure Dev Spaces][dev-spaces]gibi AKS'nin üstünde çalışan hizmetleriniz varsa, [bu hizmetlerle ilgili sertifikaları][dev-spaces-rotate] da güncelleştirmeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kümenizin sertifikalarını, CA'larını ve SA'larını otomatik olarak nasıl döndürdüğünüz gösterilen. AKS güvenlik en iyi uygulamaları hakkında daha fazla bilgi için [Azure Kubernetes Hizmeti'nde (AKS) küme güvenliği ve yükseltmeler için en][aks-best-practices-security-upgrades] iyi uygulamaları görebilirsiniz.


[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[aks-best-practices-security-upgrades]: operator-best-practices-cluster-security.md
[dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[dev-spaces-rotate]: ../dev-spaces/troubleshooting.md#error-using-dev-spaces-after-rotating-aks-certificates
