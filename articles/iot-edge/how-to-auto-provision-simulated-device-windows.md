---
title: WINDOWS aygıtlarını DPS ile otomatik olarak sağlama - Azure IoT Edge | Microsoft Dokümanlar
description: Azure IoT Edge için otomatik aygıt sağlamayı Aygıt Sağlama Hizmeti ile test etmek için Windows makinenizde simüle edilmiş bir aygıt kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/09/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ee4f01c3ec57b0cf9e3ecf47254b57be95ea051a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76510949"
---
# <a name="create-and-provision-a-simulated-iot-edge-device-with-a-virtual-tpm-on-windows"></a>Windows'da sanal TPM içeren simüle edilmiş bir IoT Edge aygıtı oluşturma ve sağlama

Azure IoT Edge aygıtları, kenar etkin olmayan aygıtlar gibi [Aygıt Sağlama Hizmeti](../iot-dps/index.yml) kullanılarak otomatik olarak kullanılabilir. Otomatik sağlama işlemine aşina değilseniz, devam etmeden önce [otomatik sağlama kavramlarını](../iot-dps/concepts-auto-provisioning.md) gözden geçirin.

DPS, hem bireysel kayıt hem de grup kaydında IoT Edge aygıtları için simetrik anahtar attestation'ı destekler. Grup kaydı için, simetrik anahtar attestation'da geçerli olmak üzere "IoT Edge aygıtıdır" seçeneğini işaretlerseniz, bu kayıt grubunun altında kayıtlı olan tüm aygıtlar IoT Edge aygıtları olarak işaretlenir.

Bu makalede, simüle edilmiş bir IoT Edge aygıtında otomatik sağlamanın nasıl test edilebildiğini aşağıdaki adımlarla gösterir:

* IoT Hub Aygıt Sağlama Hizmeti (DPS) örneğini oluşturun.
* Donanım güvenliği için simüle edilmiş güvenilir platform modülü (TPM) içeren Windows makinenizde simüle edilmiş bir aygıt oluşturun.
* Aygıt için tek tek bir kayıt oluşturun.
* IoT Edge çalışma süresini yükleyin ve aygıtı IoT Hub'ına bağlayın.

> [!TIP]
> Bu makalede, sanal aygıtlarda TPM attestation kullanarak otomatik sağlama test açıklanır, ancak çok fiziksel TPM donanım kullanırken de geçerlidir.

## <a name="prerequisites"></a>Ön koşullar

* Windows geliştirme makinesi. Bu makalede Windows 10 kullanır.
* Etkin bir IoT Hub'ı.

> [!NOTE]
> TPM 2.0 DPS ile TPM attestation kullanırken gereklidir ve sadece bireysel değil, grup, kayıtları oluşturmak için kullanılabilir.

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>IoT Hub Cihaz Sağlama Hizmetini Ayarlama

Azure'da IoT Hub Aygıt Sağlama Hizmeti'nin yeni bir örneğini oluşturun ve IoT hub'ınıza bağlayın. [IoT Hub DPS'yi](../iot-dps/quick-setup-auto-provision.md)kur'a'daki yönergeleri takip edebilirsiniz.

Aygıt Sağlama Hizmeti çalıştırdıktan **sonra, kimlik kapsamının** değerini genel bakış sayfasından kopyalayın. IoT Edge çalışma süresini yapılandırırken bu değeri kullanırsınız.

> [!TIP]
> Fiziksel bir TPM aygıtı kullanıyorsanız, her TPM yongasına özgü olan ve onunla ilişkili TPM yonga üreticisinden alınan **Onay anahtarını**belirlemeniz gerekir. TPM aygıtınız için benzersiz bir **Kayıt Kimliği** elde ederek, örneğin, onay anahtarının SHA-256 karmasını oluşturarak.
>
> DpS'de kaydınızı oluşturmak için [Azure Portal ile cihaz kayıtlarını nasıl yöneteceğimiz](../iot-dps/how-to-manage-enrollments.md) ve ardından bu makaledeki [IoT Edge çalışma süresini yükleme](#install-the-iot-edge-runtime) bölümüne devam etmek için makaledeki yönergeleri izleyin.

## <a name="simulate-a-tpm-device"></a>TPM cihazının benzetimini yapma

Windows geliştirme makinenizde simüle edilmiş bir TPM aygıtı oluşturun. Cihazınız için **Kayıt Kimliği** ve **Onay anahtarını** alın ve bunları DPS'de tek bir kayıt girişi oluşturmak için kullanın.

