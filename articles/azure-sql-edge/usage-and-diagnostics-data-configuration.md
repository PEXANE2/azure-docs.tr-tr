---
title: Azure SQL Edge kullanımı ve tanılama veri yapılandırması
description: Azure SQL Edge 'de kullanım ve tanılama verilerinin nasıl yapılandırılacağını öğrenin.
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 08/04/2020
ms.openlocfilehash: 8547c07214e94176babe4909504b9292d45c06f9
ms.sourcegitcommit: 5a37753456bc2e152c3cb765b90dc7815c27a0a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87759623"
---
# <a name="azure-sql-edge-usage-and-diagnostics-data-configuration"></a>Azure SQL Edge kullanımı ve tanılama veri yapılandırması

Varsayılan olarak, Azure SQL Edge, müşterilerinin uygulamayı nasıl kullandığı hakkında bilgi toplar. Azure SQL Edge, özellikle dağıtım deneyimi, kullanımı ve performansı hakkında bilgi toplar. Bu bilgiler, Microsoft 'un müşteri ihtiyaçlarını daha iyi karşılaması için ürünün iyileştirilmesine yardımcı olur. Örneğin, Microsoft, ilgili hataları çözebilmemiz, Azure SQL Edge 'i kullanma hakkındaki belgelerimizi iyileştirebilmek ve müşterilere daha iyi bir şekilde sunmak için özelliklerin ürüne eklenip eklenmeyeceğini belirlemek için müşterilerin karşılaştığı hata kodu türleri hakkında bilgi toplar.

Microsoft, bu mekanizmaya özellikle aşağıdaki bilgi türlerinden hiçbirini göndermez:

- Kullanıcı tablolarının içinden herhangi bir değer.
- Herhangi bir oturum açma kimlik bilgisi veya diğer kimlik doğrulama bilgileri.
- Tüm kişisel veya müşteri verileri.

Aşağıdaki örnek senaryo, ürünün iyileştirilmesine yardımcı olan özellik kullanım bilgilerini içerir.

Kullanım ve tanılama veri koleksiyonu için kullanılan sorgulardan örnek bir sorgu aşağıda verilmiştir. Sorgu, Azure SQL Edge 'de kullanılan farklı akış veri kaynaklarının sayısını ve türlerini tanımlar. Bu veriler, Microsoft 'un bu veri kaynaklarıyla ilişkili performans ve Kullanıcı deneyimini iyileştirebilmesi için Microsoft 'un hangi akış veri kaynaklarının yaygın olarak kullanıldığını belirlemesine yardımcı olur. 

```sql
select 
count(*) as [count], sum(inputs) as inputs, sum(outputs) as outputs, sum(linked_to_job) 
as linked_to_job, data_source_type  
from ( 
select isnull(value,'unknown') as data_source_type, inputs, outputs, linked_to_job  
from 
    ( 
        select 
        convert(sysname, lower(substring(ds.location, 0, charindex('://', ds.location))), 1) as data_source_type, 
        isnull(inputs, 0) as inputs, isnull(outputs, 0) as outputs, isnull(js.stream_id/js.stream_id, 0) as linked_to_job 
        from sys.external_streams es              
        join sys.external_data_sources ds 
             on es.data_source_id = ds.data_source_id             
        left join 
            ( 
            select stream_id, max(cast(is_input as int)) inputs, max(cast(is_output as int)) outputs 
            from sys.external_job_streams group by stream_id 
            ) js                
             on js.stream_id = es.object_id 
    ) ds            
left join 
    (
        select value from string_split('edgehub,sqlserver,kafka', ',')) as known_ep on data_source_type = value 
    ) known_ds        
group by data_source_type
```

## <a name="disable-usage-and-diagnostic-data-collection"></a>Kullanım ve tanılama veri toplamayı devre dışı bırak

Azure SQL Edge 'de kullanım ve tanılama veri toplama, aşağıdaki yöntemlerden biri kullanılarak devre dışı bırakılabilir.

