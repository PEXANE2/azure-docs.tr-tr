---
title: "Öğretici: R'de tahmine dayalı bir modeli eğitmek için veri hazırlama"
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu üç bölümlük öğretici serinin birinci bölümünde, Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme) ile R'de tahmine dayalı bir modeli eğitmek için azure SQL veritabanındaki verileri hazırlaacaksınız.
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
ms.openlocfilehash: 505f58f13a7186948a228fefe872d74fb98eba33
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80345783"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de tahmine dayalı bir modeli eğitmek için veri hazırlama (önizleme)

Bu üç bölümlük öğretici serinin birinci bölümünde, R kullanarak azure SQL veritabanından veri aktarAcak ve hazırlaacaksınız. Bu serinin ilerleyen saatlerinde, Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme) ile R'de tahmine dayalı bir makine öğrenme modelini eğitmek ve dağıtmak için bu verileri kullanırsınız.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

Bu öğretici serisi için, bir kayak kiralama iş sahibi düşünün ve gelecekteki bir tarihte olacak kiralama sayısını tahmin etmek istiyorum. Bu bilgiler stok, personel ve tesisleri hazır olsun yardımcı olacaktır.

Bu serinin birinci ve ikinci bölümlerinde, verilerinizi hazırlamak ve bir makine öğrenme modeli eğitmek için RStudio'da bazı R komut dosyaları geliştireceksiniz. Daha sonra, üçüncü bölümde, bu R komut dosyalarını sql veritabanında depolanmış yordamları kullanarak çalıştırırsınız.

Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
> * R kullanarak örnek bir veritabanını Azure SQL veritabanına aktarma
> * Azure SQL veritabanındaki verileri R veri çerçevesine yükleme
> * Bazı sütunları kategorik olarak tanımlayarak Verileri R'de hazırlama

İkinci [bölümde,](sql-database-tutorial-predictive-model-build-compare.md)R'de birden fazla makine öğrenme modeli oluşturmayı ve eğitmeyi öğreneceksiniz ve sonra en doğru olanı seçeceksiniz.

[Üçüncü bölümde,](sql-database-tutorial-predictive-model-deploy.md)modeli veritabanında nasıl depoladığınızı ve ardından bir ve ikinci bölümlerde geliştirdiğiniz R komut dosyalarından depolanmış yordamlar oluşturmayı öğreneceksiniz. Depolanan yordamlar, yeni verilere dayalı öngörüler yapmak için bir SQL veritabanında çalışır.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - Azure aboneliğiniz yoksa başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/)

* Machine Learning Services özellikli Azure SQL Veritabanı Sunucusu etkindir - Genel önizleme sırasında Microsoft, mevcut veya yeni veritabanlarınız için makine öğrenimini size verebecektir. Önizleme için [Kaydolun'daki](sql-database-machine-learning-services-overview.md#signup)adımları izleyin.

* RevoScaleR paketi - Bu paketi yerel olarak yükleme seçenekleri için [RevoScaleR'a](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler?view=sql-server-2017#versions-and-platforms) bakın.

* R IDE - Bu öğretici [RStudio Masaüstü](https://www.rstudio.com/products/rstudio/download/)kullanır.

* SQL sorgu aracı - Bu öğretici, [Azure Veri Stüdyosu](https://docs.microsoft.com/sql/azure-data-studio/what-is) veya SQL Server Management [Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) kullandığınızı varsayar.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="import-the-sample-database"></a>Örnek veritabanını alma

Bu öğreticide kullanılan örnek veri kümesi, karşıdan yüklemeniz ve kullanmanız için **bir .bacpac** veritabanı yedekleme dosyasına kaydedilmiştir.

1. [TutorialDB.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bacpac)dosyasını indirin.

1. Bu ayrıntıları kullanarak [bir Azure SQL veritabanı oluşturmak için BACPAC dosyasını içe aktarma](https://docs.microsoft.com/azure/sql-database/sql-database-import)yönergelerini izleyin:

   * İndirdiğiniz **TutorialDB.bacpac** dosyasından alma
   * Genel önizleme sırasında, yeni veritabanı için **Gen5/vCore** yapılandırmasını seçin
   * Yeni veritabanı "TutorialDB" adı

## <a name="load-the-data-into-a-data-frame"></a>Verileri veri çerçevesine yükleme

R'deki verileri kullanmak için Azure SQL veritabanındaki verileri bir veri`rentaldata`çerçevesine yüklersiniz .

RStudio'da yeni bir RScript dosyası oluşturun ve aşağıdaki komut dosyasını çalıştırın. **Sunucu,** **UID**ve **PWD'yi** kendi bağlantı bilgilerinizle değiştirin.

```r
#Define the connection string to connect to the TutorialDB database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Azure SQL Database Server>",
               "; Database=TutorialDB",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");

#Get the data from the table
SQL_rentaldata <- RxSqlServerData(table = "dbo.rental_data", connectionString = connStr, returnDataFrame = TRUE);

#Import the data into a data frame
rentaldata <- rxImport(SQL_rentaldata);

#Take a look at the structure of the data and the top rows
head(rentaldata);
str(rentaldata);
```

Aşağıdakilere benzer sonuçlar görmeniz gerekir.

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>Verileri hazırlama

Bu örnek veritabanında, hazırlık ların çoğu zaten yapıldı, ancak burada bir hazırlık daha yapacaksınız.
Veri türlerini *faktöre*değiştirerek üç sütunu *kategoriler* olarak tanımlamak için aşağıdaki R komut dosyasını kullanın.

```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);

#Visualize the dataset after the change
str(rentaldata);
```

Aşağıdakilere benzer sonuçlar görmeniz gerekir.

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

Veriler artık eğitim için hazırlanmıştır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyle devam etmeyecekseniz, TutorialDB veritabanını Azure SQL Veritabanı sunucunuzdan silin.

Azure portalından aşağıdaki adımları izleyin:

1. Azure portalındaki sol menüden Tüm **kaynakları** veya **SQL veritabanlarını**seçin.
1. **Ada göre Filtre...** alanına **TutorialDB'yi**girin ve aboneliğinizi seçin.
1. TutorialDB veritabanınızı seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici serinin birinci bölümünde, şu adımları tamamladınız:

* R kullanarak örnek bir veritabanını Azure SQL veritabanına aktarma
* Azure SQL veritabanındaki verileri R veri çerçevesine yükleme
* Bazı sütunları kategorik olarak tanımlayarak Verileri R'de hazırlama

TutorialDB veritabanındaki verileri kullanan bir makine öğrenme modeli oluşturmak için, bu öğretici serinin ikinci bölümünü izleyin:

> [!div class="nextstepaction"]
> [Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de tahmine dayalı bir model oluşturun (önizleme)](sql-database-tutorial-predictive-model-build-compare.md)
