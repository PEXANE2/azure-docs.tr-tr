---
title: Azure Izleyici Görünüm Tasarımcısı çalışma kitapları dönüştürme ortak görevler
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 5559dac916262998d621b40757398088ec613609
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77658753"
---
# <a name="view-designer-to-workbooks-conversion-common-tasks"></a>Tasarımcıyı çalışma kitaplarına göre dönüştürme ortak görevleri
[Görünüm Tasarımcısı](view-designer.md) , Log Analytics çalışma alanınızdaki verileri grafiklerle, listelerle ve zaman çizelgeleriyle görselleştirmenize yardımcı olmak üzere özel görünümler oluşturmanıza olanak sağlayan bir Azure izleyici özelliğidir. Bunlar kullanıma alınır ve ek işlevsellik sağlayan çalışma kitapları ile değiştirilmiştir. Bu makalede, görünümler çalışma kitaplarına dönüştürülürken ortak olan görevler ayrıntılı olarak açıklanır.


## <a name="quickstart-with-preset-view-designer-templates"></a>Önceden ayarlanmış görünüm tasarımcı şablonlarıyla hızlı başlangıç

Log Analytics çalışma alanlarındaki çalışma kitaplarında zaten Görünüm Tasarımcısı 'ndaki görünümlerden bazılarını eşleştirmek için oluşturulmuş şablonlar var. **Görünüm Tasarımcısı kılavuzlar** kategorisi altında seçenekleriniz hakkında bilgi edinmek Için **Tasarımcı geçiş kılavuzunu görüntüle** ' yi seçin veya önceden ayarlanmış şablonlardan birini seçin.

![Örnek şablonları](media/view-designer-conversion-tasks/templates.png)

## <a name="enabling-time-range-filter"></a>Zaman aralığı Filtresi etkinleştiriliyor
Görünüm tasarımcısının yerleşik bir varsayılan zaman aralığı filtresi vardır ancak, çalışma kitaplarında bu ayar varsayılan olarak etkinleştirilmemiştir. Çalışma kitapları, kullanıcıların veri günlüklerine daha uygun olabilecek kendi zaman aralığı filtrelerini oluşturmalarına olanak tanır. Filtre oluşturma adımları aşağıda listelenmiştir:

**Parametre Ekle** seçeneğini belirleyin. Varsayılan **Stil** , *Pills*olarak ayarlanır.

![Parametre Ekle](media/view-designer-conversion-tasks/add-param.png)

 **Parametre Ekle** düğmesini seçin.

![Parametre Ekle](media/view-designer-conversion-tasks/add-parameter.png)

Kenar çubuğu menüsünde, **parametre adı** metin kutusuna *timerange*yazın. **Parametre türünü** *zaman aralığı Seçicisi*olarak ayarlayın. **Gerekli?** onay kutusunu seçin.

![Parametre menüsü](media/view-designer-conversion-tasks/parameter-menu.png)

Parametreyi kenar çubuğu menüsünün sol üst köşesine kaydedin. Açılan listeyi varsayılan olarak *ayarı* varsayılan olarak bırakır veya *24 saat*gibi varsayılan bir **timerange** değeri seçebilirsiniz. **Düzenle**seçeneğini belirleyin.

Parametreler, parametre adınızın etrafına {} küme ayraçları ekleyerek sorgularda kullanılabilir. Parametreler hakkında daha fazla ayrıntı, [Parametreler hakkında çalışma kitapları belgelerinde](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Documentation/Parameters/Parameters.md)bulunabilir.

## <a name="updating-queries-with-the-timerange-parameter"></a>TimeRange parametresiyle sorguları güncelleştirme

### <a name="option-1-select-timerange-from-the-time-range-dropdown"></a>Seçenek 1: zaman aralığı açılan menüsünde TimeRange öğesini seçin

![Time parametresi](media/view-designer-conversion-tasks/time-parameter.png)

### <a name="option-2-update-your-log-queries"></a>2\. seçenek: günlük sorgularınızı güncelleştirme

Sorgunuzda aşağıdaki örnekteki satırı: `| where TimeGenerated {TimeRange}` ekleyin:

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

## <a name="including-a-list"></a>Liste ekleme
Görünüm Tasarımcısı görünümlerinin çoğu bir liste içerir ve bu standart listeyi bir çalışma kitabında yeniden oluşturabilirsiniz.

