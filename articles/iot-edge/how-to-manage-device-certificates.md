---
title: Aygıt sertifikalarını yönetme - Azure IoT Edge | Microsoft Dokümanlar
description: Üretim dağıtımına hazırlanmak için test sertifikaları oluşturun, bunları bir Azure IoT Edge aygıtında yükleyin ve yönetin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/02/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c18c3d560adb3c3cae54bda808ee5842c260fd6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79539214"
---
# <a name="manage-certificates-on-an-iot-edge-device"></a>Sertifikaları IoT Edge aygıtında yönetme

Tüm IoT Edge aygıtları, çalışma süresi ile aygıtta çalışan tüm modüller arasında güvenli bağlantılar oluşturmak için sertifikaları kullanır. Ağ geçidi olarak çalışan IoT Edge aygıtları, akış aşağı aygıtlarına bağlanmak için de bu sertifikaları kullanır.

## <a name="install-production-certificates"></a>Üretim sertifikalarını yükleme

IoT Edge'i ilk yüklediğinizde ve cihazınızı sağladığınızda, hizmeti test edebilmeniz için aygıt geçici sertifikalarla ayarlanır.
Bu geçici sertifikaların süresi 90 gün içinde sona erer veya makinenizi yeniden başlatarak sıfırlanabilir.
Aygıtlarınızı bir üretim senaryosuna taşımaya hazır olduğunuzda veya bir ağ geçidi senaryosu oluşturmak istediğinizde, kendi sertifikalarınızı sağlamanız gerekir.
Bu makalede, IoT Edge aygıtlarınıza sertifika yükleme adımları gösterilmiş.

Bir IoT Edge senaryosundaki farklı sertifika türleri ve rolleri hakkında daha fazla bilgi edinmek için azure [IoT Edge'in sertifikaları nasıl kullandığını anlayın'](iot-edge-certs.md)a bakın.

>[!NOTE]
>Bu makale boyunca kullanılan "kök CA" terimi, IoT çözümünüz için sertifika zincirinin en üstteki yetkili kamu sertifikasını ifade eder. Sendikasyon sertifika yetkilisinin sertifika kökünü veya kuruluşunuzun sertifika yetkilisinin kökünü kullanmanız gerekmez. Çoğu durumda, aslında bir ara CA ortak sertifika.

### <a name="prerequisites"></a>Ön koşullar

* [Windows](how-to-install-iot-edge-windows.md) veya [Linux](how-to-install-iot-edge-linux.md)üzerinde çalışan bir IoT Edge aygıtı.
* Baltimore, Verisign, DigiCert veya GlobalSign gibi güvenilir bir ticari sertifika yetkilisinden kendi imzalamış veya satın alınmış bir kök sertifika yetkilisi (CA) sertifikasına sahip olun.

Henüz bir kök sertifika yetkiniz yoksa, ancak üretim sertifikaları gerektiren IoT Edge özelliklerini (ağ geçidi senaryoları gibi) denemek [istiyorsanız, IoT Edge aygıt özelliklerini test etmek için demo sertifikaları oluşturabilirsiniz.](how-to-create-test-certificates.md)

### <a name="create-production-certificates"></a>Üretim sertifikaları oluşturma

Aşağıdaki dosyaları oluşturmak için kendi sertifika yetkisini kullanmalısınız:

* Kök CA
* Cihaz CA sertifikası
* Cihaz CA özel anahtar

Bu makalede, *kök CA* olarak adlandırdığımız şey, bir kuruluş için en üstteki sertifika yetkilisi değildir. IoT Edge hub modülü, kullanıcı modülleri ve herhangi bir alt akış aygıtının birbirleri arasında güven oluşturmak için kullandığı IoT Edge senaryosu nun en üstteki sertifika yetkilisidir.

