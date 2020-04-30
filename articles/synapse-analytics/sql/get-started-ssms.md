---
title: 'SSMS: Connect ve Query SYNAPSE SQL'
description: Azure SYNAPSE Analytics 'te SYNAPSE SQL 'e bağlanmak ve sorgulamak için SQL Server Management Studio (SSMS) kullanın.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 704da86fd1d816dbf5d6cd9cf67dfee53fce2622
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81423743"
---
# <a name="connect-to-synapse-sql-with-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) ile SYNAPSE SQL 'e bağlanma
> [!div class="op_single_selector"]
> * [Azure Data Studio](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [sqlcmd](../sql/get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)
> 
> 

[SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) kullanarak Azure SYNAPSE ANALYTICS 'te SQL isteğe bağlı (Önizleme) veya SQL havuzu kaynakları aracılığıyla SYNAPSE SQL 'e bağlanabilir ve bunları sorgulayabilirsiniz. 

### <a name="supported-tools-for-sql-on-demand-preview"></a>İsteğe bağlı SQL için desteklenen araçlar (Önizleme)

SSMS, sürüm 18,5 ' den başlayarak, bağlanma ve sorgulama gibi sınırlı özelliklerle kısmen desteklenir. [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) tam olarak desteklenir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdaki önkoşullara sahip olduğunuzdan emin olun:  

* [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms). 
* SQL havuzu için mevcut bir veri ambarına ihtiyacınız vardır. Bir tane oluşturmak için bkz. [SQL havuzu oluşturma](../quickstart-create-sql-pool.md). İsteğe bağlı SQL için, oluşturma sırasında çalışma alanınızda bir tane zaten sağlanmış olur. 
* Tam nitelikli SQL Server adı. Bunu bulmak için bkz. [SYNAPSE SQL 'e bağlanma](connect-overview.md).

## <a name="connect"></a>Bağlan

### <a name="sql-pool"></a>SQL havuzu

SQL havuzu kullanarak SYNAPSE SQL 'e bağlanmak için şu adımları izleyin: 

1. SQL Server Management Studio (SSMS) açın. 
1. **Sunucuya Bağlan** iletişim kutusunda alanları doldurup **Bağlan**' ı seçin. 
  
    ![Sunucuya bağlanma](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/connect-object-explorer1.png)
   
   * **Sunucu adı**: daha önce tanımlanan **sunucu adını** girin.
   * **Kimlik doğrulaması**: **SQL Server kimlik doğrulaması** veya **Active Directory tümleşik kimlik doğrulaması**gibi bir kimlik doğrulama türü seçin.
   * **Kullanıcı adı** ve **parola**: yukarıda SQL Server kimlik doğrulaması seçildiyse, Kullanıcı adınızı ve parolanızı girin.

1. Azure SQL Server **Nesne Gezgini**' ni genişletin. Sunucu ile ilişkili veritabanlarını (örneğin, örnek AdventureWorksDW veritabanı) görüntüleyebilirsiniz. Tabloları görmek için veritabanını genişletebilirsiniz:
   
    ![AdventureWorksDW'yi araştırma](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/explore-tables.png)


### <a name="sql-on-demand-preview"></a>İsteğe bağlı SQL (Önizleme)

İsteğe bağlı SQL kullanarak SYNAPSE SQL 'e bağlanmak için şu adımları izleyin: 

1. SQL Server Management Studio (SSMS) açın.
1. **Sunucuya Bağlan** iletişim kutusunda alanları doldurup **Bağlan**' ı seçin. 
   
    ![Sunucuya bağlanma](./media/get-started-ssms/connect-object-explorer1.png)
   
   * **Sunucu adı**: daha önce tanımlanan **sunucu adını** girin.
   * **Kimlik doğrulaması**: **SQL Server kimlik doğrulaması** veya **Active Directory tümleşik kimlik doğrulaması**gibi bir kimlik doğrulama türü seçin:
   * **Kullanıcı adı** ve **parola**: yukarıda SQL Server kimlik doğrulaması seçildiyse, Kullanıcı adınızı ve parolanızı girin.
   * **Bağlan**'a tıklayın.

4. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki içeriği görmek için *demo* ' i genişletin.
   
    ![AdventureWorksDW'yi araştırma](./media/get-started-ssms/explore-tables.png)


## <a name="run-a-sample-query"></a>Örnek sorgu çalıştırma

### <a name="sql-pool"></a>SQL havuzu

Bir veritabanı bağlantısı kuruldığına göre, verileri sorgulayabilirsiniz.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/new-query.png)
3. Bu T-SQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Sorguyu çalıştırın. Bunu yapmak için aşağıdaki kısayola `Execute` tıklayın veya kullanın: `F5`.
   
    ![Sorgu çalıştırma](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/execute-query.png)
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.
   
    ![Sorgu sonuçları](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/results.png)

### <a name="sql-on-demand"></a>İsteğe bağlı SQL

Bir veritabanı bağlantısı kurdığınıza göre, verileri sorgulayabilirsiniz.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu](./media/get-started-ssms/new-query.png)
3. Aşağıdaki T-SQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM demo.dbo.usPopulationView
    ```
4. Sorguyu çalıştırın. Bunu yapmak için aşağıdaki kısayola `Execute` tıklayın veya kullanın: `F5`.
   
    ![Sorgu çalıştırma](./media/get-started-ssms/execute-query.png)
5. Sorgu sonuçlarına bakın. Bu örnekte, usPopulationView görünümü 3664512 satıra sahiptir.
   
    ![Sorgu sonuçları](./media/get-started-ssms/results.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık bağlanıp sorgulama yaptığınızda, [Power BI verileri görselleştirmeyi](get-started-power-bi-professional.md)deneyin.

Ortamınızı Azure Active Directory kimlik doğrulaması için yapılandırmak üzere bkz. [SYNAPSE SQL 'de kimlik](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)doğrulama.

