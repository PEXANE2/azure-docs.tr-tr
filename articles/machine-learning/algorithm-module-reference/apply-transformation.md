---
title: 'Dönüştürme uygula: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bir giriş veri kümesini daha önce hesaplanan bir dönüşüme göre değiştirmek için Azure Machine Learning hizmetine uygulama dönüştürme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 8b4ec9e71d9ce932a7b206467db91760f4a4fc47
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128976"
---
# <a name="apply-transformation-module"></a>Dönüştürme modülü Uygula

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Bir giriş veri kümesini önceden hesaplanan bir dönüşüme göre değiştirmek için bu modülü kullanın.  
  
Örneğin, **normalize verileri** modülünü kullanarak eğitim verilerinizi normalleştirmek için z puanları kullandıysanız, Puanlama aşamasında eğitim için hesaplanmış z puanı değerini de kullanmak istersiniz. Azure Machine Learning ' de, normalleştirme yöntemini bir dönüşüm olarak kaydedebilir ve sonra, Puanlama yapmadan önce giriş verilerine z puanı uygulamak için **dönüştürme uygula** ' yı kullanabilirsiniz.
  
Azure Machine Learning birçok farklı özel dönüştürme türü oluşturma ve uygulama desteği sağlar. Örneğin, dönüştürmeleri kaydetmek ve sonra yeniden kullanmak isteyebilirsiniz:  
  
- Eksik değerleri Temizleme, eksik **verileri silme** veya değiştirme
- Verileri Normalleştir kullanarak verileri normalleştirin
  

## <a name="how-to-use-apply-transformation"></a>Dönüştürme uygula kullanımı  
  
1. Deneme bilgisayarınıza **dönüştürme modülünü Uygula** ' yı ekleyin. Bu modülü, **puan** kategorisinde **Machine Learning**altında bulabilirsiniz. 
  
2. Giriş olarak kullanmak için mevcut bir dönüştürmeyi bulun.  Daha önce kaydedilen dönüşümler sol gezinti bölmesindeki **dönüşümler** grubunda bulunabilir.  
  
   
  
3. Dönüştürmek istediğiniz veri kümesini bağlayın. Veri kümesi, dönüştürmenin ilk tasarlandığı veri kümesiyle tam olarak aynı şemaya (sütun, sütun adı, veri türleri) sahip olmalıdır.  
  
4. Dönüştürme tanımlanırken tüm özelleştirmeler yapıldığından başka parametre ayarlanması gerekmez.  
  
5. Yeni veri kümesine bir dönüşüm uygulamak için denemeyi çalıştırın.  

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 