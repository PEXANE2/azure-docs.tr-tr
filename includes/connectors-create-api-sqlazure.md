---
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.topic: include
ms.date: 11/08/2019
ms.openlocfilehash: ea0ae1b1527aa1f527c8ac8fbcd3b4e4f6b6fe2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789214"
---
* Azure SQL Veritabanı kullanıyorsanız, [Azure SQL Veritabanına Bağlan](#connect-azure-sql-db)altındaki adımları izleyin.

* SQL Server kullanıyorsanız, [SQL Server'a Bağlan](#connect-sql-server)altındaki adımları izleyin.

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database"></a>Azure SQL Veritabanına Bağlan

SQL tetikleyicisi veya eylemi bağlantı bilgileri için sizden istendiğinde, hem tetikleyiciler hem de eylemler için çalışan şu adımları izleyin.

1. **Bağlantı Adı**için bağlantınız için bir ad oluşturun.

1. **SQL Server Name**altında Azure SQL sunucunuzu seçin. SQL **Veritabanı Adı** listesi göründüğünde veritabanınızı seçin. Azure SQL sunucunuz için kullanıcı adı ve parola sağlayın.

   Bu bilgileri SQL veritabanı özellikleriniz altındaki Azure portalında veya bağlantı dizenizde de bulabilirsiniz:

   * `User ID=<your-user-name>`
   * `Password=<your-password>`

   ![Azure SQL Veritabanı'na bağlantı oluşturma](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)

1. İşiniz bittiğinde **Oluştur**’u seçin.

1. Bağlantınızı oluşturduktan [sonra, SQL tetikleyicisi ekle](#add-sql-trigger) veya [SQL eylemi ekle](#add-sql-action)ile devam edin.

<a name="connect-sql-server"></a>

### <a name="connect-to-sql-server"></a>SQL Server’a bağlanma

SQL tetikleyicisi veya eylemi bağlantı bilgileri için sizden istendiğinde, hem tetikleyiciler hem de eylemler için çalışan şu adımları izleyin. [Şirket içi veri ağ geçidini](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-install) yerel bir bilgisayara yüklemenizi ve Azure veri ağ geçidi kaynağını [oluşturmanızı](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection)gerektiren senaryolar için, önce bu gereksinimleri tamamladığınızdan emin olun. Aksi takdirde, bağlantınızı oluşturduğunuzda ağ geçidi kaynağınız ağ geçidi listesinde görünmez.

Ayrıca, bir [entegrasyon hizmeti ortamında (Ise)](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment-overview)SQL Server bağlayıcısı ile Windows kimlik doğrulamasını kullanmak için, bağlayıcının Ise olmayan sürümünü ve şirket içi veri ağ geçidini kullanın. İmKB etiketli sürüm Windows kimlik doğrulamasını desteklemez.

1. **Bağlantı Adı**için bağlantınız için bir ad oluşturun.

1. Tetikleyici veya eylemde, SQL sunucu seçeneklerinin görünmesi için **şirket içi veri ağ geçidi üzerinden Bağlan'ı** seçin.

1. **SQL sunucu Adı** ve SQL veritabanı **adı**için, SQL sunucunuzun adresini ve veritabanınızın adını girin. **Kullanıcı Adı** ve **Şifre**için, sunucunuz için kullanıcı adı ve şifre sağlayın.

   Bu bilgileri bağlantı dizenizde de bulabilirsiniz:

   * `Server=<your-server-address>`
   * `Database=<your-database-name>`
   * `User ID=<your-user-name>`
   * `Password=<your-password>`

   ![SQL Server bağlantısı oluşturma](./media/connectors-create-api-sqlazure/sql-server-create-connection.png)

1. SQL sunucunuz Windows veya Temel kimlik doğrulamasını kullanıyorsa, **Kimlik Doğrulama Türü'nü**seçin.

1. **Ağ Geçitleri**altında, daha önce oluşturulmuş şirket içi veri ağ geçidinizle ilişkili Azure aboneliğini seçin ve şirket içi veri ağ geçidinizin adını seçin.

   Ağ geçidiniz listede görünmüyorsa, ağ geçidinizi doğru şekilde ayarlayıp ayarlamadığınızı kontrol [edin.](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection)

   ![SQL Server bağlantısı tamamlandı oluşturma](./media/connectors-create-api-sqlazure/sql-server-create-connection-complete.png)

1. İşiniz bittiğinde **Oluştur**’u seçin.

1. Bağlantınızı oluşturduktan sonra [SQL tetikleyici ekle](#add-sql-trigger) veya [SQL eylemi ekle](#add-sql-action)ile devam edin.
