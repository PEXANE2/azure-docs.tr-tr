---
title: "Öğretici: R'de kümeleme gerçekleştirmek için veri hazırlama"
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu üç bölümlük öğretici serinin birinci bölümünde, Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme) ile R'de kümeleme gerçekleştirmek için azure SQL veritabanındaki verileri hazırlaacaksınız.
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
ms.openlocfilehash: abe7d5ed1d4ba1308abde04aee32a3ea222456b8
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81452885"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de kümeleme gerçekleştirmek için veri hazırlama (önizleme)

Bu üç bölümlük öğretici serinin birinci bölümünde, R kullanarak verileri bir Azure SQL veritabanından içe aktaracak ve hazırlaacaksınız. Bu serinin ilerleyen saatlerinde, Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme) ile R'de bir kümeleme modelini eğitmek ve dağıtmak için bu verileri kullanırsınız.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

*Kümeleme,* bir grubun üyelerinin bir şekilde benzer olduğu gruplar halinde verileri organize etmek olarak açıklanabilir.
Ürün satın alma ve iadeveri kümesinde müşterilerin kümelemasını gerçekleştirmek için **K-Means** algoritmasını kullanırsınız. Müşterileri kümeleyerek, belirli grupları hedefleyerek pazarlama çabalarınızı daha etkili bir şekilde odaklayabilirsiniz.
K-Ortalamakümeleme, benzerliklere dayalı verilerdeki desenleri arayan *denetimsiz* bir öğrenme algoritmasıdır.

Bu serinin birinci ve ikinci bölümlerinde, verilerinizi hazırlamak ve bir makine öğrenme modeli eğitmek için RStudio'da bazı R komut dosyaları geliştireceksiniz. Daha sonra, üçüncü bölümde, bu R komut dosyalarını sql veritabanında depolanmış yordamları kullanarak çalıştırırsınız.

Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
> * Örnek bir veritabanını Azure SQL veritabanına aktarma
> * R kullanarak farklı boyutlarda ayrı müşteriler
> * Azure SQL veritabanındaki verileri R veri çerçevesine yükleme

İkinci [bölümde,](sql-database-tutorial-clustering-model-build.md)R'de bir K-Means kümeleme modeli oluşturmayı ve nasıl eğiteceğinizi öğreneceksiniz.

[Üçüncü bölümde,](sql-database-tutorial-clustering-model-deploy.md)yeni verilere dayanarak R'de kümeleme gerçekleştirebilen bir Azure SQL veritabanında depolanmış yordamı nasıl oluşturabileceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - Azure aboneliğiniz yoksa başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/)

* [Machine Learning Services (R ile) özellikli Azure SQL Veritabanı](sql-database-machine-learning-services-overview.md) etkinleştirildi.

