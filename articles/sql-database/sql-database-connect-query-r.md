---
title: Veritabanını sorgulamak için Machine Learning Services ile R'yi kullanma (önizleme)
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu makalede, Azure SQL veritabanına bağlanmak ve Transact-SQL deyimlerini kullanarak sorgulamak için Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile bir R komut dosyasınasıl kullanılacağı nı gösterir.
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
ms.openlocfilehash: 7103afc29e4021d950d9a3634b190f4439ecfe8d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76768507"
---
# <a name="quickstart-use-r-with-machine-learning-services-to-query-an-azure-sql-database-preview"></a>Quickstart: Azure SQL veritabanını sorgulamak için Machine Learning Services ile R'yi kullanın (önizleme)

Bu hızlı başlatmada, Azure SQL veritabanına bağlanmak için Machine Learning Services ile R'yi ve verileri sorgulamak için T-SQL deyimlerini kullanırsınız.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Azure SQL veritabanı](sql-database-single-database-get-started.md)
- R özellikli [Makine Öğrenimi Hizmetleri](sql-database-machine-learning-services-overview.md) etkindir. [Önizleme için kaydolun](sql-database-machine-learning-services-overview.md#signup).
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

> [!IMPORTANT]
> Bu makaledeki komut dosyaları Adventure **Works** veritabanını kullanmak için yazılmıştır.

> [!NOTE]
> Genel önizleme sırasında Microsoft, varolan veya yeni veritabanınız için makine öğrenimini etkinleştirecek, ancak yönetilen örnek dağıtım seçeneği şu anda desteklenmez.

R özellikli Machine Learning Services, veritabanı içi R komut dosyalarını yürütmek için kullanılan Azure SQL veritabanının bir özelliğidir. Daha fazla bilgi için [R Projesi'ne](https://www.r-project.org/)bakın.

## <a name="get-sql-server-connection-information"></a>SQL sunucu bağlantı bilgilerini alın

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Gelecek yordamlar için tam nitelikli sunucu adı veya ana bilgisayar adı, veritabanı adı ve giriş bilgilerine ihtiyacınız vardır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. **SQL veritabanlarına** veya **SQL yönetilen örnekler** sayfasına gidin.

3. Genel **Bakış** sayfasında, tek bir veritabanı için **Sunucu adının** yanındaki tam nitelikli sunucu adını veya yönetilen bir örnek için **Host'un** yanındaki tam nitelikli sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine tıklayın ve **Kopyasimgesini** seçin.

## <a name="create-code-to-query-your-sql-database"></a>SQL veritabanınızı sorgulamak için kod oluşturma

1. **SQL Server Management Studio'nuzu** açın ve SQL veritabanınıza bağlanın.

   Bağlanmak için yardıma ihtiyacınız varsa, [Azure SQL veritabanını bağlamak ve sorgulamak için Hızlı Başlangıç: SQL Server Management Studio'yı kullanın.](sql-database-connect-query-ssms.md)

1. Tüm R komut dosyasını [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) depolanan yordamına geçirin.

   Komut dosyası bağımsız `@script` değişkenden geçirilir. Bağımsız değişkenin `@script` içindeki her şey geçerli Bir R kodu olmalıdır.
   
   >[!IMPORTANT]
   >Bu örnekteki kod, veritabanınızı oluştururken kaynak olarak seçebileceğiniz örnek AdventureWorksLT verilerini kullanır. Veritabanınızda farklı veriler varsa, SELECT sorgusunda kendi veritabanınızdaki tabloları kullanın. 

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'OutputDataSet <- InputDataSet;'
    , @input_data_1 = N'SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid'
    ```

   > [!NOTE]
   > Hata alırsanız Machine Learning Services (R ile) genel önizleme sürümü SQL veritabanınız için etkinleştirilmemiş olabilir. Yukarıdaki [Ön koşullara](#prerequisites) bakın.

## <a name="run-the-code"></a>Kodu çalıştırma

1. Depolanan [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) yordamı çalıştırın.

1. En iyi 20 Kategori/Ürün satırının **İletiler** penceresinde döndürüldünden doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [Azure SQL Veritabanı Makine Öğrenme Hizmetleri (R ile)](sql-database-machine-learning-services-overview.md)
- [Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde basit R komut dosyaları oluşturun ve çalıştırın (önizleme)](sql-database-quickstart-r-create-script.md)
- [Machine Learning Services 'i kullanarak Azure SQL Veritabanı'na gelişmiş R işlevleri yazın (önizleme)](sql-database-machine-learning-services-functions.md)
