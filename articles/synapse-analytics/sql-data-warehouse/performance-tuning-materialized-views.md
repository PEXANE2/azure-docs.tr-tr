---
title: Gerçekleştirilmiş görünümler ile performans ayarlama
description: Sorgu performansınızı artırmak için somutlaştırılmış görünümleri kullanırken bilmeniz gereken öneriler ve hususlar.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.custom: seo-lt-2019
ms.openlocfilehash: e359537bbd0686d27825eee79e1cd35516ba6099
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350918"
---
# <a name="performance-tuning-with-materialized-views"></a>Gerçekleştirilmiş görünümler ile performans ayarlama 
Azure SQL Veri Ambarı'ndaki somutlaştırılmış görünümler, karmaşık analitik sorguların sorgu değişikliği olmadan hızlı performans elde etmesi için düşük bakım yöntemi sağlar. Bu makalede, maddeleştirilmiş görünümlerin kullanılmasına ilişkin genel kılavuz açıklanmaktadır.


## <a name="materialized-views-vs-standard-views"></a>Maddeleştirilmiş görünümler ve standart görünümler
Azure SQL Veri Ambarı standart ve somutlaştırılmış görünümleri destekler.  Her ikisi de SELECT ifadeleri ile oluşturulan ve sorgulara mantıksal tablolar olarak sunulan sanal tablolardır.  Görünümler, ortak veri hesaplamasının karmaşıklığını kapsüller ve sorguları yeniden yazmaya gerek kalmadan hesaplama değişikliklerine bir soyutlama katmanı ekler.  

Standart görünüm, görünüm her kullanıldığında verilerini bilgilenir.  Diskte depolanan veri yok. İnsanlar genellikle bir veritabanında mantıksal nesneleri ve sorguları düzenlemeye yardımcı olan bir araç olarak standart görünümleri kullanır.  Standart bir görünüm kullanmak için, bir sorgunun doğrudan başvuruda bulunması gerekir. 

Somutlaştırılmış görünüm, verilerini bir tablo gibi Azure SQL Veri Ambarı'nda önceden hesaplar, depolar ve korur.  Somutlaştırılmış görünüm kullanıldığında her seferinde yeniden hesaplama gerekmez.  Bu nedenle, verilerin tamamını veya alt kümesini maddeleştirilmiş görünümlerde kullanan sorgular daha hızlı performans elde edebilir.  Daha da iyisi, sorgular doğrudan başvuru yapmadan somutlaştırılmış bir görünüm kullanabilir, bu nedenle uygulama kodunu değiştirmeye gerek yoktur.  

