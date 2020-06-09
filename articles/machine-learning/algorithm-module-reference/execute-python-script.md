---
title: 'Python betiğini Yürüt: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Python kodunu çalıştırmak için Azure Machine Learning Python betiğini yürütme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.custom: tracking-python
author: likebupt
ms.author: keli19
ms.date: 04/27/2020
ms.openlocfilehash: d25a738a76c955ee11f091bb0f8861bd21cc9f1d
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84555874"
---
# <a name="execute-python-script-module"></a>Python betik modülünü Yürüt

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Python kodunu çalıştırmak için bu modülü kullanın. Python 'un mimari ve tasarım ilkeleri hakkında daha fazla bilgi için [aşağıdaki makaleye](https://docs.microsoft.com/azure/machine-learning/machine-learning-execute-python-scripts)bakın.

Python ile Şu anda mevcut modüller tarafından desteklenmeyen görevleri gerçekleştirebilirsiniz:

+ Kullanarak verileri görselleştirme`matplotlib`
+ Çalışma alanınızdaki veri kümelerini ve modelleri numaralandırmak için Python kitaplıklarını kullanma
+ [Verileri Içeri aktarma](./import-data.md) modülü tarafından desteklenmeyen kaynaklardaki verileri okuma, yükleme ve düzenleme
+ Kendi derin öğrenme kodunuzu çalıştırın 


Azure Machine Learning, Python 'un, veri işleme için birçok yaygın yardımcı programını içeren Anaconda dağıtımını kullanır. Anaconda sürümünü otomatik olarak güncelleştireceğiz. Geçerli sürüm:
 -  Python 3,6 için Anaconda 4.5 + dağıtımı 

