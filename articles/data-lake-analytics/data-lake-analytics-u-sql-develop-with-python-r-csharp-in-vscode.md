---
title: Python, R ve C# işlerinde U-SQL işlerini çalıştırma - Azure Veri Gölü Analitiği
description: Azure Veri Gölü'nde iş göndermek için Python, R ve C# ile kodu niçin kullanacağınızı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 11/22/2017
ms.openlocfilehash: cb3ddf0c4147fa982e8ab0f9d440292d12803d35
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71309702"
---
# <a name="develop-u-sql-with-python-r-and-c-for-azure-data-lake-analytics-in-visual-studio-code"></a>Visual Studio Code'da Azure Veri Gölü Analitiği için Python, R ve C# ile U-SQL geliştirme
U-SQL ile Python, R ve C# kodunu yazmak ve işleri Azure Veri Gölü hizmetine göndermek için Visual Studio Code'u (VSCode) nasıl kullanacağınızı öğrenin. VSCode için Azure Veri Göl Araçları hakkında daha fazla bilgi için [bkz.](data-lake-analytics-data-lake-tools-for-vscode.md)

Kod arkası özel kodu yazmadan önce VSCode'da bir klasör veya çalışma alanı açmanız gerekir.


## <a name="prerequisites-for-python-and-r"></a>Python ve R için Ön Koşullar
ADL hesabınız için Python ve R uzantıları derlemelerini kaydedin. 
1. Hesabınızı portalda açın.
   - **Genel Bakış**’ı seçin. 
   - **Örnek Komut Dosyası'nı**tıklatın.
2. **Daha fazla**'ya tıklayın.
3. **U-SQL Uzantılarını Yükle'yi**seçin. 
4. U-SQL uzantıları yüklendikten sonra onay iletisi görüntülenir. 

   ![Python ve R için ortam ayarlama](./media/data-lake-analytics-data-lake-tools-for-vscode/setup-the-enrionment-for-python-and-r.png)

   > [!Note]
   > Python ve R dil hizmeti ile ilgili en iyi deneyimler için lütfen VSCode Python ve R uzantısını yükleyin. 

## <a name="develop-python-file"></a>Python dosyasini geliştirme
1. Çalışma alanınızda **Yeni Dosya'yı** tıklatın.
2. Kodunuzu U-SQL'e yazın. Aşağıda bir kod örneği verem.
    ```U-SQL
    REFERENCE ASSEMBLY [ExtPython];
    @t  = 
        SELECT * FROM 
        (VALUES
            ("D1","T1","A1","@foo Hello World @bar"),
            ("D2","T2","A2","@baz Hello World @beer")
        ) AS 
            D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer("pythonSample.usql.py", pyVersion : "3.5.1");

    OUTPUT @m
        TO "/tweetmentions.csv"
        USING Outputters.Csv();
    ```
    
3. Komut dosyası dosyası dosyasına sağ tıklayın ve ardından **ADL'yi seçin: Dosya Arkasında Python Kodu Oluştur'** seçeneğini belirleyin. 
4. **xxx.usql.py** dosyası çalışma klasörünüzde oluşturulur. Kodunuzu Python dosyasına yazın. Aşağıda bir kod örneği verem.

    ```Python
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ```
5. **USQL** dosyasına sağ tıklayın, **Komut Dosyasını Derle'yi** veya **İşi** Çalıştıran Işe Gönder'i tıklatabilirsiniz.

## <a name="develop-r-file"></a>R dosyasi geliştir
1. Çalışma alanınızda **Yeni Dosya'yı** tıklatın.
2. Kodunuzu U-SQL dosyasına yazın. Aşağıda bir kod örneği verem.
    ```U-SQL
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT SepalLength double,
                SepalWidth double,
                PetalLength double,
                PetalWidth double,
                Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput =
        REDUCE @ExtendedData
        ON Par
        PRODUCE Par,
                fit double,
                lwr double,
                upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile : "RClusterRun.usql.R", rReturnType : "dataframe", stringsAsFactors : false);
    OUTPUT @RScriptOutput
    TO @OutputFilePredictions
    USING Outputters.Tsv();
    ```
