---
title: sqlcmd ile bağlan
description: Synapse SQL havuzuna bağlanmak ve sorgulamak için sqlcmd command-line yardımcı programını kullanın.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: cfb1b7558f8d7675009a0ebc729cc1a560f03d12
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633371"
---
# <a name="connect-to-synapse-sql-pool-with-sqlcmd"></a>Sqlcmd ile Synapse SQL havuzuna bağlanın
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Makine Öğrenimi](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

SQL havuzuna bağlanmak ve sorgulamak için [sqlcmd][sqlcmd] komut satırı yardımcı programını kullanın.  

## <a name="1-connect"></a>1. Bağlan
[sqlcmd][sqlcmd] ile başlamak için komut istemini açın ve SQL pool veritabanınızın bağlantı dizesini takip eden **sqlcmd** girin. Bağlantı dizesi için aşağıdaki parametreler gereklidir:

* **Server (-S):**`<`Sunucu Adı`>`.database.windows.net biçiminde belirtilmiş sunucu
* **Database (-d):** Veritabanı adı.
* **Teklif Edilen Tanımlayıcıları Etkinleştir (-I):** Sql havuz örneğine bağlanmak için teklif edilen tanımlayıcıların etkinleştirilmesi gerekir.

SQL Server Kimlik Doğrulamasını kullanmak için kullanıcı adı/parola parametrelerini eklemeniz gerekir:

* **User (-U):**`<`Kullanıcı`>` biçimindeki sunucu kullanıcısı
* **Password (-P):** Kullanıcıyla ilişkili parola.

Örneğin, bağlantı dizeniz aşağıdaki gibi görünebilir:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Azure Active Directory Tümleşik kimlik doğrulamasını kullanmak için Azure Active Directory parametrelerini eklemeniz gerekir:

* **Azure Active Directory Kimlik Doğrulaması (-G):** Kimlik doğrulaması için Azure Active Directory kullanın

Örneğin, bağlantı dizeniz aşağıdaki gibi görünebilir:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Active Directory kullanarak kimlik doğrulaması yapmak için [Azure Active Directory Kimlik Doğrulamasını etkinleştirmeniz](sql-data-warehouse-authentication.md) gerekir.
> 
> 

## <a name="2-query"></a>2. Sorgula
Bağlantının ardından desteklenen herhangi bir Transact-SQL deyimini örnekte yayımlayabilirsiniz.  Bu örnekte sorgular etkileşimli modda gönderilir.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Bu sonraki örnekler -Q seçeneği kullanarak veya SQL’i sqlcmd öğesine ekleyerek sorgularınızı toplu iş modunda nasıl çalıştırabileceğinizi gösterir.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Sonraki adımlar
sqlcmd'de bulunan seçenekler hakkında daha fazla bilgi için [sqlcmd dokümantasyona](https://msdn.microsoft.com/library/ms162773.aspx) bakın.
