---
title: 'Dönüştürme uygula: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Önceden hesaplanan bir dönüşüme göre giriş veri kümesini değiştirmek için Azure Machine Learning dönüştürme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: a48ce60dca9f4221e364d53567f5b53719deb18c
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76314530"
---
# <a name="apply-transformation-module"></a>Dönüştürme modülü Uygula

Bu makalede Azure Machine Learning tasarımcısında bir modül açıklanmaktadır.

Bir giriş veri kümesini önceden hesaplanan bir dönüşüme göre değiştirmek için bu modülü kullanın.  
  
Örneğin, **normalize verileri** modülünü kullanarak eğitim verilerinizi normalleştirmek için z puanları kullandıysanız, Puanlama aşamasında eğitim için hesaplanmış z puanı değerini de kullanmak istersiniz. Azure Machine Learning ' de, normalleştirme yöntemini bir dönüşüm olarak kaydedebilir ve sonra, Puanlama yapmadan önce giriş verilerine z puanı uygulamak için **dönüştürme uygula** ' yı kullanabilirsiniz.
  
Azure Machine Learning birçok farklı özel dönüştürme türü oluşturma ve uygulama desteği sağlar. Örneğin, dönüştürmeleri kaydetmek ve sonra yeniden kullanmak isteyebilirsiniz:  
  
- Eksik değerleri Temizleme, eksik **verileri silme** veya değiştirme
- Verileri **Normalleştir kullanarak verileri** normalleştirin
  

## <a name="how-to-use-apply-transformation"></a>Dönüştürme uygula kullanımı  
  
1. İşlem hattınızda **dönüştürme modülünü Uygula** ' yı ekleyin. Bu modülü, **puan** kategorisinde **Machine Learning**altında bulabilirsiniz. 
  
2. Giriş olarak kullanmak için mevcut bir dönüştürmeyi bulun.  Daha önce kaydedilen dönüşümler sol gezinti bölmesindeki **dönüşümler** grubunda bulunabilir.  
  
   
  
3. Dönüştürmek istediğiniz veri kümesini bağlayın. Veri kümesi, dönüştürmenin ilk tasarlandığı veri kümesiyle tam olarak aynı şemaya (sütun, sütun adı, veri türleri) sahip olmalıdır.  
  
4. Dönüştürme tanımlanırken tüm özelleştirmeler yapıldığından başka parametre ayarlanması gerekmez.  
  
5. Yeni veri kümesine bir dönüşüm uygulamak için işlem hattını çalıştırın.  

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 