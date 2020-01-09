---
title: Hata ayıklama ve ParallelRunStep sorunlarını giderme
titleSuffix: Azure Machine Learning
description: Python için Azure Machine Learning SDK 'da makine öğrenimi ardışık düzenleri hata ayıklayın ve sorun giderin. İşlem hatlarında ve uzaktan yürütme sırasında ve betiklerinizde hata ayıklamanıza yardımcı olacak ipuçları hakkında genel bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye, jmartens, larryfr, vaidyas
ms.author: trmccorm
author: tmccrmck
ms.date: 11/21/2019
ms.openlocfilehash: 20e88da6721a6715a7f31f5c423e305625528383
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75540613"
---
# <a name="debug-and-troubleshoot-using-parallelrun"></a>ParallelRun kullanarak hata ayıklama ve sorun giderme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)'Dan [Parallelrunstep](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallel_run_step.parallelrunstep?view=azure-ml-py) sınıfında hata ayıklamayı ve sorun gidermeyi öğreneceksiniz.

## <a name="testing-scripts-locally"></a>Betikleri yerel olarak test etme

Makine öğrenimi ardışık düzenleri için [betikleri yerel olarak test etme bölümüne](how-to-debug-pipelines.md#testing-scripts-locally) bakın. Parallelrunadımınız, ML işlem hatlarında bir adım olarak çalışarak aynı yanıtın her ikisi için de geçerlidir.

## <a name="debugging-scripts-from-remote-context"></a>Uzak bağlamdaki betiklerin hatalarını ayıklama

Gerçek bir işlem hattındaki Puanlama betiğine yerel olarak hata ayıklama için bir Puanlama betiğinin hata ayıklamasının yapılması zor olabilir. Portalda günlüklerinizi bulma hakkında daha fazla bilgi için, [uzak bağlamdaki betiklerin Hata ayıklamasında makine öğrenimi ardışık düzenleri bölümüne](how-to-debug-pipelines.md#debugging-scripts-from-remote-context)bakın. Bu bölümdeki bilgiler aynı zamanda bir toplu çıkarım çalıştırması için de geçerlidir.

Örneğin, günlük dosyası `70_driver_log.txt` de şunları içerir: 

* Betiğinizin yürütülmesi sırasında tüm yazdırılmış deyimler.
* Betiğin yığın izlemesi.

Toplu Iş çıkarımı işlerinin dağıtılmış doğası nedeniyle, birkaç farklı kaynaktaki Günlükler vardır. Ancak, üst düzey bilgi sağlayan iki birleştirilmiş dosya oluşturulur:

- `~/logs/overview.txt`: Bu dosya, şimdiye kadar oluşturulan mini toplu işlerin sayısı (görevler olarak da bilinir) ve şimdiye kadar işlenen mini toplu iş sayısı hakkında üst düzey bir bilgi sağlar. Bu uçta, işin sonucunu gösterir. İş başarısız olursa, hata iletisini ve sorun giderme işleminin başlatılacağı konumu gösterir.

- `~/logs/master.txt`: Bu dosya, çalışan işin ana düğümü (Orchestrator olarak da bilinir) görünümünü sağlar. Görev oluşturma, ilerleme izleme, çalıştırma sonucu içerir.

Her bir düğümün puan betiğini nasıl yürütülebileceğini tam olarak anlamak istediğinizde her düğüm için ayrı işlem günlüklerine bakın. İşlem günlükleri, çalışan düğümlerine göre gruplanan `worker` klasöründe bulunabilir:

- `~/logs/worker/<ip_address>/Process-*.txt`: Bu dosya, bir çalışan tarafından çekildiği veya tamamlandığı için her bir mini toplu iş hakkında ayrıntılı bilgi sağlar. Her mini toplu iş için bu dosya şunları içerir:

    - Çalışan işlemin IP adresi ve PID 'SI. 
    - Toplam öğe sayısı ve başarıyla işlenen öğe sayısı. 
    - Duvar saati saatlerinde başlangıç ve bitiş zamanı (`start1` ve `end1`). 
    - Harcanan işlemci zamanının başlangıç ve bitiş zamanı (`start2` ve `end2`). 

Her çalışan için işlemlerin kaynak kullanımı hakkındaki bilgileri de bulabilirsiniz. Bu bilgiler CSV biçimindedir ve `~/logs/performance/<ip_address>/`adresinde bulunur. Örneğin, kaynak kullanımı denetlenirken aşağıdaki dosyalara bakın:

- `process_resource_monitor_<ip>_<pid>.csv`: çalışan işlem kaynak kullanımı başına. 
- `sys_resource_monitor_<ip>.csv`: düğüm başına günlük.

### <a name="how-do-i-log-from-my-user-script-from-a-remote-context"></a>Kullanıcı betiğimin uzak bağlamdan Nasıl yaparım? mi?
Günlüklerin **günlüklerde/kullanıcılar** klasöründe görünmesini sağlamak için aşağıdaki adımlarla bir günlükçü oluşturabilirsiniz:
1. Aşağıdaki ilk kod bölümünü File entry_script_helper. Kopyala dosyasına kaydedin ve dosyayı giriş betiğiyle aynı klasöre yerleştirin. Bu sınıf, AmlCompute içindeki yolu alır. Yerel test için `get_working_dir()` yerel bir klasör döndürecek şekilde değiştirebilirsiniz.
2. `init()` yöntemdeki bir günlükçü yapılandırma ve ardından kullanın. Aşağıdaki ikinci kod bölümü bir örnektir. 

**entry_script_helper. Kopyala:**
```python
"""
This module provides helper features for entry script.
This file should be in Python search paths or in the same folder as the entry script.
"""
import os
import socket
import logging
import time
from multiprocessing import current_process
from azureml.core import Run


class EntryScriptHelper:
    """ A helper to provide common features for entry script."""

    LOG_CONFIGED = False

    def get_logger(self, name="EntryScript"):
        """ Return a logger.
            The logger will write to the 'users' folder and show up in azure portal.
        """
        return logging.getLogger(name)

    def config(self, name="EntryScript", level="INFO"):
        """ Config a logger. This should be called in init() in score module.
            Config the logger one time if not configed.
            The logger will write to the 'users' folder and show up in azure portal.
        """
        logger = logging.getLogger(name)

        formatter = logging.Formatter(
            "%(asctime)s|%(name)s|%(levelname)s|%(process)d|%(thread)d|%(funcName)s()|%(message)s"
        )
        formatter.converter = time.gmtime

        logger.setLevel(level)

        handler = logging.FileHandler(self.get_log_file_path())
        handler.setLevel(level)
        handler.setFormatter(formatter)
        logger.addHandler(handler)

        return logger

    def get_log_file_path(self):
        """ Get the log file path for users.
            Each process has its own log file, so there is not race issue among multiple processes.
        """
        ip_address = socket.gethostbyname(socket.gethostname())
        log_dir = os.path.join(self.get_log_dir(), "user", ip_address)
        os.makedirs(log_dir, exist_ok=True)
        return os.path.join(log_dir, f"{current_process().name}.txt")

    def get_log_dir(self):
        """ Return the folder for logs.
            Files and folders in it will be uploaded and show up in run detail page in the azure portal.
        """
        log_dir = os.path.join(self.get_working_dir(), "logs")
        os.makedirs(log_dir, exist_ok=True)
        return log_dir

    def get_working_dir(self):
        """ Return the working directory."""
        return os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), self.get_run().id)

    def get_temp_dir(self):
        """ Return local temp directory."""
        local_temp_dir = os.path.join(
            os.environ.get("AZ_BATCHAI_JOB_TEMP", ""), "azureml-bi", str(os.getpid())
        )
        os.makedirs(local_temp_dir, exist_ok=True)
        return local_temp_dir

    def get_run(self):
        """ Return the Run from the context."""
        return Run.get_context(allow_offline=False)

```

**Günlükçü kullanılarak örnek bir giriş betiği:**
```python
"""
This is a sample scoring module.

This module provides a sample which passes the input back without any change.
"""
import os
import logging
from entry_script_helper import EntryScriptHelper

LOG_NAME = "score_file_list"


def init():
    """ Init """
    EntryScriptHelper().config(LOG_NAME)
    logger = logging.getLogger(LOG_NAME)
    output_folder = os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), "temp/output")
    logger.info(f"{__file__}.output_folder:{output_folder}")
    logger.info("init()")
    os.makedirs(output_folder, exist_ok=True)


def run(mini_batch):
    """ Accept and return the list back."""
    logger = logging.getLogger(LOG_NAME)
    logger.info(f"{__file__}, run({mini_batch})")

    output_folder = os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), "temp/output")
    for file_name in mini_batch:
        with open(file_name, "r") as file:
            lines = file.readlines()
        base_name = os.path.basename(file_name)
        name = os.path.join(output_folder, base_name)
        logger.info(f"{__file__}: {name}")
        with open(name, "w") as file:
            file.write(f"ouput file {name} from {__file__}:\n")
            for line in lines:
                file.write(line)

    return mini_batch
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azureml-contrib-işlem hattı-adım](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps?view=azure-ml-py) paketi ve ParallelRunStep sınıfı [belgeleriyle](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallelrunstep?view=azure-ml-py) ilgili yardım için SDK başvurusuna bakın.

* Toplu Puanlama için işlem hatlarını kullanma hakkında [Gelişmiş öğreticiyi](tutorial-pipeline-batch-scoring-classification.md) izleyin.
