---
title: "Öğretici: Python'da ilk Azure ML modelinizi eğitin"
titleSuffix: Azure Machine Learning
description: Bu eğitimde, Azure Machine Learning'deki temel tasarım modellerini öğrenir ve diyabet veri kümesine dayalı basit bir scikit-learn modeli eğitin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 02/10/2020
ms.openlocfilehash: aa90655ecb14abe38ec8fdfc6c18e7d292abbef3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238681"
---
# <a name="tutorial-train-your-first-ml-model"></a>Öğretici: Tren ilk ML modeli

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu öğretici, **iki bölümden oluşan bir öğretici serisinin ikinci bölümüdür**. Önceki öğreticide, [bir çalışma alanı oluşturdunuz ve bir geliştirme ortamı seçtiniz.](tutorial-1st-experiment-sdk-setup.md) Bu eğitimde, Azure Machine Learning'deki temel tasarım modellerini öğrenir ve diyabet veri kümesine dayalı basit bir scikit-learn modeli eğitin. Bu öğreticiyi tamamladıktan sonra, daha karmaşık deneyler ve iş akışları geliştirmeye kadar ölçeklendirmek için SDK'nın pratik bilgisine sahip olabilirsiniz.

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Çalışma alanınızı bağlayın ve bir deneme oluşturun
> * Veri yükleme ve tren scikit-öğren modelleri
> * Seminer sonuçlarını portalda görüntüleme
> * En iyi modeli alma

## <a name="prerequisites"></a>Ön koşullar

Tek ön koşul bu öğretici, Kurulum ortamı [ve çalışma alanı](tutorial-1st-experiment-sdk-setup.md)nın bir bölümünü çalıştırmaktır.

Öğreticinin bu bölümünde, jupyter dizüstü bilgisayar *öğreticiler/create-first-ml-experiment/tutorial-1st-experiment-sdk-train.ipynb* bölümünde açılan örnek kodu çalıştırın. Bu makale, not defterindeki kodun üzerinden geçer.

## <a name="open-the-notebook"></a>Not defterini açma

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com/)oturum açın.

