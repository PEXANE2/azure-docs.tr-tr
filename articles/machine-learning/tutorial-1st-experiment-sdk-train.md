---
title: "Öğretici: Python 'da ilk Azure ML modelinizi eğitme"
titleSuffix: Azure Machine Learning
description: Bu öğreticide Azure Machine Learning temel tasarım düzenlerini öğrenir ve diabetes veri kümesine bağlı olarak basit bir scikit-öğrenme modeli eğitirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 02/10/2020
ms.openlocfilehash: c8f259d2d4df46470a042c3f65ac1b8e1f66b1dd
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80546049"
---
# <a name="tutorial-train-your-first-ml-model"></a>Öğretici: ilk ML modelinizi eğitme

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu öğretici, **iki bölümden oluşan bir öğretici serisinin ikinci bölümüdür**. Önceki öğreticide [bir çalışma alanı oluşturdunuz ve bir geliştirme ortamı seçtiniz](tutorial-1st-experiment-sdk-setup.md). Bu öğreticide Azure Machine Learning temel tasarım düzenlerini öğrenir ve diabetes veri kümesine bağlı olarak basit bir scikit-öğrenme modeli eğitirsiniz. Bu Öğreticiyi tamamladıktan sonra, daha karmaşık denemeleri ve iş akışları geliştirmek üzere ölçeklenebilmeniz için SDK 'nın pratik bilgisine sahip olursunuz.

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Çalışma alanınızı bağlayın ve bir deneme oluşturun
> * Veri yükleme ve scikit 'i eğitme-modelleri öğrenme
> * Eğitim sonuçlarını Studio 'da görüntüleme
> * En iyi modeli alma

## <a name="prerequisites"></a>Ön koşullar

Tek önkoşul, bu öğreticiden birini, [kurulum ortamını ve çalışma alanını](tutorial-1st-experiment-sdk-setup.md)çalıştırmanın bir parçasıdır.

Öğreticinin bu bölümünde, birinci bölüm sonunda açılan örnek Jupyter Not defteri *öğreticileri/Create-First-ml-Experiment/tutorial-1st-Experiment-SDK-train. ipynb* 'de kodu çalıştırın. Bu makalede, not defterindeki aynı koda adım adım yol gösterilir.

## <a name="open-the-notebook"></a>Not defterini açın

