---
title: 'Azure Data Studio (Önizleme): Connect ve Query SYNAPSE SQL'
description: Azure SYNAPSE Analytics 'te SYNAPSE SQL 'e bağlanmak ve sorgulamak için Azure Data Studio (Önizleme) kullanın.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 561ea1a4f3577e037708c3b090188c59bd3c4aad
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82187554"
---
# <a name="connect-to-synapse-sql-with-azure-data-studio-preview"></a>Azure Data Studio ile SYNAPSE SQL 'e bağlanma (Önizleme)

> [!div class="op_single_selector"]
>
> * [Azure Data Studio](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [sqlcmd](get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)

Azure SYNAPSE Analytics 'te SYNAPSE SQL 'e bağlanmak ve sorgu eklemek için [Azure Data Studio (Önizleme)](/sql/azure-data-studio/download-azure-data-studio?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanabilirsiniz. 

## <a name="connect"></a>Bağlan

SYNAPSE SQL 'e bağlanmak için Azure Data Studio açın ve **Yeni bağlantı**' yı seçin.

![Azure Data Studio açın](./media/get-started-azure-data-studio/1-start.png)

**Bağlantı türü**olarak **Microsoft SQL Server** seçin.

Bağlantı aşağıdaki parametreleri gerektirir:

* **Sunucu:** Formdaki `<Azure Synapse workspace name>`sunucu-OnDemand.Sql.azuresynapse.net
* **Veritabanı:** Veritabanı adı

> [!NOTE]
> **İstek üzerine SQL (Önizleme)** kullanmak istiyorsanız URL şöyle görünmelidir:
>
> - `<Azure Synapse workspace name>`-ondemand.sql.azuresynapse.net.
>
> **SQL havuzu** kullanmak istiyorsanız URL şöyle görünmelidir:
>
> - `<Azure Synapse workspace name>`. sql.azuresynapse.net

**Kimlik doğrulama türü**olarak **Windows kimlik doğrulaması**, **Azure Active Directory**veya **SQL oturum açma** seçeneklerini belirleyin.

Kimlik doğrulama türü olarak **SQL oturum açma** 'yı kullanmak için, Kullanıcı adı/parola parametrelerini ekleyin:

* **Kullanıcı:** Formdaki sunucu kullanıcısı`<User>`
* **Parola:** Kullanıcıyla ilişkili parola

Azure Active Directory kullanmak için, gerekli kimlik doğrulama türünü seçmeniz gerekir.

![AAD Kimlik Doğrulaması](./media/get-started-azure-data-studio/3-aad-auth.png)

Aşağıdaki ekran görüntüsünde, **Windows kimlik doğrulaması**Için **Bağlantı ayrıntıları** gösterilmektedir:

![Windows Kimlik Doğrulaması](./media/get-started-azure-data-studio/3-windows-auth.png)

Aşağıdaki ekran görüntüsünde **SQL oturum açma**kullanılarak **Bağlantı ayrıntıları** gösterilmektedir:

![SQL Oturum Açma](./media/get-started-azure-data-studio/2-database-details.png)

Başarılı oturum açma işleminden sonra şu şekilde bir pano görmeniz gerekir: ![Pano](./media/get-started-azure-data-studio/4-dashboard.png)

## <a name="query"></a>Sorgu

Bağlandıktan sonra, örnek olarak desteklenen [Transact-SQL (T-SQL)](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) deyimlerini kullanarak SYNAPSE SQL 'i sorgulama yapabilirsiniz. Başlamak için Pano görünümünden **Yeni sorgu** ' yı seçin.

![Yeni Sorgu](./media/get-started-azure-data-studio/5-new-query.png)

Örneğin, isteğe bağlı SQL kullanarak [Parquet dosyalarını sorgulamak](query-parquet-files.md) Için aşağıdaki Transact-SQL ifadesini kullanabilirsiniz:

```sql
SELECT COUNT(*)
FROM  
OPENROWSET(
    BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet',
    FORMAT='PARQUET'
)
```
## <a name="next-steps"></a>Sonraki adımlar 
SYNAPSE SQL 'e bağlanmanın diğer yollarını keşfet: 

- [SSMS](get-started-ssms.md)
- [Power BI](get-started-power-bi-professional.md)
- [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [sqlcmd](get-started-connect-sqlcmd.md)
 
