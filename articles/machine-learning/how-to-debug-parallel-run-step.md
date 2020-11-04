---
title: ParallelRunStep hatalarını ayıklama ve sorunlarını giderme
titleSuffix: Azure Machine Learning
description: Python için Azure Machine Learning SDK 'sında makine öğrenimi komut zincirlerinde ParallelRunStep hatalarını ayıklayın ve sorun giderin. İşlem hatlarında ve uzaktan yürütme sırasında ve betiklerinizde hata ayıklamanıza yardımcı olacak ipuçları hakkında genel bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: troubleshooting
ms.reviewer: jmartens, larryfr, vaidyas, laobri, tracych
ms.author: trmccorm
author: tmccrmck
ms.date: 09/23/2020
ms.openlocfilehash: 93997629b6b30f87769a0154e344215ca52c8ec4
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93308383"
---
# <a name="debug-and-troubleshoot-parallelrunstep"></a>ParallelRunStep hatalarını ayıklama ve sorunlarını giderme


Bu makalede, [Azure MACHINE LEARNING SDK](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py)'Dan [Parallelrunstep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallel_run_step.parallelrunstep?preserve-view=true&view=azure-ml-py) sınıfında hata ayıklamayı ve sorun gidermeyi öğreneceksiniz.

## <a name="testing-scripts-locally"></a>Betikleri yerel olarak test etme

Makine öğrenimi ardışık düzenleri için [betikleri yerel olarak test etme bölümüne](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-machine-learning-pipelines) bakın. Parallelrunadımınız, ML işlem hatlarında bir adım olarak çalışarak aynı yanıtın her ikisi için de geçerlidir.

## <a name="debugging-scripts-from-remote-context"></a>Uzak bağlamdaki betiklerin hatalarını ayıklama

Gerçek bir işlem hattındaki Puanlama betiğine yerel olarak hata ayıklama için bir Puanlama betiğinin hata ayıklamasının yapılması zor olabilir. Portalda günlüklerinizi bulma hakkında daha fazla bilgi için, [uzak bağlamdaki betiklerin Hata ayıklamasında makine öğrenimi ardışık düzenleri bölümüne](how-to-debug-pipelines.md)bakın. Bu bölümdeki bilgiler bir ParallelRunStep için de geçerlidir.

Örneğin, günlük dosyası `70_driver_log.txt` denetleyicinin ParallelRunStep kodunu Başlatan bilgilerini içerir.

ParallelRunStep işlerinin dağıtılmış doğası nedeniyle, birkaç farklı kaynaktaki Günlükler vardır. Ancak, üst düzey bilgi sağlayan iki birleştirilmiş dosya oluşturulur:

- `~/logs/job_progress_overview.txt`: Bu dosya, şimdiye kadar oluşturulan mini toplu işlerin sayısı (görevler olarak da bilinir) ve şimdiye kadar işlenen mini toplu iş sayısı hakkında üst düzey bilgiler sağlar. Bu uçta, işin sonucunu gösterir. İş başarısız olursa, hata iletisini ve sorun giderme işleminin başlatılacağı konumu gösterir.

- `~/logs/sys/master_role.txt`: Bu dosya, çalışan işin asıl düğümünü (Orchestrator olarak da bilinir) sağlar. Görev oluşturma, ilerleme izleme, çalıştırma sonucu içerir.

Giriş betiğinin EntryScript yardımcısını ve Print deyimlerini kullanarak oluşturulan Günlükler aşağıdaki dosyalarda bulunur:

- `~/logs/user/entry_script_log/<ip_address>/<process_name>.log.txt`: Bu dosyalar, EntryScript Yardımcısı kullanılarak entry_script yazılan günlüklerdir.

- `~/logs/user/stdout/<ip_address>/<process_name>.stdout.txt`: Bu dosyalar, entry_script stdout (ör. Print deyimidir) günlüklerinden alınan günlüklerdir.

- `~/logs/user/stderr/<ip_address>/<process_name>.stderr.txt`: Bu dosyalar, entry_script stderr 'ten alınan günlüklerdir.

Betikteki hataların kısa bir şekilde anlaşılmasından bazıları şunlardır:

- `~/logs/user/error.txt`: Bu dosya, betiğinizdeki hataları özetlemeye çalışacaktır.

Betiğinizdeki hatalar hakkında daha fazla bilgi için şu olabilir:

- `~/logs/user/error/`: Giriş betiği yüklenirken ve çalıştırılırken oluşturulan özel durumların tam yığın izlemelerini içerir.

