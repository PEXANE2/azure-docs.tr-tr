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
ms.openlocfilehash: 0c77e9d0aa4f44f33b1345a6021fc0378459ee85
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79296974"
---
# <a name="monitor-azure-ml-experiment-runs-and-metrics"></a>Azure ML deneme çalıştırmaları ve ölçümlerini izleme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Denemeleri ve izleme çalıştırma ölçümlerinizi izleyerek model oluşturma sürecini geliştirin. Bu makalede, eğitim betiğe günlük kodu eklemeyi, bir deneme çalıştırması göndermeyi, çalışmayı izlemeyi ve Azure Machine Learning sonuçları incelemeyi öğrenin.

> [!NOTE]
> Azure Machine Learning, otomatik makine öğrenimi çalıştırmaları veya eğitim işini çalıştıran Docker kapsayıcısı gibi eğitim sırasında diğer kaynaklardaki bilgileri de günlüğe alabilir. Bu Günlükler belgelenmemiştir. Sorunlarla karşılaşırsanız ve Microsoft Destek ile iletişime geçerek, sorun giderme sırasında bu günlükleri kullanabiliyor olabilirler.

> [!TIP]
> Bu belgedeki bilgiler öncelikli olarak, model eğitimi sürecini izlemek isteyen veri bilimcileri ve geliştiricileri içindir. Kotalar, tamamlanan eğitim çalıştırmaları veya tamamlanmış model dağıtımları gibi Azure Machine Learning 'den kaynak kullanımını ve olayları izlemeyi ilgilenen bir yöneticiyseniz, bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="available-metrics-to-track"></a>İzlenecek kullanılabilir ölçümler

Aşağıdaki ölçümler, bir denemeyi eğitim sırasında çalıştırılacak eklenebilir. Bir çalıştırmada nelerin izlenebilecek hakkında daha ayrıntılı bir liste görüntülemek için, [sınıf başvurusunu Çalıştır belgelerine](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)bakın.

|Tür| Funkce Pythonu | Notlar|
|----|:----|:----|
|Skaler değerler |İşlev:<br>`run.log(name, value, description='')`<br><br>Örnek:<br>Run.log ("doğruluğu", 0.95) |Günlük bir sayısal veya dize değeri belirtilen ada sahip farklı çalıştır. Bir ölçüm için bir çalıştırma günlüğe kaydetme, denemeyi çalıştırma kaydı depolanması, ölçüm neden olur.  Bu ölçüm oluşan bir vektörü olarak kabul sonucu bir çalıştırma içinde birden çok kez aynı Ölçüm oturum açabilirsiniz.|
|Listeler|İşlev:<br>`run.log_list(name, value, description='')`<br><br>Örnek:<br>Run.log_list ("doğruluk" [0,6, 0,7, 0.87]) | Belirtilen ada sahip farklı çalıştır değerlerin bir listesini oturum.|
|Satır|İşlev:<br>`run.log_row(name, description=None, **kwargs)`<br>Örnek:<br>Run.log_row ("Y" X üzerinden x = 1, y 0.4 =) | *Log_row* kullanmak, kwarg 'ler içinde açıklandığı gibi birden çok sütunlu bir ölçü oluşturur. Her adlandırılmış parametre ile belirtilen değer bir sütun oluşturur.  *log_row* , rastgele bir tanımlama grubu günlüğe kaydetmek için bir kez veya bir döngüde bir bütün tablo oluşturmak için birden çok kez çağrılabilir.|
|Tablo|İşlev:<br>`run.log_table(name, value, description='')`<br><br>Örnek:<br>Run.log_table ("Y" X üzerinden {"x": [1, 2, 3], "y": [0,6, 0,7, 0.89]}) | Sözlük nesnesi, verilen ada sahip farklı çalıştır oturum açın. |
|Görüntüler|İşlev:<br>`run.log_image(name, path=None, plot=None)`<br><br>Örnek:<br>`run.log_image("ROC", plot=plt)` | Görüntü çalıştırma kaydı için oturum açın. Bir görüntü dosyası veya bir matplotlib oturum log_image kullanın çizim farklı çalıştır.  Bu görüntüler, görünür ve çalışma kaydındaki benzer olacaktır.|
|Bir etiketi|İşlev:<br>`run.tag(key, value=None)`<br><br>Örnek:<br>run.tag (","Evet"seçeneği") | Bir dize anahtarı ve isteğe bağlı dize değeri olan çalıştırma etiketleyin.|
|Dosya veya dizin karşıya yükleme|İşlev:<br>`run.upload_file(name, path_or_stream)`<br> <br> Örnek:<br>Run.upload_file ("best_model.pkl", ". / model.pkl") | Çalıştırma kaydı için bir dosya yükleyin. Çalıştırmaları otomatik olarak varsayılan olarak belirtilen çıkış dizini dosyasında yakala ". / çıkışlar" türleri çoğu çalıştırın.  Yalnızca ek dosyalar yüklenmek üzere gerektiğinde kullanım upload_file veya bir çıktı dizini belirtilmedi. Ada `outputs`, çıktılar dizinine karşıya yüklenecek şekilde eklenmesini öneririz. Bu çalıştırma kaydıyla ilişkili tüm dosyaları `run.get_file_names()` çağırılır.|

