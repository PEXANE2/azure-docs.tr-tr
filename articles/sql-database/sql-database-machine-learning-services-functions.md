---
title: Gelişmiş R işlevleri yazma
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Machine Learning Services (Önizleme) kullanarak Azure SQL veritabanı 'nda gelişmiş istatistiksel hesaplama için R işlevi yazmayı öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81453123"
---
# <a name="write-advanced-r-functions-in-azure-sql-database-using-machine-learning-services-preview"></a>Machine Learning Services kullanarak Azure SQL veritabanı 'nda gelişmiş R işlevleri yazma (Önizleme)

Bu makalede, bir SQL saklı yordamında R matematik ve yardımcı program işlevlerinin nasıl ekleneceği açıklanır. T-SQL ' de uygulanması karmaşık olan gelişmiş istatistiksel işlevler, tek bir kod satırı ile R 'de yapılabilir.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa başlamadan önce [bir hesap oluşturun](https://azure.microsoft.com/free/) .

- Örnek kodu bu alıştırmalarda çalıştırmak için, önce [Machine Learning Services (R ile) Azure SQL veritabanı](sql-database-machine-learning-services-overview.md) 'nın etkin olması gerekir.

- En son [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) yüklediğinizden emin olun. R komut dosyalarını diğer veritabanı yönetimini veya sorgu araçlarını kullanarak çalıştırabilirsiniz, ancak bu hızlı başlangıçta SSMS 'yi kullanacaksınız.

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Rastgele sayılar oluşturmak için bir saklı yordam oluşturma

Kolaylık olması için, Machine Learning Services (Önizleme) `stats` kullanarak Azure SQL veritabanı ile yüklenen ve varsayılan olarak yüklenen R paketini kullanalım. Paket, yaygın istatistiksel görevler için `rnorm` işlev aralarında yüzlerce işlev içerir. Bu işlev, standart sapma ve ortalamalar verilen normal dağıtımı kullanarak belirtilen sayıda rastgele sayı üretir.

Örneğin, aşağıdaki R kodu 100 sayısını, 3 ' ün standart sapması verildiğinde, 50 ' de döndürür.

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

T-SQL ' den bu R satırını çağırmak için, aşağıdaki `sp_execute_external_script` gibi r betiği parametresindeki r işlevini çalıştırın ve ekleyin:

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));
'
    , @input_data_1 = N'   ;'
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

Farklı bir rastgele sayılar kümesi oluşturmayı kolaylaştırmak isterseniz ne yapmalısınız?

Bu, SQL ile birleştirildiğinde kolay bir işlemdir. Kullanıcının bağımsız değişkenlerini alan bir saklı yordam tanımlar, ardından bu bağımsız değişkenleri değişkenler olarak R betiğine geçirin.

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

- İlk satır, saklı yordam yürütüldüğünde gereken her SQL giriş parametresini tanımlar.

- İle `@params` başlayan satır, R kodu tarafından kullanılan tüm değişkenleri ve KARŞıLıK gelen SQL veri türlerini tanımlar.

- Hemen izleyen satırlar, SQL parametre adlarını karşılık gelen R değişken adlarına eşleyin.

Artık R işlevini bir saklı yordamda sarmaladığınıza göre, işlevi kolayca çağırabilir ve farklı değerleri şöyle geçirebilirsiniz:

```sql
EXECUTE MyRNorm @param1 = 100
    , @param2 = 50
    , @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>Sorun giderme için R Utility işlevlerini kullanma

Varsayılan `utils` olarak yüklenen paket, geçerli R ortamını araştırmak için çeşitli yardımcı işlevler sağlar. Bu işlevler, R kodunuzun SQL 'de ve dış ortamlarda yaptığı şekilde tutarsızlıklar bulmanız halinde yararlı olabilir. Örneğin, geçerli R ortamı için bellek almak `memory.limit()` üzere r işlevini kullanabilirsiniz.

`utils` Paket yüklü ancak varsayılan olarak yüklenmediği için, önce onu yüklemek üzere `library()` işlevini kullanmanız gerekir.

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
> R işlemleri tarafından kullanılan süreyi yakalamak ve performans sorunlarını analiz etmek için, `system.time` ve `proc.time`gibi, r 'deki sistem zamanlama işlevlerini kullanmak gibi birçok kullanıcı.
