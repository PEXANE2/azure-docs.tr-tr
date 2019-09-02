---
title: Azure Data Factory veri akışlarını eşleme ile sabit uzunluklu metin dosyalarını işle
description: Veri akışlarını eşleme kullanarak Azure Data Factory sabit uzunluklu metin dosyalarını nasıl işleyeceğini öğrenin.
services: data-factory
author: balakreshnan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 8/18/2019
ms.author: makromer
ms.openlocfilehash: e1ba09f459152616941071c23f7a6545ec2a8b73
ms.sourcegitcommit: d470d4e295bf29a4acf7836ece2f10dabe8e6db2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2019
ms.locfileid: "70210682"
---
# <a name="process-fixed-length-text-files-using-data-factory-mapping-data-flows"></a>Data Factory eşleme veri akışlarını kullanarak sabit uzunluklu metin dosyalarını işle

Data Factory eşleme veri akışları, sabit genişlikli metin dosyalarından dönüşüm verilerini destekler. Sınırlayıcı olmadan bir metin dosyası için bir veri kümesi tanımlayacaksınız ve sonra sıra konumuna göre alt dize bölmelerini ayarlayacaksınız.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. Yeni bir işlem hattı başlatmak için **+ Yeni Işlem hattı** 'na gidin

2. Sabit genişlikli dosyaları işlemek için kullanılacak bir veri akışı etkinliği ekleyin

  ![Sabit genişlikli işlem hattı](media/data-flow/fwpipe.png)

3. Veri akışı etkinliğinde yeni eşleme veri akışı ' nı seçin.

4. Kaynak dönüşümü, türetilmiş sütun, seçme ve havuz dönüştürme ekleme

  ![Sabit genişlikli veri akışı](media/data-flow/fw2.png)

5. Kaynak dönüşümünü, sınırlandırılmış metin türünde olacak yeni bir veri kümesi kullanacak şekilde yapılandırın

6. Sütun sınırlayıcısı ve üst bilgi yok ayarla

Bu dosya içerikleri için yalnızca alan başlangıç noktaları ve uzunlukları ayarlayacağız:

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

7. Kaynak dönüşümünüzün Izdüşüm sekmesinde "Column_1" adlı bir dize sütunu görmeniz gerekir

8. Artık türetilmiş sütununda yeni bir sütun oluşturun

9. Sütunları Sütun1 gibi basit adlara vereceğiz

10. Sonra ifade Oluşturucusu ' nda şunu yazın:

  ```substring(Column_1,1,4)```

  ![türetilmiş sütun](media/data-flow/fwderivedcol1.png)

10. Ayrıştırmak için gereken tüm sütunlar için bunu tekrarlayın

12. Oluşturulacak yeni sütunları görmek için Inceleme sekmesine tıklayın

  ![bilgiyi](media/data-flow/fwinspect.png)

13. Dönüştürme için ihtiyaç duymayacak sütunlardan hiçbirini kaldırmak için Seç dönüşümünü kullanın

  ![dönüşüm seçin](media/data-flow/fwselect.png)

14. Son olarak, verileri bir klasöre çıkarmak için havuz kullanın:

  ![Sabit genişlikli havuz](media/data-flow/fwsink.png)

  Çıktının şöyle görünmesi gerekir:

  ![Sabit genişlikli çıkış](media/data-flow/fxdoutput.png)

  Sabit genişlikli veriler artık dört karakterle bölünür ve Sütun1, Col2, Col3, col4,... ' e atanır. Yukarıdaki örneğe göre verileri 4 sütuna bölmem

## <a name="next-steps"></a>Sonraki adımlar

* Veri akışı [dönüşümlerini](concepts-data-flow-overview.md) eşleme kullanarak veri akışı mantığınızın geri kalanını oluşturun
