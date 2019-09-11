---
title: 'Veri birleştirin: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Veri kümelerini birleştirmek için Azure Machine Learning hizmetinde birleştirme verilerini birleştirme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: peterlu
ms.date: 06/01/2019
ms.openlocfilehash: 5fdb822a445e8a62cd36e6dd05aa218b9b41ce9e
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128732"
---
# <a name="join-data"></a>Verileri birleştirme

Bu makalede, bir veritabanı stili birleştirme işlemi kullanarak iki veri kümesini birleştirmek için Azure Machine Learning hizmeti görsel arabirimindeki **veri birleştirme** modülünün nasıl kullanılacağı açıklanır.  

## <a name="how-to-configure-join-data"></a>JOIN verileri nasıl yapılandırılır

İki veri kümesi üzerinde bir JOIN gerçekleştirmek için, anahtar sütunuyla ilişkili olmaları gerekir. Birden çok sütun kullanan bileşik anahtarlar da desteklenir. 

1. Birleştirmek istediğiniz veri kümelerini ekleyin ve sonra **birleştirme verileri** modülünü denemenize sürükleyin. 

    Modülü, **veri dönüştürme** kategorisinde, **düzenleme**altında bulabilirsiniz.

1. **Veri kümelerini veri JOIN** modülüne bağlayın. 
 
1. Anahtar sütun (ler) i seçmek için **sütun seçiciyi Başlat** ' ı seçin. Sol ve sağ girdilerin her ikisi için de sütun seçeceğini unutmayın.

    Tek bir anahtar için:

    Her iki giriş için tek bir anahtar sütun seçin.
    
    Bileşik anahtar için:

    Sol girişte bulunan tüm anahtar sütunlarını ve sağ girişi aynı sırayla seçin. Tüm anahtar sütunları eşleşiyorsa, **verileri Birleştir** modülü tabloları birleştirir. Sütun sırası orijinal tabloyla aynı değilse, **seçimdeki yinelemelere Izin ver ve sütun sırasını koru** seçeneğini işaretleyin. 

    ![Sütun seçici](media/module/join-data-column-selector.png)


1. Metin sütunu birleştirmesinden büyük/küçük harf duyarlılığı korumak istiyorsanız, büyük/küçük **harf** seçeneğini belirleyin. 
   
1. Veri kümelerinin nasıl birleştirildiğini belirtmek için **JOIN türü** açılan listesini kullanın.  
  
    * **Iç birleşim**: Bir *iç birleşim* en yaygın birleşim işlemidir. Yalnızca anahtar sütunlarının değerleri eşleşiyorsa Birleşik satırları döndürür.  
  
    * **Sol dış birleşim**: *Sol dış birleşim* , sol tablodaki tüm satırlar için birleştirilmiş satırları döndürür. Sol tablodaki bir satır sağ tabloda eşleşen satır içermiyorsa, döndürülen satırda doğru tablodan gelen tüm sütunlar için eksik değerler bulunur. Eksik değerler için de bir değiştirme değeri belirtebilirsiniz.  
  
    * **Tam dış birleşim**: *Tam dış birleşim* , sol tablodaki (**Table1**) ve sağ tablodaki (**Table2**) tüm satırları döndürür.  
  
         Herhangi bir tabloda, birbirleriyle eşleşen satırları olmayan her bir satır için, sonuç eksik değerler içeren bir satır içerir.  
  
    * **Sol yarı ekleme**: *Sol yarı JOIN* yalnızca, anahtar sütunlarının değerleri eşleşiyorsa sol tablodaki değerleri döndürür.  

1. **Birleşik tablodaki doğru anahtar sütunları tut**seçeneği için:

    * Her iki giriş tablolarından anahtarları görüntülemek için bu seçeneği belirleyin.
    * Yalnızca sol girdiden anahtar sütunları döndürmek için seçimi kaldırın.

1. Denemeyi çalıştırın veya verileri Birleştir modülünü seçin ve birleşimi gerçekleştirmek için seçili **Çalıştır** ' ı seçin.

1. Sonuçları görüntülemek için, **verileri** > Birleştir**sonuçları veri kümesi** > **Görselleştir**' e sağ tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 