3. **USQL** dosyasına sağ tıklayın ve ardından **ADL'yi seçin: Dosya Arkasında R Kodu Oluştur'** seçeneğini belirleyin. 
4. **xxx.usql.r** dosyası çalışma klasörünüzde oluşturulur. Kodunuzu R dosyasına yazın. Aşağıda bir kod örneği verem.

    ```R
    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence"))
    ```
5. **USQL** dosyasına sağ tıklayın, **Komut Dosyasını Derle'yi** veya **İşi** Çalıştıran Işe Gönder'i tıklatabilirsiniz.

## <a name="develop-c-file"></a>C# dosyasi geliştir
Kod arkası dosyası, tek bir U-SQL komut dosyasıyla ilişkili bir C# dosyasıdır. Kod arkası dosyasında UDO, UDA, UDT ve UDF'ye adanmış bir komut dosyası tanımlayabilirsiniz. UDO, UDA, UDT ve UDF, önce derlemekaydedilmeden doğrudan komut dosyasında kullanılabilir. Kod arkası dosyası, bakan U-SQL komut dosyasıyla aynı klasöre konur. Komut dosyası xxx.usql adlı ise, kod arkası xxx.usql.cs olarak adlandırılır. Kod arkası dosyasını el ile silerseniz, kod arkası özelliği ilişkili U-SQL komut dosyası için devre dışı bırakılır. U-SQL komut dosyası için müşteri kodu yazma hakkında daha fazla bilgi için [U-SQL: Kullanıcı Tanımlı Fonksiyonlar'da Özel Kod Yazma ve Kullanma]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/)bilgisine bakın.

1. Çalışma alanınızda **Yeni Dosya'yı** tıklatın.
2. Kodunuzu U-SQL dosyasına yazın. Aşağıda bir kod örneği verem.
    ```U-SQL
    @a = 
        EXTRACT 
            Iid int,
        Starts DateTime,
        Region string,
        Query string,
        DwellTime int,
        Results string,
        ClickedUrls string 
        FROM @"/Samples/Data/SearchLog.tsv" 
        USING Extractors.Tsv();

    @d =
        SELECT DISTINCT Region 
        FROM @a;

    @d1 = 
        PROCESS @d
        PRODUCE 
            Region string,
        Mkt string
        USING new USQLApplication_codebehind.MyProcessor();

    OUTPUT @d1 
        TO @"/output/SearchLogtest.txt" 
        USING Outputters.Tsv();
    ```
3. **USQL** dosyasına sağ tıklayın ve ardından **ADL'yi seçin: Dosya Arkasında CS Kodu Oluştur.** 
4. **xxx.usql.cs** dosyası çalışma klasörünüzde oluşturulur. Kodunuzu CS dosyasına yazın. Aşağıda bir kod örneği verem.

    ```CS
    namespace USQLApplication_codebehind
    {
        [SqlUserDefinedProcessor]

        public class MyProcessor : IProcessor
        {
            public override IRow Process(IRow input, IUpdatableRow output)
            {
                output.Set(0, input.Get<string>(0));
                output.Set(1, input.Get<string>(0));
                return output.AsReadOnly();
            } 
        }
    }
    ```
5. **USQL** dosyasına sağ tıklayın, **Komut Dosyasını Derle'yi** veya **İşi** Çalıştıran Işe Gönder'i tıklatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Visual Studio Code için Azure Data Lake Araçları’nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md)
* [Visual Studio Code ile U-SQL yerel çalıştırma ve yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md)
* [PowerShell'i kullanarak Data Lake Analytics ile başlayın](data-lake-analytics-get-started-powershell.md)
* [Azure portalını kullanarak Data Lake Analytics ile başlayın](data-lake-analytics-get-started-portal.md)
* [U-SQL uygulamaları geliştirmek için Visual Studio için Veri Gölü Araçlarını Kullanma](data-lake-analytics-data-lake-tools-get-started.md)
* [Veri Gölü Analizi(U-SQL) kataloğunu kullanma](data-lake-analytics-use-u-sql-catalog.md)
