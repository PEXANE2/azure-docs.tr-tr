---
title: ML denemeleri & ölçümlerini günlüğe kaydet
titleSuffix: Azure Machine Learning
description: Azure ML denemeleri izleyin ve model oluşturma işlemini iyileştirmek için çalışma ölçümlerini izleyin. Eğitim betiğe günlük kaydı ekleyin ve bir çalıştırmanın günlüğe kaydedilmiş sonuçlarını görüntüleyin.  Run. log, Run. start_logging veya ScriptRunConfig kullanın.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/12/2020
ms.custom: seodec18
ms.openlocfilehash: 9613b74b727d27bd47a05fadc1398bf898f667a5
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835741"
---
# <a name="monitor-azure-ml-experiment-runs-and-metrics"></a>Azure ML deneme çalıştırmaları ve ölçümlerini izleme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Denemeleri ve izleme çalıştırma ölçümlerinizi izleyerek model oluşturma sürecini geliştirin. Bu makalede, eğitim betiğe günlük kodu eklemeyi, bir deneme çalıştırması göndermeyi, çalışmayı izlemeyi ve Azure Machine Learning sonuçları incelemeyi öğrenin.

> [!NOTE]
> Azure Machine Learning, otomatik makine öğrenimi çalıştırmaları veya eğitim işini çalıştıran Docker kapsayıcısı gibi eğitim sırasında diğer kaynaklardaki bilgileri de günlüğe alabilir. Bu Günlükler belgelenmemiştir. Sorunlarla karşılaşırsanız ve Microsoft Destek ile iletişime geçerek, sorun giderme sırasında bu günlükleri kullanabiliyor olabilirler.

> [!TIP]
> Bu belgedeki bilgiler öncelikli olarak, model eğitimi sürecini izlemek isteyen veri bilimcileri ve geliştiricileri içindir. Kotalar, tamamlanan eğitim çalıştırmaları veya tamamlanmış model dağıtımları gibi Azure Machine Learning 'den kaynak kullanımını ve olayları izlemeyi ilgilenen bir yöneticiyseniz, bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="available-metrics-to-track"></a>İzlenecek kullanılabilir ölçümler

Bir denemeye eğitim sırasında aşağıdaki ölçümler bir çalıştırmaya eklenebilir. Bir çalıştırmada nelerin izlenebilecek hakkında daha ayrıntılı bir liste görüntülemek için, [sınıf başvurusunu Çalıştır belgelerine](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)bakın.

|Tür| Python işlevi | Notlar|
|----|:----|:----|
|Skaler değerler |Çalışmayacaktır<br>`run.log(name, value, description='')`<br><br>Örnek:<br>Çalıştır. log ("doğruluk", 0,95) |Verilen ada sahip bir sayı veya dize değerini çalıştırmaya göre günlüğe kaydedin. Bir ölçüyü bir çalıştırmaya kaydetmek, ölçümün deneme içinde çalıştırma kaydında depolanmasına neden olur.  Aynı ölçümü bir çalıştırma içinde birden çok kez günlüğe kaydedebilirsiniz. sonuç, bu ölçümün bir vektörü olarak kabul edilir.|
|Listeler|Çalışmayacaktır<br>`run.log_list(name, value, description='')`<br><br>Örnek:<br>Run. log_list ("accuracies", [0,6, 0,7, 0,87]) | Verilen adla çalıştırılacak bir değer listesini günlüğe kaydedin.|
|Satır|Çalışmayacaktır<br>`run.log_row(name, description=None, **kwargs)`<br>Örnek:<br>Run. log_row (X üzerinden "Y", x = 1, Y = 0,4) | *Log_row* kullanmak, kwarg 'ler içinde açıklandığı gibi birden çok sütunlu bir ölçü oluşturur. Her adlandırılmış parametre belirtilen değeri içeren bir sütun oluşturur.  *log_row* , rastgele bir tanımlama grubu günlüğe kaydetmek için bir kez veya bir döngüde bir bütün tablo oluşturmak için birden çok kez çağrılabilir.|
|Tablo|Çalışmayacaktır<br>`run.log_table(name, value, description='')`<br><br>Örnek:<br>Run. log_table (X üzerinden "Y", {"X": [1, 2, 3], "Y": [0,6, 0,7, 0,89]}) | Bir sözlük nesnesini verilen ada sahip bir çalıştırmaya kaydedin. |
|Görüntüler|Çalışmayacaktır<br>`run.log_image(name, path=None, plot=None)`<br><br>Örnek:<br>`run.log_image("ROC", plot=plt)` | Çalıştırma kaydına bir görüntü kaydedin. Günlüğe kaydetmek için log_image kullanın. PNG resim dosyası veya çalıştırılacak bir Matplotlib çizimi.  Bu görüntüler, çalıştırma kaydında görünür ve karşılaştırılabilir olacaktır.|
|Bir çalıştırmayı etiketleme|Çalışmayacaktır<br>`run.tag(key, value=None)`<br><br>Örnek:<br>Run. Tag ("Selected", "Yes") | Çalıştırmayı bir dize anahtarıyla ve isteğe bağlı dize değeriyle etiketleyin.|
|Dosya veya dizini karşıya yükle|Çalışmayacaktır<br>`run.upload_file(name, path_or_stream)`<br> <br> Örnek:<br>Run. upload_file ("best_model. pkl", "./model.exe") | Çalıştırma kaydına bir dosya yükleyin. , Belirtilen çıkış dizininde dosyayı otomatik olarak yakala, bu, çoğu çalıştırma türü için varsayılan olarak "./çıktılar" olarak belirlenmiştir.  Yalnızca ek dosyaların karşıya yüklenmesi gerektiğinde veya bir çıktı dizini belirtilmediğinde upload_file kullanın. Ada ekleme önerdiğimiz için `outputs` , çıktılar dizinine yüklenir. Bu çalıştırma kaydıyla ilişkili tüm dosyaları şu şekilde listeleyebilirsiniz.`run.get_file_names()`|

