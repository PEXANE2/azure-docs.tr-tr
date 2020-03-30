---
title: Ölçeklenmiş bulut veritabanları arasında raporlama
description: yatay bölümler üzerinde elastik sorgular nasıl ayarlanır?
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: sstein
ms.date: 01/03/2019
ms.openlocfilehash: 79abaade22fc107fa4c848607ff48232eeeb58ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823754"
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Ölçeklenmiş bulut veritabanları arasında raporlama (önizleme)

![Kırıklar arasında sorgu][1]

Parçalanmış veritabanları satırları ölçeklenmiş bir veri katmanına dağıtır. Şema, yatay bölümleme olarak da bilinen tüm katılımcı veritabanlarında aynıdır. Elastik bir sorgu kullanarak, parçalanmış bir veritabanındaki tüm veritabanlarını kapsayan raporlar oluşturabilirsiniz.

Hızlı bir başlangıç için, [ölçeklenmiş bulut veritabanları arasında Raporlama'ya](sql-database-elastic-query-getting-started.md)bakın.

Parçalanmış olmayan veritabanları [için, farklı şemalarla bulut veritabanları arasında Sorgula'ya](sql-database-elastic-query-vertical-partitioning.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Elastik veritabanı istemci kitaplığını kullanarak bir parça eşlemi oluşturun. bkz. [Shard harita yönetimi.](sql-database-elastic-scale-shard-map-management.md) Veya [elastik veritabanı araçları ile başlamak örnek](sql-database-elastic-scale-get-started.md)uygulamayı kullanın.
* Alternatif olarak, [bkz.](sql-database-elastic-convert-to-use-elastic-tools.md)
* Kullanıcı ALTER HER HANGİ Bİr DIS VERİ KAYNAK iznine sahip olmalıdır. Bu izin ALTER DATABASE iznine dahildir.
* ALTER HERHANGI BIR Dış VERI Kaynağı izinleri temel veri kaynağıbaşvurmak için gereklidir.

## <a name="overview"></a>Genel Bakış

Bu ifadeler, elastik sorgu veritabanında parçalanmış veri katmanınızın meta veri temsilini oluşturur.

1. [ANA ANAHTAR OLUŞTUR](https://msdn.microsoft.com/library/ms174382.aspx)
2. [VERITABANı KAPSAMLI Kimlik BILGILERI OLUŞTURMA](https://msdn.microsoft.com/library/mt270260.aspx)
3. [DıŞ VERI KAYNAĞı OLUŞTURMA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [DıŞ TABLO OLUŞTURMA](https://msdn.microsoft.com/library/dn935021.aspx)

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 Veritabanı kapsamı ana anahtarı ve kimlik bilgilerini oluşturma

Kimlik bilgisi, uzak veritabanlarınıza bağlanmak için elastik sorgu tarafından kullanılır.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> *"\<kullanıcı\>adı "* herhangi bir *"\@sunucu adı"* soneki içermediğinden emin olun.

## <a name="12-create-external-data-sources"></a>1.2 Dış veri kaynakları oluşturma

Söz dizimi:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>,
            SHARD_MAP_NAME = ‘<shardmapname>’
                   ) [;]

### <a name="example"></a>Örnek

    CREATE EXTERNAL DATA SOURCE MyExtSrc
    WITH
    (
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net',
        DATABASE_NAME='ShardMapDatabase',
        CREDENTIAL= SMMUser,
        SHARD_MAP_NAME='ShardMap'
    );

Geçerli dış veri kaynaklarının listesini alın:

    select * from sys.external_data_sources;

Dış veri kaynağı, parça haritanıza başvurur. Daha sonra elastik bir sorgu, veri katmanına katılan veritabanlarını sayısallandırmak için dış veri kaynağını ve altta yatan parça eşlemi kullanır.
Aynı kimlik bilgileri, parça eşlemi okumak ve elastik bir sorgunun işlenmesi sırasında kırıklar üzerindeki verilere erişmek için kullanılır.

## <a name="13-create-external-tables"></a>1.3 Harici tablolar oluşturma

Söz dizimi:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::=
      DATA_SOURCE = <External_Data_Source>,
      [ SCHEMA_NAME = N'nonescaped_schema_name',]
      [ OBJECT_NAME = N'nonescaped_object_name',]
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Örnek**

    CREATE EXTERNAL TABLE [dbo].[order_line](
         [ol_o_id] int NOT NULL,
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL,
         [ol_number] tinyint NOT NULL,
         [ol_i_id] int NOT NULL,
         [ol_delivery_d] datetime NOT NULL,
         [ol_amount] smallmoney NOT NULL,
         [ol_supply_w_id] int NOT NULL,
         [ol_quantity] smallint NOT NULL,
         [ol_dist_info] char(24) NOT NULL
    )

    WITH
    (
        DATA_SOURCE = MyExtSrc,
         SCHEMA_NAME = 'orders',
         OBJECT_NAME = 'order_details',
        DISTRIBUTION=SHARDED(ol_w_id)
    );

Geçerli veritabanından dış tabloların listesini alın:

    SELECT * from sys.external_tables;

Dış tabloları düşürmek için:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Açıklamalar

DATA\_SOURCE yan tümcesi, dış tablo için kullanılan dış veri kaynağını (parça lı harita) tanımlar.  

SCHEMA\_NAME ve\_OBJECT NAME yan tümceleri dış tablo tanımını farklı bir şemaya göre bir tabloyla eşler. Atlanırsa, uzak nesnenin şeması "dbo" olarak kabul edilir ve adı tanımlanan dış tablo adı ile aynı olduğu varsayılır. Bu, uzak tablonuzun adı dış tabloyu oluşturmak istediğiniz veritabanında zaten alınmışsa yararlıdır. Örneğin, ölçeklenmiş veri katmanınızda katalog görünümlerinin veya DMV'lerin toplu görünümünü almak için harici bir tablo tanımlamak istiyorsunuz. Katalog görünümleri ve DMV'ler zaten yerel olarak mevcut olduğundan, dış tablo tanımı için adlarını kullanamazsınız. Bunun yerine, farklı bir ad kullanın ve SCHEMA\_NAME ve/veya OBJECT\_NAME yan tümcelerinde katalog görünümünün veya DMV'nin adını kullanın. (Aşağıdaki örneğe bakın.)

DISTRIBUTION yan tümcesi, bu tablo için kullanılan veri dağılımını belirtir. Sorgu işlemcisi, en verimli sorgu planlarını oluşturmak için DISTRIBUTION yan tümcesinde sağlanan bilgileri kullanır.

1. **SHARDED,** verilerin veritabanları arasında yatay olarak bölümlenmiş olduğu anlamına gelir. Veri dağıtımı için bölümleme anahtarı>parametresi **sharding_column_name<.**
2. **REPLICATED,** tablonun aynı kopyalarının her veritabanında bulunduğu anlamına gelir. Yinelemelerin veritabanları arasında aynı olduğundan emin olmak sizin sorumluluğunuzdadır.
3. **ROUND\_ROBIN,** tablonun uygulamaya bağımlı bir dağıtım yöntemi kullanılarak yatay olarak bölümlenmiş olduğu anlamına gelir.

**Veri katmanı başvurusu**: DDL harici tablo, harici bir veri kaynağını ifade eder. Dış veri kaynağı, veri katmanınızdaki tüm veritabanlarını bulmak için gerekli bilgileri dış tabloya sağlayan bir parça harita belirtir.

### <a name="security-considerations"></a>Güvenlik konuları

Dış tabloya erişimi olan kullanıcılar, dış veri kaynağı tanımında verilen kimlik bilgisi altında temel uzak tablolara otomatik olarak erişir. Dış veri kaynağının kimlik bilgisi aracılığıyla ayrıcalıkların istenmeyen yükseltilerinden kaçının. Normal bir tabloymuş gibi harici bir tablo için GRANT veya REVOKE'u kullanın.  

Harici veri kaynağınızı ve harici tablolarınızı tanımladıktan sonra, artık dış tablolarınız üzerinde tam T-SQL kullanabilirsiniz.

## <a name="example-querying-horizontal-partitioned-databases"></a>Örnek: yatay bölümlenmiş veritabanlarını sorgulama

Aşağıdaki sorgu depolar, siparişler ve sipariş satırları arasında üç yönlü birbirleştirme gerçekleştirir ve birkaç agrega ve seçici filtre kullanır. (1) yatay bölümleme (parçalama) ve (2) ambarların, siparişlerin ve sipariş satırlarının ambar kimliği sütunu tarafından parçalandırıldığını ve elastik sorgunun parçaların üzerindeki birleştirmeleri birlikte bulabileceğini ve sorgunun parçadaki pahalı kısmını işleyebilir Paralel.

```sql
    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount,
         min(ol_delivery_d) as min_deliv_date
    from warehouse
    join orders
    on w_id = o_w_id
    join order_line
    on o_id = ol_o_id and o_w_id = ol_w_id
    where w_id > 100 and w_id < 200
    group by w_id, o_c_id
```

## <a name="stored-procedure-for-remote-t-sql-execution-sp_execute_remote"></a>Uzaktan T-SQL yürütme saklı yordamı: sp\_execute_remote

Elastik sorgu da kırıkları doğrudan erişim sağlayan bir depolanmış yordam ı tanır. Depolanan yordam [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) olarak adlandırılır ve uzak veritabanlarında uzak depolanan yordamları veya T-SQL kodunu yürütmek için kullanılabilir. Aşağıdaki parametreleri alır:

* Veri kaynağı adı (nvarchar): RDBMS türünün dış veri kaynağının adı.
* Sorgu (nvarchar): Her parça üzerinde yürütülecek T-SQL sorgusu.
* Parametre bildirimi (nvarchar) - isteğe bağlı: Sorgu parametresinde kullanılan parametreler için veri türü tanımlarını içeren dize (sp_executesql gibi).
* Parametre değer listesi - isteğe bağlı: Parametre değerlerinin virgülle ayrılmış listesi (sp_executesql gibi).

sp\_execute\_remote veritabanlarında verilen T-SQL deyimini yürütmek için çağırma parametrelerinde sağlanan dış veri kaynağını kullanır. Shardmap manager veritabanına ve uzak veritabanlarına bağlanmak için dış veri kaynağının kimlik bilgisini kullanır.  

Örnek:

```sql
    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse'
```

## <a name="connectivity-for-tools"></a>Araçlar için bağlantı

Uygulamanızı, BI ve veri tümleştirme araçlarınızı dış tablo tanımlarınızla veritabanına bağlamak için normal SQL Server bağlantı dizelerini kullanın. SQL Server'ın aracınız için veri kaynağı olarak desteklendirdiğinden emin olun. Ardından, araca bağlı diğer SQL Server veritabanı gibi elastik sorgu veritabanına başvurun ve aracınızdan veya uygulamanızdan gelen harici tabloları yerel tablolarmış gibi kullanın.

## <a name="best-practices"></a>En iyi uygulamalar

* Elastik sorgu uç noktası veritabanına shardmap veritabanına ve SQL DB güvenlik duvarları üzerinden tüm kırıklara erişim verildiğinden emin olun.  
* Dış tablo tarafından tanımlanan veri dağıtımını doğrulayın veya uygulayın. Gerçek veri dağıtımınız tablo tanımınızda belirtilen dağıtımdan farklıysa, sorgularınız beklenmeyen sonuçlar verebilir.
* Elastik sorgu şu anda parçalama anahtarı üzerindeki yüklemler belirli parçaları işlemekten güvenli bir şekilde dışlamasına izin verdiğinde parça eleme gerçekleştirmez.
* Elastik sorgu, hesaplamanın çoğunun kırıklar üzerinde yapılabilecek sorgular için en iyi sonucu sağlar. Genellikle, tüm kırıklarda bölüm hizalı bir şekilde gerçekleştirilebilen bölümleme tuşları üzerinde değerlendirilebilen veya birleşebilen seçici filtre yüklemleriyle en iyi sorgu performansını elde elabilirsiniz. Diğer sorgu desenleri, parçaparçadan kafa düğümüne büyük miktarda veri yüklemesi gerekebilir ve

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorguya genel bakış için [bkz.](sql-database-elastic-query-overview.md)
* Dikey bölümleme öğreticisi için [bkz.](sql-database-elastic-query-getting-started-vertical.md)
* Dikey olarak bölümlenmiş veriler için sözdizimi ve örnek sorgular için [bkz.](sql-database-elastic-query-vertical-partitioning.md)
* Yatay bölümleme (parçalama) öğreticisi [için](sql-database-elastic-query-getting-started.md)bkz.
* Tek bir uzak Azure SQL Veritabanında Transact-SQL deyimini yürüten veya yatay bir bölümleme düzeninde parça olarak hizmet veren veritabanları kümesini yürüten [sp\_ \_execute remote'a](https://msdn.microsoft.com/library/mt703714) bakın.

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
