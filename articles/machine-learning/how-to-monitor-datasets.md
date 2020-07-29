---
title: Veri kümelerinde veri kümeleri için analiz ve izleme (Önizleme)
titleSuffix: Azure Machine Learning
description: Azure Machine Learning veri kümeleri izleyicileri (Önizleme) oluşturun, veri kümelerinde veri kümesini izleyin ve uyarıları ayarlayın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: sgilley
ms.author: copeters
author: lostmygithubaccount
ms.date: 06/25/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 270e93302a90c458ccbdfdc4d2ced8f0d3c263af
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319686"
---
# <a name="detect-data-drift-preview-on-datasets"></a>Veri kümelerinde veri kayması (Önizleme) Algıla
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

> [!IMPORTANT]
> Veri kümelerinde verileri algılama Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Veri fazlamlarını izlemeyi ve, DRT yüksekse uyarıları ayarlamayı öğrenin.  

Azure Machine Learning veri kümesi izleyicileri (Önizleme) ile şunları yapabilirsiniz:
* Verilerin zaman içinde nasıl değiştiği hakkında bilgi sahibi olmak için **verilerinizde DRFT 'Yi çözümleyin** .
* Eğitim ve veri kümeleri sunma arasındaki farklılıklar için **model verilerini izleyin** .  [Dağıtılan modellerden model verileri toplayarak](how-to-enable-data-collection.md)başlayın.
* Herhangi bir taban çizgisi ve hedef veri kümesi arasındaki farklılıklar için **yeni verileri izleyin** .
* İstatistik özelliklerinin zaman içinde nasıl değişme şeklini izlemek için **verilerdeki özellikleri profil** oluşturma.
* Olası sorunlara yönelik erken uyarılar için **veri kayması hakkında uyarı ayarlayın** . 

İzlemeyi oluşturmak için bir [Azure Machine Learning veri kümesi](how-to-create-register-datasets.md) kullanılır. Veri kümesi bir zaman damgası sütunu içermelidir.

Python SDK ile veya Azure Machine Learning Studio 'da veri drimetrikleri görüntüleyebilirsiniz.  Diğer ölçümler ve Öngörüler, Azure Machine Learning çalışma alanıyla ilişkili [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) kaynağı aracılığıyla kullanılabilir.

> [!Important]
> SDK ile izleme verileri, tüm sürümlerde kullanılabilir. Ancak, Web 'deki Studio üzerinden veri izleme verileri yalnızca kurumsal sürümdür.

## <a name="prerequisites"></a>Önkoşullar

Veri kümesi izleyicileri oluşturmak ve bunlarla çalışmak için şunlar gerekir:
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.
* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md).
* Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), azureml veri kümesi paketini içerir.
* Dosya yolunda belirtilen bir zaman damgasına sahip yapılandırılmış (tablolu) veriler, dosya adı veya veri sütunu.

## <a name="what-is-data-drift"></a>Veri kayması nedir?

Veri kayması, zaman içinde en çok bir model doğruluk düzeyini düşürür.  Makine öğrenimi modelleri için veri dolu modeli, model girişi verilerinde performans düşüşüne yol gösteren değişikdir.  İzleme verileri, bu model performans sorunlarını algılamaya yardımcı olur.

Veri Drın nedenleri şunlardır:

- Değiştirilen bir algılayıcı gibi, ölçü birimlerini inç ve santimetre arasında değiştiren bir algılayıcı gibi yukarı akış işlemi değişiklikleri. 
- Bozuk algılayıcı gibi veri kalitesi sorunları daima 0 ' ı okur.
- Verilerdeki doğal Drın, mevsimler ile değişen ortalama sıcaklık.
- Özellikler veya birlikte değişen kaydırma arasındaki ilişki değişikliği. 

Azure Machine Learning, karşılaştırılmakta olan veri kümelerinin karmaşıklığını soyutlayan tek bir ölçüm oluşturarak yük kümesini basitleştirir.  Bu veri kümelerinde yüzlerce özellik ve on binlerce satır olabilir. Drın saptandıktan sonra, hangi özelliklerin DRIP 'ye neden olduğunu ayrıntıya katmış olursunuz.  Ardından, hata ayıklama ve Drın kök nedenini yalıtmak için özellik düzeyi ölçümlerini inceleyebilirsiniz.