Her bir düğümün puan betiğini nasıl yürütülebileceğini tam olarak anlamak istediğinizde her düğüm için ayrı işlem günlüklerine bakın. İşlem günlükleri, `sys/node` çalışan düğümlerine göre gruplanmış şekilde klasöründe bulunabilir:

- `~/logs/sys/node/<ip_address>/<process_name>.txt`: Bu dosya, bir çalışan tarafından çekildiğinde veya tamamlandığında her mini toplu iş hakkında ayrıntılı bilgi sağlar. Her mini toplu iş için bu dosya şunları içerir:

    - Çalışan işlemin IP adresi ve PID 'SI. 
    - Toplam öğe sayısı, öğe sayısı başarıyla işlendi ve başarısız öğe sayısı.
    - Başlangıç zamanı, süre, işlem süresi ve çalıştırma yöntemi zamanı.

Her çalışan için işlemlerin kaynak kullanımı hakkındaki bilgileri de bulabilirsiniz. Bu bilgiler CSV biçimindedir ve konumunda bulunur `~/logs/sys/perf/<ip_address>/node_resource_usage.csv` . Her işlemle ilgili bilgiler altında bulunabilir `~logs/sys/perf/<ip_address>/processes_resource_usage.csv` .

### <a name="how-do-i-log-from-my-user-script-from-a-remote-context"></a>Kullanıcı betiğimin uzak bağlamdan Nasıl yaparım? mi?
ParallelRunStep, bir düğümde process_count_per_node göre birden çok işlem çalıştırabilir. Düğüm üzerindeki her bir işlemden günlükleri düzenlemek ve Print ve log ifadesini birleştirmek için aşağıda gösterildiği gibi ParallelRunStep günlükçü kullanılması önerilir. EntryScript 'ten bir günlükçüyle alınır ve günlüklerin, portalda **günlüklerde/Kullanıcı** klasöründe görünmesini sağlayabilirsiniz.

**Günlükçü kullanılarak örnek bir giriş betiği:**
```python
from azureml_user.parallel_run import EntryScript

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

Kullanıcı, ParalleRunStep side_inputs parametresi kullanarak başvuru verilerini betiğe geçirebilir. Side_inputs olarak belirtilen tüm veri kümeleri her çalışan düğümüne bağlanır. Kullanıcı, bağımsız değişkeni geçirerek bağlama konumunu alabilir.

Başvuru verilerini içeren bir [veri kümesi](/python/api/azureml-core/azureml.core.dataset.dataset?preserve-view=true&view=azure-ml-py) oluşturun ve çalışma alanınıza kaydedin. Bu `side_inputs` parametreye geçirin `ParallelRunStep` . Ayrıca, `arguments` bağlı yoluna kolayca erişmek için bölümüne yolunu ekleyebilirsiniz:

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

### <a name="how-to-use-input-datasets-with-service-principal-authentication"></a>Giriş veri kümelerini hizmet sorumlusu kimlik doğrulamasıyla kullanma?

Kullanıcı, çalışma alanında kullanılan hizmet sorumlusu kimlik doğrulaması ile giriş veri kümelerini geçirebilir. ParallelRunStep içinde böyle bir veri kümesini kullanmak için veri kümesinin ParallelRunStep yapılandırması oluşturmak için kaydedilmesini gerekir.

```python
service_principal = ServicePrincipalAuthentication(
    tenant_id="**_",
    service_principal_id="_*_",
    service_principal_password="_*_")
 
ws = Workspace(
    subscription_id="_*_",
    resource_group="_*_",
    workspace_name="_*_",
    auth=service_principal
    )
 
default_blob_store = ws.get_default_datastore() # or Datastore(ws, '_*_datastore-name_*_') 
ds = Dataset.File.from_files(default_blob_store, '_*path**_')
registered_ds = ds.register(ws, '_*_dataset-name_*_', create_new_version=True)
```

## <a name="next-steps"></a>Sonraki adımlar

_ Azure Machine Learning işlem [hatlarını gösteren bu Jupyıter not defterlerine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines) bakın

* [Azureml-işlem hattı-adımlar](/python/api/azureml-pipeline-steps/azureml.pipeline.steps?preserve-view=true&view=azure-ml-py) paketiyle ilgili yardım için SDK başvurusuna bakın. ParallelRunStep sınıfı için başvuru [belgelerini](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallelrunstep?preserve-view=true&view=azure-ml-py) görüntüleyin.

* ParallelRunStep ile işlem hatlarını kullanma hakkında [Gelişmiş öğreticiyi](tutorial-pipeline-batch-scoring-classification.md) izleyin. Öğreticide, başka bir dosyanın bir yan giriş olarak nasıl geçirileceğini gösterilmektedir.