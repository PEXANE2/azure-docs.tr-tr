---
title: Saklı yordamları kullanma
description: SYNAPSE SQL havuzunda saklı yordamları uygulayarak çözüm geliştirmeye yönelik ipuçları.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/02/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 87c7eaa57f9da87bd83f89953afc09632d42b1f8
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213406"
---
# <a name="using-stored-procedures-in-synapse-sql-pool"></a>SYNAPSE SQL havuzunda saklı yordamları kullanma

Bu makalede, saklı yordamları uygulayarak SQL havuzu çözümlerini geliştirmeye yönelik ipuçları sunulmaktadır.

## <a name="what-to-expect"></a>Bekleneceğiniz

SQL havuzu SQL Server ' de kullanılan T-SQL özelliklerinin çoğunu destekler. Daha da önemlisi, çözümünüzün performansını en üst düzeye çıkarmak için kullanabileceğiniz genişleme özel özellikleri vardır.

Ayrıca, SQL havuzunun ölçeğini ve performansını korumanıza yardımcı olmak için davranış farklılıklarına sahip ek özellikler ve işlevler vardır.

## <a name="introducing-stored-procedures"></a>Saklı yordamları tanıtma

Saklı yordamlar SQL kodunuzu kapsüllemek için harika bir yoldur. Bu, SQL havuzu verilerinize yakın bir şekilde depolanır. Saklı yordamlar, geliştiricilerin kodu yönetilebilir birimlere kapsülleyerek ve daha fazla kod yeniden kullanılabilirliğini kolaylaştırarak çözümlerini modüler olarak ayarlamasını de yardımcı olur. Her saklı yordam aynı zamanda parametreleri daha da esnek hale getirmek için kabul edebilir.

SQL havuzu Basitleştirilmiş ve kolaylaştırılmış bir saklı yordam uygulamasını sağlar. SQL Server kıyasla en büyük fark, saklı yordamın önceden derlenmiş kod olmaması gerektiğidir.

Genel olarak, veri ambarlarında, derleme süresi, büyük veri birimlerine karşı sorgu çalıştırmak için gereken zamana kıyasla küçüktür. Saklı yordam kodunun büyük sorgular için doğru iyileştirildiğinden emin olmak daha önemlidir.

> [!TIP]
> Amaç, milisaniye değil, saat, dakika ve saniyeyi tasarrufu sağlamaktır. Bu nedenle, saklı yordamları SQL Logic Container olarak düşünmek yararlı olur.

Saklı yordamınız SQL havuzu tarafından yürütüldüğünde, SQL deyimleri ayrıştırılır, çevrilir ve çalışma zamanında iyileştirilir. Bu işlem sırasında her bir ifade dağıtılmış sorgulara dönüştürülür. Verilere karşı yürütülen SQL kodu, gönderilen sorgudan farklı.

## <a name="nesting-stored-procedures"></a>Saklı yordamları iç içe geçirme

Saklı yordamlar diğer saklı yordamları çağırdığında veya dinamik SQL yürütürde, iç saklı yordam veya kod çağırma iç içe geçmiş olarak kabul edilir.

SQL havuzu, en fazla sekiz iç içe geçme düzeyini destekler. Buna karşılık, SQL Server iç içe geçme düzeyi 32 ' dir.

Üst düzey saklı yordam çağrısı, iç içe düzey 1 ' e karşılık gelir.

```sql
EXEC prc_nesting
```

Saklı yordam aynı zamanda başka bir EXEC çağrısı de yapıyorsa, iç içe düzey iki olarak artar.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```

İkinci yordam daha sonra bazı dinamik SQL çalıştırırsa, iç içe düzeyi üç olarak artar.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

SQL havuzu şu anda [@ @NESTLEVEL ](/sql/t-sql/functions/nestlevel-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)öğesini desteklemiyor. Bu nedenle, iç içe düzeyi izlemeniz gerekir. Sekiz iç içe düzey sınırı aşmanız çok düşüktür. Ancak bunu yaparsanız, kodunuzu bu sınırın içinde iç içe geçmiş düzeylerine uyacak şekilde yeniden güncelleştirmeniz gerekir.

## <a name="insertexecute"></a>INSERT..EXECUTE

SQL havuzu, bir INSERT ifadesiyle saklı yordamın sonuç kümesini kullanmanıza izin vermez. Bununla birlikte, kullanabileceğiniz alternatif bir yaklaşım vardır. Bir örnek için [geçici tablolardaki](sql-data-warehouse-tables-temporary.md)makaleye bakın.

## <a name="limitations"></a>Sınırlamalar

SQL havuzunda uygulanmayan Transact-SQL saklı yordamlarının bazı yönleri şunlardır:

* geçici saklı yordamlar
* numaralandırılmış saklı yordamlar
* Genişletilmiş saklı yordamlar
* CLR saklı yordamları
* şifreleme seçeneği
* çoğaltma seçeneği
* tablo değerli parametreler
* salt okuma parametreleri
* Varsayılan parametreler
* Yürütme bağlamları
* Return deyimi

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için bkz. [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).
