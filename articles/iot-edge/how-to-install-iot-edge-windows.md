---
title: Azure IoT Edge Windows 'a yükler | Microsoft Docs
description: Windows 10, Windows Server ve Windows IoT Core 'daki yükleme yönergelerini Azure IoT Edge
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: kgremban
ms.openlocfilehash: ba3e8b9d7649d56d1639f7f608d85a2da04ff74a
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84465567"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Windows'a Azure IoT Edge çalışma zamanını yükleme

Azure IoT Edge çalışma zamanı, bir cihazı IoT Edge cihazına dönüştürür. Çalışma zamanı, cihazlarda Raspberry Pi kadar küçük veya endüstriyel sunucu olarak büyük olarak dağıtılabilir. Bir cihaz IoT Edge çalışma zamanına göre yapılandırıldıktan sonra, buluta iş mantığı dağıtmaya başlayabilirsiniz.

IoT Edge çalışma zamanı hakkında daha fazla bilgi için bkz. [Azure IoT Edge çalışma zamanını ve mimarisini anlayın](iot-edge-runtime.md).

Bu makalede, Windows kapsayıcıları kullanarak Windows x64 (AMD/Intel) sisteminizde Azure IoT Edge çalışma zamanını yüklemek için gereken adımlar listelenmektedir.

> [!NOTE]
> Bilinen bir Windows işletim sistemi sorunu, IoT Edge modüller (işlem yalıtılmış Windows nano sunucu kapsayıcıları) çalışırken uyku ve hazırda bekleme güç durumlarına geçiş yapılmasını önler. Bu sorun, cihazdaki pil ömrünü etkiler.
>
> Geçici bir çözüm olarak, `Stop-Service iotedge` Bu güç durumlarını kullanmadan önce çalışan IoT Edge modüllerini durdurmak için komutunu kullanın.