> [!NOTE]
> Skalerler, listeler, satır ve tablolar için ölçümleri türü olabilir: kayan noktalı sayı, tamsayı veya dize.

## <a name="choose-a-logging-option"></a>Günlüğe kaydetme seçeneği seçin

İzlemek veya denemenizi izlemek istiyorsanız, çalıştırma gönderdiğinizde oturum başlatmak için kod eklemeniz gerekir. Çalıştırma gönderim tetiklemek için yollar şunlardır:
* ' İ __çalıştırın. start_logging__ -günlük işlevlerini eğitim betiğe ekleyin ve belirtilen deneyde etkileşimli bir günlüğe kaydetme oturumu başlatın. **start_logging** , Not defterleri gibi senaryolarda kullanılmak üzere etkileşimli bir çalıştırma oluşturur. Oturumu sırasında günlüğe kaydedilen tüm ölçümler, denemeyi çalıştırma kaydı eklenir.
* __ScriptRunConfig__ -günlük işlevlerini eğitim betiğe ekleyin ve tüm betik klasörünü çalıştırmaya yükleyin.  **ScriptRunConfig** , betik çalıştırmaları için yapılandırmaların ayarlanmasına yönelik bir sınıftır. Bu seçenekle tamamlanmasından almak veya izlemek için görsel bir pencere öğesi almak için izleme kodu ekleyebilirsiniz.

## <a name="set-up-the-workspace"></a>Çalışma alanını ayarlama
Günlüğe kaydetme ve deneme gönderme eklemeden önce çalışma alanını ayarlamanız gerekir.

1. Çalışma alanı yükleyin. Çalışma alanı yapılandırmasını ayarlama hakkında daha fazla bilgi için bkz. [çalışma alanı yapılandırma dosyası](how-to-configure-environment.md#workspace).

[! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb? Name = load_ws)]


## <a name="option-1-use-start_logging"></a>1\. seçenek: start_logging kullanma

**start_logging** , Not defterleri gibi senaryolarda kullanılmak üzere etkileşimli bir çalıştırma oluşturur. Oturumu sırasında günlüğe kaydedilen tüm ölçümler, denemeyi çalıştırma kaydı eklenir.

