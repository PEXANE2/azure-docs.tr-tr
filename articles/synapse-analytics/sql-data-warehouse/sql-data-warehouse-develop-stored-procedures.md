---
title: Saklı yordamları kullanma
description: Çözümler geliştirmek için Azure SQL Veri Ambarı'nda depolanan yordamları uygulama ipuçları.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/02/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 83c3187c580bda33df8780a0e36f0fb9f2a4f484
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351553"
---
# <a name="using-stored-procedures-in-sql-data-warehouse"></a>SQL Veri Ambarı'nda depolanan yordamları kullanma
Çözümler geliştirmek için Azure SQL Veri Ambarı'nda depolanan yordamları uygulama ipuçları.

## <a name="what-to-expect"></a>Ne beklenebilir

SQL Veri Ambarı, SQL Server'da kullanılan T-SQL özelliklerinin çoğunu destekler. Daha da önemlisi, çözümünuzun performansını en üst düzeye çıkarmak için kullanabileceğiniz ölçeklendirilebilir özel özellikler vardır.

Ancak, SQL Veri Ambarı ölçeğini ve performansını korumak için davranış farklılıkları ve desteklenmeyen diğerleri olan bazı özellikler ve işlevler de vardır.


## <a name="introducing-stored-procedures"></a>Depolanan yordamları tanıtma
Depolanan yordamlar, SQL kodunuzu kapsüllemenin harika bir yoludur; verilerinize yakın bir yerde veri ambarında depolamak. Depolanan yordamlar, geliştiricilerin kodu yönetilebilir birimlere kapsülleyerek çözümlerini modüle etmelerine yardımcı olur; kodun daha fazla yeniden kullanılabilirliğini kolaylaştırır. Depolanan her yordam, parametreleri daha da esnek hale getirmek için de kabul edebilir.

SQL Veri Ambarı basitleştirilmiş ve basitleştirilmiş bir saklı yordam uygulaması sağlar. SQL Server ile karşılaştırıldığında en büyük fark, depolanan yordamın önceden derlenmiş kod olmamasıdır. Veri ambarlarında, derleme süresi, sorguları büyük veri hacimlerine karşı çalıştırmak için gereken süreyle karşılaştırıldığında küçüktür. Depolanan yordam kodunun büyük sorgular için doğru şekilde optimize edilmesini sağlamak daha önemlidir. Amaç milisaniye değil, saat, dakika ve saniye kaydetmektir. Bu nedenle, depolanan yordamları SQL mantığı için kapsayıcılar olarak düşünmek daha yararlıdır.     

SQL Veri Ambarı depolanan yordamınızı çalıştırdığında, SQL deyimleri ayrıştı, çevrilmiş ve çalışma zamanında optimize edilir. Bu işlem sırasında, her deyim dağıtılmış sorgulara dönüştürülür. Verilere karşı yürütülen SQL kodu gönderilen sorgudan farklıdır.

## <a name="nesting-stored-procedures"></a>İç içe depolanan yordamlar
Depolanan yordamlar diğer depolanan yordamları aradığında veya dinamik SQL'i çalıştırdığında, iç depolanan yordamın veya kod çağırmanın iç içe olduğu söylenir.

SQL Veri Ambarı en fazla sekiz iç içe geçme düzeyi destekler. Bu, SQL Server'dan biraz farklıdır. SQL Server'daki yuva düzeyi 32'dir.

Üst düzey depolanan yordam çağrısı yuva düzeyi 1 eşittir.

```sql
EXEC prc_nesting
```
Depolanan yordam da başka bir EXEC çağrısı yaparsa, yuva düzeyi ikiye yükselir.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
İkinci yordam daha sonra bazı dinamik SQL yürütürse, iç içe düzey üçe yükselir.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Not, SQL Veri Ambarı şu anda [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql)desteklemiyor. Yuva seviyesini izlemeniz gerekiyor. Sekiz yuva düzeyi sınırını aşmanız olası değildir, ancak bunu yaparsanız, iç içe geçme düzeylerini bu sınıra sığdırmak için kodunuzu yeniden çalışmanız gerekir.

## <a name="insertexecute"></a>Ekle.. Yürütmek
SQL Veri Ambarı, depolanan yordamın sonuç kümesini insert deyimiyle tüketmenize izin vermez. Ancak, kullanabileceğiniz alternatif bir yaklaşım vardır. Örneğin, [geçici tablolardaki](sql-data-warehouse-tables-temporary.md)makaleye bakın. 

## <a name="limitations"></a>Sınırlamalar
Transact-SQL depolanan yordamlarının SQL Veri Ambarı'nda uygulanmayan bazı yönleri vardır.

Bunlar:

* geçici saklı yordamlar
* numaralı saklı yordamlar
* genişletilmiş depolanmış yordamlar
* CLR saklanan yordamlar
* şifreleme seçeneği
* çoğaltma seçeneği
* tablo değerli parametreleri
* salt okunur parametreler
* varsayılan parametreler
* yürütme bağlamları
* Return deyimi

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.

