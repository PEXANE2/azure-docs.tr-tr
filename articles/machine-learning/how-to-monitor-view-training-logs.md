---
title: Ölçüm & ML çalıştırma günlüklerini izleme ve görüntüleme
titleSuffix: Azure Machine Learning
description: Azure ML denemeleri izleyin ve model oluşturma işlemini iyileştirmek için çalışma ölçümlerini görüntüleyin. Çalışma durumunu araştırmak ve çalıştırma kayıtlarını görüntülemek için pencere öğelerini ve Studio portalını kullanın.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 07/30/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 642186ef4af43127ba0d6eb6950f09e069eb79ca
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89649291"
---
# <a name="monitor-and-view-ml-run-logs-and-metrics"></a>ML çalıştırma günlüklerini ve ölçümlerini izleme ve görüntüleme

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning çalıştırmalarını izlemeyi ve günlüklerini görüntülemeyi öğreneceksiniz. Günlükleri görüntüleyebilmeniz için önce bunları etkinleştirmeniz gerekir. Daha fazla bilgi için bkz. [Azure ML eğitim çalıştırmaları 'nda günlüğü etkinleştirme](how-to-track-experiments.md).

Günlükler, hataları ve uyarıları tanılamanıza veya parametreler ve model doğruluğu gibi performans ölçümlerini izlemenize yardımcı olabilir. Bu makalede, aşağıdaki yöntemleri kullanarak günlükleri görüntülemeyi öğreneceksiniz:

> [!div class="checklist"]
> * Studio 'da izleme çalıştırmaları
> * Jupyter Notebook pencere öğesini kullanarak izleme çalıştırmaları
> * Otomatik makine öğrenimi çalıştırmalarını izleyin
> * Tamamlanma sonrasında çıktı günlüklerini görüntüle
> * Çıkış günlüklerini Studio 'da görüntüleme

Denemeleri 'nizi yönetme hakkında genel bilgi için bkz. [başlatma, izleme ve iptal eğitimi çalıştırmaları](how-to-manage-runs.md).

## <a name="monitor-runs-in-the-studio"></a>Studio 'da izleme çalıştırmaları

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

## <a name="monitor-runs-using-the-jupyter-notebook-widget"></a>Jupyter Not defteri pencere öğesini kullanarak izleme çalıştırmaları

Çalıştırmaları göndermek için **ScriptRunConfig** yöntemini kullandığınızda, [Jupyıter pencere öğesini](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py&preserve-view=true)kullanarak çalıştırmanın ilerlemesini izleyebilirsiniz. Çalıştırma gönderimi gibi pencere öğesi de zaman uyumsuzdur ve iş tamamlanana kadar her 10-15 saniyede bir canlı güncelleştirmeler sağlar.

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

**ScriptRunConfig**kullandığınızda, ```run.wait_for_completion(show_output = True)``` model eğitiminin ne zaman tamamlandığını göstermek için kullanabilirsiniz. ```show_output```Bayrak size ayrıntılı çıkış verir. Daha fazla bilgi için bkz. [günlük kaydını etkinleştirme](how-to-track-experiments.md#scriptrunconfig-logs)ScriptRunConfig bölümü.

<a id="queryrunmetrics"></a>
## <a name="query-run-metrics"></a>Sorgu çalıştırma ölçümleri

İle eğitilen bir modelin ölçümlerini görüntüleyebilirsiniz ```run.get_metrics()``` . Örneğin, en düşük ortalama kare hatası (MSE) değerine sahip modeli arayarak en iyi modeli belirleyebilmek için bunu Yukarıdaki örnekle birlikte kullanabilirsiniz.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-run-records-in-the-studio"></a>Studio 'da çalıştırma kayıtlarını görüntüleme

Günlüğe kaydedilen ölçümler de dahil olmak üzere tamamlanan çalışma kayıtlarına gidebilirsiniz [Azure Machine Learning Studio](https://ml.azure.com).

**Denemeleri** sekmesine gidin ve denemenizin seçimini yapın. Deneme çalıştırması panosunda, her çalıştırma için izlenen ölçümleri ve günlükleri görebilirsiniz. 

Belirli bir çalıştırmaya gitmeyi, çıktılarını veya günlüklerini görüntülemek veya denemeler klasörünü başkalarıyla paylaşmak için deneme anlık görüntüsünü indirmek.

Ayrıca, çalışma listesi tablosunu, birden çok çalıştırma seçmek ve çalıştırmalarınız için en son, en düşük veya en büyük günlüğe kaydedilmiş değeri göstermek üzere düzenleyebilirsiniz. Birden çok çalıştırma sırasında günlüğe kaydedilen ölçüm değerlerini ve toplamlarını karşılaştırmak için grafiklerinizi özelleştirin.

![Azure Machine Learning Studio 'da ayrıntıları çalıştırma](media/how-to-track-experiments/experimentation-tab.gif)

### <a name="format-charts-in-the-studio"></a>Studio 'da grafikleri biçimlendirme

Aşağıdaki yöntemleri kullanarak, oluşturma API 'Lerinde ölçümleri görselleştirir.

|Günlüğe kaydedilen değer|Örnek kod| Portalda biçim|
|----|----|----|
|Sayısal değerlerden oluşan diziyi günlüğe kaydet| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Tek değişkenli çizgi grafik|
|Aynı ölçüm adı ile birlikte sürekli kullanılan tek bir sayısal değeri günlüğe kaydet (for döngüsü içinde olduğu gibi)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Tek değişkenli çizgi grafik|
|2 sayısal sütundan oluşan bir satırı sürekli olarak günlüğe kaydet|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|İki değişkenli çizgi grafik|
|2 sayısal sütun içeren günlük tablosu|`run.log_table(name='Sine Wave', value=sines)`|İki değişkenli çizgi grafik|


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning kullanmayı öğrenmek için bu sonraki adımları deneyin:

* [Azure Machine Learning tasarımcısında (Önizleme) denemeleri ve günlükleri etkinleştirme](how-to-track-designer-experiments.md)hakkında bilgi edinin.

* En iyi modeli nasıl kaydedeceğinizi ve öğreticide nasıl dağıtabileceğinizi gösteren bir örnek, [Azure Machine Learning ile bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)bölümüne bakın.

