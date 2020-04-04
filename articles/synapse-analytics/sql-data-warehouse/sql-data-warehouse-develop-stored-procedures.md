---
title: Saklı yordamları kullanma
description: Synapse SQL havuzunda depolanan yordamları uygulayarak çözümler geliştirmek için ipuçları.
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
ms.openlocfilehash: 3283fbeec2226a825625b4e3ede6942a609ae723
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633449"
---
# <a name="using-stored-procedures-in-synapse-sql-pool"></a>Synapse SQL havuzunda depolanan yordamları kullanma

Bu makalede, depolanan yordamları uygulayarak SQL havuzu çözümleri geliştirmek için ipuçları sağlar.

## <a name="what-to-expect"></a>Ne beklenebilir

SQL havuzu, SQL Server'da kullanılan T-SQL özelliklerinin çoğunu destekler. Daha da önemlisi, çözümünuzun performansını en üst düzeye çıkarmak için kullanabileceğiniz ölçeklendirilebilir özel özellikler vardır.

Ayrıca, SQL havuzunun ölçeğini ve performansını korumanıza yardımcı olmak için davranış farklılıkları olan ek özellikler ve işlevler vardır.

## <a name="introducing-stored-procedures"></a>Depolanan yordamları tanıtma

Depolanan yordamlar, SQL havuz verilerinizin yakınında depolanan SQL kodunuzu kapsüllemek için harika bir yoldur. Depolanan yordamlar, geliştiricilerin kodu yönetilebilir birimlere kapsülleyerek çözümlerini modüle etmelerine yardımcı olur ve böylece kodun yeniden kullanılabilirliğini kolaylaştırır. Depolanan her yordam, parametreleri daha da esnek hale getirmek için de kabul edebilir.

SQL havuzu basitleştirilmiş ve basitleştirilmiş bir saklı yordam uygulaması sağlar. SQL Server ile karşılaştırıldığında en büyük fark, depolanan yordamın önceden derlenmiş kod olmamasıdır.

Genel olarak, veri ambarları için derleme süresi, sorguları büyük veri hacimlerine karşı çalıştırmak için gereken süreyle karşılaştırıldığında küçüktür. Depolanan yordam kodunun büyük sorgular için doğru şekilde optimize edilmesini sağlamak daha önemlidir.

> [!TIP]
> Amaç milisaniye değil, saat, dakika ve saniye kaydetmektir. Bu nedenle, depolanan yordamları SQL mantığı için kapsayıcılar olarak düşünmek yararlıdır.

SQL havuzu depolanan yordamınızı çalıştırdığında, SQL deyimleri ayrıştı, çevrilmiş ve çalışma zamanında optimize edilir. Bu işlem sırasında, her deyim dağıtılmış sorgulara dönüştürülür. Verilere karşı yürütülen SQL kodu gönderilen sorgudan farklıdır.

## <a name="nesting-stored-procedures"></a>İç içe depolanan yordamlar

Depolanan yordamlar diğer depolanan yordamları aradığında veya dinamik SQL'i çalıştırdığında, iç depolanan yordamın veya kod çağırmanın iç içe olduğu söylenir.

SQL havuzu en fazla sekiz iç içe geçme düzeyi destekler. Buna karşılık, SQL Server'daki yuva düzeyi 32'dir.

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

SQL havuzu şu anda [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)desteklemiyor. Bu nedenle, yuva düzeyini izlemeniz gerekir. Sekiz yuva düzeyi sınırını aşmanız pek olası değil. Ancak, bunu yaparsanız, iç içe geçme düzeylerine bu sınır adedine uyacak şekilde kodunuzu yeniden çalışmanız gerekir.

## <a name="insertexecute"></a>Ekle.. Yürütmek

SQL havuzu, insert deyimiyle depolanan yordamın sonuç kümesini tüketmenize izin vermez. Ancak, kullanabileceğiniz alternatif bir yaklaşım vardır. Örneğin, [geçici tablolardaki](sql-data-warehouse-tables-temporary.md)makaleye bakın.

## <a name="limitations"></a>Sınırlamalar

Sql havuzunda uygulanmayan Transact-SQL depolanmış yordamlarının bazı yönleri şunlardır:

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
