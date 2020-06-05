---
title: Hata ayıklama & ML işlem hatları sorunlarını giderme
titleSuffix: Azure Machine Learning
description: Python 'da Azure Machine Learning işlem hatlarınızı hata ayıklayın. İşlem hatları geliştirmek için ortak olan ve uzaktan yürütme sırasında ve betiklerinizde hata ayıklamanıza yardımcı olan ipuçları hakkında bilgi edinin. Visual Studio Code kullanarak Machine Learning işlem hatlarınızı etkileşimli olarak hata ayıklamanızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: troubleshooting
author: likebupt
ms.author: keli19
ms.date: 03/18/2020
ms.openlocfilehash: bf6a8dd0bfc4ffb9f6b6fa0c9b1d864c4298755a
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84433443"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Makine öğrenmesi işlem hatlarında hata ayıklama ve sorun giderme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) ve [Azure Machine Learning Tasarımcısı 'nda (Önizleme)](https://docs.microsoft.com/azure/machine-learning/concept-designer) [makine öğrenimi](concept-ml-pipelines.md) işlem hatlarında hata ayıklamayı ve sorun gidermeyi öğreneceksiniz. Bilgiler şu şekilde sunulmaktadır:

* Azure Machine Learning SDK kullanarak hata ayıklama
* Azure Machine Learning tasarımcısını kullanarak hata ayıklama
* Application Insights kullanarak hata ayıkla
* Visual Studio Code (VS Code) ve Visual Studio için Python Araçları (PTVSD) kullanarak etkileşimli olarak hata ayıklayın

## <a name="debug-and-troubleshoot-in-the-azure-machine-learning-sdk"></a>Azure Machine Learning SDK 'da hata ayıklama ve sorun giderme
Aşağıdaki bölümler, işlem hatları oluştururken ortak olan genel bakışa genel bakış ve bir işlem hattında çalışan kodunuzda hata ayıklama için farklı stratejiler sağlar. İşlem hattını beklenen şekilde çalıştırmak için bir işlem hattı alırken sorun yaşadığınızda aşağıdaki ipuçlarını kullanın.

### <a name="testing-scripts-locally"></a>Betikleri yerel olarak test etme

Bir işlem hattındaki en yaygın hatalardan biri, ekli bir betiğin (veri temizleme betiği, Puanlama betiği, vb.), istenen şekilde çalışmadığını veya uzak işlem bağlamında, Azure Machine Learning Studio 'daki çalışma alanınızda hata ayıklamanın zor olduğu çalışma zamanı hatalarını içerir. 

Ardışık düzenleri yerel olarak çalıştırılamaz, ancak komut dosyalarını yerel makinenizde yalıtımlarda çalıştırmak, işlem ve ortam oluşturma işlemini beklemek zorunda olmadığınızdan daha hızlı hata ayıklamanıza olanak tanır. Bunu yapmak için bazı geliştirme işleri gereklidir:

* Verileriniz bir bulut veri deposunda yer alıyorsa, verileri indirmeniz ve betiğiniz için kullanılabilir hale getirmeniz gerekir. Verilerinizin küçük bir örneğini kullanmak çalışma zamanını kesmek ve betik davranışında hızlıca geri bildirim almak için iyi bir yoldur
* Ara işlem hattı adımının benzetimini gerçekleştirmeye çalışıyorsanız, belirli bir betiğin önceki adımdan beklediği nesne türlerini el ile oluşturmanız gerekebilir
* Ayrıca kendi ortamınızı tanımlamanız ve Uzaktan işlem ortamınızda tanımlanan bağımlılıkları çoğaltmanız gerekecektir

Yerel ortamınızda çalıştırmak üzere bir betik kurulumuna sahip olduktan sonra, şu gibi hata ayıklama görevlerinin olması çok daha kolaydır:

* Özel hata ayıklama yapılandırması iliştirme
* Yürütmeyi duraklatma ve nesne durumunu inceleme
* Çalışma zamanına kadar sunulmayacak tür veya mantıksal hataları yakalama

