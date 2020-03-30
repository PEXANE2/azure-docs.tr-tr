---
title: Azure Hizmet Kumaş küme ölçekleme
description: Azure Hizmet Kumaşı kümelerini ölçekleme ve yukarı veya aşağı öğrenme hakkında bilgi edinin. Uygulama talepleri değiştikçe, Service Fabric kümeleri de değişebilir.
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: atsenthi
ms.openlocfilehash: 9dd60a5898b648215fc8b26e49a706a7b19dfeeb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258700"
---
# <a name="scaling-azure-service-fabric-clusters"></a>Azure Hizmet Kumaş kümelerini ölçekleme
Service Fabric kümesi, mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir. Kümenin bir parçası olan bir makine ye veya VM'ye düğüm denir. Kümeler potansiyel olarak binlerce düğüm içerebilir. Hizmet Kumaşı kümesi oluşturduktan sonra, kümeyi yatay olarak ölçeklendirebilir (düğüm sayısını değiştirebilir) veya dikey olarak (düğümlerin kaynaklarını değiştirebilirsiniz).  Kümede iş yükleri çalışıyor olsa bile, kümeyi istediğiniz zaman ölçeklendirebilirsiniz.  Küme ölçeklendikçe, uygulamalarınız da otomatik olarak ölçeklenir.

Kümeyi neden ölçeklendirin? Uygulama talepleri zaman içinde değişir.  Artan uygulama iş yükünü veya ağ trafiğini karşılamak veya talep düştüğünde küme kaynaklarını azaltmak için küme kaynaklarını artırmanız gerekebilir.

## <a name="scaling-in-and-out-or-horizontal-scaling"></a>Girip çıkma veya yatay ölçekleme
Kümedeki düğüm sayısını değiştirir.  Yeni düğümler kümeye katıldıktan sonra, [Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md) hizmetleri onlara taşır ve bu da varolan düğümlerin yükünü azaltır.  Kümenin kaynakları verimli bir şekilde kullanılıyorsa düğüm sayısını da azaltabilirsiniz.  Düğümler kümeden çıktıkça, hizmetler bu düğümlerden hareket eder ve kalan düğümlerde yük artar.  Azure'da çalışan bir kümedeki düğüm sayısını azaltmak, kullandığınız VM'lerin sayısını ödediğiniz için, bu VM'lerin iş yükünü değil, paradan tasarruf etmenizi sağlayabilir.  

- Avantajları: Sonsuz ölçek, teoride.  Uygulamanız ölçeklenebilirlik için tasarlandıysa, daha fazla düğüm ekleyerek sınırsız büyüme sağlayabilirsiniz.  Bulut ortamlarında araç kullanımı düğüm eklemeyi veya kaldırmayı kolaylaştırır, bu nedenle kapasiteyi ayarlamak kolaydır ve yalnızca kullandığınız kaynaklar için ödeme yaparsınız.  
- Dezavantajları: Uygulamalar [ölçeklenebilirlik için tasarlanmalıdır.](service-fabric-concepts-scalability.md)  Uygulama veritabanları ve kalıcılık da ölçeklendirmek için ek mimari çalışma gerektirebilir.  Ancak Service Fabric stateful services'deki [güvenilir koleksiyonlar,](service-fabric-reliable-services-reliable-collections.md) uygulama verilerinizi ölçeklendirmenizi çok daha kolay hale getirir.

Sanal makine ölçek kümeleri, sanal makine koleksiyonunu bir küme olarak dağıtmak ve yönetmek için kullanabileceğiniz bir Azure bilgi işlem kaynağıdır. Azure kümesinde tanımlanan her düğüm türü [ayrı bir ölçek kümesi olarak ayarlanır.](service-fabric-cluster-nodetypes.md) Her düğüm türü daha sonra bağımsız olarak ölçeklenebilir veya dışarı çıkarılabilir, farklı bağlantı noktaları kümeleri açık olabilir ve farklı kapasite ölçütlerine sahip olabilir. 

