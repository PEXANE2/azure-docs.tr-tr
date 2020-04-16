---
title: "Azure Data Studio (önizleme): Synapse SQL'e bağlanma ve sorgula"
description: Azure Synapse Analytics'te Synapse SQL'e bağlanmak ve bunları sorgulamak için Azure Data Studio'u (önizleme) kullanın.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: edf5a6a6a0f17c21abb818a0d41d0d0b1c39949c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423806"
---
# <a name="connect-to-synapse-sql-with-azure-data-studio-preview"></a>Azure Data Studio ile Synapse SQL'e bağlanın (önizleme)

> [!div class="op_single_selector"]
>
> * [Azure Data Studio](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [Sqlcmd](get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)

Azure Synapse Analytics'te Synapse SQL'e bağlanmak ve bunları sorgulamak için [Azure Data Studio'u (önizleme)](/sql/azure-data-studio/download-azure-data-studio?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanabilirsiniz. 

## <a name="connect"></a>Bağlan

Synapse SQL'e bağlanmak için Azure Veri Stüdyosu'nun açılışına ve **Yeni Bağlantı'yı**seçin.

![Azure Veri Stüdyosu'ni Aç](./media/get-started-azure-data-studio/1-start.png)

Bağlantı türü olarak Microsoft SQL **Server'ı**seçin. **Microsoft SQL Server**

Bağlantı aşağıdaki parametreleri gerektirir:

* **Sunucu:** Sunucu şeklinde `<Azure Synapse workspace name>`-ondemand.sql.azuresynapse.net
* **Veritabanı:** Veritabanı adı

> [!NOTE]
> İsteğe bağlı SQL kullanmak istiyorsanız **(önizleme)** URL aşağıdaki gibi görünmelidir:
>
> - `<Azure Synapse workspace name>`-ondemand.sql.azuresynapse.net.
>
> **SQL havuzunu** kullanmak istiyorsanız URL aşağıdaki gibi görünmelidir:
>
> - `<Azure Synapse workspace name>`.sql.azuresynapse.net

Kimlik Doğrulama **Türü**olarak **Windows Kimlik Doğrulamasını,** Azure Etkin **Dizini'ni**veya **SQL Giriş'i** seçin.

Kimlik doğrulama türü olarak **SQL Giriş'i** kullanmak için kullanıcı adı/parola parametrelerini ekleyin:

* **Kullanıcı:** Formda sunucu kullanıcı`<User>`
* **Şifre:** Kullanıcıyla ilişkili parola

Azure Etkin Dizini'ni kullanmak için gerekli kimlik doğrulama türünü seçmeniz gerekir.

![AAD Kimlik Doğrulaması](./media/get-started-azure-data-studio/3-aad-auth.png)

Bu ekran **görüntüsü, Windows Kimlik Doğrulaması**için **Bağlantı Ayrıntılarını** gösterir:

![Windows Kimlik Doğrulaması](./media/get-started-azure-data-studio/3-windows-auth.png)

Bu ekran **görüntüsü, SQL Giriş**kullanarak Bağlantı **Ayrıntılarını** gösterir:

![SQL Oturum Açma](./media/get-started-azure-data-studio/2-database-details.png)

Başarılı bir girişten sonra, şu gibi ![bir pano görmeniz gerekir: Pano](./media/get-started-azure-data-studio/4-dashboard.png)

## <a name="query"></a>Sorgu

Bağlandıktan sonra, synapse SQL'i desteklenen [Transact-SQL (T-SQL)](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ifadelerini kullanarak örne karşı sorgulayabilirsiniz. Başlamak için pano görünümünden **Yeni sorgu'yu** seçin.

![Yeni Sorgu](./media/get-started-azure-data-studio/5-new-query.png)

Örneğin, isteğe bağlı SQL kullanarak [Parke dosyalarını sorgulamak](query-parquet-files.md) için aşağıdaki Transact-SQL deyimini kullanabilirsiniz:

```sql
SELECT COUNT(*)
FROM  
OPENROWSET(
    BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet',
    FORMAT='PARQUET'
)
```
## <a name="next-steps"></a>Sonraki adımlar 
Synapse SQL'e bağlanmanın diğer yollarını keşfedin: 

- [SSMS](get-started-ssms.md)
- [Power BI](get-started-power-bi-professional.md)
- [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Sqlcmd](get-started-connect-sqlcmd.md)
 
