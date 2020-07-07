---
title: Alt kümelenmiş ölçümlerin dengelenmesi
description: Yerleştirme kısıtlamalarının dengelemeye ve nasıl işleneceğini gösteren etkisi
author: nipavlo
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: nipavlo
ms.openlocfilehash: 7f571a851e4da147240c524b742bcd652bc54181
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82183134"
---
# <a name="balancing-of-subclustered-metrics"></a>Alt kümelenmiş ölçümlerin dengelenmesi

## <a name="what-is-subclustering"></a>Alt kümeleme nedir?

Alt kümeleme, farklı yerleştirme kısıtlamalarına sahip hizmetlerde ortak bir ölçüm olduğunda ve her iki raporu da bu BT için yükleme yaptığı zaman gerçekleşir. Hizmetler tarafından bildirilen yük önemli ölçüde farklıysa, düğümlerdeki Toplam yükün büyük bir standart sapması olur ve mümkün olan en iyi dengeyi elde etse bile, kümenin dengesizliği olduğu anlaşılıyor.

## <a name="how-subclustering-affects-load-balancing"></a>Alt kümeleme yük dengelemeyi nasıl etkiler

Farklı düğümlerdeki hizmetler tarafından bildirilen yükün önemli ölçüde farklı olduğu durumlarda, hiç olmadığı büyük bir dengesizliği gibi görünebilir. Ayrıca, alt kümelemenin neden olduğu hatalı ımbakiye gerçek dengesizinden fazlaysa, Kaynak Yöneticisi Dengeleme algoritmasını karıştırıp kümedeki en iyi dengeyi elde etmek için olası olur.

Örneğin, dört hizmeti sunuyoruz ve hepsi ölçüm Metric1 için bir yük raporlarız:

* A hizmeti – "NodeType = = ön uç" yerleştirme kısıtlamasına sahiptir, 10 ' un bir yükünü raporlar
* Hizmet B – "NodeType = = ön uç" yerleştirme kısıtlamasına sahiptir, 10 ' un bir yükünü raporlar
* Service C: "NodeType = = arka uç" yerleştirme kısıtlamasına sahiptir, 100 yükünü raporlar
* Hizmet D – "NodeType = = arka uç" yerleştirme kısıtlamasına sahiptir, 100 yükünü raporlar
* Dört düğümünüz vardır. İkisi de, NodeType "ön uç" olarak ayarlanmış ve diğer iki "arka uç"

Aşağıdaki yerleşimi sunuyoruz:

<center>

![Alt kümelenmiş yerleştirme örneği][Image1]
</center>

Küme dengesiz görünebilir, 3 ve 4. düğümlerde büyük bir yükleme yaptık, ancak bu yerleştirme bu durumda olası en iyi bakiyeyi oluşturuyor.

Kaynak Yöneticisi, alt kümeleme durumlarını tanıyabilir ve neredeyse tüm durumlarda verilen durum için en uygun bakiyeyi üretebilir.

Kaynak Yöneticisi, alt kümelenmiş bir ölçümü en iyi şekilde dengelemediği zaman bazı durumlar için alt kümelendirmeyi tespit eder ve sorunu çözmenizi sağlayacak bir sistem durumu raporu oluşturacaktır.

## <a name="types-of-subclustering-and-how-they-are-handled"></a>Alt kümeleme türleri ve bunların nasıl işlendiği

Alt kümeleme durumları, üç farklı kategoride sınıflandırılabilir. Belirli bir alt kümeleme durumunun kategorisi, Kaynak Yöneticisi tarafından nasıl işleneceğini belirler.

### <a name="first-category--flat-subclustering-with-disjoint-node-groups"></a>İlk kategori – ayrık düğüm gruplarıyla düz alt kümeleme

Bu kategori, düğümlerin farklı gruplara ayrılabileceği ve her hizmetin yalnızca bu gruplardan birindeki düğümlere yerleştirilebileceği en basit alt kümeleme biçimine sahiptir. Her düğüm yalnızca bir gruba ve yalnızca bir gruba aittir. Yukarıda açıklanan durum, alt kümeleme durumlarının büyük bir kısmında olduğu şekilde bu kategoride yer alıyor. 

Bu kategorideki durumlar için Kaynak Yöneticisi en iyi bakiyeyi üretebilir ve daha fazla müdahale gerekmez.

### <a name="second-category--subclustering-with-hierarchical-node-groups"></a>İkinci kategori – hiyerarşik düğüm gruplarıyla alt kümeleme

Bu durum, bir hizmet için izin verilen düğüm grubu, başka bir hizmet için izin verilen düğüm grubunun bir alt kümesi olduğunda meydana gelir. Bu durumun en yaygın örneği, bazı hizmette bir yerleştirme kısıtlaması olduğunda ve başka bir hizmetin yerleştirme kısıtlaması olmadığında ve herhangi bir düğüme yerleştirilebileceği durumdur.

