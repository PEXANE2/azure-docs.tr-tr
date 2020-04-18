---
title: Azure Machine Learning ardışık hatlarını zamanlama
titleSuffix: Azure Machine Learning
description: Python için Azure Machine Learning SDK'yı kullanarak Azure Machine Learning ardışık hatlarını zamanlayın. Zamanlanmış ardışık işlemler, veri işleme, eğitim ve izleme gibi rutin, zaman alan görevleri otomatikleştirmenize olanak tanır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 11/12/2019
ms.openlocfilehash: d9e1bff3d25a978b5159d8e6ab8ab2453df77ca3
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81640507"
---
# <a name="schedule-machine-learning-pipelines-with-azure-machine-learning-sdk-for-python"></a>Python için Azure Machine Learning SDK ile makine öğrenimi boru hatlarını zamanlayın

Bu makalede, Azure'da çalışacak bir ardışık olarak bir ardışık olarak nasıl zamanlayabileceğinizi öğreneceksiniz. Geçen süreyi veya dosya sistemi değişikliklerini temel alan bir zamanlama oluşturmayı seçebilirsiniz. Zaman tabanlı zamanlamalar, veri kayması izleme gibi rutin görevleri halletmek için kullanılabilir. Değişiklik tabanlı zamanlamalar, yüklenen yeni veriler veya düzenlenen eski veriler gibi düzensiz veya öngörülemeyen değişikliklere tepki vermek için kullanılabilir. Zamanlamaları nasıl oluşturacağınıöğrendikten sonra, bunları nasıl alıp devre dışı bırakacağınız öğrenilir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://aka.ms/AMLFree)oluşturun.

* Python için Azure Machine Learning SDK'nın yüklü olduğu bir Python ortamı. Daha fazla bilgi için [bkz.](how-to-use-environments.md)

