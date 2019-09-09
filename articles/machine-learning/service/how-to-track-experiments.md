---
title: Eğitim çalıştırmaları sırasında günlük ölçümleri
titleSuffix: Azure Machine Learning service
description: Model oluşturma işlemini iyileştirmek için denemeleri 'nizi izleyebilir ve ölçümleri izleyebilirsiniz. Eğitim betiğe nasıl günlük ekleneceğini, denemeyi nasıl göndereceğinizi, çalışan bir işin ilerlemesini nasıl denetleyeceğinizi ve bir çalıştırmanın günlüğe kaydedilen sonuçlarının nasıl görüntüleneceğini öğrenin.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/11/2019
ms.custom: seodec18
ms.openlocfilehash: 0630ca28652b48b3632dbae94c5e16d6adb462c4
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70812293"
---
# <a name="track-machine-learning-training-metrics-with-azure-machine-learning"></a>Azure Machine Learning ile Machine Learning eğitim ölçümlerini izleyin

Denemeleri ve izleme ölçümlerinizi izleyerek model oluşturma işlemini geliştirin. Bu makalede, eğitim betiğe günlük kodu eklemeyi, bir deneme çalıştırması göndermeyi, çalışmayı izlemeyi ve Azure Machine Learning hizmetinde sonuçları incelemeyi öğrenin.

> [!NOTE]
> Azure Machine Learning hizmet, otomatik makine öğrenimi çalıştırmaları veya eğitim işini çalıştıran Docker kapsayıcısı gibi eğitim sırasında diğer kaynaklardaki bilgileri de günlüğe alabilir. Bu Günlükler belgelenmemiştir. Sorunlarla karşılaşırsanız ve Microsoft Destek ile iletişime geçerek, sorun giderme sırasında bu günlükleri kullanabiliyor olabilirler.

## <a name="available-metrics-to-track"></a>İzlenecek kullanılabilir ölçümler

Aşağıdaki ölçümler, bir denemeyi eğitim sırasında çalıştırılacak eklenebilir. Bir çalıştırmada izlenebilir daha ayrıntılı bir listesi görmek için bkz: [sınıfı başvuru belgeleri çalıştırma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py).

|Tür| Funkce Pythonu | Notlar|
|----|:----|:----|
|Skaler değerler |İşlev:<br>`run.log(name, value, description='')`<br><br>Örnek:<br>Run.log ("doğruluğu", 0.95) |Günlük bir sayısal veya dize değeri belirtilen ada sahip farklı çalıştır. Bir ölçüm için bir çalıştırma günlüğe kaydetme, denemeyi çalıştırma kaydı depolanması, ölçüm neden olur.  Bu ölçüm oluşan bir vektörü olarak kabul sonucu bir çalıştırma içinde birden çok kez aynı Ölçüm oturum açabilirsiniz.|
|Listeler|İşlev:<br>`run.log_list(name, value, description='')`<br><br>Örnek:<br>Run.log_list ("doğruluk" [0,6, 0,7, 0.87]) | Belirtilen ada sahip farklı çalıştır değerlerin bir listesini oturum.|
|Satır|İşlev:<br>`run.log_row(name, description=None, **kwargs)`<br>Örnek:<br>Run.log_row ("Y" X üzerinden x = 1, y 0.4 =) | Kullanarak *log_row* kwargs içinde anlatıldığı gibi birden çok sütun içeren bir ölçü oluşturur. Her adlandırılmış parametre ile belirtilen değer bir sütun oluşturur.  *log_row* rasgele bir tanımlama grubu ya da birden çok kez bir döngüde tam bir tablo oluşturmak için oturum volat pouze jednou.|
|Tablo|İşlev:<br>`run.log_table(name, value, description='')`<br><br>Örnek:<br>Run.log_table ("Y" X üzerinden {"x": [1, 2, 3], "y": [0,6, 0,7, 0.89]}) | Sözlük nesnesi, verilen ada sahip farklı çalıştır oturum açın. |
|Görüntüler|İşlev:<br>`run.log_image(name, path=None, plot=None)`<br><br>Örnek:<br>`run.log_image("ROC", plt)` | Görüntü çalıştırma kaydı için oturum açın. Bir görüntü dosyası veya bir matplotlib oturum log_image kullanın çizim farklı çalıştır.  Bu görüntüler, görünür ve çalışma kaydındaki benzer olacaktır.|
|Bir etiketi|İşlev:<br>`run.tag(key, value=None)`<br><br>Örnek:<br>run.tag (","Evet"seçeneği") | Bir dize anahtarı ve isteğe bağlı dize değeri olan çalıştırma etiketleyin.|
|Dosya veya dizin karşıya yükleme|İşlev:<br>`run.upload_file(name, path_or_stream)`<br> <br> Örnek:<br>Run.upload_file ("best_model.pkl", ". / model.pkl") | Çalıştırma kaydı için bir dosya yükleyin. Çalıştırmaları otomatik olarak varsayılan olarak belirtilen çıkış dizini dosyasında yakala ". / çıkışlar" türleri çoğu çalıştırın.  Yalnızca ek dosyalar yüklenmek üzere gerektiğinde kullanım upload_file veya bir çıktı dizini belirtilmedi. Eklemeyi önerin `outputs` BT'nin çıkış dizinine karşıya, böylece adı. Tüm ilişkili olan dosyaları listeleyebilirsiniz bu çalıştırma kaydı tarafından çağırılır `run.get_file_names()`|

