---
title: Python istemci kitaplığı ile veri kümelerini erişin - Takım Veri Bilimi Süreci
description: Azure Machine Learning verilerine yerel bir Python ortamından güvenli bir şekilde erişmek ve yönetmek için Python istemci kitaplığını yükleyin ve kullanın.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 93ec5e740ac6acf9420a9d980092ed772ac1618e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720988"
---
# <a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Azure Machine Learning Python istemci kitaplığını kullanarak Python ile veri kümelerine erişim
Microsoft Azure Machine Learning Python istemci kitaplığı önizlemesi, Azure Machine Learning veri kümelerinize yerel bir Python ortamından güvenli erişim sağlayabilir ve bir çalışma alanında veri kümelerinin oluşturulmasını ve yönetilmesini sağlar.

Bu konu nasıl talimatlar sağlar:

* Machine Learning Python istemci kitaplığını yükleyin
* yerel Python ortamınızdan Azure Machine Learning veri kümelerine erişmek için yetkilendirme alma hakkında talimatlar da dahil olmak üzere veri kümelerine erişin ve yükleyin
* denemelerden ara veri kümeleri erişim
* veri kümelerini sayısallandırmak, meta verilere erişmek, veri kümesinin içeriğini okumak, yeni veri kümeleri oluşturmak ve varolan veri kümelerini güncelleştirmek için Python istemci kitaplığını kullanın

## <a name="prerequisites"></a><a name="prerequisites"></a>Ön koşullar
Python istemci kitaplığı aşağıdaki ortamlar altında sınanmıştır:

* Windows, Mac ve Linux
* Python 2.7, 3.3 ve 3.4

Aşağıdaki paketlere bağımlıdır:

* Istek
* python-dateutil
* Pandalar