* Yayımlanmış bir boru hattı yla Machine Learning çalışma alanı. [Azure Machine Learning SDK ile makine öğrenimi boru hatlarını oluştur ve çalıştır'da](how-to-create-your-first-pipeline.md)yerleşik olanı kullanabilirsiniz.

## <a name="initialize-the-workspace--get-data"></a>Veri almak & çalışma alanını başlatma

Bir ardışık zamanlamak için çalışma alanınıza, yayımlanmış ardışık alanınızın tanımlayıcısına ve zamanlamayı oluşturmak istediğiniz denemenin adı için bir başvuru yapmanız gerekir. Aşağıdaki kodla bu değerleri alabilirsiniz:

```Python
import azureml.core
from azureml.core import Workspace
from azureml.pipeline.core import Pipeline, PublishedPipeline
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

experiments = Experiment.list(ws)
for experiment in experiments:
    print(experiment.name)

published_pipelines = PublishedPipeline.list(ws)
for published_pipeline in  published_pipelines:
    print(f"{published_pipeline.name},'{published_pipeline.id}'")

experiment_name = "MyExperiment" 
pipeline_id = "aaaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" 
```

## <a name="create-a-schedule"></a>Zamanlama oluşturma

Bir ardışık ardışık hattı yinelenen olarak çalıştırmak için bir zamanlama oluşturursunuz. Bir `Schedule` boru hattı, bir deney ve bir tetikleyici ilişkilendirir. Tetikleyici, çalıştırmalar`ScheduleRecurrence` arasındaki beklemeyi açıklayan veya değişiklikleri izlemek için bir dizini belirten bir Datastore yolu olabilir. Her iki durumda da, zamanlamayı oluşturmak için ardışık hatlar tanımlayıcısına ve denemenin adını almanız gerekir.

Python dosyanızın üst kısmında, `Schedule` ve `ScheduleRecurrence` sınıfları içe aktarın:

```python

from azureml.pipeline.core.schedule import ScheduleRecurrence, Schedule
```

### <a name="create-a-time-based-schedule"></a>Zaman tabanlı zamanlama oluşturma

Oluşturucu `ScheduleRecurrence` aşağıdaki dizeleri olmalıdır gerekli `frequency` bir argüman vardır: "Dakika", "Saat", "Gün", "Hafta", veya "Ay". Ayrıca, zamanlama başlar `interval` arasında birimlerin kaç `frequency` tanesinin atlanması gerektiğini belirten bir tamsayı bağımsız değişkeni gerektirir. İsteğe bağlı bağımsız [değişkenler, ScheduleRecurrence SDK dokümanlarında](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedulerecurrence?view=azure-ml-py)ayrıntılı olarak belirtildiği gibi başlangıç saatleri hakkında daha spesifik olmanızı sağlar.

Her `Schedule` 15 dakikada bir çalıştırma başlatan bir çalışma oluşturun:

```python
recurrence = ScheduleRecurrence(frequency="Minute", interval=15)
recurring_schedule = Schedule.create(ws, name="MyRecurringSchedule", 
                            description="Based on time",
                            pipeline_id=pipeline_id, 
                            experiment_name=experiment_name, 
                            recurrence=recurrence)
```

### <a name="create-a-change-based-schedule"></a>Değişiklik tabanlı zamanlama oluşturma

Dosya değişiklikleri tarafından tetiklenen akdeler zaman tabanlı zamanlamalardan daha verimli olabilir. Örneğin, bir dosya değiştirildiğinde veya veri dizinine yeni bir dosya eklendiğinde ön işleme adımı gerçekleştirmek isteyebilirsiniz. Veri deposundaki değişiklikleri veya veri deposu içindeki belirli bir dizindeki değişiklikleri izleyebilirsiniz. Belirli bir dizini izlerseniz, bu dizinin alt dizini ndeki değişiklikler bir çalıştırmayı _tetiklemez._

Bir dosya reaktif `Schedule`oluşturmak için, `datastore` [zamanlama.create](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedule?view=azure-ml-py#create-workspace--name--pipeline-id--experiment-name--recurrence-none--description-none--pipeline-parameters-none--wait-for-provisioning-false--wait-timeout-3600--datastore-none--polling-interval-5--data-path-parameter-name-none--continue-on-step-failure-none--path-on-datastore-none---workflow-provider-none---service-endpoint-none-)için arama parametre ayarlamanız gerekir. Bir klasörü izlemek `path_on_datastore` için bağımsız değişkeni ayarlayın.

Bağımsız `polling_interval` değişken, veri deposunun değişiklikler için kontrol edildiği sıklık ları dakikalar içinde belirtmenize olanak tanır.

Ardışık etki alanı Bir [DataPath](https://docs.microsoft.com/python/api/azureml-core/azureml.data.datapath.datapath?view=azure-ml-py) [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelineparameter?view=azure-ml-py)ile oluşturulmuşsa, `data_path_parameter_name` bağımsız değişkeni ayarlayarak bu değişkeni değiştirilen dosyanın adına ayarlayabilirsiniz.

```python
datastore = Datastore(workspace=ws, name="workspaceblobstore")

reactive_schedule = Schedule.create(ws, name="MyReactiveSchedule", description="Based on time",
                            pipeline_id=pipeline_id, experiment_name=experiment_name, datastore=datastore, data_path_parameter_name="input_data")
```

### <a name="optional-arguments-when-creating-a-schedule"></a>Zamanlama oluştururken isteğe bağlı bağımsız değişkenler

Daha önce tartışılan bağımsız değişkenlere ek olarak, bağımsız değişkeni `status` etkin olmayan bir zamanlama oluşturacak şekilde `"Disabled"` ayarlayabilirsiniz. Son olarak, ardışık ardışık alt yapının varsayılan hata davranışını geçersiz kılacak bir Boolean'ı geçirmenizi `continue_on_step_failure` sağlar.

### <a name="use-azure-logic-apps-for-more-complex-workflows"></a>Daha karmaşık iş akışları için Azure Mantık Uygulamalarını kullanma

Azure Logic Apps daha karmaşık iş akışlarını destekler ve Azure Machine Learning ardışık iş aktarımlarından çok daha geniş bir şekilde tümleştirilir. Daha fazla bilgi için [Logic App'ten Machine Learning ardışık hattının çalışmasını tetikle'ye](how-to-trigger-published-pipeline.md) bakın.

## <a name="view-your-scheduled-pipelines"></a>Zamanlanmış ardışık hatlarınızı görüntüleme

Web tarayıcınızda Azure Machine Learning'e gidin. Gezinti panelinin **Uç Noktaları** bölümünden, Pipeline **uç noktalarını**seçin. Bu, çalışma alanında yayınlanan ardışık lıkların listesine götürür.

![AML boru hatları sayfası](./media/how-to-schedule-pipelines/scheduled-pipelines.png)

Bu sayfada Çalışma Alanı'ndaki tüm ardışık işlerle ilgili özet bilgileri görebilirsiniz: adlar, açıklamalar, durum vb. Boru hattınızı tıklayarak matkap. Ortaya çıkan sayfada, ardışık hattınız hakkında daha fazla ayrıntı vardır ve tek tek çalıştırmaları inceleyebilirsiniz.

## <a name="deactivate-the-pipeline"></a>Ardışık hattı devre dışı bırakma

Yayımlanmış, `Pipeline` ancak zamanlanmamış bir yayına sahipseniz, şu şekilde devre dışı kullanabilirsiniz:

```python
pipeline = PublishedPipeline.get(ws, id=pipeline_id)
pipeline.disable()
```

Ardışık hatlar zamanlanmışsa, önce zamanlamayı iptal etmelisiniz. Zamanlamatanımlayıcısını portaldan veya çalıştırarak alın:

```python
ss = Schedule.list(ws)
for s in ss:
    print(s)
```

Devre dışı `schedule_id` kalmak istediğiniz de varsa, çalıştırın:

```python
def stop_by_schedule_id(ws, schedule_id):
    s = next(s for s in Schedule.list(ws) if s.id == schedule_id)
    s.disable()
    return s

stop_by_schedule_id(ws, schedule_id)
```

Daha sonra `Schedule.list(ws)` tekrar çalıştırırsanız, boş bir liste almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ardışık işlem hattını iki farklı şekilde zamanlamak için Python için Azure Machine Learning SDK'yı kullandınız. Bir zamanlama, geçen saat süresine bağlı olarak yinelenir. Bir dosya belirtilen `Datastore` bir dizinde veya o mağazadaki bir dizinde değiştirilirse, diğer zamanlama çalışır. Boru hattını ve tek tek çalıştırmaları incelemek için portalı nasıl kullanacağınızı gördünüz. Son olarak, ardışık programın çalışmayı durdurması için zamanlamayı nasıl devre dışı bırakabileceğinizi öğrendiniz.

Daha fazla bilgi için bkz.

> [!div class="nextstepaction"]
> [Toplu puanlama için Azure Machine Learning Pipelines'ı kullanma](tutorial-pipeline-batch-scoring-classification.md)

* [Boru hatları](concept-ml-pipelines.md) hakkında daha fazla bilgi edinin
* [Jupyter ile Azure Machine Learning'i keşfetme](samples-notebooks.md) hakkında daha fazla bilgi edinin

