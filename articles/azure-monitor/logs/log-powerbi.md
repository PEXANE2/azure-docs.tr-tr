---
title: Power BI ve Excel ile Log Analytics tümleştirme
description: Log Analytics sonuçları Power BI 'a gönderme
ms.subservice: logs
ms.topic: conceptual
author: roygalMS
ms.author: roygal
ms.date: 11/03/2020
ms.openlocfilehash: d1db5f5e4164d971dff3ec6b042fa4b0a969e508
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101704251"
---
# <a name="log-analytics-integration-with-power-bi"></a>Power BI ile tümleştirme Log Analytics

Bu makalede, daha görsel açıdan çekici raporlar ve panolar oluşturmak için Log Analytics verileri Microsoft Power BI 'a akışa yönelik yollar ele alınmaktadır. 

## <a name="background"></a>Arka Plan 

Azure Izleyici günlükleri, günlükleri almak için uçtan uca bir çözüm sağlayan bir platformdur. [Azure izleyici Log Analytics](../platform/data-platform.md#) , bu günlükleri sorgulayan arabirimdir. Log Analytics dahil olmak üzere tüm Azure Izleyici veri platformu hakkında daha fazla bilgi için bkz. [Azure izleyici veri platformu](../data-platform.md). 

Microsoft Power BI, Microsoft 'un veri görselleştirme platformudur. Kullanmaya başlama hakkında daha fazla bilgi için bkz. [Power BI giriş sayfası](https://powerbi.microsoft.com/). 


Genel olarak, görsel açıdan çekici raporlar ve panolar bütünleştirmek ve oluşturmak için ücretsiz Power BI özellikleri kullanabilirsiniz.

Daha gelişmiş özellikler, Power BI Pro veya Premium hesabı satın almayı gerektirebilir. Bu özellikler şunları içerir: 
 - işinizi paylaşma 
 - zamanlanan yenilemeler
 - Power BI uygulamaları 
 - veri akışları ve artımlı yenileme 

Daha fazla bilgi için bkz. [Power BI fiyatlandırma ve özellikler hakkında daha fazla bilgi](https://powerbi.microsoft.com/pricing/) 

## <a name="integrating-queries"></a>Sorguları Tümleştirme  

Power BI, ana sorgulama dili olarak bir [a sorgu dili](/powerquery-m/power-query-m-language-specification/) kullanır. 

Log Analytics sorguları, bir e-Power BI doğrudan bir biçimde aktarılabilir ve doğrudan kullanılabilir. Başarılı bir sorgu çalıştırdıktan sonra, Log Analytics UI üst eylem çubuğunda **dışarı aktar** düğmesinden **Power BI dışarı aktar (e sorgusu)** seçeneğini belirleyin.


:::image type="content" source="media/log-powerbi/export-query2.png" alt-text="Dışarı aktarma seçenek menüsü açılan Log Analytics sorgu" border="true":::

Log Analytics, doğrudan Power BI kullanılabilecek olan bir. txt dosyası oluşturur.

## <a name="connecting-your-logs-to-a-dataset"></a>Günlüklerinizi bir veri kümesine bağlama 

Power BI veri kümesi, raporlama ve görselleştirme için hazırlanma verilerinin kaynağıdır. Bir Log Analytics sorgusunu bir veri kümesine bağlamak için, Log Analytics ' dan aktarılmış olan d kodunu Power BI boş bir sorguya kopyalayın. 

Daha fazla bilgi için bkz. [Power BI veri kümelerini anlama](/power-bi/service-datasets-understand/). 

## <a name="collect-data-with-power-bi-dataflows"></a>Power BI veri akışları ile veri toplama 

Power BI veri akışları, verileri toplamanıza ve depolamanıza de olanak tanır. Daha fazla bilgi için bkz. [Power BI veri akışları](/power-bi/service-dataflows-overview).

Veri akışı, verilerinizi toplayıp hazırladıktan yardımcı olmak üzere tasarlanan bir "Cloud ETL" türüdür. Veri kümesi, farklı varlıkları bağlamanıza ve gereksinimlerinize göre modellemeye yardımcı olmak için tasarlanan "modeldir".

## <a name="incremental-refresh"></a>Artımlı yenileme 

Power BI veri kümeleri ve Power BI veri akışları artımlı yenileme seçeneğine sahiptir. Power BI veri akışları ve Power BI veri kümeleri bu özelliği destekler, ancak Power BI Premium kullanmanız gerekir.  


Artımlı yenileme, küçük sorgular çalıştırır ve tüm verileri yeniden almak yerine çalışma başına daha küçük miktarlarda veri ve sorguyu çalıştırdığınızda yeniden güncelleştirilir. Büyük miktarlardaki verileri kaydetme seçeneğiniz vardır ancak sorgu her çalıştırıldığında yeni bir veri artışı ekleyebilirsiniz. Bu davranış, daha uzun çalışan raporlar için idealdir.

Power BI artımlı yenileme, sonuç kümesinde dosyalanmış bir *Tarih* ve varlığına dayanır. Artımlı yenilemeyi yapılandırmadan önce, Log Analytics sorgu sonuç kümesinde en az bir *DateTime* değeri bulunduğundan emin olun. 

Daha fazla bilgi edinmek ve artımlı yenilemeyi yapılandırmak için bkz. [Power BI veri kümeleri ve artımlı yenileme](/power-bi/service-premium-incremental-refresh) ve [Power BI veri akışları ve artımlı yenileme](/power-bi/service-dataflows-incremental-refresh).

## <a name="reports-and-dashboards"></a>Raporlar ve panolar

Verileriniz Power BI gönderildikten sonra, raporlar ve panolar oluşturmak için Power BI kullanmaya devam edebilirsiniz.

Daha fazla bilgi için, [ilk Power BI modelinizi ve raporunuzu oluşturma hakkında bu kılavuza](/learn/modules/build-your-first-power-bi-report/)bakın.  

## <a name="excel-integration"></a>Excel tümleştirmesi

Bir Excel elektronik tablosu ile tümleştirmek için Power BI ' de kullanılan aynı d tümleştirmesini kullanabilirsiniz. Daha fazla bilgi için, [Excel ile tümleştirme hakkında bu kılavuza](https://support.microsoft.com/office/import-data-from-external-data-sources-power-query-be4330b3-5356-486c-a168-b68e9e616f5a) bakın ve Log Analytics 'tan dışarıya aktarılmış olan e sorgusunu yapıştırın.

Ek bilgiler, [tümleştirme Log Analytics ve Excel](log-excel.md) 'de bulunabilir

## <a name="next-steps"></a>Sonraki adımlar

[Log Analytics sorguları](./log-query-overview.md)ile çalışmaya başlayın.