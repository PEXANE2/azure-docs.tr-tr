---
title: Log ML deneyleri & ölçümleri
titleSuffix: Azure Machine Learning
description: Model oluşturma işlemini geliştirmek için Azure ML denemelerinizi izleyin ve çalışma ölçümlerini izleyin. Eğitim komut dosyanıza günlüğe kaydetme ekleyin ve bir çalıştırmanın günlüğe kaydedilmiş sonuçlarını görüntüleyin.  run.log, Run.start_logging veya ScriptRunConfig'i kullanın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79296974"
---
# <a name="monitor-azure-ml-experiment-runs-and-metrics"></a>Azure ML deneme çalıştırmalarını ve ölçümlerini izleyin
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Denemelerinizi izleyerek ve çalışma ölçümlerini izleyerek model oluşturma işlemini geliştirin. Bu makalede, eğitim komut dosyanıza günlük kodu eklemeyi, deneme çalıştırmayı nasıl göndereceğinizi, çalıştıranları nasıl izleyeceğinizi ve Azure Machine Learning'deki sonuçları nasıl inceleyeceğinizi öğrenin.

> [!NOTE]
> Azure Machine Learning, eğitim sırasında otomatik makine öğrenimi çalıştırmaları veya eğitim işini yürüten Docker konteyneri gibi diğer kaynaklardan gelen bilgileri de günlüğe kaydedebilir. Bu günlükler belgelenmez. Sorunlarla karşılaşır ve Microsoft desteğine başvurursanız, sorun giderme sırasında bu günlükleri kullanabilirler.

> [!TIP]
> Bu belgedeki bilgiler öncelikle model eğitim sürecini izlemek isteyen veri bilimciler ve geliştiriciler içindir. Kotalar, tamamlanmış eğitim çalıştırmaları veya tamamlanmış model dağıtımları gibi Azure Machine öğrenimindeki kaynak kullanımını ve etkinlikleri izlemek isteyen bir yöneticiyseniz, [bkz.](monitor-azure-machine-learning.md)

## <a name="available-metrics-to-track"></a>İzlemek için kullanılabilir ölçümler

Bir denemeyi eğitirken bir çalıştırmaya aşağıdaki ölçümler eklenebilir. Bir çalıştırmada izlenebileceklerin daha ayrıntılı bir listesini görüntülemek için [Çalıştır sınıfı başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)bakın.

|Tür| Python fonksiyonu | Notlar|
|----|:----|:----|
|Skaler değerler |Işlev:<br>`run.log(name, value, description='')`<br><br>Örnek:<br>run.log("doğruluk", 0,95) |Verilen adla çalışana sayısal veya dize değerini günlüğe kaydedin. Bir metnin çalıştırılmak için günlüğe kaydedilmesi, bu ölçümün denemedeki çalışma kaydında depolanabılmasına neden olur.  Aynı metrik bir çalışma içinde birden çok kez günlüğe kaydedebilirsiniz, sonuç bu metrik bir vektör olarak kabul ediliyor.|
|Listeler|Işlev:<br>`run.log_list(name, value, description='')`<br><br>Örnek:<br>run.log_list("doğruluklar", [0.6, 0.7, 0.87]) | Verilen adla çalışan değerler listesini günlüğe kaydedin.|
|Satır|Işlev:<br>`run.log_row(name, description=None, **kwargs)`<br>Örnek:<br>run.log_row("X üzerinden Y", x=1, y=0.4) | *log_row* kullanmak, kwargs'da açıklandığı gibi birden çok sütuniçeren bir metrik oluşturur. Adlandırılmış her parametre belirtilen değeri olan bir sütun oluşturur.  *log_row* rasgele bir tuple günlüğe bir kez veya tam bir tablo oluşturmak için bir döngü birden çok kez çağrılabilir.|
|Tablo|Işlev:<br>`run.log_table(name, value, description='')`<br><br>Örnek:<br>run.log_table("X üzerinden Y", {"x":[1, 2, 3], "y":[0.6, 0.7, 0.89]}) | Verilen adla çalıştırığa bir sözlük nesnesini günlüğe kaydedin. |
|Görüntüler|Işlev:<br>`run.log_image(name, path=None, plot=None)`<br><br>Örnek:<br>`run.log_image("ROC", plot=plt)` | Görüntüyü çalışma kaydına kaydedin. Bir görüntü dosyasını veya çalışma matplotlib çizimini günlüğe kaydetmek için log_image kullanın.  Bu görüntüler, çalışma kaydında görünür ve karşılaştırılabilir olacaktır.|
|Bir koşuya etiketle|Işlev:<br>`run.tag(key, value=None)`<br><br>Örnek:<br>run.tag("selected", "evet") | Çalıştırmayı bir dize tuşu ve isteğe bağlı dize değeriyle etiketle.|
|Dosya veya dizin yükleme|Işlev:<br>`run.upload_file(name, path_or_stream)`<br> <br> Örnek:<br>run.upload_file("best_model.pkl", "./model.pkl") | Bir dosyayı çalıştırma kaydına yükleyin. Çoğu çalıştırma türü için varsayılan olarak "./çıktılar" varsayılan olarak belirtilen çıktı dizininde dosyayı otomatik olarak yakalar.  Yalnızca ek dosyaların yüklenmesi gerektiğinde veya çıktı dizini belirtilmediğinde upload_file kullanın. Çıktı dizine yüklenmesi için isme eklemenizi `outputs` öneririz. Bu çalıştırma kaydıyla ilişkili tüm dosyaları`run.get_file_names()`|

