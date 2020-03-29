---
title: AKS dağıtımlarında veri sürüklenme algılama
titleSuffix: Azure Machine Learning
description: Azure Kubernetes Hizmeti'nde Azure Makine Öğrenimi'nde dağıtılan modelleri veri kayması (önizleme) algıla.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: cody-dkdc
ms.date: 11/04/2019
ms.openlocfilehash: d1da7309b296b57db0c28d5b52fe91efa86709c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75537012"
---
# <a name="detect-data-drift-preview-on-models-deployed-to-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetine (AKS) dağıtılan modellerde veri kayması (önizleme) algılama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu makalede, dağıtılan bir modelin eğitim veri kümesi ve çıkarım verileri arasındaki veri sürüklenmesini nasıl izleyeceğinizi öğreneceksiniz. Makine öğrenimi bağlamında, eğitimli makine öğrenimi modelleri sürüklenme nedeniyle bozulmuş tahmin performansı yaşayabilir. Azure Machine Learning ile veri sürüklenmesini izleyebilir ve servis, sürüklenme algılandığında size bir e-posta uyarısı gönderebilir.

## <a name="what-is-data-drift"></a>Veri kayması nedir?

Makine öğrenimi bağlamında, veri kayması model performansının düşmesine yol açan model giriş verilerindeki değişimdir. Model doğruluğunun zaman içinde düşmesinin en önemli nedenlerinden biridir, böylece veri kaymasının izlenmesi model performans sorunlarını algılamaya yardımcı olur. 

## <a name="what-can-i-monitor"></a>Neyi izleyebilirim?

Azure Machine Learning ile AKS'de dağıtılan bir modelin girişlerini izleyebilir ve bu verileri modeliçin eğitim veri kümesiyle karşılaştırabilirsiniz. Düzenli aralıklarla çıkarım verileri [anlık görüntüve profillenir,](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py)daha sonra bir veri kayması çözümlemesi oluşturmak için temel veri kümesine göre hesaplanır: 

+ Sürüklenme katsayısı adı verilen veri kaymasının büyüklüğünü ölçer.
+ Veri kayması katkısını özelliğine göre ölçer, hangi özelliklerin veri kaymasına neden olduğunu gösterir.
+ Mesafe ölçümlerini ölçer. Şu anda Wasserstein ve Enerji Mesafesi hesaplanmaktadır.
+ Özelliklerin dağılımlarını ölçer. Şu anda çekirdek yoğunluğu tahmini ve histogramları.
+ E-posta ile veri kayması için uyarılar gönderin.

