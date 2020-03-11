---
title: 'Python betiğini Yürüt: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Python kodunu çalıştırmak için Azure Machine Learning Python betiğini yürütme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/10/2020
ms.openlocfilehash: 52eb3bdb463389d075421661610b5ee94d14d77d
ms.sourcegitcommit: b8d0d72dfe8e26eecc42e0f2dbff9a7dd69d3116
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79037072"
---
# <a name="execute-python-script-module"></a>Python betik modülünü Yürüt

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Python kodunu çalıştırmak için bu modülü kullanın. Python 'un mimari ve tasarım ilkeleri hakkında daha fazla bilgi için [aşağıdaki makaleye](https://docs.microsoft.com/azure/machine-learning/machine-learning-execute-python-scripts)bakın.

Python ile Şu anda mevcut modüller tarafından desteklenmeyen görevleri gerçekleştirebilirsiniz:

+ `matplotlib` kullanarak verileri görselleştirme
+ Çalışma alanınızdaki veri kümelerini ve modelleri numaralandırmak için Python kitaplıklarını kullanma
+ [Verileri Içeri aktarma](./import-data.md) modülü tarafından desteklenmeyen kaynaklardaki verileri okuma, yükleme ve düzenleme
+ Kendi derin öğrenme kodunuzu çalıştırın 


Azure Machine Learning, Python 'un, veri işleme için birçok yaygın yardımcı programını içeren Anaconda dağıtımını kullanır. Anaconda sürümünü otomatik olarak güncelleştireceğiz. Geçerli sürüm:
 -  Python 3,6 için Anaconda 4.5 + dağıtımı 

Önceden yüklenmiş paketler şunlardır:
-  asn1crypto = = 0.24.0
- attrs = = 19.1.0
- Azure-ortak = = 1.1.18
- Azure-Storage-blob = = 1.5.0
- Azure-Storage-ortak = = 1.4.0
- certifi = = 2019.3.9
- cffi = = 1.12.2
- chardet = = 3.0.4
- şifreleme = = 2.6.1
- disden = = 1.4.0
- IDNA = = 2.8
- jsonschema = = 3.0.1
- lightgbm = = 2.2.3
- more-ıtertools = = 6.0.0
- sayısal tuş takımı = = 1.16.2
- Pandas = = 0.24.2
- Pillow = = 6.0.0
- PIP = = 19.0.3
- pyarrow = = 0.12.1
- pycparser = = 2.19
- pycryptodomex = = 3.7.3
- pyrsistent = = 0.14.11
- Python-dateutil = = 2.8.0
- pytz = = 2018.9
- istekler = = 2.21.0
- scikit-öğren = = 0.20.3
- SciPy = = 1.2.1
- setuptools = = 40.8.0
- altı = = 1.12.0
- Torch = = 1.0.1. post2
- torchvision = = 0.2.2. post3
- urllib3 = = 1.24.1
- tekerlek = = 0.33.1 

 Önceden yüklenmiş listede olmayan diğer paketleri yüklemek için, örneğin *scikit-misc*, aşağıdaki kodu betiğe ekleyin: 

 ```python
import os
os.system(f"pip install scikit-misc")
```

