---
title: Veritabanını sorgulamak için Machine Learning Services ile R kullanma (Önizleme)
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu makalede bir Azure SQL veritabanına bağlanmak ve Transact-SQL deyimleriyle sorgulamak için Azure SQL veritabanı ile R betiği kullanma Machine Learning Services gösterilmektedir.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph, carlrab
manager: cgronlun
ms.date: 05/29/2019
ms.openlocfilehash: 0288d8c4710d12d8e67658caab93157c534b75ee
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76758368"
---
# <a name="quickstart-use-r-with-machine-learning-services-to-query-an-azure-sql-database-preview"></a>Hızlı başlangıç: Azure SQL veritabanını sorgulamak için Machine Learning Services ile R kullanma (Önizleme)

Bu hızlı başlangıçta, Azure SQL veritabanına bağlanmak ve T-SQL deyimlerini kullanarak verileri sorgulamak için Machine Learning Services ile R 'yi kullanırsınız.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- [Azure SQL veritabanı](sql-database-single-database-get-started.md)
  
- R özellikli [Machine Learning Services](sql-database-machine-learning-services-overview.md) . [Önizleme Için kaydolun](sql-database-machine-learning-services-overview.md#signup).

- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

> [!IMPORTANT]
> Bu makaledeki betikler, **Adventure Works** veritabanını kullanmak için yazılmıştır.

> [!NOTE]
> Microsoft, genel önizleme sırasında, mevcut veya yeni veritabanınız için sizi kullanıma sunulacaktır ve makine öğrenimini etkinleştirecektir, ancak yönetilen örnek dağıtım seçeneği şu anda desteklenmemektedir.

R ile Machine Learning Services, veritabanı içi R betiklerini yürütmek için kullanılan Azure SQL veritabanı 'nın bir özelliğidir. Daha fazla bilgi için bkz. [R Projesi](https://www.r-project.org/).

## <a name="get-sql-server-connection-information"></a>SQL Server bağlantı bilgilerini al

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, tek bir veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya yönetilen örnek Için **ana bilgisayar ' ın** yanındaki tam sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

## <a name="create-code-to-query-your-sql-database"></a>SQL veritabanınızı sorgulamak için kod oluşturma

1. **SQL Server Management Studio**’yu açın ve SQL veritabanınıza bağlanın.

   Bağlantı için yardıma ihtiyacınız varsa bkz. [hızlı başlangıç: Azure SQL veritabanına bağlanmak ve sorgu sorgulamak için SQL Server Management Studio kullanma](sql-database-connect-query-ssms.md).

1. Tüm R betiğini [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamına geçirin.

   Betik `@script` bağımsız değişkeniyle geçirilir. `@script` bağımsız değişkeninin içindeki her şey geçerli bir R kodu olmalıdır.
   
   >[!IMPORTANT]
   >Bu örnekteki kod, veritabanınızı oluştururken kaynak olarak seçebileceğiniz örnek AdventureWorksLT verilerini kullanır. Veritabanınızda farklı veriler varsa, SELECT sorgusunda kendi veritabanınızdaki tabloları kullanın. 

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'OutputDataSet <- InputDataSet;'
    , @input_data_1 = N'SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid'
    ```

   > [!NOTE]
   > Hata alırsanız Machine Learning Services (R ile) genel önizleme sürümü SQL veritabanınız için etkinleştirilmemiş olabilir. Yukarıdaki [önkoşulları](#prerequisites) inceleyin.

## <a name="run-the-code"></a>Kodu çalıştırma

1. [Sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamını yürütün.

1. **İletiler** penceresinde Ilk 20 kategori/ürün satırı döndürüldüğünden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [Azure SQL veritabanı Machine Learning Services (R ile)](sql-database-machine-learning-services-overview.md)
- [Azure SQL veritabanı 'nda basit R betikleri oluşturma ve çalıştırma Machine Learning Services (Önizleme)](sql-database-quickstart-r-create-script.md)
- [Machine Learning Services kullanarak Azure SQL veritabanı 'nda gelişmiş R işlevleri yazma (Önizleme)](sql-database-machine-learning-services-functions.md)
