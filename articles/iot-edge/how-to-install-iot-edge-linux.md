---
title: Azure IOT Edge Linux'ta yükleme | Microsoft Docs
description: Ubuntu veya Raspbian çalıştıran Linux cihazlarda yükleme yönergelerini Azure IoT Edge
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: bb23ee1e51be178f93e05b728f7b8c2e9bb18e0d
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414482"
---
# <a name="install-the-azure-iot-edge-runtime-on-debian-based-linux-systems"></a>Azure IoT Edge çalışma zamanını, detem tabanlı Linux sistemlerine yükler

Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure IoT Edge çalışma zamanını ve mimarisini anlayın](iot-edge-runtime.md).

Bu makalede, Azure IoT Edge çalışma zamanını x64, ARM32 veya ARM64 Linux cihazına yüklemek için gereken adımlar listelenmektedir. Ubuntu Server 16,04, Ubuntu Server 18,04 ve Raspbian Esnetme için yükleme paketleri sağlanır. Desteklenen Linux işletim sistemleri ve mimarilerin bir listesi için [desteklenen Azure IoT Edge sistemleri](support.md#operating-systems) bölümüne bakın.

>[!NOTE]
>ARM64 cihazlar için destek [genel önizlemede](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

> [!NOTE]
> Linux yazılım depoları paketlerinde her pakette yer alan lisans koşullarına tabidir (/ usr/paylaşım/doc/*paket adı*). Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

## <a name="install-the-latest-runtime-version"></a>En son çalışma zamanı sürümünü yükler

Azure IoT Edge çalışma zamanının en son sürümünü cihazınıza yüklemek için aşağıdaki bölümleri kullanın. 

### <a name="register-microsoft-key-and-software-repository-feed"></a>Microsoft anahtar ve yazılım deposu akışı kaydedin

Cihazınızı IoT Edge çalışma zamanı yüklemesi için hazırlayın.

Depo yapılandırmasını yükler. Cihaz işletim sisteminizle eşleşen **16,04** veya **18,04** komutunu seçin:

* **Ubuntu Server 16,04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/multiarch/prod.list > ./microsoft-prod.list
   ```

* **Ubuntu Server 18,04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
   ```

* **Raspbian Esnetme**:
   ```bash
   curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
   ```

Oluşturulan listeyi kopyalayın.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Microsoft GPG ortak anahtarını yükler

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

Azure IoT Edge, [OCI uyumlu](https://www.opencontainers.org/) bir kapsayıcı çalışma zamanına bağlıdır. Üretim senaryolarında aşağıda belirtilen [Moby tabanlı](https://mobyproject.org/) altyapıyı kullanmanızı öneririz. Bu, Azure IOT Edge ile resmi olarak desteklenen tek kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntülerini Moby çalışma zamanı ile uyumludur.

Apt güncelleştirmesi gerçekleştirin.

   ```bash
   sudo apt-get update
   ```

Moby Altyapısı'nı yükleyin.

   ```bash
   sudo apt-get install moby-engine
   ```

Moby komut satırı arabirimi (CLI) yükleyin. CLI, geliştirme için kullanışlıdır ancak üretim dağıtımları için isteğe bağlı değildir.

   ```bash
   sudo apt-get install moby-cli
   ```

Moby kapsayıcı çalışma zamanını yüklerken hatalarla karşılaşırsanız, bu makalede daha sonra sağlanmış olan [Linux çekirdeğini Moby uyumluluğu Için doğrulama](#verify-your-linux-kernel-for-moby-compatibility)adımlarını izleyin. 

### <a name="install-the-azure-iot-edge-security-daemon"></a>Azure IOT Edge güvenlik Daemon'ı yükleme

**IoT Edge güvenlik arka plan programı** , IoT Edge cihazında güvenlik standartları sağlar ve korur. Arka plan programı, her önyükleme başlar ve cihazın IOT Edge çalışma zamanı geri kalanını başlatarak bootstraps.

Yükleme komutu, zaten mevcut değilse **libiothsm** 'nin standart sürümünü de yüklüyor.

Apt güncelleştirmesi gerçekleştirin.

   ```bash
   sudo apt-get update
   ```

Güvenlik daemon'ı yükleyin. Paket yüklü `/etc/iotedge/`.

   ```bash
   sudo apt-get install iotedge
   ```

IoT Edge başarıyla yüklendikten sonra, çıkış sizden yapılandırma dosyasını güncelleştirmenizi ister. [Azure IoT Edge güvenlik cini yapılandırma](#configure-the-security-daemon) bölümündeki adımları izleyerek cihazınızı sağlamayı tamamlayın. 

## <a name="install-a-specific-runtime-version"></a>Belirli bir çalışma zamanı sürümünü yükler

Azure IoT Edge çalışma zamanının belirli bir sürümünü yüklemek istiyorsanız, bileşen dosyalarını doğrudan IoT Edge GitHub deposundan hedefleyebilirsiniz. Cihazlarınızın tüm IoT Edge bileşenlerini almak için aşağıdaki adımları kullanın: Moby Engine ve CLı, libiothsm ve son olarak IoT Edge güvenlik arka plan programı.

1. [Azure IoT Edge yayınlarına](https://github.com/Azure/azure-iotedge/releases)gidin ve hedeflemek istediğiniz yayın sürümünü bulun. 

2. Bu sürümün **varlıklar** bölümünü genişletin.

3. Verilen herhangi bir yayında Moby altyapısına yönelik güncelleştirmeler olabilir veya olmayabilir. **Moby-Engine** ve **Moby-CLI**ile başlayan dosyaları görürseniz, bu bileşenleri güncelleştirmek için aşağıdaki komutları kullanın. Herhangi bir Moby dosyası görmüyorsanız, en son sürümü bulana kadar eski sürüm varlıkları aracılığıyla geri dönün. 

   1. IoT Edge cihazınızın mimarisiyle eşleşen **Moby-Engine** dosyasını bulun. Dosya bağlantısına sağ tıklayıp bağlantı adresini kopyalayın.

   2. Moby altyapısının bu sürümünü yüklemek için aşağıdaki komutta bulunan kopyalanmış bağlantıyı kullanın: 

      ```bash
      curl -L <moby-engine link> -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb
      ```

   3. IoT Edge cihazınızın mimarisiyle eşleşen **Moby-CLI** dosyasını bulun. Moby CLı isteğe bağlı bir bileşendir, ancak geliştirme sırasında yararlı olabilir. Dosya bağlantısına sağ tıklayıp bağlantı adresini kopyalayın. 

   4. Moby CLı 'nın bu sürümünü yüklemek için aşağıdaki komutta bulunan kopyalanmış bağlantıyı kullanın: 

      ```bash
      curl -L <moby-cli link> -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb
      ```

4. Her yayında IoT Edge güvenlik Daemon ve hsmlib için yeni dosyalar olmalıdır. Bu bileşenleri güncelleştirmek için aşağıdaki komutları kullanın. 

   1. IoT Edge cihazınızın mimarisiyle eşleşen **libiothsm-STD** dosyasını bulun. Dosya bağlantısına sağ tıklayıp bağlantı adresini kopyalayın. 

   2. Hsmlib 'in bu sürümünü yüklemek için aşağıdaki komutta bulunan kopyalanmış bağlantıyı kullanın:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```
   
   3. IoT Edge cihazınızın mimarisiyle eşleşen **iotedge** dosyasını bulun. Dosya bağlantısına sağ tıklayıp bağlantı adresini kopyalayın. 

   4. IoT Edge güvenlik arka plan programının bu sürümünü yüklemek için aşağıdaki komutta bulunan kopyalanmış bağlantıyı kullanın. 

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

IoT Edge başarıyla yüklendikten sonra, çıkış sizden yapılandırma dosyasını güncelleştirmenizi ister. Cihazınızın sağlanmasına son vermek için sonraki bölümde yer alarak bulunan adımları izleyin. 

## <a name="configure-the-security-daemon"></a>Güvenlik cini yapılandırma

Azure IOT hub'ı var olan bir cihaz kimliği ile fiziksel Cihazınızı bağlamak için IOT Edge çalışma zamanı yapılandırın.

Arka plan programı, yapılandırma dosyası kullanılarak yapılandırılabilir `/etc/iotedge/config.yaml`. Dosya yazma korumalı varsayılan olarak, düzenlemek için yükseltilmiş izinlere ihtiyaç duyabilirsiniz.

Tek bir IOT Edge cihazı IOT Hub tarafından sağlanan cihaz bağlantı dizesini kullanarak el ile sağlanabilir. Veya, cihaz sağlama hizmeti sağlamak için birçok cihaz olduğunda kullanışlı olan cihazları otomatik olarak sağlamak için kullanabilirsiniz. Sağlama seçiminize bağlı olarak, uygun yükleme komut dosyasını seçin.

### <a name="option-1-manual-provisioning"></a>Seçenek 1: El ile sağlama

El ile cihaz sağlama için ile sağlamak gereken bir [cihaz bağlantı dizesini](how-to-register-device-portal.md) IOT hub'ına yeni bir cihazı kaydederek oluşturabilirsiniz.

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

Kullandıysanız **el ile yapılandırma** adımlar önceki bölümde, IOT Edge çalışma zamanı başarıyla sağlanmış ve cihazınız üzerinde olmalıdır. Kullandıysanız **otomatik yapılandırma** adımları çalışma zamanı ile sizin adınıza IOT hub'ınıza Cihazınızı kaydetmek için bazı ek adımları tamamlaması gerekir. Sonraki adımlar için bkz. [Linux sanal makinesi üzerinde sanal BIR TPM IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

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

## <a name="tips-and-troubleshooting"></a>İpuçları ve sorun giderme

`iotedge` komutlarını çalıştırmak için yükseltilmiş ayrıcalıklara ihtiyacınız olacaktır. Çalışma zamanını yükledikten sonra makinenizin dışında oturum ve izinlerinizi otomatik olarak güncelleştirmek için yeniden oturum açın. O zamana kadar kullanın **sudo** herhangi önünde `iotedge` komutları.

Kısıtlanmış bir kaynak cihazlarda ayarlamanız önerilir *OptimizeForPerformance* ortam değişkenine *false* yönergeleri uyarınca [sorun giderme kılavuzu ](troubleshoot.md).

Bir proxy sunucusu varsa, ağ adımları izlerseniz [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge Cihazınızı yapılandırma](how-to-configure-proxy-support.md).

### <a name="verify-your-linux-kernel-for-moby-compatibility"></a>Moby uyumluluğu için Linux çekirdeğini doğrulama

Birçok katıştırılmış cihaz üreticisi, kapsayıcı çalışma zamanı uyumluluğu için gerekli özellikler olmadan özel Linux çekirdekler içeren cihaz görüntülerini sunar. Önerilen Moby kapsayıcı çalışma zamanını yüklerken sorunlarla karşılaşırsanız, resmi [Moby GitHub deposundan](https://github.com/moby/moby) [Check-config](https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh) betiğini kullanarak Linux çekirdek yapılandırmanızda sorun giderebilirsiniz. Çekirdek yapılandırmanızı denetlemek için cihazda aşağıdaki komutları çalıştırın:

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

Bu işlem, Moby çalışma zamanı tarafından kullanılan çekirdek özelliklerinin durumunu içeren ayrıntılı bir çıktı sağlar. Çekirdeğin, Moby çalışma zamanına tamamen uyumlu olduğundan `Generally Necessary` emin `Network Drivers` olmak için, ve altındaki tüm öğelerin etkinleştirildiğinden emin olmak isteyeceksiniz.  Eksik özellikleri belirlediyseniz, çekirdeğini kaynaktan yeniden oluşturarak ve uygun çekirdek. config dosyasına eklenmek üzere ilişkili modülleri seçerek etkinleştirin.  Benzer şekilde, defconfig veya menuconfig gibi bir çekirdek yapılandırma Oluşturucu kullanıyorsanız ilgili özellikleri bulup etkinleştirin ve çekirdeğini uygun şekilde yeniden oluşturun.  Yeni değiştirilen çekirdeğini dağıttıktan sonra, gerekli tüm özelliklerin başarıyla etkinleştirildiğini doğrulamak için Check-config betiğini yeniden çalıştırın.


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

IoT Edge çalışma zamanının düzgün şekilde yüklenmesiyle ilgili sorun yaşıyorsanız, [sorun giderme](troubleshoot.md) sayfasına göz atın.

Mevcut bir yüklemeyi en yeni IoT Edge sürümüne güncelleştirmek için bkz. [IoT Edge güvenlik cini ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).
