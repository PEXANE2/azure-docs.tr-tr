---
title: R 'de tahmine dayalı bir model oluşturma ve eğitme
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Azure SQL veritabanı Machine Learning Services (Önizleme) kullanarak R 'de basit bir tahmine dayalı model oluşturun ve ardından yeni verileri kullanarak bir sonuç tahmin edin.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81460163"
---
# <a name="quickstart-create-and-train-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Hızlı başlangıç: Azure SQL veritabanı Machine Learning Services (Önizleme) ile R 'de tahmine dayalı bir model oluşturma ve eğitme

Bu hızlı başlangıçta, R kullanarak tahmine dayalı bir model oluşturup eğitebilirsiniz, modeli veritabanınızdaki bir tabloya kaydedin, ardından Azure SQL veritabanı 'nda Machine Learning Services (R ile) kullanarak yeni verilerden değerleri tahmin etmek için modeli kullanın.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Sunucu düzeyinde güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md) olan BIR [Azure SQL veritabanı](sql-database-single-database-get-started.md)
- R özellikli [Machine Learning Services](sql-database-machine-learning-services-overview.md) .
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

Bu örnek, R 'ye dahil olan **araba veri kümesini** kullanarak bir arabasının durdurma uzaklığını tahmin etmek için basit bir regresyon modeli kullanır.

> [!TIP]
> Birçok veri kümesi R çalışma zamanına dahildir, yüklü veri kümelerinin listesini almak için R komut isteminden yazın `library(help="datasets")` .

## <a name="create-and-train-a-predictive-model"></a>Tahmine dayalı bir model oluşturma ve eğitme

**Araba veri kümesindeki otomobil** hızı verileri, hem **sayısal: hem de** **hızlı**bir şekilde iki sütun içerir. Veriler, farklı hızlarda birden çok durdurma gözlemlerini içerir. Bu verilerden, araba hızı arasındaki ilişkiyi ve bir arabayı durdurmak için gereken mesafeyi açıklayan bir doğrusal regresyon modeli oluşturacaksınız.

Doğrusal modelin gereksinimleri oldukça basittir:
- Bağımlı değişken *hızı* ve bağımsız değişken *uzaklığı*arasındaki ilişkiyi açıklayan bir formül tanımlayın.
- Modelin eğitilmesi için kullanılacak giriş verilerini sağlayın.

> [!TIP]
> Doğrusal modeller üzerinde bir yenileyici gerekiyorsa, rxLinMod: bir modeli, [Doğrusal modellerle sığdırma](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-linear-model) kullanarak bir modele sığdırma işlemini açıklayan bu öğreticiyi deneyin

Aşağıdaki adımlarda eğitim verilerini ayarlayacaksınız, regresyon modeli oluşturacak, eğitim verilerini kullanarak eğtireceğiz ve ardından modeli bir SQL tablosuna kaydetmelisiniz.

1. **SQL Server Management Studio** açın ve SQL veritabanınıza bağlanın.

   Bağlantı için yardıma ihtiyacınız varsa bkz. [hızlı başlangıç: Azure SQL veritabanına bağlanmak ve sorgu sorgulamak için SQL Server Management Studio kullanma](sql-database-connect-query-ssms.md).

1. Eğitim verilerini kaydetmek için **Carspeed** tablosunu oluşturun.

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

1. Kullanarak `rxLinMod`regresyon modeli oluşturun. 

   Modeli oluşturmak için, R kodunun içindeki formülü tanımlar ve ardından eğitim verileri **Carspeed** ' i giriş parametresi olarak geçirin.

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

1. Daha sonra tahmin için kullanabilmeniz için modeli depoladığınız bir tablo oluşturun. 

   Model oluşturan bir R paketinin çıkışı genellikle **ikili bir nesnedir**, bu nedenle tabloda **VARBINARY (max)** türünde bir sütun olmalıdır.

    ```sql
    CREATE TABLE dbo.stopping_distance_models (
        model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(max) NOT NULL
        );
    ```

