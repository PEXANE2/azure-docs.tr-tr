---
title: Visual Studio ve SSDT ile SYNAPSE SQL 'e bağlanma ve sorgulama
description: Azure SYNAPSE Analytics kullanarak SQL havuzunu sorgulamak için Visual Studio 'Yu kullanın.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 5e725df5c875f23d82da1fda19bcdf28db548fc8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81428569"
---
# <a name="connect-to-synapse-sql-with-visual-studio-and-ssdt"></a>Visual Studio ve SSDT ile SYNAPSE SQL 'e bağlanma
> [!div class="op_single_selector"]
> * [Azure Data Studio](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](get-started-visual-studio.md)
> * [sqlcmd](get-started-connect-sqlcmd.md) 
> * [SSMS](get-started-ssms.md)
> 
> 

Azure SYNAPSE Analytics kullanarak SQL havuzunu sorgulamak için Visual Studio 'Yu kullanın. Bu yöntem, Visual Studio 2019 ' de SQL Server Veri Araçları (SSDT) uzantısını kullanır. 

### <a name="supported-tools-for-sql-on-demand-preview"></a>İsteğe bağlı SQL için desteklenen araçlar (Önizleme)

Visual Studio şu anda SQL isteğe bağlı (Önizleme) için desteklenmiyor. Ancak, Azure Data Studi (Önizleme) o, tam olarak desteklenen bir araçtır. SQL Server Management Studio sürüm 18,4 ' den kısmen desteklenir ve bağlanma ve sorgulama gibi sınırlı özelliklere sahiptir.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi kullanmak için aşağıdaki bileşenlere sahip olmanız gerekir:

* Mevcut bir SQL Havuzu. Bir tane yoksa, bu önkoşulu gerçekleştirmek için bkz. [SQL havuzu oluşturma](../sql-data-warehouse/create-data-warehouse-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) .
* Visual Studio için SSDT. Visual Studio kullanıyorsanız büyük olasılıkla bu bileşene sahipsiniz demektir. Yükleme yönergeleri ve seçenekleri için bkz. [Visual Studio’yu ve SSDT’yi yükleme](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).
* Tam SQL server adı. Bunu bulmak için bkz. [SQL havuzuna bağlanma](connect-overview.md).

## <a name="1-connect-to-sql-pool"></a>1. SQL havuzuna Bağlan
1. Visual Studio 2019 ' i açın.
2. SQL Server Nesne Gezgini açın. Bunu yapmak için**SQL Server Nesne Gezgini** **görüntüle** > ' yi seçin.
   
    ![SQL Server Nesne Gezgini](./media/get-started-visual-studio/open-ssdt.png)
3. **SQL Server ekle** simgesine tıklayın.
   
    ![SQL Server ekleme](./media/get-started-visual-studio/add-server.png)
4. Sunucuya Bağlan penceresindeki alanları doldurun.
   
    ![Sunucuya bağlanma](./media/get-started-visual-studio/connection-dialog.png)
   
   * **Sunucu adı**: daha önce tanımlanan **sunucu adını** girin.
   * **Kimlik doğrulaması**: **SQL Server kimlik doğrulaması** veya **Active Directory tümleşik kimlik doğrulaması**seçin:
   * **Kullanıcı adı** ve **parola**: yukarıda SQL Server kimlik doğrulaması seçildiyse, Kullanıcı adınızı ve parolanızı girin.
   * **Bağlan**'a tıklayın.
5. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.
   
    ![AdventureWorksDW'yi araştırma](./media/get-started-visual-studio/explore-sample.png)

## <a name="2-run-a-sample-query"></a>2. örnek bir sorgu çalıştırın
Artık veritabanınıza bir bağlantı kuruldığına göre bir sorgu yazacaksınız.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu](./media/get-started-visual-studio/new-query2.png)
3. Aşağıdaki T-SQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Sorguyu çalıştırın. Bunu gerçekleştirmek için yeşil ok simgesine tıklayın veya şu kısayolu kullanın: `CTRL`+`SHIFT`+`E`.
   
    ![Sorgu çalıştırma](./media/get-started-visual-studio/run-query.png)
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.
   
    ![Sorgu sonuçları](./media/get-started-visual-studio/query-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık bağlanıp sorgulama yaptığınızda, [Power BI verileri görselleştirmeyi](get-started-power-bi-professional.md)deneyin.
Ortamınızı Azure Active Directory kimlik doğrulaması için yapılandırmak üzere bkz. [SQL havuzunda kimlik](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)doğrulama.
 