> [!NOTE]
> Dolandırıcıklar, listeler, satırlar ve tablolar için ölçümler şunlardır: float, integer veya String.

## <a name="choose-a-logging-option"></a>Günlüğe kaydetme seçeneği seçin

Denemenizi izlemek veya izlemek isterseniz, çalıştırmayı gönderdiğinizde günlüğe kaydetmeyi başlatmak için kod eklemeniz gerekir. Aşağıda, çalıştırma gönderimini tetiklemenin yolları verilmiştir:
* ' İ __çalıştırın. start_logging__ -günlük işlevlerini eğitim betiğe ekleyin ve belirtilen deneyde etkileşimli bir günlüğe kaydetme oturumu başlatın. **start_logging** , Not defterleri gibi senaryolarda kullanılmak üzere etkileşimli bir çalıştırma oluşturur. Oturum sırasında günlüğe kaydedilen ölçümler, denemenin içindeki çalıştırma kaydına eklenir.
* __ScriptRunConfig__ -günlük işlevlerini eğitim betiğe ekleyin ve tüm betik klasörünü çalıştırmaya yükleyin.  **ScriptRunConfig** , betik çalıştırmaları için yapılandırmaların ayarlanmasına yönelik bir sınıftır. Bu seçenekle, tamamlama hakkında bildirim almak veya izlenecek bir görsel pencere öğesi almak için izleme kodu ekleyebilirsiniz.
* __Tasarımcı günlüğü__ -bir sürükle &-bırakma tasarımcı işlem hattına, __Python betiği yürütme komut dosyası__ modülünü kullanarak günlük işlevleri ekleyin. Python kodunu log Designer denemeleri 'a ekleyin. 

## <a name="set-up-the-workspace"></a>Çalışma alanını ayarlama
Günlüğe kaydetme ve deneme göndermeden önce, çalışma alanını ayarlamanız gerekir.

1. Çalışma alanını yükleyin. Çalışma alanı yapılandırmasını ayarlama hakkında daha fazla bilgi için bkz. [çalışma alanı yapılandırma dosyası](how-to-configure-environment.md#workspace).

[! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb? Name = load_ws)]


## <a name="option-1-use-start_logging"></a>Seçenek 1: start_logging kullanın

**start_logging** , Not defterleri gibi senaryolarda kullanılmak üzere etkileşimli bir çalıştırma oluşturur. Oturum sırasında günlüğe kaydedilen ölçümler, denemenin içindeki çalıştırma kaydına eklenir.

