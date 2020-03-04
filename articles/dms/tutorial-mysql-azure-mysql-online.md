---
title: "Öğretici: MySQL için Azure veritabanı 'na MySQL online geçirme"
titleSuffix: Azure Database Migration Service
description: Azure veritabanı geçiş hizmeti 'ni kullanarak şirket içi MySQL 'ten MySQL için Azure veritabanı 'na çevrimiçi geçiş gerçekleştirmeyi öğrenin.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/08/2020
ms.openlocfilehash: e0a2c9f4dd229353ef3d4dc06f7bb965d15814d9
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78255548"
---
# <a name="tutorial-migrate-mysql-to-azure-database-for-mysql-online-using-dms"></a>Öğretici: DMS hizmetini kullanarak çevrimiçi ortamda MySQL'i MySQL için Azure Veritabanı'na geçirme

Azure veritabanı geçiş hizmeti 'ni kullanarak veritabanlarını şirket içi bir MySQL örneğinden, en az kapalı kalma süresiyle [MySQL Için Azure veritabanı](https://docs.microsoft.com/azure/mysql/) 'na geçirebilirsiniz. Diğer bir deyişle, geçiş işlemi, uygulamada minimum çalışmama süresi ile gerçekleştirilebilir. Bu öğreticide, Azure veritabanı geçiş hizmeti 'nde çevrimiçi geçiş etkinliği kullanarak MySQL 5,7 ' in şirket içi bir örneğinden **çalışanlar** örnek veritabanını MySQL Için Azure veritabanı 'na geçireceğiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> * mysqldump yardımcı programını kullanarak örnek şemayı geçirme.
> * Azure veritabanı geçiş hizmeti 'nin bir örneğini oluşturun.
> * Azure veritabanı geçiş hizmeti 'ni kullanarak bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.
> * Geçişi izleme.

> [!NOTE]
> Çevrimiçi bir geçiş gerçekleştirmek için Azure veritabanı geçiş hizmeti 'nin kullanılması, Premium fiyatlandırma katmanını temel alan bir örnek oluşturulmasını gerektirir.

