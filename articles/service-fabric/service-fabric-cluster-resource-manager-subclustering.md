---
title: Alt kümelenmiş ölçümlerin dengelenmesi
description: Yerleştirme kısıtlamalarının dengelemeye ve nasıl işleneceğini gösteren etkisi
author: nipavlo
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: nipavlo
ms.openlocfilehash: fe41f45399670d5ac003e5da6c8da0fb1e0847b2
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79081682"
---
# <a name="balancing-of-subclustered-metrics"></a>Alt kümelenmiş ölçümlerin dengelenmesi

## <a name="what-is-subclustering"></a>Alt kümeleme nedir?

Alt kümeleme, farklı yerleştirme kısıtlamalarına sahip hizmetlerde ortak bir ölçüm olduğunda ve her iki raporu da bu BT için yükleme yaptığı zaman gerçekleşir. Hizmetler tarafından bildirilen yük önemli ölçüde farklıysa, düğümlerdeki Toplam yükün büyük bir standart sapması olur ve mümkün olan en iyi dengeyi elde etse bile, kümenin dengesizliği olduğu anlaşılıyor.

## <a name="how-subclustering-affects-load-balancing"></a>Alt kümeleme yük dengelemeyi nasıl etkiler

Farklı düğümlerdeki hizmetler tarafından bildirilen yükün önemli ölçüde farklı olduğu durumlarda, hiç olmadığı büyük bir dengesizliği gibi görünebilir. Ayrıca, alt kümelemenin neden olduğu hatalı ımbakiye gerçek dengesizinden fazlaysa, Kaynak Yöneticisi Dengeleme algoritmasını karıştırıp kümedeki en iyi dengeyi elde etmek için olası olur.

Örneğin, dört hizmeti sunuyoruz ve hepsi ölçüm Metric1 için bir yük raporlarız:

* A hizmeti – bir "NodeType = = Type1" yerleştirme kısıtlamasına sahiptir, 10 ' un bir yükünü raporlar
* Hizmet B – bir "NodeType = = Type1" yerleştirme kısıtlamasına sahiptir, 10 ' un bir yükünü raporlar
* Service C: "NodeType = = type2" yerleştirme kısıtlamasına sahiptir, 100 yükünü raporlar
* Hizmet D – bir "NodeType = = type2" yerleştirme kısıtlamasına sahiptir, 100 yükünü raporlar
* Dört düğümünüz vardır. İkisi de "Type1" olarak ayarlanmış NodeType ve diğer ikisi "type2"

Aşağıdaki yerleşimi sunuyoruz:

<center>

![alt kümelenmiş yerleştirme örneği][Image1]
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
* Hizmet B: yerleştirme kısıtlaması "NodeType = = Type1"
* Service C: yerleştirme kısıtlaması "NodeType = = type2"

Bu yapılandırma, farklı hizmetlerin düğüm grupları arasında bir alt küme üst kümesi ilişkisi oluşturur.

<center>

![alt küme üst kümesi alt kümeler][Image2]
</center>

Bu durumda, bir alt yük dengelenmesi olasılığı vardır.

Kaynak Yöneticisi bu durumu tanıyacak ve bir sistem durumu raporu oluşturacak ve bu hizmeti, Type2 düğümlerine yerleştirilebilecek Type1 düğümlerine ve Service a2 hizmetine yerleştirilebilecek iki hizmet-hizmet a1. Bu, en iyi şekilde dengelenebilir ilk kategori durumuna geri getirir.

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

ClusterManifest. xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="SubclusteringEnabled" Value="true" />
            <Parameter Name="SubclusteringReportingPolicy" Value="1" />
        </Section>
```

Tek başına dağıtımlar için ClusterConfig. JSON veya Azure 'da barındırılan kümeler için Template. JSON aracılığıyla:

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

[Image1]:./media/service-fabric-cluster-resource-manager-subclustering/subclustered-placement.png
[Image2]:./media/service-fabric-cluster-resource-manager-subclustering/subset-superset-nodes.png