1. Birinci [bölümde](tutorial-1st-experiment-sdk-setup.md#open)gösterildiği gibi **öğretici-1st-experiment-sdk-train.ipynb** dosyanızda açın.


> [!Warning]
> Jupyter arayüzünde *yeni* bir dizüstü bilgisayar **oluşturmayın!** Not defteri *öğreticileri/create-first-ml-experiment/tutorial-1st-experiment-sdk-train.ipynb* bu öğretici için **gerekli tüm kod ve verileri** içerir.

## <a name="connect-workspace-and-create-experiment"></a>Çalışma alanını bağlayın ve deneme oluşturun

> [!Important]
> Bu makalenin geri kalanı, not defterinde gördüğünüz le aynı içeriği içerir.  
>
> Kodu çalıştırırken okumak istiyorsanız hemen Jupyter not defterine geçin. 
> Not defterinde tek bir kod hücresini çalıştırmak için kod hücresini tıklatın ve **Shift+Enter'a**basın. Veya tüm dizüstü bilgisayarı üst araç çubuğundan **Çalıştır'ı** seçerek tüm dizüstü bilgisayarı çalıştırın.

Sınıfı `Workspace` içeri aktarın ve abonelik bilgilerinizi `config.json` dosyadan `from_config().` bu işlevi kullanarak yükleyin Bu varsayılan olarak geçerli dizindeki JSON dosyasını arar, `from_config(path="your/file/path")`ancak dosyayı kullanarak işaret etmek için bir yol parametresi de belirtebilirsiniz. Bulut not defteri sunucusunda, dosya otomatik olarak kök dizinindedir.

Aşağıdaki kod ek kimlik doğrulaması isterse, bağlantıyı bir tarayıcıya yapıştırın ve kimlik doğrulama belirteci girin.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

Şimdi çalışma alanınızda bir deneme oluşturun. Deneme, deneme koleksiyonunu (tek tek model çalıştırmaları) temsil eden başka bir temel bulut kaynağıdır. Bu öğreticide, azure machine learning stüdyosunda çalıştırmalar oluşturmak ve model eğitiminizi izlemek için denemeyi kullanırsınız. Parametreler çalışma alanı başvurunuzu ve deneme için bir dize adını içerir.


```python
from azureml.core import Experiment
experiment = Experiment(workspace=ws, name="diabetes-experiment")
```

## <a name="load-data-and-prepare-for-training"></a>Verileri yükleyin ve eğitime hazırlanın

Bu öğretici için, diyabet hastalığının ilerlemesini tahmin etmek için yaş, cinsiyet ve BMI gibi özellikleri kullanan diyabet veri kümesini kullanırsınız. [Azure Açık Veri Kümeleri](https://azure.microsoft.com/services/open-datasets/) sınıfındaki verileri yükleyin ve kullanarak `train_test_split()`eğitim ve test kümelerine bölün. Bu işlev verileri ayırır, böylece model, eğitimden sonra test etmek için kullanılacak görünmeyen verilere sahiptir.


```python
from azureml.opendatasets import Diabetes
from sklearn.model_selection import train_test_split

x_df = Diabetes.get_tabular_dataset().to_pandas_dataframe().dropna()
y_df = x_df.pop("Y")

X_train, X_test, y_train, y_test = train_test_split(x_df, y_df, test_size=0.2, random_state=66)
```

## <a name="train-a-model"></a>Modeli eğitme

Basit bir scikit-learn modelini küçük ölçekli eğitim için yerel olarak kolayca eğitim yapılabilir, ancak düzinelerce farklı özellik permütasyonu ve hiperparametre ayarlarıyla birçok yineleme yitirdiğinizde, hangi modelleri eğittiğinizi ve nasıl eğittiğinizi kolayca gözden kaçırabilirsiniz. onları eğitti. Aşağıdaki tasarım deseni, buluttaki eğitiminizi kolayca izlemek için SDK'dan nasıl yararlanabileceğinizi gösterir.

Sırt modellerini farklı hiperparametre alfa değerleri arasında bir döngü içinde eğiten bir komut dosyası oluşturun.


```python
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.externals import joblib
import math

alphas = [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0]

for alpha in alphas:
    run = experiment.start_logging()
    run.log("alpha_value", alpha)

    model = Ridge(alpha=alpha)
    model.fit(X=X_train, y=y_train)
    y_pred = model.predict(X=X_test)
    rmse = math.sqrt(mean_squared_error(y_true=y_test, y_pred=y_pred))
    run.log("rmse", rmse)

    model_name = "model_alpha_" + str(alpha) + ".pkl"
    filename = "outputs/" + model_name

    joblib.dump(value=model, filename=filename)
    run.upload_file(name=model_name, path_or_stream=filename)
    run.complete()
```

Yukarıdaki kod aşağıdakileri gerçekleştirir:

1. Dizideki `alphas` her alfa hiperparametre değeri için deneme içinde yeni bir çalıştırma oluşturulur. Alfa değeri, her çalıştırma arasında ayrım yapmak için günlüğe kaydedilir.
1. Her çalıştırmada, bir Ridge modeli anlık, eğitimli ve tahminleri çalıştırmak için kullanılır. Kök ortalaması-kareli hata, gerçek ve öngörülen değerler için hesaplanır ve sonra çalıştır'a kaydedilir. Bu noktada çalışma hem alfa değeri hem de rmse doğruluğu için bağlı meta veri vardır.
1. Ardından, her çalıştırma için model seri hale getirilir ve çalıştırmaya yüklenir. Bu, portaldaki çalıştırmadan model dosyasını indirmenizi sağlar.
1. Her yinelemenin sonunda çalışma çağrılar `run.complete()`ekilerek tamamlanır.

Eğitim tamamlandıktan sonra, `experiment` portaldaki denemeye bir bağlantı getirmek için değişkeni arayın.

```python
experiment
```

<table style="width:100%"><tr><th>Adı</th><th>Çalışma alanı</th><th>Rapor Sayfası</th><th>Dokümanlar Sayfası</th></tr><tr><td>diyabet deneyi</td><td>çalışma alanı-adı</td><td>Azure portalına bağlantı</td><td>Belgelere Bağlantı</td></tr></table>

## <a name="view-training-results-in-portal"></a>Eğitim sonuçlarını portalda görüntüleyin

Azure **bağlantısı portalı'nı** takip etmek sizi ana deneme sayfasına götürür. Burada deneydeki tüm bireysel koşuları görüyorsunuz. Özel günlüğe kaydedilmiş`alpha_value` değerler `rmse`(ve bu durumda) her çalıştırma için alan haline gelir ve deneme sayfasının üst kısmındaki grafikler ve kutucuklar için de kullanılabilir hale gelir. Bir grafiğe veya döşemeye günlüğe kaydedilmiş bir metrik eklemek için, grafiğin üzerine tıklayın, edit düğmesini tıklatın ve özel günlüğe kaydedilmiş metrikünüzü bulun.

Yüzlerce ve binlerce ayrı çalıştırmaüzerinde ölçekte eğitim modelleri incelendiğinde, bu sayfa, eğittiğiniz her modeli, özellikle nasıl eğitildiğini ve benzersiz ölçümlerinizin zaman içinde nasıl değiştiğini görmenizi kolaylaştırır.

![Portal'da Ana Deney sayfası](./media/tutorial-1st-experiment-sdk-train/experiment-main.png)

Sütundaki bir çalıştırma numarası `RUN NUMBER` bağlantısına tıkladığınızda, her bir çalışma için sayfaya gidin. Varsayılan sekme **Ayrıntılar,** her çalıştırmada daha ayrıntılı bilgiler gösterir. **Çıktılar** sekmesine gidin ve her `.pkl` eğitim yinelemesi sırasında çalıştırılan modelin dosyasını görürsünüz. Burada model dosyasını el ile yeniden eğitmek yerine indirebilirsiniz.

![Portal'da ayrıntılar sayfasını çalıştır](./media/tutorial-1st-experiment-sdk-train/model-download.png)

## <a name="get-the-best-model"></a>En iyi modeli alın

Portaldaki denemeden model dosyalarını indirebilmeek, bunları programlı olarak da indirebilirsiniz. Aşağıdaki kod, denemedeki her çalıştırmada yinelenir ve hem günlüğe kaydedilmiş çalıştırma ölçümlerine hem de çalıştırma ayrıntılarına (run_id içerir) erişir. Bu, en iyi çalıştırmayı izler, bu durumda en düşük kök ortalaması-kareli hata ile çalıştırın.

```python
minimum_rmse_runid = None
minimum_rmse = None

for run in experiment.get_runs():
    run_metrics = run.get_metrics()
    run_details = run.get_details()
    # each logged metric becomes a key in this returned dict
    run_rmse = run_metrics["rmse"]
    run_id = run_details["runId"]

    if minimum_rmse is None:
        minimum_rmse = run_rmse
        minimum_rmse_runid = run_id
    else:
        if run_rmse < minimum_rmse:
            minimum_rmse = run_rmse
            minimum_rmse_runid = run_id

print("Best run_id: " + minimum_rmse_runid)
print("Best run_id rmse: " + str(minimum_rmse))
```

    Best run_id: 864f5ce7-6729-405d-b457-83250da99c80
    Best run_id rmse: 57.234760283951765

Deneme nesnesi ile birlikte `Run` oluşturucuyu kullanarak tek tek çalıştırmayı getirmek için en iyi çalıştırılaç kimliğini kullanın. Ardından, `get_file_names()` bu çalıştırmadan indirilebilecek tüm dosyaları görmek için arayın. Bu durumda, eğitim sırasında her çalıştırma için yalnızca bir dosya yüklemiş siniz.

```python
from azureml.core import Run
best_run = Run(experiment=experiment, run_id=minimum_rmse_runid)
print(best_run.get_file_names())
```

    ['model_alpha_0.1.pkl']

İndirmek için model dosya adını belirterek çalıştır nesnesini arayın. `download()` Varsayılan olarak bu işlev geçerli dizine indirilir.

```python
best_run.download_file(name="model_alpha_0.1.pkl")
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure Machine Learning eğitimlerini çalıştırmayı planlıyorsanız bu bölümü tamamlamayın.

### <a name="stop-the-compute-instance"></a>İşlem örneğini durdurma

[!INCLUDE [aml-stop-server](../../includes/aml-stop-server.md)]

### <a name="delete-everything"></a>Her şeyi silme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

Kaynak grubunu da tutabilir, ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevleri yaptınız:

> [!div class="checklist"]
> * Çalışma alanınızı bağladı ve bir deneme oluşturdu
> * Yüklü veri ve eğitimli scikit-learn modelleri
> * Portalda görüntülenen eğitim sonuçları ve alınan modeller

Azure Machine Learning ile [modelinizi dağıtın.](tutorial-deploy-models-with-aml.md)
Otomatik makine [öğrenimi](tutorial-auto-train-models.md) deneylerini nasıl geliştireceklerini öğrenin.
