---
title: Azure Kubernetes hizmeti (AKS) düğüm görüntülerini yükseltme
description: AKS küme düğümleri ve düğüm havuzlarındaki görüntüleri nasıl yükselteceğinizi öğrenin.
author: laurenhughes
ms.author: lahugh
ms.service: container-service
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: 20379f6a1e87c7553d6567be5b50f22bbadb8db7
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84514690"
---
# <a name="preview---azure-kubernetes-service-aks-node-image-upgrades"></a>Önizleme-Azure Kubernetes Service (AKS) düğümü görüntü yükseltmeleri

AKS, en yeni işletim sistemi ve çalışma zamanı güncelleştirmeleriyle güncel olması için bir düğümdeki görüntülerin yükseltilmesini destekler. AKS, en son güncelleştirmelerle hafta başına yeni bir görüntü sağlar; bu nedenle, Linux veya Windows yamaları dahil olmak üzere en son özellikler için düğümlerinizin görüntülerini düzenli olarak yükseltmeniz yararlı olur. Bu makalede, AKS küme düğümü görüntülerinin nasıl yükseltilebileceği ve Kubernetes 'in sürümünü yükseltmeden önce düğüm havuzu görüntülerinin nasıl güncelleşileceği gösterilmektedir.

AKS tarafından sunulan en son görüntüler hakkında bilgi edinmek istiyorsanız, daha fazla ayrıntı için bkz. [aks sürüm notları](https://github.com/Azure/AKS/releases) .

Kümeniz için Kubernetes sürümünü yükseltme hakkında daha fazla bilgi için bkz. [AKS kümesini yükseltme][upgrade-cluster].

## <a name="register-the-node-image-upgrade-preview-feature"></a>Düğüm görüntüsü yükseltme önizleme özelliğini kaydetme

Önizleme döneminde düğüm görüntüsü yükseltme özelliğini kullanmak için, özelliği kaydetmeniz gerekir.

```azurecli
# Register the preview feature
az feature register --namespace "Microsoft.ContainerService" --name "NodeImageUpgradePreview"
```

Kaydın tamamlanması birkaç dakika sürer. Özelliğin kaydedildiğini doğrulamak için aşağıdaki komutu kullanın:

```azurecli
# Verify the feature is registered:
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/NodeImageUpgradePreview')].{Name:name,State:properties.state}"
```

Önizleme süresince, düğüm görüntüsü yükseltmesini kullanabilmeniz için *aks-Preview* CLI uzantısının olması gerekir. [Az EXTENSION Add] [az-Extension-Add] komutunu kullanın ve ardından [az Extension Update] [az-Extension-Update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="upgrade-all-nodes-in-all-node-pools"></a>Tüm düğüm havuzlarındaki tüm düğümleri yükselt

Düğüm görüntüsünü yükseltmek ile yapılır `az aks upgrade` . Düğüm görüntüsünü yükseltmek için aşağıdaki komutu kullanın:

```azurecli
az aks upgrade \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-image-only
```

Yükseltme sırasında, `kubectl` etiketleri almak ve geçerli düğüm görüntü bilgilerini filtrelemek için aşağıdaki komutla düğüm görüntülerinin durumunu kontrol edin:

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

Yükseltme tamamlandığında, `az aks show` güncelleştirilmiş düğüm havuzu ayrıntılarını almak için kullanın. Geçerli düğüm görüntüsü `nodeImageVersion` özellikte gösterilmektedir.

```azurecli
az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster
```

## <a name="upgrade-a-specific-node-pool"></a>Belirli bir düğüm havuzunu yükseltme

Bir düğüm havuzundaki görüntünün yükseltilmesi, bir kümedeki görüntünün yükseltilme ile benzerdir.

Düğüm havuzunun işletim sistemi görüntüsünü bir Kubernetes kümesi yükseltmesi yapmadan güncelleştirmek için `--node-image-only` Aşağıdaki örnekteki seçeneğini kullanın:

```azurecli
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-image-only
```

Yükseltme sırasında, `kubectl` etiketleri almak ve geçerli düğüm görüntü bilgilerini filtrelemek için aşağıdaki komutla düğüm görüntülerinin durumunu kontrol edin:

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

Yükseltme tamamlandığında, `az aks nodepool show` güncelleştirilmiş düğüm havuzu ayrıntılarını almak için kullanın. Geçerli düğüm görüntüsü `nodeImageVersion` özellikte gösterilmektedir.

```azurecli
az aks nodepool show \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool
```

## <a name="upgrade-node-images-with-node-surge"></a>Düğüm dalgalanma ile düğüm görüntülerini yükseltme

Düğüm görüntüsü yükseltme işlemini hızlandırmak için, bir özelleştirilebilir düğüm dalgalanma değeri kullanarak düğüm görüntülerinizi yükseltebilirsiniz. Varsayılan olarak, AKS yükseltmeleri yapılandırmak için bir ek düğüm kullanır.

Yükseltme hızını artırmak isterseniz, `--max-surge` daha hızlı tamamlanabilmeleri için yükseltme için kullanılacak düğüm sayısını yapılandırmak için değerini kullanın. Çeşitli ayarların ticareti hakkında daha fazla bilgi edinmek için `--max-surge` bkz. [Özelleştirme düğümü aşırı yükseltmesi][max-surge].

Aşağıdaki komut, düğüm görüntüsü yükseltmesi gerçekleştirmek için en fazla dalgalanma değerini ayarlar:

```azurecli
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --max-surge 33% \
    --node-image-only \
    --no-wait
```

Yükseltme sırasında, `kubectl` etiketleri almak ve geçerli düğüm görüntü bilgilerini filtrelemek için aşağıdaki komutla düğüm görüntülerinin durumunu kontrol edin:

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

`az aks nodepool show`Güncelleştirilmiş düğüm havuzu ayrıntılarını almak için kullanın. Geçerli düğüm görüntüsü `nodeImageVersion` özellikte gösterilmektedir.

```azurecli
az aks nodepool show \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool
```

## <a name="next-steps"></a>Sonraki adımlar

- En son düğüm görüntüleri hakkında bilgi için bkz. [aks sürüm notları](https://github.com/Azure/AKS/releases) .
- [AKS kümesini yükseltme][upgrade-cluster]Ile Kubernetes sürümünü nasıl yükselteceğinizi öğrenin.
- [Azure Kubernetes Service (AKS) içindeki Linux düğümlerine güvenlik ve çekirdek güncelleştirmelerini uygulama][security-update]
- Birden çok düğüm havuzu ve [birden çok düğüm havuzu oluşturma ve yönetme][use-multiple-node-pools]ile düğüm havuzlarını yükseltme hakkında daha fazla bilgi edinin.

<!-- LINKS - internal -->
[upgrade-cluster]: upgrade-cluster.md
[security-update]: node-updates-kured.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[max-surge]: upgrade-cluster.md#customize-node-surge-upgrade-preview
