---
title: Log Analytics ve Excel 'i tümleştirin
description: Excel 'de Log Analytics bir sorgu alın ve Excel içindeki sonuçları yenileyin.
ms.topic: conceptual
author: roygalMS
ms.author: roygal
ms.date: 11/03/2020
ms.openlocfilehash: f2834e9bd91ecbbf32e0321179c2359862a5b605
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102041119"
---
# <a name="integrate-log-analytics-and-excel"></a>Log Analytics ve Excel 'i tümleştirin

M sorgusunu ve Log Analytics API 'sini kullanarak Azure Izleyici Log Analytics ve Microsoft Excel 'i tümleştirebilirsiniz. Bu tümleştirme, sonuçların toplam hacmi 61MiB aşmadığı sürece Excel 'e en fazla 500.000 kayıt göndermenizi sağlar.

> [!NOTE]
> Excel bir yerel istemci uygulaması olduğundan, yerel donanım ve yazılım sınırlamaları, büyük veri kümelerini işleyebilme performansını ve yeteneğini etkiler.

## <a name="create-your-m-query-in-log-analytics"></a>Log Analytics ' de bir a sorgusu oluşturun 

1. Sorgunuzu, normalde yaptığınız gibi Log Analytics 'te **oluşturun ve çalıştırın** . Kullanıcı arabirimindeki 10.000 kayıt sınırlaması ' ne ulaşılırsa endişelenmeyin.  ' Kullanım ' işlevi veya Kullanıcı arabirimi saat Seçicisi gibi göreli tarihleri kullanmanızı öneririz. bu nedenle Excel, doğru veri kümesini yeniler.
  
2. **Sorguyu dışarı aktar** -sorgu ve sonuçlarından memnun olduktan sonra *dışarı* aktarma menüsündeki **Power BI (e sorgu)** menü seçeneğini Log Analytics kullanarak sorguyu d 'ye dışarı aktarın:

:::image type="content" source="media/log-excel/export-query.png" alt-text="Veri ve dışarı aktarma seçeneğiyle Log Analytics sorgu" border="true":::



Bu seçeneği belirlemek, Excel 'de kullanabileceğiniz bir. txt dosyasını içeren bir. txt dosyası indirir.

Yukarıda gösterilen sorgu aşağıdaki kodu dışarı aktarır. Örneğimizde sorgu için aktarılmış olan bir a koduna örnek aşağıda verilmiştir:

```m
/*
The exported Power Query Formula Language (M Language ) can be used with Power Query in Excel
and Power BI Desktop.
For Power BI Desktop follow the instructions below: 
1) Download Power BI Desktop from https://powerbi.microsoft.com/desktop/
2) In Power BI Desktop select: 'Get Data' -> 'Blank Query'->'Advanced Query Editor'
3) Paste the M Language script into the Advanced Query Editor and select 'Done'
*/

let AnalyticsQuery =
let Source = Json.Document(Web.Contents("https://api.loganalytics.io/v1/workspaces/ddcfc599-cae0-48ee-9026-fffffffffffff/query", 
[Query=[#"query"="

Heartbeat 
| summarize dcount(ComputerIP) by bin(TimeGenerated, 1h)    
| render timechart
",#"x-ms-app"="OmsAnalyticsPBI",#"timespan"="P1D",#"prefer"="ai.response-thinning=true"],Timeout=#duration(0,0,4,0)])),
TypeMap = #table(
{ "AnalyticsTypes", "Type" }, 
{ 
{ "string",   Text.Type },
{ "int",      Int32.Type },
{ "long",     Int64.Type },
{ "real",     Double.Type },
{ "timespan", Duration.Type },
{ "datetime", DateTimeZone.Type },
{ "bool",     Logical.Type },
{ "guid",     Text.Type },
{ "dynamic",  Text.Type }
}),
DataTable = Source[tables]{0},
Columns = Table.FromRecords(DataTable[columns]),
ColumnsWithType = Table.Join(Columns, {"type"}, TypeMap , {"AnalyticsTypes"}),
Rows = Table.FromRows(DataTable[rows], Columns[name]), 
Table = Table.TransformColumnTypes(Rows, Table.ToList(ColumnsWithType, (c) => { c{0}, c{3}}))
in
Table
in AnalyticsQuery
```

## <a name="connect-query-to-excel"></a>Sorguyu Excel 'e bağlama 

Sorguyu içeri aktarmak için. 

1. Microsoft Excel’i açın. 
1. Şeritte, **veri** menüsüne gidin. **Veri al**' ı seçin. **Diğer kaynaklardan** **boş sorgu**' yı seçin:
 
   :::image type="content" source="media/log-excel/excel-import-blank-query.png" alt-text="Excel 'de boş içeri aktarma seçeneği" border="true":::

1. Power Query penceresinde **Gelişmiş Düzenleyici**' yi seçin:

   :::image type="content" source="media/log-excel/advanced-editor.png" alt-text="Excel Gelişmiş sorgu Düzenleyicisi" border="true":::

 
1. Gelişmiş düzenleyicideki metni, Log Analytics ' den içe aktarılmış sorgu ile değiştirin:

   :::image type="content" source="media/log-excel/advanced-editor-2.png" alt-text="Boş bir sorgu oluşturma" border="true":::
 
1. **Bitti**' yi ve ardından **Yükle ve Kapat**' ı seçin. Excel, Log Analytics API 'sini kullanarak sorguyu yürütür ve ardından gösterilen sonuç kümesi.
 

   :::image type="content" source="media/log-excel/excel-query-result.png" alt-text="Excel 'de sorgu sonuçları" border="true":::

> [!Note]
> Kayıt sayısı beklenenden küçükse, sonuçların hacmi 61MiB sınırını aşmayabilir. `project` `project-away` Sütunları, gerek duyduğunuz bir ile sınırlamak için sorgunuzu kullanmayı deneyin.

##  <a name="refreshing--data"></a>Veriler yenileniyor

Verilerinizi doğrudan Excel 'den yenileyebilirsiniz. Excel şeridinde **veri** menüsü grubunda **Yenile** düğmesini seçin.
 
## <a name="next-steps"></a>Sonraki adımlar

Excel 'in dış veri kaynaklarıyla tümleştirmeleri hakkında daha fazla bilgi için bkz. [dış veri kaynaklarından verileri Içeri aktarma (Power Query)](https://support.office.com/article/import-data-from-external-data-sources-power-query-be4330b3-5356-486c-a168-b68e9e616f5a)