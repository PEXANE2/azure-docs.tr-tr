---
title: 'Satır Ekle: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: İki veri kümesini biraraya getirmek için Azure Machine Learning'de Satır Ekle modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: cd9b5f8f182c4deab746d2c41e516a6ac23fb7aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477740"
---
# <a name="add-rows-module"></a>Satır Ekle modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

İki veri kümesini birleştirmek için bu modülü kullanın. Concatenation olarak, ikinci veri kümesinin satırları ilk veri kümesinin sonuna eklenir.  
  
Satırların birleşmesi aşağıdaki gibi senaryolarda yararlıdır:  
  
+ Bir dizi değerlendirme istatistikleri oluşturdunuz ve bunları daha kolay raporlama için tek bir tabloda birleştirmek istiyorsunuz.  
  
+ Farklı veri kümeleriyle çalışıyorsunuz ve son bir veri kümesi oluşturmak için veri kümelerini birleştirmek istiyorsunuz.  

## <a name="how-to-use-add-rows"></a>Satır Ekle nasıl kullanılır?  

Satırları iki veri kümesinden ayırmak için satırların tam olarak aynı şemaya sahip olması gerekir. Bu, aynı sayıda sütun ve sütunlarda aynı veri türü anlamına gelir.

1.  Satır **Ekle** modülünüzü ardınıza sürükleyin, **Veri Dönüşümü**altında bulabilirsiniz.

2. Veri kümelerini iki giriş bağlantı noktasına bağlayın. Eklemek istediğiniz veri kümesi ikinci (sağ) bağlantı noktasına bağlanmalıdır. 
  
3.  Boru hattını gönderin. Çıktı veri kümesindeki satır sayısı, her iki giriş veri kümesinin satırlarının toplamına eşit olmalıdır.

    **Satır Ekle** modülünün her iki girişine de aynı veri kümesini eklerseniz, veri kümesi çoğaltılır. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 