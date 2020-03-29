---
title: "Öğretici: PostgreSQL için Oracle'ı Azure Veritabanı'na çevrimiçi geçirin"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak Oracle'dan şirket içinde veya sanal makinelerden PostgreSQL için Azure Veritabanı'na çevrimiçi geçiş gerçekleştirmeyi öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/24/2020
ms.openlocfilehash: 956523e2b51795a4bc97c653dab8b408b06061f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255576"
---
# <a name="tutorial-migrate-oracle-to-azure-database-for-postgresql-online-using-dms-preview"></a>Öğretici: Oracle'ı DMS kullanarak çevrimiçi PostgreSQL için Azure Veritabanına geçirin (Önizleme)

Veritabanlarını şirket içinde barındırılan Oracle veritabanlarından veya sanal makinelerden en az kapalı kalma süresiyle [PostgreSQL için Azure Veritabanı'na](https://docs.microsoft.com/azure/postgresql/) geçirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz. Başka bir deyişle, uygulama için en az kapalı kalma süresi ile geçiş tamamlayabilirsiniz. Bu eğitimde, Azure Veritabanı Geçiş Hizmeti'ndeki çevrimiçi geçiş etkinliğini kullanarak **İk** örnek veritabanını Oracle 11g'nin şirket içi veya sanal makine örneğinden PostgreSQL için Azure Veritabanı'na geçirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> * Ora2pg aracını kullanarak geçiş çabasını değerlendirin.
> * Ora2pg aracını kullanarak örnek şema geçirin.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturun.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.
> * Geçişi izleme.

> [!NOTE]
> Çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanmak, Premium fiyatlandırma katmanını temel alan bir örnek oluşturmayı gerektirir.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, hedef veritabanıyla aynı Azure bölgesinde Azure Veritabanı Geçiş Hizmeti örneği oluşturmanızı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, PostgreSQL için Oracle'dan Azure Veritabanı'na çevrimiçi geçişin nasıl gerçekleştirilebildiğini açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* [Oracle 11g Release 2 (Standart Sürüm, Standart Sürüm Bir veya Enterprise Edition)](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html)indirin ve yükleyin.
* Örnek **İk** veritabanını [buradan](https://docs.oracle.com/database/121/COMSC/installation.htm#COMSC00002)indirin.
* Windows veya [Linux'ta ora2pg'yi indirin ve kurutun.](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Steps%20to%20Install%20ora2pg%20on%20Windows%20and%20Linux.pdf)
* [PostgreSQL için Azure Veritabanı’nda örnek oluşturma](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal).
* Örne bağlanın ve bu [belgedeki](https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-portal)yönergeyi kullanarak bir veritabanı oluşturun.
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
* Azure Veritabanı Geçiş Hizmeti'nin varsayılan olarak TCP bağlantı noktası 1521 olan kaynak Oracle sunucusuna erişmesine izin vermek için Windows güvenlik duvarınızı açın.
* Kaynak veritabanınızın (ler) önünde bir güvenlik duvarı cihazı kullanırken, Azure Veritabanı Geçiş Hizmeti'nin geçiş için kaynak veritabanına(ler) erişmesine izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.
* Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesine izin vermek için PostgreSQL için Azure Veritabanı için sunucu düzeyinde bir [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını sağlayın.
* Kaynak Oracle veritabanlarına erişimi etkinleştirin.

  > [!NOTE]
  > Bir kullanıcının Oracle kaynağına bağlanması için DBA rolü gereklidir.

  * Veri değişikliğini yakalamak için Azure Veritabanı Geçiş Hizmeti'nde artımlı eşitleme için Arşiv Redo Günlükleri gereklidir. Oracle kaynağını yapılandırmak için aşağıdaki adımları izleyin:
    * Aşağıdaki komutu çalıştırarak SYSDBA ayrıcalığını kullanarak oturum açın:

      ```
      sqlplus (user)/(password) as sysdba
      ```

    * Aşağıdaki komutu çalıştırarak veritabanı örneğini kapatın.

      ```
      SHUTDOWN IMMEDIATE;
      ```

      Onayı `'ORACLE instance shut down'`bekleyin.

    * Aşağıdaki komutu çalıştıran arşivleme bu'yu etkinleştirmek veya devre dışı kalarak veritabanını başlatın ve veritabanını monte edin (ancak açmayın)

      ```
      STARTUP MOUNT;
      ```

      Veritabanı monte edilmelidir; onay için bekleyin 'Oracle örneği başladı'.

    * Aşağıdaki komutu çalıştırarak veritabanı arşivleme modunu değiştirin:

      ```
      ALTER DATABASE ARCHIVELOG;
      ```

    * Aşağıdaki komutu çalıştırarak veritabanını normal işlemler için açın:

      ```
      ALTER DATABASE OPEN;
      ```

      ARC dosyasının ortaya çıkarolması için yeniden başlatmanız gerekebilir.

    * Doğrulamak için aşağıdaki komutu çalıştırın:

      ```
      SELECT log_mode FROM v$database;
      ```

      Bir yanıt `'ARCHIVELOG'`almalısınız. Yanıt `'NOARCHIVELOG'`ise, gereksinim karşılanmaz.

  * Aşağıdaki seçeneklerden birini kullanarak çoğaltma için tamamlayıcı günlüğe kaydetmeyi etkinleştirin.

    * **Seçenek 1**.
      PK ve benzersiz dizin ile tüm tabloları kapsayacak şekilde veritabanı düzeyi tamamlayıcı günlüğe kaydetmeyi değiştirin. Algılama sorgusu döndürecek. `'IMPLICIT'`

      ```
      ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY, UNIQUE) COLUMNS;
      ```

      Tablo düzeyi tamamlayıcı günlüğe kaydetmeyi değiştirin. Yalnızca veri işleme si fata'sı veya benzersiz dizinleri olmayan tablolar için çalıştırın.

      ```
      ALTER TABLE [TABLENAME] ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;
      ```

    * **Seçenek 2**.
      Tüm tabloları kapsayacak şekilde veritabanı düzeyi tamamlayıcı günlüğe kaydetmeyi değiştirin ve algılama sorgusu döndürür. `'YES'`

      ```
      ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
      ```

      Tablo düzeyi tamamlayıcı günlüğe kaydetmeyi değiştirin. Her tablo için yalnızca bir deyim çalıştırmak için aşağıdaki mantığı izleyin.

      Tabloda birincil anahtar varsa:

      ```
      ALTER TABLE xxx ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY) COLUMNS;
      ```

      Tabloda benzersiz bir dizin varsa:

      ```
      ALTER TABLE xxx ADD SUPPLEMENTAL LOG GROUP (first unique index columns) ALWAYS;
      ```

      Aksi takdirde, aşağıdaki komutu çalıştırın:

      ```
      ALTER TABLE xxx ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;
      ```

    Doğrulamak için aşağıdaki komutu çalıştırın:

      ```
      SELECT supplemental_log_data_min FROM v$database;
      ```

    Bir yanıt `'YES'`almalısınız.

## <a name="assess-the-effort-for-an-oracle-to-azure-database-for-postgresql-migration"></a>PostgreSQL geçişi için Bir Oracle to Azure Veritabanı için çabayı değerlendirin

PostgreSQL için Oracle'dan Azure Veritabanı'na geçiş için gereken çabayı değerlendirmek için ora2pg kullanmanızı öneririz. Tüm `ora2pg -t SHOW_REPORT` Oracle nesnelerini, tahmini geçiş maliyetini (geliştirici günlerinde) ve dönüşümün bir parçası olarak özel ilgi gerektirebilecek belirli veritabanı nesnelerini listeleyen bir rapor oluşturmak için yönergeyi kullanın.

Çoğu müşteri, değerlendirme raporunu gözden geçirmek ve otomatik ve manuel dönüştürme çabasını göz önünde bulundurarak önemli miktarda zaman harcar.

Bir değerlendirme raporu oluşturmak için ora2pg'yi yapılandırmak ve çalıştırmak [için, PostgreSQL Yemek Kitabı için Oracle'ın Azure Veritabanı'na](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20PostgreSQL%20Migration%20Cookbook.pdf) **Ön Geçiş: Değerlendirme** bölümüne bakın. Bir örnek ora2pg değerlendirme raporu [burada](https://ora2pg.darold.net/report.html)referans için kullanılabilir.

## <a name="export-the-oracle-schema"></a>Oracle şeasını dışa aktarma

Oracle şemasını ve diğer Oracle nesnelerini (türler, yordamlar, işlevler, vb.) PostgreSQL için Azure Veritabanı ile uyumlu bir şemaya dönüştürmek için ora2pg kullanmanızı öneririz. ora2pg, belirli veri türlerini önceden tanımlamanıza yardımcı olacak birçok yönerge içerir. Örneğin, yönergeyi, `DATA_TYPE` tüm NUMBER(*,0)'ı NUMERIC yerine bigint ile değiştirmek için kullanabilirsiniz(38).

.sql dosyalarındaki veritabanı nesnelerinin her birini dışa aktarmak için ora2pg çalıştırabilirsiniz. Daha sonra .sql dosyalarını PostgreSQL için Azure Veritabanı'na aktarmadan önce psql kullanarak gözden geçirebilirsiniz veya . PGAdmin'de SQL komut dosyası.

```
psql -f [FILENAME] -h [AzurePostgreConnection] -p 5432 -U [AzurePostgreUser] -d database 
```

Örnek:

```
psql -f %namespace%\schema\sequences\sequence.sql -h server1-server.postgres.database.azure.com -p 5432 -U username@server1-server -d database
```

Şema dönüştürme için ora2pg'yi yapılandırmak ve çalıştırmak [için, PostgreSQL Yemek Kitabı için Oracle'ın Azure Veritabanına](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20PostgreSQL%20Migration%20Cookbook.pdf) **Geçiş: Şema ve veri** bölümüne bakın.

## <a name="set-up-the-schema-in-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı'nda şema ayarlama

Azure Veritabanı Geçiş Hizmeti'nde geçiş ardışık bir geçiş ardışık hattı başlatmadan önce ora2pg kullanarak Bunları Postgres uyumlu hale getirmek için Oracle tablo şemalarını, depolanmış yordamları, paketleri ve diğer veritabanı nesnelerini dönüştürmeyi seçebilirsiniz. Ora2pg ile nasıl çalışabilirsiniz için aşağıdaki bağlantılara bakın:

* [Windows'da ora2pg yükleme](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Steps%20to%20Install%20ora2pg%20on%20Windows%20and%20Linux.pdf)
* [Oracle'dan Azure PostgreSQL Geçiş Yemek Kitabı'na](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20PostgreSQL%20Migration%20Cookbook.pdf)

Azure Veritabanı Geçiş Hizmeti, PostgreSQL tablo şeasını da oluşturabilir. Hizmet, bağlı Oracle kaynağındaki tablo şemasına erişir ve PostgreSQL için Azure Veritabanı'nda uyumlu bir tablo şeması oluşturur. Azure Veritabanı Geçiş Hizmeti şemayı oluşturmayı ve verileri taşımayı bitirdikten sonra PostgreSQL için Azure Veritabanı'ndaki şema biçimini doğruladığınızdan ve denetlediğinden emin olun.

> [!IMPORTANT]
> Azure Veritabanı Geçiş Hizmeti yalnızca tablo şema oluşturur; depolanan yordamlar, paketler, dizinler, vb. gibi diğer veritabanı nesneleri oluşturulmaz.

Ayrıca, tam yükün çalışması için hedef veritabanındaki yabancı anahtarı da düşürün. Yabancı anahtarı düşürmek [için](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online) kullanabileceğiniz bir komut dosyası için **makalenin örnek şema** bölümüne bakın. Tam yükleme ve eşitleme için çalıştırmak için Azure Veritabanı Geçiş Hizmeti'ni kullanın.

### <a name="when-the-postgresql-table-schema-already-exists"></a>PostgreSQL tablo şeması zaten var olduğunda

Azure Veritabanı Geçiş Hizmeti ile veri hareketini başlatmadan önce ora2pg gibi araçları kullanarak bir PostgreSQL şeması oluşturursanız, kaynak tabloları Azure Veritabanı Geçiş Hizmeti'ndeki hedef tablolarla eşleyin.

1. PostgreSQL geçiş projesi için Azure Veritabanı'na yeni bir Oracle oluşturduğunuzda, şemazları seç adımda hedef veritabanını seçmeniz ve şema hedeflemeniz istenir. Hedef veritabanını doldurun ve şema hedefleyin.

   ![Portal aboneliklerini gösterme](media/tutorial-oracle-azure-postgresql-online/dms-map-to-target-databases.png)

2. **Geçiş ayarları** ekranı Oracle kaynağındaki tabloların bir listesini sunar. Azure Veritabanı Geçiş Hizmeti, tablo adını temel alarak kaynaktaki ve hedef tablolardaki tabloları eşleştirmeye çalışır. Farklı kasaya sahip birden çok eşleşen hedef tablo varsa, hangi hedef tabloyla eşleneceklerini seçebilirsiniz.

    ![Portal aboneliklerini gösterme](media/tutorial-oracle-azure-postgresql-online/dms-migration-settings.png)

> [!NOTE]
> Kaynak tablo adlarını farklı adlara, e-postaya [dmsfeedback@microsoft.com](mailto:dmsfeedbac@microsoft.com) sahip tablolarla eşlemeniz gerekiyorsa, işlemi otomatikleştirmek için bir komut dosyası sağlayabiliriz.

### <a name="when-the-postgresql-table-schema-doesnt-exist"></a>PostgreSQL tablo şeması olmadığında

Hedef PostgreSQL veritabanı tablo şeması bilgisi içermiyorsa, Azure Veritabanı Geçiş Hizmeti kaynak şeasını dönüştürür ve hedef veritabanında yeniden oluşturur. Azure Veritabanı Geçiş Hizmeti'nin depolanmış yordamlar, paketler ve dizinler gibi diğer veritabanı nesnelerini değil, yalnızca tablo şemasını oluşturduğunu unutmayın.
Azure Veritabanı Geçiş Hizmeti'nin sizin için şemayı oluşturması için, hedef ortamınızın varolan tabloları olmayan bir şema içerdiğinden emin olun. Azure Veritabanı Geçiş Hizmeti herhangi bir tablo keşfederse, hizmet şeamın ora2pg gibi harici bir araç tarafından oluşturulduğunu varsayar.

> [!IMPORTANT]
> Azure Veritabanı Geçiş Hizmeti, Azure Veritabanı Geçiş Hizmeti veya ora2pg gibi bir aracı kullanarak tüm tabloların aynı şekilde oluşturulmasını gerektirir, ancak her ikisini birden oluşturmaz.

Başlamak için:

1. Uygulama gereksinimlerinize göre hedef veritabanında bir şema oluşturun. Varsayılan olarak, PostgreSQL tablo şeması ve sütun adları küçük harfle hesaplanır. Diğer taraftan, Oracle tablo şeması ve sütunları varsayılan olarak tüm büyük harflerle olur.
2. Şema seç adımında hedef veritabanını ve hedef şema'yı belirtin.
3. PostgreSQL için Azure Veritabanı'nda oluşturduğunuz şema temel alınerek, Azure Veritabanı Geçiş Hizmeti aşağıdaki dönüşüm kurallarını kullanır:

    Oracle kaynağındaki şema adı ve PostgreSQL için Azure Veritabanı'ndakiyle eşleşiyorsa, Azure Veritabanı Geçiş Hizmeti *hedeftekiyle aynı örneği kullanarak tablo şeasını oluşturur.*

    Örnek:

    | Kaynak Oracle şema | Hedef PostgreSQL Database.Schema | DMS schema.table.column oluşturdu |
    | ------------- | ------------- | ------------- |
    | HR | targetHR.public | public.countries.country_id |
    | HR | targetHR.trgthr | trgthr.countries.country_id |
    | HR | targetHR.TARGETHR | "TARGETHR". ÜLKELER". COUNTRY_ID" |
    | HR | targetHR.HR | "İnsan KAYNAKLARı"." ÜLKELER". COUNTRY_ID" |
    | HR | targetHR.Hr | *Karışık servis taleplerini haritalayamadı |

    *Hedef PostgreSQL'de karışık durum şeması ve tablo [dmsfeedback@microsoft.com](mailto:dmsfeedback@microsoft.com)adları oluşturmak için iletişim kurun. Hedef PostgreSQL veritabanında karışık durum tablosu şeması kurmak için bir komut dosyası sağlayabiliriz.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-oracle-azure-postgresql-online/portal-select-subscriptions.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-oracle-azure-postgresql-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-oracle-azure-postgresql-online/portal-register-resource-provider.png)

## <a name="create-a-dms-instance"></a>DMS örneği oluşturma

1. Azure portalında + **Kaynak Oluştur'** seçeneğini belirleyin, Azure Veritabanı Geçiş Hizmeti'ni arayın ve ardından açılan listeden **Azure Veritabanı Geçiş Hizmeti'ni** seçin.

    ![Azure Market](media/tutorial-oracle-azure-postgresql-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-oracle-azure-postgresql-online/dms-create2.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Varolan bir sanal ağ seçin veya yeni bir ağ oluşturun.

    Sanal ağ, PostgreSQL örneği için kaynak Oracle'a ve hedef Azure Veritabanı'na erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar.

    Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/DMSVnet)bakın.

5. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-oracle-azure-postgresql-online/dms-settings5.png)

6. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-oracle-azure-postgresql-online/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve sonuçlardan bu örneği seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğinizi bulma](media/tutorial-oracle-azure-postgresql-online/dms-instance-search.png)

3. +**Yeni Geçiş Projesi**'ni seçin.
4. Yeni **geçiş projesi** ekranında, **Kaynak sunucu türü** metin kutusunda proje için bir ad belirtin, Hedef sunucu **türü** metin kutusunda **Oracle'ı**seçin, **PostgreSQL için Azure Veritabanı'nı**seçin.
5. Etkinlik **türünü seç** **bölümünde, Çevrimiçi veri geçişi'ni**seçin.

   ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-oracle-azure-postgresql-online/dms-create-project5.png)

   > [!NOTE]
   > Alternatif olarak, yalnızca geçiş projesini şimdi oluşturmak ve geçişi daha sonra yürütmek için **proje oluştur'u** seçebilirsiniz.

