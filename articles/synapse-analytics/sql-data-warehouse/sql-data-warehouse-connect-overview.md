---
title: Synapse SQL havuzuna bağlanın
description: SQL havuzuna bağlanın.
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
ms.openlocfilehash: e5c015751e8b0aeed7bd84086cc4f65c234fdb41
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745321"
---
# <a name="connect-to-synapse-sql-pool"></a>Synapse SQL havuzuna bağlanın

SQL havuzuna bağlanın.

## <a name="find-your-server-name"></a>Sunucu adınızı bulma

Aşağıdaki örnekteki sunucu adı sqlpoolservername.database.windows.net. Tam sunucu adını bulmak için:

1. [Azure portalına](https://portal.azure.com)gidin.
2. Azure **Synapse Analytics'e**tıklayın.
3. Bağlanmak istediğiniz SQL havuzuna tıklayın.
4. Tam sunucu adını bulun.

   ![Tam sunucu adı](media/sql-data-warehouse-connect-overview/server-connect.PNG)

## <a name="supported-drivers-and-connection-strings"></a>Desteklenen sürücüler ve bağlantı dizeleri

SQL havuzu [ADO.NET,](/dotnet/framework/data/adonet?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) [ODBC,](/sql/connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) [PHP](/sql/connect/php/overview-of-the-php-sql-driver?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)ve [JDBC'yi](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)destekler. En son sürümü ve belgeleri bulmak için, önceki sürücülerden birini tıklatın.

Azure portalından kullandığınız sürücü için bağlantı dizesini otomatik olarak oluşturmak için, önceki örnekteki **veritabanı bağlantı dizelerini göster'i** tıklatın. Aşağıda ayrıca her sürücü için bir bağlantı dizesinin nasıl göründüğü ile ilgili bazı örnekler verilmiştir.

> [!NOTE]
> Bağlantınızın kısa süreli kesintiler sırasında devam etmesi için bağlantı zaman aşımını 300 saniyeye ayarlayın.

### <a name="adonet-connection-string-example"></a>ADO.NET bağlantı dizesi örneği

```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>ODBC bağlantı dizesi örneği

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>PHP bağlantı dizesi örneği

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>JDBC bağlantı dizesi örneği

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Bağlantı ayarları

SQL havuzu, bağlantı ve nesne oluşturma sırasında bazı ayarları standartlaştırır. Bu ayarlar geçersiz kılınamaz ve şunları içerir:

| Veritabanı Ayarı | Değer |
|:--- |:--- |
| [ANSI_NULLS](/sql/t-sql/statements/set-ansi-nulls-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |AÇIK |
| [QUOTED_IDENTIFIERS](/sql/t-sql/statements/set-quoted-identifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |AÇIK |
| [DATEFORMAT](/sql/t-sql/statements/set-dateformat-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |mdy |
| [DATEFIRST](/sql/t-sql/statements/set-datefirst-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |7 |

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio ile bağlantı kurmak ve sorgulamak için bkz. [Visual Studio ile Sorgulama](sql-data-warehouse-query-visual-studio.md). Kimlik doğrulama seçenekleri hakkında daha fazla bilgi edinmek için Azure [Synapse Analytics'e kimlik doğrulama](sql-data-warehouse-authentication.md)bölümüne bakın.
