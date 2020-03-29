---
title: Makine öğrenmesi işlem hatlarında hata ayıklama ve sorun giderme
titleSuffix: Azure Machine Learning
description: Python için Azure Machine Learning SDK'da makine öğrenimi boru hatlarını ayıklama ve sorun giderme. Ardışık hatlar geliştirmek için yaygın tuzaklar ve komut dosyalarınızı uzaktan yürütme den önce ve sırasında hata ayıklamanıza yardımcı olacak ipuçlarını öğrenin. Makine öğrenimi ardışık hatlarınızı etkileşimli olarak hata ayıklamak için Visual Studio Code'u nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 03/18/2020
ms.openlocfilehash: 578e935ee742ad476aeafb53670f0a92035249e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064075"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Makine öğrenmesi işlem hatlarında hata ayıklama ve sorun giderme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) ve Azure [Machine Learning tasarımcısında (önizleme)](https://docs.microsoft.com/azure/machine-learning/concept-designer) [makine öğrenimi boru hatlarını](concept-ml-pipelines.md) nasıl hata ayıklamave sorun giderme yle öğreneceğinizi öğreneceksiniz. Nasıl yapılabilmek için bilgi verilir:

* Azure Machine Learning SDK'yı kullanarak hata ayıklama
* Azure Machine Learning Designer'ı kullanarak hata ayıklama
* Uygulama Öngörülerini Kullanarak Hata Ayıklama
* Visual Studio Code (VS Code) ve Python Tools for Visual Studio (PTVSD) araçlarını kullanarak etkileşimli hata ayıklama

## <a name="debug-and-troubleshoot-in-the-azure-machine-learning-sdk"></a>Azure Machine Learning SDK'da hata ayıklama ve sorun giderme
Aşağıdaki bölümler, boru hatları inşa ederken sık karşılaşılan tuzaklara genel bir bakış ve bir ardışık alanda çalışan kodunuzu hata ayıklama için farklı stratejiler sağlar. Beklendiği gibi çalışacak bir boru hattı almakta sorun yaşıyorsanız aşağıdaki ipuçlarını kullanın.

### <a name="testing-scripts-locally"></a>Komut dosyalarını yerel olarak test etme

Bir ardışık işlem aygıtındaki en yaygın hatalardan biri, ekli bir komut dosyasının (veri temizleme komut dosyası, puanlama komut dosyası, vb.) beklendiği gibi çalışmaması veya Azure Makinesi'ndeki çalışma alanınızda hata ayıklamanın zor olduğu uzak bilgi işlem bağlamında çalışma zamanı hataları içermesidir. Öğrenme stüdyosu. 

Ardışık hatlar yerel olarak çalıştırılamaz, ancak komut dosyalarını yerel makinenizde veya izne çalıştırmak, bilgi işlem ve ortam oluşturma işlemini beklemek zorunda kalmadığınız için hata ayıklama işlemini daha hızlı ayıklamanızı sağlar. Bunu yapmak için bazı geliştirme çalışmaları gereklidir:

* Verileriniz bir bulut veri deposundaysa, verileri karşıdan yüklemeniz ve komut dosyanız için kullanılabilir hale getirmeniz gerekir. Verilerinizin küçük bir örneğini kullanmak, çalışma süresini azaltmanın ve komut dosyası davranışı hakkında hızlı bir şekilde geri bildirim almak için iyi bir yoldur
* Bir ara ardışık yapı adımını simüle etmeye çalışıyorsanız, belirli komut dosyasının önceki adımdan beklediği nesne türlerini el ile oluşturmanız gerekebilir
* Ayrıca kendi ortamınızı tanımlamanız ve uzak bilgi işlem ortamınızda tanımlanan bağımlılıkları çoğaltmanız gerekir

Yerel ortamınızda çalışacak bir komut dosyası kurulumunuz olduğunda, hata ayıklama görevlerini yapmak çok daha kolaydır:

