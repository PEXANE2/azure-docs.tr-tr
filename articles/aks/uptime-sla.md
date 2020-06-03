---
title: Çalışma süresi SLA 'Sı ile Azure Kubernetes hizmeti (AKS)
description: Azure Kubernetes hizmeti (AKS) API sunucusu için isteğe bağlı çalışma süresi SLA teklifi hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 05/19/2020
ms.custom: references_regions
ms.openlocfilehash: 2df0ad675f03b25363ab0f5b13dceb762a657ed7
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84299562"
---
# <a name="azure-kubernetes-service-aks-uptime-sla"></a>Azure Kubernetes hizmeti (AKS) çalışma süresi SLA 'Sı

Çalışma süresi SLA 'Sı, bir küme için mali olarak desteklenen ve daha yüksek SLA 'yı etkinleştirmeye yönelik isteğe bağlı bir özelliktir Çalışma süresi SLA, Kullanılabilirlik Alanları kullanmayan kümeler için [kullanılabilirlik alanları][availability-zones] ve% 99,9 ' i kullanan kümeler Için Kubernetes API sunucu uç noktasının% 99,95 kullanılabilirliğini garanti eder. AKS, SLA gereksinimlerinin karşılanmasını sağlamak için güncelleştirme ve hata etki alanları genelinde ana düğüm çoğaltmaları kullanır.

Uyumluluk gereksinimlerini karşılamak için SLA 'ya ihtiyaç duyan veya son kullanıcılar için SLA genişletmeyi gerektiren müşterilerin bu özelliği etkinleştirmesi gerekir. Kritik iş yükleri olan müşteriler, daha yüksek bir çalışma süresi SLA 'Sı üzerinden faydalanabilir. Çalışma süresi SLA özelliğinin Kullanılabilirlik Alanları ile kullanılması, Kubernetes API sunucusunun çalışma süresi için daha yüksek bir kullanılabilirlik sağlar.  

Müşteriler, bir hizmet düzeyi hedefi (SLO 99,5) olan sınırsız sayıda ücretsiz küme oluşturabilir ve tercih edilen SLO veya SLA çalışma süresini gerektiği gibi kabul edebilir.

> [!Important]
> Çıkış kilidi olan kümeler için bkz. uygun bağlantı noktalarını açmak için [çıkış trafiğini sınırlayın](limit-egress-traffic.md) .

## <a name="sla-terms-and-conditions"></a>SLA hüküm ve koşulları

Çalışma süresi SLA 'Sı ücretli bir özelliktir ve küme başına etkindir. Çalışma süresi SLA fiyatlandırması, ayrı kümelerin boyutuna göre değil, farklı kümelerin sayısıyla belirlenir. Daha fazla bilgi için [çalışma SÜRESI SLA fiyatlandırma ayrıntılarını](https://azure.microsoft.com/pricing/details/kubernetes-service/) görüntüleyebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure CLı sürüm 2.7.0 veya üzeri

## <a name="creating-a-cluster-with-uptime-sla"></a>Çalışma süresi SLA 'Sı ile küme oluşturma

Çalışma süresi SLA 'Sı ile yeni bir küme oluşturmak için Azure CLı 'yi kullanırsınız.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```
AKS kümesi oluşturmak için [az aks create][az-aks-create] komutunu kullanın. Aşağıdaki örnekte, bir düğüm ile *myAKSCluster* adlı bir küme oluşturulmuştur. *--enable-addons monitoring* parametresiyle Kapsayıcılar için Azure İzleyici de etkinleştirilmiştir.  Bu işlemin tamamlanabilmesi birkaç dakika sürer.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster --uptime-sla --node-count 1 --enable-addons monitoring --generate-ssh-keys
```
Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON biçimli bilgileri döndürür. Aşağıdaki JSON kod parçacığında, kümenizin çalışma süresi SLA 'Sı ile etkinleştirildiğini belirten SKU 'nun ücretli katmanı gösterilmektedir.

```output
  },
  "sku": {
    "name": "Basic",
    "tier": "Paid"
  },
```

## <a name="limitations"></a>Sınırlamalar

* Şu anda, çalışma süresi SLA 'sını etkinleştirmek için mevcut küme olarak dönüştürülemiyor.
* Şu anda, bir AKS kümesinden çalışma süresi SLA 'sını, oluşturulduktan sonra oluşturma özelliği etkinken kaldırmanın bir yolu yoktur.  
* Özel kümeler Şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

AKS kümesi iş yüklerinizde yüksek kullanılabilirliği artırmak için [kullanılabilirlik alanları][availability-zones] kullanın.
Kümenizi [çıkış trafiğini sınırlayacak](limit-egress-traffic.md)şekilde yapılandırın.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service

<!-- LINKS - Internal -->
[vm-skus]: ../virtual-machines/linux/sizes.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[faq]: ./faq.md
[availability-zones]: ./availability-zones.md
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[limit-egress-traffic]: ./limit-egress-traffic.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