Windows sistemlerinde Linux kapsayıcıları kullanmak, Azure IoT Edge için önerilen veya desteklenen bir üretim yapılandırması değildir. Ancak, geliştirme ve test amacıyla kullanılabilir. Daha fazla bilgi için bkz. [Linux kapsayıcılarını çalıştırmak Için Windows 'ta IoT Edge kullanma](how-to-install-iot-edge-windows-with-linux.md).

En son IoT Edge sürümüne nelerin dahil olduğu hakkında bilgi için, bkz. [Azure IoT Edge sürümler](https://github.com/Azure/azure-iotedge/releases).

## <a name="prerequisites"></a>Ön koşullar

Windows cihazınızın IoT Edge destekleyip desteklemediğini gözden geçirmek ve yüklemeden önce bir kapsayıcı altyapısı için hazırlamak üzere bu bölümü kullanın.

### <a name="supported-windows-versions"></a>Desteklenen Windows sürümleri

Windows için IoT Edge, Windows 'un en son [Windows uzun süreli destek derlemesi](https://docs.microsoft.com/windows/release-information/)olan Windows sürümü 1809/Build 17763 gerektirir. Windows SKU desteği için bkz. üretim senaryoları veya geliştirme ve test senaryoları için hazırlandıklarınıza göre nelerin desteklendiği.

* **Üretim**: Şu anda üretim senaryolarında desteklenen işletim sistemleri hakkında en son bilgiler için, bkz. [Azure IoT Edge desteklenen sistemler](support.md#operating-systems).
* **Geliştirme ve test**: geliştirme ve test senaryoları için Windows kapsayıcılarıyla Azure IoT Edge, kapsayıcılar özelliğini destekleyen windows Build 17763 ' in HERHANGI bir SKU 'Ya (Pro, Enterprise, Server vb.) yüklenebilir.

IoT çekirdek cihazları, IoT Edge çalışma zamanını desteklemek için IoT Core Windows kapsayıcıları isteğe bağlı özelliğini içermelidir. Windows kapsayıcıları 'nın cihazınızda desteklenip desteklenmediğini denetlemek için [uzak bir PowerShell oturumunda](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell) aşağıdaki komutu kullanın:

```powershell
Get-Service vmcompute
```

Hizmet mevcutsa, **çalışıyor**olarak listelenen hizmet durumu ile başarılı bir yanıt almanız gerekir. `vmcompute`Hizmet bulunamazsa, cihazınız IoT Edge gereksinimlerini karşılamaz. Bu özellik için destek almak üzere donanım sağlayıcınızla iletişim kurun.

### <a name="prepare-for-a-container-engine"></a>Kapsayıcı altyapısı için hazırlanma

Azure IoT Edge, [OCI uyumlu](https://www.opencontainers.org/) bir kapsayıcı altyapısına bağlıdır. Üretim senaryoları için, Windows cihazınızda Windows kapsayıcıları çalıştırmak üzere yükleme betiğine eklenen Moby altyapısını kullanın.

## <a name="install-iot-edge-on-a-new-device"></a>Yeni bir cihaza IoT Edge yüklemesi

>[!NOTE]
>Azure IoT Edge yazılım paketleri, paketlerde bulunan lisans koşullarına tabidir (LISANS dizininde). Lütfen paketi kullanmadan önce lisans koşullarını okuyun. Paketi yüklemeniz ve kullanmanız, bu şartlarınızın kabul edildiğini oluşturur. Lisans koşullarını kabul etmiyorsanız, paketini kullanmayın.

Bir PowerShell betiği, Azure IoT Edge güvenlik cini indirir ve yükler. Güvenlik cini daha sonra, diğer modüllerin uzak dağıtımlarını sağlayan IoT Edge Aracısı olan iki çalışma zamanı modülünün ilki başlatılır.

>[!TIP]
>IoT çekirdek cihazları için, bir RemotePowerShell oturumu kullanarak yükleme komutlarının çalıştırılmasını öneririz. Daha fazla bilgi için bkz. [Windows IoT Için PowerShell kullanma](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell).

IoT Edge çalışma zamanını bir cihaza ilk kez yüklediğinizde, cihazı IoT Hub 'ından bir kimlikle sağlamanız gerekir. Tek bir IoT Edge cihaz, IoT Hub tarafından sağlanan bir cihaz bağlantı dizesi kullanılarak el ile sağlanabilir. Ya da cihaz sağlama hizmeti 'ni (DPS), cihazları otomatik olarak sağlamak için kullanabilirsiniz. Bu, ayarlanacak birçok cihazınız olduğunda yararlı olur. Sağlama seçiminize bağlı olarak, uygun yükleme betiğini seçin.

Aşağıdaki bölümlerde, yeni bir cihazdaki IoT Edge yükleme betiği için ortak kullanım durumları ve parametreleri açıklanır.

### <a name="option-1-install-and-manually-provision"></a>Seçenek 1: yükleyip el ile sağlama

Bu ilk seçenekte, cihazı sağlamak için IoT Hub tarafından oluşturulan bir **Cihaz bağlantı dizesi** sağlarsınız.

Bu örnekte, Windows kapsayıcılarıyla el ile yükleme gösterilmektedir:

1. Henüz yapmadıysanız, yeni bir IoT Edge cihazı kaydedin ve **Cihaz bağlantı dizesini**alın. Bu bölümün ilerleyen kısımlarında kullanılacak bağlantı dizesini kopyalayın. Aşağıdaki araçları kullanarak bu adımı tamamlayabilirsiniz:

   * [Azure portal](how-to-register-device.md#register-in-the-azure-portal)
   * [Azure CLI](how-to-register-device.md#register-with-the-azure-cli)
   * [Visual Studio Code](how-to-register-device.md#register-with-visual-studio-code)

2. PowerShell'i yönetici olarak çalıştırın.

   >[!NOTE]
   >PowerShell (x86) değil IoT Edge yüklemek için PowerShell 'in AMD64 oturumunu kullanın. Hangi oturum türünü kullandığınızdan emin değilseniz, aşağıdaki komutu çalıştırın:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. **Deploy-ıotedge** komutu, Windows makinenizin desteklenen bir sürümde olup olmadığını denetler, kapsayıcılar özelliğini açar ve ardından Moby çalışma zamanını ve IoT Edge çalışma zamanını indirir. Komut varsayılan olarak Windows kapsayıcıları ' nı kullanmaktır.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. Bu noktada, IoT çekirdek cihazları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server cihazları yeniden başlatmanızı isteyebilir. Bu durumda cihazınızı şimdi yeniden başlatın. Cihazınız çalışmaya başladıktan sonra PowerShell 'i yönetici olarak yeniden çalıştırın.

5. **Initialize-ıotedge** komutu, makinenizde IoT Edge çalışma zamanını yapılandırır. Komut, Windows kapsayıcıları ile el ile sağlamayı varsayılan olarak belirler.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge
   ```

6. İstendiğinde, 1. adımda aldığınız cihaz bağlantı dizesini belirtin. Cihaz bağlantı dizesi, fiziksel cihazı IoT Hub bir cihaz KIMLIĞIYLE ilişkilendirir.

   Cihaz bağlantı dizesi aşağıdaki biçimi alır ve tırnak işaretleri içermemelidir:`HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

7. Cihazınızdaki IoT Edge durumunu denetlemek için [başarılı yüklemeyi doğrulama](#verify-successful-installation) bölümündeki adımları kullanın.

Bir cihazı el ile yüklerken ve sağladığınızda yüklemeyi değiştirmek için ek parametreleri kullanabilirsiniz:

* Bir proxy sunucusundan gelen trafiği doğrudan gönder
* Yükleyiciyi çevrimdışı bir dizine işaret edin
* Belirli bir aracı kapsayıcı görüntüsü bildirin ve özel bir kayıt defterinde yer alıyorsa kimlik bilgilerini sağlayın

Bu yükleme seçenekleri hakkında daha fazla bilgi için, [tüm yükleme parametreleri](#all-installation-parameters)hakkında bilgi edinmek üzere ileri atlayın.

### <a name="option-2-install-and-automatically-provision"></a>2. seçenek: Install ve otomatik olarak sağlama

Bu ikinci seçenekte, cihazı IoT Hub cihaz sağlama hizmetini kullanarak sağlayacaksınız. Bir cihaz sağlama hizmeti örneğindeki **kapsam kimliğini** tercih ettiğiniz [kanıtlama mekanizmanıza](../iot-dps/concepts-security.md#attestation-mechanism)özgü diğer bilgilerle birlikte sağlayın:

* [Windows üzerinde sanal TPM ile sanal bir IoT Edge cihaz oluşturma ve sağlama](how-to-auto-provision-simulated-device-windows.md)
* [X. 509.440 sertifikalarını kullanarak sanal IoT Edge cihaz oluşturma ve sağlama](how-to-auto-provision-x509-certs.md)
* [Simetrik anahtar kanıtlama kullanarak bir IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-symmetric-keys.md)

Bir cihazı otomatik olarak yükleyip sağladığınızda, yüklemeyi değiştirmek için ek parametreleri kullanabilirsiniz:

* Bir proxy sunucusundan gelen trafiği doğrudan gönder
* Yükleyiciyi çevrimdışı bir dizine işaret edin
* Belirli bir aracı kapsayıcı görüntüsü bildirin ve özel bir kayıt defterinde yer alıyorsa kimlik bilgilerini sağlayın

Bu yükleme seçenekleri hakkında daha fazla bilgi için bu makaleyi okumaya devam edin veya [tüm yükleme parametreleri](#all-installation-parameters)hakkında bilgi edinmek için atlayın.

## <a name="offline-or-specific-version-installation"></a>Çevrimdışı veya belirli sürümü yükleme

Yükleme sırasında üç dosya indirilir:

* Yükleme yönergelerini içeren bir PowerShell betiği
* IoT Edge güvenlik cini (ıotedşlı), Moby kapsayıcı altyapısını ve Moby CLı 'yi içeren IoT Edge cab Microsoft Azure
* Visual C++ yeniden dağıtılabilir paket (VC çalışma zamanı) yükleyicisi

Cihazınız yükleme sırasında çevrimdışı kalırsa veya IoT Edge belirli bir sürümünü yüklemek istiyorsanız, bu dosyaları cihaza bir süre önce indirebilirsiniz. Yükleme zamanı olduğunda, yükleme betiğini indirilen dosyaları içeren dizine işaret edin. Yükleyici önce bu dizini denetler ve sonra yalnızca bulunamayan bileşenleri indirir. Tüm dosyalar çevrimdışı kullanılabilir ise, internet bağlantısı olmadan yükleyebilirsiniz.

IoT Edge güncelleştirmek için çevrimdışı yükleme yolu parametresini de kullanabilirsiniz. Daha fazla bilgi için bkz. [IoT Edge güvenlik cini ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).

1. Önceki sürümlerle birlikte en son IoT Edge yükleme dosyaları için bkz. [Azure IoT Edge yayınlar](https://github.com/Azure/azure-iotedge/releases).

2. Yüklemek istediğiniz sürümü bulun ve sürüm notlarının **varlıklar** bölümünden aşağıdaki dosyaları IoT cihazınıza indirin:

   * IoTEdgeSecurityDaemon. ps1
   * 1.0.9 veya daha yeni sürümlerden Microsoft-Azure-IoTEdge-amd64. cab veya yayınlar 1.0.8 ve daha eski sürümlerde Microsoft-Azure-IoTEdge. cab.

   Microsoft-Azure-IotEdge-arm32. cab yalnızca test amacıyla 1.0.9 'den başlayarak da kullanılabilir. IoT Edge Şu anda Windows ARM32 cihazlarında desteklenmemektedir.

   Her sürümdeki özellikleri desteklemek üzere değiştiğinden, kullandığınız. cab dosyası ile aynı sürümden PowerShell betiğinin kullanılması önemlidir.

3. İndirdiğiniz. cab dosyasının üzerinde bir mimari soneki varsa, dosyayı yalnızca **Microsoft-Azure-IoTEdge. cab**olarak yeniden adlandırın.

4. İsteğe bağlı olarak, Visual C++ yeniden dağıtılabilir için bir yükleyici indirebilirsiniz. Örneğin, PowerShell betiği şu sürümü kullanır: [vc_redist. x64. exe](https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe). Yükleyiciyi, IoT Edge dosyaları olarak IoT cihazınızda aynı klasöre kaydedin.

5. Çevrimdışı bileşenlerle yüklemek için, [nokta kaynağı](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7#script-scope-and-dot-sourcing) PowerShell betiğinin yerel kopyasıdır. Sonra, `-OfflineInstallationPath` parametresini komutunun bir parçası olarak kullanın `Deploy-IoTEdge` ve dosya dizinine mutlak yolu sağlayın. Örneğin,

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Deploy-IoTEdge -OfflineInstallationPath <path>
   ```

   Dağıtım komutu, belirtilen yerel dosya dizininde bulunan tüm bileşenleri kullanır. . Cab dosyası veya Visual C++ yükleyicisi eksikse, bunları indirmeyi dener.

6. `Initialize-IoTEdge`Cihazınızı IoT Hub bir kimlikle sağlamak için komutunu çalıştırın. El ile sağlama için bir cihaz bağlantı dizesi sağlayın ya da önceki [Otomatik sağlama](#option-2-install-and-automatically-provision) bölümünde açıklanan yöntemlerden birini seçin.

   Çalışmaya başladıktan sonra cihazınız yeniden başlatılırsa `Deploy-IoTEdge` , çalıştırmadan önce PowerShell betiğine göre kaynak noktası `Initialize-IoTEdge` .

Çevrimdışı yükleme seçeneği hakkında daha fazla bilgi için, [tüm yükleme parametreleri](#all-installation-parameters)hakkında bilgi edinmek üzere ileri atlayın.

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulama

IoT Edge hizmetinin durumunu kontrol edin. Çalışıyor olarak listelenmelidir.  

```powershell
Get-Service iotedge
```

Son 5 dakikadan hizmet günlüklerini inceleyin. IoT Edge çalışma zamanını yüklemeyi tamamladıysanız, çalıştırılan **dağıtım-ıotedge** ve **Initialize-ıotedge**arasındaki zamandan oluşan hataların bir listesini görebilirsiniz. Hizmetin yapılandırmadan önce başlatılmaya çalıştığı için bu hatalar beklenmektedir.

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

En yaygın yapılandırma ve ağ hatalarını denetlemek için [sorun giderme aracını](troubleshoot.md#run-the-check-command) çalıştırın.

```powershell
iotedge check
```

Cihazınızda IoT Edge için ilk modülünüzü dağıtana kadar, **$edgeHub** sistem modülü cihaza dağıtılmayacak. Sonuç olarak, otomatik denetim bağlantı denetimi için bir hata döndürür `Edge Hub can bind to ports on host` . Bu hata, cihaza bir modül dağıttıktan sonra gerçekleşmediği takdirde yoksayılabilir.

Son olarak, çalışan modülleri listeleyin:

```powershell
iotedge list
```

Yeni bir yüklemeden sonra, çalıştırmayı görmeniz gereken tek modül **Edgeagent**' dir. [IoT Edge modüllerini](how-to-deploy-modules-portal.md) ilk kez dağıttıktan sonra, diğer sistem modülü, **edgehub**, cihazda da başlatılır.

## <a name="manage-module-containers"></a>Modül kapsayıcılarını yönetme

IoT Edge hizmeti cihazınızda çalışan bir kapsayıcı altyapısı gerektirir. Bir cihaza modül dağıttığınızda, IoT Edge çalışma zamanı, kapsayıcı altyapısını buluttaki bir kayıt defterinden çekmek için kapsayıcı altyapısını kullanır. IoT Edge hizmeti modüllerinizle etkileşime geçerek günlükleri almanızı sağlar, ancak bazen kapsayıcı altyapısını kapsayıcının kendisiyle etkileşim kurmak için kullanmak isteyebilirsiniz.

Modül kavramları hakkında daha fazla bilgi için bkz. [Azure IoT Edge modüllerini anlama](iot-edge-modules.md).

Windows IoT Edge cihazınızda Windows kapsayıcıları çalıştırıyorsanız, IoT Edge yüklemesi Moby kapsayıcı altyapısını içerir. Moby motoru Docker ile aynı standartlara dayalıdır ve Docker Desktop ile aynı makinede paralel olarak çalışacak şekilde tasarlanmıştır. Bu nedenle, Moby altyapısı tarafından yönetilen kapsayıcıları hedeflemek istiyorsanız Docker yerine bu altyapıyı özellikle hedeflemek gerekir.

Örneğin, tüm Docker görüntülerini listelemek için aşağıdaki komutu kullanın:

```powershell
docker images
```

Tüm Moby görüntülerini listelemek için, aynı komutu Moby altyapısına yönelik bir işaretçi ile değiştirin:

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

Altyapı URI 'SI, yükleme komut dosyasının çıktısında listelenir veya bunu config. YAML dosyasının kapsayıcı çalışma zamanı ayarları bölümünde bulabilirsiniz.

![config. YAML içinde moby_runtime URI 'si](./media/how-to-install-iot-edge-windows/moby-runtime-uri.png)

Cihazınızda çalışan kapsayıcılarla ve görüntülerle etkileşim kurmak için kullanabileceğiniz komutlar hakkında daha fazla bilgi için bkz. [Docker komut satırı arabirimleri](https://docs.docker.com/engine/reference/commandline/docker/).

## <a name="uninstall-iot-edge"></a>IoT Edge kaldır

IoT Edge yüklemesini Windows cihazınızdan kaldırmak istiyorsanız, bir yönetim PowerShell penceresinden aşağıdaki komutu kullanın. Bu komut, IoT Edge çalışma zamanını, mevcut yapılandırmanızla ve Moby motoru verileriyle birlikte kaldırır.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-IoTEdge
```

Uninstall-ıotedge komutu Windows IoT Core üzerinde çalışmıyor. Windows IoT çekirdek cihazlarından IoT Edge kaldırmak için Windows IoT çekirdek görüntünüzü yeniden dağıtmanız gerekir.

Kaldırma seçenekleri hakkında daha fazla bilgi için komutunu kullanın `Get-Help Uninstall-IoTEdge -full` .

## <a name="verify-installation-script"></a>Yükleme betiğini doğrula

Bu makalede belirtilen yükleme komutları, yükleme betiğini ' den istemek için Invoke-WebRequest cmdlet 'ini kullanır `aka.ms/iotedge-win` . Bu bağlantı, `IoTEdgeSecurityDaemon.ps1` en son [IoT Edge sürümden](https://github.com/Azure/azure-iotedge/releases)betiğe işaret eder. IoT Edge cihazınızda yükleme komutlarını çalıştırmak için bu betiği veya belirli bir sürümdeki betiğin bir sürümünü de indirebilirsiniz.

Güvenliği artırmak için belirtilen komut dosyası imzalandı. Betiği cihazınıza indirerek ve sonra aşağıdaki PowerShell komutunu çalıştırarak imzayı doğrulayabilirsiniz:

```powershell
Get-AuthenticodeSignature "C:\<path>\IotEdgeSecurityDaemon.ps1"
```

İmza doğrulanırsa çıkış durumu **geçerli** olur.

## <a name="all-installation-parameters"></a>Tüm yükleme parametreleri

Önceki bölümlerde, yükleme betiğini değiştirmek için parametrelerin nasıl kullanılacağına ilişkin örneklerle birlikte yaygın yükleme senaryoları tanıtılmıştır. Bu bölümde IoT Edge yüklemek, güncelleştirmek veya kaldırmak için kullanılan ortak parametrelerin başvuru tabloları sağlanmaktadır.

### <a name="deploy-iotedge"></a>Dağıtım-ıotedge

Deploy-ıotedge komutu, IoT Edge güvenlik cini ve bağımlılıklarını indirir ve dağıtır. Dağıtım komutu, diğerleri arasında bu ortak parametreleri kabul eder. Tam liste için komutunu kullanın `Get-Help Deploy-IoTEdge -full` .  

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** veya **Linux** | Bir kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir.<br><br>Windows kapsayıcıları için IoT Edge, yüklemeye dahil edilen Moby kapsayıcı altyapısını kullanır. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı altyapısı yüklemeniz gerekir. |
| **Proxy** | Proxy URL 'SI | Cihazınızın internet 'e erişmek için bir proxy sunucusu üzerinden gitmesi gerekiyorsa bu parametreyi ekleyin. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Offlineınstallationpath** | Dizin yolu | Bu parametre dahil ise, yükleyici, yükleme için gereken IoT Edge cab ve VC çalışma zamanı MSI dosyaları için listelenen dizini kontrol eder. Dizinde bulunmayan tüm dosyalar indirilir. Her iki dosya da dizinde ise, internet bağlantısı olmadan IoT Edge yükleyebilirsiniz. Belirli bir sürümü kullanmak için bu parametreyi de kullanabilirsiniz. |
| **Invokewebrequestparameters** | Parametrelerin ve değerlerin Hashtable 'ı | Yükleme sırasında bazı Web istekleri yapılır. Bu Web isteklerinin parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Restartifgerekliyse** | yok | Bu bayrak dağıtım betiğinin, gerekirse, makineyi sormadan yeniden başlatmasını sağlar. |

### <a name="initialize-iotedge"></a>Initialize-ıotedge

Initialize-ıotedge komutu, IoT Edge cihaz bağlantı dizeniz ve işletimsel ayrıntılarla yapılandırır. Bu komutla oluşturulan bilgilerin çoğu daha sonra ıotedge\config.exe dosyasında depolanır. Başlatma komutu, diğerleri arasında bu ortak parametreleri kabul eder. Tam liste için komutunu kullanın `Get-Help Initialize-IoTEdge -full` .

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **El ile** | Yok | **Anahtar parametresi**. Hiçbir sağlama türü belirtilmemişse, el ile varsayılan değerdir.<br><br>Cihazı el ile sağlamak için bir cihaz bağlantı dizesi sağlayacağınızı bildirir |
| **DPS** | Yok | **Anahtar parametresi**. Hiçbir sağlama türü belirtilmemişse, el ile varsayılan değerdir.<br><br>Bir cihaz sağlama hizmeti (DPS) kapsam KIMLIĞI ve bu cihazın, DPS aracılığıyla sağlamak üzere cihazınızın kayıt KIMLIĞI sağlayacağınızı bildirir.  |
| **DeviceConnectionString** | Tek tırnak içinde bir IoT Hub kayıtlı IoT Edge cihazdan bağlantı dizesi | El ile sağlama için **gereklidir** . Betik parametrelerinde bir bağlantı dizesi sağlamazsanız sizden bir tane istenir. |
| **KML** | IoT Hub ilişkili cihaz sağlama hizmeti örneğinden bir kapsam KIMLIĞI. | DPS sağlaması için **gereklidir** . Komut dosyası parametrelerinde bir kapsam kimliği sağlamazsanız sizden bir kapsam kimliği belirtmeniz gerekir. |
| **RegistrationId** | Cihazınız tarafından oluşturulan bir kayıt KIMLIĞI | TPM veya simetrik anahtar kanıtlama kullanılıyorsa, DPS sağlaması için **gereklidir** . X. 509.440 sertifika kanıtlama kullanılıyorsa **Isteğe bağlıdır** . |
| **X509IdentityCertificate** | Cihazdaki X. 509.440 cihaz kimliği sertifikasının URI yolu. | X. 509.440 sertifika kanıtlama kullanılıyorsa DPS sağlaması için **gereklidir** . |
| **X509IdentityPrivateKey** | Cihazdaki X. 509.440 cihaz kimliği sertifika anahtarının URI yolu. | X. 509.440 sertifika kanıtlama kullanılıyorsa DPS sağlaması için **gereklidir** . |
| **SymmetricKey** | DPS kullanılırken IoT Edge cihaz kimliğini sağlamak için kullanılan simetrik anahtar | Simetrik anahtar kanıtlama kullanılıyorsa, DPS sağlaması için **gereklidir** . |
| **ContainerOs** | **Windows** veya **Linux** | Bir kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir.<br><br>Windows kapsayıcıları için IoT Edge, yüklemeye dahil edilen Moby kapsayıcı altyapısını kullanır. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı altyapısı yüklemeniz gerekir. |
| **Invokewebrequestparameters** | Parametrelerin ve değerlerin Hashtable 'ı | Yükleme sırasında bazı Web istekleri yapılır. Bu Web isteklerinin parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Dil** | IoT Edge aracı görüntüsü URI 'SI | Varsayılan olarak, yeni bir IoT Edge yüklemesi IoT Edge Aracısı görüntüsü için en son kayan etiketi kullanır. Görüntü sürümü için belirli bir etiket ayarlamak veya kendi aracı görüntünüzü sağlamak için bu parametreyi kullanın. Daha fazla bilgi için bkz. [IoT Edge etiketlerini anlama](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Nitelen** | Kapsayıcı kayıt defteri Kullanıcı adı | Bu parametreyi yalnızca,-Tımage parametresini özel bir kayıt defterindeki bir kapsayıcıya ayarlarsanız kullanın. Kayıt defterine erişimi olan bir Kullanıcı adı belirtin. |
| **Parola** | Güvenli parola dizesi | Bu parametreyi yalnızca,-Tımage parametresini özel bir kayıt defterindeki bir kapsayıcıya ayarlarsanız kullanın. Kayıt defterine erişmek için parolayı girin. |

### <a name="update-iotedge"></a>Güncelleştirme-ıotedge

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** veya **Linux** | Bir kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir. Windows kapsayıcıları için, yüklemeye bir kapsayıcı altyapısı dahil edilir. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı altyapısı yüklemeniz gerekir. |
| **Proxy** | Proxy URL 'SI | Cihazınızın internet 'e erişmek için bir proxy sunucusu üzerinden gitmesi gerekiyorsa bu parametreyi ekleyin. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Invokewebrequestparameters** | Parametrelerin ve değerlerin Hashtable 'ı | Yükleme sırasında bazı Web istekleri yapılır. Bu Web isteklerinin parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Offlineınstallationpath** | Dizin yolu | Bu parametre dahil ise, yükleyici, yükleme için gereken IoT Edge cab ve VC çalışma zamanı MSI dosyaları için listelenen dizini kontrol eder. Dizinde bulunmayan tüm dosyalar indirilir. Her iki dosya da dizinde ise, internet bağlantısı olmadan IoT Edge yükleyebilirsiniz. Belirli bir sürümü kullanmak için bu parametreyi de kullanabilirsiniz. |
| **Restartifgerekliyse** | yok | Bu bayrak dağıtım betiğinin, gerekirse, makineyi sormadan yeniden başlatmasını sağlar. |

### <a name="uninstall-iotedge"></a>Uninstall-ıotedge

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **Zorla** | yok | Bu bayrak, önceki kaldırma girişimi başarısız olursa kaldırma işlemini zorlar.
| **Restartifgerekliyse** | yok | Bu bayrak, gerekirse, kaldırma komut dosyasının makineyi sormadan yeniden başlatmasını sağlar. |

## <a name="next-steps"></a>Sonraki adımlar

Çalışma zamanının yüklü olduğu bir IoT Edge cihazınıza sahip olduğunuza göre, [IoT Edge modülleri dağıtabilirsiniz](how-to-deploy-modules-portal.md).

Yükleme IoT Edge düzgün şekilde yüklerken [sorun yaşıyorsanız sorun giderme](troubleshoot.md) sayfasına göz atın.

Mevcut bir yüklemeyi en yeni IoT Edge sürümüne güncelleştirmek için bkz. [IoT Edge güvenlik cini ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).
