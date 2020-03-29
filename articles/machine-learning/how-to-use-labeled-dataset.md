---
title: Etiketlerle veri kümeleri oluşturma ve keşfetme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning etiketleme projelerinizden veri etiketleri nasıl dışa aktaracağınızı ve bunları makine öğrenimi görevleri için nasıl kullanacağınızı öğrenin.
author: nibaccam
ms.author: nibaccam
ms.service: machine-learning
ms.topic: how-to
ms.date: 01/21/2020
ms.openlocfilehash: 5138109de3f80d405ce95b605714b511480563f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76549495"
---
# <a name="create-and-explore-azure-machine-learning-dataset-with-labels"></a>Etiketlerle Azure Machine Learning veri kümesi oluşturma ve keşfetme

Bu makalede, bir Azure Machine Learning veri etiketleme projesinden veri etiketlerini nasıl dışa aktaracağınızı ve bunları veri arama için pandalar veri çerçevesi veya görüntü dönüştürme için Torchvision veri kümesi gibi popüler biçimlere yüklemeyi öğreneceksiniz. 

## <a name="what-are-datasets-with-labels"></a>Etiketlerle veri kümeleri nedir 

Etiketli Azure Machine Learning veri kümeleri etiket özelliğine sahip [TabularDatasets'tir,](how-to-create-register-datasets.md#dataset-types) bunları etiketli veri kümeleri olarak adlandıracağız. Bu belirli Türde Veri Kümeleri yalnızca Azure Machine Learning veri etiketleme projelerinin çıktısı olarak oluşturulur. [Bu adımlarla](how-to-create-labeling-projects.md)bir veri etiketleme projesi oluşturun. Machine Learning, çok etiketli veya çok katmanlı görüntü sınıflandırması ve nesne tanımlama ve sınırlı kutularla nesne tanımlama için veri etiketleme projelerini destekler.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://aka.ms/AMLFree) bir hesap oluşturun.
* [Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya Azure Machine Learning [stüdyosuna](https://ml.azure.com/)erişim.
    * [Azure-contrib-dataset](https://docs.microsoft.com/python/api/azureml-contrib-dataset/?view=azure-ml-py) paketini yükleme
* Bir Makine Öğrenme çalışma alanı. Bkz. [Azure Makine Öğrenimi çalışma alanı oluşturun.](how-to-manage-workspace.md)
* Azure Machine Learning veri etiketleme projesine erişim. Etiketleme projeniz yoksa, [bu adımlarla](how-to-create-labeling-projects.md)bir proje oluşturun.

## <a name="export-data-labels"></a>Veri etiketlerini dışa aktarma 

Bir veri etiketleme projesini tamamladığınızda, etiket verilerini bir etiketleme projesinden dışa aktarabilirsiniz. Bunu yapmak, hem verilere hem de etiketlerine yapılan başvuruyu yakalamanıza ve bunları [COCO biçiminde](http://cocodataset.org/#format-data) veya Azure Machine Learning veri seti olarak dışa aktarmanıza olanak tanır. Etiketleme projenizin **Proje ayrıntıları** sayfasındaki **Dışa** Aktarma düğmesini kullanın.

### <a name="coco"></a>Coco 

 COCO dosyası, Azure Machine Learning çalışma alanının varsayılan blob deposunda *dışa aktarma/coco*içindeki bir klasörde oluşturulur. 

### <a name="azure-machine-learning-dataset"></a>Azure Machine Learning veri seti

Azure Machine Learning stüdyonuzun Veri **Setleri** bölümünden dışa aktarılan Azure Machine Learning veri kümesine erişebilirsiniz. Dataset **Details** sayfası, etiketlerinize Python'dan erişmek için örnek kod da sağlar.

![Dışa aktarılan veri kümesi](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="explore-labeled-datasets"></a>Etiketli veri kümelerini keşfedin

Etiketli veri kümelerinizi, veri arama için popüler açık kaynak kitaplıklarından yararlanmak için pandalar veri çerçevesine veya Torchvision veri setine yükleyin ve PyTorch görüntü dönüşümü ve eğitimi için kitaplıklar sağladı.

### <a name="pandas-dataframe"></a>Pandalar veri çerçevesi

Etiketli veri kümelerini sınıftaki yöntemle pandalar [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--) veri `azureml-contrib-dataset` çerçevesine yükleyebilirsiniz. Sınıfı aşağıdaki kabuk komutuyla yükleyin: 

```shell
pip install azureml-contrib-dataset
```

>[!NOTE]
>Hizmeti geliştirmeye çalıştığımıziçin azureml.contrib ad alanı sık sık değişir. Bu nedenle, bu ad alanındaki her şey önizleme olarak kabul edilmeli ve Microsoft tarafından tam olarak desteklenmemelidir.

Pandalar veri çerçevesine dönüştürürken dosya akışları için aşağıdaki dosya işleme seçeneklerini sunuyoruz.
* İndir: Veri dosyalarınızı yerel bir yola indirin.
* Mount: Veri dosyalarınızı bir montaj noktasına monte edin. Mount, Azure Machine Learning dizüstü bilgisayarı VM ve Azure Machine Learning Compute dahil olmak üzere yalnızca Linux tabanlı bilgi işlem için çalışır.

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

### <a name="torchvision-datasets"></a>Torchvision veri setleri

Sınıftan [da to_torchvision()](https://docs.microsoft.com/python/api/azureml-contrib-dataset/azureml.contrib.dataset.tabulardataset?view=azure-ml-py#to-torchvision--) yöntemiyle etiketli veri kümelerini Torchvision `azureml-contrib-dataset` veri kümesine yükleyebilirsiniz. Bu yöntemi kullanmak için [PyTorch'un](https://pytorch.org/) yüklü olması gerekir. 

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

* Tam eğitim örneği için [etiketlernotiçeren veri kümesine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/labeled-datasets/labeled-datasets.ipynb) bakın.
