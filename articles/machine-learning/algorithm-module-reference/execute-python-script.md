---
title: 'Python Komut Dosyasini Çalıştır: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: Python kodunu çalıştırmak için Azure Machine Learning'de Python Script'i Çalıştır modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/10/2020
ms.openlocfilehash: 0f86d1ad03062797764af6a0d49beacaa3458a8f
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365557"
---
# <a name="execute-python-script-module"></a>Python Script modüllerini çalıştırın

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Python kodunu çalıştırmak için bu modülü kullanın. Python'un mimari ve tasarım ilkeleri hakkında daha fazla bilgi için [aşağıdaki makaleye](https://docs.microsoft.com/azure/machine-learning/machine-learning-execute-python-scripts)bakın.

Python ile, şu anda şu anda şu anda şu anda gibi varolan modüller tarafından desteklenmeyen görevleri gerçekleştirebilirsiniz:

+ Kullanarak verileri görselleştirme`matplotlib`
+ Çalışma alanınızdaki veri kümelerini ve modelleri sayısallandırmak için Python kitaplıklarını kullanma
+ [Alma Verileri](./import-data.md) modülü tarafından desteklenmeyen kaynaklardan gelen verileri okuma, yükleme ve işleme
+ Kendi derin öğrenme kodunuzu çalıştırın 


Azure Machine Learning, veri işleme için birçok yaygın yardımcı program içeren Python'un Anaconda dağıtımını kullanır. Anaconda sürümünü otomatik olarak güncelleceğiz. Geçerli sürüm:
 -  Python 3.6 için Anaconda 4.5+ dağılımı 

Önceden yüklenmiş paketler şunlardır:
-    adal==1.2.2
-    uygulamalar==0.11.9
-    attrs==19.3.0
-    azure-common==1.1.25
-    azure-core==1.3.0
-    azure-graphrbac==0.61.1
-    azure-kimlik==1.3.0
-    azure-mgmt-yetkilendirme ==0.60.0
-    azure-mgmt-containerregistry ==2.8.0
-    azure-mgmt-keyvault ==2.2.0
-    azure-mgmt-kaynak ==8.0.1
-    azure-mgmt-depolama ==8.0.0
-    azure-depolama-blob==1.5.0
-    azure-depolama-ortak ==1.4.2
-    azureml-core==1.1.5.5
-    azureml-dataprep-native==14.1.0
-    azureml-dataprep==1.3.5
-    azureml-varsayılan ==1.1.5.1
-    azureml-designer-klasik-modüller==0.0.118
-    azureml-designer-core==0.0.31
-    azureml-designer-internal==0.0.18
-    azureml-model-management-sdk==1.0.1b6.post1
-    azureml-boru hattı-çekirdek ==1.1.5
-    azureml-telemetri ==1.1.5.3
-    backports.tempfile==1.0
-    backports.weakref==1.0.post1
-    boto3==1.12.29
-    botocore==1.15.29
-    önbellek araçları==4.0.0
-    sertifikalı ==2019.11.28
-    cffi==1.12.3
-    chardet==3.0.4
-    tıklayın==7.1.1
-    cloudpickle ==1.3.0
-    configparser==3.7.4
-    bağlamlib2==0.6.0.post1
-    kriptografi==2.8
-    çevrimci==0.10.0
-    dereotu==0.3.1.1
-    dağıtım ==1.4.0
-    docker==4.2.0
-    docutils==0.15.2
-    dotnetcore2==2.1.13
-    şişe ==1.0.3
-    sigorta==3.0.1
-    gensim==3.8.1
-    google-api-core==1.16.0
-    google-auth==1.12.0
-    google-bulut-core ==1.3.0
-    google-bulut depolama ==1.26.0
-    google-resumable-media==0.5.0
-    googleapis-common-protos==1.51.0
-    gunicorn==19.9.0
-    idna==2.9
-    dengesiz-öğrenmek==0.4.3
-    isodate==0.6.0
-    tehlikeli==1.1.0
-    jeepney==0.4.3
-    jinja2==2.11.1
-    jmespath==0.9.5
-    joblib==0.14.0
-    json-logging-py==0.2
-    jsonpickle==1.3
-    jsonschema ==3.0.1
-    kivi çözücü==1.1.0
-    liac-arff==2.4.0
-    lightgbm==2.2.3
-    biçimlendirme==1.1.1
-    matplotlib==3.1.3
-    daha itertools ==6.0.0
-    msal uzantıları ==0.1.3
-    msal==1.1.0
-    msrest==0.6.11
-    msrestazure==0.6.3
-    ndg-httpsclient==0.5.1
-    nimbusml ==1.6.1
-    nümpi==1.18.2
-    oauthlib==3.1.0
-    pandalar==0.25.3
-    yol spec==0.7.0
-    pip==20.0.2
-    portalocker==1.6.0
-    protobuf==3.11.3
-    pyarrow==0.16.0
-    pyasn1 modülleri==0.2.8
-    pyasn1==0.4.8
-    pycparser==2.20
-    pycryptodomex ==3.7.3
-    pyjwt ==1.7.1
-    pyopenssl==19.1.0
-    pyparsing==2.4.6
-    piristent ==0.16.0
-    python-dateutil==2.8.1
-    pytz==2019.3
-    istekler-oauthlib==1.3.0
-    istekleri==2.23.0
-    rsa==4.0
-    ruamel.yaml==0.15.89
-    s3transfer==0.3.3
-    scikit-learn==0.22.2
-    scipy==1.4.1
-    gizli depolama ==3.1.2
-    setuptools==46.1.1.post20200323
-    altı==1.14.0
-    akıllı açık==1.10.0
-    urllib3==1.25.8
-    websocket-client==0.57.0
-    werkzeug==0.16.1
-    tekerlek==0.34.2

 Önceden yüklenmiş olmayan diğer paketleri yüklemek için ( örneğin *scikit-misc),* komut dosyanıza aşağıdaki kodu ekleyin: 

 ```python
import os
os.system(f"pip install scikit-misc")
```

