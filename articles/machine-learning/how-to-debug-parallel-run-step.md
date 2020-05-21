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
ms.openlocfilehash: c4e2777f59bab8d7d874019004bff2e30395ab1d
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83723485"
---
# <a name="debug-and-troubleshoot-parallelrunstep"></a>Hata ayıklama ve ParallelRunStep sorunlarını giderme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)'Dan [Parallelrunstep](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallel_run_step.parallelrunstep?view=azure-ml-py) sınıfında hata ayıklamayı ve sorun gidermeyi öğreneceksiniz.

## <a name="testing-scripts-locally"></a>Betikleri yerel olarak test etme

Makine öğrenimi ardışık düzenleri için [betikleri yerel olarak test etme bölümüne](how-to-debug-pipelines.md#testing-scripts-locally) bakın. Parallelrunadımınız, ML işlem hatlarında bir adım olarak çalışarak aynı yanıtın her ikisi için de geçerlidir.

## <a name="debugging-scripts-from-remote-context"></a>Uzak bağlamdaki betiklerin hatalarını ayıklama

Gerçek bir işlem hattındaki Puanlama betiğine yerel olarak hata ayıklama için bir Puanlama betiğinin hata ayıklamasının yapılması zor olabilir. Portalda günlüklerinizi bulma hakkında daha fazla bilgi için, [uzak bağlamdaki betiklerin Hata ayıklamasında makine öğrenimi ardışık düzenleri bölümüne](how-to-debug-pipelines.md#debugging-scripts-from-remote-context)bakın. Bu bölümdeki bilgiler bir ParallelRunStep için de geçerlidir.

Örneğin, günlük dosyası `70_driver_log.txt` denetleyicinin ParallelRunStep kodunu Başlatan bilgilerini içerir.

ParallelRunStep işlerinin dağıtılmış doğası nedeniyle, birkaç farklı kaynaktaki Günlükler vardır. Ancak, üst düzey bilgi sağlayan iki birleştirilmiş dosya oluşturulur:

- `~/logs/overview.txt`: Bu dosya, şimdiye kadar oluşturulan mini toplu işlerin sayısı (görevler olarak da bilinir) ve şimdiye kadar işlenen mini toplu iş sayısı hakkında üst düzey bilgiler sağlar. Bu uçta, işin sonucunu gösterir. İş başarısız olursa, hata iletisini ve sorun giderme işleminin başlatılacağı konumu gösterir.

- `~/logs/sys/master.txt`: Bu dosya, çalışan işin ana düğümü (Orchestrator olarak da bilinir) görünümünü sağlar. Görev oluşturma, ilerleme izleme, çalıştırma sonucu içerir.

Giriş betiğinin EntryScript yardımcısını ve Print deyimlerini kullanarak oluşturulan Günlükler aşağıdaki dosyalarda bulunur:

- `~/logs/user/<ip_address>/<node_name>.log.txt`: Bunlar EntryScript Yardımcısı kullanılarak entry_script yazılan günlüklerdir. Ayrıca entry_script 'den Print deyiminizi (STDOUT) içerir.

Betikteki hataların kısa bir şekilde anlaşılmasından bazıları şunlardır:

- `~/logs/user/error.txt`: Bu dosya, betiğinizdeki hataları özetlemeye çalışacaktır.

Betiğinizdeki hatalar hakkında daha fazla bilgi için şu olabilir:

- `~/logs/user/error/`: Oluşturulan tüm hataları ve düğüm tarafından düzenlenen tam yığın izlemelerini içerir.

Her bir düğümün puan betiğini nasıl yürütülebileceğini tam olarak anlamak istediğinizde her düğüm için ayrı işlem günlüklerine bakın. İşlem günlükleri, `sys/node` çalışan düğümlerine göre gruplanmış şekilde klasöründe bulunabilir:

- `~/logs/sys/node/<node_name>.txt`: Bu dosya, bir çalışan tarafından çekildiği veya tamamlandığı için her bir mini toplu iş hakkında ayrıntılı bilgi sağlar. Her mini toplu iş için bu dosya şunları içerir:

    - Çalışan işlemin IP adresi ve PID 'SI. 
    - Toplam öğe sayısı, öğe sayısı başarıyla işlendi ve başarısız öğe sayısı.
    - Başlangıç zamanı, süre, işlem süresi ve çalıştırma yöntemi zamanı.

Her çalışan için işlemlerin kaynak kullanımı hakkındaki bilgileri de bulabilirsiniz. Bu bilgiler CSV biçimindedir ve konumunda bulunur `~/logs/sys/perf/overview.csv` . Her işlem hakkında daha fazla bilgi için, altında kullanılabilir `~logs/sys/processes.csv` .

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

Yan girişi içeren bir [veri kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) oluşturun ve çalışma alanınıza kaydedin. Bu `side_input` parametreye geçirin `ParallelRunStep` . Ayrıca, `arguments` bağlı yoluna kolayca erişmek için bu yolu bölümüne ekleyebilirsiniz:

```python
label_config = label_ds.as_named_input("labels_input")
batch_score_step = ParallelRunStep(
    name=parallel_step_name,
    inputs=[input_images.as_named_input("input_images")],
    output=output_dir,
    arguments=["--labels_dir", label_config],
    side_inputs=[label_config],
    parallel_run_config=parallel_run_config,
)
```

Bundan sonra, çıkarım betiğinizdeki (örneğin, init () yönteminde) aşağıdaki gibi erişebilirsiniz:

```python
parser = argparse.ArgumentParser()
parser.add_argument('--labels_dir', dest="labels_dir", required=True)
args, _ = parser.parse_known_args()

labels_path = args.labels_dir
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azureml-contrib-işlem hattı-adım](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps?view=azure-ml-py) paketi ve ParallelRunStep sınıfı [belgeleriyle](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallelrunstep?view=azure-ml-py) ilgili yardım için SDK başvurusuna bakın.

* ParallelRunStep ile işlem hatlarını kullanma hakkında [Gelişmiş öğreticiyi](tutorial-pipeline-batch-scoring-classification.md) ve diğer bir dosyayı yan giriş olarak geçirme örneği için izleyin. 