> [!Note]
> Bu hizmet (önizleme) ve yapılandırma seçenekleri sınırlıdır. Ayrıntılar ve güncellemeler için lütfen [API Dokümantasyon](https://docs.microsoft.com/python/api/azureml-datadrift/) ve [Sürüm Notlarımıza](azure-machine-learning-release-notes.md) bakın. 

### <a name="how-data-drift-is-monitored-in-azure-machine-learning"></a>Azure Machine Learning'de veri kayması nasıl izlenir?

Azure Machine Learning kullanılarak, veri kayması veri kümeleri veya dağıtımlar aracılığıyla izlenir. Veri kayması için izlemek için, bir temel veri kümesi -genellikle bir model için eğitim veri kümesi- belirtilir. İkinci bir veri kümesi -genellikle dağıtımdan toplanan giriş verilerini modelle- temel veri kümesine karşı test edilir. Her iki veri kümesi de profillidir ve veri kayması izleme hizmetine giriş yapılır. Bir makine öğrenme modeli, iki veri kümesi arasındaki farkları algılamak için eğitilir. Modelin performansı, iki veri kümesi arasındaki sürüklenme büyüklüğünü ölçen sürüklenme katsayısına dönüştürülür. [Model yorumlanabilirliği](how-to-machine-learning-interpretability.md)kullanılarak, sürüklenme katsayısına katkıda bulunan özellikler hesaplanır. Veri kümesi profilinden, her özellik hakkında istatistiksel bilgiler izlenir. 

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Hesabınız yoksa, başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

- Python için Azure Machine Learning SDK yüklendi. Aşağıdakileri yapmak için [Azure Machine Learning SDK'daki](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) yönergeleri kullanın:

    - Miniconda ortamı oluşturun
    - Python için Azure Machine Learning SDK'yı yükleyin

- [Azure Makine Öğrenimi çalışma alanı.](how-to-manage-workspace.md)

- Çalışma alanı [yapılandırma dosyası.](how-to-configure-environment.md#workspace)

- Aşağıdaki komutu kullanarak veri kayması SDK yükleyin:

    ```shell
    pip install azureml-datadrift
    ```

- Modelinizin eğitim verilerinden bir [veri kümesi](how-to-create-register-datasets.md) oluşturun.

- Modeli [kaydederken](concept-model-management-and-deployment.md) eğitim veri kümesini belirtin. Aşağıdaki örnek, eğitim `datasets` veri kümesini belirtmek için parametrenin kullanılmasını gösterir:

    ```python
    model = Model.register(model_path=model_file,
                        model_name=model_name,
                        workspace=ws,
                        datasets=datasets)

    print(model_name, image_name, service_name, model)
    ```

- [Modelin](how-to-enable-data-collection.md) AKS dağıtımından veri toplamak ve verilerin blob kapsayıcısında toplandığını doğrulamak için model veri toplamayı etkinleştirin. `modeldata`

## <a name="configure-data-drift"></a>Veri sürüklenme yapılandırma
Denemeniz için veri sürüklenme yapılandırmak için, aşağıdaki Python örneğinde görüldüğü gibi bağımlılıkları aktarın. 

Bu örnek, nesneyi [`DataDriftDetector`](/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector) yapılandırmayı gösterir:

```python
# Import Azure ML packages
from azureml.core import Experiment, Run, RunDetails
from azureml.datadrift import DataDriftDetector, AlertConfiguration

# if email address is specified, setup AlertConfiguration
alert_config = AlertConfiguration('your_email@contoso.com')

# create a new DataDriftDetector object
datadrift = DataDriftDetector.create(ws, model.name, model.version, services, frequency="Day", alert_config=alert_config)
    
print('Details of Datadrift Object:\n{}'.format(datadrift))
```

## <a name="submit-a-datadriftdetector-run"></a>DataDriftDetector çalıştırın

`DataDriftDetector` Nesne yapılandırıldı, model için belirli bir tarihte bir veri [sürüklenme çalıştır gönderebilirsiniz.](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector#run-target-date--services-none--compute-target-none--create-compute-target-false--feature-list-none--drift-threshold-none-) Çalıştırmanın bir parçası olarak, parametreyi ayarlayarak DataDriftDetector uyarılarını etkinleştirin. `drift_threshold` [datadrift_coefficient](#visualize-drift-metrics) verilenin `drift_threshold`üzerindeyse, bir e-posta gönderilir.

```python
# adhoc run today
target_date = datetime.today()

# create a new compute - creates datadrift-server
run = datadrift.run(target_date, services, feature_list=feature_list, create_compute_target=True)

# or specify existing compute cluster
run = datadrift.run(target_date, services, feature_list=feature_list, compute_target='cpu-cluster')

# show details of the data drift run
exp = Experiment(ws, datadrift._id)
dd_run = Run(experiment=exp, run_id=run.id)
RunDetails(dd_run).show()
```

## <a name="visualize-drift-metrics"></a>Drift ölçümlerini görselleştirin

<a name="metrics"></a>

DataDriftDetector çalışmanızı gönderdikten sonra, bir veri sürüklenme görevi için her çalıştırma yinelemesinde kaydedilen sürüklenme ölçümlerini görebilirsiniz:


|Ölçüm|Açıklama|
--|--|
wasserstein_distance|Tek boyutlu sayısal dağılım için tanımlanan istatistiksel uzaklık.|
energy_distance|Tek boyutlu sayısal dağılım için tanımlanan istatistiksel uzaklık.|
datadrift_coefficient|Matthew'un korelasyon katsayısına benzer şekilde hesaplanır, ancak bu çıktı 0 ile 1 arasında değişen gerçek bir sayıdır. Drift bağlamında, 0 hiçbir sürüklenme gösterir ve 1 maksimum sürüklenme gösterir.|
datadrift_contribution|Sürüklenmeye katkıda bulunan özelliklerin özelliği.|

Sürüklenme ölçümlerini görüntülemenin birden çok yolu vardır:

* `RunDetails` [Jupyter widget'ını](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)kullan.
* Herhangi [`get_metrics()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#get-metrics-name-none--recursive-false--run-type-none--populate-false-) bir `datadrift` çalışan nesneüzerinde işlevi kullanın.
* [Azure Machine Learning stüdyosunda](https://ml.azure.com)çalışma alanınızın **Modeller** bölümündeki ölçümleri görüntüleyin.

Aşağıdaki Python örneği, ilgili veri sürüklenme ölçümlerinin nasıl çizilen ini gösterir. Özel görselleştirmeler oluşturmak için döndürülen ölçümleri kullanabilirsiniz:

```python
# start and end are datetime objects 
drift_metrics = datadrift.get_output(start_time=start, end_time=end)

# Show all data drift result figures, one per service.
# If setting with_details is False (by default), only the data drift magnitude will be shown; if it's True, all details will be shown.
drift_figures = datadrift.show(with_details=True)
```

![Azure Machine Learning tarafından algılanan veri kaymasına bakın](./media/how-to-monitor-data-drift/drift_show.png)


## <a name="schedule-data-drift-scans"></a>Veri sürüklenme taramalarını zamanlama 

Veri kayması algılamasını etkinleştirdiğinizde, bir DataDriftDetector belirtilen, zamanlanmış frekansta çalıştırılır. datadrift_coefficient verilene `drift_threshold`ulaşırsa, her zamanlanmış çalıştırmayla birlikte bir e-posta gönderilir. 

```python
datadrift.enable_schedule()
datadrift.disable_schedule()
```

Veri kayması dedektörünün yapılandırması, [Azure Machine Learning stüdyosundaki](https://ml.azure.com)çalışma alanınızdaki **Ayrıntılar** sekmesinde **Modeller** altında görülebilir.

[![Azure Machine Learning stüdyo Data Drift](./media/how-to-monitor-data-drift/drift-config.png)](media/how-to-monitor-data-drift/drift-config-expanded.png)

## <a name="view-results-in-your-azure-machine-learning-studio"></a>Azure Machine Learning stüdyonuzdaki sonuçları görüntüleme

[Azure Machine Learning stüdyosundaki](https://ml.azure.com)çalışma alanınızdaki sonuçları görüntülemek için model sayfasına gidin. Modelin ayrıntılar sekmesinde, veri kayması yapılandırması gösterilir. **Veri kayması** sekmesi artık veri kayması ölçümlerini görselleştirerek kullanılabilir. 

[![Azure Machine Learning stüdyo Data Drift](./media/how-to-monitor-data-drift/drift-ui.png)](media/how-to-monitor-data-drift/drift-ui-expanded.png)

## <a name="receiving-drift-alerts"></a>Sürüklenme uyarıları alma

Sürüklenme katsayısı uyarı eşiğini ayarlayarak ve bir e-posta adresi sağlayarak, sürüklenme katsayısı eşiğin üzerinde olduğunda [bir Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) e-posta uyarısı otomatik olarak gönderilir. 

Özel uyarılar ve eylemler ayarlamanız için, tüm veri kayması ölçümleri Azure Machine Learning çalışma alanıyla birlikte oluşturulan [Application Insights](how-to-enable-app-insights.md) kaynağında depolanır. Application Insights sorgusuna e-posta uyarısı ndaki bağlantıyı takip edebilirsiniz.

![Veri Drift E-posta Uyarısı](./media/how-to-monitor-data-drift/drift_email.png)

## <a name="retrain-your-model-after-drift"></a>Sürüklendikten sonra modelinizi yeniden eğitin

Veri kayması dağıtılan modelinizin performansını olumsuz etkilediğinde, modeli yeniden eğitmenin zamanı dır. Bunu yapmak için aşağıdaki adımları ile devam edin.

* Toplanan verileri araştırın ve yeni modeli eğitmek için veri hazırlayın.
* Tren/test verilerine bölün.
* Yeni verileri kullanarak modeli yeniden eğitin.
* Yeni oluşturulan modelin performansını değerlendirin.
* Performans üretim modelinden daha iyiyse yeni modeli dağıtın.

## <a name="next-steps"></a>Sonraki adımlar

* Veri kayması kullanmanın tam bir örneği için [Azure ML veri drift not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/monitor-models/data-drift/drift-on-aks.ipynb)bakın. Bu Jupyter Notebook, hava durumunu tahmin etmek, AKS'ye dağıtmak ve veri kayması için izlemek için bir modeli eğitmek için bir [Azure Açık Veri Kümesi](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) kullandığını gösterir. 

* [Dataset monitörleri](how-to-monitor-datasets.md)ile veri sürüklenme algılama.

* Veri kayması genel kullanılabilirliğe doğru ilerlerken sorularınızı, yorumlarınızı veya önerilerinizi büyük ölçüde takdir ederiz. Aşağıdaki ürün geri bildirim düğmesini kullanın! 
