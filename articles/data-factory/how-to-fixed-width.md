---
title: Azure Veri Fabrikası'ndaki veri akışlarını eşlemeyle sabit uzunlukta metin dosyalarını işleme
description: Veri akışlarını eşlemeyi kullanarak Azure Veri Fabrikası'nda sabit uzunlukta metin dosyalarını nasıl işleyin gerektiğini öğrenin.
services: data-factory
author: balakreshnan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 8/18/2019
ms.author: makromer
ms.openlocfilehash: d6885e9b30cc71bda822a29574c4d574f2b020a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72387041"
---
# <a name="process-fixed-length-text-files-by-using-data-factory-mapping-data-flows"></a>Veri Fabrikası eşleme veri akışlarını kullanarak sabit uzunlukta metin dosyalarını işleme

Microsoft Azure Veri Fabrikası'ndaki veri akışlarını eşleyerek, verileri sabit genişlikteki metin dosyalarından dönüştürebilirsiniz. Aşağıdaki görevde, sınır tanımayan bir metin dosyası için bir veri kümesi tanımlarız ve ardından ordinal konuma göre alt dize bölmeleri ayarlarız.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. Yeni bir ardışık kanal oluşturmak için **+Yeni Ardışık Hatlar'ı** seçin.

2. Sabit genişlikli dosyaları işlemek için kullanılacak bir veri akışı etkinliği ekleyin:

    ![Sabit Genişlik boru hattı](media/data-flow/fwpipe.png)

3. Veri akışı etkinliğinde, **Yeni eşleme veri akışını**seçin.

4. Kaynak, Türetilmiş Sütun, Seç ve Büzerek dönüştürme ekleme:

    ![Sabit Genişlik veri akışı](media/data-flow/fw2.png)

5. Kaynak dönüşümünün, Sınırlı Metin türüne ait olacak yeni bir veri kümesini kullanacak şekilde yapılandırılması.

6. Sütun sınırlayıcı veya üstbilgi ayarlamayın.

   Şimdi bu dosyanın içeriği için alan başlangıç noktaları ve uzunlukları belirleyeceğiz:

    ```
    1234567813572468
    1234567813572468
    1234567813572468
    1234567813572468
    1234567813572468
    1234567813572468
    1234567813572468
    1234567813572468
    1234567813572468
    1234567813572468
    1234567813572468
    1234567813572468
    1234567813572468
    ```

7. Kaynak dönüşümünüzün **Projeksiyon** sekmesinde, *Column_1*adlı bir dize sütunu görmeniz gerekir.

8. Türetilmiş sütunda yeni bir sütun oluşturun.

9. Sütunlara *col1*gibi basit isimler vereceğiz.

10. İfade oluşturucuolarak, aşağıdakileri yazın:

    ```substring(Column_1,1,4)```

    ![türetilmiş sütun](media/data-flow/fwderivedcol1.png)

11. Ayrışdırmak için gereken tüm sütunlar için adım 10'u yineleyin.

12. Oluşturulacak yeni sütunları görmek için **Denetle** sekmesini seçin:

    ![Incelemek](media/data-flow/fwinspect.png)

13. Dönüştürme için gerek olmayan sütunlardan herhangi birini kaldırmak için Seç dönüşümünü kullanın:

    ![dönüşüm seçin](media/data-flow/fwselect.png)

14. Verileri bir klasöre çıktırmak için Lavabo'yu kullanın:

    ![sabit genişlikte lavabo](media/data-flow/fwsink.png)

    Çıktı şu şekilde görünür:

    ![sabit genişlik çıkışı](media/data-flow/fxdoutput.png)

  Sabit genişlikteki veriler artık dört karakterle bölünür ve Col1, Col2, Col3, Col4 ve benzeri ne atanır. Önceki örneğe göre, veriler dört sütuna bölünür.

## <a name="next-steps"></a>Sonraki adımlar

* Veri akışları [dönüşümlerini](concepts-data-flow-overview.md)eşleme kullanarak veri akışı mantığınızın geri kalanını oluşturun.
