---
title: Azure Monitor görünüm tasarımcısından çalışma kitaplarına dönüşüm yaygın görevler
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 5559dac916262998d621b40757398088ec613609
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658753"
---
# <a name="view-designer-to-workbooks-conversion-common-tasks"></a>Tasarımcıyı çalışma kitaplarıdönüştürme ortak görevleri ne kadar görüntüleme
[Görünüm tasarımcısı,](view-designer.md) Azure Monitor'un, Log Analytics çalışma alanınızdaki grafikler, listeler ve zaman çizelgeleriyle verileri görselleştirmenize yardımcı olacak özel görünümler oluşturmanıza olanak tanıyan bir özelliğidir. Bunlar aşamalı olarak devre dışı ediliyor ve ek işlevsellik sağlayan çalışma kitaplarıyla değiştiriliyor. Bu makalede, görünümleri çalışma kitaplarına dönüştürmede yaygın olan görevler ayrıntılarıyla anlatılır.


## <a name="quickstart-with-preset-view-designer-templates"></a>Önceden ayarlanmış görünüm tasarımcısı şablonları ile hızlı başlatma

Log Analytics çalışma alanlarındaki çalışma kitaplarında görünüm tasarımcısındaki bazı görünümlerle eşleşecek şekilde şablonlar zaten bulunmaktadır. Tasarımcı **Kılavuzlarını Görüntüle** kategorisi altında, seçenekleriniz hakkında bilgi edinmek veya önceden ayarlanmış şablonlardan birini seçmek için **Tasarımcı Geçiş Kılavuzunu** Görüntüle'yi seçin.

![Örnek şablonlar](media/view-designer-conversion-tasks/templates.png)

## <a name="enabling-time-range-filter"></a>Zaman aralığı filtresini etkinleştirme
Görünüm tasarımcısı yerleşik varsayılan zaman aralığı filtresine sahiptir, ancak çalışma kitaplarında bu ayar varsayılan olarak etkinleştirilemez. Çalışma kitapları, kullanıcıların veri günlükleri için daha uygun olabilecek kendi zaman aralığı filtrelerini oluşturmalarına olanak sağlar. Filtreyi oluşturmak için adımlar aşağıda listelenmiştir:

**Parametreleri Ekle** seçeneğini belirleyin. Varsayılan **Stil** *Pills*olarak ayarlanır.

![Param Ekle](media/view-designer-conversion-tasks/add-param.png)

 **Parametre Ekle düğmesini** seçin.

![Parametre Ekle](media/view-designer-conversion-tasks/add-parameter.png)

Kenar çubuğu menüsünden, **Parametre adı** textbox'ında *TimeRange*yazın. **Parametre Türünü** *Zaman Aralığı Seçici*olarak ayarlayın. Gerekli **onay** kutusunu seçin.

![Parametre Menüsü](media/view-designer-conversion-tasks/parameter-menu.png)

Parametreyi kenar çubuğu menüsünün sol üst köşesinde kaydedin. Açılır bırakma işlemini varsayılan olarak *unset* olarak bırakabilir veya *24 saat*gibi varsayılan bir **TimeRange** değeri seçebilirsiniz. **Bitti Düzenleme'yi**seçin.

Parametreler, parametre adınızın etrafına kıvırcık ayraçlar {} ekleyerek sorgularda kullanılabilir. Parametreler hakkında daha fazla bilgi [Çalışma Kitapları belgelerinde parametreler hakkında](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Documentation/Parameters/Parameters.md)bulunabilir.

## <a name="updating-queries-with-the-timerange-parameter"></a>Zaman Aralığı parametresi ile sorguları güncelleştirme

### <a name="option-1-select-timerange-from-the-time-range-dropdown"></a>Seçenek 1: Zaman Aralığı açılır tarihinden TimeRange'i seçin

![Zaman Parametresi](media/view-designer-conversion-tasks/time-parameter.png)

### <a name="option-2-update-your-log-queries"></a>Seçenek 2: Günlük sorgularınızı güncelleme

Sorgunuzda satırı ekleyin: `| where TimeGenerated {TimeRange}` aşağıdaki örnekte olduğu gibi:

Özgün sorgu
```KQL
search * 
| summarize count() by Type
```

Güncelleştirilmiş sorgu
```KQL
search * 
| where TimeGenerated {TimeRange} 
| summarize count() by Type
```

## <a name="including-a-list"></a>Liste Dahil
Görünüm tasarımcısı görünümlerinin çoğu bir liste içerir ve bu standart listeyi çalışma kitabında yeniden oluşturabilirsiniz.

![Döşeme listesi](media/view-designer-conversion-tasks/tile-list.png)

