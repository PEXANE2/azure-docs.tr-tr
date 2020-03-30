---
title: Farklı şema ile bulut veritabanları arasında sorgu
description: dikey bölümler üzerinde çapraz veritabanı sorguları nasıl ayarlanır
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: sstein
ms.date: 01/25/2019
ms.openlocfilehash: d5983d25685242a696300f293231bbf987e8442d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823725"
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Farklı şemalarla bulut veritabanları arasında sorgula (önizleme)

![Farklı veritabanlarındaki tablolar arasında sorgu][1]

Dikey bölümlenmiş veritabanları, farklı veritabanlarında farklı tablo kümeleri kullanır. Bu şema farklı veritabanlarında farklı olduğu anlamına gelir. Örneğin, muhasebeyle ilgili tüm tablolar ikinci bir veritabanında yken, stok için tüm tablolar tek bir veritabanındadır. 

## <a name="prerequisites"></a>Ön koşullar

* Kullanıcı ALTER HER HANGİ Bİr DIS VERİ KAYNAK iznine sahip olmalıdır. Bu izin ALTER DATABASE iznine dahildir.
* ALTER HERHANGI BIR Dış VERI Kaynağı izinleri temel veri kaynağıbaşvurmak için gereklidir.

## <a name="overview"></a>Genel Bakış

> [!NOTE]
> Yatay bölümlemeden farklı olarak, bu DDL deyimleri elastik veritabanı istemci kitaplığı üzerinden bir parça harita ile bir veri katmanı tanımlamaya bağlı değildir.
>