6. **Kaydet'i**seçin, çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni başarıyla kullanmak için gereksinimleri not edin ve ardından **Etkinlik Oluştur ve çalıştır'ı**seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

* Kaynak **Ayrıntıları Ekle** ekranında, kaynak Oracle örneğinin bağlantı ayrıntılarını belirtin.

  ![Kaynak Ayrıntıları Ekleyin ekranı](media/tutorial-oracle-azure-postgresql-online/dms-add-source-details.png)

## <a name="upload-oracle-oci-driver"></a>Oracle OCI sürücüsünü yükleyin

1. **Kaydet'i**seçin ve sonra **OCI sürücü** ekranında Oracle hesabınızda oturum açın ve sürücü **anında istemci-basiclite-windows.x64-12.2.0.1.0.zip** (37,128.586 Byte(s)) (SHA1 Checksum: 865082268) [buradan](https://www.oracle.com/technetwork/topics/winx64soft-089540.html#ic_winx64_inst)indirin .
2. Sürücüyü paylaşılan bir klasöre indirin.

   Klasörün yalnızca salt okunur okunur erişimle belirttiğiniz kullanıcı adı ile paylaşıldığınızdan emin olun. Azure Veritabanı Geçiş Hizmeti, belirttiğiniz kullanıcı adını taklit ederek OCI sürücüsünü Azure'a yüklemek için paylaşıma erişir ve okur.

   Belirttiğiniz kullanıcı adı bir Windows kullanıcı hesabı olmalıdır.

   ![OCI Sürücü Yükleme](media/tutorial-oracle-azure-postgresql-online/dms-oci-driver-install.png)

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. **Kaydet'i**ve ardından **Hedef ayrıntıları** ekranında, **HR** şemasının dağıtıldığı PostgreSQL için Azure Veritabanı'nın önceden sağlanan örneği olan PostgreSQL sunucusu için hedef Azure Veritabanı'nın bağlantı ayrıntılarını belirtin.

    ![Hedef ayrıntıları ekranı](media/tutorial-oracle-azure-postgresql-online/dms-add-target-details1.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanı kaynak veritabanıyla aynı veritabanı adını içeriyorsa, Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-oracle-azure-postgresql-online/dms-map-target-details.png)