Önceden yüklenmiş paketler şunlardır:
-    adal = = 1.2.2
-    ApplicationInsights = = 0.11.9
-    attrs = = 19.3.0
-    Azure-ortak = = 1.1.25
-    Azure-Core = = 1.3.0
-    Azure-graphrbac = = 0.61.1
-    Azure-Identity = = 1.3.0
-    Azure-MGMT-Authorization = = 0.60.0
-    Azure-MGMT-containerregistry = = 2.8.0
-    Azure-MGMT-keykasası = = 2.2.0
-    Azure-MGMT-Resource = = 8.0.1
-    Azure-MGMT-depolama = = 8.0.0
-    Azure-Storage-blob = = 1.5.0
-    Azure-Storage-ortak = = 1.4.2
-    azureml-çekirdek = = 1.1.5.5
-    azureml-dataprep-Native = = 14.1.0
-    azureml-dataprep = = 1.3.5
-    azureml-varsayılanlar = = 1.1.5.1
-    azureml-tasarımcı-klasik-modüller = = 0.0.118
-    azureml-tasarımcı-çekirdek = = 0.0.31
-    azureml-tasarımcı-iç = = 0.0.18
-    azureml-model-Yönetim-SDK = = 1.0.1 B6. post1
-    azureml-işlem hattı-çekirdek = = 1.1.5
-    azureml-telemetri = = 1.1.5.3
-    geribağlantı noktaları. TempFile = = 1.0
-    backports. WeakRef = = 1.0. post1
-    boto3 = = 1.12.29
-    botocore = = 1.15.29
-    cachetools = = 4.0.0
-    certifi = = 2019.11.28
-    cffi = = 1.12.3
-    chardet = = 3.0.4
-    = = 7.1.1 öğesine tıklayın
-    cloudpickle = = 1.3.0
-    configparser = = 3.7.4
-    contextlib2 = = 0.6.0. post1
-    şifreleme = = 2.8
-    ok = = 0.10.0
-    Dill = = 0.3.1.1
-    disden = = 1.4.0
-    Docker = = 4.2.0
-    docutils = = 0.15.2
-    dotnetcore2 = = 2.1.13
-    Flask = = 1.0.3
-    fusekopyala = = 3.0.1
-    gensim = = 3.8.1
-    Google-api-Core = = 1.16.0
-    Google-auth = = 1.12.0
-    Google-Cloud-Core = = 1.3.0
-    Google-Cloud-Storage = = 1.26.0
-    Google-sürdürülebilir-Media = = 0.5.0
-    googleapis-ortak-prototips = = 1.51.0
-    gunic, = = 19.9.0
-    IDNA = = 2.9
-    imdengeli-öğren = = 0.4.3
-    iztarihi = = 0.6.0
-    ısdangerou = = 1.1.0
-    jeepney = = 0.4.3
-    jinja2 = = 2.11.1
-    jmespath = = 0.9.5
-    joblib = = 0.14.0
-    JSON-Logging-Kopyala = = 0.2
-    jsonpickle = = 1.3
-    jsonschema = = 3.0.1
-    kiwıolver = = 1.1.0
-    Liac-arff = = 2.4.0
-    lightgbm = = 2.2.3
-    markupsafe = = 1.1.1
-    Matplotlib = = 3.1.3
-    more-ıtertools = = 6.0.0
-    msal-Extensions = = 0.1.3
-    msal = = 1.1.0
-    msrest = = 0.6.11
-    msrestazure = = 0.6.3
-    NDG-httpsclient = = 0.5.1
-    nimbusml = = 1.6.1
-    sayısal tuş takımı = = 1.18.2
-    oauthlib = = 3.1.0
-    Pandas = = 0.25.3
-    pathSpec = = 0.7.0
-    PIP = = 20.0.2
-    portalocker = = 1.6.0
-    protoarabellek = = 3.11.3
-    pyarrow = = 0.16.0
-    pyasn1-modüller = = 0.2.8
-    pyasn1 = = 0.4.8
-    pycparser = = 2.20
-    pycryptodomex = = 3.7.3
-    pyjwt = = 1.7.1
-    pyopenssl = = 19.1.0
-    pyparsing = = 2.4.6
-    pyrsistent = = 0.16.0
-    Python-dateutil = = 2.8.1
-    pytz = = 2019.3
-    istekler-oauthlib = = 1.3.0
-    istekler = = 2.23.0
-    RSA = = 4.0
-    ruamel. YAML = = 0.15.89
-    s3transfer = = 0.3.3
-    scikit-öğren = = 0.22.2
-    SciPy = = 1.4.1
-    secretstorage = = 3.1.2
-    setuptools = = 46.1.1. post20200323
-    altı = = 1.14.0
-    Smart-Open = = 1.10.0
-    urllib3 = = 1.25.8
-    WebSocket-istemci = = 0.57.0
-    Werkzeug = = 0.16.1
-    tekerlek = = 0.34.2

 Önceden yüklenmiş listede olmayan diğer paketleri yüklemek için, örneğin *scikit-misc*, aşağıdaki kodu betiğe ekleyin: 

 ```python
import os
os.system(f"pip install scikit-misc")
```
> [!NOTE]
> İşlem hatlarınız birden çok yürütme Python betik modülü içeriyorsa ve önceden yüklenmiş listesinde olmayan aynı paketlere ihtiyaç duyuyorsanız, lütfen paketleri sırasıyla her bir modüle yüklemeniz gerekir. 

