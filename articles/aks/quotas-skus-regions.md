---
title: Kaynaklar, SDO'lar, bölgeler için sınırlar
titleSuffix: Azure Kubernetes Service
description: Varsayılan kotalar, kısıtlı düğüm VM SKU boyutları ve Azure Kubernetes Hizmetinin (AKS) bölge kullanılabilirliği hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 04/09/2019
ms.openlocfilehash: 054d6ff4fc105d84192ac81feda97515f6cfae49
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886781"
---
# <a name="quotas-virtual-machine-size-restrictions-and-region-availability-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) kotalar, sanal makine boyutu kısıtlamaları ve bölge kullanılabilirliği

Tüm Azure hizmetleri, kaynaklar ve özellikler için varsayılan sınırlar ve kotalar belirler. Bazı sanal makine (VM) SUS'lar da kullanım için sınırlıdır.

Bu makalede, Azure Kubernetes Hizmeti (AKS) kaynakları için varsayılan kaynak sınırları ve AkS'nin Azure bölgelerinde kullanılabilirliği ayrıntılı olarak açıklanmaktadır.

## <a name="service-quotas-and-limits"></a>Hizmet kotaları ve limitleri

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>Sağlanan altyapı

Tüm diğer ağ, işlem ve depolama sınırları, sağlanan altyapıya uygulanır. İlgili sınırlar için [Azure abonelik ve hizmet sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md)bakın.

> [!IMPORTANT]
> Bir AKS kümesini yükselttiğinde, ek kaynaklar geçici olarak tüketilir. Bu kaynaklar, sanal ağ alt ağındaki kullanılabilir IP adreslerini veya sanal makine vCPU kotası içerir. Windows Server kapsayıcıları kullanıyorsanız (şu anda AKS önizlemede), düğümlere en son güncelleştirmeleri uygulamak için onaylanan tek yaklaşım bir yükseltme işlemi gerçekleştirmektir. Başarısız küme yükseltme işlemi, bu geçici kaynakları işlemek için kullanılabilir IP adresi alanı veya vCPU kotası olmadığını gösterebilir. Windows Server düğüm yükseltme işlemi hakkında daha fazla bilgi için [AKS'de düğüm havuzunu yükseltme bölümüne][nodepool-upgrade]bakın.

## <a name="restricted-vm-sizes"></a>Kısıtlı VM boyutları

AKS kümesindeki her düğüm, vCPU ve bellek gibi sabit miktarda işlem kaynağı içerir. Bir AKS düğümü yetersiz bilgi işlem kaynakları içeriyorsa, bölmeler düzgün çalışmayabilir. Gerekli *kube-sistem* bölmelerinin ve uygulamalarınızın güvenilir bir şekilde zamanlanabilmesini sağlamak için **AKS'de aşağıdaki VM SKU'ları kullanmayın:**

- Standard_A0
- Standard_A1
- Standard_A1_v2
- Standard_B1s
- Standard_B1ms
- Standard_F1
- Standard_F1s

VM türleri ve bunların bilgi işlem kaynakları hakkında daha fazla bilgi için [Azure'daki sanal makinelerin Boyutlar'ına][vm-skus]bakın.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Kümeleri dağıtabileceğiniz ve çalıştırabileceğiniz en son liste için [AKS bölge kullanılabilirliğine][region-availability]bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Kaynağınız bir artışı destekliyorsa, Azure [destek isteği][azure-support] aracılığıyla artışı isteyin **(Sorun türü**için , **Kota'yı**seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service

<!-- LINKS - Internal -->
[vm-skus]: ../virtual-machines/linux/sizes.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
