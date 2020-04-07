---
title: Sorun giderme bağlantısı
description: Synapse SQL havuzunda sorun giderme bağlantısı.
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
ms.openlocfilehash: 8aaca40961b1294336b236305ecdffcc92c5a4d9
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80742582"
---
# <a name="troubleshooting-connectivity-issues"></a>Bağlantı sorunlarını giderme

Bu makalede, SQL Analytics veritabanınıza bağlanma konusunda sık karşılaşılan sorun giderme teknikleri listelanmaktadır.

- [Hizmet kullanılabilirliğini denetleme](sql-data-warehouse-troubleshoot-connectivity.md#check-service-availability)
- [Duraklatılmış veya ölçeklendirilen işlemleri denetleme](sql-data-warehouse-troubleshoot-connectivity.md#check-for-paused-or-scaling-operation)
- [Güvenlik duvarı ayarlarını denetleme](sql-data-warehouse-troubleshoot-connectivity.md#check-your-firewall-settings)
- [Sanal Ağ/Hizmet Uç Noktası ayarlarınızı denetleme](sql-data-warehouse-troubleshoot-connectivity.md#check-your-vnetservice-endpoint-settings)
- [En son sürücüleri denetleme](sql-data-warehouse-troubleshoot-connectivity.md#check-for-the-latest-drivers)
- [Bağlantı dizenizi denetleme](sql-data-warehouse-troubleshoot-connectivity.md#check-your-connection-string)
- [Aralıklı bağlantı sorunları](sql-data-warehouse-troubleshoot-connectivity.md#intermittent-connection-issues)
- [Genel hata iletileri](sql-data-warehouse-troubleshoot-connectivity.md#common-error-messages)

## <a name="check-service-availability"></a>Hizmet kullanılabilirliğini denetleme

Hizmetin kullanılabilir olup olmadığını kontrol edin. Azure portalında, bağlanmaya çalıştığınız Synapse SQL havuzuna gidin. Sol DAKI TOC panelinde **Tanıla'ya**tıklayın ve sorunları çözün.

![Kaynak sistem durumunu seçin](./media/sql-data-warehouse-troubleshoot-connectivity/diagnostics-link.png)

Synapse SQL havuzunuzun durumu burada gösterilir. Hizmet **Kullanılabilir**olarak görünmüyorsa, diğer adımları denetleyin.

![Hizmet Mevcut](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health.png)

Kaynak sistem durumu, Synapse SQL havuz örneğinizin duraklatılmış veya ölçeklenirolduğunu gösteriyorsa, örneğinizi devam ettirmek için kılavuzu izleyin.

![Hizmet Duraklatıldı](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health-pausing.png) Kaynak Durumu ile ilgili ek bilgilere buradan ulabilirsiniz.

## <a name="check-for-paused-or-scaling-operation"></a>Duraklatılmış veya ölçeklendirilen işlemleri denetleme

Synapse SQL havuz örneğinizin duraklatılmış mı yoksa ölçeklenebilir mi diye portalı denetleyin.

![Servis Duraklatıldı](./media/sql-data-warehouse-troubleshoot-connectivity/overview-paused.png)

Hizmetinizin duraklatAndığını veya ölçeklemediğini görürseniz, hizmetin bakım zamanlamanız sırasında olmadığını kontrol edin. Synapse SQL havuzugenel *bakış*için portalda, seçilen bakım zamanlamasını görürsünüz.

![Genel Bakım Çizelgesi](./media/sql-data-warehouse-troubleshoot-connectivity/overview-maintance-schedule.png)

Aksi takdirde, bu bakımın zamanlanmış bir olay olmadığını doğrulamak için BT yöneticinize danışın. SQL Analytics örneğini devam ettirmek için [aşağıdaki adımları](pause-and-resume-compute-portal.md)izleyin.

## <a name="check-your-firewall-settings"></a>Güvenlik duvarı ayarlarını denetleme

SQL Analytics veritabanı port 1433 üzerinden iletişim kurar.Bir şirket ağından bağlanmaya çalışıyorsanız ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız. Güvenlik duvarı yapılandırmaları hakkında ek bilgi [burada](../../sql-database/sql-database-firewall-configure.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#create-and-manage-ip-firewall-rules)bulabilirsiniz.

## <a name="check-your-vnetservice-endpoint-settings"></a>Sanal Ağ/Hizmet Uç Noktası ayarlarınızı denetleme

Hatalar 40914 ve 40615 alıyorsanız, [burada hata açıklaması ve çözümbakın.](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#errors-40914-and-40615)

## <a name="check-for-the-latest-drivers"></a>En son sürücüleri denetleme

### <a name="software"></a>Yazılım

Synapse SQL havuzunuza bağlanmak için en son araçları kullandığınızdan emin olun:

- SSMS
- Azure Data Studio
- SQL Server Veri Araçları (Visual Studio)

### <a name="drivers"></a>Sürücüler

En son sürücü sürümlerini kullandığınızdan emin olun.Eski sürücüler yeni özellikleri desteklemeyebilir gibi sürücülerin eski bir sürümünü kullanarak beklenmeyen davranışlara neden olabilir.

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

ODBC Bağlantı dizesi

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

PHP Bağlantı dizesi

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

JDBC bağlantı dizesi

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="intermittent-connection-issues"></a>Aralıklı bağlantı sorunları

Sunucu üzerinde çok sayıda sıraya alınmış isteğin olduğu ağır yüklenme sorunu yaşayıp yaşamadığınızı denetleyin. Ek kaynaklar için Synapse SQL havuzunuzu ölçeklendirmeniz gerekebilir.

## <a name="common-error-messages"></a>Genel hata iletileri

Hatalar 40914 ve 40615, [hata açıklaması ve çözüm burada](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#errors-40914-and-40615)bakın.

## <a name="still-having-connectivity-issues"></a>Hala bağlantı sorunları yaşıyor musunuz?

Mühendislik ekibinin sizi destekleyebilmeleri için bir [destek bileti](sql-data-warehouse-get-started-create-support-ticket.md) oluşturun.