Standart görünümdeki gereksinimlerin çoğu yine de maddeleştirilmiş görünüm için geçerlidir. Maddeleştirilmiş görünüm sözdizimi ve diğer gereksinimler hakkında ayrıntılı bilgi için SELECT [olarak MATERYALIZE GÖRÜNÜM OLUŞTUR'a](https://docs.microsoft.com/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest)bakın.



| Karşılaştırma                     | Görünüm                                         | Gerçekleştirilmiş Görünüm             
|:-------------------------------|:---------------------------------------------|:--------------------------------------------------------------| 
|Tanımı görüntüleme                 | Azure veri ambarında depolanır.              | Azure veri ambarında depolanır.    
|İçeriği görüntüleme                    | Görünüm her kullanıldığında oluşturulur.   | Görünüm oluşturma sırasında Önceden işlenmiş ve Azure veri ambarında depolanmış. Veriler temel tablolara eklendikçe güncelleştirildi.                                             
|Veri yenileme                    | Her zaman güncelleştirildi                               | Her zaman güncelleştirildi                          
|Karmaşık sorgulardan görünüm verilerini alma hızı     | Yavaş                                         | Hızlı  
|Ekstra depolama                   | Hayır                                           | Evet                             
|Sözdizimi                          | GÖRÜNÜM OLUŞTUR                                  | SELECT OLARAK MADDELEŞTIRILMIŞ GÖRÜNÜM OLUŞTURMA           
     
## <a name="benefits-of-using-materialized-views"></a>Maddeleştirilmiş görünümleri kullanmanın yararları

Düzgün tasarlanmış bir materyalize görünüm aşağıdaki avantajları sağlayabilir:

- JO'lar ve toplu işlevler içeren karmaşık sorgular için yürütme süresini azaltın. Sorgu ne kadar karmaşıksa, yürütme-zaman tasarrufu potansiyeli de o kadar yüksek olabilir. Bir sorgunun hesaplama maliyeti yüksek olduğunda ve ortaya çıkan veri kümesi küçük olduğunda en fazla avantaj elde edilir.  

- Azure SQL Veri Ambarı'ndaki en iyi duruma getirici, sorgu yürütme planlarını geliştirmek için otomatik olarak dağıtılmış önemli leştirilmiş görünümleri kullanabilir.  Bu işlem, daha hızlı sorgu performansı sağlayan kullanıcılar için saydamdır ve somutlaştırılmış görünümlere doğrudan başvuru yapmak için sorgular gerektirmez. 

- Görünümler üzerinde düşük bakım gerektirir.  Temel tablolardaki tüm artımlı veri değişiklikleri, eşzamanlı bir şekilde otomatik olarak maddeleştirilmiş görünümlere eklenir.  Bu tasarım, önemlileştirilmiş görünümleri sorgulayarak, temel tabloları doğrudan sorgulayan la aynı verileri döndürmeye olanak tanır. 
- Somutlaştırılmış görünümdeki veriler temel tablolardan farklı olarak dağıtılabilir.  
- Somutlaştırılmış görünümdeki veriler, normal tablolardaki verilerle aynı yüksek kullanılabilirlik ve esneklik avantajlarından yararlanır.  
 
Diğer veri ambarı sağlayıcılarıyla karşılaştırıldığında, Azure SQL Veri Ambarı'nda uygulanan somutlaştırılmış görünümler de aşağıdaki ek avantajlar sağlar: 

- Temel tablolardaki veri değişiklikleriyle otomatik ve eşzamanlı veri yenileme. Kullanıcı eylemi gerekmez. 
- Geniş toplam işlev desteği. Bkz. [SELECT (Transact-SQL) olarak MATERYALIZE GÖRÜNÜM OLUŞTUR.](https://docs.microsoft.com/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest)
- Sorguya özgü maddeleştirilmiş görünüm önerisi desteği.  Bkz. [AÇIKLAMA (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/explain-transact-sql?view=azure-sqldw-latest).

## <a name="common-scenarios"></a>Genel senaryolar  

Somutlaştırılmış görünümler genellikle aşağıdaki senaryolarda kullanılır: 

**Boyutu büyük verilere karşı karmaşık analitik sorguların performansını artırmak gerekir**

Karmaşık analitik sorgular genellikle daha fazla toplama işlevleri ve tablo birleşimleri kullanarak sorgu yürütmede karıştırma lar ve birleştirmeler gibi daha fazla işlem ağırlıklı işleme neden olur.  Bu nedenle, özellikle büyük tablolarda bu sorguların tamamlanması daha uzun sürer.  Kullanıcılar sorguların ortak hesaplamalarından döndürülen veriler için önemlileştirilmiş görünümler oluşturabilir, bu nedenle bu veriler sorgular tarafından gerektiğinde yeniden hesaplama gerekmez ve böylece daha düşük işlem maliyeti ve daha hızlı sorgu yanıtı sağlar. 

**Hiçbir veya minimum sorgu değişikliği olmadan daha hızlı performansa ihtiyaç duyma**

Veri ambarlarında şema ve sorgu değişiklikleri genellikle normal ETL işlemleri ve raporlama desteklemek için minimumda tutulur.  Görünümlerin neden olduğu maliyet sorgu performansındaki kazançla dengelenebilirse, kişiler sorgu performansı atoması için önemlileştirilmiş görünümler kullanabilir. Ölçekleme ve istatistik yönetimi gibi diğer tuning seçenekleriyle karşılaştırıldığında, maddeleşmiş bir görünüm oluşturmak ve korumak için çok daha az etkili bir üretim değişimidir ve potansiyel performans kazancı da daha yüksektir.

- Somutlaştırılmış görünümler oluşturmak veya korumak, temel tablolara karşı çalışan sorguları etkilemez.
- Sorgu optimize edici, bir sorguda doğrudan görünüm başvurusu olmadan dağıtılan maddeleştirilmiş görünümleri otomatik olarak kullanabilir. Bu özellik, performans atonlama sorgu değişikliği ihtiyacını azaltır. 

**Daha hızlı sorgu performansı için farklı veri dağıtım stratejisine ihtiyacınız var**

Azure veri ambarı, dağıtılmış büyük ölçüde paralel işleme (MPP) sistemidir.   Veri ambarı tablosundaki veriler, üç [dağıtım stratejilerinden](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute) biri (karma, round_robin veya çoğaltılmış) kullanılarak 60 düğüme dağıtılır.  Veri dağıtımı tablo oluşturma zamanında belirtilir ve tablo bırakılana kadar değişmeden kalır. Disküzerinde sanal bir tablo olan materyalize görünüm karma ve round_robin veri dağıtımlarını destekler.  Kullanıcılar temel tablolardan farklı, ancak görünümleri en çok kullanan sorguların performansı için en uygun olan bir veri dağıtımı seçebilir.  

## <a name="design-guidance"></a>Tasarım kılavuzu 

Sorgu performansını artırmak için somutlaştırılmış görünümleri kullanma yla ilgili genel kılavuz aşağıda veda edinilir:

**İş yükünüz için tasarım**

Somutlaştırılmış görünümler oluşturmaya başlamadan önce, sorgu desenleri, önemi, sıklığı ve elde edilen verilerin boyutu açısından iş yükünüzü derinlemesine anlamanız önemlidir.  

Kullanıcılar, sorgu optimize edici tarafından önerilen önemlileştirilmiş görünümler için WITH_RECOMMENDATIONS <SQL_statement> çalıştırabilir.  Bu öneriler sorguya özgü olduğundan, tek bir sorgudan yararlanan, aynı iş yükündeki diğer sorgular için en uygun olmayabilir.  Bu önerileri iş yükünüz göz önünde bulundurularak değerlendirin.  İdeal somutlaştırılmış görünümler, iş yükünün performansından yararlanan görünümlerdir.  

**Daha hızlı sorgular ve maliyet arasındaki dengenin farkında olun** 

Her somutlaştırılmış görünüm için bir veri depolama maliyeti ve görünümü korumak için bir maliyet vardır.  Temel tablolardaki veri değiştikçe, maddeleştirilmiş görünümün boyutu artar ve fiziksel yapısı da değişir.  Sorgu performansı bozulmasını önlemek için, her somutlaştırılmış görünüm veri ambarı altyapısı tarafından ayrı ayrı tutulur.  Somutlaştırılmış görünüm sayısı ve taban tablosu değişiklikleri arttığında bakım iş yükü artar.   Kullanıcılar, tüm somutlaştırılmış görünümlerden kaynaklanan maliyetin sorgu performansı kazancıyla dengelenebilir mi kontrol etmelidir.  

Bu sorguyu, veritabanındaki maddelendirilmiş görünüm listesi için çalıştırabilirsiniz: 

```sql
SELECT V.name as materialized_view, V.object_id 
FROM sys.views V 
JOIN sys.indexes I ON V.object_id= I.object_id AND I.index_id < 2;
``` 

Somutlaştırılmış görünüm sayısını azaltmak için seçenekler: 

- İş yüklerinizdeki karmaşık sorgular tarafından sıklıkla kullanılan yaygın veri kümelerini tanımlayın.  En iyi duruma getirici yürütme planları oluştururken bunları yapı taşları olarak kullanabilsin diye bu veri kümelerini depolamak için maddeleştirilmiş görünümler oluşturun.  

- Düşük kullanıma sahip veya artık gerekli olmayan maddeleştirilmiş görünümleri bırakın.  Devre dışı bırakılmış bir maddeleştirilmiş görünüm korunmaz, ancak yine de depolama maliyetine neden olur.  

- Verileri çakışmasa bile aynı veya benzer temel tablolarda oluşturulan somutlaştırılmış görünümleri birleştirin.  Maddeleştirilmiş görünümlerin taranması, ayrı görünümlerin toplamından daha büyük bir boyut görünümüne neden olabilir, ancak görünüm bakım maliyetini azaltmalı.  Örnek:

```sql

-- Query 1 would benefit from having a materialized view created with this SELECT statement

SELECT A, SUM(B)
FROM T
GROUP BY A

-- Query 2 would benefit from having a materialized view created with this SELECT statement

SELECT C, SUM(D)
FROM T
GROUP BY C

-- You could create a single mateiralized view of this form

SELECT A, C, SUM(B), SUM(D)
FROM T
GROUP BY A, C

```

**Tüm performans atonunudlama sorgu değişikliği gerektirmez**

Veri ambarı optimize edici, sorgu performansını artırmak için dağıtılan maddeleştirilmiş görünümleri otomatik olarak kullanabilir.  Bu destek, somutlaştırılmış görünümler oluşturma da desteklenmeyen agregakullanan görünümler ve sorgular başvuruolmayan sorgular için saydam olarak uygulanır.  Sorgu değişikliği gerekmez. Somutlaştırılmış bir görünüm kullanılıp kullanılıp kullanılmamasını onaylamak için bir sorgunun tahmini yürütme planını denetleyebilirsiniz.  

**Önemli leştirilmiş görünümleri izleme** 

Maddeleştirilmiş görünüm, kümelenmiş sütun deposu dizini (CCI) olan bir tablo gibi veri ambarında depolanır.  Verileri somutlaştırılmış bir görünümden okuma, CCI dizin segmentlerini taramayı ve temel tablolardan herhangi bir artımlı değişiklik uygulamayı içerir. Artımlı değişiklik sayısı çok yüksekolduğunda, bir sorguyu somutlaştırılmış görünümden çözme, temel tabloları doğrudan sorgulamaktan daha uzun sürebilir.  Sorgu performansı bozulmasını önlemek için, görünümün overhead_ratio (total_rows / max(1, base_view_row)) izlemek için [DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql?view=azure-sqldw-latest) çalıştırmak için iyi bir uygulamadır.  Kullanıcılar, overhead_ratio çok yüksekse, materyalize görünümü YENIDEN OLUŞTURMALIDIR. 

**Maddeleştirilmiş görünüm ve sonuç kümesi önbelleğe alma**

Bu iki özellik, sorgu performans için aynı zamanda Azure SQL Veri Ambarı'nda tanıtılır.  Sonuç kümesi önbelleğe alma, statik verilere karşı yinelenen sorgulardan yüksek eşzamanlılık ve hızlı yanıt almak için kullanılır.  Önbelleğe alınmış sonucu kullanmak için, sorgu isteyen önbellek biçiminin önbelleği oluşturan sorguyla eşleşmesi gerekir.  Ayrıca, önbelleğe alınmış sonucun tüm sorguiçin geçerli olması gerekir.  Somutlaştırılmış görünümler temel tablolarda veri değişikliklerine izin verir.  Maddeleştirilmiş görünümdeki veriler bir sorguparçasına uygulanabilir.  Bu destek, aynı somutlaştırılmış görünümlerin daha hızlı performans için bazı hesaplamaları paylaşan farklı sorgular tarafından kullanılmasını sağlar.

## <a name="example"></a>Örnek

Bu örnekte, katalog üzerinden mağazalara göre daha fazla para harcayan müşterileri bulan, tercih edilen müşterileri ve geldikleri ülkeyi tanımlayan TPCDS benzeri bir sorgu kullanılır.   Sorgu, SUM() ve GROUP BY içeren üç alt SELECT deyiminin birliğinden TOP 100 kayıtlarını seçmeyi içerir. 

```sql
WITH year_total AS (
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(ss_ext_list_price-ss_ext_wholesale_cost-ss_ext_discount_amt+ss_ext_sales_price, 0)/2) year_total
       ,'s' sale_type
FROM customer
     ,store_sales 
     ,date_dim
WHERE c_customer_sk = ss_customer_sk
   AND ss_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year
UNION ALL
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(cs_ext_list_price-cs_ext_wholesale_cost-cs_ext_discount_amt+cs_ext_sales_price, 0)/2) year_total
       ,'c' sale_type
FROM customer
     ,catalog_sales 
     ,date_dim
WHERE c_customer_sk = cs_bill_customer_sk
   AND cs_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year
UNION ALL
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(ws_ext_list_price-ws_ext_wholesale_cost-ws_ext_discount_amt+ws_ext_sales_price, 0)/2) year_total
       ,'w' sale_type
FROM customer
     ,web_sales 
     ,date_dim
WHERE c_customer_sk = ws_bill_customer_sk
   AND ws_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year
         )
  SELECT TOP 100 
                  t_s_secyear.customer_id
                 ,t_s_secyear.customer_first_name
                 ,t_s_secyear.customer_last_name
                 ,t_s_secyear.customer_birth_country
FROM year_total t_s_firstyear
     ,year_total t_s_secyear
     ,year_total t_c_firstyear
     ,year_total t_c_secyear
     ,year_total t_w_firstyear
     ,year_total t_w_secyear
WHERE t_s_secyear.customer_id = t_s_firstyear.customer_id
   AND t_s_firstyear.customer_id = t_c_secyear.customer_id
   AND t_s_firstyear.customer_id = t_c_firstyear.customer_id
   AND t_s_firstyear.customer_id = t_w_firstyear.customer_id
   AND t_s_firstyear.customer_id = t_w_secyear.customer_id
   AND t_s_firstyear.sale_type = 's'
   AND t_c_firstyear.sale_type = 'c'
   AND t_w_firstyear.sale_type = 'w'
   AND t_s_secyear.sale_type = 's'
   AND t_c_secyear.sale_type = 'c'
   AND t_w_secyear.sale_type = 'w'
   AND t_s_firstyear.dyear+0 =  1999
   AND t_s_secyear.dyear+0 = 1999+1
   AND t_c_firstyear.dyear+0 =  1999
   AND t_c_secyear.dyear+0 =  1999+1
   AND t_w_firstyear.dyear+0 = 1999
   AND t_w_secyear.dyear+0 = 1999+1
   AND t_s_firstyear.year_total > 0
   AND t_c_firstyear.year_total > 0
   AND t_w_firstyear.year_total > 0
   AND CASE WHEN t_c_firstyear.year_total > 0 THEN t_c_secyear.year_total / t_c_firstyear.year_total ELSE NULL END
           > CASE WHEN t_s_firstyear.year_total > 0 THEN t_s_secyear.year_total / t_s_firstyear.year_total ELSE NULL END
   AND CASE WHEN t_c_firstyear.year_total > 0 THEN t_c_secyear.year_total / t_c_firstyear.year_total ELSE NULL END
           > CASE WHEN t_w_firstyear.year_total > 0 THEN t_w_secyear.year_total / t_w_firstyear.year_total ELSE NULL END
ORDER BY t_s_secyear.customer_id
         ,t_s_secyear.customer_first_name
         ,t_s_secyear.customer_last_name
         ,t_s_secyear.customer_birth_country
OPTION ( LABEL = 'Query04-af359846-253-3');
```

Sorgunun tahmini yürütme planını denetleyin.  Yürütmek için daha fazla zaman alan 18 shuffles ve 17 birleştirme işlemleri vardır. Şimdi üç alt SELECT deyiminin her biri için bir somutlaştırılmış görünüm oluşturalım.   

```sql
CREATE materialized view nbViewSS WITH (DISTRIBUTION=HASH(customer_id)) AS
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(ss_ext_list_price-ss_ext_wholesale_cost-ss_ext_discount_amt+ss_ext_sales_price, 0)/2) year_total
          , count_big(*) AS cb
FROM dbo.customer
     ,dbo.store_sales
     ,dbo.date_dim
WHERE c_customer_sk = ss_customer_sk
   AND ss_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year
GO
CREATE materialized view nbViewCS WITH (DISTRIBUTION=HASH(customer_id)) AS
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(cs_ext_list_price-cs_ext_wholesale_cost-cs_ext_discount_amt+cs_ext_sales_price, 0)/2) year_total
          , count_big(*) as cb
FROM dbo.customer
     ,dbo.catalog_sales
     ,dbo.date_dim
WHERE c_customer_sk = cs_bill_customer_sk
   AND cs_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year

GO
CREATE materialized view nbViewWS WITH (DISTRIBUTION=HASH(customer_id)) AS
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(ws_ext_list_price-ws_ext_wholesale_cost-ws_ext_discount_amt+ws_ext_sales_price, 0)/2) year_total
          , count_big(*) AS cb
FROM dbo.customer
     ,dbo.web_sales
     ,dbo.date_dim
WHERE c_customer_sk = ws_bill_customer_sk
   AND ws_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year

```
Özgün sorgunun yürütme planını yeniden denetleyin.  Şimdi birleştirme sayısı 17'den 5'e değişiyor ve artık bir değişiklik yok.  Plandaki Filtre işlemi simgesine tıklayın, Çıktı Listesi verilerin temel tablolar yerine maddelendirilmiş görünümlerden okunduğunu gösterir.  

 ![Plan_Output_List_with_Materialized_Views](./media/performance-tuning-materialized-views/output-list.png)

Somutlaştırılmış görünümlerle, aynı sorgu herhangi bir kod değişikliği olmadan çok daha hızlı çalışır.  

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Data Warehouse geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).
