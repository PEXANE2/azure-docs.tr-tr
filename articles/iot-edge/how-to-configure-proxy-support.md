---
title: Cihazları ağ proxy 'leri için Yapılandırma-Azure IoT Edge | Microsoft Docs
description: Azure IoT Edge çalışma zamanını ve internet 'e yönelik IoT Edge modüllerini bir ara sunucu üzerinden iletişim kuracak şekilde yapılandırma.
author: kgremban
ms.author: kgremban
ms.date: 09/03/2020
ms.topic: how-to
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- contperfq1
ms.openlocfilehash: e6c85ba79c21c9a8120feebc02477506eb93d2e5
ms.sourcegitcommit: 206629373b7c2246e909297d69f4fe3728446af5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2020
ms.locfileid: "89500377"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>IoT Edge cihazını ara sunucu üzerinden iletişim kuracak şekilde yapılandırma

IoT Edge cihazlar, IoT Hub iletişim kurmak için HTTPS istekleri gönderir. Cihazınız bir ara sunucu kullanan bir ağa bağlıysa, IoT Edge çalışma zamanını sunucu üzerinden iletişim kuracak şekilde yapılandırmanız gerekir. Proxy sunucular Ayrıca, IoT Edge hub 'ı üzerinden yönlendirilmeyen HTTP veya HTTPS isteklerini IoT Edge bağımsız modülleri etkileyebilir.

Bu makalede, bir proxy sunucusunun arkasında bir IoT Edge cihazını yapılandırmak ve yönetmek için aşağıdaki dört adım izlenecek yol gösterilmektedir:

1. [**IoT Edge çalışma zamanını cihazınıza yüklemesi**](#install-the-runtime-through-a-proxy)

   IoT Edge yükleme betikleri, paketleri ve dosyaları internet 'ten çeker, böylece cihazınızın bu istekleri yapması için proxy sunucusu üzerinden iletişim kurması gerekir. Windows cihazlarında, yükleme betiği de [çevrimdışı bir yükleme](how-to-install-iot-edge-windows.md#offline-or-specific-version-installation) seçeneği sağlar.

   Bu adım, ilk kez ayarladığınızda IoT Edge cihazı yapılandırmak için tek seferlik bir işlemdir. IoT Edge çalışma zamanını güncelleştirdiğinizde aynı bağlantılar da gereklidir.

2. [**Cihazınızda Docker Daemon ve IoT Edge Daemon 'ı yapılandırma**](#configure-the-daemons)

   IoT Edge, cihazda iki Daemon 'ları kullanır ve bunların her ikisi de proxy sunucusu üzerinden Web istekleri yapması gerekir. IoT Edge Daemon, IoT Hub iletişiminden sorumludur. Moby arka plan programı kapsayıcı yönetiminden sorumludur, bu nedenle kapsayıcı kayıt defterleri ile iletişim kurar.

   Bu adım, ilk kez ayarladığınızda IoT Edge cihazı yapılandırmak için tek seferlik bir işlemdir.

3. [**Cihazınızdaki config. YAML dosyasında IoT Edge Aracısı özelliklerini yapılandırın**](#configure-the-iot-edge-agent)

   IoT Edge Daemon, başlangıçta edgeAgent modülünü başlatır. Ardından, edgeAgent modülü IoT Hub dağıtım bildirimini alır ve diğer tüm modülleri başlatır. IoT Edge aracısının ilk bağlantıyı IoT Hub hale getirmek için, edgeAgent modülü ortam değişkenlerini cihazın kendisinde el ile yapılandırın. İlk bağlantıdan sonra, edgeAgent modülünü uzaktan yapılandırabilirsiniz.

   Bu adım, ilk kez ayarladığınızda IoT Edge cihazı yapılandırmak için tek seferlik bir işlemdir.

4. [**Tüm gelecek modül dağıtımları için, ara sunucu üzerinden iletişim kuran herhangi bir modülün ortam değişkenlerini ayarlayın**](#configure-deployment-manifests)

   IoT Edge cihazınız kurulduktan ve proxy sunucusu üzerinden IoT Hub bağlandıktan sonra, bağlantıyı gelecekteki tüm modül dağıtımlarında korumanız gerekir.

   Bu adım, her yeni modül veya dağıtım güncelleştirmesinin, cihazın ara sunucu üzerinden iletişim kurma yeteneğini sakladığı şekilde uzaktan yapılan bir işlemdir.

## <a name="know-your-proxy-url"></a>Proxy URL 'nizi öğrenin

Bu makaledeki adımlardan herhangi birine başlamadan önce, proxy URL 'nizi bilmeniz gerekir.

Proxy URL 'Leri şu biçimi alır: **protokol**://**proxy_host**:**proxy_port**.

* **Protokol** http ya da https olur. Docker Daemon, kapsayıcı kayıt defteri ayarlarınıza bağlı olarak her iki protokolü de kullanabilir, ancak IoT Edge Daemon ve Runtime kapsayıcıları, proxy 'ye bağlanmak için her zaman HTTP 'yi kullanmalıdır.

* **Proxy_host** , proxy sunucu için bir adrestir. Proxy sunucunuz kimlik doğrulaması gerektiriyorsa, kimlik bilgilerinizi proxy konağının bir parçası olarak aşağıdaki biçimde sağlayabilirsiniz: **User**:**Password** \@ **proxy_host**.

* **Proxy_port** , proxy 'nin ağ trafiğine verdiği ağ bağlantı noktasıdır.

## <a name="install-the-runtime-through-a-proxy"></a>Çalışma zamanını bir ara sunucu aracılığıyla yükler

IoT Edge cihazınızın Windows veya Linux üzerinde çalışıp çalışmadığını, yükleme paketlerine proxy sunucusu üzerinden erişmeniz gerekir. İşletim sisteminize bağlı olarak, IoT Edge çalışma zamanını bir ara sunucuya yüklemek için adımları izleyin.

### <a name="linux-devices"></a>Linux cihazları

IoT Edge çalışma zamanını bir Linux cihazına yüklüyorsanız, paket yöneticisini, yükleme paketine erişmek için proxy sunucunuzun üzerinden gezinmek üzere yapılandırın. Örneğin, [bir http-proxy kullanmak için apt-get](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy)' i ayarlayın. Paket yöneticiniz yapılandırıldıktan sonra, [Linux üzerinde Azure IoT Edge Runtime 'ı](how-to-install-iot-edge-linux.md) her zamanki gibi Install ' daki yönergeleri izleyin.

### <a name="windows-devices"></a>Windows cihazları

IoT Edge çalışma zamanını bir Windows cihazına yüklüyorsanız, proxy sunucusu üzerinden iki kez gitmeniz gerekir. İlk bağlantı yükleyici betik dosyasını indirir ve ikinci bağlantı yükleme sırasında gerekli bileşenleri indirmek için kullanılır. Windows ayarları 'nda ara sunucu bilgilerini yapılandırabilir veya proxy bilgilerinizi doğrudan PowerShell komutlarına dahil edebilirsiniz.

Aşağıdaki adımlarda, bağımsız değişkenini kullanarak bir Windows yüklemesinin örneği gösterilmektedir `-proxy` :

1. Invoke-WebRequest komutu, yükleyici betiğine erişmek için ara sunucu bilgilerine sahip olmalıdır. Ardından, dağıtım-ıotedge komutunun yükleme dosyalarını indirmesi için proxy bilgilerine ihtiyacı vardır.

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Deploy-IoTEdge -proxy <proxy URL>
   ```

2. Initialize-ıotedge komutunun ara sunucu üzerinden gitmesi gerekmez, bu nedenle ikinci adım yalnızca Invoke-WebRequest için proxy bilgileri gerektirir.

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Initialize-IoTEdge
   ```

Proxy sunucusu için URL 'ye dahil olmayan karmaşık kimlik bilgileriniz varsa, `-ProxyCredential` içindeki parametresini kullanın `-InvokeWebRequestParameters` . Örneğin,

```powershell
$proxyCredential = (Get-Credential).GetNetworkCredential()
. {Invoke-WebRequest -proxy <proxy URL> -ProxyCredential $proxyCredential -useb aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -InvokeWebRequestParameters @{ '-Proxy' = '<proxy URL>'; '-ProxyCredential' = $proxyCredential }
```

Ara sunucu parametreleri hakkında daha fazla bilgi için bkz. [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest). Çevrimdışı yükleme dahil Windows yükleme seçenekleri hakkında daha fazla bilgi için bkz. [Windows üzerinde Azure IoT Edge çalışma zamanı yükleme](how-to-install-iot-edge-windows.md).

## <a name="configure-the-daemons"></a>Daemon 'ları yapılandırma

IoT Edge, IoT Edge cihazında çalışan iki Daemon 'ları dayanır. Moby cini, Web isteklerini kapsayıcı kayıt defterlerinden kapsayıcı görüntüleri çekmek için yapar. IoT Edge Daemon, Web isteklerinin IoT Hub iletişim kurmasını sağlar.

Hem Moby hem de IoT Edge Daemon 'ları, devam eden cihaz işlevselliği için proxy sunucusunu kullanacak şekilde yapılandırılmalıdır. Bu adım ilk cihaz kurulumu sırasında IoT Edge cihazında gerçekleşir.

### <a name="moby-daemon"></a>Moby Daemon

Moby Docker üzerine inşa edildiğinden, Moby cini ortam değişkenleriyle yapılandırmak için Docker belgelerine bakın. Çoğu kapsayıcı kayıt defterleri (DockerHub ve Azure Container Registry 'ler dahil) HTTPS isteklerini destekler, bu nedenle ayarlamanız gereken parametre **https_proxy**. Aktarım Katmanı Güvenliği 'ni (TLS) desteklemeyen bir kayıt defterindeki görüntüleri çekyorsanız, **http_proxy** parametresini ayarlamanız gerekir.

IoT Edge cihaz işletim sisteminiz için geçerli olan makaleyi seçin:

* [Linux 'Ta Docker Daemon 'ı yapılandırma](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy) Linux cihazlarındaki Moby Daemon, Docker adını tutar.
* [Windows 'Da Docker Daemon yapılandırma](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon#proxy-configuration) Windows cihazlarındaki Moby Daemon, ıotedge-Moby olarak adlandırılır. Bir Windows cihazında hem Docker Desktop hem de Moby paralel olarak çalıştırılabilmesini sağladığından adlar farklıdır.

### <a name="iot-edge-daemon"></a>IoT Edge Daemon

IoT Edge Daemon, Moby cinine benzer bir şekilde yapılandırılmıştır. İşletim sisteminize bağlı olarak hizmet için bir ortam değişkeni ayarlamak için aşağıdaki adımları kullanın.

IoT Edge Daemon, istekleri IoT Hub göndermek için her zaman HTTPS kullanır.

#### <a name="linux"></a>Linux

IoT Edge Daemon 'ı yapılandırmak için terminalde bir düzenleyici açın.

```bash
sudo systemctl edit iotedge
```

Aşağıdaki metni girerek **\<proxy URL>** proxy sunucu adresiniz ve bağlantı noktasıyla değiştirin. Sonra Kaydet ve çık.

```ini
[Service]
Environment="https_proxy=<proxy URL>"
```

IoT Edge için yeni yapılandırmayı seçmek üzere Service Manager 'ı yenileyin.

```bash
sudo systemctl daemon-reload
```

Değişikliklerin etkili olması için IoT Edge yeniden başlatın.

```bash
sudo systemctl restart iotedge
```

Ortam değişkeninizin oluşturulduğunu ve yeni yapılandırmanın yüklendiğini doğrulayın.

```bash
systemctl show --property=Environment iotedge
```

#### <a name="windows"></a>Windows

Yönetici olarak bir PowerShell penceresi açın ve yeni ortam değişkeniyle kayıt defterini düzenlemek için aşağıdaki komutu çalıştırın. **\<proxy url>** Ara sunucu adresiniz ve bağlantı noktasıyla değiştirin.

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Services\iotedge /v Environment /t REG_MULTI_SZ /d https_proxy=<proxy URL>
```

Değişikliklerin etkili olması için IoT Edge yeniden başlatın.

```powershell
Restart-Service iotedge
```

## <a name="configure-the-iot-edge-agent"></a>IoT Edge aracısını yapılandırma

IoT Edge Aracısı, herhangi bir IoT Edge cihazında başlatılacak ilk modüldür. IoT Edge config. YAML dosyasındaki bilgilere dayalı olarak ilk kez başlatılır. IoT Edge Aracısı daha sonra dağıtım bildirimlerini almak için IoT Hub bağlanır, bu da cihaza hangi modüllerin dağıtılması gerektiğini bildirir.

Bu adım ilk cihaz kurulumu sırasında IoT Edge cihaza bir kez gerçekleşir.

1. IoT Edge cihazınızda config. YAML dosyasını açın. Linux sistemlerinde, bu dosya **/etc/iotedge/config.exe**yolunda bulunur. Windows sistemlerinde bu dosya **C:\programdata\iotedge\config.exe**yolunda bulunur. Yapılandırma dosyası korunuyor, bu nedenle ona erişmek için yönetim ayrıcalıklarına sahip olmanız gerekir. Linux sistemlerinde, `sudo` dosyayı tercih ettiğiniz metin düzenleyicisinde açmadan önce komutunu kullanın. Windows 'ta, Not Defteri gibi bir metin düzenleyicisini yönetici olarak açın ve dosyayı açın.

2. Config. YAML dosyasında **Edge Aracısı modülü belirtimi** bölümünü bulun. IoT Edge Aracısı tanımı, ortam değişkenleri ekleyebileceğiniz bir **env** parametresi içerir.

3. Env parametresi için yer tutucular olan süslü ayraçları kaldırın ve yeni değişkeni yeni bir satıra ekleyin. YAML 'deki girintiler iki boşluk olduğunu unutmayın.

   ```yaml
   https_proxy: "<proxy URL>"
   ```

4. IoT Edge Runtime, IoT Hub konuşmak için AMQP 'yi varsayılan olarak kullanır. Bazı proxy sunucuları AMQP bağlantı noktalarını engeller. Bu durumda, Ayrıca, edgeAgent 'ı WebSocket üzerinden AMQP kullanacak şekilde yapılandırmanız gerekir. İkinci bir ortam değişkeni ekleyin.

   ```yaml
   UpstreamProtocol: "AmqpWs"
   ```

   ![ortam değişkenleriyle edgeAgent tanımı](./media/how-to-configure-proxy-support/edgeagent-edited.png)

5. Config. YAML içindeki değişiklikleri kaydedin ve düzenleyiciyi kapatın. Değişikliklerin etkili olması için IoT Edge yeniden başlatın.

   * Linux:

      ```bash
      sudo systemctl restart iotedge
      ```

   * Windows:

      ```powershell
      Restart-Service iotedge
      ```

## <a name="configure-deployment-manifests"></a>Dağıtım bildirimlerini yapılandırma  

IoT Edge cihazınız ara sunucu ile çalışacak şekilde yapılandırıldıktan sonra, gelecekteki dağıtım bildirimlerinde HTTPS_PROXY ortam değişkenini bildirmeye devam etmeniz gerekir. Azure portal sihirbazını kullanarak veya bir dağıtım bildirimi JSON dosyasını düzenleyerek dağıtım bildirimlerini düzenleyebilirsiniz.

İki çalışma zamanı modülünü, edgeAgent ve edgeHub 'ı her zaman, IoT Hub ile bir bağlantı sürdürmek için proxy sunucu üzerinden iletişim kurmak üzere yapılandırın. EdgeAgent modülünden proxy bilgilerini kaldırırsanız, bağlantıyı yeniden kurmak için tek yol, önceki bölümde açıklandığı gibi, cihazdaki config. YAML dosyasını düzenlemedir.

EdgeAgent ve edgeHub modüllerine ek olarak, diğer modüllerin ara sunucu yapılandırmasına ihtiyacı olabilir. BLOB depolama gibi IoT Hub yanı sıra Azure kaynaklarına erişmesi gereken modüller, dağıtım bildirimi dosyasında belirtilen HTTPS_PROXY değişkenine sahip olmalıdır.

Aşağıdaki yordam IoT Edge cihazının ömrü boyunca geçerlidir.

### <a name="azure-portal"></a>Azure portal

IoT Edge cihazları için dağıtımlar oluşturmak üzere **modülleri ayarlama** Sihirbazı 'nı kullandığınızda, her modülün proxy sunucu bağlantılarını yapılandırabileceğiniz bir **ortam değişkenleri** bölümü vardır.

IoT Edge Aracısı ve IoT Edge hub modüllerini yapılandırmak için, sihirbazın ilk adımında **çalışma zamanı ayarları** ' nı seçin.

![Gelişmiş Edge çalışma zamanı ayarlarını yapılandırma](./media/how-to-configure-proxy-support/configure-runtime.png)

**Https_proxy** ortam değişkenini hem IoT Edge Aracısı hem de IoT Edge hub modülü tanımlarına ekleyin. IoT Edge cihazınızdaki config. YAML dosyasına **Upstreamprotocol** ortam değişkenini eklediyseniz, bunu IoT Edge aracı modül tanımına da ekleyin.

![Https_proxy ortam değişkenini ayarla](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

Dağıtım bildirimine eklediğiniz tüm diğer modüller aynı kalıbı izler.

### <a name="json-deployment-manifest-files"></a>JSON dağıtım bildirim dosyaları

Visual Studio Code şablonları kullanarak veya JSON dosyalarını el ile oluşturarak IoT Edge cihazları için dağıtımlar oluşturursanız, ortam değişkenlerini her modül tanımına doğrudan ekleyebilirsiniz.

Aşağıdaki JSON biçimini kullanın:

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    }
}
```

Ortam değişkenleri dahil olmak üzere, modül tanımınızın aşağıdaki edgeHub örneği gibi görünmesi gerekir:

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": ""
    },
    "env": {
        "https_proxy": {
            "value": "http://proxy.example.com:3128"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

IoT Edge cihazınızdaki confige. YAML dosyasına **Upstreamprotocol** ortam değişkenini eklediyseniz, bunu IoT Edge aracı modül tanımına da ekleyin.

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    },
    "UpstreamProtocol": {
        "value": "AmqpWs"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge çalışma zamanının](iot-edge-runtime.md)rolleri hakkında daha fazla bilgi edinin.

[Azure IoT Edge Için sık karşılaşılan sorunlar ve çözünürlüklerle](troubleshoot.md) yükleme ve yapılandırma hatalarını giderme
