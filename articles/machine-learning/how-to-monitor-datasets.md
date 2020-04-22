---
title: Veri kümelerinde veri kayması için analiz ve izleme (önizleme)
titleSuffix: Azure Machine Learning
description: Azure Machine Learning veri kümeleri monitörleri (önizleme), veri kümelerinde veri kayması için monitör oluşturma ve uyarılar ayarlayın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
ms.author: copeters
author: lostmygithubaccount
ms.date: 11/04/2019
ms.openlocfilehash: e49c621d92a8aa604b5f95291c5d80c0141f41dd
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682715"
---
# <a name="detect-data-drift-preview-on-datasets"></a>Veri kümelerinde veri sürüklenmesini (önizlemeyi) algılama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning veri seti monitörlerini (önizleme), veri kayması ve veri kümelerinde istatistiksel değişiklikleri izlemeyi ve uyarıları nasıl ayarladığınızı öğreneceksiniz.

Azure Machine Learning veri seti monitörleri ile şunları yapabilirsiniz:
* Zaman içinde nasıl değiştiğini anlamak için **verilerinizdeki sürüklenmeyi analiz** edin.
* Eğitim ve hizmet veri kümeleri arasındaki farklar için **model verilerini izleyin.**
* Herhangi bir taban çizgisi ve hedef veri kümesi arasındaki farklar için **yeni verileri izleyin.**
* İstatistiksel özelliklerin zaman içinde nasıl değiştiğini izlemek için **verilerdeki profil özellikleri.**
* Olası sorunlara yönelik erken uyarılar için **veri kayması yla ilgili uyarıları ayarlayın.** 

Ölçümler ve öngörüler, Azure Machine Learning çalışma alanıyla ilişkili [Azure Uygulama Öngörüleri](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) kaynağından edinilebilir.

> [!Important]
> SDK ile veri sürüklenme izleme tüm sürümlerinde kullanılabilir olduğunu unutmayın, web üzerinde stüdyo üzerinden veri sürüklenme izleme sadece Enterprise edition olduğunu.

## <a name="prerequisites"></a>Ön koşullar

