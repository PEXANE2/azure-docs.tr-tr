---
title: Cihazlarda IoT Edge sürümü güncelleştirme-Azure IoT Edge | Microsoft Docs
description: Güvenlik cini ve IoT Edge çalışma zamanının en son sürümlerini çalıştırmak için IoT Edge cihazları güncelleştirme
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/22/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ee00425da89391e5228f2d48b49ca85426066f1e
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85299016"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>IoT Edge güvenlik daemon'ını ve çalışma zamanını güncelleştirme

IoT Edge hizmeti yeni sürümleri yayındıkça, en son özellikler ve güvenlik geliştirmeleri için IoT Edge cihazlarınızı güncellemek isteyeceksiniz. Bu makalede yeni bir sürüm kullanılabilir olduğunda IoT Edge cihazlarınızı güncelleştirme hakkında bilgi sağlanır.

Daha yeni bir sürüme geçmek istiyorsanız, bir IoT Edge cihazının iki bileşeni de güncelleştirilmeleri gerekir. Birincisi, cihazda çalışan ve cihaz başladığında çalışma zamanı modüllerini Başlatan güvenlik cini ' dir. Şu anda güvenlik arka plan programı yalnızca cihazın kendisinden güncelleştirilebilen olabilir. İkinci bileşen, IoT Edge hub ve IoT Edge aracı modülleriyle oluşturulan çalışma zamanı. Dağıtımınızı nasıl yapılandırdığınıza bağlı olarak, çalışma zamanı cihazdan veya uzaktan güncelleştirilebilen olabilir.

