---
title: En iyi uygulamalar için dağıtımları denetleme
titleSuffix: Azure Kubernetes Service
description: Kube-advisor'ı kullanarak Azure Kubernetes Hizmeti'ndeki dağıtımlarınızda en iyi uygulamaların uygulanıp uygulanabildiğini nasıl denetleriz öğrenin
services: container-service
author: seanmck
ms.topic: troubleshooting
ms.date: 11/05/2018
ms.author: seanmck
ms.openlocfilehash: 17e21c142dc354de7b72bc17396b19366027c5cd
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668389"
---
# <a name="checking-for-kubernetes-best-practices-in-your-cluster"></a>Kümenizdeki Kubernetes en iyi yöntemlerini denetleme

Uygulamalarınız için en iyi performansı ve esnekliği sağlamak için Kubernetes dağıtımlarınızda izlemeniz gereken en iyi uygulamalar vardır. Kube-advisor aracını, bu önerileri takip etmeyen dağıtımları aramak için kullanabilirsiniz.

## <a name="about-kube-advisor"></a>Kube-danışman hakkında

[Kube-advisor aracı,][kube-advisor-github] kümenizde çalıştırılmak üzere tasarlanmış tek bir kapsayıcıdır. Kubernetes API sunucusunu dağıtımlarınız hakkında bilgi için sorgular ve önerilen bir dizi geliştirmeyi döndürür.

Kube-advisor aracı kaynak isteği ve Windows uygulamaları için PodSpecs yanı sıra Linux uygulamaları eksik sınırları rapor edebilirsiniz, ancak kube-danışman aracı kendisi bir Linux pod üzerinde zamanlanmış olmalıdır. Pod yapılandırmasında bir [düğüm seçici][k8s-node-selector] kullanarak belirli bir işletim sistemi ile bir düğüm havuzunda çalışacak bir bölme zamanlayabilirsiniz.

> [!NOTE]
> Kube-advisor aracı, Microsoft tarafından en iyi çabayla desteklenir. Sorunlar ve öneriler GitHub üzerinde dosyalanmalıdır.

## <a name="running-kube-advisor"></a>Koşu kube-danışman

Aşağıdaki komutları kullanarak [rol tabanlı erişim denetimi (RBAC)](azure-ad-integration.md)için yapılandırılan bir küme üzerinde aracı çalıştırmak için. İlk komut bir Kubernetes hizmet hesabı oluşturur. İkinci komut, aracı bu hizmet hesabını kullanarak bir bölmede çalıştırır ve çıktıktan sonra bölmeyi silinmesi için yapılandırır. 

```bash
kubectl apply -f https://raw.githubusercontent.com/Azure/kube-advisor/master/sa.yaml

kubectl run --rm -i -t kubeadvisor --image=mcr.microsoft.com/aks/kubeadvisor --restart=Never --overrides="{ \"apiVersion\": \"v1\", \"spec\": { \"serviceAccountName\": \"kube-advisor\" } }"
```

RBAC kullanmıyorsanız, komutu aşağıdaki gibi çalıştırabilirsiniz:

```bash
kubectl run --rm -i -t kubeadvisor --image=mcr.microsoft.com/aks/kubeadvisor --restart=Never
```

Birkaç saniye içinde, dağıtımlarınızda olası iyileştirmeleri açıklayan bir tablo görmeniz gerekir.

![Kube-danışman çıktısı](media/kube-advisor-tool/kube-advisor-output.png)

## <a name="checks-performed"></a>Gerçekleştirilen denetimler

Araç, her biri kendi önerilen düzeltmesi ile birkaç Kubernetes en iyi uygulamaları doğrular.

### <a name="resource-requests-and-limits"></a>Kaynak istekleri ve sınırlar

Kubernetes, [pod belirtimlerinde kaynak isteklerini ve sınırlarını tanımlamayı][kube-cpumem]destekler. İstek, kapsayıcıyı çalıştırmak için gereken minimum CPU ve belleği tanımlar. Sınır, izin verilmesi gereken maksimum CPU ve belleği tanımlar.

Varsayılan olarak, pod belirtimlerinde hiçbir istek veya sınır ayarlanır. Bu düğümlerin aşırı planlanmış ve konteynerler açlıktan yol açabilir. Kube-advisor aracı, istek ve limitler belirlenmeden bölmeleri vurgular.

## <a name="cleaning-up"></a>Temizleme

Kümeniz RBAC etkinleştirilmişse, aşağıdaki `ClusterRoleBinding` komutu kullanarak aracı çalıştırdıktan sonra temizleyebilirsiniz:

```bash
kubectl delete -f https://raw.githubusercontent.com/Azure/kube-advisor/master/sa.yaml
```

Aracı RBAC etkin olmayan bir kümeye karşı çalıştırıyorsanız, temizleme gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Kubernetes Hizmeti ile ilgili sorun giderme sorunları](troubleshooting.md)

<!-- RESOURCES -->

[kube-cpumem]: https://github.com/Azure/azure-quickstart-templates
[kube-advisor-github]: https://github.com/azure/kube-advisor
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors