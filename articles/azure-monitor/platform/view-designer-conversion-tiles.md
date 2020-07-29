---
title: Azure Izleyici Görünüm Tasarımcısı çalışma kitapları kutucuk dönüştürmeleri
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: f07d15521c787dfd588c285bff57616059caa2f3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77658635"
---
# <a name="azure-monitor-view-designer-tile-conversions"></a>Azure Izleyici Görünüm Tasarımcısı kutucuk dönüştürmeleri
[Görünüm Tasarımcısı](view-designer.md) , Log Analytics çalışma alanınızdaki verileri grafiklerle, listelerle ve zaman çizelgeleriyle görselleştirmenize yardımcı olmak üzere özel görünümler oluşturmanıza olanak sağlayan bir Azure izleyici özelliğidir. Bunlar kullanıma alınır ve ek işlevsellik sağlayan çalışma kitapları ile değiştirilmiştir. Bu makalede, farklı kutucukları çalışma kitaplarına dönüştürmeye ilişkin ayrıntılar sağlanmaktadır.

## <a name="donut--list-tile"></a>Halka & listesi kutucuğu

![Halka listesi](media/view-designer-conversion-tiles/donut-list.png)

Çalışma kitaplarında halka & listesi kutucuğunu yeniden oluşturmak iki ayrı görselleştirmeyi içerir. Halka bölümü için iki seçenek vardır.
Her iki başlatma için de **Sorgu Ekle** ' ye ve özgün sorguyu görünüm tasarımcısından hücreye yapıştırın.

**Seçenek 1:** **Görselleştirme** açılan menüsünde **pasta grafik** seçin: ![ pasta grafik görselleştirme menüsü](media/view-designer-conversion-tiles/pie-chart.png)

**Seçenek 2:** **Görselleştirme** açılan menüsünde **sorguya göre ayarla** ' yı seçin ve `| render piechart` sorguya ekleyin:

 ![Görselleştirme menüsü](media/view-designer-conversion-tiles/set-by-query.png)

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

Bir liste oluşturmak ve mini grafikleri etkinleştirmek için [ortak görevlerle](view-designer-conversion-tasks.md)ilgili makaleye bakın.

Aşağıda, halka & listesi kutucuğunun çalışma kitaplarında yeniden nasıl yorumlanabilecek bir örnek verilmiştir:

![Halka listesi çalışma kitapları](media/view-designer-conversion-tiles/donut-workbooks.png)

## <a name="line-chart--list-tile"></a>Çizgi grafik & liste kutucuğu
![Çizgi grafik listesi](media/view-designer-conversion-tiles/line-list.png) 

Çizgi grafik bölümünü yeniden oluşturmak için sorguyu aşağıdaki gibi güncelleştirin:

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

Çizgi grafiği görselleştirmeye yönelik iki seçenek vardır

**Seçenek 1:** **Görselleştirme** açılan menüsünden **çizgi grafik** ' i seçin:
 
 ![Çizgi grafik menüsü](media/view-designer-conversion-tiles/line-visualization.png)

**Seçenek 2:** **Görselleştirme** açılan menüsünde **sorguya göre ayarla** ' yı seçin ve `| render linechart` sorguya ekleyin:

 ![Görselleştirme menüsü](media/view-designer-conversion-tiles/set-by-query.png)

**Örnek**

```KQL
search * 
| make-series Count = count() default=0 on TimeGenerated from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain} by Type 
| render linechart_
```

Bir liste oluşturmak ve mini grafikleri etkinleştirmek için [ortak görevlerle](view-designer-conversion-tasks.md)ilgili makaleye bakın.

Aşağıda, çizgi grafik & listesi kutucuğunun çalışma kitaplarında yeniden nasıl yorumlanabilecek bir örnek verilmiştir:

![Çizgi grafik listesi çalışma kitapları](media/view-designer-conversion-tiles/line-workbooks.png)

## <a name="number--list-tile"></a>& listesi kutucuğu sayısı

 ![Kutucuk Listesi](media/view-designer-conversion-tiles/tile-list-example.png)

Sayı kutucuğu için sorguyu aşağıdaki gibi güncelleştirin:

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

Görselleştirme açılan menüsünü **döşemeler** olarak değiştirip **kutucuk ayarları**' nı seçin.
 ![Kutucuk görselleştirmesi](media/view-designer-conversion-tiles/tile-visualization.png)

**Başlık** bölümünü boş bırakın ve **sol**' u seçin. **Kullanım sütunu:** **Count**ve **sütun oluşturucuyu** **büyük sayı**olarak değiştirin:

![Kutucuk ayarları](media/view-designer-conversion-tiles/tile-settings.png)

 
Bir liste oluşturmak ve mini grafikleri etkinleştirmek için [ortak görevlerle](view-designer-conversion-tasks.md)ilgili makaleye bakın.

Aşağıda, sayı & listesi kutucuğunun çalışma kitaplarında yeniden nasıl yorumlanabilecek bir örnek verilmiştir:

![Sayı listesi çalışma kitapları](media/view-designer-conversion-tiles/number-workbooks.png)

## <a name="timeline--list"></a>Zaman çizelgesi ve Liste

 ![Zaman çizelgesi listesi](media/view-designer-conversion-tiles/time-list.png)

Zaman çizelgesi için sorgunuzu aşağıdaki şekilde güncelleştirin:

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

Sorguyu çubuk grafik olarak görselleştirmeye yönelik iki seçenek vardır:

**Seçenek 1:** **Görselleştirme** açılan menüsünde **çubuk grafik** ' i seçin: ![ bargrafik görselleştirmesi](media/view-designer-conversion-tiles/bar-visualization.png)
 
**Seçenek 2:** **Görselleştirme** açılan menüsünde **sorguya göre ayarla** ' yı seçin ve `| render barchart` sorguya ekleyin:

 ![Görselleştirme menüsü](media/view-designer-conversion-tiles/set-by-query.png)

 
Bir liste oluşturmak ve mini grafikleri etkinleştirmek için [ortak görevlerle](view-designer-conversion-tasks.md)ilgili makaleye bakın.

Aşağıda, zaman çizelgesi & listesi kutucuğunun çalışma kitaplarında yeniden nasıl yorumlanabilecek bir örnek verilmiştir:

![Zaman çizelgesi listesi çalışma kitapları](media/view-designer-conversion-tiles/time-workbooks.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Çalışma kitapları geçişine görünüm tasarımcısına genel bakış](view-designer-conversion-overview.md)
