---
title: 'Öğretici: bir not defteri ile tahmine dayalı model oluşturma (Bölüm 1/2)'
titleSuffix: Azure Machine Learning
description: Jupyter Notebook kod kullanarak makine öğrenimi modeli oluşturmayı ve dağıtmayı öğrenin ve bu sayede Microsoft Power BI sonuçları tahmin etmek için kullanabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: f8209c0d26cf8c572d10666696231b0468cfcbc6
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97370330"
---
# <a name="tutorial-power-bi-integration---create-the-predictive-model-with-a-notebook-part-1-of-2"></a>Öğretici: Power BI tümleştirme-bir not defteri ile tahmine dayalı model oluşturma (Bölüm 1/2)

Bu öğreticinin ilk bölümünde, bir Jupyter Notebook kod kullanarak tahmine dayalı bir makine öğrenme modeli eğitecaksınız ve dağıtırsınız. 2. bölümde, Microsoft Power BI sonuçları tahmin etmek için modeli kullanacaksınız.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Jupyter Notebook oluşturma
> * Azure Machine Learning işlem örneği oluşturma
> * Scikit-öğrenme kullanarak regresyon modeli eğitme
> * Modeli gerçek zamanlı bir Puanlama uç noktasına dağıtma

Power BI ' de kullanacağınız modeli oluşturmanın ve dağıtmanın üç farklı yolu vardır.  Bu makalede bir seçenek yer almaktadır: not defterlerini kullanarak modelleri eğitme ve dağıtma.  Bu seçenek, Azure Machine Learning Studio 'da barındırılan Jupyıter not defterlerini kullanan kod ilk yazma deneyimini gösterir. 

Bunun yerine şunları kullanabilirsiniz:

* Seçenek B: tasarımcı kullanarak (bir sürükle ve bırak Kullanıcı arabirimi) düşük kodlu bir yazma deneyimi [kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-designer-model.md).
* [Seçenek C: OTOMATIK ml kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-automated-model.md) -kod içermeyen bir yazma deneyimi, veri hazırlama ve model eğitimini tamamen otomatikleştirir.


## <a name="prerequisites"></a>Önkoşullar

