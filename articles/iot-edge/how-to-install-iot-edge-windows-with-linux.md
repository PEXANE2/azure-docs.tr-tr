---
title: Windows'da Linux için Azure IoT Edge'i yükleyin | Microsoft Dokümanlar
description: Windows 10, Windows Server ve Windows IoT Core'daki Linux kapsayıcıları için Azure IoT Edge yükleme yönergeleri
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: kgremban
ms.openlocfilehash: 8a4579e092bbc4fd58954f1ce1f1dad3a8ddbbba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80133172"
---
# <a name="use-iot-edge-on-windows-to-run-linux-containers"></a>Linux kapsayıcılarını çalıştırmak için Windows'da IoT Edge'i kullanma

Windows makinesi ni kullanarak Linux aygıtları için IoT Edge modüllerini test edin.

Bir üretim senaryosunda, Windows aygıtları yalnızca Windows kapsayıcıları çalıştırmalıdır. Ancak, yaygın bir geliştirme senaryosu Linux aygıtları için IoT Edge modülleri oluşturmak için bir Windows bilgisayarı kullanmaktır. Windows için IoT Edge çalışma süresi, **test ve geliştirme** amacıyla Linux kapsayıcılarını çalıştırmanıza olanak tanır.

Bu makalede, Windows x64 (AMD/Intel) sisteminizde Linux kapsayıcılarını kullanarak Azure IoT Edge çalışma süresini yükleme adımları listelenir. Tüm yükleme parametreleri hakkındaki ayrıntılar da dahil olmak üzere IoT Edge çalışma zamanı yükleyicisi hakkında daha fazla bilgi edinmek için [bkz.](how-to-install-iot-edge-windows.md)

IoT Edge'in en son sürümünde nelerin yer aldığı hakkında bilgi için [Azure IoT Edge sürümlerine](https://github.com/Azure/azure-iotedge/releases)bakın.

## <a name="prerequisites"></a>Ön koşullar

Windows aygıtınızın IoT Edge'i destekleyip desteklemeyeceğini gözden geçirmek ve yüklemeden önce bir kapsayıcı altyapısına hazırlamak için bu bölümü kullanın.

### <a name="supported-windows-versions"></a>Desteklenen Windows sürümleri

Linux kapsayıcıları ile Azure IoT [Edge, Docker Desktop gereksinimlerini](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) karşılayan windows'un herhangi bir sürümünde çalıştırılabilir

IoT Edge'i sanal bir makineye yüklemek istiyorsanız, iç içe geçmiş sanallaştırmayı etkinleştirin ve en az 2 GB bellek ayırın. İç içe sanallaştırmayı nasıl etkinleştirdiğiniz, kullandığınız hipervizöre bağlı olarak değişir. Hyper-V için, nesil 2 sanal makineler varsayılan olarak etkinleştirilen sanallaştırma iç içe var. VMWare için, sanal makinenizde özelliği etkinleştirmek için bir geçiş vardır.

### <a name="prepare-the-container-engine"></a>Konteyner motorini hazırlayın

