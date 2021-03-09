---
title: Python 'da eğitim çalıştırmalarını başlatın, izleyin ve iptal edin
titleSuffix: Azure Machine Learning
description: Azure Machine Learning Python SDK ile Machine Learning deneme çalışmalarınızı başlatma, durum ve yönetme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 03/04/2021
ms.topic: conceptual
ms.custom: how-to, devx-track-python, devx-track-azurecli
ms.openlocfilehash: d142c523862d61bf56723726be50cd6f095c5ee9
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102520345"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Python 'da eğitim çalıştırmalarını başlatın, izleyin ve iptal edin

[Python için Azure MACHINE LEARNING SDK](/python/api/overview/azure/ml/intro), [Machine Learning CLI](reference-azure-machine-learning-cli.md)ve [Azure Machine Learning Studio](https://ml.azure.com) , eğitim ve deneme için çalıştırmalarını izlemek, düzenlemek ve yönetmek için çeşitli yöntemler sağlar.

Bu makalede aşağıdaki görevlerin örnekleri gösterilmektedir:

* Çalışma performansını izleme.
* İptal veya başarısız çalıştırmalar.
* Alt çalıştırmalar oluşturun.
* Etiket ve bulma çalıştırmaları.

> [!TIP]
> Azure Machine Learning hizmeti ve ilişkili Azure hizmetlerini izleme hakkında bilgi arıyorsanız bkz. [nasıl yapılır Azure Machine Learning](monitor-azure-machine-learning.md).
> Web Hizmetleri veya IoT Edge modülleri olarak dağıtılan izleme modelleri hakkında bilgi arıyorsanız, bkz. [model verilerini toplama](how-to-enable-data-collection.md) ve [Application Insights ile izleme](how-to-enable-app-insights.md).

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki öğeler gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md).

* Python için Azure Machine Learning SDK (sürüm 1.0.21 veya üzeri). SDK 'nın en son sürümünü yüklemek veya güncelleştirmek için bkz. [SDK 'Yı yüklemek veya güncelleştirmek](/python/api/overview/azure/ml/install).

    Azure Machine Learning SDK sürümünüzü denetlemek için aşağıdaki kodu kullanın:

    ```python
    print(azureml.core.VERSION)
    ```

* Azure Machine Learning için [Azure CLI](/cli/azure/) ve [CLI uzantısı](reference-azure-machine-learning-cli.md).

## <a name="monitor-run-performance"></a>Çalışma performansını izleme

