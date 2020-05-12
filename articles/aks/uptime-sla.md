---
title: Azure Kubernetes hizmeti (AKS) çalışma süresi SLA 'Sı ile yüksek kullanılabilirlik
description: Azure Kubernetes Service (AKS) API sunucusu için isteğe bağlı yüksek kullanılabilirlik çalışma süresi SLA teklifi hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 05/11/2020
ms.openlocfilehash: 1c340f85a107cac437e1241025d8c9bc6991b965
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83125732"
---
# <a name="azure-kubernetes-service-aks-uptime-sla"></a>Azure Kubernetes hizmeti (AKS) çalışma süresi SLA 'Sı

Çalışma süresi SLA, bir küme için mali olarak desteklenen daha yüksek SLA 'yı etkinleştirmek üzere isteğe bağlı bir özelliktir. Çalışma süresi SLA 'Sı, kullanılabilirlik [bölgesi][availability-zones] kullanan kümeler Için Kubernetes API sunucu uç noktasının% 99,95 kullanılabilirliğini garanti eder ve kullanılabilirlik alanlarını kullanmayan kümeler için% 99,9 kullanılabilirlik sağlar. AKS, SLA gereksinimlerinin karşılanmasını sağlamak için güncelleştirme ve hata etki alanları genelinde ana düğüm çoğaltmaları kullanır.

Uyumluluk nedenleriyle SLA 'ya veya SLA 'nın müşterilerine genişlemelerini gerektiren müşterilerin bu özelliği açmanız gerekir. Bu özelliğin etkinleştirilmesiyle ilgili bir SLA avantajı seçeneği ile daha yüksek kullanılabilirlik gerektiren kritik iş yükleri olan müşteriler. Kubernetes API sunucusunun daha yüksek kullanılabilirliğini elde etmek için Kullanılabilirlik Alanları özelliğini etkinleştirin.  

Müşteriler, hizmet düzeyi hedefi (SLO) ile% 99,5 arasında sınırsız sayıda boş küme oluşturabilir.

> [!Important]
> Çıkış kilidi olan kümeler için bkz. çalışma süresi SLA 'Sı için uygun bağlantı noktalarını açmak üzere [çıkış trafiğini sınırlayın](limit-egress-traffic.md) .

## <a name="sla-terms-and-conditions"></a>SLA hüküm ve koşulları

Çalışma süresi SLA 'Sı ücretli bir özelliktir ve küme başına etkindir. Çalışma süresi SLA fiyatlandırması, kümelerin boyutuyla değil, küme sayısına göre belirlenir. Daha fazla bilgi için [çalışma SÜRESI SLA fiyatlandırma ayrıntılarını](https://azure.microsoft.com/pricing/details/kubernetes-service/) görüntüleyebilirsiniz.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Çalışma süresi SLA 'Sı aşağıdaki bölgelerde kullanılabilir:

* Doğu Avustralya
* Orta Kanada
* Doğu ABD
* Doğu ABD 2
* Orta Güney ABD
* Güneydoğu Asya
* Batı ABD 2

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
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters",
  "windowsProfile": null
```

## <a name="limitations"></a>Sınırlamalar

* Şu anda mevcut kümelere çalışma süresi SLA 'Sı ekleyemezsiniz.
* Şu anda bir AKS kümesinden çalışma süresi SLA 'Sı kaldırmanın bir yolu yoktur.  

## <a name="next-steps"></a>Sonraki adımlar

AKS kümesi iş yüklerinizde yüksek kullanılabilirliği artırmak için [kullanılabilirlik alanları][availability-zones] kullanın.

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