> [!IMPORTANT]
> En iyi geçiş deneyimi için, Microsoft, hedef veritabanıyla aynı Azure bölgesinde Azure veritabanı geçiş hizmeti örneği oluşturulmasını önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* [MySQL Community Edition](https://dev.mysql.com/downloads/mysql/) 5.6 veya 5.7'yi indirme ve yükleme. Şirket içi MySQL sürümü ile MySQL için Azure Veritabanı sürümü eşleşmelidir. Örneğin, MySQL 5.6 yalnızca MySQL için Azure Veritabanı 5.6'ya geçirilebilir; 5.7 sürümüne yükseltilemez.
* [MySQL için Azure Veritabanı örneği oluşturma](https://docs.microsoft.com/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal). Azure portal'ı kullanarak veritabanı oluşturma ve veritabanlarına bağlanma ile ilgili ayrıntılar için [MySQL Workbench kullanarak bağlanma ve veri sorgulama](https://docs.microsoft.com/azure/mysql/connect-workbench) başlıklı makaleye bakın.  
* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlayan Azure Resource Manager dağıtım modelini kullanarak Azure veritabanı geçiş hizmeti için bir Microsoft Azure sanal ağ oluşturun. Sanal ağ oluşturma hakkında daha fazla bilgi için [sanal ağ belgelerine](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarla birlikte hızlı başlangıç makalelerine bakın.

    > [!NOTE]
    > Sanal networkNet kurulumu sırasında, Microsoft 'a ağ eşlemesi ile ExpressRoute kullanırsanız, hizmetin sağlanacağı alt ağa aşağıdaki hizmet [uç noktalarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) ekleyin:
    >
    > * Hedef veritabanı uç noktası (örneğin, SQL uç noktası, Cosmos DB uç noktası vb.)
    > * Depolama uç noktası
    > * Service Bus uç noktası
    >
    > Azure veritabanı geçiş hizmeti internet bağlantısı olmadığından bu yapılandırma gereklidir.

* Sanal ağ ağ güvenlik grubu kurallarınızın, Azure veritabanı geçiş hizmeti 'ne yönelik aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olun: 443, 53, 9354, 445, 12000. Sanal ağ NSG trafik filtrelemesi hakkında daha fazla bilgi için ağ [güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)makalesine bakın.
* [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* Azure veritabanı geçiş hizmeti 'nin kaynak MySQL sunucusuna erişmesine izin vermek için Windows Güvenlik duvarınızı açın, varsayılan olarak TCP bağlantı noktası 3306 ' dir.
* Kaynak veritabanınızın önünde bir güvenlik duvarı gereci kullanırken, Azure veritabanı geçiş hizmeti 'nin geçiş için kaynak veritabanına erişmesine izin vermek üzere güvenlik duvarı kuralları eklemeniz gerekebilir.
* Azure veritabanı geçiş hizmeti 'nin hedef veritabanlarına erişmesine izin vermek için MySQL için Azure veritabanı 'nın sunucu düzeyinde bir [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure veritabanı geçiş hizmeti için kullanılan sanal ağın alt ağ aralığını belirtin.
* Kaynak MySQL'in desteklenen MySQL Community Edition sürümünde olması gerekir. MySQL örneğinin sürümünü belirlemek için, MySQL yardımcı programında veya MySQL Workbench'te şu komutu çalıştırın:

    ```
    SELECT @@version;
    ```

* MySQL için Azure Veritabanı yalnızca InnoDB tablolarını destekler. MyISAM tablolarını InnoDB'ye dönüştürmek için [Converting Tables from MyISAM to InnoDB (Tabloları MyISAM'dan InnoDB'ye Dönüştürme)](https://dev.mysql.com/doc/refman/5.7/en/converting-tables-to-innodb.html) başlıklı makaleye bakın

* Şu yapılandırmayı kullanarak kaynak veritabanındaki my.ini (Windows) veya my.cnf (Unix) dosyasında ikili günlük dosyası kaydetmeyi etkinleştirin:

  * **server_id** = 1 veya üzeri (yalnızca MySQL 5.6 için geçerlidir)
  * **günlük-bin** =\<yol > (yalnızca MySQL 5,6 için geçerlidir) Örneğin: log-bin = e:\ MySQL_logs \binlog
  * **binlog_format** = row
  * **Expire_logs_days** = 5 (sıfır kullanılması önerilir; yalnızca MySQL 5,6 için geçerlidir)
  * **Binlog_row_image** = full (yalnızca MySQL 5.6 için geçerlidir)
  * **log_slave_updates** = 1

* Kullanıcının, şu ayrıcalıkları içeren ReplicationAdmin rolüne sahip olması gerekir:

  * **ÇOĞALTMA İSTEMCİSİ** - Yalnızca değişiklik işleme görevleri için gereklidir. Başka bir deyişle, bu ayrıcalık yalnızca Tam Yük görevleri için gerekli değildir.
  * **ÇOĞALTMA ÇOĞALTMASI** - Yalnızca değişiklik işleme görevleri için gereklidir. Başka bir deyişle, bu ayrıcalık yalnızca Tam Yük görevleri için gerekli değildir.
  * **SÜPER** - yalnızca MySQL 5.6.6'dan önceki sürümlerde gereklidir.

## <a name="migrate-the-sample-schema"></a>Örnek şemayı geçirme

Tablo şemaları, dizinler ve saklı yordamlar gibi tüm veritabanı nesnelerini tamamlamak için kaynak veritabanındaki şemayı ayıklamamız ve veritabanına uygulamamız gerekir. Şemayı ayıklamak için `--no-data` parametresiyle mysqldump yardımcı programını kullanabilirsiniz.

MySQL **çalışanlarının** şirket içi sistemde örnek veritabanı olduğunu varsayarak, mysqldump kullanarak şema geçişi yapmak için komutu:

```
mysqldump -h [servername] -u [username] -p[password] --databases [db name] --no-data > [schema file path]
```

Örnek:

```
mysqldump -h 10.10.123.123 -u root -p --databases employees --no-data > d:\employees.sql
```

Şemayı MySQL için Azure Veritabanı hedefine aktarmak için şu komutu çalıştırın:

```
mysql.exe -h [servername] -u [username] -p[password] [database]< [schema file path]
 ```

Örnek:

```
mysql.exe -h shausample.mysql.database.azure.com -u dms@shausample -p employees < d:\employees.sql
 ```

Şemanızda yabancı anahtarlar varsa geçişe ilişkin ilk yük ve sürekli eşitleme başarısız olur.  Bırakma yabancı anahtar betiği çıkarmak ve yabancı anahtar betiği eklemek için MySQL çalışma ekranında aşağıdaki betiği yürütün.

```
SET group_concat_max_len = 8192;
    SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
    FROM
    (SELECT
    KCU.REFERENCED_TABLE_SCHEMA as SchemaName,
    KCU.TABLE_NAME,
    KCU.COLUMN_NAME,
    CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
    CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
    FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
    WHERE
      KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
      AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
  AND KCU.REFERENCED_TABLE_SCHEMA = 'SchemaName') Queries
  GROUP BY SchemaName;
 ```

Yabancı anahtarı bırakmak için sorgu sonucunda DROP yabancı anahtarını (ikinci sütun) çalıştırın.

> [!IMPORTANT]
> Bir yedekleme kullanarak veri içeri aktardıysanız, bir mysqldump gerçekleştirirken CREATE DEFINER komutlarını manuel olarak veya--Skip-definer komutunu kullanarak kaldırın. DEFINER, MySQL için Azure veritabanı 'nda oluşturulacak ve kısıtlanmış bir süper ayrıcalık gerektirir.

Veride (ekleme veya güncelleştirme tetikleyicisi) bir Tetikleyiciniz varsa, çoğaltılan verilerin önünde kaynak üzerinde veri bütünlüğünü zorlayacaktır. Geçiş sırasında tüm tablolardaki tetikleyicileri devre dışı bırakmanız ve geçişin tamamlanmasının ardından etkinleştirmeniz önerilir.

Hedef veritabanındaki Tetikleyicileri devre dışı bırakmak için aşağıdaki komutu kullanın:

```
SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
```

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-mysql-to-azure-mysql-online/portal-select-subscriptions.png)

2. Azure veritabanı geçiş hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **kaynak sağlayıcıları**' nı seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-mysql-to-azure-mysql-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-mysql-to-azure-mysql-online/portal-register-resource-provider.png)

## <a name="create-a-dms-instance"></a>DMS örneği oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Market](media/tutorial-mysql-to-azure-mysql-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-mysql-to-azure-mysql-online/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Var olan bir sanal ağı seçin veya yeni bir ağ oluşturun.

    Sanal ağ, kaynak SQL Server ve hedef Azure SQL veritabanı örneğine erişimi olan Azure veritabanı geçiş hizmeti sağlar.

    Azure portal sanal ağ oluşturma hakkında daha fazla bilgi için [Azure Portal kullanarak sanal ağ oluşturma](https://aka.ms/DMSVnet)makalesine bakın.

5. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-mysql-to-azure-mysql-online/dms-settings3.png)

6. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

      ![Azure veritabanı geçiş hizmeti 'nin tüm örneklerini bulun](media/tutorial-mysql-to-azure-mysql-online/dms-search.png)

2. **Azure veritabanı geçiş Hizmetleri** ekranında, oluşturduğunuz Azure veritabanı geçiş hizmeti örneğinin adını arayın ve ardından örneği seçin.

     ![Azure veritabanı geçiş hizmeti örneğinizi bulun](media/tutorial-mysql-to-azure-mysql-online/dms-instance-search.png)

3. +**Yeni Geçiş Projesi**'ni seçin.
4. **Yeni geçiş projesi** ekranında proje için bir ad belirtin, **Kaynak sunucu türü** metin kutusunda **MySQL**, **Hedef sunucu türü** metin kutusunda ise **AzureDbForMySQL** seçeneğini belirleyin.
5. **Etkinlik türünü seçin** bölümünde, **Çevrimiçi veri geçişi** seçeneğini belirleyin

    ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-mysql-to-azure-mysql-online/dms-create-project4.png)

    > [!NOTE]
    > Alternatif olarak, şimdi **proje oluştur** ' u seçerek geçiş projesini hemen oluşturabilir ve geçişi daha sonra yürütebilirsiniz.

6. **Kaydet**'i seçin, verileri geçirmek üzere DMS hizmetini başarıyla kullanmaya yönelik gereksinimlere dikkat edin ve **Etkinlik oluştur ve çalıştır** seçeneğini belirleyin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Kaynak Ayrıntıları Ekleyin** ekranında, kaynak MySQL örneği için bağlantı ayrıntılarını belirtin.

    ![Kaynak Ayrıntıları Ekleyin ekranı](media/tutorial-mysql-to-azure-mysql-online/dms-add-source-details.png)

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. **Kaydet**'i seçip **Hedef ayrıntıları** ekranında, **Employees** şemasının mysqldump kullanılarak dağıtıldığı MySQL için Azure Veritabanı'nın önceden sağlanmış örneği olan hedef MySQL için Azure Veritabanı sunucusuna ilişkin bağlantı ayrıntılarını belirtin.

    ![Hedef ayrıntıları ekranı](media/tutorial-mysql-to-azure-mysql-online/dms-add-target-details.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanı, kaynak veritabanıyla aynı veritabanı adını içeriyorsa, Azure veritabanı geçiş hizmeti varsayılan olarak hedef veritabanını seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-mysql-to-azure-mysql-online/dms-map-target-details.png)

3. **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş Özeti](media/tutorial-mysql-to-azure-mysql-online/dms-migration-summary.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi açılır ve etkinliğin **Durum** bilgisi **Başlatılıyor** olarak değişir.

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Tamamlandı** olana kadar güncelleştirebilirsiniz.

     ![Etkinlik Durumu - tamamlandı](media/tutorial-mysql-to-azure-mysql-online/dms-activity-completed.png)

2. **Veritabanı Adı** bölümünde, **Tam veri yüklemesi** ve **Artımlı veri eşitleme** işlemleri için geçiş durumunu almak üzere belirli bir veritabanı seçin.

    Tam veri yüklemesi ilk yük geçiş durumunu, Artımlı veri eşitleme ise değişiklik verilerini yakalama (CDC) durumunu gösterir.

     ![Etkinlik Durumu - Tam yük tamamlandı](media/tutorial-mysql-to-azure-mysql-online/dms-activity-full-load-completed.png)

     ![Etkinlik Durumu - Artımlı veri eşitleme](media/tutorial-mysql-to-azure-mysql-online/dms-activity-incremental-data-sync.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme

İlk Tam yük tamamlandıktan sonra, veritabanları **Geçiş için hazır** olarak işaretlenir.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

    ![Tam geçişi başlat](media/tutorial-mysql-to-azure-mysql-online/dms-start-cutover.png)

2. **Bekleyen değişiklikler** sayacı **0** değerini gösterene kadar bekleyerek kaynak veritabanına gelen tüm işlemleri durdurduğunuzdan emin olun.
3. **Onayla**'yı ve ardından, **Uygula**'yı seçin.
4. Veritabanı geçişi durumu **Tamamlandı** olarak gösterildiğinde, uygulamalarınızı yeni hedef Azure SQL Veritabanı'na bağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* MySQL için Azure Veritabanı'na yönelik çevrimiçi geçiş gerçekleştirirken karşılaşılan bilinen sorunlar ve sınırlamalar hakkında bilgi için [MySQL için Azure Veritabanı geçiş işlemleri ile ilgili bilinen sorunlar ve geçici çözümler](known-issues-azure-mysql-online.md) başlıklı makaleye bakın.
* Azure veritabanı geçiş hizmeti hakkında daha fazla bilgi için [Azure veritabanı geçiş hizmeti nedir?](https://docs.microsoft.com/azure/dms/dms-overview)makalesine bakın.
* MySQL için Azure veritabanı hakkında daha fazla bilgi için, [MySQL Için Azure veritabanı nedir?](https://docs.microsoft.com/azure/mysql/overview)makalesine bakın.