* Çalıştırma ve günlük işlemini başlatma

    # <a name="python"></a>[Python](#tab/python)
    
    1. [Çalışma alanını](/python/api/azureml-core/azureml.core.workspace.workspace) [, denemeyi,](/python/api/azureml-core/azureml.core.experiment.experiment) [çalıştırmayı](/python/api/azureml-core/azureml.core.run%28class%29)ve [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig) sınıflarını [azureml. Core](/python/api/azureml-core/azureml.core) paketinden içeri aktararak denemenizin kurulumunu yapın.
    
        ```python
        import azureml.core
        from azureml.core import Workspace, Experiment, Run
        from azureml.core import ScriptRunConfig
        
        ws = Workspace.from_config()
        exp = Experiment(workspace=ws, name="explore-runs")
        ```
    
    1. Bir çalıştırma ve günlük oluşturma işlemini [`start_logging()`](/python/api/azureml-core/azureml.core.experiment%28class%29#start-logging--args----kwargs-) yöntemiyle başlatın.
    
        ```python
        notebook_run = exp.start_logging()
        notebook_run.log(name="message", value="Hello from run!")
        ```
        
    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
    
    Denemenizin çalışmasını başlatmak için aşağıdaki adımları kullanın:
    
    1. Bir kabuk veya komut isteminden Azure aboneliğinizde kimlik doğrulaması yapmak için Azure CLı 'yı kullanın:
    
        ```azurecli-interactive
        az login
        ```
        
        [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 
    
    1. Eğitim betiğinizi içeren klasöre bir çalışma alanı yapılandırması ekleyin. `myworkspace`Azure Machine Learning çalışma alanınız ile değiştirin. `myresourcegroup`Çalışma alanınızı Içeren Azure Kaynak grubuyla değiştirin:
    
        ```azurecli-interactive
        az ml folder attach -w myworkspace -g myresourcegroup
        ```
    
        Bu komut `.azureml` , örnek runconfig ve Conda ortam dosyalarını içeren bir alt dizin oluşturur. Ayrıca `config.json` , Azure Machine Learning çalışma alanınız ile iletişim kurmak için kullanılan bir dosya içerir.
    
        Daha fazla bilgi için bkz. [az ml Folder Attach](/cli/azure/ext/azure-cli-ml/ml/folder#ext-azure-cli-ml-az-ml-folder-attach).
    
    2. Çalıştırmayı başlatmak için aşağıdaki komutu kullanın. Bu komutu kullanırken,-c parametresine karşı runconfig dosyasının adını ( \* dosya sisteminize bakıyorsanız. runconfig öğesinden önceki metin) belirtin.
    
        ```azurecli-interactive
        az ml run submit-script -c sklearn -e testexperiment train.py
        ```
    
        > [!TIP]
        > `az ml folder attach`Komut `.azureml` , iki örnek runconfig dosyası içeren bir alt dizin oluşturdu.
        >
        > Programlı olarak çalıştırılan bir yapılandırma nesnesi oluşturan bir Python betiğinizin olması halinde, [runconfig. Save ()](/python/api/azureml-core/azureml.core.runconfiguration#save-path-none--name-none--separate-environment-yaml-false-) komutunu bir runconfig dosyası olarak kaydetmek için kullanabilirsiniz.
        >
        > Daha fazla örnek runconfig dosyası için bkz [https://github.com/MicrosoftDocs/pipelines-azureml/](https://github.com/MicrosoftDocs/pipelines-azureml/) ..
    
        Daha fazla bilgi için bkz. [az ml Run gönderme-betiği](/cli/azure/ext/azure-cli-ml/ml/run#ext-azure-cli-ml-az-ml-run-submit-script).

    # <a name="studio"></a>[Studio](#tab/azure-studio)

    Azure Machine Learning tasarımcısında bir modeli eğitmek için bir örnek için bkz. [öğretici: tasarımcı ile otomobil fiyatlarını tahmin](tutorial-designer-automobile-price-train-score.md)etme.

    ---

* Bir çalıştırmanın durumunu izleme

    # <a name="python"></a>[Python](#tab/python)
    
    * Yöntemi ile bir çalıştırmanın durumunu alır [`get_status()`](/python/api/azureml-core/azureml.core.run%28class%29#get-status--) .
    
        ```python
        print(notebook_run.get_status())
        ```
    
    * Çalıştırma KIMLIĞINI, yürütme süresini ve çalıştırma hakkında ek ayrıntıları almak için [`get_details()`](/python/api/azureml-core/azureml.core.workspace.workspace#get-details--) yöntemini kullanın.
    
        ```python
        print(notebook_run.get_details())
        ```
    
    * Çalıştırma başarıyla tamamlandığında, [`complete()`](/python/api/azureml-core/azureml.core.run%28class%29#complete--set-status-true-) yöntemini kullanarak tamamlandı olarak işaretleyin.
    
        ```python
        notebook_run.complete()
        print(notebook_run.get_status())
        ```
    
    * Python 'un `with...as` tasarım modelini kullanıyorsanız, çalıştırma kapsam dışında olduğunda çalıştırma otomatik olarak tamamlandı olarak işaretlenir. Çalıştırmanın tamamlandı olarak el ile işaretlenmesi gerekmez.
        
        ```python
        with exp.start_logging() as notebook_run:
            notebook_run.log(name="message", value="Hello from run!")
            print(notebook_run.get_status())
        
        print(notebook_run.get_status())
        ```
    
    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
    
    * Denemenizin çalışma listesini görüntülemek için aşağıdaki komutu kullanın. `experiment`Denemenizin adıyla değiştirin:
    
        ```azurecli-interactive
        az ml run list --experiment-name experiment
        ```
    
        Bu komut, bu deneme için çalıştırmalar hakkındaki bilgileri listeleyen bir JSON belgesi döndürür.
    
        Daha fazla bilgi için bkz. [az ml deneme listesi](/cli/azure/ext/azure-cli-ml/ml/experiment#ext-azure-cli-ml-az-ml-experiment-list).
    
    * Belirli bir çalıştırma hakkındaki bilgileri görüntülemek için aşağıdaki komutu kullanın. `runid`ÇALıŞTıRMANıN kimliğiyle değiştirin:
    
        ```azurecli-interactive
        az ml run show -r runid
        ```
    
        Bu komut, çalıştırma hakkındaki bilgileri listeleyen bir JSON belgesi döndürür.
    
        Daha fazla bilgi için bkz. [az ml Run Show](/cli/azure/ext/azure-cli-ml/ml/run#ext-azure-cli-ml-az-ml-run-show).
    
    
    # <a name="studio"></a>[Studio](#tab/azure-studio)
    
    Çalışma Studio 'da çalıştırmalarını görüntülemek için: 
    
    1. **Denemeleri** sekmesine gidin.
    
    1. **Tüm denemeleri** seçin ya da çalışma alanında gönderilen tüm çalıştırmaları görüntülemek için tüm çalıştırmalar ' **ı seçin.**
    
        **Tüm çalıştırmalar '** sayfasında, çalışma listenizi etiketlere, denemeleri, işlem hedefine ve daha fazlasını düzenleyerek işlerinizi daha iyi düzenleme ve kapsama göre filtreleyebilirsiniz.  
    
    1. Karşılaştırmak, grafikler eklemek veya filtre uygulamak için çalıştırmalar ' ı seçerek sayfada özelleştirmeler yapın. Bu değişiklikler, kolayca çalışmanıza geri dönebilmeniz için **özel bir görünüm** olarak kaydedilebilir. Çalışma alanı izinlerine sahip kullanıcılar özel görünümü düzenleyebilir veya görüntüleyebilir. Ayrıca, **görünümü paylaşma**' yı seçerek özel görünümü gelişmiş işbirliği için ekip üyeleriyle paylaşabilirsiniz.   
    
        :::image type="content" source="media/how-to-manage-runs/custom-views.gif" alt-text="Ekran görüntüsü: özel görünüm oluşturma":::
    
    1. Çalıştırma günlüklerini görüntülemek için belirli bir çalıştırma seçin ve **çıktılar + Günlükler** sekmesinde, çalıştıralım için tanılama ve hata günlüklerini bulabilirsiniz.
    
    ---

## <a name="run-description"></a>Çalıştırma açıklaması 

Çalıştırmaya daha fazla bağlam ve bilgi sağlamak için çalıştırma açıklaması bir çalıştırmaya eklenebilir. Ayrıca, çalıştırmalar listesinde bu açıklamaları arayabilir ve çalıştırma açıklamasını çalıştırmalar listesinde bir sütun olarak ekleyebilirsiniz. 

Çalıştırmanıza ilişkin **ayrıntıları Çalıştır** sayfasına gidin ve çalıştırmanıza yönelik açıklamaları eklemek, düzenlemek veya silmek için Düzenle veya kurşun kalem simgesini seçin. Çalışma listesindeki değişiklikleri kalıcı hale getirmek için, değişiklikleri var olan özel görünüminizdeki veya yeni bir özel görünümdeki kaydedin. Aşağıda gösterildiği gibi görüntülerin gömülmesini ve derin bağlamayı sağlayan çalışma açıklamaları için markas biçimi desteklenir.

:::image type="content" source="media/how-to-manage-runs/run-description.gif" alt-text="Ekran görüntüsü: çalıştırma açıklaması oluşturma"::: 

## <a name="tag-and-find-runs"></a>Etiket ve bulma çalıştırmaları

Azure Machine Learning ' de, önemli bilgiler için çalıştırmalarınızı düzenlemeye ve sorgulamaya yardımcı olması için özellikleri ve etiketleri kullanabilirsiniz.

* Özellikler ve etiketler ekleme

    # <a name="python"></a>[Python](#tab/python)
    
    Çalışmalarınızın aranabilir meta verilerini eklemek için [`add_properties()`](/python/api/azureml-core/azureml.core.run%28class%29#add-properties-properties-) yöntemini kullanın. Örneğin, aşağıdaki kod, `"author"` çalıştırmaya özelliği ekler:
    
    ```Python
    local_run.add_properties({"author":"azureml-user"})
    print(local_run.get_properties())
    ```
    
    Özellikler sabittir, bu nedenle denetim amaçlarıyla kalıcı bir kayıt oluşturur. Aşağıdaki kod örneği, `"azureml-user"` `"author"` Önceki kodda Özellik değeri olarak zaten eklendiğimiz için bir hatayla sonuçlanır:
    
    ```Python
    try:
        local_run.add_properties({"author":"different-user"})
    except Exception as e:
        print(e)
    ```
    
    Özelliklerden farklı olarak Etiketler değişebilir. Denemenizin tüketicilere yönelik aranabilir ve anlamlı bilgiler eklemek için [`tag()`](/python/api/azureml-core/azureml.core.run%28class%29#tag-key--value-none-) yöntemini kullanın.
    
    ```Python
    local_run.tag("quality", "great run")
    print(local_run.get_tags())
    
    local_run.tag("quality", "fantastic run")
    print(local_run.get_tags())
    ```
    
    Basit dize etiketleri de ekleyebilirsiniz. Bu Etiketler etiket sözlüğünde anahtar olarak görüntülendiğinde, bir değeri vardır `None` .
    
    ```Python
    local_run.tag("worth another look")
    print(local_run.get_tags())
    ```
    
    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
    
    > [!NOTE]
    > CLı 'yi kullanarak yalnızca etiketleri ekleyebilir veya güncelleştirebilirsiniz.
    
    Bir etiketi eklemek veya güncelleştirmek için aşağıdaki komutu kullanın:
    
    ```azurecli-interactive
    az ml run update -r runid --add-tag quality='fantastic run'
    ```
    
    Daha fazla bilgi için bkz. [az ml Run Update](/cli/azure/ext/azure-cli-ml/ml/run#ext-azure-cli-ml-az-ml-run-update).
    
    # <a name="studio"></a>[Studio](#tab/azure-studio)
    
    Çalışma etiketlerini Studio 'dan ekleyebilir, düzenleyebilir veya silebilirsiniz. Çalışmanıza ilişkin **ayrıntıları Çalıştır** sayfasına gidin ve çalışmalarınız için Etiketler eklemek, düzenlemek veya silmek için Düzenle veya kurşun kalem simgesini seçin. Ayrıca, çalışma listesi sayfasından bu etiketlerin üzerinde arama ve filtreleme yapabilirsiniz.
    
    :::image type="content" source="media/how-to-manage-runs/run-tags.gif" alt-text="Ekran görüntüsü: çalıştırma etiketlerini ekleme, düzenleme veya silme":::
    
    ---

* Sorgu özellikleri ve Etiketler

    Belirli özellikler ve etiketlerle eşleşen çalıştırmaların bir listesini döndürmek için bir deneydeki çalıştırmaları sorgulayabilirsiniz.

    # <a name="python"></a>[Python](#tab/python)
    
    ```Python
    list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
    list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
    ```
    
    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
    
    Azure CLı, özellikleri ve etiketleri temel alarak çalıştırmaları filtrelemek için kullanılabilen [Jmespath](http://jmespath.org) sorgularını destekler. Azure CLı ile JMESPath sorgusu kullanmak için, `--query` parametresini parametresiyle belirtin. Aşağıdaki örneklerde Özellikler ve Etiketler kullanılarak bazı sorgular gösterilmektedir:
    
    ```azurecli-interactive
    # list runs where the author property = 'azureml-user'
    az ml run list --experiment-name experiment [?properties.author=='azureml-user']
    # list runs where the tag contains a key that starts with 'worth another look'
    az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
    # list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
    az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
    ```
    
    Azure CLı sonuçlarını sorgulama hakkında daha fazla bilgi için bkz. [Azure CLI komut çıkışını sorgulama](/cli/azure/query-azure-cli).
    
    # <a name="studio"></a>[Studio](#tab/azure-studio)
    
    1. **Tüm çalıştırmalar** listesine gidin.
    
    1. Etiketler, açıklamalar, deneme adları ve gönderenin adı gibi çalıştırma meta verilerini filtrelemek için arama çubuğunu kullanın. Etiketler filtresi, etiketleri filtrelemek için de kullanılabilir. 
    
    ---


## <a name="cancel-or-fail-runs"></a>İptal veya başarısız çalıştırmalar

Bir hata fark ederseniz veya çalıştırmanın tamamlanmasının çok uzun sürmesi durumunda, çalıştırmayı iptal edebilirsiniz.

# <a name="python"></a>[Python](#tab/python)

SDK 'Yı kullanarak bir çalıştırmayı iptal etmek için yöntemini kullanın [`cancel()`](/python/api/azureml-core/azureml.core.run%28class%29#cancel--) :

```python
src = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')
local_run = exp.submit(src)
print(local_run.get_status())

local_run.cancel()
print(local_run.get_status())
```

Çalıştırmanız bittiğinde, ancak bir hata içeriyorsa (örneğin, yanlış eğitim betiği kullanılmışsa), [`fail()`](/python/api/azureml-core/azureml.core.run%28class%29#fail-error-details-none--error-code-none---set-status-true-) yöntemi başarısız olarak işaretlemek için kullanabilirsiniz.

```python
local_run = exp.submit(src)
local_run.fail()
print(local_run.get_status())
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

CLı kullanarak bir çalıştırmayı iptal etmek için aşağıdaki komutu kullanın. `runid`ÇALıŞTıRMANıN kimliğiyle değiştirin

```azurecli-interactive
az ml run cancel -r runid -w workspace_name -e experiment_name
```

Daha fazla bilgi için bkz. [az ml Run Cancel](/cli/azure/ext/azure-cli-ml/ml/run#ext-azure-cli-ml-az-ml-run-cancel).

# <a name="studio"></a>[Studio](#tab/azure-studio)

Aşağıdaki adımları kullanarak Studio 'daki bir çalıştırmayı iptal etmek için:

1. **Denemeleri** veya **ardışık** düzen bölümünde çalışan işlem hattına gidin. 

1. İptal etmek istediğiniz işlem hattı çalıştırma numarasını seçin.

1. Araç çubuğunda **iptal** ' i seçin.

---

## <a name="create-child-runs"></a>Alt çalıştırmalar oluştur

Farklı hiper parametre ayarlama yinelemeleri gibi ilgili çalıştırmaları gruplamak için alt çalıştırmalar oluşturun.

> [!NOTE]
> Alt çalıştırmalar yalnızca SDK kullanılarak oluşturulabilir.

Bu kod örneği, `hello_with_children.py` yöntemi kullanılarak gönderilen çalışma içinden beş alt çalıştırılan bir toplu iş oluşturmak için betiğini kullanır [`child_run()`](/python/api/azureml-core/azureml.core.run%28class%29#child-run-name-none--run-id-none--outputs-none-) :

```python
!more hello_with_children.py
src = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_run = exp.submit(src)
local_run.wait_for_completion(show_output=True)
print(local_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> Kapsam dışına ilerlediği için alt çalıştırmalar otomatik olarak tamamlandı olarak işaretlenir.

Çok sayıda alt çalışma oluşturmak için [`create_children()`](/python/api/azureml-core/azureml.core.run.run#create-children-count-none--tag-key-none--tag-values-none-) yöntemini kullanın. Her oluşturma bir ağ çağrısıyla sonuçlandığından, bir toplu iş oluşturmak, bunlardan birini oluşturmaktan daha etkilidir.

### <a name="submit-child-runs"></a>Alt çalıştırmaları gönder

Alt çalıştırmalar da bir üst çalışmadan gönderilebilir. Bu, üst ve alt çalıştırmaların hiyerarşilerini oluşturmanızı sağlar. Parentdaha az alt çalışma oluşturamazsınız: üst çalıştırma Nothing olsa da alt çalıştırmaları çalıştırsa bile, hiyerarşinin oluşturulması hala gereklidir. Tüm çalıştırmaların durumu bağımsızdır: bir `"Completed"` veya daha fazla alt çalışma iptal edildiyse veya başarısız olsa bile üst öğe başarılı durumunda olabilir.  

Çocuğunuzun, üst çalışmadan farklı bir çalıştırma yapılandırması kullanmasını isteyebilirsiniz. Örneğin, çocuklarınız için GPU tabanlı yapılandırmalar kullanırken üst için daha az güçlü ve CPU tabanlı bir yapılandırma kullanabilirsiniz. Diğer bir yaygın, her bir alt öğenin farklı bağımsız değişkenlerini ve verileri geçirmektir. Alt çalışmayı özelleştirmek için `ScriptRunConfig` alt çalıştırma için bir nesne oluşturun. Aşağıdaki kod aşağıdakileri yapar:

- Çalışma alanından adlı bir işlem kaynağı alma `"gpu-cluster"``ws`
- Alt nesnelere geçirilecek farklı bağımsız değişken değerlerini yineler `ScriptRunConfig`
- Özel işlem kaynağını ve bağımsız değişkenini kullanarak yeni bir alt çalışma oluşturur ve gönderir
- Tüm alt çalışma tamamlanana kadar bloklar

```python
# parent.py
# This script controls the launching of child scripts
from azureml.core import Run, ScriptRunConfig

compute_target = ws.compute_targets["gpu-cluster"]

run = Run.get_context()

child_args = ['Apple', 'Banana', 'Orange']
for arg in child_args: 
    run.log('Status', f'Launching {arg}')
    child_config = ScriptRunConfig(source_directory=".", script='child.py', arguments=['--fruit', arg], compute_target=compute_target)
    # Starts the run asynchronously
    run.submit_child(child_config)

# Experiment will "complete" successfully at this point. 
# Instead of returning immediately, block until child runs complete

for child in run.get_children():
    child.wait_for_completion()
```

Aynı yapılandırmalarda, bağımsız değişkenlerle ve girişlerle çok sayıda alt çalışma oluşturmak için [`create_children()`](/python/api/azureml-core/azureml.core.run.run#create-children-count-none--tag-key-none--tag-values-none-) yöntemini kullanın. Her oluşturma bir ağ çağrısıyla sonuçlandığından, bir toplu iş oluşturmak, bunlardan birini oluşturmaktan daha etkilidir.

Bir alt çalıştırma içinde, üst çalıştırma KIMLIĞINI görüntüleyebilirsiniz:

```python
## In child run script
child_run = Run.get_context()
child_run.parent.id
```

### <a name="query-child-runs"></a>Alt çalıştırmaları sorgula

Belirli bir üst öğenin alt çalıştırmalarını sorgulamak için [`get_children()`](/python/api/azureml-core/azureml.core.run%28class%29#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) yöntemini kullanın. ``recursive = True``Bağımsız değişkeni, iç içe geçmiş alt öğe ve alt alt ağacı sorgulamanızı sağlar.

```python
print(parent_run.get_children())
```

### <a name="log-to-parent-or-root-run"></a>Üst veya kök çalıştırmada günlüğe kaydet

`Run.parent`Alanı, geçerli alt çalışmayı başlatan çalıştırmaya erişmek için kullanabilirsiniz. Bunun için genel kullanım örneği, günlük sonuçlarını tek bir yerde birleştirmek istediğinizde kullanılır. Alt çalıştırmanın zaman uyumsuz olarak yürütüleceğini ve üst öğenin alt çalıştırmanın tamamlanmasını bekleyebilme özelliğinden daha fazla bir sıralama veya eşitleme garantisi bulunmadığını unutmayın.

```python
# in child (or even grandchild) run

def root_run(self : Run) -> Run :
    if self.parent is None : 
        return self
    return root_run(self.parent)

current_child_run = Run.get_context()
root_run(current_child_run).log("MyMetric", f"Data from child run {current_child_run.id}")

```

## <a name="example-notebooks"></a>Örnek not defterleri

Aşağıdaki Not defterleri bu makaledeki kavramları göstermektedir:

* Günlüğe kaydetme API 'Leri hakkında daha fazla bilgi edinmek için bkz. [günlük API Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api/logging-api.ipynb).

* Azure Machine Learning SDK ile çalıştırmaları yönetme hakkında daha fazla bilgi için bkz. [Manage çalıştırmaları Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/manage-runs/manage-runs.ipynb).

## <a name="next-steps"></a>Sonraki adımlar

* Denemeleri için ölçümleri günlüğe kaydetme hakkında bilgi edinmek için bkz. [eğitim çalıştırmaları sırasında günlük ölçümleri](how-to-track-experiments.md).
* Kaynak ve günlüklerin Azure Machine Learning nasıl izleneceğini öğrenmek için bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md).