* RevoScaleR paketi - Bu paketi yerel olarak yükleme seçenekleri için [RevoScaleR'a](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler?view=sql-server-2017#versions-and-platforms) bakın.

* R IDE - Bu öğretici [RStudio Masaüstü](https://www.rstudio.com/products/rstudio/download/)kullanır.

* SQL sorgu aracı - Bu öğretici, [Azure Veri Stüdyosu](https://docs.microsoft.com/sql/azure-data-studio/what-is) veya SQL Server Management [Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) kullandığınızı varsayar.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="import-the-sample-database"></a>Örnek veritabanını alma

Bu öğreticide kullanılan örnek veri kümesi, karşıdan yüklemeniz ve kullanmanız için **bir .bacpac** veritabanı yedekleme dosyasına kaydedilmiştir. Bu veri kümesi, [İşlem İşlemperformans Konseyi (TPC)](http://www.tpc.org/default.asp)tarafından sağlanan [tpcx-bb](http://www.tpc.org/tpcx-bb/default.asp) veri kümesinden türetilmiştir.

1. [Dosyayı tpcxbb_1gb.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bacpac)indirin.

1. Bu ayrıntıları kullanarak [bir Azure SQL veritabanı oluşturmak için BACPAC dosyasını içe aktarma](https://docs.microsoft.com/azure/sql-database/sql-database-import)yönergelerini izleyin:

   * İndirdiğiniz **tpcxbb_1gb.bacpac** dosyasından alma
   * Genel önizleme sırasında, yeni veritabanı için **Gen5/vCore** yapılandırmasını seçin
   * Yeni veritabanına "tpcxbb_1gb" adını ver

## <a name="separate-customers"></a>Ayrı müşteriler

RStudio'da yeni bir RScript dosyası oluşturun ve aşağıdaki komut dosyasını çalıştırın.
SQL sorgusunda, müşterileri aşağıdaki boyutlar boyunca ayırıyorsunuz:

* **orderRatio** = iade sipariş oranı (toplam sipariş sayısına karşılık kısmen veya tamamen iade edilen toplam sipariş sayısı)
* **itemsRatio** = iade madde oranı (geri dönen toplam madde sayısı ile satın alınan madde sayısı)
* **monetaryRatio** = iade tutarı oranı (iade edilen kalemlerin toplam parasal tutarı ile satın alınan tutar)
* **frekans** = dönüş frekansı

Yapıştır **işlevinde,** **Sunucu,** **UID**ve **PWD'yi** kendi bağlantı bilgilerinizle değiştirin.

```r
# Define the connection string to connect to the tpcxbb_1gb database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Azure SQL Database Server>",
               "; Database=tpcxbb_1gb",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");


#Define the query to select data from SQL Server
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE 
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE 
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE 
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE 
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
"
```

## <a name="load-the-data-into-a-data-frame"></a>Verileri veri çerçevesine yükleme

Şimdi **rxSqlServerData** işlevini kullanarak sorgudan sonuçları bir R veri çerçevesine döndürmek için aşağıdaki komut dosyasını kullanın.
İşlemin bir parçası olarak, türlerin Doğru R'ye aktarıldığından emin olmak için seçili sütunların türünü (colClasses kullanarak) tanımlarsınız.

```r
# Query SQL Server using input_query and get the results back
# to data frame customer_returns
# Define the types for selected columns (using colClasses),
# to make sure that the types are correctly transferred to R
customer_returns <- rxSqlServerData(
                     sqlQuery=input_query,
                     colClasses=c(customer ="numeric",
                                  orderRatio="numeric",
                                  itemsRatio="numeric",
                                  monetaryRatio="numeric",
                                  frequency="numeric" ),
                     connectionString=connStr);

# Transform the data from an input dataset to an output dataset
customer_data <- rxDataStep(customer_returns);

# Take a look at the data just loaded from SQL Server
head(customer_data, n = 5);
```

Aşağıdakilere benzer sonuçlar görmeniz gerekir.

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

***Bu öğreticiye devam etmeyecekseniz,*** Azure SQL Veritabanı sunucunuzdaki tpcxbb_1gb veritabanını silin.

Azure portalından aşağıdaki adımları izleyin:

1. Azure portalındaki sol menüden Tüm **kaynakları** veya **SQL veritabanlarını**seçin.
1. **Ada göre Filtre'ye...** alanına **tpcxbb_1gb**girin ve aboneliğinizi seçin.
1. **tpcxbb_1gb** veritabanınızı seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici serinin birinci bölümünde, şu adımları tamamladınız:

* Örnek bir veritabanını Azure SQL veritabanına aktarma
* R kullanarak farklı boyutlarda ayrı müşteriler
* Azure SQL veritabanındaki verileri R veri çerçevesine yükleme

Bu müşteri verilerini kullanan bir makine öğrenme modeli oluşturmak için, bu öğretici serinin ikinci bölümünü izleyin:

> [!div class="nextstepaction"]
> [Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de tahmine dayalı bir model oluşturun (önizleme)](sql-database-tutorial-clustering-model-build.md)
