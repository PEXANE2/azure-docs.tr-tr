---
title: R'de tahmine dayalı bir model oluşturma ve eğitme
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme) kullanarak R'de basit bir tahmin modeli oluşturun ve ardından yeni veriler kullanarak bir sonucu tahmin edin.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 3c88bdf141e7784837a89c8104574d97c93296dc
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460163"
---
# <a name="quickstart-create-and-train-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Hızlı başlangıç: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de tahmine dayalı bir model oluşturma ve bunları eğitme (önizleme)

Bu hızlı başlangıçta, R kullanarak tahmine dayalı bir model oluşturur ve eğitirsiniz, modeli veritabanınızdaki bir tabloya kaydedin, ardından Azure SQL Veritabanında Machine Learning Services (R ile) kullanarak yeni verilerden gelen değerleri tahmin etmek için modeli kullanırsınız.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- Sunucu düzeyinde güvenlik [duvarı kuralına](sql-database-server-level-firewall-rule.md) sahip bir [Azure SQL veritabanı](sql-database-single-database-get-started.md)
- R özellikli [Makine Öğrenimi Hizmetleri](sql-database-machine-learning-services-overview.md) etkindir.
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

Bu örnek, R ile birlikte verilen **araç** veri setini kullanarak bir arabanın durma mesafesini tahmin etmek için basit bir regresyon modeli kullanır.

> [!TIP]
> Yüklü veri kümelerinin listesini almak için R çalışma süresine, R `library(help="datasets")` komut isteminden yazın.

## <a name="create-and-train-a-predictive-model"></a>Tahmine dayalı bir model oluşturma ve eğitme

Araç veri kümesindeki **araç** hız verileri iki sütun içerir, hem sayısal: **dist** ve **hız.** Veriler, farklı hızlarda birden çok durdurma gözlemi içerir. Bu verilerden, araç hızı ile bir otomobili durdurmak için gereken mesafe arasındaki ilişkiyi açıklayan doğrusal bir regresyon modeli oluşturursunuz.

Doğrusal modelin gereksinimleri oldukça basittir:
- Bağımlı değişken *hızı* ile bağımsız değişken *uzaklığı*arasındaki ilişkiyi açıklayan bir formül tanımlayın.
- Modelin eğitilmesi için kullanılacak giriş verilerini sağlayın.

> [!TIP]
> Doğrusal modellerde bir tazelemeye ihtiyacınız varsa, rxLinMod: [Fitting Lineer Modelleri](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-linear-model) kullanarak bir modele uyum sürecini açıklayan bu öğreticiyi deneyin

Aşağıdaki adımlarda eğitim verilerini ayarlarsınız, bir regresyon modeli oluşturursunuz, eğitim verilerini kullanarak çalışırsınız ve modeli BIR SQL tablosuna kaydedebilirsiniz.

