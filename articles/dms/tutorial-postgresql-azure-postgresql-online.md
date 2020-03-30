---
title: "Öğretici: PostgreSQL'i Azure CLI üzerinden çevrimiçi PostgreSQL için Azure Veritabanına geçirin"
titleSuffix: Azure Database Migration Service
description: CLI üzerinden Azure Veritabanı Geçiş Hizmeti'ni kullanarak PostgreSQL'den PostgreSQL'den PostgreSQL için Azure Veritabanı'na çevrimiçi geçiş gerçekleştirmeyi öğrenin.
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
ms.openlocfilehash: 44df35957dfbd3aa4856d256dc1a7d9e6527fde0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240664"
---
# <a name="tutorial-migrate-postgresql-to-azure-db-for-postgresql-online-using-dms-via-the-azure-cli"></a>Öğretici: PostgreSQL'i Azure CLI üzerinden DMS kullanarak PostgreSQL çevrimiçi için Azure DB'ye geçirin

Veritabanlarını şirket içi PostgreSQL örneğinden en az kapalı kalma süresiyle [PostgreSQL için Azure Veritabanı'na](https://docs.microsoft.com/azure/postgresql/) geçirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz. Başka bir deyişle, uygulama için en az kapalı kalma süresi ile geçiş elde edilebilir. Bu eğitimde, DVD **Kiralama** örnek veritabanını, Azure Veritabanı Geçiş Hizmeti'ndeki çevrimiçi geçiş etkinliğini kullanarak PostgreSQL 9.6'nın şirket içi bir örneğinden PostgreSQL için Azure Veritabanı'na geçirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> * Örnek şema pg_dump yardımcı programı kullanarak geçirin.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak geçiş projesi oluşturma.
> * Geçişi çalıştırma.
> * Geçişi izleme.

> [!NOTE]
> Çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanmak, Premium fiyatlandırma katmanını temel alan bir örnek oluşturmayı gerektirir. Geçiş işlemi sırasında veri hırsızlığını önlemek için diski şifreleriz.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, hedef veritabanıyla aynı Azure bölgesinde Azure Veritabanı Geçiş Hizmeti örneği oluşturmanızı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* [PostgreSQL topluluk sürümü](https://www.postgresql.org/download/) 9.5, 9.6 veya 10'u indirin ve yükleyin. Kaynak PostgreSQL Server sürümü 9.5.11, 9.6.7, 10 veya daha sonra olmalıdır. Daha fazla bilgi için, [makaleye](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)bkz.

    Ayrıca, şirket içi PostgreSQL sürümünün, PostgreSQL için Azure Veritabanı sürümü ile eşleşmesi gerekir. Örneğin, PostgreSQL 9.5.11.5 yalnızca PostgreSQL için Azure Veritabanı 9.5.11 sürümüne geçirilebilir ve 9.6.7 sürümüne geçirilemez.

* [PostgreSQL için Azure Veritabanı'nda bir örnek oluşturun](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal) veya [PostgreSQL - Hyperscale (Citus) sunucusu için bir Azure Veritabanı oluşturun.](https://docs.microsoft.com/azure/postgresql/quickstart-create-hyperscale-portal)
* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun. Sanal ağ oluşturma hakkında daha fazla bilgi için [Sanal Ağ Belgeleri'ne](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarıiçeren hızlı başlangıç makalelerini görün.

    > [!NOTE]
    > Sanal ağ kurulumu sırasında, Microsoft'a ağ la bakan ExpressRoute kullanıyorsanız, hizmetin sağlandığı alt ağa aşağıdaki hizmet [bitiş noktalarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) ekleyin:
    >
    > * Hedef veritabanı bitiş noktası (örneğin, SQL bitiş noktası, Cosmos DB bitiş noktası vb.)
    > * Depolama bitiş noktası
    > * Servis veri günü bitiş noktası
    >
    > Azure Veritabanı Geçiş Hizmeti internet bağlantısından yoksun olduğundan bu yapılandırma gereklidir.

* Sanal ağ Ağ Güvenlik Grubu (NSG) kurallarınızın Azure Veritabanı Geçiş Hizmeti'ne aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olun: 443, 53, 9354, 445, 12000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)makale Filtre ağ trafiği bakın.
* [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* Azure Veritabanı Geçiş Hizmeti'nin varsayılan olarak TCP bağlantı noktası 5432 olan kaynak PostgreSQL Server'a erişmesine izin vermek için Windows güvenlik duvarınızı açın.
* Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
* Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesine izin vermek için PostgreSQL için Azure Veritabanı için sunucu düzeyinde bir [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını sağlayın.
* CLI’yi çağırmak için iki yöntem vardır:

  * Azure portalının sağ üst köşesinde Bulut Kabuğu düğmesini seçin:

       ![Azure portaldaki Cloud Shell düğmesi](media/tutorial-postgresql-to-azure-postgresql-online/cloud-shell-button.png)

  * CLI’yi yerel olarak yükleyip çalıştırın. CLI 2.0, Azure kaynaklarını yönetmeye yönelik komut satırı aracıdır.

       CLI’yi indirmek için [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) makalesindeki yönergeleri izleyin. Makalede ayrıca CLI 2.0'ı destekleyen platformlar listelenir.

       Linux için Windows Alt Sistemi’ni (WSL) ayarlamak istiyorsanız [Windows 10 Yükleme Kılavuzu](https://docs.microsoft.com/windows/wsl/install-win10) içindeki yönergeleri izleyin

* postgresql.config dosyasında mantıksal çoğaltmayı etkinleştirin ve aşağıdaki parametreleri ayarlayın:

  * wal_level = **logical**
  * max_replication_slots = [yuva sayısı], **beş yuvaya** ayar öneririz
  * max_wal_senders =[eşzamanlı görev sayısı] - max_wal_senders parametresi, çalışabilecek eşzamanlı görevlerin sayısını ayarlar; önerilen ayar **10 görevdir**

## <a name="migrate-the-sample-schema"></a>Örnek şemayı geçirme

Tablo şemaları, dizinler ve saklı yordamlar gibi tüm veritabanı nesnelerini tamamlamak için kaynak veritabanındaki şemayı ayıklamamız ve veritabanına uygulamamız gerekir.

1. Bir veritabanına yönelik şema döküm dosyası oluşturmak için pg_dump -s komutunu kullanın. 

    ```
    pg_dump -o -h hostname -U db_username -d db_name -s > your_schema.sql
    ```

    Örneğin, dvdrental veritabanındaki bir şema dosyasının dökümünü çıkarmak için:
    ```
    pg_dump -o -h localhost -U postgres -d dvdrental -s  > dvdrentalSchema.sql
    ```

    pg_dump yardımcı programını kullanma hakkında daha fazla bilgi için [pg-dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html#PG-DUMP-EXAMPLES) öğreticisindeki örneklere bakın.

2. Hedef ortamınız olan PostgreSQL için Azure Veritabanı içinde boş bir veritabanı oluşturun.

    Veritabanına nasıl bağlanıp oluşturulacaklarına ilişkin ayrıntılar için, [Azure portalında PostgreSQL sunucusu için Bir Azure Veritabanı Oluştur](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal) veya Azure [portalında PostgreSQL - Hyperscale (Citus) sunucusu için bir Azure Veritabanı oluşturun makalesine](https://docs.microsoft.com/azure/postgresql/quickstart-create-hyperscale-portal)bakın.

3. Şema döküm dosyasını geri yükleyerek, şemayı oluşturduğunuz hedef veritabanına aktarın.

    ```
    psql -h hostname -U db_username -d db_name < your_schema.sql 
    ```

    Örnek:

    ```
    psql -h mypgserver-20170401.postgres.database.azure.com  -U postgres -d dvdrental < dvdrentalSchema.sql
    ```

4. Şemanızda yabancı anahtarlar varsa geçişe ilişkin ilk yük ve sürekli eşitleme başarısız olur. Hedefte (PostgreSQL için Azure Veritabanı) bırakma yabancı anahtar betiği ve ekleme yabancı anahtar betiğini ayıklamak için PgAdmin veya psql içinde aşağıdaki betiği yürütün.
  
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

    Sorgu sonucunda bırakma yabancı anahtarını (ikinci sütun) çalıştırın.

5. Verilerdeki tetikleyiciler (ekleme veya güncelleştirme tetikleyicileri) kaynaktan çoğaltılan veriler ile hedef arasında veri bütünlüğü sağlar. Geçiş sırasında hedefteki tüm **tablolardaki** tetikleyicileri devre dışı bırakıp geçiş tamamlandıktan sonra tetikleyicileri yeniden etkinleştirmeniz önerilir.

    Hedef veritabanında tetikleyicileri devre dışı bırakmak için şu komutu kullanın:

    ```
    select concat ('alter table ', event_object_table, ' disable trigger ', trigger_name)
    from information_schema.triggers;
    ```

6. Herhangi bir tabloda ENUM veri türü varsa, bunu geçici olarak hedef tablodaki 'karakter değişen' veri türüne güncelleştirmeniz önerilir. Veri çoğaltma tamamlandıktan sonra veri türünü yeniden ENUM yapın.

## <a name="provisioning-an-instance-of-dms-using-the-cli"></a>CLI kullanarak DMS örneği sağlama

1. Dms eşitleme uzantısını yükleyin:
   * Aşağıdaki komutu çalıştırarak Azure'da oturum açın:
       ```azurecli
       az login
       ```

   * Sorulduğunda bir web tarayıcısı açın ve cihazınızın kimliğini doğrulamak için bir kod girin. Listelenen yönergeleri uygulayın.
   * Dms uzantısını ekleyin:
       * Kullanılabilir uzantıları listelemek için aşağıdaki komutu çalıştırın:

           ```azurecli
           az extension list-available –otable
           ```

       * Uzantıyı yüklemek için aşağıdaki komutu çalıştırın:

           ```azurecli
           az extension add –n dms-preview
           ```

   * Dms uzantısının doğru yüklendiğini onaylamak için aşağıdaki komutu çalıştırın:

       ```azurecli
       az extension list -otable
       ```
       Aşağıdaki çıktıyı görmeniz gerekir:

       ```output
       ExtensionType    Name
       ---------------  ------
       whl              dms
       ```

      > [!IMPORTANT]
      > Uzantı lı sürümünüzün 0.11.0'ın üzerinde olduğundan emin olun.

   * Dilediğiniz zaman aşağıdaki komutu çalıştırarak desteklenen tüm komutları görüntüleyin:

       ```azurecli
       az dms -h
       ```

   * Birden çok Azure aboneliğiniz varsa, DMS hizmetinin bir örneğini sağlamak için kullanmak istediğiniz aboneliği ayarlamak üzere aşağıdaki komutu çalıştırın.

        ```azurecli
       az account set -s 97181df2-909d-420b-ab93-1bff15acb6b7
        ```

2. Aşağıdaki komutu çalıştırarak bir DMS örneğini sağlayın:

   ```azurecli
   az dms create -l [location] -n <newServiceName> -g <yourResourceGroupName> --sku-name Premium_4vCores --subnet/subscriptions/{vnet subscription id}/resourceGroups/{vnet resource group}/providers/Microsoft.Network/virtualNetworks/{vnet name}/subnets/{subnet name} –tags tagName1=tagValue1 tagWithNoValue
   ```

   Örneğin, aşağıdaki komut şurada bir hizmet oluşturur:

   * Konum: Doğu ABD2
   * Abonelik: 97181df2-909d-420b-ab93-1bff15acb6b7
   * Kaynak Grubu Adı: PostgresDemo
   * DMS Hizmet Adı: PostgresCLI

   ```azurecli
   az dms create -l eastus2 -g PostgresDemo -n PostgresCLI --subnet /subscriptions/97181df2-909d-420b-ab93-1bff15acb6b7/resourceGroups/ERNetwork/providers/Microsoft.Network/virtualNetworks/AzureDMS-CORP-USC-VNET-5044/subnets/Subnet-1 --sku-name Premium_4vCores
   ```

   DMS hizmeti örneğini oluşturmak yaklaşık 10-12 dakika sürer.

3. Postgres pg_hba.conf dosyasına eklemek üzere DMS aracısının IP adresini tanımlamak için aşağıdaki komutu çalıştırın:

    ```azurecli
    az network nic list -g <ResourceGroupName>--query '[].ipConfigurations | [].privateIpAddress'
    ```

    Örnek:

    ```azurecli
    az network nic list -g PostgresDemo --query '[].ipConfigurations | [].privateIpAddress'
    ```

    Aşağıdaki adrese benzer bir sonuç almalısınız: 

    ```output
    [
      "172.16.136.18"
    ]
    ```

4. DMS aracısının IP adresini Postgres pg_hba.conf dosyasına ekleyin.

    * DMS içinde sağlamayı bitirdikten sonra DMS IP adresini not alın.
    * IP adresini aşağıdaki girişe benzer şekilde kaynaktaki pg_hba.conf dosyasına ekleyin:

        ```
        host    all     all     172.16.136.18/10    md5
        host    replication     postgres    172.16.136.18/10    md5
        ```

5. Ardından, aşağıdaki komutu çalıştırarak bir PostgreSQL geçiş projesi oluşturun:
    
    ```azurecli
    az dms project create -l <location> -g <ResourceGroupName> --service-name <yourServiceName> --source-platform PostgreSQL --target-platform AzureDbforPostgreSQL -n <newProjectName>
    ```

    Örneğin, aşağıdaki komut bu parametreleri kullanarak bir proje oluşturur:

   * Konum: Orta Batı ABD
   * Kaynak Grubu Adı: PostgresDemo
   * Hizmet Adı: PostgresCLI
   * Proje adı: PGMigration
   * Kaynak platform: PostgreSQL
   * Hedef platform: AzureDbForPostgreSql

     ```azurecli
     az dms project create -l westcentralus -n PGMigration -g PostgresDemo --service-name PostgresCLI --source-platform PostgreSQL --target-platform AzureDbForPostgreSql
     ```

6. Aşağıdaki adımları kullanarak bir PostgreSQL geçiş görevi oluşturun.

    Bu adım, bağlantı kurmak için kaynak IP, UserID ve parola, hedef IP, UserID, parola ve görev türünü kullanmayı içerir.

   * Seçeneklerin tam listesini görmek için komutu çalıştırın:

       ```azurecli
       az dms project task create -h
       ```

       Hem kaynak hem de hedef bağlantı için giriş parametresi, nesne listesini içeren bir json dosyasına başvurur.

       PostgreSQL bağlantıları için bağlantı JSON nesnesinin biçimi.
        
       ```json
       {
                   "userName": "user name",    // if this is missing or null, you will be prompted
                   "password": null,           // if this is missing or null (highly recommended) you will
               be prompted
                   "serverName": "server name",
                   "databaseName": "database name", // if this is missing, it will default to the 'postgres'
               server
                   "port": 5432                // if this is missing, it will default to 5432
               }
       ```

   * Json nesnelerini listeleyen bir veritabanı seçeneği json dosyası da vardır. PostgreSQL için veri tabanı seçeneği JSON nesnesinin biçimi aşağıda gösterilmiştir:

       ```json
       [
           {
               "name": "source database",
               "target_database_name": "target database",
           },
           ...n
       ]
       ```

   * Not Defteri ile bir json dosyası oluşturun, aşağıdaki komutları kopyalayın ve dosyanın içine yapıştırın, ardından dosyayı C:\DMS\source.json yoluna kaydedin.

        ```json
       {
                   "userName": "postgres",    
                   "password": null,           
               be prompted
                   "serverName": "13.51.14.222",
                   "databaseName": "dvdrental", 
                   "port": 5432                
               }
        ```

   * Target.json adlı başka bir dosya oluşturun ve C:\DMS\target.json olarak kaydedin. Aşağıdaki komutları ekleyin:

       ```json
       {
               "userName": " dms@builddemotarget",    
               "password": null,           
               "serverName": " builddemotarget.postgres.database.azure.com",
               "databaseName": "inventory", 
               "port": 5432                
           }
       ```

   * Geçirilecek veritabanı olarak stok listeleyen bir veritabanı seçeneği json dosyası oluşturun:

       ```json
       [
           {
               "name": "dvdrental",
               "target_database_name": "dvdrental",
           }
       ]
       ```

   * Kaynak, hedef ve DB seçeneği json dosyalarını alan aşağıdaki komutu çalıştırın.

       ```azurecli
       az dms project task create -g PostgresDemo --project-name PGMigration --source-platform postgresql --target-platform azuredbforpostgresql --source-connection-json c:\DMS\source.json --database-options-json C:\DMS\option.json --service-name PostgresCLI --target-connection-json c:\DMS\target.json –task-type OnlineMigration -n runnowtask    
       ```

     Bu noktada, bir geçiş görevini başarıyla gönderdiniz.

7. Görevin ilerleme durumunu göstermek için aşağıdaki komutu çalıştırın:

   ```azurecli
   az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask
   ```

   OR

    ```azurecli
   az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask --expand output
    ```

8. Genişletme çıktısından migrationState sorgusu da yapabilirsiniz:

    ```azurecli
    az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask --expand output --query 'properties.output[].migrationState | [0]' "READY_TO_COMPLETE"
    ```

## <a name="understanding-migration-task-status"></a>Geçiş görevinin durumunu anlama

Çıktı dosyasında geçişin ilerleme durumunu gösteren birkaç parametre vardır. Örneğin, aşağıdaki çıktı dosyasına bakın:

  ```output
    "output": [                                 Database Level
          {
            "appliedChanges": 0,        //Total incremental sync applied after full load
            "cdcDeleteCounter": 0       // Total delete operation  applied after full load
            "cdcInsertCounter": 0,      // Total insert operation applied after full load
            "cdcUpdateCounter": 0,      // Total update operation applied after full load
            "databaseName": "inventory",
            "endedOn": null,
            "fullLoadCompletedTables": 2,   //Number of tables completed full load
            "fullLoadErroredTables": 0, //Number of tables that contain migration error
            "fullLoadLoadingTables": 0, //Number of tables that are in loading status
            "fullLoadQueuedTables": 0,  //Number of tables that are in queued status
            "id": "db|inventory",
            "incomingChanges": 0,       //Number of changes after full load
            "initializationCompleted": true,
            "latency": 0,
            "migrationState": "READY_TO_COMPLETE",  //Status of migration task. READY_TO_COMPLETE means the database is ready for cutover
            "resultType": "DatabaseLevelOutput",
            "startedOn": "2018-07-05T23:36:02.27839+00:00"
          },
          {
            "databaseCount": 1,
            "endedOn": null,
            "id": "dd27aa3a-ed71-4bff-ab34-77db4261101c",
            "resultType": "MigrationLevelOutput",
            "sourceServer": "138.91.123.10",
            "sourceVersion": "PostgreSQL",
            "startedOn": "2018-07-05T23:36:02.27839+00:00",
            "state": "PENDING",
            "targetServer": "builddemotarget.postgres.database.azure.com",
            "targetVersion": "Azure Database for PostgreSQL"
          },
          {                                     Table 1
            "cdcDeleteCounter": 0,
            "cdcInsertCounter": 0,
            "cdcUpdateCounter": 0,
            "dataErrorsCount": 0,
            "databaseName": "inventory",
            "fullLoadEndedOn": "2018-07-05T23:36:20.740701+00:00",  //Full load completed time
            "fullLoadEstFinishTime": "1970-01-01T00:00:00+00:00",
            "fullLoadStartedOn": "2018-07-05T23:36:15.864552+00:00",    //Full load started time
            "fullLoadTotalRows": 10,                    //Number of rows loaded in full load
            "fullLoadTotalVolumeBytes": 7056,               //Volume in Bytes in full load
            "id": "or|inventory|public|actor",          
            "lastModifiedTime": "2018-07-05T23:36:16.880174+00:00",
            "resultType": "TableLevelOutput",
            "state": "COMPLETED",                   //State of migration for this table
            "tableName": "public.catalog",              //Table name
            "totalChangesApplied": 0                //Total sync changes that applied after full load
          },
          {                                     Table 2
            "cdcDeleteCounter": 0,
            "cdcInsertCounter": 50,
            "cdcUpdateCounter": 0,
            "dataErrorsCount": 0,
            "databaseName": "inventory",
            "fullLoadEndedOn": "2018-07-05T23:36:23.963138+00:00",
            "fullLoadEstFinishTime": "1970-01-01T00:00:00+00:00",
            "fullLoadStartedOn": "2018-07-05T23:36:19.302013+00:00",
            "fullLoadTotalRows": 112,
            "fullLoadTotalVolumeBytes": 46592,
            "id": "or|inventory|public|address",
            "lastModifiedTime": "2018-07-05T23:36:20.308646+00:00",
            "resultType": "TableLevelOutput",
            "state": "COMPLETED",
            "tableName": "public.orders",
            "totalChangesApplied": 0
          }
        ],                          DMS migration task state
        "state": "Running", //Migration task state – Running means it is still listening to any changes that might come in                  
        "taskType": null
      },
      "resourceGroup": "PostgresDemo",
      "type": "Microsoft.DataMigration/services/projects/tasks"
  ```

## <a name="cutover-migration-task"></a>Tam geçiş görevi

Tam yükleme tamamlandığında veritabanı tam geçiş için hazırdır. Kaynak sunucunun gelen yeni işlemlerle ne kadar meşgul olduğuna bağlı olarak, tam yükleme tamamlandıktan sonra DMS görevi hala değişiklikleri uyguluyor olabilir.

Tüm verilerin yakalandığından emin olmak için kaynak ve hedef veritabanları arasındaki satır sayılarını doğrulayın. Örneğin, aşağıdaki komutu kullanabilirsiniz:

```
"migrationState": "READY_TO_COMPLETE", //Status of migration task. READY_TO_COMPLETE means database is ready for cutover
 "incomingChanges": 0,  //continue to check for a period of 5-10 minutes to make sure no new incoming changes that need to be applied to the target server
   "fullLoadTotalRows": 10, //full load for table 1
    "cdcDeleteCounter": 0,  //delete, insert and update counter on incremental sync after full load
    "cdcInsertCounter": 50,
    "cdcUpdateCounter": 0,
     "fullLoadTotalRows": 112,  //full load for table 2
```

1. Aşağıdaki komutu kullanarak tam veritabanı geçiş görevini gerçekleştirin:

    ```azurecli
    az dms project task cutover -h
    ```

    Örnek:

    ```azurecli
    az dms project task cutover --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask  --object-name Inventory
    ```

2. Tam geçişin ilerleme durumunu izlemek için aşağıdaki komutu çalıştırın:

    ```azurecli
    az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask
    ```

## <a name="service-project-task-cleanup"></a>Hizmet, proje, görev temizleme

Herhangi bir DMS görevini, projesini veya hizmetini iptal etmeniz ya da silmeniz gerekirse iptal işlemini aşağıdaki sırayla gerçekleştirin:

* Çalışan tüm görevleri iptal edin
* Görevi silin
* Projeyi silin
* DMS hizmetini silin

1. Çalışan bir görevi iptal etmek için aşağıdaki komutu kullanın:

    ```azurecli
    az dms project task cancel --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask
     ```

2. Çalışan bir görevi silmek için aşağıdaki komutu kullanın:
    ```azurecli
    az dms project task delete --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask
    ```

3. Çalışan bir projeyi iptal etmek için aşağıdaki komutu kullanın:
     ```azurecli
    az dms project task cancel -n runnowtask --project-name PGMigration -g PostgresDemo --service-name PostgresCLI
     ```

4. Çalışan bir projeyi silmek için aşağıdaki komutu kullanın:
    ```azurecli
    az dms project task delete -n runnowtask --project-name PGMigration -g PostgresDemo --service-name PostgresCLI
    ```

5. DMS hizmetini silmek için aşağıdaki komutu kullanın:

     ```azurecli
    az dms delete -g ProgresDemo -n PostgresCLI
     ```

## <a name="next-steps"></a>Sonraki adımlar

* PostgreSQL için Azure Veritabanı'na yönelik çevrimiçi geçiş gerçekleştirirken karşılaşılan bilinen sorunlar ve sınırlamalar hakkında bilgi için [PostgreSQL için Azure Veritabanı geçiş işlemleri ile ilgili bilinen sorunlar ve geçici çözümler](known-issues-azure-postgresql-online.md) başlıklı makaleye bakın.
* Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
* PostgreSQL için Azure Veritabanı hakkında daha fazla bilgi için, [PostgreSQL için Azure Veritabanı nedir makalesine bakın.](https://docs.microsoft.com/azure/postgresql/overview)
