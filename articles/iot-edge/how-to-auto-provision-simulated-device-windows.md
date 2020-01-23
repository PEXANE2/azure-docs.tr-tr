---
title: DPS ile Windows cihazlarını otomatik olarak Sağlama-Azure IoT Edge | Microsoft Docs
description: Otomatik cihaz, cihaz sağlama hizmeti ile Azure IOT Edge için sağlama test etmek için Windows makinenizde bir simülasyon cihazı kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/09/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ee4f01c3ec57b0cf9e3ecf47254b57be95ea051a
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76510949"
---
# <a name="create-and-provision-a-simulated-iot-edge-device-with-a-virtual-tpm-on-windows"></a>Windows üzerinde sanal TPM ile sanal bir IoT Edge cihaz oluşturma ve sağlama

Azure IOT Edge cihazları otomatik-sağlanabilir kullanarak [cihaz sağlama hizmeti](../iot-dps/index.yml) edge etkin olmayan cihazlar'olduğu gibi. Otomatik sağlama işlemine bilmiyorsanız gözden [otomatik sağlama kavramlarını](../iot-dps/concepts-auto-provisioning.md) devam etmeden önce.

DPS, hem bireysel kayıt hem de grup kaydında IoT Edge cihazlar için simetrik anahtar kanıtlamayı destekler. Grup kaydı için, simetrik anahtar kanıtlama içinde "IoT Edge cihaz" seçeneğini true olarak belirlerseniz, bu kayıt grubu altında kayıtlı tüm cihazlar IoT Edge cihaz olarak işaretlenir.

Bu makalede, aşağıdaki adımlarla sanal bir IoT Edge cihazında otomatik sağlamayı test etme işlemi gösterilmektedir:

* Bir örnek, IOT Hub cihaz sağlama hizmeti (DPS) oluşturun.
* Donanım güvenlik için Windows makinenizde ile bir sanal Güvenilir Platform Modülü (TPM) bir simülasyon cihazı oluşturun.
* Cihazı için bireysel bir kayıt oluşturun.
* IOT Edge çalışma zamanını yükleyin ve cihazı IOT hub'a bağlayın.

> [!TIP]
> Bu makalede, sanal cihazlarda TPM kanıtlaması kullanılarak otomatik sağlama testi açıklanır, ancak fiziksel TPM donanımı de kullanılırken bunun çoğu geçerlidir.

## <a name="prerequisites"></a>Ön koşullar

* Bir Windows geliştirme makinesi. Bu makalede, Windows 10 kullanır.
* Etkin bir IOT Hub.

> [!NOTE]
> TPM kanıtlaması ile TPM kanıtlama kullanılırken TPM 2,0 gereklidir ve yalnızca bireysel, grup değil kayıtları oluşturmak için kullanılabilir.

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>IOT Hub cihazı sağlama hizmetini ayarlama

Azure'da yeni bir IOT Hub cihazı sağlama hizmeti örneğini oluşturun ve IOT hub'ınıza bağlayın. ' Ndaki yönergeleri takip edebilirsiniz [IOT hub'ı DPS ' ayarlamak](../iot-dps/quick-setup-auto-provision.md).

Cihaz sağlama hizmeti çalışıyor sonra değerini kopyalayın **kimlik kapsamı** genel bakış sayfasında. IOT Edge çalışma zamanı yapılandırdığınızda bu değeri kullanın.