1. [Azure Machine Learning Studio](https://ml.azure.com/)'da oturum açın.

1. [Bir kısım](tutorial-1st-experiment-sdk-setup.md#open)içinde gösterildiği gibi, klasörünüzde **-1. deneme-SDK-tren. ipynb öğreticisini** açın.


> [!Warning]
> Jupyter arabiriminde *Yeni* bir Not **defteri oluşturmayın!** Bu öğretici için **gereken tüm kod ve verileri** içeren Not defteri *öğreticileri/Create-First-ml-Experiment/tutorial-1st-Experiment-SDK-train. ipynb* .

## <a name="connect-workspace-and-create-experiment"></a>Çalışma alanını bağlama ve deneme oluşturma

> [!Important]
> Bu makalenin geri kalanında not defterinde gördüğünüz içerikle aynı içerik yer almaktadır.  
>
> Kodu çalıştırırken okumak istiyorsanız, Jupyter not defterine şimdi geçin. 
> Bir not defterinde tek bir kod hücresini çalıştırmak için, kod hücresine tıklayın ve **SHIFT + enter**tuşuna basın. Ya da tüm not defteri ' ni üstteki araç çubuğundan **Çalıştır** ' ı seçerek çalıştırın.

`Workspace` Sınıfını içeri aktarın ve bu işlevi `config.json` `from_config().` kullanarak ABONELIK bilgilerinizi varsayılan olarak geçerli dizindeki json dosyasını arar, ancak kullanarak `from_config(path="your/file/path")`dosyayı işaret etmek için bir yol parametresi de belirtebilirsiniz. Bir bulut Not defteri sunucusunda, dosya otomatik olarak kök dizinde bulunur.

Aşağıdaki kod ek kimlik doğrulaması isterse, bağlantıyı bir tarayıcıya yapıştırmanız ve kimlik doğrulama belirtecini girmeniz yeterlidir.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

Şimdi çalışma alanınızda bir deneme oluşturun. Deneme, deneme koleksiyonunu temsil eden başka bir temel bulut kaynağıdır (bireysel model çalıştırmaları). Bu öğreticide, çalıştırma oluşturmak ve Azure Machine Learning Studio 'daki model öğreticinizi izlemek için denemeyi kullanırsınız. Parametreler, çalışma alanı başvurunuz ve deneme için bir dize adı içerir.


```python
from azureml.core import Experiment
experiment = Experiment(workspace=ws, name="diabetes-experiment")
```

## <a name="load-data-and-prepare-for-training"></a>Veri yükleme ve eğitim için hazırlanma

Bu öğreticide, diabetes ilerlemesini tahmin etmek için Age, cinsiyet ve BMI gibi özellikleri kullanan diabetes veri kümesini kullanırsınız. [Azure Open DataSet](https://azure.microsoft.com/services/open-datasets/) sınıfından verileri yükleyin ve kullanarak `train_test_split()`eğitim ve test kümelerine ayırın. Bu işlev, modeli aşağıdaki eğitimin test edilmesi için kullanılacak görülmeyen verileri içerecek şekilde ayırır.


```python
from azureml.opendatasets import Diabetes
from sklearn.model_selection import train_test_split

x_df = Diabetes.get_tabular_dataset().to_pandas_dataframe().dropna()
y_df = x_df.pop("Y")

X_train, X_test, y_train, y_test = train_test_split(x_df, y_df, test_size=0.2, random_state=66)
```

## <a name="train-a-model"></a>Modeli eğitme

Basit bir scikit-eğitim modeli, küçük ölçekli eğitim için kolayca yerel olarak yapılabilir, ancak onlarca farklı özellik bilimiyle ve hiper parametre ayarları sayesinde çok sayıda yineleme eğitmeniz halinde, eğitilen modelleri ve bunları nasıl eğititireceğinizi kolayca kaybetmezsiniz. Aşağıdaki tasarım modelinde, bulutta öğreticinizi kolayca izlemek için SDK 'nın nasıl kullanılacağı gösterilmektedir.

Farklı hiper parametre alfa değerleri aracılığıyla bir döngüdeki modelleri gösteren bir betik oluşturun.


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

Yukarıdaki kod şunları gerçekleştirir:

1. `alphas` Dizideki her Alfa hiper parametre değeri için, deneme içinde yeni bir çalıştırma oluşturulur. Alfa değeri, her çalıştırma arasında ayrım yapmak için günlüğe kaydedilir.
1. Her çalıştırmada, bir Ridge modeli örneği oluşturulur, eğitilmiş ve tahmin çalıştırmaları için kullanılır. Kök-ortalama-kare-hatası, fiili ve tahmin edilen değerler için hesaplanır ve ardından çalıştırmaya kaydedilir. Bu noktada, çalışma için hem alfa değeri hem de rmo doğruluğu için eklenmiş meta veriler vardır.
1. Ardından, her bir çalıştırmaya ait model serileştirilir ve çalıştırmaya yüklenir. Bu, model dosyasını Studio 'daki çalıştırağından indirmelerini sağlar.
1. Her yinelemenin sonunda çalıştırma, çağırarak `run.complete()`tamamlanır.

Eğitim tamamlandıktan sonra, Studio 'da deneme için `experiment` bir bağlantı getirmek üzere değişkenini çağırın.

```python
experiment
```

<table style="width:100%"><tr><th>Adı</th><th>Çalışma alanı</th><th>Rapor sayfası</th><th>Docs sayfası</th></tr><tr><td>Diabetes-deneme</td><td>çalışma alanınızın adı</td><td>Azure Machine Learning Studio 'ya bağlantı</td><td>Belge bağlantısı</td></tr></table>

## <a name="view-training-results-in-studio"></a>Studio 'da eğitim sonuçlarını görüntüleme

**Azure Machine Learning Studio bağlantısını** takip etmek sizi ana deneme sayfasına götürür. İşte denemenize ait tüm bireysel çalıştırmaları görürsünüz. Tüm özel günlüğe kaydedilmiş değerler (`alpha_value` ve `rmse`bu durumda) her çalıştırma için alanlar haline gelir ve ayrıca deneme sayfasının en üstündeki grafikler ve kutucuklar için kullanılabilir hale gelir. Bir grafiğe veya kutucuğa günlüğe kaydedilmiş bir ölçüm eklemek için, üzerine gelin, Düzenle düğmesine tıklayın ve özel olarak günlüğe kaydedilen ölçümünüzün bulun.

Eğitim modelleri, yüzlerce ve binlerce ayrı çalıştırma üzerinde ölçeklendirirseniz, Bu sayfa, eğitilen her modeli, özellikle eğitilen ve benzersiz ölçümlerinizin zaman içinde nasıl değiştiğini görmenizi kolaylaştırır.

:::image type="content" source="./media/tutorial-1st-experiment-sdk-train/experiment-main.png" alt-text="Studio 'da ana deneme sayfası.":::


Tek bir çalıştırmanın sayfasını görmek için `RUN NUMBER` sütununda bir çalıştırma numarası bağlantısı seçin. Varsayılan sekme **ayrıntıları** , her çalıştırma hakkında daha ayrıntılı bilgi gösterir. **Çıktılar + Günlükler** sekmesine gidin ve her eğitim yinelemesi sırasında çalıştırmaya yüklenmiş `.pkl` olan modele ait dosyayı görürsünüz. Burada, model dosyasını el ile yeniden eğitmek yerine indirebilirsiniz.

:::image type="content" source="./media/tutorial-1st-experiment-sdk-train/model-download.png" alt-text="Studio 'da Ayrıntılar sayfasını çalıştırın.":::

## <a name="get-the-best-model"></a>En iyi modeli al

Model dosyalarını Studio 'daki denemeden indirebilmenin yanı sıra, programlama yoluyla da indirebilirsiniz. Aşağıdaki kod, denemenin her bir çalıştırmasında yinelenir ve hem günlüğe kaydedilen çalışma ölçümlerine hem de çalıştırma ayrıntılarına (run_id içeren) erişir. Bu, en iyi çalışmayı izler, bu durumda en düşük kök-ortalama-kare-hatası ile çalıştırılır.

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

Deneme nesnesiyle birlikte `Run` oluşturucuyu kullanarak bireysel çalıştırmayı getirmek için en ıyı çalıştırma kimliğini kullanın. Sonra bu `get_file_names()` çalıştırınızdan yüklenebilecek tüm dosyaları görmek için çağırın. Bu durumda, yalnızca eğitim sırasında her çalıştırma için bir dosya karşıya yüklenir.

```python
from azureml.core import Run
best_run = Run(experiment=experiment, run_id=minimum_rmse_runid)
print(best_run.get_file_names())
```

    ['model_alpha_0.1.pkl']

Çalıştır `download()` nesnesinde, indirilecek model dosyası adını belirterek çağırın. Varsayılan olarak, bu işlev geçerli dizine indirir.

```python
best_run.download_file(name="model_alpha_0.1.pkl")
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure Machine Learning öğreticileri çalıştırmayı planlıyorsanız, bu bölümü tamamlamayın.

### <a name="stop-the-compute-instance"></a>İşlem örneğini durdur

[!INCLUDE [aml-stop-server](../../includes/aml-stop-server.md)]

### <a name="delete-everything"></a>Her şeyi sil

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

Ayrıca, kaynak grubunu koruyabilir ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevleri yaptınız:

> [!div class="checklist"]
> * Çalışma alanınızı bağladınız ve bir deneme oluşturdunuz
> * Yüklenen veriler ve eğitimli scikit-modelleri öğrenme
> * Eğitim sonuçları Studio 'ya ve alınan modellere göre görüntülenir

[Modelinizi](tutorial-deploy-models-with-aml.md) Azure Machine Learning ile dağıtın.
[Otomatik makine öğrenimi](tutorial-auto-train-models.md) denemeleri geliştirmeyi öğrenin.
