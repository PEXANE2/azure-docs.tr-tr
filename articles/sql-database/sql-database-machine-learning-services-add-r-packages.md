---
title: Machine Learning Services'a R paketi ekleme (önizleme)
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu makalede, Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde (önizleme) zaten yüklenmemiş bir R paketinin nasıl yüklenilen açıklanmaktadır.
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
ms.date: 04/29/2019
ms.openlocfilehash: ce85f45d823df42e70af53824e175968439621d3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73819876"
---
# <a name="add-an-r-package-to-azure-sql-database-machine-learning-services-preview"></a>Azure SQL Veritabanı Makine Öğrenme Hizmetleri'ne R paketi ekleme (önizleme)

Bu makalede, Azure SQL Veritabanı Makine Öğrenme Hizmetleri'ne R paketi nasıl ekleyeceğiniz açıklanmaktadır (önizleme).

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Yerel bilgisayarınıza [R](https://www.r-project.org) ve [RStudio Desktop'ı](https://www.rstudio.com/products/rstudio/download/) yükleyin. R; Windows, MacOS ve Linux platformlarında kullanılabilir. Bu makalede, Windows kullandığınızı varsayar.

- Bu makalede, Azure SQL Veritabanı'nda bir R komut dosyası çalıştırmak için [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) veya SQL Server Management [Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) kullanma örneği yer almaktadır. Diğer veritabanı yönetimi veya sorgu araçlarını kullanarak R komut dosyalarını çalıştırabilirsiniz, ancak bu örnek Azure Veri Stüdyosu veya SSMS'i varsayar.
   
> [!NOTE]
> Azure Veri Stüdyosu veya SSMS'te **sp_execute_external_script** kullanarak Bir R komut dosyası çalıştırarak paket yükleyemezsiniz. Paketleri yalnızca bu makalede açıklandığı şekilde R komut satırı ve RStudio'yu kullanarak yükleyebilir ve kaldırabilirsiniz. Paket yüklendikten sonra, **sp_execute_external_script**kullanarak paket işlevlerine R komut dosyasında erişebilirsiniz.

## <a name="list-r-packages"></a>R paketlerinin listesi

Microsoft, Azure SQL veritabanınızda Machine Learning Services ile önceden yüklenmiş bir dizi R paketi sağlar.
Azure Veri Stüdyosu veya SSMS'te aşağıdaki komutu çalıştırarak yüklü R paketlerinin listesini görebilirsiniz.

1. Azure Veri Stüdyosu'nı veya SSMS'i açın ve Azure SQL Veritabanınıza bağlanın.

1. Şu komutu çalıştırın:

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License")]);'
WITH RESULT SETS((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            ));
```

Çıktı aşağıdakine benzer olmalıdır.

**Sonuç -ları**

![R içindeki yüklü paketler](./media/sql-database-machine-learning-services-add-r-packages/r-installed-packages.png)

## <a name="add-a-package-with-sqlmlutils"></a>sqlmlutils ile bir paket ekleme

Azure SQL Veritabanınızda zaten yüklü olmayan bir paket kullanmanız gerekiyorsa, paketi [sqlmlutils](https://github.com/Microsoft/sqlmlutils)kullanarak yükleyebilirsiniz. **sqlmlutils,** kullanıcıların SQL veritabanlarıyla (SQL Server ve Azure SQL Veritabanı) etkileşimde kalmalarına ve SQL'deki R veya Python kodunu bir R veya Python istemcisinden yürütmelerine yardımcı olmak için tasarlanmış bir pakettir. Şu anda Azure SQL Veritabanı'nda yalnızca **sqlmlutils'in** R sürümü desteklenir.

Aşağıdaki örnekte, dizeleri biçimlendirebilen ve enterpolasyona getirebilen **[tutkal](https://cran.r-project.org/web/packages/glue/)** paketini yüklersiniz. Bu adımlar **sqlmlutils** ve **RODBCext** **(sqlmlutils**için bir ön koşul) yükler ve **tutkal** paketi ekleyin.

### <a name="install-sqlmlutils"></a>**sqlmlutils yükleyin**

1. Yerel bilgisayarınıza en son **sqlmlutils** zip dosyasını https://github.com/Microsoft/sqlmlutils/tree/master/R/dist indirin. Dosyanın zip'ini açmanız gerekmez.

1. Komut **İstemi'ni** açın ve **rodbcext** ve **sqlmlutils'i** yerel bilgisayarınıza yüklemek için aşağıdaki komutları çalıştırın. İndirdiğiniz **sqlmlutils** zip dosyasına tam yol değiştirin (örnek dosyanın Belgeler klasörünüzde olduğunu varsayar).
    
    ```console
    R -e "install.packages('RODBCext', repos='https://cran.microsoft.com')"
    R CMD INSTALL %UserProfile%\Documents\sqlmlutils_0.5.1.zip
    ```

    Gördüğünüz çıktı aşağıdakilere benzer olmalıdır.

    ```text
    In R CMD INSTALL
    * installing to library 'C:/Users/<username>/Documents/R/win-library/3.5'
    package sqlmlutils successfully unpacked and MD5 sums checked
    ```

    > [!TIP]
    > Hata alırsanız, " 'R' bir iç veya dış komut, çalışabilir program veya toplu dosya olarak tanınmıyor", büyük olasılıkla R.exe yolu Windows'da **PATH** ortamı değişkenine dahil olmadığı anlamına gelir. Yolu ortam değişkenine ekleyebilir veya komut istemindeki klasöre (örneğin) `cd C:\Program Files\R\R-3.5.3\bin`gidebilir ve ardından komutu yeniden deneyebilirsiniz.

### <a name="add-the-package"></a>Paketi ekle

1. RStudio'yu açın ve yeni bir **R Script** dosyası oluşturun. 

1. **Sqlmlutils**kullanarak **tutkal** paketini yüklemek için aşağıdaki R kodunu kullanın. Kendi Azure SQL Veritabanı bağlantı bilgilerinizi değiştirin.

    ```R
    library(sqlmlutils)
    connection <- connectionInfo(
    server= "yourserver.database.windows.net",
    database = "yourdatabase",
    uid = "yoursqluser",
    pwd = "yoursqlpassword")
    
    sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
    ```

    > [!TIP]
    > **Kapsam** **GENEL** veya **ÖZEL**olabilir. Genel kapsam, veritabanı yöneticisinin tüm kullanıcıların kullanabileceği paketleri yüklemesi açısından yararlıdır. Özel kapsam, paketi yalnızca onu yükleyen kullanıcı tarafından kullanılabilir hale getirir. Kapsam değeri belirtmezseniz **PRIVATE** varsayılan kapsamı kullanılır.

### <a name="verify-the-package"></a>Paketi doğrulayın

**Yapıştırıcı** paketinin RStudio'da aşağıdaki R komut dosyasını çalıştırarak yüklendiğini doğrulayın. Önceki adımda tanımladığınız **bağlantıyı** kullanın.

```R
r<-sql_installed.packages(connectionString = connection, fields=c("Package", "Version", "Depends", "License"))
View(r)
```

**Sonuç -ları**

![RTestData tablosunun içeriği](./media/sql-database-machine-learning-services-add-r-packages/r-verify-package-install.png)

### <a name="use-the-package"></a>Paketi kullanma

Paket yüklendikten sonra, **sp_execute_external_script**aracılığıyla r komut dosyasında kullanabilirsiniz.

1. Azure Veri Stüdyosu'nı veya SSMS'i açın ve Azure SQL Veritabanınıza bağlanın.

1. Şu komutu çalıştırın:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    library(glue)
    
    name <- "Fred"
    age <- 50
    anniversary <- as.Date("2020-06-14")
    text <- glue(''My name is {name}, '',
    ''my age next year is {age + 1}, '',
    ''my anniversary is {format(anniversary, "%A, %B %d, %Y")}.'')
    
    print(text)
    ';
    ```

    **İletiler** sekmesinde aşağıdaki sonucu görürsünüz.

    **Sonuç -ları**

    ```text
    My name is Fred, my age next year is 51, my anniversary is Sunday, June 14, 2020.
    ```

### <a name="remove-the-package"></a>Paketi kaldırma

Paketi kaldırmak istiyorsanız, Aşağıdaki R komut dosyasını RStudio'da çalıştırın. Daha önce **tanımladığınız** bağlantıyı kullanın.

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

> [!TIP]
> Azure SQL veritabanınıza Bir R paketi yüklemenin başka bir yolu da **CREATE EXTERNAL LIBRARY** T-SQL deyimini kullanarak R paketini bayt akışından yüklemektir. Bkz. CREATE EXTERNAL LIBRARY başvuru belgelerindeki [bir bayt akışından kitaplık](/sql/t-sql/statements/create-external-library-transact-sql#create-a-library-from-a-byte-stream) [oluştur.](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql)

## <a name="next-steps"></a>Sonraki adımlar

R ile Azure SQL Veritabanı Makine Öğrenme Hizmetleri hakkında daha fazla bilgi için (önizleme), aşağıdaki makalelere bakın.

- [R ile Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme)](sql-database-machine-learning-services-overview.md)
- [Machine Learning Services 'i kullanarak Azure SQL Veritabanı'na gelişmiş R işlevleri yazın (önizleme)](sql-database-machine-learning-services-functions.md)
- [Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde R ve SQL verileriyle çalışma (önizleme)](sql-database-machine-learning-services-data-issues.md)