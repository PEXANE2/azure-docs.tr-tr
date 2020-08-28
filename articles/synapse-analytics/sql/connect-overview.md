---
title: SYNAPSE SQL 'e bağlanma
description: SYNAPSE SQL 'e bağlanın.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.custom: devx-track-csharp
ms.openlocfilehash: a9a9b8b9ed3e65ae9b8500017b838dc320ecbaac
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89005029"
---
# <a name="connect-to-synapse-sql"></a>SYNAPSE SQL 'e bağlanma
Azure SYNAPSE Analytics 'te SYNAPSE SQL özelliğine bağlanın.

## <a name="supported-tools-for-sql-on-demand-preview"></a>İsteğe bağlı SQL için desteklenen araçlar (Önizleme)

[Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) , 1.18.0 sürümünden başlayarak tam olarak desteklenmektedir. SSMS, sürüm 18,5 ' den başlayarak kısmen desteklenir, bunu yalnızca bağlanmak ve sorgulamak için kullanabilirsiniz.

> [!NOTE]
> AAD oturumunun sorgu yürütme sırasında 1 saatten uzun süre açık bir bağlantısı varsa, AAD 'yi kullanan herhangi bir sorgu başarısız olur. Bu, AAD ile etkileşime geçen AAD geçişli geçiş ve deyimlerini kullanarak depolamayı sorgulamayı içerir (dış sağlayıcı oluşturma gibi). Bu, SSMS ve REKLAMLARı 'nda sorgu Düzenleyicisi gibi bağlantıları açık tutan tüm araçları etkiler. SYNAPSE Studio gibi bir sorgu yürütmek için yeni bağlantılar açan araçlar etkilenmez.

> Bu sorunu gidermek için SSMS 'yi yeniden başlatabilir veya reklamlar ' a bağlanıp bağlantıyı kesebilirsiniz. 

## <a name="find-your-server-name"></a>Sunucu adınızı bulma

Aşağıdaki örnekteki SQL havuzu için sunucu adı: showdemoweu.sql.azuresynapse.net.
Aşağıdaki örnekteki SQL için isteğe bağlı sunucu adı: showdemoweu-ondemand.sql.azuresynapse.net.

Tam sunucu adını bulmak için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. **SYNAPSE çalışma alanları**' na tıklayın.
3. Bağlanmak istediğiniz çalışma alanına tıklayın.
4. Genel Bakış ' a gidin.
5. Tam sunucu adını bulun.

## <a name="sql-pool"></a>**SQL havuzu**

![Tam sunucu adı](./media/connect-overview/server-connect-example.png)

## <a name="sql-on-demand"></a>**İsteğe bağlı SQL**

![Tam sunucu adı isteğe bağlı SQL](./media/connect-overview/server-connect-example-sqlod.png)

## <a name="supported-drivers-and-connection-strings"></a>Desteklenen sürücüler ve bağlantı dizeleri
SYNAPSE SQL [ADO.net](https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx), [ODBC](https://msdn.microsoft.com/library/jj730314.aspx), [php](https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396)ve [JDBC](https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx)destekler. En son sürümü ve belgeleri bulmak için, önceki sürücülerden birine tıklayın. Azure portal kullandığınız sürücü için bağlantı dizesini otomatik olarak oluşturmak için, önceki örnekteki **veritabanı bağlantı dizelerini göster** ' e tıklayın. Aşağıda ayrıca her sürücü için bir bağlantı dizesinin nasıl göründüğü ile ilgili bazı örnekler verilmiştir.

> [!NOTE]
> Bağlantınızın kısa süreli kesintiler sırasında devam etmesi için bağlantı zaman aşımını 300 saniyeye ayarlayın.

### <a name="adonet-connection-string-example"></a>ADO.NET bağlantı dizesi örneği

```csharp
Server=tcp:{your_server}.sql.azuresynapse.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>ODBC bağlantı dizesi örneği

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.sql.azuresynapse.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>PHP bağlantı dizesi örneği

```PHP
Server: {your_server}.sql.azuresynapse.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.sql.azuresynapse.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.sql.azuresynapse.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>JDBC bağlantı dizesi örneği

```Java
jdbc:sqlserver://yourserver.sql.azuresynapse.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.sql.azuresynapse.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Bağlantı ayarları
SYNAPSE SQL, bağlantı ve nesne oluşturma sırasında bazı ayarları standartlaştırır. Bu ayarlar geçersiz kılınamaz ve şunları içerir:

| Veritabanı Ayarı | Değer |
|:--- |:--- |
| [ANSI_NULLS](/sql/t-sql/statements/set-ansi-nulls-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |AÇIK |
| [QUOTED_IDENTIFIERS](/sql/t-sql/statements/set-quoted-identifier-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |AÇIK |
| [DATEFORMAT](/sql/t-sql/statements/set-dateformat-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |mdy |
| [DATEFIRST](/sql/t-sql/statements/set-datefirst-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |7 |

## <a name="recommendations"></a>Öneriler

**SQL isteğe** bağlı sorguları yürütmek için önerilen Araçlar [Azure Data Studio](get-started-azure-data-studio.md) ve Azure SYNAPSE Studio.

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio ile bağlantı kurmak ve sorgulamak için bkz. [Visual Studio ile Sorgulama](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json). Kimlik doğrulama seçenekleri hakkında daha fazla bilgi için bkz. [SYNAPSE SQL kimlik doğrulaması](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).