* Özel hata ayıklama yapılandırması ekleme
* Yürütmeyi duraklatma ve nesne durumunu denetleme
* Çalışma süresine kadar açıklanmayacak tür veya mantıksal hataları yakalama

> [!TIP] 
> Komut dosyanızın beklendiği gibi çalıştığını doğruladıktan sonra, komut dosyasını birden çok adımla bir ardışık alanda çalıştırmayı denemeden önce iyi bir sonraki adım komut dosyasını tek adımlı bir ardışık alanda çalıştırıyor.

### <a name="debugging-scripts-from-remote-context"></a>Komut dosyalarını uzak bağlamdan hata ayıklama

Komut dosyalarını yerel olarak test etmek, bir ardışık yapı oluşturmaya başlamadan önce büyük kod parçalarını ve karmaşık mantığı hata ayıklamanın harika bir yoludur, ancak bir noktada büyük olasılıkla gerçek ardışık hatlar çalışır durumdayken komut hatalarını ayıklamanız gerekir, özellikle de oluşan davranışı tanılarken boru hattı adımları arasındaki etkileşim sırasında. JavaScript kodunu `print()` nasıl hata ayıklayacağınıza benzer şekilde, uzaktan yürütme sırasında nesne durumunu ve beklenen değerleri görebilmeniz için adım komut dosyalarınızdaki ifadelerin liberal kullanımını öneririz.

Günlük dosyası `70_driver_log.txt` şunları içerir: 

* Komut dosyanızın yürütülmesi sırasında yazdırılan tüm ifadeler
* Komut dosyası için yığın izi 

Bu ve diğer günlük dosyalarını portalda bulmak için, çalışma alanınızdaki ardışık hatlar çalıştır'a ilk tıklayın.

![Boru hattı çalıştırma listesi sayfası](./media/how-to-debug-pipelines/pipelinerun-01.png)

Ardışık hatlar çalıştır ayrıntı sayfasına gidin.

![Pipeline run ayrıntı sayfası](./media/how-to-debug-pipelines/pipelinerun-02.png)

Belirli bir adım için modüle tıklayın. **Günlükler** sekmesine gidin. Diğer günlükler, ortam resmi oluşturma işleminiz ve adım hazırlama komut dosyalarınız hakkında bilgi içerir.

![Pipeline run detay sayfası günlüğü sekmesi](./media/how-to-debug-pipelines/pipelinerun-03.png)

> [!TIP]
> *Yayımlanmış ardışık lıklar* için çalışır, çalışma alanınızdaki **Uç Noktalar** sekmesinde bulunabilir. *Yayınlanmamış boru hatlarıiçin* çalışır **Denemeler** veya **Boru Hatları**bulunabilir.

### <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

Aşağıdaki tablo, olası çözümlerle birlikte, boru hattı geliştirme sırasında sık karşılaşılan sorunları içerir.

