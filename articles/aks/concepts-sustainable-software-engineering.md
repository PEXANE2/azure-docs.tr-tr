---
title: Kavramlar-Azure Kubernetes hizmetlerinde (AKS) sürdürülebilirlik Yazılım Mühendisliği
description: Azure Kubernetes Service (AKS) ' de sürdürülebilirlik yazılım mühendisliği hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 08/26/2020
ms.openlocfilehash: 2457de7bdaa94a6e2269515fafe6689d44960625
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90984977"
---
# <a name="sustainable-software-engineering-principles-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içinde sürdürülebilir Yazılım Mühendisliği ilkeleri

Sürdürülebilir Yazılım Mühendisliği ilkeleri, sürdürülebilir uygulamalar tanımlamanıza, oluşturmanıza ve çalıştırmanıza yardımcı olan bir uzmanlık kümesidir. Genel amaç, uygulamanızın her yönüyle karbon izi azaltmaktır. [İlkeler. yeşil proje][principles-green] , sürdürülebilirlik yazılım mühendisliğinin ilkelerine genel bir bakış içerir.

Yazılım mühendisliğinin, önceliklerin ve odaklanmanın bir vardiyası olduğunu anlamak için önemli bir fikir. Çoğu durumda, yazılım, hızlı performans ve düşük gecikme süresine odaklanan bir biçimde tasarlanır ve çalışır. Sürdürülebilir Yazılım Mühendisliği, mümkün olduğunca fazla bilgi azaltmada odaklanır. Bazı durumlarda, sürdürülebilir Yazılım Mühendisliği ilkelerini uygulamak, toplam ağ yolculuğu gibi daha hızlı performans veya daha düşük gecikme süresi sağlayabilir. Diğer durumlarda, bilgi azaltma düşük öncelikli iş yüklerini erteleyerek daha yavaş performans veya daha fazla gecikme süresine neden olabilir. Uygulamanıza sürdürülebilir Yazılım Mühendisliği ilkeleri uygulamayı düşünmeden önce, uygulamanızın önceliklerini, ihtiyaçlarını ve dengeliğini gözden geçirin.

## <a name="measure-and-optimize"></a>Ölçme ve iyileştirme

AKS kümelerinizin boyutunu düşürmek için, kümenizin kaynaklarının nasıl kullanıldığını anlamanız gerekir. [Azure izleyici][azure-monitor] , kümenizin bellek ve CPU kullanımı gibi kaynak kullanımı hakkında ayrıntılı bilgi sağlar. Bu veriler, kümenizin karbon boyutunu azaltmaya ve değişikliklerinizin etkisini gözlemlemeye yardımcı olabilir. Ayrıca, tüm Azure kaynaklarınızın boyutunu görmek için [Microsoft Suslebilirliği Hesaplayıcı][sustainability-calculator] ' yı da yükleyebilirsiniz.

## <a name="increase-resource-utilization"></a>Kaynak kullanımını artırma

Karbon parmak izini düşürmeye yönelik bir yaklaşım, işlem kaynaklarınız için boşta kalma süresi miktarını azaltmaktır. Boşta kalma süresini azaltmak, işlem kaynaklarınızın kullanımını artırmayı içerir. Örneğin, kümenizde dört düğüm varsa, her biri %50 kapasitesinde çalışıyorsa, düğümlerinizin dört bölümünde %50 kullanılmayan kapasite kalmadı. Kümenizi üç düğüme düşürüyor, aynı iş yükü üç Düğümünüzün %67 kapasitesinde çalışmasına neden olur; bu da, kullanılmayan kapasitenizi her bir düğümdeki %33 ' e düşürüyor ve kullanımınızı artırıyor.

> [!IMPORTANT]
> Kümenizdeki kaynaklarda değişiklik yapmayı düşünürken, [sistem havuzlarınızın][system-pools] kümenizin çekirdek sistem bileşenlerinin kararlılığını sürdürmek için yeterli kaynaklara sahip olduğunu doğrulayın. Kümenizin kaynaklarını kümenizin kararsız hale gelebileceği noktaya göre hiçbir şekilde azaltmayın.

Kümenizin kullanımını inceledikten sonra, [birden çok düğüm havuzu][multiple-node-pools]tarafından sunulan özellikleri kullanmayı göz önünde bulundurun. Düğüm [boyutlandırmayı][node-sizing] , belirli CPU ve bellek profilleriyle düğüm havuzları tanımlamak için kullanabilirsiniz. böylece düğümlerinizi iş yükü gereksinimlerinize uyarlayabilirsiniz. Düğümlerinizin iş yükünüzün gereksinimlerine göre Boyutlandırılması, daha yüksek bir kullanım sırasında birkaç düğüm çalıştırmanıza yardımcı olabilir. Ayrıca, kümenize göre kümenizi otomatik olarak ölçeklendirmek için kümenizi ölçeklendirerek [yatay Pod otomatik Scaler][scale-horizontal] 'ı ve [küme otomatik Scaler][scale-auto] 'sini nasıl [ölçeklendirebileceğiniz][scale] bir şekilde yapılandırabilirsiniz. Kümenizin ölçeklendirme şeklini denetlemek, kümenizin iş yükündeki değişikliklerle çalışırken tüm düğümlerinizi yüksek bir kullanım sırasında korumanıza yardımcı olabilir. Bir iş yükünün ani kesintiler veya sonlandırmalar için dayanıklı olduğu durumlarda, Azure 'daki boş kapasiteden yararlanmak için [spot havuzlar][spot-pools] kullanabilirsiniz. Örneğin, spot havuzlar toplu işler veya geliştirme ortamları için iyi çalışabilir.

