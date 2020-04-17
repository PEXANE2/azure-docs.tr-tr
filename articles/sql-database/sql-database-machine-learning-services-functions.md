---
title: Gelişmiş R işlevleri yazma
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Machine Learning Services (önizleme) kullanarak Azure SQL Veritabanı'nda gelişmiş istatistiksel hesaplama için Nasıl R işlevi yazabileceğinizi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ba78267b1c6dc8f0e1bd25bb8ecdb1d8d344d03e
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81453123"
---
# <a name="write-advanced-r-functions-in-azure-sql-database-using-machine-learning-services-preview"></a>Machine Learning Services 'i kullanarak Azure SQL Veritabanı'na gelişmiş R işlevleri yazın (önizleme)

Bu makalede, R matematiksel ve yardımcı program işlevlerini SQL depolanan yordamına nasıl katıştırılacak açıklanmaktadır. T-SQL'de uygulanması karmaşık olan gelişmiş istatistiksel işlevler, yalnızca tek bir kod satırıyla R'de yapılabilir.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa, başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/)

- Bu alıştırmalarda örnek kodu çalıştırmak için öncelikle [Machine Learning Services (R ile) özellikli Azure SQL Veritabanı'nı](sql-database-machine-learning-services-overview.md) etkinleştirmeniz gerekir.

- En son [SQL Server Management Studio'yu](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) yüklediğinizden emin olun. Diğer veritabanı yönetimi veya sorgu araçlarını kullanarak R komut dosyalarını çalıştırabilirsiniz, ancak bu hızlı başlatmada SSMS'i kullanırsınız.

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Rasgele sayılar oluşturmak için depolanmış yordam oluşturma

Basitlik için, Machine Learning Services `stats` (önizleme) kullanarak Varsayılan olarak Azure SQL Veritabanı ile yüklenen ve yüklenen R paketini kullanalım. Paket, aralarında işlevin `rnorm` de bulunduğu yaygın istatistiksel görevler için yüzlerce işlev içerir. Bu işlev, standart sapma ve araçlar göz önüne alındığında, normal dağılımı kullanarak rasgele sayıların belirli bir sayı oluşturur.

Örneğin, standart sapma 3 göz önüne alındığında, aşağıdaki R kodu ortalama 50'de 100 sayı döndürür.

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

Bu R satırını T-SQL'den `sp_execute_external_script` çağırmak için R komut dosyası parametresinde R işlevini çalıştırın ve ekleyin:

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));
'
    , @input_data_1 = N'   ;'
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

Farklı bir rasgele sayı kümesi oluşturmayı kolaylaştırmak isterseniz ne olur?

SQL ile birleştirildiğinde bu çok kolay. Kullanıcıdan bağımsız değişkenler alan bir depolanmış yordam tanımlarsınız, ardından bu bağımsız değişkenleri değişken olarak R komut dosyasına geçirirsiniz.

```sql
CREATE PROCEDURE MyRNorm (
    @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- as.data.frame(rnorm(mynumbers, mymean, mysd));
'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- İlk satır, depolanan yordam yürütüldüğünde gereken SQL giriş parametrelerinin her birini tanımlar.

- Başlangıç `@params` satırı, R kodu tarafından kullanılan tüm değişkenleri ve ilgili SQL veri türlerini tanımlar.

- Hemen takip satırları ilgili R değişken adlarına SQL parametre adlarını eşler.

Artık R işlevini depolanmış bir yordamda tamamladığınızda, işlevi kolayca arayabilir ve aşağıdaki gibi farklı değerlerde geçirebilirsiniz:

```sql
EXECUTE MyRNorm @param1 = 100
    , @param2 = 50
    , @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>Sorun giderme için R yardımcı programı işlevlerini kullanma

Varsayılan `utils` olarak yüklenen paket, geçerli R ortamını araştırmak için çeşitli yardımcı program işlevleri sağlar. Bu işlevler, R kodunuzu SQL'de ve dış ortamlarda performans biçiminde tutarsızlıklar buluyorsanız yararlı olabilir. Örneğin, geçerli R ortamı `memory.limit()` için bellek almak için R işlevini kullanabilirsiniz.

`utils` Paket yüklü olduğundan ancak varsayılan olarak yüklenmediği `library()` için, önce yükleme işlevini kullanmanız gerekir.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
library(utils);
mymemory <- memory.limit();
OutputDataSet <- as.data.frame(mymemory);
'
    , @input_data_1 = N' ;'
WITH RESULT SETS(([Col1] INT NOT NULL));
```

> [!TIP]
> Birçok kullanıcı R süreçleri tarafından kullanılan zamanı `system.time` yakalamak `proc.time`ve performans sorunlarını analiz etmek gibi R sistem zamanlama işlevlerini kullanmayı sever.