Aşağıdaki örnek, yerel bir Jupyter not defterinde bir basit sktrıdge modelini yerel olarak öğrendi. Farklı ortamlara denemeleri gönderme hakkında daha fazla bilgi edinmek için bkz. [Azure Machine Learning ile model eğitimi için işlem hedeflerini ayarlama](https://docs.microsoft.com/azure/machine-learning/how-to-set-up-training-targets).

### <a name="load-the-data"></a>Verileri yükleme

Bu örnek, scikit-öğren ile birlikte gelen iyi bilinen küçük bir veri kümesi olan Diabetes veri kümesini kullanır. Bu hücre, veri kümesini yükler ve rastgele eğitim ve test kümelerine böler.

[! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb? Name = load_data)]

### <a name="add-tracking"></a>İzleme Ekle
Azure Machine Learning SDK kullanarak deneme izleme ekleyin ve deneme çalıştırması kaydına kalıcı bir model yükleyin. Aşağıdaki kod, Etiketler, Günlükler ve bir model dosyasını deneme çalıştırmasına ekler.

[! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb? Name = create_experiment)]

Komut dosyası ile biter ```run.complete()``` ve çalıştırma tamamlandı olarak işaretlenir.  Bu işlev genellikle etkileşimli not defteri senaryolarında kullanılır.

## <a name="option-2-use-scriptrunconfig"></a>Seçenek 2: ScriptRunConfig kullanma

[**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) , betik çalıştırmaları için yapılandırmaların ayarlanmasına yönelik bir sınıftır. Bu seçenekle, tamamlama hakkında bildirim almak veya izlenecek bir görsel pencere öğesi almak için izleme kodu ekleyebilirsiniz.

Bu örnek, yukarıdaki temel sköğren Ridge model üzerinde genişletilir. Bu, deneme kapsamındaki çalışma aşamasındaki ölçümleri ve eğitilen modelleri yakalamak için modelin Alfa değerlerinin üzerinde tarama yapmak üzere basit bir parametre tarama işlemi yapar. Örnek, Kullanıcı tarafından yönetilen bir ortamda yerel olarak çalışır. 

1. Bir eğitim betiği oluşturun `train.py` .

   [! Code-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]

2. `train.py` `mylib.py` Ridge modelinde kullanılacak Alfa değerlerinin listesini almanızı sağlayan betik başvuruları.

   [! Code-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/mylib.py)] 

3. Kullanıcı tarafından yönetilen bir yerel ortam yapılandırın.

   [! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? Name = user_managed_env)]


4. ```train.py```Betiği Kullanıcı tarafından yönetilen ortamda çalıştırılacak şekilde gönderebilirsiniz. Bu tüm betik klasörü, dosya dahil olmak üzere eğitim için gönderilir ```mylib.py``` .

   [! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? Name = src)] [! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? ad = Run)]

## <a name="option-3-log-designer-experiments"></a>Seçenek 3: log Designer denemeleri

Tasarımcı denemeleri günlük mantığını eklemek için __Python betiği yürütme__ modülünü kullanın. Bu iş akışını kullanarak herhangi bir değeri günlüğe kaydedebilirsiniz, ancak farklı çalışmalardan model performansını izlemek için __model değerlendir__ modülünden ölçümleri günlüğe kaydetmek yararlı olur.

1. Bir __Python betik__ modülünü, __değerlendirme modeli__ modülünüzün çıkışına bağlayın.

    ![Model modülünü değerlendirmek için Python betik modülünü yürütmeyi bağlama](./media/how-to-track-experiments/designer-logging-pipeline.png)

1. Eğitilen modelinize ilişkin ortalama mutlak hatayı günlüğe kaydetmek için __Python betik__ kodu düzenleyicisini Yürüt ' e aşağıdaki kodu yapıştırın:

    ```python
    # dataframe1 contains the values from Evaluate Model
    def azureml_main(dataframe1 = None, dataframe2 = None):
        print(f'Input pandas.DataFrame #1: {dataframe1}')

        from azureml.core import Run

        run = Run.get_context()

        # Log the mean absolute error to the current run to see the metric in the module detail pane.
        run.log(name='Mean_Absolute_Error', value=dataframe1['Mean_Absolute_Error'])

        # Log the mean absolute error to the parent run to see the metric in the run details page.
        run.parent.log(name='Mean_Absolute_Error', value=dataframe1['Mean_Absolute_Error'])
    
        return dataframe1,
    ```

