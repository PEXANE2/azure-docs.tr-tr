---
title: Sık karşılaşılan hatalar-Azure IoT Edge | Microsoft Docs
description: IoT Edge çözümü dağıtımında karşılaşılan yaygın sorunları çözümlemek için bu makaleyi kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/27/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: ed93d24bc06a6622a8ace2b0ab6b44582da001c0
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82783753"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Azure IoT Edge için genel sorunlar ve çözümler

IoT Edge çözümlerini dağıttığınızda karşılaşabileceğiniz yaygın sorunları gidermeye yönelik adımları bulmak için bu makaleyi kullanın. IoT Edge cihazınızdan günlükleri ve hataları bulma hakkında bilgi almanız gerekiyorsa, bkz. [IoT Edge cihazınızda sorun giderme](troubleshoot.md).

## <a name="iot-edge-agent-stops-after-about-a-minute"></a>IoT Edge aracı bir dakika sonra durdu

**Gözlemlenen davranış:**

EdgeAgent modülü yaklaşık bir dakika boyunca başarıyla başlatılır ve çalışır ve sonra duraklar. Günlükler IoT Edge aracısının AMQP üzerinden IoT Hub bağlanmaya çalıştığı ve sonra da WebSocket üzerinden AMQP kullanarak bağlanmaya çalıştığı anlamına gelebilir. Bu başarısız olduğunda IoT Edge Aracısı çıkar.

Örnek edgeAgent günlükleri:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent.
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5)
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP...
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket...
```

**Kök nedeni:**

Konak ağındaki bir ağ yapılandırması, IoT Edge aracısının ağa ulaşmasını engellemektedir. Aracı ilk olarak AMQP (5671 numaralı bağlantı noktası) üzerinden bağlanma girişiminde bulunur. Bağlantı başarısız olursa, WebSockets (bağlantı noktası 443) çalışır.

IoT Edge çalışma zamanı, her bir modül için iletişim kurulacak bir ağ ayarlar. Linux’ta bu ağ bir köprü ağıdır. Windows’da NAT kullanır. Bu sorun, NAT ağını kullanan Windows kapsayıcılarının kullanıldığı Windows cihazlarında daha yaygın olarak görülür.

**Çözünürlüğüne**

Bu köprüye/NAT ağına atanan IP adresleri için bir İnternet rotası olduğundan emin olun. Bazen konaktaki VPN yapılandırması, IoT Edge ağını geçersiz kılar.

## <a name="iot-edge-agent-cant-access-a-modules-image-403"></a>IoT Edge Aracısı bir modülün görüntüsüne erişemiyor (403)

**Gözlemlenen davranış:**

Bir kapsayıcı çalıştırılamaz ve edgeAgent günlükleri bir 403 hatası gösterir.

**Kök nedeni:**

IoT Edge aracısının bir modülün görüntüsüne erişme izni yok.

**Çözünürlüğüne**

Kayıt defteri kimlik bilgilerinizin dağıtım bildiriminizde doğru belirtildiğinden emin olun.

## <a name="edge-agent-module-reports-empty-config-file-and-no-modules-start-on-the-device"></a>Edge Aracısı modülü ' boş yapılandırma dosyası ' bildiriyor ve cihazda hiçbir modül başlamıyor

**Gözlemlenen davranış:**

Cihazda dağıtımda tanımlanan modüller başlatılırken sorun yaşanıyor. Yalnızca edgeAgent çalışıyor ancak sürekli olarak ' boş yapılandırma dosyası... ' bildiriyor.

**Kök nedeni:**

Varsayılan olarak, IoT Edge kendi yalıtılmış kapsayıcı ağında modüller başlatır. Bu özel ağ içinde, bu cihaz DNS ad çözümlemesi ile ilgili sorun yaşıyor olabilir.

**Çözünürlüğüne**

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

## <a name="iot-edge-hub-fails-to-start"></a>IoT Edge hub 'ı başlatılamıyor

**Gözlemlenen davranış:**

EdgeHub modülü başlatılamıyor. Günlüklerde aşağıdaki hatalardan biri gibi bir ileti görebilirsiniz:

```output
One or more errors occurred.
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80):
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n)
```

Veya

```output
info: edgelet_docker::runtime -- Starting module edgeHub...
warn: edgelet_utils::logging -- Could not start module edgeHub
warn: edgelet_utils::logging --     caused by: failed to create endpoint edgeHub on network nat: hnsCall failed in Win32:  
        The process cannot access the file because it is being used by another process. (0x20)
