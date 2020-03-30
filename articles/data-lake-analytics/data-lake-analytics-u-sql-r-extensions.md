---
title: Azure Veri Gölü Analitiğinde R ile U-SQL komut dosyalarını genişletme
description: Azure Veri Gölü Analizi'ni kullanarak U-SQL komut dosyasında R kodunu nasıl çalıştırılacakyapılacağını öğrenin. R kodunu satır satıra veya dosyalardan başvuruya gömün.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.topic: conceptual
ms.date: 06/20/2017
ms.openlocfilehash: c5dd3f493e85afc925b639c142a293eed1e8cbd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71672692"
---
# <a name="extend-u-sql-scripts-with-r-code-in-azure-data-lake-analytics"></a>Azure Veri Gölü Analizi'nde R koduyla U-SQL komut dosyalarını genişletme

Aşağıdaki örnekte, R kodunu dağıtmak için temel adımlar gösteriş verilmiştir:
* U-SQL Script için R uzantılarını etkinleştirmek için `REFERENCE ASSEMBLY` deyimi kullanın.
* Giriş `REDUCE` verilerini bir anahtarüzerinde bölmek için işlemi kullanın.
* U-SQL için R uzantıları, indiriciye atanan`Extension.R.Reducer`her tepe noktası üzerinde R kodunu çalıştıran yerleşik bir indirgeci () içerir. 
* U-SQL ve R. `inputFromUSQL` `outputToUSQL` Giriş ve çıkış DataFrame tanımlayıcı adları arasında veri aktarmak için çağrılan ve sırasıyla adlandırılmış veri çerçevelerinin kullanımı sabittir (diğer bir deyişle, kullanıcılar giriş ve çıktı Veri Çerçevesi tanımlayıcılarının bu önceden tanımlanmış adlarını değiştiremez).

## <a name="embedding-r-code-in-the-u-sql-script"></a>U-SQL komut dosyasına R kodunu gömme

U-SQL komut dosyanızın r kodunu n komut parametresini `Extension.R.Reducer`kullanarak satır satıra alabilirsiniz. Örneğin, R komut dosyasını bir dize değişkeni olarak bildirebilir ve ölçere parametre olarak geçirebilirsiniz.


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that the column names are the same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-the-r-code-in-a-separate-file-and-reference-it--the-u-sql-script"></a>R kodunu ayrı bir dosyada tutun ve U-SQL komut dosyasına başvurun

Aşağıdaki örnekte daha karmaşık bir kullanım gösterin. Bu durumda, R kodu U-SQL komut dosyası olan bir KAYNAK olarak dağıtılır.

Bu R kodunu ayrı bir dosya olarak kaydedin.

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

Bu R komut dosyasını DEPLOY RESOURCE deyimiyle dağıtmak için bir U-SQL komut dosyası kullanın.

    REFERENCE ASSEMBLY [ExtR];

    DEPLOY RESOURCE @"/usqlext/samples/R/RinUSQL_PredictUsingLinearModelasDF.R";
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT 
            SepalLength double,
            SepalWidth double,
            PetalLength double,
            PetalWidth double,
            Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT 
            Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput = REDUCE @ExtendedData ON Par
        PRODUCE Par, fit double, lwr double, upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile:"RinUSQL_PredictUsingLinearModelasDF.R", rReturnType:"dataframe", stringsAsFactors:false);
        OUTPUT @RScriptOutput TO @OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a>R, U-SQL ile Nasıl Bütünleşir?

### <a name="datatypes"></a>Veri Tipleri
* U-SQL'den dize ve sayısal sütunlar R DataFrame ve U-SQL arasında `double`olduğu `string` `bool`gibi `integer` `byte`dönüştürülür [desteklenen türler: , , , , ].
* Veri `Factor` türü U-SQL'de desteklenmez.
* `byte[]`base64 kodlanmış `string`olarak serihale edilmelidir.
* U-SQL dizeleri R kodundaki etkenlere dönüştürülebilir, U-SQL R giriş veri çerçevesi oluşturduktan veya indirgeme parametresi `stringsAsFactors: true`ayarlanınca.