1. **SQL Server Management Studio'nuzu** açın ve SQL veritabanınıza bağlanın.

   Bağlanmak için yardıma ihtiyacınız varsa, [Azure SQL veritabanını bağlamak ve sorgulamak için Hızlı Başlangıç: SQL Server Management Studio'yı kullanın.](sql-database-connect-query-ssms.md)

1. Eğitim verilerini kaydetmek için **CarSpeed** tablosunu oluşturun.

    ```sql
    CREATE TABLE dbo.CarSpeed (
        speed INT NOT NULL
        , distance INT NOT NULL
        )
    GO
    
    INSERT INTO dbo.CarSpeed (
        speed
        , distance
        )
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'car_speed <- cars;'
        , @input_data_1 = N''
        , @output_data_1_name = N'car_speed'
    GO
    ```

1. `rxLinMod`Kullanarak bir regresyon modeli oluşturun. 

   Modeli oluşturmak için Formülü R kodunun içindeki olarak tanımlarsınız ve ardından eğitim verileri **CarSpeed'i** giriş parametresi olarak geçirirsiniz.

    ```sql
    DROP PROCEDURE IF EXISTS generate_linear_model;
    GO
    CREATE PROCEDURE generate_linear_model
    AS
    BEGIN
      EXECUTE sp_execute_external_script
      @language = N'R'
      , @script = N'
    lrmodel <- rxLinMod(formula = distance ~ speed, data = CarsData);
    trained_model <- data.frame(payload = as.raw(serialize(lrmodel, connection=NULL)));
    '
      , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
      , @input_data_1_name = N'CarsData'
      , @output_data_1_name = N'trained_model'
      WITH RESULT SETS ((model VARBINARY(max)));
    END;
    GO
    ```

     rxLinMod için ilk bağımsız değişken, mesafeyi hıza bağımlı olan bir değer olarak tanımlayan *formula* parametresidir. Giriş verileri SQL sorgusu tarafından doldurulan `CarsData` değişkeninde depolanır.

1. Modeli daha sonra tahmin için kullanabilmeniz için depoladığınız bir tablo oluşturun. 

   Bir model oluşturan bir R paketinin çıktısı genellikle ikili bir **nesnedir,** bu nedenle tablonun **VARBINARY(max)** türünde bir sütunu olmalıdır.

    ```sql
    CREATE TABLE dbo.stopping_distance_models (
        model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(max) NOT NULL
        );
    ```

1. Şimdi depolanan yordamı arayın, modeli oluşturun ve bir tabloya kaydedin.

   ```sql
   INSERT INTO dbo.stopping_distance_models (model)
   EXECUTE generate_linear_model;
   ```

   Bu kodu ikinci kez çalıştırdığınızda şu hatayı alırsınız:

   ```text
   Violation of PRIMARY KEY constraint...Cannot insert duplicate key in object bo.stopping_distance_models
   ```

   Bu hatayı önlemek için bir seçenek her yeni model için adını güncelleştirmektir. Örneğin adı daha açıklayıcı bir değer olarak değiştirebilir ve model türü, oluşturduğunuz tarih gibi bilgileri ekleyebilirsiniz.

   ```sql
   UPDATE dbo.stopping_distance_models
   SET model_name = 'rxLinMod ' + FORMAT(GETDATE(), 'yyyy.MM.HH.mm', 'en-gb')
   WHERE model_name = 'default model'
   ```

## <a name="view-the-table-of-coefficients"></a>Katsayılar tablosunu görüntüleme

[sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamından döndürülen R çıkışı genellikle tek bir veri çerçevesiyle sınırlıdır. Ancak veri çerçevesine ek olarak skaler değer gibi diğer türlerdeki çıkışları da döndürebilirsiniz.

Örneğin, bir model eğitmek istediğinizi ancak modeldeki katsayılar tablosunu hemen görüntülemek istediğinizi varsayalım. Bunu yapmak için, ana sonuç kümesi olarak katsayılar tablosunu oluşturur ve bir SQL değişkeninde eğitilen modeli çıktınız. Değişkeni arayarak modeli hemen yeniden kullanabilir veya modeli burada gösterildiği gibi bir tabloya kaydedebilirsiniz.

```sql
DECLARE @model VARBINARY(max)
    , @modelname VARCHAR(30)

EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
speedmodel <- rxLinMod(distance ~ speed, CarsData)
modelbin <- serialize(speedmodel, NULL)
OutputDataSet <- data.frame(coefficients(speedmodel));
'
    , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
    , @input_data_1_name = N'CarsData'
    , @params = N'@modelbin varbinary(max) OUTPUT'
    , @modelbin = @model OUTPUT
WITH RESULT SETS(([Coefficient] FLOAT NOT NULL));

-- Save the generated model
INSERT INTO dbo.stopping_distance_models (
    model_name
    , model
    )
VALUES (
    'latest model'
    , @model
    )
```

**Sonuçlar**

![Ek çıktıya sahip eğitilen model](./media/sql-database-quickstart-r-train-score-model/r-train-model-with-additional-output.png)

## <a name="score-new-data-using-the-trained-model"></a>Eğitilmiş modeli kullanarak yeni veriler elde edin

*Puanlama,* veri biliminde, eğitilmiş bir modele beslenen yeni verilere dayalı öngörüler, olasılıklar veya diğer değerler oluşturmak anlamına gelmek için kullanılan bir terimdir. Önceki bölümde oluşturduğunuz modeli, yeni verilere karşı tahmin puanları elde etmek için kullanırsınız.

Özgün eğitim verilerindeki en yüksek hızın saatte 25 mil olduğunu fark ettiniz mi? Bunun nedeni özgün verilerin 1920'de yapılan bir deneyden alınmış olmasıdır! Merak edebilirsiniz, 1920'lerden kalma bir otomobilin durması ne kadar sürerdi? Bu soruyu yanıtlamak için, modelinize bazı yeni hız değerleri sağlayabilirsiniz.

1. Yeni hız verileri içeren bir tablo oluşturun.

   ```sql
    CREATE TABLE dbo.NewCarSpeed (
        speed INT NOT NULL
        , distance INT NULL
        )
    GO
    
    INSERT dbo.NewCarSpeed (speed)
    VALUES (40)
        , (50)
        , (60)
        , (70)
        , (80)
        , (90)
        , (100)
   ```

2. Bu yeni hız değerlerinden durma mesafelerini tahmin edin.

   Modeliniz **RevoScaleR** paketinin bir parçası olarak sağlanan **rxLinMod** algoritmasını temel aldığından, genel R `predict` işlevi yerine [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) işlevini çağırırsınız.

   Bu örnek komut dosyası:
   - Tablodan tek bir model almak için SELECT deyimi kullanır
   - Giriş parametresi olarak geçer
   - Modeldeki `unserialize` işlevi çağırır
   - İşlev, `rxPredict` modele uygun bağımsız değişkenlerle uygular
   - Yeni giriş verilerini sağlar

   > [!TIP]
   > Gerçek zamanlı puanlama için RevoScaleR tarafından sağlanan [Serileştirme işlevlerine](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) bakın.

   ```sql
    DECLARE @speedmodel VARBINARY(max) = (
            SELECT model
            FROM dbo.stopping_distance_models
            WHERE model_name = 'latest model'
            );
    
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    current_model <- unserialize(as.raw(speedmodel));
    new <- data.frame(NewCarData);
    predicted.distance <- rxPredict(current_model, new);
    str(predicted.distance);
    OutputDataSet <- cbind(new, ceiling(predicted.distance));
    '
        , @input_data_1 = N'SELECT speed FROM [dbo].[NewCarSpeed]'
        , @input_data_1_name = N'NewCarData'
        , @params = N'@speedmodel varbinary(max)'
        , @speedmodel = @speedmodel
    WITH RESULT SETS((
                new_speed INT
                , predicted_distance INT
                ));
   ```

   **Sonuçlar**

   ![Durdurma mesafesini tahmin etmek için sonuç kümesi](./media/sql-database-quickstart-r-train-score-model/r-predict-stopping-distance-resultset.png)

> [!NOTE]
> Bu örnek komut `str` dosyasında, R'den döndürülen verilerin şemasını denetlemek için sınama aşamasında işlev eklenir. İfadeyi daha sonra kaldırabilirsiniz.
>
> R betiğinde kullanılan sütun adlarının, saklı yordam çıkışına geçirilenlerle aynı olması şart değildir. Burada WITH RESULTS yan tümcesi bazı yeni sütun adlarını tanımlar.

## <a name="next-steps"></a>Sonraki adımlar

R ile Azure SQL Veritabanı Makine Öğrenme Hizmetleri hakkında daha fazla bilgi için (önizleme), aşağıdaki makalelere bakın.

- [R ile Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme)](sql-database-machine-learning-services-overview.md)
- [Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde basit R komut dosyaları oluşturun ve çalıştırın (önizleme)](sql-database-quickstart-r-create-script.md)
- [Machine Learning Services 'i kullanarak Azure SQL Veritabanı'na gelişmiş R işlevleri yazın (önizleme)](sql-database-machine-learning-services-functions.md)
- [Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde R ve SQL verileriyle çalışma (önizleme)](sql-database-machine-learning-services-data-issues.md)