## <a name="upload-files"></a>Dosyaları karşıya yükleme
**Execute Python betiği** , [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#upload-file-name--path-or-stream-)kullanılarak dosyaların yüklenmesini destekler.

Aşağıdaki örnek, **Python betik** modülündeki bir görüntü dosyasının nasıl karşıya yükleneceğini göstermektedir:

```Python

# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# imports up here can be used to
import pandas as pd

# The entry point function must have two input arguments:
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

İşlem hattı çalıştırması tamamlandıktan sonra, modülün sağ panelinde görüntünün önizlemesini yapabilirsiniz

> [!div class="mx-imgBorder"]
> ![Karşıya yüklenen-görüntü](media/module/upload-image-in-python-script.png)

## <a name="how-to-configure-execute-python-script"></a>Python betiğini yürütme betiği nasıl yapılandırılır

**Execute Python betik** modülü, başlangıç noktası olarak kullanabileceğiniz örnek Python kodunu içerir. Python betik modülünü **Yürüt** ' ü yapılandırmak Için, **Python betiği** metin kutusunda yürütülecek bir giriş ve Python kodu kümesi sağlarsınız.

1. İşlem hattınızla **Python betiği yürütme** modülünü ekleyin.

2. Giriş için kullanmak istediğiniz tasarımcıdan **dataSet1** herhangi bir veri kümesini ekleyin ve bağlayın. Bu veri kümesine Python betiğinizdeki **DataFrame1**olarak başvurun.

    Veri kümesinin kullanımı isteğe bağlıdır, Python kullanarak veri oluşturmak istiyorsanız veya verileri doğrudan modüle aktarmak için Python kodu kullanın.

    Bu modül, **DataSet2**üzerindeki ikinci veri kümesinin eklenmesini destekler. Python betiğinizdeki ikinci veri kümesine DataFrame2 olarak başvurun.

    Azure Machine Learning depolanan veri kümeleri, bu modülle yüklendiğinde otomatik olarak **Pandas** Data. Frames 'e dönüştürülür.

    ![Python giriş eşlemesini Yürüt](media/module/python-module.png)

4. Yeni Python paketleri veya kodu eklemek için, bu özel kaynakları içeren daraltılmış dosyayı **betik paketi**'ne ekleyin. **Betik** paketine giriş, dosya türü veri kümesi olarak çalışma alanınıza yüklenmiş sıkıştırılmış bir dosya olmalıdır. **Veri kümesi varlık sayfasında** veri kümesini karşıya yükleyebilir ve veri kümesi modülünü tasarımcı yazma sayfasındaki sol modül ağacında bulunan **veri kümeleri** listesinden sürükleyip bırakabilirsiniz. 

    Karşıya yüklenen sıkıştırılmış arşivde bulunan herhangi bir dosya, işlem hattı yürütmesi sırasında kullanılabilir. Arşiv bir dizin yapısı içeriyorsa, yapı korunur, ancak **src** adlı bir dizini yola eklemek zorundasınız.

5. **Python betiği** metin kutusuna geçerli Python betiği yazın veya yapıştırın.

    > [!NOTE]
    > Lütfen komut dosyanızı yazarken çok dikkatli olun ve bildirilmeyen nesneler veya içeri aktarılmayan modüller kullanma gibi sözdizimi hatası olmadığından emin olun. Ayrıca önceden yüklenmiş modül listesine daha fazla dikkat ödeyin. Listelenmemiş modülleri içeri aktarmak için, komut dosyasına karşılık gelen paketleri
    >  ``` Python
    > import os
    > os.system(f"pip install scikit-misc")
    > ```
    
    **Python betiği** metin kutusu, açıklamalarda bazı yönergeler ve veri erişimi ve çıktısı için örnek kod ile önceden doldurulur. Bu kodu düzenlemeniz veya değiştirmeniz gerekir. Girintileme ve büyük küçük harf ile ilgili Python kurallarını izlediğinizden emin olun.

    + Betik, `azureml_main` Bu modül için giriş noktası olarak adlandırılan bir işlev içermelidir.
    + Giriş noktası işlevinin iki giriş bağımsız değişkeni olmalıdır: `Param<dataframe1>` ve `Param<dataframe2>` Bu bağımsız değişkenler betikte kullanılmıyor olsa bile.
    + Üçüncü giriş bağlantı noktasına bağlı daraltılmış dosyalar, `.\Script Bundle` aynı zamanda Python 'a eklenen dizininde sıkıştırıldı ve dizinde depolanmaktadır `sys.path` . 

    Bu nedenle, ZIP dosyanız içeriyorsa `mymodule.py` kullanarak dosyayı içeri aktarın `import mymodule` .

    + İki veri kümesi tasarımcıya döndürülebilir ve bu tür bir dizi olmalıdır `pandas.DataFrame` . Python kodunuzda başka çıktılar oluşturabilir ve bunları doğrudan Azure depolama 'ya yazabilirsiniz.

6. İşlem hattını gönderme veya modülü seçme ve yalnızca Python betiğini çalıştırmak için **Seçileni Çalıştır** ' a tıklayın.

    Tüm veriler ve kodlar bir sanal makineye yüklenir ve belirtilen Python ortamı kullanılarak çalıştırılır.

## <a name="results"></a>Sonuçlar

Katıştırılmış Python kodu tarafından gerçekleştirilen hesaplamaların sonuçlarının bir Pandas olarak sağlanması gerekir. Sonuçları, işlem hattındaki diğer modüllerle kullanabilmeniz için Azure Machine Learning veri kümesi biçimine otomatik olarak dönüştürülecek olan DataFrame.

Modül iki veri kümesi döndürür:  
  
+ Python betikteki ilk döndürülen Pandas dataframe tarafından tanımlanan **sonuçlar veri kümesi 1**

+ **Sonuç veri kümesi 2**, Python betiğinde ikinci döndürülen Pandas dataframe tarafından tanımlanır


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
