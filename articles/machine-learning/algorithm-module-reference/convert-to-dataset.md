---
title: "Dataset'e dönüştür: Modül başvurusu"
titleSuffix: Azure Machine Learning
description: Veri girişini Microsoft Azure Machine Learning tarafından kullanılan dahili veri kümesi biçimine dönüştürmek için Azure Machine Learning'de Veri Kümesine Dönüştür modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: fd45db779b7a828d247f09cae38f90fc20d12c16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456599"
---
# <a name="convert-to-dataset"></a>Veri kümesine dönüştürme

Bu makalede, bir boru hattı için herhangi bir veriyi tasarımcının iç biçimine dönüştürmek için Azure Machine Learning tasarımcısında (önizleme) Veri Kümesine Dönüştür modülünün nasıl kullanılacağı açıklanmaktadır.
  
Dönüşüm çoğu durumda gerekli değildir. Azure Machine Learning, veriler üzerinde herhangi bir işlem yapıldığında verileri dolaylı olarak yerel veri kümesi biçimine dönüştürür. 

Bir veri kümesinde bir tür normalleştirme veya temizleme işlemi gerçekleştirdiyseniz ve değişikliklerin diğer ardışık hatlar hatlarında kullanıldığından emin olmak istiyorsanız, verileri veri kümesi biçimine kaydetmenizi öneririz.  
  
> [!NOTE]
> Dataset'e dönüştürme yalnızca verilerin biçimini değiştirir. Çalışma alanında verilerin yeni bir kopyasını kaydetmez. Veri kümesini kaydetmek için çıktı bağlantı noktasını çift tıklatın, **veri kümesi olarak kaydet'i**seçin ve yeni bir ad girin.  
  
## <a name="how-to-use-convert-to-dataset"></a>Dataset'e Dönüştür nasıl kullanılır?  

Veri Kümesine Dönüştür'ü kullanmadan önce veri kümesini hazırlamak için [Meta Verileri Edit](edit-metadata.md) modüllerini kullanmanızı öneririz. Sütun adlarını ekleyebilir veya değiştirebilir, veri türlerini ayarlayabilir ve gerektiğinde başka değişiklikler yapabilirsiniz.

1.  Veri toniz aparatına Dönüştür modülünüzü ardınıza ekleyin. Bu modülü tasarımcıda **Veri dönüşümü** kategorisinde bulabilirsiniz. 

2. Veri kümesini oluşturan herhangi bir modüle bağlayın.   

    Veriler [tabular](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py)olduğu sürece, verileri bir veri kümesine dönüştürebilirsiniz. Bu, Alma [Verileri](import-data.md)aracılığıyla yüklenen verileri, [Verileri El Ile Girin](enter-data-manually.md)yoluyla oluşturulan verileri veya Dönüşüm [Uygula](apply-transformation.md)yoluyla dönüştürülmüş veri kümelerini içerir.

3.  **Eylem** açılır listesinde, veri kümesini kaydetmeden önce verilerüzerinde herhangi bir temizleme yapmak isteyip istemediğinizi belirtin:  
  
    - **Yok**: Verileri olduğu gibi kullanın.  
  
    - **SetMissingValue**: Veri kümesinde eksik bir değere belirli bir değer ayarlayın. Varsayılan yer tutucu soru işareti karakteridir (?), ancak farklı bir değer girmek için **Özel eksik değer** seçeneğini kullanabilirsiniz. Örneğin, **Özel eksik değer**için **Taksi** girerseniz, veri kümesindeki Tüm **Taksi** örnekleri eksik değere değiştirilir.
  
    - **Değiştirilen Değerler**: Başka bir tam değerle değiştirilecek tek bir tam değeri belirtmek için bu seçeneği kullanın. **Değiştir** yöntemini ayarlayarak eksik değerleri veya özel değerleri değiştirebilirsiniz:

      - **Eksik**: Giriş veri kümesindeki eksik değerleri değiştirmek için bu seçeneği seçin. **Yeni Değer**için, eksik değerleri değiştirmek için değeri girin.
      - **Özel**: Giriş veri kümesinde özel değerleri değiştirmek için bu seçeneği belirleyin. **Özel değer**için, bulmak istediğiniz değeri girin. Örneğin, verileriniz eksik değerler `obs` için yer tutucu olarak kullanılan `obs`dizeyi içeriyorsa, . **Yeni değer**için, özgün dize ile değiştirmek için yeni değeri girin.
  
    **ReplaceValues** işleminin yalnızca tam eşleşmeler için geçerli olduğunu unutmayın. Örneğin, bu dizeleri etkilenmez: `obs.` `obsolete`, .  
 
  
5.  Boru hattını gönderin.  

## <a name="results"></a>Sonuçlar

+  Ortaya çıkan veri kümesini yeni bir adla kaydetmek için, modülün sağ panelindeki **Çıktılar** sekmesi altındaki simge **Kayıt veri kümesini** seçin.  
  
## <a name="technical-notes"></a>Teknik notlar  

-   Giriş olarak veri kümesi alan herhangi bir modül, CSV dosyasındaki veya TSV dosyasındaki verileri de alabilir. Herhangi bir modül kodu çalıştırılmadan önce, girişler önceden işlenir. Ön işleme, girişte Dataset modülüne Dönüştür'ü çalıştırmaya eşdeğerdir.  
  
-   SVMLight biçiminden veri kümesine dönüştüremezsiniz.  
  
-   Özel bir değiştirme işlemi belirtirken, arama ve değiştirme işlemi tam değerler için geçerlidir. Kısmi eşleşmeler yasaktır. Örneğin, 3'u -1 veya 33 ile değiştirebilirsiniz, ancak 35 gibi iki basamaklı bir sayıda 3'u değiştiremezsiniz.  
  
-   Özel değiştirme işlemleri için, sütunun geçerli veri türüne uymayan herhangi bir karakteri değiştirme olarak kullanırsanız, değiştirme sessizce başarısız olur.  

  
## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