Bu yukarıdan aşağı yaklaşımı, geleneksel kurallar tabanlı teknikler yerine verilerin izlenmesini kolaylaştırır. İzin verilen veri aralığı veya izin verilen benzersiz değerler gibi kurallara dayalı teknikler zaman alabilir ve hataya yol açabilir.

Azure Machine Learning, veri kümesi izleyicilerini kullanarak verileri algılayın ve uyarır.
  
### <a name="dataset-monitors"></a>Veri kümesi izleyicileri 

Veri kümesi izleyicisinde şunları yapabilirsiniz:

* Veri kümesindeki yeni verilerde verileri tespit edin ve uyarır.
* DRFT için geçmiş verileri analiz edin.
* Zaman içinde yeni veri profili.

Veri drara algoritması, verilerde bir değişikliğin genel bir ölçüsünü ve daha fazla araştırmadan hangi özelliklerin sorumlu olduğunu belirtir. Veri kümesi izleyicileri, veri kümesindeki yeni verileri profilleyerek birçok farklı ölçüm üretir `timeseries` . 

Özel uyarı, [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)aracılığıyla izleyici tarafından oluşturulan tüm ölçümlerde ayarlanabilir. Veri kümesi izleyicileri, veri sorunlarını hızla yakalamak ve olası nedenleri tanımlayarak sorun ayıklama süresini azaltmak için kullanılabilir.  

Kavramsal olarak, Azure Machine Learning veri kümesi izleyicileri ayarlamaya yönelik üç birincil senaryo vardır.

Senaryo | Açıklama
---|---
Eğitim verilerinden bir modelin drara verilerini izleme | Bu senaryonun sonuçları, model doğruluk verileri eğitim verilerinden Drifts olduğunda model doğruluğu düşürür olduğundan, modelin doğruluğu için bir proxy 'yi izleme olarak yorumlanamaz.
Bir zaman serisi veri kümesini önceki bir dönemdeki DRFT için izleyin. | Bu senaryo daha genel olduğundan, yukarı akış veya model oluşturma yönündeki aşağı akış olan veri kümelerini izlemek için kullanılabilir.  Hedef veri kümesinin bir zaman damgası sütunu olmalıdır. Taban çizgisi veri kümesi, hedef veri kümesiyle ortak özellikleri olan herhangi bir tablo veri kümesi olabilir.
Geçmiş veriler üzerinde analiz gerçekleştirin. | Bu senaryo geçmiş verileri anlamak ve veri kümesi izleyicilerine yönelik ayarlarda kararları bilgilendirmek için kullanılabilir.

Veri kümesi izleyicileri aşağıdaki Azure hizmetlerine bağımlıdır.

|Azure hizmeti  |Açıklama  |
|---------|---------|
| *Veri kümesi* | DRIFT, eğitim verilerini almak ve model eğitimi için verileri karşılaştırmak üzere Machine Learning veri kümelerini kullanır.  Minimum, Max, DISTINCT Values, DISTINCT Values Count gibi bildirilen ölçümlerin bazılarını oluşturmak için veri profili oluşturma kullanılır. |
| *Azureml işlem hattı ve işlem* | DRFT hesaplama işi, azureml işlem hattında barındırılır.  İş, istek üzerine veya DRFT izleyici oluşturma zamanında yapılandırılan bir işlem üzerinde çalışacak şekilde başlatılır.
| *Application Insights*| DRFT, Machine Learning çalışma alanına ait Application Insights ölçümleri yayar.
| *Azure Blob depolama*| Drift, Azure Blob depolama alanına JSON biçiminde ölçümler yayar.

## <a name="how-dataset-monitors-data"></a>Veri kümesi verileri nasıl izler

Veri kümesini izlemek için Machine Learning veri kümelerini kullanın. Bir temel veri kümesi belirtin-genellikle bir model için eğitim veri kümesi. Hedef veri kümesi-genellikle model girişi verileri-taban çizgisi veri kümeniz zamana göre karşılaştırılır. Bu karşılaştırma, hedef veri kümenizin belirtilen bir zaman damgası sütunu olması gerektiği anlamına gelir.

## <a name="create-target-dataset"></a>Hedef veri kümesi oluştur

