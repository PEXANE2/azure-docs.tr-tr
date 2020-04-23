---
title: Sorun Giderme - Azure IoT Edge | Microsoft Dokümanlar
description: Azure IoT Edge için bileşen durumu ve günlükleri alma ve sık karşılaşılan sorunları çözme gibi standart tanılama becerilerini öğrenmek için bu makaleyi kullanın
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
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086530"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Azure IoT Edge için genel sorunlar ve çözümler

Ortamınızda Azure IoT Edge’i kullanma konusunda sorun yaşarsanız, sorun giderme ve çözümleme için kılavuz olarak bu makaleden yararlanın.

## <a name="run-the-iotedge-check-command"></a>iotedge 'check' komutunu çalıştırın

IoT Edge sorun giderme ilk adım, `check` yapılandırma ve sık karşılaşılan sorunlar için bağlantı testleri bir koleksiyon çalıştıran komutu kullanmak olmalıdır. Komut `check` [sürüm 1.0.7](https://github.com/Azure/azure-iotedge/releases/tag/1.0.7) ve sonrası kullanılabilir.

Komutu `check` aşağıdaki gibi çalıştırabilir veya `--help` seçeneklerin tam listesini görmek için bayrağı ekleyebilirsiniz:

* Linux üzerinde:

  ```bash
  sudo iotedge check
  ```

* Windows'da:

  ```powershell
  iotedge check
  ```

Sorun giderme aracı, bu üç kategoriye sıralanmış birçok denetim çalıştırMaktadır:

* Yapılandırma denetimleri: *Config.yaml* ve konteyner motoruyla ilgili sorunlar da dahil olmak üzere Edge aygıtlarının buluta bağlanmasını engelleyebilecek ayrıntıları inceler.
* Bağlantı denetimleri: IoT Edge çalışma zamanı nın ana bilgisayar aygıtındaki bağlantı noktalarına erişebileceğini ve tüm IoT Edge bileşenlerinin IoT Hub'ına bağlanabileceğini doğrular.
* Üretime hazırlık denetimleri: Aygıt sertifikası yetkilisi (CA) sertifikalarıve modül günlüğü dosyası yapılandırması durumu gibi önerilen üretim en iyi uygulamalarını arar.

Bir hata veya uyarı alırsanız ne yapmanız gerektiğini de içeren bu aracın çalıştırılabildikleri tanılama denetimlerinin her biri hakkında bilgi için [Bkz. IoT Edge sorun giderme denetimleri.](https://github.com/Azure/iotedge/blob/master/doc/troubleshoot-checks.md)

## <a name="gather-debug-information-with-iotedge-support-bundle-command"></a>iotedge 'destek paketi' komutuyla hata ayıklama bilgilerini toplama

Bir IoT Edge aygıtından günlükleri toplamanız gerektiğinde, en uygun `support-bundle` yol komutu kullanmaktır. Varsayılan olarak, bu komut modülü, IoT Edge Security Manager ve konteyner motoru günlükleri, 'iotedge denetimi' JSON çıkışı ve diğer yararlı hata ayıklama bilgilerini toplar. Kolay paylaşım için bunları tek bir dosyaya sıkıştırır. Komut `support-bundle` [sürüm 1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) ve sonrası kullanılabilir.

Günlükleri `support-bundle` almak `--since` istediğinizi geçmişten ne kadar süre önce belirtmek için bayrağı ni ile komutu çalıştırın. Örneğin `6h` son 6 saatten beri, son `6d` 6 günden beri, `6m` son 6 dakikadan beri günlükleri alırsınız ve benzeri. Seçeneklerin `--help` tam listesini görmek için bayrağı ekleyin.

* Linux üzerinde:

  ```bash
  sudo iotedge support-bundle --since 6h
  ```

* Windows'da:

  ```powershell
  iotedge support-bundle --since 6h
  ```

> [!WARNING]
> `support-bundle` Komuttan çıkan çıktılar ana bilgisayar, cihaz ve modül adları, modülleriniz tarafından günlüğe kaydedilen bilgiler vb. içerebilir. Çıktıyı herkese açık bir forumda paylaşıyorsanız lütfen bunun farkında olun.

## <a name="standard-diagnostic-steps"></a>Standart tanılama adımları

Bir sorunla karşılaşırsanız, kapsayıcı günlüklerini ve aygıta ve cihazdan geçen iletileri inceleyerek IoT Edge cihazınızın durumu hakkında daha fazla bilgi edinebilirsiniz. Bilgi toplamak için bu bölümdeki komutları ve araçları kullanın.

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>IoT Edge Güvenlik Yöneticisi'nin durumunu ve günlüklerini kontrol edin

Linux üzerinde:

* IoT Edge Güvenlik Yöneticisi'nin durumunu görüntülemek için:

   ```bash
   sudo systemctl status iotedge
   ```

* IoT Edge Güvenlik Yöneticisi'nin günlüklerini görüntülemek için:

    ```bash
    sudo journalctl -u iotedge -f
    ```

* IoT Edge Güvenlik Yöneticisi'nin daha ayrıntılı günlüklerini görüntülemek için:

  * Iotedge daemon ayarlarını edin:

      ```bash
      sudo systemctl edit iotedge.service
      ```

  * Aşağıdaki satırları güncelleyin:

      ```bash
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```

  * IoT Edge Güvenlik Daemon yeniden başlatın:

      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

Windows'da:

* IoT Edge Güvenlik Yöneticisi'nin durumunu görüntülemek için:

   ```powershell
   Get-Service iotedge
   ```

* IoT Edge Güvenlik Yöneticisi'nin günlüklerini görüntülemek için:

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>IoT Edge Güvenlik Yöneticisi çalışmıyorsa, yaml yapılandırma dosyanızı doğrulayın

> [!WARNING]
> YAML dosyaları girintisi olarak sekmeleri içeremez. Bunun yerine 2 boşluk kullanın. Üst düzey öğelerin satır aralığı olmamalıdır.

Linux üzerinde:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Windows'da:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Sorunlar için kapsayıcı günlüklerini denetleme

IoT Edge Security Daemon çalışmaya başladığında, sorunları algılamak için kapsayıcıların günlüklerine bakın. Dağıtılan kapsayıcılarınızla başlayın, ardından IoT Edge çalışma süresini oluşturan kapsayıcılara bakın: edgeAgent ve edgeHub. IoT Edge aracısı günlükleri genellikle her kapsayıcının yaşam döngüsü hakkında bilgi sağlar. IoT Edge hub günlükleri mesajlaşma ve yönlendirme hakkında bilgi sağlar.

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-iot-edge-hub"></a>IoT Edge hub'ından geçen iletileri görüntüleme

IoT Edge hub'ından geçen iletileri görüntüleyebilir ve çalışma zamanı kapsayıcılarından ayrıntılı günlüklerden öngörüler toplayabilirsiniz. Bu kapsayıcılar üzerinde ayrıntılı günlükleri açmak `RuntimeLogLevel` için, yaml yapılandırma dosyanızda ayarlayın. Dosyayı açmak için:

Linux üzerinde:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Windows'da:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

Varsayılan olarak, `agent` öğe aşağıdaki örnek gibi görünecektir:

   ```yaml
   agent:
     name: edgeAgent
     type: docker
     env: {}
     config:
       image: mcr.microsoft.com/azureiotedge-agent:1.0
       auth: {}
   ```

Şuyla değiştirin: `env: {}`

   ```yaml
   env:
     RuntimeLogLevel: debug
   ```

   > [!WARNING]
   > YAML dosyaları identasyon olarak sekmeleri içeremez. Bunun yerine 2 boşluk kullanın. Üst düzey öğelerin önde gelen beyaz ayarı olamaz.

Dosyayı kaydedin ve IoT Edge güvenlik yöneticisini yeniden başlatın.

IoT Hub ile IoT Edge cihazları arasında gönderilmekte olan iletileri de denetleyebilirsiniz. [Visual Studio Code için Azure IoT Hub uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)kullanarak bu iletileri görüntüleyin. Daha fazla bilgi için [Azure IoT ile geliştirdiğinizde Kullanışlı araca](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/)bakın.

### <a name="restart-containers"></a>Kapsayıcıları yeniden başlatın

Bilgi için günlükleri ve mesajları araştırdıktan sonra, kapsayıcıları yeniden başlatmayı deneyebilirsiniz:

```cmd
iotedge restart <container name>
```

IoT Edge çalışma zamanı kaplarını yeniden başlatın:

```cmd
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>IoT Edge güvenlik yöneticisini yeniden başlatın

Sorun devam ediyorsa, IoT Edge güvenlik yöneticisini yeniden başlatmayı deneyebilirsiniz.

Linux üzerinde:

   ```cmd
   sudo systemctl restart iotedge
   ```

Windows'da:

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

## <a name="iot-edge-agent-stops-after-about-a-minute"></a>IoT Edge aracısı yaklaşık bir dakika sonra durur

edgeAgent modülü başlar ve yaklaşık bir dakika boyunca başarılı bir şekilde çalışır, sonra durur. Günlükler, IoT Edge aracısının AMQP üzerinden IoT Hub'ına bağlanmaya ve ardından WebSocket üzerinden AMQP kullanarak bağlanmaya çalıştığına işaret eder. Bu başarısız olduğunda, IoT Edge aracısı çıkar.

Örnek edgeAgent günlükleri:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent.
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5)
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP...
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket...
```

**Kök neden**

Ana bilgisayar ağındaki ağ yapılandırması, IoT Edge aracısının ağa ulaşmasını engelliyor. Aracı ilk olarak AMQP (5671 numaralı bağlantı noktası) üzerinden bağlanma girişiminde bulunur. Bağlantı başarısız olursa, WebSockets (port 443) çalışır.

IoT Edge çalışma zamanı, her bir modül için iletişim kurulacak bir ağ ayarlar. Linux’ta bu ağ bir köprü ağıdır. Windows’da NAT kullanır. Bu sorun, NAT ağını kullanan Windows kapsayıcılarının kullanıldığı Windows cihazlarında daha yaygın olarak görülür.

**Çözünürlük**

Bu köprüye/NAT ağına atanan IP adresleri için bir İnternet rotası olduğundan emin olun. Bazen konaktaki VPN yapılandırması, IoT Edge ağını geçersiz kılar.

## <a name="iot-edge-hub-fails-to-start"></a>IoT Edge hub'ı başlatılmaz

edgeHub modülü başlatılamadı ve aşağıdaki iletiyi günlüklere yazdırır:

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n)
```

**Kök neden**

Konak makinedeki başka bir işlem, 443 numaralı bağlantı noktasına bağlıdır. IoT Edge hub'ı ağ geçidi senaryolarında kullanılmak üzere 5671 ve 443 bağlantı noktalarını eşler. Başka bir işlem zaten bu bağlantı noktasına bağlıysa bu bağlantı noktası eşlemesi başarısız olur.

**Çözünürlük**

443 numaralı bağlantı noktasını kullanan işlemi bulup durdurun. Bu işlem genellikle bir web sunucusudur.

## <a name="iot-edge-agent-cant-access-a-modules-image-403"></a>IoT Edge aracısı bir modülün görüntüsüne erişemez (403)

Bir kapsayıcı çalışmaz ve edgeAgent günlükleri 403 hata gösterir.

**Kök neden**

IoT Edge aracısının bir modülün görüntüsüne erişmek için izinleri yoktur.

**Çözünürlük**

Kayıt defteri kimlik bilgilerinizin dağıtım bildiriminizde doğru şekilde belirtildiğinden emin olun

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>IoT Edge güvenlik daemon geçersiz bir ana bilgisayar adı ile başarısız olur

Komut `sudo journalctl -u iotedge` başarısız olur ve aşağıdaki iletiyi yazdırır:

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

**Kök neden**

IoT Edge çalışma süresi yalnızca 64 karakterden kısa olan ana bilgisayar adlarını destekleyebilir. Fiziksel makineler genellikle uzun ana bilgisayar adları yoktur, ancak sorun sanal bir makinede daha yaygındır. Özellikle Azure'da barındırılan Windows sanal makineleri için otomatik olarak oluşturulan ana bilgisayar adları uzun olma eğilimindedir.

**Çözünürlük**

Bu hatayı gördüğünüzde, sanal makinenizin DNS adını yapılandırarak ve ardından kurulum komutundaki ana bilgisayar adı olarak DNS adını ayarlayarak bunu çözebilirsiniz.

1. Azure portalında, sanal makinenizin genel bakış sayfasına gidin.
2. DNS adı altında **yapılandırma'yı** seçin. Sanal makinenizde zaten yapılandırılmış bir DNS adı varsa, yeni bir makine yapılandırmanız gerekmez.

   ![Sanal makinenin DNS adını yapılandırma](./media/troubleshoot/configure-dns.png)

3. **DNS ad etiketi** için bir değer sağlayın ve **Kaydet'i**seçin.
4. ** \<DNSnamelabel\>biçiminde olması gereken yeni DNS adını\< kopyalayın. vmlocation\>.cloudapp.azure.com**.
5. Sanal makinenin içinde, DNS adınız ile IoT Edge çalışma süresini ayarlamak için aşağıdaki komutu kullanın:

   * Linux üzerinde:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   * Windows'da:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="stability-issues-on-resource-constrained-devices"></a>Kaynak kısıtlı aygıtlarında kararlılık sorunları

Raspberry Pi gibi kısıtlı cihazlarda, özellikle ağ geçidi olarak kullanıldığında stabilite sorunlarıyla karşılaşabilirsiniz. Belirtiler, kenar hub modülünde bellek dışında özel durumlar içerir, alt akış aygıtları bağlanamaz veya aygıt birkaç saat sonra telemetri iletileri göndermeyi durdurur.

**Kök neden**

IoT Edge çalışma zamanının bir parçası olan IoT Edge hub'ı varsayılan olarak performans için optimize edilir ve büyük bellek parçalarını ayırmaya çalışır. Bu optimizasyon kısıtlı kenar aygıtları için ideal değildir ve kararlılık sorunlarına neden olabilir.

**Çözünürlük**

IoT Edge hub'ı için, bir ortam değişkeni **optimizeforperformance'ı** **false**olarak ayarlayın. Ortam değişkenlerini ayarlamanın iki yolu vardır:

Azure portalında:

IoT Hub'ınızda, IoT Edge cihazınızı ve cihaz ayrıntıları sayfasından seçin ve **Modülleri** > Çalıştırma Zamanı Ayarlarını**Ayarla'yı**seçin. Edge Hub modülü için *False*olarak ayarlanmış *OptimizeForPerformance* adlı bir ortam değişkeni oluşturun.

![OptimizeForPerformance yanlış olarak ayarlanmış](./media/troubleshoot/optimizeforperformance-false.png)

**Veya**

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

## <a name="cant-get-the-iot-edge-daemon-logs-on-windows"></a>Windows'daki IoT Edge daemon günlüklerini alamıyorum

Windows'da kullanırken `Get-WinEvent` bir EventLogException alırsanız, kayıt defteri girişlerinizi kontrol edin.

**Kök neden**

`Get-WinEvent` PowerShell komutu, belirli `ProviderName`bir .

**Çözünürlük**

IoT Edge daemon için bir kayıt defteri girişi ayarlayın. Aşağıdaki içeriğe sahip bir **iotedge.reg** dosyası oluşturun ve çift tıklatarak veya komutu `reg import iotedge.reg` kullanarak Windows Kayıt Defteri'ne aktarın:

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="iot-edge-module-fails-to-send-a-message-to-the-edgehub-with-404-error"></a>IoT Edge modülü edgeHub'a 404 hatasıyla mesaj göndermiyor

Özel bir IoT Edge modülü edgeHub'a 404 `Module not found` hatasıyla ileti göndermeyi başaramaz. IoT Edge daemon günlüklere aşağıdaki iletiyi yazdırır:

```output
Error: Time:Thu Jun  4 19:44:58 2018 File:/usr/sdk/src/c/provisioning_client/adapters/hsm_client_http_edge.c Func:on_edge_hsm_http_recv Line:364 executing HTTP request fails, status=404, response_buffer={"message":"Module not found"}u, 04 )
```

**Kök neden**

IoT Edge daemon, güvenlik nedenleriyle edgeHub'a bağlanan tüm modüller için işlem tanımlamasını zorlar. Bir modül tarafından gönderilen tüm iletilerin modülün ana işlem kimliğinden geldiğini doğrular. Bir ileti, bir modül tarafından başlangıçta kurulandan farklı bir işlem kimliğinden gönderiliyorsa, iletiyi 404 hata iletisi ile reddeder.

**Çözünürlük**

Sürüm 1.0.7 itibariyle, tüm modül işlemleri bağlanmaya yetkilidir. 1.0.7'ye yükseltme mümkün değilse, aşağıdaki adımları tamamlayın. Daha fazla bilgi için [1.0.7 sürüm değiştirbiyi](https://github.com/Azure/iotedge/blob/master/CHANGELOG.md#iotedged-1)ne bürünme tarihine bakın.

EdgeHub'a ileti göndermek için aynı işlem kimliğinin her zaman özel IoT Edge modülü tarafından kullanıldığından emin olun. Örneğin, Docker `ENTRYPOINT` dosyanızda `CMD` komut yerine emin olun, çünkü `CMD` modül için bir işlem kimliğine ve ana programı çalıştıran `ENTRYPOINT` bash komutu için başka bir işlem kimliğine yol açarken, tek bir işlem kimliğine yol açacaktır.

## <a name="firewall-and-port-configuration-rules-for-iot-edge-deployment"></a>IoT Edge dağıtımı için Güvenlik Duvarı ve Bağlantı Noktası yapılandırma kuralları

Azure IoT Edge, desteklenen IoT Hub protokollerini kullanarak şirket içi sunucudan Azure bulutuna [iletişim](../iot-hub/iot-hub-devguide-protocols.md)sağlar ve bkz. Gelişmiş güvenlik için, Azure IoT Edge ve Azure IoT Hub arasındaki iletişim kanalları her zaman Giden olacak şekilde yapılandırılır. Bu yapılandırma, kötü amaçlı bir varlığın keşfetmesi için saldırı yüzeyini en aza indiren [Hizmetler Destekli İletişim desenine](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/)dayanır. Gelen iletişim yalnızca Azure IoT Hub'ın iletileri Azure IoT Edge aygıtına iletmesi gereken belirli senaryolar için gereklidir. Buluttan cihaza iletiler güvenli TLS kanalları kullanılarak korunur ve X.509 sertifikaları ve TPM cihaz modülleri kullanılarak daha da güvenli hale getirilebilir. Azure IoT Edge Güvenlik Yöneticisi, bu iletişimin nasıl kurulabileceğini yönetir, [bkz.](../iot-edge/iot-edge-security-manager.md)

IoT Edge, Azure IoT Edge çalışma zamanı ve dağıtılmış modülleri güvence altına almak için gelişmiş yapılandırma sağlarken, yine de temel makine ve ağ yapılandırmasına bağlıdır. Bu nedenle, bulut iletişiminin güvenli kenarı için uygun ağ ve güvenlik duvarı kurallarının ayarlı olduğundan emin olmak zorunludur. Aşağıdaki tablo, Azure IoT Edge çalışma zamanının barındırıldığı temel sunucular için yapılandırma güvenlik duvarı kuralları olduğunda kılavuz olarak kullanılabilir:

|Protokol|Bağlantı noktası|Gelen|Giden|Rehber|
|--|--|--|--|--|
|MQTT|8883|ENGELLENMIS (Varsayılan)|ENGELLENMIS (Varsayılan)|<ul> <li>İletişim protokolü olarak MQTT kullanırken Giden (Giden) Açık olacak şekilde yapılandırın.<li>MQTT için 1883 IoT Edge tarafından desteklenmez. <li>Gelen (Gelen) bağlantılar engellenmelidir.</ul>|
|AMQP|5671|ENGELLENMIS (Varsayılan)|OPEN (Varsayılan)|<ul> <li>IoT Edge için varsayılan iletişim protokolü. <li> Azure IoT Edge diğer desteklenen protokoller için yapılandırılmamışsa veya AMQP istenen iletişim protokolüyse, Açık olarak yapılandırılacak şekilde yapılandırılmalıdır.<li>AMQP için 5672 IoT Edge tarafından desteklenmez.<li>Azure IoT Edge farklı bir IoT Hub desteklenen protokol kullandığında bu bağlantı noktasını engelleyin.<li>Gelen (Gelen) bağlantılar engellenmelidir.</ul></ul>|
|HTTPS|443|ENGELLENMIS (Varsayılan)|OPEN (Varsayılan)|<ul> <li>Giden (Giden) IoT Edge sağlama için 443'te açık olacak şekilde yapılandırın. Bu yapılandırma, el ile komut dosyaları veya Azure IoT Aygıt Sağlama Hizmeti (DPS) kullanırken gereklidir. <li>Gelen (Gelen) bağlantı yalnızca belirli senaryolar için Açık olmalıdır: <ul> <li>  Yöntem istekleri gönderebilecek yaprak aygıtları olan saydam bir ağ geçidiniz varsa. Bu durumda, IoTHub'a bağlanmak veya Azure IoT Edge üzerinden IoTHub hizmetleri sağlamak için Port 443'ün harici ağlara açık olması gerekmez. Böylece gelen kural, iç ağdan yalnızca gelen (Gelen) açık olmasıyla sınırlandırılabilir. <li> İstemciden Aygıta (C2D) senaryolar için.</ul><li>HTTP için 80, IoT Edge tarafından desteklenmez.<li>Kuruluşta HTTP dışı protokoller (örneğin, AMQP veya MQTT) yapılandırılemiyorsa; iletiler WebSockets üzerinden gönderilebilir. Bu durumda Port 443 WebSocket iletişimi için kullanılacaktır.</ul>|

## <a name="edge-agent-module-continually-reports-empty-config-file-and-no-modules-start-on-the-device"></a>Edge Agent modülü sürekli olarak 'boş config dosyası' bildirir ve cihazda modül başlamaz

Aygıt, dağıtımda tanımlanan modülleri başlatmakta sorun yaşıyor. Yalnızca edgeAgent çalışıyor ama sürekli 'boş config dosyası...' bildiriyor.

**Kök neden**

Varsayılan olarak, IoT Edge modülleri kendi yalıtılmış kapsayıcı ağında başlatır. Aygıt bu özel ağ içinde DNS ad çözünürlüğü ile ilgili sorun yaşıyor olabilir.

**Çözünürlük**

**Seçenek 1: DNS sunucusunun konteyner motoru ayarlarını ayarlaması**

Motor tarafından başlatılan tüm konteyner modülleri için geçerli olacak kapsayıcı motoru ayarlarında ortamınız için DNS sunucusunu belirtin. Kullanılacak DNS sunucusunu belirten adlandırılmış `daemon.json` bir dosya oluşturun. Örneğin:

```json
{
    "dns": ["1.1.1.1"]
}
```

Yukarıdaki örnekte, DNS sunucusu nu herkese açık bir DNS hizmeti olarak ayarlar. Kenar aygıtı bu IP'ye ortamından erişemiyorsa, erişilebilir DNS sunucu adresiyle değiştirin.

Platformunuz için doğru konuma yerleştirin: `daemon.json`

| Platform | Konum |
| --------- | -------- |
| Linux | `/etc/docker` |
| Windows kapsayıcıları ile Windows ana bilgisayar | `C:\ProgramData\iotedge-moby\config` |

Konum zaten dosya `daemon.json` içeriyorsa, **dns** tuşunu ekleyin ve dosyayı kaydedin.

Güncelleştirmelerin etkili olması için kapsayıcı altyapısını yeniden başlatın.

| Platform | Komut |
| --------- | -------- |
| Linux | `sudo systemctl restart docker` |
| Windows (Yönetici Powershell) | `Restart-Service iotedge-moby -Force` |

**Seçenek 2: Modül başına IoT Edge dağıtımında DNS sunucusu ayarlama**

IoT Edge dağıtımında her modülün *oluşturma Seçenekleri* için DNS sunucusu ayarlayabilirsiniz. Örneğin:

```json
"createOptions": {
  "HostConfig": {
    "Dns": [
      "x.x.x.x"
    ]
  }
}
```

EdgeAgent ve *edgeHub* modülleri *edgeHub* için de bu yapılandırmayı ayarladıklarından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge platformunda bir hata bulduğunuzu düşünüyor musunuz? Geliştirmeye devam edebilmemiz için [bir sorun gönderin.](https://github.com/Azure/iotedge/issues)

Daha fazla sorunuz varsa, yardım için bir [Destek isteği](https://portal.azure.com/#create/Microsoft.Support) oluşturun.
