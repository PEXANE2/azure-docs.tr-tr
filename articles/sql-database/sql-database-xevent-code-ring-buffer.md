---
title: XEvent Zil Arabellek kodu
description: Azure SQL Veritabanı'nda Zil Arabelleği hedefi nin kullanımıyla kolay ve hızlı hale getirilen bir Transact-SQL kod örneği sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: jrasnik
ms.date: 12/19/2018
ms.openlocfilehash: ad98b61d6339388551af93671b3d4d892942f4e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79213976"
---
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>SQL Veritabanı'nda genişletilmiş olaylar için Zil Arabelleği hedef kodu

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Bir test sırasında uzun bir olay için bilgileri yakalamanın ve bildirmenin en kolay hızlı yolu için tam bir kod örneği istiyorsunuz. Genişletilmiş olay verileri için en kolay hedef [Halka Arabellek hedefidir.](https://msdn.microsoft.com/library/ff878182.aspx)

Bu konu, bir Transact-SQL kod örneği sunar:

1. Gösterilen verilerle bir tablo oluşturur.
2. **Sqlserver.sql_statement_starting**gibi varolan genişletilmiş bir olay için bir oturum oluşturur.
   
   * Olay belirli bir Güncelleştirme dizesi içeren SQL deyimleri ile sınırlıdır: **deyimi LIKE '%UPDATE sekmesiÇalışan%'**.
   * Olayın çıktısını, **yani package0.ring_buffer**türü Ndeki Bir Hedef'e göndermeyi seçer.
3. Olay oturumunu başlatır.
4. Birkaç basit SQL UPDATE deyimi ni yayınlar.
5. Zil Arabelleği'nden olay çıktısını almak için bir SQL SELECT deyimi yayınlar.
   
   * **sys.dm_xe_database_session_targets** ve diğer dinamik yönetim görünümleri (DMV) birleştirilir.
6. Olay oturumunu durdurur.
7. Kaynaklarını serbest bırakmak için Ring Arabellek hedefini düşürür.
8. Etkinlik oturumunu ve demo tablosunu düşürür.

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabı ve aboneliği [Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Tablo oluşturabileceğiniz herhangi bir veritabanı.
  
  * İsteğe bağlı olarak birkaç dakika içinde [bir **AdventureWorksLT** gösteri veritabanı oluşturabilirsiniz.](sql-database-get-started.md)
* SQL Server Management Studio (ssms.exe), ideal olarak en son aylık güncelleme sürümü. 
  En son ssms.exe indirebilirsiniz:
  
  * Konu [Başlıklı SQL Server Management Studio İndir](https://msdn.microsoft.com/library/mt238290.aspx).
  * [İndirmek için doğrudan bir bağlantı.](https://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a>Kod örneği

Çok küçük bir değişiklikle, aşağıdaki Zil Buffer kodu örneği Azure SQL Veritabanı veya Microsoft SQL Server'da çalıştırılabilir. Aradaki fark, Adım 5'teki FROM yan tümcesinde kullanılan bazı dinamik yönetim görünümleri (DMV) adına '_database' düğümünün varlığıdır. Örnek:

* sys.dm_xe<strong>_database</strong>_session_targets
* sys.dm_xe_session_targets

&nbsp;

```sql
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;

## <a name="ring-buffer-contents"></a>Zil Arabellek içeriği

Kod örneğini çalıştırmak için ssms.exe kullandık.

Sonuçları görüntülemek için sütun başlığının altındaki hücreyi tıklattık **target_data_XML.**

Daha sonra sonuç bölmesinde sütun başlığının altındaki **hücreyi**tıklattık target_data_XML. Bu tıklatma, ssms.exe'de sonuç hücresinin içeriğinin XML olarak görüntülendiği başka bir dosya sekmesi oluşturmuştur.

Çıktı aşağıdaki blokta gösterilir. Uzun görünüyor, ama sadece ** \<** iki olay>elemanlarıdır.

&nbsp;

```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Halka Arabelleği tarafından tutulan kaynakları serbest bırakma

Zil Buffer'ınızla işiniz bittiğinde, bu bellek teni kaldırabilir ve aşağıdaki gibi bir **ALTER** veren kaynaklarını serbest bırakabilirsiniz:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Olay oturumunuzun tanımı güncelleştirildi, ancak düşürülmedi. Daha sonra olay oturumunuza Halka Arabelleği'nin başka bir örneğini ekleyebilirsiniz:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Daha fazla bilgi

Azure SQL Veritabanı'ndaki genişletilmiş etkinliklerin birincil konusu:

* [Sql Veritabanı'nda,](sql-database-xevent-db-diff-from-svr.md)Azure SQL Veritabanı ile Microsoft SQL Server arasında farklılık gösteren genişletilmiş olayların bazı yönlerini karşılaştıran genişletilmiş olay değerlendirmeleri.

Genişletilmiş olaylar için diğer kod örnek konuları aşağıdaki bağlantılarda mevcuttur. Ancak, örneğin Microsoft SQL Server ile Azure SQL Veritabanı'nı hedefleyip hedeflemediğini görmek için herhangi bir örneği düzenli olarak denetlemeniz gerekir. Ardından, örneği çalıştırmak için küçük değişiklikler gerekip gerekmediğine karar verebilirsiniz.

* Azure SQL Veritabanı için kod örneği: [SQL Veritabanı'ndaki genişletilmiş olaylar için Olay Dosyası hedef kodu](sql-database-xevent-code-event-file.md)

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](https://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](https://msdn.microsoft.com/library/bb630355.aspx)
-->
