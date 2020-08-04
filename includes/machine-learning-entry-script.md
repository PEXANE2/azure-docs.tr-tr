---
author: gvashishtha
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: include
ms.date: 07/31/2020
ms.author: gopalv
ms.openlocfilehash: 2b4f768b25917e712380ca4a7f8ac58cb6140777
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87542831"
---
Giriş betiği, dağıtılan bir web hizmetine gönderilen verileri alır ve modele geçirir. Ardından model tarafından döndürülen yanıtı alır ve bunu istemciye döndürür. *Betik, modelinize özeldir*. Modelin beklediği ve döndürdüğü verileri anlaması gerekir.

Betik, modeli yükleyen ve çalıştıran iki işlev içerir:

* `init()`: Genellikle, bu işlev modeli genel bir nesneye yükler. Bu işlev, Web hizmetiniz için Docker kapsayıcısı başlatıldığında yalnızca bir kez çalıştırılır.

* `run(input_data)`: Bu işlev, giriş verilerine göre bir değeri tahmin etmek için modeli kullanır. Çalıştırmanın girişleri ve çıkışlarında serileştirme ve seriden çıkarma için normal olarak JSON kullanılır. Ham ikili verilerle de çalışabilirsiniz. Verileri modele göndermeden veya istemciye döndürmeden önce dönüştürebilirsiniz.

REST API, isteğin gövdesinin aşağıdaki yapıyla bir JSON belgesi olmasını bekliyor:

```json
{
    "data":
        [
            <model-specific-data-structure>
        ]
}
```

Aşağıdaki örnek, kayıtlı bir scikit-öğrenme modelinin nasıl yükleneceğini ve sayısal tuş bir y verisi ile nasıl puan alınacağını gösterir:

```python
#Example: scikit-learn and Swagger
import json
import numpy as np
import os
from sklearn.externals import joblib
from sklearn.linear_model import Ridge

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType


def init():
    global model
    # AZUREML_MODEL_DIR is an environment variable created during deployment. Join this path with the filename of the model file.
    # It holds the path to the directory that contains the deployed model (./azureml-models/$MODEL_NAME/$VERSION).
    # If there are multiple models, this value is the path to the directory containing all deployed models (./azureml-models).
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_mnist_model.pkl')

    # If your model were stored in the same directory as your score.py, you could also use the following:
    # model_path = os.path.abspath(os.path.join(os.path.dirname(__file_), 'sklearn_mnist_model.pkl')

    # Deserialize the model file back into a sklearn model
    model = joblib.load(model_path)


input_sample = np.array([[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]])
output_sample = np.array([3726.995])


@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # You can return any data type, as long as it is JSON serializable.
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

Daha fazla örnek için aşağıdaki betiklerine bakın:

* [PyTorch](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/pytorch)
* [TensorFlow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/tensorflow)
* [Keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* [AutoML](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features)
* [ONNX](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* [İkili veriler](../articles/machine-learning/how-to-deploy-advanced-entry-script.md#binary-data)