---
title: Python
titleSuffix: Azure Machine Learning
description: Verileri dönüştürmek için Azure Machine Learning tasarımcısında Python'u nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: peterclu
ms.author: peterlu
ms.date: 02/28/2020
ms.openlocfilehash: a2bd9845cd29c7d139e2042f39b4697847639207
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455800"
---
# <a name="execute-python-code-in-azure-machine-learning-designer"></a>Azure Machine Learning tasarımcısında Python kodunu çalıştırma

Bu makalede, Azure Machine Learning tasarımcısına özel mantık eklemek için [Python Script](algorithm-module-reference/execute-python-script.md) modüllerini nasıl kullanacağınızı öğreneceksiniz. Aşağıdaki nasıl yapılacağını, basit özellik mühendisliği yapmak için Pandas kitaplığını kullanın.

Basit Python mantığını hızla eklemek için dahili kod düzenleyicisini kullanabilirsiniz. Daha karmaşık kod eklemek veya ek Python kitaplıkları yüklemek istiyorsanız, zip dosyası yöntemini kullanmanız gerekir.

Varsayılan yürütme ortamı Python Anacondas dağıtımını kullanır. Önceden yüklenmiş paketlerin tam listesi için [Python Script modül başvuru](algorithm-module-reference/execute-python-script.md) sayfasına bakın.

![Python giriş haritasını yürüt](media/how-to-designer-python/execute-python-map.png)

## <a name="execute-python-written-in-the-designer"></a>Tasarımcıda yazılan Python'u çalıştırın

### <a name="add-the-execute-python-script-module"></a>Execute Python Script modülekle

1. Designer paletinde Python Script modüllerini **çalıştır'ı** bulun. **Python Dili** bölümünde bulunabilir.

1. Modülü sürükleyin ve boru hattı tuvaline bırakın.

### <a name="connect-input-datasets"></a>Giriş veri kümelerini bağlama

Bu makalede örnek veri kümesi, **Otomobil fiyat verileri (Ham)** kullanır. 

1. Veri setinizi sürükleyin ve boru hattı tuvaline bırakın.

1. Veri kümesinin çıkış bağlantı noktasını **Python Script** modülünün sol üst giriş bağlantı noktasına bağlayın. Tasarımcı giriş noktası komut dosyasına bir parametre olarak giriş ortaya çıkarır.
    
    Doğru giriş bağlantı noktası sıkıştırılmış python kitaplıkları için ayrılmıştır.

    ![Veri kümelerini bağlama](media/how-to-designer-python/connect-dataset.png)
        

1. Hangi giriş bağlantı noktasını kullandığınıza dikkat edin. Tasarımcı, sol giriş bağlantı noktasını değişkene `dataset1` ve orta giriş `dataset2`bağlantı noktasına . 

Doğrudan **Execute Python Script** modülünde veri oluşturabileceğiniz veya içe aktarabileceğiniz için giriş modülleri isteğe bağlıdır.

### <a name="write-your-python-code"></a>Python kodunuzu yazın

Tasarımcı, kendi Python kodunuzu ve kodunuzu yeniden ve girebilmek için bir başlangıç giriş noktası komut dosyası sağlar. 

Bu örnekte, otomobil veri kümesi, **Fiyat** ve **Beygir gücü**bulunan iki sütun birleştirmek için Pandalar kullanın, yeni bir sütun oluşturmak için, **Beygir gücü başına Dolar**. Bu sütun, bir araba para için iyi bir anlaşma olup olmadığına karar vermek için yararlı bir özellik olabilir her beygir gücü için ne kadar ödeme temsil eder. 

1. Python Script modülünü **yürüt'ünü** seçin.

1. Tuvalin sağında görünen bölmede **Python komut dosyası** metin kutusunu seçin.

1. Aşağıdaki kodu metin kutusuna kopyalayıp yapıştırın.

    ```python
    import pandas as pd
    
    def azureml_main(dataframe1 = None, dataframe2 = None):
        dataframe1['Dollar/HP'] = dataframe1.price / dataframe1.horsepower
        return dataframe1
    ```
    Ardışık hattınız aşağıdaki resme bakmalıdır:
    
    ![Python ardışık hattını çalıştırma](media/how-to-designer-python/execute-python-pipeline.png)

    Giriş noktası komut dosyası `azureml_main`işlevi içermelidir. **Python Script** modülü için iki giriş bağlantı noktası için eşleyen iki işlev parametresi vardır.

    İade değeri bir Pandas Dataframe olmalıdır. Modül çıktıları olarak en fazla iki veri çerçevesi döndürebilirsiniz.
    
1. Boru hattını gönderin.

Şimdi, yeni özellik Dolar / **HP**ile bir veri seti var , bir araba tavsiye eğitimi yararlı olabilir. Bu özellik çıkarma ve boyutsallık azaltma bir örnektir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning [tasarımcısında kendi verilerinizi nasıl içe aktarılacınız](how-to-designer-import-data.md) öğrenin.