> [!NOTE]
> Skalerler, listeler, satır ve tablolar için ölçümleri türü olabilir: kayan noktalı sayı, tamsayı veya dize.

## <a name="choose-a-logging-option"></a>Günlüğe kaydetme seçeneği seçin

İzlemek veya denemenizi izlemek istiyorsanız, çalıştırma gönderdiğinizde oturum başlatmak için kod eklemeniz gerekir. Çalıştırma gönderim tetiklemek için yollar şunlardır:
* __Run.start_logging__ - günlük işlevleri eğitim komut dosyanıza ekleyin ve bir etkileşimli günlüğe kaydetme oturumu içinde belirtilen denemeyi başlatın. **start_logging** dizüstü bilgisayarlar gibi senaryolarda kullanım için etkileşimli bir çalışma oluşturur. Oturumu sırasında günlüğe kaydedilen tüm ölçümler, denemeyi çalıştırma kaydı eklenir.
* __ScriptRunConfig__ - günlük işlevleri eğitim komut dosyanıza ekleyin ve Çalıştır tüm betik klasörünü yükleyemedi.  **ScriptRunConfig** betiği için yapılandırmaları ayarlamayı çalıştırmaları için bir sınıftır. Bu seçenekle tamamlanmasından almak veya izlemek için görsel bir pencere öğesi almak için izleme kodu ekleyebilirsiniz.

## <a name="set-up-the-workspace"></a>Çalışma alanını ayarlama
Günlüğe kaydetme ve deneme gönderme eklemeden önce çalışma alanını ayarlamanız gerekir.