- Bir Azure aboneliği ([ücretsiz deneme sürümü mevcuttur](https://aka.ms/AMLFree)). 
- Azure Machine Learning çalışma alanı. Zaten bir çalışma alanınız yoksa [Azure Machine Learning çalışma alanı oluşturma](./how-to-manage-workspace.md#create-a-workspace)' yı izleyin.
- Python dili ve makine öğrenimi iş akışlarının giriş bilgisi.

## <a name="create-a-notebook-and-compute"></a>Not defteri ve işlem oluşturma

[Azure Machine Learning Studio](https://ml.azure.com) giriş sayfasında **Yeni oluştur** ve **Not defteri**' ni seçin:

:::image type="content" source="media/tutorial-power-bi/create-new-notebook.png" alt-text="Bir not defteri oluşturmayı gösteren ekran görüntüsü":::
 
**Yeni bir dosya oluşturmak** için bir iletişim kutusu gösterilir:

1. Not defteriniz için bir dosya adı (örneğin `my_model_notebook` )
1. **Dosya türünü** **Not defteri** olarak değiştirme

**Oluştur**’u seçin. Daha sonra, kod hücrelerini çalıştırmak için bir işlem oluşturmanız ve Not defterinize eklemeniz gerekir. Bunu yapmak için, Not defterinin en üstündeki artı simgesini seçin:

:::image type="content" source="media/tutorial-power-bi/create-compute.png" alt-text="İşlem örneğinin nasıl oluşturulacağını gösteren ekran görüntüsü":::

Sonra, **işlem örneği oluştur** sayfasında:

1. Bir CPU sanal makine boyutu seçin-Bu öğreticinin amaçları doğrultusunda bir **Standard_D11_v2** (iki çekirdek, 14 GB RAM) iyi olacaktır.
1. **İleri**’yi seçin. 
1. **Ayarları Yapılandır** sayfasında geçerli bir **işlem adı** sağlayın (geçerli karakterler büyük ve küçük harf, rakam ve-karakter) sağlar.
1. **Oluştur**’u seçin.

Not defteri üzerinde, **işlem** örneğinin oluşturulduğunu belirten, işlemin yanında bulunan dairenin Camgöbeği olduğunu fark edebilirsiniz:

:::image type="content" source="media/tutorial-power-bi/creating.png" alt-text="Oluşturulan işlem gösteren ekran görüntüsü":::

> [!NOTE]
> İşlemin sağlanması yaklaşık 2-4 dakika sürebilir.

İşlem sağlandıktan sonra kod hücrelerini yürütmek için Not defterini kullanabilirsiniz. Örneğin, hücresine yazın:

```python
import numpy as np

np.sin(3)
```

Ardından **SHIFT tuşuna basın** (veya **Denetim-** hücrenin yanındaki Oynat düğmesini girin veya seçin). Aşağıdaki çıkışı görmeniz gerekir:

:::image type="content" source="media/tutorial-power-bi/simple-sin.png" alt-text="Hücre yürütmeyi gösteren ekran görüntüsü":::

Artık bir Machine Learning modeli oluşturmaya hazırsınız!

## <a name="build-a-model-using-scikit-learn"></a>Scikit-öğrenme kullanarak model oluşturma

Bu öğreticide, [Azure açık veri kümelerinde](https://azure.microsoft.com/services/open-datasets/)kullanılabilir hale getirilen [diabetes veri kümesini](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html)kullanırsınız. 


### <a name="import-data"></a>Veri içeri aktarma

Verilerinizi içeri aktarmak için aşağıdaki kodu kopyalayın ve Not defterinize yeni bir **kod hücresine** yapıştırın:

```python
from azureml.opendatasets import Diabetes

diabetes = Diabetes.get_tabular_dataset()
X = diabetes.drop_columns("Y")
y = diabetes.keep_columns("Y")
X_df = X.to_pandas_dataframe()
y_df = y.to_pandas_dataframe()
X_df.info()
```

`X_df`Pandas veri çerçevesi, 10 temel giriş değişkenleri (yaş, Sex, gövde yığın dizini, ortalama kan basıncı ve altı kan serum ölçümleri) içerir. `y_df`Pandas veri çerçevesi, temel bir yılda bir yıllık bir nicelik ölçüsünün bir nicel ölçüsünü içeren hedef değişkenidir. Toplam 442 kayıt vardır.

### <a name="train-model"></a>Modeli eğitme

Not defterinizde yeni bir **kod hücresi** oluşturun ve aşağıdaki kod parçacığını kopyalayıp yapıştırın. Bu, bir Ridge regresyon modeli oluşturur ve Python 'un Pickle biçimini kullanarak modeli seri hale getirir:

```python
import joblib
from sklearn.linear_model import Ridge

model = Ridge().fit(X,y)
joblib.dump(model, 'sklearn_regression_model.pkl')
```

### <a name="register-the-model"></a>Modeli Kaydet

Model dosyasının içeriğinin yanı sıra, kayıtlı modeliniz model meta veri modeli açıklaması, Etiketler ve çerçeve bilgilerini de depolar; bu, çalışma alanınızdaki modelleri yönetirken ve dağıtmada yararlı olacaktır. Örneğin, etiketleri kullanarak modellerinizi kategorilere ayırabilirsiniz ve çalışma alanınızdaki modelleri listelerken filtre uygulayabilirsiniz. Ayrıca, bu modeli scikit-öğren çerçevesiyle işaretlemek, daha sonra göreceğiniz gibi Web hizmeti olarak dağıtımı basitleştirir.

Aşağıdaki kodu kopyalayıp Not defterinize yeni bir **kod hücresine** yapıştırın:

```python
import sklearn

from azureml.core import Workspace
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

ws = Workspace.from_config()

model = Model.register(workspace=ws,
                       model_name='my-sklearn-model',                # Name of the registered model in your workspace.
                       model_path='./sklearn_regression_model.pkl',  # Local file to upload and register as a model.
                       model_framework=Model.Framework.SCIKITLEARN,  # Framework used to create the model.
                       model_framework_version=sklearn.__version__,  # Version of scikit-learn used to create the model.
                       sample_input_dataset=X,
                       sample_output_dataset=y,
                       resource_configuration=ResourceConfiguration(cpu=2, memory_in_gb=4),
                       description='Ridge regression model to predict diabetes progression.',
                       tags={'area': 'diabetes', 'type': 'regression'})

print('Name:', model.name)
print('Version:', model.version)
```

Ayrıca, sol taraftaki menüdeki **uç noktalara** giderek modeli Azure Machine Learning Studio de görüntüleyebilirsiniz:

:::image type="content" source="media/tutorial-power-bi/model.png" alt-text="Modeli gösteren ekran görüntüsü":::

### <a name="define-the-scoring-script"></a>Puanlama betiğini tanımlama

Bir modeli Microsoft Power BI ile tümleştirilecek şekilde dağıttığınızda, bir Python *Puanlama betiği* ve özel ortam tanımlamanız gerekir. Puanlama betiği iki işlev içerir:

- `init()` -Bu işlev, hizmet başladıktan sonra yürütülür. Bu işlev modeli yükler (modelin model kayıt defterinden otomatik olarak indirildiğini unutmayın) ve onu serileştirir.
- `run(data)` -Bu işlev, hizmete Puanlama gerektiren bazı giriş verileriyle bir çağrı yapıldığında yürütülür. 

>[!NOTE]
> Microsoft Power BI tümleştirmesinin çalışması için önemli olan giriş ve çıkış verilerinin şemasını tanımlamak için Python dekoratlarını kullanırız.

Aşağıdaki kodu kopyalayıp Not defterinize yeni bir **kod hücresine** yapıştırın. Aşağıdaki kod parçacığında, kodu score.py adlı bir dosyalana yazılacak bir hücre Magic vardır.

```python
%%writefile score.py

import json
import pickle
import numpy as np
import pandas as pd
import os
import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType


def init():
    global model
    # Replace filename if needed.
    path = os.getenv('AZUREML_MODEL_DIR') 
    model_path = os.path.join(path, 'sklearn_regression_model.pkl')
    # Deserialize the model file back into a sklearn model.
    model = joblib.load(model_path)


input_sample = pd.DataFrame(data=[{
    "AGE": 5,
    "SEX": 2,
    "BMI": 3.1,
    "BP": 3.1,
    "S1": 3.1,
    "S2": 3.1,
    "S3": 3.1,
    "S4": 3.1,
    "S5": 3.1,
    "S6": 3.1
}])

# This is an integer type sample. Use the data type that reflects the expected result.
output_sample = np.array([0])

# To indicate that we support a variable length of data input,
# set enforce_shape=False
@input_schema('data', PandasParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        print("input_data....")
        print(data.columns)
        print(type(data))
        result = model.predict(data)
        print("result.....")
        print(result)
    # You can return any data type, as long as it is JSON serializable.
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

### <a name="define-the-custom-environment"></a>Özel ortamı tanımlama

Daha sonra, modeli puanlamaya yönelik ortamı tanımlamanız gerekir. bu ortamda, Pandas, scikit-öğren, vb. için tanımlanan Puanlama betiği (score.py) için gerekli olan Python paketlerini tanımlamanız gerekir.

Ortamı tanımlamak için aşağıdaki kodu kopyalayın ve Not defterinize yeni bir **kod hücresine** yapıştırın:

```python
from azureml.core.model import InferenceConfig
from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies

environment = Environment('my-sklearn-environment')
environment.python.conda_dependencies = CondaDependencies.create(pip_packages=[
    'azureml-defaults',
    'inference-schema[numpy-support]',
    'joblib',
    'numpy',
    'pandas',
    'scikit-learn=={}'.format(sklearn.__version__)
])

inference_config = InferenceConfig(entry_script='./score.py',environment=environment)
```

### <a name="deploy-the-model"></a>Modeli dağıtma

Modeli dağıtmak için aşağıdaki kodu kopyalayın ve Not defterinize yeni bir **kod hücresine** yapıştırın:

```python
service_name = 'my-diabetes-model'

service = Model.deploy(ws, service_name, [model], inference_config, overwrite=True)
service.wait_for_deployment(show_output=True)
```

>[!NOTE]
> Hizmetin dağıtılması 2-4 dakika sürebilir.

Başarıyla dağıtılan bir hizmetin aşağıdaki çıktısını görmeniz gerekir:

```txt
Tips: You can try get_logs(): https://aka.ms/debugimage#dockerlog or local deployment: https://aka.ms/debugimage#debug-locally to debug if deployment takes longer than 10 minutes.
Running......................................................................................
Succeeded
ACI service creation operation finished, operation "Succeeded"
```

Ayrıca, sol taraftaki menüde **uç noktalar** ' a giderek hizmeti Azure Machine Learning Studio de görebilirsiniz:

:::image type="content" source="media/tutorial-power-bi/endpoint.png" alt-text="Uç noktayı gösteren ekran görüntüsü":::

Web hizmeti 'nin beklendiği gibi çalıştığından emin olmak için test etmeniz önerilir. Azure Machine Learning Studio sol taraftaki menüden **Not defterleri** ' ni seçerek not defterinize geri gidin. Hizmeti test etmek için aşağıdaki kodu kopyalayıp Not defterinize yeni bir **kod hücresine** yapıştırın:

```python
import json


input_payload = json.dumps({
    'data': X_df[0:2].values.tolist(),
    'method': 'predict'  # If you have a classification model, you can get probabilities by changing this to 'predict_proba'.
})

output = service.run(input_payload)

print(output)
```

Çıktı aşağıdaki JSON yapısına benzer olmalıdır: `{'predict': [[205.59], [68.84]]}` .

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir modeli Microsoft Power BI tarafından tüketilebilen şekilde nasıl oluşturup dağıtacağınızı gördünüz. Bir sonraki bölümde, bu modeli bir Power BI raporundan kullanmayı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Öğretici: Power BI modeli kullanın](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
