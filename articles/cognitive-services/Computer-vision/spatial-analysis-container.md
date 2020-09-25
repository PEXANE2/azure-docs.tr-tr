---
title: Uzamsal analiz kapsayıcısını yüklemek ve çalıştırmak-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Uzamsal analiz kapsayıcısı, kişileri ve uzaklıkları tespit etmenizi sağlar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: aahi
ms.openlocfilehash: 52df2ad0dc4c60c24e341a9765e31bcf9776bf5e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91277300"
---
# <a name="install-and-run-the-spatial-analysis-container-preview"></a>Uzamsal analiz kapsayıcısını (Önizleme) yükleyip çalıştırın

Uzamsal analiz kapsayıcısı, kişiler, taşınanlar ve fiziksel ortamlardaki nesnelerle etkileşimler arasındaki uzamsal ilişkileri anlamak için gerçek zamanlı akış videosunu analiz etmenizi sağlar. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için çok kullanışlıdır.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir görüntü işleme kaynağı oluşturun görüntü işleme bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uzamsal analiz kapsayıcısını çalıştırmak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra kullanacaksınız.


### <a name="spatial-analysis-container-requirements"></a>Uzamsal analiz kapsayıcısı gereksinimleri

Uzamsal analiz kapsayıcısını çalıştırmak için, [NVIDIA Tesla T4 GPU 'su](https://www.nvidia.com/en-us/data-center/tesla-t4/)olan bir işlem cihazına ihtiyacınız vardır. GPU hızlandırmalı [Azure Stack Edge](https://azure.microsoft.com/products/azure-stack/edge/) kullanmanızı öneririz, ancak kapsayıcı, en düşük gereksinimleri karşılayan diğer masaüstü makineler üzerinde çalışır. Bu cihaza ana bilgisayar olarak başvuracağız.

#### <a name="azure-stack-edge-device"></a>[Azure Stack Edge cihazı](#tab/azure-stack-edge)

Azure Stack Edge, ağ veri aktarımı özelliklerine sahip bir hizmet olarak donanım ve bir AI özellikli sınır bilgi işlem aygıtıdır. Ayrıntılı hazırlık ve kurulum yönergeleri için [Azure Stack Edge belgelerine](https://docs.microsoft.com/azure/databox-online/azure-stack-edge-deploy-prep)bakın.

#### <a name="desktop-machine"></a>[Masaüstü makinesi](#tab/desktop-machine)

#### <a name="minimum-hardware-requirements"></a>En düşük donanım gereksinimleri

* 4 GB sistem RAM 'i
* 4 GB GPU RAM
* 8 çekirdekli CPU
* 1 NVıDıA Tesla T4 GPU
* 20 GB HDD alanı

#### <a name="recommended-hardware"></a>Önerilen donanım

* 32 GB sistem RAM 'i
* 16 GB GPU RAM
* 8 çekirdekli CPU
* 2 NVıDıA Tesla T4 GPU 'ları
* 50 GB SSD alanı

Bu makalede, aşağıdaki yazılım paketlerini indirip yükleyeceksiniz. Ana bilgisayar, şunları çalıştırabilmelidir (yönergeler için aşağıya bakın):

* [NVIDIA grafik sürücüleri](https://docs.nvidia.com/datacenter/tesla/tesla-installation-notes/index.html) ve [NVIDIA CUDA araç seti](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
* [NVıDıA MPS](https://docs.nvidia.com/deploy/pdf/CUDA_Multi_Process_Service_Overview.pdf) için yapılandırma (çok işlem hizmeti).
* [Docker CE](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-engine---community-1) ve [NVIDIA-Docker2](https://github.com/NVIDIA/nvidia-docker) 
* [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) çalışma zamanı.

---

| Gereksinim | Açıklama |
|--|--|
| Kamera | Uzamsal analiz kapsayıcısı belirli bir kamera markasına bağlı değil. Kamera cihazının şunları yapması gerekir: gerçek zamanlı akış protokolü (RTSP) ve H. ıseencoding 'i desteklemek, ana bilgisayar için erişilebilir olması ve 15 fps ve 1080p çözünürlüğünde akış yapabilen bir cihaz olabilir. |
| Linux işletim sistemi | [Ubuntu Desktop 18,04 LTS](http://releases.ubuntu.com/18.04/) , ana bilgisayarda yüklü olmalıdır.  |


## <a name="request-approval-to-run-the-container"></a>Kapsayıcıyı çalıştırmak için onay isteyin

Kapsayıcıyı çalıştırmak için onay istemek üzere [istek formunu](https://aka.ms/csgate) doldurun ve iletin.

Form, siz, şirketiniz ve kapsayıcısını kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Formu gönderdikten sonra, Azure bilişsel hizmetler ekibi bunu gözden geçirir ve bir karar vererek size e-posta ile gönderilir.

> [!IMPORTANT]
> * Formunda, bir Azure aboneliği KIMLIĞIYLE ilişkili bir e-posta adresi kullanmanız gerekir.
> * Kapsayıcıyı çalıştırmak için kullandığınız Görüntü İşleme kaynak, onaylanan Azure abonelik KIMLIĞIYLE oluşturulmuş olmalıdır.

Onaylandıktan sonra, makalenin ilerleyen kısımlarında açıklanan Microsoft Container Registry (MCR) dosyasından indirdikten sonra kapsayıcıyı çalıştırabileceksiniz.

Azure aboneliğiniz onaylanmamışsa kapsayıcıyı çalıştıramayacağız.

## <a name="set-up-the-host-computer"></a>Ana bilgisayarı ayarlama

Ana bilgisayarınız için bir Azure Stack Edge cihazı kullanmanız önerilir. Farklı bir cihaz yapılandırıyorsanız **Masaüstü makine** ' ye tıklayın.

#### <a name="azure-stack-edge-device"></a>[Azure Stack Edge cihazı](#tab/azure-stack-edge)

### <a name="configure-compute-on-the-azure-stack-edge-portal"></a>Azure Stack Edge portalında işlem yapılandırma 
 
Uzamsal analiz, bir AI çözümünü çalıştırmak için Azure Stack ucunun işlem özelliklerini kullanır. İşlem özelliklerini etkinleştirmek için aşağıdakileri yaptığınızdan emin olun: 

* Azure Stack Edge cihazınızı [bağladınız ve etkinleştirdiniz](https://docs.microsoft.com/azure/databox-online/azure-stack-edge-deploy-connect-setup-activate) . 
* Cihaza erişmek için PowerShell 5,0 veya sonraki bir sürümünü çalıştıran bir Windows istemci sisteminiz vardır.  
* Bir Kubernetes kümesi dağıtmak için, Azure Stack Edge cihazınızı [Azure Portal](https://portal.azure.com/) **Yerel Kullanıcı arabirimi** aracılığıyla yapılandırmanız gerekir: 
  1. Azure Stack Edge cihazınızda işlem özelliğini etkinleştirin. İşlem özelliğini etkinleştirmek için, cihazınızın web arabirimindeki **işlem** sayfasına gidin. 
  2. İşlem için etkinleştirmek istediğiniz bir ağ arabirimi seçin ve ardından **Etkinleştir**' e tıklayın. Bu, cihazınızda bu ağ arabiriminde bir sanal anahtar oluşturur.
  3. Kubernetes sınama düğümü IP adreslerini ve Kubernetes dış hizmetler IP adreslerini boş bırakın.
  4. **Uygula**’ya tıklayın. Bu işlem, iki dakika sürebilir. 

![İşlem yapılandırma](media/spatial-analysis/configure-compute.png)

### <a name="set-up-an-edge-compute-role-and-create-an-iot-hub-resource"></a>Bir uç işlem rolü ayarlama ve bir IoT Hub kaynağı oluşturma

[Azure Portal](https://portal.azure.com/), Azure Stack Edge kaynağına gidin. **Genel bakış** sayfasında veya gezinti listesinde **Edge COMPUTE Başlarken** düğmesine tıklayın.  **Uç Işlem yapılandırma**   kutucuğunda **Yapılandır**' ı tıklatın. 

![Bağlantı](media/spatial-analysis/configure-edge-compute-tile.png)

 **Edge Işlem yapılandırma**   sayfasında, mevcut bir IoT Hub seçin veya yenisini oluşturmayı seçin. Varsayılan olarak, bir IoT Hub kaynağı oluşturmak için standart (S1) Fiyatlandırma Katmanı kullanılır. Boş bir katman IoT Hub kullanmak için, bir tane oluşturun ve ardından seçin. IoT Hub kaynak, Azure Stack Edge kaynağı tarafından kullanılan aynı abonelik ve kaynak grubunu kullanır 

**Oluştur**’a tıklayın. IoT Hub kaynak oluşturma birkaç dakika sürebilir. IoT Hub kaynağı oluşturulduktan sonra, yeni yapılandırmayı göstermek için **Edge işlem bölmesini Yapılandır** ' ı güncellecektir. Edge işlem rolünün yapılandırıldığını doğrulamak için, **Işlem yapılandırma**kutucuğunda yapılandırmayı **görüntüle** ' yi seçin   .

Edge cihazında Edge hesaplama rolü ayarlandığında, iki cihaz oluşturur: bir IoT cihazı ve bir IoT Edge cihaz. Her iki cihaz de IoT Hub kaynağında görüntülenebilir. Azure IoT Edge çalışma zamanı IoT Edge cihazda zaten çalışıyor.            

> [!NOTE]
> * Şu anda yalnızca Linux platformu IoT Edge cihazlar için desteklenir. Azure Stack Edge cihazında sorun giderme konusunda yardım için bkz. [günlük ve sorun giderme](spatial-analysis-logging.md) makalesi.
> * Bir proxy sunucusu üzerinden iletişim kurmak üzere bir IoT Edge cihazının nasıl yapılandırılacağı hakkında daha fazla bilgi edinmek için bkz. bir [IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](https://docs.microsoft.com/azure/iot-edge/how-to-configure-proxy-support#azure-portal)

###  <a name="enable-mps-on-azure-stack-edge"></a>Azure Stack Edge üzerinde MPS 'yi etkinleştirme 

1. Yönetici olarak bir Windows PowerShell oturumu çalıştırın. 

2. Windows Uzaktan Yönetimi hizmetinin istemciniz üzerinde çalıştığından emin olun. PowerShell terminalinde aşağıdaki komutu kullanın 
    
    ```powershell
    winrm quickconfig
    ```
    
    Bir güvenlik duvarı özel durumu hakkında uyarı görürseniz, ağ bağlantı türünü denetleyin ve [Windows Uzaktan Yönetimi](https://docs.microsoft.com/windows/win32/winrm/installation-and-configuration-for-windows-remote-management) belgelerine bakın.

3. Cihaz IP adresine bir değişken atayın. 
    
    ```powershell
    $ip = "" Replace with the IP address of your device. 
    ```
    
4. Cihazınızın IP adresini istemcinin güvenilen konaklar listesine eklemek için aşağıdaki komutu kullanın: 
    
    ```powershell
    Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force 
    ```

5. Cihazda bir Windows PowerShell oturumu başlatın. 

    ```powershell
    Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell 
    ```

6. İstendiğinde parolayı girin. Yerel Web Kullanıcı arabiriminde oturum açmak için kullanılan parolayı kullanın. Varsayılan yerel Web UI parolası `Password1` .

`Start-HcsGpuMPS`CIHAZDA MPS hizmetini başlatmak için yazın. 

Azure Stack Edge cihazında sorun giderme konusunda yardım için bkz [. Azure Stack Edge cihazında sorun giderme](spatial-analysis-logging.md#troubleshooting-the-azure-stack-edge-device) 

#### <a name="desktop-machine"></a>[Masaüstü makinesi](#tab/desktop-machine)

Ana bilgisayarınız Azure Stack Edge aygıtı değilse bu yönergeleri izleyin.

#### <a name="install-nvidia-cuda-toolkit-and-nvidia-graphics-drivers-on-the-host-computer"></a>Ana bilgisayara NVıDıA CUDA araç seti ve NVIDIA grafik sürücülerini yükler

Gerekli NVIDIA grafik sürücülerini ve CUDA araç setini yüklemek için aşağıdaki Bash betiğini kullanın.

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
sudo add-apt-repository "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/ /"
sudo apt-get update
sudo apt-get -y install cuda
```

Makineyi yeniden başlatın ve aşağıdaki komutu çalıştırın.

```bash
nvidia-smi
```

Aşağıdaki çıkışı görmelisiniz.

![NVıDıA sürücü çıkışı](media/spatial-analysis/nvidia-driver-output.png)

### <a name="install-docker-ce-and-nvidia-docker2-on-the-host-computer"></a>Ana bilgisayara Docker CE ve NVIDIA-docker2 'yi yükler

Ana bilgisayara Docker CE 'yi yükler.

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

*NVIDIA-Docker-2* yazılım paketini yükler.

```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
sudo apt-get install -y docker-ce nvidia-docker2
sudo systemctl restart docker
```

## <a name="enable-nvidia-mps-on-the-host-computer"></a>Ana bilgisayarda NVıDıA MPS 'yi etkinleştirme

> [!TIP]
> * GPU işlem özelliği 7. x (Volta öncesi) değerinden küçükse MPS 'yi yüklemeyin. Başvuru için bkz. [CUDA compatability](https://docs.nvidia.com/deploy/cuda-compatibility/index.html#support-title) . 
> * Ana bilgisayardaki bir terminal penceresinden MPS yönergelerini çalıştırın. Docker kapsayıcı örneğinizin içinde değil.

En iyi performans ve kullanım için, konak bilgisayarın GPU 'ları [NVIDIA Multiprocess hizmeti (MPS)](https://docs.nvidia.com/deploy/pdf/CUDA_Multi_Process_Service_Overview.pdf)için yapılandırın. Ana bilgisayardaki bir terminal penceresinden MPS yönergelerini çalıştırın.

```bash
# Set GPU(s) compute mode to EXCLUSIVE_PROCESS
sudo nvidia-smi --compute-mode=EXCLUSIVE_PROCESS

# Cronjob for setting GPU(s) compute mode to EXCLUSIVE_PROCESS on boot
echo "SHELL=/bin/bash" > /tmp/nvidia-mps-cronjob
echo "PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin" >> /tmp/nvidia-mps-cronjob
echo "@reboot   root    nvidia-smi --compute-mode=EXCLUSIVE_PROCESS" >> /tmp/nvidia-mps-cronjob

sudo chown root:root /tmp/nvidia-mps-cronjob
sudo mv /tmp/nvidia-mps-cronjob /etc/cron.d/

# Service entry for automatically starting MPS control daemon
echo "[Unit]" > /tmp/nvidia-mps.service
echo "Description=NVIDIA MPS control service" >> /tmp/nvidia-mps.service
echo "After=cron.service" >> /tmp/nvidia-mps.service
echo "" >> /tmp/nvidia-mps.service
echo "[Service]" >> /tmp/nvidia-mps.service
echo "Restart=on-failure" >> /tmp/nvidia-mps.service
echo "ExecStart=/usr/bin/nvidia-cuda-mps-control -f" >> /tmp/nvidia-mps.service
echo "" >> /tmp/nvidia-mps.service
echo "[Install]" >> /tmp/nvidia-mps.service
echo "WantedBy=multi-user.target" >> /tmp/nvidia-mps.service

sudo chown root:root /tmp/nvidia-mps.service
sudo mv /tmp/nvidia-mps.service /etc/systemd/system/

sudo systemctl --now enable nvidia-mps.service
```

## <a name="configure-azure-iot-edge-on-the-host-computer"></a>Ana bilgisayarda Azure IoT Edge yapılandırma

Uzamsal analiz kapsayıcısını ana bilgisayara dağıtmak için standart (S1) veya ücretsiz (F0) fiyatlandırma katmanını kullanarak bir [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal) hizmeti örneği oluşturun. Ana bilgisayarınız Azure Stack bir kenar ise, Azure Stack Edge kaynağı tarafından kullanılan aynı abonelik ve kaynak grubunu kullanın.

Azure IoT Hub 'nin bir örneğini oluşturmak için Azure CLı 'yi kullanın. Parametreleri uygun yerlerde değiştirin. Alternatif olarak, Azure IoT Hub [Azure Portal](https://portal.azure.com/)oluşturabilirsiniz.

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
az login
az account set --subscription <name or ID of Azure Subscription>
az group create --name "test-resource-group" --location "WestUS"

az iot hub create --name "test-iot-hub-123" --sku S1 --resource-group "test-resource-group"

az iot hub device-identity create --hub-name "test-iot-hub-123" --device-id "my-edge-device" --edge-enabled
```

Ana bilgisayar Azure Stack Edge aygıtı değilse, [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) sürüm 1.0.8 yüklemeniz gerekir. Doğru sürümü indirmek için şu adımları izleyin:

Ubuntu Server 18,04:
```bash
curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
```

Oluşturulan listeyi kopyalayın.
```bash
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
```

Microsoft GPG ortak anahtarını yükler.

```bash
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

Cihazınızdaki paket listelerini güncelleştirin.

```bash
sudo apt-get update
```

1.0.8 sürümünü yükler:

```bash
sudo apt-get install iotedge=1.0.8* libiothsm-std=1.0.8*
```

Sonra, bir [bağlantı dizesi](https://docs.microsoft.com/azure/iot-edge/how-to-register-device#register-in-the-azure-portal)kullanarak ana bilgisayarı IoT Hub örneğiniz IoT Edge bir cihaz olarak kaydedin.

IoT Edge cihazını Azure IoT Hub bağlamanız gerekir. Bağlantı dizesini daha önce oluşturduğunuz IoT Edge cihazdan kopyalamanız gerekir. Alternatif olarak, Azure CLı 'de aşağıdaki komutu çalıştırabilirsiniz.

```bash
az iot hub device-identity show-connection-string --device-id my-edge-device --hub-name test-iot-hub-123
```

Ana bilgisayarda  `/etc/iotedge/config.yaml` düzenlenmek üzere açın. `ADD DEVICE CONNECTION STRING HERE`Bağlantı dizesiyle değiştirin. Dosyayı kaydedin ve kapatın. Ana bilgisayarda IoT Edge hizmetini yeniden başlatmak için bu komutu çalıştırın.

```bash
sudo systemctl restart iotedge
```

Uzamsal analiz kapsayıcısını, [Azure Portal](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal) veya [Azure CLI](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-cli)'Dan ana bilgisayar üzerinde IoT modülü olarak dağıtın. Portalı kullanıyorsanız, görüntü URI 'sini Azure Container Registry konumuna ayarlayın. 

Azure CLı kullanarak kapsayıcıyı dağıtmak için aşağıdaki adımları kullanın.

---

### <a name="iot-deployment-manifest"></a>IoT dağıtım bildirimi

Birden çok konak bilgisayarında kapsayıcı dağıtımını kolaylaştırmak için, kapsayıcı oluşturma seçeneklerini ve ortam değişkenlerini belirtmek üzere bir dağıtım bildirim dosyası oluşturabilirsiniz. [GitHub üzerinde bir dağıtım bildirimi](https://go.microsoft.com/fwlink/?linkid=2142179)örneği bulabilirsiniz.

Aşağıdaki tabloda IoT Edge modülü tarafından kullanılan çeşitli ortam değişkenleri gösterilmektedir. Ayrıca, içindeki özniteliğini kullanarak bunları yukarıda bağlı dağıtım bildiriminde da ayarlayabilirsiniz `env` `spatialanalysis` :

| Ayar Adı | Değer | Açıklama|
|---------|---------|---------|
| ARCHON_LOG_LEVEL | Bilgisine Seçeneini | Günlüğe kaydetme düzeyi, iki değerden birini seçin|
| ARCHON_SHARED_BUFFER_LIMIT | 377487360 | Değiştirme|
| ARCHON_PERF_MARKER| yanlış| Performans günlüğü için bunu true olarak ayarlayın, aksi takdirde bu değer yanlış olmalıdır| 
| ARCHON_NODES_LOG_LEVEL | Bilgisine Seçeneini | Günlüğe kaydetme düzeyi, iki değerden birini seçin|
| OMP_WAIT_POLICY | ETKIN | Değiştirme|
| QT_X11_NO_MITSHM | 1 | Değiştirme|
| API_KEY | API anahtarınız| Bu değeri Görüntü İşleme kaynağınızın Azure portal toplayın. Bunu, kaynağınız için **anahtar ve uç nokta** bölümünde bulabilirsiniz. |
| BILLING_ENDPOINT | Uç nokta URI 'niz| Bu değeri Görüntü İşleme kaynağınızın Azure portal toplayın. Bunu, kaynağınız için **anahtar ve uç nokta** bölümünde bulabilirsiniz.|
| SÖZLEŞMESI | ettiğinizde | Kapsayıcının çalışması için bu değerin *kabul edileceği* şekilde ayarlanması gerekir |
| GÖRÜNTÜLENME | : 1 | Bu değerin, ana bilgisayardaki çıkışıyla aynı olması gerekir `echo $DISPLAY` . Azure Stack Edge cihazlarının bir ekranı yoktur. Bu ayar geçerli değil|


> [!IMPORTANT]
> `Eula` `Billing` `ApiKey` Kapsayıcıyı çalıştırmak için, ve seçenekleri belirtilmelidir; Aksi takdirde kapsayıcı başlatılmaz.  Daha fazla bilgi için bkz. [faturalandırma](#billing).

Dosyadaki örnek [DeploymentManifest.js](https://go.microsoft.com/fwlink/?linkid=2142179) , kendi ayarlarınızı ve işlem seçiminizle güncelleştirdikten sonra, kapsayıcıyı ana bilgisayarda bir IoT Edge modülü olarak dağıtmak Için AŞAĞıDAKI [Azure CLI](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-cli) komutunu kullanabilirsiniz.

```azurecli
az login
az extension add --name azure-iot
az iot edge set-modules --hub-name "<IoT Hub name>" --device-id "<IoT Edge device name>" --content DeploymentManifest.json -–subscription "<subscriptionId>"
```

|Parametre  |Açıklama  |
|---------|---------|
| `--deployment-id` | Dağıtım için yeni bir ad. |
| `--hub-name` | Azure IoT Hub adınız. |
| `--content` | Dağıtım dosyasının adı. |
| `--target-condition` | Ana bilgisayar için IoT Edge cihaz adı. |
| `-–subscription` | Abonelik KIMLIĞI veya adı. |

Bu komut dağıtımı başlatacak. Dağıtım durumunu görmek için Azure portal Azure IoT Hub örneğinizin sayfasına gidin. Durum 417 olarak gösterilebilir: cihaz kapsayıcı görüntülerini indirene ve çalışmaya başladıktan sonra *cihazın dağıtım yapılandırması ayarlı değildir* .

## <a name="validate-that-the-deployment-is-successful"></a>Dağıtımın başarılı olduğunu doğrulama

Kapsayıcının çalıştığını doğrulamak için birkaç yol vardır. Azure portal Azure IoT Hub örneğinizin uzamsal analiz modülünün **IoT Edge modülü ayarları** ' nda *çalışma zamanı durumunu* bulun. *Çalışma zamanı durumu* Için **Istenen değerin** ve **bildirilen değerin** *çalıştığını*doğrulayın.

![Örnek dağıtım doğrulaması](./media/spatial-analysis/deployment-verification.png)

Dağıtım tamamlandıktan ve kapsayıcı çalışmaya başladıktan sonra **ana bilgisayar** Azure IoT Hub olay göndermeye başlar. `.debug`İşlemlerin sürümünü kullandıysanız, dağıtım bildiriminde yapılandırdığınız her kamera için bir Görselleştirici penceresi görürsünüz. Artık dağıtım bildiriminde izlemek istediğiniz satırları ve bölgeleri tanımlayabilir ve yeniden dağıtmak için yönergeleri izleyebilirsiniz. 

## <a name="configure-the-operations-performed-by-spatial-analysis"></a>Uzamsal analiz tarafından gerçekleştirilen işlemleri yapılandırma

Kapsayıcıyı bağlı kameraları kullanacak şekilde yapılandırmak, işlemleri yapılandırmak ve daha fazlasını yapmak için [uzamsal analiz işlemleri](spatial-analysis-operations.md) kullanmanız gerekir. Yapılandırdığınız her kamera cihazında, uzamsal analize yönelik işlemler, Azure IoT Hub örneğinizle gönderilen JSON iletilerinin çıktı akışını oluşturacaktır.

## <a name="redeploy-or-delete-the-deployment"></a>Dağıtımı yeniden dağıtma veya silme

Dağıtımı güncelleştirmeniz gerekiyorsa, önceki dağıtımlarınızın başarılı bir şekilde dağıtıldığından emin olun veya tamamlanmamış IoT Edge cihaz dağıtımlarını silmeniz gerekir. Aksi takdirde, bu dağıtımlar devam eder ve sistem hatalı durumda bırakılır. Azure portal veya [Azure CLI](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment)'yi kullanabilirsiniz.

## <a name="use-the-output-generated-by-the-container"></a>Kapsayıcı tarafından oluşturulan çıktıyı kullanın

Kapsayıcı tarafından oluşturulan çıktıyı tüketmeye başlamak istiyorsanız aşağıdaki makalelere bakın:

*   Azure IoT Hub uç noktasına bağlanmak ve olayları almak için seçtiğiniz programlama diline yönelik Azure Event hub SDK 'sını kullanın. Daha fazla bilgi için bkz. [yerleşik uç noktadan cihazdan buluta Iletileri okuma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin) . 
*   Azure IoT Hub, olayları diğer uç noktalara göndermek veya olayları Azure Blob depolama alanına kaydetmek için Ileti yönlendirme ayarlayın, vb. Daha fazla bilgi için bkz. [IoT Hub Ileti yönlendirme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c) . 

## <a name="running-spatial-analysis-with-a-recorded-video-file"></a>Kayıtlı bir video dosyası ile uzamsal analizler çalıştırma

Hem kayıtlı hem de canlı video ile uzamsal analizler kullanabilirsiniz. Kayıtlı video için uzamsal analizler kullanmak üzere bir video dosyası kaydetmeyi ve bir MP4 dosyası olarak kaydetmeyi deneyin. Azure 'da bir BLOB depolama hesabı oluşturun veya mevcut bir tane kullanın. Ardından Azure portal aşağıdaki BLOB depolama ayarlarını güncelleştirin:
    1. **Güvenli aktarımı** **devre dışı** olarak değiştirme
    2. **BLOB genel erişimine Izin ver** özelliğinin **etkin** olarak değiştirilmesi

**Kapsayıcı** bölümüne gidin ve yeni bir kapsayıcı oluşturun ya da var olan bir kapsayıcıyı kullanın. Ardından, video dosyasını kapsayıcıya yükleyin. Karşıya yüklenen dosyanın dosya ayarlarını genişletin ve **SAS oluştur**' u seçin. **Süre sonu tarihini** , test dönemini kapsayacak kadar uzun bir süre ayarladığınızdan emin olun. **Izin verilen protokolleri** *http* olarak ayarlama (*https* desteklenmez).

**SAS belirteci oluştur ve URL** 'yi tıklatın ve BLOB SAS URL 'sini kopyalayın. Başlangıcını `https` `http` ve video oynatmayı destekleyen bir tarayıcıda URL 'yi test etme ve test etme ile değiştirin.

`VIDEO_URL` [Dağıtım bildiriminde](https://go.microsoft.com/fwlink/?linkid=2142179) , tüm GRAFIKLER için oluşturduğunuz URL ile değiştirin. `VIDEO_IS_LIVE`Olarak ayarlayın `false` ve ardından, güncelleştirilmiş bildirimle uzamsal analiz kapsayıcısını yeniden dağıtın. Aşağıdaki örneğe bakın.

Uzamsal analiz modülü video dosyasını tüketmeye başlayacak ve sürekli olarak otomatik olarak yeniden oynacaktır.


```json
"zonecrossing": {
  "operationId" : "cognitiveservices.vision.spatialanalysis-personcrossingpolygon",
  "version": 1,
  "enabled": true,
  "parameters": {
      "VIDEO_URL": "Replace http url here",
      "VIDEO_SOURCE_ID": "personcountgraph",
      "VIDEO_IS_LIVE": false,
        "VIDEO_DECODE_GPU_INDEX": 0,
      "DETECTOR_NODE_CONFIG": "{ \"gpu_index\": 0 }",
      "SPACEANALYTICS_CONFIG": "{\"zones\":[{\"name\":\"queue\",\"polygon\":[[0.3,0.3],[0.3,0.9],[0.6,0.9],[0.6,0.3],[0.3,0.3]], \"threshold\":35.0}]}"
    }
  },

```

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı başlatırken veya çalıştırırken sorunlarla karşılaşırsanız, genel sorunların adımları için bkz. [telemetri ve sorun giderme](spatial-analysis-logging.md) . Bu makale ayrıca günlükleri oluşturma ve toplama ve sistem durumunu toplama hakkındaki bilgileri de içerir.

## <a name="billing"></a>Faturalandırma

Uzamsal analiz kapsayıcısı Azure hesabınızdaki bir Görüntü İşleme kaynağı kullanarak Azure 'a faturalandırma bilgilerini gönderir. Genel önizlemede uzamsal analiz kullanımı Şu anda ücretsizdir. 

Azure bilişsel hizmetler kapsayıcıları, ölçüm/faturalandırma uç noktasına bağlı kalmadan çalıştırılmak üzere lisanslanmaz. Her zaman Faturalandırma bitiş noktasıyla faturalandırma bilgilerini iletmek için kapsayıcıları etkinleştirmeniz gerekir. Bilişsel hizmetler kapsayıcıları, Microsoft 'a, analiz edilen video veya resim gibi müşteri verileri göndermez.


## <a name="summary"></a>Özet

Bu makalede, uzamsal analiz kapsayıcısını indirmek, yüklemek ve çalıştırmak için kavramların ve iş akışının öğrenmiş olursunuz. Özet:

* Uzamsal analiz, Docker için bir Linux kapsayıcısıdır.
* Kapsayıcı görüntüleri Microsoft Container Registry indirilir.
* Kapsayıcı görüntüleri Azure IoT Edge ' de IoT modülleri olarak çalışır.
* Kapsayıcıyı yapılandırma ve bir konak makinesine dağıtma.

## <a name="next-steps"></a>Sonraki adımlar

* [Web uygulaması sayımını yapan bir kişi dağıtın](spatial-analysis-web-app.md)
* [Uzamsal analiz işlemlerini yapılandırma](spatial-analysis-operations.md)
* [Günlüğe kaydetme ve sorun giderme](spatial-analysis-logging.md)
* [Kamera Yerleştirme Kılavuzu](spatial-analysis-camera-placement.md)
* [Bölge ve satır yerleştirme Kılavuzu](spatial-analysis-zone-line-placement.md)