Aşağıdaki örnek, yerel Jupyter not defterini yerel olarak basit bir sklearn Ridge modeli eğitir. Farklı ortamlara denemeleri gönderme hakkında daha fazla bilgi edinmek için bkz. [Azure Machine Learning ile model eğitimi için işlem hedeflerini ayarlama](https://docs.microsoft.com/azure/machine-learning/how-to-set-up-training-targets).

### <a name="load-the-data"></a>Verileri yükleme

Bu örnek, scikit-öğren ile birlikte gelen iyi bilinen küçük bir veri kümesi olan Diabetes veri kümesini kullanır. Bu hücre, veri kümesini yükler ve rastgele eğitim ve test kümelerine böler.

[! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb? Name = load_data)]

### <a name="add-tracking"></a>İzleme Ekle
Azure Machine Learning SDK kullanarak deneme izleme ekleyin ve deneme çalıştırması kaydına kalıcı bir model yükleyin. Aşağıdaki kod, etiketler, günlükleri, ekler ve denemeyi çalıştırmak için bir model dosyası yükler.

[! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb? Name = create_experiment)]

Betik, çalışmayı tamamlandı olarak işaretleyen ```run.complete()```biter.  Bu işlev, genellikle etkileşimli bir not defteri senaryolarda kullanılır.

## <a name="option-2-use-scriptrunconfig"></a>2\. seçenek: ScriptRunConfig kullanma

[**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) , betik çalıştırmaları için yapılandırmaların ayarlanmasına yönelik bir sınıftır. Bu seçenekle tamamlanmasından almak veya izlemek için görsel bir pencere öğesi almak için izleme kodu ekleyebilirsiniz.

Bu örnek, yukarıda temel sklearn Ridge modeli genişletir. Alfa değerleri ölçümleri yakalamak için model ve deneme altında çalıştığı eğitilen modeller üzerinde basit bir parametre için tarama Süpürme yapar. Örnek, bir kullanıcı tarafından yönetilen ortamında yerel olarak çalıştırılır. 

1. Eğitim betiği oluşturun `train.py`.

   [! Code-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]

2. `train.py` betiği, Ridge modelinde kullanılacak Alfa değerlerinin listesini almanıza izin veren `mylib.py` başvurur.

   [! Code-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/mylib.py)] 

3. Bir kullanıcı tarafından yönetilen yerel ortam yapılandırın.

   [! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? Name = user_managed_env)]


4. ```train.py``` betiği Kullanıcı tarafından yönetilen ortamda çalışacak şekilde gönderebilirsiniz. Bu tüm betik klasörü, ```mylib.py``` dosyası dahil olmak üzere eğitim için gönderilir.

   [! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? Name = src)] [! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? ad = Run)]




## <a name="manage-a-run"></a>Bir çalıştırmayı yönetme

[Başlat, izle ve iptal eğitimi çalıştırmaları](how-to-manage-runs.md) makalesinde, denemeleri ' yi yönetme hakkında belirli Azure Machine Learning iş akışları vurgulanmaktadır.

## <a name="view-run-details"></a>Çalıştırma Ayrıntıları görünümü

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

2. **[Otomatik makine öğrenimi çalıştırmaları için]** Önceki bir çalıştırmalardan grafiklere erişmek için. `<<experiment_name>>` uygun deneme adıyla değiştirin:

   ``` 
   from azureml.widgets import RunDetails
   from azureml.core.run import Run

   experiment = Experiment (workspace, <<experiment_name>>)
   run_id = 'autoML_my_runID' #replace with run_ID
   run = Run(experiment, run_id)
   RunDetails(run).show()
   ```

   ![Jupyter not defteri pencere otomatik Machine Learning için](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)


Bir işlem hattının daha ayrıntılı ayrıntılarını görüntülemek için tabloda araştırmak istediğiniz işlem hattına tıklayın ve grafikler Azure Machine Learning Studio 'dan bir açılır pencerede işlenir.

### <a name="get-log-results-upon-completion"></a>Tamamlandıktan sonra günlük sonuçlarını alma

