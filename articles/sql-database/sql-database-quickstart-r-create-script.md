---
title: Basit R komut dosyaları oluşturma ve çalıştırma
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde basit R komut dosyaları çalıştırın (önizleme).
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
ms.openlocfilehash: bb6cb6d86933166d2427788d697d9cd38cf04bf0
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460180"
---
# <a name="quickstart-create-and-run-simple-r-scripts-in-azure-sql-database-machine-learning-services-preview"></a>Quickstart: Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde basit R komut dosyaları oluşturun ve çalıştırın (önizleme)

Bu hızlı başlangıçta, Azure SQL Veritabanı'nda Machine Learning Services (R ile) kullanarak bir R komut dosyası kümesi oluşturur ve çalıştırAbilirsiniz.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- Sunucu düzeyinde güvenlik [duvarı kuralına](sql-database-server-level-firewall-rule.md) sahip bir [Azure SQL veritabanı](sql-database-single-database-get-started.md)
- R özellikli [Makine Öğrenimi Hizmetleri](sql-database-machine-learning-services-overview.md) etkindir.
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

Bu örnek, iyi biçimlendirilmiş bir R komut dosyasını sarmak için depolanan [yordamı sp_execute_external_script](/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) kullanır.

## <a name="run-a-simple-script"></a>Basit bir komut dosyası çalıştırma

R komut dosyası çalıştırmak için, sistem depolanan yordam, [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql)bir argüman olarak geçer.

