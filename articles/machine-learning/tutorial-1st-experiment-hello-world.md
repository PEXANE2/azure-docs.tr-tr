---
title: 'Öğretici: "Merhaba Dünya" Python betiğini Çalıştır'
titleSuffix: Azure Machine Learning
description: Azure ML Başlarken serisinin 2. bölümünde, buluta nasıl önemsiz bir "Hello World" Python betiği gönderileceği gösterilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: 98f4c30d03763e070b1bdc32a5e6e099556916ab
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90946754"
---
# <a name="tutorial-run-hello-world-python-script-part-2-of-4"></a>Öğretici: "Merhaba Dünya" Python betiğini çalıştırın (Bölüm 2/4)

Bu öğreticide, Python "Merhaba Dünya" betiği göndermek ve çalıştırmak için Azure Machine Learning Python SDK 'sını nasıl kullanacağınızı öğreneceksiniz.

Bu öğretici, Azure 'da Azure Machine Learning ve iş tabanlı makine öğrenimi görevlerinin temellerini öğrendiğiniz **dört bölümden oluşan bir öğretici serisinin ikinci bölümüdür** . Bu [öğreticide, 1. Bölüm: yerel makinenizi Azure Machine Learning için ayarlama](
tutorial-1st-experiment-sdk-setup-local.md)işlemi tamamladığınız iş oluşturulur.

Bu öğreticide şunları yapmanız gerekir:

> [!div class="checklist"]
> * Yerel olarak "Merhaba Dünya" Python betiği oluşturma ve çalıştırma
> * "Hello World" öğesini Azure Machine Learning göndermek için bir Python denetim betiği oluşturun
> * Denetim betikindeki Azure Machine Learning kavramlarını anlayın
> * "Merhaba Dünya" Gönder ve Çalıştır
> * Kod çıktılarınızı bulutta görüntüleyin

## <a name="prerequisites"></a>Önkoşullar

- Zaten bir Azure Machine Learning çalışma alanınız yoksa [yerel makine kurulumunda öğreticinin 1. kısmını](tutorial-1st-experiment-sdk-setup-local.md) doldurun.
- Python dili ve makine öğrenimi iş akışlarının giriş bilgisi.
- Yerel geliştirme ortamı. Bu, Visual Studio Code, Jupyıter veya Pydüğme ile sınırlı değildir.
- Python (sürüm 3.5-3.7).

## <a name="create-and-run-a-python-script-locally"></a>Yerel olarak bir Python betiği oluşturma ve çalıştırma

`src` `tutorial` Azure Machine Learning bir işlem kümesi üzerinde çalıştırmak istediğiniz kodu depolamak için dizin altında adlı yeni bir alt dizin oluşturun. `src`Alt dizinde `hello.py` Python betiğini oluşturun:

```python
# src/hello.py
print("Hello world!")
```

Proje dizin yapınız şu şekilde görünür:

```Bash
tutorial
└──.azureml
|  └──config.json
└──src
|  └──hello.py
└──01-create-workspace.py
└──02-create-compute.py
```

### <a name="test-your-script-locally"></a>Betiğinizi yerel olarak test etme

Kodunuzun etkileşimli olarak hata ayıklamanın avantajı, en sevdiğiniz IDE 'yi kullanarak veya bir Terminal aracılığıyla yerel olarak çalıştırabilirsiniz.

```bash
cd <path/to/tutorial>
python ./src/hello.py
```

## <a name="create-a-control-script"></a>Denetim betiği oluşturma

Bir *Denetim betiği* `hello.py` , komut dosyanızı bulutta çalıştırmanızı sağlar.  Denetim betiği, Machine Learning kodunuzun nasıl ve nerede çalıştırılacağını denetlemenize olanak tanır.  

Öğretici dizininizde adlı yeni bir Python dosyası oluşturun `03-run-hello.py` ve aşağıdaki kodu kopyalayın ve bu dosyaya yapıştırın:

```python
# tutorial/03-run-hello.py
from azureml.core import Workspace, Experiment, Environment, ScriptRunConfig

ws = Workspace.from_config()
experiment = Experiment(workspace=ws, name='day1-experiment-hello')

config = ScriptRunConfig(source_directory='./src', script='hello.py', compute_target='cpu-cluster')

run = experiment.submit(config)
aml_url = run.get_portal_url()
print(aml_url)
```

### <a name="understand-the-code"></a>Kodu anlama

Denetim betiğinin nasıl çalıştığına ilişkin bir açıklama aşağıda verilmiştir:

:::row:::
   :::column span="":::
      `ws = Workspace.from_config()`
   :::column-end:::
   :::column span="2":::
      [Çalışma alanı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true) Azure Machine Learning çalışma alanınıza bağlanarak Azure Machine Learning kaynaklarınızla iletişim kurabilirsiniz.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `experiment =  Experiment( ... )`
   :::column-end:::
   :::column span="2":::
      [Deneme](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py&preserve-view=true) , tek bir ad altında birden çok çalıştırma düzenlemek için basit bir yol sağlar. Daha sonra, denemeleri 'ın onlarca çalışma arasında ölçümleri karşılaştırmayı nasıl kolaylaştıradiğini görebilirsiniz.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `config = ScriptRunConfig( ... )` 
   :::column-end:::
   :::column span="2":::
      [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true) kodunuzu sarmalar `hello.py` ve çalışma alanınıza geçirir. Adından da anlaşılacağı gibi, bu sınıfı kullanarak _betiğinizi_ Azure Machine Learning ' de nasıl _çalıştırmak_ istediğinizi _yapılandırabilirsiniz_ . Ayrıca, betiğin hangi işlem hedefine çalışacağını de belirtir.  Bu kodda hedef, [Kurulum öğreticisinde](tutorial-1st-experiment-sdk-setup-local.md)oluşturduğunuz işlem kümesidir.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `run = experiment.submit(config)`
   :::column-end:::
   :::column span="2":::
       Betiğinizi gönderir. Bu gönderim bir [çalıştırma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py&preserve-view=true)olarak adlandırılır.  Bir çalıştırma, kodunuzun tek bir yürütmesini kapsüller. Betik ilerlemesini izlemek için bir çalıştırma kullanın, çıktıyı yakalayın, sonuçları çözümleyin, ölçümleri ve daha fazlasını görselleştirin.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `aml_url = run.get_portal_url()` 
   :::column-end:::
   :::column span="2":::
        `run`Nesnesi, kodunuzun yürütülmesi üzerinde bir tanıtıcı sağlar. Python betiğiyle yazdırılan URL ile Azure Machine Learning Studio ilerleme durumunu izleyin.  
   :::column-end:::
:::row-end:::

## <a name="submit-and-run-your-code-in-the-cloud"></a>Kodunuzu buluta gönderme ve çalıştırma

Denetim betiğinizi çalıştırın, bu, `hello.py` [Kurulum öğreticisinde](tutorial-1st-experiment-sdk-setup-local.md)oluşturduğunuz işlem kümesi üzerinde çalışır.

```bash
python 03-run-hello.py
```

## <a name="monitor-your-code-in-the-cloud-using-studio"></a>Studio 'yu kullanarak kodunuzu bulutta izleyin

Çıktı, şuna benzer bir şekilde Azure Machine Learning Studio 'ya bir bağlantı içerecektir: `https://ml.azure.com/experiments/hello-world/runs/<run-id>?wsid=/subscriptions/<subscription-id>/resourcegroups/<resource-group>/workspaces/<workspace-name>` .

Bağlantıyı izleyin ve **çıktılar + Günlükler** sekmesine gidin. Şöyle bir dosya görebilirsiniz `70_driver_log.txt` :

```txt
 1: [2020-08-04T22:15:44.407305] Entering context manager injector.
 2: [context_manager_injector.py] Command line Options: Namespace(inject=['ProjectPythonPath:context_managers.ProjectPythonPath', 'RunHistory:context_managers.RunHistory', 'TrackUserError:context_managers.TrackUserError', 'UserExceptions:context_managers.UserExceptions'], invocation=['hello.py'])
 3: Starting the daemon thread to refresh tokens in background for process with pid = 31263
 4: Entering Run History Context Manager.
 5: Preparing to call script [ hello.py ] with arguments: []
 6: After variable expansion, calling script [ hello.py ] with arguments: []
 7:
 8: Hello world!
 9: Starting the daemon thread to refresh tokens in background for process with pid = 31263
10:
11:
12: The experiment completed successfully. Finalizing run...
13: Logging experiment finalizing status in history service.
14: [2020-08-04T22:15:46.541334] TimeoutHandler __init__
15: [2020-08-04T22:15:46.541396] TimeoutHandler __enter__
16: Cleaning up all outstanding Run operations, waiting 300.0 seconds
17: 1 items cleaning up...
18: Cleanup took 0.1812913417816162 seconds
19: [2020-08-04T22:15:47.040203] TimeoutHandler __exit__
```

8. satırda "Merhaba Dünya!" öğesini görürsünüz. çıktıların.

`70_driver_log.txt`Dosya, bir çalıştırmanın standart çıktısını içerir. Bu dosya, bulutta uzak çalıştırmaların hata ayıklaması sırasında yararlı olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, basit bir "Hello World" betiği alırsınız ve Azure 'da çalıştırdınız. Azure Machine Learning çalışma alanınıza bağlanmayı, deneme oluşturmayı ve `hello.py` kodunuzu buluta göndermeyi gördünüz.

Sonraki öğreticide, ' den daha ilginç bir şey çalıştırarak bu dersleri üzerinde derleyebilirsiniz `print("Hello world!")` .

> [!div class="nextstepaction"]
> [Öğretici: Modeli eğitme](tutorial-1st-experiment-sdk-train.md)

>[!NOTE] 
> Öğretici serisini burada bitirebilmeniz ve bir sonraki adımda ilerlemeniz istiyorsanız lütfen [kaynaklarınızı temizlemeyi](tutorial-1st-experiment-bring-data.md#clean-up-resources) unutmayın
