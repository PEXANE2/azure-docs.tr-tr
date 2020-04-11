---
title: Azure IoT Edge'i Windows'a yükleme | Microsoft Dokümanlar
description: Windows 10, Windows Server ve Windows IoT Core'da Azure IoT Edge yükleme yönergeleri
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: kgremban
ms.openlocfilehash: 61b382f1c286209a12d0be39a81e6817806d3251
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113454"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Windows'a Azure IoT Edge çalışma zamanını yükleme

Azure IoT Edge çalışma zamanı, aygıtı IoT Edge aygıtına dönüştüren şeydir. Çalışma süresi Raspberry Pi kadar küçük veya endüstriyel sunucu kadar büyük aygıtlarda dağıtılabilir. Bir aygıt IoT Edge çalışma zamanı ile yapılandırıldıktan sonra, buluttan iş mantığı dağıtmaya başlayabilirsiniz.

IoT Edge çalışma zamanı hakkında daha fazla bilgi edinmek için Azure [IoT Edge çalışma süresini ve mimarisini anlayın.](iot-edge-runtime.md)

Bu makalede, Windows kapsayıcılarını kullanarak Windows x64 (AMD/Intel) sisteminizde Azure IoT Edge çalışma süresini yükleme adımları listelenir.

> [!NOTE]
> Bilinen bir Windows işletim sistemi sorunu, IoT Edge modülleri (işlemya yalıtılmış Windows Nano Server kapsayıcıları) çalışırken uyku ve hazırda bekleme güç durumları geçişini engeller. Bu sorun cihazdaki pil ömrünü etkiler.
>
> Geçici çözüm olarak, bu `Stop-Service iotedge` güç durumlarını kullanmadan önce çalışan IoT Edge modüllerini durdurmak için komutu kullanın.

Windows sistemlerinde Linux kapsayıcıları kullanmak Azure IoT Edge için önerilen veya desteklenen bir üretim yapılandırması değildir. Ancak, geliştirme ve test amacıyla kullanılabilir. Daha fazla bilgi için Linux [kapsayıcılarını çalıştırmak için Windows'da IoT Edge kullanın'a](how-to-install-iot-edge-windows-with-linux.md)bakın.

IoT Edge'in en son sürümünde nelerin yer aldığı hakkında bilgi için [Azure IoT Edge sürümlerine](https://github.com/Azure/azure-iotedge/releases)bakın.

## <a name="prerequisites"></a>Ön koşullar

Windows aygıtınızın IoT Edge'i destekleyip desteklemeyeceğini gözden geçirmek ve yüklemeden önce bir kapsayıcı altyapısına hazırlamak için bu bölümü kullanın.

### <a name="supported-windows-versions"></a>Desteklenen Windows sürümleri

