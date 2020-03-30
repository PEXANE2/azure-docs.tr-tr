---
title: Simetrik anahtar attestation kullanarak sağlama cihazı - Azure IoT Edge
description: Azure IoT Edge için otomatik aygıt sağlamayı Aygıt Sağlama Hizmeti ile test etmek için simetrik anahtar attestation'ı kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: mrohera
ms.date: 10/04/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: d9944308d00c9cfecbd38a6443efb49913148806
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535927"
---
# <a name="create-and-provision-an-iot-edge-device-using-symmetric-key-attestation"></a>Simetrik anahtar attestation kullanarak bir IoT Edge cihazı oluşturma ve sağlama

Azure IoT Edge aygıtları, kenar etkin olmayan aygıtlar gibi [Aygıt Sağlama Hizmeti](../iot-dps/index.yml) kullanılarak otomatik olarak kullanılabilir. Otomatik sağlama işlemine aşina değilseniz, devam etmeden önce [otomatik sağlama kavramlarını](../iot-dps/concepts-auto-provisioning.md) gözden geçirin.

Bu makalede, aşağıdaki adımları içeren bir IoT Edge aygıtında simetrik anahtar attestation kullanarak bir Aygıt Sağlama Hizmeti tek tek kayıt oluşturmak için nasıl gösterir:

* IoT Hub Aygıt Sağlama Hizmeti (DPS) örneğini oluşturun.
* Aygıt için tek tek bir kayıt oluşturun.
* IoT Edge çalışma süresini yükleyin ve IoT Hub'ına bağlanın.

Simetrik anahtar attestation bir Aygıt Sağlama Hizmeti örneği ile bir aygıtın kimlik doğrulaması için basit bir yaklaşımdır. Bu attestation yöntemi, aygıt sağlamada yeni olan veya sıkı güvenlik gereksinimleri olmayan geliştiriciler için bir "Merhaba dünya" deneyimini temsil eder. [TPM](../iot-dps/concepts-tpm-attestation.md) veya [X.509 sertifikaları](../iot-dps/concepts-security.md#x509-certificates) kullanan aygıt attestation'ı daha güvenlidir ve daha sıkı güvenlik gereksinimleri için kullanılmalıdır.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir IoT Hub'ı
* Fiziksel veya sanal bir aygıt

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>IoT Hub Cihaz Sağlama Hizmetini Ayarlama

Azure'da IoT Hub Aygıt Sağlama Hizmeti'nin yeni bir örneğini oluşturun ve IoT hub'ınıza bağlayın. [IoT Hub DPS'yi](../iot-dps/quick-setup-auto-provision.md)kur'a'daki yönergeleri takip edebilirsiniz.

Aygıt Sağlama Hizmeti çalıştırdıktan **sonra, kimlik kapsamının** değerini genel bakış sayfasından kopyalayın. IoT Edge çalışma süresini yapılandırırken bu değeri kullanırsınız.

## <a name="choose-a-unique-registration-id-for-the-device"></a>Aygıt için benzersiz bir kayıt kimliği seçin

Her aygıtı tanımlamak için benzersiz bir kayıt kimliği tanımlanmalıdır. MAC adresini, seri numarasını veya aygıttaki benzersiz bilgileri kullanabilirsiniz.

Bu örnekte, bir kayıt kimliği için aşağıdaki dizeyi oluşturan bir `sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6`MAC adresi ve seri numarasının birleşimini kullanırız: .

Cihazınız için benzersiz bir kayıt kimliği oluşturun. Geçerli karakterler küçük alfasayısal ve tire ('-') vardır.

## <a name="create-a-dps-enrollment"></a>DPS kaydı oluşturma

DPS'ye tek tek bir kayıt oluşturmak için cihazınızın kayıt kimliğini kullanın.

DPS'de bir kayıt oluşturduğunuzda, Başlangıç **Aygıtı İkiz Durumu**bildirme fırsatınız vardır. Aygıt ikizinde, etiketleri aygıtları bölge, ortam, konum veya aygıt türü gibi çözümünüzde gereksinim duyduğunuz herhangi bir metrike göre gruplayabilir. Bu etiketler otomatik [dağıtımlar](how-to-deploy-monitor.md)oluşturmak için kullanılır.

> [!TIP]
> Grup kayıtları, simetrik anahtar attestation'ı kullanırken ve tek tek kayıtlarla aynı kararları içerirken de mümkündür.

