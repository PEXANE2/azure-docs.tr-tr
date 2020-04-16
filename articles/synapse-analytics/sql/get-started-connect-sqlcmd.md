---
title: Sqlcmd kullanarak Synapse SQL'e bağlanın
description: SQL on-demand (önizleme) ve SQL havuzuna bağlanmak ve sorgulamak için sqlcmd komut satırı yardımcı programını kullanın.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 8ff9034e6c31c8d95e862570e3962990dfec8442
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423757"
---
# <a name="connect-to-synapse-sql-with-sqlcmd"></a>Sqlcmd ile Synapse SQL'e bağlanın

> [!div class="op_single_selector"]
>
> * [Azure Veri Stüdyosu (önizleme)](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [Sqlcmd](../sql/get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)

Synapse SQL'deki SQL on-demand (önizleme) ve SQL havuzuna bağlanmak ve sorgulamak için [sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) komut satırı yardımcı programını kullanabilirsiniz.  

## <a name="1-connect"></a>1. Bağlan
[Sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ile başlamak için komut istemini açın ve synapse SQL veritabanınızın bağlantı dizesini takip eden **sqlcmd** girin. Bağlantı dizesi için aşağıdaki parametreler gereklidir:

* **Server (-S):**`<`Sunucu Adı`>`.database.windows.net biçiminde belirtilmiş sunucu
* **Veritabanı (-d):** Veritabanı adı
* **Teklif Edilen Tanımlayıcıları Etkinleştir (-I):** Synapse SQL örneğine bağlanmak için teklif edilen tanımlayıcılar etkinleştirilmelidir

SQL Server Kimlik Doğrulaması'nı kullanmak için kullanıcı adı ve parola parametrelerini eklemeniz gerekir:

* **User (-U):**`<`Kullanıcı`>` biçimindeki sunucu kullanıcısı
* **Şifre (-P):** Kullanıcıyla ilişkili parola

Bağlantı dizeniz aşağıdaki örnek gibi görünebilir:

**İsteğe bağlı SQL**

```sql
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P Enter_Your_Password_Here -I
```

**SQL havuzu**

```
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Azure Active Directory Tümleşik kimlik doğrulamasını kullanmak için Azure Active Directory parametrelerini eklemeniz gerekir:

* **Azure Active Directory Kimlik Doğrulaması (-G):** Kimlik doğrulaması için Azure Active Directory kullanın

Bağlantı dizeniz aşağıdaki örneklerde aşağıdaki gibi görünebilir:

**İsteğe bağlı SQL**

```
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -G -I
```

**SQL havuzu**

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Active Directory kullanarak kimlik doğrulaması yapmak için [Azure Active Directory Kimlik Doğrulamasını etkinleştirmeniz](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) gerekir.

## <a name="2-query"></a>2. Sorgula

### <a name="use-sql-pool"></a>SQL havuzu kullanma

Bağlantının ardından desteklenen herhangi bir Transact-SQL deyimini örnekte yayımlayabilirsiniz.  Bu örnekte, sorgular etkileşimli modda gönderilir:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

SQL havuzu için aşağıdaki örnekler, -Q seçeneğini kullanarak toplu modda sorguları nasıl çalıştırabileceğinizi veya SQL'inizi sqlcmd'ye nasıl çalıştırabileceğinizi gösterir:

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

### <a name="use-sql-on-demand"></a>İsteğe bağlı SQL kullanma

Bağlandıktan sonra, desteklenen tüm [Transact-SQL](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) (T-SQL) deyimlerini örneğin karşısına koyabilirsiniz.  Aşağıdaki örnekte, sorgular etkileşimli modda gönderilir:

```sql
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P Enter_Your_Password_Here -I
1> SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')
2> GO
3> QUIT
```

İsteğe bağlı SQL için, takip eden örnekler -Q seçeneğini kullanarak toplu modda sorguları nasıl çalıştırabileceğinizi veya SQL'inizi sqlcmd'ye nasıl çalıştırabileceğinizi gösterir:

```sql
sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P 'Enter_Your_Password_Here' -I -Q "SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')"
```

```sql
"SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')" | sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P 'Enter_Your_Password_Here' -I > ./tables.out
```

## <a name="next-steps"></a>Sonraki adımlar

sqlcmd seçenekleri hakkında daha fazla bilgi için [sqlcmd dokümantasyona](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)bakın.
