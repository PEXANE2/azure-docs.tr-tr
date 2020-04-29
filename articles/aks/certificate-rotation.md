---
title: Azure Kubernetes Service (AKS) içinde sertifikaları döndürme
description: Sertifikalarınızı bir Azure Kubernetes hizmeti (AKS) kümesinde döndürme hakkında bilgi edinin.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 11/15/2019
ms.author: zarhoads
ms.openlocfilehash: 00dcef4ae0f04fc7f550859238ae8c7e1ad19384
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80549062"
---
# <a name="rotate-certificates-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içinde sertifikaları döndürme

Azure Kubernetes hizmeti (AKS), birçok bileşeni ile kimlik doğrulaması için sertifikalar kullanır. Düzenli olarak, güvenlik veya ilke nedenleriyle bu sertifikaları döndürmeniz gerekebilir. Örneğin, tüm sertifikalarınızı her 90 günde bir döndürebilmeniz için bir ilkeniz olabilir.

Bu makalede, AKS kümenizdeki sertifikaların nasıl döndürübir gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure CLı sürüm 2.0.77 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="aks-certificates-certificate-authorities-and-service-accounts"></a>AKS sertifikaları, sertifika yetkilileri ve hizmet hesapları

AKS aşağıdaki sertifikaları, sertifika yetkililerini ve hizmet hesaplarını oluşturur ve kullanır:

* AKS API sunucusu, küme CA 'sı adlı bir sertifika yetkilisi (CA) oluşturur.
* API sunucusunda, API sunucusundan kubeyalar 'a tek yönlü iletişim için sertifikalar imzalayan bir küme CA 'sı vardır.
* Her kubelet aynı zamanda, API sunucusuna kubelet 'ten iletişim sağlamak için, küme CA 'sı tarafından imzalanan bir sertifika Imzalama Isteği (CSR) oluşturur.
* Etcd anahtar değer deposunda, etcd 'den API sunucusuna iletişim için küme CA 'sı tarafından imzalanmış bir sertifika vardır.
* Etcd anahtar değeri deposu, AKS kümesindeki etcd çoğaltmaları arasında veri çoğaltmasına kimlik doğrulaması ve yetkilendirme için sertifikaları imzalayan bir CA oluşturur.
* API toplayıcısı, diğer API 'lerle iletişim için sertifikalar vermek üzere Küme CA 'sını kullanır. API toplayıcısı 'nın bu sertifikaları vermek için kendi CA 'sı da olabilir, ancak şu anda küme CA 'sını kullanır.
* Her düğüm, küme CA 'sı tarafından imzalanmış bir hizmet hesabı (SA) belirteci kullanır.
* `kubectl` İstemcinin aks kümesiyle iletişim kurmak için bir sertifikası vardır.

> [!NOTE]
> Mart 2019 ' den önce oluşturulan AKS kümelerinde iki yıl sonra süre sonu olan sertifikalar vardır. Mart 2019 ' den sonra oluşturulan herhangi bir küme veya sertifikaları döndürülen herhangi bir küme, 30 yıl sonra süresi dolan küme CA sertifikalardır. Diğer tüm sertifikaların süreleri iki yıl sonra doluyor. Kümenizin oluşturulduğunu doğrulamak için, düğüm havuzlarınızın `kubectl get nodes` *yaşını* görmek üzere kullanın.
> 
> Ayrıca, kümenizin sertifikasının sona erme tarihini kontrol edebilirsiniz. Örneğin, aşağıdaki komut, *Myakscluster* kümesi için sertifika ayrıntılarını görüntüler.
> ```console
> kubectl config view --raw -o jsonpath="{.clusters[?(@.name == 'myAKSCluster')].cluster.certificate-authority-data}" | base64 -d > my-cert.crt
> openssl x509 -in my-cert.crt -text
> ```

## <a name="rotate-your-cluster-certificates"></a>Küme sertifikalarınızı döndürme

> [!WARNING]
> Kullanarak `az aks rotate-certs` sertifikalarınızı döndürme, aks kümeniz için 30 dakikalık kapalı kalma süresine kadar zaman alabilir.

AKS kümenizde oturum açmak için [az aks Get-Credentials][az-aks-get-credentials] kullanın. Bu komut ayrıca yerel makinenizde `kubectl` istemci sertifikasını indirir ve yapılandırır.

```azurecli
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

Kümenizdeki `az aks rotate-certs` tüm sertifikaları, CA 'Ları ve SAS 'yi döndürmek için kullanın.

```azurecli
az aks rotate-certs -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

> [!IMPORTANT]
> Tamamlanması 30 dakika `az aks rotate-certs` sürebilir. Komut tamamlanmadan önce başarısız olursa, kümenin durumunun `az aks show` *sertifika döndürme*olduğunu doğrulamak için kullanın. Küme başarısız durumdaysa, sertifikalarınızı yeniden döndürmek için yeniden çalıştırın `az aks rotate-certs` .

Bir `kubectl` komut çalıştırarak eski sertifikaların artık geçerli olmadığını doğrulayın. Tarafından `kubectl`kullanılan sertifikaları güncelleştirmediyseniz bir hata görürsünüz.  Örneğin:

```console
$ kubectl get no
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "ca")
```

Çalıştırarak `kubectl` `az aks get-credentials`tarafından kullanılan sertifikayı güncelleştirin.

```azurecli
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME --overwrite-existing
```

Bir `kubectl` komut çalıştırarak sertifikaların güncelleştirildiğini doğrulayın, şimdi başarılı olur. Örneğin:

```console
kubectl get no
```

> [!NOTE]
> AKS üzerinde çalışan, [Azure dev Spaces][dev-spaces]gibi bir hizmetleriniz varsa, [Bu hizmetlerle ilgili sertifikaları da güncelleştirmeniz][dev-spaces-rotate] gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kümenizin sertifikalarını, CA 'Ları ve SAs 'yi otomatik olarak döndürme konusu gösterilmektedir. AKS güvenlik en iyi uygulamaları hakkında daha fazla bilgi için [Azure Kubernetes Service 'te (aks) küme güvenliği ve yükseltmeleri Için en iyi uygulamaları][aks-best-practices-security-upgrades] görebilirsiniz.


[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[aks-best-practices-security-upgrades]: operator-best-practices-cluster-security.md
[dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[dev-spaces-rotate]: ../dev-spaces/troubleshooting.md#error-using-dev-spaces-after-rotating-aks-certificates