Aşağıdaki adımlarda, bu örnek R komut dosyasını SQL veritabanınızda çalıştıracaksınız:

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. **SQL Server Management Studio'nuzu** açın ve SQL veritabanınıza bağlanın.

   Bağlanmak için yardıma ihtiyacınız varsa, [Azure SQL veritabanını bağlamak ve sorgulamak için Hızlı Başlangıç: SQL Server Management Studio'yı kullanın.](sql-database-connect-query-ssms.md)

1. Tüm R komut dosyasını [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) depolanan yordamına geçirin.

   Komut dosyası bağımsız `@script` değişkenden geçirilir. Bağımsız değişkenin `@script` içindeki her şey geçerli Bir R kodu olmalıdır.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

   Hata alırsanız Machine Learning Services (R ile) genel önizleme sürümü SQL veritabanınız için etkinleştirilmemiş olabilir. Yukarıdaki [Ön koşullara](#prerequisites) bakın.

   > [!NOTE]
   > Yöneticiyseniz, harici kodu otomatik olarak çalıştırabilirsiniz. Komutu kullanarak diğer kullanıcılara izin verebilirsiniz:
   <br>**GRANT KULLANıCı** * \<Adı\>* için HERHANGI BIR Dış SCRIPT YÜRÜTMEK .

2. Doğru sonuç hesaplanır ve `print` R işlevi sonucu **İletiler** penceresine döndürür.

   Buna benzer bir şey olmalı.

    **Sonuçlar**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Hello World komut dosyası çalıştırın

Tipik bir örnek komut dosyası sadece dize "Hello World" çıkışları biridir. Şu komutu çalıştırın.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Bu depolanan yordamın girdileri şunlardır:

| | |
|-|-|
| @language | aramak için dil uzantısı tanımlar, bu durumda, R |
| @script | R çalışma zamanına geçen komutları tanımlar. Tüm R komut dosyanız, Unicode metni olarak bu bağımsız değişkene eklenmelidir. Ayrıca metin **türü nvarchar** bir değişkenekleyebilirsiniz ve daha sonra değişken çağırabilirsiniz |
| @input_data_1 | sorgu tarafından döndürülen, verileri veri çerçevesi olarak SQL Server'a döndüren R çalışma süresine geçirilen veriler |
|SONUÇ SETLERI ILE | yan tümce, SQL Server için döndürülen veri tablosunun şemasını tanımlar, "Hello World" sütun adı olarak, veri türü için **int** |

Komut aşağıdaki metni çıkar:

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Girişleri ve çıkışları kullanma

Varsayılan olarak, [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) genellikle geçerli bir SQL sorgusu şeklinde sağladığınız tek bir veri kümesini giriş olarak kabul eder. Daha sonra çıktı olarak tek bir R veri çerçevesi döndürür.

Şimdilik, [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql)varsayılan giriş ve çıkış değişkenlerini kullanalım: **InputDataSet** ve **OutputDataSet**.

1. Küçük bir test verisi tablosu oluşturun.

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    
    INSERT INTO RTestData
    VALUES (1);
    
    INSERT INTO RTestData
    VALUES (10);
    
    INSERT INTO RTestData
    VALUES (100);
    GO
    ```

1. Tabloyu `SELECT` sorgulamak için deyimi kullanın.
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **Sonuçlar**

    ![RTestData tablosunun içeriği](./media/sql-database-quickstart-r-create-script/select-rtestdata.png)

1. Aşağıdaki R komut dosyasını çalıştırın. `SELECT` İfadeyi kullanarak tablodaki verileri alır, R çalışma zamanından geçirir ve verileri veri çerçevesi olarak döndürür. Yan `WITH RESULT SETS` tümce, SQL Veritabanı için döndürülen veri tablosunun şeasını tanımlar ve sütun adı *NewColName'i*ekler.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Sonuçlar**

    ![Tablodan veri döndüren R betiğinin çıktısı](./media/sql-database-quickstart-r-create-script/r-output-rtestdata.png)

1. Şimdi giriş ve çıktı değişkenlerinin adlarını değiştirelim. Varsayılan giriş ve çıkış değişken adları **InputDataSet** ve **OutputDataSet**vardır, bu komut dosyası **SQL_in** ve **SQL_out**adlarını değiştirir:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    R'nin büyük/küçük harf duyarlı olduğunu unutmayın. R komut dosyasında **(SQL_out**, **SQL_in)** kullanılan giriş ve `@input_data_1_name` `@output_data_1_name`çıkış değişkenlerinin, büyük/küçük harf dahil tanımlanan değerlerle eşleşmesi gerekir.

   > [!TIP]
   > Yalnızca bir giriş veri kümesi parametre olarak iletilebilir ve yalnızca bir veri kümesini döndürebilirsiniz. Ancak R kodunuzdan diğer tüm veri kümelerini çağırabilir ve veri kümesine ek olarak diğer türlerin çıkışlarını döndürebilirsiniz. Ayrıca OUTPUT anahtar sözcüğünü herhangi bir parametreye ekleyerek sonuçlarla birlikte döndürülmesini sağlayabilirsiniz.

1. Ayrıca, giriş verisi olmayan (boş olarak`@input_data_1` ayarlanır) yalnızca R komut dosyasını kullanarak değerler oluşturabilirsiniz.

   Aşağıdaki komut dosyası "merhaba" ve "dünya" metnini çıkar.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Sonuçlar**

    ![Giriş olarak @script kullanarak sonuçları sorgulama](./media/sql-database-quickstart-r-create-script/r-data-generated-output.png)

## <a name="check-r-version"></a>R sürümünü denetleme

SQL veritabanınızda Hangi R sürümünün yüklü olduğunu görmek istiyorsanız, aşağıdaki komut dosyasını çalıştırın.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

R `print` işlevi, sürümü **İletiler** penceresinde döndürür. Aşağıdaki örnek çıktıda, bu durumda SQL Veritabanı'nın 3.4.4 sürümünün yüklü olduğunu görebilirsiniz.

**Sonuçlar**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>R paketlerinin listesi

Microsoft, SQL veritabanınızda önceden Machine Learning Services ile birlikte yüklenmiş bir dizi R paketi sunar.

Sürüm, bağımlılıklar, lisans ve kitaplık yol bilgileri de dahil olmak üzere R paketlerinin yüklü olduğu bir listeyi görmek için aşağıdaki komut dosyasını çalıştırın.

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

Çıktı R'den geliyor `installed.packages()` ve sonuç kümesi olarak döndürülür.

**Sonuçlar**

![R içindeki yüklü paketler](./media/sql-database-quickstart-r-create-script/r-installed-packages.png)

## <a name="next-steps"></a>Sonraki adımlar

SQL Veritabanı'nda R kullanarak bir makine öğrenme modeli oluşturmak için aşağıdaki hızlı başlatmayı izleyin:

> [!div class="nextstepaction"]
> [Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de tahmine dayalı bir model oluşturma ve bunları eğitme (önizleme)](sql-database-quickstart-r-train-score-model.md)

R ile Azure SQL Veritabanı Makine Öğrenme Hizmetleri hakkında daha fazla bilgi için (önizleme), aşağıdaki makalelere bakın.

- [R ile Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme)](sql-database-machine-learning-services-overview.md)
- [Machine Learning Services 'i kullanarak Azure SQL Veritabanı'na gelişmiş R işlevleri yazın (önizleme)](sql-database-machine-learning-services-functions.md)
- [Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde R ve SQL verileriyle çalışma (önizleme)](sql-database-machine-learning-services-data-issues.md)
