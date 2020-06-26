---
title: Etiketler ile veri kümeleri oluşturma ve araştırma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning etiketleme projelerinizden veri etiketlerini dışarı aktarmayı öğrenin ve makine öğrenimi görevleri için bunları kullanın.
author: nibaccam
ms.author: nibaccam
ms.service: machine-learning
ms.topic: how-to
ms.date: 05/14/2020
ms.openlocfilehash: 6401d620aea720584df0838cacc6df2f837ca699
ms.sourcegitcommit: dfa5f7f7d2881a37572160a70bac8ed1e03990ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85374483"
---
# <a name="create-and-explore-azure-machine-learning-dataset-with-labels-preview"></a>Etiketlerle Azure Machine Learning veri kümesi oluşturma ve araştırma (Önizleme)

Bu makalede, veri etiketlerini bir Azure Machine Learning veri etiketleme projesinden dışarı aktarmayı ve bunları, veri araştırması için bir Pandas dataframe veya görüntü dönüştürmesi için Torchvision veri kümesi gibi popüler biçimlere yüklemeyi öğreneceksiniz. 

## <a name="what-are-datasets-with-labels"></a>Etiketleri olan veri kümeleri nelerdir 

Etiketlere sahip Azure Machine Learning veri kümeleri etiket özelliği olan [Tabulardataset](how-to-create-register-datasets.md#dataset-types) ' tir, bunlara etiketli veri kümeleri olarak başvuracağız. Bu özel Tabulardataset türleri yalnızca Azure Machine Learning veri etiketleme projelerinin çıktısı olarak oluşturulur. [Bu adımlarla](how-to-create-labeling-projects.md)bir veri etiketleme projesi oluşturun. Machine Learning, çok etiketli veya çok sınıflı ve nesne tanımlamasının sınırlı kutular ile birlikte görüntü sınıflandırması için veri etiketleme projelerini destekler.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLFree) oluşturun.
* [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya [Azure Machine Learning Studio](https://ml.azure.com/)'ya erişim.
    * [Azure-contrib-DataSet](https://docs.microsoft.com/python/api/azureml-contrib-dataset/?view=azure-ml-py) paketini yükler
* Machine Learning çalışma alanı. Bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).
* Azure Machine Learning veri etiketleme projesine erişim. Etiketleme projeniz yoksa, [Bu adımlarla](how-to-create-labeling-projects.md)bir tane oluşturun.

## <a name="export-data-labels"></a>Veri etiketlerini dışarı aktar 

Bir veri etiketleme projesini tamamladığınızda, etiket verilerini etiketleme projesinden dışarı aktarabilirsiniz. Bunu yaptığınızda, hem verilere hem de etiketlerine olan başvuruyu yakalayıp [Coco formatında](http://cocodataset.org/#format-data) veya bir Azure Machine Learning veri kümesi olarak dışarı aktarabilirsiniz. Etiketleme projenizin **proje ayrıntıları** sayfasında **dışarı aktar** düğmesini kullanın.

### <a name="coco"></a>COCO 

 COCO dosyası, Azure Machine Learning çalışma alanının varsayılan Blob deposunda *dışarı aktarma/Coco*içindeki bir klasörde oluşturulur. 

### <a name="azure-machine-learning-dataset"></a>Azure Machine Learning veri kümesi

Azure Machine Learning Studio 'nun **veri kümeleri** bölümünde, aktarılmış Azure Machine Learning veri kümesine erişebilirsiniz. Veri kümesi **ayrıntıları** sayfası, Python 'daki etiketlerinize erişmek için örnek kod de sağlar.

![Aktarılmış veri kümesi](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="explore-labeled-datasets"></a>Etiketli veri kümelerini keşfet

Veri araştırması için popüler açık kaynaklı kitaplıkların yanı sıra görüntü dönüştürme ve eğitimle ilgili kitaplıklar için, etiketli veri kümelerinizi bir Pandas dataframe veya Torchvision veri kümesine yükleyin.

### <a name="pandas-dataframe"></a>Pandas dataframe

Etiketli veri kümelerini, sınıfındaki yöntemiyle bir Pandas dataframe 'e yükleyebilirsiniz [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--) `azureml-contrib-dataset` . Aşağıdaki kabuk komutuyla sınıfı yüklersiniz: 

```shell
pip install azureml-contrib-dataset
```

>[!NOTE]
>Hizmetin geliştirilmesi için çalışdığımız için, azureml. contrib ad alanı sıklıkla değişir. Bu nedenle, bu ad alanındaki her şey Microsoft tarafından tam olarak desteklenmez ve önizleme olarak değerlendirilmelidir.

Bir Pandas dataframe 'e dönüştürürken dosya akışları için aşağıdaki dosya işleme seçeneklerini sunuyoruz.
* İndir: veri dosyalarınızı yerel bir yola Indirin.
* Bağla: veri dosyalarınızı bağlama noktasına bağlayın. Bağlama yalnızca Azure Machine Learning Not defteri VM ve Azure Machine Learning Işlem gibi Linux tabanlı işlem için geçerlidir.

```Python
import azureml.contrib.dataset
from azureml.contrib.dataset import FileHandlingOption
animal_pd = animal_labels.to_pandas_dataframe(file_handling_option=FileHandlingOption.DOWNLOAD, target_path='./download/', overwrite_download=True)

import matplotlib.pyplot as plt
import matplotlib.image as mpimg

#read images from downloaded path
img = mpimg.imread(animal_pd.loc[0,'image_url'])
imgplot = plt.imshow(img)
```

### <a name="torchvision-datasets"></a>Torchvision veri kümeleri

Etiketli veri kümelerini, sınıfından de [to_torchvision ()](https://docs.microsoft.com/python/api/azureml-contrib-dataset/azureml.contrib.dataset.tabulardataset?view=azure-ml-py#to-torchvision--) yöntemiyle birlikte Torchvision veri kümesine yükleyebilirsiniz `azureml-contrib-dataset` . Bu yöntemi kullanmak için [Pytorch](https://pytorch.org/) yüklü olmalıdır. 

```python
from torchvision.transforms import functional as F

# load animal_labels dataset into torchvision dataset
pytorch_dataset = animal_labels.to_torchvision()
img = pytorch_dataset[0][0]
print(type(img))

# use methods from torchvision to transform the img into grayscale
pil_image = F.to_pil_image(img)
gray_image = F.to_grayscale(pil_image, num_output_channels=3)

imgplot = plt.imshow(gray_image)
```

## <a name="next-steps"></a>Sonraki adımlar

* Tüm eğitim örneği için [Etiketler Not defteri ile veri kümesine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/labeled-datasets/labeled-datasets.ipynb) bakın.