Windows için IoT Edge Windows sürümü 1809/build 17763, en son [Windows uzun vadeli destek oluşturmak](https://docs.microsoft.com/windows/release-information/)gerektirir. Windows SKU desteği için, üretim senaryolarına mı yoksa geliştirme ve test senaryolarına mı hazırlandığınıza bağlı olarak nelerin desteklenmeyene bakın:

* **Üretim**: Üretim senaryoları için şu anda hangi işletim sistemlerinin desteklendiğine ilişkin en son bilgiler için [Azure IoT Edge desteklenen sistemlere](support.md#operating-systems)bakın.
* **Geliştirme ve sınama**: Geliştirme ve test senaryoları için, Windows kapsayıcılı Azure IoT Edge, kapsayıcıözelliğini destekleyen Windows 10 veya Windows Server 2019'un herhangi bir sürümüne yüklenebilir.

IoT Core aygıtları, IoT Edge çalışma süresini desteklemek için Isteğe bağlı Özelliği IoT Core Windows Containers içermelidir. Windows kapsayıcılarının cihazınızda destekleniyi kontrol etmek için uzak bir [PowerShell oturumunda](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell) aşağıdaki komutu kullanın:

```powershell
Get-Service vmcompute
```

Hizmet varsa, **çalışan**olarak listelenen hizmet durumu ile başarılı bir yanıt almalısınız. `vmcompute` Hizmet bulunamazsa, aygıtınız IoT Edge gereksinimlerini karşılamaz. Bu özellik için destek hakkında soru sormak için donanım sağlayıcınıza başvurun.

### <a name="prepare-for-a-container-engine"></a>Konteyner motoruna hazırlanın

Azure IoT Edge, [OCI uyumlu](https://www.opencontainers.org/) bir kapsayıcı motoruna dayanır. Üretim senaryoları için, Windows aygıtınızda Windows kapsayıcılarını çalıştırmak için yükleme komut dosyasına dahil olan Moby altyapısını kullanın.

## <a name="install-iot-edge-on-a-new-device"></a>IoT Edge'i yeni bir cihaza yükleme

>[!NOTE]
>Azure IoT Edge yazılım paketleri, paketlerde (LICENSE dizininde) bulunan lisans koşullarına tabidir. Lütfen paketi kullanmadan önce lisans koşullarını okuyun. Paketi yüklemeniz ve kullanmanız bu koşulları kabul etmek anlamına dalır. Lisans koşullarını kabul etmiyorsanız, paketi kullanmayın.

PowerShell komut dosyası Azure IoT Edge güvenlik daemon'u indirir ve yükler. Güvenlik daemon sonra diğer modüllerin uzaktan dağıtımsağlayan iki çalışma zamanı modülleri, IoT Edge aracısı, ilk başlar.

>[!TIP]
>IoT Core aygıtları için, yükleme komutlarını RemotePowerShell oturumunu kullanarak çalıştırmanızı öneririz. Daha fazla bilgi için [Windows IoT için PowerShell kullanma'ya](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell)bakın.

IoT Edge çalışma saatini ilk kez bir aygıta yüklediğinizde, aygıtı bir IoT hub'ından bir kimlik le sağlamanız gerekir. Tek bir IoT Edge aygıtı, IoT Hub tarafından sağlanan bir aygıt bağlantı dizesi kullanılarak el ile kullanılabilir. Veya, cihazları otomatik olarak sağlamak için Aygıt Sağlama Hizmeti'ni (DPS) kullanabilirsiniz, bu da ayarlayabileceğiniz çok sayıda aygıtınız olduğunda yararlıdır. Sağlama tercihinize bağlı olarak, uygun yükleme komut dosyasını seçin.

Aşağıdaki bölümlerde, yeni bir aygıttaki IoT Edge yükleme komut dosyası nın ortak kullanım örnekleri ve parametreleri açıklayınız.

### <a name="option-1-install-and-manually-provision"></a>Seçenek 1: Yükleme ve el ile sağlama

Bu ilk seçenekte, aygıtı sağlamak için IoT Hub tarafından oluşturulan bir **aygıt bağlantı dizesi** sağlarsınız.

Bu örnek, Windows kapsayıcıları ile el ile yükleme gösterir:

1. Henüz yapmadıysanız, yeni bir IoT Edge aygıtı kaydettirin ve **aygıt bağlantı dizesini**alın. Daha sonra bu bölümde kullanmak üzere bağlantı dizesini kopyalayın. Aşağıdaki araçları kullanarak bu adımı tamamlayabilirsiniz:

   * [Azure portal](how-to-register-device.md#register-in-the-azure-portal)
   * [Azure CLI](how-to-register-device.md#register-with-the-azure-cli)
   * [Visual Studio Code](how-to-register-device.md#register-with-visual-studio-code)

2. PowerShell'i yönetici olarak çalıştırın.

   >[!NOTE]
   >IoT Edge'i yüklemek için POWERShell'in AMD64 oturumunu kullanın, PowerShell'i (x86) değil. Hangi oturum türünü kullandığınızdan emin değilseniz aşağıdaki komutu çalıştırın:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. **Deploy-IoTEdge** komutu, Windows makinenizin desteklenen bir sürümde olup olmadığını denetler, kapsayıcılar özelliğini açar ve moby çalışma süresini ve IoT Edge çalışma süresini karşıdan yüklez. Komut, Windows kapsayıcılarını kullanmayı varsayılan olarak kullanır.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. Bu noktada, IoT Core aygıtları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server aygıtları yeniden başlatmanızı isteyebilir. Öyleyse, cihazınızı şimdi yeniden başlatın. Cihazınız hazır olduğunda PowerShell'i yeniden yönetici olarak çalıştırın.

5. **Initialize-IoTEdge komutu,** Makinenizdeki IoT Edge çalışma süresini yapılandırır. Komut, Windows kapsayıcılarıyla el ile sağlama için varsayılandır.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge
   ```

6. İstendiğinde, adım 1'de aldığınız aygıt bağlantı dizesini sağlayın. Aygıt bağlantı dizesi, fiziksel aygıtı IoT Hub'ındaki bir aygıt kimliğiyle ilişkilendirer.

   Aygıt bağlantı dizesi aşağıdaki biçimi alır ve tırnak işaretleri içermemelidir:`HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

7. Aygıtınızdaki IoT Edge'in durumunu kontrol etmek için [Başarılı yüklemeyi Doğrula'daki](#verify-successful-installation) adımları kullanın.

Bir aygıtı el ile yüklediğinizde ve tedarik ettiğinizde, aşağıdakiler de dahil olmak üzere yüklemeyi değiştirmek için ek parametreler kullanabilirsiniz:

* Proxy sunucusundan geçmek için doğrudan trafik
* Yükleyiciyi çevrimdışı dizine yönlendirin
* Belirli bir aracı kapsayıcı görüntüsünü bildirin ve özel bir kayıt defterindeyse kimlik bilgileri sağlayın

Bu yükleme seçenekleri hakkında daha fazla bilgi için, [tüm yükleme parametreleri](#all-installation-parameters)hakkında bilgi edinmek için ileri ye atlayın.

### <a name="option-2-install-and-automatically-provision"></a>Seçenek 2: Yükleme ve otomatik olarak sağlama

Bu ikinci seçenekte, aygıtı IoT Hub Aygıt Sağlama Hizmeti'ni kullanarak sağlarsınız. Bir Aygıt Sağlama Hizmeti örneğinden **Kapsam Kimliğini** ve tercih ettiğiniz [attestation mekanizmasına](../iot-dps/concepts-security.md#attestation-mechanism)özel diğer bilgileri sağlayın:

* [Windows'da sanal TPM içeren simüle edilmiş bir IoT Edge aygıtı oluşturma ve sağlama](how-to-auto-provision-simulated-device-windows.md)
* [X.509 sertifikalarını kullanarak simüle edilmiş bir IoT Edge aygıtı oluşturma ve sağlama](how-to-auto-provision-x509-certs.md)
* [Simetrik anahtar attestation kullanarak bir IoT Edge cihazı oluşturma ve sağlama](how-to-auto-provision-symmetric-keys.md)

Bir aygıtı otomatik olarak yüklediğinizde ve tedarik ettiğinizde, aşağıdakiler de dahil olmak üzere yüklemeyi değiştirmek için ek parametrelerk kullanabilirsiniz:

* Proxy sunucusundan geçmek için doğrudan trafik
* Yükleyiciyi çevrimdışı dizine yönlendirin
* Belirli bir aracı kapsayıcı görüntüsünü bildirin ve özel bir kayıt defterindeyse kimlik bilgileri sağlayın

Bu yükleme seçenekleri hakkında daha fazla bilgi için, bu makaleyi okumaya devam edin veya [tüm yükleme parametreleri](#all-installation-parameters)hakkında bilgi edinmek için atlayın.

## <a name="offline-or-specific-version-installation"></a>Çevrimdışı veya belirli sürüm yüklemesi

Yükleme sırasında üç dosya indirilir:

* Yükleme yönergelerini içeren bir PowerShell komut dosyası
* IoT Edge güvenlik daemon (iotedged), Moby konteyner motoru ve Moby CLI içeren Microsoft Azure IoT Edge kabini
* Visual C++ yeniden dağıtılabilir paket (VC çalışma zamanı) yükleyici

Cihazınız yükleme sırasında çevrimdışı olacaksa veya IoT Edge'in belirli bir sürümünü yüklemek istiyorsanız, bu dosyaları önceden aygıta indirebilirsiniz. Yükleme zamanı geldiğinde, yükleme komut dosyasını indirilen dosyaları içeren dizine yönlendirin. Yükleyici önce bu dizini denetler ve sonra yalnızca bulunamayan bileşenleri karşıdan yükler. Tüm dosyalar çevrimdışı olarak kullanılabilse, internet bağlantısı olmadan yükleyebilirsiniz.

IoT Edge'i güncelleştirmek için çevrimdışı yükleme yolu parametresini de kullanabilirsiniz. Daha fazla bilgi için Bkz. [IoT Edge güvenlik daemon ve çalışma süresini güncelleştirin.](how-to-update-iot-edge.md)

1. Önceki sürümlerle birlikte en son IoT Edge yükleme dosyaları için [Azure IoT Edge sürümlerine](https://github.com/Azure/azure-iotedge/releases)bakın.

2. Yüklemek istediğiniz sürümü bulun ve sürüm notlarının **Varlıklar** bölümünden Aşağıdaki dosyaları IoT aygıtınıza indirin:

   * IoTEdgeSecurityDaemon.ps1
   * 1.0.9 veya daha yeni sürümlerden Microsoft-Azure-IoTEdge-amd64.cab veya 1.0.8 ve üzeri sürümlerden Microsoft-Azure-IoTEdge.cab.

   Microsoft-Azure-IotEdge-arm32.cab, yalnızca test amacıyla 1.0.9'dan itibaren de kullanılabilir. IoT Edge şu anda Windows ARM32 aygıtlarında desteklenmemektedir.

   PowerShell komut dosyasını kullandığınız .cab dosyasıyla aynı sürümden kullanmanız önemlidir, çünkü işlevsellik her sürümdeki özellikleri destekleyecek şekilde değişir.

3. İndirdiğiniz .cab dosyasında mimari sonek varsa, dosyayı sadece **Microsoft-Azure-IoTEdge.cab**olarak yeniden adlandırın.

4. İsteğe bağlı olarak, Visual C++ yeniden dağıtılabilir için bir yükleyici indirin. Örneğin, PowerShell komut dosyası bu sürümü kullanır: [vc_redist.x64.exe](https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe). Yükleyiyi IoT aygıtınızdaki IoT Edge dosyalarıyla aynı klasöre kaydedin.

5. Çevrimdışı bileşenlerle yüklemek için [PowerShell](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7#script-scope-and-dot-sourcing) komut dosyasının yerel kopyasını nokta kaynağı na sahip olur. Ardından, komutun `-OfflineInstallationPath` bir `Deploy-IoTEdge` parçası olarak parametreyi kullanın ve dosya dizinine mutlak yolu sağlayın. Örneğin,

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Deploy-IoTEdge -OfflineInstallationPath <path>
   ```

   Dağıtım komutu, sağlanan yerel dosya dizininde bulunan tüm bileşenleri kullanır. .cab dosyası veya Visual C++ yükleyicisi eksikse, bunları karşıdan yüklemeyi dener.

6. Aygıtınızı `Initialize-IoTEdge` IoT Hub'da bir kimlikle sağlamak için komutu çalıştırın. El ile sağlama için bir aygıt bağlantı dizesi sağlayın veya önceki [otomatik sağlama](#option-2-install-and-automatically-provision) bölümünde açıklanan yöntemlerden birini seçin.

   Cihazınız çalıştırdıktan `Deploy-IoTEdge`sonra yeniden başlatıldıysa, çalışmadan `Initialize-IoTEdge`önce PowerShell komut dosyasını yeniden kaynak olarak göredir.

Çevrimdışı yükleme seçeneği hakkında daha fazla bilgi için, [tüm yükleme parametreleri](#all-installation-parameters)hakkında bilgi edinmek için ileri ye atlayın.

## <a name="verify-successful-installation"></a>Başarılı yüklemeyi doğrulama

IoT Edge hizmetinin durumunu kontrol edin. Çalışan olarak listelenmelidir.  

```powershell
Get-Service iotedge
```

Son 5 dakikadaki servis kayıtlarını inceleyin. IoT Edge çalışma süresini yüklemeyi yeni bitirdiyseniz, **Deploy-IoTEdge ve Initialize-IoTEdge'i** çalıştırma arasındaki zamana ait hataların bir listesini görebilirsiniz. **Initialize-IoTEdge** Hizmet yapılandırmadan önce başlamaya çalıştığından, bu hatalar beklenmektedir.

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

En yaygın yapılandırma ve ağ hataları için otomatik bir denetim çalıştırın.

```powershell
iotedge check
```

Çalışan modülleri listele. Yeni bir yüklemeden sonra, çalışan görmeniz gereken tek modül **edgeAgent**olduğunu. [IoT Edge modüllerini](how-to-deploy-modules-portal.md) ilk kez dağıttıktan sonra, **edgeHub**adındaki diğer sistem modülü de cihazda başlayacaktır.

```powershell
iotedge list
```

## <a name="manage-module-containers"></a>Modül konteynerlerini yönetme

IoT Edge hizmeti için cihazınızda çalışan bir konteyner motoru gerekir. Bir aygıta bir modül dağıttığınızda, IoT Edge çalışma zamanı, buluttaki bir kayıt defterinden kapsayıcı görüntüsünü çekmek için kapsayıcı altyapısını kullanır. IoT Edge hizmeti, modüllerinizle etkileşimkurmanızı ve günlükleri almanızı sağlar, ancak bazen kapsayıcının kendisiyle etkileşim kurmak için konteyner motorunu kullanmak isteyebilirsiniz.

Modül kavramları hakkında daha fazla bilgi için [bkz.](iot-edge-modules.md)

Windows IoT Edge aygıtınızda Windows kapsayıcıları çalıştırıyorsanız, IoT Edge yüklemesi Moby kapsayıcı motorunu içerir. Moby motoru Docker ile aynı standartlara dayanıyordu ve Docker Desktop ile aynı makinede çalışacak şekilde tasarlandı. Bu nedenle, Moby motoru tarafından yönetilen konteynerleri hedeflemek istiyorsanız, docker yerine özellikle o motoru hedeflemeniz gerekir.

Örneğin, tüm Docker resimlerini listelemek için aşağıdaki komutu kullanın:

```powershell
docker images
```

Tüm Moby görüntülerini listelemek için, aynı komutu Moby motoruna işaretçiyle değiştirin:

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

Motor URI yükleme komut dosyasının çıktısında listelenir veya config.yaml dosyası için konteyner çalışma zamanı ayarları bölümünde bulabilirsiniz.

![moby_runtime in config.yaml içinde uri](./media/how-to-install-iot-edge-windows/moby-runtime-uri.png)

Cihazınızda çalışan kapsayıcılar ve görüntülerle etkileşimde kalmak için kullanabileceğiniz komutlar hakkında daha fazla bilgi için [Docker komut satırı arabirimlerine](https://docs.docker.com/engine/reference/commandline/docker/)bakın.

## <a name="uninstall-iot-edge"></a>IoT Edge'i kaldır

IoT Edge yüklemesini Windows aygıtınızdan kaldırmak istiyorsanız, yönetimdeki PowerShell penceresinden aşağıdaki komutu kullanın. Bu komut, mevcut yapılandırmanız ve Moby motor verilerinizle birlikte IoT Edge çalışma süresini kaldırır.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-IoTEdge
```

Kaldır-IoTEdge komutu Windows IoT Core'da çalışmaz. IoT Edge'i Windows IoT Core aygıtlarından kaldırmak için Windows IoT Core görüntünüze yeniden dağıtmanız gerekir.

Yüklemeyi kaldırma seçenekleri hakkında daha `Get-Help Uninstall-IoTEdge -full`fazla bilgi için, komutu kullanın.

## <a name="verify-installation-script"></a>Yükleme komut dosyalarını doğrulama

Bu makalede sağlanan yükleme komutları, yükleme komutunu ' dan istemek `aka.ms/iotedge-win`için Invoke-WebRequest cmdlet'i kullanır. Bu bağlantı,`IoTEdgeSecurityDaemon.ps1` en son [IoT Edge sürümünden](https://github.com/Azure/azure-iotedge/releases)komut dosyasına işaret. IoT Edge aygıtınızdaki yükleme komutlarını çalıştırmak için bu komut dosyasını veya komut dosyasının bir sürümünü belirli bir sürümden de indirebilirsiniz.

Sağlanan komut dosyası güvenliği artırmak için imzalanır. Komut dosyasını cihazınıza indirerek ve ardından aşağıdaki PowerShell komutunu çalıştırarak imzayı doğrulayabilirsiniz:

```powershell
Get-AuthenticodeSignature "C:\<path>\IotEdgeSecurityDaemon.ps1"
```

İmza doğrulanırsa çıktı durumu **geçerlidir.**

## <a name="all-installation-parameters"></a>Tüm kurulum parametreleri

Önceki bölümlerde, yükleme komut dosyasını değiştirmek için parametrelerin nasıl kullanılacağına örnekler içeren ortak yükleme senaryoları sunulmuştur. Bu bölüm, IoT Edge'i yüklemek, güncelleştirmek veya kaldırmak için kullanılan yaygın parametrelerin başvuru tablolarını sağlar.

### <a name="deploy-iotedge"></a>Dağıtma-IoTEdge

Deploy-IoTEdge komutu, IoT Edge Güvenlik Daemon'unu ve bağımlılıklarını karşıdan yükler ve dağıtır. Dağıtım komutu, diğerlerinin yanı sıra bu ortak parametreleri kabul eder. Tam liste için, komutu `Get-Help Deploy-IoTEdge -full`kullanın.  

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **Konteyner Otoları** | **Windows** veya **Linux** | Kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir.<br><br>Windows kapsayıcıları için IoT Edge, yüklemede yer alan moby kapsayıcı motorunu kullanır. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı motoru yüklemeniz gerekir. |
| **Proxy** | Proxy URL | Cihazınızın internete ulaşmak için bir proxy sunucusundan geçmesi gerekiyorsa bu parametreyi ekleyin. Daha fazla bilgi için, [proxy sunucusu üzerinden iletişim kurmak için bir IoT Edge aygıtını yapılandırın'](how-to-configure-proxy-support.md)a bakın. |
| **ÇevrimdışıYüklemeYolu** | Dizin yolu | Bu parametre de dahil edilirse, yükleyici yükleme için gereken IoT Edge kabini ve VC Runtime MSI dosyaları için listelenen dizini denetler. Dizinde bulunmayan dosyalar indirilir. Her iki dosya da dizindeyse, Internet bağlantısı olmadan IoT Edge'i yükleyebilirsiniz. Bu parametreyi belirli bir sürümü kullanmak için de kullanabilirsiniz. |
| **InvokeWebRequestParametreleri** | Parametrelerin ve değerlerin karma tablosu | Yükleme sırasında, çeşitli web istekleri yapılır. Bu web istekleri için parametreleri ayarlamak için bu alanı kullanın. Bu parametre proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için, [proxy sunucusu üzerinden iletişim kurmak için bir IoT Edge aygıtını yapılandırın'](how-to-configure-proxy-support.md)a bakın. |
| **Yeniden BaşlatIfGerekli** | yok | Bu bayrak, dağıtım komut dosyasının gerekirse istenmeden makineyi yeniden başlatmasına olanak tanır. |

### <a name="initialize-iotedge"></a>Initialize-IoTEdge

Initialize-IoTEdge komutu, IoT Edge'i aygıt bağlantı dizenizle ve işletim ayrıntılarınızla yapılandırır. Bu komut tarafından oluşturulan bilgilerin çoğu daha sonra iotedge\config.yaml dosyasında depolanır. Başlatma komutu, diğerlerinin yanı sıra bu ortak parametreleri kabul eder. Tam liste için, komutu `Get-Help Initialize-IoTEdge -full`kullanın.

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **El ile** | None | **Geçiş parametresi**. Hiçbir sağlama türü belirtilmemişse, el kitabı varsayılan değerdir.<br><br>Aygıtı el ile sağlamak için bir aygıt bağlantı dizesi sağlayacağınızı bildirir |
| **Dps** | None | **Geçiş parametresi**. Hiçbir sağlama türü belirtilmemişse, el kitabı varsayılan değerdir.<br><br>DPS üzerinden sağlamak için bir Cihaz Sağlama Hizmeti (DPS) kapsam kimliği ve cihazınızın Kayıt Kimliği sağlayacağını beyan eder.  |
| **AygıtBağlantısı String** | Bir IoT Hub'ında kayıtlı bir IoT Edge aygıtından tek tırnak içinde bağlantı dizesi | Manuel sağlama için **gereklidir.** Komut dosyası parametrelerinde bir bağlantı dizesi sağlamazsanız, bir bağlantı için istenir. |
| **Scopeıd** | IoT Hub'ınızla ilişkili Aygıt Sağlama Hizmeti örneğinden bir kapsam kimliği. | DPS sağlanması için **gereklidir.** Komut dosyası parametrelerinde bir kapsam kimliği sağlamazsanız, sizden bir tane istenir. |
| **RegistrationId** | Cihazınız tarafından oluşturulan bir kayıt kimliği | TPM veya simetrik anahtar attestation kullanıyorsanız DPS sağlanması için **gereklidir.** X.509 sertifikası kullanıyorsanız **isteğe bağlıdır.** |
| **X509Kimlik Sertifikası** | Cihazdaki X.509 aygıt kimlik sertifikasına uri yolu. | X.509 sertifikası attestation kullanıyorsanız DPS sağlama için **gereklidir.** |
| **X509IdentityPrivateKey** | Uri yolu, aygıttaki X.509 aygıt kimlik sertifikası anahtarına. | X.509 sertifikası attestation kullanıyorsanız DPS sağlama için **gereklidir.** |
| **Simetrik Anahtar** | DPS kullanırken IoT Edge aygıt kimliğini sağlamak için kullanılan simetrik anahtar | Simetrik anahtar attestation kullanıyorsanız DPS sağlanması için **gereklidir.** |
| **Konteyner Otoları** | **Windows** veya **Linux** | Kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir.<br><br>Windows kapsayıcıları için IoT Edge, yüklemede yer alan moby kapsayıcı motorunu kullanır. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı motoru yüklemeniz gerekir. |
| **InvokeWebRequestParametreleri** | Parametrelerin ve değerlerin karma tablosu | Yükleme sırasında, çeşitli web istekleri yapılır. Bu web istekleri için parametreleri ayarlamak için bu alanı kullanın. Bu parametre proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için, [proxy sunucusu üzerinden iletişim kurmak için bir IoT Edge aygıtını yapılandırın'](how-to-configure-proxy-support.md)a bakın. |
| **AgentImage** | IoT Edge aracı sıyrık görüntüsü URI | Varsayılan olarak, yeni bir IoT Edge yüklemesi IoT Edge aracısı görüntüsü için en son yuvarlanma etiketini kullanır. Görüntü sürümü için belirli bir etiket ayarlamak veya kendi aracı resminizi sağlamak için bu parametreyi kullanın. Daha fazla bilgi için [Bkz. IoT Edge etiketlerini anlayın.](how-to-update-iot-edge.md#understand-iot-edge-tags) |
| **Username** | Konteyner kayıt defteri kullanıcı adı | Bu parametreyi yalnızca -AgentImage parametresini özel bir kayıt defterindeki bir kapsayıcıya ayarlarsanız kullanın. Kayıt defterine erişimi olan bir kullanıcı adı sağlayın. |
| **Parola** | Güvenli parola dizesi | Bu parametreyi yalnızca -AgentImage parametresini özel bir kayıt defterindeki bir kapsayıcıya ayarlarsanız kullanın. Kayıt defterine erişmek için parolayı girin. |

### <a name="update-iotedge"></a>Güncelleme-IoTEdge

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **Konteyner Otoları** | **Windows** veya **Linux** | Kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir. Windows kapsayıcıları için yüklemeye bir kapsayıcı motoru dahil edilir. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı motoru yüklemeniz gerekir. |
| **Proxy** | Proxy URL | Cihazınızın internete ulaşmak için bir proxy sunucusundan geçmesi gerekiyorsa bu parametreyi ekleyin. Daha fazla bilgi için, [proxy sunucusu üzerinden iletişim kurmak için bir IoT Edge aygıtını yapılandırın'](how-to-configure-proxy-support.md)a bakın. |
| **InvokeWebRequestParametreleri** | Parametrelerin ve değerlerin karma tablosu | Yükleme sırasında, çeşitli web istekleri yapılır. Bu web istekleri için parametreleri ayarlamak için bu alanı kullanın. Bu parametre proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için, [proxy sunucusu üzerinden iletişim kurmak için bir IoT Edge aygıtını yapılandırın'](how-to-configure-proxy-support.md)a bakın. |
| **ÇevrimdışıYüklemeYolu** | Dizin yolu | Bu parametre de dahil edilirse, yükleyici yükleme için gereken IoT Edge kabini ve VC Runtime MSI dosyaları için listelenen dizini denetler. Dizinde bulunmayan dosyalar indirilir. Her iki dosya da dizindeyse, Internet bağlantısı olmadan IoT Edge'i yükleyebilirsiniz. Bu parametreyi belirli bir sürümü kullanmak için de kullanabilirsiniz. |
| **Yeniden BaşlatIfGerekli** | yok | Bu bayrak, dağıtım komut dosyasının gerekirse istenmeden makineyi yeniden başlatmasına olanak tanır. |

### <a name="uninstall-iotedge"></a>Kaldır-IoTEdge

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **Kuvvet** | yok | Bu bayrak, önceki kaldırma girişiminin başarısız olması durumunda yüklemeyi kaldırmayı zorlar.
| **Yeniden BaşlatIfGerekli** | yok | Bu bayrak, gerekirse komut dosyasının kaldırmasını istenmeden makineyi yeniden başlatmasına olanak tanır. |

## <a name="next-steps"></a>Sonraki adımlar

Artık çalışma zamanı yüklü bir IoT Edge aygıtınız olduğuna göre, [IoT Edge modüllerini dağıtabilirsiniz.](how-to-deploy-modules-portal.md)

IoT Edge'in düzgün bir şekilde yüklenmesinde sorun yaşıyorsanız, [sorun giderme](troubleshoot.md) sayfasına göz atın.

Varolan bir yüklemeyi IoT Edge'in en yeni sürümüne güncelleştirmek için [bkz.](how-to-update-iot-edge.md)
