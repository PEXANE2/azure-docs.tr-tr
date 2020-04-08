---
title: Kaynak yönetimi en iyi uygulamaları
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmeti'nde (AKS) kaynak yönetimi için uygulama geliştiricisinin en iyi uygulamalarını öğrenin
services: container-service
author: zr-msft
ms.topic: conceptual
ms.date: 11/13/2019
ms.author: zarhoads
ms.openlocfilehash: 0052657c947f8a9ff9c9d6aef86ff16d9a22adae
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80803492"
---
# <a name="best-practices-for-application-developers-to-manage-resources-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti 'ndeki (AKS) kaynakları yönetmek için uygulama geliştiricileri için en iyi uygulamalar

Azure Kubernetes Hizmeti'nde (AKS) uygulama geliştirip çalıştırdığınızda, göz önünde bulundurulması gereken birkaç önemli alan vardır. Uygulama dağıtımlarınızı nasıl yönetebileceğiniz, sağladığınız hizmetlerin son kullanıcı deneyimini olumsuz etkileyebilir. Başarılı olmak için, AKS'de uygulamaları geliştirirken ve çalıştırırken takip edebileceğiniz en iyi uygulamaları aklınızda bulundurun.

Bu en iyi uygulamalar makalesi, kümenizi ve iş yüklerinizi uygulama geliştiricisi açısından nasıl çalıştırılacağını zayiata odaklanır. Yönetimen iyi uygulamalar hakkında bilgi için Azure [Kubernetes Hizmeti'nde (AKS) yalıtım ve kaynak yönetimi için Cluster işleci nin en iyi uygulamaları'na][operator-best-practices-isolation]bakın. Bu makalede, öğrenmek:

> [!div class="checklist"]
> * Pod kaynak istekleri ve sınırları nelerdir
> * Dev Spaces ve Visual Studio Code ile uygulama geliştirme nin ve dağıtmanın yolları
> * Dağıtımlarla ilgili `kube-advisor` sorunları denetlemek için aracı kullanma

## <a name="define-pod-resource-requests-and-limits"></a>Pod kaynak isteklerini ve sınırlarını tanımlama

**En iyi uygulama kılavuzu** - YAML bildirimlerinizdeki tüm bölmelerde pod isteklerini ve sınırlarını ayarlayın. AKS kümesi *kaynak kotaları kullanıyorsa,* bu değerleri tanımlamazsanız dağıtımınız reddedilebilir.

Aks kümesi içindeki bilgi işlem kaynaklarını yönetmenin birincil yolu, bölme isteklerini ve sınırlarını kullanmaktır. Bu istekler ve sınırlar, Kubernetes zamanlayıcısına bir bölmenin hangi bilgi işlem kaynaklarının atanması gerektiğini bilmesini sağlar.

* **Pod CPU/Bellek istekleri,** bölmenin düzenli olarak ihtiyaç duyduğu belirli bir cpu ve bellek miktarını tanımlar.
    * Kubernetes zamanlayıcısı bir düğüme bölme yerleştirmeye çalıştığında, bölme istekleri zamanlama için kullanılabilir sayıda kaynağa sahip olduğunu belirlemek için kullanılır.
    * Bir bölme isteği ayarlanmamak, isteği tanımlanan sınıra varsayılan olarak belirler.
    * Bu istekleri ayarlamak için uygulamanızın performansını izlemek çok önemlidir. Yetersiz isteklerde bulunulursa, başvurunuz bir düğümün zamanlaması nedeniyle bozulmuş performans alabilir. İstekler fazla tahmin ediliyorsa, uygulamanız zamanlanma da zorluk larını artırabilir.
