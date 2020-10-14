---
title: Azure IoT Edge yüklensin | Microsoft Docs
description: Windows veya Linux cihazlarda yükleme yönergelerini Azure IoT Edge
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: kgremban
ms.openlocfilehash: 3a02459f5b92aa7d708c29c737ed9428ed14215a
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92045695"
---
# <a name="install-or-uninstall-the-azure-iot-edge-runtime"></a>Azure IoT Edge çalışma zamanını yükleme veya kaldırma

Azure IoT Edge çalışma zamanı, bir cihazı IoT Edge cihazına dönüştürür. Çalışma zamanı, cihazlarda Raspberry Pi kadar küçük veya endüstriyel sunucu olarak büyük olarak dağıtılabilir. Bir cihaz IoT Edge çalışma zamanıyla yapılandırıldığında, buluttan cihaza iş mantığını dağıtmaya başlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure IoT Edge çalışma zamanını ve mimarisini anlayın](iot-edge-runtime.md).

IoT Edge bir cihaz kurmak için iki adım vardır. İlk adım, bu makalede ele alınan çalışma zamanını ve bağımlılıklarını yüklemektir. İkinci adım, cihazı buluttaki kimliğine bağlamak ve IoT Hub kimlik doğrulamasını kurmak olur. Bu adımlar sonraki makalelerde bulunur.

Bu makalede, Azure IoT Edge çalışma zamanını Linux veya Windows cihazlarına yüklemek için gereken adımlar listelenmektedir. Windows cihazlarında, Linux kapsayıcıları veya Windows kapsayıcıları kullanma ek bir seçeneğiniz vardır. Şu anda, üretim senaryolarında Windows 'daki Windows kapsayıcıları önerilir. Windows üzerinde Linux kapsayıcıları, özellikle de Linux cihazlarına dağıtmak üzere bir Windows BILGISAYARı geliştiriyorsanız geliştirme ve test senaryoları için yararlıdır.

## <a name="prerequisites"></a>Önkoşullar

