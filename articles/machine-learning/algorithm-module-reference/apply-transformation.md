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
ms.date: 10/26/2020
ms.openlocfilehash: a5db3935ae445ee7dcf8129eb1d4c75fcb64302f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "96003914"
---
# <a name="apply-transformation-module"></a>Dönüştürme modülünü Uygula

Bu makalede Azure Machine Learning tasarımcısında bir modül açıklanmaktadır.

Bir giriş veri kümesini önceden hesaplanan bir dönüşüme göre değiştirmek için bu modülü kullanın. Bu modül, çıkarım işlem hatlarında dönüşümleri güncelleştirmeniz gerekiyorsa ' de gereklidir.

Örneğin, **normalize verileri** modülünü kullanarak eğitim verilerinizi normalleştirmek için z puanları kullandıysanız, Puanlama aşamasında eğitim için hesaplanmış z puanı değerini de kullanmak istersiniz. Azure Machine Learning ' de, normalleştirme yöntemini bir dönüşüm olarak kaydedebilir ve sonra, Puanlama yapmadan önce giriş verilerine z puanı uygulamak için **dönüştürme uygula** ' yı kullanabilirsiniz.

## <a name="how-to-save-transformations"></a>Dönüşümleri kaydetme

Tasarımcı veri dönüştürmelerini diğer işlem hatlarında kullanabilmeniz için veri **kümeleri** olarak kaydetmenizi sağlar.

1. Başarıyla çalışan bir veri dönüştürme modülü seçin.

1. **Çıktılar + Günlükler** sekmesini seçin.

1. Dönüştürme çıkışını bulun ve modül paletinde **veri kümeleri** kategorisi altına bir modül olarak kaydetmek için **kayıt veri kümesini** seçin.

## <a name="how-to-use-apply-transformation"></a>Dönüştürme uygula kullanımı  
  
1. İşlem hattınızda **dönüştürme modülünü Uygula** ' yı ekleyin. Bu modülü modül paleti 'nin **model puanlama & değerlendirme** bölümünde bulabilirsiniz. 
  
1. Modül paletindeki **veri kümeleri** altında kullanmak istediğiniz kaydedilen dönüşümü bulun.

1. Kaydedilen dönüşümün çıkışını, **uygulama dönüştürme** modülünün sol giriş bağlantı noktasına bağlayın.

    Veri kümesi, dönüştürmenin ilk tasarlandığı veri kümesiyle tam olarak aynı şemaya (sütun, sütun adı, veri türleri) sahip olmalıdır.  
  
1. İstenen modülün veri kümesi çıkışını, **uygulama dönüştürme** modülünün doğru giriş bağlantı noktasına bağlayın.
  
1. Yeni veri kümesine bir dönüşüm uygulamak için işlem hattını gönderebilirsiniz.  

> [!IMPORTANT]
> Eğitim işlem hatlarında güncelleştirilmiş dönüşümün, çıkarım işlem hatları 'nda da uygun olduğundan emin olmak için, eğitim ardışık düzeninde her değişiklik olduğunda aşağıdaki adımları izlemeniz gerekir:
> 1. Eğitim ardışık düzeninde, [sütun Seç dönüştürmesinin](select-columns-transform.md) çıkışını bir veri kümesi olarak kaydedin.
> ![Modül çıkışının veri kümesini Kaydet](media/module/select-columns-transform-register-dataset.png)
> 1. Çıkarım ardışık düzeninde, **TD** modülünü kaldırın ve önceki adımda kayıtlı veri kümesiyle değiştirin.
> ![TD modülünü Değiştir](media/module/replace-tranformation-directory.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 