> [!TIP]
> Fiziksel bir TPM cihazı kullanıyorsanız, her TPM yongası için benzersiz olan ve kendisiyle ilişkili TPM yonga üreticisinden elde edilen **onay anahtarını**belirlemeniz gerekir. TPM cihazınız için benzersiz bir **kayıt kimliği** türetebilirsiniz; Örneğin, onay anahtarının SHA-256 karmasını oluşturma.
>
> Kayıt bilgilerinizi DPS içinde oluşturmak için [Azure portalı ile cihaz kayıtlarını yönetme](../iot-dps/how-to-manage-enrollments.md) makalesindeki yönergeleri izleyin ve sonra devam etmek için bu makaledeki [IoT Edge çalışma zamanını Install](#install-the-iot-edge-runtime) bölümüne ilerleyin.

## <a name="simulate-a-tpm-device"></a>TPM cihazının benzetimini yapma

Windows geliştirme makinenizde sanal bir TPM cihazı oluşturma. Cihazınızın **kayıt kimliği** ve **onay anahtarını** alın ve DPS 'de tek bir kayıt girişi oluşturmak için bunları kullanın.

DPS'de bir kayıt oluşturduğunuzda, bildirme fırsatına sahip bir **ilk cihaz İkizi durumu**. Cihaz ikizinde bölge, ortam, konuma veya cihaz türü gibi çözümünüzdeki gereken herhangi bir ölçümü tarafından cihazları için etiketler ayarlayabilirsiniz. Bu etiketleri oluşturmak için kullanılan [otomatik dağıtımlar](how-to-deploy-monitor.md).

Sanal cihazı oluşturmak için kullanmak istediğiniz SDK dil seçin ve bireysel kayıt oluşturana kadar adımları izleyin.

Tek bir kaydı oluşturduğunuzda, Windows geliştirme makinenizdeki sanal bir TPM cihazının **IoT Edge bir cihaz**olduğunu bildirmek için **true** ' ı seçin.

Sanal cihazı ve bireysel kayıt kılavuzları:

* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C#](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Bireysel kayıt oluşturduktan sonra değerini kaydedin **kayıt kimliği**. IOT Edge çalışma zamanı yapılandırdığınızda bu değeri kullanın.

## <a name="install-the-iot-edge-runtime"></a>IOT Edge çalışma zamanını yükleme

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenleri kapsayıcılarında çalıştırmak ve kod ucuna çalıştırabilmeniz için cihaza ek kapsayıcıları dağıtma olanak sağlar.

Cihazınızı sağlarken aşağıdaki bilgilere sahip olmanız gerekir:

* DPS **kimlik kapsamı** değeri
* Oluşturduğunuz cihaz **kayıt kimliği**

IoT Edge çalışma zamanını, sanal TPM 'nin çalıştığı cihaza yükler. IoT Edge çalışma zamanını otomatik, el ile değil, sağlama için yapılandıracaksınız.

> [!TIP]
> TPM simülatörünü yüklenmesi sırasında açık ve test çalıştıran pencereyi tutun.

Windows üzerinde IoT Edge yükleme hakkında daha ayrıntılı bilgi için IoT Edge, bkz. [Azure IoT Edge çalışma zamanını Windows 'A yükleme](how-to-install-iot-edge-windows.md).

1. Yönetici modunda bir PowerShell penceresi açın. PowerShell (x86) değil IoT Edge yüklerken PowerShell 'in AMD64 oturumunu kullandığınızdan emin olun.

1. **Deploy-ıotedge** komutu, Windows makinenizin desteklenen bir sürümde olup olmadığını denetler, kapsayıcılar özelliğini açar ve ardından Moby çalışma zamanını ve IoT Edge çalışma zamanını indirir. Komut varsayılan olarak Windows kapsayıcıları ' nı kullanmaktır.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

1. Bu noktada, IoT çekirdek cihazları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server cihazları yeniden başlatmanızı isteyebilir. Bu durumda cihazınızı şimdi yeniden başlatın. Cihazınız çalışmaya başladıktan sonra PowerShell 'i yönetici olarak yeniden çalıştırın.

1. **Initialize-ıotedge** komutu, makinenizde IoT Edge çalışma zamanını yapılandırır. Komut, Windows kapsayıcıları ile el ile sağlamayı varsayılan olarak belirler. El ile sağlama yerine cihaz sağlama hizmetini kullanmak için `-Dps` bayrağını kullanın.

   `{scope_id}` ve `{registration_id}` için yer tutucu değerlerini daha önce topladığınız verilerle değiştirin.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps -ScopeId {scope ID} -RegistrationId {registration ID}
   ```

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

Çalışma zamanı başarıyla başlatıldı, IOT Hub'ına gidin ve IOT Edge modülleri, cihazınıza dağıtmaya başlayın. Aşağıdaki komutlar, çalışma zamanı yüklü ve başarıyla başlatıldı doğrulamak için Cihazınızda kullanın.  

IoT Edge hizmetinin durumunu kontrol edin.

```powershell
Get-Service iotedge
```

Son 5 dakika Hizmeti günlüklerini inceleyin.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Çalışan modülleri listeleyin.

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Cihaz sağlama hizmeti kayıt işlemi, yeni cihaz sağlama gibi cihaz kimliği ve cihaz ikizi etiketleri aynı anda belirlemenizi sağlar. Bu değerleri ayrı ayrı cihazlar ya da otomatik cihaz Yönetimi'ni kullanarak cihaz grupları hedeflemek için kullanabilirsiniz. Bilgi edinmek için nasıl [dağıtma ve izleme IOT Edge modülleri, ölçeklendirme Azure portalını kullanarak](how-to-deploy-monitor.md) veya [Azure CLI kullanarak](how-to-deploy-monitor-cli.md)
