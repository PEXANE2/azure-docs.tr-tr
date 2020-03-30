---
title: Azure IoT Edge'i Linux'a yükleme | Microsoft Dokümanlar
description: Ubuntu veya Raspbian çalıştıran Linux cihazlarında Azure IoT Edge yükleme yönergeleri
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: kgremban
ms.openlocfilehash: fb86ee9ce956917f8da44146e58a4775e0ba639f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535910"
---
# <a name="install-the-azure-iot-edge-runtime-on-debian-based-linux-systems"></a>Debian tabanlı Linux sistemlerine Azure IoT Edge çalışma zamanını yükleme

Azure IoT Edge çalışma zamanı, aygıtı IoT Edge aygıtına dönüştüren şeydir. Çalışma süresi Raspberry Pi kadar küçük veya endüstriyel sunucu kadar büyük aygıtlarda dağıtılabilir. Bir aygıt IoT Edge çalışma zamanı ile yapılandırıldıktan sonra, buluttan iş mantığı dağıtmaya başlayabilirsiniz. Daha fazla bilgi için Azure [IoT Edge çalışma süresini ve mimarisini anlayın.](iot-edge-runtime.md)

Bu makalede, Azure IoT Edge çalışma saatini bir X64, ARM32 veya ARM64 Linux aygıtına yüklemek için gereken adımlar listelenir. Biz Ubuntu Server 16.04, Ubuntu Server 18.04 ve Raspbian Stretch için kurulum paketleri sağlar. Desteklenen Linux işletim sistemleri ve mimarilerinin listesi için [Azure IoT Edge desteklenen sistemlere](support.md#operating-systems) bakın.

> [!NOTE]
> Linux yazılım depolarında yer alan paketler her pakette bulunan lisans koşullarına (/usr/share/doc/*package-name)* tabidir. Paketi kullanmadan önce lisans koşullarını okuyun. Paketi yüklemeniz ve kullanmanız bu koşulları kabul etmek anlamına dalır. Lisans koşullarını kabul etmiyorsanız, paketi kullanmayın.

## <a name="install-the-latest-runtime-version"></a>En son çalışma zamanı sürümünü yükleme

Azure IoT Edge çalışma zamanının en son sürümünü cihazınıza yüklemek için aşağıdaki bölümleri kullanın.

>[!NOTE]
>ARM64 aygıtları için destek [genel önizlemede.](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

### <a name="register-microsoft-key-and-software-repository-feed"></a>Microsoft anahtarını ve yazılım deposu akışını kaydetme

Cihazınızı IoT Edge çalışma zamanı yüklemesi için hazırlayın.

Depo yapılandırmasını yükleyin. Cihazınızın işletim sistemiyle eşleşen **16.04** veya **18.04** komutunu seçin:

* **Ubuntu Server 16.04**:

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/multiarch/prod.list > ./microsoft-prod.list
   ```

* **Ubuntu Server 18.04**:

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
   ```

* **Raspbian Stretch**:

   ```bash
   curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
   ```

Oluşturulan listeyi kopyalayın.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Microsoft GPG ortak anahtarını yükleme

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-the-container-runtime"></a>Kapsayıcı çalışma süresini yükleme

Azure IoT Edge, [OCI uyumlu](https://www.opencontainers.org/) bir kapsayıcı çalışma süresine dayanır. Üretim senaryoları için, aşağıda sağlanan [Moby tabanlı](https://mobyproject.org/) motoru kullanmanızı tavsiye ettik. Moby motoru, Azure IoT Edge ile resmi olarak desteklenen tek konteyner motorudur. Docker CE/EE konteyner görüntüleri Moby çalışma süresi ile uyumludur.

Cihazınızdaki paket listelerini güncelleştirin.

   ```bash
   sudo apt-get update
   ```

Moby motorini tarayın.

   ```bash
   sudo apt-get install moby-engine
   ```

Moby komut satırı arabirimini (CLI) yükleyin. CLI geliştirme için yararlıdır, ancak üretim dağıtımları için isteğe bağlıdır.

   ```bash
   sudo apt-get install moby-cli
   ```

Moby kapsayıcıçalışma süresini yüklerken hata lar alırsanız, bu makalenin ilerleyen zamanlarında sağlanan [Moby uyumluluğu için Linux çekirdeğinizi doğrulamak için](#verify-your-linux-kernel-for-moby-compatibility)aşağıdaki adımları izleyin.

### <a name="install-the-azure-iot-edge-security-daemon"></a>Azure IoT Edge Güvenlik Daemon'u yükleyin

**IoT Edge güvenlik daemon ioT** Edge aygıtında güvenlik standartları sağlar ve korur. Daemon her önyüklemede başlar ve Aygıtı IoT Edge çalışma süresinin geri kalanını başlatarak botlara yakalar.

Yükleme komutu, mevcut değilse **libiothsm'un** standart sürümünü de yükler.

Cihazınızdaki paket listelerini güncelleştirin.

   ```bash
   sudo apt-get update
   ```

Güvenlik daemon yükleyin. Paket ' de `/etc/iotedge/`yüklenir.

   ```bash
   sudo apt-get install iotedge
   ```

IoT Edge başarıyla yüklendikten sonra, çıktı yapılandırma dosyasını güncelleştirmenizi ister. Aygıt sağlama işlemlerini tamamlamak için [güvenlik daemon](#configure-the-security-daemon) bölümünü yapılandır'daki adımları izleyin.

## <a name="install-a-specific-runtime-version"></a>Belirli bir çalışma zamanı sürümünü yükleme

En son sürümleri kullanmak yerine Moby ve Azure IoT Edge'in belirli bir sürümünü yüklemek istiyorsanız, bileşen dosyalarını doğrudan IoT Edge GitHub deposundan hedefleyebilirsiniz. Tüm IoT Edge bileşenlerini cihazınıza geçirmek için aşağıdaki adımları kullanın: Moby motoru ve CLI, libiothsm ve son olarak IoT Edge güvenlik daemonu. Bir sonraki bölüme atla, belirli bir çalışma zamanı sürümüne değiştirmek istemiyorsanız, [güvenlik daemon yapılandırın.](#configure-the-security-daemon)

1. [Azure IoT Edge sürümlerine](https://github.com/Azure/azure-iotedge/releases)gidin ve hedeflemek istediğiniz sürüm sürümünü bulun.

2. Bu sürüm için **Varlıklar** bölümünü genişletin.

3. Herhangi bir sürümde Moby motorunda güncellemeler olabilir veya olmayabilir. **Moby-engine** ve **moby-cli**ile başlayan dosyaları görürseniz, bu bileşenleri güncelleştirmek için aşağıdaki komutları kullanın. Herhangi bir Moby dosyası görmüyorsanız, en son sürümü bulana kadar eski sürüm varlıklarına geri gidin.

   1. IoT Edge cihazınızın mimarisiyle eşleşen **moby motor** dosyasını bulun. Dosya bağlantısına sağ tıklayın ve bağlantı adresini kopyalayın.

   2. Moby altyapısının bu sürümünü yüklemek için aşağıdaki komuttaki kopyalanan bağlantıyı kullanın:

      ```bash
      curl -L <moby-engine link> -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb
      ```

   3. IoT Edge cihazınızın mimarisiyle eşleşen **moby-cli** dosyasını bulun. Moby CLI isteğe bağlı bir bileşendir, ancak geliştirme sırasında yararlı olabilir. Dosya bağlantısına sağ tıklayın ve bağlantı adresini kopyalayın.

   4. Moby CLI'nin bu sürümünü yüklemek için aşağıdaki komuttaki kopyalanan bağlantıyı kullanın:

      ```bash
      curl -L <moby-cli link> -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb
      ```

4. Her sürüm IoT Edge güvenlik daemon ve hsmlib için yeni dosyalar olmalıdır. Bu bileşenleri güncelleştirmek için aşağıdaki komutları kullanın.

   1. IoT Edge cihazınızın mimarisiyle eşleşen **libiothsm-std** dosyasını bulun. Dosya bağlantısına sağ tıklayın ve bağlantı adresini kopyalayın.

   2. Hsmlib'in bu sürümünü yüklemek için aşağıdaki komuttaki kopyalanan bağlantıyı kullanın:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```

   3. IoT Edge cihazınızın mimarisiyle eşleşen **iotedge** dosyasını bulun. Dosya bağlantısına sağ tıklayın ve bağlantı adresini kopyalayın.

   4. IoT Edge güvenlik daemon bu sürümünü yüklemek için aşağıdaki komutta kopyalanan bağlantıyı kullanın.

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

IoT Edge başarıyla yüklendikten sonra, çıktı yapılandırma dosyasını güncelleştirmenizi ister. Aygıt sağlamayı tamamlamak için sonraki bölümdeki adımları izleyin.

## <a name="configure-the-security-daemon"></a>Güvenlik daemon yapılandırma

IoT Edge çalışma zamanını, fiziksel aygıtınızı Azure IoT hub'ında bulunan bir aygıt kimliğine bağlamak için yapılandırın.

Daemon, 'deki `/etc/iotedge/config.yaml`yapılandırma dosyası kullanılarak yapılandırılabilir. Dosya varsayılan olarak yazma korumalı, bunu düzenlemek için yüksek izinler gerekebilir.

Tek bir IoT Edge aygıtı, IoT Hub tarafından sağlanan aygıt bağlantıları dizesi kullanılarak el ile kullanılabilir. Veya, aygıtları otomatik olarak sağlamak için Aygıt Sağlama Hizmetini kullanabilirsiniz, bu da sağlamanız gereken çok sayıda cihaz olduğunda yararlıdır. Sağlama tercihinize bağlı olarak, uygun yükleme komut dosyasını seçin.

### <a name="option-1-manual-provisioning"></a>Seçenek 1: Manuel sağlama

Bir aygıtı el ile sağlamak için, aygıtı IoT hub'ınıza yeni bir aygıt kaydederek oluşturabileceğiniz bir [aygıt bağlantı dizesi](how-to-register-device.md#register-in-the-azure-portal) sağlamanız gerekir.

Yapılandırma dosyasını açın.

```bash
sudo nano /etc/iotedge/config.yaml
```

Dosyanın sağlama yapılandırmalarını bulun ve **El Kitabı sağlama yapılandırma** sı bölümünün yorumlarını bırakın. IoT Edge aygıtınızdaki bağlantı dizesiyle **device_connection_string** değerini güncelleştirin. Diğer sağlama bölümlerinin yorumlanmasından emin olun. **Sağlama:** satırın önceki beyaz boşluk olmadığından ve iç içe olan öğelerin iki boşluk tarafından girintisi olduğundan emin olun.

```yml
# Manual provisioning configuration
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
```

Pano içeriğini Nano'ya `Shift+Right Click` yapıştırmak `Shift+Insert`veya .

Dosyayı kaydedin ve kapatın.

   `CTRL + X`, `Y`, `Enter`

Yapılandırma dosyasındaki sağlama bilgilerini girdikten sonra, daemon'u yeniden başlatın:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Seçenek 2: Otomatik sağlama

IoT Edge aygıtları [Azure IoT Hub Aygıt Sağlama Hizmeti (DPS)](../iot-dps/index.yml)kullanılarak otomatik olarak kullanılabilir. Şu anda, IoT Edge otomatik sağlama kullanırken iki attestation mekanizmasını destekler, ancak donanım gereksinimleriniz seçimlerinizi etkileyebilir. Örneğin, Raspberry Pi aygıtları varsayılan olarak Güvenilir Platform Modülü (TPM) yongasıyla birlikte gelmez. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Linux VM'de sanal TPM içeren bir IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-simulated-device-linux.md)
* [X.509 sertifikalarını kullanarak bir IoT Edge aygıtı oluşturma ve sağlama](how-to-auto-provision-x509-certs.md)
* [Simetrik anahtar attestation kullanarak bir IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-symmetric-keys.md)

Bu makaleler, DPS'de kayıt oluşturma ve attestation için uygun sertifikaları veya anahtarları oluşturma konusunda size yol alır. Hangi attestation mekanizmasını seçerseniz seçin, sağlama bilgileri IoT Edge aygıtınızdaki IoT Edge yapılandırma dosyasına eklenir.

Yapılandırma dosyasını açın.

```bash
sudo nano /etc/iotedge/config.yaml
```

Dosyanın sağlama yapılandırmalarını bulun ve attestation mekanizmasınız için uygun bölümü yorumsuz bırakın. Diğer sağlama bölümlerinin yorumlanmasından emin olun. **Sağlama:** satır ın önceki beyaz boşluğu olmamalıdır ve iç içe olan öğeler iki boşluk tarafından girintisi olmalıdır. **ioT** Hub Aygıt Sağlama Hizmeti örneğinizdeki değerle scope_id değerini güncelleştirin ve attestation alanları için uygun değerleri sağlayın.

TPM attestation:

```yml
# DPS TPM provisioning configuration
provisioning:
  source: "dps"
  global_endpoint: "https://global.azure-devices-provisioning.net"
  scope_id: "<SCOPE_ID>"
  attestation:
    method: "tpm"
    registration_id: "<REGISTRATION_ID>"
```

X.509 attestation:

```yml
# DPS X.509 provisioning configuration
provisioning:
  source: "dps"
  global_endpoint: "https://global.azure-devices-provisioning.net"
  scope_id: "<SCOPE_ID>"
  attestation:
    method: "x509"
#   registration_id: "<OPTIONAL REGISTRATION ID. LEAVE COMMENTED OUT TO REGISTER WITH CN OF identity_cert>"
    identity_cert: "<REQUIRED URI TO DEVICE IDENTITY CERTIFICATE>"
    identity_pk: "<REQUIRED URI TO DEVICE IDENTITY PRIVATE KEY>"
```

Simetrik anahtar attestation:

```yml
# DPS symmetric key provisioning configuration
provisioning:
  source: "dps"
  global_endpoint: "https://global.azure-devices-provisioning.net"
  scope_id: "<SCOPE_ID>"
  attestation:
    method: "symmetric_key"
    registration_id: "<REGISTRATION_ID>"
    symmetric_key: "<SYMMETRIC_KEY>"
```

Pano içeriğini Nano'ya `Shift+Right Click` yapıştırmak `Shift+Insert`veya .

Dosyayı kaydedin ve kapatın. `CTRL + X`, `Y`, `Enter`

Yapılandırma dosyasındaki sağlama bilgilerini girdikten sonra, daemon'u yeniden başlatın:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Başarılı yüklemeyi doğrulama

Önceki bölümde **el ile yapılandırma** adımlarını kullandıysanız, IoT Edge çalışma zamanı başarıyla sağlanmalıdır ve cihazınızda çalıştırılmalıdır. **Otomatik yapılandırma** adımlarını kullandıysanız, çalışma zamanının cihazınızı sizin adınıza IoT hub'ınıza kaydedebilmeleri için bazı ek adımları tamamlamanız gerekir. Sonraki adımlar için, linux [sanal makinede simüle edilmiş bir TPM IoT Edge aygıtı oluştur ve sağlama](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm)konusuna bakın.

IoT Edge Daemon'un durumunu kontrol edebilirsiniz:

```bash
systemctl status iotedge
```

Daemon günlüklerini inceleyin:

```bash
journalctl -u iotedge --no-pager --no-full
```

En yaygın yapılandırma ve ağ hataları için otomatik denetim çalıştırın:

```bash
sudo iotedge check
```

Cihazınızdaki Ilk modülünüzü IoT Edge'e dağıtana **kadar, $edgeHub** sistem modülü aygıta dağıtılacak. Sonuç olarak, otomatik denetim `Edge Hub can bind to ports on host` bağlantı denetimi için bir hata döndürecektir. Bu hata, aygıta bir modül dağıtıldıktan sonra oluşmadığı sürece yoksayılabilir.

Son olarak, çalışan modülleri listele:

```bash
sudo iotedge list
```

IoT Edge'i cihazınıza yükledikten sonra, çalışırken görmeniz gereken tek modül **edgeAgent'dir.** İlk dağıtımınızı oluşturduktan sonra, diğer sistem modülü **$edgeHub** aygıtta da başlayacaktır. Daha fazla bilgi için [IoT Edge modüllerini dağıtın.](how-to-deploy-modules-portal.md)

## <a name="tips-and-troubleshooting"></a>İpuçları ve sorun giderme

`iotedge` komutlarını çalıştırmak için yükseltilmiş ayrıcalıklara ihtiyacınız olacaktır. Çalışma süresini yükledikten sonra makinenizden çıkış ve izinlerinizi otomatik olarak güncellemek için yeniden oturum açın. O zamana kadar, herhangi `iotedge` bir komutönünde **sudo** kullanın.

Kaynak kısıtlı aygıtlarda, [sorun giderme kılavuzundaki](troubleshoot.md)talimatlara göre *OptimizeForPerformance* ortamı değişkenini *false* olarak ayarlamanız önerilir.

Proxy sunucusu olan ağınız varsa, [proxy sunucusu üzerinden iletişim kurmak için IoT Edge aygıtınızı yapılandır'daki](how-to-configure-proxy-support.md)adımları izleyin.

### <a name="verify-your-linux-kernel-for-moby-compatibility"></a>Moby uyumluluğu için Linux çekirdeğinizi doğrulayın

Birçok gömülü aygıt üreticisi, konteyner çalışma zamanı uyumluluğu için gerekli özellikler olmadan özel Linux çekirdekleri içeren aygıt görüntüleri sevk eder. Önerilen Moby kapsayıcı çalışma süresini yüklerken sorunlarla karşılaşırsanız, resmi [Moby GitHub deposundan](https://github.com/moby/moby) [gelen denetim-config](https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh) komut dosyasını kullanarak Linux çekirdeği yapılandırmanızı sorun giderebilirsiniz. Çekirdek yapılandırmanızı denetlemek için aygıtta aşağıdaki komutları çalıştırın:

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

Bu komut, Moby çalışma zamanı tarafından kullanılan çekirdek özelliklerinin durumunu içeren ayrıntılı bir çıktı sağlar. Çekirdeğinizin Moby çalışma `Generally Necessary` süresiyle tam olarak uyumlu olduğundan emin olmak için altındaki ve `Network Drivers` etkin olan tüm öğelerin olduğundan emin olmak isteyeceksiniz.  Eksik özellikleri tespit ettiyseniz, çekirdekinizi kaynaktan yeniden oluşturarak ve ilgili modülleri uygun çekirdek .config'e dahil etmek üzere seçerek bunları etkinleştirin.  Benzer şekilde, gibi `defconfig` bir çekirdek yapılandırma jeneratör `menuconfig`ü kullanıyorsanız veya, bulmak ve ilgili özellikleri etkinleştirmek ve buna göre çekirdek yeniden.  Yeni değiştirilen çekirdeğinizi dağıttıktan sonra, gerekli tüm özelliklerin başarıyla etkinleştirildiğini doğrulamak için denetim config komut dosyasını yeniden çalıştırın.

## <a name="uninstall-iot-edge"></a>IoT Edge'i kaldır

IoT Edge yüklemesini Linux aygıtınızdan kaldırmak istiyorsanız, komut satırından aşağıdaki komutları kullanın.

IoT Edge çalışma zamanını kaldırın.

```bash
sudo apt-get remove --purge iotedge
```

IoT Edge çalışma zamanı kaldırıldığında, oluşturduğu kapsayıcılar durdurulur ancak cihazınızda hala bulunur. Hangilerinin kaldığını görmek için tüm kapsayıcıları görüntüleyin.

```bash
sudo docker ps -a
```

İki çalışma zamanı kapsayıcısı da dahil olmak üzere aygıtınızdaki kapsayıcıları silin.

```bash
sudo docker rm -f <container name>
```

Son olarak, kapsayıcıçalışma süresini cihazınızdan çıkarın.

```bash
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Sonraki adımlar

Artık çalışma zamanı yüklü bir IoT Edge aygıtınız olduğuna göre, [IoT Edge modüllerini dağıtabilirsiniz.](how-to-deploy-modules-portal.md)

IoT Edge çalışma zamanının düzgün yüklenmesiyle ilgili sorunlar yaşıyorsanız, [sorun giderme](troubleshoot.md) sayfasına göz atın.

Varolan bir yüklemeyi IoT Edge'in en yeni sürümüne güncelleştirmek için [bkz.](how-to-update-iot-edge.md)