Azure IoT Edge, [OCI uyumlu](https://www.opencontainers.org/) bir kapsayıcı motoruna dayanır. Windows makinesinde Windows ve Linux kapsayıcılarını çalıştırmak arasındaki en büyük yapılandırma farkı, IoT Edge yüklemesinin bir Windows kapsayıcı çalışma süresi içerolmasıdır, ancak IoT Edge'i yüklemeden önce Linux kapsayıcıları için kendi çalışma sürenizi sağlamanız gerekir.

Linux aygıtları için kapsayıcılar geliştirmek ve test etmek için bir Windows makinesi kurmak için [Docker Desktop'ı](https://www.docker.com/docker-windows) konteyner motoru olarak kullanabilirsiniz. Docker'ı yüklemeniz ve IoT Edge'i yüklemeden önce [Linux kapsayıcılarını kullanacak](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) şekilde yapılandırmanız gerekir.  

IoT Edge aygıtınız bir Windows bilgisayarıysa, Hyper-V için [sistem gereksinimlerini](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) karşılayıp karşılamadığını denetleyin.

## <a name="install-iot-edge-on-a-new-device"></a>IoT Edge'i yeni bir cihaza yükleme

>[!NOTE]
>Azure IoT Edge yazılım paketleri, paketlerde (LICENSE dizininde) bulunan lisans koşullarına tabidir. Lütfen paketi kullanmadan önce lisans koşullarını okuyun. Paketi yüklemeniz ve kullanmanız bu koşulları kabul etmek anlamına dalır. Lisans koşullarını kabul etmiyorsanız, paketi kullanmayın.

PowerShell komut dosyası Azure IoT Edge güvenlik daemon'u indirir ve yükler. Güvenlik daemon sonra diğer modüllerin uzaktan dağıtımsağlayan iki çalışma zamanı modülleri, IoT Edge aracısı, ilk başlar.

IoT Edge çalışma saatini ilk kez bir aygıta yüklediğinizde, aygıtı bir IoT hub'ından bir kimlik le sağlamanız gerekir. Tek bir IoT Edge aygıtı, IoT hub'ınız tarafından sağlanan aygıt bağlantıları dizesi kullanılarak el ile kullanılabilir. Veya, cihazları otomatik olarak sağlamak için Aygıt Sağlama Hizmetini kullanabilirsiniz, bu da ayarlayabileceğiniz çok sayıda aygıtınız olduğunda yararlıdır.

Farklı yükleme seçenekleri ve parametreleri hakkında daha fazla bilgi makalede [Windows'da Azure IoT Edge çalışma süresini yükleyin.](how-to-install-iot-edge-windows.md) Docker Desktop'ı Linux kapları için yükledikten ve yapılandırdıktan sonra, temel kurulum farkı Linux'u **-ContainerOs** parametresi ile bildirmektir. Örnek:

1. Henüz yapmadıysanız, yeni bir IoT Edge aygıtı kaydedin ve aygıt bağlantı dizesini alın. Daha sonra bu bölümde kullanmak üzere bağlantı dizesini kopyalayın. Aşağıdaki araçları kullanarak bu adımı tamamlayabilirsiniz:

   * [Azure portalında](how-to-register-device.md#register-in-the-azure-portal)
   * [Azure CLI](how-to-register-device.md#register-with-the-azure-cli)
   * [Visual Studio Code](how-to-register-device.md#register-with-visual-studio-code)

2. PowerShell'i yönetici olarak çalıştırın.

   >[!NOTE]
   >IoT Edge'i yüklemek için POWERShell'in AMD64 oturumunu kullanın, PowerShell'i (x86) değil. Hangi oturum türünü kullandığınızdan emin değilseniz aşağıdaki komutu çalıştırın:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. **Deploy-IoTEdge** komutu, Windows makinenizin desteklenen bir sürümde olup olmadığını denetler, kapsayıcılar özelliğini açar ve moby çalışma süresini (Linux kapsayıcıları için kullanılmaz) ve IoT Edge çalışma süresini indirir. Komut Windows kapsayıcılarına varsayılan olarak gelir, bu nedenle Linux'u istenen kapsayıcı işletim sistemi olarak bildirin.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Linux
   ```

4. Bu noktada, IoT Core aygıtları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server aygıtları yeniden başlatmanızı isteyebilir. Öyleyse, cihazınızı şimdi yeniden başlatın. Cihazınız hazır olduğunda PowerShell'i yeniden yönetici olarak çalıştırın.

5. **Initialize-IoTEdge komutu,** Makinenizdeki IoT Edge çalışma süresini yapılandırır. Komut, aygıt bağlantı dizesiyle el ile sağlama için varsayılandır. Linux'u tekrar istenilen konteyner işletim sistemi olarak bildirin.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ContainerOs Linux
   ```

6. İstendiğinde, adım 1'de aldığınız aygıt bağlantı dizesini sağlayın. Aygıt bağlantı dizesi, fiziksel aygıtı IoT Hub'ındaki bir aygıt kimliğiyle ilişkilendirer.

   Aygıt bağlantı dizesi aşağıdaki biçimi alır ve tırnak işaretleri içermemelidir:`HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

## <a name="verify-successful-installation"></a>Başarılı yüklemeyi doğrulama

IoT Edge hizmetinin durumunu kontrol edin:

```powershell
Get-Service iotedge
```

Son 5 dakikadaki servis günlüklerini inceleyin:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

En yaygın yapılandırma ve ağ hataları için otomatik denetim çalıştırın:

```powershell
iotedge check
```

Çalışan modülleri listele. Yeni bir yüklemeden sonra, çalışan görmeniz gereken tek modül **edgeAgent**olduğunu. [IoT Edge modüllerini](how-to-deploy-modules-portal.md) ilk kez dağıttıktan sonra, **edgeHub**adındaki diğer sistem modülü de cihazda başlayacaktır.

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Artık çalışma zamanı yüklü bir IoT Edge aygıtınız olduğuna göre, [IoT Edge modüllerini dağıtabilirsiniz.](how-to-deploy-modules-portal.md)

IoT Edge'in düzgün bir şekilde yüklenmesinde sorun yaşıyorsanız, [sorun giderme](troubleshoot.md) sayfasına göz atın.

Varolan bir yüklemeyi IoT Edge'in en yeni sürümüne güncelleştirmek için [bkz.](how-to-update-iot-edge.md)
