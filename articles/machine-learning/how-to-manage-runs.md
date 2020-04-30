---
title: Python 'da eğitim çalıştırmalarını başlatın, izleyin ve iptal edin
titleSuffix: Azure Machine Learning
description: Nasıl başlayacağınızı, denemeleri durumunu ayarlamayı ve makinenizin öğrendiklerinizi düzenlemenizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 01/09/2020
ms.openlocfilehash: cdc739c7464b3deb87faaaabfd8d657ae8c28678
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81617763"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Python 'da eğitim çalıştırmalarını başlatın, izleyin ve iptal edin
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

[Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), [Machine Learning CLI](reference-azure-machine-learning-cli.md)ve [Azure Machine Learning Studio](https://ml.azure.com) , eğitim ve deneme için çalıştırmalarını izlemek, düzenlemek ve yönetmek için çeşitli yöntemler sağlar.

Bu makalede aşağıdaki görevlerin örnekleri gösterilmektedir:

* Çalışma performansını izleme.
* İptal veya başarısız çalıştırmalar.
* Alt çalıştırmalar oluşturun.
* Etiket ve bulma çalıştırmaları.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki öğeler gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md).

* Python için Azure Machine Learning SDK (sürüm 1.0.21 veya üzeri). SDK 'nın en son sürümünü yüklemek veya güncelleştirmek için bkz. [SDK 'Yı yüklemek veya güncelleştirmek](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Azure Machine Learning SDK sürümünüzü denetlemek için aşağıdaki kodu kullanın:

    ```python
    print(azureml.core.VERSION)
    ```

* Azure Machine Learning için [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) ve [CLI uzantısı](reference-azure-machine-learning-cli.md).

## <a name="start-a-run-and-its-logging-process"></a>Çalıştırma ve günlük işlemini başlatma

### <a name="using-the-sdk"></a>SDK’yı kullanarak

[Çalışma alanını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) [, denemeyi,](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) [çalıştırmayı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)ve [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) sınıflarını [azureml. Core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py) paketinden içeri aktararak denemenizin kurulumunu yapın.

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
    
    [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

1. Eğitim betiğinizi içeren klasöre bir çalışma alanı yapılandırması ekleyin. Azure Machine Learning `myworkspace` çalışma alanınız ile değiştirin. Çalışma `myresourcegroup` alanınızı içeren Azure Kaynak grubuyla değiştirin:

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
    > `az ml folder attach` Komut, iki örnek `.azureml` runconfig dosyası içeren bir alt dizin oluşturdu.
    >
    > Programlı olarak çalıştırılan bir yapılandırma nesnesi oluşturan bir Python betiğinizin olması halinde, [runconfig. Save ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) komutunu bir runconfig dosyası olarak kaydetmek için kullanabilirsiniz.
    >
    > Daha fazla örnek runconfig dosyası için bkz [https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml](https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml)..

    Daha fazla bilgi için bkz. [az ml Run gönderme-betiği](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

### <a name="using-azure-machine-learning-studio"></a>Azure Machine Learning Studio 'yu kullanma

Tasarımcı 'da (Önizleme) bir işlem hattı çalıştırması başlatmak için aşağıdaki adımları kullanın:

1. İşlem hatlarınız için varsayılan bir işlem hedefi ayarlayın.

1. İşlem hattı tuvalinin en üstünde **Çalıştır** ' ı seçin.

1. İşlem hattı çalıştırmalarını gruplamak için bir deneme seçin.

## <a name="monitor-the-status-of-a-run"></a>Bir çalıştırmanın durumunu izleme

### <a name="using-the-sdk"></a>SDK’yı kullanarak

[`get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--) Yöntemi ile bir çalıştırmanın durumunu alır.

```python
print(notebook_run.get_status())
```

Çalıştırma KIMLIĞINI, yürütme süresini ve çalıştırma hakkında ek ayrıntıları almak için [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--) yöntemini kullanın.

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


### <a name="using-azure-machine-learning-studio"></a>Azure Machine Learning Studio 'yu kullanma

Studio 'da denemenizin etkin çalıştırmaların sayısını görüntülemek için.

1. **Denemeleri** bölümüne gidin. 

1. Bir deneme seçin.

    Deneme sayfasında, etkin işlem hedeflerinin sayısını ve her çalıştırma için süreyi görebilirsiniz. 

1. Belirli bir çalıştırma numarası seçin.

1. **Günlükler** sekmesinde, işlem hattı çalıştırınızla ilgili tanılama ve hata günlüklerini bulabilirsiniz.


## <a name="cancel-or-fail-runs"></a>İptal veya başarısız çalıştırmalar

Bir hata fark ederseniz veya çalıştırmanın tamamlanmasının çok uzun sürmesi durumunda, çalıştırmayı iptal edebilirsiniz.

### <a name="using-the-sdk"></a>SDK’yı kullanarak

SDK 'Yı kullanarak bir çalıştırmayı iptal etmek için [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) yöntemini kullanın:

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
az ml run cancel -r runid -w workspace_name -e experiment_name
```

Daha fazla bilgi için bkz. [az ml Run Cancel](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

### <a name="using-azure-machine-learning-studio"></a>Azure Machine Learning Studio 'yu kullanma

Aşağıdaki adımları kullanarak Studio 'daki bir çalıştırmayı iptal etmek için:

1. **Denemeleri** veya **ardışık** düzen bölümünde çalışan işlem hattına gidin. 

1. İptal etmek istediğiniz işlem hattı çalıştırma numarasını seçin.

1. Araç çubuğunda **iptal** ' i seçin.


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

Çok sayıda alt çalışma oluşturmak için [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-) yöntemini kullanın. Her oluşturma bir ağ çağrısıyla sonuçlandığından, bir toplu iş oluşturmak, bunlardan birini oluşturmaktan daha etkilidir.

### <a name="submit-child-runs"></a>Alt çalıştırmaları gönder

Alt çalıştırmalar da bir üst çalışmadan gönderilebilir. Bu, üst ve alt çalıştırmaların hiyerarşilerini oluşturmanızı sağlar. 

Çocuğunuzun, üst çalışmadan farklı bir çalıştırma yapılandırması kullanmasını isteyebilirsiniz. Örneğin, çocuklarınız için GPU tabanlı yapılandırmalar kullanırken üst için daha az güçlü ve CPU tabanlı bir yapılandırma kullanabilirsiniz. Diğer bir yaygın, her bir alt öğenin farklı bağımsız değişkenlerini ve verileri geçirmektir. Bir alt çalışmayı özelleştirmek için, bir `RunConfiguration` nesneyi çocuk `ScriptRunConfig` oluşturucusuna geçirin. Bu kod örneği, üst `ScriptRunConfig` nesnenin betiğinin bir parçası olacaktır:

- Adlandırılmış bir `RunConfiguration` işlem kaynağı alma işlemi oluşturur`"gpu-compute"`
- Alt `ScriptRunConfig` nesnelere geçirilecek farklı bağımsız değişken değerlerini yineler
- Özel işlem kaynağını ve bağımsız değişkenini kullanarak yeni bir alt çalışma oluşturur ve gönderir
- Tüm alt çalışma tamamlanana kadar bloklar

```python
# parent.py
# This script controls the launching of child scripts
from azureml.core import Run, ScriptRunConfig, RunConfiguration

run_config_for_aml_compute = RunConfiguration()
run_config_for_aml_compute.target = "gpu-compute"
run_config_for_aml_compute.environment.docker.enabled = True 

run = Run.get_context()

child_args = ['Apple', 'Banana', 'Orange']
for arg in child_args: 
    run.log('Status', f'Launching {arg}')
    child_config = ScriptRunConfig(source_directory=".", script='child.py', arguments=['--fruit', arg], run_config = run_config_for_aml_compute)
    # Starts the run asynchronously
    run.submit_child(child_config)

# Experiment will "complete" successfully at this point. 
# Instead of returning immediately, block until child runs complete

for child in run.get_children():
    child.wait_for_completion()
```

Aynı yapılandırmalarda, bağımsız değişkenlerle ve girişlerle çok sayıda alt çalışma oluşturmak için [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-) yöntemini kullanın. Her oluşturma bir ağ çağrısıyla sonuçlandığından, bir toplu iş oluşturmak, bunlardan birini oluşturmaktan daha etkilidir.

Bir alt çalıştırma içinde, üst çalıştırma KIMLIĞINI görüntüleyebilirsiniz:

```python
## In child run script
child_run = Run.get_context()
child_run.parent.id
```

### <a name="query-child-runs"></a>Alt çalıştırmaları sorgula

Belirli bir üst öğenin alt çalıştırmalarını sorgulamak için [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) yöntemini kullanın. Bağımsız ``recursive = True`` değişkeni, iç içe geçmiş alt öğe ve alt alt ağacı sorgulamanızı sağlar.

```python
print(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Etiket ve bulma çalıştırmaları

Azure Machine Learning ' de, önemli bilgiler için çalıştırmalarınızı düzenlemeye ve sorgulamaya yardımcı olması için özellikleri ve etiketleri kullanabilirsiniz.

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

Basit dize etiketleri de ekleyebilirsiniz. Bu Etiketler etiket sözlüğünde anahtar olarak görüntülendiğinde, bir değeri vardır `None`.

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

Azure CLı, özellikleri ve etiketleri temel alarak çalıştırmaları filtrelemek için kullanılabilen [Jmespath](http://jmespath.org) sorgularını destekler. Azure CLı ile JMESPath sorgusu kullanmak için, `--query` parametresini parametresiyle belirtin. Aşağıdaki örneklerde Özellikler ve Etiketler kullanılarak temel sorgular gösterilmektedir:

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Azure CLı sonuçlarını sorgulama hakkında daha fazla bilgi için bkz. [Azure CLI komut çıkışını sorgulama](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).

### <a name="using-azure-machine-learning-studio"></a>Azure Machine Learning Studio 'yu kullanma

1. İşlem **hatları** bölümüne gidin.

1. Etiketleri, açıklamaları, deneme adlarını ve gönderenin adını kullanarak işlem hatlarını filtrelemek için arama çubuğunu kullanın.

## <a name="example-notebooks"></a>Örnek Not defterleri

Aşağıdaki Not defterleri bu makaledeki kavramları göstermektedir:

* Günlüğe kaydetme API 'Leri hakkında daha fazla bilgi edinmek için bkz. [günlük API Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api/logging-api.ipynb).

* Azure Machine Learning SDK ile çalıştırmaları yönetme hakkında daha fazla bilgi için bkz. [Manage çalıştırmaları Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/manage-runs/manage-runs.ipynb).

## <a name="next-steps"></a>Sonraki adımlar

* Denemeleri için ölçümleri günlüğe kaydetme hakkında bilgi edinmek için bkz. [eğitim çalıştırmaları sırasında günlük ölçümleri](how-to-track-experiments.md).
* Kaynak ve günlüklerin Azure Machine Learning nasıl izleneceğini öğrenmek için bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md).