---
title: Cihazlarda IoT Edge sürümünü güncelle - Azure IoT Edge | Microsoft Dokümanlar
description: Güvenlik daemon ve IoT Edge çalışma zamanının en son sürümlerini çalıştırmak için IoT Edge aygıtları nasıl güncellenir?
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/08/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ce69593c1df0039d64f89e79124af1150409eff7
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113313"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>IoT Edge güvenlik daemon'ını ve çalışma zamanını güncelleştirme

IoT Edge hizmeti yeni sürümler yayımlanınca, IoT Edge aygıtlarınızı en son özellikler ve güvenlik iyileştirmeleri için güncelleştirmek istersiniz. Bu makalede, yeni bir sürüm kullanılabilir olduğunda IoT Edge aygıtlarınızı nasıl güncelleştireceğimiz hakkında bilgi verilmektedir.

Daha yeni bir sürüme geçmek istiyorsanız, Bir IoT Edge aygıtının iki bileşeninin güncellenmesi gerekir. Bunlardan ilki, aygıtta çalışan ve aygıt başlatıldığında çalışma zamanı modüllerini başlatan güvenlik daemonudur. Şu anda, güvenlik daemon yalnızca cihazın kendisinden güncelleştirilebilir. İkinci bileşen, IoT Edge hub'ı ve IoT Edge aracı modüllerinden oluşan çalışma zamanıdır. Dağıtımınızı nasıl yapılandırdığınıza bağlı olarak, çalışma süresi aygıttan veya uzaktan güncellenebilir.

Azure IoT Edge'in en son sürümünü bulmak için [Azure IoT Edge sürümlerine](https://github.com/Azure/azure-iotedge/releases)bakın.

## <a name="update-the-security-daemon"></a>Güvenlik daemon'u güncelleştir

IoT Edge güvenlik daemon IoT Edge aygıtında paket yöneticisi kullanılarak güncellenmesi gereken yerel bir bileşenidir.

Cihazınızda çalışan güvenlik daemonunun sürümünü komutu `iotedge version`kullanarak kontrol edin.

### <a name="linux-devices"></a>Linux cihazları

Linux x64 cihazlarında, güvenlik daemonunu en son sürüme güncellemek için apt-get'i veya uygun paket yöneticinizi kullanın.

```bash
apt-get update
apt-get install libiothsm iotedge
```

Güvenlik daemon'unun belirli bir sürümüne güncelleştirmek istiyorsanız, [IoT Edge sürümlerinden](https://github.com/Azure/azure-iotedge/releases)hedeflemek istediğiniz sürümü bulun. Bu sürümde, cihazınız için uygun **libiothsm-std** ve **iotedge** dosyalarını bulun. Her dosya için dosya bağlantısını sağ tıklatın ve bağlantı adresini kopyalayın. Bu bileşenlerin belirli sürümlerini yüklemek için bağlantı adresini kullanın:

```bash
curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
```

### <a name="windows-devices"></a>Windows cihazları

Windows aygıtlarında, güvenlik daemon'unu güncelleştirmek için PowerShell komut dosyasını kullanın. Komut dosyası, güvenlik daemon'unun en son sürümünü otomatik olarak çeker.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

Update-IoTEdge komutunu çalıştırmak, iki çalışma zamanı kapsayıcı görüntüsüyle birlikte güvenlik daemonu'nu aygıtınızdan kaldırır ve güncelleştirir. Config.yaml dosyası cihazda ve Moby konteyner motorundan gelen verilerde tutulur (Windows kapsayıcıları kullanıyorsanız). Yapılandırma bilgilerini saklamak, güncelleştirme işlemi sırasında aygıtınız için bağlantı dizesi veya Aygıt Sağlama Hizmeti bilgilerini yeniden sağlamanız gerekmediğiniz anlamına gelir.