> [!TIP] 
> Betiğinizin beklendiği gibi çalıştığını doğrulayabilmeniz için, bir sonraki adım komut dosyasını birden çok adımla bir işlem hattında çalıştırmayı denemeden önce tek adımlı bir işlem hattında çalıştırıyor.

### <a name="debugging-scripts-from-remote-context"></a>Uzak bağlamdaki betiklerin hatalarını ayıklama

Komut dosyalarını yerel olarak test etmek, bir işlem hattı oluşturmaya başlamadan önce büyük kod parçalarının ve karmaşık mantığın hatalarını ayıklamanın harika bir yoludur. ancak bazı bir noktada, özellikle de işlem hattı adımları arasındaki etkileşim sırasında oluşan davranışı tanılarken, büyük olasılıkla gerçek ardışık düzen sırasında betiklerde hata ayıklaması yapmanız gerekir. `print()`Uzaktan yürütme sırasında nesne durumunu ve beklenen değerleri görebilmeniz için, JavaScript kodunun hatalarını ayıklamanıza benzer şekilde, adım betiklerinizde deyimlerin serbest bir şekilde kullanılmasını öneririz.

Günlük dosyası `70_driver_log.txt` şunları içerir: 

* Betiğinizin yürütülmesi sırasında tüm yazdırılmış deyimler
* Betik için yığın izlemesi 

Portalda bu ve diğer günlük dosyalarını bulmak için, önce çalışma alanınızdaki işlem hattına tıklayın.

![İşlem hattı çalıştırma listesi sayfası](./media/how-to-debug-pipelines/pipelinerun-01.png)

İşlem hattı çalıştırma ayrıntısı sayfasına gidin.

![İşlem hattı çalıştırma ayrıntısı sayfası](./media/how-to-debug-pipelines/pipelinerun-02.png)

Belirli bir adım için modüle tıklayın. **Günlükler** sekmesine gidin. Diğer Günlükler, ortam görüntüsü oluşturma işlemi ve adım hazırlama betikleriniz hakkında bilgiler içerir.

![İşlem hattı çalıştırma ayrıntısı sayfa günlüğü sekmesi](./media/how-to-debug-pipelines/pipelinerun-03.png)

> [!TIP]
> Yayınlanan işlem *hatları* için çalıştırmalar çalışma alanınızdaki **uç noktalar** sekmesinde bulunabilir. *Yayımlanamayan işlem hatları* için çalıştırmalar, **denemeleri** veya işlem **hatları**içinde bulunabilir.

### <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

Aşağıdaki tabloda, potansiyel çözümlerle birlikte işlem hattı geliştirme sırasında yaygın sorunlar yer almaktadır.

