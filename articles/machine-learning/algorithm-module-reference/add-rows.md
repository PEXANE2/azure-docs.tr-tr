---
title: 'Satır ekleme: modül başvurusu'
titleSuffix: Azure Machine Learning
description: İki veri kümesini birleştirmek için Azure Machine Learning satır ekle modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 353ecc89e242e1d17dd83436f83afe77cc8c84ae
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90898974"
---
# <a name="add-rows-module"></a>Satır ekleme modülü

Bu makalede Azure Machine Learning tasarımcısında bir modül açıklanmaktadır.

İki veri kümesini birleştirmek için bu modülü kullanın. Birleştirme sırasında, ikinci veri kümesinin satırları ilk veri kümesinin sonuna eklenir.  
  
Satırları birleştirme, aşağıdaki gibi senaryolarda faydalıdır:  
  
+ Bir dizi değerlendirme istatistiği oluşturmuş ve bunları daha kolay raporlama için tek bir tabloda birleştirmek istiyorsunuz.  
  
+ Farklı veri kümeleriyle çalışıyorsunuz ve son veri kümesi oluşturmak için veri kümelerini birleştirmek istiyorsunuz.  

## <a name="how-to-use-add-rows"></a>Satır Ekle kullanma  

İki veri kümesinden satırları birleştirmek için, satırların tam olarak aynı şemaya sahip olması gerekir. Bu, aynı sayıda sütun ve sütunlarda aynı tür verilerin olması anlamına gelir.

1.  **Satır ekleme** modülünü işlem hattınızla sürükleyin, **veri dönüştürme**bölümünde bulabilirsiniz.

2. Veri kümelerini iki giriş bağlantı noktasına bağlayın. Eklemek istediğiniz veri kümesi ikinci (sağdaki) bağlantı noktasına bağlanmalıdır. 
  
3.  İşlem hattını gönderme. Çıktı veri kümesindeki satır sayısı, her iki giriş veri kümesinin satır toplamına eşit olmalıdır.

    Aynı veri kümesini **satır ekle** modülünün her iki girişe de eklerseniz, veri kümesi yinelenir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 