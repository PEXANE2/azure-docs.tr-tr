---
title: Python 'da eğitim çalıştırmalarını başlatın, izleyin ve iptal edin
titleSuffix: Azure Machine Learning service
description: Nasıl başlayacağınızı, denemeleri durumunu ayarlamayı ve makinenizin öğrendiklerinizi düzenlemenizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 07/12/2019
ms.openlocfilehash: 701c266705c16198f35cddc36cdf1d431331c2d2
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68847951"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Python 'da eğitim çalıştırmalarını başlatın, izleyin ve iptal edin

Python ve [MACHINE LEARNING CLI](reference-azure-machine-learning-cli.md) [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) , eğitim ve deneme için çalıştırmalarını izlemek, düzenlemek ve yönetmek için çeşitli yöntemler sağlar.

Bu makalede aşağıdaki görevlerin örnekleri gösterilmektedir:

* Çalışma performansını izleme.
* İptal veya başarısız çalıştırmalar.
* Alt çalıştırmalar oluşturun.
* Etiket ve bulma çalıştırmaları.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki öğeler gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* Bir [Azure Machine Learning hizmet çalışma alanı](how-to-manage-workspace.md).

* Python için Azure Machine Learning SDK (sürüm 1.0.21 veya üzeri). SDK 'nın en son sürümünü yüklemek veya güncelleştirmek için bkz. [SDK 'Yı yüklemek veya güncelleştirmek](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Azure Machine Learning SDK sürümünüzü denetlemek için aşağıdaki kodu kullanın:

    ```python
    print(azureml.core.VERSION)
    ```

* [Azure Machine Learning hizmeti Için](reference-azure-machine-learning-cli.md) [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) ve CLI uzantısı.

## <a name="start-a-run-and-its-logging-process"></a>Çalıştırma ve günlük işlemini başlatma

### <a name="using-the-sdk"></a>SDK’yı kullanarak

[Çalışma alanını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), denemeyi, [çalıştırmayı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)ve [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) sınıflarını [](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) [azureml. Core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py) paketinden içeri aktararak denemenizin kurulumunu yapın.

```python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Bir çalıştırma ve günlük oluşturma işlemini [`start_logging()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-) yöntemiyle başlatın.

```python
notebook_run = exp.start_logging()
notebook_run.log(name="message", value="Hello from run!")
```

### <a name="using-the-cli"></a>CLı 'yi kullanma

Denemenizin çalışmasını başlatmak için aşağıdaki adımları kullanın:

1. Bir kabuk veya komut isteminden Azure aboneliğinizde kimlik doğrulaması yapmak için Azure CLı 'yı kullanın:

    ```azurecli-interactive
    az login
    ```

