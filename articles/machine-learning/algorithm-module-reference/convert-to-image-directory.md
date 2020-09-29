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
ms.date: 09/28/2020
ms.openlocfilehash: 9f5f4b2b069ebc65430fba4bc31a9891ed61fedf
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91450109"
---
# <a name="convert-to-image-directory"></a>Görüntü Dizinine Dönüştürme

Bu makalede görüntü veri kümesini ' görüntü dizini ' veri türüne dönüştürmeye yardımcı olması için görüntü veri kümesinin nasıl kullanılacağı açıklanır. Azure Machine Learning tasarımcısında görüntü sınıflandırması gibi görüntüyle ilgili görevlerde standartlaştırılmış veri biçimi.

## <a name="how-to-use-convert-to-image-directory"></a>Görüntü dizinine Dönüştür kullanımı  

1.  **Görüntü dizinine Dönüştür** modülünü tuvale ekleyin. Modül listesindeki ' Görüntü İşleme/Image Data Transformation ' kategorisinde bu modülü bulabilirsiniz. 

2.  **Görüntü dizinine Dönüştür** modülünün girişi bir dosya veri kümesi olmalıdır. [Bir görüntü veri kümesini kaydedin](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets) ve modül giriş bağlantı noktasına bağlayın. Lütfen giriş veri kümesinde resim olduğundan emin olun. Şu anda tasarımcı Image veri kümesini görselleştirmeyi desteklemiyor.
 
    Aşağıdaki veri kümesi biçimleri desteklenir:

    - Şu Uzantılarda sıkıştırılmış dosya: '. zip ', '. tar ', '. gz ', '. bz2 '.
    - Görüntüleri içeren klasör. **Öncelikle bu gibi bir klasörü sıkıştırmak ve sonra sıkıştırılmış dosyayı veri kümesi olarak kullanmak önerilir**.

    > [!WARNING]
    > **Veri alma** modülünün çıkış türü yalnızca dosya yolu dizesini Içeren Dataframe dizini olduğundan, görüntü veri kümesini içeri aktarmak Için **veri alma** **modülünü kullanamazsınız.**
    

    > [!NOTE]
    > - Denetimli öğreniminde görüntü veri kümesini kullanıyorsanız eğitim veri kümesinin etiketini belirtmeniz gerekir.
    > - Görüntü sınıflandırma görevi için, bu görüntü veri kümesi torchvision ımagefolder biçiminde düzenleniyorsa, etiket modül çıkışında ' Category ' görüntüsü olarak oluşturulabilir. Aksi halde, etiket olmadan yalnızca görüntüler kaydedilir. Aşağıda, resim veri kümesini etiketi almak için nasıl düzenleyecağınız hakkında bir örnek verilmiştir. alt klasör adı olarak görüntü kategorisini kullanın. 
    > - Her bir kategori klasörüne aynı sayıda görüntü yüklemeniz gerekmez.
    > - Bu uzantılara sahip görüntüler (küçük harfle) desteklenir: '. jpg ', '. jpeg ', '. png ', '. ppm ', '. bmp ', '. PGM ', '. tif ', '. tiff ', '. webp '. Ayrıca, bir klasörde birden fazla görüntü türüne sahip olabilirsiniz.    
    > - Daha fazla bilgi için lütfen [torchvision veri kümelerine](https://pytorch.org/docs/stable/torchvision/datasets.html#imagefolder) bakın.
    >
    > ```
    > Your_image_folder_name/Category_1/xxx.png
    > Your_image_folder_name/Category_1/xxy.jpg
    > Your_image_folder_name/Category_1/xxz.jpeg
    >
    > Your_image_folder_name/Category_2/123.png
    > Your_image_folder_name/Category_2/nsdf3.png
    > Your_image_folder_name/Category_2/asd932_.png
    > ```
    > - Puanlama için görüntü veri kümesini kullanıyorsanız, Bu modülün giriş dosyası veri kümesi sınıflandırılmamış görüntüleri içermelidir.
    
3.  İşlem hattını gönderme. Bu modül GPU ya da CPU üzerinde çalıştırılabilir.

## <a name="results"></a>Sonuçlar

**Görüntü dizini modülüne dönüştürme** çıkışı görüntü dizini biçimindedir ve giriş bağlantı noktası biçiminin de görüntü dizini olduğu diğer görüntüyle ilgili modüllerle bağlantı yapılabilir.

## <a name="technical-notes"></a>Teknik notlar 

###  <a name="expected-inputs"></a>Beklenen girişler  

| Ad          | Tür                  | Açıklama   |
| ------------- | --------------------- | ------------- |
| Giriş veri kümesi | AnyDirectory, ZipFile | Giriş veri kümesi |

###  <a name="output"></a>Çıkış  

| Ad                   | Tür           | Açıklama            |
| ---------------------- | -------------- | ---------------------- |
| Çıkış resmi dizini | ImageDirectory | Çıkış resmi dizini |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
