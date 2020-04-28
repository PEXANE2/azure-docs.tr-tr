---
title: Hata ayıklama ve ParallelRunStep sorunlarını giderme
titleSuffix: Azure Machine Learning
description: Python için Azure Machine Learning SDK 'sında makine öğrenimi komut zincirlerinde ParallelRunStep hatalarını ayıklayın ve sorun giderin. İşlem hatlarında ve uzaktan yürütme sırasında ve betiklerinizde hata ayıklamanıza yardımcı olacak ipuçları hakkında genel bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye, jmartens, larryfr, vaidyas
ms.author: trmccorm
author: tmccrmck
ms.date: 01/15/2020
ms.openlocfilehash: ca50d70965d5edc4e31606e542ddf163fe3b0741
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76122970"
---
# <a name="debug-and-troubleshoot-parallelrunstep"></a>Hata ayıklama ve ParallelRunStep sorunlarını giderme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)'Dan [Parallelrunstep](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallel_run_step.parallelrunstep?view=azure-ml-py) sınıfında hata ayıklamayı ve sorun gidermeyi öğreneceksiniz.

## <a name="testing-scripts-locally"></a>Betikleri yerel olarak test etme

Makine öğrenimi ardışık düzenleri için [betikleri yerel olarak test etme bölümüne](how-to-debug-pipelines.md#testing-scripts-locally) bakın. Parallelrunadımınız, ML işlem hatlarında bir adım olarak çalışarak aynı yanıtın her ikisi için de geçerlidir.

## <a name="debugging-scripts-from-remote-context"></a>Uzak bağlamdaki betiklerin hatalarını ayıklama

Gerçek bir işlem hattındaki Puanlama betiğine yerel olarak hata ayıklama için bir Puanlama betiğinin hata ayıklamasının yapılması zor olabilir. Portalda günlüklerinizi bulma hakkında daha fazla bilgi için, [uzak bağlamdaki betiklerin Hata ayıklamasında makine öğrenimi ardışık düzenleri bölümüne](how-to-debug-pipelines.md#debugging-scripts-from-remote-context)bakın. Bu bölümdeki bilgiler, paralel bir adım çalıştırması için de geçerlidir.

Örneğin, günlük dosyası `70_driver_log.txt` , denetleyiciyi paralel çalıştırma adım kodu Başlatan denetleyiciden bilgiler içerir.

Paralel çalışma işlerinin dağıtılmış doğası nedeniyle, birkaç farklı kaynaktaki Günlükler vardır. Ancak, üst düzey bilgi sağlayan iki birleştirilmiş dosya oluşturulur:

- `~/logs/overview.txt`: Bu dosya, şimdiye kadar oluşturulan mini toplu işlerin sayısı (görevler olarak da bilinir) ve şimdiye kadar işlenen mini toplu iş sayısı hakkında üst düzey bilgiler sağlar. Bu uçta, işin sonucunu gösterir. İş başarısız olursa, hata iletisini ve sorun giderme işleminin başlatılacağı konumu gösterir.

- `~/logs/sys/master.txt`: Bu dosya, çalışan işin ana düğümü (Orchestrator olarak da bilinir) görünümünü sağlar. Görev oluşturma, ilerleme izleme, çalıştırma sonucu içerir.

Giriş betiğinden EntryScript. günlükçü ve Print deyimlerini kullanarak oluşturulan Günlükler aşağıdaki dosyalarda bulunur:

- `~/logs/user/<ip_address>/Process-*.txt`: Bu dosya, EntryScript. günlükçü kullanılarak entry_script yazılmış günlükleri içerir. Ayrıca, entry_script 'den Print deyiminizi (STDOUT) içerir.

Her bir düğümün puan betiğini nasıl yürütülebileceğini tam olarak anlamak istediğinizde her düğüm için ayrı işlem günlüklerine bakın. İşlem günlükleri, çalışan düğümlerine göre gruplanmış şekilde `sys/worker` klasöründe bulunabilir:

- `~/logs/sys/worker/<ip_address>/Process-*.txt`: Bu dosya, bir çalışan tarafından çekildiği veya tamamlandığı için her bir mini toplu iş hakkında ayrıntılı bilgi sağlar. Her mini toplu iş için bu dosya şunları içerir:

    - Çalışan işlemin IP adresi ve PID 'SI. 
    - Toplam öğe sayısı, öğe sayısı başarıyla işlendi ve başarısız öğe sayısı.
    - Başlangıç zamanı, süre, işlem süresi ve çalıştırma yöntemi zamanı.

Her çalışan için işlemlerin kaynak kullanımı hakkındaki bilgileri de bulabilirsiniz. Bu bilgiler CSV biçimindedir ve konumunda `~/logs/sys/perf/<ip_address>/`bulunur. Tek bir düğüm için, iş dosyaları altında `~logs/sys/perf`kullanılabilir olacaktır. Örneğin, kaynak kullanımı denetlenirken aşağıdaki dosyalara bakın:

- `Process-*.csv`: Çalışan işlem kaynak kullanımı başına. 
- `sys.csv`: Düğüm başına günlük.

### <a name="how-do-i-log-from-my-user-script-from-a-remote-context"></a>Kullanıcı betiğimin uzak bağlamdan Nasıl yaparım? mi?
Günlüklerin **günlüklerde/Kullanıcı** klasöründe günlük görünmesini sağlamak için aşağıdaki örnek kodda gösterildiği gibi entryscript 'ten bir günlükçü alabilirsiniz.

**Günlükçü kullanılarak örnek bir giriş betiği:**
```python
from entry_script import EntryScript

def init():
    """ Initialize the node."""
    entry_script = EntryScript()
    logger = entry_script.logger
    logger.debug("This will show up in files under logs/user on the Azure portal.")


def run(mini_batch):
    """ Accept and return the list back."""
    # This class is in singleton pattern and will return same instance as the one in init()
    entry_script = EntryScript()
    logger = entry_script.logger
    logger.debug(f"{__file__}: {mini_batch}.")
    ...

    return mini_batch
```

### <a name="how-could-i-pass-a-side-input-such-as-a-file-or-files-containing-a-lookup-table-to-all-my-workers"></a>Tüm çalışanlarıma, bir arama tablosu içeren dosya veya dosyalar gibi bir yan girişi nasıl geçebilirim?

Yan girişi içeren bir [veri kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) nesnesi oluşturun ve çalışma alanınıza kaydolun. Bundan sonra, çıkarım betiğinizdeki (örneğin, init () yönteminde) aşağıdaki gibi erişebilirsiniz:

```python
from azureml.core.run import Run
from azureml.core.dataset import Dataset

ws = Run.get_context().experiment.workspace
lookup_ds = Dataset.get_by_name(ws, "<registered-name>")
lookup_ds.download(target_path='.', overwrite=True)
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azureml-contrib-işlem hattı-adım](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps?view=azure-ml-py) paketi ve ParallelRunStep sınıfı [belgeleriyle](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallelrunstep?view=azure-ml-py) ilgili yardım için SDK başvurusuna bakın.

* Paralel çalıştırma adımıyla işlem hatlarını kullanma hakkında [Gelişmiş öğreticiyi](tutorial-pipeline-batch-scoring-classification.md) izleyin.