| Sorun | Olası çözüm |
|--|--|
| Verileri dizine geçiremedi `PipelineData` | Komut dosyasında, işlem hattının adım çıkış verilerini beklediği yere karşılık gelen bir dizin oluşturduğunuzdan emin olun. Çoğu durumda, bir giriş bağımsız değişkeni çıkış dizinini tanımlar ve ardından dizini açıkça oluşturursunuz. `os.makedirs(args.output_dir, exist_ok=True)`Çıkış dizinini oluşturmak için kullanın. Bu tasarım modelini gösteren bir Puanlama betiği örneği için [öğreticiye](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script) bakın. |
| Bağımlılık hataları | Komut dosyalarını yerel olarak geliştirdiyseniz, ancak işlem hattındaki uzak bir işlem üzerinde çalışırken bağımlılık sorunları bulursanız, işlem ortamı bağımlılıklarınızın ve sürümlerinin test ortamınızla eşleştiğinden emin olun. (Bkz. [ortam oluşturma, önbelleğe alma ve yeniden kullanma](https://docs.microsoft.com/azure/machine-learning/concept-environments#environment-building-caching-and-reuse)|
| İşlem hedefleri ile belirsiz hatalar | İşlem hedeflerini silme ve yeniden oluşturma işlemi, işlem hedefleriyle ilgili bazı sorunları çözebilir. |
| İşlem hattı adımları yeniden kullanma | Adım yeniden kullanım varsayılan olarak etkindir, ancak işlem hattı adımında devre dışı bırakılmadığınızdan emin olun. Yeniden kullanım devre dışıysa, `allow_reuse` adımdaki parametre olarak ayarlanır `False` . |
| İşlem hattı gereksiz yere yeniden çalıştırılıyor | Adımların yalnızca temel alınan verileri veya betikleri değiştiğinde yeniden çalıştırılması için dizinlerinizi her bir adımla ayırın. Birden çok adım için aynı kaynak dizinini kullanırsanız, gereksiz yeniden çalıştırma işlemleri yaşayabilirsiniz. İşlem `source_directory` hattı adımı nesnesi üzerinde parametresini kullanarak bu adım için yalıtılmış dizininizi işaret edin ve `source_directory` birden çok adım için aynı yolu kullanmadığınız emin olun. |

### <a name="logging-options-and-behavior"></a>Günlüğe kaydetme seçenekleri ve davranışı

Aşağıdaki tabloda, işlem hatları için farklı hata ayıklama seçenekleri sağlanmıştır. Burada yalnızca Azure Machine Learning, Python ve OpenCensus seçeneklerinin yanı sıra diğer seçenekler mevcut olduğundan, kapsamlı bir liste değildir.

| Kitaplık                    | Tür   | Örnek                                                          | Hedef                                  | Kaynaklar                                                                                                                                                                                                                                                                                                                    |
|----------------------------|--------|------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure Machine Learning SDK’sı | Ölçüm | `run.log(name, val)`                                             | Azure Machine Learning Portal Kullanıcı arabirimi             | [Denemeleri izleme](how-to-track-experiments.md#available-metrics-to-track)<br>[azureml. Core. Run sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=experimental)                                                                                                                                                 |
| Python yazdırma/günlüğe kaydetme    | Günlük    | `print(val)`<br>`logging.info(message)`                          | Sürücü günlükleri, Azure Machine Learning Tasarımcısı | [Denemeleri izleme](how-to-track-experiments.md#available-metrics-to-track)<br><br>[Python günlüğü](https://docs.python.org/2/library/logging.html)                                                                                                                                                                       |
| OpenCensus Python          | Günlük    | `logger.addHandler(AzureLogHandler())`<br>`logging.log(message)` | Application Insights-izlemeler                | [Application Insights’ta işlem hatlarında hata ayıklama](how-to-debug-pipelines-application-insights.md)<br><br>[OpenCensus Azure İzleyici Dışarı Aktarıcıları](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)<br>[Python günlüğü tanıtım rehberi](https://docs.python.org/3/howto/logging-cookbook.html) |

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

## <a name="debug-and-troubleshoot-in-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısında hata ayıklama ve sorun giderme (Önizleme)

Bu bölümde, tasarımcıda işlem hatlarında sorun giderme hakkında genel bakış sunulmaktadır. Tasarımcıda oluşturulan işlem hatları için, **70_driver_log** dosyasını yazma sayfasında veya işlem hattı çalıştırma ayrıntısı sayfasında bulabilirsiniz.

### <a name="get-logs-from-the-authoring-page"></a>Yazma sayfasından günlükleri al

Bir işlem hattı çalıştırması gönderdiğinizde ve yazma sayfasında kaladığınızda, her modül çalışmayı bitirdiğinde her modül için oluşturulan günlük dosyalarını bulabilirsiniz.

1. Yazma tuvalinde çalışmayı tamamlamış bir modül seçin.
1. Modülün sağ bölmesinde, **çıktılar + Günlükler** sekmesine gidin.
1. Sağ bölmeyi genişletin ve dosyayı tarayıcıda görüntülemek için **70_driver_log. txt** ' yi seçin. Günlükleri yerel olarak da indirebilirsiniz.

    ![Tasarımcıda genişletilmiş çıkış bölmesi](./media/how-to-debug-pipelines/designer-logs.png)

### <a name="get-logs-from-pipeline-runs"></a>İşlem hattı çalıştırmalarından günlükleri al

Ayrıca, Studio 'nun işlem **hatları** veya **denemeleri** bölümünde bulunan işlem hattı çalışma ayrıntısı sayfasında belirli çalıştırmalar için günlük dosyalarını bulabilirsiniz.

1. Tasarımcıda oluşturulan bir işlem hattı çalıştırması seçin.

    ![İşlem hattı çalıştırma sayfası](./media/how-to-debug-pipelines/designer-pipelines.png)

1. Önizleme bölmesinde bir modül seçin.
1. Modülün sağ bölmesinde, **çıktılar + Günlükler** sekmesine gidin.
1. Sağ bölmeyi genişleterek **70_driver_log. txt** dosyasını tarayıcıda görüntüleyin veya günlükleri yerel olarak indirmek için dosyayı seçin.

> [!IMPORTANT]
> İşlem hattı çalıştırma ayrıntıları sayfasından bir işlem hattını güncelleştirmek için, işlem hattı çalıştırmasını yeni bir işlem hattı taslağında **kopyalamanız** gerekir. İşlem hattı çalıştırması, işlem hattının anlık görüntüsüdür. Bir günlük dosyasına benzer ve değiştirilemez. 

## <a name="debug-and-troubleshoot-in-application-insights"></a>Application Insights hata ayıklama ve sorun giderme
OpenCensus Python kitaplığını bu şekilde kullanma hakkında daha fazla bilgi için şu kılavuza bakın: [Application Insights Machine Learning işlem hatlarında hata ayıklama ve sorun giderme](how-to-debug-pipelines-application-insights.md)

## <a name="debug-and-troubleshoot-in-visual-studio-code"></a>Visual Studio Code hata ayıklama ve sorun giderme

Bazı durumlarda, ML ardışık düzeninde kullanılan Python kodunda etkileşimli olarak hata ayıklaması yapmanız gerekebilir. Visual Studio Code (VS Code) ve Visual Studio için Python Araçları (PTVSD) kullanarak, eğitim ortamında çalışırken koda ekleyebilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

* __Azure sanal ağını__kullanmak üzere yapılandırılmış bir __Azure Machine Learning çalışma alanı__ .
* İşlem hattı adımlarının bir parçası olarak Python betikleri kullanan bir __Azure Machine Learning işlem hattı__ . Örneğin, bir PythonScriptStep.
* __Sanal ağdaki__ bir Azure Machine Learning işlem kümesi ve __eğitim için işlem hattı tarafından kullanılır__.
* __Sanal ağdaki__bir __geliştirme ortamı__ . Geliştirme ortamı aşağıdakilerden biri olabilir:

    * Sanal ağdaki bir Azure sanal makinesi
    * Sanal ağdaki bir not defteri VM 'sinin Işlem örneği
    * Sanal bir özel ağ (VPN) tarafından sanal ağa bağlı bir istemci makinesi.

Azure Machine Learning ile bir Azure sanal ağı kullanma hakkında daha fazla bilgi için bkz. Azure [sanal ağ Içindeki güvenli Azure ML deneme ve çıkarım işleri](how-to-enable-virtual-network.md).

### <a name="how-it-works"></a>Nasıl çalışır?

ML işlem hattı adımlarınız Python betikleri çalıştırır. Bu betikler aşağıdaki eylemleri gerçekleştirecek şekilde değiştirilir:
    
1. Çalıştıkları konağın IP adresini günlüğe kaydedin. Hata ayıklayıcıyı betiğe bağlamak için IP adresini kullanın.

2. PTVSD hata ayıklama bileşenini başlatın ve bir hata ayıklayıcının bağlanmasını bekleyin.

3. Geliştirme ortamınızdan, komut dosyasının çalıştığı IP adresini bulmak için eğitim süreci tarafından oluşturulan günlükleri izleyebilirsiniz.

4. Bir dosya kullanarak hata ayıklayıcıyı bağlamak için IP adresine VS Code söylemiş olursunuz `launch.json` .

5. Hata ayıklayıcıyı ekler ve komut dosyasında etkileşimli olarak ilereolursunuz.

### <a name="configure-python-scripts"></a>Python betikleri yapılandırma

Hata ayıklamayı etkinleştirmek için, ML işlem hattınızdaki adımlar tarafından kullanılan Python betiklerinde aşağıdaki değişiklikleri yapın:

1. Aşağıdaki içeri aktarma deyimlerini ekleyin:

    ```python
    import ptvsd
    import socket
    from azureml.core import Run
    ```

1. Aşağıdaki bağımsız değişkenleri ekleyin. Bu bağımsız değişkenler, gereken şekilde hata ayıklayıcıyı etkinleştirmenizi ve hata ayıklayıcıyı iliştirmek için zaman aşımını ayarlamanıza olanak tanır:

    ```python
    parser.add_argument('--remote_debug', action='store_true')
    parser.add_argument('--remote_debug_connection_timeout', type=int,
                    default=300,
                    help=f'Defines how much time the Azure ML compute target '
                    f'will await a connection from a debugger client (VSCODE).')
    ```

1. Aşağıdaki deyimleri ekleyin. Bu deyimler, kodun çalıştığı düğümün IP adresini günlüğe kaydetmek için geçerli çalışma bağlamını yükler:

    ```python
    global run
    run = Run.get_context()
    ```

1. `if`PTVSD ' i başlatan ve bir hata ayıklayıcının iliştirmesini bekleyen bir ifade ekleyin. Zaman aşımından önce bir hata ayıklayıcı yoksa, komut dosyası normal olarak devam eder.

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

Aşağıdaki Python örneği, `train.py` hata ayıklamayı sağlayan temel bir dosya göstermektedir:

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

### <a name="configure-ml-pipeline"></a>ML ardışık düzenini yapılandırma

PTVSD başlatmak ve çalıştırma bağlamını almak için gereken Python paketlerini sağlamak için bir ortam oluşturun ve ayarlayın `pip_packages=['ptvsd', 'azureml-sdk==1.0.83']` . SDK sürümünü, kullanmakta olduğunuz bir sürümle eşleşecek şekilde değiştirin. Aşağıdaki kod parçacığı bir ortamın nasıl oluşturulacağını gösterir:

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

[Python betikleri Yapılandır](#configure-python-scripts) bölümünde, ml ardışık düzen adımlarınız tarafından kullanılan betiklerin iki yeni bağımsız değişkeni eklenmiştir. Aşağıdaki kod parçacığı, bileşen için hata ayıklamayı etkinleştirmek ve zaman aşımı ayarlamak için bu bağımsız değişkenlerin nasıl kullanılacağını gösterir. Ayrıca, daha önce oluşturulan ortamın şu şekilde nasıl kullanılacağını gösterir `runconfig=run_config` :

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

İşlem hattı çalıştırıldığında, her adım bir alt çalıştırma oluşturur. Hata ayıklama etkinleştirilirse, değiştirilen betik, alt çalışma için içindeki aşağıdaki metne benzer bilgileri günlüğe kaydeder `70_driver_log.txt` :

```text
Timeout for debug connection: 300
ip_address: 10.3.0.5
```

Değeri kaydedin `ip_address` . Sonraki bölümde kullanılır.

> [!TIP]
> Bu işlem hattı adımı için alt çalıştırma için çalıştırma günlüklerinden IP adresini de bulabilirsiniz. Bu bilgileri görüntüleme hakkında daha fazla bilgi için bkz. [Azure ML deneme çalıştırmaları ve ölçümlerini izleme](how-to-track-experiments.md).

### <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

1. VS Code geliştirme ortamınıza Visual Studio için Python Araçları (PTVSD) yüklemek için şu komutu kullanın:

    ```
    python -m pip install --upgrade ptvsd
    ```

    VS Code ile PTVSD kullanma hakkında daha fazla bilgi için bkz. [Uzaktan hata ayıklama](https://code.visualstudio.com/docs/python/debugging#_remote-debugging).

1. Hata ayıklayıcıyı çalıştıran Azure Machine Learning işlem ile iletişim kurmak üzere VS Code yapılandırmak için yeni bir hata ayıklama yapılandırması oluşturun:

    1. VS Code, __Hata Ayıkla__ menüsünü ve ardından __yapılandırma aç__' ı seçin. __Launch. JSON__ adlı bir dosya açılır.

    1. __Launch. JSON__ dosyasında, içeren satırı bulun `"configurations": [` ve sonra aşağıdaki metni ekleyin. Girişi, `"host": "10.3.0.5"` önceki bölümden günlüklerinizin DÖNDÜRDÜĞÜ IP adresiyle değiştirin. Girişi, `"localRoot": "${workspaceFolder}/code/step"` hata ayıklanan betiğin bir kopyasını içeren bir yerel dizin olarak değiştirin:

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
        > Konfigürasyonlar bölümünde zaten başka girdiler varsa, eklediğiniz koddan sonra bir virgül (,) ekleyin.

        > [!TIP]
        > En iyi uygulama, örnek değerin neden başvurduğu, farklı dizinlerde komut dosyaları için kaynakları tutmaktır `localRoot` `/code/step1` .
        >
        > Birden çok betikte hata ayıklaması yapıyorsanız, farklı dizinlerde, her komut dosyası için ayrı bir yapılandırma bölümü oluşturun.

    1. __Launch. JSON__ dosyasını kaydedin.

### <a name="connect-the-debugger"></a>Hata ayıklayıcıyı bağlama

1. VS Code açın ve betiğin yerel bir kopyasını açın.
2. Komut dosyasının iliştirdikten sonra durdurulmasını istediğiniz kesme noktalarını ayarlayın.
3. Alt işlem betiği çalıştırırken ve `Timeout for debug connection` günlüklerde görüntülenirken, F5 tuşunu veya __Hata Ayıkla__' yı seçin. İstendiğinde, __Azure Machine Learning işlem: uzaktan hata ayıklama__ yapılandırması ' nı seçin. Ayrıca, yan çubukta hata ayıklama simgesini, Azure Machine Learning: uzaktan hata ayıklama açılan menüsünden __Uzaktan hata ayıklama__ girişini seçebilir ve ardından hata ayıklayıcıyı eklemek için yeşil oku kullanabilirsiniz.

    Bu noktada, VS Code işlem düğümündeki PTVSD öğesine bağlanır ve daha önce ayarladığınız kesme noktasında durmaktadır. Artık kodu çalışırken, değişkenleri görüntülerken vb. adımları izleyebilirsiniz.

    > [!NOTE]
    > Günlükte belirten bir giriş görüntüleniyorsa `Debugger attached = False` , zaman aşımı süresi doldu ve betik hata ayıklayıcı olmadan devam eder. İşlem hattını yeniden gönder ve `Timeout for debug connection` iletiden sonra, zaman aşımı süresi dolmadan önce hata ayıklayıcıyı bağla.

## <a name="next-steps"></a>Sonraki adımlar

* [Azureml işlem hatları-Core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) paketi ve [azureml-işlem hatları-adımlar](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) PAKETIYLE ilgili yardım için SDK başvurusuna bakın.

* [Tasarımcı özel durumları ve hata kodları](algorithm-module-reference/designer-error-codes.md)listesine bakın.