1. Azure [portalında,](https://portal.azure.com)IoT Hub Aygıt Sağlama Hizmeti örneğinize gidin.

1. **Ayarlar** **altında, kayıtları yönet'i**seçin.

1. **Tek tek kayıt ekle'yi** seçin ve ardından kaydı yapılandırmak için aşağıdaki adımları tamamlayın:  

   1. **Mekanizma**için **Simetrik Anahtar'ı**seçin.

   1. Otomatik **oluşturma tuşları** onay kutusunu seçin.

   1. Cihazınız için oluşturduğunuz **Kayıt Kimliğini** sağlayın.

   1. İsterseniz cihazınız için bir **IoT Hub Aygıt Kimliği** sağlayın. Modül dağıtımı için tek bir aygıtı hedeflemek için aygıt adlarını kullanabilirsiniz. Aygıt kimliği sağlamazsanız, kayıt kimliği kullanılır.

   1. Kaydın bir IoT Edge aygıtı için olduğunu bildirmek için **True'yu** seçin. Grup kaydı için tüm aygıtların IoT Edge aygıtları olması gerekir veya bunların hiçbiri olamaz.

   1. Aygıt Sağlama Hizmeti'nin tahsisat ilkesindeki varsayılan değeri **hub'lara nasıl atamak istediğinize** veya bu kayda özgü farklı bir değer seçme şeklinize ilişkin olarak kabul edin.

   1. Cihazınızı bağlamak istediğiniz bağlantılı **IoT Hub'ını** seçin. Birden çok hub seçebilirsiniz ve aygıt seçili ayırma ilkesine göre bunlardan birine atanır.

   1. Aygıtlar ilk defa sağlama istediğinde **cihaz verilerinin yeniden sağlama da nasıl işleneceğini** seçin.

   1. İstersenize **İlk Aygıt İkiz Durumu'na** etiket değeri ekleyin. Modül dağıtımı için aygıt gruplarını hedeflemek için etiketleri kullanabilirsiniz. Örnek:

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

   1. **Etkinleştir** **girişietkinleştir'e**ayarlandığından emin olun.

   1. **Kaydet'i**seçin.

Bu aygıt için bir kayıt bulunduğuna göre, IoT Edge çalışma süresi yükleme sırasında aygıtı otomatik olarak sağlayabilir. IoT Edge çalışma saatini yüklerken veya grup kaydıyla kullanılmak üzere aygıt anahtarları oluşturacaksanız, kaydınızın Birincil **Anahtar** değerini kopyaladiğinizden emin olun.

## <a name="derive-a-device-key"></a>Aygıt anahtarını türetin

> [!NOTE]
> Bu bölüm yalnızca bir grup kaydı kullanıyorsanız gereklidir.

Her cihaz, sağlama sırasında kayıtla birlikte simetrik anahtar attestation gerçekleştirmek için türetilmiş aygıt anahtarını benzersiz kayıt kimliğinizle birlikte kullanır. Aygıt anahtarını oluşturmak için DPS kaydınızdan kopyaladığınız anahtarı kullanarak aygıtın benzersiz kayıt kimliğinin [HMAC-SHA256'sını](https://wikipedia.org/wiki/HMAC) hesaplayın ve sonucu Base64 biçimine dönüştürün.

Cihazınızın birincil veya ikincil anahtarını aygıt kodunuza eklemeyin.

### <a name="linux-workstations"></a>Linux iş istasyonları

Bir Linux iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi türemiş aygıt anahtarınızı oluşturmak için openssl'yi kullanabilirsiniz.

**KEY** değerini daha önce belirttiğiniz **Birincil Anahtar** la değiştirin.

**REG_ID** değerini cihazınızın kayıt kimliğiyle değiştirin.

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

Windows tabanlı bir iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi türemiş aygıt anahtarınızı oluşturmak için PowerShell'i kullanabilirsiniz.

**KEY** değerini daha önce belirttiğiniz **Birincil Anahtar** la değiştirin.

**REG_ID** değerini cihazınızın kayıt kimliğiyle değiştirin.

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

## <a name="install-the-iot-edge-runtime"></a>IoT Edge çalışma süresini yükleme

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenleri kapsayıcılarda çalışır ve kenarda kod çalıştırabilmeniz için aygıta ek kapsayıcılar dağıtmanızı sağlar.

Cihazınızı sağlarken aşağıdaki bilgilere ihtiyacınız olacak:

* DPS **Id Kapsam** değeri
* Oluşturduğunuz aygıt **Kayıt Kimliği**
* DPS kaydından kopyaladığınız **Birincil Anahtar**

> [!TIP]
> Grup kayıtları için, DPS kayıt anahtarı yerine her aygıtın [türetilmiş anahtarına](#derive-a-device-key) ihtiyacınız vardır.

### <a name="linux-device"></a>Linux cihazı

Cihazınızın mimarisi yle ilgili yönergeleri izleyin. IoT Edge çalışma süresini manuel değil otomatik olarak yapılandırdığından emin olun.

[Azure IoT Edge çalışma süresini Linux'a yükleme](how-to-install-iot-edge-linux.md)

Simetrik anahtar sağlama yapılandırma dosyasındaki bölüm aşağıdaki gibi görünür:

```yaml
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

Yer tutucu değerlerini `<SCOPE_ID>`, `<REGISTRATION_ID>`ve `<SYMMETRIC_KEY>` daha önce topladığınız verilerle değiştirin. **Sağlama:** satırın önceki beyaz boşluk olmadığından ve iç içe olan öğelerin iki boşluk tarafından girintisi olduğundan emin olun.

### <a name="windows-device"></a>Windows cihazı

IoT Edge çalışma zamanını, türetilmiş bir aygıt anahtarı oluşturduğunuz aygıta yükleyin. IoT Edge çalışma süresini manuel değil otomatik olarak yapılandıracaksınız.

Kapsayıcıları yönetme ve IoT Edge'i güncelleştirme gibi görevler için ön koşullar ve yönergeler de dahil olmak üzere Windows'a IoT [Edge](how-to-install-iot-edge-windows.md)yükleme hakkında daha ayrıntılı bilgi için bkz.

1. Yönetici modunda bir PowerShell penceresi açın. IoT Edge'i yüklerken AMD64 PowerShell oturumunu kullandığınızdan emin olun, PowerShell'i (x86) değil.

1. **Deploy-IoTEdge** komutu, Windows makinenizin desteklenen bir sürümde olup olmadığını denetler, kapsayıcılar özelliğini açar ve moby çalışma süresini ve IoT Edge çalışma süresini karşıdan yüklez. Komut, Windows kapsayıcılarını kullanmayı varsayılan olarak kullanır.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

1. Bu noktada, IoT Core aygıtları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server aygıtları yeniden başlatmanızı isteyebilir. Öyleyse, cihazınızı şimdi yeniden başlatın. Cihazınız hazır olduğunda PowerShell'i yeniden yönetici olarak çalıştırın.

1. **Initialize-IoTEdge komutu,** Makinenizdeki IoT Edge çalışma süresini yapılandırır. Komut, otomatik sağlama kullanmak için `-Dps` bayrağı kullanmadığınız sürece Windows kapsayıcılarıyla el ile sağlama yı varsayılan olarak kullanır.

   Yer tutucu değerlerini `{scope_id}`, `{registration_id}`ve `{symmetric_key}` daha önce topladığınız verilerle değiştirin.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps -ScopeId {scope ID} -RegistrationId {registration ID} -SymmetricKey {symmetric key}
   ```

## <a name="verify-successful-installation"></a>Başarılı yüklemeyi doğrulama

Çalışma süresi başarıyla başladıysa, IoT Hub'ınıza gidip IoT Edge modüllerini cihazınıza dağıtmaya başlayabilirsiniz. Çalışma zamanının yüklü olduğunu ve başarılı bir şekilde başladığını doğrulamak için cihazınızdaki aşağıdaki komutları kullanın.

### <a name="linux-device"></a>Linux cihazı

IoT Edge hizmetinin durumunu kontrol edin.

```cmd/sh
systemctl status iotedge
```

Servis günlüklerini inceleyin.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Çalışan modülleri listele.

```cmd/sh
iotedge list
```

### <a name="windows-device"></a>Windows cihazı

IoT Edge hizmetinin durumunu kontrol edin.

```powershell
Get-Service iotedge
```

Servis günlüklerini inceleyin.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Çalışan modülleri listele.

```powershell
iotedge list
```

Aygıt Sağlama Hizmeti'nde oluşturduğunuz tek tek kaydın kullanıldığını doğrulayabilirsiniz. Azure portalındaki Aygıt Sağlama Hizmeti örneğinize gidin. Oluşturduğunuz tek tek kayıt için kayıt ayrıntılarını açın. Kaydın durumunun **atandığını** ve aygıt kimliğinin listelenmiş olduğuna dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

Aygıt Sağlama Hizmeti kayıt işlemi, aygıt kimliğini ve aygıt ikiz etiketlerini yeni aygıtı sağlarken aynı anda ayarlamanızı sağlar. Bu değerleri, otomatik aygıt yönetimini kullanarak tek tek aygıtları veya aygıt gruplarını hedeflemek için kullanabilirsiniz. Azure portalını kullanarak veya [Azure CLI'yi kullanarak](how-to-deploy-monitor-cli.md) [IoT Edge modüllerini ölçekte nasıl dağıtıp izleyeceğinizi](how-to-deploy-monitor.md) öğrenin.
