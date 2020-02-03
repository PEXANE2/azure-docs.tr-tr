---
title: Python istemci kitaplığı - Team Data Science Process ile veri kümelerine erişim
description: Yükleme ve Python istemci kitaplığı erişebilir ve Azure Machine Learning verileri bir yerel Python ortamından güvenli bir şekilde yönetmek için kullanın.
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
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720988"
---
# <a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Azure Machine Learning Python istemci kitaplığını kullanarak Python ile veri kümelerine erişim
Microsoft Azure Machine Learning Python istemci kitaplığı önizlemesi, Azure Machine Learning veri kümeleriniz için yerel bir Python ortamından güvenli erişimi etkinleştirebilir ve oluşturulmasını ve yönetimini bir çalışma alanındaki veri kümesi sağlar.

Bu konu hakkında yönergeler sağlar:

* Machine Learning Python istemci Kitaplığı'nı yükleyin
* erişim ve veri kümeleri, Azure Machine Learning veri kümeleri, yerel Python ortamınızdan erişmek için yetki alma hakkında yönergeler dahil olmak üzere karşıya yükleyin
* Ara veri kümeleri denemelerle erişim
* veri kümelerini numaralandırmak, meta verilere erişmek, bir veri kümesinin içeriğini okumak, yeni veri kümeleri oluşturmak ve mevcut veri kümelerini güncelleştirmek için Python istemci kitaplığını kullanın

## <a name="prerequisites"></a>Önkoşullar
Python istemci kitaplığı altında aşağıdaki ortamları test edilmiştir:

* Windows, Mac ve Linux
* Python 2.7, 3.3 ve 3.4

Bunu, aşağıdaki paketleri bağımlılık vardır:

* istekler
* Python dateutil
* pandas

