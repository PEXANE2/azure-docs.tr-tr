---
title: Azure Veri Gezgini verilerini sorgulamak ve görselleştirmek için Power BI kullanmak için en iyi uygulamalar
description: Bu makalede, Azure Veri Gezgini verilerini sorgulamak ve görselleştirmek için Power BI'yi kullanmak için en iyi uygulamaları öğrenirsiniz.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/26/2019
ms.openlocfilehash: db1d530c9cab77ae612c83a0d4f52478fb9ee270
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251745"
---
# <a name="best-practices-for-using-power-bi-to-query-and-visualize-azure-data-explorer-data"></a>Azure Veri Gezgini verilerini sorgulamak ve görselleştirmek için Power BI kullanmak için en iyi uygulamalar

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. [Power BI,](https://docs.microsoft.com/power-bi/) verilerinizi görselleştirmenize ve sonuçları kuruluşunuz genelinde paylaşmanıza olanak tanıyan bir iş analitiği çözümüdür. Azure Veri Gezgini, Power BI'deki verilere bağlanmak için üç seçenek sunar. Yerleşik [bağlayıcıyı](power-bi-connector.md)kullanın, [Azure Veri Gezgini'nden Power BI'ye bir sorgu içe aktarın](power-bi-imported-query.md)veya [BIR SQL sorgusu](power-bi-sql-query.md)kullanın. Bu makalede, Power BI ile Azure Veri Gezgini verilerinizi sorgulamak ve görselleştirmek için ipuçları sağlar. 

## <a name="best-practices-for-using-power-bi"></a>Power BI kullanmak için en iyi uygulamalar 

Terabaytlarca taze ham veriyle çalışırken, Power BI panoları ve raporlarını hızlı ve güncel tutmak için aşağıdaki yönergeleri izleyin:

* **Seyahat ışığı** - Yalnızca raporlarınız için ihtiyacınız olan verileri Power BI'ye getirin. Derin etkileşimli analiz için, Kusto Sorgu Dili ile özel keşif için optimize edilmiş [Azure Veri Gezgini Web UI'sini](web-query-data.md) kullanın.

