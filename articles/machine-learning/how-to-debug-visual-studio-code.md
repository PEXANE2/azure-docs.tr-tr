---
title: Visual Studio Code ile etkileşimli hata ayıklama
titleSuffix: Azure Machine Learning
description: Visual Studio Code kullanarak etkileşimli olarak hata ayıklama Azure Machine Learning kod, ardışık düzen ve dağıtım
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: luisquintanilla
ms.author: luquinta
ms.date: 08/06/2020
ms.openlocfilehash: 08859eb87431b59e8be68369ce8f3876aa7d9f21
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90530310"
---
# <a name="interactive-debugging-with-visual-studio-code"></a>Visual Studio Code ile etkileşimli hata ayıklama

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Visual Studio Code (VS Code) ve [depugpy](https://github.com/microsoft/debugpy/)kullanarak Azure Machine Learning işlem hatları ve dağıtımlara etkileşimli olarak hata ayıklama yapmayı öğrenin.

## <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Makine öğrenmesi işlem hatlarında hata ayıklama ve sorun giderme

Bazı durumlarda, ML ardışık düzeninde kullanılan Python kodunda etkileşimli olarak hata ayıklaması yapmanız gerekebilir. VS Code ve hata ayıklama GPY kullanarak, eğitim ortamında çalışırken koda ekleyebilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

* __Azure sanal ağını__kullanmak üzere yapılandırılmış bir __Azure Machine Learning çalışma alanı__ .
* İşlem hattı adımlarının bir parçası olarak Python betikleri kullanan bir __Azure Machine Learning işlem hattı__ . Örneğin, bir PythonScriptStep.
* __Sanal ağdaki__ bir Azure Machine Learning işlem kümesi ve __eğitim için işlem hattı tarafından kullanılır__.
* __Sanal ağdaki__bir __geliştirme ortamı__ . Geliştirme ortamı aşağıdakilerden biri olabilir:

  * Sanal ağdaki bir Azure sanal makinesi
  * Sanal ağdaki bir not defteri VM 'sinin Işlem örneği
  * VPN ya da ExpressRoute aracılığıyla sanal ağa özel ağ bağlantısı olan bir istemci makinesi.

Azure Machine Learning ile bir Azure sanal ağı kullanma hakkında daha fazla bilgi için bkz. [sanal ağ yalıtımı ve gizliliğe genel bakış](how-to-network-security-overview.md).

> [!TIP]
> Bir sanal ağın arkasında olmayan Azure Machine Learning kaynaklarıyla çalışabilseniz de sanal ağ kullanılması önerilir.

### <a name="how-it-works"></a>Nasıl çalışır?

ML işlem hattı adımlarınız Python betikleri çalıştırır. Bu betikler aşağıdaki eylemleri gerçekleştirecek şekilde değiştirilir:

1. Çalıştıkları konağın IP adresini günlüğe kaydedin. Hata ayıklayıcıyı betiğe bağlamak için IP adresini kullanın.

2. Hata ayıklayıcı GPY hata ayıklama bileşenini başlatın ve bir hata ayıklayıcının bağlanmasını bekleyin.

3. Geliştirme ortamınızdan, komut dosyasının çalıştığı IP adresini bulmak için eğitim süreci tarafından oluşturulan günlükleri izleyebilirsiniz.

4. Bir dosya kullanarak hata ayıklayıcıyı bağlamak için IP adresine VS Code söylemiş olursunuz `launch.json` .

5. Hata ayıklayıcıyı ekler ve komut dosyasında etkileşimli olarak ilereolursunuz.

### <a name="configure-python-scripts"></a>Python betikleri yapılandırma

Hata ayıklamayı etkinleştirmek için, ML işlem hattınızdaki adımlar tarafından kullanılan Python betiklerinde aşağıdaki değişiklikleri yapın:

1. Aşağıdaki içeri aktarma deyimlerini ekleyin:

    ```python
    import argparse
    import os
    import debugpy
    import socket
    from azureml.core import Run
    ```

1. Aşağıdaki bağımsız değişkenleri ekleyin. Bu bağımsız değişkenler, gereken şekilde hata ayıklayıcıyı etkinleştirmenizi ve hata ayıklayıcıyı iliştirmek için zaman aşımını ayarlamanıza olanak tanır:

    ```python
    parser.add_argument('--remote_debug', action='store_true')
    parser.add_argument('--remote_debug_connection_timeout', type=int,
                        default=300,
                        help=f'Defines how much time the AML compute target '
                        f'will await a connection from a debugger client (VSCODE).')
    parser.add_argument('--remote_debug_client_ip', type=str,
                        help=f'Defines IP Address of VS Code client')
    parser.add_argument('--remote_debug_port', type=int,
                        default=5678,
                        help=f'Defines Port of VS Code client')
    ```

1. Aşağıdaki deyimleri ekleyin. Bu deyimler, kodun çalıştığı düğümün IP adresini günlüğe kaydetmek için geçerli çalışma bağlamını yükler:

    ```python
    global run
    run = Run.get_context()
    ```

1. `if`Hata ayıklayıcı GPY 'yi Başlatan bir ifade ekleyin ve bir hata ayıklayıcının iliştirmesini bekler. Zaman aşımından önce bir hata ayıklayıcı yoksa, komut dosyası normal olarak devam eder. `HOST`Ve `PORT` değerlerini `listen` kendi değerlerinizle değiştirdiğinizden emin olun.

    ```python
    if args.remote_debug:
        print(f'Timeout for debug connection: {args.remote_debug_connection_timeout}')
        # Log the IP and port
        try:
            ip = args.remote_debug_client_ip
        except:
            print("Need to supply IP address for VS Code client")
        print(f'ip_address: {ip}')
        debugpy.listen(address=(ip, args.remote_debug_port))
        # Wait for the timeout for debugger to attach
        debugpy.wait_for_client()
        print(f'Debugger attached = {debugpy.is_client_connected()}')
    ```

Aşağıdaki Python örneği, `train.py` hata ayıklamayı sağlayan temel bir dosya göstermektedir:

```python
# Copyright (c) Microsoft. All rights reserved.
# Licensed under the MIT license.

import argparse
import os
import debugpy
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
parser.add_argument('--remote_debug_client_ip', type=str,
                    help=f'Defines IP Address of VS Code client')
parser.add_argument('--remote_debug_port', type=int,
                    default=5678,
                    help=f'Defines Port of VS Code client')

# Get run object, so we can find and log the IP of the host instance
global run
run = Run.get_context()

args = parser.parse_args()

# Start debugger if remote_debug is enabled
if args.remote_debug:
    print(f'Timeout for debug connection: {args.remote_debug_connection_timeout}')
    # Log the IP and port
    # ip = socket.gethostbyname(socket.gethostname())
    try:
        ip = args.remote_debug_client_ip
    except:
        print("Need to supply IP address for VS Code client")
    print(f'ip_address: {ip}')
    debugpy.listen(address=(ip, args.remote_debug_port))
    # Wait for the timeout for debugger to attach
    debugpy.wait_for_client()
    print(f'Debugger attached = {debugpy.is_client_connected()}')

print("Argument 1: %s" % args.input_data)
print("Argument 2: %s" % args.output_train)

if not (args.output_train is None):
    os.makedirs(args.output_train, exist_ok=True)
    print("%s created" % args.output_train)
```

### <a name="configure-ml-pipeline"></a>ML ardışık düzenini yapılandırma

Hata ayıklama GPY 'yi başlatmak için gereken Python paketlerini sağlamak ve çalıştırma bağlamını almak için bir ortam oluşturun ve ayarlayın `pip_packages=['debugpy', 'azureml-sdk==<SDK-VERSION>']` . SDK sürümünü, kullanmakta olduğunuz bir sürümle eşleşecek şekilde değiştirin. Aşağıdaki kod parçacığı bir ortamın nasıl oluşturulacağını gösterir:

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
                                                                           pip_packages=['debugpy', 'azureml-sdk==<SDK-VERSION>'])
