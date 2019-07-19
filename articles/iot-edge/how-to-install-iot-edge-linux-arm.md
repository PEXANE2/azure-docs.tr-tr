---
title: Linux 'ta Azure IoT Edge 'yi ARM32 | Microsoft Docs
description: ARM32 cihazlarda Linux 'ta Raspberry PI gibi yükleme yönergeleri Azure IoT Edge
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: kgremban
ms.openlocfilehash: 72349597a2831a64cb11ff66b30e6948f34cc921
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68224704"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Linux 'ta Azure IoT Edge çalışma zamanı 'nı (ARM32v7/armhf) yükler

Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz. 

IOT Edge çalışma zamanı nasıl çalıştığını ve hangi bileşenler dahildir hakkında daha fazla bilgi için bkz: [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md).

Bu makalede bir Linux ARM32v7/armhf IoT Edge cihazına Azure IoT Edge çalışma zamanını yüklemek için gereken adımlar listelenmektedir. Örneğin, bu adımlar Raspberry PI cihazlarda çalışır. Desteklenen ARM32 işletim sistemlerinin bir listesi için bkz. [Azure IoT Edge desteklenen sistemler](support.md#operating-systems). 

>[!NOTE]
>Linux yazılım depoları paketlerinde her pakette yer alan lisans koşullarına tabidir (/ usr/paylaşım/doc/*paket adı*). Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

## <a name="install-the-latest-version"></a>En son sürümü yükler

Linux ARM cihazlarınıza Azure IoT Edge hizmetinin en son sürümünü yüklemek için aşağıdaki bölümleri kullanın. 

### <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

Azure IoT Edge, [OCI uyumlu](https://www.opencontainers.org/) bir kapsayıcı çalışma zamanına bağlıdır. Üretim senaryoları için kullanmanız önerilir [Moby tabanlı](https://mobyproject.org/) aşağıda sağlanan altyapısı. Bu, Azure IOT Edge ile resmi olarak desteklenen tek kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntüleri, Moby tabanlı çalışma zamanına göre uyumludur.

Aşağıdaki komutlar hem Moby tabanlı altyapıyı hem de komut satırı arabirimini (CLı) yükler. CLI, geliştirme için kullanışlıdır ancak üretim dağıtımları için isteğe bağlı değildir.

```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f
```

### <a name="install-the-iot-edge-security-daemon"></a>IoT Edge güvenlik cini 'nı yükler

**IoT Edge güvenlik arka plan programı** , IoT Edge cihazında güvenlik standartları sağlar ve korur. Arka plan programı, her önyükleme başlar ve cihazın IOT Edge çalışma zamanı geri kalanını başlatarak bootstraps. 


```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```

IoT Edge başarıyla yüklendikten sonra, çıkış sizden yapılandırma dosyasını güncelleştirmenizi ister. [Azure IoT Edge güvenlik cini yapılandırma](#configure-the-azure-iot-edge-security-daemon) bölümündeki adımları izleyerek cihazınızı sağlamayı tamamlayın. 

## <a name="install-a-specific-version"></a>Belirli bir sürümü yükler

Azure IoT Edge belirli bir sürümünü yüklemek istiyorsanız, bileşen dosyalarını doğrudan IoT Edge GitHub deposundan hedefleyebilirsiniz. Önceki bölümlerde listelenen komutları kullanarak tüm IoT Edge bileşenlerini cihazlarınıza alın: Moby Engine ve CLI, libiothsm ve son olarak IoT Edge güvenlik arka plan programı. `curl` Tek fark, **aka.MS** URL 'lerini doğrudan, kullanmak istediğiniz her bileşenin sürümüne işaret eden bağlantılarla değiştirme.

[Azure IoT Edge yayınlarına](https://github.com/Azure/azure-iotedge/releases)gidin ve hedeflemek istediğiniz yayın sürümünü bulun. Sürüm için **varlıklar** bölümünü genişletin ve IoT Edge cihazınızın mimarisiyle eşleşen dosyaları seçin. Her IoT Edge sürümü **ıotedge** ve **libiothsm** dosyalarını içerir. Tüm yayınlar **Moby-Engine** veya **Moby-CLI**içermez. Moby kapsayıcı altyapınız zaten yüklü değilse, Moby bileşenlerini içeren bir tane bulana kadar eski sürümlere göz atın. 

IoT Edge başarıyla yüklendikten sonra, çıkış sizden yapılandırma dosyasını güncelleştirmenizi ister. Cihazınızın sağlanmasına son vermek için sonraki bölümde yer alarak bulunan adımları izleyin. 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Azure IoT Edge güvenlik arka plan programını yapılandırma

Azure IOT hub'ı var olan bir cihaz kimliği ile fiziksel Cihazınızı bağlamak için IOT Edge çalışma zamanı yapılandırın. 

Arka plan programı, yapılandırma dosyası kullanılarak yapılandırılabilir `/etc/iotedge/config.yaml`. Dosya varsayılan olarak yazma korumalı olduğundan, onu düzenlemek için yükseltilmiş izinlere sahip olmanız gerekebilir.

Tek bir IOT Edge cihazı IOT Hub tarafından sağlanan cihaz bağlantı dizesini kullanarak el ile sağlanabilir. Veya, cihaz sağlama hizmeti sağlamak için birçok cihaz olduğunda kullanışlı olan cihazları otomatik olarak sağlamak için kullanabilirsiniz. Sağlama seçiminize bağlı olarak, uygun yükleme komut dosyasını seçin. 

### <a name="option-1-manual-provisioning"></a>Seçenek 1: El ile sağlama

Bir cihazı el ile sağlamak için, IoT Hub 'ınıza yeni bir IoT Edge cihaz kaydederek oluşturabileceğiniz bir [Cihaz bağlantı dizesi](how-to-register-device-portal.md) sağlamanız gerekir.

Yapılandırma dosyasını açın. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Dosyanın sağlama yapılandırmalarını bulun ve **el ile sağlama yapılandırma** bölümünün açıklamasını kaldırın. Değerini güncelleştirin **device_connection_string** IOT Edge cihazınızdan bağlantı dizesiyle. Diğer sağlama bölümlerinin açıklama olarak belirlendiğinden emin olun.

```yaml
# Manual provisioning configuration
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
# DPS TPM provisioning configuration
# provisioning:
#   source: "dps"
#   global_endpoint: "https://global.azure-devices-provisioning.net"
#   scope_id: "{scope_id}"
#   attestation:
#     method: "tpm"
#     registration_id: "{registration_id}"
```

Dosyayı kaydedin ve kapatın. 

`CTRL + X`, `Y`, `Enter`

Yapılandırma dosyasında sağlama bilgilerini girdikten sonra Daemon programını yeniden başlatın:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Seçenek 2: Otomatik sağlama

Otomatik olarak bir cihazı sağlamak için [cihaz sağlama hizmetini ayarlama ve cihaz kayıt Kimliğinizi almak](how-to-auto-provision-simulated-device-linux.md). Otomatik sağlama kullanılırken IoT Edge tarafından desteklenen çeşitli kanıtlama mekanizmaları vardır ancak donanım gereksinimleriniz de seçimlerinizi etkiler. Örneğin, Raspberry PI cihazları varsayılan olarak Güvenilir Platform Modülü (TPM) yongasıyla birlikte gelmiyor.

Yapılandırma dosyasını açın. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Dosyanın sağlama yapılandırmasını bulun ve kanıtlama mekanizmanız için uygun bölümün açıklamasını kaldırın. Örneğin, TPM kanıtlama kullanırken, **scope_id** ve **Registration_id** değerlerini IoT Hub cihaz sağlama hizmetinizdeki değerlerle ve IoT Edge cihazınızdan sırasıyla TPM ile güncelleştirin.

```yaml
   # Manual provisioning configuration
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # DPS TPM provisioning configuration
   provisioning:
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     attestation:
       method: "tpm"
       registration_id: "{registration_id}"
```

Dosyayı kaydedin ve kapatın. 

`CTRL + X`, `Y`, `Enter`

Yapılandırma dosyasında sağlama bilgilerini girdikten sonra Daemon programını yeniden başlatın:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

Kullandıysanız **el ile yapılandırma** adımlar önceki bölümde, IOT Edge çalışma zamanı başarıyla sağlanmış ve cihazınız üzerinde olmalıdır. Ya da **otomatik yapılandırma** adımlarını kullandıysanız, çalışma zamanının cihazınızı sizin adınıza IoT Hub 'ınıza kaydedebilmesi için bazı ek adımlar gerçekleştirmeniz gerekir. Sonraki adımlar için bkz. [Linux sanal makinesi üzerinde sanal BIR TPM IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

IOT Edge arka plan programı kullanarak durumu denetleyebilirsiniz:

```bash
systemctl status iotedge
```

Kullanarak arka plan programının günlüklerini inceleyin:

```bash
journalctl -u iotedge --no-pager --no-full
```

Ve modüller ile çalışan listesi:

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>İpuçları ve öneriler

`iotedge` komutlarını çalıştırmak için yükseltilmiş ayrıcalıklara ihtiyacınız olacaktır. Çalışma zamanını yükledikten sonra makinenizin dışında oturum ve izinlerinizi otomatik olarak güncelleştirmek için yeniden oturum açın. O zamana kadar kullanın **sudo** herhangi önünde `iotedge` komutları.

Kısıtlanmış bir kaynak cihazlarda ayarlamanız önerilir *OptimizeForPerformance* ortam değişkenine *false* yönergeleri uyarınca [sorun giderme kılavuzu ](troubleshoot.md#stability-issues-on-resource-constrained-devices).

Bir proxy sunucusu varsa, ağ adımları izlerseniz [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge Cihazınızı yapılandırma](how-to-configure-proxy-support.md).

## <a name="uninstall-iot-edge"></a>IoT Edge kaldır

Linux cihazınızdan IoT Edge yüklemesini kaldırmak istiyorsanız, komut satırından aşağıdaki komutları kullanın. 

IoT Edge çalışma zamanını kaldırın. 

```bash
sudo apt-get remove --purge iotedge
```

IoT Edge çalışma zamanı kaldırıldığında, oluşturduğu kapsayıcı durdurulur ancak cihazınızda hala bulunur. Hangi olanların kaldığını görmek için tüm kapsayıcıları görüntüleyin. 

```bash
sudo docker ps -a
```

İki çalışma zamanı kapsayıcısı dahil olmak üzere cihazınızdaki kapsayıcıları silin. 

```bash
sudo docker rm -f <container name>
```

Son olarak, kapsayıcı çalışma zamanını cihazınızdan kaldırın. 

```bash 
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Sonraki adımlar

Yüklü olan çalışma zamanı ile sağlanan bir IOT Edge cihazına sahip olduğunuza göre şunları yapabilirsiniz [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md).

IoT Edge çalışma zamanının düzgün şekilde yüklenmesiyle ilgili sorun yaşıyorsanız, [sorun giderme](troubleshoot.md#stability-issues-on-resource-constrained-devices) sayfasına bakın.

Mevcut bir yüklemeyi en yeni IoT Edge sürümüne güncelleştirmek için bkz. [IoT Edge güvenlik cini ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).