1. Şimdi saklı yordamı çağırın, modeli oluşturun ve bir tabloya kaydedin.

   ```sql
   INSERT INTO dbo.stopping_distance_models (model)
   EXECUTE generate_linear_model;
   ```

   Bu kodu ikinci kez çalıştırdığınızda şu hatayı alırsınız:

   ```text
   Violation of PRIMARY KEY constraint...Cannot insert duplicate key in object bo.stopping_distance_models
   ```

   Bu hatadan kaçınmak için bir seçenek, her yeni modelin adını güncelleştirmaktır. Örneğin adı daha açıklayıcı bir değer olarak değiştirebilir ve model türü, oluşturduğunuz tarih gibi bilgileri ekleyebilirsiniz.

   ```sql
   UPDATE dbo.stopping_distance_models
   SET model_name = 'rxLinMod ' + FORMAT(GETDATE(), 'yyyy.MM.HH.mm', 'en-gb')
   WHERE model_name = 'default model'
   ```

## <a name="view-the-table-of-coefficients"></a>Katsayıların tablosunu görüntüleme

[sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamından döndürülen R çıkışı genellikle tek bir veri çerçevesiyle sınırlıdır. Ancak veri çerçevesine ek olarak skaler değer gibi diğer türlerdeki çıkışları da döndürebilirsiniz.

Örneğin, bir modeli eğitebilmeniz, ancak modeldeki katsayıların tablosunu hemen görüntülemek istediğinizi varsayalım. Bunu yapmak için, katsayıların bir kısmını ana sonuç kümesi olarak oluşturur ve eğitilen modeli bir SQL değişkeninde çıktı olarak alırsınız. Değişkeni çağırarak modeli hemen yeniden kullanabilirsiniz ya da modeli burada gösterildiği gibi bir tabloya kaydedebilirsiniz.

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

## <a name="score-new-data-using-the-trained-model"></a>Eğitilen modeli kullanarak yeni verileri puan edin

*Puanlama* , veri bilimi ' nde, öngörülere, olasılıklara veya eğitilen bir modele yapılan yeni verileri temel alan diğer değerleri oluşturmak için kullanılan bir terimdir. Önceki bölümde oluşturduğunuz modeli kullanarak yeni verilere yönelik tahmine dayalı puan elde edersiniz.

Özgün eğitim verilerindeki en yüksek hızın saatte 25 mil olduğunu fark ettiniz mi? Bunun nedeni özgün verilerin 1920'de yapılan bir deneyden alınmış olmasıdır! 60 mph veya 100 mph olarak hızlı bir şekilde çalışmaya başlasa da, 1920s 'den nasıl bir otomobil gönderileceğini merak ediyor olabilirsiniz. Bu soruyu yanıtlamak için modelinize bazı yeni hız değerleri sağlayabilirsiniz.

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

2. Bu yeni hız değerlerinden uzaklığı durdurmayı tahmin edin.

   Modeliniz **, geri alma paketinin bir** parçası olarak sunulan **Rxlinmod** algoritmasını temel aldığı için, genel R `predict` işlevi yerine [rxınpredıal](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) işlevini çağırın.

   Bu örnek komut dosyası:
   - Tablodan tek bir model almak için SELECT ifadesini kullanır
   - Bunu bir giriş parametresi olarak geçirir
   - Modeldeki `unserialize` işlevi çağırır
   - Uygun bağımsız `rxPredict` değişkenlerle işlevi modele uygular
   - Yeni giriş verilerini sağlar

   > [!TIP]
   > Gerçek zamanlı Puanlama için bkz. Iptal eden tarafından sunulan [serileştirme işlevleri](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) .

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
> Bu örnek betikte,, `str` R 'den döndürülen verilerin şemasını denetlemek için test aşamasında işlev eklenir. Deyimden daha sonra kaldırabilirsiniz.
>
> R betiğinde kullanılan sütun adlarının, saklı yordam çıkışına geçirilenlerle aynı olması şart değildir. Burada WıTH RESULTS yan tümcesi bazı yeni sütun adlarını tanımlar.

## <a name="next-steps"></a>Sonraki adımlar

R (Önizleme) ile Azure SQL veritabanı Machine Learning Services hakkında daha fazla bilgi için aşağıdaki makalelere bakın.

- [R ile Azure SQL veritabanı Machine Learning Services (Önizleme)](sql-database-machine-learning-services-overview.md)
- [Azure SQL veritabanı 'nda basit R betikleri oluşturma ve çalıştırma Machine Learning Services (Önizleme)](sql-database-quickstart-r-create-script.md)
- [Machine Learning Services kullanarak Azure SQL veritabanı 'nda gelişmiş R işlevleri yazma (Önizleme)](sql-database-machine-learning-services-functions.md)
- [Azure SQL veritabanı 'nda R ve SQL verileriyle çalışma Machine Learning Services (Önizleme)](sql-database-machine-learning-services-data-issues.md)
