---
title: "DataSet 'e Dönüştür: modül başvurusu"
titleSuffix: Azure Machine Learning service
description: Veri girişini Microsoft Azure Machine Learning tarafından kullanılan iç veri kümesi biçimine dönüştürmek için Azure Machine Learning hizmetindeki veri kümesine dönüştürme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: b19d812d45b625531583aff726571c7da59f97f4
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73516259"
---
# <a name="convert-to-dataset"></a>Veri kümesine Dönüştür

Bu makalede, bir işlem hattındaki verileri tasarımcı tarafından kullanılan dahili biçime dönüştürmek için Azure Machine Learning Tasarımcısı 'nda (Önizleme) veri [kümesi dönüştürme](convert-to-dataset.md) modülünün nasıl kullanılacağı açıklanır.
  
Veri üzerinde herhangi bir işlem yapıldığında Azure Machine Learning örtük olarak verileri yerel veri kümesi biçimine dönüştürdüğünden, dönüştürme çoğu durumda gerekli değildir. 

Ancak, veri kümesinde bir dizi normalleştirme veya temizleme gerçekleştirdiyseniz ve değişikliklerin daha fazla işlem hatlarında kullanıldığından emin olmak istiyorsanız veri kümesi biçimine veri kaydedilmesi önerilir.  
  
> [!NOTE]
> Veri [kümesine dönüştürme](convert-to-dataset.md) yalnızca verilerin biçimini değiştirir. Çalışma alanındaki verilerin yeni bir kopyasını kaydetmez. Veri kümesini kaydetmek için, çıkış bağlantı noktasına çift tıklayın, **veri kümesi olarak kaydet**' i seçin ve yeni bir ad yazın.  
  
## <a name="how-to-use-convert-to-dataset"></a>Veri kümesine dönüştürme kullanma  

Veri kümesine [Dönüştür](convert-to-dataset.md)kullanmadan önce veri kümesini hazırlamak Için [meta verileri Düzenle](edit-metadata.md) modülünü kullanmanızı öneririz.  Sütun adları ekleyebilir veya değiştirebilir, veri türlerini ayarlayabilir ve benzeri bir şekilde yapabilirsiniz.

1.  [Veri kümesine dönüştürme](convert-to-dataset.md) modülünü ardışık düzene ekleyin. Bu modülü, tasarımcıda **veri dönüştürme** kategorisinde bulabilirsiniz. 

2. Veri kümesini çıkaran herhangi bir modüle bağlayın.   

    Veriler [tablosal](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py)olduğu sürece, veri kümesine dönüştürebilirsiniz. Bu, verileri [Içeri aktarma](import-data.md)kullanılarak yüklenen verileri, [verileri el ile gir](enter-data-manually.md)kullanılarak oluşturulan verileri veya [Uygula dönüşümü](apply-transformation.md)kullanılarak dönüştürülmüş veri kümelerini içerir.

3.  **Eylem** açılan listesinde, veri kümesini kaydetmeden önce verilerde herhangi bir temizlik yapmak istediğinizi belirtin:  
  
    - **Hiçbiri**: verileri olduğu gibi kullanın.  
  
    - **Setmissingvalue**: veri kümesinde eksik değere belirli bir değer ayarlayın. Varsayılan yer tutucu, soru işareti karakteridir (?), ancak farklı bir değer yazmak için **özel eksik değer** seçeneğini kullanabilirsiniz. Örneğin, **özel eksik değer**Için "Taxi" yazarsanız, veri kümesindeki tüm "TAXI" eksik değere değiştirilir.
  
    - **Replacevalues**: başka bir tam değer ile değiştirilmeleri için tek bir tam değer belirtmek için bu seçeneği kullanın.  **Replace** metodunu ayarlayarak eksik değerleri veya özel değerleri değiştirebilirsiniz:- **eksik**: giriş veri kümesindeki eksik değerleri değiştirmek için bu seçeneği belirleyin. **Yeni değer**için, eksik değerleri ile değiştirmek için değeri yazın.
            **özel**- : giriş veri kümesindeki özel değerleri değiştirmek için bu seçeneği belirleyin. **Özel değer**için bulmak istediğiniz değeri yazın. Örneğin, verilerinizin eksik değerler için yer tutucu olarak kullanılan dize `obs` içerdiğini varsayarsak `obs`yazmanız gerekir. **Yeni değer**için, özgün dizeyi ile değiştirilecek yeni değeri yazın.
  
    **Replacevalues** işleminin yalnızca tam eşleşmeler için geçerli olduğunu unutmayın. Örneğin, bu dizeler etkilenmemelidir: `obs.`, `obsolete`.  
 
  
5.  İşlem hattını çalıştırın veya [veri kümesine Dönüştür](convert-to-dataset.md) modülüne sağ tıklayıp **Seçileni Çalıştır**' ı seçin.  

## <a name="results"></a>Sonuçlar

+  Elde edilen veri kümesini yeni bir adla kaydetmek için, [veri kümesine Dönüştür](convert-to-dataset.md) çıktısına sağ tıklayın ve **veri kümesi olarak kaydet**' i seçin.  
  
## <a name="technical-notes"></a>Teknik notlar  

Bu bölümde, sık sorulan soruların uygulama ayrıntıları, ipuçları ve yanıtları yer almaktadır.

-   Giriş olarak bir veri kümesi alan tüm modüller CSV veya TSV içinde de veri alabilir. Herhangi bir modül kodu yürütülmeden önce girişlerin ön işlemesi gerçekleştirilir ve bu, girişte [veri kümesine Dönüştür](convert-to-dataset.md) modülüne çalıştırmaya eşdeğerdir.  
  
-   SVMLight biçiminden DataSet 'e dönüştüremezsiniz.  
  
-   Özel bir değiştirme işlemi belirtirken, arama ve değiştirme işlemi tüm değerler için geçerlidir; kısmi eşleştirmelere izin verilmiyor. Örneğin, 3 ' ü bir-1 veya 33 ile değiştirebilirsiniz, ancak 3 35 gibi iki basamaklı bir sayı içinde bir 3 ' ü değiştiremezsiniz.  
  
-   Özel değiştirme işlemleri için, sütunun geçerli veri türüyle uyumlu olmayan herhangi bir karakter değiştirme olarak kullanırsanız değiştirme işlemi sessizce başarısız olur.  

  
## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 
