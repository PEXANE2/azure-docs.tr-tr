---
title: "Öğretici: R'de tahmine dayalı bir model dağıtma"
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu üç bölümlük öğreticinin üçüncü bölümünde, Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme) ile R'de tahmine dayalı bir model dağıtırsınız.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 07/26/2019
ROBOTS: NOINDEX
ms.openlocfilehash: efcb73866c83dcc03b5db8b7b97f438fb3010511
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81452817"
---
# <a name="tutorial-deploy-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de tahmine dayalı bir model dağıtma (önizleme)

Bu üç bölümlük öğreticinin üçüncü bölümünde, Azure SQL Veritabanı Makine Öğrenme Hizmetleri 'ni (önizleme) kullanarak R'de geliştirilen bir tahminmodelisql veritabanına dağıtırsınız.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

Modeli kullanarak öngörüler yapan katıştırılmış Bir R komut dosyası ile depolanmış bir yordam oluşturursunuz. Modeliniz Azure SQL veritabanında yürütüldeğinden, veritabanında depolanan verilere karşı kolayca eğitilebilir.

Bu makalede, birinci ve ikinci bölümlerde geliştirdiğiniz R komut dosyalarını kullanarak şunları öğreneceksiniz:

> [!div class="checklist"]
> * Makine öğrenme modelini oluşturan depolanmış bir yordam oluşturma
> * Modeli veritabanı tablosunda depolama
> * Modeli kullanarak öngörüler yapan depolanmış yordam oluşturma
> * Modeli yeni verilerle yürütme

[Birinci bölümde,](sql-database-tutorial-predictive-model-prepare-data.md)örnek bir veritabanını nasıl içe aktarabileceğinizi ve daha sonra Verileri R'de bir tahminmodelinin eğitimi için nasıl hazırlayacağınızı öğrendiniz.

[İkinci bölümde,](sql-database-tutorial-predictive-model-build-compare.md)R'de birden fazla makine öğrenme modeli oluşturmayı ve eğitmeyi öğrendiniz ve sonra en doğru olanı seçtiniz.

## <a name="prerequisites"></a>Ön koşullar

* Bu öğretici serisinin Bölüm üç [**bölüm bir**](sql-database-tutorial-predictive-model-prepare-data.md) ve [**bölüm iki**](sql-database-tutorial-predictive-model-build-compare.md)tamamlamış varsayar.

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Modeli oluşturan depolanmış yordam oluşturma

Bu öğretici serisinin ikinci bölümünde, bir karar ağacı (dtree) modeli en doğru olduğuna karar verdi. Şimdi, geliştirdiğiniz R komut dosyalarını kullanarak, RevoScaleR paketinden rxDTree kullanarak dtree modelini eğiten ve oluşturan bir depolanmış yordam`generate_rental_rx_model`() oluşturun.

Azure Veri Stüdyosu veya SSMS'te aşağıdaki komutları çalıştırın.

```sql
-- Stored procedure that trains and generates an R model using the rental_data and a decision tree algorithm
DROP PROCEDURE IF EXISTS generate_rental_rx_model;
GO
CREATE PROCEDURE generate_rental_rx_model (@trained_model VARBINARY(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
require("RevoScaleR");

rental_train_data$Holiday <- factor(rental_train_data$Holiday);
rental_train_data$Snow    <- factor(rental_train_data$Snow);
rental_train_data$WeekDay <- factor(rental_train_data$WeekDay);

#Create a dtree model and train it using the training data set
model_dtree <- rxDTree(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = rental_train_data);
#Serialize the model before saving it to the database table
trained_model <- as.raw(serialize(model_dtree, connection=NULL));
'
        , @input_data_1 = N'
            SELECT RentalCount
                 , Year
                 , Month
                 , Day
                 , WeekDay
                 , Snow
                 , Holiday
            FROM dbo.rental_data
            WHERE Year < 2015
            '
        , @input_data_1_name = N'rental_train_data'
        , @params = N'@trained_model varbinary(max) OUTPUT'
        , @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Modeli veritabanı tablosunda depolama

TutorialDB veritabanında bir tablo oluşturun ve ardından modeli tabloya kaydedin.

1. Modeli depolamak`rental_rx_models`için bir tablo ( ) oluşturun.

    ```sql
    USE TutorialDB;
    DROP TABLE IF EXISTS rental_rx_models;
    GO
    CREATE TABLE rental_rx_models (
          model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(MAX) NOT NULL
        );
    GO
    ```

1. Modeli "rxDTree" model adı ile ikili nesne olarak tabloya kaydedin.

    ```sql
    -- Save model to table
    TRUNCATE TABLE rental_rx_models;
    
    DECLARE @model VARBINARY(MAX);
    
    EXECUTE generate_rental_rx_model @model OUTPUT;
    
    INSERT INTO rental_rx_models (
          model_name
        , model
        )
    VALUES (
         'rxDTree'
        , @model
        );
    
    SELECT *
    FROM rental_rx_models;
    ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Öngörüler yapan depolanmış yordam oluşturma