> [!NOTE]
> Skaler, listeler, satırlar ve tablolar için ölçümler türü olabilir: float, insaveya veya dize.

## <a name="choose-a-logging-option"></a>Günlüğe kaydetme seçeneği ni seçin

Denemenizi izlemek veya izlemek istiyorsanız, çalıştırmayı gönderirken günlüğe kaydetmeye başlamak için kod eklemeniz gerekir. Çalıştır gönderimini tetiklemenin yolları şunlardır:
* __Run.start_logging__ - Eğitim komut dosyanıza günlük işlevleri ekleyin ve belirtilen denemede etkileşimli bir günlük oturumu başlatın. **start_logging,** not defterleri gibi senaryolarda kullanılmak üzere etkileşimli bir çalışma oluşturur. Oturum sırasında günlüğe kaydedilen tüm ölçümler denemedeki çalışma kaydına eklenir.
* __ScriptRunConfig__ - Eğitim komut dosyasına günlük işlevleri ekleyin ve tüm komut dosyası klasörünü çalıştırarak yükleyin.  **ScriptRunConfig** komut dosyası çalıştırmaları için yapılandırmaları ayarlamak için bir sınıftır. Bu seçenekle, tamamlanma dan haberdar olmak veya izlemek için görsel bir widget almak için izleme kodu ekleyebilirsiniz.

## <a name="set-up-the-workspace"></a>Çalışma alanını ayarlama
Günlüğe kaydetme ve deneme göndermeden önce çalışma alanını ayarlamanız gerekir.

1. Çalışma alanını yükleyin. Çalışma alanı yapılandırmasını ayarlama hakkında daha fazla bilgi edinmek için [çalışma alanı yapılandırma dosyasına](how-to-configure-environment.md#workspace)bakın.

[!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb?name=load_ws)]


## <a name="option-1-use-start_logging"></a>Seçenek 1: start_logging kullanın

**start_logging,** not defterleri gibi senaryolarda kullanılmak üzere etkileşimli bir çalışma oluşturur. Oturum sırasında günlüğe kaydedilen tüm ölçümler denemedeki çalışma kaydına eklenir.

Aşağıdaki örnek, yerel bir Jupyter dizüstü bilgisayarda yerel olarak basit bir sklearn Ridge modeli trenler. Denemeleri farklı ortamlara gönderme hakkında daha fazla bilgi edinmek için Azure [Machine Learning ile model eğitimi için bilgi işlem hedefleri ayarlayın'a](https://docs.microsoft.com/azure/machine-learning/how-to-set-up-training-targets)bakın.

### <a name="load-the-data"></a>Verileri yükleme

Bu örnek, scikit-learn ile birlikte gelen tanınmış küçük bir veri kümesi olan diyabet veri kümesini kullanır. Bu hücre veri kümesini yükler ve rasgele eğitim ve test kümelerine böler.

[!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb?name=load_data)]

### <a name="add-tracking"></a>İzleme ekleme
Azure Machine Learning SDK'yı kullanarak deneme izleme ekleyin ve deneme çalıştırma kaydına kalıcı bir model yükleyin. Aşağıdaki kod, deneme çalışmasına etiketler, günlükler ve bir model dosyası yükler.

[!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb?name=create_experiment)]

Komut dosyası, ```run.complete()```tamamlanan olarak çalıştırmayı işaretler , ile sona erer.  Bu işlev genellikle etkileşimli not defteri senaryolarında kullanılır.

## <a name="option-2-use-scriptrunconfig"></a>Seçenek 2: ScriptRunConfig kullanın

[**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) komut dosyası çalıştırmaları için yapılandırmaları ayarlamak için bir sınıftır. Bu seçenekle, tamamlanma dan haberdar olmak veya izlemek için görsel bir widget almak için izleme kodu ekleyebilirsiniz.

Bu örnek yukarıdan temel sklearn Ridge modeli genişletir. Deneme nin altındaki çalıştırmalarda ölçümleri ve eğitilmiş modelleri yakalamak için modelin alfa değerlerini süpürmek için basit bir parametre süpürme yapar. Örnek, kullanıcı tarafından yönetilen bir ortama karşı yerel olarak çalışır. 

1. Bir eğitim `train.py`komut dosyası oluşturun.

   [!code-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]

2. Sırt `train.py` modelinde kullanmak için alfa değerlerinin listesini almak için izin veren komut dosyası başvuruları. `mylib.py`

   [!code-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/mylib.py)] 

3. Kullanıcı tarafından yönetilen yerel ortamı yapılandırın.

   [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=user_managed_env)]


4. Komut ```train.py``` dosyasını kullanıcı tarafından yönetilen ortamda çalışacak şekilde gönderin. Tüm bu komut dosyası ```mylib.py``` klasörü, dosya da dahil olmak üzere eğitim için gönderilir.

   [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=src)] [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=run)]




## <a name="manage-a-run"></a>Çalıştırmayı yönetme

[Başlangıç, izleme ve iptal eğitimleri makale,](how-to-manage-runs.md) denemelerinizi nasıl yöneteceklerine yönelik belirli Azure Machine Learning iş akışlarını vurgular.

## <a name="view-run-details"></a>Çalışma ayrıntılarını görüntüleme

### <a name="view-activequeued-runs-from-the-browser"></a>Tarayıcıdan etkin/sıralı çalıştırmaları görüntüleme

Modelleri eğitmek için kullanılan bilgi işlem hedefleri paylaşılan bir kaynaktır. Bu nedenle, belirli bir zamanda birden çok çalıştırma sıraya veya etkin olabilir. Tarayıcınızdan belirli bir bilgi işlem hedefinin çalışanlarını görmek için aşağıdaki adımları kullanın:

1. Azure [Machine Learning stüdyosundan](https://ml.azure.com/)çalışma alanınızı seçin ve sayfanın sol tarafından __Bilgi İşlem'i__ seçin.

1. Eğitim için kullanılan işlem hedeflerinin listesini görüntülemek için __Eğitim Kümeleri'ni__ seçin. Sonra kümeyi seçin.

    ![Eğitim kümesini seçin](./media/how-to-track-experiments/select-training-compute.png)

1. __Çalışır'ı__seçin. Bu kümeyi kullanan çalıştırmaların listesi görüntülenir. Belirli bir çalıştırmanın ayrıntılarını görüntülemek için __Çalıştır__ sütunundaki bağlantıyı kullanın. Denemenin ayrıntılarını görüntülemek için __Deneme__ sütunundaki bağlantıyı kullanın.

    ![Eğitim kümesi için çalıştırmaları seçme](./media/how-to-track-experiments/show-runs-for-compute.png)
    
    > [!TIP]
    > Çalışma, alt çalıştırmalar içerebilir, bu nedenle bir eğitim işi birden çok girişe neden olabilir.

Bir çalışma tamamlandıktan sonra, artık bu sayfada görüntülenmez. Tamamlanan çalıştırmalar hakkındaki bilgileri görüntülemek için stüdyonun __Denemeler__ bölümünü ziyaret edin ve denemeyi seçin ve çalıştırın. Daha fazla bilgi için [Sorgu çalışma ölçümleri](#queryrunmetrics) bölümüne bakın.

### <a name="monitor-run-with-jupyter-notebook-widget"></a>Jupyter dizüstü widget ile monitör çalışması
Çalıştırmaları göndermek için **ScriptRunConfig** yöntemini kullandığınızda, bir [Jupyter widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)ile çalışma ilerlemesini izleyebilirsiniz. Çalıştırma gönderimi gibi pencere öğesi de zaman uyumsuzdur ve iş tamamlanana kadar her 10-15 saniyede bir canlı güncelleştirmeler sağlar.

1. Koşunun tamamlanmasını beklerken Jupyter widget'ını görüntüleyin.

   ```python
   from azureml.widgets import RunDetails
   RunDetails(run).show()
   ```

   ![Jupyter dizüstü widget ekran görüntüsü](./media/how-to-track-experiments/run-details-widget.png)

   Çalışma alanınızda aynı ekrana bağlantı da alabilirsiniz.

   ```python
   print(run.get_portal_url())
   ```

2. **[Otomatik makine öğrenimi çalıştırmaları için]** Önceki bir çalıştırmadan grafiklere erişmek için. Uygun `<<experiment_name>>` deneme adı ile değiştirin:

   ``` 
   from azureml.widgets import RunDetails
   from azureml.core.run import Run

   experiment = Experiment (workspace, <<experiment_name>>)
   run_id = 'autoML_my_runID' #replace with run_ID
   run = Run(experiment, run_id)
   RunDetails(run).show()
   ```

   ![Otomatik Makine Öğrenimi için Jupyter dizüstü widget](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)


Bir boru hattıyla ilgili daha fazla ayrıntıyı görüntülemek için tabloda incelemek istediğiniz Boru Hattı'nı tıklatın ve grafikler Azure Machine Learning stüdyosundan açılan bir açılır pencerede işlenir.

### <a name="get-log-results-upon-completion"></a>Tamamlandıktan sonra günlük sonuçlarını alma

Beklerken diğer görevleri çalıştırabilmeniz için model eğitimi ve izleme arka planda gerçekleşir. Ayrıca, daha fazla kod çalıştırmadan önce modelin eğitimini tamamlamasını da bekleyebilirsiniz. **ScriptRunConfig'i**kullandığınızda, ```run.wait_for_completion(show_output = True)``` model eğitiminin ne zaman tamamolduğunu göstermek için kullanabilirsiniz. Bayrak ```show_output``` size ayrıntılı çıktı verir. 

<a id="queryrunmetrics"></a>

### <a name="query-run-metrics"></a>Sorgu çalıştırma ölçümleri

Eğitimli bir modelin ölçümlerini kullanarak ```run.get_metrics()```görüntüleyebilirsiniz. Artık en iyi modeli belirlemek için yukarıdaki örnekte günlüğe kaydedilen tüm ölçümleri alabilirsiniz.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-the-experiment-in-your-workspace-in-azure-machine-learning-studio"></a>[Azure Machine Learning stüdyosunda](https://ml.azure.com) çalışma alanınızdaki denemeyi görüntüleyin

Bir deneme nin çalışması tamamlandığında, kaydedilen deneme çalıştırma kaydına göz atabilirsiniz. Azure [Machine Learning stüdyosundan](https://ml.azure.com)geçmişe erişebilirsiniz.

Denemeler sekmesine gidin ve denemenizi seçin. Her çalıştırma için günlüğe kaydedilmiş izlenen ölçümleri ve grafikleri görebileceğiniz deneme çalıştırma panosuna getirilirsiniz. Bu durumda, MSE ve alfa değerlerini kaydettik.

  ![Azure Machine Learning stüdyosunda ayrıntıları çalıştırın](./media/how-to-track-experiments/experiment-dashboard.png)

Çıktılarını veya günlüklerini görüntülemek için belirli bir çalıştırmaya sondaj yapabilir veya deneme klasörünü başkalarıyla paylaşabilmek için gönderdiğiniz denemenin anlık görüntüsünü indirebilirsiniz.

### <a name="viewing-charts-in-run-details"></a>Grafikleri çalışma ayrıntılarında görüntüleme

Çalışma sırasında farklı türde ölçümleri kaydetmek ve Bunları Azure Machine Learning stüdyosunda grafik olarak görüntülemek için günlük API'lerini kullanmanın çeşitli yolları vardır.

|Günlüğe Kaydedilmiş Değer|Örnek kod| Portalda görüntüle|
|----|----|----|
|Bir dizi sayısal değer günlüğe kaydetme| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|tek değişkenli çizgi grafiği|
|Tek bir sayısal değeri, sürekli olarak kullanılan aynı metrik ada sahip olarak günlüğe kaydetme (for döngüsü içinden olduğu gibi)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Tek değişkenli çizgi grafiği|
|2 sayısal sütunlu bir satırı art arda günlüğe kaydetme|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|İki değişkenli çizgi grafiği|
|2 sayısal sütunlu günlük tablosu|`run.log_table(name='Sine Wave', value=sines)`|İki değişkenli çizgi grafiği|


## <a name="example-notebooks"></a>Örnek defterler
Aşağıdaki not defterleri bu makalede kavramları gösterir:
* [nasıl kullanılır-azureml/eğitim/eğitim-içinde-not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook)
* [nasıl kullanılır-azureml/eğitim/eğitim-on-yerel](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [nasıl kullanılır-azureml/track-and-monitor-experiments/logging-api](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Python için Azure Machine Learning SDK'sını kullanmayı öğrenmek için bu sonraki adımları deneyin:

* En iyi modeli nasıl kaydedip öğreticide dağıtılayabildiğini bir örnek görün, [Azure Machine Learning ile bir resim sınıflandırma modeli eğitin.](tutorial-train-models-with-aml.md)

* [Azure Machine Learning ile PyTorch Modellerini](how-to-train-pytorch.md)Nasıl Eğitin öğrenin.
