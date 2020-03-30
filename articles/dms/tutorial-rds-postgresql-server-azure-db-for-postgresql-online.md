---
title: "Öğretici: RDS PostgreSQL'i PostgreSQL için Azure Veritabanına çevrimiçi olarak geçirin"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak RDS PostgreSQL'den PostgreSQL için Azure Veritabanı'na çevrimiçi geçiş gerçekleştirmeyi öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/17/2020
ms.openlocfilehash: 12aa11aa5064b3a0a2ff18f88161f44f37208aec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240689"
---
# <a name="tutorial-migrate-rds-postgresql-to-azure-db-for-postgresql-online-using-dms"></a>Öğretici: RDS PostgreSQL'i DMS kullanarak PostgreSQL için Azure DB'ye geçirin

Kaynak veritabanı geçiş sırasında çevrimiçi kalırken, veritabanlarını RDS PostgreSQL örneğinden [PostgreSQL için Azure Veritabanı'na](https://docs.microsoft.com/azure/postgresql/) geçirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz. Başka bir deyişle, uygulama için en az kapalı kalma süresi ile geçiş elde edilebilir. Bu eğitimde, DVD **Kiralama** örnek veritabanını RDS PostgreSQL 9.6 örneğinden, Azure Veritabanı Geçiş Hizmeti'ndeki çevrimiçi geçiş etkinliğini kullanarak PostgreSQL için Azure Veritabanı'na geçirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> * örnek şema pg_dump yardımcı programı kullanarak geçirin.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturun.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.
> * Geçişi izleme.
> * Geçiş kesme gerçekleştirin.

> [!NOTE]
> Çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanmak, Premium fiyatlandırma katmanını temel alan bir örnek oluşturmayı gerektirir. Daha fazla bilgi için Azure Veritabanı Geçiş Hizmeti [fiyatlandırma](https://azure.microsoft.com/pricing/details/database-migration/) sayfasına bakın. Geçiş işlemi sırasında veri hırsızlığını önlemek için diski şifreleriz.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, Azure Veritabanı Geçiş Hizmeti’nin bir örneğini hedef veritabanıyla aynı Azure bölgesinde oluşturmayı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, PostgreSQL'in şirket içi bir örneğinden PostgreSQL için Azure Veritabanı'na çevrimiçi geçişin nasıl gerçekleştirilileceği açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* [PostgreSQL topluluk sürümü](https://www.postgresql.org/download/) 9.5, 9.6 veya 10'u indirin ve yükleyin. Kaynak PostgreSQL Server sürümü 9.5.11, 9.6.7, 10 veya daha sonra olmalıdır. Daha fazla bilgi için, [makaleye](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)bkz.

    Ayrıca, RDS PostgreSQL sürümü PostgreSQL sürümü için Azure Veritabanı ile eşleşmelidir. Örneğin, RDS PostgreSQL 9.5.11.5, 9.6.7 sürümüne değil, yalnızca PostgreSQL 9.5.11 için Azure Veritabanı'na geçiş yapabilir.

* [PostgreSQL için Azure Veritabanı](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal) örneği veya [PostgreSQL için Azure Veritabanı örneği oluşturun - Hyperscale (Citus)](https://docs.microsoft.com/azure/postgresql/quickstart-create-hyperscale-portal). PgAdmin kullanarak PostgreSQL Server'a nasıl bağlanılabağlana ilgili ayrıntılı bilgi için belgenin bu [bölümüne](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal#connect-to-the-postgresql-server-using-pgadmin) bakın.
* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun. Sanal ağ oluşturma hakkında daha fazla bilgi için [Sanal Ağ Belgeleri'ne](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarıiçeren hızlı başlangıç makalelerini görün.
* Sanal ağ Ağ Güvenlik Grubu kurallarınızın Azure Veritabanı Geçiş Hizmeti'ne aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olun: 443, 53, 9354, 445 ve 12000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)makale Filtre ağ trafiği bakın.
* [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* Azure Veritabanı Geçiş Hizmeti'nin varsayılan olarak TCP bağlantı noktası 5432 olan kaynak PostgreSQL sunucusuna erişmesine izin vermek için Windows güvenlik duvarınızı açın.
* Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
* Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesine izin vermek için PostgreSQL sunucusu için Azure Veritabanı için sunucu düzeyinde bir [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını sağlayın.

### <a name="set-up-aws-rds-postgresql-for-replication"></a>Çoğaltma için AWS RDS PostgreSQL'i ayarlama

1. Yeni bir parametre grubu oluşturmak için, [DB Parametre Grupları ile Çalışma](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html)makalesinde AWS tarafından sağlanan yönergeleri izleyin.
2. Azure Veritabanı Geçiş Hizmeti'nden kaynağa bağlanmak için ana kullanıcı adını kullanın. Ana kullanıcı hesabı dışında bir hesap kullanıyorsanız, hesabın rds_superuser rolü ve rds_replication rolü olmalıdır. rds_replication rolü, mantıksal yuvaları yönetmek ve mantıksal yuvaları kullanarak veri akışı için izin verir.
3. Aşağıdaki yapılandırmayla yeni bir parametre grubu oluşturun:

    a. DB parametre grubunuzun rds.logical_replication parametresini 1 olarak ayarlayın.

    b. max_wal_senders =[eşzamanlı görev sayısı] - max_wal_senders parametresi, çalıştırılabilen eşzamanlı görev sayısını belirler, 10 görev önerir.

    c. max_replication_slots – = [yuva sayısı], beş yuvaya ayarlı öneririz.

4. Oluşturduğunuz parametre grubunu RDS PostgreSQL örneğine ilişkilendirin.

## <a name="migrate-the-schema"></a>Şema geçirin

1. Şemayı kaynak veritabanından ayıklayın ve tablo şemaları, dizinler ve depolanan yordamlar gibi tüm veritabanı nesnelerinin geçişini tamamlamak için hedef veritabanına uygulayın.

    Yalnızca şemayı geçirmenin en kolay yolu -s seçeneği ile pg_dump kullanmaktır. Daha fazla bilgi için Postgres pg_dump öğreticideki [örneklere](https://www.postgresql.org/docs/9.6/app-pgdump.html#PG-DUMP-EXAMPLES) bakın.

    ```
    pg_dump -o -h hostname -U db_username -d db_name -s > your_schema.sql
    ```

    Örneğin, **dvdrental** veritabanı için bir şema dosyasını dökümü için aşağıdaki komutu kullanın:

    ```
    pg_dump -o -h localhost -U postgres -d dvdrental -s  > dvdrentalSchema.sql
    ```

2. Hedef serviste, PostgreSQL için Azure Veritabanı olan boş bir veritabanı oluşturun. Bağlanmak ve veritabanı oluşturmak için aşağıdaki makalelerden birine bakın:

    * [Azure portalını kullanarak PostgreSQL sunucusu için bir Azure Veritabanı oluşturma](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal)
    * [Azure portalını kullanarak PostgreSQL - Hyperscale (Citus) sunucusu için bir Azure Veritabanı oluşturma](https://docs.microsoft.com/azure/postgresql/quickstart-create-hyperscale-portal)

3. PostgreSQL için Azure Veritabanı olan hedef hizmetine şemayı aktarın. Şema döküm dosyasını geri yüklemek için aşağıdaki komutu çalıştırın:

    ```
    psql -h hostname -U db_username -d db_name < your_schema.sql
    ```

    Örnek:

    ```
    psql -h mypgserver-20170401.postgres.database.azure.com  -U postgres -d dvdrental < dvdrentalSchema.sql
    ```

4. Şemanızda yabancı anahtarlar varsa geçişe ilişkin ilk yük ve sürekli eşitleme başarısız olur. Drop foreign key komut dosyasını ayıklamak ve hedefe yabancı anahtar komut dosyası eklemek için (PostgreSQL için Azure Veritabanı), Aşağıdaki komut dosyasını PgAdmin veya psql'de çalıştırın:
  
    ```
    SELECT Queries.tablename
           ,concat('alter table ', Queries.tablename, ' ', STRING_AGG(concat('DROP CONSTRAINT ', Queries.foreignkey), ',')) as DropQuery
                ,concat('alter table ', Queries.tablename, ' ',
                                                STRING_AGG(concat('ADD CONSTRAINT ', Queries.foreignkey, ' FOREIGN KEY (', column_name, ')', 'REFERENCES ', foreign_table_name, '(', foreign_column_name, ')' ), ',')) as AddQuery
        FROM
        (SELECT
        tc.table_schema,
        tc.constraint_name as foreignkey,
        tc.table_name as tableName,
        kcu.column_name,
        ccu.table_schema AS foreign_table_schema,
        ccu.table_name AS foreign_table_name,
        ccu.column_name AS foreign_column_name
    FROM
        information_schema.table_constraints AS tc
        JOIN information_schema.key_column_usage AS kcu
          ON tc.constraint_name = kcu.constraint_name
          AND tc.table_schema = kcu.table_schema
        JOIN information_schema.constraint_column_usage AS ccu
          ON ccu.constraint_name = tc.constraint_name
          AND ccu.table_schema = tc.table_schema
    WHERE constraint_type = 'FOREIGN KEY') Queries
      GROUP BY Queries.tablename;
    ```

5. Yabancı anahtarı düşürmek için sorgu sonucuna bırakma yabancı anahtarı (ikinci sütun) çalıştırın.

6. Verilerde tetikleyiciler (ekleme veya güncelleştirme tetikleyicisi) varsa, kaynaktan gelen verileri çoğaltmadan önce hedefteki veri bütünlüğünü zorlar. Öneri, geçiş sırasında hedefteki tüm *tablolardaki* tetikleyicileri devre dışı bırakıp geçiş tamamlandıktan sonra tetikleyicileri etkinleştirmektir.

    Hedef veritabanındaki tetikleyicileri devre dışı kalım için:

    ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
    ```

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-select-subscription1.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **Kaynak sağlayıcılarını**seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-register-resource-provider.png)

## <a name="create-an-instance-of-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti örneği oluşturma

1. Azure portalında + **Kaynak Oluştur'** seçeneğini belirleyin, Azure Veritabanı Geçiş Hizmeti'ni arayın ve ardından açılan listeden **Azure Veritabanı Geçiş Hizmeti'ni** seçin.

    ![Azure Market](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz konumu seçin.

5. Varolan bir sanal ağ seçin veya yeni bir ağ oluşturun.

    Sanal ağ, Kaynak PostgreSQL örneğine ve PostgreSQL örneği için hedef Azure Veritabanı'na erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar.

    Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/DMSVnet)bakın.

6. Bir fiyatlandırma katmanı seçin; Bu çevrimiçi geçiş için Premium: 4vCores fiyatlandırma katmanını seçtiğinizden emin olun.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-settings5.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

      ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-search.png)

2. Azure **Veritabanı Geçiş Hizmetleri** ekranında, oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın, örneği seçin ve sonra + **Yeni Geçiş Projesi'ni**seçin.
3. Yeni **geçiş projesi** ekranında, **Kaynak sunucu türü** metin kutusunda proje için bir ad belirtin, **PostgreSQL için AWS RDS'yi**seçin ve ardından **Hedef sunucu türü** metin kutusunda **PostgreSQL için Azure Veritabanı'nı**seçin.
4. Etkinlik **türünü seç** **bölümünde, Çevrimiçi veri geçişi'ni**seçin.

    > [!IMPORTANT]
    > Çevrimiçi veri **geçişi**seçtiğinizden emin olun; çevrimdışı geçişler bu senaryo için desteklenmez.

    ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-create-project5.png)

    > [!NOTE]
    > Alternatif olarak, yalnızca geçiş projesini şimdi oluşturmak ve geçişi daha sonra yürütmek için **proje oluştur'u** seçebilirsiniz.

5. **Kaydet'i**seçin.

6. Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

    > [!NOTE]
    > Lütfen proje oluşturma bıçağında çevrimiçi geçiş ayarlamak için gereken ön koşulları not edin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

* Kaynak **Ayrıntıları Ekle** ekranında, kaynak PostgreSQL örneğinin bağlantı ayrıntılarını belirtin.

   ![Kaynak Ayrıntıları](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-source-details5.png)

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. **Kaydet'i**seçin ve ardından **Hedef ayrıntılar** ekranında, önceden sağlanan ve **DVD Kiralama** şemasını pg_dump kullanarak dağıtılan PostgreSQL sunucusu için hedef Azure Veritabanı'nın bağlantı ayrıntılarını belirtin.

    ![Hedef ayrıntılar](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-target-details.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanı kaynak veritabanıyla aynı veritabanı adını içeriyorsa, Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-map-target-databases.png)

3. **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş Özeti](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-migration-summary1.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi görüntülenir ve etkinliğin **Durumu** **Başharftir.**

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Çalıştırılıyor** olana kadar güncelleştirebilirsiniz.

    ![Etkinlik Durumu - çalışan](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-activity-status3.png)

2. **DATABASE NAME**altında, Tam **veri yükü** ve **Artımlı veri eşitleme** işlemleri için geçiş durumuna ulaşmak için belirli bir veritabanı seçin.

    **Tam veri yükü** ilk yük geçiş durumunu gösterirken, **Artıl veri eşitleme** değişim veri yakalama (CDC) durumunu gösterir.

    ![Stok ekranı - tam veri yükü](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-inventory-full-load.png)

    ![Stok ekranı - artımlı veri eşitleme](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-inventory-incremental.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme

İlk Tam yük tamamlandıktan sonra, veritabanları **Kesmeye Hazır**olarak işaretlenir.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

2. Kaynak veritabanına gelen tüm hareketlerin durdurulduğundan emin olmak için **Bekleyen değişiklikler** sayacı **0** gösterene kadar bekleyin, **onay** kutusunu seçin ve sonra **Uygula'yı**seçin.

    ![Komple kesme ekranı](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-complete-cutover.png)

3. Veritabanı geçiş durumu **Tamamlandı'yı**gösterdiğinde, uygulamalarınızı PostgreSQL veritabanı için yeni hedef Azure Veritabanı'na bağlayın.

Ar-Ge PostgreSQL'in şirket içi bir örneğini PostgreSQL için Azure Veritabanına çevrimiçi geçişiniz tamamlandı.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
* PostgreSQL için Azure Veritabanı hakkında daha fazla bilgi için, [PostgreSQL için Azure Veritabanı nedir makalesine bakın.](https://docs.microsoft.com/azure/postgresql/overview)
* Diğer sorularınız için [Azure Veritabanı Geçişleri Sor](mailto:AskAzureDatabaseMigrations@service.microsoft.com) takma adını e-postayla gönder.
