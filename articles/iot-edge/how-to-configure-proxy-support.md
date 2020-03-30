---
title: Ağ yakınlıkları için aygıtları yapılandırma - Azure IoT Edge | Microsoft Dokümanlar
description: Azure IoT Edge çalışma süresini ve internete bakan IoT Edge modüllerini proxy sunucusu üzerinden iletişim kurmak için nasıl yapılandırın.
author: kgremban
ms.author: kgremban
ms.date: 3/10/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0600568ace5384cfb13688d14d1cf79e473f3208
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80133211"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>IoT Edge cihazını ara sunucu üzerinden iletişim kuracak şekilde yapılandırma

IoT Edge aygıtları, IoT Hub ile iletişim kurmak için HTTPS istekleri gönderir. Aygıtınız proxy sunucusu kullanan bir ağa bağlıysa, sunucu üzerinden iletişim kurmak için IoT Edge çalışma süresini yapılandırmanız gerekir. Proxy sunucuları, IoT Edge hub'ı üzerinden yönlendirilen HTTP veya HTTPS isteklerini gerçekleştirirlerse, tek tek IoT Edge modüllerini de etkileyebilir.

Bu makalede, bir proxy sunucusunun arkasında bir IoT Edge aygıtıyapılandırmak ve yönetmek için aşağıdaki dört adımda yürür:

1. **IoT Edge çalışma süresini cihazınıza yükleyin.**

   IoT Edge yükleme komut dosyaları internetten paketleri ve dosyaları çeker, bu nedenle cihazınızın bu istekleri gerçekleştirmek için proxy sunucusu üzerinden iletişim kurması gerekir. Ayrıntılı adımlar için, bu [makalenin proxy](#install-the-runtime-through-a-proxy) bölümü aracılığıyla çalışma saatini yükle'ye bakın. Windows aygıtları için yükleme komut dosyası da çevrimdışı [yükleme](how-to-install-iot-edge-windows.md#offline-or-specific-version-installation) seçeneği sağlar.

   Bu adım, ioT Edge aygıtını ilk ayarladığınızda gerçekleştirilen tek seferlik bir işlemdir. IoT Edge çalışma saatini güncelleştirdiğinizde aynı bağlantılar da gereklidir.

2. **Docker daemon ve IoT Edge daemon'u cihazınızda yapılandırın.**

   IoT Edge, her ikisi de proxy sunucusu üzerinden web istekleri yapmak gerekir cihazda iki daemons kullanır. IoT Edge daemon, IoT Hub ile iletişimden sorumludur. Moby daemon konteyner yönetiminden sorumludur, bu nedenle konteyner kayıtları ile iletişim kurar. Ayrıntılı adımlar için, bu [makalenin daemons bölümünü yapılandırın](#configure-the-daemons) bakın.

   Bu adım, ioT Edge aygıtını ilk ayarladığınızda gerçekleştirilen tek seferlik bir işlemdir.

3. **Aygıtınızdaki config.yaml dosyasındaki IoT Edge aracı özelliklerini yapılandırın.**

   IoT Edge daemon başlangıçta edgeAgent modüllerini başlatır, ancak daha sonra edgeAgent modülü dağıtım bildirimini IoT Hub'dan almak ve diğer tüm modülleri başlatmaktan sorumludur. IoT Edge aracısının IoT Hub'a ilk bağlantıyı kurması için edgeAgent modülü ortamı değişkenlerini aygıtın üzerinde el ile yapılandırın. İlk bağlantıdan sonra edgeAgent modüllerini uzaktan yapılandırabilirsiniz. Ayrıntılı adımlar için, bu [makalenin IoT Edge aracısını yapılandırın](#configure-the-iot-edge-agent) bölümüne bakın.

   Bu adım, ioT Edge aygıtını ilk ayarladığınızda gerçekleştirilen tek seferlik bir işlemdir.

4. **Gelecekteki tüm modül dağıtımları için, proxy aracılığıyla iletişim kuran herhangi bir modül için ortam değişkenlerini ayarlayın.**

   IoT Edge aygıtınız proxy sunucusu üzerinden IoT Hub'a kurulduktan ve bağlandıktan sonra, gelecekteki tüm modül dağıtımlarında bağlantıyı korumanız gerekir. Ayrıntılı adımlar için, bu makalenin [Yapılandırma dağıtım bildirimleri](#configure-deployment-manifests) bölümüne bakın.

   Bu adım, her yeni modül veya dağıtım güncelleştirmesinin aygıtın proxy sunucusu üzerinden iletişim kurma yeteneğini sürdürmesi için uzaktan gerçekleştirilen devam eden bir işlemdir.

## <a name="know-your-proxy-url"></a>Proxy URL'nizi bilin

Bu makaledeki adımlardan herhangi birini başlatmadan önce proxy URL'nizi bilmeniz gerekir.

Proxy URL'leri aşağıdaki biçimi alır: **protokol**://**proxy_host**:**proxy_port**.

* **Protokol** HTTP veya HTTPS'dir. Docker daemon, konteyner kayıt defteri ayarlarınıza bağlı olarak her iki protokolü de kullanabilir, ancak IoT Edge daemon ve çalışma zamanı kapları proxy'ye bağlanmak için her zaman HTTP'yi kullanmalıdır.

* **proxy_host** proxy sunucusu için bir adrestir. Proxy sunucunuz kimlik doğrulaması gerektiriyorsa, kimlik bilgilerinizi proxy ana bilgisayarının bir parçası olarak aşağıdaki biçimle sağlayabilirsiniz: **kullanıcı**:**parola**\@**proxy_host.**

* **proxy_port,** proxy'nin ağ trafiğine yanıt verebisi olan ağ bağlantı noktasıdır.

## <a name="install-the-runtime-through-a-proxy"></a>Çalışma süresini proxy üzerinden yükleme

IoT Edge aygıtınız Ister Windows veya Linux'ta çalışır, yükleme paketlerine proxy sunucusu üzerinden erişmeniz gerekir. İşletim sisteminize bağlı olarak, IoT Edge çalışma süresini proxy sunucusu üzerinden yüklemek için gereken adımları izleyin.

### <a name="linux-devices"></a>Linux cihazları

IoT Edge çalışma süresini bir Linux aygıtına yüklüyorsanız, yükleme paketine erişmek için paket yöneticisini proxy sunucunuzdan geçecek şekilde yapılandırın. Örneğin, [bir http-proxy kullanmak için apt-get ayarlayın.](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy) Paket yöneticiniz yapılandırıldıktan sonra, her zamanki gibi [Linux'ta Azure IoT Edge çalışma saatindeki](how-to-install-iot-edge-linux.md) yönergeleri izleyin.

### <a name="windows-devices"></a>Windows cihazları

IoT Edge çalışma zamanını bir Windows aygıtına yüklüyorsanız, proxy sunucusundan iki kez geçmeniz gerekir. İlk bağlantı yükleyici komut dosyası dosyasını karşıdan yükler ve ikinci bağlantı gerekli bileşenleri indirmek için yükleme sırasındadır. Proxy bilgilerinizi Windows ayarlarında yapılandırabilir veya proxy bilgilerinizi doğrudan PowerShell komutlarına ekleyebilirsiniz.

Aşağıdaki adımlar, bağımsız değişkeni kullanarak `-proxy` bir windows yüklemesi örneğini gösterir:

1. Invoke-WebRequest komutu yükleyici komutuna erişmek için proxy bilgilerine ihtiyaç duyar. Daha sonra Deploy-IoTEdge komutu yükleme dosyalarını indirmek için proxy bilgilerine ihtiyaç duyar.

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Deploy-IoTEdge -proxy <proxy URL>
   ```

2. Initialize-IoTEdge komutu proxy sunucusu üzerinden gitmek gerekmez, bu nedenle ikinci adım yalnızca Invoke-WebRequest için proxy bilgileri gerektirir.

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Initialize-IoTEdge
   ```

URL'ye dahil edilemeyecek proxy sunucusu için karmaşık kimlik bilgilerivarsa, `-ProxyCredential` içindeki `-InvokeWebRequestParameters`parametreyi kullanın. Örneğin,

```powershell
$proxyCredential = (Get-Credential).GetNetworkCredential()
. {Invoke-WebRequest -proxy <proxy URL> -ProxyCredential $proxyCredential -useb aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -InvokeWebRequestParameters @{ '-Proxy' = '<proxy URL>'; '-ProxyCredential' = $proxyCredential }
```

Proxy parametreleri hakkında daha fazla bilgi için [Invoke-WebRequest'e](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest)bakın. Çevrimdışı yükleme de dahil olmak üzere Windows yükleme seçenekleri hakkında daha fazla bilgi [için](how-to-install-iot-edge-windows.md)bkz.

## <a name="configure-the-daemons"></a>Daemons yapılandırma

IoT Edge, IoT Edge aygıtında çalışan iki daemon'a dayanır. Moby daemon konteyner kayıt larından konteyner görüntüleri çekmek için web istekleri yapar. IoT Edge daemon, IoT Hub ile iletişim kurmak için web isteklerinde dir.

Hem Moby hem de IoT Edge daemons devam eden aygıt işlevselliği için proxy sunucusu kullanmak için yapılandırılmalıdır. Bu adım, ilk aygıt kurulumu sırasında IoT Edge aygıtında gerçekleşir.

### <a name="moby-daemon"></a>Moby daemon

Moby Docker üzerine inşa edilmiş olduğundan, ortam değişkenleri ile Moby daemon yapılandırmak için Docker belgelerine bakın. Çoğu kapsayıcı kayıt defteri (DockerHub ve Azure Kapsayıcı Kayıt Defterleri dahil) HTTPS isteklerini destekler, bu nedenle ayarlamanız gereken parametre **HTTPS_PROXY.** Aktarım katmanı güvenliğini (TLS) desteklemeyen bir kayıt defterinden görüntü çekiyorsanız, **HTTP_PROXY** parametresini ayarlamanız gerekir.

IoT Edge aygıt işletim sisteminize uygulanan makaleyi seçin:

* [Linux'ta Docker daemon'u yapılandır](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy) Linux cihazlarındaki Moby daemon Docker adını tutar.
* [Windows'da Docker daemon'u yapılandır](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon#proxy-configuration) Windows cihazlarda Moby daemon iotedge-moby denir. Bir Windows aygıtında hem Docker Desktop hem de Moby'yi paralel olarak çalıştırmak mümkün olduğundan adlar farklıdır.

### <a name="iot-edge-daemon"></a>IoT Edge daemon

IoT Edge daemon Moby daemon benzer bir şekilde yapılandırılmıştır. İşletim sisteminizi temel alan hizmet için bir ortam değişkeni ayarlamak için aşağıdaki adımları kullanın.

IoT Edge daemon, IoT Hub'a istek göndermek için her zaman HTTPS'yi kullanır.

#### <a name="linux"></a>Linux

IoT Edge daemon'u yapılandırmak için terminalde bir editör açın.

```bash
sudo systemctl edit iotedge
```

** \<Proxy URL>** proxy sunucu adresiniz ve bağlantı bağlantı noktası ile değiştirerek aşağıdaki metni girin. Sonra, kaydet ve çık.

```ini
[Service]
Environment="https_proxy=<proxy URL>"
```

IoT Edge için yeni yapılandırmayı almak için servis yöneticisini yenileyin.

```bash
sudo systemctl daemon-reload
```

Değişikliklerin etkili olması için IoT Edge'i yeniden başlatın.

```bash
sudo systemctl restart iotedge
```

Ortam değişkeninizin oluşturulduğunu ve yeni yapılandırmanın yüklendiğini doğrulayın.

```bash
systemctl show --property=Environment iotedge
```

#### <a name="windows"></a>Windows

Yönetici olarak bir PowerShell penceresini açın ve kayıt defterini yeni ortam değişkeniyle birlikte düzenlemesi için aşağıdaki komutu çalıştırın. ** \<Proxy url>** proxy sunucu adresiniz ve bağlantı bağlantı noktası ile değiştirin.

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Services\iotedge /v Environment /t REG_MULTI_SZ /d https_proxy=<proxy URL>
```

Değişikliklerin etkili olması için IoT Edge'i yeniden başlatın.

```powershell
Restart-Service iotedge
```

## <a name="configure-the-iot-edge-agent"></a>IoT Edge aracısını yapılandırma

IoT Edge aracısı, herhangi bir IoT Edge aygıtında başlayan ilk modüldür. Bu IoT Edge config.yaml dosyasında bilgilere dayalı ilk kez başladı. IoT Edge aracısı daha sonra dağıtım bildirimlerini almak için IoT Hub'a bağlanır ve bu da aygıtta hangi diğer modüllerin dağıtılması gerektiğini bildirir.

Bu adım, ilk aygıt kurulumu sırasında IoT Edge aygıtında bir kez gerçekleşir.

1. IoT Edge cihazınızdaki config.yaml dosyasını açın. Linux sistemlerinde bu dosya **/etc/iotedge/config.yaml**adresinde bulunmaktadır. Windows sistemlerinde bu dosya **C:\ProgramData\iotedge\config.yaml**adresinde bulunur. Yapılandırma dosyası korunur, bu nedenle erişmek için yönetim ayrıcalıklarına ihtiyacınız vardır. Linux sistemlerinde, tercih `sudo` ettiğiniz metin düzenleyicisinde dosyayı açmadan önce komutu kullanın. Windows'da, Notepad gibi bir metin düzenleyicisini yönetici olarak açın ve ardından dosyayı açın.

2. config.yaml dosyasında Edge **Agent modülü spec** bölümünü bulun. IoT Edge aracısı tanımı, ortam değişkenleri ekleyebileceğiniz bir **env** parametresi içerir.

3. Env parametresi için yer tutucu olan kıvırcık braketleri çıkarın ve yeni bir satıra yeni değişken ekleyin. YAML'deki girintinlerin iki boşluk olduğunu unutmayın.

   ```yaml
   https_proxy: "<proxy URL>"
   ```

4. IoT Edge çalışma süresi, Varsayılan olarak IoT Hub ile konuşmak için AMQP kullanır. Bazı proxy sunucuları AMQP bağlantı noktalarını engeller. Bu durumda, o zaman da WebSocket üzerinde AMQP kullanmak edgeAgent yapılandırmanız gerekir. İkinci bir ortam değişkeni ekleyin.

   ```yaml
   UpstreamProtocol: "AmqpWs"
   ```

   ![çevre değişkenleri ile edgeAgent tanımı](./media/how-to-configure-proxy-support/edgeagent-edited.png)

5. Değişiklikleri config.yaml'ye kaydedin ve editörü kapatın. Değişikliklerin etkili olması için IoT Edge'i yeniden başlatın.

   * Linux:

      ```bash
      sudo systemctl restart iotedge
      ```

   * Windows:

      ```powershell
      Restart-Service iotedge
      ```

## <a name="configure-deployment-manifests"></a>Dağıtım bildirimlerini yapılandırma  

IoT Edge aygıtınız proxy sunucunuzla çalışacak şekilde yapılandırıldıktan sonra, gelecekteki dağıtım bildirimlerinde HTTPS_PROXY ortamı değişkenini beyan etmeye devam etiz gerekir. Azure portal sihirbazını kullanarak veya bir dağıtım bildirimi JSON dosyasını düzenleyerek dağıtım bildirimlerini düzenleyebilirsiniz.

IoT Hub ile bağlantı kurabilmeleri için proxy sunucusu üzerinden iletişim kurmak için her zaman iki çalışma zamanı modüllerini, edgeAgent ve edgeHub'ı yapılandırın. Proxy bilgilerini edgeAgent modülünden kaldırırsanız, bağlantıyı yeniden kurmanın tek yolu, önceki bölümde açıklandığı gibi aygıttaki config.yaml dosyasını düzenlemektir.

edgeAgent ve edgeHub modüllerine ek olarak, diğer modüllerin proxy yapılandırmasına ihtiyacı olabilir. Bunlar, blob depolama gibi IoT Hub'ı dışında Azure kaynaklarına erişmesi gereken modüllerdir ve dağıtım bildirimi dosyasında bu modül için belirtilen HTTPS_PROXY değişkene sahip olmalıdır.

Aşağıdaki yordam IoT Edge cihazının ömrü boyunca geçerlidir.

### <a name="azure-portal"></a>Azure portalında

IoT Edge aygıtları için dağıtımlar oluşturmak için **Set modülleri** sihirbazını kullandığınızda, her modülün proxy sunucu bağlantılarını yapılandırmak için kullanabileceğiniz bir **Çevre Değişkenleri** bölümü vardır.

IoT Edge aracısını ve IoT Edge hub modüllerini yapılandırmak için sihirbazın ilk adımında **Çalışma Zamanı Ayarları'nı** seçin.

![Gelişmiş Kenar Çalışma Zamanı ayarlarını yapılandırma](./media/how-to-configure-proxy-support/configure-runtime.png)

Hem IoT Edge aracısına hem de IoT Edge hub modülü tanımlarına **https_proxy** ortam değişkenini ekleyin. IoT Edge cihazınızdaki config.yaml dosyasına **UpstreamProtocol** ortam değişkenini eklediyseniz, bunu IoT Edge aracımodülü tanımına da ekleyin.

![ortam değişkenini https_proxy ayarlama](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

Dağıtım bildirimine eklediğiniz diğer tüm modüller aynı deseni izler. Modül adını ve görüntüsünü ayarladığınız sayfada bir ortam değişkenleri bölümü vardır.

### <a name="json-deployment-manifest-files"></a>JSON dağıtım bildirimi dosyaları

Visual Studio Code'daki şablonları kullanarak veya JSON dosyalarını el ile oluşturarak IoT Edge aygıtları için dağıtımlar oluşturursanız, ortam değişkenlerini doğrudan her modül tanımına ekleyebilirsiniz.

Aşağıdaki JSON biçimini kullanın:

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    }
}
```

Ortam değişkenleri dahil edilirken, modül tanımınız aşağıdaki edgeHub örneği gibi görünmelidir:

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

IoT Edge cihazınızdaki confige.yaml dosyasına **UpstreamProtocol** ortam değişkenini eklediyseniz, bunu IoT Edge aracı modülü tanımına da ekleyin.

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

[Azure IoT Edge için sık karşılaşılan sorunlar ve çözümlerle](troubleshoot.md) birlikte yükleme ve yapılandırma hatalarıyla sorun giderme