En son Azure IoT Edge sürümünü bulmak için, bkz. [Azure IoT Edge yayınlar](https://github.com/Azure/azure-iotedge/releases).

## <a name="update-the-security-daemon"></a>Güvenlik cini güncelleştirme

IoT Edge güvenlik arka plan programı, IoT Edge cihazında Paket Yöneticisi kullanılarak güncelleştirilmesi gereken yerel bir bileşendir.

Komutunu kullanarak cihazınızda çalışan güvenlik arka plan programının sürümünü denetleyin `iotedge version` .

### <a name="linux-devices"></a>Linux cihazları

Linux x64 cihazlarda, güvenlik cini en son sürüme güncelleştirmek için apt-get veya uygun paket yöneticisini kullanın.

Microsoft 'tan en son depo yapılandırmasını alın:

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

Oluşturulan listeyi kopyalayın.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Microsoft GPG ortak anahtarını yükler.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

Apt 'yi güncelleştirin.

   ```bash
   sudo apt-get update
   ```

Hangi IoT Edge sürümlerinin kullanılabilir olduğunu görmek için denetleyin.

   ```bash
   apt list -a iotedge
   ```

Güvenlik arka plan programının en son sürümüne güncelleştirmek istiyorsanız, **libiothsm-STD** ' ı en son sürüme de güncelleştiren aşağıdaki komutu kullanın:

   ```bash
   sudo apt-get install iotedge
   ```

Güvenlik arka plan programının belirli bir sürümüne güncelleştirmek istiyorsanız, apt listesi çıktısından sürümü belirtin. **İotedge** güncelleştirildiğinde, otomatik olarak **libiothsm-STD** paketini en son sürümüne güncelleştirmeye çalışır ve bu da bağımlılık çakışmasına neden olabilir. En son sürüme gitmeyecekleriniz için her iki paketi de aynı sürüme hedeflediğinizden emin olun. Örneğin, aşağıdaki komut 1.0.9 sürümünün belirli bir sürümünü yüklüyor:

   ```bash
   sudo apt-get install iotedge=1.0.9-1 libiothsm-std=1.0.9-1
   ```

Yüklemek istediğiniz sürüm apt-get ile kullanılamıyorsa, [IoT Edge yayımları](https://github.com/Azure/azure-iotedge/releases) deposundaki herhangi bir sürümü hedeflemek için kıvrımlı kullanabilirsiniz. Yüklemek istediğiniz sürüm için cihazınız için uygun **libiothsm-STD** ve **ıotedge** dosyalarını bulun. Her dosya için dosya bağlantısına sağ tıklayın ve bağlantı adresini kopyalayın. Bu bileşenlerin belirli sürümlerini yüklemek için bağlantı adresini kullanın:

```bash
curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
```

### <a name="windows-devices"></a>Windows cihazları

Windows cihazlarında güvenlik cini 'nı güncelleştirmek için PowerShell betiğini kullanın. Komut dosyası otomatik olarak güvenlik arka plan programının en son sürümünü çeker.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

Update-ıotedge komutunun çalıştırılması, iki çalışma zamanı kapsayıcı görüntüsü ile birlikte cihazınızdan güvenlik cini kaldırır ve güncelleştirir. Config. YAML dosyası cihazda, ayrıca Moby kapsayıcı altyapısından (Windows kapsayıcıları kullanıyorsanız) veriler tutulur. Yapılandırma bilgilerinin tutulması, güncelleştirme işlemi sırasında cihazınız için bağlantı dizesini veya cihaz sağlama hizmeti bilgilerini yeniden sağlamanız gerekmediği anlamına gelir.

Güvenlik arka plan programının belirli bir sürümüne güncelleştirmek istiyorsanız, [IoT Edge sürümlerden](https://github.com/Azure/azure-iotedge/releases)hedeflemek istediğiniz sürümü bulun. Bu sürümde, **Microsoft-Azure-IoTEdge.cab** dosyasını indirin. Ardından, `-OfflineInstallationPath` yerel dosya konumunu işaret etmek için parametresini kullanın. Örneğin:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux> -OfflineInstallationPath <absolute path to directory>
```

>[!NOTE]
>`-OfflineInstallationPath`Parametresi, belirtilen dizinde **Microsoft-Azure-IoTEdge.cab** adlı bir dosya arar. IoT Edge Version 1.0.9-RC4 ile başlayarak, biri AMD64 cihaz ve diğeri ARM32 için kullanılabilecek iki. cab dosyası vardır. Cihazınız için doğru dosyayı indirin ve ardından mimari sonekini kaldırmak için dosyayı yeniden adlandırın.

Güncelleştirme seçenekleri hakkında daha fazla bilgi için komutunu kullanın `Get-Help Update-IoTEdge -full` veya [tüm yükleme parametrelerine](how-to-install-iot-edge-windows.md#all-installation-parameters)başvurun.

## <a name="update-the-runtime-containers"></a>Çalışma zamanı kapsayıcılarını güncelleştirme

IoT Edge aracısını ve IoT Edge hub kapsayıcılarını güncelleştirme yöntemi, dağıtımınızda (1,0 gibi) veya belirli Etiketler (örneğin, 1.0.7) kullanmanıza göre değişir.

IoT Edge aracısının sürümünü ve şu anda cihazınızdaki IoT Edge hub modüllerini ve komutlarını kullanarak denetleyin `iotedge logs edgeAgent` `iotedge logs edgeHub` .

  ![Günlüklerde kapsayıcı sürümünü bul](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>IoT Edge etiketlerini anlama

IoT Edge Aracısı ve IoT Edge hub görüntüleri ilişkili oldukları IoT Edge sürümü ile etiketlenir. Çalışma zamanı görüntülerinde Etiketler kullanmanın iki farklı yolu vardır:

* **Kayan Etiketler** -bu basamakla eşleşen en son görüntüyü almak için sürüm numarasının yalnızca ilk iki değerini kullanın. Örneğin, en son 1.0. x sürümünü işaret eden yeni bir sürüm olduğunda 1,0 güncelleştirilir. IoT Edge cihazınızdaki kapsayıcı çalışma zamanı görüntüyü yeniden çeker, çalışma zamanı modülleri en son sürüme güncelleştirilir. Bu yaklaşım geliştirme amaçlarıyla önerilir. Azure portal varsayılan olan dağıtımlar, kayan etiketlere.

* **Belirli Etiketler** -görüntü sürümünü açıkça ayarlamak için sürüm numarasının tüm üç değerini kullanın. Örneğin, 1.0.7 ilk sürümünden sonra değişmez. Güncelleştirme yapmaya hazırsanız dağıtım bildiriminde yeni bir sürüm numarası bildirebilirsiniz. Bu yaklaşım, üretim amaçları için önerilir.

### <a name="update-a-rolling-tag-image"></a>Bir sıralı etiket görüntüsünü güncelleştirme

Dağıtımınızda sıralı Etiketler kullanıyorsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1,0**), cihazdaki kapsayıcı çalışma zamanını görüntünün en son sürümünü çekmek için zorlamanız gerekir.

IoT Edge cihazınızdan görüntünün yerel sürümünü silin. Windows makinelerinde, güvenlik arka plan programının kaldırılması çalışma zamanı görüntülerini da kaldırır, bu nedenle bu adımı tekrar yapmanız gerekmez.

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

`-f`Görüntüleri kaldırmak için zorla bayrağını kullanmanız gerekebilir.

IoT Edge hizmeti, çalışma zamanı görüntülerinin en son sürümlerini çeker ve yeniden cihazınızda otomatik olarak başlatır.

### <a name="update-a-specific-tag-image"></a>Belirli bir etiket görüntüsünü güncelleştirme

Dağıtımınızda belirli Etiketler kullanırsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1.0.8**), tüm yapmanız gereken, dağıtım bildiriminizde etiketi güncelleştirin ve değişiklikleri cihazınıza uygular.

1. Azure portal IoT Hub IoT Edge cihazınızı seçin ve **modülleri ayarla**' yı seçin.

1. **IoT Edge modüller** bölümünde **çalışma zamanı ayarları**' nı seçin.

   ![Çalışma zamanı ayarlarını yapılandırma](./media/how-to-update-iot-edge/configure-runtime.png)

1. **Çalışma zamanı ayarları**' nda **Edge hub** için **görüntü** değerini istenen sürümle güncelleştirin. Henüz **Kaydet** ' i seçmeyin.

   ![Edge hub görüntüsünü güncelleştirme sürümü](./media/how-to-update-iot-edge/runtime-settings-edgehub.png)

1. **Edge hub** ayarlarını daraltın veya aşağı kaydırın ve **Edge Aracısı** için **görüntü** değerini aynı istenen sürümle güncelleştirin.

   ![Edge hub Agent sürümünü Güncelleştir](./media/how-to-update-iot-edge/runtime-settings-edgeagent.png)

1. **Kaydet**’i seçin.

1. **Gözden geçir + oluştur**' u seçin, dağıtımı gözden geçirin ve **Oluştur**' u seçin.

## <a name="update-offline-or-to-a-specific-version"></a>Çevrimdışı veya belirli bir sürümü güncelleştirme

Bir cihazı çevrimdışı güncelleştirmek veya en son sürümü yerine IoT Edge belirli bir sürümüne güncelleştirmek istiyorsanız, bunu `-OfflineInstallationPath` parametresiyle yapabilirsiniz.

IoT Edge cihazı güncelleştirmek için iki bileşen kullanılır:

* Yükleme yönergelerini içeren bir PowerShell betiği
* IoT Edge güvenlik cini (ıotedşlı), Moby kapsayıcı altyapısını ve Moby CLı 'yi içeren IoT Edge cab Microsoft Azure

1. Önceki sürümlerle birlikte en son IoT Edge yükleme dosyaları için bkz. [Azure IoT Edge yayınlar](https://github.com/Azure/azure-iotedge/releases).

2. Yüklemek istediğiniz sürümü bulun ve sürüm notlarının **varlıklar** bölümünden aşağıdaki dosyaları IoT cihazınıza indirin:

   * IoTEdgeSecurityDaemon.ps1
   * 1.0.9 veya daha yeni sürümlerden Microsoft-Azure-IoTEdge.cab veya 1.0.8 'den daha eski sürümlerden Microsoft-Azure-IoTEdge-amd64.cab.

   Microsoft-Azure-IotEdge-arm32.cab, yalnızca test için 1.0.9 ile başlayarak da kullanılabilir. IoT Edge Şu anda Windows ARM32 cihazlarında desteklenmemektedir.

   Her sürümdeki özellikleri desteklemek üzere değiştiğinden, kullandığınız. cab dosyası ile aynı sürümden PowerShell betiğinin kullanılması önemlidir.

3. İndirdiğiniz. cab dosyasının üzerinde bir mimari soneki varsa, dosyayı yalnızca **Microsoft-Azure-IoTEdge.cab**olarak yeniden adlandırın.

4. Çevrimdışı bileşenlerle güncelleştirmek için, [nokta kaynağı](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7#script-scope-and-dot-sourcing) PowerShell betiğinin yerel kopyasıdır. Sonra, `-OfflineInstallationPath` parametresini komutunun bir parçası olarak kullanın `Update-IoTEdge` ve dosya dizinine mutlak yolu sağlayın. Örneğin,

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Update-IoTEdge -OfflineInstallationPath <path>
   ```

## <a name="update-to-a-release-candidate-version"></a>Sürüm adayı sürümüne güncelleştirme

Azure IoT Edge, IoT Edge hizmetinin yeni sürümlerini düzenli olarak yayınlar. Her kararlı sürümden önce bir veya daha fazla sürüm adayı (RC) sürümü vardır. RC sürümleri, yayın için tüm planlı özellikleri içerir, ancak yine de test ve doğrulama işlemleri yapılır. Yeni bir özelliği erken test etmek istiyorsanız bir RC sürümü yükleyebilir ve GitHub aracılığıyla geri bildirim sağlayabilirsiniz.

Sürüm adayı sürümleri, yayınların aynı numaralandırma kuralını izler, ancak **-RC** ve sonuna artımlı bir sayı eklenir. Sürüm adaylarını kararlı sürümlerle aynı [Azure IoT Edge yayınları](https://github.com/Azure/azure-iotedge/releases) listesinde görebilirsiniz. Örneğin, **1.0.9**önce gelen sürüm adaylarınızdan iki **1.0.9-RC5** ve **1.0.9-RC6**bulun. Ayrıca, RC sürümlerinin **yayın öncesi** etiketleriyle işaretlendiğini de görebilirsiniz.

IoT Edge Aracısı ve hub modülleri aynı kurala göre etiketlenmiş RC sürümlerine sahiptir. Örneğin, **MCR.Microsoft.com/azureiotedge-Hub:1.0.9-RC6**.

Önizleme olarak, sürüm adayı sürümleri, normal yükleyicilerin hedefi olan en son sürüm olarak dahil değildir. Bunun yerine, sınamak istediğiniz RC sürümü için varlıkları el ile hedefleyebilirsiniz. Çoğu bölüm için, bir RC sürümüne yükleme veya güncelleştirme, IoT Edge diğer belirli sürümlerini hedefleyecek şekilde aynıdır.

Bir IoT Edge cihazını güvenlik arka plan veya çalışma zamanı modüllerinin belirli bir sürümüne güncelleştirme hakkında bilgi edinmek için bu makaledeki bölümleri kullanın.

Yeni bir makineye IoT Edge yüklüyorsanız, cihaz işletim sisteminize bağlı olarak belirli bir sürümü yükleme hakkında bilgi edinmek için aşağıdaki bağlantıları kullanın:

* [Linux](how-to-install-iot-edge-linux.md#install-runtime-using-release-assets)
* [Windows](how-to-install-iot-edge-windows.md#offline-or-specific-version-installation)

## <a name="next-steps"></a>Sonraki adımlar

En son [Azure IoT Edge sürümlerini](https://github.com/Azure/azure-iotedge/releases)görüntüleyin.

[Nesnelerin interneti blogdaki](https://azure.microsoft.com/blog/topics/internet-of-things/) son güncelleştirmeler ve Duyurular ile güncel kalın