Bir Azure kümesini ölçeklerken aşağıdaki yönergeleri göz önünde bulundurun:
- üretim iş yüklerini çalıştıran birincil düğüm türlerinde her zaman beş veya daha fazla düğüm olmalıdır.
- durumlu üretim iş yüklerini çalıştıran birincil olmayan düğüm türleri her zaman beş veya daha fazla düğüm olmalıdır.
- durumsuz üretim iş yüklerini çalıştıran birincil olmayan düğüm türleri her zaman iki veya daha fazla düğüm olmalıdır.
- Altın veya Gümüş [dayanıklılık düzeyi](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) herhangi bir düğüm türü her zaman beş veya daha fazla düğüm olmalıdır.
- Rastgele VM örneklerini/düğümlerini düğüm türünden çıkarmayın, her zaman sanal makine ölçeği küme küçültme özelliğini kullanın. Rasgele VM örneklerinin silinmesi, sistemlerin dengeyi düzgün bir şekilde yükleme yeteneğini olumsuz etkileyebilir.
- Otomatik ölçeklendirme kuralları kullanıyorsanız, kuralları, aynı anda tek düğüm le ölçeklendirme (VM örneklerini kaldırma) olacak şekilde ayarlayın. Aynı anda birden fazla örneği ölçekleme güvenli değildir.

Kümenizdeki Servis Kumaş düğüm türleri arka uçtaki sanal makine ölçeği kümelerinden oluşur, otomatik ölçek kuralları ayarlayabilir veya her düğüm türü/sanal makine ölçeği kümesini [el ile ölçeklendirebilirsiniz.](service-fabric-cluster-scale-up-down.md)

### <a name="programmatic-scaling"></a>Programlı ölçekleme
Birçok senaryoda, [bir kümeyi el ile veya otomatik ölçeklendirme kurallarıyla ölçeklendirme](service-fabric-cluster-scale-up-down.md) iyi çözümlerdir. Daha gelişmiş senaryolar için, ancak, onlar doğru uygun olmayabilir. Bu yaklaşımların olası dezavantajları şunlardır:

- El ile ölçekleme, oturum açmanızı ve açıkça ölçekleme işlemleri istemenizi gerektirir. Ölçekleme işlemleri sık sık veya öngörülemeyen zamanlarda gerekliyse, bu yaklaşım iyi bir çözüm olmayabilir.
- Otomatik ölçek kuralları bir örneği sanal makine ölçeği kümesinden kaldırdığında, düğüm türü Gümüş veya Altın dayanıklılık düzeyine sahip olmadığı sürece, bu düğümün bilgilerini ilişkili Service Fabric kümesinden otomatik olarak kaldırmaz. Otomatik ölçek kuralları ölçek ayarlanan düzeyde (Hizmet Kumaşı düzeyinde değil) çalıştığı için, otomatik ölçek kuralları Servis Kumaş düğümlerini düzgün bir şekilde kapatmadan kaldırabilir. Bu kaba düğüm kaldırma ölçek-in işlemlerinden sonra geride 'hayalet' Hizmet Kumaş düğüm durumu bırakacaktır. Bir bireyin (veya bir hizmetin) Hizmet Kumaşı kümesindeki kaldırılan düğüm durumunu düzenli olarak temizlemesi gerekir.
- Dayanıklılık düzeyi Altın veya Gümüş olan bir düğüm türü, kaldırılan düğümleri otomatik olarak temizler, bu nedenle ek temizleme gerekmez.
- Otomatik ölçek kurallarıyla desteklenen [birçok ölçüm](../azure-monitor/platform/autoscale-common-metrics.md) olmasına rağmen, yine de sınırlı bir kümedir. Senaryonuz, bu kümede kapsanmayan bazı ölçümlere göre ölçeklendirme gerektiriyorsa, otomatik ölçeklendirme kuralları iyi bir seçenek olmayabilir.

Service Fabric ölçekleme yaklaşımınız senaryonuza bağlıdır. Ölçekleme nadir sayılsa, düğümleri el ile ekleme veya kaldırma yeteneği büyük olasılıkla yeterlidir. Daha karmaşık senaryolar için, otomatik ölçeklendirme kuralları ve SDK'lar programsal olarak güçlü alternatifler sunma yeteneğini ortaya çıkarır.