Şu anda üretim senaryolarında desteklenen işletim sistemleri hakkında en son bilgiler için, bkz. [Azure IoT Edge desteklenen sistemler](support.md#operating-systems)

# <a name="linux"></a>[Linux](#tab/linux)

X64, ARM32 veya ARM64 Linux cihazı vardır. Microsoft, Ubuntu Server 16,04, Ubuntu Server 18,04 ve Raspbian Esnetme işletim sistemleri için yükleme paketleri sağlar.

>[!NOTE]
>ARM64 cihazlar için destek [genel önizlemede](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Cihazınızı Microsoft Yükleme paketlerine erişmek için hazırlayın.

1. Cihaz işletim sisteminizle eşleşen depo yapılandırmasını yükler.

   * **Ubuntu Server 16,04**:

     ```bash
     curl https://packages.microsoft.com/config/ubuntu/16.04/multiarch/prod.list > ./microsoft-prod.list
     ```

   * **Ubuntu Server 18,04**:

     ```bash
     curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
     ```

   * **Raspbian Esnetme**:

     ```bash
     curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
     ```

2. Oluşturulan listeyi sources. List. d dizinine kopyalayın.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

3. Microsoft GPG ortak anahtarını yükler.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

# <a name="windows"></a>[Windows](#tab/windows)

### <a name="windows-version"></a>Windows sürümü

Windows kapsayıcıları ile IoT Edge, en son [Windows uzun süreli destek derlemesi](/windows/release-information/)olan Windows sürümü 1809/Build 17762 gerektirir. Geliştirme ve test senaryolarında, kapsayıcıları özelliğini destekleyen herhangi bir SKU (Pro, kurumsal, sunucu vb.) çalışır. Ancak, üretime geçmeden önce desteklenen sistemler listesini gözden geçirdiğinizden emin olun.

Linux kapsayıcılarıyla IoT Edge, [Docker Desktop gereksinimlerini](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)karşılayan herhangi bir Windows sürümünde çalıştırılabilir.

### <a name="container-engine-requirements"></a>Kapsayıcı altyapısı gereksinimleri

Azure IoT Edge, [OCI uyumlu](https://www.opencontainers.org/) bir kapsayıcı altyapısına bağlıdır. Cihazınızın kapsayıcıları destekleyeceği şekilde emin olun.

Bir sanal makineye IoT Edge yüklüyorsanız, iç içe sanallaştırmayı etkinleştirin ve en az 2 GB bellek ayırın. Hyper-V için 2. nesil sanal makinelerde varsayılan olarak etkinleştirilmiş iç içe sanallaştırma vardır. VMware için, sanal makinenizde özelliği etkinleştirmek üzere bir geçiş düğmesi vardır.

---

Azure IoT Edge yazılım paketleri, her pakette ( `usr/share/doc/{package-name}` veya dizinde) bulunan lisans koşullarına tabidir `LICENSE` . Paket kullanmadan önce lisans koşullarını okuyun. Bir paketi yüklemeniz ve kullanmanız, bu şartlarınızın kabul edildiğini oluşturur. Lisans koşullarını kabul etmiyorsanız, bu paketi kullanmayın.

## <a name="install-a-container-engine"></a>Kapsayıcı Altyapısı yükler

Azure IoT Edge, OCı uyumlu bir kapsayıcı çalışma zamanına bağlıdır. Üretim senaryolarında, Moby tabanlı altyapıyı kullanmanızı öneririz. Moby motoru, Azure IoT Edge ile resmi olarak desteklenen tek kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntüleri Moby çalışma zamanına göre uyumludur.

# <a name="linux"></a>[Linux](#tab/linux)

Cihazınızdaki paket listelerini güncelleştirin.

   ```bash
   sudo apt-get update
   ```

Moby altyapısını yükler.

   ```bash
   sudo apt-get install moby-engine
   ```

Moby kapsayıcı altyapısını yüklerken hata alırsanız, Moby uyumluluğu için Linux çekirdeğini doğrulayın. Bazı katıştırılmış cihaz üreticileri, kapsayıcı altyapısı uyumluluğu için gerekli özellikler olmadan özel Linux çekirdekleri içeren cihaz görüntülerini sevk etti. Çekirdek yapılandırmanızı denetlemek için Moby tarafından sunulan [Check-config betiğini](https://github.com/moby/moby/blob/master/contrib/check-config.sh) kullanan aşağıdaki komutu çalıştırın:

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

Komut dosyasının çıktısında, ve altındaki tüm öğelerin etkin olduğunu kontrol edin `Generally Necessary` `Network Drivers` . Özellikler eksik ise, çekirdeğini kaynaktan yeniden oluşturarak ve uygun çekirdek. config dosyasına eklenmek üzere ilişkili modülleri seçerek etkinleştirin. Benzer şekilde, veya gibi bir çekirdek yapılandırma Oluşturucu kullanıyorsanız `defconfig` `menuconfig` ilgili özellikleri bulup etkinleştirin ve çekirdeğini uygun şekilde yeniden oluşturun. Yeni değiştirilen çekirdeğini dağıttıktan sonra, gerekli tüm özelliklerin başarıyla etkinleştirildiğini doğrulamak için Check-config betiğini yeniden çalıştırın.

# <a name="windows"></a>[Windows](#tab/windows)

Üretim senaryolarında, yükleme betiğine dahil olan Moby tabanlı altyapıyı kullanın. Altyapıyı yüklemek için başka bir adım yoktur.

Linux kapsayıcılarıyla IoT Edge için kendi kapsayıcı çalışma zamanını sağlamanız gerekir. Cihazınıza [Docker Desktop](https://docs.docker.com/docker-for-windows/install/) 'ı yükleyip devam etmeden önce [Linux kapsayıcılarını kullanacak](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) şekilde yapılandırın.

---

## <a name="install-the-iot-edge-security-daemon"></a>IoT Edge güvenlik cini 'nı yükler

IoT Edge güvenlik arka plan programı, IoT Edge cihazında güvenlik standartları sağlar ve korur. Arka plan programı her önyüklemede başlar ve IoT Edge çalışma zamanının geri kalanını başlatarak cihazı önyükleme.

Bu bölümdeki adımlarda, internet bağlantısı olan bir cihaza en son sürümü yüklemek için tipik bir işlem temsil etmektedir. Yayın öncesi sürüm gibi belirli bir sürümü yüklemeniz ya da çevrimdışıyken yüklemeniz gerekiyorsa, sonraki bölümde [çevrimdışı veya belirli sürüm yükleme](#offline-or-specific-version-installation) adımlarını izleyin.

# <a name="linux"></a>[Linux](#tab/linux)

Cihazınızdaki paket listelerini güncelleştirin.

   ```bash
   sudo apt-get update
   ```

Hangi IoT Edge sürümlerinin kullanılabilir olduğunu görmek için denetleyin.

   ```bash
   apt list -a iotedge
   ```

Güvenlik arka plan programının en son sürümünü yüklemek istiyorsanız, **libiothsm-STD** paketinin en son sürümünü de yükleyen aşağıdaki komutu kullanın:

   ```bash
   sudo apt-get install iotedge
   ```

Güvenlik arka plan programının belirli bir sürümünü yüklemek istiyorsanız, apt liste çıktısından sürümü belirtin. Ayrıca, **libiothsm-STD** paketi için aynı sürümü belirtin, aksi takdirde en son sürümünü yükler. Örneğin, aşağıdaki komut 1.0.8 sürümünün en son sürümünü yüklüyor:

   ```bash
   sudo apt-get install iotedge=1.0.8* libiothsm-std=1.0.8*
   ```

Yüklemek istediğiniz sürüm listelenmemişse, sonraki bölümde yer alan [çevrimdışı veya belirli sürüm yükleme](#offline-or-specific-version-installation) adımlarını izleyin. Bu bölümde, IoT Edge güvenlik arka plan programının veya sürüm adayı sürümlerinin önceki bir sürümünün nasıl hedeflenecek gösterilmektedir.

# <a name="windows"></a>[Windows](#tab/windows)

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

3. Bu noktada, çıkış sizden yeniden başlatmanız istenebilir. Bu durumda cihazınızı şimdi yeniden başlatın.

Bir cihaza IoT Edge yüklediğinizde, aşağıdakileri de içeren ek parametreleri kullanarak işlemi değiştirebilirsiniz:

* Bir proxy sunucusundan gelen trafiği doğrudan gönder
* Yükleyiciyi çevrimdışı yükleme için yerel bir dizine işaret edin.

Bu ek parametreler hakkında daha fazla bilgi için bkz. [Windows üzerinde IoT Edge Için PowerShell betikleri](reference-windows-scripts.md).

---

Artık kapsayıcı altyapısı ve IoT Edge çalışma zamanı cihazınızda yüklü olduğuna göre, cihazınızı IoT Hub kaydetmek ve cihazı bulut kimliği ve kimlik doğrulama bilgileriyle ayarlamak için bir sonraki adıma hazırsınız.

Kullanmak istediğiniz kimlik doğrulama türüne göre sonraki makaleyi seçin:

* [Simetrik anahtar kimlik doğrulaması](how-to-manual-provision-symmetric-key.md) , başlamak için daha hızlıdır.
* [X. 509.440 sertifikası kimlik doğrulaması](how-to-manual-provision-x509.md) , üretim senaryolarında daha güvenlidir.

## <a name="offline-or-specific-version-installation"></a>Çevrimdışı veya belirli sürümü yükleme

Bu bölümdeki adımlar, standart yükleme adımlarının kapsamadığı senaryolar içindir. Bu şunlar olabilir:

* Çevrimdışıyken IoT Edge yüklemesi
* Sürüm Adayı sürümü yüklemesi
* Windows 'ta, en son sürümü dışında bir sürüm yükler

# <a name="linux"></a>[Linux](#tab/linux)

Tarafından kullanılamayan Azure IoT Edge çalışma zamanının belirli bir sürümünü yüklemek istiyorsanız bu bölümdeki adımları kullanın `apt-get install` . Microsoft paket listesi yalnızca sınırlı sayıda yeni sürümü ve alt sürümlerini içerir, bu nedenle bu adımlar daha eski bir sürüm veya sürüm adayı sürümü yüklemek isteyen herkese yöneliktir.

Kıvrımlı komutlarını kullanarak bileşen dosyalarını doğrudan IoT Edge GitHub deposundan hedefleyebilirsiniz.

<!-- TODO: Offline installation? -->

1. [Azure IoT Edge yayınlarına](https://github.com/Azure/azure-iotedge/releases)gidin ve hedeflemek istediğiniz yayın sürümünü bulun.

2. Bu sürümün **varlıklar** bölümünü genişletin.

3. Her yayında IoT Edge güvenlik Daemon ve hsmlib için yeni dosyalar olmalıdır. Bu bileşenleri güncelleştirmek için aşağıdaki komutları kullanın.

   1. IoT Edge cihazınızın mimarisiyle eşleşen **libiothsm-STD** dosyasını bulun. Dosya bağlantısına sağ tıklayıp bağlantı adresini kopyalayın.

   2. Hsmlib 'in bu sürümünü yüklemek için aşağıdaki komutta bulunan kopyalanmış bağlantıyı kullanın:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```

   3. IoT Edge cihazınızın mimarisiyle eşleşen **iotedge** dosyasını bulun. Dosya bağlantısına sağ tıklayıp bağlantı adresini kopyalayın.

   4. IoT Edge güvenlik arka plan programının bu sürümünü yüklemek için aşağıdaki komutta bulunan kopyalanmış bağlantıyı kullanın.

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

# <a name="windows"></a>[Windows](#tab/windows)

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

3. İndirdiğiniz. cab dosyasının üzerinde bir mimari soneki varsa, dosyayı yalnızca **Microsoft-Azure-IoTEdge.cab**olarak yeniden adlandırın.

4. İsteğe bağlı olarak, Visual C++ yeniden dağıtılabilir için bir yükleyici indirebilirsiniz. Örneğin, PowerShell betiği şu sürümü kullanır: [vc_redist.x64.exe](https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe). Yükleyiciyi, IoT Edge dosyaları olarak IoT cihazınızda aynı klasöre kaydedin.

5. Çevrimdışı bileşenlerle yüklemek için, [nokta kaynağı](/powershell/module/microsoft.powershell.core/about/about_scripts#script-scope-and-dot-sourcing) PowerShell betiğinin yerel kopyasıdır. 

6. Parametresiyle [Deploy-ıotedge](reference-windows-scripts.md#deploy-iotedge) komutunu çalıştırın `-OfflineInstallationPath` . Dosya dizinine mutlak yol girin. Örneğin,

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Deploy-IoTEdge -OfflineInstallationPath <path>
   ```

   Dağıtım komutu, belirtilen yerel dosya dizininde bulunan tüm bileşenleri kullanır. . Cab dosyası veya Visual C++ yükleyicisi eksikse, bunları indirmeyi dener.

---

Artık kapsayıcı altyapısı ve IoT Edge çalışma zamanı cihazınızda yüklü olduğuna göre, bir sonraki adım için hazır olursunuz. Bu, [IoT Hub bir IoT Edge cihazının kimlik doğrulaması](how-to-manual-provision-symmetric-key.md)için kullanılır.

## <a name="uninstall-iot-edge"></a>IoT Edge kaldır

IoT Edge yüklemesini cihazınızdan kaldırmak istiyorsanız aşağıdaki komutları kullanın.

# <a name="linux"></a>[Linux](#tab/linux)

IoT Edge çalışma zamanını kaldırın.

```bash
sudo apt-get remove --purge iotedge
```

IoT Edge çalışma zamanı kaldırıldığında, oluşturduğu kapsayıcılar durdurulur ancak cihazınızda hala bulunur. Hangi olanların kaldığını görmek için tüm kapsayıcıları görüntüleyin.

```bash
sudo docker ps -a
```

İki çalışma zamanı kapsayıcısı dahil olmak üzere cihazınızdaki kapsayıcıları silin.

```bash
sudo docker rm -f <container name>
```

Son olarak, kapsayıcı çalışma zamanını cihazınızdan kaldırın.

```bash
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

# <a name="windows"></a>[Windows](#tab/windows)

IoT Edge yüklemesini Windows cihazınızdan kaldırmak istiyorsanız, bir yönetim PowerShell penceresinden [Uninstall-ıotedge](reference-windows-scripts.md#uninstall-iotedge) komutunu kullanın. Bu komut, IoT Edge çalışma zamanını, mevcut yapılandırmanızla ve Moby motoru verileriyle birlikte kaldırır.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-IoTEdge
```

Kaldırma seçenekleri hakkında daha fazla bilgi için komutunu kullanın `Get-Help Uninstall-IoTEdge -full` .

---

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge çalışma zamanını yükledikten sonra cihazınızı IoT Hub bağlanacak şekilde yapılandırın. Aşağıdaki makalelerde, buluta yeni bir cihaz kaydetme ve ardından cihazı kimlik ve kimlik doğrulama bilgilerini sağlama adımları gösterilmektedir.

Kullanmak istediğiniz kimlik doğrulaması türüne göre sonraki makaleyi seçin:

* **Simetrik anahtar**: hem IoT Hub hem de IoT Edge cihaz güvenli anahtarın bir kopyasına sahiptir. Cihaz IoT Hub bağlandığı zaman, anahtarların eşleştiğini denetler. Bu kimlik doğrulama yöntemi kullanmaya başlamak için daha hızlıdır, ancak güvenli değildir.

  [Simetrik anahtar kimlik doğrulaması ile Azure IoT Edge cihazı ayarlama](how-to-manual-provision-symmetric-key.md)

* **X. 509.440 otomatik imzalı**: IoT Edge cihazda x. 509.440 kimlik sertifikaları vardır ve IoT Hub sertifikaların parmak izi verilir. Cihaz IoT Hub bağlandığı zaman, sertifikayı parmak izine göre karşılaştırırlar. Bu kimlik doğrulama yöntemi daha güvenlidir ve üretim senaryoları için önerilir.

  [X. 509.440 sertifikası kimlik doğrulaması ile bir Azure IoT Edge cihazı ayarlama](how-to-manual-provision-x509.md)