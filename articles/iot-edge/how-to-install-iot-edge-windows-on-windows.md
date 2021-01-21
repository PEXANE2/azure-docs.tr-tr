---
title: Windows için Azure IoT Edge 'i yükler | Microsoft Docs
description: Windows cihazlarına Windows kapsayıcıları için Azure IoT Edge yüklemesi
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: kgremban
ms.openlocfilehash: 7857f93e8c767f270041bb6bf041447786ce19ff
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634245"
---
# <a name="install-and-manage-azure-iot-edge-for-windows"></a>Windows için Azure IoT Edge yükleyip yönetme

Windows için Azure IoT Edge doğrudan ana bilgisayar Windows cihazınızda çalışır ve kenarda iş mantığını çalıştırmak için Windows kapsayıcıları kullanır.

Azure IoT Edge çalışma zamanı, bir cihazı IoT Edge cihazına dönüştürür. Çalışma zamanı, cihazlarda Raspberry Pi kadar küçük veya endüstriyel sunucu olarak büyük olarak dağıtılabilir. Bir cihaz IoT Edge çalışma zamanıyla yapılandırıldığında, buluttan cihaza iş mantığını dağıtmaya başlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure IoT Edge çalışma zamanını ve mimarisini anlayın](iot-edge-runtime.md).

>[!NOTE]
>Windows için Azure IoT Edge, Azure IoT Edge sürüm 1.2.0 ile başlayarak desteklenmez.
>
>Windows cihazlarda IoT Edge çalıştırmak için yeni yöntemi kullanmayı düşünün, Windows 'da Linux için Azure IoT Edge.

<!-- TODO: link to EFLOW-->

IoT Edge bir cihaz kurmak için iki adım vardır. İlk adım çalışma zamanını ve bağımlılıklarını yüklemektir. İkinci adım, cihazı buluttaki kimliğine bağlamak ve IoT Hub kimlik doğrulamasını kurmak olur.

Bu makalede, Windows cihazlarına Azure IoT Edge çalışma zamanını yüklemek için gereken adımlar listelenmektedir. Çalışma zamanını yüklediğinizde, Linux kapsayıcıları veya Windows kapsayıcıları kullanma seçeneğiniz vardır. Şu anda, üretim senaryolarında yalnızca Windows 'daki Windows kapsayıcıları desteklenir. Windows üzerinde Linux kapsayıcıları, özellikle de Linux cihazlarına dağıtmak üzere bir Windows BILGISAYARı geliştiriyorsanız geliştirme ve test senaryoları için yararlıdır.

## <a name="prerequisites"></a>Önkoşullar