1. Çalışma alanı yükleyin. Çalışma alanı yapılandırmasını ayarlama hakkında daha fazla bilgi için bkz. [çalışma alanı yapılandırma dosyası](how-to-configure-environment.md#workspace).

   ```python
   from azureml.core import Experiment, Run, Workspace
   import azureml.core
  
   ws = Workspace.from_config()
   ```
  
## <a name="option-1-use-start_logging"></a>Seçenek 1: Start_logging kullanma

**start_logging** dizüstü bilgisayarlar gibi senaryolarda kullanım için etkileşimli bir çalışma oluşturur. Oturumu sırasında günlüğe kaydedilen tüm ölçümler, denemeyi çalıştırma kaydı eklenir.

Aşağıdaki örnek, yerel Jupyter not defterini yerel olarak basit bir sklearn Ridge modeli eğitir. Farklı ortamlara denemeleri gönderme hakkında daha fazla bilgi için bkz. [işlem hedeflerine yönelik Azure Machine Learning hizmeti ile model eğitiminin ayarlama](https://docs.microsoft.com/azure/machine-learning/service/how-to-set-up-training-targets).

1. Bir eğitim betiği yerel bir Jupyter not defteri oluşturun. 

   ```python
   # load diabetes dataset, a well-known small dataset that comes with scikit-learn
   from sklearn.datasets import load_diabetes
   from sklearn.linear_model import Ridge
   from sklearn.metrics import mean_squared_error
   from sklearn.model_selection import train_test_split
   from sklearn.externals import joblib

   X, y = load_diabetes(return_X_y = True)
   columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
   data = {
      "train":{"X": X_train, "y": y_train},        
      "test":{"X": X_test, "y": y_test}
   }
   reg = Ridge(alpha = 0.03)
   reg.fit(data['train']['X'], data['train']['y'])
   preds = reg.predict(data['test']['X'])
   print('Mean Squared Error is', mean_squared_error(preds, data['test']['y']))
   joblib.dump(value = reg, filename = 'model.pkl');
   ```

2. Azure Machine Learning hizmeti SDK'sını kullanarak deneme izleme ekleyebilir ve kalıcı bir modeli çalıştırma kaydı denemesine yükleme. Aşağıdaki kod, etiketler, günlükleri, ekler ve denemeyi çalıştırmak için bir model dosyası yükler.

   ```python
    # Get an experiment object from Azure Machine Learning
    experiment = Experiment(workspace=ws, name="train-within-notebook")
    
    # Create a run object in the experiment
    run =  experiment.start_logging()
    # Log the algorithm parameter alpha to the run
    run.log('alpha', 0.03)
    
    # Create, fit, and test the scikit-learn Ridge regression model
    regression_model = Ridge(alpha=0.03)
    regression_model.fit(data['train']['X'], data['train']['y'])
    preds = regression_model.predict(data['test']['X'])
    
    # Output the Mean Squared Error to the notebook and to the run
    print('Mean Squared Error is', mean_squared_error(data['test']['y'], preds))
    run.log('mse', mean_squared_error(data['test']['y'], preds))
    
    # Save the model to the outputs directory for capture
    model_file_name = 'outputs/model.pkl'
    
    joblib.dump(value = regression_model, filename = model_file_name)
    
    # upload the model file explicitly into artifacts 
    run.upload_file(name = model_file_name, path_or_stream = model_file_name)
    
    # Complete the run
    run.complete()
   ```

    Komut dosyası ile biter ```run.complete()```, tamamlandı olarak çalıştırmayı işaretler.  Bu işlev, genellikle etkileşimli bir not defteri senaryolarda kullanılır.

## <a name="option-2-use-scriptrunconfig"></a>Seçenek 2: ScriptRunConfig kullanma

[**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) , betik çalıştırmaları için yapılandırmaların ayarlanmasına yönelik bir sınıftır. Bu seçenekle tamamlanmasından almak veya izlemek için görsel bir pencere öğesi almak için izleme kodu ekleyebilirsiniz.

Bu örnek, yukarıda temel sklearn Ridge modeli genişletir. Alfa değerleri ölçümleri yakalamak için model ve deneme altında çalıştığı eğitilen modeller üzerinde basit bir parametre için tarama Süpürme yapar. Örnek, bir kullanıcı tarafından yönetilen ortamında yerel olarak çalıştırılır. 

1. Bir eğitim betiği `train.py`oluşturun.

   ```python
   # train.py

   import os
   from sklearn.datasets import load_diabetes
   from sklearn.linear_model import Ridge
   from sklearn.metrics import mean_squared_error
   from sklearn.model_selection import train_test_split
   from azureml.core.run import Run
   from sklearn.externals import joblib

   import numpy as np

   #os.makedirs('./outputs', exist_ok = True)

   X, y = load_diabetes(return_X_y = True)

   run = Run.get_context()

   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
   data = {"train": {"X": X_train, "y": y_train},
          "test": {"X": X_test, "y": y_test}}

   # list of numbers from 0.0 to 1.0 with a 0.05 interval
   alphas = mylib.get_alphas()

   for alpha in alphas:
      # Use Ridge algorithm to create a regression model
      reg = Ridge(alpha = alpha)
      reg.fit(data["train"]["X"], data["train"]["y"])

      preds = reg.predict(data["test"]["X"])
      mse = mean_squared_error(preds, data["test"]["y"])
      # log the alpha and mse values
      run.log('alpha', alpha)
      run.log('mse', mse)

      model_file_name = 'ridge_{0:.2f}.pkl'.format(alpha)
      # save model in the outputs folder so it automatically get uploaded
      with open(model_file_name, "wb") as file:
          joblib.dump(value = reg, filename = model_file_name)

      # upload the model file explicitly into artifacts 
      run.upload_file(name = model_file_name, path_or_stream = model_file_name)

      # register the model
      #run.register_model(file_name = model_file_name)

      print('alpha is {0:.2f}, and mse is {1:0.2f}'.format(alpha, mse))
  
   ```

2. Ridge modelinde `mylib.py` kullanılacak Alfa değerlerinin listesini almanızı sağlayan betikbaşvuruları.`train.py`

   ```python
   # mylib.py
  
   import numpy as np

   def get_alphas():
      # list of numbers from 0.0 to 1.0 with a 0.05 interval
      return np.arange(0.0, 1.0, 0.05)
   ```

3. Bir kullanıcı tarafından yönetilen yerel ortam yapılandırın.

   ```python
   from azureml.core import Environment
    
   # Editing a run configuration property on-fly.
   user_managed_env = Environment("user-managed-env")
    
   user_managed_env.python.user_managed_dependencies = True
    
   # You can choose a specific Python environment by pointing to a Python path 
   #user_managed_env.python.interpreter_path = '/home/johndoe/miniconda3/envs/myenv/bin/python'
   ```

4. Gönderme ```train.py``` kullanıcı tarafından yönetilen bir ortamda çalıştırılacak komut. Bu tüm kod klasörü eğitim için gönderilen dahil olmak üzere ```mylib.py``` dosya.

   ```python
   from azureml.core import ScriptRunConfig
    
   exp = Experiment(workspace=ws, name="train-on-local")
   src = ScriptRunConfig(source_directory='./', script='train.py')
   src.run_config.environment = user_managed_env
   run = exp.submit(src)
   ```

## <a name="manage-a-run"></a>Bir çalıştırmayı yönetme

[Başlat, izle ve iptal eğitimi çalıştırmaları](how-to-manage-runs.md) makalesinde, denemeleri ' yi yönetme hakkında belirli Azure Machine Learning iş akışları vurgulanmaktadır.

## <a name="view-run-details"></a>Çalıştırma Ayrıntıları görünümü

### <a name="monitor-run-with-jupyter-notebook-widget"></a>Jupyter Not defteri pencere öğesiyle çalışma izleme
Çalıştırmaları göndermek için **ScriptRunConfig** yöntemini kullandığınızda, bir [Jupyıter pencere öğesi](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)ile çalıştırmanın ilerlemesini izleyebilirsiniz. Çalıştırma gönderimi gibi pencere öğesi de zaman uyumsuzdur ve iş tamamlanana kadar her 10-15 saniyede bir canlı güncelleştirmeler sağlar.

1. Jupyter pencere öğesi, çalışma tamamlanması beklenirken görüntüleyin.

   ```python
   from azureml.widgets import RunDetails
   RunDetails(run).show()
   ```

   ![Ekran görüntüsü, Jupyter not defteri pencere öğesi](./media/how-to-track-experiments/run-details-widget.png)

Ayrıca, çalışma alanınızda aynı görüntüleme bağlantısını da alabilirsiniz.

```python
print(run.get_portal_url())
```

2. **[Çalıştırmalarını otomatik makine öğrenme]**  Grafikleri önceki bir çalıştırma işleminden erişmek için. Uygun `<<experiment_name>>` deneme adıyla değiştirin:

   ``` 
   from azureml.widgets import RunDetails
   from azureml.core.run import Run

   experiment = Experiment (workspace, <<experiment_name>>)
   run_id = 'autoML_my_runID' #replace with run_ID
   run = Run(experiment, run_id)
   RunDetails(run).show()
   ```

   ![Jupyter not defteri pencere otomatik Machine Learning için](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)


Daha fazla işlem hattını bir işlem hattı tıklama ayrıntılarını görüntülemek için keşfetmek tabloda istediğiniz ve grafikleri, Azure portalından bir açılır pencere içinde işlenir.

### <a name="get-log-results-upon-completion"></a>Tamamlandıktan sonra günlük sonuçlarını alma

Beklerken diğer görevleri çalıştırabileceği şekilde onlara eğitim ve izleme modeli arka planda gerçekleştirilir. Ayrıca daha fazla kod çalıştırmadan önce eğitim modeli tamamlanana kadar bekleyebilirsiniz. Kullanırken **ScriptRunConfig**, kullanabileceğiniz ```run.wait_for_completion(show_output = True)``` modeli eğitimi ne zaman tamamlandığını göstermek için. ```show_output``` Bayrağı ayrıntılı çıkış sağlar. 


### <a name="query-run-metrics"></a>Sorgu ölçümleri Çalıştır

Eğitilen modeli kullanarak bir ölçümleri görüntüleyebilir ```run.get_metrics()```. Artık tüm en iyi modeli belirlemek için yukarıdaki örnekte, günlüğe kaydedilen ölçümleri de alabilirsiniz.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-the-experiment-in-the-azure-portal"></a>Azure portalında deneme görüntüleyin

Bir deney çalışması tamamlandığında kayıtlı denemeyi çalıştırma kaydı için göz atabilirsiniz. Geçmişe iki şekilde erişebilirsiniz:

* Farklı Çalıştır URL'sini doğrudan ```print(run.get_portal_url())```
* Çalıştırma adı göndererek çalıştırma ayrıntılarını görüntüleyin. (Bu durumda, ```run```). Bu şekilde deney adı, kimliği, türü, durum, Ayrıntılar sayfası, Azure portalına bir bağlantı ve belgeler için bir bağlantı gösterir.

Farklı Çalıştır bağlantısını çalıştırma ayrıntıları sayfasına doğrudan Azure portalında getirir. Burada, özellikleri, izlenen ölçümler, görüntüleri ve deneme günlüğe kaydedilen grafik görebilirsiniz. Bu durumda, MSE ve alfa değerleri oturum.

  ![Azure portalında çalışma ayrıntıları](./media/how-to-track-experiments/run-details-page.png)

Ayrıca, herhangi bir çıkış veya çalıştırma için günlükleri görüntüleyin veya anlık görüntüsünü ve böylelikle deneme klasörü başkalarıyla paylaşabilirsiniz gönderdiğiniz denemeyi indirin.

### <a name="viewing-charts-in-run-details"></a>Grafikler çalıştırma ayrıntılarını görüntüleme

Bir çalıştırma sırasında günlük kaydı ölçümleri farklı kayıt türleri için API'leri kullanma ve bunları Azure portalındaki grafikleri olarak görüntülemek için çeşitli yollar vardır. 

|Günlüğe kaydedilen değeri|Örnek kod| Portalda görüntüle|
|----|----|----|
|Günlük sayısal değerler dizisi| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Tek değişkenli çizgi grafik|
|Sürekli olarak kullanılan aynı Ölçüm adı tek bir sayısal değer oturum (içinden benzer bir for döngüsü)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Tek değişkenli çizgi grafik|
|Bir satır ile 2 sayısal sütunlara tekrar tekrar oturum|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|İki değişken çizgi grafik|
|2 sayısal sütunlara sahip günlüğü tablosu|`run.log_table(name='Sine Wave', value=sines)`|İki değişken çizgi grafik|


## <a name="example-notebooks"></a>Örnek Not Defterleri
Aşağıdaki not defterleri, bu makaledeki kavramları göstermektedir:
* [How-to-use-azureml/Training/Train-within-Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook)
* [How-to-use-azureml/Training/Train-on-Local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [How-to-use-azureml/Training/Logging-api/Logging-api.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Python için Azure Machine Learning SDK'sı kullanma hakkında bilgi edinmek için sonraki adımları deneyin:

* En iyi modeli kaydedin ve öğreticide dağıtmak nasıl bir örneğini görmek [bir Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).

* Bilgi edinmek için nasıl [eğitme PyTorch modeller Azure Machine Learning ile](how-to-train-pytorch.md).