* **Pod CPU/Bellek sınırları,** bir bölmenin kullanabileceği maksimum CPU ve bellek miktarıdır. Bu sınırlar, yetersiz kaynaklar nedeniyle düğüm kararsızlığı durumunda hangi bölmelerin öldürülmesi gerektiğini tanımlamaya yardımcı olur. Uygun sınırlar olmadan, kaynak baskısı kaldırılana kadar kapsüller öldürülecektir.
    * Pod sınırları, bir bölmenin kaynak tüketimi denetimini kaybettiğinde tanımlanmasına yardımcı olur. Bir sınır aşıldığında, düğüm durumunu korumak ve düğümü paylaşan bölmelere etkisini en aza indirmek için bölmeye öldürme için öncelik verilir.
    * Bir bölme sınırı ayarlanmaması varsayılan olarak belirli bir düğümdeki kullanılabilir en yüksek değere iner.
    * Düğümlerinizin destekedebileceğinden daha yüksek bir bölme sınırı belirlemeyin. Her AKS düğümü, çekirdek Kubernetes bileşenleri için belirli bir cpu ve bellek miktarı ayırır. Uygulamanız, diğer bölmelerin başarıyla çalıştırılaması için düğümüzerinde çok fazla kaynak tüketmeyi deneyebilir.
    * Yine, gün veya hafta boyunca farklı zamanlarda uygulamanızın performansını izlemek için çok önemlidir. En yüksek talebin ne zaman olduğunu belirleyin ve pod sınırlarını uygulamanın maksimum gereksinimlerini karşılamak için gereken kaynaklarla hizalayın.

Pod belirtimlerinizde, yukarıdaki bilgilere dayanarak bu istek ve sınırları tanımlamak **en iyi uygulamadır ve çok önemlidir.** Bu değerleri eklemezseniz, Kubernetes zamanlayıcısı, uygulamalarınızın zamanlama kararlarına yardımcı olmak için ihtiyaç duyduğu kaynakları dikkate alamaz.

Zamanlayıcı, yetersiz kaynaklara sahip bir düğümüzerine bir bölme yerleştirirse, uygulama performansı düşürülür. Küme yöneticilerinin kaynak istekleri ni ve sınırlarını ayarlamanızı gerektiren bir ad alanında *kaynak kotaları ayarlamaları* önerilir. Daha fazla bilgi için [AKS kümelerinde kaynak kotalarına][resource-quotas]bakın.

Bir CPU isteği veya sınırı tanımladığınızda, değer CPU birimleri cinsinden ölçülür. 
* *1.0* CPU düğümüzerinde bir altta yatan sanal CPU çekirdeğine eşittir. 
* Aynı ölçüm GPU'lar için kullanılır.
* Milicorecinsinden ölçülen kesirleri tanımlayabilirsiniz. Örneğin, *100m* altta yatan bir vCPU çekirdeğinin *0,1'idir.*

