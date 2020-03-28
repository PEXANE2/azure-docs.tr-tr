---
title: (AmortismanA Uğradı) Azure Konteyner Hizmeti öğretici - Ölçek Uygulaması
description: Azure Container Service öğreticisi - Uygulamayı Ölçeklendirme
author: dlepow
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 2ea8a5428c1fabdfda4f2298c0559792537df481
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78273997"
---
# <a name="deprecated-scale-kubernetes-pods-and-kubernetes-infrastructure"></a>(AmortismanA Uğradı) Ölçek Kubernetes pods ve Kubernetes altyapı

> [!TIP]
> Azure Kubernetes Hizmetini kullanan bu öğreticinin güncelleştirilmiş sürümü için [Bkz.](../../aks/tutorial-kubernetes-scale.md)

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Öğreticileri takip ediyorsanız, Azure Container Service’te çalışan bir Kubernetes kümesine sahipsinizdir ve Azure Voting uygulamasını dağıtmışsınızdır. 

Yedi öğreticinin beşinci parçası olan bu öğreticide, uygulamada pod’ları ölçeklendirirsiniz ve otomatik pod ölçeklendirmeyi denersiniz. Ayrıca, barındırılan iş yükleri için kümenin kapasitesini değiştirmek üzere Azure VM aracı düğümlerinin sayısını ölçeklendirmeyi de öğrenirsiniz. Tamamlanan görevler şunları içerir:

> [!div class="checklist"]
> * Kubernetes pod’larını el ile ölçeklendirme
> * Uygulama ön ucunda çalışan Otomatik pod ölçeklendirmeyi yapılandırma
> * Kubernetes Azure aracı düğümlerini ölçeklendirme

Sonraki öğreticilerde Azure Vote uygulaması güncelleştirilir ve Log Analytics, Kubernetes kümesini izlemek için yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, bu görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu. Ardından uygulama Kubernetes kümesinde çalıştırıldı. 

Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma](./container-service-tutorial-kubernetes-prepare-app.md) konusuna dönün. 

## <a name="manually-scale-pods"></a>Pod’ları el ile ölçeklendirme

Şu ana kadar hem Azure Vote ön ucu hem de Redis örneği tek bir çoğaltma dağıtıldı. Doğrulamak için [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) komutunu çalıştırın.

Tarayıcınızda [https://shell.azure.com](https://shell.azure.com) Cloud Shell'i açmak için gidin.

```console
kubectl get pods
```

Çıktı:

```output
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

[kubectl scale](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale) komutunu kullanarak `azure-vote-front` dağıtımında pod’ların sayısını el ile değiştirin. Bu örnek, sayısı 5’e yükseltir.

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

[kubectl get pods](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) komutunu çalıştırarak Kubernetes’in pod’ları oluşturduğunu doğrulayın. Yaklaşık bir dakika sonra ek pod’lar çalışır:

```console
kubectl get pods
```

Çıktı:

```output
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Pod’ları otomatik ölçeklendirme

Kubernetes, bir dağıtımdaki pod’ların sayısını CPU kullanımı ve diğer seçili ölçümleri temel alarak ayarlamak için [yatay pod otomatik ölçeklendirmeyi](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) destekler. 

Otomatik ölçeklendiriciyi kullanmak için pod’larınızın CPU istekleri olmalı ve sınırları tanımlanmış olmalıdır. `azure-vote-front` dağıtımında, ön uç kapsayıcısı 0,5 sınırlı, 0,25 CPU kullanımı ister. Ayarları şöyle görünür:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Aşağıdaki örnek,  dağıtımındaki pod’ların sayısını otomatik olarak ölçeklendirmek için [kubectl autoscale`azure-vote-front`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale) komutunu kullanır. Burada CPU kullanımı %50’yi aşarsa, otomatik ölçeklendirici, pod’ların sayısını üst sınır olan 10’a yükseltir.


```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Otomatik ölçeklendiricinin durumunu görmek için aşağıdaki komutu çalıştırın:

```console
kubectl get hpa
```

Çıktı:

```output
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Birkaç dakika sonra Azure Vote uygulamasında en az yük ile, pod çoğaltmalarının sayısı otomatik olarak 3’e azalır.

## <a name="scale-the-agents"></a>Aracıları ölçeklendirme

Önceki öğreticide Kubernetes kümenizi varsayılan komutları kullanarak oluşturduysanız, burada üç aracı düğümü bulunur. Kümenizde daha fazla veya daha az kapsayıcı iş yükü planlıyorsanız, aracı sayısını el ile ayarlayabilirsiniz. [az acs scale](/cli/azure/acs#az-acs-scale) komutunu kullanın ve `--new-agent-count` parametresi ile aracı sayısını belirtin.

Aşağıdaki örnek, *myK8sCluster* adlı Kubernetes kümesinde aracı düğümlerinin sayısını 4’e yükseltir. Komutun tamamlanması birkaç dakika sürer.

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

Komut çıktısı, aracı düğüm sayısını `agentPoolProfiles:count` değerinde gösterir:

```output
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Kubernetes kümenizde farklı ölçeklendirme özellikleri kullandınız. Dahil edilen görevler:

> [!div class="checklist"]
> * Kubernetes pod’larını el ile ölçeklendirme
> * Uygulama ön ucunda çalışan Otomatik pod ölçeklendirmeyi yapılandırma
> * Kubernetes Azure aracı düğümlerini ölçeklendirme

Kubernetes’te uygulama güncelleştirme hakkında daha fazla bilgi için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes'te uygulama güncelleştirme](./container-service-tutorial-kubernetes-app-update.md)

