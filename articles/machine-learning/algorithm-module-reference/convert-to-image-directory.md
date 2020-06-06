---
title: Görüntü dizinine Dönüştür
titleSuffix: Azure Machine Learning
description: Veri kümesini görüntü dizini biçimine dönüştürmek için görüntü dizinine dönüştürme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: dc40e0a644f692b397b1f2107b27b1d940d2b284
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84450636"
---
# <a name="convert-to-image-directory"></a>Görüntü dizinine Dönüştür

Bu makalede görüntü veri kümesini ' görüntü dizini ' veri türüne dönüştürmeye yardımcı olmak üzere görüntü veri kümesini ' resim dizinine dönüştürme ' veya görüntü sınıflandırması (Önizleme) gibi görüntü Azure Machine Learning sınıflandırması gibi görüntüyle ilgili görevlere yönelik standart veri biçimi olan ' görüntü dizini ' için nasıl kullanılacağı açıklanır.

## <a name="how-to-use-convert-to-image-directory"></a>Görüntü dizinine Dönüştür kullanımı  

1.  **Görüntü dizinine dönüştürme** modülünü denemenize ekleyin. Modül listesindeki ' Görüntü İşleme/Image Data Transformation ' kategorisinde bu modülü bulabilirsiniz. 

2.  Görüntü veri kümesini girdi olarak bağlayın. Lütfen giriş veri kümesinde resim olduğundan emin olun.
    Aşağıdaki veri kümesi biçimleri desteklenir:

    - Şu Uzantılarda sıkıştırılmış dosya: '. zip ', '. tar ', '. gz ', '. bz2 '.
    - Geçerli uzantılar üzerinde 1 sıkıştırılmış dosya içeren klasör. 
    - Görüntüleri içeren klasör.

    > [!NOTE]
    > Görüntü kategorisi, bu görüntü veri kümesi torchvision ımagefolder biçiminde düzenleniyorsa modül çıktısına kaydedilebilir, lütfen daha fazla bilgi için [torchvision veri kümelerine](https://pytorch.org/docs/stable/torchvision/datasets.html#imagefolder) bakın. Aksi takdirde, yalnızca görüntüler kaydedilir.

3.  İşlem hattını gönderme.

## <a name="results"></a>Sonuçlar

**Görüntü dizini modülüne dönüştürme** çıkışı görüntü dizini biçimindedir ve giriş bağlantı noktası biçiminin de görüntü dizini olduğu diğer görüntüyle ilgili modüllerle bağlantı yapılabilir.

## <a name="technical-notes"></a>Teknik notlar 

###  <a name="expected-inputs"></a>Beklenen girişler  

| Adı          | Tür                  | Açıklama   |
| ------------- | --------------------- | ------------- |
| Giriş veri kümesi | AnyDirectory, ZipFile | Giriş veri kümesi |

###  <a name="output"></a>Çıktı  

| Adı                   | Tür           | Açıklama            |
| ---------------------- | -------------- | ---------------------- |
| Çıkış resmi dizini | ImageDirectory | Çıkış resmi dizini |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
