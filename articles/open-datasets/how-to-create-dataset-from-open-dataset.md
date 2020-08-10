---
title: Azure açık veri kümeleri ile veri kümeleri oluşturma
titleSuffix: Azure Open Datasets
description: Azure açık veri kümelerinden Azure Machine Learning veri kümesi oluşturmayı öğrenin.
ms.service: open-datasets
ms.topic: conceptual
ms.author: nibaccam
author: nibaccam
ms.date: 08/05/2020
ms.custom: how-to, tracking-python
ms.openlocfilehash: db2e493f04d74308b21e8a63222f26b1e2af8546
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88038295"
---
# <a name="create-azure-machine-learning-datasets-from-azure-open-datasets"></a>Azure açık veri kümelerinde Azure Machine Learning veri kümeleri oluşturma
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, yerel veya uzak denemeleri verilerinize erişmek için [Azure açık veri](https://azure.microsoft.com/services/open-datasets/) kümelerinden Azure Machine Learning veri kümesi oluşturmayı öğreneceksiniz. 

Azure Machine Learning bir veri [kümesi](../machine-learning/how-to-create-register-datasets.md)oluşturarak, veri kaynağı konumuna, meta verilerinin bir kopyasıyla birlikte bir başvuru oluşturursunuz. Veriler var olan konumunda kaldığı için ek depolama maliyeti yoktur ve özgün veri kaynaklarınızı yanlışlıkla değiştirmezsiniz. Ayrıca, veri kümeleri geç değerlendirilir ve bu, iş akışı performans hızlarına yardımcı olur.
 
Veri kümelerinin Azure Machine Learning genel veri erişimi iş akışına uygun olduğunu anlamak için, [güvenli erişim verileri](../machine-learning/concept-data.md#data-workflow) makalesine bakın.


Açık veri kümeleri bulutta Microsoft Azure ve hem [Azure Machine Learning Python SDK](#create-datasets-with-the-sdk) hem de [Azure Machine Learning Studio](#create-datasets-with-the-studio)'ya dahil edilmiştir.

## <a name="why-use-azure-open-datasets"></a>Azure açık veri kümeleri neden kullanılmalıdır? 

Azure açık veri kümeleri, daha doğru modeller için makine öğrenimi çözümlerine senaryoya özgü özellikler eklemek için kullanabileceğiniz, seçkin ortak veri kümeleridir. Veri kümeleri, makine öğrenimi modellerini ve zenginleştirme çözümlerini eğitmenize yardımcı olan hava durumu, Census, tatiller, genel güvenlik ve konum için genel etki alanı verilerini içerir. 

Daha fazla bilgi için bkz. [açık veri kümeleri nelerdir?](overview-what-are-open-datasets.md)

## <a name="prerequisites"></a>Ön koşullar

Veri kümeleri oluşturmak ve bunlarla çalışmak için şunlar gerekir:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* [Azure Machine Learning çalışma alanı](../machine-learning/how-to-manage-workspace.md).

* Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), azureml veri kümesi paketini içerir.

    * Tümleşik Not defterleri ve SDK 'nın zaten yüklü olduğu tam olarak yapılandırılmış ve yönetilen bir geliştirme ortamı olan [Azure Machine Learning işlem örneği](../machine-learning/concept-compute-instance.md#managing-a-compute-instance)oluşturun.

    **VEYA**

    * Kendi Jupyter Not defteriniz üzerinde çalışın ve [bu yönergelerle](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)SDK 'yı kendiniz de yüklersiniz.

> [!NOTE]
> Bazı veri kümesi sınıflarının yalnızca 64 bitlik Python ile uyumlu olan [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) paketine bağımlılıkları vardır. Linux kullanıcıları için, bu sınıflar yalnızca şu dağıtımlara göre desteklenir: Red Hat Enterprise Linux (7, 8), Ubuntu (14,04, 16,04, 18,04), Fedora (27, 28), demı (8, 9) ve CentOS (7).

## <a name="create-datasets-with-the-sdk"></a>SDK ile veri kümeleri oluşturma

Azure Machine Learning Python SDK 'sında açık veri kümeleri sınıfları aracılığıyla veri kümeleri oluşturmak için paketini ile yüklediğinizden emin olun `pip install azureml-opendatasets` . Her ayrık veri kümesi SDK 'daki kendi sınıfıyla temsil edilir ve belirli sınıflar `TabularDataset` ,, `FileDataset` veya her ikisi olarak kullanılabilir. Sınıfların tam listesi için [başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py) bakın.

`TabularDataset` `FileDataset` Dosyaları doğrudan yönetmenize ve/veya indirmenizi sağlayan bir veya olarak belirli sınıfları alabilirsiniz. Diğer sınıflar **yalnızca** veya işlevlerinden birini kullanarak bir veri kümesi alabilir `get_tabular_dataset()` `get_file_dataset()` . Aşağıdaki kod örneği, bu sınıf türleri için birkaç örnek gösterir.

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

Açık veri kümelerinden oluşturulan bir veri kümesini kaydettiğinizde, hiçbir veri hemen indirilir, ancak istendiğinde (eğitim sırasında, örneğin, merkezi bir depolama konumundan), verilere daha sonra erişilir.

## <a name="create-datasets-with-the-studio"></a>Studio ile veri kümeleri oluşturma

Açık veri kümelerinden [Azure Machine Learning Studio](https://ml.azure.com)ile veri kümeleri de oluşturabilirsiniz. Çalışma alanınızda **varlıklar**' ın altındaki **veri kümeleri** sekmesini seçin. **Veri kümesi oluştur** aşağı açılan menüsünde **açık veri kümeleri ' nden**seçim yapın.

![UI ile veri kümesini açma](./media/how-to-create-dataset-from-open-dataset/open-datasets-1.png)

Kutucuğunu seçerek bir veri kümesi seçin. (Arama çubuğunu kullanarak filtreleme seçeneğiniz vardır.) **İleri ' yi**seçin.

![Veri kümesi seçin](./media/how-to-create-dataset-from-open-dataset/open-datasets-2.png)

Veri kümesinin kaydedileceği bir ad seçin ve isteğe bağlı olarak, kullanılabilir filtreleri kullanarak verileri filtreleyin. Bu durumda, genel tatiller veri kümesi için zaman aralığını bir yıla, ülke kodunu ise yalnızca BIZIMLE filtreleyerek filtreleyebilirsiniz. **Oluştur**’u seçin.

![Veri kümesi params 'i ayarlama ve veri kümesi oluşturma](./media/how-to-create-dataset-from-open-dataset/open-datasets-3.png)

Veri kümesi artık veri **kümeleri**altında çalışma alanınızda kullanılabilir. Bunu, oluşturduğunuz diğer veri kümeleriyle aynı şekilde kullanabilirsiniz.


## <a name="access-datasets-for-your-experiments"></a>Denemeleri için veri kümelerine erişin

ML modellerinizi eğitmek için Machine Learning denemeleri 'te veri kümelerinizi kullanın. [Veri kümeleriyle eğitme hakkında daha fazla bilgi edinin](../machine-learning/how-to-train-with-datasets.md).

## <a name="example-notebooks"></a>Örnek not defterleri

Açık veri kümesi işlevlerinin örnekleri ve gösterileri için bu [Not defterlerine](https://github.com/Azure/OpenDatasetsNotebooks/tree/master/tutorials)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri kümeleri ile model eğitme](../machine-learning/how-to-train-with-datasets.md).

* [Bir Azure Machine Learning veri kümesi oluşturun](../machine-learning/how-to-create-register-datasets.md).



