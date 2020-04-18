---
title: Python'da eğitim çalıştırmayı başlatın, izleyin ve iptal edin
titleSuffix: Azure Machine Learning
description: Makine öğrenimi denemelerinizi nasıl başlatacak, durumunu ayarlamayı, etiketlemeyi ve düzenleyeceklerinizi öğrenin.
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
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617763"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Python'da eğitim çalıştırmayı başlatın, izleyin ve iptal edin
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

[Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), Machine Learning [CLI](reference-azure-machine-learning-cli.md)ve [Azure Machine Learning stüdyosu,](https://ml.azure.com) eğitim ve deneme için koşularınızı izlemek, düzenlemek ve yönetmek için çeşitli yöntemler sağlar.

Bu makalede, aşağıdaki görevlerin örnekleri gösterilmektedir:

* Çalıştırma performansını izleyin.
* Çalıştırmaları iptal edin veya başarısız edin.
* Çocuk çalıştırmaları oluşturun.
* Etiketle ve koşa.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki öğelere ihtiyacınız olacak:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure Makine Öğrenimi çalışma alanı.](how-to-manage-workspace.md)

* Python için Azure Machine Learning SDK (sürüm 1.0.21 veya sonrası). SDK'nın en son sürümünü yüklemek veya güncelleştirmek için [SDK'yı yükleyin veya güncelleştirin' e](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)bakın.

    Azure Machine Learning SDK sürümünüzü kontrol etmek için aşağıdaki kodu kullanın:

    ```python
    print(azureml.core.VERSION)
    ```

* Azure Machine Learning için [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) ve [CLI uzantısı.](reference-azure-machine-learning-cli.md)

## <a name="start-a-run-and-its-logging-process"></a>Çalıştırmayı ve günlüğe kaydetme işlemini başlatın

### <a name="using-the-sdk"></a>SDK’yı kullanarak

[Azureml.core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py) paketinden [Çalışma Alanı,](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) [Deneme,](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) [Çalıştır](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)ve [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) sınıflarını içe aktararak denemenizi ayarlayın.

```python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Yöntemle bir çalıştırma ve [`start_logging()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-) günlüğe kaydetme işlemini başlatın.

```python
notebook_run = exp.start_logging()
notebook_run.log(name="message", value="Hello from run!")
```

### <a name="using-the-cli"></a>CLI'yi kullanma

Denemenizin çalışmasını başlatmak için aşağıdaki adımları kullanın:

