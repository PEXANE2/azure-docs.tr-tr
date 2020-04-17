---
title: "Öğretici: R'de bir kümeleme modeli dağıtma"
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu üç bölümlük öğretici serinin üçüncü bölümünde, Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme) ile R'de bir kümeleme modeli dağıtacaksınız.
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
ms.date: 07/29/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ef478246108d40a0c97d7dab03ecf1e5b474410b
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81452902"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de bir kümeleme modeli dağıtma (önizleme)

Bu üç bölümlük öğretici serinin üçüncü bölümünde, Azure SQL Veritabanı Makine Öğrenme Hizmetleri 'ni (önizleme) kullanarak R'de geliştirilen bir kümeleme modelini bir SQL veritabanına dağıtırsınız.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

Kümeleme gerçekleştiren katıştırılmış Bir R komut dosyası ile depolanmış bir yordam oluşturursunuz. Modeliniz Azure SQL veritabanında yürütüldeğinden, veritabanında depolanan verilere karşı kolayca eğitilebilir.

Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
> * Modeli oluşturan depolanmış yordam oluşturma
> * SQL Veritabanında kümeleme gerçekleştirin
> * Kümeleme bilgilerini kullanma

Birinci [bölümde,](sql-database-tutorial-clustering-model-prepare-data.md)kümeleme gerçekleştirmek için azure SQL veritabanından verileri nasıl hazırlayacağınızı öğrendiniz.

[İkinci bölümde,](sql-database-tutorial-clustering-model-build.md)R'de bir K-Means kümeleme modeli oluşturmayı ve eğitmeyi öğrendiniz.

## <a name="prerequisites"></a>Ön koşullar

* Bu öğretici serisinin Bölüm üç [**bölüm bir**](sql-database-tutorial-clustering-model-prepare-data.md) ve [**bölüm iki**](sql-database-tutorial-clustering-model-build.md)tamamlamış varsayar.

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Modeli oluşturan depolanmış yordam oluşturma

Depolanan yordamı oluşturmak için aşağıdaki T-SQL komut dosyasını çalıştırın. Yordam, bu öğretici serinin birinci ve ikinci bölümlerinde geliştirdiğiniz adımları yeniden oluşturur:

* müşterileri satın alma ve iade geçmişlerine göre sınıflandırmak
* K-Means algoritması kullanarak dört müşteri kümesi oluşturmak

Yordam, veritabanı tablosunda ortaya çıkan müşteri küme eşlemlerini **customer_return_clusters.**

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE 
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE 
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE 
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE 
            WHEN (returns_count IS NULL)
                THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string
connStr <- paste("Driver=SQL Server; Server=", instance_name,
               "; Database=", database_name,
               "; Trusted_Connection=true; ",
                  sep="" );

# Input customer data that needs to be classified.
# This is the result we get from the query.
customer_returns <- RxSqlServerData(
                     sqlQuery=input_query,
                     colClasses=c(customer ="numeric",
                                  orderRatio="numeric",
                                  itemsRatio="numeric",
                                  monetaryRatio="numeric",
                                  frequency="numeric" ),
                     connectionString=connStr);
# Output table to hold the customer cluster mappings
return_cluster = RxSqlServerData(table = "customer_return_clusters",
                                 connectionString = connStr);

# Set seed for random number generator for predictability
set.seed(10);

# Generate clusters using rxKmeans and output clusters to a table
# called "customer_return_clusters"
clust <- rxKmeans( ~ orderRatio + itemsRatio + monetaryRatio + frequency,
                   customer_returns,
                   numClusters = 4,
                   outFile = return_cluster,
                   outColName = "cluster",
                   writeModelVars = TRUE ,
                   extraVarsToWrite = c("customer"),
                   overwrite = TRUE);
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering-in-sql-database"></a>SQL Veritabanında kümeleme gerçekleştirin

Depolanan yordamı oluşturduğunuza göre, kümeleme gerçekleştirmek için aşağıdaki komut dosyasını çalıştırın.

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

İşe yaradığını ve müşterilerin listesini ve küme eşlemelerini gerçekten aldığımızı doğrulayın.

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>Kümeleme bilgilerini kullanma

Kümeleme yordamını veritabanında depoladığıiçin, kümeleme işlemini aynı veritabanında depolanan müşteri verilerine karşı verimli bir şekilde gerçekleştirebilir. Müşteri verileriniz güncelleştirildiğinde yordamı yürütebilir ve güncelleştirilmiş kümeleme bilgilerini kullanabilirsiniz.

Daha etkin iade davranışına sahip grup olan küme 3'teki müşterilere tanıtım e-postası göndermek istediğinizi varsayalım (dört kümenin [ikinci bölümde](sql-database-tutorial-clustering-model-build.md#analyze-the-results)nasıl açıklandığını görebilirsiniz). Aşağıdaki kod, küme 3'teki müşterilerin e-posta adreslerini seçer.

```sql
USE [tpcxbb_1gb]

SELECT customer.[c_email_address],
    customer.c_customer_sk
FROM dbo.customer
JOIN [dbo].[customer_return_clusters] AS r ON r.customer = customer.c_customer_sk
WHERE r.cluster = 3
```

Diğer kümelerde müşteriler için e-posta adreslerini döndürmek için **r.cluster** değerini değiştirebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyi bitirdikten sonra, Azure SQL Veritabanı sunucunuzdaki tpcxbb_1gb veritabanını silebilirsiniz.

Azure portalından aşağıdaki adımları izleyin:

1. Azure portalındaki sol menüden Tüm **kaynakları** veya **SQL veritabanlarını**seçin.
1. **Ada göre Filtre'ye...** alanına **tpcxbb_1gb**girin ve aboneliğinizi seçin.
1. **tpcxbb_1gb** veritabanınızı seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici serinin üçüncü bölümünde, şu adımları tamamladınız:

* Modeli oluşturan depolanmış yordam oluşturma
* SQL Veritabanında kümeleme gerçekleştirin
* Kümeleme bilgilerini kullanma

Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde R kullanma hakkında daha fazla bilgi edinmek için (önizleme), bkz:

* [Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de tahmine dayalı bir modeli eğitmek için veri hazırlama (önizleme)](sql-database-tutorial-predictive-model-prepare-data.md)
* [Machine Learning Services 'i kullanarak Azure SQL Veritabanı'na gelişmiş R işlevleri yazın (önizleme)](sql-database-machine-learning-services-functions.md)
* [Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde R ve SQL verileriyle çalışma (önizleme)](sql-database-machine-learning-services-data-issues.md)
* [Azure SQL Veritabanı Makine Öğrenme Hizmetleri'ne R paketi ekleme (önizleme)](sql-database-machine-learning-services-add-r-packages.md)