### <a name="disable-usage-and-diagnostics-using-environment-variables"></a>Ortam değişkenlerini kullanarak kullanımı ve tanılamayı devre dışı bırakma

Azure SQL Edge 'de kullanım ve tanılama veri toplamayı devre dışı bırakmak için aşağıdaki ortam değişkenini ekleyin ve değerini olarak ayarlayın `*False*` . Ortam değişkenlerini kullanarak Azure SQL Edge 'i yapılandırma hakkında daha fazla bilgi için bkz. [ortam değişkenlerini kullanarak yapılandırma](configure.md#configure-by-using-environment-variables).

`MSSQL_TELEMETRY_ENABLED = TRUE | FALSE`

- TRUE-kullanım ve tanılama verilerinin toplanmasını mümkün. Bu, varsayılan yapılandırmadır.
- FALSE-kullanım ve tanılama verilerinin toplanmasını devre dışı bırakır

### <a name="disable-usage-and-diagnostics-using-mssqlconf-file"></a>MSSQL. conf dosyasını kullanarak kullanımı ve tanılamayı devre dışı bırakma

Azure SQL Edge 'de kullanım ve tanılama veri toplamayı devre dışı bırakmak için, SQL Edge modülünde/var/seçenek/MSSQL/Folder öğesine eşlenen kalıcı depolama sürücüsündeki MSSQL. conf dosyasına aşağıdaki dosyaları ekleyin. MSSQL. conf dosyasını kullanarak Azure SQL Edge 'i yapılandırma hakkında daha fazla bilgi için bkz. [MSSQL. conf dosyasını kullanarak yapılandırma](configure.md#configure-by-using-an-mssqlconf-file).

```ini
[telemetry]
customerfeedback = false
```

## <a name="local-audit-of-usage-and-diagnostic-data-collection"></a>Kullanım ve Tanılama verileri toplama için yerel denetim

Azure SQL Edge kullanımı ve tanılama veri koleksiyonunun yerel denetim bileşeni, hizmet tarafından toplanan verileri Microsoft 'a gönderilecek verileri (Günlükler) temsil eden bir belirtilen klasöre yazabilir. Yerel denetimin amacı, müşterilerin Microsoft 'un bu özellikle topladığı tüm verileri, uyumluluk, mevzuat veya gizlilik doğrulama nedenleriyle görmesine izin versağlamaktır.

### <a name="enable-local-audit-of-usage-and-diagnostics-data"></a>Kullanım ve tanılama verilerinin yerel denetimini etkinleştirin

Azure SQL Edge 'de yerel denetim kullanımını ve tanılama verilerini etkinleştirmek için

1. Yeni yerel denetim günlüğü depolaması için bir hedef dizin oluşturun. Bu hedef dizin, konakta ya da kapsayıcı içinde olabilir. Aşağıdaki örnekte, hedef dizin SQL Edge 'de/var/seçenek/MSSQL/Path ile eşlenmiş aynı bağlama biriminde oluşturulur.

   ```bash
   sudo mkdir <host mount path>/audit
   ```

2. Ortam değişkenlerini veya MSSQL. conf dosyasını kullanarak kullanım ve tanılama verilerinin denetimini yapılandırın.

   - Ortam değişkenlerini kullanma-aşağıdaki ortam değişkenini SQL Edge dağıtımınıza ekleyin ve denetim dosyaları için hedef dizini belirtin.
   
     `*MSSQL_TELEMETRY_DIR = <host mount path>/audit*`
   
   - MSSQL. conf dosyasını kullanarak-MSSQL. conf dosyasına aşağıdaki satırları ekleyin ve denetim dosyaları için hedef dizini belirtin.
       ```ini
       [telemetry]
       userrequestedlocalauditdirectory  = <host mount path>/audit
       ```  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL Edge 'e bağlanma](connect.md)
- [SQL Edge ile uçtan uca IoT çözümü oluşturma](tutorial-deploy-azure-resources.md)