Hedef veri kümesi, `timeseries` verilerdeki bir sütundan ya da dosyaların yol düzeninden türetilmiş bir sanal sütunda zaman damgası sütununu belirterek, üzerinde nitelik kümesini gerektirir. [Python SDK](#sdk-dataset) veya [Azure Machine Learning Studio](#studio-dataset)aracılığıyla bir zaman damgasıyla veri kümesi oluşturun. Veri kümesine nitelik eklemek için "timestamp" belirten bir sütun belirtilmelidir `timeseries` . Verileriniz ' {yyyy/aa/gg} ' gibi zaman bilgileriyle klasör yapısına bölünmemişse, zaman serisi işlevinin önemini artırmak için yol deseninin ayarı aracılığıyla bir sanal sütun oluşturun ve bunu "bölüm zaman damgası" olarak ayarlayın.

### <a name="python-sdk"></a><a name="sdk-dataset"></a>Python SDK'sı

[`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-)Class [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) yöntemi veri kümesi için zaman damgası sütununu tanımlar.

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

Nitelik of veri kümelerini kullanmanın tam bir örneği için `timeseries` bkz. [örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb) veya [veri kümesi SDK belgeleri](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-).

### <a name="azure-machine-learning-studio"></a><a name="studio-dataset"></a>Azure Machine Learning Studio
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

Azure Machine Learning Studio kullanarak veri kümenizi oluşturursanız, verilerinize ait yolun zaman damgası bilgilerini içerdiğinden emin olun, verileri içeren tüm alt klasörleri dahil edin ve bölüm biçimini ayarlayın.

Aşağıdaki örnekte, *Noaaisdflorde/2019* alt klasörü altındaki tüm veriler alınır ve bölüm biçimi zaman damgasının yıl, ay ve gününü belirtir.

[![Bölüm biçimi](./media/how-to-monitor-datasets/partition-format.png)](media/how-to-monitor-datasets/partition-format-expand.png)

**Şema** ayarları ' nda, belirtilen veri kümesindeki bir sanal veya gerçek sütundan zaman damgası sütununu belirtin:

:::image type="content" source="media/how-to-monitor-datasets/timestamp.png" alt-text="Zaman damgasını ayarla":::

Verileriniz tarihe göre bölümlenmemişse, burada olduğu gibi partition_timestamp de belirtebilirsiniz.  Bu, tarihlerin daha verimli işlenmesini sağlar.

:::image type="content" source="media/how-to-monitor-datasets/timeseries-partitiontimestamp.png" alt-text="Bölüm zaman damgası":::


## <a name="create-dataset-monitors"></a>Veri kümesi izleyicileri oluşturma

Yeni bir veri kümesindeki verileri algılamak ve uyarmak için veri kümesi izleyicileri oluşturun.  [Python SDK](#sdk-monitor) veya [Azure Machine Learning Studio](#studio-monitor)kullanın.

### <a name="python-sdk"></a><a name="sdk-monitor"></a>Python SDK'sı

Tüm ayrıntılar için [veri üzerinde Python SDK başvuru belgelerine](/python/api/azureml-datadrift/azureml.datadrift) bakın. 

Aşağıdaki örnek, Python SDK kullanarak bir veri kümesi izleyicisinin nasıl oluşturulduğunu gösterir

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

Bir veri kümesi ve veri kayması algılayıcısı ayarlama hakkında tam bir örnek için `timeseries` bkz. [örnek Not Defterim](https://aka.ms/datadrift-notebook).

### <a name="azure-machine-learning-studio"></a><a name="studio-monitor"></a>Azure Machine Learning Studio
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

Veri kümesi izleyicinizdeki uyarıları ayarlamak için, bir izleyici oluşturmak istediğiniz veri kümesini içeren çalışma alanının Enterprise Edition becerileri olmalıdır.

Çalışma alanı işlevselliği onaylandıktan sonra, [Studio 'nun giriş sayfasına](https://ml.azure.com) gidin ve sol taraftaki **veri kümeleri** sekmesini seçin. **Veri kümesi izleyicileri**seçin.

![İzleme listesi](./media/how-to-monitor-datasets/monitor-list.png)

**+ Izleyici oluştur** düğmesine tıklayın ve **İleri**' ye tıklayarak sihirbaza devam edin.  

:::image type="content" source="media/how-to-monitor-datasets/wizard.png" alt-text="İzleme oluşturma Sihirbazı":::

* **Hedef veri kümesini seçin**.  Hedef veri kümesi, veri kayması için analiz edilecek zaman damgası sütunuyla belirtilen tablo veri kümesidir. Hedef veri kümesinde, temel veri kümesiyle ortak olan özellikler olmalıdır ve `timeseries` yeni verilerin eklendiği bir veri kümesi olmalıdır. Hedef veri kümesindeki geçmiş verileri analiz edilebilir veya yeni veriler izlenebilir.

* **Taban çizgisi veri kümesi seçin.**  Hedef veri kümesinin zaman içinde karşılaştırmasının temeli olarak kullanılacak tablo veri kümesini seçin.  Temel veri kümesi, hedef veri kümesiyle ortak olan özelliklere sahip olmalıdır.  Hedef veri kümesinin bir dilimini kullanmak için bir zaman aralığı seçin veya taban çizgisi olarak kullanılacak ayrı bir veri kümesi belirtin.

* **İzleme ayarları**.  Bu ayarlar, oluşturulan zamanlanmış veri kümesi İzleyicisi işlem hattı içindir. 

    | Ayar | Açıklama | İpuçları | Değiştirilebilir | 
    | ------- | ----------- | ---- | ------- |
    | Ad | Veri kümesi izleyicisinin adı. | | Hayır |
    | Özellikler | Zaman içinde veri kayması için analiz edilecek özelliklerin listesi. | Bir modelin, kavram SLA 'ları ölçmek için çıkış özelliklerine ayarlayın. Zamana göre (ay, yıl, Dizin vb.) bir zaman içinde olan özellikleri içermez. Özellik listesini ayarladıktan sonra, var olan veri kayması izleyicisini geri doldurabilir. | Yes | 
    | İşlem hedefi | Veri kümesi izleyici işlerini çalıştırmak için işlem hedefini Azure Machine Learning. | | Yes | 
    | Etkinleştirme | Veri kümesi izleyici ardışık düzeninde zamanlamayı etkinleştirme veya devre dışı bırakma | Geri doldurma ayarıyla geçmiş verileri çözümleme zamanlamasını devre dışı bırakın. Veri kümesi İzleyicisi oluşturulduktan sonra etkinleştirilebilir. | Yes | 
    | Sıklık | Ardışık düzen işini zamanlamak ve geri doldurma çalıştırıyorsa geçmiş verileri çözümlemek için kullanılacak sıklık. Seçenekler günlük, haftalık veya aylık olarak verilebilir. | Her çalıştırma, hedef veri kümesindeki verileri sıklık sıklığına göre karşılaştırır: <li>Günlük: hedef veri kümesindeki en son tamamlanan günü taban çizgisiyle karşılaştırın <li>Haftalık: hedef veri kümesindeki en son tam haftayı (Pazartesi-Pazar) temel alarak karşılaştırın <li>Aylık: hedef veri kümesindeki en son tamamlanan ayı taban çizgisiyle karşılaştırın | Hayır | 
    | Gecikme süresi | Saat olarak, verilerin veri kümesine gelmesi için zaman alır. Örneğin, verilerin veri kümesi kapsülleyen SQL DB 'ye gelmesi üç gün sürüyorsa, gecikme süresini 72 olarak ayarlayın. | Veri kümesi İzleyicisi oluşturulduktan sonra değiştirilemez | Hayır | 
    | E-posta adresleri | Veri DRIP yüzdesi eşiğini ihlal eden uyarı için e-posta adresleri. | E-postalar Azure Izleyici aracılığıyla gönderilir. | Yes | 
    | Eşik | E-posta uyarısı için veri kayması yüzdesi eşiği. | Daha fazla uyarı ve olay, çalışma alanının ilişkili Application Insights kaynağındaki diğer birçok ölçümde ayarlanabilir. | Yes |

Sihirbazı tamamladıktan sonra, elde edilen veri kümesi İzleyicisi listede görüntülenir. Bu izleyicinin ayrıntılar sayfasına gitmek için seçin.

## <a name="understand-data-drift-results"></a>Veri drime sonuçlarını anlama

Bu bölümde, Azure Studio 'daki veri **kümeleri**  /  **veri kümesi izleyicileri** sayfasında bulunan bir veri kümesini izlemenin sonuçları gösterilmektedir.  Bu sayfada belirli bir zaman aralığı için ayarları güncelleştirebilir ve var olan verileri analiz edebilirsiniz.  

En üst düzey Öngörüler ile veri kayması ve daha fazla Araştırılması gereken özellikler vurgulaması ile başlayın.

:::image type="content" source="media/how-to-monitor-datasets/drift-overview.png" alt-text="Drift genel bakış":::


| Ölçüm | Açıklama | 
| ------ | ----------- | 
| Veri dolu büyüklüğü | Zaman içinde taban çizgisi ve hedef veri kümesi arasında DRFT yüzdesi. 0 ile 100 arasında bir Aralık olmak üzere, 0 özdeş veri kümeleri ve 100, Azure Machine Learning Data Ise modelinin iki veri kümesini birbirinden tamamen bildirebileceğini belirtir. Bu büyüklüğü oluşturmak için kullanılan makine öğrenimi teknikleri nedeniyle ölçülen tam yüzdedeki gürültü beklenmektedir. | 
| En üstteki Drın oluşturma özellikleri | Veri kümesinden en düzeltebilecekler sahip olan ve bu nedenle en çok kullanılan, en fazla bir ölçüm ölçüsüne katkıda bulunan özellikleri gösterir. Birlikte değişen kaydırma nedeniyle, bir özelliğin temel dağıtımı görece yüksek Özellik önem düzeyine sahip olacak şekilde değişikliğe gerek duymayabilir. |
| Eşik | Küme eşiğinin ötesinde veri dolu büyüklüğü uyarıları tetikler. Bu, izleyici ayarlarından yapılandırılabilir. | 

### <a name="drift-magnitude-trend"></a>Sürücü büyüklüğü eğilimi

Veri kümesinin, belirtilen dönemde hedef veri kümesinden farklı olduğunu görün.  Daha fazla %100, iki veri kümesi farklılık gösterir.

:::image type="content" source="media/how-to-monitor-datasets/drift-magnitude.png" alt-text="Sürücü büyüklüğü eğilimi":::

### <a name="drift-magnitude-by-features"></a>Özelliklere göre DRT büyüklüğü

Bu bölümde, Seçili özelliğin dağıtımında değişiklik ve zaman içinde diğer istatistiklerle ilgili özellik düzeyi Öngörüler bulunur.

Hedef veri kümesi zaman içinde de profili oluşturulmuş olur. Her bir özelliğin temel dağıtımı arasındaki istatistiksel mesafe, hedef veri kümesiyle zaman içinde karşılaştırılır.  Kavramsal olarak, bu veri büyüklüğü ile benzerdir.  Ancak bu istatistiksel mesafe, tüm özellikler yerine bireysel bir özellik içindir. Min, Max ve ortalama de mevcuttur.

Azure Machine Learning Studio 'da, bu tarihin Özellik düzeyi ayrıntılarını görmek için grafikteki bir çubuğa tıklayın. Varsayılan olarak, temel veri kümesinin dağıtımını ve en son çalıştırmanın aynı özelliğin dağıtımını görürsünüz.

:::image type="content" source="media/how-to-monitor-datasets/drift-by-feature.gif" alt-text="Özelliklere göre DRT büyüklüğü":::

Bu ölçümler Ayrıca `get_metrics()` bir nesne üzerindeki yöntemi aracılığıyla Python SDK ' da alınabilir `DataDriftDetector` .

### <a name="feature-details"></a>Özellik ayrıntıları

Son olarak, her bir özelliğin ayrıntılarını görüntülemek için aşağı kaydırın.  Özelliği seçmek için grafiğin üzerindeki açılan listeleri kullanın ve ayrıca görüntülemek istediğiniz ölçümü seçin.

:::image type="content" source="media/how-to-monitor-datasets/numeric-feature.gif" alt-text="Sayısal Özellik grafiği ve karşılaştırması":::

Grafikteki ölçümler özelliğin türüne bağlıdır.

* Sayısal özellikler

    | Ölçüm | Açıklama |  
    | ------ | ----------- |  
    | Wasserstein mesafe | Taban çizgisi dağıtımını hedef dağıtıma dönüştürmek için gereken en düşük iş miktarı. |
    | Ortalama değer | Özelliğin ortalama değeri. |
    | En düşük değer | Özelliğin en küçük değeri. |
    | En yüksek değer | Özelliğin maksimum değeri. |

* Kategorik Özellikler
    
    | Ölçüm | Açıklama |  
    | ------ | ----------- |  
    | Eucliyen uzaklığı     |  Kategorik sütunlar için hesaplanır.Euclidean bir mesafe iki vektörde hesaplanır, iki veri kümesinden de aynı kategorik sütunun empırical dağılımesinden oluşturulur.0, empırik dağıtımlarıyla bir fark olmadığını gösterir.0 ' dan fazla farklılık gösterir, bu sütunda daha fazla düzeltebilecekler vardır.Eğilimler, bu ölçümün zaman serisi çiziminde gözlemlenebilir ve bir dral oluşturma özelliğinin yanı sıra faydalı olabilir.  |
    | Benzersiz değerler | Özelliğin benzersiz değerlerinin (kardinalite) sayısı. |

Bu grafikte, görüntülenecek özellik için hedef ve bu tarih arasındaki özellik dağıtımını karşılaştırmak için tek bir tarih seçin. Sayısal özellikler için, bu iki olasılık dağıtımlarını gösterir.  Özellik sayısal ise, bir çubuk grafik görüntülenir.

:::image type="content" source="media/how-to-monitor-datasets/select-date-to-compare.gif" alt-text="Hedefle Karşılaştırılacak bir tarih seçin":::

## <a name="metrics-alerts-and-events"></a>Ölçümler, uyarılar ve olaylar

Ölçümler, Machine Learning çalışma alanı ile ilişkili [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) kaynağında sorgulanabilir. E-posta/SMS/Push/Voice veya Azure Işlevi gibi bir eylemi tetiklemek için özel uyarı kuralları ve eylem grupları için ayarlama dahil Application Insights tüm özelliklerine erişebilirsiniz. Ayrıntılar için Application Insights tüm belgeleri inceleyin. 

Başlamak için [Azure Portal](https://portal.azure.com) gidin ve çalışma alanınızın **genel bakış** sayfasını seçin.  İlişkili Application Insights kaynağı en sağda:

[![Azure portal genel bakış](./media/how-to-monitor-datasets/ap-overview.png)](media/how-to-monitor-datasets/ap-overview-expanded.png)

Sol bölmedeki Izleme altında günlükleri (Analiz) seçin:

![Application Insights genel bakış](./media/how-to-monitor-datasets/ai-overview.png)

Veri kümesi izleme ölçümleri olarak depolanır `customMetrics` . Bir veri kümesi izleyicisini ayarladıktan sonra, bunları görüntülemek için bir sorgu yazabilir ve çalıştırabilirsiniz:

[![Log Analytics sorgusu](./media/how-to-monitor-datasets/simple-query.png)](media/how-to-monitor-datasets/simple-query-expanded.png)

Uyarı kurallarını ayarlamaya yönelik ölçümleri tanımladıktan sonra yeni bir uyarı kuralı oluşturun:

![Yeni uyarı kuralı](./media/how-to-monitor-datasets/alert-rule.png)

Küme Koşulları karşılandığında gerçekleştirilecek eylemi tanımlamak için mevcut bir eylem grubunu kullanabilir veya yeni bir tane oluşturabilirsiniz:

![Yeni eylem grubu](./media/how-to-monitor-datasets/action-group.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bir veri kümesi izleyicisini ayarlamak için [Azure Machine Learning Studio](https://ml.azure.com) 'Ya veya [Python not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datadrift-tutorial/datadrift-tutorial.ipynb) gidin.
* Bkz. [Azure Kubernetes hizmetine dağıtılan modellerde](how-to-monitor-data-drift.md)veri drları ayarlama.
* [Olay kılavuzuyla](how-to-use-event-grid.md)veri kümesi DRFT izleyicileri ayarlayın. 
* Sorun yaşıyorsanız bu ortak [sorun giderme ipuçlarına](resource-known-issues.md#data-drift) bakın.
