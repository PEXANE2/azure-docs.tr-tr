---
title: Visual Studio ve SSDT ile Synapse SQL'e bağlanma ve sorgulama
description: Azure Synapse Analytics'i kullanarak SQL havuzusorgulamak için Visual Studio'yı kullanın.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 5e725df5c875f23d82da1fda19bcdf28db548fc8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428569"
---
# <a name="connect-to-synapse-sql-with-visual-studio-and-ssdt"></a>Visual Studio ve SSDT ile Synapse SQL'e bağlanın
> [!div class="op_single_selector"]
> * [Azure Data Studio](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](get-started-visual-studio.md)
> * [Sqlcmd](get-started-connect-sqlcmd.md) 
> * [SSMS](get-started-ssms.md)
> 
> 

Azure Synapse Analytics'i kullanarak SQL havuzusorgulamak için Visual Studio'yı kullanın. Bu yöntem Visual Studio 2019'da SQL Server Data Tools (SSDT) uzantısını kullanır. 

### <a name="supported-tools-for-sql-on-demand-preview"></a>İsteğe bağlı SQL için desteklenen araçlar (önizleme)

Visual studio şu anda SQL isteğe bağlı (önizleme) için desteklenmez. Ancak, Azure Data Studi (preview)o tam olarak desteklenen bir araçtır. SQL Server Management Studio kısmen sürüm 18.4'ten desteklenir ve bağlanma ve sorgulama gibi sınırlı özelliklere sahiptir.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi kullanmak için aşağıdaki bileşenlere sahip olmanız gerekir:

* Varolan bir SQL havuzu. Yoksa, bu ön koşulu tamamlamak için [bir SQL havuzu oluştur'a](../sql-data-warehouse/create-data-warehouse-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) bakın.
* Visual Studio için SSDT. Visual Studio'nuz varsa, muhtemelen bu bileşene zaten sahipsinizdir. Yükleme yönergeleri ve seçenekleri için bkz. [Visual Studio’yu ve SSDT’yi yükleme](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).
* Tam SQL server adı. Bunu bulmak için SQL [havuzuna bağlan'a](connect-overview.md)bakın.

## <a name="1-connect-to-sql-pool"></a>1. SQL havuzuna bağlanın
1. Görsel Stüdyo 2019'u açın.
2. SQL Server Object Explorer'ı açın. Bunu yapmak için**SQL Server Object Explorer'ı** **Görüntüle'yi** > seçin.
   
    ![SQL Server Nesne Gezgini](./media/get-started-visual-studio/open-ssdt.png)
3. **SQL Server ekle** simgesine tıklayın.
   
    ![SQL Server ekleme](./media/get-started-visual-studio/add-server.png)
4. Sunucuya Bağlan penceresindeki alanları doldurun.
   
    ![Sunucuya bağlanma](./media/get-started-visual-studio/connection-dialog.png)
   
   * **Sunucu adı**: Daha önce tanımlanan **sunucu adını** girin.
   * **Kimlik Doğrulama**: **SELECT SQL Server Authentication** veya Active **Directory Integrated Authentication**:
   * **Kullanıcı Adı** ve **Şifre**: Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adınızı ve şifrenizi girin.
   * **Bağlan**'a tıklayın.
5. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.
   
    ![AdventureWorksDW'yi araştırma](./media/get-started-visual-studio/explore-sample.png)

## <a name="2-run-a-sample-query"></a>2. Örnek bir sorgu çalıştırma
Veritabanınıza bir bağlantı kurulduğuna göre, bir sorgu yazarsınız.

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
Artık bağlanıp sorgulayabildiğinize göre, [Power BI ile verileri görselleştirmeyi](get-started-power-bi-professional.md)deneyin.
Azure Active Directory kimlik doğrulaması için ortamınızı yapılandırmak [için, Kimlik Doğrulaması'nın SQL havuzuna](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)olduğunu görme.'ye bakın.
 