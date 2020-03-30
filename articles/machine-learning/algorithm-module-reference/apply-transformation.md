---
title: 'Dönüşüm Uygula: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de Daha önce hesaplanmış bir dönüştürmeye dayalı bir giriş veri kümesini değiştirmek için Azure Machine Learning'de Dönüşüm Uygula modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/05/2020
ms.openlocfilehash: ccf9d0c3eef50c7dfd838f1929e52506e8984879
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78395266"
---
# <a name="apply-transformation-module"></a>Dönüşüm modüllerini uygula

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Daha önce hesaplanmış bir dönüştürmeyi temel alan bir giriş veri kümesini değiştirmek için bu modülü kullanın.

Örneğin, **Verileri Normalleştir** modüllerini kullanarak eğitim verilerinizi normalleştirmek için z-skorları kullandıysanız, puanlama aşamasında da eğitim için hesaplanan z-puan değerini kullanmak isteyebilirsiniz. Azure Machine Learning'de, normalleştirme yöntemini dönüştürme olarak kaydedebilir ve ardından puanlamadan önce giriş verilerine z puanı uygulamak için **Dönüşüm** uygula'yı kullanarak dönüşüm ekleyebilirsiniz.

## <a name="how-to-save-transformations"></a>Dönüşümleri kaydetme

Tasarımcı, veri dönüşümlerini **veri kümeleri** olarak kaydetmenize olanak tanır, böylece bunları diğer ardışık alanlarda da kullanabilirsiniz.

1. Başarıyla çalışan bir veri dönüştürme modülü seçin.

1. Çıktılar **+ günlükler** sekmesini seçin.

1. **Sonuç Dönüşümünü**kaydetmek için **Kaydet simgesini** seçin.

## <a name="how-to-use-apply-transformation"></a>Dönüşüm Uygula nasıl kullanılır?  
  
1. Dönüşüm **Uygula** modüllerini ardınıza ekleyin. Bu modülü modül paletinin **Model Puanlama & Değerlendirme** bölümünde bulabilirsiniz. 
  
1. Modül paletinde **Datasets** > **My Datasets** altında kullanmak istediğiniz kaydedilen dönüşümü bulun.

1. Kaydedilen dönüşümün çıktısını **Dönüşüm Uygula** modülünün sol giriş bağlantı noktasına bağlayın.

    Veri kümesi, dönüşümün ilk tasarlandığı veri kümesiyle tam olarak aynı şema (sütun sayısı, sütun adları, veri türleri) olmalıdır.  
  
1. İstenilen modülün veri seti çıktısını Dönüşüm **Uygula** modülünün doğru giriş noktasına bağlayın.
  
1. Yeni veri kümesine dönüşüm uygulamak için ardışık lığı çalıştırın.  

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 