Tek bir NGINX pod için aşağıdaki temel örnekte, pod *100m* CPU süresi ve *128Mi* bellek ister. Bölme için kaynak sınırları *250m* CPU ve *256Mi* bellek olarak ayarlanır:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
```

Kaynak ölçümleri ve atamaları hakkında daha fazla bilgi için [kapsayıcılar için bilgi işlem kaynaklarını yönetme'ye][k8s-resource-limits]bakın.

## <a name="develop-and-debug-applications-against-an-aks-cluster"></a>Bir AKS kümesine karşı uygulamalar geliştirme ve hata ayıklama

**En iyi uygulama kılavuzu** - Geliştirme ekipleri Dev Spaces kullanarak bir AKS kümesine karşı dağıtmalı ve hata ayıklama yapmalıdır. Bu geliştirme modeli, uygulama üretime dağıtılmadan önce rol tabanlı erişim denetimlerinin, ağ veya depolama gereksinimlerinin uygulanmasını sağlar.

Azure Dev Spaces ile uygulamaları doğrudan bir AKS kümesine karşı geliştirir, hata ayıklar ve test ekarsınız. Ekip içindeki geliştiriciler, uygulama yaşam döngüsü boyunca oluşturmak ve test etmek için birlikte çalışır. Visual Studio veya Visual Studio Code gibi varolan araçları kullanmaya devam edebilirsiniz. Bir AKS kümesinde uygulamayı çalıştırma ve hata ayıklama seçeneği veren Dev Spaces için bir uzantı yüklenir:

![Dev Spaces ile bir AKS kümesindeki hata ayıklama uygulamaları](media/developer-best-practices-resource-management/dev-spaces-debug.png)

Dev Spaces ile bu entegre geliştirme ve test [süreci, minikube][minikube]gibi yerel test ortamları için ihtiyacı azaltır. Bunun yerine, bir AKS kümesi geliştirmek ve karşı test. Bu küme, bir kümeyi mantıksal olarak yalıtmak için ad alanlarının kullanımı yla ilgili önceki bölümde belirtildiği gibi güvenli ve yalıtılabilir. Uygulamalarınız üretime dağıtılanmaya hazır olduğunda, geliştirmeniz gerçek bir AKS kümesine karşı yapıldığı için güvenle dağıtabilirsiniz.

Azure Dev Spaces, Linux bölmeleri ve düğümleri üzerinde çalışan uygulamalarda kullanılmak üzere tasarlanmıştır.

## <a name="use-the-visual-studio-code-extension-for-kubernetes"></a>Kubernetes için Visual Studio Code uzantısını kullanma

**En iyi uygulama kılavuzu** - YAML bildirimlerini yazarken Kubernetes için VS Kodu uzantısını yükleyin ve kullanın. Uzantıyı, AKS kümesiyle seyrek etkileşimde bulunan uygulama sahiplerine yardımcı olabilecek tümleşik dağıtım çözümü için de kullanabilirsiniz.

[Kubernetes için Visual Studio Code uzantısı,][vscode-kubernetes] uygulamaları GELIŞTIRMEnize ve AKS'ye dağıtmanıza yardımcı olur. Uzantı, Kubernetes kaynakları ve Miğfer grafikleri ve şablonları için intellisense sağlar. Ayrıca VS Code'un içinden Kubernetes kaynaklarına göz atabilir, dağıtabilir ve edinebilirsiniz. Uzantı ayrıca, bakla belirtimlerinde ayarlanan kaynak istekleri veya sınırları için intellisense denetimi de sağlar:

![Eksik bellek limitleri hakkında Kubernetes uyarısı için VS Kodu uzantısı](media/developer-best-practices-resource-management/vs-code-kubernetes-extension.png)

## <a name="regularly-check-for-application-issues-with-kube-advisor"></a>Kube-advisor ile uygulama sorunlarını düzenli olarak kontrol edin

**En iyi uygulama kılavuzu** - Kümenizdeki sorunları algılamak için açık kaynak aracının `kube-advisor` en son sürümünü düzenli olarak çalıştırın. Varolan bir AKS kümesine kaynak kotaları uygularsanız, kaynak istekleri ve sınırları tanımlı olmayan bölmeleri bulmak için önce çalıştırın. `kube-advisor`

[Kube-danışman][kube-advisor] aracı, bir Kubernetes kümesini tarayan ve bulduğu sorunları raporlayan ilişkili bir AKS açık kaynak projesidir. Yararlı denetimlerden biri, kaynak istekleri ve sınırları olmayan bölmeleri tanımlamaktır.

Kube-advisor aracı kaynak isteği ve Windows uygulamaları için PodSpecs yanı sıra Linux uygulamaları eksik sınırları rapor edebilirsiniz, ancak kube-danışman aracı kendisi bir Linux pod üzerinde zamanlanmış olmalıdır. Pod yapılandırmasında bir [düğüm seçici][k8s-node-selector] kullanarak belirli bir işletim sistemi ile bir düğüm havuzunda çalışacak bir bölme zamanlayabilirsiniz.

Birçok geliştirme ekibi ve uygulamayı barındıran bir AKS kümesinde, bu kaynak istekleri ve sınırları ayarlı olmadan bölmeleri izlemek zor olabilir. En iyi uygulama olarak, `kube-advisor` AKS kümelerinizde düzenli olarak çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu en iyi uygulamalar makalesi, kümenizi ve iş yüklerinizi küme işleci perspektifinden nasıl çalıştırılacak üzerinde odaklanmıştır. Yönetimen iyi uygulamalar hakkında bilgi için Azure [Kubernetes Hizmeti'nde (AKS) yalıtım ve kaynak yönetimi için Cluster işleci nin en iyi uygulamaları'na][operator-best-practices-isolation]bakın.

Bu en iyi uygulamalardan bazılarını uygulamak için aşağıdaki makalelere bakın:

* [Dev Spaces ile geliştirme][dev-spaces]
* [Kube-advisor ile ilgili sorunları kontrol edin][aks-kubeadvisor]

<!-- EXTERNAL LINKS -->
[k8s-resource-limits]: https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/
[vscode-kubernetes]: https://github.com/Azure/vscode-kubernetes-tools
[kube-advisor]: https://github.com/Azure/kube-advisor
[minikube]: https://kubernetes.io/docs/setup/minikube/

<!-- INTERNAL LINKS -->
[aks-kubeadvisor]: kube-advisor-tool.md
[dev-spaces]: ../dev-spaces/get-started-netcore.md
[operator-best-practices-isolation]: operator-best-practices-cluster-isolation.md
[resource-quotas]: operator-best-practices-scheduler.md#enforce-resource-quotas
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors
