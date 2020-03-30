---
title: "Öğretici: PostgreSQL'i Azure portalı üzerinden çevrimiçi PostgreSQL için Azure DB'ye geçirin"
titleSuffix: Azure Database Migration Service
description: Azure portalı üzerinden Azure Veritabanı Geçiş Hizmeti'ni kullanarak PostgreSQL'den Çevrimiçi Olarak PostgreSQL'den PostgreSQL için Azure Veritabanı'na çevrimiçi geçiş gerçekleştirmeyi öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 03/25/2020
ms.openlocfilehash: 4985c492c8ca71da87cf1a519ebc658c203d3952
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246985"
---
# <a name="tutorial-migrate-postgresql-to-azure-db-for-postgresql-online-using-dms-via-the-azure-portal"></a>Öğretici: PostgreSQL'i Azure portalı üzerinden DMS kullanarak PostgreSQL çevrimiçi için Azure DB'ye geçirin

Veritabanlarını şirket içi PostgreSQL örneğinden, uygulama için en az kapalı kalma süresiyle [PostgreSQL için Azure Veritabanı'na](https://docs.microsoft.com/azure/postgresql/) geçirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz. Bu eğitimde, DVD **Kiralama** örnek veritabanını, Azure Veritabanı Geçiş Hizmeti'ndeki çevrimiçi geçiş etkinliğini kullanarak PostgreSQL 9.6'nın şirket içi bir örneğinden PostgreSQL için Azure Veritabanı'na geçirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> * örnek şema pg_dump yardımcı programı kullanarak geçirin.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturun.
> * Azure Veritabanı Geçiş Hizmeti'nde bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.
> * Geçişi izleme.
> * Geçiş kesme gerçekleştirin.

> [!NOTE]
> Çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanmak, Premium fiyatlandırma katmanını temel alan bir örnek oluşturmayı gerektirir. Geçiş işlemi sırasında veri hırsızlığını önlemek için diski şifreleriz

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, hedef veritabanıyla aynı Azure bölgesinde Azure Veritabanı Geçiş Hizmeti örneği oluşturmanızı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* [PostgreSQL topluluk sürümünü](https://www.postgresql.org/download/) 9.4, 9.5, 9.6 veya 10'u indirin ve yükleyin. Kaynak PostgreSQL Server sürümü 9.4, 9.5, 9.6, 10 veya 11 olmalıdır. Daha fazla bilgi için, [makaleye](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)bkz.

    Ayrıca, şirket içi PostgreSQL sürümünün, PostgreSQL için Azure Veritabanı sürümü ile eşleşmesi gerekir. Örneğin, PostgreSQL 9.6 yalnızca PostgreSQL 9.6, 10 veya 11 için Azure Veritabanı'na geçiş yapabilir, ancak PostgreSQL 9.5 için Azure Veritabanı'na geçiremez.

* [PostgreSQL sunucusu için bir Azure Veritabanı oluşturun](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal) veya [PostgreSQL - Hyperscale (Citus) sunucusu için bir Azure Veritabanı oluşturun.](https://docs.microsoft.com/azure/postgresql/quickstart-create-hyperscale-portal)
* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun. Sanal ağ oluşturma hakkında daha fazla bilgi için [Sanal Ağ Belgeleri'ne](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarıiçeren hızlı başlangıç makalelerini görün.

    > [!NOTE]
    > Sanal ağ kurulumu sırasında, Microsoft'a ağ la bakan ExpressRoute kullanıyorsanız, hizmetin sağlandığı alt ağa aşağıdaki hizmet [bitiş noktalarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) ekleyin:
    >
    > * Hedef veritabanı bitiş noktası (örneğin, SQL bitiş noktası, Cosmos DB bitiş noktası vb.)
    > * Depolama bitiş noktası
    > * Servis veri günü bitiş noktası
    >
    > Azure Veritabanı Geçiş Hizmeti internet bağlantısından yoksun olduğundan bu yapılandırma gereklidir.

* Sanal ağınız için Ağ Güvenlik Grubu (NSG) kurallarının Azure Veritabanı Geçiş Hizmeti'ne aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olun: 443, 53, 9354, 445, 12000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)makale Filtre ağ trafiği bakın.
* [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* Azure Veritabanı Geçiş Hizmeti'nin varsayılan olarak TCP bağlantı noktası 5432 olan kaynak PostgreSQL Server'a erişmesine izin vermek için Windows güvenlik duvarınızı açın.
* Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
* Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesine izin vermek için PostgreSQL için Azure Veritabanı için sunucu düzeyinde bir [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını sağlayın.
* postgresql.config dosyasında mantıksal çoğaltmayı etkinleştirin ve aşağıdaki parametreleri ayarlayın:

  * wal_level = **logical**
  * max_replication_slots = [yuva sayısı], **beş yuvaya** ayar öneririz
  * max_wal_senders =[eşzamanlı görev sayısı] - max_wal_senders parametresi, çalışabilecek eşzamanlı görevlerin sayısını ayarlar; önerilen ayar **10 görevdir**

> [!IMPORTANT]
> Varolan veritabanınızdaki tüm tabloların, değişikliklerin hedef veritabanıyla eşitlenebileceğinden emin olmak için birincil anahtar gerekir.

## <a name="migrate-the-sample-schema"></a>Örnek şemayı geçirme

Tablo şemaları, dizinler ve saklı yordamlar gibi tüm veritabanı nesnelerini tamamlamak için kaynak veritabanındaki şemayı ayıklamamız ve veritabanına uygulamamız gerekir.

1. Bir veritabanına yönelik şema döküm dosyası oluşturmak için pg_dump -s komutunu kullanın.

    ```
    pg_dump -o -h hostname -U db_username -d db_name -s > your_schema.sql
    ```

    Örneğin, **dvdrental** veritabanı için bir şema dökümü dosyası oluşturmak için:

    ```
    pg_dump -o -h localhost -U postgres -d dvdrental -s -O -x > dvdrentalSchema.sql
    ```

    pg_dump yardımcı programını kullanma hakkında daha fazla bilgi için [pg-dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html#PG-DUMP-EXAMPLES) öğreticisindeki örneklere bakın.

2. Hedef ortamınız olan PostgreSQL için Azure Veritabanı içinde boş bir veritabanı oluşturun.

    Veritabanına nasıl bağlanıp oluşturulacaklarına ilişkin ayrıntılar için, [Azure portalında PostgreSQL sunucusu için Bir Azure Veritabanı Oluştur](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal) veya Azure [portalında PostgreSQL - Hyperscale (Citus) sunucusu için bir Azure Veritabanı oluşturun makalesine](https://docs.microsoft.com/azure/postgresql/quickstart-create-hyperscale-portal)bakın.

    > [!NOTE]
    > PostgreSQL için Azure Veritabanı bir örneği - Hyperscale (Citus) sadece tek bir veritabanı vardır: **citus**.

3. Şema döküm dosyasını geri yükleyerek, şemayı oluşturduğunuz hedef veritabanına aktarın.

    ```
    psql -h hostname -U db_username -d db_name < your_schema.sql
    ```

    Örnek:

    ```
    psql -h mypgserver-20170401.postgres.database.azure.com  -U postgres -d dvdrental citus < dvdrentalSchema.sql
    ```

4. Damla yabancı anahtar komut dosyasını ayıklamak ve hedefte eklemek için (PostgreSQL için Azure Veritabanı), PgAdmin veya psql'de aşağıdaki komut dosyasını çalıştırın.

   > [!IMPORTANT]
   > Şemanızdaki yabancı anahtarlar, geçişin ilk yükünün ve sürekli eşitlemenin başarısız lığa neden olur.

    ```
    SELECT Q.table_name
        ,CONCAT('ALTER TABLE ', table_schema, '.', table_name, STRING_AGG(DISTINCT CONCAT(' DROP CONSTRAINT ', foreignkey), ','), ';') as DropQuery
            ,CONCAT('ALTER TABLE ', table_schema, '.', table_name, STRING_AGG(DISTINCT CONCAT(' ADD CONSTRAINT ', foreignkey, ' FOREIGN KEY (', column_name, ')', ' REFERENCES ', foreign_table_schema, '.', foreign_table_name, '(', foreign_column_name, ')' ), ','), ';') as AddQuery
    FROM
        (SELECT
        S.table_schema,
        S.foreignkey,
        S.table_name,
        STRING_AGG(DISTINCT S.column_name, ',') AS column_name,
        S.foreign_table_schema,
        S.foreign_table_name,
        STRING_AGG(DISTINCT S.foreign_column_name, ',') AS foreign_column_name
    FROM
        (SELECT DISTINCT
        tc.table_schema,
        tc.constraint_name AS foreignkey,
        tc.table_name,
        kcu.column_name,
        ccu.table_schema AS foreign_table_schema,
        ccu.table_name AS foreign_table_name,
        ccu.column_name AS foreign_column_name
        FROM information_schema.table_constraints AS tc
        JOIN information_schema.key_column_usage AS kcu ON tc.constraint_name = kcu.constraint_name AND tc.table_schema = kcu.table_schema
        JOIN information_schema.constraint_column_usage AS ccu ON ccu.constraint_name = tc.constraint_name AND ccu.table_schema = tc.table_schema
    WHERE constraint_type = 'FOREIGN KEY'
        ) S
        GROUP BY S.table_schema, S.foreignkey, S.table_name, S.foreign_table_schema, S.foreign_table_name
        ) Q
        GROUP BY Q.table_schema, Q.table_name;
    ```

5. Sorgu sonucunda bırakma yabancı anahtarını (ikinci sütun) çalıştırın.

6. Hedef veritabanındaki tetikleyicileri devre dışı bırakın, aşağıdaki komut dosyasını çalıştırın.

   > [!IMPORTANT]
   > Verideki tetikleyiciler (ekleme veya güncelleştirme), kaynaktan çoğaltılan verilerden önce hedefteki veri bütünlüğünü zorlar. Sonuç olarak, geçiş sırasında hedefteki tüm **tablolardaki** tetikleyicileri devre dışı bırakıp geçiş tamamlandıktan sonra tetikleyicileri yeniden etkinleştirmeniz önerilir.

    ```
    SELECT DISTINCT CONCAT('ALTER TABLE ', event_object_schema, '.', event_object_table, ' DISABLE TRIGGER ', trigger_name, ';')
    FROM information_schema.triggers
    ```

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-postgresql-to-azure-postgresql-online-portal/portal-select-subscriptions.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **Kaynak sağlayıcılarını**seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-postgresql-to-azure-postgresql-online-portal/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-postgresql-to-azure-postgresql-online-portal/portal-register-resource-provider.png)

## <a name="create-a-dms-instance"></a>DMS örneği oluşturma

1. Azure portalında + **Kaynak Oluştur'** seçeneğini belirleyin, Azure Veritabanı Geçiş Hizmeti'ni arayın ve ardından açılan listeden **Azure Veritabanı Geçiş Hizmeti'ni** seçin.

    ![Azure Market](media/tutorial-postgresql-to-azure-postgresql-online-portal/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-create1.png)
  
3. Geçiş **Hizmeti Oluştur** ekranında, bir ad, abonelik, yeni veya varolan kaynak grubu ve hizmetin konumunu belirtin.

4. Varolan bir sanal ağ seçin veya yeni bir ağ oluşturun.

    Sanal ağ, Kaynak PostgreSQL sunucusuna ve PostgreSQL örneği için hedef Azure Veritabanı'na erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar.

    Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/DMSVnet)bakın.

5. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-settings4.png)

6. Hizmeti oluşturmak için **Gözden Geçir + oluştur'u** seçin.

   Hizmet oluşturma yaklaşık 10 ila 15 dakika içinde tamamlanacaktır.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

      ![Azure Veritabanı Geçiş Hizmeti'nin tüm örneklerini bulma](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-search.png)

2. Azure **Veritabanı Geçiş Hizmetleri** ekranında, oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın, örneği seçin ve sonra + **Yeni Geçiş Projesi'ni**seçin.

3. Yeni **geçiş projesi** ekranında, **Kaynak sunucu türü** metin kutusunda proje için bir ad belirtin, Hedef sunucu **türü** metin kutusunda **PostgresSQL'i**seçin, **PostgreSQL için Azure Veritabanı'nı**seçin.

4. Etkinlik **türünü seç** **bölümünde, Çevrimiçi veri geçişi'ni**seçin.

    ![Azure Veritabanı Geçiş Hizmeti projesi oluşturma](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-create-project.png)

    > [!NOTE]
    > Alternatif olarak, yalnızca geçiş projesini şimdi oluşturmak ve geçişi daha sonra yürütmek için **proje oluştur'u** seçebilirsiniz.

5. **Kaydet'i**seçin, verileri geçirmek için Azure Veritabanı Geçiş Hizmeti'ni başarıyla kullanmak için gereksinimleri not edin ve ardından **Etkinlik Oluştur ve çalıştır'ı**seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. Kaynak **Ayrıntıları Ekle** ekranında, kaynak PostgreSQL örneğinin bağlantı ayrıntılarını belirtin.

    ![Kaynak Ayrıntıları Ekleyin ekranı](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-add-source-details.png)

2. **Kaydet'i**seçin.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Hedef **ayrıntılar** ekranında, **DVD Rentals** şemasının pg_dump kullanılarak dağıtıldığı Hyperscale (Citus) şemasının önceden sağlanmış örneği olan hedef Hyperscale (Citus) sunucusunun bağlantı ayrıntılarını belirtin.

    ![Hedef ayrıntıları ekranı](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-add-target-details.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanı kaynak veritabanıyla aynı veritabanı adını içeriyorsa, Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    ![Hedef veritabanları ekranı için harita](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-map-target-databases.png)

3. **Kaydet'i**seçin ve ardından **Geçiş ayarları** ekranında varsayılan değerleri kabul edin.

    ![Geçiş ayarları ekranı](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-migration-settings.png)

4. **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş özeti ekranı](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-migration-summary.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi görüntülenir ve etkinliğin **Durumu** Devam Eden **Yedekleme**olarak göstermek üzere güncelleştirilmelidir.

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Tamamlandı** olana kadar güncelleştirebilirsiniz.

     ![Geçiş işlemini izleme](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-monitor-migration.png)

2. Geçiş tamamlandığında, Veritabanı **Adı**altında, **Tam veri yükü** ve **Artımlı veri eşitleme** işlemleri için geçiş durumuna ulaşmak için belirli bir veritabanı seçin.

   > [!NOTE]
   > **Tam veri yükü** ilk yük geçiş durumunu gösterirken, **Artıl veri eşitleme** değişim veri yakalama (CDC) durumunu gösterir.

     ![Tam veri yükleme ayrıntıları](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-full-data-load-details.png)

     ![Artımlı veri eşitleme ayrıntıları](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-incremental-data-sync-details.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme

İlk Tam yük tamamlandıktan sonra, veritabanları **Geçiş için hazır** olarak işaretlenir.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

2. Kaynak veritabanına gelen tüm hareketlerin durdurulduğundan emin olmak için **Bekleyen değişiklikler** sayacı **0** gösterene kadar bekleyin, **onay** kutusunu seçin ve sonra **Uygula'yı**seçin.

    ![Komple kesme ekranı](media/tutorial-postgresql-to-azure-postgresql-online-portal/dms-complete-cutover.png)

3. Veritabanı geçiş durumu **Tamamlandı'yı**gösterdiğinde, uygulamalarınızı PostgreSQL için Azure Veritabanı'nın yeni hedef örneğine bağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* PostgreSQL için Azure Veritabanı'na yönelik çevrimiçi geçiş gerçekleştirirken karşılaşılan bilinen sorunlar ve sınırlamalar hakkında bilgi için [PostgreSQL için Azure Veritabanı geçiş işlemleri ile ilgili bilinen sorunlar ve geçici çözümler](known-issues-azure-postgresql-online.md) başlıklı makaleye bakın.
* Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
* PostgreSQL için Azure Veritabanı hakkında daha fazla bilgi için, [PostgreSQL için Azure Veritabanı nedir makalesine bakın.](https://docs.microsoft.com/azure/postgresql/overview)
