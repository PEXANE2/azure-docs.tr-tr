---
title: Cihaza sertifika yükler-Azure IoT Edge | Microsoft Docs
description: Test sertifikaları oluşturun ve üretim dağıtımına hazırlanmak için bunları Azure IoT Edge cihaza yüklemeyi öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/03/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 8232a71140dcded907562a4f1c0f2196ff012c12
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75486254"
---
# <a name="install-production-certificates-on-an-iot-edge-device"></a>IoT Edge cihaza üretim sertifikaları yükler

Tüm IoT Edge cihazlar, çalışma zamanı ve cihazda çalışan modüller arasında güvenli bağlantılar oluşturmak için sertifikaları kullanır.
Ağ geçitleri olarak çalışan IoT Edge cihazların, bu sertifikaları da kendi aşağı akış cihazlarına bağlamak için kullanır. 

IoT Edge ilk yüklediğinizde ve cihazınızı sağladığınızda cihaz, hizmeti test edebilmeniz için geçici sertifikalarla ayarlanır.
Bu geçici sertifikaların süreleri 90 gün içinde doluyor veya makineniz yeniden başlatılarak sıfırlanabilir.
Cihazlarınızı bir üretim senaryosuna taşımaya hazırsanız ya da bir ağ geçidi senaryosu oluşturmak istiyorsanız kendi sertifikalarınızı sağlamanız gerekir.
Bu makalede, IoT Edge cihazlarınıza sertifika yüklemek için gereken adımlar gösterilir.

Farklı sertifika türleri ve IoT Edge senaryolarındaki rolleri hakkında daha fazla bilgi edinmek için bkz. [Azure IoT Edge sertifikaları nasıl kullandığını anlayın](iot-edge-certs.md).

>[!NOTE]
>Bu makale boyunca kullanılan "kök CA" terimi, IoT çözümünüz için Sertifika zincirinin en üst yetkili ortak sertifikasına başvurur. Bir dağıtılmış sertifika yetkilisinin sertifika kökünü veya kuruluşunuzun sertifika yetkilisinin kökünü kullanmanız gerekmez. Çoğu durumda, aslında bir ara CA genel sertifikasıdır. 

## <a name="prerequisites"></a>Ön koşullar 

* [Windows](how-to-install-iot-edge-windows.md) veya [Linux](how-to-install-iot-edge-linux.md)üzerinde çalışan IoT Edge bir cihaz.
* Bir kök sertifika yetkilisi (CA) sertifikasına sahip veya Baltimore, Verisign, DigiCert veya GlobalSign gibi güvenilir bir ticari sertifika yetkilisinden satın alınmış.

Henüz bir kök sertifika yetkiliniz yoksa ve üretim sertifikaları gerektiren IoT Edge özellikleri denemek istiyorsanız (Ağ Geçidi senaryoları gibi), [IoT Edge cihaz özelliklerini sınamak için tanıtım sertifikaları oluşturabilirsiniz](how-to-create-test-certificates.md).

## <a name="create-production-certificates"></a>Üretim sertifikaları oluşturma

Aşağıdaki dosyaları oluşturmak için kendi sertifika yetkilinizi kullanmanız gerekir:
* Kök CA
* Cihaz CA sertifikası
* Cihaz CA özel anahtarı

Bu makalede *kök CA 'sı* olarak adlandırdığımız, bir kuruluşun en üst sertifika yetkilisi değil. Bu, IoT Edge hub modülünün, Kullanıcı modüllerinin ve herhangi bir aşağı akış aygıtının birbirleriyle güven sağlamak için kullanacağı IoT Edge senaryoya yönelik en üst sertifika yetkilissudur. 

Bu sertifikalara bir örnek görmek için, [örnekler ve öğreticiler için test CA sertifikalarını yönetme](https://github.com/Azure/iotedge/tree/master/tools/CACertificates)bölümünde tanıtım Sertifikaları oluşturan betikleri gözden geçirin. 

## <a name="install-certificates-on-the-device"></a>Cihaza sertifika yükler

Sertifika zincirinizi IoT Edge cihaza yükleyip IoT Edge çalışma zamanını yeni sertifikalara başvuracak şekilde yapılandırın. 

Örneğin, [tanıtım sertifikaları oluşturmak](how-to-create-test-certificates.md)için örnek betikleri kullandıysanız, IoT Edge cihazınıza kopyalamanız gereken üç dosya şunlardır:

* Cihaz CA sertifikası: `<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
* Cihaz CA özel anahtarı: `<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`
* Kök CA: `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

1. IoT Edge cihazınıza üç sertifika ve anahtar dosyasını kopyalayın.

   Sertifika dosyalarını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) veya [Güvenli kopya Protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlev gibi bir hizmet kullanabilirsiniz.  Sertifikaları IoT Edge cihazında oluşturduysanız, bu adımı atlayabilir ve çalışma dizininin yolunu kullanabilirsiniz.

2. IOT Edge güvenlik daemon yapılandırma dosyasını açın. 

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Config. YAML dosyasındaki **sertifika** özelliklerini, IoT Edge cihazdaki sertifika ve anahtar DOSYALARıNıN dosya URI 'sine ayarlayın. Dört satırın açıklamasını kaldırmak için, sertifika özelliklerinden önce `#` karakterini kaldırın. YAML 'deki girintiler iki boşluk olduğunu unutmayın. Örneğin:

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "file:///c:/path/device-ca.cert.pem"
        device_ca_pk: "file:///c:/path/device-ca.key.pem"
        trusted_ca_certs: "file:///c:/path/root-ca.root.ca.cert.pem"
      ```
   
   * Linux: 
      ```yaml
      certificates:
        device_ca_cert: "file:///path/device-ca.cert.pem"
        device_ca_pk: "file:///path/device-ca.key.pem"
        trusted_ca_certs: "file:///path/root-ca.root.ca.cert.pem"
      ```

4. Linux cihazlarda, kullanıcının **ıotedge** 'in sertifikaları tutan dizin için okuma izinlerine sahip olduğundan emin olun. 

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge cihaza sertifika yükleme, çözümünüzü üretime dağıtmak için gereken bir adımdır. [IoT Edge çözümünüzü üretime dağıtmaya hazırlanma](production-checklist.md)hakkında daha fazla bilgi edinin.