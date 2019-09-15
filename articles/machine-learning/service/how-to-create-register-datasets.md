---
title: Azureml veri kümeleri ile verilere erişmek için veri kümeleri oluşturma
titleSuffix: Azure Machine Learning service
description: Çeşitli kaynaklardan veri kümeleri oluşturmayı ve veri kümelerini çalışma alanınıza kaydetmeyi öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 08/22/2019
ms.openlocfilehash: 8f684a9c0c40774c8c17a08801997c569be74c8d
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70993361"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Azure Machine Learning veri kümeleri (Önizleme) oluşturma ve erişme

Bu makalede, Azure Machine Learning veri kümeleri (Önizleme) oluşturma ve yerel veya uzak denemeleri verilere erişme hakkında bilgi edineceksiniz.

Azure Machine Learning veri kümeleri ile şunları yapabilirsiniz: 

* Veri kümeleri tarafından başvurulan **depolama alanındaki verilerin tek bir kopyasını saklayın** . 

* Bağlantı dizeleri veya veri yolları hakkında endişelenmeden **model eğitimi sırasında verilere kolayca erişin** .

* **Veri paylaşma & diğer kullanıcılarla işbirliği yapın** .

## <a name="prerequisites"></a>Önkoşullar

Veri kümeleri oluşturmak ve bunlarla çalışmak için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure Machine Learning hizmet çalışma alanı](how-to-manage-workspace.md)

* Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), azureml veri kümesi paketini içerir.

> [!Note]
> Bazı veri kümesi sınıflarının (Önizleme), [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) paketine bağımlılıkları vardır. Linux kullanıcıları için, bu sınıflar yalnızca aşağıdaki dağıtımlarda desteklenir:  Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="dataset-types"></a>Veri kümesi türleri

Veri kümeleri, kullanıcıların eğitim aşamasında nasıl tükettiği hakkında çeşitli türlerde kategorilere ayrılır. Veri kümesi türlerinin listesi:
* [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) , belirtilen dosya veya dosya listesini ayrıştırarak verileri tablolu biçimde temsil eder. Bu, verileri bir Pandas DataFrame 'te yürütme yeteneği sağlar. CSV `TabularDataset` , TSV, Parquet dosyalarından bir nesne oluşturulabilir, SQL sorgu sonuçları vb. olabilir. Listenin tamamı için lütfen [belgelerimizi](https://aka.ms/tabulardataset-api-reference)ziyaret edin.
* [Filedataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) , veri mağazalarınızın veya genel URL 'lerdeki tek veya birden çok dosyaya başvurur. Bu, dosyaları kendi işlem dosyalarınıza indirme veya bağlama olanağı sağlar. Dosyalar, derin öğrenme dahil olmak üzere daha geniş bir makine öğrenimi senaryolarına izin veren herhangi bir biçimde olabilir.

Yaklaşan API değişiklikleri hakkında daha fazla bilgi edinmek için [buraya](https://aka.ms/tabular-dataset)bakın.

## <a name="create-datasets"></a>Veri kümeleri oluşturma 

Bir veri kümesi oluşturarak, veri kaynağı konumuna, meta verilerinin bir kopyasıyla birlikte bir başvuru oluşturursunuz. Veriler mevcut konumunda kalır, bu nedenle ek depolama maliyeti tahakkuk etmemesi gerekir.

Verilere Azure Machine Learning hizmeti tarafından erişilebilmeleri için, veri kümelerinin [Azure veri depoları](how-to-access-data.md) veya genel Web URL 'lerinde yollardan oluşturulması gerekir.

Bir [Azure veri deposundan](how-to-access-data.md)veri kümeleri oluşturmak için:

* Kayıtlı Azure veri `contributor` deposuna `owner` sahip olduğunuzu veya erişiminiz olduğunu doğrulayın.

* Veri deposundaki bir yola başvurarak veri kümesini oluşturun.

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()

# retrieve an existing datastore in the workspace by name
datastore = Datastore.get(workspace, datastore_name)
```

### <a name="create-tabulardatasets"></a>Tabulardataset oluşturma

Tabulardataset 'ler SDK aracılığıyla veya çalışma alanı giriş sayfası (Önizleme) kullanılarak oluşturulabilir.

#### <a name="sdk"></a>SDK 

CSV veya TSV biçimindeki `TabularDatasetFactory` dosyaları okumak için sınıfındaki yönteminikullanınvekayıtlıolmayanbirtabulardatasetoluşturun.`from_delimited_files()` Birden çok dosyadan okuyorsanız, sonuçlar tek tablolu bir gösterimde toplanacaktır.

```Python
# create a TabularDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'weather/2018/11.csv'),
                  (datastore, 'weather/2018/12.csv'),
                  (datastore, 'weather/2019/*.csv')
                 ]
weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)

