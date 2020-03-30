---
title: Kavramlar - Azure Kubernetes Hizmetlerinde (AKS) ölçeklendirme uygulamaları
description: Yatay bölme otomatik ölçekleyicisi, küme otomatik ölçeklendirici ve Azure Kapsayıcı Örnekleri bağlayıcısı dahil olmak üzere Azure Kubernetes Hizmeti'nde (AKS) ölçeklendirme hakkında bilgi edinin.
services: container-service
author: zr-msft
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: zarhoads
ms.openlocfilehash: 396e5bc31723768ada334dd5043bca724af5e84f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77595867"
---
# <a name="scaling-options-for-applications-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) uygulamaları için ölçeklendirme seçenekleri

Azure Kubernetes Hizmeti'nde (AKS) uygulamaları çalıştırdığınızda, bilgi işlem kaynaklarının miktarını artırmanız veya azaltmanız gerekebilir. Gerek duyan uygulama örneklerinin sayısı değiştikçe, altta yatan Kubernetes düğümlerinin sayısının da değişmesi gerekebilir. Ayrıca, çok sayıda ek uygulama örneğini hızla sağlamanız gerekebilir.

Bu makalede, AKS uygulamaları ölçeklendirmenize yardımcı olan temel kavramlar tanıtıcı:

- [El ile ölçeklendirin](#manually-scale-pods-or-nodes)
- [Yatay pod otomatik ölçeklendirici (HPA)](#horizontal-pod-autoscaler)
- [Küme otoölçekleyici](#cluster-autoscaler)
- [AKS ile Azure Kapsayıcı Örneği (ACI) entegrasyonu](#burst-to-azure-container-instances)

## <a name="manually-scale-pods-or-nodes"></a>Bölmeleri veya düğümleri el ile ölçeklendir

Uygulamanızın kullanılabilir kaynaklardaki ve durumdaki bir değişikliğe nasıl yanıt verebileceğini sınamak için yinelemeleri (bölmeleri) ve düğümleri el ile ölçeklendirebilirsiniz. Kaynakları el ile ölçekleme, düğüm sayısı gibi sabit bir maliyeti korumak için kullanılacak belirli bir kaynak miktarını da tanımlamanıza olanak tanır. El ile ölçeklendirmek için yineleme veya düğüm sayısını tanımlarsınız. Kubernetes API'si daha sonra bu yineleme veya düğüm sayısına göre ek bölmeler veya boşaltma düğümleri oluşturarak zamanlar.

Düğümleri ölçeklerken, Kubernetes API'si kümeniz tarafından kullanılan işlem türüne bağlı ilgili Azure İşlem API'sini çağırır. Örneğin, VM Ölçeği'nde oluşturulmuş kümeler için hangi düğümlerin kaldırılacak seçiliş mantığı, VM Ölçeği SETLERI API'si tarafından belirlenir. Düğümlerin ölçek küçültmede kaldırılması için nasıl seçildiği hakkında daha fazla bilgi edinmek için [VMSS SSS](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-faq#if-i-reduce-my-scale-set-capacity-from-20-to-15-which-vms-are-removed)bölümüne bakın.

El ile ölçekleme bölmeleri ve düğümleri ile başlamak için [AKS ölçek uygulamaları][aks-scale]bakın.

## <a name="horizontal-pod-autoscaler"></a>Yatay pod otoölçekleyici

Kubernetes, kaynak talebini izlemek ve çoğaltma sayısını otomatik olarak ölçeklendirmek için yatay pod otomatik ölçeklendiricisini (HPA) kullanır. Varsayılan olarak, yatay bölme otomatik ölçeklendirici çoğaltma sayısında gerekli değişiklikler için Her 30 saniyede Ölçümler API denetler. Değişiklik gerektiğinde, yineleme sayısı buna göre artar veya azalır. Yatay pod otomatik ölçeklendirici, Kubernetes 1.8+ için Metrik Sunucusu'nu dağıtan AKS kümeleriyle çalışır.

![Kubernetes yatay pod otomatikleme](media/concepts-scale/horizontal-pod-autoscaling.png)

Belirli bir dağıtım için yatay bölme otomatik ölçeklendiriyi yapılandırdığınızda, çalıştırılabilen en az ve en büyük yineleme sayısını tanımlarsınız. Cpu kullanımı gibi ölçekleme kararlarını izlemek ve temel almak için ölçümü de tanımlarsınız.

AKS'deki yatay pod otomatik ölçekleyiciile başlamak için [AKS'deki Otomatik Ölçeklendirme bölmelerine][aks-hpa]bakın.

### <a name="cooldown-of-scaling-events"></a>Ölçekleme olaylarının soğuması

Yatay bölme otomatik ölçeklendirici Ölçüm API'sini her 30 saniyede denetlerken, önceki ölçek olayları başka bir denetim yapmadan önce başarıyla tamamlanmamış olabilir. Bu davranış, önceki ölçek olayı uygulama iş yükünü ve kaynak taleplerini göre ayarlamak için alabilirsiniz önce yatay pod otomatik ölçeklendirici yinelemelerin sayısını değiştirmek için neden olabilir.

Bu yarış olaylarını en aza indirmek için, bekleme veya gecikme değerleri ayarlanır. Bu değerler, yatay bölme otomatik ölçeklendiricinin başka bir ölçek olayı tetiklenmeden önce bir ölçek olayından sonra ne kadar beklemesi gerektiğini tanımlar. Bu davranış, yeni yineleme sayısının etkili olmasını ve Ölçümler API'sinin dağıtılmış iş yükünü yansıtmasını sağlar. Varsayılan olarak, ölçeklendirme olaylarındaki gecikme 3 dakika, küçültülmüş olaylardaki gecikme 5 dakikadır

Şu anda, bu bekleme alanı değerlerini varsayılandan ayarlayamadığınız için.

## <a name="cluster-autoscaler"></a>Küme otoölçekleyici

Değişen bölme taleplerine yanıt vermek için Kubernetes'in düğüm havuzunda istenen işlem kaynaklarına göre düğüm sayısını ayarlayan bir küme otomatik ölçekleyicisi vardır. Varsayılan olarak, küme otomatik ölçeklendirici düğüm sayısında gerekli değişiklikler için her 10 saniyede bir MetrikLER API sunucusunu denetler. Küme otomatik ölçek bir değişiklik gerekli olduğunu belirlerse, AKS kümenizdeki düğüm sayısı buna göre artar veya azalır. Küme otomatik ölçeklendirici, Kubernetes 1.10.x veya daha yüksek çalıştıran RBAC etkin AKS kümeleri ile çalışır.

![Kubernetes küme otoölçekleyici](media/concepts-scale/cluster-autoscaler.png)

Küme otomatik ölçekleyici genellikle yatay pod otomatik ölçekleyici yanında kullanılır. Birleştirildiğinde, yatay pod otomatik ölçeklendirici uygulama talebine bağlı olarak bölme sayısını artırır veya azaltır ve küme otomatik ölçeklendirici bu ek bölmeleri çalıştırmak için gerektiğinde düğüm sayısını ayarlar.

AKS'deki küme otomatik ölçekleyicisi ile başlamak için [AKS'deki Cluster Autoscaler'a][aks-cluster-autoscaler]bakın.

### <a name="scale-up-events"></a>Olayları ölçeklendir

Bir düğüm, istenen bir bölmeyi çalıştırmak için yeterli bilgi işlem kaynağına sahip değilse, bu bölme zamanlama işlemi boyunca ilerleyemez. Düğüm havuzunda ek bilgi işlem kaynakları bulunmadıkça bölme başlatılamıyor.

Küme otomatik ölçeklendirici düğüm havuzu kaynak kısıtlamaları nedeniyle zamanlanamaz bölmeleri fark ettiğinde, düğüm havuzundaki düğüm sayısı ek bilgi işlem kaynaklarını sağlamak için artırılır. Bu ek düğümler başarıyla dağıtıldığında ve düğüm havuzuiçinde kullanılabilir olduğunda, bölmeler bunların üzerinde çalışacak şekilde zamanlanır.

Uygulamanızın hızla ölçeklendirmesi gerekiyorsa, küme otomatik ölçekleyicitarafından dağıtılan ek düğümler zamanlanan bölmeleri kabul edene kadar zamanlanacak durumda kalabilir. Yüksek seri talepleri olan uygulamalarda sanal düğümlerle ve Azure Kapsayıcı Örnekleriyle ölçeklendirebilirsiniz.

### <a name="scale-down-events"></a>Olayları küçültme

Küme otomatik ölçeklendirici, son zamanlarda yeni zamanlama istekleri almamış düğümler için bölme zamanlama durumunu da izler. Bu senaryo, düğüm havuzunun gerekenden daha fazla işlem kaynağına sahip olduğunu ve düğüm sayısının azaltılabilir olduğunu gösterir.

Varsayılan olarak 10 dakika boyunca artık gerekklenmeden bir eşikten geçen bir düğüm silinmesi için zamanlanır. Bu durum oluştuğunda, bölmeler düğüm havuzundaki diğer düğümlerde çalışacak şekilde zamanlanır ve küme otomatik ölçeklendirici düğüm sayısını azaltır.

Küme otomatik ölçekleyici düğüm sayısını azalttığında bölmeler farklı düğümlerde zamanlandığından, uygulamalarınız bazı kesintileryaşayabilir. Kesintiyi en aza indirmek için, tek bir bölme örneği kullanan uygulamalardan kaçının.

## <a name="burst-to-azure-container-instances"></a>Azure Kapsayıcı Örneklerine Patlama

AKS kümenizi hızla ölçeklendirmek için Azure Kapsayıcı Örnekleri (ACI) ile tümleştirebilirsiniz. Kubernetes çoğaltma ve düğüm sayısını ölçeklendirmek için yerleşik bileşenlere sahiptir. Ancak, uygulamanızın hızla ölçeklendirmesi gerekiyorsa, yatay bölme otomatik ölçeklendirici düğüm havuzundaki varolan bilgi işlem kaynakları tarafından sağlanabileceklerden daha fazla bölme zamanlayabilir. Yapılandırılırsa, bu senaryo düğüm havuzuna ek düğümler dağıtmak için küme otomatik ölçeklendiricisini tetikler, ancak bu düğümlerin başarıyla sağlaması ve Kubernetes zamanlayıcısının üzerlerinde bölmeçalıştırmasını sağlaması birkaç dakika sürebilir.

![Kubernetes ACI için ölçekleme patladı](media/concepts-scale/burst-scaling.png)

ACI, ek altyapı yükü olmadan kapsayıcı örneklerini hızla dağıtmanızı sağlar. AKS ile bağlandığınızda, ACI AKS kümenizin güvenli ve mantıksal bir uzantısı haline gelir. [Sanal Kubelet'i][virtual-kubelet]temel alan [sanal düğüm bileşeni,][virtual-nodes-cli] ACI'yi sanal Kubernetes düğümü olarak sunan AKS kümenize yüklenir. Kubernetes daha sonra doğrudan AKS kümenizde VM düğümleri üzerinde pod olarak değil, sanal düğümler üzerinden ACI örnekleri olarak çalışan bölmeleri zamanlayabilirsiniz. Sanal düğümler şu anda AKS önizleme bulunmaktadır.

Uygulamanız sanal düğümleri kullanmak için hiçbir değişiklik gerektirmez. Küme otomatik ölçeklendirici AKS kümenizde yeni düğümler dağıtırken dağıtımlar AKS ve ACI genelinde ölçeklendirilebilir ve gecikmeden.

Sanal düğümler AKS kümenizle aynı sanal ağdaki ek bir alt ağa dağıtılır. Bu sanal ağ yapılandırması, ACI ve AKS arasındaki trafiğin güvenli olmasını sağlar. AKS kümesi gibi, ACI örneği de diğer kullanıcılardan yalıtılmış güvenli ve mantıksal bir bilgi işlem kaynağıdır.

## <a name="next-steps"></a>Sonraki adımlar

Ölçekleme uygulamalarına başlamak için önce [Azure CLI ile bir AKS kümesi oluşturmak için hızlı başlatış'ı][aks-quickstart]izleyin. Daha sonra AKS kümenizdeki uygulamaları el ile veya otomatik olarak ölçeklendirmeye başlayabilirsiniz:

- [Bölmeleri][aks-manually-scale-pods] veya [düğümleri][aks-manually-scale-nodes] el ile ölçeklendir
- Yatay [pod otomatik ölçekleyicisini][aks-hpa] kullanma
- Küme [otomatik ölçekleyicisini][aks-cluster-autoscaler] kullanma

Çekirdek Kubernetes ve AKS kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes / AKS kümeleri ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes / AKS erişim ve kimlik][aks-concepts-identity]
- [Kubernetes / AKS güvenlik][aks-concepts-security]
- [Kubernetes / AKS sanal ağlar][aks-concepts-network]
- [Kubernetes / AKS depolama][aks-concepts-storage]

<!-- LINKS - external -->
[virtual-kubelet]: https://virtual-kubelet.io/

<!-- LINKS - internal -->
[aks-quickstart]: kubernetes-walkthrough.md
[aks-hpa]: tutorial-kubernetes-scale.md#autoscale-pods
[aks-scale]: tutorial-kubernetes-scale.md
[aks-manually-scale-pods]: tutorial-kubernetes-scale.md#manually-scale-pods
[aks-manually-scale-nodes]: tutorial-kubernetes-scale.md#manually-scale-aks-nodes
[aks-cluster-autoscaler]: autoscaler.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-network]: concepts-network.md
[virtual-nodes-cli]: virtual-nodes-cli.md