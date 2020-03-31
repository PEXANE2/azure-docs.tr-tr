---
title: "Öğretici: RDS MySQL'i MySQL için Azure Veritabanına çevrimiçi geçirin"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak RDS MySQL'den MySQL için Azure Veritabanı'na çevrimiçi geçiş gerçekleştirmeyi öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/08/2020
ms.openlocfilehash: c34de48d0184057f42d1b779abee56e1fa9ac169
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255159"
---
# <a name="tutorial-migrate-rds-mysql-to-azure-database-for-mysql-online-using-dms"></a>Öğretici: RDS MySQL'i DMS kullanarak MySQL çevrimiçi için Azure Veritabanına geçirin

Kaynak veritabanı geçiş sırasında çevrimiçi kalırken, veritabanlarını RDS MySQL örneğinden [MySQL için Azure Veritabanı'na](https://docs.microsoft.com/azure/mysql/) geçirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz. Başka bir deyişle, uygulama için en az kapalı kalma süresi ile geçiş elde edilebilir. Bu öğreticide, Azure Veritabanı Geçiş Hizmeti'ndeki çevrimiçi geçiş etkinliğini kullanarak **Çalışanlar** örnek veritabanını RDS MySQL örneğinden MySQL için Azure Veritabanı'na geçirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> * Mysqldump ve mysql yardımcı programları kullanarak örnek şema geçirin.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturun.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.
> * Geçişi izleme.

> [!NOTE]
> Çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanmak, Premium fiyatlandırma katmanını temel alan bir örnek oluşturmayı gerektirir. Daha fazla bilgi için Azure Veritabanı Geçiş Hizmeti [fiyatlandırma](https://azure.microsoft.com/pricing/details/database-migration/) sayfasına bakın.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, Azure Veritabanı Geçiş Hizmeti’nin bir örneğini hedef veritabanıyla aynı Azure bölgesinde oluşturmayı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, RDS MySQL örneğinden MySQL için Azure Veritabanı'na çevrimiçi geçişin nasıl gerçekleştirilileceği açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* Kaynak MySQL sunucusunun desteklenen bir MySQL topluluk sürümü çalıştırdığından emin olun. MySQL örneğinizin sürümünü mysql yardımcı programı veya MySQL Workbench'te belirlemek için aşağıdaki komutu çalıştırın:

    ```
    SELECT @@version;
    ```

    Daha fazla bilgi için [MySQL sürümleri için Desteklenen Azure Veritabanı](https://docs.microsoft.com/azure/mysql/concepts-supported-versions)makalesine bakın.

* [MySQL **Çalışanları** örnek veritabanını](https://dev.mysql.com/doc/employee/en/employees-installation.html)indirin ve kurun.
* [MySQL için Azure Veritabanı](https://docs.microsoft.com/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)örneği oluşturun.
* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun. Sanal ağ oluşturma hakkında daha fazla bilgi için [Sanal Ağ Belgeleri'ne](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarıiçeren hızlı başlangıç makalelerini görün.
* Sanal ağ Ağ Güvenlik Grubu kurallarınızın Azure Veritabanı Geçiş Hizmeti'ne aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olun: 443, 53, 9354, 445 ve 12000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)makale Filtre ağ trafiği bakın.
* Veritabanı altyapısına erişime izin verecek şekilde [Windows Güvenlik Duvarınızı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) (veya Linux güvenlik duvarınızı) yapılandırın. MySQL sunucusu için bağlantı bağlantısı için 3306 bağlantı noktasına izin verin.

> [!NOTE]
> MySQL için Azure Veritabanı yalnızca InnoDB tablolarını destekler. MyISAM tablolarını InnoDB'ye dönüştürmek için lütfen [Tabloları MyISAM'dan InnoDB'ye dönüştüren makaleye](https://dev.mysql.com/doc/refman/5.7/en/converting-tables-to-innodb.html) bakın.

### <a name="set-up-aws-rds-mysql-for-replication"></a>Çoğaltma için AWS RDS MySQL'i ayarlama

1. Yeni bir parametre grubu oluşturmak için, [MySQL Veritabanı Günlük Dosyaları](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html)makalesinde AWS tarafından sağlanan yönergeleri izleyin , **İkili Günlük Biçimi** bölümünde.
2. Aşağıdaki yapılandırmayla yeni bir parametre grubu oluşturun:
    * binlog_format = row
    * binlog_checksum = YOK
3. Yeni parametre grubunu kaydedin.
4. Yeni parametre grubunu RDS MySQL örneğiyle ilişkilendirin. Yeniden başlatma gerekebilir.

## <a name="migrate-the-schema"></a>Şema geçirin

1. Şemayı kaynak veritabanından ayıklayın ve tablo şemaları, dizinler ve depolanan yordamlar gibi tüm veritabanı nesnelerinin geçişini tamamlamak için hedef veritabanına uygulayın.

    Yalnızca şemayı geçirmenin en kolay yolu --veri yok parametresi ile mysqldump kullanmaktır. Şemayı geçirme komutu:

    ```
    mysqldump -h [servername] -u [username] -p[password] --databases [db name] --no-data > [schema file path]
    ```
    
    Örneğin, **Çalışanlar** veritabanı için şema dosyasını dökümü için aşağıdaki komutu kullanın:
    
    ```
    mysqldump -h 10.10.123.123 -u root -p --databases employees --no-data > d:\employees.sql
    ```

2. MySQL için Azure Veritabanı olan hedef hizmetine şemayı aktarın. Şema döküm dosyasını geri yüklemek için aşağıdaki komutu çalıştırın:

    ```
    mysql.exe -h [servername] -u [username] -p[password] [database]< [schema file path]
    ```

    Örneğin, **Çalışanlar** veritabanı için şemayı almak için:

    ```
    mysql.exe -h shausample.mysql.database.azure.com -u dms@shausample -p employees < d:\employees.sql
    ```

3. Şemanızda yabancı anahtarlar varsa geçişe ilişkin ilk yük ve sürekli eşitleme başarısız olur. Açılan yabancı anahtar komut dosyasını ayıklamak ve hedefe yabancı anahtar komut dosyası eklemek için (MySQL için Azure Veritabanı), MySQL Workbench'te aşağıdaki komut dosyasını çalıştırın:

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

4. Yabancı anahtarı düşürmek için sorgu sonucuna bırakma yabancı anahtarı (ikinci sütun) çalıştırın.

5. Verilerde tetikleyiciler (ekleme veya güncelleştirme tetikleyicisi) varsa, kaynaktan gelen verileri çoğaltmadan önce hedefteki veri bütünlüğünü zorlar. Öneri, geçiş sırasında hedefteki tüm *tablolardaki* tetikleyicileri devre dışı bırakıp geçiş tamamlandıktan sonra tetikleyicileri etkinleştirmektir.

    Hedef veritabanındaki tetikleyicileri devre dışı kalım için:

    ```
    select concat ('alter table ', event_object_table, ' disable trigger ', trigger_name)
    from information_schema.triggers;
    ```

6. Herhangi bir tabloda ENUM veri türü örnekleri varsa, hedef tablodaki 'karakter değişen' veri türüne geçici olarak güncelleştirmenizi öneririz. WHen veri çoğaltma tamamlandı, sonra ENUM için veri türü geri.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-select-subscription1.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **Kaynak sağlayıcılarını**seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-register-resource-provider.png)

## <a name="create-an-instance-of-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti örneği oluşturma

1. Azure portalında + **Kaynak Oluştur'** seçeneğini belirleyin, Azure Veritabanı Geçiş Hizmeti'ni arayın ve ardından açılan listeden **Azure Veritabanı Geçiş Hizmeti'ni** seçin.

    ![Azure Market](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz konumu seçin.

5. Varolan bir sanal ağ seçin veya yeni bir ağ oluşturun.

    Sanal ağ, MySQL örneğine ve MySQL örneği için hedef Azure Veritabanı'na erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar.

    Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/DMSVnet)bakın.

6. Bir fiyatlandırma katmanı seçin; Bu çevrimiçi geçiş için Premium: 4vCores fiyatlandırma katmanını seçtiğinizden emin olun.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-settings3.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

      ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve sonuçlardan bu örneği seçin.

     ![Azure Veritabanı Geçiş Hizmeti örneğinizi bulma](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-instance-search.png)

3. +**Yeni Geçiş Projesi**'ni seçin.
4. Yeni **geçiş projesi** ekranında, **Kaynak sunucu türü** metin kutusunda proje için bir ad belirtin, **MySQL'i**seçin ve ardından **Hedef sunucu türü** metin kutusunda **AzureDbForMySQL'i**seçin.
5. Etkinlik **türünü seç** **bölümünde, Çevrimiçi veri geçişi'ni**seçin.

    > [!IMPORTANT]
    > Çevrimiçi veri **geçişi**seçtiğinizden emin olun; çevrimdışı geçişler bu senaryo için desteklenmez.

    ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-create-project6.png)

    > [!NOTE]
    > Alternatif olarak, yalnızca geçiş projesini şimdi oluşturmak ve geçişi daha sonra yürütmek için **proje oluştur'u** seçebilirsiniz.

6. **Kaydet'i**seçin.

7. Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

    > [!NOTE]
    > Lütfen proje oluşturma bıçağında çevrimiçi geçiş ayarlamak için gereken ön koşulları not edin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

* Geçiş **kaynak ayrıntı** ekranında, kaynak MySQL örneğinin bağlantı ayrıntılarını belirtin.

   ![Kaynak Ayrıntıları](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-source-details5.png)

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. **Kaydet'i**seçin ve ardından **Hedef ayrıntılar** ekranında, önceden sağlanan ve **Çalışanların** şemasını MySQLDump kullanarak dağıtan MySQL sunucusu için hedef Azure Veritabanı'nın bağlantı ayrıntılarını belirtin.

    ![Hedef seçme](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-select-target5.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanı kaynak veritabanıyla aynı veritabanı adını içeriyorsa, Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-map-targets-activity5.png)

3. **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş Özeti](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-migration-summary2.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi görüntülenir ve etkinliğin **Durumu** **Başharftir.**

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Çalıştırılıyor** olana kadar güncelleştirebilirsiniz.

    ![Etkinlik Durumu - çalışan](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-activity-status4.png)

2. **DATABASE NAME**altında, Tam **veri yükü** ve **Artımlı veri eşitleme** işlemleri için geçiş durumuna ulaşmak için belirli bir veritabanı seçin.

    **Tam veri yükü** ilk yük geçiş durumunu gösterirken, **Artıl veri eşitleme** değişim veri yakalama (CDC) durumunu gösterir.

    ![Stok ekranı - tam veri yükü](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-inventory-full-load.png)

    ![Stok ekranı - artımlı veri eşitleme](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-inventory-incremental.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme

İlk Tam yük tamamlandıktan sonra, veritabanları **Kesmeye Hazır**olarak işaretlenir.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

    ![Kesmeye başla](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-inventory-start-cutover.png)

2. **Bekleyen değişiklikler** sayacı **0** değerini gösterene kadar bekleyerek kaynak veritabanına gelen tüm işlemleri durdurduğunuzdan emin olun.
3. **Onayla**'yı ve ardından, **Uygula**'yı seçin.
4. Veritabanı geçiş durumu **Tamamlandı'yı**gösterdiğinde, uygulamalarınızı MySQL veritabanı için yeni hedef Azure Veritabanı'na bağlayın.

MySQL için Azure Veritabanı'na mySQL'in şirket içi bir örneğini çevrimiçi geçişiniz tamamlandı.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
* MySQL için Azure Veritabanı hakkında daha fazla bilgi için, [MySQL için Azure Veritabanı nedir makalesine bakın.](https://docs.microsoft.com/azure/mysql/overview)
* Diğer sorularınız için [Azure Veritabanı Geçişleri Sor](mailto:AskAzureDatabaseMigrations@service.microsoft.com) takma adını e-postayla gönder.
