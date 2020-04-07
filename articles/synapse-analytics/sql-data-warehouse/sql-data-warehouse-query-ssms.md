---
title: SSMS ile bağlanma
description: Azure Synapse Analytics'e bağlanmak ve bunları sorgulamak için SQL Server Management Studio'yı (SSMS) kullanın.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 0d65ea9b684439cab384bbaa09fcdef1302b6bcf
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743854"
---
# <a name="connect-to-azure-synapse-analytics-with-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) ile Azure Synapse Analytics'e bağlanın

> [!div class="op_single_selector"]
>
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Makine Öğrenimi](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)
> * [SSMS](sql-data-warehouse-query-ssms.md)

Azure Synapse'deki bir veri ambarına bağlanmak ve bunları sorgulamak için SQL Server Management Studio'yu (SSMS) kullanın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi kullanmak için şunlar gerekir:

* Varolan bir SQL havuzu. Bir tane oluşturmak için [bkz.](create-data-warehouse-portal.md)
* SQL Server Management Studio (SSMS) yüklendi. Zaten yoksa Ücretsiz [SSMS indirin.](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* Tam SQL server adı. Bu bilgileri bulmak için sql [havuzuna bağlan'a](sql-data-warehouse-connect-overview.md)bakın.

## <a name="1-connect-to-your-sql-pool"></a>1. SQL havuzunuza bağlanın

1. SSMS’i açın.
2. **Dosya** > **Bağlan Nesne Gezgini'ni**seçerek Nesne Gezgini'ni açın.

    ![SQL Server Nesne Gezgini](./media/sql-data-warehouse-query-ssms/connect-object-explorer.png)
3. Sunucuya Bağlan penceresindeki alanları doldurun.

   ![Sunucuya bağlanma](./media/sql-data-warehouse-query-ssms/connect-object-explorer1.png)

   * **Sunucu adı**. Önceden tanımlanmış olan **sunucu adını** girin.
   * **Kimlik doğrulama.** **SQL Server Kimlik Doğrulaması**'nı veya **Active Directory Tümleşik Kimlik Doğrulaması**'nı seçin.
   * **Kullanıcı Adı** ve **Parola**. Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adını ve parolayı girin.
   * **Bağlan**'a tıklayın.
4. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.

   ![AdventureWorksDW'yi araştırma](./media/sql-data-warehouse-query-ssms/explore-tables.png)

## <a name="2-run-a-sample-query"></a>2. Örnek bir sorgu çalıştırma

Artık veritabanınızla bağlantı kurulduğuna göre bir sorgu yazalım.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.

   ![Yeni sorgu](./media/sql-data-warehouse-query-ssms/new-query.png)
3. Aşağıdaki T-SQL sorgusunu sorgu penceresine kopyalayın:

   ```sql
   SELECT COUNT(*) FROM dbo.FactInternetSales;
   ```

4. Aşağıdaki kısayolu tıklatarak sorguyu `Execute` çalıştırın: . `F5`

   ![Sorgu çalıştırma](./media/sql-data-warehouse-query-ssms/execute-query.png)
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.

   ![Sorgu sonuçları](./media/sql-data-warehouse-query-ssms/results.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık bağlanıp sorgulayabildiğinize göre, [Power BI ile verileri görselleştirmeyi](sql-data-warehouse-get-started-visualize-with-power-bi.md)deneyin. Azure Active Directory kimlik doğrulaması için ortamınızı yapılandırmak [için, Kimlik Doğrulaması'nın SQL havuzuna](sql-data-warehouse-authentication.md)olduğunu görme.'ye bakın.
