---
title: Cihaz sertifikalarını yönetme-Azure IoT Edge | Microsoft Docs
description: Üretim dağıtımına hazırlanmak için test sertifikaları oluşturun, bunları bir Azure IoT Edge cihazında yükleyip yönetin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/02/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b13944e30c339357997fbc5f0919e5eb8485a0a9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84308787"
---
# <a name="manage-certificates-on-an-iot-edge-device"></a>IoT Edge cihazda sertifikaları yönetme

Tüm IoT Edge cihazlar, çalışma zamanı ve cihazda çalışan modüller arasında güvenli bağlantılar oluşturmak için sertifikaları kullanır. Ağ geçitleri olarak çalışan IoT Edge cihazların, bu sertifikaları da kendi aşağı akış cihazlarına bağlamak için kullanır.

## <a name="install-production-certificates"></a>Üretim sertifikalarını yükleme

IoT Edge ilk yüklediğinizde ve cihazınızı sağladığınızda cihaz, hizmeti test edebilmeniz için geçici sertifikalarla ayarlanır.
Bu geçici sertifikaların süreleri 90 gün içinde doluyor veya makineniz yeniden başlatılarak sıfırlanabilir.
Bir üretim senaryosuna geçtiğinizde veya bir ağ geçidi cihazı oluşturmak istiyorsanız kendi sertifikalarınızı sağlamanız gerekir.
Bu makalede, IoT Edge cihazlarınıza sertifika yüklemek için gereken adımlar gösterilir.

Farklı sertifika türleri ve rolleri hakkında daha fazla bilgi edinmek için bkz. [Azure IoT Edge sertifikaları nasıl kullandığını anlayın](iot-edge-certs.md).

>[!NOTE]
>Bu makale boyunca kullanılan "kök CA" terimi, IoT çözümünüz için Sertifika zincirinin en üst yetkili ortak sertifikasına başvurur. Bir dağıtılmış sertifika yetkilisinin sertifika kökünü veya kuruluşunuzun sertifika yetkilisinin kökünü kullanmanız gerekmez. Çoğu durumda, aslında bir ara CA genel sertifikasıdır.

### <a name="prerequisites"></a>Ön koşullar

* [Windows](how-to-install-iot-edge-windows.md) veya [Linux](how-to-install-iot-edge-linux.md)üzerinde çalışan IoT Edge bir cihaz.
* Bir kök sertifika yetkilisi (CA) sertifikasına sahip veya Baltimore, Verisign, DigiCert veya GlobalSign gibi güvenilir bir ticari sertifika yetkilisinden satın alınmış.

Henüz bir kök sertifika yetkiliniz yoksa ve üretim sertifikaları gerektiren IoT Edge özellikleri denemek istiyorsanız (Ağ Geçidi senaryoları gibi), [IoT Edge cihaz özelliklerini sınamak için tanıtım sertifikaları oluşturabilirsiniz](how-to-create-test-certificates.md).

### <a name="create-production-certificates"></a>Üretim sertifikaları oluşturma

Aşağıdaki dosyaları oluşturmak için kendi sertifika yetkilinizi kullanmanız gerekir:

* Kök CA
* Cihaz CA sertifikası
* Cihaz CA özel anahtarı

Bu makalede *kök CA 'sı* olarak adlandırdığımız, bir kuruluşun en üst sertifika yetkilisi değil. Bu, IoT Edge hub modülünün, Kullanıcı modüllerinin ve herhangi bir aşağı akış aygıtının birbirleriyle güven sağlamak için kullanacağı IoT Edge senaryoya yönelik en üst sertifika yetkilissudur.

