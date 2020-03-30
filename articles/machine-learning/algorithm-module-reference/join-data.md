---
title: 'Veriye Katılma: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: Veri kümelerini birleştirmek için Azure Machine Learning'de Veriye Katıl modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/19/2019
ms.openlocfilehash: 38606f424e38fc68519181f485b5b698d0705d6a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477587"
---
# <a name="join-data"></a>Verileri birleştirme

Bu makalede, veritabanı stili birleştirme işlemini kullanarak iki veri kümesini birleştirmek için Azure Machine Learning tasarımcısında (önizleme) **Veri birleştirme** modülünün nasıl kullanılacağı açıklanmaktadır.  

## <a name="how-to-configure-join-data"></a>Birleştirme Verilerini yapılandırma

İki veri kümesinde birleştirme gerçekleştirmek için, bunlar anahtar bir sütun la ilişkili olmalıdır. Birden çok sütun kullanarak bileşik tuşları da desteklenir. 

1. Birleştirmek istediğiniz veri kümelerini ekleyin ve ardından **Verileri Birleştir** modülünüzü ardınıza sürükleyin. 

    Modülü **Veri Dönüşümü** kategorisinde, **Manipülasyon**altında bulabilirsiniz.

1. Veri kümelerini **Veri birleştirme** modülüne bağlayın. 
 
1. Anahtar sütun(lar) seçmek için **Başlat sütun seçicisini** seçin. Hem sol hem de sağ girişler için sütun seçmeyi unutmayın.

    Tek bir anahtar için:

    Her iki giriş için de tek bir anahtar sütunu seçin.
    
    Bileşik anahtar için:

    Sol girişten tüm anahtar sütunları ve sağ girişi aynı sırada seçin. **Veriye Katıl** modülü, tüm anahtar sütunlar eşleştiğinde tablolara katılır. Sütun sırası özgün tabloyla aynı değilse, yinelenenlere izin ver seçeneğini işaretleyin **ve sütun sırasını seçimde koruyun.** 

    ![sütun seçici](media/module/join-data-column-selector.png)


1. Metin sütunu birleştirmede büyük/küçük harf duyarlılığını korumak **istiyorsanız, Büyük/Küçük Harf** seçeneğini belirleyin. 
   
1. Veri kümelerinin nasıl birleştirileceğini belirtmek için **Join türü** açılır listesini kullanın.  
  
    * **Inner Join**: *İç birleştirme* en yaygın birleştirme işlemidir. Yalnızca anahtar sütunların değerleri eşleştiğinde birleştirilmiş satırları döndürür.  
  
    * **Sol Dış Birleştirme**: Sol tablodaki tüm satırlar için *sol dış birleştirme,* birleştirilmiş satırları döndürür. Sol tablodaki bir satır sağ tabloda eşleşen satır yoksa, döndürülen satır sağ tablodan gelen tüm sütunlar için eksik değerler içerir. Eksik değerler için bir değiştirme değeri de belirtebilirsiniz.  
  
    * **Tam Dış Birleştirme**: *Tam dış birleştirme* sol tablodaki tüm satırları döndürür (**tablo1**) ve sağ tablodan **(tablo2).**  
  
         Her iki tablodaki diğerinde eşleşen satır olmayan satırların her biri için, sonuç eksik değerleri içeren bir satır içerir.  
  
    * **Sol Yarı Birleştirme**: *Sol yarı birleştirme,* anahtar sütunların değerleri eşleştiğinde yalnızca sol tablodaki değerleri döndürür.  

1. Seçenek için **sağ tuş sütunlarını birleştirilmiş tabloda tutun:**

    * Her iki giriş tablolarından tuşları görüntülemek için bu seçeneği belirleyin.
    * Yalnızca sol girişteki anahtar sütunları döndürmek için seçin.

1. Boru hattını gönderin.

1. Sonuçları görüntülemek için **Verileri Birleştir'e** sağ tıklayın ve **Görselleştir'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 