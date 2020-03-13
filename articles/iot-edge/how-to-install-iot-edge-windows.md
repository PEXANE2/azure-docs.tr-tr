---
title: Azure IoT Edge Windows 'a yükler | Microsoft Docs
description: Windows 10, Windows Server ve Windows IoT Core 'daki yükleme yönergelerini Azure IoT Edge
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: kgremban
ms.openlocfilehash: e3f55f9be28a8b53f012e111e43ba1f495b1d585
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79285064"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Azure IoT Edge çalışma zamanını Windows 'a yükler

Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz.

IoT Edge çalışma zamanı hakkında daha fazla bilgi için bkz. [Azure IoT Edge çalışma zamanını ve mimarisini anlayın](iot-edge-runtime.md).

Bu makalede, Windows kapsayıcıları kullanarak Windows x64 (AMD/Intel) sisteminizde Azure IoT Edge çalışma zamanını yüklemek için gereken adımlar listelenmektedir.

> [!NOTE]
> Bilinen bir Windows işletim sistemi sorunu, IoT Edge modüller (işlem yalıtılmış Windows nano sunucu kapsayıcıları) çalışırken uyku ve hazırda bekleme güç durumlarına geçiş yapılmasını önler. Bu sorun, cihazdaki pil ömrünü etkiler.
>
> Geçici bir çözüm olarak, bu güç durumlarını kullanmadan önce çalışan IoT Edge modüllerini durdurmak için `Stop-Service iotedge` komutunu kullanın.