Bu sertifikalara bir örnek görmek için, [örnekler ve öğreticiler için test CA sertifikalarını yönetme](https://github.com/Azure/iotedge/tree/master/tools/CACertificates)bölümünde tanıtım Sertifikaları oluşturan betikleri gözden geçirin.

### <a name="install-certificates-on-the-device"></a>Cihaza sertifika yükler

Sertifika zincirinizi IoT Edge cihaza yükleyip IoT Edge çalışma zamanını yeni sertifikalara başvuracak şekilde yapılandırın.

Örneğin, [tanıtım sertifikaları oluşturmak](how-to-create-test-certificates.md)için örnek betikleri kullandıysanız, aşağıdaki dosyaları IoT Edge cihazınıza kopyalayın:

* Cihaz CA sertifikası:`<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
* Cihaz CA özel anahtarı:`<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`
* Kök CA:`<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

1. IoT Edge cihazınıza üç sertifika ve anahtar dosyasını kopyalayın.

   Sertifika dosyalarını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) veya [Güvenli kopya Protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlev gibi bir hizmet kullanabilirsiniz.  Sertifikaları IoT Edge cihazında oluşturduysanız, bu adımı atlayabilir ve çalışma dizininin yolunu kullanabilirsiniz.

1. IoT Edge güvenliği Daemon yapılandırma dosyasını açın.

   * Pencerelerin`C:\ProgramData\iotedge\config.yaml`
   * 'Un`/etc/iotedge/config.yaml`

1. Config. YAML içindeki **sertifika** özelliklerini IoT Edge cihazdaki sertifika ve anahtar dosyaları IÇIN dosya URI yolu olarak ayarlayın. `#`Dört satırın açıklamasını kaldırmak için, sertifika özelliklerinden önceki karakteri kaldırın. **Sertifikalarda:** Line 'ın önünde boşluk olmadığından ve iç içe yerleştirilmiş öğelerin iki boşlukla girintilendiğinden emin olun. Örneğin:

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "file:///C:/<path>/<device CA cert>"
        device_ca_pk: "file:///C:/<path>/<device CA key>"
        trusted_ca_certs: "file:///C:/<path>/<root CA cert>"
      ```

   * Linux:

      ```yaml
      certificates:
        device_ca_cert: "file:///<path>/<device CA cert>"
        device_ca_pk: "file:///<path>/<device CA key>"
        trusted_ca_certs: "file:///<path>/<root CA cert>"
      ```

1. Linux cihazlarda, kullanıcının **ıotedge** 'in sertifikaları tutan dizin için okuma izinlerine sahip olduğundan emin olun.

1. Daha önce cihazda IoT Edge için başka bir sertifika kullandıysanız, IoT Edge başlatmadan veya yeniden başlatmadan önce aşağıdaki iki dizindeki dosyaları silin:

   * Windows: `C:\ProgramData\iotedge\hsm\certs` ve`C:\ProgramData\iotedge\hsm\cert_keys`

   * Linux: `/var/lib/iotedge/hsm/certs` ve`/var/lib/iotedge/hsm/cert_keys`

## <a name="customize-certificate-lifetime"></a>Sertifika ömrünü özelleştirme

IoT Edge, cihaz üzerinde aşağıdakiler dahil olmak üzere çeşitli durumlarda otomatik olarak sertifika üretir:

* IoT Edge yüklerken ve sağladığınızda kendi üretim sertifikalarınızı sağlamazsanız, IoT Edge Güvenlik Yöneticisi otomatik olarak bir **CIHAZ CA sertifikası**oluşturur. Bu otomatik olarak imzalanan sertifika yalnızca geliştirme ve test senaryoları için tasarlanmıştır, üretim değildir. Bu sertifikanın süresi 90 gün sonra doluyor.
* IoT Edge Güvenlik Yöneticisi Ayrıca cihaz CA sertifikası tarafından imzalanan bir **iş yükü CA sertifikası** oluşturur

IoT Edge aygıttaki farklı sertifikaların işlevi hakkında daha fazla bilgi için bkz. [Azure IoT Edge sertifikaları nasıl kullandığını anlama](iot-edge-certs.md).

Bu iki otomatik oluşturulan sertifika için, sertifikaların kullanım ömrü için gün sayısını yapılandırmak üzere config. YAML içinde **auto_generated_ca_lifetime_days** bayrağını ayarlama seçeneğiniz vardır.

>[!NOTE]
>IoT Edge Güvenlik Yöneticisi 'nin oluşturduğu, **IoT Edge merkezi sunucu sertifikası**olan üçüncü bir otomatik oluşturulan sertifika vardır. Bu sertifika her zaman bir 90 gündür ve süresi dolmadan önce otomatik olarak yenilenir. **Auto_generated_ca_lifetime_days** değeri bu sertifikayı etkilemez.

Sertifika süre sonunu varsayılan 90 gün dışında bir şeye göre yapılandırmak için, değeri config. YAML dosyasının **Sertifikalar** bölümüne gün olarak ekleyin.

```yaml
certificates:
  device_ca_cert: "<ADD URI TO DEVICE CA CERTIFICATE HERE>"
  device_ca_pk: "<ADD URI TO DEVICE CA PRIVATE KEY HERE>"
  trusted_ca_certs: "<ADD URI TO TRUSTED CA CERTIFICATES HERE>"
  auto_generated_ca_lifetime_days: <value>
```

Kendi cihaz CA sertifikalarınızı sağladıysanız, bu değer hala iş yükü CA sertifikası için geçerlidir, ancak ayarladığınız yaşam süresi değeri cihaz CA sertifikasının kullanım süresinden daha kısadır.

Config. YAML dosyasında bayrağı belirttikten sonra, aşağıdaki adımları uygulayın:

1. Klasörün içeriğini silin `hsm` .

   Windows: `C:\ProgramData\iotedge\hsm\certs and C:\ProgramData\iotedge\hsm\cert_keys` Linux:`/var/lib/iotedge/hsm/certs and /var/lib/iotedge/hsm/cert_keys`

1. IoT Edge hizmetini yeniden başlatın.

   Windows:

   ```powershell
   Restart-Service iotedge
   ```

   Linux:

   ```bash
   sudo systemctl restart iotedge
   ```

1. Ömür ayarını onaylayın.

   Windows:

   ```powershell
   iotedge check --verbose
   ```

   Linux:

   ```bash
   sudo iotedge check --verbose
   ```

   Otomatik olarak oluşturulan cihaz CA sertifikalarının sona erene kadar geçen gün sayısını listeleyen **Üretim hazırlığı: sertifika** denetimi çıktısını denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge cihaza sertifika yükleme, çözümünüzü üretime dağıtmak için gereken bir adımdır. [IoT Edge çözümünüzü üretime dağıtmaya hazırlanma](production-checklist.md)hakkında daha fazla bilgi edinin.