```

[Python betikleri Yapılandır](#configure-python-scripts) bölümünde, ml ardışık düzen adımlarınız tarafından kullanılan betiklerin yeni bağımsız değişkenleri eklenmiştir. Aşağıdaki kod parçacığı, bileşen için hata ayıklamayı etkinleştirmek ve zaman aşımı ayarlamak için bu bağımsız değişkenlerin nasıl kullanılacağını gösterir. Ayrıca, daha önce oluşturulan ortamın şu şekilde nasıl kullanılacağını gösterir `runconfig=run_config` :

```python
# Use RunConfig from a pipeline step
step1 = PythonScriptStep(name="train_step",
                         script_name="train.py",
                         arguments=['--remote_debug', '--remote_debug_connection_timeout', 300,'--remote_debug_client_ip','<VS-CODE-CLIENT-IP>','--remote_debug_port',5678],
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

1. VS Code geliştirme ortamınıza hata ayıklama GPY yüklemek için aşağıdaki komutu kullanın:

    ```bash
    python -m pip install --upgrade debugpy
    ```

    VS Code ile hata ayıklama GPY kullanma hakkında daha fazla bilgi için bkz. [Uzaktan hata ayıklama](https://code.visualstudio.com/docs/python/debugging#_debugging-by-attaching-over-a-network-connection).

1. Hata ayıklayıcıyı çalıştıran Azure Machine Learning işlem ile iletişim kurmak üzere VS Code yapılandırmak için yeni bir hata ayıklama yapılandırması oluşturun:

    1. VS Code, __Hata Ayıkla__ menüsünü ve ardından __yapılandırma aç__' ı seçin. __launch.js__ adlı bir dosya açılır.

    1. Dosyadaki __launch.js__ , içeren satırı bulun `"configurations": [` ve sonra aşağıdaki metni ekleyin. Girişi, `"host": "<IP-ADDRESS>"` önceki bölümden günlüklerinizin DÖNDÜRDÜĞÜ IP adresiyle değiştirin. Girişi, `"localRoot": "${workspaceFolder}/code/step"` hata ayıklanan betiğin bir kopyasını içeren bir yerel dizin olarak değiştirin:

        ```json
        {
            "name": "Azure Machine Learning Compute: remote debug",
            "type": "python",
            "request": "attach",
            "port": 5678,
            "host": "<IP-ADDRESS>",
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
        > Özellikle işlem hatları için en iyi yöntem, kodun yalnızca adımların her biri için uygun olması için, kaynakların ayrı dizinlerde betikleri tutmaktır. Bu örnekte `localRoot` örnek değer başvuruları vardır `/code/step1` .
        >
        > Birden çok betikte hata ayıklaması yapıyorsanız, farklı dizinlerde, her komut dosyası için ayrı bir yapılandırma bölümü oluşturun.

    1. __launch.js__ dosyaya kaydedin.

### <a name="connect-the-debugger"></a>Hata ayıklayıcıyı bağlama

1. VS Code açın ve betiğin yerel bir kopyasını açın.
2. Komut dosyasının iliştirdikten sonra durdurulmasını istediğiniz kesme noktalarını ayarlayın.
3. Alt işlem betiği çalıştırırken ve `Timeout for debug connection` günlüklerde görüntülenirken, F5 tuşunu veya __Hata Ayıkla__' yı seçin. İstendiğinde, __Azure Machine Learning işlem: uzaktan hata ayıklama__ yapılandırması ' nı seçin. Ayrıca, yan çubukta hata ayıklama simgesini, Azure Machine Learning: uzaktan hata ayıklama açılan menüsünden __Uzaktan hata ayıklama__ girişini seçebilir ve ardından hata ayıklayıcıyı eklemek için yeşil oku kullanabilirsiniz.

    Bu noktada, VS Code işlem düğümündeki hata ayıklama GPY 'ye bağlanır ve daha önce ayarladığınız kesme noktasında durmaktadır. Artık kodu çalışırken, değişkenleri görüntülerken vb. adımları izleyebilirsiniz.

    > [!NOTE]
    > Günlükte belirten bir giriş görüntüleniyorsa `Debugger attached = False` , zaman aşımı süresi doldu ve betik hata ayıklayıcı olmadan devam eder. İşlem hattını yeniden gönder ve `Timeout for debug connection` iletiden sonra, zaman aşımı süresi dolmadan önce hata ayıklayıcıyı bağla.

## <a name="debug-and-troubleshoot-deployments"></a>Hata ayıklama ve Dağıtımda sorun giderme

Bazı durumlarda, model dağıtımınızda bulunan Python kodunda etkileşimli olarak hata ayıklaması yapmanız gerekebilir. Örneğin, giriş betiği başarısız olursa ve neden ek günlüğe kaydetme ile saptanamaz. VS Code ve hata ayıklama GPY kullanarak, Docker kapsayıcısının içinde çalışan koda iliştirebilirsiniz.

> [!IMPORTANT]
> Bu hata ayıklama yöntemi, `Model.deploy()` `LocalWebservice.deploy_configuration` bir modeli yerel olarak dağıtırken ve kullanılırken çalışmaz. Bunun yerine, [model. Package ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#&preserve-view=truepackage-workspace--models--inference-config-none--generate-dockerfile-false-) yöntemini kullanarak bir görüntü oluşturmanız gerekir.

Yerel Web hizmeti dağıtımları, yerel sisteminizde çalışan bir Docker yüklemesi gerektirir. Docker kullanma hakkında daha fazla bilgi için [Docker belgelerine](https://docs.docker.com/)bakın. İşlem örnekleriyle çalışırken Docker 'ın zaten yüklü olduğunu unutmayın.

### <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

1. Yerel VS Code geliştirme ortamınıza hata ayıklama GPY yüklemek için şu komutu kullanın:

    ```bash
    python -m pip install --upgrade debugpy
    ```

    VS Code ile hata ayıklama GPY kullanma hakkında daha fazla bilgi için bkz. [Uzaktan hata ayıklama](https://code.visualstudio.com/docs/python/debugging#_debugging-by-attaching-over-a-network-connection).

1. Docker görüntüsüyle iletişim kurmak üzere VS Code yapılandırmak için yeni bir hata ayıklama yapılandırması oluşturun:

    1. VS Code, __Hata Ayıkla__ menüsünü ve ardından __yapılandırma aç__' ı seçin. __launch.js__ adlı bir dosya açılır.

    1. Dosyadaki __launch.js__ , içeren satırı bulun `"configurations": [` ve sonra aşağıdaki metni ekleyin:

        ```json
        {
            "name": "Azure Machine Learning Deployment: Docker Debug",
            "type": "python",
            "request": "attach",
            "connect": {
                "port": 5678,
                "host": "0.0.0.0",
            },
            "pathMappings": [
                {
                    "localRoot": "${workspaceFolder}",
                    "remoteRoot": "/var/azureml-app"
                }
            ]
        }
        ```

        > [!IMPORTANT]
        > Konfigürasyonlar bölümünde zaten başka girdiler varsa, eklediğiniz koddan sonra bir virgül (,) ekleyin.

        Bu bölüm, 5678 numaralı bağlantı noktasını kullanarak Docker kapsayıcısına ekler.

    1. __launch.js__ dosyaya kaydedin.

### <a name="create-an-image-that-includes-debugpy"></a>Hata ayıklama GPY içeren bir görüntü oluşturma

1. Dağıtımınız için Conda ortamını değiştirerek, hata ayıklama GPY 'yi içerir. Aşağıdaki örnek, parametresini kullanarak eklemeyi gösterir `pip_packages` :

    ```python
    from azureml.core.conda_dependencies import CondaDependencies 


    # Usually a good idea to choose specific version numbers
    # so training is made on same packages as scoring
    myenv = CondaDependencies.create(conda_packages=['numpy==1.15.4',
                                'scikit-learn==0.19.1', 'pandas==0.23.4'],
                                 pip_packages = ['azureml-defaults==1.0.83', 'debugpy'])

    with open("myenv.yml","w") as f:
        f.write(myenv.serialize_to_string())
    ```

1. Hata ayıklama GPY 'yi başlatmak ve hizmet başladığında bir bağlantı beklemek için, dosyanızın en üstüne aşağıdakileri ekleyin `score.py` :

    ```python
    import debugpy
    # Allows other computers to attach to debugpy on this IP address and port.
    debugpy.listen(('0.0.0.0', 5678))
    # Wait 30 seconds for a debugger to attach. If none attaches, the script continues as normal.
    debugpy.wait_for_client()
    print("Debugger attached...")
    ```

1. Ortam tanımına dayalı bir görüntü oluşturun ve görüntüyü yerel kayıt defterine çekin. 

    > [!NOTE]
    > Bu örnek `ws` , Azure Machine Learning çalışma alanınıza işaret ettiğini ve bu `model` modelin dağıtılmakta olduğunu varsayar. `myenv.yml`Dosya, 1. adımda oluşturulan Conda bağımlılıklarını içerir.

    ```python
    from azureml.core.conda_dependencies import CondaDependencies
    from azureml.core.model import InferenceConfig
    from azureml.core.environment import Environment


    myenv = Environment.from_conda_specification(name="env", file_path="myenv.yml")
    myenv.docker.base_image = None
    myenv.docker.base_dockerfile = "FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04"
    inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
    package = Model.package(ws, [model], inference_config)
    package.wait_for_creation(show_output=True)  # Or show_output=False to hide the Docker build logs.
    package.pull()
    ```

    Görüntü oluşturulup indirildikten sonra, görüntü yolu (Bu örnekte de kendi Özeti olan depo, ad ve etiket dahil) aşağıdakine benzer bir iletide görüntülenir:

    ```text
    Status: Downloaded newer image for myregistry.azurecr.io/package@sha256:<image-digest>
    ```

1. Görüntüyle çalışmayı kolaylaştırmak için, bir etiket eklemek üzere aşağıdaki komutu kullanın. `myimagepath`Önceki adımdaki konum değeriyle değiştirin.

    ```bash
    docker tag myimagepath debug:1
    ```

    Adımlar geri kalanında, `debug:1` tam görüntü yolu değeri yerine yerel görüntüye başvurabilirsiniz.

### <a name="debug-the-service"></a>Hizmette hata ayıkla

> [!TIP]
> Dosyadaki hata ayıklayıcı GPY bağlantısı için bir zaman aşımı ayarlarsanız `score.py` , zaman aşımı süresi dolmadan önce vs Code hata ayıklama oturumuna bağlamanız gerekir. VS Code başlatın, yerel kopyasını açın `score.py` , bir kesme noktası ayarlayın ve bu bölümdeki adımları kullanmadan önce başlamaya hazırlanın.
>
> Hata ayıklama ve kesme noktaları ayarlama hakkında daha fazla bilgi için bkz. [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging).

1. Görüntüyü kullanarak bir Docker kapsayıcısı başlatmak için aşağıdaki komutu kullanın:

    ```bash
    docker run -it --name debug -p 8000:5001 -p 5678:5678 -v <my_path_to_score.py>:/var/azureml-apps/score.py debug:1 /bin/bash
    ```

    Bu `score.py` , yerel olarak kapsayıcıdaki birine ekler. Bu nedenle, düzenleyicide yapılan tüm değişiklikler otomatik olarak kapsayıcıya yansıtılır.

1. Kapsayıcının içinde, kabukta aşağıdaki komutu çalıştırın

    ```bash
    runsvdir /var/runit
    ```

1. Kapsayıcı içinde hata ayıklayıcı GPY 'ye VS Code iliştirmek için, VS Code açın ve F5 tuşunu kullanın veya __Hata Ayıkla__' yı seçin. İstendiğinde, __Azure Machine Learning dağıtım: Docker hata ayıklama__ yapılandırması ' nı seçin. Ayrıca, hata ayıklama açılan menüsünden __Azure Machine Learning Deployment: Docker hata ayıklama__ girişi olan yan çubukta hata ayıklama simgesini seçebilir ve ardından hata ayıklayıcıyı eklemek için yeşil oku kullanabilirsiniz.

    ![Hata ayıklama simgesi, hata ayıklamayı Başlat düğmesi ve yapılandırma Seçicisi](./media/how-to-troubleshoot-deployment/start-debugging.png)

Bu noktada, VS Code Docker kapsayıcısının içinde hata ayıklama GPY 'ye bağlanır ve daha önce ayarladığınız kesme noktasında durdurulur. Artık kodu çalışırken, değişkenleri görüntülerken vb. adımları izleyebilirsiniz.

Python 'da hata ayıklamak için VS Code kullanma hakkında daha fazla bilgi için bkz. [Python kodunuzda hata ayıklama](https://docs.microsoft.com/visualstudio/python/debugging-python-in-visual-studio?view=vs-2019).

### <a name="stop-the-container"></a>Kapsayıcıyı durdurma

Kapsayıcıyı durdurmak için aşağıdaki komutu kullanın:

```bash
docker stop debug
```

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code uzak olarak ayarladığınıza göre, kodunuzda etkileşimli olarak hata ayıklamak için Visual Studio Code uzaktan işlem olarak bir işlem örneği kullanabilirsiniz. 

[Öğretici: Ilk ml modelinize eğitme](tutorial-1st-experiment-sdk-train.md) bir işlem örneğinin tümleşik bir not defteriyle nasıl kullanılacağını gösterir.