Uygulamaların sanal makine ölçek kümeleri ve Service Fabric kümeleriyle programlanabilir şekilde çalışmasına olanak tanıyan Azure API'leri vardır. Varolan otomatik ölçek seçenekleri senaryonuz için işe yaramazsa, bu API'ler özel ölçeklendirme mantığı uygulanmasını mümkün kılar. 

Bu 'ev yapımı' otomatik ölçekleme işlevini uygulamak için bir yaklaşım ölçekleme işlemleri yönetmek için Hizmet Kumaş uygulamasına yeni bir devletsiz hizmet eklemektir. Kendi ölçekleme hizmetinizi oluşturmak, uygulamanızın ölçekleme davranışı üzerinde en yüksek düzeyde denetim ve özelleştirilebilirlik sağlar. Bu, bir uygulamanın ne zaman veya nasıl ölçeklendirildiğinde veya nasıl ölçeklendirildiğinde hassas denetim gerektiren senaryolar için yararlı olabilir. Ancak, bu denetim kod karmaşıklığı bir tradeoff ile birlikte gelir. Bu yaklaşımı kullanmak, önemsiz olmayan ölçekleme koduna sahip olmanız gerektiği anlamına gelir. Hizmetin `RunAsync` yöntemi içinde, ölçekleme nin gerekli olup olmadığını (maksimum küme boyutu ve ölçekleme bekleme sürelerini denetleme dahil) bir dizi tetikleyici belirleyebilir.   