## <a name="manage-a-run"></a>Bir çalıştırmayı yönetme

[Başlat, izle ve iptal eğitimi çalıştırmaları](how-to-manage-runs.md) makalesinde, denemeleri ' yi yönetme hakkında belirli Azure Machine Learning iş akışları vurgulanmaktadır.

## <a name="view-run-details"></a>Çalışma ayrıntılarını görüntüle

### <a name="view-activequeued-runs-from-the-browser"></a>Tarayıcıdan etkin/kuyruğa alınmış çalıştırmaları görüntüleme

Modelleri eğitemak için kullanılan işlem hedefleri paylaşılan bir kaynaktır. Bu nedenle, belirli bir zamanda sıralanmış veya etkin birden çok çalıştırılabilir. Tarayıcınızdan belirli bir işlem hedefine ait çalıştırmaları görmek için aşağıdaki adımları kullanın:

1. [Azure Machine Learning Studio](https://ml.azure.com/)'da, çalışma alanınızı seçin ve ardından sayfanın sol tarafındaki __işlem__ ' ı seçin.

1. Eğitim için kullanılan işlem hedeflerinin bir listesini göstermek için __eğitim kümelerini__ seçin. Ardından kümeyi seçin.

    ![Eğitim kümesini seçin](./media/how-to-track-experiments/select-training-compute.png)

1. __Çalıştırmalar__' ı seçin. Bu kümeyi kullanan çalıştırmaların listesi görüntülenir. Belirli bir çalıştırmanın ayrıntılarını görüntülemek için __Çalıştır__ sütunundaki bağlantıyı kullanın. Deneme ayrıntılarını görüntülemek için, __deneme__ sütunundaki bağlantıyı kullanın.

    ![Eğitim kümesi için çalıştırmaları seçin](./media/how-to-track-experiments/show-runs-for-compute.png)
    
    > [!TIP]
    > Bir çalıştırma alt çalıştırmalar içerebilir, bu nedenle bir eğitim işi birden fazla girişe neden olabilir.

Bir çalıştırma tamamlandıktan sonra bu sayfada artık görüntülenmez. Tamamlanan çalıştırmalar hakkındaki bilgileri görüntülemek için, Studio 'nun __denemeleri__ bölümünü ziyaret edin ve denemeyi ve çalıştırmayı seçin. Daha fazla bilgi için bkz [sorgu ölçümleri çalıştırma](#queryrunmetrics) bölümü.

### <a name="monitor-run-with-jupyter-notebook-widget"></a>Jupyter Not defteri pencere öğesiyle çalışma izleme
Çalıştırmaları göndermek için **ScriptRunConfig** yöntemini kullandığınızda, bir [Jupyıter pencere öğesi](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)ile çalıştırmanın ilerlemesini izleyebilirsiniz. Çalıştırma gönderimi gibi pencere öğesi de zaman uyumsuzdur ve iş tamamlanana kadar her 10-15 saniyede bir canlı güncelleştirmeler sağlar.

1. Çalıştırmanın tamamlanmasını beklerken Jupyıter pencere öğesini görüntüleyin.

   ```python
   from azureml.widgets import RunDetails
   RunDetails(run).show()
   ```

   ![Jupyter Not defteri pencere öğesinin ekran görüntüsü](./media/how-to-track-experiments/run-details-widget.png)

   Ayrıca, çalışma alanınızda aynı görüntüleme bağlantısını da alabilirsiniz.

   ```python
   print(run.get_portal_url())
   ```

2. **[Otomatik makine öğrenimi çalıştırmaları için]** Önceki bir çalıştırmalardan grafiklere erişmek için. `<<experiment_name>>`Uygun deneme adıyla değiştirin:

   ``` 
   from azureml.widgets import RunDetails
   from azureml.core.run import Run

   experiment = Experiment (workspace, <<experiment_name>>)
   run_id = 'autoML_my_runID' #replace with run_ID
   run = Run(experiment, run_id)
   RunDetails(run).show()
   ```

   ![Otomatik Machine Learning için Jupyter Not defteri pencere öğesi](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)


Bir işlem hattının daha ayrıntılı ayrıntılarını görüntülemek için tabloda araştırmak istediğiniz işlem hattına tıklayın ve grafikler Azure Machine Learning Studio 'dan bir açılır pencerede işlenir.

### <a name="get-log-results-upon-completion"></a>Tamamlandıktan sonra günlük sonuçlarını alma

Model eğitimi ve izleme arka planda gerçekleşirken diğer görevleri beklerken çalıştırabilirsiniz. Ayrıca, modelin daha fazla kod çalıştırmadan önce eğitimi tamamlamasını de bekleyebilirsiniz. **ScriptRunConfig**kullandığınızda, ```run.wait_for_completion(show_output = True)``` model eğitiminin ne zaman tamamlandığını göstermek için kullanabilirsiniz. ```show_output```Bayrak size ayrıntılı çıkış verir. 

<a id="queryrunmetrics"></a>

### <a name="query-run-metrics"></a>Sorgu çalıştırma ölçümleri

İle eğitilen bir modelin ölçümlerini görüntüleyebilirsiniz ```run.get_metrics()``` . Bundan böyle, en iyi modeli belirleyebilmek için yukarıdaki örnekte günlüğe kaydedilen tüm ölçümleri elde edebilirsiniz.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-the-experiment-in-your-workspace-in-azure-machine-learning-studio"></a>[Azure Machine Learning Studio](https://ml.azure.com) 'daki çalışma alanınızdaki denemeyi görüntüleyin

Bir deneme çalışmayı tamamladığında, kaydedilen deneme çalıştırması kaydına gidebilirsiniz. [Azure Machine Learning Studio](https://ml.azure.com)'dan geçmişe erişebilirsiniz.

Denemeleri sekmesine gidin ve denemenizin seçimini yapın. Her çalıştırma için günlüğe kaydedilen izlenen ölçümleri ve grafikleri görebileceğiniz deneme çalıştırması panosuna yönlendirilirsiniz. Bu durumda, MSE ve Alfa değerlerini günlüğe verdik.

  ![Azure Machine Learning Studio 'da ayrıntıları çalıştırma](./media/how-to-track-experiments/experiment-dashboard.png)

Belirli bir çalıştırmaya gidebilir ya da çıktılarını veya günlüklerini görüntüleyebilir ya da deneme klasörünü başkalarıyla paylaşabileceğiniz şekilde gönderdiğiniz deneyinin anlık görüntüsünü indirebilirsiniz.

### <a name="viewing-charts-in-run-details"></a>Çalışma ayrıntılarında grafikleri görüntüleme

Bir çalıştırma sırasında farklı ölçüm türlerini kaydetmek ve bunları Azure Machine Learning Studio 'da grafik olarak görüntülemek için günlük API 'Leri kullanmanın çeşitli yolları vardır.

|Günlüğe kaydedilen değer|Örnek kod| Portalda görüntüle|
|----|----|----|
|Sayısal değerlerden oluşan diziyi günlüğe kaydet| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Tek değişkenli çizgi grafik|
|Aynı ölçüm adı ile birlikte sürekli kullanılan tek bir sayısal değeri günlüğe kaydet (for döngüsü içinde olduğu gibi)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Tek değişkenli çizgi grafik|
|2 sayısal sütundan oluşan bir satırı sürekli olarak günlüğe kaydet|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|İki değişkenli çizgi grafik|
|2 sayısal sütun içeren günlük tablosu|`run.log_table(name='Sine Wave', value=sines)`|İki değişkenli çizgi grafik|


## <a name="example-notebooks"></a>Örnek Not defterleri
Aşağıdaki Not defterleri bu makaledeki kavramları göstermektedir:
* [Nasıl yapılır kullanımı-bir not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook)
* [Nasıl yapılır-kullanım-azureml/eğitim/yerel olarak eğitme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [Nasıl yapılır kullanımı-azureml/izleme-ve izleme-denemeleri/Logging-API](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Python için Azure Machine Learning SDK'sını kullanmayı öğrenmek için bu sonraki adımları deneyin:

* En iyi modeli nasıl kaydedeceğinizi ve öğreticide nasıl dağıtabileceğinizi gösteren bir örnek, [Azure Machine Learning ile bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)bölümüne bakın.

* [PyTorch modellerini Azure Machine Learning Ile eğitme](how-to-train-pytorch.md)hakkında bilgi edinin.
