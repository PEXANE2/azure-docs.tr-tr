---
title: Hata Ayıklama ve Sorun Giderme ParallelRunStep
titleSuffix: Azure Machine Learning
description: Python için Azure Machine Learning SDK'daki makine öğrenimi boru hatlarında ParallelRunStep hata ayıklama ve sorun giderme. Ardışık hatlar ile geliştirmenin yaygın tuzaklarını ve komut dosyalarınızı uzaktan yürütmeden önce ve sırasında hata ayıklamanıza yardımcı olacak ipuçlarını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye, jmartens, larryfr, vaidyas
ms.author: trmccorm
author: tmccrmck
ms.date: 01/15/2020
ms.openlocfilehash: ca50d70965d5edc4e31606e542ddf163fe3b0741
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122970"
---
# <a name="debug-and-troubleshoot-parallelrunstep"></a>Hata Ayıklama ve Sorun Giderme ParallelRunStep
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure Machine Learning SDK'dan](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) [ParallelRunStep](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallel_run_step.parallelrunstep?view=azure-ml-py) sınıfını nasıl hata ayıklama ve sorun giderme yi öğreneceksiniz.

## <a name="testing-scripts-locally"></a>Komut dosyalarını yerel olarak test etme

Makine öğrenimi boru hatları için [yerel test komut dosyası bölümüne](how-to-debug-pipelines.md#testing-scripts-locally) bakın. ParallelRunStep'iniz ML ardışık hatlarında bir adım olarak çalışır, böylece aynı yanıt her ikisi için de geçerlidir.

## <a name="debugging-scripts-from-remote-context"></a>Komut dosyalarını uzak bağlamdan hata ayıklama

Bir puanlama komut dosyasının yerel olarak hata ayıklanmasından gerçek bir ardışık ardışık alt bilgi hattında hata ayıklamasına geçiş zor bir sıçrama olabilir. Portalda günlüklerinizi bulma hakkında bilgi için, [makine öğrenme boru hatları bölümü uzak bir bağlamdan komut ayıklama.](how-to-debug-pipelines.md#debugging-scripts-from-remote-context) Bu bölümdeki bilgiler, paralel bir adım çalıştırıiçin de geçerlidir.

Örneğin, günlük dosyası, `70_driver_log.txt` paralel çalıştırma adım kodunu başlatan denetleyiciden gelen bilgileri içerir.

Paralel çalıştırma işlerinin dağıtılmış doğası nedeniyle, birkaç farklı kaynaktan günlükler vardır. Ancak, üst düzey bilgi sağlayan iki birleştirilmiş dosya oluşturulur:

- `~/logs/overview.txt`: Bu dosya, şimdiye kadar oluşturulan mini toplu iş sayısı (görevler olarak da bilinir) ve şimdiye kadar işlenen mini toplu iş sayısı hakkında üst düzey bir bilgi sağlar. Bu noktada, işin sonucunu gösterir. İş başarısız olduysa, hata iletisini ve sorun giderme nereden başlatılacağını gösterir.

- `~/logs/sys/master.txt`: Bu dosya, çalışan işin ana düğüm (orkestratör olarak da bilinir) görünümünü sağlar. Görev oluşturma, ilerleme izleme, çalıştırma sonucu içerir.

EntryScript.logger ve yazdırma ekstrelerini kullanarak giriş komut dosyasından oluşturulan günlükler aşağıdaki dosyalarda bulunur:

- `~/logs/user/<ip_address>/Process-*.txt`: Bu dosya, entryscript.logger kullanarak entry_script yazılmış günlükleri içerir. Ayrıca entry_script baskı deyimi (stdout) içerir.

Her düğümün skor komut dosyasını nasıl çalıştırdığını tam olarak anlamanız gerektiğinde, her düğüm için tek tek işlem günlüklerine bakın. İşlem `sys/worker` günlükleri, alt düğümlere göre gruplanmış klasörde bulunabilir:

- `~/logs/sys/worker/<ip_address>/Process-*.txt`: Bu dosya, bir işçi tarafından alınır veya tamamlanır gibi her mini toplu iş hakkında ayrıntılı bilgi sağlar. Her mini toplu iş için bu dosya şunları içerir:

    - İşçi işleminin IP adresi ve PID'si. 
    - Toplam madde sayısı, başarıyla işlenen madde sayısı ve başarısız madde sayısı.
    - Başlangıç zamanı, süre, işlem süresi ve çalışma yöntemi süresi.

Ayrıca, her bir çalışan için işlemlerin kaynak kullanımı hakkında bilgi bulabilirsiniz. Bu bilgiler CSV formatındadır `~/logs/sys/perf/<ip_address>/`ve . Tek bir düğüm için iş dosyaları `~logs/sys/perf`. Örneğin, kaynak kullanımını denetlerken aşağıdaki dosyalara bakın:

- `Process-*.csv`: İşçi süreci kaynak kullanımı başına. 
- `sys.csv`: Düğüm kütüğü başına.

### <a name="how-do-i-log-from-my-user-script-from-a-remote-context"></a>Kullanıcı komut dosyamdan uzak bir bağlamdan nasıl günlüğe kaydolurum?
Portaldaki **günlüklerin/kullanıcı** klasöründe günlüklerin görünmesi için aşağıda örnek kodda gösterildiği gibi EntryScript'ten bir logger alabilirsiniz.

**Logger kullanarak bir örnek giriş komut dosyası:**
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

### <a name="how-could-i-pass-a-side-input-such-as-a-file-or-files-containing-a-lookup-table-to-all-my-workers"></a>Tüm çalışanlarıma arama tablosu içeren bir dosya veya dosya(lar) gibi bir yan girişi nasıl geçirebilirim?

Yan girişi içeren bir [Dataset nesnesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) oluştur ve çalışma alanınıza kaydedin. Bundan sonra çıkarım komut dosyasında (örneğin, init() yönteminizde) aşağıdaki gibi erişebilirsiniz:

```python
from azureml.core.run import Run
from azureml.core.dataset import Dataset

ws = Run.get_context().experiment.workspace
lookup_ds = Dataset.get_by_name(ws, "<registered-name>")
lookup_ds.download(target_path='.', overwrite=True)
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azureml-contrib-pipeline-step](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps?view=azure-ml-py) paketi ve ParallelRunStep sınıfı için [belgelerle](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallelrunstep?view=azure-ml-py) ilgili yardım için SDK başvurusuna bakın.

* Paralel çalıştırma adımı ile ardışık hatları kullanma konusunda [gelişmiş öğretici](tutorial-pipeline-batch-scoring-classification.md) izleyin.