## <a name="upload-files"></a>Dosyaları karşıya yükleme
**Execute Python Script,** Azure [Machine Learning Python SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#upload-file-name--path-or-stream-)kullanarak dosya yüklemeyi destekler.

Aşağıdaki örnek, **Python Script'i Çalıştır** modülünde bir resim dosyasının nasıl yüklenir olduğunu gösterir:

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

Boru hattı çalışması tamamlandıktan sonra, görüntüyü modülün sağ panelinde önizleyebilirsiniz

> [!div class="mx-imgBorder"]
> ![Yüklenen resim](media/module/upload-image-in-python-script.png)

## <a name="how-to-configure-execute-python-script"></a>Execute Python Komut Dosyası nasıl yapılandırılır?

**Python Script'i Yürüt** modülü, başlangıç noktası olarak kullanabileceğiniz örnek Python kodu içerir. Python Script modülunu **yapılandırmak** için **Python komut dosyası** metin kutusunda yürütülecek bir dizi giriş ve Python kodu sağlarsınız.

1. Python **Script'i çalıştır** modüllerini ardınıza ekleyin.

2. **Dataset1'de** giriş için kullanmak istediğiniz tasarımcıdan herhangi bir veri seti ekleyin ve bağlayın. Python komut dosyanızdaki bu veri kümesini **DataFrame1**olarak başvurun.

    Python kullanarak veri oluşturmak veya verileri doğrudan modüle almak için Python kodunu kullanmak istiyorsanız, veri kümesinin kullanımı isteğe bağlıdır.

    Bu modül **Dataset2'ye**ikinci bir veri kümesi eklenmesini destekler. Python komut dosyanızdaki ikinci veri kümesine DataFrame2 olarak başvurun.

    Azure Machine Learning'de depolanan veri kümeleri, bu modülle yüklendiğinde otomatik olarak **pandalar** data.frame'lerine dönüştürülür.

    ![Python giriş haritasını yürüt](media/module/python-module.png)

4. Yeni Python paketleri veya kodu eklemek için, **Komut Dosyası paketine**bu özel kaynakları içeren sıkıştırılmış dosyayı ekleyin. Komut Dosyası **paketine** giriş, Dosya türü Veri Kümesi olarak çalışma alanınıza yüklenen sıkıştırılmış bir dosya olmalıdır. **Datasets** varlık sayfasındaki veri kümesini yükleyebilir ve tasarımcı yazma sayfasında sol modül ağacındaki **Veri Kümeleri** listemdeki veri kümesi modülünü sürükleyip bırakabilirsiniz. 

    Yüklenen sıkıştırılmış arşivde bulunan herhangi bir dosya, boru hattı yürütmesırasında kullanılabilir. Arşiv bir dizin yapısı içeriyorsa, yapı korunur, ancak yola **src** adlı bir dizin hazırlamanız gerekir.

5. Python **komut dosyası** metin kutusunda, geçerli Python komut dosyası yazın veya yapıştırın.

    **Python komut dosyası** metin kutusu, yorumlarda bazı yönergeler ve veri erişimi ve çıktısı için örnek kodla önceden doldurulmuştür. Bu kodu değiştirmeniz veya değiştirmeniz gerekir. Girintinasyon ve kasa ile ilgili Python kurallarını takip etmeyi unutmayın.

    + Komut dosyası, bu `azureml_main` modülün giriş noktası olarak adlandırılan bir işlev içermelidir.
    + Giriş noktası işlevi en fazla iki giriş `Param<dataframe1>` bağımsız değişkeni içerebilir: ve`Param<dataframe2>`
    + Üçüncü giriş bağlantı noktasına bağlı sıkıştırılmış dosyalar gün önce açılır `.\Script Bundle`ve Python'a `sys.path`eklenen dizinde depolanır. 

    Bu nedenle, zip `mymodule.py`dosyanız içeriyorsa, 'yi kullanarak `import mymodule`içeri aktarın.

    + İki veri kümesi tasarımcıya döndürülebilir, bu da `pandas.DataFrame`bir tür dizisi olmalıdır. Python kodunuzda başka çıktılar oluşturabilir ve bunları doğrudan Azure depolama alanına yazabilirsiniz.

6. Ardışık komut dosyasını gönderin veya modülü seçin ve yalnızca Python komut dosyasını çalıştırmak için **seçili Çalıştır'ı** tıklatın.

    Tüm veriler ve kod sanal bir makineye yüklenir ve belirtilen Python ortamını kullanarak çalıştırılır.

## <a name="results"></a>Sonuçlar

Gömülü Python kodu tarafından gerçekleştirilen tüm hesaplamaların sonuçları panda olarak sağlanmalıdır. Otomatik olarak Azure Machine Learning veri seti biçimine dönüştürülen DataFrame, böylece sonuçları ardışık alandaki diğer modüllerle kullanabilirsiniz.

Modül iki veri kümesi döndürür:  
  
+ Python komut dosyasında ilk döndürülen pandalar veri çerçevesi tarafından tanımlanan **Sonuçlar Dataset 1**

+ **Python**komut dosyasında döndürülen ikinci pandalar veri çerçevesi tarafından tanımlanan Sonuç Veri Kümesi 2


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 