Hücre seçeneklerinden sorgu **ekle'yi** tıklatarak görselleştirme ekleyin.

![Param Ekle](media/view-designer-conversion-tasks/add-param.png)

Görünüm tasarımcısı, Özgün örnekteki sözdizimiyle eşleşen varsayılan bir sorgu kullanır. Bu, sorguyu aşağıdaki örnekte olduğu gibi güncelleştirilmiş forma değiştirerek güncellenebilir:

Özgün sorgu
```KQL
search * 
| summarize AggregatedValue = count() by Type
```

Güncelleştirilmiş sorgu
```KQL
search * 
| summarize Count = count() by Type
```

Bu, aşağıdakilere benzer bir liste oluşturur:

![Liste Örneği](media/view-designer-conversion-tasks/list-example.png)

## <a name="enabling-sparklines"></a>Sparklines etkinleştirme
Izgaralar için ortak bir özellik, zaman içinde çeşitli veri desenleri özetlemek için Sparklines eklemektir. Görünüm tasarımcısı, çalışma kitaplarında olduğu gibi tüm listeler için **Sparklines'ı Etkinleştir** özelliğini sunar. Görünüm tasarımcısıyla eşleşen verilerinize Çizgiler eklemek için, aşağıdaki örnekte olduğu gibi verileri özgün sorgunuzla birleştirin:

Özgün sorgu
```KQL
search *
| summarize AggregatedValue = count() by Type) on Type
```

Güncelleştirilmiş sorgu
```KQL
search * 
| summarize AggregatedValue = count() by Type
| join kind = inner (search * 
    | make-series Trend = count() default=0 on TimeGenerated from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain} by Type) on Type
| project Type, AggregatedValue, Trend
```

**Sütun Ayarlarını**seçin.
![Sütun Ayarları](media/view-designer-conversion-tasks/column-settings.png)

Sütun **işleyici** açılır düşüşünü Bir *Kıvılcım alanı*olarak güncelleştirin.
![Sparklines](media/view-designer-conversion-tasks/sparkline.png)

Ayarlarınızı kaydedin ve tablonuzu bir ışıltı içerecek şekilde güncelleştirmek için sorguyu yeniden çalıştırın.

Elde edilen ızgara aşağıdakilere benzer: ![Sparkline örneği](media/view-designer-conversion-tasks/sparkline-example.png)

## <a name="advanced-cell-settings"></a>Gelişmiş hücre ayarları
Görünüm tasarımcısını yansıtmak için, çalışma kitabı hücrelerinin boyutunu değiştirme veya günlüklere pin ve dış bağlantılar ekleme gibi görevleri gerçekleştirebilirsiniz.

Gelişmiş **Ayarlar'a** erişmek için her hücrenin altındaki vites simgesini seçin.

![Gelişmiş ayarlar](media/view-designer-conversion-tasks/advanced-settings.png)

Bu, çeşitli seçenekleriçeren bir menü görüntüler:

![Gelişmiş ayarlar ayarları](media/view-designer-conversion-tasks/advanced-settings-settings.png)

Harici bir sorguya pin ve bağlantı eklemek için ilgili onay kutularını seçin. Hücrenize bir başlık eklemek **için, Grafik başlığı** bölümüne istediğiniz başlığı yazın.

Varsayılan olarak herhangi bir çalışma kitabı hücresi tüm sayfa genişliğini kapacak şekilde ayarlanmıştır, ancak gelişmiş **ayarlar** menüsünün **Stil** sekmesi altında hücreyi ölçekleyerek bunu ayarlayabilirsiniz

![Gelişmiş ayarlar stili](media/view-designer-conversion-tasks/advanced-settings-style.png)

 
## <a name="additional-parameters"></a>Ek parametreler
Çalışma kitabınızda yeni bir parametre oluşturmak için **Parametre Ekle'yi** seçin. 

Abonelik seçmek için, yan menüdeki **Parametre ad** alanına *Abonelik* yazın ve **Parametre türü** açılır menüsünden Abonelik *Seçici'yi* seçin

![Abonelik Menüsü](media/view-designer-conversion-tasks/subscription-filter.png)

Kaynak seçmek için, yan menüdeki **Parametre ad** alanına *Kaynak* yazın ve **Parametre türü** açılır menüsünden Kaynak *Seçici'yi* seçin.

![Kaynak Menüsü](media/view-designer-conversion-tasks/resource-filter.png)

Bu, çeşitli aboneliklerinize ve kaynaklarınıza erişmenize izin vermek için açılır bilgiler ekler.

![Abonelik Kaynak Açılır Bırakma](media/view-designer-conversion-tasks/subscription-resource.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Döşeme dönüşümleri](view-designer-conversion-tiles.md)
