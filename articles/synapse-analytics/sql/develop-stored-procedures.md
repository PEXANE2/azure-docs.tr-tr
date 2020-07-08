---
title: Saklı yordamları kullanma
description: Çözümleri geliştirmek için SYNAPSE SQL havuzunda (veri ambarı) saklı yordamları uygulamaya yönelik ipuçları.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 0f88b994104ed8a2d80fb3b16f125f8a087cbe3c
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85958387"
---
# <a name="use-stored-procedures-in-sql-pool"></a>SQL havuzunda saklı yordamları kullanma

Çözümleri geliştirmek için SYNAPSE SQL havuzunda (veri ambarı) saklı yordamları uygulamaya yönelik ipuçları.

## <a name="what-to-expect"></a>Bekleneceğiniz

SQL havuzu SQL Server ' de kullanılan T-SQL özelliklerinin çoğunu destekler. Daha da önemlisi, çözümünüzün performansını en üst düzeye çıkarmak için kullanabileceğiniz genişleme özel özellikleri vardır.

Ancak, SQL havuzunun ölçeğini ve performansını korumak için, davranış farklılıkları ve bazıları desteklenmeyen bazı özellikler ve işlevler de vardır.

## <a name="introducing-stored-procedures"></a>Saklı yordamları tanıtma

Saklı yordamlar, SQL kodunuzu kapsüllemek için harika bir yoldur; veri ambarındaki verilerinize yakın bir şekilde depolanın. Saklı yordamlar, geliştiricilerin kodu yönetilebilir birimlere kapsülleyerek çözümlerini modüler hale getirmenize yardımcı olur; kodun daha fazla yeniden kullanılabilirliğini kolaylaştırın. Her saklı yordam aynı zamanda parametreleri daha da esnek hale getirmek için kabul edebilir.

SQL havuzu Basitleştirilmiş ve kolaylaştırılmış bir saklı yordam uygulamasını sağlar. SQL Server kıyasla en büyük fark, saklı yordamın önceden derlenmiş kod olmaması gerektiğidir. Veri ambarlarında, derleme süresi, büyük veri birimlerine karşı sorgu çalıştırmak için gereken zamana kıyasla küçüktür. Saklı yordam kodunun büyük sorgular için doğru iyileştirildiğinden emin olmak daha önemlidir. Amaç, milisaniye değil, saat, dakika ve saniyeyi tasarrufu sağlamaktır. Bu nedenle, saklı yordamları SQL Logic Container olarak düşünmek daha yararlıdır.

Saklı yordamınız SQL havuzu tarafından yürütüldüğünde, SQL deyimleri ayrıştırılır, çevrilir ve çalışma zamanında iyileştirilir. Bu işlem sırasında her bir ifade dağıtılmış sorgulara dönüştürülür. Verilere karşı yürütülen SQL kodu, gönderilen sorgudan farklı.

## <a name="nesting-stored-procedures"></a>Saklı yordamları iç içe geçirme

Saklı yordamlar diğer saklı yordamları çağırdığında veya dinamik SQL yürütürde, iç saklı yordam veya kod çağırma iç içe geçmiş olarak kabul edilir.

SQL havuzu, en fazla sekiz iç içe geçme düzeyini destekler. Bu SQL Server biraz farklıdır. SQL Server iç içe geçme düzeyi 32 ' dir.

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

> [!NOTE]
> SQL havuzu şu anda [@ @NESTLEVEL ](/sql/t-sql/functions/nestlevel-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)öğesini desteklemiyor. İç içe düzeyi izlemeniz gerekir. Sekiz iç içe düzey sınırı aşmanız çok düşüktür, ancak bunu yaparsanız, iç içe geçme düzeylerine bu sınırın sığması için kodunuzu yeniden güncelleştirmeniz gerekir.

## <a name="insertexecute"></a>INSERT..EXECUTE

SQL havuzu, bir INSERT ifadesiyle saklı yordamın sonuç kümesini kullanmanıza izin vermez. Ancak, kullanabileceğiniz alternatif bir yaklaşım vardır. Bir örnek için [geçici tablolardaki](develop-tables-temporary.md)makaleye bakın.

## <a name="limitations"></a>Sınırlamalar

SQL havuzunda uygulanmayan Transact-SQL saklı yordamlarının bazı yönleri vardır.

Bunlar:

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

Daha fazla geliştirme ipucu için bkz. [geliştirmeye genel bakış](develop-overview.md).
