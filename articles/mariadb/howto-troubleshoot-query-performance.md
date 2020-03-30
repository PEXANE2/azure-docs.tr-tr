---
title: Sorun giderme sorgu performansı - MariaDB için Azure Veritabanı
description: MariaDB için Azure Veritabanı'nda sorgu performansını gidermek için EXPLAIN'ı nasıl kullanacağınızı öğrenin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: troubleshooting
ms.date: 3/18/2020
ms.openlocfilehash: b06fe37b63494eb4ee0ca680733a801c26415d67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530062"
---
# <a name="how-to-use-explain-to-profile-query-performance-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı'nda sorgu performansını profillemek için EXPLAIN nasıl kullanılır?
**EXPLAIN** sorguları en iyi duruma getirmek için kullanışlı bir araçtır. EXPLAIN deyimi, SQL deyimlerinin nasıl yürütüldedildiği hakkında bilgi almak için kullanılabilir. Aşağıdaki çıktı, bir EXPLAIN deyiminin yürütülmesine bir örnek gösterir.

```sql
mysql> EXPLAIN SELECT * FROM tb1 WHERE id=100\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 10.00
        Extra: Using where
```

Bu örnekten de görüleceği gibi, *anahtarın* değeri NULL'dur. Bu çıktı, MariaDB'nin sorgu için en iyi duruma getirilmiş dizinleri bulamadığı ve tam tablo taradığı anlamına gelir. Bu sorguyu **kimlik** sütununa bir dizin ekleyerek en iyi duruma getirelim.

```sql
mysql> ALTER TABLE tb1 ADD KEY (id);
mysql> EXPLAIN SELECT * FROM tb1 WHERE id=100\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ref
possible_keys: id
          key: id
      key_len: 4
          ref: const
         rows: 1
     filtered: 100.00
        Extra: NULL
```

Yeni EXPLAIN, MariaDB'nin artık satır sayısını 1 ile sınırlamak için bir dizin kullandığını ve bunun da arama süresini önemli ölçüde kısaltdığını gösterir.
 
## <a name="covering-index"></a>Kaplama dizini
Kaplama dizini, veri tablolarından değer alınmasını azaltmak için dizindeki bir sorgunun tüm sütunlarından oluşur. Aşağıda, GROUP **BY** deyiminde bir örnek ve bir örnek verged.
 
```sql
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using temporary; Using filesort
```

Çıktıdan da görüleceği gibi, Uygun dizinler bulunmadığından MariaDB herhangi bir dizin kullanmaz. Ayrıca *geçici kullanma gösterir; Dosya sıralamasını kullanarak,* MariaDB **GROUP BY** yan tümcesini karşılamak için geçici bir tablo oluşturur.
 
Tek başına **c2** sütununda dizin oluşturmak bir fark yaratmaz ve MariaDB'nin yine de geçici bir tablo oluşturması gerekir:

```sql 
mysql> ALTER TABLE tb1 ADD KEY (c2);
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using temporary; Using filesort
```

Bu durumda, hem **c1** hem de **c2** üzerinde kapalı bir **dizin** oluşturulabilir ve böylece daha fazla veri aramasını ortadan kaldırmak için **c2**" değerini doğrudan dizin olarak ekler.

```sql 
mysql> ALTER TABLE tb1 ADD KEY covered(c1,c2);
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: index
possible_keys: covered
          key: covered
      key_len: 108
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using index
```

Yukarıdaki EXPLAIN'ın da gösterdiği gibi, MariaDB artık kapsanan dizini kullanır ve geçici bir tablo oluşturmaktan kaçınır. 

## <a name="combined-index"></a>Birleşik dizin
Birleşik dizin, birden çok sütundan gelen değerlerden oluşur ve dizinlenmiş sütunların birleştirilmiş değerlerine göre sıralanmış bir dizi satır olarak kabul edilebilir.Bu yöntem **GROUP BY** deyiminde yararlı olabilir.

```sql
mysql> EXPLAIN SELECT c1, c2 from tb1 WHERE c2 LIKE '%100' ORDER BY c1 DESC LIMIT 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using filesort
```

MariaDB, özellikle çok sayıda satırı sıralamak zorunda kaldığında, oldukça yavaş bir *dosya sıralama* işlemi gerçekleştirir. Bu sorguen iyi duruma getirmek için, sıralanan her iki sütunda da birleştirilmiş dizin oluşturulabilir.

```sql 
mysql> ALTER TABLE tb1 ADD KEY my_sort2 (c1, c2);
mysql> EXPLAIN SELECT c1, c2 from tb1 WHERE c2 LIKE '%100' ORDER BY c1 DESC LIMIT 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: index
possible_keys: NULL
          key: my_sort2
      key_len: 108
          ref: NULL
         rows: 10
     filtered: 11.11
        Extra: Using where; Using index
```

EXPLAIN şimdi MariaDB dizin zaten sıralanmış olduğundan ek sıralama önlemek için birleşik dizin kullanabilirsiniz gösterir.
 
## <a name="conclusion"></a>Sonuç
 
EXPLAIN ve farklı türde Dizinler kullanmak performansı önemli ölçüde artırabilir. Tabloda bir dizin olması mutlaka MariaDB sorguları için kullanmak mümkün olacağı anlamına gelmez. EXPLAIN'ı kullanarak varsayımlarınızı her zaman doğrulayın ve dizinleri kullanarak sorgularınızı optimize edin.

## <a name="next-steps"></a>Sonraki adımlar
- En ilgili sorularınıza akran yanıtları bulmak veya yeni bir soru/yanıt göndermek için [MSDN forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDatabaseforMariadb) veya Stack Overflow'u ziyaret [edin.](https://stackoverflow.com/questions/tagged/azure-database-mariadb)