```

**Kök nedeni:**

Ana makinedeki diğer bazı işlemler, edgeHub modülünün bağlamaya çalıştığı bir bağlantı noktasını bağlamıştır. IoT Edge hub, ağ geçidi senaryolarında kullanılmak üzere 443, 5671 ve 8883 bağlantı noktalarını eşler. Başka bir işlem bu bağlantı noktalarından birini zaten bağlamışsa modül başlayamaz.

**Çözünürlüğüne**

Bu sorunu iki şekilde çözebilirsiniz:

IoT Edge cihaz bir ağ geçidi cihazı olarak çalışıyorsa, 443, 5671 veya 8883 numaralı bağlantı noktasını kullanan işlemi bulup durdurmanız gerekir. Bağlantı noktası 443 hatası genellikle diğer işlemin bir Web sunucusu olduğu anlamına gelir.

IoT Edge cihazını bir ağ geçidi olarak kullanmanız gerekmiyorsa, edgeHub 'ın modül oluşturma seçenekleri ' nden bağlantı noktası bağlamalarını kaldırabilirsiniz. Azure portal oluşturma seçeneklerini veya doğrudan Deployment. json dosyasını değiştirebilirsiniz.

Azure portalında:

1. IoT Hub 'ınıza gidip **IoT Edge**seçin.

2. Güncelleştirmek istediğiniz IoT Edge cihazı seçin.

3. **Modülleri Ayarlama**'yı seçin.

4. **Çalışma zamanı ayarları**' nı seçin.

5. **Edge hub 'ı** modül ayarları ' nda, **oluşturma seçenekleri** metin kutusundan her şeyi silin.

6. Değişikliklerinizi kaydedin ve dağıtımı oluşturun.

Deployment. json dosyasında:

1. IoT Edge cihazınıza uyguladığınız Deployment. json dosyasını açın.

2. EdgeAgent istenen Özellikler bölümünde `edgeHub` ayarları bulun:

   ```json
   "edgeHub": {
       "settings": {
           "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
           "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
       },
       "type": "docker",
       "status": "running",
       "restartPolicy": "always"
   }
   ```

3. `createOptions` Satırı ve önünde `image` satırın sonundaki sondaki virgülü kaldırın:

   ```json
   "edgeHub": {
       "settings": {
           "image": "mcr.microsoft.com/azureiotedge-hub:1.0"
       },
       "type": "docker",
       "status": "running",
       "restartPolicy": "always"
   }
   ```

4. Dosyayı kaydedin ve IoT Edge cihazınıza yeniden uygulayın.

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>IoT Edge güvenlik arka plan programı geçersiz bir ana bilgisayar adıyla başarısız oluyor

**Gözlemlenen davranış:**

[IoT Edge Güvenlik Yöneticisi günlüklerini denetlemeye](troubleshoot.md#check-the-status-of-the-iot-edge-security-manager-and-its-logs) çalışmak başarısız olur ve şu iletiyi yazdırır:

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

**Kök nedeni:**

IoT Edge çalışma zamanı yalnızca 64 karakterden kısa olan ana bilgisayar adlarını destekleyebilir. Fiziksel makinelerde genellikle uzun ana bilgisayar adları yoktur, ancak sorun bir sanal makinede daha yaygındır. Azure 'da barındırılan Windows sanal makineleri için otomatik olarak oluşturulan ana bilgisayar adları, özellikle de uzun olur.

**Çözünürlüğüne**

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

## <a name="cant-get-the-iot-edge-daemon-logs-on-windows"></a>Windows 'da IoT Edge Daemon günlükleri alınamıyor

**Gözlemlenen davranış:**

Windows 'da kullanırken `Get-WinEvent` bir EventLogException alırsınız.

**Kök nedeni:**

PowerShell `Get-WinEvent` komutu, belirli `ProviderName`bir günlük bulmak için bir kayıt defteri girdisini temel alır.

**Çözünürlüğüne**

IoT Edge Daemon için bir kayıt defteri girişi ayarlayın. Aşağıdaki içerikle bir **ıotedge. reg** dosyası oluşturun ve çift tıklayarak veya `reg import iotedge.reg` komutunu kullanarak Windows kayıt defterine aktarın:

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="stability-issues-on-smaller-devices"></a>Daha küçük cihazlarda kararlılık sorunları

**Gözlemlenen davranış:**

Özellikle bir ağ geçidi olarak kullanıldığında Raspberry PI gibi kaynak kısıtlı cihazlarda kararlılık sorunlarıyla karşılaşabilirsiniz. Belirtiler IoT Edge hub modülündeki bellek dışında özel durumları, bağlantı başarısız olan akış cihazlarını veya cihazın birkaç saatten sonra telemetri iletileri gönderememesi gerektiğini içerir.

**Kök nedeni:**

IoT Edge çalışma zamanının parçası olan IoT Edge hub, varsayılan olarak performans için en iyi duruma getirilmiştir ve büyük bellek öbeklerini ayırmaya çalışır. Bu iyileştirme kısıtlı sınır cihazları için ideal değildir ve kararlılık sorunlarına neden olabilir.

**Çözünürlüğüne**

IoT Edge hub 'ı için **Optimizeforperformance** ortam değişkenini **false**olarak ayarlayın. Ortam değişkenlerini ayarlamak için iki yol vardır:

Azure portalında:

IoT Hub, IoT Edge cihazınızı ve cihaz ayrıntıları sayfasında, **Modül** > **çalışma zamanı ayarlarını**ayarla ' yı seçin. *Yanlış*olarak ayarlanan *Optimizeforperformance* adlı IoT Edge hub modülü için bir ortam değişkeni oluşturun.

![OptimizeForPerformance, false olarak ayarlandı](./media/troubleshoot/optimizeforperformance-false.png)

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

## <a name="iot-edge-module-fails-to-send-a-message-to-edgehub-with-404-error"></a>IoT Edge modülü, 404 hatasıyla edgeHub 'a ileti gönderemediğinde

**Gözlemlenen davranış:**

Özel bir IoT Edge modülü, bir 404 `Module not found` hatası ile IoT Edge hub 'ına ileti gönderemediğinde. IoT Edge Daemon, günlüklere aşağıdaki iletiyi yazdırır:

```output
Error: Time:Thu Jun  4 19:44:58 2018 File:/usr/sdk/src/c/provisioning_client/adapters/hsm_client_http_edge.c Func:on_edge_hsm_http_recv Line:364 executing HTTP request fails, status=404, response_buffer={"message":"Module not found"}u, 04 )
```

**Kök nedeni:**

IoT Edge Daemon, güvenlik nedenleriyle edgeHub 'a bağlanan tüm modüller için işlem tanımlamayı zorlar. Modül tarafından gönderilen tüm iletilerin, modülün ana işlem KIMLIĞINDEN geldiğini doğrular. Bir ileti, başlangıçta kurulduğu farklı bir işlem KIMLIĞINDEN bir modül tarafından gönderiliyorsa, iletiyi 404 hata iletisiyle reddeder.

**Çözünürlüğüne**

Sürüm 1.0.7 itibariyle, tüm modül işlemlerinin bağlanmasına izin verilir. Daha fazla bilgi için bkz. [1.0.7 Release changelog](https://github.com/Azure/iotedge/blob/master/CHANGELOG.md#iotedged-1).

1.0.7 sürümüne yükseltme yapılamıyorsa, aşağıdaki adımları izleyin. Aynı işlem KIMLIĞININ, her zaman özel IoT Edge modülü tarafından edgeHub 'a ileti göndermek için kullanıldığından emin olun. Örneğin, Docker dosyanızda `ENTRYPOINT` `CMD` komutunu kullandığınızdan emin olun. `CMD` Komut, modül için BIR işlem kimliği ve ana programı çalıştıran Bash komutu için başka BIR işlem kimliği sağlar, ancak `ENTRYPOINT` tek bir işlem kimliğine yol açar.

## <a name="iot-edge-module-deploys-successfully-then-disappears-from-device"></a>IoT Edge modül başarıyla dağıtılır ve sonra cihazdan kaybolur

**Gözlemlenen davranış:**

Bir IoT Edge cihaz için modüller ayarladıktan sonra, modüller başarıyla dağıtılır, ancak cihazdan ve Azure portal cihaz ayrıntılarından kaybolur. Tanımlandıklardan başka modüller de cihazda görünebilir.

**Kök nedeni:**

Otomatik dağıtım, bir cihazı hedefliyorsa, tek bir cihaz için modülleri el ile ayarlayarak öncelik kazanır. Visual Studio Code tek bir cihaz işlevselliği için Azure portal veya **dağıtım oluşturma** Içindeki **modülleri ayarlama** işlevselliği bir süre içinde geçerli olacaktır. Cihazda başlangıç olarak tanımladığınız modülleri görürsünüz. Sonra otomatik dağıtımın öncelikli noktaları, cihazın istenen özelliklerinin üzerine yazar.

**Çözünürlüğüne**

Her cihaz için bir otomatik dağıtım veya tek cihaz dağıtımı olmak üzere yalnızca bir tür dağıtım mekanizması kullanın. Bir cihazı hedefleyen birden fazla otomatik dağıtımınız varsa, belirli bir cihaza doğru bir uygulama olduğundan emin olmak için öncelik veya hedef açıklamalarını değiştirebilirsiniz. Ayrıca, ikizi cihazını otomatik dağıtımın hedef açıklamasıyla artık eşleşmesiz şekilde güncelleştirebilirsiniz.

Daha fazla bilgi için bkz. [tek cihazlarda veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge platformunda bir hata bulduğunuzu düşünüyor musunuz? İyileşmeye devam edebilmemiz için [bir sorun gönderin](https://github.com/Azure/iotedge/issues) .

Daha fazla sorunuz varsa yardım için bir [destek isteği](https://portal.azure.com/#create/Microsoft.Support) oluşturun.