1. Bir kabuk veya komut isteminden Azure aboneliğinize kimlik doğrulamak için Azure CLI'yi kullanın:

    ```azurecli-interactive
    az login
    ```
    
    [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

1. Çalışma komut dosyanızı içeren klasöre bir çalışma alanı yapılandırması takın. Azure `myworkspace` Makine Öğrenimi çalışma alanınızla değiştirin. Çalışma `myresourcegroup` alanınızı içeren Azure kaynak grubuyla değiştirin:

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Bu komut, `.azureml` örnek runconfig ve conda ortamı dosyaları içeren bir alt dizin oluşturur. Ayrıca, Azure `config.json` Machine Learning çalışma alanınızla iletişim kurmak için kullanılan bir dosya da içerir.

    Daha fazla bilgi için [az ml klasöre iliştirin.](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach)

2. Çalıştırmayı başlatmak için aşağıdaki komutu kullanın. Bu komutu kullanırken, -c parametresi karşı \*runconfig dosyasının adını (dosya sisteminize bakıyorsanız .runconfig'den önceki metin) belirtin.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > Komut, `az ml folder attach` iki `.azureml` örnek runconfig dosyaları içeren bir alt dizin oluşturdu.
    >
    > Programlı bir çalıştır yapılandırma nesnesi oluşturan bir Python komut dosyası varsa, [runconfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) kullanarak runconfig dosyası olarak kaydedebilirsiniz.
    >
    > Daha fazla örnek runconfig [https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml](https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml)dosyaları için bkz.

    Daha fazla bilgi için az [ml run submit-script'e](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script)bakın.

### <a name="using-azure-machine-learning-studio"></a>Azure Machine Learning stüdyosukullanma

Tasarımcıda (önizleme) bir denetim hattı yayımı göndermeye başlamak için aşağıdaki adımları kullanın:

1. Ardışık hattınız için varsayılan bir işlem hedefi ayarlayın.

1. Boru hattı tuvalinin üst kısmında **Çalıştır'ı** seçin.

1. Boru hattı çalışanlarınızı gruplandırmak için bir Deneme seçin.

## <a name="monitor-the-status-of-a-run"></a>Çalıştırmanın durumunu izleme

### <a name="using-the-sdk"></a>SDK’yı kullanarak

[`get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--) Yöntemle bir çalıştırmanın durumunu alın.

```python
print(notebook_run.get_status())
```

Çalışma kimliğini, yürütme süresini ve çalışmayla ilgili ek [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--) ayrıntıları almak için yöntemi kullanın.

```python
print(notebook_run.get_details())
```

Çalışmanız başarıyla tamamlandığında, [`complete()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) tamamlanmış olarak işaretlemek için yöntemi kullanın.

```python
notebook_run.complete()
print(notebook_run.get_status())
```

Python'un `with...as` tasarım deseni kullanırsanız, çalışma kapsam dışında olduğunda çalışma otomatik olarak tamamlanmış olarak işaretlenir. Çalıştırmayı tamamlanmış olarak el ile işaretlemeniz gerekmez.

```python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print(notebook_run.get_status())

print(notebook_run.get_status())
```

### <a name="using-the-cli"></a>CLI'yi kullanma

1. Denemeniz için çalışan listesini görüntülemek için aşağıdaki komutu kullanın. Denemenizin adı ile değiştirin: `experiment`

    ```azurecli-interactive
    az ml run list --experiment-name experiment
    ```

    Bu komut, bu denemenin çalıştırmaları hakkındaki bilgileri listeleyen bir JSON belgesini döndürür.

    Daha fazla bilgi için [az ml deney listesine](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list)bakın.

2. Belirli bir çalışmadaki bilgileri görüntülemek için aşağıdaki komutu kullanın. Çalıştırmanın kimliğiyle değiştirin: `runid`

    ```azurecli-interactive
    az ml run show -r runid
    ```

    Bu komut, çalışma yla ilgili bilgileri listeleyen bir JSON belgesini döndürür.

    Daha fazla bilgi için [az ml run show'a](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show)bakın.


### <a name="using-azure-machine-learning-studio"></a>Azure Machine Learning stüdyosukullanma

Stüdyodaki denemeniz için etkin çalışan sayısını görüntülemek için.

1. **Denemeler** bölümüne gidin... 

1. Bir deneme seçin.

    Deneme sayfasında, etkin işlem hedeflerinin sayısını ve her çalıştırmanın süresini görebilirsiniz. 

1. Belirli bir çalıştırma numarası seçin.

1. **Günlükler** sekmesinde, ardışık hat lar çalıştırın için tanılama ve hata günlüklerini bulabilirsiniz.


## <a name="cancel-or-fail-runs"></a>Çalıştırmaları iptal etme veya başarısız

Bir hata fark ederseniz veya çalışmanızın tamamlanması çok uzun sürüyorsa, çalıştırmayı iptal edebilirsiniz.

### <a name="using-the-sdk"></a>SDK’yı kullanarak

SDK'yı kullanarak bir çalıştırmayı [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) iptal etmek için aşağıdaki yöntemi kullanın:

```python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')
local_script_run = exp.submit(run_config)
print(local_script_run.get_status())

local_script_run.cancel()
print(local_script_run.get_status())
```

Çalıştırmanız bitiyorsa, ancak bir hata içeriyorsa (örneğin, yanlış eğitim [`fail()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)#fail-error-details-none--error-code-none---set-status-true-) komut dosyası kullanıldı), başarısız olarak işaretlemek için yöntemi kullanabilirsiniz.

```python
local_script_run = exp.submit(run_config)
local_script_run.fail()
print(local_script_run.get_status())
```

### <a name="using-the-cli"></a>CLI'yi kullanma

CLI kullanarak bir çalıştırmayı iptal etmek için aşağıdaki komutu kullanın. Çalıştırmanın kimliğiyle değiştirin `runid`

```azurecli-interactive
az ml run cancel -r runid -w workspace_name -e experiment_name
```

Daha fazla bilgi için az [ml çalıştır ın iptaline](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel)bakın.

### <a name="using-azure-machine-learning-studio"></a>Azure Machine Learning stüdyosukullanma

Aşağıdaki adımları kullanarak stüdyodaki bir çalıştırmayı iptal etmek için:

1. **Denemeler** veya **Boru Hatları** bölümünde çalışan ardışık bölüme gidin. 

1. İptal etmek istediğiniz ardışık hatlar hattı çalıştırma numarasını seçin.

1. Araç çubuğunda **İptal** et'i seçin


## <a name="create-child-runs"></a>Alt çalıştırmalar oluşturma

Farklı hiperparametre ayarı yinelemeleri gibi ilgili çalıştırmaları gruplandırmak için alt çalıştırmalar oluşturun.

> [!NOTE]
> Alt çalıştırmalar yalnızca SDK kullanılarak oluşturulabilir.

Bu kod örneği, yöntem kullanılarak gönderilen bir çalıştırın içinden beş alt çalışandan oluşan bir toplu iş oluşturmak için `hello_with_children.py` komut dosyasını [`child_run()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) kullanır:

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
> Kapsam dışına çıktıkça, alt çalıştırmalar otomatik olarak tamamlanmış olarak işaretlenir.

Birçok alt çalışan verimli oluşturmak [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-) için yöntemi kullanın. Her oluşturma bir ağ çağrısıyla sonuçlandığı için, bir dizi çalıştırma oluşturmak, bunları tek tek oluşturmaktan daha verimlidir.

### <a name="submit-child-runs"></a>Çocuk çalıştırmaları gönderme

Alt çalıştırmalar bir üst çalıştırmadan da gönderilebilir. Bu, üst ve alt çalışır hiyerarşileri oluşturmanıza olanak sağlar. 

Çocuğunuzun çalışır ana çalıştırmadan farklı bir çalıştırma yapılandırması kullanmasını isteyebilirsiniz. Örneğin, çocuklarınız için GPU tabanlı yapılandırmaları kullanırken üst öğe için daha az güçlü, CPU tabanlı bir yapılandırma kullanabilirsiniz. Başka bir ortak arzu her çocuk farklı argümanlar ve veri geçmektir. Bir alt çalıştırmayı özelleştirmek `RunConfiguration` için, bir `ScriptRunConfig` nesneyi çocuğun oluşturucusuna geçirin. Üst `ScriptRunConfig` nesnenin komut dosyasının bir parçası olacak bu kod örneği:

- Adlandırılmış `RunConfiguration` bir bilgi işlem kaynağı alma oluşturur`"gpu-compute"`
- Çocuklara `ScriptRunConfig` aktarılacak farklı bağımsız değişken değerlerini yineler nesneler
- Özel bilgi işlem kaynağını ve bağımsız değişkenini kullanarak yeni bir alt çalışma oluşturur ve gönderir
- Tüm çocuk tamamlanana kadar engeller

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

Aynı yapılandırmaları, bağımsız değişkenleri ve girişleri verimli bir şekilde [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-) birçok alt çalışır oluşturmak için yöntemi kullanın. Her oluşturma bir ağ çağrısıyla sonuçlandığı için, bir dizi çalıştırma oluşturmak, bunları tek tek oluşturmaktan daha verimlidir.

Bir alt çalışma içinde, üst çalışan kimliği görüntüleyebilirsiniz:

```python
## In child run script
child_run = Run.get_context()
child_run.parent.id
```

### <a name="query-child-runs"></a>Alt çalıştırmaları sorgula

Belirli bir ebeveynin alt çalışanlarını [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) sorgulamak için yöntemi kullanın. Bağımsız ``recursive = True`` değişken, iç içe olan bir çocuk ve torun ağacını sorgulamanızı sağlar.

```python
print(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Etiketler ve çalışır bul

Azure Machine Learning'de, önemli bilgiler için çalışanlarınızı düzenlemeye ve sorgulamaya yardımcı olmak için özellikleri ve etiketleri kullanabilirsiniz.

### <a name="add-properties-and-tags"></a>Özellikler ve etiketler ekleme

#### <a name="using-the-sdk"></a>SDK’yı kullanarak

Çalıştırmalarınıza aranabilir meta veriler eklemek [`add_properties()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) için yöntemi kullanın. Örneğin, aşağıdaki kod çalışma `"author"` özelliğiekler:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Özellikler değişmezolduğundan, denetim amaçları için kalıcı bir kayıt oluştururlar. Yukarıdaki kodda `"azureml-user"` `"author"` özellik değeri olarak zaten eklediğimiz için aşağıdaki kod örneği bir hataya neden olabilir:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Özellikleriaksine, etiketleri mutable vardır. Denemenizin tüketicileri için aranabilir ve anlamlı [`tag()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) bilgiler eklemek için yöntemi kullanın.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Ayrıca basit dize etiketleri ekleyebilirsiniz. Bu etiketler etiket sözlüğünde anahtar olarak göründüğünde, `None`'nin bir değeri vardır.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

#### <a name="using-the-cli"></a>CLI'yi kullanma

> [!NOTE]
> CLI'yi kullanarak yalnızca etiket ekleyebilir veya güncelleyebilirsiniz.

Etiket eklemek veya güncelleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml run update -r runid --add-tag quality='fantastic run'
```

Daha fazla bilgi için [az ml run güncellemesine](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update)bakın.

### <a name="query-properties-and-tags"></a>Sorgu özellikleri ve etiketleri

Belirli özellikler ve etiketlerle eşleşen bir çalıştırma listesini döndürmek için deneme içinde çalışır sorgulayabilirsiniz.

#### <a name="using-the-sdk"></a>SDK’yı kullanarak

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

#### <a name="using-the-cli"></a>CLI'yi kullanma

Azure CLI, özellikleri ve etiketleri temel alan çalıştırmaları filtrelemek için kullanılabilen [JMESPath](http://jmespath.org) sorgularını destekler. Azure CLI ile bir JMESPath sorgusu kullanmak `--query` için, parametre ile belirtin. Aşağıdaki örnekler, özellikleri ve etiketleri kullanarak temel sorguları gösterir:

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Azure CLI sonuçlarını sorgulama hakkında daha fazla bilgi için Bkz. [Azure CLI sorgula komut çıktısı.](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest)

### <a name="using-azure-machine-learning-studio"></a>Azure Machine Learning stüdyosukullanma

1. **Boru Hatları** bölümüne gidin.

1. Etiketleri, açıklamaları, deneme adlarını ve gönderen inadını kullanarak ardışık hatları filtrelemek için arama çubuğunu kullanın.

## <a name="example-notebooks"></a>Örnek defterler

Aşağıdaki not defterleri bu makaledeki kavramları gösterir:

* Günlük API'leri hakkında daha fazla bilgi edinmek için [günlük API not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api/logging-api.ipynb)bakın.

* Azure Machine Learning SDK ile çalıştırmaları yönetme hakkında daha fazla bilgi için [yönetici çalışır not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/manage-runs/manage-runs.ipynb)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Denemelerinizin ölçümlerini nasıl günlüğe kaydedebilirsiniz öğrenmek [için, eğitim çalıştırmaları sırasında Günlük ölçümlerine](how-to-track-experiments.md)bakın.
* Azure Machine Learning'deki kaynakları ve günlükleri nasıl izleyeceğinizi öğrenmek için [bkz.](monitor-azure-machine-learning.md)