Örnek:

* Hizmet A: yerleştirme kısıtlaması yok
* Hizmet B: yerleştirme kısıtlaması "NodeType = = ön uç"
* Service C: yerleştirme kısıtlaması "NodeType = = arka uç"

Bu yapılandırma, farklı hizmetlerin düğüm grupları arasında bir alt küme üst kümesi ilişkisi oluşturur.

<center>

![Alt küme alt kümesi alt kümeleri][Image2]
</center>

Bu durumda, bir alt yük dengelenmesi olasılığı vardır.

Kaynak Yöneticisi, bu durumu tanıyacak ve arka uç düğümlerine yerleştirilebilecek bir ön uç düğümlerinde ve Service a2 hizmetine yerleştirilebilecek Service a1 hizmetini iki hizmet halinde bölmek için bir sistem durumu raporu oluşturur. Bu, en iyi şekilde dengelenebilir ilk kategori durumuna geri getirir.

### <a name="third-category--subclustering-with-partial-overlap-between-node-sets"></a>Üçüncü kategori – düğüm kümeleri arasında kısmi çakışma olan alt kümeleme

Bu durum, üzerinde bazı hizmetlerin yerleştirilebileceği düğüm kümeleri arasında kısmi bir çakışma olduğunda meydana gelir.

Örneğin, NodeColor adlı bir Node özelliği varsa ve üç düğümünüz varsa:

* Düğüm 1: NodeColor = Red
* Düğüm 2: NodeColor = mavi
* Düğüm 2: NodeColor = yeşil

Ve iki hizmetimiz vardır:

* Hizmet A: yerleştirme kısıtlamasıyla "Color = = Red | | Color = = mavi "
* Hizmet B: yerleştirme kısıtlaması ile "Color = = Blue | | Color = = yeşil "

Bu nedenle, A hizmeti 1 ve 2 düğümlerine yerleştirilebilir ve B hizmeti, 2 ve 3 düğümlerine yerleştirilebilir.

Bu durumda, bir alt yük dengelenmesi olasılığı vardır.

Kaynak Yöneticisi bu durumu tanıyacak ve hizmetlerden bazılarını bölmeniz için sizi bildiren bir sistem durumu raporu oluşturacak.

Bu durumda Kaynak Yöneticisi, birden çok bölme yapılacağından ve hizmetlerin bölüneceği en uygun yöntemi tahmin etmenin bir yolu olmadığından, hizmetleri nasıl bölmeye yönelik bir teklife izin veremeyecektir.

## <a name="configuring-subclustering"></a>Alt kümeleme yapılandırma

Alt kümeleme hakkında Kaynak Yöneticisi davranışı aşağıdaki yapılandırma parametreleri değiştirilerek değiştirilebilir:
* SubclusteringEnabled-parametresi, Yük Dengeleme yapılırken Kaynak Yöneticisi hesaba alt kümelendirmeyi alıp almayacağını belirler. Bu parametre kapatılmışsa, Kaynak Yöneticisi alt kümelendirmeyi yoksayacak ve küresel düzeyde en iyi dengeyi elde etmek için denenecek. Bu parametrenin varsayılan değeri false 'dur.
* SubclusteringReportingPolicy-Kaynak Yöneticisi hiyerarşik ve kısmi çakışma alt Kümelemesi için nasıl durum raporları yayacağını belirler. Sıfır değeri, alt kümeleme hakkındaki sistem durumu raporlarının kapalı olduğu anlamına gelir, "1", sistem durumu raporlarının, alt kümeleme durumları ve "2" değeri "Tamam" durum raporları üretecektir. Bu parametre için varsayılan değer "1" değeridir.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="SubclusteringEnabled" Value="true" />
            <Parameter Name="SubclusteringReportingPolicy" Value="1" />
        </Section>
```

Tek başına dağıtımlar veya Azure 'da barındırılan kümeler için Template.jsClusterConfig.jsaracılığıyla:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "SubclusteringEnabled",
          "value": "true"
      },
      {
          "name": "SubclusteringReportingPolicy",
          "value": "1"
      },
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar
* Kaynak Yöneticisi kümenin, kümedeki yükü nasıl yönettiğini ve dengelemediğini öğrenmek için [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md) sayfasındaki makaleye göz atın
* Hizmetlerinizin yalnızca belirli düğümlere yerleştirilebilecek şekilde kısıtlanmasını öğrenmek için bkz. [düğüm özellikleri ve yerleştirme kısıtlamaları](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)

[Image1]:./media/cluster-resource-manager-subclustering/subclustered-placement.png
[Image2]:./media/cluster-resource-manager-subclustering/subset-superset-nodes.png
