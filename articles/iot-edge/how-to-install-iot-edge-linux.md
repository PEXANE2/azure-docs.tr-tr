---
title: Linux 'ta Azure IoT Edge 'yi yükler | Microsoft Docs
description: Ubuntu veya Raspbian çalıştıran Linux cihazlarda yükleme yönergelerini Azure IoT Edge
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/22/2020
ms.author: kgremban
ms.openlocfilehash: d73f3a37bb084533733b27b49ac171747cee814c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85321876"
---
# <a name="install-the-azure-iot-edge-runtime-on-debian-based-linux-systems"></a>Debian tabanlı Linux sistemlerine Azure IoT Edge çalışma zamanını yükleme

Azure IoT Edge çalışma zamanı, bir cihazı IoT Edge cihazına dönüştürür. Çalışma zamanı, cihazlarda Raspberry Pi kadar küçük veya endüstriyel sunucu olarak büyük olarak dağıtılabilir. Bir cihaz IoT Edge çalışma zamanına göre yapılandırıldıktan sonra, buluta iş mantığı dağıtmaya başlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure IoT Edge çalışma zamanını ve mimarisini anlayın](iot-edge-runtime.md).

Bu makalede, Azure IoT Edge çalışma zamanını x64, ARM32 veya ARM64 Linux cihazına yüklemek için gereken adımlar listelenmektedir. Ubuntu Server 16,04, Ubuntu Server 18,04 ve Raspbian Esnetme için yükleme paketleri sağlıyoruz. Desteklenen Linux işletim sistemleri ve mimarilerin bir listesi için [desteklenen Azure IoT Edge sistemleri](support.md#operating-systems) bölümüne bakın.

> [!NOTE]
> Linux yazılım depolarındaki paketler, her pakette bulunan lisans koşullarına tabidir (/usr/share/doc/*Package-Name*). Paketi kullanmadan önce lisans koşullarını okuyun. Paketi yüklemeniz ve kullanmanız, bu şartlarınızın kabul edildiğini oluşturur. Lisans koşullarını kabul etmiyorsanız, paketini kullanmayın.

## <a name="install-iot-edge-and-container-runtimes"></a>IoT Edge ve kapsayıcı çalışma zamanlarını yükleyip

Azure IoT Edge çalışma zamanının en son sürümünü cihazınıza yüklemek için aşağıdaki bölümleri kullanın.

>[!NOTE]
>ARM64 cihazlar için destek [genel önizlemede](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="register-microsoft-key-and-software-repository-feed"></a>Microsoft anahtar ve yazılım deposu akışını Kaydet

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

Microsoft GPG ortak anahtarını yükler.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

Azure IoT Edge, [OCI uyumlu](https://www.opencontainers.org/) bir kapsayıcı çalışma zamanına bağlıdır. Üretim senaryolarında aşağıda belirtilen [Moby tabanlı](https://mobyproject.org/) altyapıyı kullanmanızı öneririz. Moby motoru, Azure IoT Edge ile resmi olarak desteklenen tek kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntüleri Moby çalışma zamanına göre uyumludur.

Cihazınızdaki paket listelerini güncelleştirin.

   ```bash
   sudo apt-get update
   ```

Moby altyapısını yükler.

   ```bash
   sudo apt-get install moby-engine
   ```

Moby komut satırı arabirimini (CLı) yükler. CLı geliştirme için faydalıdır, ancak üretim dağıtımları için isteğe bağlıdır.

   ```bash
   sudo apt-get install moby-cli
   ```

Moby kapsayıcı çalışma zamanını yüklerken hata alırsanız, bu makalede daha sonra sağlanmış olan [Linux çekirdeğini Moby uyumluluğu Için doğrulamak](#verify-your-linux-kernel-for-moby-compatibility)üzere adımları izleyin.

### <a name="install-the-azure-iot-edge-security-daemon"></a>Azure IoT Edge güvenlik cini 'nı yükler

**IoT Edge güvenlik arka plan programı** , IoT Edge cihazında güvenlik standartları sağlar ve korur. Arka plan programı her önyüklemede başlar ve IoT Edge çalışma zamanının geri kalanını başlatarak cihazı önyükleme.

Cihazınızdaki paket listelerini güncelleştirin.

   ```bash
   sudo apt-get update
   ```

Hangi IoT Edge sürümlerinin kullanılabilir olduğunu görmek için denetleyin.

   ```bash
   apt list -a iotedge
   ```

Güvenlik arka plan programının en son sürümünü yüklemek isterseniz, aşağıdaki komutu kullanarak **libiothsm-STD** paketinin en son sürümünü de yükler:

   ```bash
   sudo apt-get install iotedge
   ```

Güvenlik arka plan programının belirli bir sürümünü yüklemek istiyorsanız, apt liste çıktısından sürümü belirtin. Ayrıca, **libiothsm-STD** paketi için aynı sürümü belirtin, aksi takdirde en son sürümünü yükler. Örneğin, aşağıdaki komut 1.0.8 sürümünün en son sürümünü yüklüyor:

   ```bash
   sudo apt-get install iotedge=1.0.8* libiothsm-std=1.0.8*
   ```

Yüklemek istediğiniz sürüm listede yoksa, [Sürüm varlıklarını kullanarak çalışma zamanında yüklemeyi çalıştırma](#install-runtime-using-release-assets)bölümündeki adımları uygulayın. Bu bölümde, IoT Edge güvenlik arka plan programının veya sürüm adayı sürümlerinin önceki bir sürümünün nasıl hedeflenecek gösterilmektedir.

IoT Edge başarılı bir şekilde yüklendikten sonra `/etc/iotedge/` çıktı, yapılandırma dosyasını güncelleştirmenizi ister. Cihaz sağlamayı tamamlamaya yönelik bir sonraki bölüme geçin.

## <a name="configure-the-security-daemon"></a>Güvenlik cini yapılandırma

Fiziksel cihazınızı Azure IoT Hub 'ında bulunan bir cihaz kimliğiyle bağlamak için IoT Edge çalışma zamanını yapılandırın.

Daemon, konumundaki yapılandırma dosyası kullanılarak yapılandırılabilir `/etc/iotedge/config.yaml` . Dosya varsayılan olarak yazma korumalı, düzenlemek için yükseltilmiş izinlere sahip olabilirsiniz.

Tek bir IoT Edge cihaz, IoT Hub tarafından sağlanan bir cihaz bağlantı dizesi kullanılarak el ile sağlanabilir. Veya, cihaz sağlama hizmeti 'ni kullanarak cihazları otomatik olarak temin edebilir, bu da birçok cihaza sahip olduğunuzda yararlı olur. Sağlama seçiminize bağlı olarak, uygun yükleme betiğini seçin.

### <a name="option-1-manual-provisioning"></a>Seçenek 1: El Ile sağlama

Bir cihazı el ile sağlamak için, IoT Hub 'ınıza yeni bir cihaz kaydederek oluşturabileceğiniz bir [Cihaz bağlantı dizesi](how-to-register-device.md#register-in-the-azure-portal) sağlamanız gerekir.

Yapılandırma dosyasını açın.

```bash
sudo nano /etc/iotedge/config.yaml
```

Dosyanın sağlama yapılandırmalarını bulun ve **el ile sağlama yapılandırma** bölümünün açıklamasını kaldırın. **Device_connection_string** değerini IoT Edge cihazınızdan bağlantı dizesiyle güncelleştirin. Diğer sağlama bölümlerinin açıklama olarak belirlendiğinden emin olun. **Sağlama:** satırının önünde boşluk olmadığından ve iç içe yerleştirilmiş öğelerin iki boşlukla girintilendiğinden emin olun.

```yml
# Manual provisioning configuration
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
```

Pano içeriğini nano `Shift+Right Click` veya Press 'e yapıştırmak için `Shift+Insert` .

Dosyayı kaydedin ve kapatın.

   `CTRL + X`, `Y`, `Enter`

Yapılandırma dosyasına sağlama bilgilerini girdikten sonra, arka plan programını yeniden başlatın:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Seçenek 2: otomatik sağlama

IoT Edge cihazlar [Azure IoT Hub cihaz sağlama hizmeti (DPS)](../iot-dps/index.yml)kullanılarak otomatik olarak sağlanabilir. Şu anda, otomatik sağlama kullanılırken IoT Edge üç kanıtlama mekanizmasını destekler, ancak donanım gereksinimleriniz seçimlerinizi etkileyebilir. Örneğin, Raspberry PI cihazları varsayılan olarak Güvenilir Platform Modülü (TPM) yongasıyla birlikte gelmiyor. Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

* [Linux VM 'de sanal TPM ile IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-simulated-device-linux.md)
* [X. 509.440 sertifikalarını kullanarak bir IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-x509-certs.md)
* [Simetrik anahtar kanıtlama kullanarak bir IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-symmetric-keys.md)

Bu makaleler, DPS 'de kayıt ayarlama ve kanıtlama için uygun sertifikaları veya anahtarları oluşturma konusunda size yol gösterir. Seçtiğiniz kanıtlama mekanizmasından bağımsız olarak, sağlama bilgileri IoT Edge cihazınızdaki IoT Edge yapılandırma dosyasına eklenir.

Yapılandırma dosyasını açın.

```bash
sudo nano /etc/iotedge/config.yaml
```

Dosyanın sağlama yapılandırmasını bulun ve kanıtlama mekanizmanız için uygun bölümün açıklamasını kaldırın. Diğer sağlama bölümlerinin açıklama olarak belirlendiğinden emin olun. **Sağlama:** satırının önünde boşluk olmaması ve iç içe geçmiş öğelerin iki boşluk olması gerekir. **Scope_id** değerini IoT Hub cihaz sağlama hizmeti örneğinizin değeri ile güncelleştirin ve kanıtlama alanları için uygun değerleri sağlayın.

TPM kanıtlama:

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

X. 509.440 kanıtlama:

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

Simetrik anahtar kanıtlama:

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

Pano içeriğini nano `Shift+Right Click` veya Press 'e yapıştırmak için `Shift+Insert` .

Dosyayı kaydedin ve kapatın. `CTRL + X`, `Y`, `Enter`

Yapılandırma dosyasına sağlama bilgilerini girdikten sonra, arka plan programını yeniden başlatın:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulama

Önceki bölümde **el ile yapılandırma** adımlarını kullandıysanız, IoT Edge çalışma zamanının cihazınızda başarıyla sağlanması ve çalıştırılması gerekir. **Otomatik yapılandırma** adımlarını kullandıysanız, çalışma zamanının cihazınızı sizin adınıza IoT Hub 'ınıza kaydedebilmesi için bazı ek adımlar gerçekleştirmeniz gerekir. Sonraki adımlar için bkz. [Linux sanal makinesi üzerinde sanal BIR TPM IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

IoT Edge Daemon 'ın durumunu denetleyebilirsiniz:

```bash
systemctl status iotedge
```

Daemon günlüklerini inceleyin:

```bash
journalctl -u iotedge --no-pager --no-full
```

En yaygın yapılandırma ve ağ hatalarını denetlemek için [sorun giderme aracını](troubleshoot.md#run-the-check-command) çalıştırın:

```bash
sudo iotedge check
```

Cihazınızda IoT Edge için ilk modülünüzü dağıtana kadar, **$edgeHub** sistem modülü cihaza dağıtılmayacak. Sonuç olarak, otomatik denetim bağlantı denetimi için bir hata döndürür `Edge Hub can bind to ports on host` . Bu hata, cihaza bir modül dağıttıktan sonra gerçekleşmediği takdirde yoksayılabilir.

Son olarak, çalışan modülleri listeleyin:

```bash
sudo iotedge list
```

Cihazınıza IoT Edge yükledikten sonra, çalıştırmayı görmeniz gereken tek modül **Edgeagent**' dir. İlk dağıtımınızı oluşturduktan sonra, diğer sistem modülü **$edgeHub** de cihazda başlayacaktır. Daha fazla bilgi için bkz. [IoT Edge modülleri dağıtma](how-to-deploy-modules-portal.md).

## <a name="tips-and-troubleshooting"></a>İpuçları ve sorun giderme

`iotedge` komutlarını çalıştırmak için yükseltilmiş ayrıcalıklara ihtiyacınız olacaktır. Çalışma zamanını yükledikten sonra, makinenizde oturumunuzu kapatın ve izinlerinizi otomatik olarak güncelleştirmek için yeniden oturum açın. Bundan sonra, herhangi bir komutun önünde **sudo** kullanın `iotedge` .

Kaynak kısıtlı cihazlarda, [sorun giderme kılavuzunda](troubleshoot.md)her yönerge Için *Optimizeforperformance* ortam değişkenini *false* olarak ayarlamanız kesinlikle önerilir.

Bir proxy sunucusu olan ağınız, [proxy sunucusu üzerinden iletişim kurmak için IoT Edge cihazınızı yapılandırma](how-to-configure-proxy-support.md)bölümündeki adımları uygulayın.

### <a name="verify-your-linux-kernel-for-moby-compatibility"></a>Moby uyumluluğu için Linux çekirdeğini doğrulama

Birçok katıştırılmış cihaz üreticisi, kapsayıcı çalışma zamanı uyumluluğu için gerekli özellikler olmadan özel Linux çekirdekler içeren cihaz görüntülerini sunar. Önerilen Moby kapsayıcı çalışma zamanını yüklerken sorunlarla karşılaşırsanız, resmi [Moby GitHub deposundan](https://github.com/moby/moby) [Check-config](https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh) betiğini kullanarak Linux çekirdek yapılandırmanızda sorun giderebilirsiniz. Çekirdek yapılandırmanızı denetlemek için cihazda aşağıdaki komutları çalıştırın:

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

Bu komut, Moby çalışma zamanı tarafından kullanılan çekirdek özelliklerinin durumunu içeren ayrıntılı bir çıktı sağlar. `Generally Necessary` `Network Drivers` Çekirdeğin, Moby çalışma zamanına tamamen uyumlu olduğundan emin olmak için, ve altındaki tüm öğelerin etkinleştirildiğinden emin olmak isteyeceksiniz.  Eksik özellikleri belirlediyseniz, çekirdeğini kaynaktan yeniden oluşturarak ve uygun çekirdek. config dosyasına eklenmek üzere ilişkili modülleri seçerek etkinleştirin.  Benzer şekilde, veya gibi bir çekirdek yapılandırma Oluşturucu kullanıyorsanız `defconfig` `menuconfig` ilgili özellikleri bulup etkinleştirin ve çekirdeğini uygun şekilde yeniden oluşturun.  Yeni değiştirilen çekirdeğini dağıttıktan sonra, gerekli tüm özelliklerin başarıyla etkinleştirildiğini doğrulamak için Check-config betiğini yeniden çalıştırın.

## <a name="install-runtime-using-release-assets"></a>Sürüm varlıklarını kullanarak çalışma zamanını yükler

Moby 'nin belirli bir sürümünü ve aracılığıyla kullanılamayan Azure IoT Edge çalışma zamanını yüklemek istiyorsanız bu bölümdeki adımları kullanın `apt-get install` . Microsoft paket listesi yalnızca sınırlı sayıda yeni sürümü ve alt sürümlerini içerir, bu nedenle bu adımlar daha eski bir sürüm veya sürüm adayı sürümü yüklemek isteyen herkese yöneliktir.

Kıvrımlı komutlarını kullanarak bileşen dosyalarını doğrudan IoT Edge GitHub deposundan hedefleyebilirsiniz. Cihazlarınızın tüm IoT Edge bileşenlerini almak için aşağıdaki adımları kullanın: Moby Engine ve CLı, libiothsm ve son olarak IoT Edge güvenlik arka plan programı.

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

IoT Edge başarılı bir şekilde yüklendikten sonra `/etc/iotedge` çıktı, yapılandırma dosyasını güncelleştirmenizi ister. Cihaz sağlamayı tamamlamaya yönelik [güvenlik cini 'Nı yapılandırma](#configure-the-security-daemon) bölümündeki adımları izleyin.

## <a name="uninstall-iot-edge"></a>IoT Edge kaldır

Linux cihazınızdan IoT Edge yüklemesini kaldırmak istiyorsanız, komut satırından aşağıdaki komutları kullanın.

IoT Edge çalışma zamanını kaldırın.

```bash
sudo apt-get remove --purge iotedge
```

IoT Edge çalışma zamanı kaldırıldığında, oluşturduğu kapsayıcılar durdurulur ancak cihazınızda hala bulunur. Hangi olanların kaldığını görmek için tüm kapsayıcıları görüntüleyin.

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

Çalışma zamanının yüklü olduğu bir IoT Edge cihazınıza sahip olduğunuza göre, [IoT Edge modülleri dağıtabilirsiniz](how-to-deploy-modules-portal.md).

IoT Edge çalışma zamanının düzgün şekilde yüklenmesiyle ilgili sorun yaşıyorsanız, [sorun giderme](troubleshoot.md) sayfasına göz atın.

Mevcut bir yüklemeyi en yeni IoT Edge sürümüne güncelleştirmek için bkz. [IoT Edge güvenlik cini ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).