Eğitilmiş modeli ve`predict_rentalcount_new`yeni veri kümesini kullanarak öngörüler yapan depolanmış yordam () oluşturun.

```sql
-- Stored procedure that takes model name and new data as input parameters and predicts the rental count for the new data
DROP PROCEDURE IF EXISTS predict_rentalcount_new;
GO
CREATE PROCEDURE predict_rentalcount_new (
      @model_name VARCHAR(100)
    , @input_query NVARCHAR(MAX)
    )
AS
BEGIN
    DECLARE @rx_model VARBINARY(MAX) = (
            SELECT model
            FROM rental_rx_models
            WHERE model_name = @model_name
            );

    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
require("RevoScaleR");

#Convert types to factors
rentals$Holiday <- factor(rentals$Holiday);
rentals$Snow    <- factor(rentals$Snow);
rentals$WeekDay <- factor(rentals$WeekDay);

#Before using the model to predict, we need to unserialize it
rental_model <- unserialize(rx_model);

#Call prediction function
rental_predictions <- rxPredict(rental_model, rentals);
'
        , @input_data_1 = @input_query
        , @input_data_1_name = N'rentals'
        , @output_data_1_name = N'rental_predictions'
        , @params = N'@rx_model varbinary(max)'
        , @rx_model = @rx_model
    WITH RESULT SETS(("RentalCount_Predicted" FLOAT));
END;
GO
```

## <a name="execute-the-model-with-new-data"></a>Modeli yeni verilerle yürütme

Artık yeni verilerden kira `predict_rentalcount_new` sayısını tahmin etmek için depolanan yordamı kullanabilirsiniz.

```sql
-- Use the predict_rentalcount_new stored procedure with the model name and a set of features to predict the rental count
EXECUTE dbo.predict_rentalcount_new @model_name = 'rxDTree'
    , @input_query = '
        SELECT CONVERT(INT,  3) AS Month
             , CONVERT(INT, 24) AS Day
             , CONVERT(INT,  4) AS WeekDay
             , CONVERT(INT,  1) AS Snow
             , CONVERT(INT,  1) AS Holiday
        ';
GO
```

Aşağıdakilere benzer bir sonuç görmeniz gerekir.

```results
RentalCount_Predicted
332.571428571429
```

Bir Azure SQL veritabanında başarılı bir şekilde bir model oluşturdunuz, eğittiniz ve dağıttınız. Daha sonra bu modeli, yeni verilere dayalı değerleri tahmin etmek için depolanan bir yordamda kullandınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

TutorialDB veritabanını kullanmayı bitirdiğinizde, Azure SQL Veritabanı sunucunuzdan silin.

Azure portalından aşağıdaki adımları izleyin:

1. Azure portalındaki sol menüden Tüm **kaynakları** veya **SQL veritabanlarını**seçin.
1. **Ada göre Filtre...** alanına **TutorialDB'yi**girin ve aboneliğinizi seçin.
1. TutorialDB veritabanınızı seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici serinin üçüncü bölümünde, şu adımları tamamladınız:

* Makine öğrenme modelini oluşturan depolanmış bir yordam oluşturma
* Modeli veritabanı tablosunda depolama
* Modeli kullanarak öngörüler yapan depolanmış yordam oluşturma
* Modeli yeni verilerle yürütme

Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde R kullanma hakkında daha fazla bilgi edinmek için (önizleme), bkz:

* [Machine Learning Services 'i kullanarak Azure SQL Veritabanı'na gelişmiş R işlevleri yazın (önizleme)](sql-database-machine-learning-services-functions.md)
* [Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde R ve SQL verileriyle çalışma (önizleme)](sql-database-machine-learning-services-data-issues.md)
* [Azure SQL Veritabanı Makine Öğrenme Hizmetleri'ne R paketi ekleme (önizleme)](sql-database-machine-learning-services-add-r-packages.md)