* Bir Windows cihazı

  Windows kapsayıcıları ile IoT Edge, en son [Windows uzun süreli destek derlemesi](/windows/release-information/)olan Windows sürümü 1809/Build 17762 gerektirir. Geliştirme ve test senaryolarında, kapsayıcıları özelliğini destekleyen herhangi bir SKU (Pro, kurumsal, sunucu vb.) çalışır. Ancak, üretime geçmeden önce [desteklenen sistemler listesini](support.md#operating-systems) gözden geçirdiğinizden emin olun.

  Linux kapsayıcılarıyla IoT Edge, [Docker Desktop gereksinimlerini](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)karşılayan herhangi bir Windows sürümünde çalıştırılabilir.

* Cihazda kapsayıcı desteği

  Azure IoT Edge, [OCI uyumlu](https://www.opencontainers.org/) bir kapsayıcı altyapısına bağlıdır. Cihazınızın kapsayıcıları destekleyeceği şekilde emin olun.

  Bir sanal makineye IoT Edge yüklüyorsanız, iç içe sanallaştırmayı etkinleştirin ve en az 2 GB bellek ayırın. Hyper-V için 2. nesil sanal makinelerde varsayılan olarak etkinleştirilmiş iç içe sanallaştırma vardır. VMware için, sanal makinenizde özelliği etkinleştirmek üzere bir geçiş düğmesi vardır.

  IoT çekirdek cihazına IoT Edge yüklüyorsanız, cihazınızda Windows kapsayıcılarının desteklenip desteklenmediğini denetlemek için [uzak bir PowerShell oturumunda](/windows/iot-core/connect-your-device/powershell) aşağıdaki komutu kullanın:

  ```powershell
  Get-Service vmcompute
  ```

* [Kayıtlı bir cıhaz kimliği](how-to-register-device.md)

  Cihazınızın simetrik anahtar kimlik doğrulaması ile kaydolduysanız, cihaz bağlantı dizesinin hazır olmasını sağlayabilirsiniz.

  Cihazınızı X. 509.440 otomatik olarak imzalanan sertifika kimlik doğrulaması ile kaydettiniz, cihazı kaydetmek için kullandığınız kimlik sertifikalarından en az birine ve ilgili özel özel anahtarı cihazınızda kullanılabilir.

## <a name="install-a-container-engine"></a>Kapsayıcı Altyapısı yükler

Azure IoT Edge, OCı uyumlu bir kapsayıcı çalışma zamanına bağlıdır. Üretim senaryolarında, Moby tabanlı altyapıyı kullanmanızı öneririz. Moby motoru, Azure IoT Edge ile resmi olarak desteklenen tek kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntüleri Moby çalışma zamanına göre uyumludur.

Üretim senaryolarında, yükleme betiğine dahil olan Moby tabanlı altyapıyı kullanın. Altyapıyı yüklemek için başka bir adım yoktur.

Linux kapsayıcılarıyla IoT Edge için kendi kapsayıcı çalışma zamanını sağlamanız gerekir. Cihazınıza [Docker Desktop](https://docs.docker.com/docker-for-windows/install/) 'ı yükleyip devam etmeden önce [Linux kapsayıcılarını kullanacak](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) şekilde yapılandırın.

## <a name="install-the-iot-edge-security-daemon"></a>IoT Edge güvenlik cini 'nı yükler

>[!TIP]
>IoT çekirdek cihazları için, uzak bir PowerShell oturumu kullanarak yükleme komutlarının çalıştırılmasını öneririz. Daha fazla bilgi için bkz. [Windows IoT Için PowerShell kullanma](/windows/iot-core/connect-your-device/powershell).

1. PowerShell'i yönetici olarak çalıştırın.

   PowerShell (x86) değil, PowerShell için AMD64 oturumu kullanın. Hangi oturum türünü kullandığınızı bilmiyorsanız, aşağıdaki komutu çalıştırın:

   ```powershell
   (Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   ```

2. Aşağıdaki görevleri gerçekleştiren [Deploy-ıotedge](reference-windows-scripts.md#deploy-iotedge) komutunu çalıştırın:

   * Windows makinenizin desteklenen bir sürümde olup olmadığını denetler.
   * Kapsayıcılar özelliğini etkinleştirir.
   * Moby altyapısını ve IoT Edge çalışma zamanını indirir.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

   `Deploy-IoTEdge`Komut varsayılan olarak Windows kapsayıcıları ' nı kullanmaktır. Linux kapsayıcıları kullanmak istiyorsanız, şu `ContainerOs` parametreyi ekleyin:

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Linux
   ```

3. Bu noktada, IoT çekirdek cihazları otomatik olarak yeniden başlatılabilir. Windows 10 veya Windows Server cihazları yeniden başlatmanızı isteyebilir. Bu durumda cihazınızı şimdi yeniden başlatın.

Bir cihaza IoT Edge yüklediğinizde, aşağıdakileri de içeren ek parametreleri kullanarak işlemi değiştirebilirsiniz:

* Bir proxy sunucusundan gelen trafiği doğrudan gönder
* Yükleyiciyi çevrimdışı yükleme için yerel bir dizine işaret edin.

Bu ek parametreler hakkında daha fazla bilgi için bkz. [Windows üzerinde IoT Edge Için PowerShell betikleri](reference-windows-scripts.md).

## <a name="provision-the-device-with-its-cloud-identity"></a>Cihazı bulut kimliğiyle sağlama

Artık kapsayıcı altyapısı ve IoT Edge çalışma zamanı cihazınızda yüklü olduğuna göre, bir sonraki adım için hazır olursunuz. Bu, cihazı bulut kimliği ve kimlik doğrulama bilgileriyle ayarlamaya yöneliktir.

Kullanmak istediğiniz kimlik doğrulama türüne göre sonraki bölümü seçin:

* [Seçenek 1: simetrik anahtarlarla kimlik doğrulama](#option-1-authenticate-with-symmetric-keys)
* [Seçenek 2: X. 509.440 sertifikalarıyla kimlik doğrulama](#option-2-authenticate-with-x509-certificates)

### <a name="option-1-authenticate-with-symmetric-keys"></a>Seçenek 1: simetrik anahtarlarla kimlik doğrulama

Bu noktada, IoT Edge çalışma zamanı Windows cihazınıza yüklenir ve cihazı bulut kimliği ve kimlik doğrulama bilgileriyle sağlamanız gerekir.

Bu bölümde, simetrik anahtar kimlik doğrulaması ile cihaz sağlama adımları gösterilmektedir. Cihazınızı IoT Hub kaydolmalı ve cihaz bilgileri 'nden bağlantı dizesini almıştır. Aksi takdirde, [IoT Hub IoT Edge cihazı kaydetme](how-to-register-device.md)bölümündeki adımları izleyin.

1. IoT Edge cihazda, PowerShell 'i yönetici olarak çalıştırın.

2. Makinenizde IoT Edge çalışma zamanını yapılandırmak için [Initialize-ıotedge](reference-windows-scripts.md#initialize-iotedge) komutunu kullanın. Komut, Windows kapsayıcıları ile el ile sağlamayı varsayılan olarak belirler.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ManualConnectionString -ContainerOs Windows
   ```

   * Linux kapsayıcıları kullanıyorsanız, `-ContainerOs` bayrağına parametresini ekleyin. Daha önce çalıştırdığınız komutla seçtiğiniz kapsayıcı seçeneği ile tutarlı olun `Deploy-IoTEdge` .

      ```powershell
      . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
      Initialize-IoTEdge -ContainerOs Linux
      ```

   * IoTEdgeSecurityDaemon.ps1 betiğini çevrimdışı veya belirli bir sürüm yüklemesi için cihazınıza indirdiyseniz, betiğin yerel kopyasına başvurduğunuzdan emin olun.

      ```powershell
      . <path>/IoTEdgeSecurityDaemon.ps1
      Initialize-IoTEdge -ManualX509
      ```

3. İstendiğinde, önceki bölümde aldığınız cihaz bağlantı dizesini belirtin. Cihaz bağlantı dizesi, fiziksel cihazı IoT Hub bir cihaz KIMLIĞIYLE ilişkilendirir ve kimlik doğrulama bilgilerini sağlar.

   Cihaz bağlantı dizesi aşağıdaki biçimi alır ve tırnak işaretleri içermemelidir: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

Bir cihazı el ile sağladığınızda, aşağıdakileri de içeren ek parametreleri kullanarak işlemi değiştirebilirsiniz:

* Bir proxy sunucusundan gelen trafiği doğrudan gönder
* Belirli bir edgeAgent kapsayıcı görüntüsü bildirin ve özel bir kayıt defterindeki kimlik bilgilerini sağlayın

Bu ek parametreler hakkında daha fazla bilgi için bkz. [Windows üzerinde IoT Edge Için PowerShell betikleri](reference-windows-scripts.md).

### <a name="option-2-authenticate-with-x509-certificates"></a>Seçenek 2: X. 509.440 sertifikalarıyla kimlik doğrulama

Bu noktada, IoT Edge çalışma zamanı Windows cihazınıza yüklenir ve cihazı bulut kimliği ve kimlik doğrulama bilgileriyle sağlamanız gerekir.

Bu bölümde, X. 509.440 sertifika kimlik doğrulamasıyla bir cihaz sağlama adımları gösterilmektedir. Cihazınızı, IoT Edge cihazınızda bulunan sertifikayla ve özel anahtarla eşleşen parmak izleri sağlayan IoT Hub 'ye kaydettiniz. Aksi takdirde, [IoT Hub IoT Edge cihazı kaydetme](how-to-register-device.md)bölümündeki adımları izleyin.

1. IoT Edge cihazda, PowerShell 'i yönetici olarak çalıştırın.

2. Makinenizde IoT Edge çalışma zamanını yapılandırmak için [Initialize-ıotedge](reference-windows-scripts.md#initialize-iotedge) komutunu kullanın.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ManualX509
   ```

   * Linux kapsayıcıları kullanıyorsanız, `-ContainerOs` bayrağına parametresini ekleyin. Daha önce çalıştırdığınız komutla seçtiğiniz kapsayıcı seçeneği ile tutarlı olun `Deploy-IoTEdge` .

      ```powershell
      . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
      Initialize-IoTEdge -ManualX509 -ContainerOs Linux
      ```

   * IoTEdgeSecurityDaemon.ps1 betiğini çevrimdışı veya belirli bir sürüm yüklemesi için cihazınıza indirdiyseniz, betiğin yerel kopyasına başvurduğunuzdan emin olun.

      ```powershell
      . <path>/IoTEdgeSecurityDaemon.ps1
      Initialize-IoTEdge -ManualX509
      ```

3. İstendiğinde aşağıdaki bilgileri girin:

   * **Iothubhostname**: cihazın bağlanacağı IoT Hub 'ının ana bilgisayar adı. Örneğin, `{IoT hub name}.azure-devices.net`.
   * **DeviceID**: cihazı kaydettirdiğiniz sırada verdiğiniz kimlik.
   * **X509IdentityCertificate**: cihazdaki bir kimlik sertifikasının mutlak yolu. Örneğin, `C:\path\identity_certificate.pem`.
   * **X509IdentityPrivateKey**: belirtilen kimlik sertifikası için özel anahtar dosyasının mutlak yolu. Örneğin, `C:\path\identity_key.pem`.

Bir cihazı el ile sağladığınızda, aşağıdakileri de içeren ek parametreleri kullanarak işlemi değiştirebilirsiniz:

* Bir proxy sunucusundan gelen trafiği doğrudan gönder
* Belirli bir edgeAgent kapsayıcı görüntüsü bildirin ve özel bir kayıt defterindeki kimlik bilgilerini sağlayın

Bu ek parametreler hakkında daha fazla bilgi için bkz. [Windows üzerinde IoT Edge Için PowerShell betikleri](reference-windows-scripts.md).

## <a name="offline-or-specific-version-installation-optional"></a>Çevrimdışı veya belirli sürümü yükleme (isteğe bağlı)

Bu bölümdeki adımlar, standart yükleme adımlarının kapsamadığı senaryolar içindir. Bu şunlar olabilir:

* Çevrimdışıyken IoT Edge yüklemesi
* Sürüm Adayı sürümü yüklemesi
* En son sürümü dışında bir sürüm yükler

Yükleme sırasında üç dosya indirilir:

* Yükleme yönergelerini içeren bir PowerShell betiği
* IoT Edge güvenlik cini (ıotedşlı), Moby kapsayıcı altyapısını ve Moby CLı 'yi içeren IoT Edge cab Microsoft Azure
* Visual C++ yeniden dağıtılabilir paket (VC çalışma zamanı) yükleyicisi

Cihazınız yükleme sırasında çevrimdışı kalırsa veya IoT Edge belirli bir sürümünü yüklemek istiyorsanız, bu dosyaları cihaza bir süre önce indirebilirsiniz. Yükleme zamanı olduğunda, yükleme betiğini indirilen dosyaları içeren dizine işaret edin. Yükleyici önce bu dizini denetler ve sonra yalnızca bulunamayan bileşenleri indirir. Tüm dosyalar çevrimdışı kullanılabilir ise, internet bağlantısı olmadan yükleyebilirsiniz.

1. Önceki sürümlerle birlikte en son IoT Edge yükleme dosyaları için bkz. [Azure IoT Edge yayınlar](https://github.com/Azure/azure-iotedge/releases).

2. Yüklemek istediğiniz sürümü bulun ve sürüm notlarının **varlıklar** bölümünden aşağıdaki dosyaları IoT cihazınıza indirin:

   * IoTEdgeSecurityDaemon.ps1
   * 1.0.9 veya daha yeni sürümlerden Microsoft-Azure-IoTEdge.cab veya 1.0.8 'den daha eski sürümlerden Microsoft-Azure-IoTEdge-amd64.cab.

   Microsoft-Azure-IotEdge-arm32.cab, yalnızca test için 1.0.9 ile başlayarak da kullanılabilir. IoT Edge Şu anda Windows ARM32 cihazlarında desteklenmemektedir.

   Her sürümdeki özellikleri desteklemek üzere değiştiğinden, kullandığınız. cab dosyası ile aynı sürümden PowerShell betiğinin kullanılması önemlidir.

3. İndirdiğiniz. cab dosyasının üzerinde bir mimari soneki varsa, dosyayı yalnızca **Microsoft-Azure-IoTEdge.cab** olarak yeniden adlandırın.

4. İsteğe bağlı olarak, Visual C++ yeniden dağıtılabilir için bir yükleyici indirebilirsiniz. Örneğin, PowerShell betiği şu sürümü kullanır: [vc_redist.x64.exe](https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe). Yükleyiciyi, IoT Edge dosyaları olarak IoT cihazınızda aynı klasöre kaydedin.

5. Çevrimdışı bileşenlerle yüklemek için, [nokta kaynağı](/powershell/module/microsoft.powershell.core/about/about_scripts#script-scope-and-dot-sourcing) PowerShell betiğinin yerel kopyasıdır.

6. Parametresiyle [Deploy-ıotedge](reference-windows-scripts.md#deploy-iotedge) komutunu çalıştırın `-OfflineInstallationPath` . Dosya dizinine mutlak yol girin. Örneğin,

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Deploy-IoTEdge -OfflineInstallationPath <path>
   ```

   Dağıtım komutu, belirtilen yerel dosya dizininde bulunan tüm bileşenleri kullanır. . Cab dosyası veya Visual C++ yükleyicisi eksikse, bunları indirmeyi dener.

## <a name="update-iot-edge"></a>Güncelleştirme IoT Edge

`Update-IoTEdge`Güvenlik cini 'nı güncelleştirmek için komutunu kullanın. Komut dosyası otomatik olarak güvenlik arka plan programının en son sürümünü çeker.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

Update-IoTEdge komutunun çalıştırılması, iki çalışma zamanı kapsayıcı görüntüsü ile birlikte cihazınızdan güvenlik arka plan programını kaldırır ve güncelleştirir. Config. YAML dosyası cihazda, ayrıca Moby kapsayıcı altyapısından (Windows kapsayıcıları kullanıyorsanız) veriler tutulur. Yapılandırma bilgilerinin tutulması, güncelleştirme işlemi sırasında cihazınız için bağlantı dizesini veya cihaz sağlama hizmeti bilgilerini yeniden sağlamanız gerekmediği anlamına gelir.

Güvenlik arka plan programının belirli bir sürümüne güncelleştirmek istiyorsanız, [IoT Edge sürümlerden](https://github.com/Azure/azure-iotedge/releases)hedeflemek istediğiniz sürümü bulun. Bu sürümde, **Microsoft-Azure-IoTEdge.cab** dosyasını indirin. Ardından, `-OfflineInstallationPath` yerel dosya konumunu işaret etmek için parametresini kullanın. Örnek:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux> -OfflineInstallationPath <absolute path to directory>
```

>[!NOTE]
>`-OfflineInstallationPath`Parametresi, belirtilen dizinde **Microsoft-Azure-IoTEdge.cab** adlı bir dosya arar. IoT Edge Version 1.0.9-RC4 ile başlayarak, biri AMD64 cihaz ve diğeri ARM32 için kullanılabilecek iki. cab dosyası vardır. Cihazınız için doğru dosyayı indirin ve ardından mimari sonekini kaldırmak için dosyayı yeniden adlandırın.

Bir cihazı çevrimdışı güncelleştirmek istiyorsanız [Azure IoT Edge sürümlerden](https://github.com/Azure/azure-iotedge/releases)hedeflemek istediğiniz sürümü bulun. Bu sürümde, *IoTEdgeSecurityDaemon.ps1* ve *Microsoft-Azure-IoTEdge.cab* dosyalarını indirin. Her sürümdeki özellikleri desteklemek üzere değiştiğinden, kullandığınız. cab dosyası ile aynı sürümden PowerShell betiğinin kullanılması önemlidir.

İndirdiğiniz. cab dosyasının üzerinde bir mimari soneki varsa, dosyayı yalnızca **Microsoft-Azure-IoTEdge.cab** olarak yeniden adlandırın.

Çevrimdışı bileşenlerle güncelleştirmek için, [nokta kaynağı](/powershell/module/microsoft.powershell.core/about/about_scripts#script-scope-and-dot-sourcing) PowerShell betiğinin yerel kopyasıdır. Sonra, `-OfflineInstallationPath` parametresini komutunun bir parçası olarak kullanın `Update-IoTEdge` ve dosya dizinine mutlak yolu sağlayın. Örneğin,

```powershell
. <path>\IoTEdgeSecurityDaemon.ps1
Update-IoTEdge -OfflineInstallationPath <path>
```

Güncelleştirme seçenekleri hakkında daha fazla bilgi için komutunu kullanın `Get-Help Update-IoTEdge -full` veya [Windows üzerinde IoT Edge için PowerShell betiği](reference-windows-scripts.md)bölümüne başvurun.

## <a name="uninstall-iot-edge"></a>IoT Edge kaldır

IoT Edge yüklemesini cihazınızdan kaldırmak istiyorsanız aşağıdaki komutları kullanın.

IoT Edge yüklemesini Windows cihazınızdan kaldırmak istiyorsanız, bir yönetim PowerShell penceresinden [Uninstall-ıotedge](reference-windows-scripts.md#uninstall-iotedge) komutunu kullanın. Bu komut, IoT Edge çalışma zamanını, mevcut yapılandırmanızla ve Moby motoru verileriyle birlikte kaldırır.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-IoTEdge
```

`Uninstall-IoTEdge`Komut Windows IoT Core üzerinde çalışmaz. IoT Edge kaldırmak için Windows IoT çekirdek görüntünüzü yeniden dağıtmanız gerekir.

Kaldırma seçenekleri hakkında daha fazla bilgi için komutunu kullanın `Get-Help Uninstall-IoTEdge -full` .

## <a name="next-steps"></a>Sonraki adımlar

Modülleri cihazınıza dağıtmayı öğrenmek için [IoT Edge modüllerini dağıtmaya](how-to-deploy-modules-portal.md) devam edin.