Kullanım artırma, [her düğümdeki kaynak ayırmaları][resource-reservations]tarafından tüketilen enerji miktarını azaltan fazla düğümleri de azaltabilir.

Ayrıca, uygulamalarınızın Kubernetes bildirimlerinde CPU ve bellek *isteklerini* ve *sınırlarını* gözden geçirin. Bellek ve CPU için bu değerleri düşürdükçe, diğer iş yüklerini çalıştırmak için kümede daha fazla bellek ve CPU vardır. Daha düşük CPU ve bellekle daha fazla iş yükü çalıştırırken, kümeniz daha seyrek bir şekilde ayrılmış hale gelir ve bu da kullanımınızı artırır. Uygulamalarınız için CPU ve bellek azaltıldığında, bu değerleri çok düşük olarak ayarlarsanız uygulamalarınızın davranışı azaltılabilir veya kararsız hale gelebilir. CPU ve bellek *isteklerini* ve *sınırlarını*değiştirmeden önce, bu değerlerin uygun şekilde ayarlanmış olup olmadığını anlamak için bazı benchsýnamalar çalıştırmayı göz önünde bulundurun. Ayrıca, uygulamanız kararsız hale geldiğinde bu değerleri hiçbir zaman noktaya azaltmayın.

## <a name="reduce-network-travel"></a>Ağ yolculuğu azaltma

Kümeinizden gelen ve olmayan mesafe isteklerinin ve yanıtlarının azaltılması, genellikle ağ cihazlarına karşı elektrik tüketimini azaltır ve karbon emislerini azaltır. Ağ trafiğinizi inceledikten sonra, ağ trafiğinizin kaynağına yakın [bölgelerde][regions] kümeler oluşturmayı göz önünde bulundurun. Azure Traffic Manager, Azure kaynakları arasındaki mesafeyi azaltmaya yardımcı olmak üzere en yakın kümeye ve [yakınlık yerleştirme gruplarına][proiximity-placement-groups] yönlendirme konusunda yardımcı olmak için [Azure][azure-traffic-manager] de kullanabilirsiniz.

> [!IMPORTANT]
> Kümenizin ağ üzerinde değişiklik yapmayı düşünürken, ağ yolculuğu 'nı toplantı iş yükü gereksinimlerinin maliyetiyle hiçbir zaman azaltmayın. Örneğin, [kullanılabilirlik bölgelerini][availability-zones] kullanmak kümenizde daha fazla ağ yolculuğu sağlar ancak bu özelliğin kullanılması iş yükü gereksinimlerini işlemek için gerekli olabilir.

## <a name="demand-shaping"></a>Talep şekillendirme

Mümkün olduğunda, kümenizin kaynaklarına yönelik talebi, fazla kapasiteyi kullanabileceğiniz saatlere veya bölgelere değiştirmeyi göz önünde bulundurun. Örneğin, bir Batch işinin çalıştırılacağı saati veya bölgeyi değiştirmek veya [spot havuzları][spot-pools]kullanmak için göz önünde bulundurun. Ayrıca, anında işleme gerektirmeyen çalışan iş yüklerini erteetmek için uygulamanızı bir kuyruk kullanacak şekilde yeniden düzenlemeyi düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bahsedilen AKS özellikleri hakkında daha fazla bilgi edinin:

* [Birden çok düğüm havuzu][multiple-node-pools]
* [Düğüm boyutlandırma][node-sizing]
* [Küme ölçekleme][scale]
* [Yatay pod otomatik ölçeklendiricisi][scale-horizontal]
* [Küme otomatik ölçeklendiricisi][scale-auto]
* [Spot havuzlar][spot-pools]
* [Sistem havuzları][system-pools]
* [Kaynak ayırmaları][resource-reservations]
* [Yakınlık yerleştirme grupları][proiximity-placement-groups]
* [Kullanılabilirlik Alanları][availability-zones]

[availability-zones]: availability-zones.md
[azure-monitor]: ../azure-monitor/insights/container-insights-overview.md
[azure-traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[proiximity-placement-groups]: reduce-latency-ppg.md
[regions]: faq.md#which-azure-regions-currently-provide-aks
[resource-reservations]: concepts-clusters-workloads.md#resource-reservations
[scale]: concepts-scale.md
[scale-auto]: concepts-scale.md#cluster-autoscaler
[scale-horizontal]: concepts-scale.md#horizontal-pod-autoscaler
[spot-pools]: spot-node-pool.md
[multiple-node-pools]: use-multiple-node-pools.md
[node-sizing]: use-multiple-node-pools.md#specify-a-vm-size-for-a-node-pool
[sustainability-calculator]: https://azure.microsoft.com/blog/microsoft-sustainability-calculator-helps-enterprises-analyze-the-carbon-emissions-of-their-it-infrastructure/
[system-pools]: use-system-pools.md
[principles-green]: https://principles.green/