DPS'de bir kayıt oluşturduğunuzda, Başlangıç **Aygıtı İkiz Durumu**bildirme fırsatınız vardır. Aygıt ikizinde etiketleri, bölge, ortam, konum veya aygıt türü gibi çözümünüzde gereksinim duyduğunuz herhangi bir metrike göre aygıtları gruplandırmak için ayarlayabilirsiniz. Bu etiketler otomatik [dağıtımlar](how-to-deploy-monitor.md)oluşturmak için kullanılır.

Benzetimli aygıtı oluşturmak için kullanmak istediğiniz SDK dilini seçin ve tek tek kaydı oluşturana kadar adımları izleyin.

Tek tek kaydı oluşturduğunuzda, Windows geliştirme makinenizdeki simüle edilmiş TPM aygıtının bir **IoT Edge aygıtı**olduğunu bildirmek için **True'yu** seçin.

Simüle cihaz ve bireysel kayıt kılavuzları:

* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C #](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Tek tek kaydı oluşturduktan sonra, **Kayıt Kimliği'nin**değerini kaydedin. IoT Edge çalışma süresini yapılandırırken bu değeri kullanırsınız.

## <a name="install-the-iot-edge-runtime"></a>IoT Edge çalışma süresini yükleme

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenleri kapsayıcılarda çalışır ve kenarda kod çalıştırabilmeniz için aygıta ek kapsayıcılar dağıtmanızı sağlar.

Cihazınızı sağlarken aşağıdaki bilgilere ihtiyacınız olacak:

* DPS **Id Kapsam** değeri
* Oluşturduğunuz aygıt **Kayıt Kimliği**

IoT Edge çalışma zamanını benzetimli TPM'yi çalıştıran aygıta yükleyin. IoT Edge çalışma süresini manuel değil otomatik olarak yapılandıracaksınız.

> [!TIP]
> Yükleme ve test sırasında TPM simülatörü çalıştıran pencereyi açık tutun.

Kapsayıcıları yönetme ve IoT Edge'i güncelleştirme gibi görevler için ön koşullar ve yönergeler de dahil olmak üzere Windows'a IoT [Edge](how-to-install-iot-edge-windows.md)yükleme hakkında daha ayrıntılı bilgi için bkz.

1. Yönetici modunda bir PowerShell penceresi açın. IoT Edge'i yüklerken AMD64 PowerShell oturumunu kullandığınızdan emin olun, PowerShell'i (x86) değil.

1. **Deploy-IoTEdge** komutu, Windows makinenizin desteklenen bir sürümde olup olmadığını denetler, kapsayıcılar özelliğini açar ve moby çalışma süresini ve IoT Edge çalışma süresini karşıdan yüklez. Komut, Windows kapsayıcılarını kullanmayı varsayılan olarak kullanır.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

1. Bu noktada, IoT Core aygıtları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server aygıtları yeniden başlatmanızı isteyebilir. Öyleyse, cihazınızı şimdi yeniden başlatın. Cihazınız hazır olduğunda PowerShell'i yeniden yönetici olarak çalıştırın.

1. **Initialize-IoTEdge komutu,** Makinenizdeki IoT Edge çalışma süresini yapılandırır. Komut, Windows kapsayıcılarıyla el ile sağlama için varsayılandır. El `-Dps` ile sağlama yerine Aygıt Sağlama Hizmetini kullanmak için bayrağı kullanın.

   Yer tutucu değerlerini `{scope_id}` daha `{registration_id}` önce topladığınız verilerle değiştirin.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps -ScopeId {scope ID} -RegistrationId {registration ID}
   ```

## <a name="verify-successful-installation"></a>Başarılı yüklemeyi doğrulama

Çalışma süresi başarıyla başladıysa, IoT Hub'ınıza gidip IoT Edge modüllerini cihazınıza dağıtmaya başlayabilirsiniz. Çalışma zamanının yüklü olduğunu ve başarılı bir şekilde başladığını doğrulamak için cihazınızdaki aşağıdaki komutları kullanın.  

IoT Edge hizmetinin durumunu kontrol edin.

```powershell
Get-Service iotedge
```

Son 5 dakikadaki servis kayıtlarını inceleyin.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Çalışan modülleri listele.

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Aygıt Sağlama Hizmeti kayıt işlemi, aygıt kimliğini ve aygıt ikiz etiketlerini yeni aygıtı sağlarken aynı anda ayarlamanızı sağlar. Bu değerleri, otomatik aygıt yönetimini kullanarak tek tek aygıtları veya aygıt gruplarını hedeflemek için kullanabilirsiniz. Azure portalını kullanarak veya [Azure CLI'yi kullanarak](how-to-deploy-monitor-cli.md) [IoT Edge modüllerini ölçekte](how-to-deploy-monitor.md) nasıl dağıtılayacağım ve izleyeceğinizi öğrenin