## <a name="upload-files"></a>Dosyaları karşıya yükleme
**Execute Python betiği** , [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#upload-file-name--path-or-stream-)kullanılarak dosyaların yüklenmesini destekler.

Aşağıdaki örnek, **Python betik** modülündeki bir görüntü dosyasının nasıl karşıya yükleneceğini göstermektedir:

```Python

# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# imports up here can be used to
import pandas as pd

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a pandas.DataFrame
#   Param<dataframe2>: a pandas.DataFrame
def azureml_main(dataframe1 = None, dataframe2 = None):

    # Execution logic goes here
    print(f'Input pandas.DataFrame #1: {dataframe1}')

    from matplotlib import pyplot as plt
    plt.plot([1, 2, 3, 4])
    plt.ylabel('some numbers')
    img_file = "line.png"
    plt.savefig(img_file)

    from azureml.core import Run
    run = Run.get_context(allow_offline=True)
    run.upload_file(f"graphics/{img_file}", img_file)

    # Return value must be of a sequence of pandas.DataFrame
    # E.g.
    #   -  Single return value: return dataframe1,
    #   -  Two return values: return dataframe1, dataframe2
    return dataframe1,
}
```

İşlem hattı başarıyla gönderildikten sonra, modülün sağ panelindeki resmin önizlemesini karşıya yükleyebilirsiniz ![resim](media/module/upload-image-in-python-script.png)

## <a name="how-to-configure-execute-python-script"></a>Python betiğini yürütme betiği nasıl yapılandırılır

**Execute Python betik** modülü, başlangıç noktası olarak kullanabileceğiniz örnek Python kodunu içerir. Python betik modülünü **Yürüt** ' ü yapılandırmak Için, **Python betiği** metin kutusunda yürütülecek bir giriş ve Python kodu kümesi sağlarsınız.

1. İşlem hattınızla **Python betiği yürütme** modülünü ekleyin.

2. Giriş için kullanmak istediğiniz tasarımcıdan **dataSet1** herhangi bir veri kümesini ekleyin ve bağlayın. Bu veri kümesine Python betiğinizdeki **DataFrame1**olarak başvurun.

    Veri kümesinin kullanımı isteğe bağlıdır, Python kullanarak veri oluşturmak istiyorsanız veya verileri doğrudan modüle aktarmak için Python kodu kullanın.

    Bu modül, **DataSet2**üzerindeki ikinci veri kümesinin eklenmesini destekler. Python betiğinizdeki ikinci veri kümesine DataFrame2 olarak başvurun.

    Azure Machine Learning depolanan veri kümeleri, bu modülle yüklendiğinde otomatik olarak **Pandas** Data. Frames 'e dönüştürülür.

    ![Python giriş eşlemesini Yürüt](media/module/python-module.png)

4. Yeni Python paketleri veya kodu eklemek için, bu özel kaynakları içeren daraltılmış dosyayı **betik paketi**'ne ekleyin. **Betik** paketine giriş, dosya türü veri kümesi olarak çalışma alanınıza yüklenmiş sıkıştırılmış bir dosya olmalıdır. 

    Karşıya yüklenen sıkıştırılmış arşivde bulunan herhangi bir dosya, işlem hattı yürütmesi sırasında kullanılabilir. Arşiv bir dizin yapısı içeriyorsa, yapı korunur, ancak **src** adlı bir dizini yola eklemek zorundasınız.

5. **Python betiği** metin kutusuna geçerli Python betiği yazın veya yapıştırın.

    **Python betiği** metin kutusu, açıklamalarda bazı yönergeler ve veri erişimi ve çıktısı için örnek kod ile önceden doldurulur. Bu kodu düzenlemeniz veya değiştirmeniz gerekir. Girintileme ve büyük küçük harf ile ilgili Python kurallarını izlediğinizden emin olun.

    + Betik, Bu modülün giriş noktası olarak `azureml_main` adlı bir işlev içermelidir.
    + Giriş noktası işlevi, en fazla iki giriş bağımsız değişkeni içerebilir: `Param<dataframe1>` ve `Param<dataframe2>`
    + Üçüncü giriş bağlantı noktasına bağlı daraltılmış dosyalar, `.\Script Bundle`, Python `sys.path`de eklenen dizininde sıkıştırıldı ve dizinde depolanmaktadır. 

    Bu nedenle, ZIP dosyanız `mymodule.py`içeriyorsa `import mymodule`kullanarak içeri aktarın.

    + İki veri kümesi, `pandas.DataFrame`türünde bir dizi olması gereken tasarımcıya döndürülür. Python kodunuzda başka çıktılar oluşturabilir ve bunları doğrudan Azure depolama 'ya yazabilirsiniz.

6. İşlem hattını çalıştırın veya modülü seçip yalnızca Python betiğini çalıştırmak için **Seçileni Çalıştır** ' a tıklayın.

    Tüm veriler ve kodlar bir sanal makineye yüklenir ve belirtilen Python ortamı kullanılarak çalıştırılır.

## <a name="results"></a>Sonuçlar

Katıştırılmış Python kodu tarafından gerçekleştirilen hesaplamaların sonuçlarının bir Pandas olarak sağlanması gerekir. Sonuçları, işlem hattındaki diğer modüllerle kullanabilmeniz için Azure Machine Learning veri kümesi biçimine otomatik olarak dönüştürülecek olan DataFrame.

Modül iki veri kümesi döndürür:  
  
+ Python betikteki ilk döndürülen Pandas dataframe tarafından tanımlanan **sonuçlar veri kümesi 1**

+ **Sonuç veri kümesi 2**, Python betiğinde ikinci döndürülen Pandas dataframe tarafından tanımlanır


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 