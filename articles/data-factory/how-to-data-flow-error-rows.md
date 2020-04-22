---
title: Azure Veri Fabrikası'nda veri akışlarını eşleme yle hata satırlarını işleme
description: Veri akışlarını eşlemeyi kullanarak Azure Veri Fabrikası'nda SQL kesilme hatalarını nasıl işleyeceğinizi öğrenin.
services: data-factory
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: makromer
ms.openlocfilehash: 8225143bb75118620b45c2520bb62ea30501a617
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732697"
---
# <a name="handle-sql-truncation-error-rows-in-data-factory-mapping-data-flows"></a>Veri Fabrikası eşleme veri akışlarında SQL kesilme hata satırlarını işleme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri akışlarını eşleme kullanırken Veri Fabrikası'nda sık karşılaşılan bir senaryo, dönüştürülmüş verilerinizi bir Azure SQL veritabanına yazmaktır. Bu senaryoda, karşı önlemeniz gereken yaygın bir hata koşulu olası sütun kesilmesidir. Hedef dize sütununa sığmayacak sütunların günlüğe kaydedilmesini sağlayarak veri akışınızın bu senaryolarda devam etmesini sağlamak için aşağıdaki adımları izleyin.

## <a name="scenario"></a>Senaryo

1. "Ad" adında bir sütuna ```nvarchar(5)``` sahip bir hedef Azure SQL veritabanı tablomuz var.

2. Veri akışımızın içinde, film başlıklarını lavabomuzdan hedef "ad" sütununa kadar eşlemek istiyoruz.

    ![Film veri akışı 1](media/data-flow/error4.png)
    
3. Sorun şu ki, film başlığı nın tümü yalnızca 5 karakter tutabilen bir lavabo sütununa sığmaz. Bu veri akışını çalıştırdığınızda, bunun gibi bir hata alırsınız:```"Job failed due to reason: DF-SYS-01 at Sink 'WriteToDatabase': java.sql.BatchUpdateException: String or binary data would be truncated. java.sql.BatchUpdateException: String or binary data would be truncated."```

Bu video, veri akışınızda hata satır işleme mantığının ayarlanmasına bir örnek üzerinden geçer:
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4uOHj]

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