[Biz Anaconda](http://continuum.io/downloads#all) veya [Canopy](https://store.enthought.com/downloads/)gibi python dağıtım kullanmanızı öneririz , Python ile birlikte gelen, IPython ve yüklü yukarıda listelenen üç paket. IPython kesinlikle gerekli olmasa da, verileri etkileşimli olarak işlemek ve görselleştirmek için harika bir ortamdır.

### <a name="how-to-install-the-azure-machine-learning-python-client-library"></a><a name="installation"></a>Azure Machine Learning Python istemci kitaplığı nasıl yüklenir?
Bu konuda özetlenen görevleri tamamlamak için Azure Machine Learning Python istemci kitaplığını yükleyin. Bu kitaplık [Python Paket Dizini'nden](https://pypi.python.org/pypi/azureml)edinilebilir. Python ortamınıza yüklemek için yerel Python ortamınızdan aşağıdaki komutu çalıştırın:

    pip install azureml

Alternatif olarak, [GitHub'daki](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python)kaynaklardan indirip yükleyebilirsiniz.

    python setup.py install

Makinenize git yüklü varsa, doğrudan git deposundan yüklemek için pip kullanabilirsiniz:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <a name="use-code-snippets-to-access-datasets"></a><a name="datasetAccess"></a>Veri kümelerine erişmek için kod parçacıklarını kullanma
Python istemci kitaplığı, çalıştırılanmış denemelerden varolan veri kümelerinize programlı erişim sağlar.

Azure Machine Learning Studio (klasik) web arabiriminden, yerel makinenizdeki pandalar DataFrame nesneleri olarak veri kümelerini indirmek ve deserialize etmek için gerekli tüm bilgileri içeren kod parçacıkları oluşturabilirsiniz.

### <a name="security-for-data-access"></a><a name="security"></a>Veri erişimi için güvenlik
Python istemci kitaplığıyla kullanılmak üzere Azure Machine Learning Studio (klasik) tarafından sağlanan kod parçacıkları, çalışma alanı kimliğinizi ve yetkilendirme belirtecinizi içerir. Bunlar çalışma alanınıza tam erişim sağlar ve parola gibi korunması gerekir.

Güvenlik nedenleriyle, kod snippet işlevselliği yalnızca çalışma alanı için **Sahibi** olarak rol kümeleyen kullanıcılar tarafından kullanılabilir. Rolünüz **Ayarlar**altında **USERS** sayfasında Azure Machine Learning Studio 'da (klasik) görüntülenir.

![Güvenlik][security]

Rolünüz **Sahip**olarak ayarlanmazsa, ya bir sahibi olarak yeniden davet edilmeyi isteyebilir veya çalışma alanının sahibinden size kod parçacığı sağlamasını isteyebilirsiniz.

Yetkilendirme belirteci elde etmek için aşağıdaki seçeneklerden birini seçebilirsiniz:

* Sahibinden bir jeton iste. Araç sahipleri yetkilendirme belirteçlerine Azure Machine Learning Studio'daki (klasik) çalışma alanının Ayarlar sayfasından erişebilirler. Sol bölmeden **Ayarlar'ı** seçin ve birincil ve ikincil belirteçleri görmek için **YETKILENDIRME BELIRTEÇleri'ni** tıklatın. Birincil veya ikincil yetkilendirme belirteçleri kod snippet'te kullanılabilse de, sahiplerinin yalnızca ikincil yetkilendirme belirteçlerini paylaşmaları önerilir.

   ![Yetkilendirme belirteçleri](./media/python-data-access/ml-python-access-settings-tokens.png)

* Sahibi rolüne terfi etmek isteyin: çalışma alanının geçerli bir sahibinin önce sizi çalışma alanından kaldırması, ardından sahibi olarak yeniden davet etmesi gerekir.

Geliştiriciler çalışma alanı kimliğini ve yetkilendirme belirteci'ni aldıktan sonra, rolleri ne olursa olsun kod parçacıklarını kullanarak çalışma alanına erişebilirler.

Yetkilendirme belirteçleri **AYARLAR**altında **YETKILENDIRME belirteçleri** sayfasında yönetilir. Bunları yeniden oluşturabilirsiniz, ancak bu yordam önceki belirteç erişimi iptal eder.

### <a name="access-datasets-from-a-local-python-application"></a><a name="accessingDatasets"></a>Yerel bir Python uygulamasından veri kümelerini erişin
1. Machine Learning Studio'da (klasik), soldaki gezinti çubuğundaki **DATASETS'i** tıklatın.
2. Erişmek istediğiniz veri kümesini seçin. VERI kümelerinden herhangi birini **MY DATASETS** listesinden veya **ÖRNEKLER** listesinden seçebilirsiniz.
3. Alttaki araç çubuğundan **Veri Erişim Kodu Oluştur'u**tıklatın. Veriler Python istemci kitaplığıyla uyumsuz bir biçimdeyse, bu düğme devre dışı bırakılır.
   
    ![Veri kümeleri][datasets]
4. Görünen pencereden kod parçacığı seçin ve panonuza kopyalayın.
   
    ![Veri erişim kodu düğmesi oluşturma][dataset-access-code]
5. Kodu yerel Python uygulamanızın not defterine yapıştırın.
   
    ![Kodu not defterine yapıştır][ipython-dataset]

## <a name="access-intermediate-datasets-from-machine-learning-experiments"></a><a name="accessingIntermediateDatasets"></a>Machine Learning denemelerinden ara veri kümelerini erişin
Machine Learning Studio'da (klasik) bir deney çalıştırdıktan sonra, ara veri kümelerine modüllerin çıkış düğümlerinden erişmek mümkündür. Ara veri kümeleri, bir model aracı çalıştırıldığında oluşturulan ve ara adımlar için kullanılan verilerdir.

Veri biçimi Python istemci kitaplığıyla uyumlu olduğu sürece ara veri kümeleri erişilebilir.

Aşağıdaki biçimler desteklenir (bu biçimler için sabitler `azureml.DataTypeIds` sınıftadır):

* PlainText
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

Bir modül çıkış düğümü üzerinde gezinerek biçimi belirleyebilirsiniz. Düğüm adı ile birlikte bir araç ucunda görüntülenir.

[Bölme][split] modülü gibi bazı modüller, Python istemci kitaplığı tarafından desteklenmeyen , adlı `Dataset`bir biçime çıktı.

![Dataset Biçimi][dataset-format]

Bir çıktıyı desteklenen biçime dönüştürmek için [CSV'ye dönüştür][convert-to-csv]gibi bir dönüşüm modülü kullanmanız gerekir.

![GenericCSV Biçimi][csv-format]

Aşağıdaki adımlar, bir deneme oluşturan, çalıştıran ve ara veri kümesine erişen bir örnek gösterir.

1. Yeni bir deneme oluşturun.
2. **Yetişkin Nüfus Sayımı Gelir İkili Sınıflandırması dataset** modülü ekleyin.
3. [Bir Bölme][split] modülü ekleyin ve girişini veri kümesi modülü çıkışına bağlayın.
4. [CSV modülüne dönüştür'ü][convert-to-csv] ekleyin ve girdisini [Split][split] modülü çıktılarından birine bağlayın.
5. Denemeyi kaydedin, çalıştırın ve işin bitmesini bekleyin.
6. [CSV][convert-to-csv] modülüne dönüştür modülündeki çıkış düğümüne tıklayın.
7. Bağlam menüsü göründüğünde, **Veri Erişim Kodu Oluştur'u**seçin.
   
    ![Bağlam Menüsü][experiment]
8. Kod snippet'i seçin ve görünen pencereden panonuza kopyalayın.
   
    ![Bağlam menüsünden erişim kodu oluşturma][intermediate-dataset-access-code]
9. Kodu not defterinize yapıştırın.
   
    ![Kodu not defterine yapıştır][ipython-intermediate-dataset]
10. Matplotlib kullanarak verileri görselleştirebilirsiniz. Bu yaş sütunu için bir histogram görüntüler:
    
    ![Histogram][ipython-histogram]

## <a name="use-the-machine-learning-python-client-library-to-access-read-create-and-manage-datasets"></a><a name="clientApis"></a>Veri kümelerine erişmek, okumak, oluşturmak ve yönetmek için Machine Learning Python istemci kitaplığını kullanın
### <a name="workspace"></a>Çalışma alanı
Çalışma alanı Python istemci kitaplığı için giriş noktasıdır. `Workspace` Bir örnek oluşturmak için sınıfa çalışma alanı kimliğinizi ve yetkilendirme belirtecinizi sağlayın:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Veri kümelerini sayısallandırma
Belirli bir çalışma alanındaki tüm veri kümelerini sayısallandırmak için:

    for ds in ws.datasets:
        print(ds.name)

Yalnızca kullanıcı tarafından oluşturulan veri kümelerini sayısallandırmak için:

    for ds in ws.user_datasets:
        print(ds.name)

Yalnızca örnek veri kümelerini doğrulamak için:

    for ds in ws.example_datasets:
        print(ds.name)

Bir veri kümesine ada göre erişebilirsiniz (büyük/küçük harf duyarlıdır):

    ds = ws.datasets['my dataset name']

Veya dizin tarafından erişebilirsiniz:

    ds = ws.datasets[0]


### <a name="metadata"></a>Meta Veriler
Veri kümeleri, içeriğe ek olarak meta verilere sahiptir. (Ara veri kümeleri bu kuralın bir istisnasıdır ve herhangi bir meta verisi yoktur.)

Bazı meta veri değerleri oluşturma zamanında kullanıcı tarafından atanır:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Diğerleri Azure ML tarafından atanan değerlerdir:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Kullanılabilir `SourceDataset` meta veriler hakkında daha fazla bilgi için sınıfa bakın.

### <a name="read-contents"></a>İçeriği okuma
Machine Learning Studio (klasik) tarafından sağlanan kod parçacıkları, veri kümesini pandalar DataFrame nesnesine otomatik olarak indirir ve deserialize eder. Bu `to_dataframe` yöntem ile yapılır:

    frame = ds.to_dataframe()

Ham verileri indirmeyi ve deserialization'ı kendiniz gerçekleştirmeyi tercih ederseniz, bu bir seçenektir. Şu anda, Python istemci kitaplığı deserialize edemez 'ARFF' gibi biçimler için tek seçenektir.

İçeriği metin olarak okumak için:

    text_data = ds.read_as_text()

İçeriği ikili olarak okumak için:

    binary_data = ds.read_as_binary()

Ayrıca, içindekilere bir akış da açabilirsiniz:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Yeni bir veri kümesi oluşturma
Python istemci kitaplığı, Python programınızdan veri kümeleri yüklemenize olanak tanır. Bu veri kümeleri daha sonra çalışma alanınızda kullanılabilir.

Verileriniz pandadataframe'de yse, aşağıdaki kodu kullanın:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Verileriniz zaten seri hale getirıldıysa, şunları kullanabilirsiniz:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Python istemci kitaplığı bir panda DataFrame'i aşağıdaki biçimlere seri hale ebilir `azureml.DataTypeIds` (bunların sabitleri sınıftadır):

* PlainText
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

### <a name="update-an-existing-dataset"></a>Varolan bir veri kümesini güncelleştirme
Varolan bir veri kümesiyle eşleşen bir adiçeren yeni bir veri kümesi yüklemeye çalışırsanız, çakışma hatası almanız gerekir.

Varolan bir veri kümesini güncelleştirmek için öncelikle varolan veri kümesine başvuruda bulunmalısınız:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Ardından `update_from_dataframe` Azure'daki veri kümesinin içeriğini seri hale getirmek ve değiştirmek için kullanın:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Verileri farklı bir biçimde seri hale getirmek istiyorsanız, isteğe bağlı `data_type_id` parametre için bir değer belirtin.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Parametre için `description` bir değer belirterek isteğe bağlı olarak yeni bir açıklama ayarlayabilirsiniz.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

Parametre için `name` bir değer belirterek isteğe bağlı olarak yeni bir ad ayarlayabilirsiniz. Şu andan itibaren, yalnızca yeni adı kullanarak veri kümesini alırsınız. Aşağıdaki kod verileri, adı ve açıklamayı güncelleştirir.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

`data_type_id`, `name` ve `description` parametreler isteğe bağlıdır ve önceki değeri varsayılandır. `dataframe` Parametre her zaman gereklidir.

Verileriniz zaten seri hale `update_from_raw_data` getirılmışsa, "' yerine `update_from_dataframe`. Sadece `raw_data` yerine `dataframe`geçerseniz, benzer bir şekilde çalışır.

<!-- Images -->
[security]:./media/python-data-access/security.png
[dataset-format]:./media/python-data-access/dataset-format.png
[csv-format]:./media/python-data-access/csv-format.png
[datasets]:./media/python-data-access/datasets.png
[dataset-access-code]:./media/python-data-access/dataset-access-code.png
[ipython-dataset]:./media/python-data-access/ipython-dataset.png
[experiment]:./media/python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

