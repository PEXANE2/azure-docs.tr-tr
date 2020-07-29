---
title: Sorgu performansı sorunlarını giderme-MariaDB için Azure veritabanı
description: MariaDB için Azure veritabanı 'nda sorgu performansı sorunlarını gidermek için AÇıKLA ' yı nasıl kullanacağınızı öğrenin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: troubleshooting
ms.date: 3/18/2020
ms.openlocfilehash: ca9a74763715c5c68526ff3213a14d2148f5ad30
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83834314"
---
# <a name="how-to-use-explain-to-profile-query-performance-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda sorgu performansını profil oluşturma hakkında açıklama kullanma
**Açıkla** sorguları iyileştirmek için kullanışlı bir araçtır. AÇıKLA deyimi, SQL deyimlerinin nasıl yürütüldüğü hakkında bilgi almak için kullanılabilir. Aşağıdaki çıktıda bir açıklama ifadesinin yürütülmesi örneği gösterilmektedir.

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

Bu örnekte görünebilirler, *anahtarın* değeri null olur. Bu çıktı, MariaDB 'nin sorgu için en iyi duruma getirilmiş dizinleri bulamadığı ve tam tablo taraması gerçekleştirdiği anlamına gelir. **Kimlik** sütununa bir dizin ekleyerek bu sorguyu iyileştirelim.

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

Yeni AÇıKLA, MariaDB 'nin artık satır sayısını 1 olarak sınırlandırmak için bir dizin kullandığını gösterir; bu da arama süresini önemli ölçüde kısaltır.
 
## <a name="covering-index"></a>Kapsayan Dizin
Kapsayan Dizin, veri tablolarından değer alımını azaltmak için dizindeki bir sorgunun tüm sütunlarından oluşur. Aşağıdaki **Group By** deyimindeki bir çizim aşağıda verilmiştir.
 
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

Çıkışta görünebileceğinden, MariaDB hiçbir dizini kullanmaz çünkü uygun dizin yok. Ayrıca, *geçici kullanmayı da gösterir; Dosya sıralamayı kullanarak*, MariaDB, **Group By** yan tümcesini karşılamak için geçici bir tablo oluşturur.
 
Yalnızca **C2** sütununda bir dizin oluşturmak farklılık yapmaz ve MariaDB 'nin hala geçici bir tablo oluşturması gerekir:

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

Bu durumda, daha fazla veri aramasını ortadan kaldırmak için, **C1** ve **C2** ' de **Kapsanan bir dizin** oluşturulabilir ve bu değerin **C2**"değeri doğrudan dizinde eklenmesi

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

Yukarıdaki AÇıKLANMANıN gösterdiği gibi, MariaDB artık kapsanan dizini kullanmaktadır ve geçici bir tablo oluşturmaktan kaçınmaz. 

## <a name="combined-index"></a>Birleşik Dizin
Birleşik bir dizin birden fazla sütundan değerler içerir ve dizine alınmış sütunların değerlerini birleştirerek sıralanan bir satır dizisi olarak kabul edilebilir.Bu yöntem **Group By** ifadesinde yararlı olabilir.

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

MariaDB, özellikle de birçok satırı sıralamak gerektiğinde oldukça yavaş olan bir *Dosya sıralama* işlemi gerçekleştirir. Bu sorguyu iyileştirmek için, sıralanan her iki sütunda da birleştirilmiş bir dizin oluşturulabilir.

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

Şimdi AÇıKLA, MariaDB 'nin, dizin zaten sıralandığı için ek sıralama yapmaktan kaçınmak üzere Birleşik dizini kullanabildiğini gösterir.
 
## <a name="conclusion"></a>Sonuç
 
AÇıKLA ve farklı türde dizinlerin kullanılması performansı önemli ölçüde artırabilir. Tabloda bir dizin olması, MariaDB 'nin sorgular için kullanabilmesi anlamına gelmez. Her zaman varsayımlarınızı doğrulayın ve dizinleri kullanarak sorgularınızı iyileştirin.

## <a name="next-steps"></a>Sonraki adımlar
- En çok ilgili sorularınızın eş yanıtlarını bulmak veya yeni bir soru/cevap göndermek için [Microsoft Q&soru sayfası](https://docs.microsoft.com/answers/topics/azure-database-mariadb.html) veya [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mariadb)ziyaret edin.