Windows sistemlerinde Linux kapsayıcıları kullanmak, Azure IoT Edge için önerilen veya desteklenen bir üretim yapılandırması değildir. Ancak, geliştirme ve test amacıyla kullanılabilir. Daha fazla bilgi için bkz. [Linux kapsayıcılarını çalıştırmak Için Windows 'ta IoT Edge kullanma](how-to-install-iot-edge-windows-with-linux.md).

En son IoT Edge sürümüne nelerin dahil olduğu hakkında bilgi için, bkz. [Azure IoT Edge sürümler](https://github.com/Azure/azure-iotedge/releases).

## <a name="prerequisites"></a>Önkoşullar

Windows cihazınızın IoT Edge destekleyip desteklemediğini gözden geçirmek ve yüklemeden önce bir kapsayıcı altyapısı için hazırlamak üzere bu bölümü kullanın.

### <a name="supported-windows-versions"></a>Desteklenen Windows sürümleri

Geliştirme ve test senaryoları için Windows kapsayıcılarına sahip Azure IoT Edge, Windows 10 ' un veya Windows Server 2019 (derleme 17763) ' nin kapsayıcılar özelliğini destekleyen herhangi bir sürümüne yüklenebilir. Şu anda üretim senaryolarında hangi işletim sistemlerinin desteklendiği hakkında bilgi için bkz. [Azure IoT Edge desteklenen sistemler](support.md#operating-systems).

IoT çekirdek cihazları, IoT Edge çalışma zamanını desteklemek için IoT Core-Windows kapsayıcıları isteğe bağlı özelliğini içermelidir. Windows kapsayıcıları 'nın cihazınızda desteklenip desteklenmediğini denetlemek için [uzak bir PowerShell oturumunda](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell) aşağıdaki komutu kullanın:

```powershell
Get-Service vmcompute
```

Hizmet mevcutsa, **çalışıyor**olarak listelenen hizmet durumu ile başarılı bir yanıt almanız gerekir. Vmcompute hizmeti bulunamazsa, cihazınız IoT Edge gereksinimlerini karşılamaz. Bu özellik için destek almak üzere donanım sağlayıcınızla iletişim kurun.

### <a name="prepare-for-a-container-engine"></a>Kapsayıcı altyapısı için hazırlanma

Azure IoT Edge, [OCI uyumlu](https://www.opencontainers.org/) bir kapsayıcı altyapısına bağlıdır. Üretim senaryoları için, Windows cihazınızda Windows kapsayıcıları çalıştırmak üzere yükleme betiğine eklenen Moby altyapısını kullanın.

## <a name="install-iot-edge-on-a-new-device"></a>Yeni bir cihaza IoT Edge yüklemesi

>[!NOTE]
>Azure IOT Edge yazılım paketlerini (lisans dizininde) paketleri bulunan lisans koşullarına tabidir. Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

Bir PowerShell betiği, Azure IoT Edge güvenlik cini indirir ve yükler. Güvenlik cini daha sonra, diğer modüllerin uzak dağıtımlarını sağlayan IoT Edge Aracısı olan iki çalışma zamanı modülünün ilki başlatılır.

>[!TIP]
>IoT çekirdek cihazları için, bir RemotePowerShell oturumu kullanarak yükleme komutlarının çalıştırılmasını öneririz. Daha fazla bilgi için bkz. [Windows IoT Için PowerShell kullanma](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell).

IoT Edge çalışma zamanını bir cihaza ilk kez yüklediğinizde, cihazı IoT Hub 'ından bir kimlikle sağlamanız gerekir. Tek bir IoT Edge cihaz, IoT Hub tarafından sağlanan bir cihaz bağlantı dizesi kullanılarak el ile sağlanabilir. Ya da cihaz sağlama hizmeti 'ni (DPS), cihazları otomatik olarak sağlamak için kullanabilirsiniz. Bu, ayarlanacak birçok cihazınız olduğunda yararlı olur. Sağlama seçiminize bağlı olarak, uygun yükleme komut dosyasını seçin.

Aşağıdaki bölümlerde, yeni bir cihazdaki IoT Edge yükleme betiği için ortak kullanım durumları ve parametreleri açıklanır.

### <a name="option-1-install-and-manually-provision"></a>1\. seçenek: Yükleme ve el ile sağlama

Bu ilk seçenekte, cihazı sağlamak için IoT Hub tarafından oluşturulan bir **Cihaz bağlantı dizesi** sağlarsınız.

Bu örnekte, Windows kapsayıcılarıyla el ile yükleme gösterilmektedir:

1. Henüz yapmadıysanız, yeni bir IoT Edge cihazı kaydedin ve **Cihaz bağlantı dizesini**alın. Bu bölümün ilerleyen kısımlarında kullanılacak bağlantı dizesini kopyalayın. Aşağıdaki araçları kullanarak bu adımı tamamlayabilirsiniz:

   * [Azure portalında](how-to-register-device.md#register-in-the-azure-portal)
   * [Azure CLI](how-to-register-device.md#register-with-the-azure-cli)
   * [Visual Studio Code](how-to-register-device.md#register-with-visual-studio-code)

2. PowerShell 'i yönetici olarak çalıştırın.

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

   Cihaz bağlantı dizesi aşağıdaki biçimi alır ve tırnak işaretleri içermemelidir: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

7. Cihazınızdaki IoT Edge durumunu denetlemek için [başarılı yüklemeyi doğrulama](#verify-successful-installation) bölümündeki adımları kullanın.

Bir cihazı el ile yüklerken ve sağladığınızda yüklemeyi değiştirmek için ek parametreleri kullanabilirsiniz:

* Bir proxy sunucusundan gelen trafiği doğrudan gönder
* Yükleyiciyi çevrimdışı bir dizine işaret edin
* Belirli bir aracı kapsayıcı görüntüsü bildirin ve özel bir kayıt defterinde yer alıyorsa kimlik bilgilerini sağlayın

Bu yükleme seçenekleri hakkında daha fazla bilgi için, [tüm yükleme parametreleri](#all-installation-parameters)hakkında bilgi edinmek üzere ileri atlayın.

### <a name="option-2-install-and-automatically-provision"></a>2\. seçenek: Yükleme ve otomatik olarak sağlama

Bu ikinci seçenekte, cihazı IoT Hub cihaz sağlama hizmetini kullanarak sağlayacaksınız. Bir cihaz sağlama hizmeti örneğindeki **kapsam kimliğini** tercih ettiğiniz [kanıtlama mekanizmanıza](../iot-dps/concepts-security.md#attestation-mechanism)özgü diğer bilgilerle birlikte sağlayın:

* [Windows üzerinde sanal TPM ile sanal bir IoT Edge cihaz oluşturma ve sağlama](how-to-auto-provision-simulated-device-windows.md)
* [Simetrik anahtar kanıtlama kullanarak bir IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-symmetric-keys.md)

Bir cihazı otomatik olarak yükleyip sağladığınızda, yüklemeyi değiştirmek için ek parametreleri kullanabilirsiniz:

* Bir proxy sunucusundan gelen trafiği doğrudan gönder
* Yükleyiciyi çevrimdışı bir dizine işaret edin
* Belirli bir aracı kapsayıcı görüntüsü bildirin ve özel bir kayıt defterinde yer alıyorsa kimlik bilgilerini sağlayın

Bu yükleme seçenekleri hakkında daha fazla bilgi için bu makaleyi okumaya devam edin veya [tüm yükleme parametreleri](#all-installation-parameters)hakkında bilgi edinmek için atlayın.

## <a name="offline-or-specific-version-installation"></a>Çevrimdışı veya belirli sürümü yükleme

Yükleme sırasında iki dosya indirilir:

* IoT Edge güvenlik cini (ıotedşlı), Moby kapsayıcı altyapısını ve Moby CLı 'yi içeren IoT Edge cab Microsoft Azure.
* Visual C++ yeniden dağıtılabilir paket (VC çalışma zamanı) MSI

Cihazınız yükleme sırasında çevrimdışı kalırsa veya IoT Edge belirli bir sürümünü yüklemek istiyorsanız, bu dosyaların bir veya her ikisini de cihaza daha güncel bir şekilde indirebilirsiniz. Yükleme zamanı olduğunda, yükleme betiğini indirilen dosyaları içeren dizine işaret edin. Yükleyici önce bu dizini denetler ve sonra yalnızca bulunamayan bileşenleri indirir. Tüm dosyalar çevrimdışı kullanılabilir ise, internet bağlantısı olmadan yükleyebilirsiniz.

Önceki sürümlerle birlikte en son IoT Edge yükleme dosyaları için bkz. [Azure IoT Edge yayınlar](https://github.com/Azure/azure-iotedge/releases).

Çevrimdışı bileşenlerle yüklemek için, Deploy-ıotedge komutunun bir parçası olarak `-OfflineInstallationPath` parametresini kullanın ve dosya dizinine mutlak yolu sağlayın. Örneğin,

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

>[!NOTE]
>`-OfflineInstallationPath` parametresi, belirtilen dizinde **Microsoft-Azure-IoTEdge. cab** adlı bir dosya arar. IoT Edge Version 1.0.9-RC4 ile başlayarak, biri AMD64 cihaz ve diğeri ARM32 için kullanılabilecek iki. cab dosyası vardır. Cihazınız için doğru dosyayı indirin ve ardından mimari sonekini kaldırmak için dosyayı yeniden adlandırın.

`Deploy-IoTEdge` komutu IoT Edge bileşenlerini yükledikten sonra cihazı IoT Hub cihaz KIMLIĞI ve bağlantısıyla sağlamak için `Initialize-IoTEdge` komutuna devam etmeniz gerekir. Komutu doğrudan çalıştırın ve IoT Hub bir bağlantı dizesi sağlayın ya da cihaz sağlama hizmeti ile cihazları otomatik olarak sağlamayı öğrenmek için önceki bölümdeki bağlantılardan birini kullanın.

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
Initialize-IoTEdge
```

Update-ıotedge komutuyla çevrimdışı yükleme yolu parametresini de kullanabilirsiniz.

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

IoT Edge hizmetinin durumunu kontrol edin. Çalışıyor olarak listelenmelidir.  

```powershell
Get-Service iotedge
```

Son 5 dakika Hizmeti günlüklerini inceleyin. IoT Edge çalışma zamanını yüklemeyi tamamladıysanız, çalıştırılan **dağıtım-ıotedge** ve **Initialize-ıotedge**arasındaki zamandan oluşan hataların bir listesini görebilirsiniz. Hizmetin yapılandırmadan önce başlatılmaya çalıştığı için bu hatalar beklenmektedir.

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

En yaygın yapılandırma ve ağ hataları için otomatik bir denetim çalıştırın.

```powershell
iotedge check
```

Çalışan modülleri listeleyin. Yeni bir yüklemeden sonra, çalıştırmayı görmeniz gereken tek modül **Edgeagent**' dir. [IoT Edge modüllerini](how-to-deploy-modules-portal.md) ilk kez dağıttıktan sonra, diğer sistem modülü, **edgehub**, cihazda da başlatılır.

```powershell
iotedge list
```

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

Kaldırma seçenekleri hakkında daha fazla bilgi için `Get-Help Uninstall-IoTEdge -full`komutunu kullanın.

## <a name="all-installation-parameters"></a>Tüm yükleme parametreleri

Önceki bölümlerde, yükleme betiğini değiştirmek için parametrelerin nasıl kullanılacağına ilişkin örneklerle birlikte yaygın yükleme senaryoları tanıtılmıştır. Bu bölümde IoT Edge yüklemek, güncelleştirmek veya kaldırmak için kullanılan ortak parametrelerin başvuru tabloları sağlanmaktadır.

### <a name="deploy-iotedge"></a>Dağıtım-ıotedge

Deploy-ıotedge komutu, IoT Edge güvenlik cini ve bağımlılıklarını indirir ve dağıtır. Dağıtım komutu, diğerleri arasında bu ortak parametreleri kabul eder. Tam liste için `Get-Help Deploy-IoTEdge -full`komutunu kullanın.  

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** veya **Linux** | Bir kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir.<br><br>Windows kapsayıcıları için IoT Edge, yüklemeye dahil edilen Moby kapsayıcı altyapısını kullanır. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı altyapısı yüklemeniz gerekir. |
| **Proxy** | Proxy URL 'SI | Cihazınızın internet 'e erişmek için bir proxy sunucusu üzerinden gitmesi gerekiyorsa bu parametreyi ekleyin. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Offlineınstallationpath** | Dizin yolu | Bu parametre dahil ise, yükleyici, yükleme için gereken IoT Edge cab ve VC çalışma zamanı MSI dosyaları için listelenen dizini kontrol eder. Dizinde bulunmayan tüm dosyalar indirilir. Her iki dosya da dizinde ise, internet bağlantısı olmadan IoT Edge yükleyebilirsiniz. Belirli bir sürümü kullanmak için bu parametreyi de kullanabilirsiniz. |
| **Invokewebrequestparameters** | Parametrelerin ve değerlerin Hashtable 'ı | Yükleme sırasında bazı Web istekleri yapılır. Bu Web isteklerinin parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Restartifgerekliyse** | yok | Bu bayrak dağıtım betiğinin, gerekirse, makineyi sormadan yeniden başlatmasını sağlar. |

### <a name="initialize-iotedge"></a>Initialize-ıotedge

Initialize-ıotedge komutu, IoT Edge cihaz bağlantı dizeniz ve işletimsel ayrıntılarla yapılandırır. Bu komutla oluşturulan bilgilerin çoğu daha sonra ıotedge\config.exe dosyasında depolanır. Başlatma komutu, diğerleri arasında bu ortak parametreleri kabul eder. Tam liste için `Get-Help Initialize-IoTEdge -full`komutunu kullanın.

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **El ile** | Yok | **Anahtar parametresi**. Hiçbir sağlama türü belirtilmemişse, el ile varsayılan değerdir.<br><br>Cihazı el ile sağlamak için bir cihaz bağlantı dizesi sağlayacağınızı bildirir |
| **DPS** | Yok | **Anahtar parametresi**. Hiçbir sağlama türü belirtilmemişse, el ile varsayılan değerdir.<br><br>Bir cihaz sağlama hizmeti (DPS) kapsam KIMLIĞI ve bu cihazın, DPS aracılığıyla sağlamak üzere cihazınızın kayıt KIMLIĞI sağlayacağınızı bildirir.  |
| **DeviceConnectionString** | Tek tırnak içinde bir IoT Hub kayıtlı IoT Edge cihazdan bağlantı dizesi | El ile yükleme için **gereklidir** . Betik parametrelerinde bir bağlantı dizesi sağlamazsanız, yükleme sırasında sizden bir tane istenir. |
| **KML** | IoT Hub ilişkili cihaz sağlama hizmeti örneğinden bir kapsam KIMLIĞI. | DPS yüklemesi için **gereklidir** . Betik parametrelerinde kapsam KIMLIĞI sağlamazsanız, yükleme sırasında sizden bir tane istenir. |
| **RegistrationId** | Cihazınız tarafından oluşturulan bir kayıt KIMLIĞI | TPM veya simetrik anahtar kanıtlama kullanılıyorsa, DPS yüklemesi için **gereklidir** . |
| **SymmetricKey** | DPS kullanılırken IoT Edge cihaz kimliğini sağlamak için kullanılan simetrik anahtar | Simetrik anahtar kanıtlama kullanılıyorsa, DPS yüklemesi için **gereklidir** . |
| **ContainerOs** | **Windows** veya **Linux** | Bir kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir.<br><br>Windows kapsayıcıları için IoT Edge, yüklemeye dahil edilen Moby kapsayıcı altyapısını kullanır. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı altyapısı yüklemeniz gerekir. |
| **Invokewebrequestparameters** | Parametrelerin ve değerlerin Hashtable 'ı | Yükleme sırasında bazı Web istekleri yapılır. Bu Web isteklerinin parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Dil** | IoT Edge aracı görüntüsü URI 'SI | Varsayılan olarak, yeni bir IoT Edge yüklemesi IoT Edge Aracısı görüntüsü için en son kayan etiketi kullanır. Görüntü sürümü için belirli bir etiket ayarlamak veya kendi aracı görüntünüzü sağlamak için bu parametreyi kullanın. Daha fazla bilgi için bkz. [IoT Edge etiketlerini anlama](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Kullanıcı Adı** | Kapsayıcı kayıt defteri Kullanıcı adı | Bu parametreyi yalnızca,-Tımage parametresini özel bir kayıt defterindeki bir kapsayıcıya ayarlarsanız kullanın. Kayıt defterine erişimi olan bir Kullanıcı adı belirtin. |
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
