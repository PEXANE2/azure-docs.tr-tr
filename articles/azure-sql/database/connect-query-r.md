---
title: Veritabanını sorgulamak için Azure SQL veritabanı Machine Learning Services (Önizleme) ile R kullanın
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu makalede, Azure SQL veritabanı ile bir R betiği kullanarak Azure SQL veritabanı 'nda bir veritabanına bağlanma ve Transact-SQL deyimlerini kullanarak sorgulama Machine Learning Services gösterilmektedir.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: sqldbrb=2 
ms.devlang: python
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph, carlrab
manager: cgronlun
ms.date: 05/29/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 7bfc00566ef24fb6d6851a7cdee402598fe15951
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84054452"
---
# <a name="quickstart-use-r-with-azure-sql-database-machine-learning-services-preview-to-query-a-database"></a>Hızlı başlangıç: veritabanını sorgulamak için Azure SQL veritabanı Machine Learning Services (Önizleme) ile R kullanın 

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu hızlı başlangıçta, Azure SQL veritabanı 'ndaki bir veritabanına bağlanmak ve verileri sorgulamak için T-SQL deyimlerini kullanmak Machine Learning Services Azure SQL veritabanı ile R 'yi kullanırsınız.

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure SQL veritabanı](single-database-create-quickstart.md)
- R özellikli [Machine Learning Services](machine-learning-services-overview.md) .
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

> [!IMPORTANT]
> Bu makaledeki betikler, **Adventure Works** veritabanını kullanmak için yazılmıştır.

R ile Machine Learning Services, veritabanı içi R betiklerini yürütmek için kullanılan Azure SQL veritabanı 'nın bir özelliğidir. Daha fazla bilgi için bkz. [R Projesi](https://www.r-project.org/).

## <a name="get-the-sql-server-connection-information"></a>SQL Server bağlantı bilgilerini al

Azure SQL veritabanı 'nda veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, Azure SQL veritabanı 'ndaki bir veritabanı için **sunucu adı** ' nın yanındaki tam sunucu adını veya Azure SQL yönetilen örneği 'nde yönetilen bir örnek için **ana bilgisayar** ' ın yanındaki tam sunucu adı ' nı gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

## <a name="create-code-to-query-your-database"></a>Veritabanınızı sorgulamak için kod oluşturma

1. **SQL Server Management Studio** açın ve veritabanınıza bağlanın.

   Bağlantı için yardıma ihtiyacınız varsa bkz. [hızlı başlangıç: Azure SQL veritabanı 'nda bir veritabanına bağlanmak ve veritabanını sorgulamak için SQL Server Management Studio kullanın](connect-query-ssms.md).

1. Tüm R betiğini [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamına geçirin.

   Betiği, `@script` bağımsız değişkeniyle geçirilir. Bağımsız değişkenin içindeki her şey `@script` geçerli bir R kodu olmalıdır.
   
   >[!IMPORTANT]
   >Bu örnekteki kod, veritabanınızı oluştururken kaynak olarak seçebileceğiniz örnek AdventureWorksLT verilerini kullanır. Veritabanınızda farklı veriler varsa, SELECT sorgusunda kendi veritabanınızdaki tabloları kullanın. 

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'OutputDataSet <- InputDataSet;'
    , @input_data_1 = N'SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid'
    ```

   > [!NOTE]
   > Herhangi bir hata alırsanız, Machine Learning Services (R ile) genel önizlemesi veritabanınız için etkinleştirilmemiş olabilir. Yukarıdaki [önkoşulları](#prerequisites) inceleyin.

## <a name="run-the-code"></a>Kodu çalıştırma

1. [Sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamını yürütün.

1. **İletiler** penceresinde Ilk 20 kategori/ürün satırı döndürüldüğünden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veritabanı 'nda ilk veritabanınızı tasarlama](design-first-database-tutorial.md)
- [Azure SQL veritabanı Machine Learning Services (R ile)](machine-learning-services-overview.md)
- [Azure SQL veritabanı 'nda basit R betikleri oluşturma ve çalıştırma Machine Learning Services (Önizleme)](r-script-create-quickstart.md)
- [Machine Learning Services kullanarak Azure SQL veritabanı 'nda gelişmiş R işlevleri yazma (Önizleme)](machine-learning-services-functions.md)
