---
title: SSMS ile bağlanma
description: Azure SYNAPSE Analytics 'e bağlanmak ve bunları sorgulamak için SQL Server Management Studio (SSMS) kullanın.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 2109402a874ff8c722bd05e1e5cb62b461cb2292
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78198639"
---
# <a name="connect-to-azure-synapse-analytics-with-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) ile Azure SYNAPSE Analytics 'e bağlanma
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Azure SYNAPSE içinde bir veri ambarına bağlanmak ve bu ambarı sorgulamak için SQL Server Management Studio (SSMS) kullanın. 

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi kullanmak için şunlar gerekir:

* Mevcut bir SQL Havuzu. Bir tane oluşturmak için bkz. [SQL havuzu oluşturma](sql-data-warehouse-get-started-provision.md).
* SQL Server Management Studio (SSMS) yüklendi. Zaten yoksa [SSMS 'yi](https://msdn.microsoft.com/library/hh213248.aspx) ücretsiz olarak yükleyebilirsiniz.
* Tam SQL server adı. Bu bilgileri bulmak için bkz. [SQL havuzuna bağlanma](sql-data-warehouse-connect-overview.md).

## <a name="1-connect-to-your-sql-pool"></a>1. SQL havuzunuza bağlanma
1. SSMS’i açın.
2. Nesne Gezgini **dosya** > **Connect Nesne Gezgini**' i seçerek açın.
   
    ![SQL Server Nesne Gezgini](media/sql-data-warehouse-query-ssms/connect-object-explorer.png)
3. Sunucuya Bağlan penceresindeki alanları doldurun.
   
    ![Sunucuya bağlanma](media/sql-data-warehouse-query-ssms/connect-object-explorer1.png)
   
   * **Sunucu adı** Önceden tanımlanmış olan **sunucu adını** girin.
   * **Kimlik Doğrulaması**. **SQL Server Kimlik Doğrulaması**'nı veya **Active Directory Tümleşik Kimlik Doğrulaması**'nı seçin.
   * **Kullanıcı Adı** ve **Parola**. Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adını ve parolayı girin.
   * **Bağlan**'a tıklayın.
4. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.
   
    ![AdventureWorksDW'yi araştırma](media/sql-data-warehouse-query-ssms/explore-tables.png)

## <a name="2-run-a-sample-query"></a>2. örnek bir sorgu çalıştırın
Artık veritabanınızla bağlantı kurulduğuna göre bir sorgu yazalım.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu]( media/sql-data-warehouse-query-ssms/new-query.png)
3. Aşağıdaki T-SQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. `Execute` tıklayarak sorguyu çalıştırın veya şu kısayolu kullanın: `F5`.
   
    ![Sorgu çalıştırma](media/sql-data-warehouse-query-ssms/execute-query.png)
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.
   
    ![Sorgu sonuçları](media/sql-data-warehouse-query-ssms/results.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık bağlanıp sorgulama yaptığınızda, [Power BI verileri görselleştirmeyi](sql-data-warehouse-get-started-visualize-with-power-bi.md )deneyin.
Ortamınızı Azure Active Directory kimlik doğrulaması için yapılandırmak üzere bkz. [SQL havuzunda kimlik](sql-data-warehouse-authentication.md)doğrulama.
