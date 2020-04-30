---
title: Sorun giderme-Azure IoT Edge | Microsoft Docs
description: Bileşen durumu ve günlüklerini alma ve genel sorunları çözme gibi Azure IoT Edge için standart tanılama becerileri öğrenmek için bu makaleyi kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/21/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 2e15dffac73b4a50b1ef9288feaeb6073dea91e0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82086530"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Azure IoT Edge için genel sorunlar ve çözümler

Ortamınızda Azure IoT Edge’i kullanma konusunda sorun yaşarsanız, sorun giderme ve çözümleme için kılavuz olarak bu makaleden yararlanın.

## <a name="run-the-iotedge-check-command"></a>İotedge ' Check ' komutunu çalıştırın

Sorun giderme IoT Edge ilk adımlarınız, genel sorunlar için yapılandırma `check` ve bağlantı testlerinin bir koleksiyonunu çalıştıran komutunu kullanmaktır. Komut `check` , [Release 1.0.7](https://github.com/Azure/azure-iotedge/releases/tag/1.0.7) ve üzeri sürümlerde kullanılabilir.

`check` Komutu aşağıdaki gibi çalıştırabilirsiniz veya seçeneklerin tamamı listesini görmek için `--help` bayrağını dahil edebilirsiniz:

* Linux 'ta:

  ```bash
  sudo iotedge check
  ```

* Windows'da:

  ```powershell
  iotedge check
  ```

Sorun giderme aracı, bu üç kategoride sıralanan birçok denetimi çalıştırır:

* Yapılandırma denetimleri: *config. YAML* ve kapsayıcı altyapısı ile ilgili sorunlar da dahil olmak üzere uç cihazların buluta bağlanmasını engelleyebilecek ayrıntıları inceler.
* Bağlantı denetimleri: IoT Edge çalışma zamanının konak cihazdaki bağlantı noktalarına erişebileceğini ve tüm IoT Edge bileşenlerinin IoT Hub bağlanabildiğini doğrular.
* Üretime hazırlık denetimleri: cihaz sertifika yetkilisi (CA) sertifikaları ve modül günlük dosyası yapılandırması gibi önerilen üretim en iyi uygulamalarına bakar.

Bu aracın çalıştığı her bir tanılama denetimi hakkında bilgi için, bir hata veya uyarı alırsanız, [IoT Edge sorun giderme denetimleri](https://github.com/Azure/iotedge/blob/master/doc/troubleshoot-checks.md)bölümüne bakın.

## <a name="gather-debug-information-with-iotedge-support-bundle-command"></a>İotedge ' support-demeti ' komutuyla hata ayıklama bilgilerini toplayın

IoT Edge cihazdan Günlükler toplamanız gerektiğinde, en uygun yöntem `support-bundle` komutunu kullanmaktır. Varsayılan olarak, bu komut modül, güvenlik yöneticisi ve kapsayıcı altyapısı günlükleri IoT Edge, ' ıotedge Check ' JSON çıkışını ve diğer yararlı hata ayıklama bilgilerini toplar. Bunları kolay paylaşım için tek bir dosya halinde sıkıştırır. Komut `support-bundle` , [Release 1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) ve üzeri sürümlerde kullanılabilir.

Geçmişteki günlüklere `support-bundle` ne kadar süre `--since` almak istediğinizi belirtmek için komutunu bayrağıyla çalıştırın. Örneğin `6h` , son 6 günden `6d` `6m` bu yana son 6 dakikadan bu yana son 6 saatten bu yana Günlükler alınır. Seçeneklerin tamamı `--help` listesini görmek için bayrağını ekleyin.

* Linux 'ta:

  ```bash
  sudo iotedge support-bundle --since 6h
  ```

* Windows'da:

  ```powershell
  iotedge support-bundle --since 6h
  ```

> [!WARNING]
> `support-bundle` Komutun çıktısı ana bilgisayar, cihaz ve modül adları, modülleriniz tarafından günlüğe kaydedilen bilgiler içerebilir. Çıktıyı ortak bir forumunda paylaşımında lütfen unutmayın.

## <a name="standard-diagnostic-steps"></a>Standart tanılama adımları

Bir sorunla karşılaşırsanız, kapsayıcı günlüklerini ve cihazdan geçen iletileri inceleyerek IoT Edge cihazınızın durumu hakkında daha fazla bilgi edinebilirsiniz. Bilgi toplamak için bu bölümdeki komutları ve araçları kullanın.

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>IoT Edge Güvenlik Yöneticisi ve günlüklerinin durumunu kontrol edin

Linux 'ta:

* IoT Edge Güvenlik Yöneticisi 'nin durumunu görüntülemek için:

   ```bash
   sudo systemctl status iotedge
   ```

* IoT Edge Güvenlik Yöneticisi 'nin günlüklerini görüntülemek için:

    ```bash
    sudo journalctl -u iotedge -f
    ```

* IoT Edge Güvenlik Yöneticisi 'nin daha ayrıntılı günlüklerini görüntülemek için:

  * İotedge Daemon ayarlarını düzenleyin:

      ```bash
      sudo systemctl edit iotedge.service
      ```

  * Aşağıdaki satırları güncelleştirin:

      ```bash
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```

  * IoT Edge güvenlik cini 'nı yeniden başlatın:

      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

Windows'da:

* IoT Edge Güvenlik Yöneticisi 'nin durumunu görüntülemek için:

   ```powershell
   Get-Service iotedge
   ```

* IoT Edge Güvenlik Yöneticisi 'nin günlüklerini görüntülemek için:

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>IoT Edge Güvenlik Yöneticisi çalışmıyorsa, YAML yapılandırma dosyanızı doğrulayın

> [!WARNING]
> YAML dosyaları girintileme olarak sekme içeremez. Bunun yerine 2 boşluk kullanın. Üst düzey öğelerin başında boşluk olmaması gerekir.

Linux 'ta:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Windows'da:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Sorunlar için kapsayıcı günlüklerine bakın

IoT Edge güvenlik arka plan programı çalışmaya başladıktan sonra, sorunları algılamak için kapsayıcıların günlüklerine bakın. Dağıtılan kapsayıcılarınız ile başlayın ve ardından IoT Edge çalışma zamanını oluşturan kapsayıcılara bakın: edgeAgent ve edgeHub. IoT Edge Aracısı günlükleri genellikle her kapsayıcının yaşam döngüsü hakkında bilgi sağlar. IoT Edge hub günlükleri mesajlaşma ve yönlendirme hakkında bilgi sağlar.

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-iot-edge-hub"></a>IoT Edge hub 'ından giden iletileri görüntüleme

IoT Edge hub 'ından gelen iletileri görüntüleyebilir ve çalışma zamanı kapsayıcılarından ayrıntılı günlüklerden Öngörüler toplayabilirsiniz. Bu kapsayıcılarda ayrıntılı günlükleri açmak için, YAML `RuntimeLogLevel` yapılandırma dosyanızda öğesini ayarlayın. Dosyayı açmak için:

Linux 'ta:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Windows'da:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

Varsayılan olarak, `agent` öğesi aşağıdaki örneğe benzer şekilde görünür:

   ```yaml
   agent:
     name: edgeAgent
     type: docker
     env: {}
     config:
       image: mcr.microsoft.com/azureiotedge-agent:1.0
       auth: {}
   ```

Değiştir `env: {}` :

   ```yaml
   env:
     RuntimeLogLevel: debug
   ```

   > [!WARNING]
   > YAML dosyaları tanımlayıcı olarak sekme içeremez. Bunun yerine 2 boşluk kullanın. Üst düzey öğelerin başında boşluk bulunamaz.

Dosyayı kaydedin ve IoT Edge Güvenlik Yöneticisi ' ni yeniden başlatın.

IoT Hub ile IoT Edge cihazları arasında gönderilmekte olan iletileri de denetleyebilirsiniz. [Visual Studio Code Için Azure IoT Hub uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)kullanarak bu iletileri görüntüleyin. Daha fazla bilgi için bkz. [Azure IoT ile geliştirme yaparken kullanışlı araç](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

### <a name="restart-containers"></a>Kapsayıcıları yeniden Başlat

Günlükler ve iletileri bilgi için araştırdıktan sonra kapsayıcıları yeniden başlatmayı deneyebilirsiniz:

```cmd
iotedge restart <container name>
```

IoT Edge çalışma zamanı kapsayıcılarını yeniden başlatın:

```cmd
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>IoT Edge Güvenlik Yöneticisi 'ni yeniden başlatın

Sorun hala sürdürülürken, IoT Edge Güvenlik Yöneticisi 'ni yeniden başlatmayı deneyebilirsiniz.

Linux 'ta:

   ```cmd
   sudo systemctl restart iotedge
   ```

Windows'da:

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

## <a name="iot-edge-agent-stops-after-about-a-minute"></a>IoT Edge aracı bir dakika sonra durdu

EdgeAgent modülü yaklaşık bir dakika boyunca başarıyla başlatılır ve çalışır ve sonra duraklar. Günlükler IoT Edge aracısının AMQP üzerinden IoT Hub bağlanmaya çalıştığı ve sonra da WebSocket üzerinden AMQP kullanarak bağlanmaya çalıştığı anlamına gelebilir. Bu başarısız olduğunda IoT Edge Aracısı çıkar.

Örnek edgeAgent günlükleri:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent.
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5)
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP...
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket...
```

**Kök neden**

Konak ağındaki bir ağ yapılandırması, IoT Edge aracısının ağa ulaşmasını engellemektedir. Aracı ilk olarak AMQP (5671 numaralı bağlantı noktası) üzerinden bağlanma girişiminde bulunur. Bağlantı başarısız olursa, WebSockets (bağlantı noktası 443) çalışır.

IoT Edge çalışma zamanı, her bir modül için iletişim kurulacak bir ağ ayarlar. Linux’ta bu ağ bir köprü ağıdır. Windows’da NAT kullanır. Bu sorun, NAT ağını kullanan Windows kapsayıcılarının kullanıldığı Windows cihazlarında daha yaygın olarak görülür.

**Çözünürlük**

Bu köprüye/NAT ağına atanan IP adresleri için bir İnternet rotası olduğundan emin olun. Bazen konaktaki VPN yapılandırması, IoT Edge ağını geçersiz kılar.

## <a name="iot-edge-hub-fails-to-start"></a>IoT Edge hub 'ı başlatılamıyor

EdgeHub modülü başlatılamıyor ve günlüklere şu iletiyi yazdırır:

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n)
```

**Kök neden**

Konak makinedeki başka bir işlem, 443 numaralı bağlantı noktasına bağlıdır. IoT Edge hub, 5671 ve 443 bağlantı noktalarını ağ geçidi senaryolarında kullanılmak üzere eşleştirir. Başka bir işlem zaten bu bağlantı noktasına bağlıysa bu bağlantı noktası eşlemesi başarısız olur.

**Çözünürlük**

443 numaralı bağlantı noktasını kullanan işlemi bulup durdurun. Bu işlem genellikle bir web sunucusudur.

## <a name="iot-edge-agent-cant-access-a-modules-image-403"></a>IoT Edge Aracısı bir modülün görüntüsüne erişemiyor (403)

Bir kapsayıcı çalıştırılamaz ve edgeAgent günlükleri bir 403 hatası gösterir.

**Kök neden**

IoT Edge aracısının bir modülün görüntüsüne erişme izni yok.

**Çözünürlük**

Kayıt defteri kimlik bilgilerinizin dağıtım bildiriminde doğru şekilde belirtildiğinden emin olun

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>IoT Edge güvenlik arka plan programı geçersiz bir ana bilgisayar adıyla başarısız oluyor

Komut `sudo journalctl -u iotedge` başarısız olur ve şu iletiyi yazdırır:

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

**Kök neden**

IoT Edge çalışma zamanı yalnızca 64 karakterden kısa olan ana bilgisayar adlarını destekleyebilir. Fiziksel makinelerde genellikle uzun ana bilgisayar adları yoktur, ancak sorun bir sanal makinede daha yaygındır. Azure 'da barındırılan Windows sanal makineleri için otomatik olarak oluşturulan ana bilgisayar adları, özellikle de uzun olur.

**Çözünürlük**

Bu hatayı gördüğünüzde, sanal makinenizin DNS adını yapılandırarak ve sonra DNS adını Kurulum komutunda ana bilgisayar adı olarak ayarlayarak bu sorunu çözebilirsiniz.

1. Azure portal, sanal makinenizin genel bakış sayfasına gidin.
2. DNS adı altında **Yapılandır** ' ı seçin. Sanal makinenizin yapılandırılmış bir DNS adı zaten varsa, yeni bir tane yapılandırmanız gerekmez.

   ![Sanal makinenin DNS adını yapılandırın](./media/troubleshoot/configure-dns.png)

3. **DNS ad etiketi** için bir değer girin ve **Kaydet**' i seçin.
4. ** \<Dnsnamelabel\>biçiminde olması gereken yeni DNS adını kopyalayın.\< vmlocation\>. cloudapp.Azure.com**.
5. Sanal makinenin içinde, DNS adınızla IoT Edge çalışma zamanını ayarlamak için aşağıdaki komutu kullanın:

   * Linux 'ta:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   * Windows'da:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="stability-issues-on-resource-constrained-devices"></a>Kaynak kısıtlı cihazlarda kararlılık sorunları

Özellikle bir ağ geçidi olarak kullanıldığında Raspberry PI gibi kısıtlı cihazlarda kararlılık sorunlarıyla karşılaşabilirsiniz. Belirtiler, uç hub modülünde bellek dışında özel durumlar içerir, aşağı akış cihazları bağlanamaz veya cihaz birkaç saatten sonra telemetri iletileri göndermeyi durduruyor.

**Kök neden**

IoT Edge çalışma zamanının parçası olan IoT Edge hub, varsayılan olarak performans için en iyi duruma getirilmiştir ve büyük bellek öbeklerini ayırmaya çalışır. Bu iyileştirme kısıtlı sınır cihazları için ideal değildir ve kararlılık sorunlarına neden olabilir.

**Çözünürlük**

IoT Edge hub 'ı için **Optimizeforperformance** ortam değişkenini **false**olarak ayarlayın. Ortam değişkenlerini ayarlamak için iki yol vardır:

Azure portalında:

IoT Hub, IoT Edge cihazınızı ve cihaz ayrıntıları sayfasında, **Modül** > **çalışma zamanı ayarlarını**ayarla ' yı seçin. Edge hub modülü için, *false*olarak ayarlanan *Optimizeforperformance* adlı bir ortam değişkeni oluşturun.

![OptimizeForPerformance, false olarak ayarlandı](./media/troubleshoot/optimizeforperformance-false.png)

**VEYA**

Dağıtım bildiriminde:

```json
  "edgeHub": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
      "createOptions": <snipped>
    },
    "env": {
      "OptimizeForPerformance": {
          "value": "false"
      }
    },
```

## <a name="cant-get-the-iot-edge-daemon-logs-on-windows"></a>Windows 'da IoT Edge Daemon günlükleri alınamıyor

Windows 'da kullanırken `Get-WinEvent` bir EventLogException alırsanız kayıt defteri girdilerinizi kontrol edin.

**Kök neden**

PowerShell `Get-WinEvent` komutu, belirli `ProviderName`bir günlük bulmak için bir kayıt defteri girdisini temel alır.

**Çözünürlük**

IoT Edge Daemon için bir kayıt defteri girişi ayarlayın. Aşağıdaki içerikle bir **ıotedge. reg** dosyası oluşturun ve çift tıklayarak veya `reg import iotedge.reg` komutunu kullanarak Windows kayıt defterine aktarın:

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="iot-edge-module-fails-to-send-a-message-to-the-edgehub-with-404-error"></a>IoT Edge modülü, 404 hatasıyla edgeHub 'a ileti gönderemediğinde

Özel bir IoT Edge modülü, 404 `Module not found` hatasıyla edgeHub 'a ileti gönderemediğinde. IoT Edge Daemon, günlüklere aşağıdaki iletiyi yazdırır:

```output
Error: Time:Thu Jun  4 19:44:58 2018 File:/usr/sdk/src/c/provisioning_client/adapters/hsm_client_http_edge.c Func:on_edge_hsm_http_recv Line:364 executing HTTP request fails, status=404, response_buffer={"message":"Module not found"}u, 04 )
```

**Kök neden**

IoT Edge Daemon, güvenlik nedenleriyle edgeHub 'a bağlanan tüm modüller için işlem tanımlamayı zorlar. Modül tarafından gönderilen tüm iletilerin, modülün ana işlem KIMLIĞINDEN geldiğini doğrular. Bir ileti, başlangıçta kurulduğu farklı bir işlem KIMLIĞINDEN bir modül tarafından gönderiliyorsa, iletiyi 404 hata iletisiyle reddeder.

**Çözünürlük**

Sürüm 1.0.7 itibariyle, tüm modül işlemlerinin bağlanmasına izin verilir. 1.0.7 sürümüne yükseltme yapılamıyorsa, aşağıdaki adımları izleyin. Daha fazla bilgi için bkz. [1.0.7 Release changelog](https://github.com/Azure/iotedge/blob/master/CHANGELOG.md#iotedged-1).

Aynı işlem KIMLIĞININ, her zaman özel IoT Edge modülü tarafından edgeHub 'a ileti göndermek için kullanıldığından emin olun. Örneğin `ENTRYPOINT` , Docker dosyanızda `CMD` komutunu kullanın, çünkü `CMD` modül için bir işlem kimliği ve ana programı çalıştıran Bash komutu için başka bir Işlem kimliği, tek bir işlem kimliğine neden `ENTRYPOINT` olur.

## <a name="firewall-and-port-configuration-rules-for-iot-edge-deployment"></a>IoT Edge dağıtımı için güvenlik duvarı ve bağlantı noktası yapılandırma kuralları

Azure IoT Edge, desteklenen IoT Hub protokolleri kullanılarak şirket içi bir sunucudan Azure bulutuna iletişimin iletişimine izin veriyor, bkz. [iletişim protokolü seçme](../iot-hub/iot-hub-devguide-protocols.md). Gelişmiş güvenlik için Azure IoT Edge ile Azure IoT Hub arasındaki iletişim kanalları her zaman giden olarak yapılandırılır. Bu yapılandırma, kötü amaçlı bir varlığın araştırılacak saldırı yüzeyini en aza indiren [hizmet yardımlı iletişim düzenine](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/)dayanır. Gelen iletişim yalnızca Azure IoT Hub Azure IoT Edge cihaza ileti göndermek için gereken belirli senaryolar için gereklidir. Buluttan cihaza iletiler, güvenli TLS kanalları kullanılarak korunur ve X. 509.440 sertifikaları ve TPM cihaz modülleri kullanılarak daha da güvenli hale getirilir. Azure IoT Edge Güvenlik Yöneticisi bu iletişimin nasıl kurulamadığını yönetir, bkz. [IoT Edge Security Manager](../iot-edge/iot-edge-security-manager.md).

IoT Edge, Azure IoT Edge çalışma zamanına ve dağıtılan modüllere güvenli hale getirmek için gelişmiş yapılandırma sağlarken, hala temel alınan makineye ve ağ yapılandırmasına bağımlıdır. Bu nedenle, güvenli uç için bulut iletişimine doğru ağ ve Güvenlik Duvarı kurallarının ayarlandığından emin olmanız zorunludur. Aşağıdaki tablo, Azure IoT Edge çalışma zamanının barındırıldığı temel sunucular için yapılandırma güvenlik duvarı kuralları olduğunda bir kılavuz olarak kullanılabilir:

|Protokol|Bağlantı noktası|Gelen|Tarafına|Rehber|
|--|--|--|--|--|
|MQTT|8883|ENGELLENDI (varsayılan)|ENGELLENDI (varsayılan)|<ul> <li>İletişim protokolü olarak MQTT kullanılırken açık olacak giden (giden) öğesini yapılandırın.<li>MQTT için 1883 IoT Edge desteklenmez. <li>Gelen (gelen) bağlantıların engellenmesi gerekir.</ul>|
|AMQP|5671|ENGELLENDI (varsayılan)|Açık (varsayılan)|<ul> <li>IoT Edge için varsayılan iletişim protokolü. <li> Desteklenen diğer protokoller için Azure IoT Edge yapılandırılmamışsa veya AMQP istenen iletişim protokollemiyorsa, bu açık olacak şekilde yapılandırılmış olmalıdır.<li>IoT Edge, AMQP için 5672 tarafından desteklenmiyor.<li>Azure IoT Edge farklı IoT Hub desteklenen bir protokol kullandığında bu bağlantı noktasını engelleyin.<li>Gelen (gelen) bağlantıların engellenmesi gerekir.</ul></ul>|
|HTTPS|443|ENGELLENDI (varsayılan)|Açık (varsayılan)|<ul> <li>IoT Edge sağlama için 443 tarihinde açık olan giden (giden) öğesini yapılandırın. El ile betik veya Azure IoT cihaz sağlama hizmeti (DPS) kullanılırken bu yapılandırma gereklidir. <li>Gelen (gelen) bağlantı yalnızca belirli senaryolar için açık olmalıdır: <ul> <li>  Yöntem istekleri gönderebilen yaprak cihazlara sahip saydam bir ağ geçidiniz varsa. Bu durumda, ıothub 'e bağlanmak veya Azure IoT Edge aracılığıyla ıothub hizmetleri sağlamak için bağlantı noktası 443 ' nin dış ağlara açık olması gerekmez. Bu nedenle gelen kural, iç ağdan yalnızca gelen (gelen) öğesini açmak üzere kısıtlanabilir. <li> Istemciden cihaza (C2D) senaryolar için.</ul><li>HTTP için 80 IoT Edge desteklenmez.<li>HTTP olmayan protokoller (örneğin, AMQP veya MQTT) kuruluşta yapılandırılamaz; iletiler WebSockets üzerinden gönderilebilir. Bağlantı noktası 443, bu durumda WebSocket iletişimi için kullanılacaktır.</ul>|

## <a name="edge-agent-module-continually-reports-empty-config-file-and-no-modules-start-on-the-device"></a>Edge Aracısı modülü sürekli olarak ' boş yapılandırma dosyası ' bildiriyor ve cihazda hiçbir modül başlamıyor

Cihazda dağıtımda tanımlanan modüller başlatılırken sorun yaşanıyor. Yalnızca edgeAgent çalışıyor ancak sürekli olarak ' boş yapılandırma dosyası... ' bildiriyor.

**Kök neden**

Varsayılan olarak, IoT Edge kendi yalıtılmış kapsayıcı ağında modüller başlatır. Bu özel ağ içinde, bu cihaz DNS ad çözümlemesi ile ilgili sorun yaşıyor olabilir.

**Çözünürlük**

**Seçenek 1: kapsayıcı altyapısı ayarlarındaki DNS sunucusunu ayarlama**

Kapsayıcı altyapısı ayarlarında ortamınız için DNS sunucusunu belirtin; Bu, altyapı tarafından başlatılan tüm kapsayıcı modülleri için geçerlidir. Kullanılacak DNS sunucusunu belirten `daemon.json` adlı bir dosya oluşturun. Örneğin:

```json
{
    "dns": ["1.1.1.1"]
}
```

Yukarıdaki örnek, DNS sunucusunu genel olarak erişilebilen bir DNS hizmetine ayarlar. Sınır cihazı bu IP ortamından erişemezse, erişilebilir olan DNS sunucusu adresi ile değiştirin.

Platformunuzun doğru konumunu yerleştirin `daemon.json` :

| Platform | Konum |
| --------- | -------- |
| Linux | `/etc/docker` |
| Windows kapsayıcıları ile Windows ana bilgisayarı | `C:\ProgramData\iotedge-moby\config` |

Konum zaten dosya içeriyorsa `daemon.json` , **DNS** anahtarını buna ekleyin ve dosyayı kaydedin.

Güncelleştirmelerin etkili olabilmesi için kapsayıcı altyapısını yeniden başlatın.

| Platform | Komut |
| --------- | -------- |
| Linux | `sudo systemctl restart docker` |
| Windows (yönetici PowerShell) | `Restart-Service iotedge-moby -Force` |

**Seçenek 2: modül başına IoT Edge dağıtımında DNS sunucusunu ayarlama**

Her modülün *createOptions* için DNS sunucusunu IoT Edge dağıtımında ayarlayabilirsiniz. Örneğin:

```json
"createOptions": {
  "HostConfig": {
    "Dns": [
      "x.x.x.x"
    ]
  }
}
```

Bu yapılandırmayı *Edgeagent* ve *edgehub* modülleri de ayarladığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge platformunda bir hata bulduğunuzu düşünüyor musunuz? İyileşmeye devam edebilmemiz için [bir sorun gönderin](https://github.com/Azure/iotedge/issues) .

Daha fazla sorunuz varsa yardım için bir [destek isteği](https://portal.azure.com/#create/Microsoft.Support) oluşturun.
