---
title: Görüntü Dizinine Dönüştürme
titleSuffix: Azure Machine Learning
description: Veri kümesini görüntü dizini biçimine dönüştürmek için görüntü dizinine dönüştürme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: 41724753df0d529e4c44344e8e975e68ee5eafd6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84904601"
---
# <a name="convert-to-image-directory"></a>Görüntü Dizinine Dönüştürme

Bu makalede görüntü veri kümesini ' görüntü dizini ' veri türüne dönüştürmeye yardımcı olmak üzere görüntü veri kümesini ' resim dizinine dönüştürme ' veya görüntü sınıflandırması (Önizleme) gibi görüntü Azure Machine Learning sınıflandırması gibi görüntüyle ilgili görevlere yönelik standart veri biçimi olan ' görüntü dizini ' için nasıl kullanılacağı açıklanır.

## <a name="how-to-use-convert-to-image-directory"></a>Görüntü dizinine Dönüştür kullanımı  

1.  **Görüntü dizinine dönüştürme** modülünü denemenize ekleyin. Modül listesindeki ' Görüntü İşleme/Image Data Transformation ' kategorisinde bu modülü bulabilirsiniz. 

2.  Görüntü veri kümesini girdi olarak bağlayın. Lütfen giriş veri kümesinde resim olduğundan emin olun.
    Aşağıdaki veri kümesi biçimleri desteklenir:

    - Şu Uzantılarda sıkıştırılmış dosya: '. zip ', '. tar ', '. gz ', '. bz2 '.
    - Görüntüleri içeren klasör. **Öncelikle bu gibi bir klasörü sıkıştırmak ve sonra sıkıştırılmış dosyayı veri kümesi olarak kullanmak önerilir**.

    > [!NOTE]
    > Denetimli öğreniminde görüntü veri kümesini kullanıyorsanız etiket gereklidir.
    > Görüntü sınıflandırma görevi için, bu görüntü veri kümesi torchvision ımagefolder biçiminde düzenleniyorsa, etiket modül çıkışında ' Category ' görüntüsü olarak oluşturulabilir. Aksi halde, etiket olmadan yalnızca görüntüler kaydedilir. Etiketi almak için görüntü veri kümesini nasıl düzenleyecağınız hakkında bir örnek olarak, görüntü kategorisini alt klasör adı olarak kullanın. Daha fazla bilgi için lütfen [torchvision veri kümelerine](https://pytorch.org/docs/stable/torchvision/datasets.html#imagefolder) bakın.
    >
    > ```
    > root/dog/xxx.png
    > root/dog/xxy.png
    > root/dog/xxz.png
    >
    > root/cat/123.png
    > root/cat/nsdf3.png
    > root/cat/asd932_.png
    > ```

3.  İşlem hattını gönderme.

## <a name="results"></a>Sonuçlar

**Görüntü dizini modülüne dönüştürme** çıkışı görüntü dizini biçimindedir ve giriş bağlantı noktası biçiminin de görüntü dizini olduğu diğer görüntüyle ilgili modüllerle bağlantı yapılabilir.

## <a name="technical-notes"></a>Teknik notlar 

###  <a name="expected-inputs"></a>Beklenen girişler  

| Name          | Tür                  | Açıklama   |
| ------------- | --------------------- | ------------- |
| Giriş veri kümesi | AnyDirectory, ZipFile | Giriş veri kümesi |

###  <a name="output"></a>Çıktı  

| Name                   | Tür           | Açıklama            |
| ---------------------- | -------------- | ---------------------- |
| Çıkış resmi dizini | ImageDirectory | Çıkış resmi dizini |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