Beklerken diğer görevleri çalıştırabileceği şekilde onlara eğitim ve izleme modeli arka planda gerçekleştirilir. Ayrıca daha fazla kod çalıştırmadan önce eğitim modeli tamamlanana kadar bekleyebilirsiniz. **ScriptRunConfig**kullandığınızda, model eğitiminin ne zaman tamamlandığını göstermek için ```run.wait_for_completion(show_output = True)``` kullanabilirsiniz. ```show_output``` bayrağı ayrıntılı çıkış sağlar. 

<a id="queryrunmetrics"></a>

### <a name="query-run-metrics"></a>Sorgu ölçümleri Çalıştır

```run.get_metrics()```kullanarak eğitilen bir modelin ölçümlerini görüntüleyebilirsiniz. Artık tüm en iyi modeli belirlemek için yukarıdaki örnekte, günlüğe kaydedilen ölçümleri de alabilirsiniz.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-the-experiment-in-your-workspace-in-azure-machine-learning-studio"></a>[Azure Machine Learning Studio](https://ml.azure.com) 'daki çalışma alanınızdaki denemeyi görüntüleyin

Bir deney çalışması tamamlandığında kayıtlı denemeyi çalıştırma kaydı için göz atabilirsiniz. [Azure Machine Learning Studio](https://ml.azure.com)'dan geçmişe erişebilirsiniz.

Denemeleri sekmesine gidin ve denemenizin seçimini yapın. Her çalıştırma için günlüğe kaydedilen izlenen ölçümleri ve grafikleri görebileceğiniz deneme çalıştırması panosuna yönlendirilirsiniz. Bu durumda, MSE ve alfa değerleri oturum.

  ![Azure Machine Learning Studio 'da ayrıntıları çalıştırma](./media/how-to-track-experiments/experiment-dashboard.png)

Belirli bir çalıştırmaya gidebilir ya da çıktılarını veya günlüklerini görüntüleyebilir ya da deneme klasörünü başkalarıyla paylaşabileceğiniz şekilde gönderdiğiniz deneyinin anlık görüntüsünü indirebilirsiniz.

### <a name="viewing-charts-in-run-details"></a>Grafikler çalıştırma ayrıntılarını görüntüleme

Bir çalıştırma sırasında farklı ölçüm türlerini kaydetmek ve bunları Azure Machine Learning Studio 'da grafik olarak görüntülemek için günlük API 'Leri kullanmanın çeşitli yolları vardır.

|Günlüğe kaydedilen değeri|Örnek kod| Portalda görüntüle|
|----|----|----|
|Günlük sayısal değerler dizisi| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Tek değişkenli çizgi grafik|
|Sürekli olarak kullanılan aynı Ölçüm adı tek bir sayısal değer oturum (içinden benzer bir for döngüsü)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Tek değişkenli çizgi grafik|
|Bir satır ile 2 sayısal sütunlara tekrar tekrar oturum|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|İki değişken çizgi grafik|
|2 sayısal sütunlara sahip günlüğü tablosu|`run.log_table(name='Sine Wave', value=sines)`|İki değişken çizgi grafik|


## <a name="example-notebooks"></a>Örnek Not Defterleri
Aşağıdaki not defterleri, bu makaledeki kavramları göstermektedir:
* [Nasıl yapılır kullanımı-bir not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook)
* [Nasıl yapılır-kullanım-azureml/eğitim/yerel olarak eğitme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [Nasıl yapılır kullanımı-azureml/izleme-ve izleme-denemeleri/Logging-API](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Python için Azure Machine Learning SDK'sını kullanmayı öğrenmek için bu sonraki adımları deneyin:

* En iyi modeli nasıl kaydedeceğinizi ve öğreticide nasıl dağıtabileceğinizi gösteren bir örnek, [Azure Machine Learning ile bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)bölümüne bakın.

* [PyTorch modellerini Azure Machine Learning Ile eğitme](how-to-train-pytorch.md)hakkında bilgi edinin.
