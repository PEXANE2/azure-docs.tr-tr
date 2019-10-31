---
title: include dosyası
description: include dosyası
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.author: estfan
ms.custom: include file
ms.date: 05/15/2018
ms.openlocfilehash: d60d7727e0674298fa6da7e7330221318da23efd
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73161628"
---
* Azure SQL veritabanı kullanıyorsanız [Azure SQL veritabanı 'Na bağlanma](#connect-azure-sql-db)bölümündeki adımları izleyin.

* SQL Server kullanıyorsanız, [SQL Server Connect](#connect-sql-server)' in altındaki adımları izleyin.

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database"></a>Azure SQL veritabanı 'na bağlanma

SQL tetikleyicisi veya eylemi sizden bağlantı bilgilerini isterse, her iki tetikleyici ve eylem için çalışan aşağıdaki adımları izleyin.

1. **Bağlantı adı**için bağlantınız için bir ad oluşturun.

1. **SQL Server adı**altında Azure SQL Server ' ı seçin. **SQL veritabanı ad** listesi göründüğünde, veritabanınızı seçin. Azure SQL sunucunuz için Kullanıcı adı ve parola belirtin.

   Bu bilgileri, SQL veritabanı özelliklerindeki veya bağlantı dizinizdeki Azure portal da bulabilirsiniz:

   * `User ID=<your-user-name>`
   * `Password=<your-password>`

   ![Azure SQL veritabanı ile bağlantı oluştur](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)

1. İşiniz bittiğinde **Oluştur**’u seçin.

1. Bağlantınızı oluşturduktan sonra, [SQL tetikleyicisi ekleme](#add-sql-trigger) veya [bir SQL eylemi ekleme](#add-sql-action)ile devam edin.

<a name="connect-sql-server"></a>

### <a name="connect-to-sql-server"></a>SQL Server'a bağlanma

SQL tetikleyicisi veya eylemi sizden bağlantı bilgilerini isterse, her iki tetikleyici ve eylem için çalışan aşağıdaki adımları izleyin. Bununla birlikte, başlamadan önce şirket [içi veri ağ geçidinizi](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection)oluşturduğunuzdan emin olun. Aksi takdirde, bağlantınızı oluştururken ağ geçidiniz ağ geçitleri listesinde görünmez.

1. **Bağlantı adı**için bağlantınız için bir ad oluşturun.

1. Tetikleyici veya eylemde, SQL Server seçeneklerinin görünmesi için şirket **içi veri ağ geçidi üzerinden Bağlan** ' ı seçin.

1. **SQL Server adı** ve **SQL veritabanı adı**için, SQL sunucunuzun adresini ve veritabanınızın adını belirtin. Kullanıcı **adı** ve **parola**için, sunucunuzun Kullanıcı adını ve parolasını belirtin.

   Bu bilgileri, bağlantı dizeniz içinde de bulabilirsiniz:

   * `Server=<your-server-address>`
   * `Database=<your-database-name>`
   * `User ID=<your-user-name>`
   * `Password=<your-password>`

   ![SQL Server bağlantı oluştur](./media/connectors-create-api-sqlazure/sql-server-create-connection.png)

1. SQL sunucunuz Windows veya temel kimlik doğrulaması kullanıyorsa, **kimlik doğrulama türünü**seçin.

1. **Ağ geçitleri**altında, önceden oluşturduğunuz şirket içi veri ağ geçidiniz Ile ilişkili Azure aboneliğini seçin ve şirket içi veri ağ geçidinizin adını seçin.

   Ağ geçidiniz listede görünmüyorsa, [ağ geçidinizin](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection)doğru şekilde ayarlandığından emin olun.

   ![SQL Server bağlantı oluşturma tamamlandı](./media/connectors-create-api-sqlazure/sql-server-create-connection-complete.png)

1. İşiniz bittiğinde **Oluştur**’u seçin.

1. Bağlantınızı oluşturduktan sonra, [SQL tetikleyicisi ekleme](#add-sql-trigger) veya [SQL eylemi ekleme](#add-sql-action)ile devam edin.