### <a name="schemas"></a>Şemalar
* U-SQL veri kümelerinin yinelenen sütun adları olamaz.
* U-SQL veri kümeleri sütun adları dizeleri olmalıdır.
* Sütun adları U-SQL ve R komut dosyalarında aynı olmalıdır.
* Yalnızca okunur sütun çıktı veri çerçevesinin bir parçası olamaz. Çünkü udo çıktı şemasının bir parçasıysa, yalnızca okunan sütunlar U-SQL tablosuna otomatik olarak geri enjekte edilir.

### <a name="functional-limitations"></a>Fonksiyonel sınırlamalar
* R Motoru aynı işlemde iki kez anında olamaz. 
* Şu anda U-SQL, Reducer UDOs kullanılarak oluşturulan bölümlenmiş modelleri kullanarak tahmin için Combiner UDOs'u desteklememektedir. Kullanıcılar bölümlenmiş modelleri kaynak olarak bildirebilir ve R Komut `ExtR_PredictUsingLMRawStringReducer.usql`Dosyası'nda kullanabilir (örnek koda bakınız)

### <a name="r-versions"></a>R Versiyonları
Yalnızca R 3.2.2 desteklenir.

### <a name="standard-r-modules"></a>Standart R modülleri

    base
    boot
    Class
    Cluster
    codetools
    compiler
    datasets
    doParallel
    doRSR
    foreach
    foreign
    Graphics
    grDevices
    grid
    iterators
    KernSmooth
    lattice
    MASS
    Matrix
    Methods
    mgcv
    nlme
    Nnet
    Parallel
    pkgXMLBuilder
    RevoIOQ
    revoIpe
    RevoMods
    RevoPemaR
    RevoRpeConnector
    RevoRsrConnector
    RevoScaleR
    RevoTreeView
    RevoUtils
    RevoUtilsMath
    Rpart
    RUnit
    spatial
    splines
    Stats
    stats4
    survival
    Tcltk
    Tools
    translations
    utils
    XML

### <a name="input-and-output-size-limitations"></a>Giriş ve Çıkış boyutu sınırlamaları
Her tepe noktası, ona atanmış sınırlı miktarda belleğe sahiptir. Giriş ve çıktı DataFrame'lerinin R kodundaki bellekte bulunması gerektiğinden, giriş ve çıktının toplam boyutu 500 MB'ı geçemez.

### <a name="sample-code"></a>Örnek Kod
U-SQL Advanced Analytics uzantılarını yükledikten sonra Data Lake Store hesabınızda daha fazla örnek kod kullanılabilir. Daha fazla örnek kodun `<your_account_address>/usqlext/samples/R`yolu: . 

## <a name="deploying-custom-r-modules-with-u-sql"></a>U-SQL ile Özel R modüllerinin dağıtılması

İlk olarak, bir R özel modülü oluşturun ve zip ve sonra ADL deposuna sıkıştırılmış R özel modül dosyası yükleyin. Örnekte, kullandığımız ADLA hesabı için varsayılan ADLS hesabının köküne magittr_1.5.zip yükleyeceğiz. Modülü ADL deposuna yükledikten sonra, U-SQL komut dosyanızda kullanılabilir hale getirmek `install.packages` için KAYNAĞı KULLANIN'ı kullanın ve yüklemeyi çağırın.

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomPackages.txt";

    // R script to run
    DECLARE @myRScript = @"
    # install the magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load the magrittr package,
    require(magrittr),
    # demonstrate use of the magrittr package,
    2 %>% sqrt
    ";

    @InputData =
    EXTRACT SepalLength double,
    SepalWidth double,
    PetalLength double,
    PetalWidth double,
    Species string
    FROM @IrisData
    USING Extractors.Csv();

    @ExtendedData =
    SELECT 0 AS Par,
    *
    FROM @InputData;

    @RScriptOutput = REDUCE @ExtendedData ON Par
    PRODUCE Par, RowId int, ROutput string
    READONLY Par
    USING new Extension.R.Reducer(command:@myRScript, rReturnType:"charactermatrix");

    OUTPUT @RScriptOutput TO @OutputFileModelSummary USING Outputters.Tsv();

## <a name="next-steps"></a>Sonraki Adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure Veri Gölü Analizi işleri için U-SQL pencere işlevlerini kullanma](data-lake-analytics-use-window-functions.md)