* **Bileşik model** - Üst düzey panolar için toplanan verileri filtrelenmiş operasyonel ham verilerle birleştirmek için [bileşik modeli](https://docs.microsoft.com/power-bi/desktop-composite-models) kullanın. Ham verilerin ne zaman kullanılacağını ve toplu görünümün ne zaman kullanılacağını açıkça tanımlayabilirsiniz. 

* **Alma modu ve DirectQuery modu** - Daha küçük veri kümelerinin etkileşimi için **Alma** modunu kullanın. Büyük, sık güncelleştirilen veri kümeleri için **DirectQuery** modunu kullanın. Örneğin, küçük oldukları ve sık sık değişmedikleri için **Alma** modunu kullanarak boyut tabloları oluşturun. Yenileme aralığını beklenen veri güncelleştirme hızına göre ayarlayın. Bu tablolar büyük olduğundan ve ham veri içerdiğinden **DirectQuery** modunu kullanarak olgu tabloları oluşturun. Power BI [delinme yoluyla](https://docs.microsoft.com/power-bi/desktop-drillthrough)filtrelenmiş verileri sunmak için bu tabloları kullanın.

* **Paralellik** – Azure Veri Gezgini doğrusal ölçeklenebilir bir veri platformudur, bu nedenle uçtan uca akışın paralelliğini aşağıdaki gibi artırarak pano oluşturma performansını artırabilirsiniz:

   * [Power BI'de DirectQuery'deki eşzamanlı bağlantı sayısını artırın.](https://docs.microsoft.com/power-bi/desktop-directquery-about#maximum-number-of-connections-option-for-directquery)

   * [Paralelliği geliştirmek için zayıf tutarlılık](/azure/kusto/concepts/queryconsistency)kullanın. Bu, verilerin tazeliği üzerinde bir etkiye sahip olabilir.

* **Etkili dilimleyiciler** – Hazır olmadan önce raporların veri yüklemesini önlemek için [eşitleyiciler](https://docs.microsoft.com/power-bi/visuals/power-bi-visualization-slicers#sync-and-use-slicers-on-other-pages) kullanın. Veri kümesini yapıladıktan, tüm görselleri yerleştirdikten ve tüm dilimleyicileri işaretledikten sonra, yalnızca gereken verileri yüklemek için eşitleyiciyi seçebilirsiniz.

* **Filtreleri kullanın** - Azure Veri Gezgini aramasını ilgili veri parçalarına odaklamak için mümkün olduğunca çok Power BI filtresi kullanın.

* **Verimli görseller** – Verileriniz için en çok performans gösteren görselleri seçin.

## <a name="tips-for-using-the-azure-data-explorer-connector-for-power-bi-to-query-data"></a>Verileri sorgulamak için Power BI için Azure Veri Gezgini bağlayıcısını kullanma ipuçları

Aşağıdaki bölümde Power BI ile Kusto sorgu dilini kullanmak için ipuçları ve püf noktaları içerir. Verileri görselleştirmek [için Power BI için Azure Veri Gezgini bağlayıcısını](power-bi-connector.md) kullanın

### <a name="complex-queries-in-power-bi"></a>Power BI'deki karmaşık sorgular

Karmaşık sorgular, Kusto'da Güç Sorgusu'ndan daha kolay ifade edilir. [Bunlar Kusto işlevleri](/azure/kusto/query/functions)olarak uygulanmalı ve Power BI'de çağrılmalıdır. Bu yöntem, Kusto `let` sorgunuzdaki ifadeler ile **DirectQuery** kullanırken gereklidir. Power BI iki sorguyu birleştirdiği ve `let` deyimler işleçle birlikte kullanılamadığından `join` sözdizimi hataları oluşabilir. Bu nedenle, birleştirmenin her bir bölümünü Bir Kusto işlevi olarak kaydedin ve Power BI'nin bu iki işlevi biraraya getirmesine izin verin.

### <a name="how-to-simulate-a-relative-date-time-operator"></a>Göreceli bir tarih-saat işleci nasıl simüle edilir?

Power BI gibi `ago()` *göreceli* bir tarih-saat işleci içermez.
Simüle `ago()`etmek için, `DateTime.FixedLocalNow()` `#duration` bir arada kullanın ve Güç BI işlevleri.

`ago()` İşleç kullanarak bu sorgu yerine:

```kusto
    StormEvents | where StartTime > (now()-5d)
    StormEvents | where StartTime > ago(5d)
``` 

Aşağıdaki eşdeğer sorguyu kullanın:

```powerquery-m
let
    Source = Kusto.Contents("help", "Samples", "StormEvents", []),
    #"Filtered Rows" = Table.SelectRows(Source, each [StartTime] > (DateTime.FixedLocalNow()-#duration(5,0,0,0)))
in
    #"Filtered Rows"
```

### <a name="reaching-kusto-query-limits"></a>Kusto sorgu sınırlarına ulaşma 

Kusto sorguları, varsayılan olarak, [sorgu sınırlarında](/azure/kusto/concepts/querylimits)açıklandığı gibi 500.000 satıra veya 64 MB'a kadar geri döner. **Azure Veri Gezgini (Kusto)** bağlantı penceresinde **Gelişmiş seçenekleri** kullanarak bu varsayılanları geçersiz kılabilirsiniz:

![Gelişmiş seçenekler](media/power-bi-best-practices/advanced-options.png)

Varsayılan sorgu sınırlarını değiştirmek için sorgunuzla birlikte bu seçenekler [ayarlı deyimler:](/azure/kusto/query/setstatement)

  * **Limit sorgu sonuç kayıt numarası** oluşturur`set truncationmaxrecords`
  * **Bytes'te sorgu sonuç veri boyutunu sınırlandır**`set truncationmaxsize`
  * **Devre dışı bırakılmaz sonuç kümesi kesilme,**`set notruncation`

### <a name="using-query-parameters"></a>Sorgu parametrelerini kullanma

[Sorgu parametrelerinizi](/azure/kusto/query/queryparametersstatement) dinamik olarak değiştirmek için kullanabilirsiniz. 

#### <a name="using-a-query-parameter-in-the-connection-details"></a>Bağlantı ayrıntılarında sorgu parametresini kullanma

Sorgudaki bilgileri filtrelemek ve sorgu performansını optimize etmek için bir sorgu parametresi kullanın.
 
**Sorguları Edit** penceresinde, **Ana Sayfa** > **Gelişmiş Düzenleyici**

1. Sorgunun aşağıdaki bölümünü bulun:

    ```powerquery-m
    Source = Kusto.Contents("<Cluster>", "<Database>", "<Query>", [])
    ```
   
   Örnek:

    ```powerquery-m
    Source = Kusto.Contents("Help", "Samples", "StormEvents | where State == 'ALABAMA' | take 100", [])
    ```

1. Sorgunun ilgili bölümünü parametrenizle değiştirin. Sorguyu birden çok parçaya bölün ve parametreyle birlikte bir ampersand (&) kullanarak geri dönüştürün.

   Örneğin, `State == 'ALABAMA'` yukarıdaki sorguda, rolü alıp şu şekilde bölüşürüz: `State == '` `'` ve parametreyi `State` aralarında yerleştireceğiz:
   
    ```kusto
    "StormEvents | where State == '" & State & "' | take 100"
    ```

1. Sorgunuz tırnak işaretleri içeriyorsa, bunları doğru şekilde kodlayın. Örneğin, aşağıdaki sorgu: 

   ```kusto
   "StormEvents | where State == "ALABAMA" | take 100" 
   ```

   Advanced **Editor'da** iki tırnak işareti yle aşağıdaki gibi görünür:

   ```kusto
    "StormEvents | where State == ""ALABAMA"" | take 100"
   ```

   Üç tırnak işareti ile aşağıdaki sorgu ile değiştirilmelidir:

   ```kusto
   "StormEvents | where State == """ & State & """ | take 100"
   ```

#### <a name="use-a-query-parameter-in-the-query-steps"></a>Sorgu adımlarında sorgu parametresi kullanma

Sorgu parametresi, onu destekleyen herhangi bir sorgu adımında kullanabilirsiniz. Örneğin, sonuçları bir parametrenin değerine göre filtreleyin.

![bir parametre kullanarak filtre sonuçları](media/power-bi-best-practices/filter-using-parameter.png)

### <a name="dont-use-power-bi-data-refresh-scheduler-to-issue-control-commands-to-kusto"></a>Kusto'ya denetim komutları vermek için Power BI veri yenileme zamanlayıcısı kullanmayın

Power BI, bir veri kaynağına karşı düzenli olarak sorgu düzenleyebilen bir veri yenileme zamanlayıcısı içerir. Bu mekanizma, Denetim komutlarını Kusto'ya zamanlamak için kullanılmamalıdır, çünkü Power BI tüm sorguların salt okunur olduğunu varsayar.

### <a name="power-bi-can-send-only-short-lt2000-characters-queries-to-kusto"></a>Power BI, Kusto'ya yalnızca kısa (2000&lt;karakter) sorgu gönderebilir

Power BI'de bir sorgu çalıştırmak aşağıdaki hatayla sonuçlanırsa: _"DataSource.Error: Web.Contents içeriği alamadı..."_ sorgu büyük olasılıkla 2000 karakterden daha uzundur. Power BI, URI'nin bir parçası olarak sorguyu kodlayan bir HTTP GET isteği yayınlayarak Kusto'yu sorgulamak için **PowerQuery'yi** kullanır. Bu nedenle, Power BI tarafından verilen Kusto sorguları bir istek URI (2000 karakter, eksi küçük ofset) maksimum uzunluğu ile sınırlıdır. Geçici çözüm olarak, Kusto'da [depolanmış](/azure/kusto/query/schema-entities/stored-functions) bir işlev tanımlayabilir ve Power BI'nin sorguda bu işlevi kullanmasını sağlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Power BI için Azure Veri Gezgini bağlayıcısını kullanarak verileri görselleştirin](power-bi-connector.md)




