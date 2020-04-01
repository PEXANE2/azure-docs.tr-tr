---
title: Azure Hizmet Kumaşı küme dağıtımı planlama
description: Azure'da bir üretim Hizmeti Kumaş küme dağıtımı planlama ve hazırlama hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: ad6a7a6ea9a90bea4a3b6bc553da67a46144dc03
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422276"
---
# <a name="plan-and-prepare-for-a-cluster-deployment"></a>Küme dağıtımı planlama ve hazırlama

Planlama ve bir üretim kümesi dağıtım için hazırlanması çok önemlidir.  Göz önünde bulundurulması gereken birçok faktör vardır.  Bu makalede, küme dağıtım hazırlama adımları size yol.

## <a name="read-the-best-practices-information"></a>En iyi uygulamalarla ilgili bilgileri okuyun
Azure Hizmet Kumaşı uygulamalarını ve kümelerini başarıyla yönetmek için, üretim ortamınızın güvenilirliğini optimize etmek için gerçekleştirmenizi şiddetle tavsiye ettiğimiz işlemler vardır.  Daha fazla bilgi için [Service Fabric uygulamasını okuyun ve küme en iyi uygulamaları.](service-fabric-best-practices-overview.md)

## <a name="select-the-os-for-the-cluster"></a>Küme için işletim sistemi'ni seçin
Service Fabric, Windows Server veya Linux çalıştıran herhangi bir VM veya bilgisayarda Service Fabric kümelerinin oluşturulmasına olanak tanır.  Kümenizi dağıtmadan önce işletim sistemi: Windows veya Linux'u seçmeniz gerekir.  Kümedeki her düğüm (sanal makine) aynı işletim sistemi çalışır, Windows ve Linux VM'leri aynı kümede karıştıramazsınız.

## <a name="capacity-planning"></a>Kapasite planlaması
Herhangi bir üretim dağıtımı için kapsite planlaması önemli bir adımdır. Bu süreç kapsamında dikkat etmeniz gerekenler şunlardır:

* Kümeniz için ilk düğüm türü sayısı 
* Düğüm türünün her birinin özellikleri (boyut, örnek sayısı, birincil, internete dönük, VM sayısı, vb.)
* Kümenin güvenilirlik ve dayanıklılık özellikleri