Bu sertifikaların bir örneğini görmek [için, örnekler ve öğreticiler için test CA sertifikalarını yönetmede](https://github.com/Azure/iotedge/tree/master/tools/CACertificates)demo sertifikaları oluşturan komut dosyalarını gözden geçirin.

### <a name="install-certificates-on-the-device"></a>Aygıta sertifika yükleme

Sertifika zincirinizi IoT Edge aygıtına yükleyin ve yeni sertifikalara başvurmak için IoT Edge çalışma zamanını yapılandırın.

Örneğin, [demo sertifikaları oluşturmak](how-to-create-test-certificates.md)için örnek komut dosyalarını kullandıysanız, aşağıdaki dosyaları IoT-Edge aygıtınıza kopyalayın:

* Cihaz CA sertifikası:`<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
* Cihaz CA özel anahtar:`<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`
* Kök CA:`<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

1. Üç sertifikayı ve anahtar dosyasını IoT Edge aygıtınıza kopyalayın.

   Sertifika dosyalarını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) gibi bir hizmeti veya Güvenli kopyalama [protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlevi kullanabilirsiniz.  Sertifikaları IoT Edge aygıtında oluşturduysanız, bu adımı atlayabilir ve çalışma dizinine giden yolu kullanabilirsiniz.

1. IoT Edge güvenlik daemon config dosyasını açın.

   * Windows:`C:\ProgramData\iotedge\config.yaml`
   * Linux:`/etc/iotedge/config.yaml`

1. Config.yaml dosyasındaki **sertifika** özelliklerini, Sertifikave IoT Edge aygıtındaki anahtar dosyalarına tam yol olarak ayarlayın. Dört `#` satırın yorumlarını kaldırmak için sertifika özelliklerinden önce karakteri kaldırın. **Sertifikaların:** satırın önceki beyaz boşluk olmadığından ve iç içe olan öğelerin iki boşluk tarafından girindi olduğundan emin olun. Örnek:

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "c:\\<path>\\device-ca.cert.pem"
        device_ca_pk: "c:\\<path>\\device-ca.key.pem"
        trusted_ca_certs: "c:\\<path>\\root-ca.root.ca.cert.pem"
      ```

   * Linux:

      ```yaml
      certificates:
        device_ca_cert: "<path>/device-ca.cert.pem"
        device_ca_pk: "<path>/device-ca.key.pem"
        trusted_ca_certs: "<path>/root-ca.root.ca.cert.pem"
      ```

1. Linux aygıtlarında, kullanıcı **iotedge'in** sertifikaları tutan dizin için izinleri okuduğundan emin olun.

1. Aygıtta daha önce IoT Edge için başka sertifikalar kullandıysanız, IoT Edge'i başlatmadan veya yeniden başlatmadan önce aşağıdaki iki dizindeki dosyaları silin:

   * Windows: `C:\ProgramData\iotedge\hsm\certs` ve`C:\ProgramData\iotedge\hsm\cert_keys`

   * Linux: `/var/lib/iotedge/hsm/certs` ve`/var/lib/iotedge/hsm/cert_keys`

## <a name="customize-certificate-lifetime"></a>Sertifika ömrünü özelleştirme

IoT Edge, aşağıdakiler de dahil olmak üzere çeşitli durumlarda aygıtta otomatik olarak sertifika lar oluşturur:

* IoT Edge'i yüklediğinizde ve sağladığınızda kendi üretim sertifikalarınızı sağlamazsanız, IoT Edge güvenlik yöneticisi otomatik olarak bir **aygıt CA sertifikası**oluşturur. Bu kendi imzalı sertifika yalnızca geliştirme ve test senaryoları içindir, üretim için değil. Bu sertifika 90 gün sonra sona erer.
* IoT Edge güvenlik yöneticisi ayrıca aygıt CA sertifikası tarafından imzalanmış bir **iş yükü CA sertifikası** oluşturur

Bir IoT Edge aygıtındaki farklı sertifikaların işlevi hakkında daha fazla bilgi için azure [IoT Edge'in sertifikaları nasıl kullandığını anlayın'](iot-edge-certs.md)a bakın.

Otomatik olarak oluşturulan bu iki sertifika için, sertifikaların kullanım ömrü için gün sayısını yapılandırmak için **auto_generated_ca_lifetime_days** bayrağını config.yaml'de ayarlama seçeneğiniz vardır.

>[!NOTE]
>IoT Edge güvenlik yöneticisinin oluşturduğu üçüncü bir otomatik oluşturulan sertifika vardır, **IoT Edge hub sunucu sertifikası.** Bu sertifikanın her zaman 90 günü vardır, ancak süresi dolmadan önce otomatik olarak yenilenir. **auto_generated_ca_lifetime_days** değeri bu sertifikayı etkilemez.

Sertifikanın son kullanma tarihini varsayılan 90 gün dışında bir şeyle yapılandırmak için, değeri gün içinde config.yaml dosyasının **sertifikalar** bölümüne ekleyin.

```yaml
certificates:
  device_ca_cert: "<ADD URI TO DEVICE CA CERTIFICATE HERE>"
  device_ca_pk: "<ADD URI TO DEVICE CA PRIVATE KEY HERE>"
  trusted_ca_certs: "<ADD URI TO TRUSTED CA CERTIFICATES HERE>"
  auto_generated_ca_lifetime_days: <value>
```

Kendi aygıtCA sertifikalarınızı sağladıysanız, ayarladığınız yaşam boyu değerinin aygıt CA sertifikasının kullanım ömründen daha kısa olması koşuluyla, bu değer yine de iş yükü CA sertifikası için geçerlidir.

Config.yaml dosyasındaki bayrağı belirttikten sonra aşağıdaki adımları izleyin:

1. Klasörün `hsm` içeriğini silin.

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

1. Ömür boyu ayarı onaylayın.

   Windows:

   ```powershell
   iotedge check --verbose
   ```

   Linux:

   ```bash
   sudo iotedge check --verbose
   ```

   **Üretime hazır olma çıktısını denetleyin:** otomatik olarak oluşturulan aygıt CA sertifikalarının süresi dolana kadar gün sayısını listeleyen sertifikalar denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

Sertifikaları bir IoT Edge cihazına yüklemek, çözümünüzü üretime dağıtmadan önce gerekli bir adımdır. [IoT Edge çözümünüzü üretimde dağıtmaya nasıl hazırlanacağınız](production-checklist.md)hakkında daha fazla bilgi edinin.
