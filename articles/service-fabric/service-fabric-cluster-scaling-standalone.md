---
title: Azure Hizmet Kumaş bağımsız küme ölçekleme
description: Service Fabric'in tek başına kümelerini girip çıkarma ve yukarı veya aşağı ölçekleme hakkında bilgi edinin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: dekapur
ms.openlocfilehash: 16ec0eb429ec6e8f6613490226b7cff01dff1b32
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451907"
---
# <a name="scaling-service-fabric-standalone-clusters"></a>Ölçekleme Servisi Kumaş bağımsız kümeler
Service Fabric kümesi, mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir. Kümenin bir parçası olan bir makine ye veya VM'ye düğüm denir. Kümeler potansiyel olarak binlerce düğüm içerebilir. Hizmet Kumaşı kümesi oluşturduktan sonra, kümeyi yatay olarak ölçeklendirebilir (düğüm sayısını değiştirebilir) veya dikey olarak (düğümlerin kaynaklarını değiştirebilirsiniz).  Kümede iş yükleri çalışıyor olsa bile, kümeyi istediğiniz zaman ölçeklendirebilirsiniz.  Küme ölçeklendikçe, uygulamalarınız da otomatik olarak ölçeklenir.

Kümeyi neden ölçeklendirin? Uygulama talepleri zaman içinde değişir.  Artan uygulama iş yükünü veya ağ trafiğini karşılamak veya talep düştüğünde küme kaynaklarını azaltmak için küme kaynaklarını artırmanız gerekebilir.

## <a name="scaling-in-and-out-or-horizontal-scaling"></a>Girip çıkma veya yatay ölçekleme
Kümedeki düğüm sayısını değiştirir.  Yeni düğümler kümeye katıldıktan sonra, [Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md) hizmetleri onlara taşır ve bu da varolan düğümlerin yükünü azaltır.  Kümenin kaynakları verimli bir şekilde kullanılıyorsa düğüm sayısını da azaltabilirsiniz.  Düğümler kümeden çıktıkça, hizmetler bu düğümlerden hareket eder ve kalan düğümlerde yük artar.  Azure'da çalışan bir kümedeki düğüm sayısını azaltmak, kullandığınız VM'lerin sayısını ödediğiniz için, bu VM'lerin iş yükünü değil, paradan tasarruf etmenizi sağlayabilir.  

- Avantajları: Sonsuz ölçek, teoride.  Uygulamanız ölçeklenebilirlik için tasarlandıysa, daha fazla düğüm ekleyerek sınırsız büyüme sağlayabilirsiniz.  Bulut ortamlarında araç kullanımı düğüm eklemeyi veya kaldırmayı kolaylaştırır, bu nedenle kapasiteyi ayarlamak kolaydır ve yalnızca kullandığınız kaynaklar için ödeme yaparsınız.  
- Dezavantajları: Uygulamalar [ölçeklenebilirlik için tasarlanmalıdır.](service-fabric-concepts-scalability.md)  Uygulama veritabanları ve kalıcılık da ölçeklendirmek için ek mimari çalışma gerektirebilir.  Ancak Service Fabric stateful services'deki [güvenilir koleksiyonlar,](service-fabric-reliable-services-reliable-collections.md) uygulama verilerinizi ölçeklendirmenizi çok daha kolay hale getirir.

Bağımsız kümeler, Service Fabric kümesini şirket içinde veya seçtiğiniz bulut sağlayıcısında dağıtmanıza olanak sağlar.  Düğüm türleri, dağıtımınıza bağlı olarak fiziksel makinelerden veya sanal makinelerden oluşur. Azure'da çalışan kümelerle karşılaştırıldığında, bağımsız bir kümeyi ölçekleme işlemi biraz daha fazla ilgilidir.  Kümedeki düğüm sayısını el ile değiştirmeniz ve ardından küme yapılandırma yükseltmesini çalıştırmanız gerekir.

Düğümlerin kaldırılması birden çok yükseltme başlatabilir. Bazı düğümler etiketle `IsSeedNode=”true”` işaretlenir ve [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest)kullanarak küme bildirimisorgulayarak tanımlanabilir. Tohum düğümleri bu tür senaryolarda etrafında taşınması gerekecektir bu tür düğümlerin kaldırılması diğerlerinden daha uzun sürebilir. Küme, en az üç birincil düğüm türü düğümü tutmalı.

> [!WARNING]
> Küme için Güvenilirlik Katmanı küme [boyutunun](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) altındaki düğüm sayısını düşürmemenizi öneririz. Bu, Service Fabric System Services'ın küme üzerinde çoğaltılması yeteneğini engelleyecek ve kümenin dengesini bozacak veya büyük olasılıkla yok edecektir.
>

Bağımsız bir küme ölçeklenirken aşağıdaki yönergeleri göz önünde bulundurun:
- Birincil düğümlerin değiştirilmesi, kaldırılıp toplu olarak eklemek yerine birdüğüm diğerine yapılmalıdır.
- Düğüm türünü çıkarmadan önce, düğüm türüne başvuran düğüm olup olmadığını kontrol edin. Karşılık gelen düğüm türünü çıkarmadan önce bu düğümleri kaldırın. Karşılık gelen tüm düğümler kaldırıldıktan sonra, Küme yapılandırmasından NodeType'ı kaldırabilir ve [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade)kullanarak yapılandırma yükseltmesi başlatabilirsiniz.

Daha fazla bilgi için tek [başına bir kümeölçek](service-fabric-cluster-windows-server-add-remove-nodes.md)bakın.

## <a name="scaling-up-and-down-or-vertical-scaling"></a>Yukarı ve aşağı ölçekleme veya dikey ölçekleme 
Kümedeki düğümlerin kaynaklarını (CPU, bellek veya depolama) değiştirir.
- Avantajları: Yazılım ve uygulama mimarisi aynı kalır.
- Dezavantajları: Sonlu ölçek, tek tek düğümlerde kaynakları ne kadar artırabileceğinizin bir sınırı olduğundan. Kaynak eklemek veya kaldırmak için fiziksel veya sanal makineleri çevrimdışı almanız gerekeceği için kapalı kalma süresi.

## <a name="next-steps"></a>Sonraki adımlar
* Uygulama [ölçeklenebilirliği](service-fabric-concepts-scalability.md)hakkında bilgi edinin.
* [Azure kümesini içinde veya dışında ölçeklendirin.](service-fabric-tutorial-scale-cluster.md)
* Akıcı Azure işlem SDK'sını kullanarak [bir Azure kümesini programlı olarak](service-fabric-cluster-programmatic-scaling.md) ölçeklendirin.
* [Bağımsız bir kümeyi içinde veya dışında ölçeklendirin.](service-fabric-cluster-windows-server-add-remove-nodes.md)

