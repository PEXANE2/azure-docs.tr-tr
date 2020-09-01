---
title: Modelleri izlemeyi ve dağıtımı deneyin
titleSuffix: Azure Data Science Virtual Machine
description: Denemeleri ve/veya MLFlow Azure Machine Learning ile DSVM 'deki izleme ve günlüğe kaydetme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: samkemp
ms.author: samkemp
ms.topic: conceptual
ms.date: 07/17/2020
ms.openlocfilehash: 943e8bd9f272f3dc8cefbfbccd326cf520497bb2
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89146904"
---
# <a name="track-experiments-and-deploy-models-in-azure-machine-learning"></a>Azure Machine Learning ' de izleme denemeleri ve modelleri dağıtma

Denemeleri ve izleme çalıştırma ölçümlerinizi izleyerek model oluşturma sürecini geliştirin. Bu makalede, [Mlflow](https://mlflow.org/) API 'sini kullanarak eğitim betiğe günlük kodu eklemeyi öğrenin ve Azure Machine Learning deneme deneyimini izleyin.

Aşağıdaki diyagramda, MLflow Izlemenin yanı sıra, Azure Machine Learning çalışma alanınızda bir deneyin çalışma ölçümlerini ve mağaza modeli yapılarını izlersiniz.

![denemeleri izleme](./media/how-to-track-experiments/mlflow-diagram-track.png)

## <a name="prerequisites"></a>Önkoşullar

* [Bir Azure Machine Learning çalışma alanı](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace#create-a-workspace) sağlamanız gerekir

## <a name="create-a-new-notebook"></a>Yeni not defteri oluşturma

Azure Machine Learning ve MLFlow SDK Veri Bilimi VM'si önceden yüklenir ve **azureml_py36_ \* ** Conda ortamından erişilebilir. Jupyterlab içinde, başlatıcı 'ye tıklayın ve aşağıdaki çekirdeği seçin:

![çekirdek seçimi](./media/how-to-track-experiments/experiment-tracking-1.png)

## <a name="set-up-the-workspace"></a>Çalışma alanını ayarlama

[Azure Portal](https://portal.azure.com) gidin ve önkoşulların bir parçası olarak sağladığınız çalışma alanını seçin. __Karşıdan yükleme config.js__ görürsünüz (aşağıya bakın)-yapılandırmayı indirin ve dsvm 'deki çalışma dizininizde depolandığından emin olun.

![Yapılandırma dosyası Al](./media/how-to-track-experiments/experiment-tracking-2.png)

Yapılandırma, çalışma alanı adı, abonelik vb. gibi bilgileri içerir ve bu parametreleri sabit olarak kodistemediğiniz anlamına gelir.

## <a name="track-dsvm-runs"></a>DSVM çalıştırmalarını izle

AzureML çalışma alanı nesnesini ayarlamak için Not defterinize (veya betiğe) aşağıdaki kodu ekleyin.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
İzleme URI 'SI saat veya daha az bir saate kadar geçerlidir. Bir boşta kalma zamanından sonra betiğinizi yeniden başlatırsanız, yeni bir URI almak için get_mlflow_tracking_uri API 'sini kullanın.

### <a name="load-the-data"></a>Verileri yükleme

Bu örnek, scikit-öğren ile birlikte gelen iyi bilinen küçük bir veri kümesi olan Diabetes veri kümesini kullanır. Bu hücre, veri kümesini yükler ve rastgele eğitim ve test kümelerine böler.

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
from sklearn.externals import joblib

X, y = load_diabetes(return_X_y = True)
columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
data = {
    "train":{"X": X_train, "y": y_train},
    "test":{"X": X_test, "y": y_test}
}

print ("Data contains", len(data['train']['X']), "training samples and",len(data['test']['X']), "test samples")
```

### <a name="add-tracking"></a>İzleme Ekle

Azure Machine Learning SDK kullanarak deneme izleme ekleyin ve deneme çalıştırması kaydına kalıcı bir model yükleyin. Aşağıdaki kod Günlükler ekler ve bir model dosyasını deneme çalıştırmasına yükler. Model de Azure Machine Learning model kayıt defterine kaydedilir.

```python
# Get an experiment object from Azure Machine Learning
from azureml.mlflow import register_model
experiment_name = 'experiment_with_mlflow'
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    # Log the algorithm parameter alpha to the run
    mlflow.log_param('alpha', 0.03)

    # Create, fit, and test the scikit-learn Ridge regression model
    regression_model = Ridge(alpha=0.03)
    regression_model.fit(data['train']['X'], data['train']['y'])
    preds = regression_model.predict(data['test']['X'])

    # Output the Mean Squared Error to the notebook and to the run
    print('Mean Squared Error is', mean_squared_error(data['test']['y'], preds))
    mlflow.log_metric('mse', mean_squared_error(data['test']['y'], preds))

    # Save the model
    model_file_name = 'model.pkl'
    joblib.dump(value = regression_model, filename = model_file_name)

    # upload the model file explicitly into artifacts
    mlflow.log_artifact(model_file_name)
    # register the model
    register_model(mlflow.active_run(), 'diabetes_model', 'model.pkl', model_framework="ScikitLearn")
```

### <a name="view-runs-in-azure-machine-learning"></a>Azure Machine Learning çalıştırmaları görüntüle

Deneme çalıştırmasının [Azure Machine Learning Studio](https://ml.azure.com)içinde görüntüleyebilirsiniz. Sol taraftaki menüden __denemeleri__ ' a tıklayın ve ' experiment_with_mlflow ' seçin (ya da denemeyi Yukarıdaki kod parçacığında farklı şekilde adlandırın, kullanılan ada tıklayın):

![deneme seçin](./media/how-to-track-experiments/mlflow-experiments.png)

Günlüğe kaydedilen ortalama kare hatası (MSE) görmeniz gerekir:

![MSE](./media/how-to-track-experiments/mlflow-experiments-2.png)

Çalıştırmaya tıkladığınızda diğer ayrıntılar ve ayrıca __çıkışlar + günlüklerde__ bulunan Pickle modeli görüntülenir

## <a name="deploy-model-in-azure-machine-learning"></a>Modeli Azure Machine Learning dağıt

Bu bölümde, bir DSVM üzerinde eğitilen modellerin Azure Machine Learning için nasıl dağıtılacağı ana hatlarıyla anlatılmaktadır.

### <a name="step-1-create-inference-compute"></a>1. Adım: çıkarım Işlem oluşturma

[AzureML](https://ml.azure.com) için sol taraftaki menüde __işlem__ ' e ve ardından __çıkarım kümeleri__ sekmesine tıklayın. Sonra, aşağıda belirtildiği gibi __+ Yeni__ seçeneğine tıklayın:

![Çıkarım Işlem oluşturma](./media/how-to-track-experiments/mlflow-experiments-6.png)

__Yeni çıkarım kümesi__ bölmesinde ayrıntıları buraya girin:

* İşlem adı
* Kubernetes hizmeti-yeni oluştur seçeneğini belirleyin
* Bölgeyi seçin
* VM boyutunu seçin (Bu öğreticinin amaçları doğrultusunda Standard_D3_v2 için varsayılan değer yeterlidir)
* Küme amacı- __geliştirme-test__ ' i seçin
* Düğüm sayısı __1__ ' e eşit olmalıdır
* Ağ yapılandırması-temel

Sonra __Oluştur__' a tıklayın.

![işlem ayrıntıları](./media/how-to-track-experiments/mlflow-experiments-7.png)

### <a name="step-2-deploy-no-code-inference-service"></a>2. Adım: dağıtım No-Code çıkarımı hizmeti

Modeli kodumuza kaydettiğimiz zaman, `register_model` Framework 'ü sköğren olarak belirttik. Azure Machine Learning, aşağıdaki çerçeveler için kod dağıtımı desteklemiyor:

* scikit-learn
* TensorFlow SaveModel biçimi
* ONNX model biçimi

Kod olmadan dağıtım, belirli bir Puanlama betiği belirtmeye gerek kalmadan model yapıtını doğrudan dağıtabileceğiniz anlamına gelir.

Diabetes modelini dağıtmak için [Azure Machine Learning Studio](https://ml.azure.com) Sol menüye gidin ve __modeller__' ı seçin. Sonra, kayıtlı diabetes_model ' ye tıklayın:

![Model Seç](./media/how-to-track-experiments/mlflow-experiments-3.png)

Ardından, model ayrıntıları bölmesindeki __Dağıt__ düğmesine tıklayın:

![Dağıtma](./media/how-to-track-experiments/mlflow-experiments-4.png)

Modeli, 1. adımda oluşturduğumuz çıkarım kümesine (Azure Kubernetes hizmeti) dağıtacağız. Hizmet için bir ad ve AKS işlem kümesinin adı (adım 1 ' de oluşturulur) sağlayarak aşağıdaki ayrıntıları girin. Ayrıca __CPU Reserve kapasitesini__ 1 ' e (0,1 ' den) ve __bellek ayırma kapasitesini__ 1 ' e (0,5 ' den) artırmanız önerilir. bunu, __Gelişmiş__ ' e tıklayıp Ayrıntıları doldurarak yapabilirsiniz. Ardından __Dağıt__' a tıklayın.

![Dağıtım ayrıntıları](./media/how-to-track-experiments/mlflow-experiments-5.png)

### <a name="step-3-consume"></a>3. Adım: tüketme

Model başarıyla dağıtıldığında, aşağıdakileri görmeniz gerekir (Bu sayfaya ulaşmak için sol taraftaki menüden uç noktalara tıklayın > sonra dağıtılan hizmetin adına tıklayın):

![Modeli tüketme](./media/how-to-track-experiments/mlflow-experiments-8.png)

Dağıtım durumunun __sağlıklı__duruma geçme aşamasından __kaynaklandığına__ dikkat etmelisiniz. Bu ayrıntılar bölümünde, bir uygulama geliştiricisinin ML modelinizi uygulamalarıyla tümleştirmeleri için kullanabileceği REST uç noktası ve Swagger URL 'Leri de sağlanmaktadır.

Uç noktayı [Postman](https://www.postman.com/)kullanarak test edebilirsiniz, alternatif olarak, AzureML SDK 'sını kullanabilirsiniz:

```python
from azureml.core import Webservice
import json

# if you called your service differently then change the name below
service = Webservice(ws, name="diabetes-service")

input_payload = json.dumps({
    'data': X_test[0:2].tolist(),
    'method': 'predict'  # If you have a classification model, you can get probabilities by changing this to 'predict_proba'.
})

output = service.run(input_payload)

print(output)
```

### <a name="step-4-clean-up"></a>4. Adım: Temizleme

Adım 1 ' de oluşturduğunuz çıkarım işlemi, devam eden işlem ücretlerine tabi kalmayacak şekilde silmelisiniz. Azure Machine Learning Studio sol taraftaki menüde, Işlem > çıkarım kümelerine tıklayın > işlem > Sil ' i seçin.

## <a name="next-steps"></a>Sonraki Adımlar

* [AzureML için model dağıtma](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where) hakkında daha fazla bilgi edinin
