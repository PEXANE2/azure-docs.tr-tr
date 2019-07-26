---
title: Simetrik anahtar kanıtlama kullanarak cihazları DPS ile oto Sağlama-Azure IoT Edge | Microsoft Docs
description: Cihaz sağlama hizmeti ile Azure IoT Edge için otomatik cihaz sağlamayı test etmek için simetrik anahtar kanıtlama kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: mrohera
ms.date: 07/10/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 3c21c0bdce6f6a5cd3c8f634bf400600b30a8ead
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414600"
---
# <a name="create-and-provision-an-iot-edge-device-using-symmetric-key-attestation"></a>Simetrik anahtar kanıtlama kullanarak bir IoT Edge cihazı oluşturma ve sağlama

Azure IOT Edge cihazları olabilir autoprovisioned kullanarak [cihaz sağlama hizmeti](../iot-dps/index.yml) edge etkin olmayan cihazlar'olduğu gibi. Autoprovisioning işlemine bilmiyorsanız gözden [autoprovisioning kavramları](../iot-dps/concepts-auto-provisioning.md) devam etmeden önce.

Bu makalede, aşağıdaki adımlarla bir IoT Edge cihazında simetrik anahtar kanıtlama kullanarak bir cihaz sağlama hizmeti bireysel kaydı oluşturma işlemi gösterilmektedir:

* Bir örnek, IOT Hub cihaz sağlama hizmeti (DPS) oluşturun.
* Cihazı için bireysel bir kayıt oluşturun.
* IoT Edge çalışma zamanını yükleyip IoT Hub bağlayın.

Simetrik anahtar kanıtlama, cihaz sağlama hizmeti örneğiyle bir cihazın kimliğini doğrulamaya yönelik basit bir yaklaşımdır. Bu kanıtlama yöntemi, cihaz sağlama için yeni olan veya katı güvenlik gereksinimleri olmayan geliştiriciler için bir "Hello World" deneyimini temsil eder. [TPM](../iot-dps/concepts-tpm-attestation.md) kullanan cihaz kanıtlama daha güvenlidir ve daha sıkı güvenlik gereksinimleri için kullanılmalıdır.

## <a name="prerequisites"></a>Önkoşullar

* Etkin bir IoT Hub
* Fiziksel veya sanal cihaz

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>IOT Hub cihazı sağlama hizmetini ayarlama

Azure'da yeni bir IOT Hub cihazı sağlama hizmeti örneğini oluşturun ve IOT hub'ınıza bağlayın. ' Ndaki yönergeleri takip edebilirsiniz [IOT hub'ı DPS ' ayarlamak](../iot-dps/quick-setup-auto-provision.md).

Cihaz sağlama hizmeti çalışıyor sonra değerini kopyalayın **kimlik kapsamı** genel bakış sayfasında. IOT Edge çalışma zamanı yapılandırdığınızda bu değeri kullanın.

## <a name="choose-a-unique-registration-id-for-the-device"></a>Cihaz için benzersiz bir kayıt KIMLIĞI seçin

Her bir cihazı tanımlamak için benzersiz bir kayıt KIMLIĞI tanımlanmalıdır. MAC adresi, seri numarası veya cihazdan herhangi bir benzersiz bilgi kullanabilirsiniz.

Bu örnekte, bir kayıt KIMLIĞI için aşağıdaki dizeyi oluşturan bir MAC adresi ve seri numarası birleşimini kullanırız.

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Cihazınız için benzersiz bir kayıt KIMLIĞI oluşturun. Geçerli karakterler küçük harfli alfasayısal ve tire ('-').

## <a name="create-a-dps-enrollment"></a>DPS kayıt oluşturma

DPS 'de tek bir kayıt oluşturmak için cihazınızın kayıt KIMLIĞINI kullanın.

DPS'de bir kayıt oluşturduğunuzda, bildirme fırsatına sahip bir **ilk cihaz İkizi durumu**. Cihaz ikizinde bölge, ortam, konuma veya cihaz türü gibi çözümünüzdeki gereken herhangi bir ölçümü tarafından cihazları için etiketler ayarlayabilirsiniz. Bu etiketleri oluşturmak için kullanılan [otomatik dağıtımlar](how-to-deploy-monitor.md).