1. [ANA ANAHTAR OLUŞTUR](https://msdn.microsoft.com/library/ms174382.aspx)
2. [VERITABANı KAPSAMLI Kimlik BILGILERI OLUŞTURMA](https://msdn.microsoft.com/library/mt270260.aspx)
3. [DıŞ VERI KAYNAĞı OLUŞTURMA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [DıŞ TABLO OLUŞTURMA](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a>Veritabanı kapsamı ana anahtar ve kimlik bilgileri oluşturma

Kimlik bilgisi, uzak veritabanlarınıza bağlanmak için elastik sorgu tarafından kullanılır.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'master_key_password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Herhangi bir `<username>` **\@"sunucu adı"** soneki içermediğinden emin olun. 
>

## <a name="create-external-data-sources"></a>Dış veri kaynakları oluşturma

Söz dizimi:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> TYPE parametresi **RDBMS**olarak ayarlanmalıdır. 
>

### <a name="example"></a>Örnek

Aşağıdaki örnek, CREATE deyiminin dış veri kaynakları için kullanımını göstermektedir. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

Geçerli dış veri kaynaklarının listesini almak için: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Dış Tablolar

Söz dizimi:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Örnek

```sql
    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 
```

Aşağıdaki örnek, geçerli veritabanından dış tablolar listesini nasıl alsüreceğini gösterir: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Açıklamalar

Elastik sorgu, RDBMS türünün dış veri kaynaklarını kullanan dış tabloları tanımlamak için varolan dış tablo sözdizimini genişletir. Dikey bölümleme için harici bir tablo tanımı aşağıdaki yönleri kapsar: 

* **Şema**: DDL harici tablosu, sorgularınızın kullanabileceği bir şema tanımlar. Dış tablo tanımınızda sağlanan şema, gerçek verilerin depolandığı uzak veritabanındaki tabloların şemasını eşleştirmeli. 
* **Uzak veritabanı başvurusu**: DDL harici tablo, harici bir veri kaynağını ifade eder. Dış veri kaynağı, gerçek tablo verilerinin depolandığı uzak veritabanının SQL Veritabanı sunucu adını ve veritabanı adını belirtir. 

Önceki bölümde özetlenen bir dış veri kaynağı kullanarak, dış tablolar oluşturmak için sözdizimi aşağıdaki gibidir: 

DATA_SOURCE yan tümcesi, dış tablo için kullanılan dış veri kaynağını (yani dikey bölümleme durumunda uzak veritabanını) tanımlar.  

SCHEMA_NAME ve OBJECT_NAME yan tümceleri, dış tablo tanımını uzak veritabanındaki farklı bir şemaya veya sırasıyla farklı bir ada sahip bir tabloyla eşlenebilme olanağı sağlar. Bu, uzak veritabanınızdaki bir katalog görünümüne veya DMV'ye harici bir tablo tanımlamak istiyorsanız veya uzak tablo adının zaten yerel olarak alındığı başka bir durum için yararlıdır.  

Aşağıdaki DDL deyimi yerel katalogdan varolan bir dış tablo tanımı düşer. Uzak veritabanını etkilemez. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**CREATE/DROP EXTERNAL TABLE için izinler**: ALTER ANY EXTERNAL DATA SOURCE izinleri, altta yatan veri kaynağına başvurmak için gerekli olan harici tablo DDL için gereklidir.  

## <a name="security-considerations"></a>Güvenlik konuları

Dış tabloya erişimi olan kullanıcılar, dış veri kaynağı tanımında verilen kimlik bilgisi altında temel uzak tablolara otomatik olarak erişir. Dış veri kaynağının kimlik bilgisi aracılığıyla ayrıcalıkların istenmeyen yükseltisini önlemek için dış tabloya erişimi dikkatle yönetmelisiniz. Normal SQL izinleri, normal bir tabloymuş gibi harici bir tabloya erişimi vermek veya iptal etmek için kullanılabilir.  

## <a name="example-querying-vertically-partitioned-databases"></a>Örnek: dikey bölümlenmiş veritabanlarını sorgulama

Aşağıdaki sorgu, siparişler ve sipariş satırları için iki yerel tablo ile müşteriler için uzak tablo arasında üç yönlü bir birleştirme gerçekleştirir. Bu, elastik sorgu için başvuru veri kullanımı örneğine bir örnektir: 

```sql
    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100
```

## <a name="stored-procedure-for-remote-t-sql-execution-sp_execute_remote"></a>Uzaktan T-SQL yürütme saklı yordamı: sp\_execute_remote

Elastik sorgu, uzak veritabanına doğrudan erişim sağlayan depolanmış bir yordam da sunar. Depolanan yordam [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) olarak adlandırılır ve uzak veritabanında uzak depolanan yordamları veya T-SQL kodunu yürütmek için kullanılabilir. Aşağıdaki parametreleri alır: 

* Veri kaynağı adı (nvarchar): RDBMS türünün dış veri kaynağının adı. 
* Sorgu (nvarchar): Uzak veritabanında yürütülecek T-SQL sorgusu. 
* Parametre bildirimi (nvarchar) - isteğe bağlı: Sorgu parametresinde kullanılan parametreler için veri türü tanımlarını içeren dize (sp_executesql gibi). 
* Parametre değer listesi - isteğe bağlı: Parametre değerlerinin virgülle ayrılmış listesi (sp_executesql gibi).

sp\_execute\_remote veritabanında verilen T-SQL deyimini yürütmek için çağırma parametrelerinde sağlanan dış veri kaynağını kullanır. Uzak veritabanına bağlanmak için dış veri kaynağının kimlik bilgisini kullanır.  

Örnek: 

```sql
    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 
```

## <a name="connectivity-for-tools"></a>Araçlar için bağlantı

Bi ve veri tümleştirme araçlarınızı sql DB sunucusunda elastik sorgu etkinleştirilmiş ve dış tablolar tanımlanmış veritabanlarına bağlamak için normal SQL Server bağlantı dizelerini kullanabilirsiniz. SQL Server'ın aracınız için veri kaynağı olarak desteklendirdiğinden emin olun. Ardından, aracınızla bağlanacağınız diğer SQL Server veritabanı gibi elastik sorgu veritabanına ve dış tablolarına bakın. 

## <a name="best-practices"></a>En iyi uygulamalar

* SQL DB güvenlik duvarı yapılandırmasında Azure Hizmetleri'ne erişim sağlayarak elastik sorgu uç noktası veritabanına uzak veritabanına erişim verildiğinden emin olun. Ayrıca, dış veri kaynağı tanımında sağlanan kimlik bilgisinin uzak veritabanına başarıyla oturum açabilmesini ve uzak tabloya erişmek için izinlere sahip olduğundan emin olun.  
* Elastik sorgu, hesaplamanın çoğunun uzak veritabanlarında yapılabilecek sorgular için en iyi sonucu sağlar. Genellikle uzak veritabanlarında değerlendirilebilen seçici filtre yüklemleri veya uzak veritabanında tamamen gerçekleştirilebilen birleştirmelerle en iyi sorgu performansını elde elabilirsiniz. Diğer sorgu desenleri uzak veritabanından büyük miktarda veri yüklemek gerekebilir ve kötü performans gösterebilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorguya genel bakış için [bkz.](sql-database-elastic-query-overview.md)
* Dikey bölümleme öğreticisi için [bkz.](sql-database-elastic-query-getting-started-vertical.md)
* Yatay bölümleme (parçalama) öğreticisi [için](sql-database-elastic-query-getting-started.md)bkz.
* Yatay olarak bölümlenmiş veriler için sözdizimi ve örnek sorgular için bkz: [Yatay olarak bölümlenmiş verileri sorgula)](sql-database-elastic-query-horizontal-partitioning.md)
* Tek bir uzak Azure SQL Veritabanında Transact-SQL deyimini yürüten veya yatay bir bölümleme düzeninde parça olarak hizmet veren veritabanları kümesini yürüten [sp\_ \_execute remote'a](https://msdn.microsoft.com/library/mt703714) bakın.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
