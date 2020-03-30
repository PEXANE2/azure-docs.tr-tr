---
title: VSTS ile bağlantı kurun
description: Azure Synapse Analytics'i Visual Studio ile sorgulayın.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 08/15/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: e986515a911cf1bbd88dfc73c56efcc6e81826d6
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351398"
---
# <a name="connect-to-azure-synapse-analytics-with-visual-studio-and-ssdt"></a>Visual Studio ve SSDT ile Azure Synapse Analytics'e bağlanın
> [!div class="op_single_selector"]
> * [Güç BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Makine Öğrenimi](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Azure Synapse içindeki bir SQL havuzunda yalnızca birkaç dakika içinde sorguyapmak için Visual Studio'yı kullanın. Bu yöntem Visual Studio 2019'da SQL Server Data Tools (SSDT) uzantısını kullanır. 

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi kullanmak için şunlar gerekir:

* Varolan bir SQL havuzu. Bir tane oluşturmak için [bkz.](create-data-warehouse-portal.md)
* Visual Studio için SSDT. Visual Studio'nuz varsa, visual studio için ssdt'niz vardır. Yükleme yönergeleri ve seçenekleri için bkz. [Visual Studio’yu ve SSDT’yi yükleme](sql-data-warehouse-install-visual-studio.md).
* Tam SQL server adı. Bu bilgileri bulmak için sql [havuzuna bağlan'a](sql-data-warehouse-connect-overview.md)bakın.

## <a name="1-connect-to-your-sql-pool"></a>1. SQL havuzunuza bağlanın
1. Görsel Stüdyo 2019'u açın.
2. SQL Server Object **Explorer'ı Görüntüle'yi** > seçerek SQL**Server Object Explorer'ı**açın.
   
    ![SQL Server Nesne Gezgini](./media/sql-data-warehouse-query-visual-studio/open-ssdt.png)
3. **SQL Server ekle** simgesine tıklayın.
   
    ![SQL Server ekleme](./media/sql-data-warehouse-query-visual-studio/add-server.png)
4. Sunucuya Bağlan penceresindeki alanları doldurun.
   
    ![Sunucuya bağlanma](./media/sql-data-warehouse-query-visual-studio/connection-dialog.png)
   
   * **Sunucu adı**. Önceden tanımlanmış olan **sunucu adını** girin.
   * **Kimlik doğrulama.** **SQL Server Kimlik Doğrulaması**'nı veya **Active Directory Tümleşik Kimlik Doğrulaması**'nı seçin.
   * **Kullanıcı Adı** ve **Parola**. Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adını ve parolayı girin.
   * **Bağlan**'a tıklayın.
5. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.
   
    ![AdventureWorksDW'yi araştırma](./media/sql-data-warehouse-query-visual-studio/explore-sample.png)

## <a name="2-run-a-sample-query"></a>2. Örnek bir sorgu çalıştırma
Artık veritabanınızla bağlantı kurulduğuna göre bir sorgu yazalım.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu](./media/sql-data-warehouse-query-visual-studio/new-query2.png)
3. Aşağıdaki T-SQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Yeşil oku tıklatarak sorguyu çalıştırın veya aşağıdaki `CTRL` + `SHIFT` + `E`kısayolu kullanın: .
   
    ![Sorgu çalıştırma](./media/sql-data-warehouse-query-visual-studio/run-query.png)
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.
   
    ![Sorgu sonuçları](./media/sql-data-warehouse-query-visual-studio/query-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık bağlanıp sorgulayabildiğinize göre, [Power BI ile verileri görselleştirmeyi](sql-data-warehouse-get-started-visualize-with-power-bi.md)deneyin.

Azure Active Directory kimlik doğrulaması için ortamınızı yapılandırmak [için, Kimlik Doğrulaması'nın SQL havuzuna](sql-data-warehouse-authentication.md)olduğunu görme.'ye bakın.