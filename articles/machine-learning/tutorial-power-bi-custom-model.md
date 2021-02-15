---
title: 'Öğretici: bir not defteri ile tahmine dayalı model oluşturma (Bölüm 1/2)'
titleSuffix: Azure Machine Learning
description: Jupyter Notebook kod kullanarak makine öğrenimi modeli oluşturmayı ve dağıtmayı öğrenin. Ayrıca, Microsoft Power BI 'e kolay tümleştirme için giriş ve çıktıyı tanımlayan bir Puanlama betiği oluşturun.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: 00d5fa43245fb25b8ee99a0523d680bef891b71e
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100387011"
---
# <a name="tutorial-power-bi-integration---create-the-predictive-model-with-a-jupyter-notebook-part-1-of-2"></a>Öğretici: Power BI tümleştirme-tahmine dayalı modeli bir Jupyter Notebook oluşturun (Bölüm 1/2)

Bu öğreticinin 1. bölümünde, tahmine dayalı bir makine öğrenimi modelini bir Jupyter Notebook kod kullanarak eğitecaksınız ve dağıtırsınız. Ayrıca, Power BI tümleştirme için modelin giriş ve çıkış şemasını tanımlamak üzere bir Puanlama betiği oluşturacaksınız.  2. bölümde, Microsoft Power BI sonuçları tahmin etmek için modeli kullanacaksınız.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Jupyter Notebook oluşturma.
> * Azure Machine Learning işlem örneği oluşturun.
> * Scikit-öğrenme kullanarak regresyon modeli eğitme.
> * Microsoft Power BI 'e kolay tümleştirme için giriş ve çıktıyı tanımlayan bir Puanlama betiği yazın.
> * Modeli gerçek zamanlı bir Puanlama uç noktasına dağıtın.

Power BI ' de kullanacağınız modeli oluşturmanın ve dağıtmanın üç yolu vardır.  Bu makalede "seçenek A:" not defterlerini kullanarak modelleri eğitme ve dağıtma "konuları ele alınmaktadır.  Bu seçenek, kod ilk yazma deneyimidir. Azure Machine Learning Studio barındırılan JUPITER not defterlerini kullanır. 

Ancak bunun yerine diğer seçeneklerden birini de kullanabilirsiniz:

* [Seçenek B: Azure Machine Learning tasarımcısını kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-designer-model.md). Bu düşük kod yazma deneyimi bir sürükle ve bırak Kullanıcı arabirimi kullanır.
* [Seçenek C: otomatik makine öğrenimini kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-automated-model.md). Bu kod içermeyen yazma deneyimi, veri hazırlama ve model eğitimini tamamen otomatikleştirir.


## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://aka.ms/AMLFree)kullanabilirsiniz. 
- Azure Machine Learning çalışma alanı. Zaten bir çalışma alanınız yoksa, bkz. [Azure Machine Learning çalışma alanları oluşturma ve yönetme](./how-to-manage-workspace.md#create-a-workspace).
- Python dili ve makine öğrenimi iş akışlarının giriş bilgisi.

## <a name="create-a-notebook-and-compute"></a>Not defteri ve işlem oluşturma

[**Azure Machine Learning Studio**](https://ml.azure.com) giriş sayfasında **Yeni**  >  **Not defteri** oluştur ' u seçin:

:::image type="content" source="media/tutorial-power-bi/create-new-notebook.png" alt-text="Bir not defteri oluşturmayı gösteren ekran görüntüsü.":::
 
**Yeni dosya oluştur** sayfasında:

1. Not defterinizi adlandırın (örneğin, *my_model_notebook*).
1. **Dosya türünü** **Not defteri** olarak değiştirin.
1. **Oluştur**’u seçin. 
 
Ardından, kod hücrelerini çalıştırmak için bir işlem örneği oluşturun ve Not defterinize ekleyin. Not defterinin en üstünde artı simgesini seçerek başlayın:

:::image type="content" source="media/tutorial-power-bi/create-compute.png" alt-text="İşlem örneğinin nasıl oluşturulacağını gösteren ekran görüntüsü.":::

**İşlem örneği oluştur** sayfasında:

1. Bir CPU sanal makine boyutu seçin. Bu öğreticide, 2 çekirdek ve 14 GB RAM içeren bir **Standard_D11_v2** seçebilirsiniz.
1. **İleri**’yi seçin. 
1. **Ayarları Yapılandır** sayfasında, geçerli bir **işlem adı** belirtin. Geçerli karakterler büyük ve küçük harfler, rakamlar ve kısa çizgilerden (-) oluşur.
1. **Oluştur**’u seçin.

Not defterinde, **işlem** kapalı Camgöbeği ' ın yanındaki daireyi fark edebilirsiniz. Bu renk değişikliği, işlem örneğinin oluşturulduğunu belirtir:

:::image type="content" source="media/tutorial-power-bi/creating.png" alt-text="Oluşturulmakta olan bir işlem gösteren ekran görüntüsü.":::

> [!NOTE]
> İşlem örneğinin sağlanması 2 ila 4 dakika sürebilir.

İşlem sağlandıktan sonra kod hücrelerini çalıştırmak için Not defterini kullanabilirsiniz. Örneğin, hücresinde aşağıdaki kodu yazabilirsiniz:

```python
import numpy as np

np.sin(3)
```

Ardından SHIFT + enter ' ı seçin (ya da CTRL + ENTER ' u seçin ya da hücrenin yanındaki **oynat** düğmesini seçin). Aşağıdaki çıkışı görmeniz gerekir:

:::image type="content" source="media/tutorial-power-bi/simple-sin.png" alt-text="Bir hücrenin çıkışını gösteren ekran görüntüsü.":::

Artık bir makine öğrenimi modeli oluşturmaya hazır olursunuz.

## <a name="build-a-model-by-using-scikit-learn"></a>Scikit-öğrenme kullanarak model oluşturma

Bu öğreticide [diabetes veri kümesini](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html)kullanırsınız. Bu veri kümesi, [Azure açık veri kümelerinde](https://azure.microsoft.com/services/open-datasets/)kullanılabilir.


### <a name="import-data"></a>Veri içeri aktarma

Verilerinizi içeri aktarmak için aşağıdaki kodu kopyalayın ve Not defterinize yeni bir *kod hücresine* yapıştırın.

```python
from azureml.opendatasets import Diabetes

diabetes = Diabetes.get_tabular_dataset()
X = diabetes.drop_columns("Y")
y = diabetes.keep_columns("Y")
X_df = X.to_pandas_dataframe()
y_df = y.to_pandas_dataframe()
X_df.info()
```

`X_df`Pandas veri çerçevesi 10 temel giriş değişkeni içerir. Bu değişkenler Age, Sex, Body kütle dizini, ortalama kan basıncı ve altı kan serum ölçümlerini içerir. `y_df`Pandas veri çerçevesi hedef değişkendir. Taban çizgisinden sonraki bir yılda bir yıllık bir nicelik ölçüsünün nicelik ölçüsünü içerir. Veri çerçevesi 442 kayıt içerir.

### <a name="train-the-model"></a>Modeli eğitme

Not defterinizde yeni bir *kod hücresi* oluşturun. Ardından aşağıdaki kodu kopyalayın ve hücreye yapıştırın. Bu kod parçacığı, bir Ridge regresyon modeli oluşturur ve Python Pickle biçimini kullanarak modeli seri hale getirir.

```python
import joblib
from sklearn.linear_model import Ridge

model = Ridge().fit(X_df,y_df)
joblib.dump(model, 'sklearn_regression_model.pkl')
```

### <a name="register-the-model"></a>Modeli Kaydet

Model dosyasının içeriğinin yanı sıra, kayıtlı modeliniz meta verileri de depolar. Meta veriler model açıklaması, Etiketler ve çerçeve bilgilerini içerir. 

Meta veriler, çalışma alanınızda modelleri yönetirken ve dağıttığınızda yararlıdır. Örneğin, etiketleri kullanarak, çalışma alanınızdaki modelleri listelediğinde modellerinizi kategorilere ayırabilirsiniz ve filtre uygulayabilirsiniz. Ayrıca, bu modeli scikit-öğren çerçevesi ile işaretlerseniz, Web hizmeti olarak dağıtmak kolaylaşır.

Aşağıdaki kodu kopyalayın ve Not defterinize yeni bir *kod hücresine* yapıştırın.

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

Ayrıca modeli Azure Machine Learning Studio de görüntüleyebilirsiniz. Soldaki menüde **modeller**' i seçin:

:::image type="content" source="media/tutorial-power-bi/model.png" alt-text="Bir modelin nasıl görüntüleneceğini gösteren ekran görüntüsü.":::

## <a name="define-the-scoring-script"></a>Puanlama betiğini tanımlama

Power BI ile tümleştirilebilen bir model dağıttığınızda, bir Python *Puanlama betiği* ve özel ortam tanımlamanız gerekir. Puanlama betiği iki işlev içerir:

- `init()`İşlev, hizmet başladığında çalışır. Modeli yükler (model kayıt defterinden otomatik olarak indirilir) ve onu yeniden serileştirir.
- `run(data)`İşlevi, hizmet çağrısı puanlanması gereken giriş verilerini içerdiğinde çalışır. 

>[!NOTE]
> Aşağıdaki kodda bulunan Python Dekoratörleri, Power BI tümleştirme açısından önemli olan giriş ve çıkış verilerinin şemasını tanımlar.

Aşağıdaki kodu kopyalayın ve Not defterinize yeni bir *kod hücresine* yapıştırın. Aşağıdaki kod parçacığında, kodu *Score.py* adlı bir dosyaya yazan bir hücre Magic vardır.

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
    # You can return any data type, as long as it can be serialized by JSON.
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

### <a name="define-the-custom-environment"></a>Özel ortamı tanımlama

Ardından, modeli skor ortamını tanımlayın. Ortamında, Puanlama betiğinin (*Score.py*) gerektirdiği Pandas ve scikit-öğren gibi Python paketlerini tanımlayın.

Ortamı tanımlamak için aşağıdaki kodu kopyalayın ve Not defterinize yeni bir *kod hücresine* yapıştırın.

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

Modeli dağıtmak için aşağıdaki kodu kopyalayın ve Not defterinize yeni bir *kod hücresine* yapıştırın:

```python
service_name = 'my-diabetes-model'

service = Model.deploy(ws, service_name, [model], inference_config, overwrite=True)
service.wait_for_deployment(show_output=True)
```

>[!NOTE]
> Hizmetin dağıtılması 2 ila 4 dakika sürebilir.

Hizmet başarıyla dağıtıyor, aşağıdaki çıktıyı görmeniz gerekir:

```txt
Tips: You can try get_logs(): https://aka.ms/debugimage#dockerlog or local deployment: https://aka.ms/debugimage#debug-locally to debug if deployment takes longer than 10 minutes.
Running......................................................................................
Succeeded
ACI service creation operation finished, operation "Succeeded"
```

Ayrıca, hizmeti Azure Machine Learning Studio da görüntüleyebilirsiniz. Soldaki menüde **uç noktalar**' ı seçin:

:::image type="content" source="media/tutorial-power-bi/endpoint.png" alt-text="Hizmetin nasıl görüntüleneceğini gösteren ekran görüntüsü.":::

Web hizmetini, beklendiği gibi çalıştığından emin olmak için test etmenizi öneririz. Not defterinizi döndürmek için, sol taraftaki menüde, Azure Machine Learning Studio ' de, **Not defterleri**' ni seçin. Ardından, hizmeti test etmek için aşağıdaki kodu kopyalayın ve Not defterinize yeni bir *kod hücresine* yapıştırın.

```python
import json

input_payload = json.dumps({
    'data': X_df[0:2].values.tolist()
})

output = service.run(input_payload)

print(output)
```

Çıktı şu JSON yapısına benzer olmalıdır: `{'predict': [[205.59], [68.84]]}` .

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Power BI tarafından tüketilebilmesi için bir modeli nasıl derleyip dağıtacağınızı gördünüz. Bir sonraki bölümde, bu modeli bir Power BI raporunda kullanmayı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Öğretici: Power BI model kullanma](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
