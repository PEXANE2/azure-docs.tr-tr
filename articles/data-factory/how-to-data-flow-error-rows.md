---
title: Azure Veri Fabrikası'nda veri akışlarını eşleme yle hata satırlarını işleme
description: Veri akışlarını eşlemeyi kullanarak Azure Veri Fabrikası'nda SQL kesilme hatalarını nasıl işleyeceğinizi öğrenin.
services: data-factory
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/28/2019
ms.author: makromer
ms.openlocfilehash: 3fe3403ad06d82ba5ccd33d2718bf0e5eff64490
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73166544"
---
# <a name="handle-sql-truncation-error-rows-in-data-factory-mapping-data-flows"></a>Veri Fabrikası eşleme veri akışlarında SQL kesilme hata satırlarını işleme

Veri akışlarını eşleme kullanırken Veri Fabrikası'nda sık karşılaşılan bir senaryo, dönüştürülmüş verilerinizi bir Azure SQL veritabanına yazmaktır. Bu senaryoda, karşı önlemeniz gereken yaygın bir hata koşulu olası sütun kesilmesidir. Hedef dize sütununa sığmayacak sütunların günlüğe kaydedilmesini sağlayarak veri akışınızın bu senaryolarda devam etmesini sağlamak için aşağıdaki adımları izleyin.

## <a name="scenario"></a>Senaryo

1. "Ad" adında bir sütuna ```nvarchar(5)``` sahip bir hedef Azure SQL veritabanı tablomuz var.

2. Veri akışımızın içinde, film başlıklarını lavabomuzdan hedef "ad" sütununa kadar eşlemek istiyoruz.

    ![Film veri akışı 1](media/data-flow/error4.png)
    
3. Sorun şu ki, film başlığı nın tümü yalnızca 5 karakter tutabilen bir lavabo sütununa sığmaz. Bu veri akışını çalıştırdığınızda, bunun gibi bir hata alırsınız:```"Job failed due to reason: DF-SYS-01 at Sink 'WriteToDatabase': java.sql.BatchUpdateException: String or binary data would be truncated. java.sql.BatchUpdateException: String or binary data would be truncated."```

## <a name="how-to-design-around-this-condition"></a>Bu durum etrafında nasıl tasarlar

1. Bu senaryoda, "ad" sütununun en büyük uzunluğu beş karakterdir. Bu nedenle, beş karakterden daha uzun olan "başlıklar" içeren satırları kaydetmemize olanak tanıyan koşullu bölünmüş dönüşüm eklerken, aynı zamanda bu alana sığabilecek diğer satırların veritabanına yazılmasına izin verelim.

    ![koşullu bölünme](media/data-flow/error1.png)

2. Bu koşullu bölme dönüşümü, "başlık"ın en büyük uzunluğunu beş olarak tanımlar. Beşten az veya beşe eşit olan ```GoodRows``` herhangi bir satır akışa gider. Beşten büyük olan herhangi bir ```BadRows``` satır akışa gider.

3. Şimdi başarısız olan satırları kaydetmemiz gerekiyor. Günlüğe kaydetme için ```BadRows``` akışa bir lavabo dönüşümü ekleyin. Burada, tüm alanların "otomatik eşlenimi" böylece tüm işlem kaydının oturumlarını kaydedeceğiz. Bu, Blob Depolama'daki tek bir dosyaya metin sınırlandıran bir CSV dosya çıkışıdır. Günlük dosyasına "badrows.csv" diyeceğiz.

    ![Kötü satırlar](media/data-flow/error3.png)
    
4. Tamamlanan veri akışı aşağıda gösterilmiştir. Artık SQL kesme hatalarını önlemek ve bu girişleri bir günlük dosyasına koymak için hata satırlarını bölebiliyoruz. Bu arada, başarılı satırlar hedef veritabanımıza yazmaya devam edebilirsiniz.

    ![tam veri akışı](media/data-flow/error2.png)

## <a name="next-steps"></a>Sonraki adımlar

* Veri akışları [dönüşümlerini](concepts-data-flow-overview.md)eşleme kullanarak veri akışı mantığınızın geri kalanını oluşturun.
