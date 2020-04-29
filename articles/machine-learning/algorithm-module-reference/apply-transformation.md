---
title: 'Dönüştürme uygula: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Önceden hesaplanan bir dönüşüme göre giriş veri kümesini değiştirmek için Azure Machine Learning dönüştürme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/05/2020
ms.openlocfilehash: ccf9d0c3eef50c7dfd838f1929e52506e8984879
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78395266"
---
# <a name="apply-transformation-module"></a>Dönüştürme modülünü Uygula

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Bir giriş veri kümesini önceden hesaplanan bir dönüşüme göre değiştirmek için bu modülü kullanın.

Örneğin, **normalize verileri** modülünü kullanarak eğitim verilerinizi normalleştirmek için z puanları kullandıysanız, Puanlama aşamasında eğitim için hesaplanmış z puanı değerini de kullanmak istersiniz. Azure Machine Learning ' de, normalleştirme yöntemini bir dönüşüm olarak kaydedebilir ve sonra, Puanlama yapmadan önce giriş verilerine z puanı uygulamak için **dönüştürme uygula** ' yı kullanabilirsiniz.

## <a name="how-to-save-transformations"></a>Dönüşümleri kaydetme

Tasarımcı veri dönüştürmelerini diğer işlem hatlarında kullanabilmeniz için veri **kümeleri** olarak kaydetmenizi sağlar.

1. Başarıyla çalışan bir veri dönüştürme modülü seçin.

1. **Çıktılar + Günlükler** sekmesini seçin.

1. **Sonuç dönüşümünü**kaydetmek için **Kaydet simgesini** seçin.

## <a name="how-to-use-apply-transformation"></a>Dönüştürme uygula kullanımı  
  
1. İşlem hattınızda **dönüştürme modülünü Uygula** ' yı ekleyin. Bu modülü modül paleti 'nin **model puanlama & değerlendirme** bölümünde bulabilirsiniz. 
  
1. Modül **paletinde veri** > **kümelerim** altında kullanmak istediğiniz kaydedilen dönüşümü bulun.

1. Kaydedilen dönüşümün çıkışını, **uygulama dönüştürme** modülünün sol giriş bağlantı noktasına bağlayın.

    Veri kümesi, dönüştürmenin ilk tasarlandığı veri kümesiyle tam olarak aynı şemaya (sütun, sütun adı, veri türleri) sahip olmalıdır.  
  
1. İstenen modülün veri kümesi çıkışını, **uygulama dönüştürme** modülünün doğru giriş bağlantı noktasına bağlayın.
  
1. Yeni veri kümesine bir dönüşüm uygulamak için işlem hattını çalıştırın.  

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 