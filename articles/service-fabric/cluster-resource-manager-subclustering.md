---
title: Kümelenmiş ölçümlerin dengelenmesi
description: Yerleştirme kısıtlamalarının dengeleme üzerindeki etkisi ve nasıl işleyeceğiniz
author: nipavlo
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: nipavlo
ms.openlocfilehash: 23782a86d31251cb1a3474e0395df716a2e832df
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430649"
---
# <a name="balancing-of-subclustered-metrics"></a>Kümelenmiş ölçümlerin dengelenmesi

## <a name="what-is-subclustering"></a>Subclustering nedir

Alt kümeleme, farklı yerleşim kısıtlamalarına sahip hizmetlerin ortak bir ölçüye sahip olması ve her ikisi nin de bunun için yük bildirmesi sonucu gerçekleşir. Hizmetler tarafından bildirilen yük önemli ölçüde farklıysa, düğümler üzerindeki toplam yük büyük bir standart sapmaya sahip olur ve mümkün olan en iyi dengeye sahip olsa bile küme dengesiz görünür.

## <a name="how-subclustering-affects-load-balancing"></a>Alt kümeleme yük dengelemeyi nasıl etkiler?

Hizmetler tarafından farklı düğümlerde bildirilen yük önemli ölçüde farklıysa, hiçbir düğümün olmadığı büyük bir dengesizlik varmış gibi görünebilir. Ayrıca, alt kümelenmenin neden olduğu yanlış dengesizlik gerçek dengesizlikten daha büyükse, Kaynak Yöneticisi dengeleme algoritmasını karıştırma ve kümede optimal olmayan dengeyi oluşturma potansiyeline sahiptir.

Örneğin, dört hizmetimiz olduğunu ve hepsinin metrik Metrik1 için bir yük raporettiğimizi varsayalım:

* Hizmet A – bir yerleşim kısıtlaması vardır "NodeType==Type1", 10 yük raporlar
* Hizmet B – bir yerleşim kısıtlaması vardır "NodeType ==Type1", 10 yük raporlar
* Hizmet C – "NodeType==Type2" bir yerleşim kısıtlaması vardır, 100 yük bildirir
* Hizmet D – bir yerleşim kısıtlaması vardır "NodeType ==Type2", 100 yük raporlar
* Ve dört düğümümüz var. Bunlardan ikitanesi "Type1" olarak nodeType kümesi ne de diğer ikisi "Type2" olarak ayarlanmıştır.

Ve biz aşağıdaki yerleşim var:

<center>

![Alt kümelenmiş yerleşim örneği][Image1]
</center>

Küme dengesiz görünebilir, 3 ve 4 düğümleri üzerinde büyük bir yük var, ancak bu yerleşim bu durumda mümkün olan en iyi dengeyi oluşturur.

Kaynak Yöneticisi alt kümeleme durumlarını tanıyabilir ve hemen hemen her durumda verilen durum için en uygun dengeyi üretebilir.

Kaynak Yöneticisi'nin alt kümelenmiş bir ölçümü en iyi şekilde dengeleyemediğinde bazı istisnai durumlarda, alt kümelenmeyi algılamaya devam eder ve sorunu çözmenizi tavsiye edecek bir sistem durumu raporu oluşturur.

## <a name="types-of-subclustering-and-how-they-are-handled"></a>Alt kümelenme türleri ve bunların nasıl işlenir

Alt kümelenme durumları üç farklı kategoriye ayırılabilir. Belirli bir alt kümelenme durumunun kategorisi, bunun Kaynak Yöneticisi tarafından nasıl işleneceğini belirler.

### <a name="first-category--flat-subclustering-with-disjoint-node-groups"></a>Birinci kategori – ayrık düğüm grupları ile düz alt kümeleme

Bu kategori, düğümlerin farklı gruplara ayrılabildiği ve her hizmetin yalnızca bu gruplardan birinde düğümlere yerleştirilebildiği en basit alt kümeleme biçimine sahiptir. Her düğüm yalnızca bir gruba ve bir gruba aittir. Yukarıda açıklanan durum, alt kümeleme durumlarının çoğu gibi bu kategoriye aittir. 

Bu kategorideki durumlar için Kaynak Yöneticisi en uygun dengeyi üretebilir ve başka bir müdahaleye gerek yoktur.

### <a name="second-category--subclustering-with-hierarchical-node-groups"></a>İkinci kategori – hiyerarşik düğüm grupları ile alt kümeleme

Bu durum, bir hizmet için izin verilen bir düğüm grubu, başka bir hizmet için izin verilen düğüm grubu kümesinin bir alt kümesi olduğunda ortaya çıkar. Bu durumun en yaygın örneği, bazı hizmetin tanımlanmış bir yerleşim kısıtlaması olması ve başka bir hizmetin yerleşim kısıtlaması olmaması ve herhangi bir düğüme yerleştirilemesidir.