| Sorun | Olası çözüm |
|--|--|
| Dizin için `PipelineData` veri geçirilemiyor | Komut dosyasında, ardışık alanınızın adım çıktı verilerini beklediği yere karşılık gelen bir dizin oluşturduğunuzdan emin olun. Çoğu durumda, bir giriş bağımsız değişkeni çıktı dizini tanımlar ve sonra açıkça dizin oluşturursunuz. Çıktı `os.makedirs(args.output_dir, exist_ok=True)` dizini oluşturmak için kullanın. Bu tasarım deseni gösteren bir puanlama komut dosyası örneği için [öğreticiye](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script) bakın. |
| Bağımlılık hataları | Komut dosyalarını yerel olarak geliştirdiyseniz ve sınadıysanız, ancak ardışık yolda uzak bir bilgi işlemde çalışırken bağımlılık sorunları bulduysanız, bilgi işlem ortamı bağımlılıklarınızın ve sürümlerinizin test ortamınızla eşleştiğinden emin olun. (Bkz. [Çevre oluşturma, önbelleğe alma ve yeniden kullanma](https://docs.microsoft.com/azure/machine-learning/concept-environments#environment-building-caching-and-reuse)|
| İşlem hedefleri ile belirsiz hatalar | İşlem hedeflerini siler ve yeniden oluşturmak, işlem hedefleriyle belirli sorunları çözebilir. |
| Adımları yeniden kullanmıyor | Adım yeniden kullanımı varsayılan olarak etkinleştirilir, ancak bir ardışık aygıt adımında devre dışı bırakmadığınızdan emin olun. Yeniden kullanım devre dışı `allow_reuse` bırakılırsa, adımdaki `False`parametre . |
| Boru hattı gereksiz yere yeniden çalışıyor | Adımların yalnızca temel verileri veya komut dosyaları değiştiğinde yeniden çalıştırıldığından emin olmak için, her adım için dizinlerinizi ayırın. Aynı kaynak dizinini birden çok adım için kullanırsanız, gereksiz yeniden çalıştırmalar yaşayabilirsiniz. Bu `source_directory` adım için yalıtılmış dizininizi işaret etmek ve birden çok adım için aynı `source_directory` yolu kullanmadığınızdan emin olmak için bir ardışık etki hattı adımı nesnesindeki parametreyi kullanın. |

### <a name="logging-options-and-behavior"></a>Günlüğe kaydetme seçenekleri ve davranışı

Aşağıdaki tablo, ardışık hatlar için farklı hata ayıklama seçenekleri için bilgi sağlar. Burada gösterilen Azure Machine Learning, Python ve OpenCensus seçeneklerinin yanı sıra diğer seçenekler de mevcut olduğundan, kapsamlı bir liste değildir.

| Kitaplık                    | Tür   | Örnek                                                          | Hedef                                  | Kaynaklar                                                                                                                                                                                                                                                                                                                    |
|----------------------------|--------|------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure Machine Learning SDK’sı | Ölçüm | `run.log(name, val)`                                             | Azure Makine Öğrenme Portalı UI             | [Denemeler nasıl izleyebilirsiniz?](how-to-track-experiments.md#available-metrics-to-track)<br>[azureml.core.Run sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=experimental)                                                                                                                                                 |
| Python yazdırma/günlüğe kaydetme    | Günlük    | `print(val)`<br>`logging.info(message)`                          | Sürücü günlükleri, Azure Machine Learning Designer | [Denemeler nasıl izleyebilirsiniz?](how-to-track-experiments.md#available-metrics-to-track)<br><br>[Python günlüğü](https://docs.python.org/2/library/logging.html)                                                                                                                                                                       |
| Açık Nüfus Sayımı Python          | Günlük    | `logger.addHandler(AzureLogHandler())`<br>`logging.log(message)` | Uygulama Öngörüleri - izlemeler                | [Application Insights’ta işlem hatlarında hata ayıklama](how-to-debug-pipelines-application-insights.md)<br><br>[OpenCensus Azure İzleyici Dışarı Aktarıcıları](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)<br>[Python günlük yemek kitabı](https://docs.python.org/3/howto/logging-cookbook.html) |

#### <a name="logging-options-example"></a>Günlüğe kaydetme seçenekleri örneği

```python
import logging

from azureml.core.run import Run
from opencensus.ext.azure.log_exporter import AzureLogHandler

run = Run.get_context()

# Azure ML Scalar value logging
run.log("scalar_value", 0.95)

# Python print statement
print("I am a python print statement, I will be sent to the driver logs.")

# Initialize python logger
logger = logging.getLogger(__name__)
logger.setLevel(args.log_level)

# Plain python logging statements
logger.debug("I am a plain debug statement, I will be sent to the driver logs.")
logger.info("I am a plain info statement, I will be sent to the driver logs.")

handler = AzureLogHandler(connection_string='<connection string>')
logger.addHandler(handler)

# Python logging with OpenCensus AzureLogHandler
logger.warning("I am an OpenCensus warning statement, find me in Application Insights!")
logger.error("I am an OpenCensus error statement with custom dimensions", {'step_id': run.id})
``` 

## <a name="debug-and-troubleshoot-in-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısında hata ayıklama ve sorun giderme (önizleme)

Bu bölümde, tasarımcıdaki ardışık işlerle nasıl giderilene genel bir bakış sağlanmıştır.
Tasarımcıda oluşturulan ardışık hatlar için, **günlük dosyalarını** yazma sayfasında veya ardışık hatlar çalıştır ayrıntı sayfasında bulabilirsiniz.

### <a name="access-logs-from-the-authoring-page"></a>Yazma sayfasından günlüklere erişin

Bir ardışık hatlar hattı çalıştırdığınızda ve yazma sayfasında kaldığınızda, her modül için oluşturulan günlük dosyalarını bulabilirsiniz.

1. Yazma tuvalindeki herhangi bir modülü seçin.
1. Modülün sağ bölmesinde **Çıktılar+ogs** sekmesine gidin.
1. Günlük dosyasını seçin`70_driver_log.txt`

    ![Sayfa modülü günlüklerinin yazilmesi](./media/how-to-debug-pipelines/pipelinerun-05.png)

### <a name="access-logs-from-pipeline-runs"></a>Ardışık hatlar çalıştıran erişim günlükleri

Belirli çalıştırmaların günlük **dosyalarını, Ardışık Hatlar** veya **Denemeler** bölümlerinde, ardışık hatlar çalıştır ayrıntı sayfasında da bulabilirsiniz.

1. Tasarımcıda oluşturulan bir ardışık hatlar hattı çalıştırın.
    ![Boru hattı çalıştırma sayfası](./media/how-to-debug-pipelines/pipelinerun-04.png)
1. Önizleme bölmesindeki herhangi bir modülü seçin.
1. Modülün sağ bölmesinde **Çıktılar+ogs** sekmesine gidin.
1. Günlük dosyasını seçin`70_driver_log.txt`

## <a name="debug-and-troubleshoot-in-application-insights"></a>Uygulama Öngörüleri'nde hata ayıklama ve sorun giderme
OpenCensus Python kitaplığını bu şekilde kullanma hakkında daha fazla bilgi için şu kılavuza bakın: [Application Insights'ta hata ayıklama ve sorun giderme makinesi öğrenme ardışık hatları](how-to-debug-pipelines-application-insights.md)

## <a name="debug-and-troubleshoot-in-visual-studio-code"></a>Visual Studio Code'da hata ayıklama ve sorun giderme

Bazı durumlarda, ML ardışık ardınızda kullanılan Python kodunu etkileşimli olarak hata ayıklamanız gerekebilir. Visual Studio Code (VS Code) ve Python Tools for Visual Studio (PTVSD) araçlarını kullanarak, kodlama ortamında çalışırken koda ekleyebilirsiniz.

### <a name="prerequisites"></a>Ön koşullar

* Azure __Sanal Ağı__kullanacak şekilde yapılandırılan Bir __Azure Machine Learning çalışma alanı.__
* Satır başlarının bir parçası olarak Python komut dosyalarını kullanan bir __Azure Machine Learning ardışık hattı.__ Örneğin, bir PythonScriptStep.
* __Sanal ağda__ bulunan ve boru hattı tarafından eğitim __için kullanılan__Bir Azure Machine Learning Compute kümesi.
* __Sanal ağda__olan bir __geliştirme ortamı.__ Geliştirme ortamı aşağıdakilerden biri olabilir:

    * Sanal ağdaki Azure Sanal Makinesi
    * Sanal ağdaki Notebook VM'nin bir işlem örneği
    * Sanal özel ağ (VPN) ile sanal ağa bağlı bir istemci makinesi.

Azure Machine Learning ile Azure Sanal Ağı kullanma hakkında daha fazla bilgi için, [Azure Sanal Ağı'ndaki Güvenli Azure ML deneme ve çıkarım işleri](how-to-enable-virtual-network.md)ne bakın.

### <a name="how-it-works"></a>Nasıl çalışır?

ML ardışık adımlar Python komut dosyaları çalıştırın. Bu komut dosyaları aşağıdaki eylemleri gerçekleştirmek için değiştirilir:
    
1. Üzerinde çalıştıkları ana bilgisayarın IP adresini günlüğe kaydedin. Hata ayıkıcıyı komut dosyasına bağlamak için IP adresini kullanırsınız.

2. PTVSD hata ayıklama bileşenini başlatın ve bir hata ayıklamanın bağlanmasını bekleyin.

3. Geliştirme ortamınızdan, komut dosyasının çalıştığı IP adresini bulmak için eğitim işlemi tarafından oluşturulan günlükleri izlersiniz.

4. Hata ayıklayıcıyı bir `launch.json` dosya kullanarak bağlamak için VS Code IP adresini söylersiniz.

5. Hata ayıklama yı ekleyip komut dosyasına etkileşimli olarak basabilirsiniz.

### <a name="configure-python-scripts"></a>Python komut dosyalarını yapılandırma

Hata ayıklamayı etkinleştirmek için ML ardışık ardınızdaki adımlar tarafından kullanılan Python komut dosyasında aşağıdaki değişiklikleri yapın:

1. Aşağıdaki alma deyimlerini ekleyin:

    ```python
    import ptvsd
    import socket
    from azureml.core import Run
    ```

1. Aşağıdaki bağımsız değişkenleri ekleyin. Bu bağımsız değişkenler hata ayıklamayı gerektiği gibi etkinleştirmenize ve hata ayıklamayı eklemek için zaman arasını ayarlamanıza olanak tanır:

    ```python
    parser.add_argument('--remote_debug', action='store_true')
    parser.add_argument('--remote_debug_connection_timeout', type=int,
                    default=300,
                    help=f'Defines how much time the Azure ML compute target '
                    f'will await a connection from a debugger client (VSCODE).')
    ```

1. Aşağıdaki ifadeleri ekleyin. Bu ifadeler, kodun üzerinde çalışan düğümün IP adresini günlüğe kaydedebilmeniz için geçerli çalıştır bağlamını yükler:

    ```python
    global run
    run = Run.get_context()
    ```

1. PTVSD'yi başlatan ve hata ayıklamanın eklenmesini bekleyen bir `if` deyim ekleyin. Zaman aramadan önce hata ayıklama eklenmezse, komut dosyası normal olarak devam eder.

    ```python
    if args.remote_debug:
        print(f'Timeout for debug connection: {args.remote_debug_connection_timeout}')
        # Log the IP and port
        ip = socket.gethostbyname(socket.gethostname())
        print(f'ip_address: {ip}')
        ptvsd.enable_attach(address=('0.0.0.0', 5678),
                            redirect_output=True)
        # Wait for the timeout for debugger to attach
        ptvsd.wait_for_attach(timeout=args.remote_debug_connection_timeout)
        print(f'Debugger attached = {ptvsd.is_attached()}')
    ```

Aşağıdaki Python örneği, `train.py` hata ayıklamayı sağlayan temel bir dosyayı gösterir:

```python
# Copyright (c) Microsoft. All rights reserved.
# Licensed under the MIT license.

import argparse
import os
import ptvsd
import socket
from azureml.core import Run

print("In train.py")
print("As a data scientist, this is where I use my training code.")

parser = argparse.ArgumentParser("train")

parser.add_argument("--input_data", type=str, help="input data")
parser.add_argument("--output_train", type=str, help="output_train directory")

# Argument check for remote debugging
parser.add_argument('--remote_debug', action='store_true')
parser.add_argument('--remote_debug_connection_timeout', type=int,
                    default=300,
                    help=f'Defines how much time the AML compute target '
                    f'will await a connection from a debugger client (VSCODE).')
# Get run object, so we can find and log the IP of the host instance
global run
run = Run.get_context()

args = parser.parse_args()

# Start debugger if remote_debug is enabled
if args.remote_debug:
    print(f'Timeout for debug connection: {args.remote_debug_connection_timeout}')
    # Log the IP and port
    ip = socket.gethostbyname(socket.gethostname())
    print(f'ip_address: {ip}')
    ptvsd.enable_attach(address=('0.0.0.0', 5678),
                        redirect_output=True)
    # Wait for the timeout for debugger to attach
    ptvsd.wait_for_attach(timeout=args.remote_debug_connection_timeout)
    print(f'Debugger attached = {ptvsd.is_attached()}')

print("Argument 1: %s" % args.input_data)
print("Argument 2: %s" % args.output_train)

if not (args.output_train is None):
    os.makedirs(args.output_train, exist_ok=True)
    print("%s created" % args.output_train)
```

### <a name="configure-ml-pipeline"></a>ML boru hattını yapılandırma

PTVSD'yi başlatmak ve çalışma bağlamını elde etmek için gereken `pip_packages=['ptvsd', 'azureml-sdk==1.0.83']`Python paketlerini sağlamak için bir ortam oluşturun ve ayarlayın. SDK sürümünü kullandığınız sürümle eşleşecek şekilde değiştirin. Aşağıdaki kod snippet nasıl bir ortam oluşturmak için gösterir:

```python
# Use a RunConfiguration to specify some additional requirements for this step.
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import DEFAULT_CPU_IMAGE

# create a new runconfig object
run_config = RunConfiguration()

# enable Docker 
run_config.environment.docker.enabled = True

# set Docker base image to the default CPU-based image
run_config.environment.docker.base_image = DEFAULT_CPU_IMAGE

# use conda_dependencies.yml to create a conda environment in the Docker image for execution
run_config.environment.python.user_managed_dependencies = False

# specify CondaDependencies obj
run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'],
                                                                           pip_packages=['ptvsd', 'azureml-sdk==1.0.83'])
```

Python [komut dosyalarını yapılandır](#configure-python-scripts) bölümünde, ML ardışık komut satırlarınızın kullandığı komut dosyalarına iki yeni bağımsız değişken eklendi. Aşağıdaki kod snippet bileşeni için hata ayıklama etkinleştirmek ve bir zaman aşıntısı ayarlamak için bu bağımsız değişkenler nasıl kullanılacağını gösterir. Ayrıca, daha önce ayarlayarak `runconfig=run_config`oluşturulan ortamın nasıl kullanılacağını da gösterir:

```python
# Use RunConfig from a pipeline step
step1 = PythonScriptStep(name="train_step",
                         script_name="train.py",
                         arguments=['--remote_debug', '--remote_debug_connection_timeout', 300],
                         compute_target=aml_compute, 
                         source_directory=source_directory,
                         runconfig=run_config,
                         allow_reuse=False)
```

Ardışık hatlar çalıştığında, her adım bir alt çalışma oluşturur. Hata ayıklama etkinleştirilmişse, değiştirilen komut dosyası, alt çalıştırma `70_driver_log.txt` için aşağıdaki metne benzer bilgileri kaydeder:

```text
Timeout for debug connection: 300
ip_address: 10.3.0.5
```

Değeri `ip_address` kaydedin. Bir sonraki bölümde kullanılır.

> [!TIP]
> Bu ardışık işlem adımı için çalıştırılabilen alt çalışma günlüklerinden IP adresini de bulabilirsiniz. Bu bilgileri görüntüleme hakkında daha fazla bilgi için Bkz. [Azure ML deneme çalışır ve ölçümleri izleyin.](how-to-track-experiments.md)

### <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

1. Python Araçlar Görsel Studio (PTVSD) için VS Kodu geliştirme ortamına yüklemek için aşağıdaki komutu kullanın:

    ```
    python -m pip install --upgrade ptvsd
    ```

    VS Code ile PTVSD kullanma hakkında daha fazla bilgi için [Uzaktan Hata Ayıklama](https://code.visualstudio.com/docs/python/debugging#_remote-debugging)bölümüne bakın.

1. Hata ayıklayıcıyı çalıştıran Azure Machine Learning bilgiişlemiyle iletişim kuracak şekilde VS Kodunu yapılandırmak için yeni bir hata ayıklama yapılandırması oluşturun:

    1. VS Kodu'ndan __Hata Ayıklama__ menüsünü seçin ve ardından __yapılandırmaları aç'ı__seçin. __launch.json__ adlı bir dosya açılır.

    1. __launch.json__ dosyasında, içeren `"configurations": [`satırı bulun ve aşağıdaki metni ekleyin. `"host": "10.3.0.5"` Girişi, önceki bölümden günlüklerinizde döndürülen IP adresine değiştirin. `"localRoot": "${workspaceFolder}/code/step"` Girişi, komut dosyasının bir kopyasının debugged tutulduğunu içeren yerel bir dizine değiştirin:

        ```json
        {
            "name": "Azure Machine Learning Compute: remote debug",
            "type": "python",
            "request": "attach",
            "port": 5678,
            "host": "10.3.0.5",
            "redirectOutput": true,
            "pathMappings": [
                {
                    "localRoot": "${workspaceFolder}/code/step1",
                    "remoteRoot": "."
                }
            ]
        }
        ```

        > [!IMPORTANT]
        > Yapılandırmalar bölümünde zaten başka girişler varsa, eklediğiniz koddan sonra virgül (,) ekleyin.

        > [!TIP]
        > En iyi yöntem, komut dosyalarının kaynaklarını ayrı dizinlerde `localRoot` tutmaktır, `/code/step1`bu nedenle örnek değer başvuruları.
        >
        > Farklı dizinlerde birden çok komut dosyası nın hata ayıklama hizmetindeyseniz, her komut dosyası için ayrı bir yapılandırma bölümü oluşturun.

    1. __Launch.json__ dosyasını kaydedin.

### <a name="connect-the-debugger"></a>Hata ayıklamayı bağlama

1. VS Kodu'nu açın ve komut dosyasının yerel bir kopyasını açın.
2. Bağlandıktan sonra komut dosyasının durmasını istediğiniz kesme noktaları ayarlayın.
3. Alt işlem komut dosyası çalışırken ve `Timeout for debug connection` günlüklerde görüntülenirken, F5 tuşunu kullanın veya __Hata Ayıklama'yı__seçin. İstendiğinde, __Azure Machine Learning Compute: uzaktan hata ayıklama__ yapılandırmasını seçin. Ayrıca yan çubuktan hata ayıklama simgesini, __Azure Machine Learning:Hata Ayıklama__ menüsünden uzaktan hata ayıklama girişi'ni seçebilir ve hata ayıklama aracını eklemek için yeşil oku kullanabilirsiniz.

    Bu noktada, VS Kodu işlem düğümünde PTVSD'ye bağlanır ve daha önce ayarladığınız kesme noktasında durur. Artık çalışırken kodun içinden geçebilir, değişkenleri görüntüleyebilir, vb.

    > [!NOTE]
    > Günlük, zaman aşımı `Debugger attached = False`süresi dolmuş ve komut dosyası hata ayıklama olmadan devam belirten bir giriş görüntüler. Ardışık bölümü yeniden gönderin ve `Timeout for debug connection` iletiden sonra ve zaman aşımı sona ermeden önce hata ayıklama bağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) paketi ve [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) paketi ile ilgili yardım için SDK başvurusuna bakın.

* Tasarımcı özel [durumları ve hata kodları](algorithm-module-reference/designer-error-codes.md)listesine bakın.
