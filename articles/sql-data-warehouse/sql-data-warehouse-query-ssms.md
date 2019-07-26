---
title: Azure SQL veri ambarı 'na bağlanma-SSMS | Microsoft Docs
description: Azure SQL veri ambarı 'na bağlanmak ve sorgu yapmak için SQL Server Management Studio (SSMS) kullanın.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 67707b64bb6018b322ba980535651c126a526eb7
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479544"
---
# <a name="connect-to-sql-data-warehouse-with-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) ile SQL Data Warehouse 'a bağlanma
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Azure SQL veri ambarı 'na bağlanmak ve sorgu yapmak için SQL Server Management Studio (SSMS) kullanın. 

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi kullanmak için şunlar gerekir:

* Var olan bir SQL veri ambarı. Bir tane oluşturmak için bkz. [SQL Data Warehouse oluşturma][Create a SQL Data Warehouse].
* SQL Server Management Studio (SSMS) yüklendi. Zaten yoksa [SSMS 'yi][Install SSMS] ücretsiz olarak yükleyebilirsiniz.
* Tam SQL server adı. Bunu bulmak için bkz. [SQL Data Warehouse'a bağlanma][Connect to SQL Data Warehouse].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. SQL Data Warehouse'unuza bağlanma
1. SSMS’i açın.
2. Nesne Gezgini açın. Bunu yapmak için **Dosya** > **Connect Nesne Gezgini**seçin.
   
    ![SQL Server Object Explorer][1]
3. Sunucuya Bağlan penceresindeki alanları doldurun.
   
    ![Sunucuya bağlanma][2]
   
   * **Sunucu adı** Önceden tanımlanmış olan **sunucu adını** girin.
   * **Kimlik Doğrulaması**. **SQL Server Kimlik Doğrulaması**'nı veya **Active Directory Tümleşik Kimlik Doğrulaması**'nı seçin.
   * **Kullanıcı Adı** ve **Parola**. Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adını ve parolayı girin.
   * **Bağlan**'a tıklayın.
4. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.
   
    ![AdventureWorksDW'yi araştırma][3]

## <a name="2-run-a-sample-query"></a>2. Örnek sorgu çalıştırma
Artık veritabanınızla bağlantı kurulduğuna göre bir sorgu yazalım.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu][4]
3. Şu TSQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Sorguyu çalıştırın. Bunu yapmak için aşağıdaki kısayola `Execute` tıklayın veya kullanın:. `F5`
   
    ![Sorguyu çalıştır][5]
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.
   
    ![Sorgu sonuçları][6]

## <a name="next-steps"></a>Sonraki adımlar
Artık bağlanıp sorgulama yapabildiğinize göre [PowerBI ile verileri görselleştirmeyi][visualizing the data with PowerBI] deneyin.

Ortamınızı Azure Active Directory kimlik doğrulaması için yapılandırmak üzere bkz. [SQL Data Warehouse’da kimlik doğrulama][Authenticate to SQL Data Warehouse].

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate to SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing the data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

<!--Other-->
[Azure portal]: https://portal.azure.com
[Install SSMS]: https://msdn.microsoft.com/library/hh213248.aspx


<!--Image references-->

[1]: media/sql-data-warehouse-query-ssms/connect-object-explorer.png
[2]: media/sql-data-warehouse-query-ssms/connect-object-explorer1.png
[3]: media/sql-data-warehouse-query-ssms/explore-tables.png
[4]: media/sql-data-warehouse-query-ssms/new-query.png
[5]: media/sql-data-warehouse-query-ssms/execute-query.png
[6]: media/sql-data-warehouse-query-ssms/results.png