Güvenlik daemon'unun belirli bir sürümüne güncelleştirmek istiyorsanız, [IoT Edge sürümlerinden](https://github.com/Azure/azure-iotedge/releases)hedeflemek istediğiniz sürümü bulun. Bu sürümde **Microsoft-Azure-IoTEdge.cab** dosyasını indirin. Ardından, yerel `-OfflineInstallationPath` dosya konumunu işaret etmek için parametreyi kullanın. Örneğin:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux> -OfflineInstallationPath <absolute path to directory>
```

>[!NOTE]
>Parametre, `-OfflineInstallationPath` sağlanan dizinde **Microsoft-Azure-IoTEdge.cab** adlı bir dosya arar. IoT Edge sürüm 1.0.9-rc4 ile başlayarak, biri AMD64 aygıtları ve diğeri ARM32 için olmak üzere iki adet .cab dosyası kullanılabilir. Aygıtınız için doğru dosyayı indirin ve ardından mimari soneki kaldırmak için dosyayı yeniden adlandırın.

Güncelleştirme seçenekleri hakkında daha fazla `Get-Help Update-IoTEdge -full` bilgi için komutu kullanın veya [tüm yükleme parametrelerine](how-to-install-iot-edge-windows.md#all-installation-parameters)bakın.

## <a name="update-the-runtime-containers"></a>Çalışma zamanı kapsayıcılarını güncelleştirme

IoT Edge aracısını ve IoT Edge hub kapsayıcılarını güncelleştirme şekliniz, dağıtımınızda yuvarlanma etiketleri (1,0 gibi) veya belirli etiketleri (1,0,7 gibi) kullanıp kullanmadığınıza bağlıdır.

Komutları kullanarak cihazınızda şu anda ioT Edge aracısı ve IoT Edge hub modüllerinin `iotedge logs edgeAgent` sürümünü kontrol edin veya `iotedge logs edgeHub`.

  ![Günlüklerde kapsayıcı sürümünü bulma](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>IoT Edge etiketlerini anlayın

IoT Edge aracısı ve IoT Edge hub görüntüleri, ilişkili oldukları IoT Edge sürümüyle etiketlenir. Çalışma zamanı görüntüleriyle etiketleri kullanmanın iki farklı yolu vardır:

* **Rolling etiketleri** - Bu basamaklarla eşleşen en son görüntüyü elde etmek için sürüm numarasının yalnızca ilk iki değerini kullanın. Örneğin, 1.0 en son 1.0.x sürümüne işaret etmek için yeni bir sürüm olduğunda güncelleştirilir. IoT Edge aygıtınızdaki kapsayıcı çalışma süresi görüntüyü yeniden çekerse, çalışma zamanı modülleri en son sürüme güncelleştirilir. Bu yaklaşım geliştirme amacıyla önerilmektedir. Azure portalındaki dağıtımlar varsayılan olarak yuvarlanan etiketlere.

* **Belirli etiketler** - Resim sürümünü açıkça ayarlamak için sürüm numarasının üç değerini de kullanın. Örneğin, 1.0.7 ilk sürümünden sonra değişmez. Güncelleştirmeye hazır olduğunuzda dağıtım bildiriminde yeni bir sürüm numarası bildirebilirsiniz. Bu yaklaşım üretim amaçlı önerilmektedir.

### <a name="update-a-rolling-tag-image"></a>Yuvarlanan etiket görüntüsünü güncelleştir

Dağıtımınızda yuvarlanan etiketlerkullanıyorsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1.0**) sonra, aygıtınızdaki kapsayıcı çalışma süresini görüntünün en son sürümünü çekmeye zorlamanız gerekir.

IoT Edge aygıtınızdan resmin yerel sürümünü silin. Windows makinelerde, güvenlik daemon kaldırma da çalışma zamanı görüntüleri kaldırır, böylece bu adımı yeniden almak gerekmez.

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

Görüntüleri kaldırmak için kuvvet `-f` bayrağını kullanmanız gerekebilir.

IoT Edge hizmeti, çalışma zamanı görüntülerinin en son sürümlerini çeker ve otomatik olarak cihazınızda yeniden başlatacaktır.

### <a name="update-a-specific-tag-image"></a>Belirli bir etiket görüntüsünü güncelleştirme

Dağıtımınızda belirli etiketler kullanıyorsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1.0.8**) tek yapmanız gereken dağıtım bildiriminizdeki etiketi güncelleştirmek ve değişiklikleri cihazınıza uygulamaktır.

1. Azure portalındaki IoT Hub'ında IoT Edge cihazınızı seçin ve **Modülleri Ayarla'yı**seçin.

1. **IoT Edge Modülleri** bölümünde **Çalışma Zamanı Ayarları'nı**seçin.

   ![Çalışma zamanı ayarlarını yapılandırma](./media/how-to-update-iot-edge/configure-runtime.png)

1. **Çalışma Zamanı Ayarları'nda**Edge **Hub'ın** **Görüntü** değerini istenen sürümle güncelleyin. Hemen **Kaydet'i** seçmeyin.

   ![Kenar Hub'ı Görüntüsünü Güncelleştir](./media/how-to-update-iot-edge/runtime-settings-edgehub.png)

1. Kenar **Hub** ayarlarını daraltın veya aşağı kaydırın ve **Edge Agent** için **Görüntü** değerini istenen sürümle güncelleştirin.

   ![Kenar Hub Aracısı sürümünü güncelleştirme](./media/how-to-update-iot-edge/runtime-settings-edgeagent.png)

1. **Kaydet'i**seçin.

1. **Gözden Geçir + oluştur'** seçeneğini seçin, dağıtımı gözden geçirin ve **Oluştur'u**seçin.

## <a name="update-offline-or-to-a-specific-version"></a>Çevrimdışı veya belirli bir sürüme güncelleştirme

Bir aygıtı çevrimdışı güncelleştirmek veya en son sürüm yerine IoT Edge'in belirli bir sürümüne `-OfflineInstallationPath` güncelleştirmek istiyorsanız, bunu parametreyle yapabilirsiniz.

Bir IoT Edge aygıtını güncelleştirmek için iki bileşen kullanılır:

* Yükleme yönergelerini içeren bir PowerShell komut dosyası
* IoT Edge güvenlik daemon (iotedged), Moby konteyner motoru ve Moby CLI içeren Microsoft Azure IoT Edge kabini

1. Önceki sürümlerle birlikte en son IoT Edge yükleme dosyaları için [Azure IoT Edge sürümlerine](https://github.com/Azure/azure-iotedge/releases)bakın.

2. Yüklemek istediğiniz sürümü bulun ve sürüm notlarının **Varlıklar** bölümünden Aşağıdaki dosyaları IoT aygıtınıza indirin:

   * IoTEdgeSecurityDaemon.ps1
   * 1.0.9 veya daha yeni sürümlerden Microsoft-Azure-IoTEdge-amd64.cab veya 1.0.8 ve üzeri sürümlerden Microsoft-Azure-IoTEdge.cab.

   Microsoft-Azure-IotEdge-arm32.cab, yalnızca test amacıyla 1.0.9'dan itibaren de kullanılabilir. IoT Edge şu anda Windows ARM32 aygıtlarında desteklenmemektedir.

   PowerShell komut dosyasını kullandığınız .cab dosyasıyla aynı sürümden kullanmanız önemlidir, çünkü işlevsellik her sürümdeki özellikleri destekleyecek şekilde değişir.

3. İndirdiğiniz .cab dosyasında mimari sonek varsa, dosyayı sadece **Microsoft-Azure-IoTEdge.cab**olarak yeniden adlandırın.

4. Çevrimdışı bileşenlerle güncelleştirmek için [PowerShell](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7#script-scope-and-dot-sourcing) komut dosyasının yerel kopyasını nokta kaynağı na göre yapın. Ardından, komutun `-OfflineInstallationPath` bir `Update-IoTEdge` parçası olarak parametreyi kullanın ve dosya dizinine mutlak yolu sağlayın. Örneğin,

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Update-IoTEdge -OfflineInstallationPath <path>
   ```