# create a TabularDataset from a delimited file behind a public web url
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |Passengerıd|Kalan|PClass|Name|Komutu|Yaş|SibSp|Parch|Ticket|Tarifeli havayolu|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1\.|0|3|Braund, Mr. Owen HARRIS|erkek|22,0|1\.|0|A/5 21171|7,2500||P
1\.|2|1\.|1\.|Hanler, Mrs. John Bradley (çiçek)...|kadın|38,0|1\.|0|BILGISAYAR 17599|71,2833|C85|C
2|3|1\.|3|Heıkkinen, Isabetsizlik. Laina|kadın|26,0|0|0|STON/O2. 3101282|7,9250||P

#### <a name="workspace-landing-page"></a>Çalışma alanı giriş sayfası 

Web deneyimi aracılığıyla bir veri kümesi oluşturmak için [çalışma alanı giriş sayfasında](https://ml.azure.com) oturum açın. Şu anda, çalışma alanı giriş sayfası yalnızca Tabulardataset 'leri oluşturmayı destekler.

Aşağıdaki animasyon çalışma alanı giriş sayfasında bir veri kümesi oluşturmayı gösterir. 

İlk olarak, sol bölmedeki **varlıklar** bölümünde **veri kümeleri** ' ni seçin. Ardından, veri kümenizin kaynağını seçmek için **+ veri kümesi oluştur** ' u seçin. Bu, yerel dosyalardan, veri deposundan veya genel Web URL 'lerinden olabilir. **Ayarlar ve önizleme** ve **şema** formları dosya türüne göre akıllıca doldurulur. Bunları gözden geçirmek veya oluşturmadan önce veri kümenizi daha fazla yapılandırmak için **İleri ' yi** seçin. Veri kümesi oluşturma **işlemi tamamlandığında bitti** ' yi seçin. 

![UI ile veri kümesi oluşturma](media/how-to-create-register-datasets/create-dataset-ui.gif)

### <a name="create-filedatasets"></a>Dosya veri kümeleri oluştur

Dosyaları herhangi bir biçimde `FileDatasetFactory` yüklemek için sınıfındaki yönteminikullanınvekayıtlıolmayanbirfiledatasetoluşturun.`from_files()`

```Python
# create a FileDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'animals/dog/1.jpg'),
                  (datastore, 'animals/dog/2.jpg'),
                  (datastore, 'animals/dog/*.jpg')
                 ]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = [
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz'
           ]          
mnist_ds = Dataset.File.from_files(path=web_paths)
```
## <a name="register-datasets"></a>Veri kümelerini Kaydet

Oluşturma işlemini gerçekleştirmek için, veri kümelerinizi çalışma alanına kaydedin:

Veri kümelerini başkalarıyla paylaşılabilmesi ve çeşitli denemeleri genelinde yeniden kullanmak için çalışma alanınıza kaydetmek üzere yönteminikullanın.`register()`

```Python
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'titanic training data')
```

>[!Note]
> Çalışma alanı giriş sayfası aracılığıyla oluşturulan veri kümeleri otomatik olarak çalışma alanına kaydedilir. 

## <a name="version-datasets"></a>Sürüm veri kümeleri

Yeni bir sürüm oluşturarak aynı ada sahip yeni bir veri kümesini kaydedebilirsiniz. Veri kümesi sürümü verilerinizin durumuna yer işaretinin bir yoludur. bu sayede, deneme veya gelecekteki bir çoğaltma için veri kümesinin belirli bir sürümünü uygulayabilirsiniz. Sürüm oluşturmayı düşünmek için tipik senaryolar: 
* Yeni veriler yeniden eğitimine uygun olduğunda.
* Farklı veri hazırlama veya özellik Mühendisliği yaklaşımları uyguladığınızda.

```Python
# create a TabularDataset from Titanic training data
web_paths = [
            'https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
            'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv'
           ]          
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

# create a new version of titanic_ds
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'new titanic training data',
                                 create_new_version = True)
```


## <a name="access-your-data-during-training"></a>Eğitim sırasında verilerinize erişin

Kayıtlı veri kümelerine, Azure Machine Learning işlem gibi işlem kümelerinde yerel olarak ve Uzaktan erişilebilir. Kayıtlı veri kümenize denemeleri üzerinden erişmek için, çalışma alanınızı ve kayıt kümenizi ada göre almak üzere aşağıdaki kodu kullanın. Sınıf üzerindeki yöntemi, varsayılan olarak çalışma alanına kayıtlı veri kümesinin en son sürümünü döndürür. [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) `Dataset`

```Python
%%writefile $script_folder/train.py

from azureml.core import Dataset, Run

run = Run.get_context()
workspace = run.experiment.workspace

dataset_name = 'titanic_ds'

# Get a dataset by name
titanic_ds = Dataset.get_by_name(workspace=workspace, name=dataset_name)

# Load a TabularDataset into pandas DataFrame
df = titanic_ds.to_pandas_dataframe()
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tabulardataset ile eğilmesi](https://aka.ms/automl-dataset)için otomatik makine öğrenimini kullanın.
* Veri kümeleri ile eğitim hakkında daha fazla örnek için bkz. [örnek Not defterleri](https://aka.ms/dataset-tutorial).
