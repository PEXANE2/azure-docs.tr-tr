---
title: 'SSMS: Bağlan ve Synapse SQL sorgulayın'
description: Azure Synapse Analytics'te Synapse SQL'e bağlanmak ve bunları sorgulamak için SQL Server Management Studio'yı (SSMS) kullanın.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 704da86fd1d816dbf5d6cd9cf67dfee53fce2622
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423743"
---
# <a name="connect-to-synapse-sql-with-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) ile Synapse SQL'e bağlanın
> [!div class="op_single_selector"]
> * [Azure Data Studio](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [Sqlcmd](../sql/get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)
> 
> 

[SQL Server Management Studio'ya (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) Azure Synapse Analytics'e bağlanmak ve sql isteğe bağlı (önizleme) veya SQL havuzu kaynakları aracılığıyla Synapse SQL'e bağlanmak ve bunları sorgulamak için sql server management studio'u (SSMS) kullanabilirsiniz. 

### <a name="supported-tools-for-sql-on-demand-preview"></a>İsteğe bağlı SQL için desteklenen araçlar (önizleme)

SSMS, 18.5 sürümünde bağlanmak ve sorgulama gibi sınırlı özelliklerle kısmen desteklenir. [Azure Veri Stüdyosu](/sql/azure-data-studio/download-azure-data-studio) tamamen desteklenir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdaki ön koşullara sahip olduğunuzdan emin olun:  

* [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms). 
* SQL havuzu için varolan bir veri ambarı gerekir. Bir tane oluşturmak için [bkz.](../quickstart-create-sql-pool.md) İsteğe bağlı SQL için, oluşturma zamanında çalışma alanınızda zaten bir tane sağlanmış durumdadır. 
* Tam nitelikli SQL Server adı. Bunu bulmak için Bkz. [Synapse SQL'e Bağlan.](connect-overview.md)

## <a name="connect"></a>Bağlan

### <a name="sql-pool"></a>SQL havuzu

SQL havuzukullanarak Synapse SQL'e bağlanmak için aşağıdaki adımları izleyin: 

1. SQL Server Management Studio 'yı (SSMS) açın. 
1. **Sunucuya Bağlan** iletişim kutusunda, alanları doldurun ve sonra **Bağlan'ı**seçin: 
  
    ![Sunucuya bağlanma](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/connect-object-explorer1.png)
   
   * **Sunucu adı**: Daha önce tanımlanan **sunucu adını** girin.
   * **Kimlik Doğrulama**: **SQL Server Authentication** veya Active **Directory Integrated Authentication**gibi bir kimlik doğrulama türü seçin.
   * **Kullanıcı Adı** ve **Şifre**: Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adınızı ve şifrenizi girin.

1. **Object Explorer'da**Azure SQL Sunucunuzu genişletin. Örnek AdventureWorksDW veritabanı gibi sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Tabloları görmek için veritabanını genişletebilirsiniz:
   
    ![AdventureWorksDW'yi araştırma](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/explore-tables.png)


### <a name="sql-on-demand-preview"></a>İsteğe bağlı SQL (önizleme)

İsteğe bağlı SQL kullanarak Synapse SQL'e bağlanmak için aşağıdaki adımları izleyin: 

1. SQL Server Management Studio 'yı (SSMS) açın.
1. **Sunucuya Bağlan** iletişim kutusunda, alanları doldurun ve sonra **Bağlan'ı**seçin: 
   
    ![Sunucuya bağlanma](./media/get-started-ssms/connect-object-explorer1.png)
   
   * **Sunucu adı**: Daha önce tanımlanan **sunucu adını** girin.
   * **Kimlik Doğrulama**: **SQL Server Authentication** veya Active **Directory Integrated Authentication**gibi bir kimlik doğrulama türü seçin:
   * **Kullanıcı Adı** ve **Şifre**: Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adınızı ve şifrenizi girin.
   * **Bağlan**'a tıklayın.

4. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki içeriği görmek için *demoyu* genişletin.
   
    ![AdventureWorksDW'yi araştırma](./media/get-started-ssms/explore-tables.png)


## <a name="run-a-sample-query"></a>Örnek sorgu çalıştırma

### <a name="sql-pool"></a>SQL havuzu

Artık bir veritabanı bağlantısı kurulduğuna göre, verileri sorgulayabilirsiniz.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/new-query.png)
3. Bu T-SQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Sorguyu çalıştırın. Bunu yapmak için `Execute` aşağıdaki kısayolu tıklatın `F5`veya kullanın: .
   
    ![Sorgu çalıştırma](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/execute-query.png)
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.
   
    ![Sorgu sonuçları](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/results.png)

### <a name="sql-on-demand"></a>İsteğe bağlı SQL

Artık bir veritabanı bağlantısı kurduğunuza göre, verileri sorgulayabilirsiniz.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu](./media/get-started-ssms/new-query.png)
3. Aşağıdaki T-SQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM demo.dbo.usPopulationView
    ```
4. Sorguyu çalıştırın. Bunu yapmak için `Execute` aşağıdaki kısayolu tıklatın `F5`veya kullanın: .
   
    ![Sorgu çalıştırma](./media/get-started-ssms/execute-query.png)
5. Sorgu sonuçlarına bakın. Bu örnekte, usPopulationView görünümü 3664512 satır vardır.
   
    ![Sorgu sonuçları](./media/get-started-ssms/results.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık bağlanıp sorgulayabildiğinize göre, [Power BI ile verileri görselleştirmeyi](get-started-power-bi-professional.md)deneyin.

Azure Active Directory kimlik doğrulaması için ortamınızı yapılandırmak için Bkz. Kimlik Doğrulama ile [Synapse SQL.](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