## <a name="update-to-a-release-candidate-version"></a>Sürüm adayı sürümüne güncelleştirme

Azure IoT Edge, IoT Edge hizmetinin yeni sürümlerini düzenli olarak yayımlar. Her kararlı sürümden önce, bir veya daha fazla sürüm adayı (RC) sürümü vardır. RC sürümleri sürüm için planlanan tüm özellikleri içerir, ancak yine de sınama ve doğrulama yoluyla devam etmektedir. Yeni bir özelliği erken test etmek istiyorsanız, bir RC sürümü yükleyebilir ve GitHub üzerinden geri bildirim sağlayabilirsiniz.

Sürüm adayı sürümleri, aynı numaralandırma sözleşmesini izler, ancak sonuna eklenen **-rc** artı artımlı bir sayıya sahiptir. Sürüm adaylarını, kararlı sürümler ile aynı Azure [IoT Edge sürümleri](https://github.com/Azure/azure-iotedge/releases) listesinde görebilirsiniz. Örneğin, **1.0.7-rc1 ve 1.0.7-rc2 , 1.0.7** önce gelen iki sürüm adayları bulmak . **1.0.7-rc2** **1.0.7** Ayrıca, RC sürümlerinin yayın **öncesi** etiketlerle işaretlendiğini de görebilirsiniz.

IoT Edge aracısı ve hub modülleri, aynı kuralla etiketlenmiş RC sürümlerine sahiptir. Örneğin, **mcr.microsoft.com/azureiotedge-hub:1.0.7-rc2.**

Önizlemeler olarak, sürüm adayı sürümleri normal yükleyicilerin hedef aldığı en son sürüm olarak dahil değildir. Bunun yerine, test etmek istediğiniz RC sürümü için varlıkları el ile hedeflemeniz gerekir. Çoğunlukla, bir RC sürümüne yükleme veya güncelleme, IoT Edge'in diğer belirli bir sürümünü hedeflemeyle aynıdır.

Bir IoT Edge aygıtını güvenlik daemonunun veya çalışma zamanı modüllerinin belirli bir sürümüne nasıl güncelleştireceğimi öğrenmek için bu makaledeki bölümleri kullanın.

IoT Edge'i yeni bir makineye yüklüyorsanız, cihazınızın işletim sistemine bağlı olarak belirli bir sürümü nasıl yükleyeceğimize ilişkin öğrenmek için aşağıdaki bağlantıları kullanın:

* [Linux](how-to-install-iot-edge-linux.md#install-a-specific-runtime-version)
* [Windows](how-to-install-iot-edge-windows.md#offline-or-specific-version-installation)

## <a name="next-steps"></a>Sonraki adımlar

En son [Azure IoT Edge sürümlerini](https://github.com/Azure/azure-iotedge/releases)görüntüleyin.

[Nesnelerin İnterneti blogunda](https://azure.microsoft.com/blog/topics/internet-of-things/) son güncellemeler ve duyurulardan haberdar olun