1. Eğitim betiğinizi içeren klasöre bir çalışma alanı yapılandırması ekleyin. Azure Machine Learning `myworkspace` hizmet çalışma alanınız ile değiştirin. Çalışma `myresourcegroup` alanınızı içeren Azure Kaynak grubuyla değiştirin:

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Bu komut, örnek `.azureml` runconfig ve Conda ortam dosyalarını içeren bir alt dizin oluşturur. Ayrıca, Azure Machine Learning çalışma `config.json` alanınız ile iletişim kurmak için kullanılan bir dosya içerir.

    Daha fazla bilgi için bkz. [az ml Folder Attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

2. Çalıştırmayı başlatmak için aşağıdaki komutu kullanın. Bu komutu kullanırken,-c parametresine karşı runconfig dosyasının adını (dosya sisteminize bakıyorsanız \*. runconfig öğesinden önceki metin) belirtin.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > Komut, iki örnek `.azureml` runconfig dosyası içeren bir alt dizin oluşturdu. `az ml folder attach`
    >
    > Programlı olarak çalıştırılan bir yapılandırma nesnesi oluşturan bir Python betiğinizin olması halinde, runconfig [. Save ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) komutunu bir runconfig dosyası olarak kaydetmek için kullanabilirsiniz.
    >
    > Daha fazla örnek runconfig dosyası için bkz [https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml](https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml).

    Daha fazla bilgi için bkz. [az ml Run gönderme-betiği](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

## <a name="monitor-the-status-of-a-run"></a>Bir çalıştırmanın durumunu izleme

### <a name="using-the-sdk"></a>SDK’yı kullanarak

[`get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--) Yöntemi ile bir çalıştırmanın durumunu alır.

```python
print(notebook_run.get_status())
```

Çalıştırma kimliğini, yürütme süresini ve çalıştırma hakkında ek ayrıntıları almak için [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--) yöntemini kullanın.

```python
print(notebook_run.get_details())
```

Çalıştırma başarıyla tamamlandığında, [`complete()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) yöntemini kullanarak tamamlandı olarak işaretleyin.

```python
notebook_run.complete()
print(notebook_run.get_status())
```

Python 'un `with...as` tasarım modelini kullanıyorsanız, çalıştırma kapsam dışında olduğunda çalıştırma otomatik olarak tamamlandı olarak işaretlenir. Çalıştırmanın tamamlandı olarak el ile işaretlenmesi gerekmez.

```python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print(notebook_run.get_status())

print(notebook_run.get_status())
```

### <a name="using-the-cli"></a>CLı 'yi kullanma

1. Denemenizin çalışma listesini görüntülemek için aşağıdaki komutu kullanın. Denemenizin adıyla değiştirin `experiment` :

    ```azurecli-interactive
    az ml run list --experiment-name experiment
    ```

    Bu komut, bu deneme için çalıştırmalar hakkındaki bilgileri listeleyen bir JSON belgesi döndürür.

    Daha fazla bilgi için bkz. [az ml deneme listesi](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

2. Belirli bir çalıştırma hakkındaki bilgileri görüntülemek için aşağıdaki komutu kullanın. Çalıştırmanın `runid` kimliğiyle değiştirin:

    ```azurecli-interactive
    az ml run show -r runid
    ```

    Bu komut, çalıştırma hakkındaki bilgileri listeleyen bir JSON belgesi döndürür.

    Daha fazla bilgi için bkz. [az ml Run Show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show).

## <a name="cancel-or-fail-runs"></a>İptal veya başarısız çalıştırmalar

Bir hata fark ederseniz veya çalıştırmanın tamamlanmasının çok uzun sürmesi durumunda, çalıştırmayı iptal edebilirsiniz.

### <a name="using-the-sdk"></a>SDK’yı kullanarak

SDK 'yı kullanarak bir çalıştırmayı iptal etmek için [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) yöntemini kullanın:

```python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')
local_script_run = exp.submit(run_config)
print(local_script_run.get_status())

local_script_run.cancel()
print(local_script_run.get_status())
```

Çalıştırmanız bittiğinde, ancak bir hata içeriyorsa (örneğin, yanlış eğitim betiği kullanılmışsa), [`fail()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)#fail-error-details-none--error-code-none---set-status-true-) yöntemi başarısız olarak işaretlemek için kullanabilirsiniz.

```python
local_script_run = exp.submit(run_config)
local_script_run.fail()
print(local_script_run.get_status())
```

### <a name="using-the-cli"></a>CLı 'yi kullanma

CLı kullanarak bir çalıştırmayı iptal etmek için aşağıdaki komutu kullanın. Çalıştırmanın `runid` kimliğiyle değiştirin

```azurecli-interactive
az ml run cancel -r runid
```

Daha fazla bilgi için bkz. [az ml Run Cancel](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

## <a name="create-child-runs"></a>Alt çalıştırmalar oluştur

Farklı hiper parametre ayarlama yinelemeleri gibi ilgili çalıştırmaları gruplamak için alt çalıştırmalar oluşturun.

> [!NOTE]
> Alt çalıştırmalar yalnızca SDK kullanılarak oluşturulabilir.

Bu kod örneği, `hello_with_children.py` [`child_run()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) yöntemi kullanılarak gönderilen çalışma içinden beş alt çalıştırılan bir toplu iş oluşturmak için betiğini kullanır:

```python
!more hello_with_children.py
run_config = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_script_run = exp.submit(run_config)
local_script_run.wait_for_completion(show_output=True)
print(local_script_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> Kapsam dışına ilerlediği için alt çalıştırmalar otomatik olarak tamamlandı olarak işaretlenir.

Ayrıca, alt çalıştırmalarını tek tek başlatabilirsiniz, ancak her oluşturma bir ağ çağrısıyla sonuçlandığından, bir toplu iş gönderilmesi daha az verimlidir.

Belirli bir üst öğenin alt çalıştırmalarını sorgulamak için [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) yöntemini kullanın.

```python
print(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Etiket ve bulma çalıştırmaları

Azure Machine Learning hizmetinde, önemli bilgiler için çalıştırmalarınızı düzenlemeye ve sorgulamaya yardımcı olması için özellikleri ve etiketleri kullanabilirsiniz.

### <a name="add-properties-and-tags"></a>Özellikler ve etiketler ekleme

#### <a name="using-the-sdk"></a>SDK’yı kullanarak

Çalışmalarınızın aranabilir meta verilerini eklemek için [`add_properties()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) yöntemini kullanın. Örneğin, aşağıdaki kod, çalıştırmaya `"author"` özelliği ekler:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Özellikler sabittir, bu nedenle denetim amaçlarıyla kalıcı bir kayıt oluşturur. Aşağıdaki kod örneği, önceki kodda `"azureml-user"` `"author"` Özellik değeri olarak zaten eklendiğimiz için bir hatayla sonuçlanır:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Özelliklerden farklı olarak Etiketler değişebilir. Denemenizin tüketicilere yönelik aranabilir ve anlamlı bilgiler eklemek için [`tag()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) yöntemini kullanın.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Basit dize etiketleri de ekleyebilirsiniz. Bu Etiketler etiket sözlüğünde anahtar olarak görüntülendiğinde, bir değeri `None`vardır.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

#### <a name="using-the-cli"></a>CLı 'yi kullanma

> [!NOTE]
> CLı 'yi kullanarak yalnızca etiketleri ekleyebilir veya güncelleştirebilirsiniz.

Bir etiketi eklemek veya güncelleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml run update -r runid --add-tag quality='fantastic run'
```

Daha fazla bilgi için bkz. [az ml Run Update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update).

### <a name="query-properties-and-tags"></a>Sorgu özellikleri ve Etiketler

Belirli özellikler ve etiketlerle eşleşen çalıştırmaların bir listesini döndürmek için bir deneydeki çalıştırmaları sorgulayabilirsiniz.

#### <a name="using-the-sdk"></a>SDK’yı kullanarak

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

#### <a name="using-the-cli"></a>CLı 'yi kullanma

Azure CLı, özellikleri ve etiketleri temel alarak çalıştırmaları filtrelemek için kullanılabilen [Jmespath](http://jmespath.org) sorgularını destekler. Azure CLI ile jmespath sorgusu kullanmak için, `--query` parametresini parametresiyle belirtin. Aşağıdaki örneklerde Özellikler ve Etiketler kullanılarak temel sorgular gösterilmektedir:

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Azure CLı sonuçlarını sorgulama hakkında daha fazla bilgi için bkz. [Azure CLI komut çıkışını sorgulama](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).

## <a name="example-notebooks"></a>Örnek Not Defterleri

Aşağıdaki Not defterleri bu makaledeki kavramları göstermektedir:

* Günlüğe kaydetme API 'Leri hakkında daha fazla bilgi edinmek için bkz. [günlük API Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Azure Machine Learning SDK ile çalıştırmaları yönetme hakkında daha fazla bilgi için bkz. [Manage çalıştırmaları Not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Sonraki adımlar

* Denemeleri için ölçümleri günlüğe kaydetme hakkında bilgi edinmek için bkz. [eğitim çalıştırmaları sırasında günlük ölçümleri](how-to-track-experiments.md).