> [!TIP]
> Simetrik anahtar kanıtlama kullanılırken grup kayıtları da mümkündür ve bireysel kayıtlar ile aynı kararları içerir.

1. [Azure Portal](https://portal.azure.com), IoT Hub cihaz sağlama hizmeti örneğinize gidin.

1. Altında **ayarları**seçin **kayıtları Yönet**.

1. Seçin **Ekle bireysel kayıt** ardından kayıt yapılandırmak için aşağıdaki adımları tamamlayın:  

   1. **Mekanizma**Için **simetrik anahtar**' ı seçin.

   1. **Anahtarları otomatik oluştur** onay kutusunu seçin.

   1. Cihazınız için oluşturduğunuz **kayıt kimliğini** belirtin.

   1. İsterseniz cihazınız için bir **IoT Hub CIHAZ kimliği** sağlayın. Cihaz kimliklerini modülü dağıtımı için tek bir cihaza hedeflemek için kullanabilirsiniz. Bir cihaz KIMLIĞI sağlamazsanız, kayıt KIMLIĞI kullanılır.

   1. Kaydın IoT Edge bir cihaz için olduğunu bildirmek için **true** ' ı seçin. Bir grup kaydı için tüm cihazların IoT Edge cihaz olması veya hiçbirinin olmaması gerekir.

   1. **Cihazları hub 'lara atamak** istediğiniz veya bu kayda özgü farklı bir değer seçebileceğiniz cihaz sağlama hizmeti 'nin ayırma ilkesinden varsayılan değeri kabul edin.

   1. Bağlantılı seçin **IOT hub'ı** cihazınıza bağlanmak istiyorsanız. Birden çok hub seçebilirsiniz ve bu cihaz, seçilen ayırma ilkesine göre bu cihazdan birine atanır.

   1. Cihazların ilk kez sağlama istemesi durumunda **cihaz verilerinin yeniden hazırlanması için nasıl işleneceğini** seçin.

   1. Etiketi değer eklemek **ilk cihaz İkizi durumu** istiyorsanız. Hedef cihaz gruplarına etiketleri modülü dağıtımı için kullanabilirsiniz. Örneğin:

      ```json
      {
         "tags": {
            "environment": "test"
         },
         "properties": {
            "desired": {}
         }
      }
      ```

   1. **Enable girişinin** **Enable**olarak ayarlandığından emin olun.

   1. **Kaydet**’i seçin.

Bu cihaz için bir kayıt mevcut olduğuna göre, IoT Edge çalışma zamanı cihazı yükleme sırasında otomatik olarak sağlayabilir. Cihazınızın anahtarını oluştururken kullanılacak kayıt **birincil anahtar** değerini kaydettiğinizden emin olun.

## <a name="derive-a-device-key"></a>Bir cihaz anahtarı türet

Cihazınız, sağlama sırasında kayıt ile simetrik anahtar kanıtlama gerçekleştirmek için benzersiz kayıt KIMLIĞINIZLE türetilmiş Cihaz anahtarını kullanır. Cihaz anahtarını oluşturmak için, DPS kaydınızdan kopyaladığınız anahtarı kullanarak cihazın benzersiz kayıt KIMLIĞI için [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) ' ı hesaplamanız ve sonucu base64 biçimine dönüştürmeniz gerekir.

Kayıt kodunuzun birincil veya ikincil anahtarını cihaz kodunuza eklemeyin.

### <a name="linux-workstations"></a>Linux iş istasyonları

Bir Linux iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi, türetilen cihaz anahtarınızı oluşturmak için OpenSSL kullanabilirsiniz.

**Anahtarın** değerini, daha önce not ettiğiniz **birincil anahtarla** değiştirin.

**REG_ID** değerini CIHAZıNıZıN kayıt kimliğiyle değiştirin.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```

### <a name="windows-based-workstations"></a>Windows tabanlı iş istasyonları

Windows tabanlı bir iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi, türetilmiş cihaz anahtarınızı oluşturmak için PowerShell kullanabilirsiniz.

**Anahtarın** değerini, daha önce not ettiğiniz **birincil anahtarla** değiştirin.

**REG_ID** değerini CIHAZıNıZıN kayıt kimliğiyle değiştirin.

```powershell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```powershell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```

## <a name="install-the-iot-edge-runtime"></a>IOT Edge çalışma zamanını yükleme

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenleri kapsayıcılarında çalıştırmak ve kod ucuna çalıştırabilmeniz için cihaza ek kapsayıcıları dağıtma olanak sağlar.

Cihazınızı sağlarken aşağıdaki bilgilere sahip olmanız gerekir:

* DPS **kimlik kapsamı** değeri
* Oluşturduğunuz cihaz **kayıt kimliği**
* Simetrik anahtar kanıtlama için cihazın türetilmiş cihaz anahtarı

### <a name="linux-device"></a>Linux cihazı

Cihazınızın mimarisine yönelik yönergeleri izleyin. Otomatik değil el ile sağlama için IOT Edge çalışma zamanı yapılandırdığınızdan emin olun.

[Linux üzerinde Azure IoT Edge çalışma zamanını yükler](how-to-install-iot-edge-linux.md)

Simetrik anahtar sağlama yapılandırma dosyasındaki bölümü şöyle görünür:

```yaml
# DPS symmetric key provisioning configuration
provisioning:
   source: "dps"
   global_endpoint: "https://global.azure-devices-provisioning.net"
   scope_id: "{scope_id}"
   attestation:
      method: "symmetric_key"
      registration_id: "{registration_id}"
      symmetric_key: "{symmetric_key}"
```

, `{scope_id}` ,`{registration_id}`Ve içinyertutucudeğerlerinidahaönce`{symmetric_key}` topladığınız verilerle değiştirin.

### <a name="windows-device"></a>Windows cihazı

IoT Edge çalışma zamanını türetilmiş bir cihaz anahtarı oluşturduğunuz cihaza yüklemek için yönergeleri izleyin. Otomatik değil el ile sağlama için IOT Edge çalışma zamanı yapılandırdığınızdan emin olun.

[Windows üzerinde IoT Edge yükleyip otomatik olarak sağla](how-to-install-iot-edge-windows.md#option-2-install-and-automatically-provision)

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

Çalışma zamanı başarıyla başlatıldı, IOT Hub'ına gidin ve IOT Edge modülleri, cihazınıza dağıtmaya başlayın. Aşağıdaki komutlar, çalışma zamanı yüklü ve başarıyla başlatıldı doğrulamak için Cihazınızda kullanın.

### <a name="linux-device"></a>Linux cihazı

IoT Edge hizmetinin durumunu kontrol edin.

```cmd/sh
systemctl status iotedge
```

Hizmet günlüklerini inceleyin.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Çalışan modülleri listeleyin.

```cmd/sh
iotedge list
```

### <a name="windows-device"></a>Windows cihazı

IoT Edge hizmetinin durumunu kontrol edin.

```powershell
Get-Service iotedge
```

Hizmet günlüklerini inceleyin.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Çalışan modülleri listeleyin.

```powershell
iotedge list
```

Cihaz sağlama hizmeti 'nde oluşturduğunuz bireysel kaydın kullanıldığını doğrulayabilirsiniz. Azure portal cihaz sağlama hizmeti örneğinize gidin. Oluşturduğunuz bireysel kayıt için kayıt ayrıntılarını açın. Kayıt durumunun **atandığını** ve cihaz kimliğinin listelendiğini unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz sağlama hizmeti kayıt işlemi, yeni cihaz sağlama gibi cihaz kimliği ve cihaz ikizi etiketleri aynı anda belirlemenizi sağlar. Bu değerleri ayrı ayrı cihazlar ya da otomatik cihaz Yönetimi'ni kullanarak cihaz grupları hedeflemek için kullanabilirsiniz. Bilgi edinmek için nasıl [dağıtma ve izleme IOT Edge modülleri, ölçeklendirme Azure portalını kullanarak](how-to-deploy-monitor.md) veya [Azure CLI kullanarak](how-to-deploy-monitor-cli.md).