Veri kümesi monitörleri oluşturmak ve onlarla çalışmak için şunları yapmanız gerekir:
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.
* [Azure Makine Öğrenimi çalışma alanı.](how-to-manage-workspace.md)
* Azureml-datasets paketini içeren [Python için Azure Machine Learning SDK yüklendi.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* Dosya yolunda, dosya adında veya verilerde sütunda belirtilen bir zaman damgası ile yapılandırılmış (tabular) veriler.

## <a name="what-is-data-drift"></a>Veri kayması nedir?

Makine öğrenimi bağlamında, veri kayması model performansının düşmesine yol açan model giriş verilerindeki değişimdir. Bu model doğruluğu zaman içinde bozulur üst nedenlerinden biridir, böylece veri sürüklenme izleme model performans sorunları algılamaya yardımcı olur.

Veri kayması nedenleri şunlardır: 

- Ölçüm birimlerini inç'ten santimetreye değiştiren bir sensör gibi yukarı akış işlemi değişiklikleri. 
- Her zaman 0 okuma kırık bir sensör gibi veri kalitesi sorunları.
- Doğal sürüklenme gibi verilerde ortalama sıcaklık mevsimleri ile değişir.
- Özellikler arasındaki ilişkide değişiklik veya covariate shift. 

Azure Machine Learning veri seti monitörleri ile zaman içinde veri kümelerinde veri kayması algılamaya yardımcı olan uyarılar ayarlayabilirsiniz. 

### <a name="dataset-monitors"></a>Dataset monitörleri 

Bir veri kümesindeki yeni verilerdeki verileri algılamak ve uyarmak, sürüklenme için geçmiş verileri çözümlemek ve zaman içinde yeni verileri profillemek için bir veri kümesi monitörü oluşturabilirsiniz. Veri kayması algoritması, verilerdeki değişimin genel bir ölçüsünü ve hangi özelliklerin daha fazla araştırmadan sorumlu olduğunun göstergesini sağlar. Dataset `timeseries` monitörleri, veri kümesinde yeni veriler profiloluşturaraarak bir dizi başka ölçüm üretir. [Azure Uygulama Öngörüleri](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)aracılığıyla monitör tarafından oluşturulan tüm ölçümlerde özel uyarı ayarlanabilir. Dataset monitörleri, veri sorunlarını hızlı bir şekilde yakalamak ve olası nedenleri belirleyerek sorunu ayıklama süresini kısaltmak için kullanılabilir.  

Kavramsal olarak, Azure Machine Learning'de veri kümesi monitörleri kurmak için üç temel senaryo vardır.

Senaryo | Açıklama
---|---
Modelin eğitim verilerinden sürüklenme için bir modelin hizmet verilerini izleme | Bu senaryodan elde edilen sonuçlar, hizmet verisi eğitim verilerinden sürükleniyorsa modelin doğruluğunun düşmesi göz önüne alındığında, modelin doğruluğu için bir proxy izleme olarak yorumlanabilir.
Önceki bir zaman döneminden sürüklenme için bir zaman serisi veri kümesiizleme. | Bu senaryo daha geneldir ve model oluşturmanın akış yukarı veya alt akışıyla ilgili veri kümelerini izlemek için kullanılabilir.  Hedef veri kümesinin bir zaman damgası sütunu olmalıdır, temel veri kümesi ise hedef veri kümesiyle ortak özelliklere sahip herhangi bir tabular veri kümesi olabilir.
Geçmiş veriler üzerinde analiz ler yapmak. | Bu senaryo, geçmiş verileri anlamak ve verileri ayarlı izleme ayarlarındaki kararları bilgilendirmek için kullanılabilir.

## <a name="how-dataset-can-monitor-data"></a>Dataset verileri nasıl izleyebilir?

Azure Machine Learning kullanılarak veri kayması veri kümeleri aracılığıyla izlenir. Veri kayması için izlemek için, bir temel veri kümesi -genellikle bir model için eğitim veri kümesi- belirtilir. Hedef veri kümesi -genellikle model giriş verileri- zaman içinde temel veri kümenizle karşılaştırılır. Bu karşılaştırma, hedef veri kümenizin bir zaman damgası sütunu belirtilmiş olması gerektiği anlamına gelir.

### <a name="set-the-timeseries-trait-in-the-target-dataset"></a>Hedef `timeseries` veri kümesindeki özelliği ayarlama

Hedef veri kümesinin, `timeseries` verideki bir sütundan veya dosyaların yol deseninden türetilen sanal sütundan zaman damgası sütunu belirterek üzerinde özellik ayarlanması gerekir. Bu, Python SDK veya Azure Machine Learning stüdyosu aracılığıyla yapılabilir. Veri kümesine özellik eklemek `timeseries` için "ince tane" zaman damgasını temsil eden bir sütun belirtilmelidir. Verileriniz '{yyyy/MM/dd}' gibi zaman bilgileriyle klasör yapısına bölünürse, yol deseni ayarı üzerinden sanal bir sütun oluşturabilir ve zaman serisi işlevselliğinin önemini artırmak için bunu "kaba tane" zaman damgası olarak ayarlayabilirsiniz. 

#### <a name="python-sdk"></a>Python SDK'sı

Sınıfın [`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) yöntemi, veri kümesinin zaman damgası sütununa göre tanımlanır. 

```python 
from azureml.core import Workspace, Dataset, Datastore

# get workspace object
ws = Workspace.from_config()

# get datastore object 
dstore = Datastore.get(ws, 'your datastore name')

# specify datastore paths
dstore_paths = [(dstore, 'weather/*/*/*/*/data.parquet')]

# specify partition format
partition_format = 'weather/{state}/{date:yyyy/MM/dd}/data.parquet'

# create the Tabular dataset with 'state' and 'date' as virtual columns 
dset = Dataset.Tabular.from_parquet_files(path=dstore_paths, partition_format=partition_format)

# assign the timestamp attribute to a real or virtual column in the dataset
dset = dset.with_timestamp_columns('date')

# register the dataset as the target dataset
dset = dset.register(ws, 'target')
```

Veri kümelerinin `timeseries` özelliğini kullanmanın tam bir örneği için örnek not [defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb) veya veri [kümeleri SDK belgelerine](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-)bakın.

#### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

Azure Machine Learning stüdyosunu kullanarak veri kümenizi oluşturursanız, verilerinizin yolunun zaman damgası bilgileri içerdiğinden emin olun, veri içeren tüm alt klasörleri ekleyin ve bölüm biçimini ayarlayın. 

Aşağıdaki örnekte, *NoaaIsdFlorida/2019* alt klasörü altındaki tüm veriler alınır ve bölüm biçimi zaman damgasının yılını, ayı ve gününü belirtir. 

[![Bölüm biçimi](./media/how-to-monitor-datasets/partition-format.png)](media/how-to-monitor-datasets/partition-format-expand.png)

**Şema** ayarlarında, belirtilen veri kümesindeki sanal veya gerçek sütundaki zaman damgası sütununa belirtiniz:

![Zaman damgası](./media/how-to-monitor-datasets/timestamp.png)

## <a name="dataset-monitor-settings"></a>Dataset monitör ayarları

Belirtilen zaman damgası ayarlarıyla veri setinizi oluşturduktan sonra, veri kümesi monitörünüzü yapılandırmaya hazırsınız.

Çeşitli veri kümesi monitör ayarları üç gruba ayrılır: **Temel bilgi, Monitör ayarları** ve Yedek **dolum ayarları.**

### <a name="basic-info"></a>Temel bilgiler

Bu tablo, veri kümesi monitörü için kullanılan temel ayarları içerir.

| Ayar | Açıklama | İpuçları | Değiştirilebilir | 
| ------- | ----------- | ---- | ------- | 
| Adı | Veri kümesi monitörünün adı. | | Hayır |
| Temel veri kümesi | Zaman içinde hedef veri kümesinin karşılaştırılması için temel olarak kullanılacak tabular veri kümesi. | Temel veri kümesinin hedef veri kümesiyle ortak özellikleri olmalıdır. Genellikle, taban çizgisi bir modelin eğitim veri kümesine veya hedef veri kümesinin bir bölümüne ayarlanmalıdır. | Hayır |
| Hedef veri kümesi | Veri kayması için analiz edilecek zaman damgası sütunu ile tabular veri kümesi. | Hedef veri kümesinin temel veri kümesiyle ortak özellikleri olmalı `timeseries` ve yeni verilere eklenen bir veri kümesi olmalıdır. Hedef veri kümesindeki geçmiş veriler analiz edilebilir veya yeni veriler izlenebilir. | Hayır | 
| Frequency | Bir geri doldurma çalıştırıyorsanız, ardışık iş zamanlamak ve geçmiş verileri çözümlemek için kullanılacak sıklık. Seçenekler günlük, haftalık veya aylık içerir. | Bu ayarı, taban çizgisine karşılaştırılabilir bir veri boyutu içerecek şekilde ayarlayın. | Hayır | 
| Özellikler | Zaman içinde veri kayması için analiz edilecek özelliklerin listesi. | Kavram sürüklenme ölçmek için bir modelin çıkış özelliği(ler) ayarlayın. Zaman içinde doğal olarak sürüklenen özellikleri (ay, yıl, dizin, vb.) eklemeyin. Özellikler listesini ayarladıktan sonra geri doldurup varolan veri sürüklenme monitörünü doldurabilirsiniz. | Evet | 
| İşlem hedefi | Veri kümesi izleme işlerini çalıştırmak için Azure Machine Learning işlem hedefi. | | Evet | 

### <a name="monitor-settings"></a>Monitör ayarları

Bu ayarlar, oluşturulacak zamanlanmış veri kümesi izleme ardışık kurulumu içindir. 

| Ayar | Açıklama | İpuçları | Değiştirilebilir | 
| ------- | ----------- | ---- | ------- |
| Etkinleştirme | Veri kümesi izleme ardışık hattındaki zamanlamayı etkinleştirme veya devre dışı bırak | Geri doldurma ayarı ile geçmiş verileri çözümlemek için zamanlamayı devre dışı kakın. Veri kümesi monitörü oluşturulduktan sonra etkinleştirilebilir. | Evet | 
| Gecikme süresi | Saatler içinde, verilerin veri kümesine ulaşması zaman alır. Örneğin, verilerin SQL DB'ye ulaşması üç gün sürerse, veri kümesi kapsüller, gecikme süresini 72 olarak ayarlayın. | Veri kümesi monitörü oluşturulduktan sonra değiştirilemez | Hayır | 
| E-posta adresleri | Veri kayması yüzdesi eşiğinin ihlaline dayalı uyarı için e-posta adresleri. | E-postalar Azure Monitor üzerinden gönderilir. | Evet | 
| Eşik | E-posta uyarısı için veri kayması yüzdesi eşiği. | Çalışma alanının ilişkili Application Insights kaynağındaki diğer birçok ölçümde başka uyarılar ve olaylar ayarlanabilir. | Evet | 

### <a name="backfill-settings"></a>Geri doldurma ayarları

Bu ayarlar, veri kayması ölçümleri için geçmiş veriler üzerinde bir geri dolgu çalıştırmak içindir.

| Ayar | Açıklama | İpuçları |
| ------- | ----------- | ---- |
| Başlangıç tarihi | Geri doldurma işinin başlangıç tarihi. | | 
| Bitiş tarihi | Geri doldurma işinin bitiş tarihi. | Bitiş tarihi, başlangıç tarihinden itibaren 31*frekans lı bir zaman biriminden fazla olamaz. Varolan bir veri kümesi monitöründe, geçmiş verileri çözümlemek veya ölçümleri güncelleştirilmiş ayarlarla değiştirmek için ölçümler geri doldurulabilir. |

## <a name="create-dataset-monitors"></a>Veri kümesi monitörleri oluşturma 

Azure Machine Learning stüdyosu veya Python SDK ile yeni bir veri kümesinde veri kayması algılamak ve uyarmak için veri kümesi monitörleri oluşturun. 

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

Veri kümesi monitörünüzde uyarıları ayarlamak için, bir monitör oluşturmak için oluşturmak istediğiniz veri kümesini içeren çalışma alanının Enterprise edition özelliklerine sahip olması gerekir. 

Çalışma alanı işlevi onaylandıktan sonra stüdyonun ana sayfasına gidin ve soldaki Veri Kümeleri sekmesini seçin. Dataset monitörlerini seçin.

![İzleme listesi](./media/how-to-monitor-datasets/monitor-list.png)

**+Oluştur monitörü** düğmesine tıklayın ve **İleri'yi**tıklatarak sihirbazdan devam edin.

![Sihirbazı](./media/how-to-monitor-datasets/wizard.png)

Elde edilen veri kümesi monitörü listede görünür. Monitörün ayrıntılar sayfasına gitmek için seçin.

### <a name="from-python-sdk"></a>Tarafından Python SDK

Tüm ayrıntılar için [veri kayması ile ilgili Python SDK başvuru belgelerine](/python/api/azureml-datadrift/azureml.datadrift) bakın. 

Aşağıdaki örnek, Python SDK'yı kullanarak bir veri kümesi monitörü oluşturmanın nasıl yapılacağını gösterir

```python
from azureml.core import Workspace, Dataset
from azureml.datadrift import DataDriftDetector
from datetime import datetime

# get the workspace object
ws = Workspace.from_config()

# get the target dataset
dset = Dataset.get_by_name(ws, 'target')

# set the baseline dataset
baseline = target.time_before(datetime(2019, 2, 1))

# set up feature list
features = ['latitude', 'longitude', 'elevation', 'windAngle', 'windSpeed', 'temperature', 'snowDepth', 'stationName', 'countryOrRegion']

# set up data drift detector
monitor = DataDriftDetector.create_from_datasets(ws, 'drift-monitor', baseline, target, 
                                                      compute_target='cpu-cluster', 
                                                      frequency='Week', 
                                                      feature_list=None, 
                                                      drift_threshold=.6, 
                                                      latency=24)

# get data drift detector by name
monitor = DataDriftDetector.get_by_name(ws, 'drift-monitor')

# update data drift detector
monitor = monitor.update(feature_list=features)

# run a backfill for January through May
backfill1 = monitor.backfill(datetime(2019, 1, 1), datetime(2019, 5, 1))

# run a backfill for May through today
backfill1 = monitor.backfill(datetime(2019, 5, 1), datetime.today())

# disable the pipeline schedule for the data drift detector
monitor = monitor.disable_schedule()

# enable the pipeline schedule for the data drift detector
monitor = monitor.enable_schedule()
```

Veri kümesi ve veri `timeseries` kayması dedektörü oluşturmanın tam bir örneği için [örnek not defterimize](https://aka.ms/datadrift-notebook)bakın.

## <a name="understanding-data-drift-results"></a>Veri kayması sonuçlarını anlama

Veri monitörü iki sonuç grubu üretir: Drift'e genel bakış ve Özellik ayrıntıları. Aşağıdaki animasyon, seçili özelliği ve metnine göre kullanılabilir sürüklenme monitör grafiklerini gösterir. 

![Demo video](./media/how-to-monitor-datasets/video.gif)

### <a name="drift-overview"></a>Drift'e genel bakış

**Drift'e genel bakış** bölümü, veri kaymasının büyüklüğü ve hangi özelliklerin daha fazla araştırılması gerektiğine ilişkin üst düzey bilgiler içerir. 

| Ölçüm | Açıklama | İpuçları | 
| ------ | ----------- | ---- | 
| Veri kayması büyüklüğü | Zaman içinde taban çizgisi ve hedef veri kümesi arasında bir yüzde olarak verilir. 0 ile 100 arasında değişen, 0 aynı veri kümelerini ve 100'ün Azure Machine Learning veri kayması yeteneğini niçin iki veri kümesini birbirinden tamamen ayırt edebileceğini gösterir. | Bu büyüklüğü oluşturmak için kullanılan makine öğrenimi teknikleri nedeniyle ölçülen kesin yüzdedeki gürültü beklenmektedir. | 
| Özelliğe göre Drift katkısı | Hedef veri kümesindeki her özelliğin ölçülen sürüklenme büyüklüğüne katkısı. |  Covariate kayması nedeniyle, bir özelliğin temel dağılımının göreceli olarak yüksek özellik önemine sahip olması için mutlaka değiştirilmesi gerekmez. | 

Aşağıdaki resim, [NOAA Entegre Yüzey Verilerinin](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/)yedek doldurulundan kaynaklanan Azure Machine Learning stüdyosunda **Drift'e genel bakış** sonuçlarında görülen grafiklere bir örnektir. `stationName contains 'FLORIDA'`Veriler, Ocak 2019'un temel veri kümesi olarak kullanılması ve hedef olarak kullanılan tüm 2019 verilerine örneklendi.
 
![Drift'e genel bakış](./media/how-to-monitor-datasets/drift-overview.png)

### <a name="feature-details"></a>Özellik ayrıntıları

**Özellik ayrıntıları** bölümü, seçili özelliğin dağıtımındaki değişikliğin yanı sıra zaman içinde diğer istatistiklere ilişkin özellik düzeyinde öngörüler içerir. 

Hedef veri kümesi de zaman içinde profillenir. Her özelliğin temel dağılımı arasındaki istatistiksel uzaklık, bu istatistiksel uzaklık tek bir özellik için olması dışında, kavramsal olarak veri kayması büyüklüğüne benzer olan hedef veri kümesinin zaman içinde olan ile karşılaştırılır. Min, max ve ortalama da mevcuttur. 

Azure Machine Learning stüdyosunda, grafikteki bir veri noktasına tıklarsanız gösterilen özelliğin dağılımı buna göre ayarlanır. Varsayılan olarak, temel veri kümesinin dağıtımını ve aynı özelliğin en son çalıştırma dağılımını gösterir. 

Bu ölçümler python SDK'da bir `get_metrics()` `DataDriftDetector` nesne üzerindeki yöntem le de alınabilir. 

#### <a name="numeric-features"></a>Sayısal özellikler 

Sayısal özellikler her veri kümesi monitör çalışmasında profillenir. Aşağıdakiler Azure Machine Learning stüdyosunda açıklanmaktadır. Dağılım için olasılık yoğunluğu gösterilir.

| Ölçüm | Açıklama |  
| ------ | ----------- |  
| Wasserstein mesafesi | Temel dağılımı hedef dağılıma dönüştürmek için minimum çalışma miktarı. |
| Ortalama değer | Özelliğin ortalama değeri. |
| En düşük değer | Özelliğin minimum değeri. |
| En yüksek değer | Özelliğin maksimum değeri. |

![Özellik ayrıntıları sayısal](./media/how-to-monitor-datasets/feature-details.png)

#### <a name="categorical-features"></a>Kategorik özellikler 

Sayısal özellikler her veri kümesi monitör çalışmasında profillenir. Aşağıdakiler Azure Machine Learning stüdyosunda açıklanmaktadır. Dağılım için bir histogram gösterilir.

| Ölçüm | Açıklama |  
| ------ | ----------- |  
| Öklisidan mesafesi | Taban çizgisi ve hedef dağılımları arasındaki geometrik uzaklık. |
| Benzersiz değerler | Özelliğin benzersiz değerlerinin sayısı (kardinallik). |


![Özellik ayrıntıları kategorik](./media/how-to-monitor-datasets/feature-details2.png)

## <a name="metrics-alerts-and-events"></a>Ölçümler, uyarılar ve olaylar

Ölçümler, makine öğrenimi çalışma alanınız ile ilişkili [Azure Uygulama Öngörüleri](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) kaynağında sorgulanabilir. E-posta/SMS/Push/Voice veya Azure İşi gibi bir eylemi tetiklemek için özel uyarı kuralları ve eylem grupları için ayarlanmış da dahil olmak üzere Uygulama Öngörüleri'nin tüm özelliklerine erişim sağlar. Ayrıntılar için lütfen Uygulama Öngörüleri belgelerinin tamamına bakın. 

Başlamak için Azure portalına gidin ve çalışma alanınızın **Genel Bakış** sayfasını seçin.  İlişkili Application Insights kaynağı en sağdadır:

[![Azure portalına genel bakış](./media/how-to-monitor-datasets/ap-overview.png)](media/how-to-monitor-datasets/ap-overview-expanded.png)

Sol bölmede İzleme altında Günlükler (Analytics) seçeneğini belirleyin:

![Uygulama öngörülerine genel bakış](./media/how-to-monitor-datasets/ai-overview.png)

Veri kümesi izleme ölçümleri `customMetrics`. Bunları görüntülemek için bir veri kümesi monitörü ayarladıktan sonra bir sorgu yazabilir ve çalıştırabilirsiniz:

[![Günlük analiz sorgusu](./media/how-to-monitor-datasets/simple-query.png)](media/how-to-monitor-datasets/simple-query-expanded.png)

Uyarı kuralları ayarlamak için ölçümleri tanımladıktan sonra yeni bir uyarı kuralı oluşturun:

![Yeni uyarı kuralı](./media/how-to-monitor-datasets/alert-rule.png)

Varolan bir eylem grubunu kullanabilir veya ayarlanan koşullar karşılandığında yapılacak eylemi tanımlamak için yeni bir eylem oluşturabilirsiniz:

![Yeni eylem grubu](./media/how-to-monitor-datasets/action-group.png)

## <a name="troubleshooting"></a>Sorun giderme

Sınırlamalar ve bilinen sorunlar:

* Geri doldurma işlerinin zaman aralığı, monitörün frekans ayarının 31 aralığıyla sınırlıdır. 
* Bir özellik listesi belirtilmedikçe (tüm özellikler kullanıldı) 200 özelliğin sınırlandırılması.
* İşlem boyutu verileri işlemek için yeterince büyük olmalıdır. 
* Veri setinizin belirli bir monitör çalışması için başlangıç ve bitiş tarihinde veri olduğundan emin olun.
* Dataset monitörleri yalnızca 50 satır veya daha fazla satır içeren veri kümelerinde çalışır. 

Veri kümesindeki sütunlar veya özellikler, aşağıdaki tablodaki koşullara göre kategorik veya sayısal olarak sınıflandırılır. Özellik bu koşulları karşılamazsa - örneğin, 100 benzersiz değere >tür dize sütunu - özellik veri kayması algoritmamızdan bırakılır, ancak yine de profillenir. 

| Özellik türü | Veri türü | Koşul | Sınırlamalar | 
| ------------ | --------- | --------- | ----------- |
| Kategorik | dize, bool, int, float | Özellikteki benzersiz değerlerin sayısı 100'den az dır ve satır sayısının %5'inden azdır. | Null kendi kategorisi olarak kabul edilir. | 
| Sayısal | int, float | Özellikteki değerler sayısal veri türüne aittir ve kategorik bir özellik koşulunu karşılamaz. | Değerlerin %15'>null ise özellik düştü. | 

## <a name="next-steps"></a>Sonraki adımlar

* Veri seti monitörü kurmak için [Azure Machine Learning stüdyosuna](https://ml.azure.com) veya [Python not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datadrift-tutorial/datadrift-tutorial.ipynb) gidin.
* [Azure Kubernetes Hizmetine dağıtılan modellerde](how-to-monitor-data-drift.md)veri kaymasının nasıl ayarlandığını görün.
* [Olay ızgarası](how-to-use-event-grid.md)ile veri kümesi sürüklenme monitörleri ayarlayın. 
