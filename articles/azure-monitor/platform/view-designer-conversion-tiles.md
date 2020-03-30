---
title: Çalışma kitapları döşeme dönüşümlerine Azure Monitor görünüm tasarımcısı
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: f07d15521c787dfd588c285bff57616059caa2f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658635"
---
# <a name="azure-monitor-view-designer-tile-conversions"></a>Azure Monitor görünüm tasarımcı döşemesi dönüşümleri
[Görünüm tasarımcısı,](view-designer.md) Azure Monitor'un, Log Analytics çalışma alanınızdaki grafikler, listeler ve zaman çizelgeleriyle verileri görselleştirmenize yardımcı olacak özel görünümler oluşturmanıza olanak tanıyan bir özelliğidir. Bunlar aşamalı olarak devre dışı ediliyor ve ek işlevsellik sağlayan çalışma kitaplarıyla değiştiriliyor. Bu makalede, çalışma kitaplarına farklı kutucuklar dönüştürme için ayrıntılar sağlar.

## <a name="donut--list-tile"></a>Donut & liste karo

![Donut Listesi](media/view-designer-conversion-tiles/donut-list.png)

Çalışma kitaplarındaki donut & listesi döşemesini yeniden oluşturmak iki ayrı görselleştirme içerir. Çörek kısmı için iki seçenek vardır.
Sorgu **ekle'yi** seçerek her iki başlangıç için de özgün sorguyu görünüm tasarımcısından hücreye yapıştırın.

**Seçenek 1:** **Visualization** açılan menüsünden Pasta ![ **Grafiği'ni** seçin: Pasta grafiği görselleştirme menüsü](media/view-designer-conversion-tiles/pie-chart.png)

**Seçenek 2:** **Visualization** açılır tarafından `| render piechart` **sorguya göre ayarla'yı** seçin ve sorguya ekleyin:

 ![Görselleştirme Menüsü](media/view-designer-conversion-tiles/set-by-query.png)

**Örnek**

Özgün sorgu
```KQL
search * 
| summarize AggregatedValue = count() by Type 
| order by AggregatedValue desc
```

Güncelleştirilmiş sorgu
```KQL
search * 
| summarize AggregatedValue = count() by Type 
| order by AggregatedValue desc 
| render piechart
```

Liste oluşturmak ve sparklines'ı etkinleştirmek [için, ortak görevler](view-designer-conversion-tasks.md)hakkındaki makaleye bakın.

Donut & listesinin çalışma kitaplarında nasıl yeniden yorumlanabildiğini bir örnek aşağıda verilmiştir:

![Donut listesi çalışma kitapları](media/view-designer-conversion-tiles/donut-workbooks.png)

## <a name="line-chart--list-tile"></a>Liste & grafik döşeme
![Çizgi grafiği Listesi](media/view-designer-conversion-tiles/line-list.png) 

Satır grafiği bölümünü yeniden oluşturmak için sorguyu aşağıdaki gibi güncelleştirin:

Özgün sorgu
```KQL
search * 
| summarize AggregatedValue = count() by Type
```

Güncelleştirilmiş sorgu
```KQL
search * 
| make-series Count = count() default=0 on TimeGenerated from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain} by Type
```

Çizgi grafiğini görselleştirmek için iki seçenek vardır

**Seçenek 1:** **Visualization** açılır listesinden **Satır grafiğini** seçin:
 
 ![Çizgi grafiği Menü](media/view-designer-conversion-tiles/line-visualization.png)

**Seçenek 2:** **Visualization** açılır tarafından `| render linechart` **sorguya göre ayarla'yı** seçin ve sorguya ekleyin:

 ![Görselleştirme Menüsü](media/view-designer-conversion-tiles/set-by-query.png)

**Örnek**

```KQL
search * 
| make-series Count = count() default=0 on TimeGenerated from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain} by Type 
| render linechart_
```

Liste oluşturmak ve sparklines'ı etkinleştirmek [için, ortak görevler](view-designer-conversion-tasks.md)hakkındaki makaleye bakın.

Aşağıda, satır grafiği & liste döşemesinin çalışma kitaplarında nasıl yeniden yorumlanananadana bir örnek verilmiştir:

![Çizgi grafik listesi çalışma kitapları](media/view-designer-conversion-tiles/line-workbooks.png)

## <a name="number--list-tile"></a>Sayı & liste döşemesi

 ![Döşeme listesi](media/view-designer-conversion-tiles/tile-list-example.png)

Sayı döşemesi için sorguyu aşağıdaki gibi güncelleştirin:

Özgün sorgu
```KQL
search * 
| summarize AggregatedValue = count() by Computer | count
```

Güncelleştirilmiş sorgu
```KQL
search *
| summarize AggregatedValue = count() by Computer 
| summarize Count = count()
```

Visualization açılır düşüşünü **Kutucuklar** olarak değiştirin ve ardından **Döşeme Ayarları'nı**seçin.
 ![Çini Görselleştirme](media/view-designer-conversion-tiles/tile-visualization.png)

**Başlık** bölümünü boş bırakın ve **Sol'u**seçin. **Kullanım sütunu** için değeri değiştirme: **Count**ve **Column Renderer** için **Büyük Sayı**:

![Döşeme Ayarları](media/view-designer-conversion-tiles/tile-settings.png)

 
Liste oluşturmak ve sparklines'ı etkinleştirmek [için, ortak görevler](view-designer-conversion-tasks.md)hakkındaki makaleye bakın.

Aşağıdaki, & liste döşemesinin çalışma kitaplarında nasıl yeniden yorumlanabildiğini bir örnektir:

![Sayı Listesi Çalışma Kitapları](media/view-designer-conversion-tiles/number-workbooks.png)

## <a name="timeline--list"></a>Zaman çizelgesi ve Liste

 ![Zaman Çizelgesi Listesi](media/view-designer-conversion-tiles/time-list.png)

Zaman çizelgesi için sorgunuzu aşağıdaki gibi güncelleyin:

Özgün sorgu
```KQL
search * 
| sort by TimeGenerated desc
```

Güncelleştirilmiş sorgu
```KQL
search * 
| summarize Count = count() by Computer, bin(TimeGenerated,{TimeRange:grain})
```

Sorguyu çubuk grafik olarak görselleştirmek için iki seçenek vardır:

**Seçenek 1:** **Visualization** açılır listesinden Çubuk ![ **grafiği** ni seçin: Barchart görselleştirme](media/view-designer-conversion-tiles/bar-visualization.png)
 
**Seçenek 2:** **Visualization** açılır tarafından `| render barchart` **sorguya göre ayarla'yı** seçin ve sorguya ekleyin:

 ![Görselleştirme menüsü](media/view-designer-conversion-tiles/set-by-query.png)

 
Liste oluşturmak ve sparklines'ı etkinleştirmek [için, ortak görevler](view-designer-conversion-tasks.md)hakkındaki makaleye bakın.

Aşağıdaki zaman çizelgesi & liste döşemesi çalışma kitaplarında yeniden nasıl yorumlanabileceği bir örnektir:

![Zaman Çizelgesi Listesi Çalışma Kitapları](media/view-designer-conversion-tiles/time-workbooks.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Çalışma kitapları geçişine görünüm tasarımcısına genel bakış](view-designer-conversion-overview.md)
