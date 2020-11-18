---
title: Sqlcmd ile SYNAPSE SQL 'e bağlanma
description: Sunucu sunucusuz SQL havuzuna (Önizleme) ve adanmış SQL havuzuna bağlanmak ve sorgulamak için sqlcmd komut satırı yardımcı programını kullanın.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: 43fae026f91e4430fbce0d01141a86a913db089d
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94686149"
---
# <a name="connect-to-synapse-sql-with-sqlcmd"></a>Sqlcmd ile SYNAPSE SQL 'e bağlanma

> [!div class="op_single_selector"]
> * [Azure Data Studio (Önizleme)](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [sqlcmd](../sql/get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)

SYNAPSE SQL içinde sunucusuz SQL havuzuna (Önizleme) ve adanmış SQL havuzuna bağlanmak ve sorgulamak için [sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) komut satırı yardımcı programını kullanabilirsiniz.  

## <a name="1-connect"></a>1. Bağlan
[Sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanmaya başlamak için, komut istemi ' ni açın ve **sqlcmd** yazıp SYNAPSE SQL veritabanınızın bağlantı dizesini girin. Bağlantı dizesi için aşağıdaki parametreler gereklidir:

* **Server (-S):**`<`Sunucu Adı`>`.database.windows.net biçiminde belirtilmiş sunucu
* **Veritabanı (-d):** Veritabanı adı
* **Alıntılanmış tanımlayıcıları etkinleştir (-ı):** Bir Synapse SQL örneğine bağlanmak için tırnak işareti tanımlayıcılarının etkinleştirilmesi gerekir

SQL Server kimlik doğrulaması kullanmak için, Kullanıcı adı ve parola parametrelerini eklemeniz gerekir:

* **User (-U):**`<`Kullanıcı`>` biçimindeki sunucu kullanıcısı
* **Parola (-P):** Kullanıcıyla ilişkili parola

Bağlantı dizeniz aşağıdaki örnekteki gibi görünebilir:

**Sunucusuz SQL havuzu**

```sql
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P Enter_Your_Password_Here -I
```

**Ayrılmış SQL havuzu**

```
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Azure Active Directory Tümleşik kimlik doğrulamasını kullanmak için Azure Active Directory parametrelerini eklemeniz gerekir:

* **Azure Active Directory Kimlik Doğrulaması (-G):** Kimlik doğrulaması için Azure Active Directory kullanın

Bağlantı dizeniz aşağıdaki örneklerde gösterildiği gibi görünebilir:

**Sunucusuz SQL havuzu**

```
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -G -I
```

**Ayrılmış SQL havuzu**

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Active Directory kullanarak kimlik doğrulaması yapmak için [Azure Active Directory Kimlik Doğrulamasını etkinleştirmeniz](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) gerekir.

## <a name="2-query"></a>2. sorgu

### <a name="use-dedicated-sql-pool"></a>Adanmış SQL havuzu kullan

Bağlantıdan sonra, örnek için desteklenen [Transact-SQL](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) (T-SQL) deyimlerini gönderebilirsiniz. Bu örnekte sorgular etkileşimli modda gönderilir:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Adanmış SQL havuzu için aşağıdaki örnekler,-Q seçeneğini kullanarak sorguları Batch modunda nasıl çalıştıracağınızı veya SQL ' i sqlcmd ' ye yönelterek gösterir:

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

### <a name="use-serverless-sql-pool"></a>Sunucusuz SQL havuzu kullanma

Bağlandıktan sonra, örnek için desteklenen [Transact-SQL](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) (T-SQL) deyimlerini gönderebilirsiniz.  Aşağıdaki örnekte sorgular etkileşimli modda gönderilir:

```sql
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P Enter_Your_Password_Here -I
1> SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')
2> GO
3> QUIT
```

Sunucusuz SQL havuzu için aşağıdaki örneklerde,-Q seçeneğini kullanarak sorguların Batch modunda nasıl çalıştırılacağı veya SQL to sqlcmd ile nasıl çalıştırılacağı gösterilmektedir:

```sql
sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P 'Enter_Your_Password_Here' -I -Q "SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')"
```

```sql
"SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')" | sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P 'Enter_Your_Password_Here' -I > ./tables.out
```

## <a name="next-steps"></a>Sonraki adımlar

Sqlcmd seçenekleri hakkında daha fazla bilgi için bkz. [sqlcmd belgeleri](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).
