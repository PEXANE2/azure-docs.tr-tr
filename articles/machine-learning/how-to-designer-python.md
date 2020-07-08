---
title: Tasarımcıda Python betiğini yürütme
titleSuffix: Azure Machine Learning
description: Verileri dönüştürmek için Azure Machine Learning tasarımcısında Python kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: peterclu
ms.author: peterlu
ms.date: 02/28/2020
ms.custom: designer, tracking-python
ms.openlocfilehash: 31028ba0417f312aa0caaf49cb4b2d432f89979f
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86041022"
---
# <a name="run-python-code-in-azure-machine-learning-designer"></a>Azure Machine Learning tasarımcısında Python kodu çalıştırma

Bu makalede, Azure Machine Learning tasarımcısına özel mantık eklemek için [Python betiği yürütme](algorithm-module-reference/execute-python-script.md) modülünü nasıl kullanacağınızı öğreneceksiniz. Aşağıdaki nasıl yapılır, basit özellik Mühendisliği yapmak için Pandas kitaplığını kullanırsınız.

Basit Python mantığını hızlıca eklemek için yerleşik kod düzenleyicisini kullanabilirsiniz. Daha karmaşık kod eklemek veya ek Python kitaplıklarını karşıya yüklemek istiyorsanız ZIP dosyası yöntemini kullanmanız gerekir.

Varsayılan yürütme ortamı, Python 'un Anauyumluluk dağıtımını kullanır. Önceden yüklenmiş paketlerin tam listesi için bkz. [Python betik modülü başvurusunu yürütme](algorithm-module-reference/execute-python-script.md) sayfası.

![Python giriş eşlemesini Yürüt](media/how-to-designer-python/execute-python-map.png)

## <a name="execute-python-written-in-the-designer"></a>Tasarımcıda yazılan Python yürütme

### <a name="add-the-execute-python-script-module"></a>Python betiği yürütme modülünü ekleme

1. Tasarımcı paletinde **Python betiği yürütme** modülünü bulun. **Python dili** bölümünde bulunabilir.

1. Modülün işlem hattı tuvaline sürükleyip bırakın.

### <a name="connect-input-datasets"></a>Giriş veri kümelerini bağlama

Bu makalede, **otomobil fiyat verileri (ham)** örnek veri kümesi kullanılmıştır. 

1. Veri kümenizi sürükleyin ve işlem hattı tuvaline bırakın.

1. Veri kümesinin çıkış bağlantı noktasını, **Python betiği Yürüt** modülünün sol üst giriş bağlantı noktasına bağlayın. Tasarımcı girişi giriş noktası betiğinin parametresi olarak kullanıma sunar.
    
    Doğru giriş bağlantı noktası daraltılmış Python kitaplıkları için ayrılmıştır.

    ![Veri kümelerini bağlama](media/how-to-designer-python/connect-dataset.png)
        

1. Kullandığınız giriş bağlantı noktasını bir yere göz atın. Tasarımcı, sol giriş bağlantı noktasını değişkenine `dataset1` ve ortadaki giriş bağlantı noktasını öğesine atar `dataset2` . 

Doğrudan **Python betik** modülünde veri oluşturabileceğiniz veya içeri aktarabileceğinden, giriş modülleri isteğe bağlıdır.

### <a name="write-your-python-code"></a>Python kodunuzu yazma

Tasarımcı, düzenlemeniz için bir başlangıç giriş noktası betiği sağlar ve kendi Python kodunuzu girersiniz. 

Bu örnekte, **her horseüs için dolar**, yeni bir sütun oluşturmak için, otomobil veri kümesinde, **Price** ve **gücünü**'da bulunan iki sütunu birleştirmek üzere Pandas kullanırsınız. Bu sütun her horseüs için ne kadar ücret Ödediğinize göre temsil eder. Bu, bir otomobil para için iyi bir anlaşma olup olmadığına karar veren yararlı bir özelliktir. 

1. **Python betik modülünü Yürüt** ' ü seçin.

1. Tuvalin sağında görüntülenen bölmede, **Python betiği** metin kutusunu seçin.

1. Aşağıdaki kodu kopyalayıp metin kutusuna yapıştırın.

    ```python
    import pandas as pd
    
    def azureml_main(dataframe1 = None, dataframe2 = None):
        dataframe1['Dollar/HP'] = dataframe1.price / dataframe1.horsepower
        return dataframe1
    ```
    İşlem hatlarınız aşağıdaki görüntüye bakmalıdır:
    
    ![Python işlem hattını yürütme](media/how-to-designer-python/execute-python-pipeline.png)

    Giriş noktası betiği işlevi içermelidir `azureml_main` . **Python betik modülünün yürütülmesi** için iki giriş bağlantı noktası ile eşlenen iki işlev parametresi vardır.

    Dönüş değeri bir Pandas Dataframe olmalıdır. Modül çıkışları olarak en fazla iki veri çerçevesine döndürebilirsiniz.
    
1. İşlem hattını gönderme.

Şimdi, bir otomobil öneren eğitiminde yararlı olabilecek yeni **ABD Doları/HP**özelliğini içeren bir veri kümeniz var. Bu, özellik ayıklama ve boyutlılık azaltmaya bir örnektir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning tasarımcısında [kendi verilerinizi nasıl içeri aktaracağınızı](how-to-designer-import-data.md) öğrenin.