Python, IPython ve yukarıda listelenen üç paket ile birlikte gelen [Anaconda](http://continuum.io/downloads#all) veya [Canopy](https://store.enthought.com/downloads/)gibi bir Python dağıtımı kullanmanızı öneririz. Ipython kesinlikle gerekli olmamakla birlikte, düzenleme ve etkileşimli veri görselleştirme için mükemmel bir ortamdır.

### <a name="installation"></a>Azure Machine Learning Python istemci kitaplığı 'nı yüklemek
Bu konuda özetlenen görevleri gerçekleştirmek için Python istemci kitaplığı Azure Machine Learning ' nı yükler. Bu kitaplık, [Python paket dizininden](https://pypi.python.org/pypi/azureml)kullanılabilir. Python ortamınızda yüklemek için yerel Python ortamınızdan aşağıdaki komutu çalıştırın:

    pip install azureml

Alternatif olarak, [GitHub](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python)'daki kaynaklardan indirebilir ve yükleyebilirsiniz.

    python setup.py install

Makinenizde git varsa, doğrudan git deposundan yüklemek için pip kullanabilirsiniz:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <a name="datasetAccess"></a>Veri kümelerine erişmek için kod parçacıklarını kullanma
Python istemci kitaplığı, programlı erişim, var olan veri kümelerine çalıştırılmış denemelerle sağlar.

Azure Machine Learning Studio (klasik) web arabiriminden, yerel makinenizde Pandas DataFrame nesneleri olarak veri kümelerini indirmek ve seri durumdan çıkarmak için gerekli tüm bilgileri içeren kod parçacıkları oluşturabilirsiniz.

### <a name="security"></a>Veri erişimi için güvenlik
Azure Machine Learning Studio (klasik) tarafından Python istemci kitaplığı ile kullanılmak üzere sunulan kod parçacıkları, çalışma alanı KIMLIĞINIZI ve yetkilendirme belirtecini içerir. Bu, çalışma alanınız için tam erişim sağlamak ve bir parola gibi korunmalıdır.

Güvenlik nedenleriyle, kod parçacığı işlevselliği yalnızca rolü çalışma alanı için **sahip** olarak ayarlanmış kullanıcılar tarafından kullanılabilir. Rolünüzde, **Ayarlar**altındaki **Kullanıcılar** sayfasında Azure Machine Learning Studio (klasik) görüntülenir.

![Güvenlik][security]

Rolünüz **sahip**olarak ayarlanmamışsa, bir sahip olarak yeniden davet edebilir veya çalışma alanının sahibinden kod parçacığını size vermesini isteyebilirsiniz.

Yetkilendirme belirtecini almak için şu seçeneklerden birini seçebilirsiniz:

* Bir sahibinden için bir belirteç isteyin. Sahipler, Azure Machine Learning Studio (klasik) içindeki çalışma alanının ayarlar sayfasından yetkilendirme belirteçlerine erişebilirler. Sol bölmeden **Ayarlar** ' ı seçin ve birincil ve ikincil belirteçleri görmek IÇIN **Yetkilendirme belirteçleri** ' ne tıklayın. Birincil veya ikincil yetkilendirme belirteçleri kod parçacığında kullanılabilir olsa da, sahipleri yalnızca ikincil yetkilendirme belirteçleri paylaşıma önerilir.

   ![Yetkilendirme belirteçleri](./media/python-data-access/ml-python-access-settings-tokens.png)

* Sahip rolüne yükseltilmesini sorma: çalışma alanının geçerli bir sahibinin öncelikle çalışma alanından birini kaldırması ve bunu bir sahip olarak yeniden davet etmesi gerekir.

Geliştiriciler çalışma alanı KIMLIĞINI ve yetkilendirme belirtecini aldıktan sonra, rolünden bağımsız olarak kod parçacığını kullanarak çalışma alanına erişebilirler.

Yetkilendirme belirteçleri, **Ayarlar**altındakı **Yetkilendirme belirteçleri** sayfasında yönetilir. Bunları yeniden oluşturabilirsiniz, ancak bu yordamın önceki belirteç erişimi iptal eder.

### <a name="accessingDatasets"></a>Yerel Python uygulamasından veri kümelerine erişme
1. Machine Learning Studio (klasik) içinde, sol taraftaki Gezinti çubuğunda **veri kümeleri** ' ne tıklayın.
2. Erişmek istediğiniz veri kümesini seçin. **Veri kümeleri listesinden veya** **örnekler** listesinden herhangi bir veri kümesinden birini seçebilirsiniz.
3. Alt araç çubuğundan **veri erişim kodu oluştur**' a tıklayın. Python istemci kitaplığı ile uyumlu bir biçimde veri olması durumunda bu düğmeyi devre dışı bırakıldı.
   
    ![Veri kümeleri][datasets]
4. Görünen ve panonuza kopyalayın penceresinden kod parçacığını seçin.
   
    ![Veri erişim kodu düğme oluşturma][dataset-access-code]
5. Kodu yerel Python uygulamanızı not defteri yapıştırın.
   
    ![Not defterinize kodu yapıştırın][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Machine Learning denemeleri 'deki ara veri kümelerine erişin
Machine Learning Studio (klasik) ' de bir deneme çalıştıktan sonra, modül çıkış düğümlerinden ara veri kümelerine erişmek mümkündür. Ara veri kümelerinde oluşturulan ve bir modeli aracı çalıştırdığınızda Ara adımları için kullanılan verilerdir.

Ara veri kümeleri, veri biçimi Python istemci kitaplığı ile uyumlu olduğu sürece erişilebilir.

Aşağıdaki biçimler desteklenir (Bu biçimler için sabitler `azureml.DataTypeIds` sınıfında):

* Düz metin
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

Modül çıkışı düğüm üzerinde gelerek biçimi belirleyebilirsiniz. Düğüm adına, bir araç ipucuyla birlikte görüntülenir.

[Bölünmüş][split] modül gibi bazı modüller, Python istemci kitaplığı tarafından desteklenmeyen `Dataset`adlı bir biçime çıktı.

![Veri kümesi biçimi][dataset-format]

Desteklenen bir biçime çıkış almak için [CSV 'ye Dönüştür][convert-to-csv]gibi bir dönüştürme modülü kullanmanız gerekir.

![GenericCSV biçimi][csv-format]

Aşağıdaki adımlarda, bir denemeyi oluşturur, çalıştığı ve Ara dataset erişen bir örnek gösterilmektedir.

1. Yeni bir deneme oluşturun.
2. **Yetişkin Census geliri Ikili sınıflandırma veri kümesi** modülünü ekleyin.
3. [Bölünmüş][split] bir modül ekleyin ve girişini veri kümesi modül çıktısına bağlayın.
4. CSV modülüne bir [dönüştürme][convert-to-csv] ekleyin ve girişini [bölünmüş][split] modül çıktılarından birine bağlayın.
5. Denemeyi kaydedin, çalıştırın ve işin bitmesini bekleyin.
6. [CSV 'ye Dönüştür][convert-to-csv] modülüne çıkış düğümüne tıklayın.
7. Bağlam menüsü göründüğünde, **veri erişim kodu oluştur**' u seçin.
   
    ![Bağlam Menüsü][experiment]
8. Kod parçacığını seçin ve açılır penceresinden panonuza kopyalayın.
   
    ![Bağlam menüsünden erişim kodu oluştur][intermediate-dataset-access-code]
9. Kodu defterinizdeki yapıştırın.
   
    ![Not defterinize kodu yapıştırın][ipython-intermediate-dataset]
10. Matplotlib kullanarak verileri görselleştirebilirsiniz. Bu yaş sütunu için bir çubuk grafik görüntüler:
    
    ![Çubuk grafik][ipython-histogram]

## <a name="clientApis"></a>Veri kümelerine erişmek, okumak, oluşturmak ve yönetmek için Python istemci kitaplığı Machine Learning kullanın
### <a name="workspace"></a>Çalışma alanı
Çalışma alanı Python istemci kitaplığı için giriş noktasıdır. Örnek oluşturmak için çalışma alanı KIMLIĞINIZ ve yetkilendirme belirtecinizle birlikte `Workspace` sınıfı sağlayın:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Veri kümeleri listeleme
Belirli bir çalışma alanındaki tüm veri kümelerini listelemek için:

    for ds in ws.datasets:
        print(ds.name)

Yalnızca kullanıcı tarafından oluşturulan veri kümelerini listelemek için:

    for ds in ws.user_datasets:
        print(ds.name)

Yalnızca örnek veri kümelerini listelemek için:

    for ds in ws.example_datasets:
        print(ds.name)

(Bu, büyük küçük harfe duyarlıdır) adına göre bir veri kümesine erişebilirsiniz:

    ds = ws.datasets['my dataset name']

Veya dizine göre erişebilirsiniz:

    ds = ws.datasets[0]


### <a name="metadata"></a>Meta Veriler
Veri kümeniz içerik yanı sıra meta veri yok. (Ara veri kümesi bu kural için bir özel durumdur ve tüm meta verilere sahip.)

Bazı meta veri değerleri, oluşturma zamanında kullanıcı tarafından atanan:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Diğer Azure ML tarafından atanan değerler şunlardır:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Kullanılabilir meta veriler hakkında daha fazla bilgi için `SourceDataset` sınıfına bakın.

### <a name="read-contents"></a>İçeriğini okuma
Machine Learning Studio (klasik) tarafından sunulan kod parçacıkları, veri kümesini bir Pandas DataFrame nesnesine otomatik olarak indirip serisini kaldıramıyor. Bu, `to_dataframe` yöntemiyle yapılır:

    frame = ds.to_dataframe()

Ham verileri indirebilirsiniz ve seri durumundan çıkarma kendiniz gerçekleştirmek isterseniz, bir seçenektir. Şu anda Python istemci kitaplığı seri durumdan çıkarılamıyor 'ARFF'ye' gibi biçimler için tek seçenek budur.

İçeriği metin okuma için:

    text_data = ds.read_as_text()

İçeriğini ikili olarak okumak için:

    binary_data = ds.read_as_binary()

Ayrıca yalnızca bir akışa içeriği açabilirsiniz:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Yeni veri kümesi oluşturma
Python istemci kitaplığı Python programınızı veri kümelerinden yüklemenize olanak sağlar. Bu veri kümelerini ardından çalışma alanınızdaki kullanılabilir.

Verilerinizi bir pandas DataFrame varsa, aşağıdaki kodu kullanın:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Verilerinizi zaten serileştirildiği, kullanabilirsiniz:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Python istemci kitaplığı, bir Pandas veri çerçevesini aşağıdaki biçimlere seri hale getirmek için (sabitleri `azureml.DataTypeIds` sınıfında bulunur):

* Düz metin
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

### <a name="update-an-existing-dataset"></a>Güncelleştirme var olan bir dataset
Var olan bir dataset eşleşen bir ada sahip yeni bir veri kümesi karşıya denerseniz, bir çakışma hatası almanız gerekir.

Mevcut bir veri kümesini güncelleştirmek için önce varolan bir veri kümesi bir başvuru almak gerekir:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Ardından, Azure 'da veri kümesinin içeriğini seri hale getirmek ve değiştirmek için `update_from_dataframe` kullanın:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Verileri farklı bir biçimde seri hale getirmek istiyorsanız, isteğe bağlı `data_type_id` parametresi için bir değer belirtin.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

İsteğe bağlı olarak, `description` parametresi için bir değer belirterek yeni bir açıklama ayarlayabilirsiniz.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

İsteğe bağlı olarak, `name` parametresi için bir değer belirterek yeni bir ad belirleyebilirsiniz. Şu andan itibaren yalnızca yeni bir ad kullanarak bir veri kümesini almak. Aşağıdaki kod, verileri, adı ve açıklamayı günceller.

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

`data_type_id`, `name` ve `description` parametreleri isteğe bağlıdır ve önceki değerleri için varsayılandır. `dataframe` parametresi her zaman gereklidir.

Verileriniz zaten serileştirilmiş ise, `update_from_dataframe`yerine `update_from_raw_data` kullanın. `dataframe`yerine yalnızca `raw_data` geçirirseniz, benzer bir şekilde çalışmaktadır.

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

