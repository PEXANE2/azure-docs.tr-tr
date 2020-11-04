---
title: SSMS ile bağlanma
description: Azure SYNAPSE Analytics 'te adanmış bir SQL havuzuna bağlanmak ve sorgu eklemek için SQL Server Management Studio (SSMS) kullanın.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: c2fab8f02f31897a2e3d38094b9bd365f8e921e9
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93325331"
---
# <a name="connect-to-a-dedicated-sql-pool-in-azure-synapse-analytics-with-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) ile Azure SYNAPSE Analytics 'te adanmış bir SQL havuzuna bağlanma

> [!div class="op_single_selector"]
>
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)
> * [SSMS](sql-data-warehouse-query-ssms.md)

Azure SYNAPSE içinde bir veri ambarına bağlanmak ve bu ambarı sorgulamak için SQL Server Management Studio (SSMS) kullanın.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi kullanmak için şunlar gerekir:

* Mevcut bir adanmış SQL Havuzu. Bir tane oluşturmak için bkz. [adanmış BIR SQL havuzu oluşturma](create-data-warehouse-portal.md).
* SQL Server Management Studio (SSMS) yüklendi. Zaten yoksa [SSMS](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) 'Yi ücretsiz indirin.
* Tam SQL server adı. Bu bilgileri bulmak için bkz. [ADANMıŞ SQL havuzuna bağlanma](sql-data-warehouse-connect-overview.md).

## <a name="1-connect-to-your-dedicated-sql-pool"></a>1. adanmış SQL havuzunuza bağlanın

1. SSMS’i açın.
2. **Dosya**  >  **Connect Nesne Gezgini** ' i seçerek Nesne Gezgini açın.

    ![SQL Server Nesne Gezgini](./media/sql-data-warehouse-query-ssms/connect-object-explorer.png)
3. Sunucuya Bağlan penceresindeki alanları doldurun.

   ![Sunucuya bağlanma](./media/sql-data-warehouse-query-ssms/connect-object-explorer1.png)

   * **Sunucu adı**. Önceden tanımlanmış olan **sunucu adını** girin.
   * **Kimlik doğrulaması**. **SQL Server Kimlik Doğrulaması** 'nı veya **Active Directory Tümleşik Kimlik Doğrulaması** 'nı seçin.
   * **Kullanıcı Adı** ve **Parola**. Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adını ve parolayı girin.
   * **Bağlan** 'a tıklayın.
4. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.

   ![AdventureWorksDW'yi araştırma](./media/sql-data-warehouse-query-ssms/explore-tables.png)

## <a name="2-run-a-sample-query"></a>2. örnek bir sorgu çalıştırın

Artık veritabanınızla bağlantı kurulduğuna göre bir sorgu yazalım.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu** ’yu seçin. Yeni bir sorgu penceresi açılır.

   ![Yeni sorgu](./media/sql-data-warehouse-query-ssms/new-query.png)
3. Aşağıdaki T-SQL sorgusunu sorgu penceresine kopyalayın:

   ```sql
   SELECT COUNT(*) FROM dbo.FactInternetSales;
   ```

4. Sorguyu çalıştırın `Execute` veya şu kısayolu kullanın: `F5` .

   ![Sorgu çalıştırma](./media/sql-data-warehouse-query-ssms/execute-query.png)
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.

   ![Sorgu sonuçları](./media/sql-data-warehouse-query-ssms/results.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık bağlanıp sorgulama yaptığınızda, [Power BI verileri görselleştirmeyi](sql-data-warehouse-get-started-visualize-with-power-bi.md)deneyin. Ortamınızı Azure Active Directory kimlik doğrulaması için yapılandırmak üzere bkz. [ADANMıŞ SQL havuzunda kimlik doğrulama](sql-data-warehouse-authentication.md).
