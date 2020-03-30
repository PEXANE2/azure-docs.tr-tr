---
title: Azure Veri Gölü Analytics'te U-SQL dili yle başlayın
description: Azure Veri Gölü Analitiği'nde U-SQL dilinin temellerini öğrenin. Dosyalardan gelen ekstra verileri kullanmak, satır kümesini dönüştürmek ve verileri toplamak için ilk sorgunuzu değişkenleri kullanarak yazın.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/23/2017
ms.openlocfilehash: 8130679dcc519cecd25abf43902c003ad8047df3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71672831"
---
# <a name="get-started-with-u-sql-in-azure-data-lake-analytics"></a>Azure Veri Gölü Analizi'nde U-SQL ile başlayın
U-SQL, verileri herhangi bir ölçekte işlemenizi sağlamak için bildirimsel SQL ile zorunlu C# ile birleştiren bir dildir. U-SQL'in ölçeklenebilir, dağıtılmış sorgu özelliği sayesinde, Verileri Azure SQL Veritabanı gibi ilişkisel mağazalar arasında verimli bir şekilde çözümleyebilirsiniz. U-SQL ile, okunan şema uygulayarak ve özel mantık ve UDF'ler ekleyerek yapılandırılmamış verileri işleyebilirsiniz. Ayrıca, U-SQL ölçekte yürütme konusunda ince taneli denetim sağlayan genişletilebilirlik içerir. 

## <a name="learning-resources"></a>Öğrenme kaynakları

* [U-SQL Öğretici, U-SQL](https://aka.ms/usqltutorial) dilinin çoğunun kılavuzlu bir geçişini sağlar. Bu belge, U-SQL öğrenmek isteyen tüm geliştiriciler için okunması önerilir.
* **U-SQL dil sözdizimi**hakkında ayrıntılı bilgi için [U-SQL Dil Başvurusu'na](https://docs.microsoft.com/u-sql/)bakın.
* **U-SQL tasarım felsefesini**anlamak için Visual Studio blog yazısı na bakın [U-SQL Tanıtılması – Büyük Veri İşlemeyi Kolaylaştıran Bir Dil](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

## <a name="prerequisites"></a>Ön koşullar

Bu belgedeki U-SQL örneklerini gözden geçirmeden önce, Öğreticiyi okuyun ve [tamamlayın: Visual Studio için Data Lake Tools'u kullanarak U-SQL komut dosyaları geliştirin.](data-lake-analytics-data-lake-tools-get-started.md) Bu öğretici, Visual Studio için Azure Data Lake Tools ile U-SQL kullanmanın mekaniğini açıklar.

## <a name="your-first-u-sql-script"></a>İlk U-SQL betiğiniz

Aşağıdaki U-SQL komut dosyası basittir ve U-SQL dilini birçok açıdan keşfetmemizi sağlar.

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog   
    TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

Bu komut dosyasının herhangi bir dönüşüm adımı yok. Bu adlı `SearchLog.tsv`kaynak dosyadan okur , şematize, ve searchlog-first-u-sql.csv adlı bir dosyaya rowset geri yazar.

Alandaki veri türünün yanındaki soru `Duration` işaretine dikkat edin. `Duration` Bu, alanın hükümsüz olabileceği anlamına geliyor.

### <a name="key-concepts"></a>Önemli kavramlar
* **Rowset değişkenleri**: Bir satır kümesi oluşturan her sorgu ifadesi bir değişkene atanabilir. U-SQL komut dosyasında T-SQL`@searchlog`değişken adlandırma deseni (örneğin) izler.
* **EXTRACT** anahtar kelimesi bir dosyadaki verileri okur ve okuma daki şemı tanımlar. `Extractors.Tsv`sekme-ayrılmış değer dosyaları için yerleşik bir U-SQL çıkarıcıdır. Özel çıkarıcılar geliştirebilirsiniz.
* **OUTPUT,** bir satır kümesinden dosyaya veri yazar. `Outputters.Csv()`virgülle ayrılmış değer dosyası oluşturmak için yerleşik bir U-SQL çıktısıdır. Özel çıktılar geliştirebilirsiniz.

### <a name="file-paths"></a>Dosya yolları

EXTRACT ve OUTPUT deyimleri dosya yollarını kullanır. Dosya yolları mutlak veya göreceli olabilir:

Bu aşağıdaki mutlak dosya yolu, `mystore`Veri Gölü Deposu'ndaki bir dosyayı ifade eder:

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

Bu aşağıdaki dosya `"/"`yolu ile başlar. Varsayılan Data Lake Store hesabındaki bir dosyayı ifade eder:

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a>Skaler değişkenleri kullanma

Komut dosyası bakımınızı kolaylaştırmak için skaler değişkenleri de kullanabilirsiniz. Önceki U-SQL komut dosyası da şu şekilde yazılabilir:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Sıra kümelerini dönüştürme

Sıra kümelerini dönüştürmek için **SELECT'i** kullanın:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

WHERE yan tümcesi [C# Boolean ifadesini](/dotnet/csharp/language-reference/operators/index)kullanır. Kendi ifadelerinizi ve işlevlerinizi yapmak için C# ifade dilini kullanabilirsiniz. Hatta mantıksal bağlaçlar (ANDs) ve disjunctions (ORs) ile birleştirerek daha karmaşık filtreleme gerçekleştirebilirsiniz.

Aşağıdaki komut dosyası DateTime.Parse() yöntemini ve bağlaçları kullanır.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 >İkinci sorgu, iki filtrenin bir bileşimini oluşturan ilk satır kümesinin sonucu üzerinde çalışıyor. Değişken adını yeniden kullanabilirsiniz ve adlar sözlü olarak kapsamlıdır.

## <a name="aggregate-rowsets"></a>Toplam sıra kümeleri
U-SQL size tanıdık ORDER BY, GROUP BY ve toplamaları verir.

Aşağıdaki sorgu bölge başına toplam süreyi bulur ve ardından sırayla en iyi beş süreyi görüntüler.

U-SQL rowsets bir sonraki sorgu için kendi sırasını korumaz. Bu nedenle, bir çıktı sipariş etmek için, OUTPUT deyimine ORDER BY eklemeniz gerekir:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U-SQL ORDER BY yan tümcesi SELECT ifadesinde FETCH yan tümcesini kullanmayı gerektirir.

U-SQL HAVING yan tümcesi, çıktıyı HAVING koşulunu karşılayan gruplarla sınırlamak için kullanılabilir:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
        GROUP BY Region
        HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Gelişmiş toplama senaryoları [için, toplama, analitik ve başvuru işlevleri](/u-sql/built-in-functions) için U-SQL başvuru belgelerine bakın

## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