Sanal makine ölçeği ayar etkileşimleri için kullanılan API (hem sanal makine örneklerinin geçerli sayısını kontrol etmek hem de değiştirmek için) [akıcı Azure Yönetim Bilgi İşlem kitaplığıdır.](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/) Akıcı bilgi işlem kitaplığı, sanal makine ölçek kümeleriyle etkileşim kurmak için kullanımı kolay bir API sağlar.  Service Fabric kümesinin kendisi ile etkileşim kurmak için [System.Fabric.FabricClient'ı](/dotnet/api/system.fabric.fabricclient)kullanın.

Ancak ölçeklendirme kodunun ölçeklendirilecek kümede bir hizmet olarak çalışmasına gerek yoktur. Her `IAzure` `FabricClient` ikisi de ilişkili Azure kaynaklarına uzaktan bağlanabildiği için ölçekleme hizmeti, Service Fabric uygulaması dışından çalışan bir konsol uygulaması veya Windows hizmeti olabilir.

Bu sınırlamalara bağlı olarak, [daha özelleştirilmiş otomatik ölçekleme modelleri uygulamak](service-fabric-cluster-programmatic-scaling.md)isteyebilirsiniz.

## <a name="scaling-up-and-down-or-vertical-scaling"></a>Yukarı ve aşağı ölçekleme veya dikey ölçekleme 
Kümedeki düğümlerin kaynaklarını (CPU, bellek veya depolama) değiştirir.
- Avantajları: Yazılım ve uygulama mimarisi aynı kalır.
- Dezavantajları: Sonlu ölçek, tek tek düğümlerde kaynakları ne kadar artırabileceğinizin bir sınırı olduğundan. Kaynak eklemek veya kaldırmak için fiziksel veya sanal makineleri çevrimdışı almanız gerekeceği için kapalı kalma süresi.

Sanal makine ölçek kümeleri, sanal makine koleksiyonunu bir küme olarak dağıtmak ve yönetmek için kullanabileceğiniz bir Azure bilgi işlem kaynağıdır. Azure kümesinde tanımlanan her düğüm türü [ayrı bir ölçek kümesi olarak ayarlanır.](service-fabric-cluster-nodetypes.md) Her düğüm türü daha sonra ayrı ayrı yönetilebilir.  Düğüm türünü yukarı veya aşağı ölçeklendirmek, ölçek kümesindeki sanal makine örneklerinin SKU'sunun değiştirilmesini içerir. 

> [!WARNING]
> [Silver dayanıklılıkta veya daha büyük](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster)bir ölçekte çalışmadığı sürece ölçek kümesi/düğümü türünün VM SKU'yu değiştirmemenizi öneririz. VM SKU Boyutunu değiştirmek, yerinde veri yıkıcı bir altyapı işlemidir. Bu değişikliği geciktirme veya izleme yeteneği olmadan, işlemin devlet tarafından sunulan hizmetler için veri kaybına veya devlet siz iş yüklerinde bile öngörülemeyen diğer operasyonel sorunlara neden olması mümkündür. 
>

Bir Azure kümesini ölçeklerken aşağıdaki kılavuzu göz önünde bulundurun:
- Birincil düğüm türünü ölçeklendiriyorsanız, bunu hiçbir zaman güvenilirlik [katmanının](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) izin verdiğinden daha fazla küçültmemelisiniz.

Bir düğüm türünü yukarı veya aşağı ölçekleme işlemi, birincil olmayan veya birincil düğüm türü olup olmadığına bağlı olarak farklıdır.

### <a name="scaling-non-primary-node-types"></a>Birincil olmayan düğüm türlerini ölçekleme
İhtiyacınız olan kaynaklarla yeni bir düğüm türü oluşturun.  Yeni düğüm türünü içerecek şekilde çalışan hizmetlerin yerleşim kısıtlamalarını güncelleştirin.  Yavaş yavaş (birer birer), kümenin güvenilirliğinin etkilenmemesi için eski düğüm türü örnek sayısının örnek sayısını sıfıra indirin.  Eski düğüm türü kullanımdan kaldırıldığında hizmetler yavaş yavaş yeni düğüm türüne taşınır.

### <a name="scaling-the-primary-node-type"></a>Birincil düğüm türünü ölçekleme
Birincil düğüm türünün VM SKU'yu değiştirmemenizi öneririz. Daha fazla küme kapasitesine ihtiyacınız varsa, daha fazla örnek eklemenizi öneririz. 

Bu mümkün değilse, eski kümenizden yeni bir küme oluşturabilir ve uygulama durumunu (varsa) [geri yükleyebilirsiniz.](service-fabric-reliable-services-backup-restore.md) Herhangi bir sistem hizmet durumunu geri yüklemeniz gerekmez, uygulamalarınızı yeni kümenize dağıttığınızda yeniden oluşturulurlar. Kümenizde durum ifadesüz uygulamalar çalıştırıyorsanız, tek yaptığınız uygulamaları yeni kümeye dağıtmaksa, geri yüklemeniz gereken hiçbir şey yoktur. Desteklenmeyen rotaya gitmeye karar verirseniz ve VM SKU'yu değiştirmek istiyorsanız, yeni SKU'yu yansıtacak şekilde sanal makine ölçeğinde Model tanımını ayarlayın değişiklikler yapın. Kümenizde yalnızca bir düğüm türü varsa, tüm durum lu uygulamalarınızın tüm [Hizmet yineleme yaşam döngüsü olaylarına](service-fabric-reliable-services-lifecycle.md) (yapıdaki yineleme sıkışmış gibi) zamanında yanıt verdiğinden ve hizmet çoğaltma yeniden oluşturma sürenizin beş dakikadan az olduğundan (Gümüş dayanıklılık düzeyi için) emin olun. 

## <a name="next-steps"></a>Sonraki adımlar
* Uygulama [ölçeklenebilirliği](service-fabric-concepts-scalability.md)hakkında bilgi edinin.
* [Azure kümesini içinde veya dışında ölçeklendirin.](service-fabric-tutorial-scale-cluster.md)
* Akıcı Azure işlem SDK'sını kullanarak [bir Azure kümesini programlı olarak](service-fabric-cluster-programmatic-scaling.md) ölçeklendirin.
* [Bağımsız bir kümeyi içinde veya dışında ölçeklendirin.](service-fabric-cluster-windows-server-add-remove-nodes.md)