### <a name="select-the-initial-number-of-node-types"></a>Düğüm türlerinin başlangıç sayısını seçin
İlk olarak, oluşturduğunuz kümenin ne için kullanılacağını bulmanız gerekir. Bu kümeye ne tür uygulamalar dağıtmayı planlıyorsunuz? Uygulamanızın birden çok hizmeti var mı ve bunlardan herhangi birinin herkese açık veya internetle karşı karşıya olması gerekiyor mu? Hizmetleriniz (uygulamanızı oluşturan) daha fazla RAM veya daha yüksek CPU döngüleri gibi farklı altyapı gereksinimlerine sahip mi? Hizmet Kumaşı kümesi birden fazla düğüm türünden oluşabilir: birincil düğüm türü ve bir veya daha fazla birincil olmayan düğüm türü. Her düğüm türü sanal makine ölçeği kümesine eşlenir. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. [Düğüm özellikleri ve yerleşim kısıtlamaları,][placementconstraints] belirli hizmetleri belirli düğüm türlerine kısıtlamak için ayarlanabilir.  Daha fazla bilgi için, [kümenizin başlangıç](service-fabric-cluster-capacity.md#the-number-of-node-types-your-cluster-needs-to-start-out-with)yapması gereken düğüm türlerinin sayısını okuyun.

### <a name="select-node-properties-for-each-node-type"></a>Her düğüm türü için düğüm özelliklerini seçin
Düğüm türleri, vm'lerin vm sku'yu, sayısını ve özelliklerini ilişkili ölçek kümesinde tanımlar.

Her düğüm türü için minimum VM boyutu, düğüm türü için seçtiğiniz [dayanıklılık katmanına][durability] göre belirlenir.

Birincil düğüm türü için minimum VM sayısı seçtiğiniz [güvenilirlik katmanına][reliability] göre belirlenir.

[Birincil düğüm türleri](service-fabric-cluster-capacity.md#primary-node-type---capacity-guidance)için en az önerileri, birincil olmayan düğüm [türlerinde durum lu iş yüklerini](service-fabric-cluster-capacity.md#non-primary-node-type---capacity-guidance-for-stateful-workloads)ve [birincil olmayan düğüm türlerinde durum dışı iş yüklerini](service-fabric-cluster-capacity.md#non-primary-node-type---capacity-guidance-for-stateless-workloads)görün.

En az düğüm sayısından daha fazla sayıda düğüm, bu düğüm türünde çalıştırmak istediğiniz uygulama/hizmet yinelemelerinin sayısına dayanmalıdır.  [Service Fabric uygulamaları için kapasite planlaması,](service-fabric-capacity-planning.md) uygulamalarınızı çalıştırmak için ihtiyacınız olan kaynakları tahmin etmenize yardımcı olur. Uygulama iş yükünü değiştirmek için kümeyi her zaman yukarı veya aşağı ölçeklendirebilirsiniz. 

#### <a name="use-ephemeral-os-disks-for-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri için kısa ömürlü işletim sistemi diskleri kullanma

*Kısa ömürlü işletim sistemi diskleri,* yerel sanal makinede (VM) oluşturulan ve uzak Azure Depolama'ya kaydedilmez depolama alanıdır. Geleneksel kalıcı işletim sistemi diskleri, kısa ömürlü işletim sistemi diskleri ile karşılaştırıldığında tüm Service Fabric düğüm türleri (Birincil ve İkincil) için önerilir:

* Os diskine okuma/yazma gecikmesüresini azaltın
* Daha hızlı sıfırlama/yeniden görüntüle bilme mesuliyet yönetimi işlemlerini etkinleştirme
* Genel maliyetleri azaltın (diskler ücretsizdir ve ek depolama maliyeti içermez)

Kısa ömürlü işletim sistemi diskleri belirli bir Hizmet Kumaşı özelliği değil, Hizmet Kumaşı düğüm türlerine eşlenen Azure *sanal makine ölçek kümelerinin* bir özelliğidir. Bunları Hizmet Dokusu ile kullanmak, küme Azure Kaynak Yöneticisi şablonunuzda aşağıdakileri gerektirir:

1. Düğüm tiplerinizin Kısa ömürlü işletim sistemi diskleri için [desteklenen Azure VM boyutlarını](../virtual-machines/windows/ephemeral-os-disks.md) belirttiğinden ve VM boyutunun işletim sistemi disk boyutunu desteklemek için yeterli önbellek boyutuna sahip olduğundan emin olun (aşağıdaki *Nota* bakın.) Örneğin:

    ```xml
    "vmNodeType1Size": {
        "type": "string",
        "defaultValue": "Standard_DS3_v2"
    ```

    > [!NOTE]
    > Önbellek boyutu VM'nin işletim sistemi boyutuna eşit veya daha büyük bir VM boyutu seçtiğinizden emin olun, aksi takdirde Azure dağıtımınız hataya neden olabilir (başlangıçta kabul edilmiş olsa bile).

2. Sanal makine ölçeği kümesi`vmssApiVersion`sürümünü `2018-06-01` ( ) veya daha sonra belirtin:

    ```xml
    "variables": {
        "vmssApiVersion": "2018-06-01",
    ```

3. Dağıtım şablonunuzun sanal makine ölçeği kümesi `Local` bölümünde `diffDiskSettings`aşağıdakiler için seçeneğini belirtiniz:

    ```xml
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
        "virtualMachineProfile": {
            "storageProfile": {
                "osDisk": {
                        "caching": "ReadOnly",
                        "createOption": "FromImage",
                        "diffDiskSettings": {
                            "option": "Local"
                        },
                }
            }
        }
    ```

> [!NOTE]
> Os diski işletim sistemi yükseltmesi durumunda kaybolacağından, kullanıcı uygulamalarının işletim sistemi diskinde herhangi bir bağımlılık/dosya/yapı olmamalıdır.
> Bu nedenle, geçici diskler ile [PatchOrchestrationApplication](https://github.com/microsoft/Service-Fabric-POA) kullanılması tavsiye edilmez.
>

> [!NOTE]
> Varolan geçici olmayan VMSS, geçici diskleri kullanmak üzere yerinde yükseltilemez.
> Geçiş yapmak için, kullanıcıların geçici disklere sahip yeni bir düğüm Type [eklemeleri,](./virtual-machine-scale-set-scale-node-type-scale-out.md) iş yüklerini yeni düğümTürüne taşımaları & varolan düğüm Türünü [kaldırmaları](./service-fabric-how-to-remove-node-type.md) gerekir.
>

Daha fazla bilgi ve daha fazla yapılandırma seçeneği için [Azure VM'leri için Geçici İşletim Sistemi diskleri'ne](../virtual-machines/windows/ephemeral-os-disks.md) bakın 


### <a name="select-the-durability-and-reliability-levels-for-the-cluster"></a>Kümeiçin dayanıklılık ve güvenilirlik düzeylerini seçin
Dayanıklılık katmanı, vm'lerinizin temel Azure altyapısıyla sahip olduğu ayrıcalıkları sisteme göstermek için kullanılır. Birincil düğüm türünde, bu ayrıcalık Service Fabric'in sistem hizmetleri ve devlet hizmetleri için yeterli çoğunluk gereksinimlerini etkileyen vm düzeyindeki altyapı isteğini (VM yeniden başlatma, VM yeniden başlatma veya VM geçişi gibi) duraklatmasına olanak tanır. Birincil olmayan düğüm türlerinde bu ayrıcalık, Service Fabric'in, devlet hizmetlerinizin yeterli liğini etkileyen VM düzeyindeki altyapı isteklerini (VM yeniden başlatma, VM yeniden görüntü ve VM geçişi gibi) duraklatmasına olanak tanır.  Hangi düzeyde ve ne zaman kullanılacağı konusunda farklı düzeyve önerilerin avantajları için [kümenin dayanıklılık özelliklerine][durability]bakın.

Güvenilirlik katmanı, bu kümede çalıştırmak istediğiniz sistem hizmetlerinin yineleme sayısını birincil düğüm türünde ayarlamak için kullanılır. Çoğaltma sayısı ne kadar çoksa, sistem hizmetleri kümenizde o kadar güvenilir dir.  Hangi düzeyde ve ne zaman kullanılacağı konusunda farklı düzey ve önerilerin avantajları için [kümenin güvenilirlik özelliklerine][reliability]bakın. 

## <a name="enable-reverse-proxy-andor-dns"></a>Ters proxy ve/veya DNS'yi etkinleştirme
Kümedeki düğümler aynı yerel ağda olduğundan, küme içinde birbirine bağlanan hizmetler genellikle diğer hizmetlerin uç noktalarına doğrudan erişebilir. Hizmetler arasında bağlantı kurmayı kolaylaştırmak için Service Fabric ek hizmetler sunar: [Bir DNS hizmeti](service-fabric-dnsservice.md) ve [ters proxy hizmeti.](service-fabric-reverseproxy.md)  Bir küme dağıtılırken her iki hizmet de etkinleştirilebilir.

Birçok hizmet, özellikle kapsayıcı hizmetler, varolan bir URL adı olabilir, standart DNS protokolü (Adlandırma Hizmeti protokolü yerine) kullanarak bunları çözmek için, özellikle uygulama "asansör ve kaydırma" senaryoları uygundur. DNS hizmeti nin yaptığı tam olarak budur. DNS adlarını bir hizmet adı ile eşlenebilmenizi ve dolayısıyla uç nokta IP adreslerini çözmenizi sağlar.

Ters proxy, kümedeki HTTP uç noktalarını (HTTPS dahil) ortaya çıkaran hizmetleri ele adamaktadır. Ters proxy, belirli bir URI biçimi sağlayarak diğer hizmetleri aramayı büyük ölçüde kolaylaştırır.  Ters proxy, bir hizmetin başka bir servisle iletişim kurması için gereken çözüm, bağlanma ve yeniden deneme adımlarını da işler.

## <a name="prepare-for-disaster-recovery"></a>Olağanüstü durum kurtarmaya hazırlanma
Yüksek kullanılabilirlik sağlamanın önemli bir parçası, hizmetlerin her türlü hatadan sağ çıkabilmesini sağlamaktır. Bu, özellikle planlanmamış ve denetiminiz dışında olan hatalar için önemlidir. [Olağanüstü durum kurtarma için hazırlanın,](service-fabric-disaster-recovery.md) doğru şekilde modellenip yönetilmezse felaket olabilecek bazı yaygın hata modları açıklanır. Ayrıca, bir felaket zaten oldu eğer almak için hafifletme ve eylemleri tartışır.

## <a name="production-readiness-checklist"></a>Üretim hazırlığı denetim listesi
Uygulamanız ve kümeniz üretim trafiğine hazır mı? Kümenizi üretime dağıtmadan önce [Üretime hazırlık denetim listesini](service-fabric-production-readiness-checklist.md)çalıştırın. Bu denetim listesindeki öğeler üzerinde çalışarak uygulamanızın ve kümelemenin sorunsuz çalışmasını tutun. Üretime geçmeden önce tüm bu öğelerin kontrol edilmesine şiddetle tavsiye ediyoruz.

## <a name="next-steps"></a>Sonraki adımlar
* [Windows çalıştıran bir Service Fabric kümesi oluşturma](service-fabric-best-practices-overview.md)
* [Linux çalıştıran bir Service Fabric kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md)

[placementconstraints]: service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints
[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[reliability]: service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster