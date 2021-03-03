---
title: Ölçüm & ML çalıştırma günlüklerini izleme ve görüntüleme
titleSuffix: Azure Machine Learning
description: ML denemeleri izleyin ve Jupiter pencere öğeleri ve Azure Machine Learning Studio ile çalıştırma ölçümlerini görüntüleyin.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 07/30/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 8b2a61a92a25e1c0da9f85439438e75969fcfbf0
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101661027"
---
# <a name="monitor-and-view-ml-run-logs-and-metrics"></a>ML çalıştırma günlüklerini ve ölçümlerini izleme ve görüntüleme

Azure Machine Learning çalıştırmalarını izlemeyi ve günlüklerini görüntülemeyi öğrenin. 

Bir deneme çalıştırdığınızda Günlükler ve ölçümler sizin için akışla yapılır.  Ayrıca, kendi kendinizinkini ekleyebilirsiniz.  Nasıl yapılacağını öğrenmek için bkz. [Azure ML eğitim çalıştırmaları 'nda günlüğü etkinleştirme](how-to-track-experiments.md).

Günlükler, çalıştırmanızda hata ve uyarıları tanılamanıza yardımcı olabilir. Parametreler ve model doğruluğu gibi performans ölçümleri, çalıştırmalarınızı izlemenize ve izlemenize yardımcı olur.

Bu makalede, aşağıdaki yöntemleri kullanarak günlükleri görüntülemeyi öğreneceksiniz:

> [!div class="checklist"]
> * Studio 'da izleme çalıştırmaları
> * Jupyter Notebook pencere öğesini kullanarak izleme çalıştırmaları
> * Otomatik makine öğrenimi çalıştırmalarını izleyin
> * Tamamlanma sonrasında çıktı günlüklerini görüntüle
> * Çıkış günlüklerini Studio 'da görüntüleme

Denemeleri 'nizi yönetme hakkında genel bilgi için bkz. [başlatma, izleme ve iptal eğitimi çalıştırmaları](how-to-manage-runs.md).

## <a name="monitor-runs-using-the-jupyter-notebook-widget"></a>Jupyter Not defteri pencere öğesini kullanarak izleme çalıştırmaları

Çalıştırmaları göndermek için **ScriptRunConfig** yöntemini kullandığınızda, [Jupyıter pencere öğesini](/python/api/azureml-widgets/azureml.widgets?preserve-view=true&view=azure-ml-py)kullanarak çalıştırmanın ilerlemesini izleyebilirsiniz. Çalıştırma gönderimi gibi pencere öğesi de zaman uyumsuzdur ve iş tamamlanana kadar her 10-15 saniyede bir canlı güncelleştirmeler sağlar.

Çalıştırmanın tamamlanmasını beklerken Jupyıter pencere öğesini görüntüleyin.
    
```python
from azureml.widgets import RunDetails
RunDetails(run).show()
```

![Jupyter Not defteri pencere öğesinin ekran görüntüsü](./media/how-to-track-experiments/run-details-widget.png)

Ayrıca, çalışma alanınızda aynı görüntüleme bağlantısını da alabilirsiniz.

```python
print(run.get_portal_url())
```

## <a name="monitor-automated-machine-learning-runs"></a>Otomatik makine öğrenimi çalıştırmalarını izleyin

Otomatik makine öğrenimi çalıştırmaları için, önceki bir çalıştırmalardan grafiklere erişmek üzere `<<experiment_name>>` uygun deneme adıyla değiştirin:

```python
from azureml.widgets import RunDetails
from azureml.core.run import Run

experiment = Experiment (workspace, <<experiment_name>>)
run_id = 'autoML_my_runID' #replace with run_ID
run = Run(experiment, run_id)
RunDetails(run).show()
```

![Otomatik Machine Learning için Jupyter Not defteri pencere öğesi](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)

## <a name="show-output-upon-completion"></a>Tamamlama sırasında çıktıyı göster

**ScriptRunConfig** kullandığınızda, ```run.wait_for_completion(show_output = True)``` model eğitiminin ne zaman tamamlandığını göstermek için kullanabilirsiniz. ```show_output```Bayrak size ayrıntılı çıkış verir. Daha fazla bilgi için bkz. [günlük kaydını etkinleştirme](how-to-track-experiments.md#scriptrun-logs)ScriptRunConfig bölümü.

<a id="queryrunmetrics"></a>

### <a name="logging-run-metrics"></a>Günlük çalıştırma ölçümleri 

Ölçüm görselleştirmelerini etkilemek için günlüğe kaydetme API 'Lerinde aşağıdaki yöntemleri kullanın. Günlüğe kaydedilen bu ölçümler için [hizmet sınırlarını](https://docs.microsoft.com/azure/machine-learning/resource-limits-quotas-capacity#metrics) göz önünde edin. 

|Günlüğe kaydedilen değer|Örnek kod| Portalda biçim|
|----|----|----|
|Sayısal değerlerden oluşan diziyi günlüğe kaydet| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Tek değişkenli çizgi grafik|
|Aynı ölçüm adı ile birlikte sürekli kullanılan tek bir sayısal değeri günlüğe kaydet (for döngüsü içinde olduğu gibi)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Tek değişkenli çizgi grafik|
|2 sayısal sütundan oluşan bir satırı sürekli olarak günlüğe kaydet|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|İki değişkenli çizgi grafik|
|2 sayısal sütun içeren günlük tablosu|`run.log_table(name='Sine Wave', value=sines)`|İki değişkenli çizgi grafik|

## <a name="query-run-metrics"></a>Sorgu çalıştırma ölçümleri

İle eğitilen bir modelin ölçümlerini görüntüleyebilirsiniz ```run.get_metrics()``` . Örneğin, en düşük ortalama kare hatası (MSE) değerine sahip modeli arayarak en iyi modeli belirleyebilmek için bunu Yukarıdaki örnekle birlikte kullanabilirsiniz.

<a name="view-the-experiment-in-the-web-portal"></a>

## <a name="view-run-records-in-the-studio"></a>Studio 'da çalıştırma kayıtlarını görüntüleme

Günlüğe kaydedilen ölçümler de dahil olmak üzere tamamlanan çalışma kayıtlarına gidebilirsiniz [Azure Machine Learning Studio](https://ml.azure.com).

**Denemeleri** sekmesine gidin. Çalışma alanınızdaki tüm çalıştırmalarınızı denemeleri genelinde görüntülemek için **Tüm çalıştırmalar** sekmesini seçin. En üstteki menü çubuğuna deneme filtresini uygulayarak belirli denemeleri için çalıştırmalar üzerinde ayrıntıya gidebilirsiniz.

Bireysel deneme görünümü için **tüm denemeleri** sekmesini seçin. Deneme çalıştırması panosunda, her çalıştırma için izlenen ölçümleri ve günlükleri görebilirsiniz. 

Ayrıca, çalışma listesi tablosunu, birden çok çalıştırma seçmek ve çalıştırmalarınız için en son, en düşük veya en büyük günlüğe kaydedilmiş değeri göstermek üzere düzenleyebilirsiniz. Birden çok çalıştırma sırasında günlüğe kaydedilen ölçüm değerlerini ve toplamlarını karşılaştırmak için grafiklerinizi özelleştirin. 

![Azure Machine Learning Studio 'da ayrıntıları çalıştırma](media/how-to-track-experiments/experimentation-tab.gif)

### <a name="view-log-files-for-a-run"></a>Bir çalıştırma için günlük dosyalarını görüntüleme 

Günlük dosyaları, Azure ML iş yüklerinde hata ayıklamak için gereken önemli bir kaynaktır. Günlüklerini ve çıktılarını görüntülemek için belirli bir çalıştırmaya detaya gitme:  

1. **Denemeleri** sekmesine gidin.
1. Belirli bir çalıştırma için RunId 'yi seçin.
1. Sayfanın üst kısmındaki **çıktılar ve Günlükler '** i seçin.

:::image type="content" source="media/how-to-monitor-view-training-logs/view-logs.png" alt-text="Bir çalıştırmanın çıkış ve Günlükler bölümünün ekran görüntüsü":::

Aşağıdaki tablolarda, bu bölümde göreceğiniz klasörlerdeki günlük dosyalarının içerikleri gösterilmektedir.

> [!NOTE]
> Her çalıştırma için her dosyayı görmeyecektir. Örneğin, 20_image_build_log *. txt yalnızca yeni bir görüntü oluşturulduğunda görüntülenir (örneğin, ortamınızı değiştirdiğinizde).

#### <a name="azureml-logs-folder"></a>`azureml-logs` klasörde

|Dosya  |Açıklama  |
|---------|---------|
|20_image_build_log.txt     | Eğitim ortamı için Docker görüntü oluşturma günlüğü, her çalıştırma için isteğe bağlı. Yalnızca ortamınız güncelleştirilirken geçerlidir. Aksi takdirde AML, önbelleğe alınmış görüntüyü yeniden kullanır. Başarılı olursa, ilgili görüntü için görüntü kayıt defteri ayrıntılarını içerir.         |
|55_azureml yürütme-<node_id # C1.txt     | düğüm başına bir tane olmak üzere konak aracının stdout/stderr günlüğü. İşlem hedefine görüntü çeker. Bu günlük, işlem kaynaklarını güvenli hale getirildikten sonra yalnızca bir kez görünür.         |
|65_job_prep-<node_id # C1.txt     |   Her düğüm için bir tane olmak üzere iş hazırlama betiği stdout/stderr günlüğü. Hedef ve veri depolarını (istenirse) hesaplamak için kodunuzu indirin.       |
|70_driver_log (_x). txt      |  AML denetim betiğinden stdout/stderr günlüğü, işlem başına bir tane. **Bu, betiğinizdeki standart çıktıdır. Bu, kodunuzun günlüklerinin (ör. Print deyimlerinin) gösterilmesi burada gösterilmektedir.** Çoğu durumda günlükleri burada izlemeniz gerekir.       |
|70_mpi_log.txt     |   MPı Framework günlüğü, isteğe bağlı, her çalıştırma için bir. Yalnızca MPı çalıştırmaları için.   |
|75_job_post-<node_id # C1.txt     |  Her düğüm için bir tane olmak üzere iş sürümü betiği stdout/stderr günlüğü. Günlükleri gönderin, işlem kaynaklarını Azure 'a geri bırakın.        |
|Üzerinde process_info.js      |   hangi işlemin hangi düğümde çalıştığını gösterir.  |
|Üzerinde process_status.js      | işlem durumunu göster, yani bir işlem başlatılmamışsa, çalışıyor veya tamamlandı.         |

#### <a name="logs--azureml-folder"></a>`logs > azureml` klasörde

|Dosya  |Açıklama  |
|---------|---------|
|110_azureml. log      |         |
|job_prep_azureml. log     |   iş hazırlığı için sistem günlüğü        |
|job_release_azureml. log     | iş sürümü için sistem günlüğü        |

#### <a name="logs--azureml--sidecar--node_id-folder"></a>`logs > azureml > sidecar > node_id` klasörde

Sepet etkinleştirildiğinde, iş hazırlığı ve iş sürümü betikleri, sepet kapsayıcısı içinde çalıştırılır.  Her düğüm için bir klasör vardır. 

|Dosya  |Açıklama  |
|---------|---------|
|start_cms.txt     |  Sepet kapsayıcısı başladığında başlayan işlem günlüğü       |
|prep_cmd.txt      |   Çalıştırıldığında girilen Contextyöneticileri için günlük kaydı `job_prep.py` (bir kısmı bu şekilde akışa alınacaktır `azureml-logs/65-job_prep` )       |
|release_cmd.txt     |  Çalıştırıldıktan sonra Comtextyöneticileri için günlüğe çıkıldı `job_release.py`        |

#### <a name="other-folders"></a>Diğer klasörler

Çoklu işlem kümelerinde iş eğitimi için, her düğüm IP 'si için Günlükler bulunur. Her bir düğümün yapısı, tek düğümlü işlerle aynıdır. Genel yürütme, stderr ve STDOUT günlükleri için bir ek günlük klasörü vardır.

Azure Machine Learning, eğitim sırasında (örneğin, oto ml) veya eğitim işini çalıştıran Docker kapsayıcısı gibi çeşitli kaynaklardan günlük bilgileri günlüğe kaydeder. Bu günlüklerin birçoğu açıklanmamıştır. Sorunlarla karşılaşırsanız ve Microsoft Destek ile iletişime geçerek, sorun giderme sırasında bu günlükleri kullanabiliyor olabilirler.

## <a name="monitor-a-compute-cluster"></a>İşlem kümesini izleme

Tarayıcınızdan belirli bir işlem hedefine ait çalıştırmaları izlemek için aşağıdaki adımları kullanın:

1. [Azure Machine Learning Studio](https://ml.azure.com/)'da, çalışma alanınızı seçin ve ardından sayfanın sol tarafındaki __işlem__ ' ı seçin.

1. Eğitim için kullanılan işlem hedeflerinin bir listesini göstermek için __eğitim kümelerini__ seçin. Ardından kümeyi seçin.

    ![Eğitim kümesini seçin](./media/how-to-track-experiments/select-training-compute.png)

1. __Çalıştırmalar__' ı seçin. Bu kümeyi kullanan çalıştırmaların listesi görüntülenir. Belirli bir çalıştırmanın ayrıntılarını görüntülemek için __Çalıştır__ sütunundaki bağlantıyı kullanın. Deneme ayrıntılarını görüntülemek için, __deneme__ sütunundaki bağlantıyı kullanın.

    ![Eğitim kümesi için çalıştırmaları seçin](./media/how-to-track-experiments/show-runs-for-compute.png)
    
    > [!TIP]
    > Eğitim işlem hedefleri paylaşılan bir kaynak olduğundan, belirli bir zamanda kuyruğa alınmış veya etkin birden fazla çalıştırma olabilir.
    > 
    > Bir çalıştırma alt çalıştırmalar içerebilir, bu nedenle bir eğitim işi birden fazla girişe neden olabilir.

Bir çalıştırma tamamlandıktan sonra bu sayfada artık görüntülenmez. Tamamlanan çalıştırmalar hakkındaki bilgileri görüntülemek için, Studio 'nun __denemeleri__ bölümünü ziyaret edin ve denemeyi ve çalıştırmayı seçin. Daha fazla bilgi için, [Tamamlanan çalıştırmalar için ölçümleri görüntüleme](#view-the-experiment-in-the-web-portal)bölümüne bakın.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning kullanmayı öğrenmek için bu sonraki adımları deneyin:

* [Azure Machine Learning tasarımcısında denemeleri ve günlükleri etkinleştirme](how-to-track-designer-experiments.md)hakkında bilgi edinin.

* En iyi modeli kaydetme ve dağıtma öğreticisi için bkz. [Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).
