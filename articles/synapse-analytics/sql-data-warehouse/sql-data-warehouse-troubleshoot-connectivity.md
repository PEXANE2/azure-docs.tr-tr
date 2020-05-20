---
title: Bağlantı sorunlarını giderme
description: SYNAPSE SQL havuzunda bağlantı sorunlarını giderme.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/27/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: b4fbfb65a609742105056fa7fb849f84579245cb
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650492"
---
# <a name="troubleshooting-connectivity-issues-in-synapse-sql-pool"></a>SYNAPSE SQL havuzunda bağlantı sorunlarını giderme

Bu makalede, SQL havuzu veritabanınıza bağlanılmasıyla ilgili yaygın sorun giderme teknikleri listelenmektedir.

## <a name="check-service-availability"></a>Hizmet kullanılabilirliğini denetle

Hizmetin kullanılabilir olup olmadığını denetleyin. Azure portal, bağlanmaya çalıştığınız SQL havuzuna gidin. Sol IÇINDEKILER panelinde, **sorunları Tanıla ve çöz**' e tıklayın.

![Kaynak durumunu seçin](./media/sql-data-warehouse-troubleshoot-connectivity/diagnostics-link.png)

SQL havuzunuzun durumu burada gösterilir. Hizmet **kullanılabilir**olarak görüntülenmiyorsa, daha fazla adım denetleyin.

![Hizmet kullanılabilir](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health.png)

Kaynak sistem durumunuzun SQL havuzu örneğinizin duraklatıldığını veya ölçeklendirilmesini gösteriyorsa, örneğinizi sürdürmeye yönelik yönergeleri izleyin.

![Hizmet duraklatılmış ](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health-pausing.png) kaynak durumu hakkındaki ek bilgileri burada bulabilirsiniz.

## <a name="check-for-paused-or-scaling-operation"></a>Duraklatılmış veya ölçeklendirilen işlemleri denetleme

SQL havuzu örneğinizin duraklatıldığını veya ölçeklendirilmesini görmek için portalı denetleyin.

![Hizmet duraklatıldı](./media/sql-data-warehouse-troubleshoot-connectivity/overview-paused.png)

Hizmetinizin duraklatıldığını veya ölçeklendirilmesini görürseniz, bakım zamanlamanız sırasında olup olmadığını kontrol edin. SQL havuzuna *genel bakış*için portalda, seçili bakım zamanlamasını görürsünüz.

![Genel Bakış bakım zamanlaması](./media/sql-data-warehouse-troubleshoot-connectivity/overview-maintance-schedule.png)

Aksi takdirde, bu bakımın zamanlanmış bir olay olmadığını doğrulamak için BT yöneticinize başvurun. SQL havuzu örneğini yeniden başlatmak için [aşağıdaki adımları](pause-and-resume-compute-portal.md)izleyin.

## <a name="check-your-firewall-settings"></a>Güvenlik duvarı ayarlarını denetleme

SQL havuzu veritabanı 1433 bağlantı noktası üzerinden iletişim kurar.Bir şirket ağından bağlanmaya çalışıyorsanız ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız. Güvenlik Duvarı yapılandırmalarına ilişkin ek bilgilere [buradan](../../sql-database/sql-database-firewall-configure.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#create-and-manage-ip-firewall-rules)ulaşabilirsiniz.

## <a name="check-your-vnetservice-endpoint-settings"></a>Sanal Ağ/Hizmet Uç Noktası ayarlarınızı denetleme

40914 ve 40615 hatalarını alıyorsanız, [buraya hata açıklaması ve çözüm](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#errors-40914-and-40615)bölümüne bakın.

## <a name="check-for-the-latest-drivers"></a>En son sürücüleri denetleme

### <a name="software"></a>Yazılım

SQL havuzunuza bağlanmak için en son araçları kullandığınızdan emin olun:

- SSMS
- Azure Data Studio
- SQL Server Veri Araçları (Visual Studio)

### <a name="drivers"></a>Sürücüler

En son sürücü sürümlerini kullandığınızdan emin olun.Eski sürücüler yeni özellikleri desteklemedikleri için sürücülerin eski bir sürümünü kullanmak beklenmeyen davranışlara neden olabilir.

- [ODBC](/sql/connect/odbc/download-odbc-driver-for-sql-server?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [JDBC](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [OLE DB](/sql/connect/oledb/download-oledb-driver-for-sql-server?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [PHP](/sql/connect/php/download-drivers-php-sql-server?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="check-your-connection-string"></a>Bağlantı dizenizi denetleme

Bağlantı dizelerinizin düzgün ayarlandığından emin olun.  Aşağıda bazı örnekler verilmiştir.  Bağlantı dizeleriyle ilgili ek bilgileri [burada](sql-data-warehouse-connection-strings.md) bulabilirsiniz.

ADO.NET bağlantı dizesi

```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

ODBC bağlantı dizesi

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

PHP bağlantı dizesi

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

JDBC bağlantı dizesi

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="intermittent-connection-issues"></a>Aralıklı bağlantı sorunları

Sunucu üzerinde çok sayıda sıraya alınmış isteğin olduğu ağır yüklenme sorunu yaşayıp yaşamadığınızı denetleyin. Ek kaynaklar için SQL havuzunuzu ölçeklendirmeniz gerekebilir.

## <a name="common-error-messages"></a>Genel hata iletileri

Hatalar 40914 ve 40615, [buradaki hata açıklamasına ve çözümüne](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#errors-40914-and-40615)bakın.

## <a name="still-having-connectivity-issues"></a>Hala bağlantı sorunları var mı?

Mühendislik ekibinin sizi destekleyebilmesi için bir [destek bileti](sql-data-warehouse-get-started-create-support-ticket.md) oluşturun.