Örnek:

* Hizmet A: hiçbir yerleşim kısıtlaması
* Hizmet B: yerleşim kısıtlaması "NodeType==Type1"
* Hizmet C: yerleşim kısıtlaması "NodeType==Type2"

Bu yapılandırma, farklı hizmetler için düğüm grupları arasında bir alt küme-superset ilişkisi oluşturur.

<center>

![Alt küme alt kümeleri][Image2]
</center>

Bu durumda, bir suboptimal denge yapılır bir şans vardır.

Kaynak Yöneticisi bu durumu algılar ve Hizmet A'yı iki hizmete bölmenizi tavsiye eden bir sağlık raporu hazırlayacaktır – Type1 düğümlerine yerleştirilebilen Hizmet A1 ve Type2 düğümlerine yerleştirilebilen Hizmet A2. Bu bizi en iyi şekilde dengelenebilir ilk kategori durumuna geri getirecektir.

### <a name="third-category--subclustering-with-partial-overlap-between-node-sets"></a>Üçüncü kategori – düğüm kümeleri arasında kısmi çakışma ile alt kümeleme

Bu durum, bazı hizmetlerin yerleştirilebileceği düğüm kümeleri arasında kısmi bir çakışma olduğunda ortaya çıkan durumdur.

Örneğin, NodeColor adında bir düğümüz varsa ve üç düğümüz varsa:

* Düğüm 1: DüğümColor=Kırmızı
* Düğüm 2: DüğümColor=Mavi
* Düğüm 2: DüğümColor=Yeşil

Ve iki hizmetimiz var:

* Hizmet A: yerleşim kısıtlaması ile "Renk ==Kırmızı || Renk ==Mavi"
* Hizmet B: yerleşim kısıtlaması ile "Renk ==Mavi || Renk ==Yeşil"

Bu nedenle, Hizmet A düğümleri 1 ve 2 ve Hizmet B düğümleri 2 ve 3 yerleştirilebilir.

Bu durumda, bir suboptimal denge yapılır bir şans vardır.

Kaynak Yöneticisi bu durumu algılar ve bazı hizmetleri bölmenizi tavsiye eden bir sağlık raporu hazırlayacaktır.

Bu durum için Kaynak Yöneticisi, birden çok bölme yapılabilecek ve hizmetleri bölmek için en uygun yolun hangi yol olacağını tahmin etmenin bir yolu olmadığından, hizmetlerin nasıl bölüneceği konusunda bir öneri veremez.

## <a name="configuring-subclustering"></a>Alt kümelemenin yapılandırılması

Kaynak Yöneticisi'nin alt kümeleme ile ilgili davranışı aşağıdaki yapılandırma parametreleri değiştirilerek değiştirilebilir:
* SubclusteringEnabled - parametre, Kaynak Yöneticisi'nin yük dengelemesini yaparken alt kümelenmeyi dikkate alıp almayacağını belirler. Bu parametre kapatılırsa, Kaynak Yöneticisi alt kümelemayı yoklar ve genel düzeyde en iyi dengeyi sağlamaya çalışır. Bu parametrenin varsayılan değeri yanlıştır.
* SubclusteringReportingPolicy - Kaynak Yöneticisi'nin hiyerarşik ve kısmi çakışan alt kümeleme için sistem durumu raporlarını nasıl yayacağını belirler. Sıfır değeri, alt kümeleme yle ilgili sağlık raporlarının kapatılması anlamına gelir, "1", alt kümeleme durumları için uyarı sağlık raporlarının üretileceği ve "2" değerinin "Tamam" sağlık raporları üreteceği anlamına gelir. Bu parametrenin varsayılan değeri "1"dir.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="SubclusteringEnabled" Value="true" />
            <Parameter Name="SubclusteringReportingPolicy" Value="1" />
        </Section>
```

Bağımsız dağıtımlar için ClusterConfig.json veya Azure barındırılan kümeler için Template.json aracılığıyla:

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
* Küme Kaynak Yöneticisi'nin kümedeki yükü nasıl yönettiği ve dengeler yaptığı hakkında bilgi edinmek [için, yükü dengeleme](service-fabric-cluster-resource-manager-balancing.md) makalesine göz atın
* Hizmetlerinizin yalnızca belirli düğümlere yerleştirilecek şekilde nasıl sınırlandırılabildiğini öğrenmek için [Düğüm özelliklerini ve yerleşim kısıtlamalarını](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints) görün

[Image1]:./media/cluster-resource-manager-subclustering/subclustered-placement.png
[Image2]:./media/cluster-resource-manager-subclustering/subset-superset-nodes.png