3. **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş Özeti](media/tutorial-oracle-azure-postgresql-online/dms-migration-summary.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

  Geçiş etkinliği penceresi açılır ve etkinliğin **Durum** bilgisi **Başlatılıyor** olarak değişir.

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Çalıştırılıyor** olana kadar güncelleştirebilirsiniz.

     ![Etkinlik Durumu - çalışan](media/tutorial-oracle-azure-postgresql-online/dms-activity-running.png)

2. **Veritabanı Adı**altında, Tam **veri yükü** ve **Artımlı veri eşitleme** işlemleri için geçiş durumuna ulaşmak için belirli bir veritabanı seçin.

    Tam veri yüklemesi ilk yük geçiş durumunu, Artımlı veri eşitleme ise değişiklik verilerini yakalama (CDC) durumunu gösterir.

     ![Etkinlik Durumu - Tam yük tamamlandı](media/tutorial-oracle-azure-postgresql-online/dms-activity-full-load-completed.png)

     ![Etkinlik Durumu - Artımlı veri eşitleme](media/tutorial-oracle-azure-postgresql-online/dms-activity-incremental-data-sync.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme

İlk Tam yük tamamlandıktan sonra, veritabanları **Geçiş için hazır** olarak işaretlenir.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

2. **Bekleyen değişiklikler** sayacı **0** değerini gösterene kadar bekleyerek kaynak veritabanına gelen tüm işlemleri durdurduğunuzdan emin olun.

   ![Tam geçişi başlat](media/tutorial-oracle-azure-postgresql-online/dms-start-cutover.png)

3. **Onayla**'yı ve ardından, **Uygula**'yı seçin.
4. Veritabanı geçiş durumu **Tamamlandı'yı**gösterdiğinde, uygulamalarınızı PostgreSQL örneği için yeni hedef Azure Veritabanı'na bağlayın.

 > [!NOTE]
 > PostgreSQL varsayılan olarak küçük harfolarak schema.table.column olduğundan, bu makalenin başlarında **PostgreSQL için Azure Veritabanı'ndaki şemayı ayarla'daki** komut dosyasını kullanarak büyük harften küçük harfe geri dönebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* PostgreSQL için Azure Veritabanı'na yönelik çevrimiçi geçiş gerçekleştirirken karşılaşılan bilinen sorunlar ve sınırlamalar hakkında bilgi için [PostgreSQL için Azure Veritabanı geçiş işlemleri ile ilgili bilinen sorunlar ve geçici çözümler](known-issues-azure-postgresql-online.md) başlıklı makaleye bakın.
* Azure Veritabanı Geçiş Hizmeti hakkında bilgi için, makaleye bakın [Azure Veritabanı Geçiş Hizmeti nedir?](https://docs.microsoft.com/azure/dms/dms-overview)
* PostgreSQL için Azure Veritabanı hakkında daha fazla bilgi için, [PostgreSQL için Azure Veritabanı nedir makalesine bakın.](https://docs.microsoft.com/azure/postgresql/overview)