![Kutucuk Listesi](media/view-designer-conversion-tasks/tile-list.png)

Hücre seçeneklerinde **Sorgu Ekle** ' ye tıklayarak bir görselleştirme ekleyin.

![Parametre Ekle](media/view-designer-conversion-tasks/add-param.png)

Görünüm Tasarımcısı, özgün örnekteki sözdizimiyle eşleşen varsayılan bir sorgu kullanır. Bu, aşağıdaki örnekte olduğu gibi sorgu güncelleştirilmiş formla değiştirilerek güncelleştirilemeyebilir:

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

Bu, şuna benzer bir liste oluşturur:

![Liste örneği](media/view-designer-conversion-tasks/list-example.png)

## <a name="enabling-sparklines"></a>Mini grafikleri etkinleştirme
Kılavuzlar için ortak bir özellik, zaman içinde çeşitli veri düzenlerini özetlemek üzere mini grafikler eklemektir. Görünüm Tasarımcısı, çalışma kitapları gibi tüm listeler için **mini grafikleri etkinleştir** özelliğini sunar. Verilerinize, görünüm tasarımcısına uyan mini grafikler eklemek için aşağıdaki örnekte gösterildiği gibi verileri özgün Sorgunuzla birleştirin:

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

**Sütun ayarlarını**seçin.
![sütun ayarları](media/view-designer-conversion-tasks/column-settings.png)

**Sütun Oluşturucu** açılan listesini *Spark alanı*olarak güncelleştirin.
![mini grafikler](media/view-designer-conversion-tasks/sparkline.png)

Tablonuzu bir mini grafik içerecek şekilde güncelleştirmek için ayarları kaydedin ve sorguyu yeniden çalıştırın.

Elde edilen kılavuz şuna benzer: ![mini grafik örneği](media/view-designer-conversion-tasks/sparkline-example.png)

## <a name="advanced-cell-settings"></a>Gelişmiş hücre ayarları
Görünüm tasarımcısını yansıtmak için, çalışma kitabı hücrelerinin boyutunu değiştirme veya PIN 'ler ile dış bağlantıları günlüğe ekleme gibi görevleri gerçekleştirebilirsiniz.

**Gelişmiş ayarlara** erişmek için, her hücrenin altındaki dişli simgesini seçin.

![Gelişmiş ayarlar](media/view-designer-conversion-tasks/advanced-settings.png)

Bu, çeşitli seçeneklerle bir menü görüntüler:

![Gelişmiş ayarlar ayarları](media/view-designer-conversion-tasks/advanced-settings-settings.png)

Bir dış sorguya bir PIN ve bağlantı eklemek için ilgili onay kutularını seçin. Hücreye bir başlık eklemek için, istediğiniz başlığı **grafik başlığı** bölümüne yazın.

Varsayılan olarak, tüm çalışma kitapları hücresi sayfa genişliğinin tamamını alacak şekilde ayarlanır, ancak bunu, **Gelişmiş ayarlar** menüsünün **Stil** sekmesi altında hücreyi aşağı ölçeklendirerek yapabilirsiniz.

![Gelişmiş ayarlar stili](media/view-designer-conversion-tasks/advanced-settings-style.png)

 
## <a name="additional-parameters"></a>Ek parametreler
Çalışma kitabınızda yeni bir parametre oluşturmak için **parametre Ekle** ' yi seçin. 

Bir abonelik seçmek için, yan menüdeki **parametre adı** alanına *abonelik* yazın ve **parametre türü** açılan listesinden *abonelik seçici* ' yi seçin.

![Abonelik menüsü](media/view-designer-conversion-tasks/subscription-filter.png)

Kaynak seçmek için, yan menüdeki **parametre adı** alanına *kaynak* yazın ve **parametre türü** açılan listesinden *kaynak seçici* ' yi seçin.

![Kaynak menüsü](media/view-designer-conversion-tasks/resource-filter.png)

Bu, çeşitli aboneliklerinize ve kaynaklarınıza erişmenizi sağlamak için açılan listeleri ekleyecektir.

![Abonelik kaynak açılan kutusu](media/view-designer-conversion-tasks/subscription-resource.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Kutucuk dönüştürmeleri](view-designer-conversion-tiles.md)
