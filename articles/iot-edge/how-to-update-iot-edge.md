---
title: Güncelleştirme IOT Edge cihazları - Azure IOT Edge sürümünde | Microsoft Docs
description: IOT Edge cihazları, güvenlik daemon'ı ve IOT Edge çalışma zamanı en son sürümünü çalıştırma güncelleştirme
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/07/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4a7c27beeb7208efcf6687e49193c8d3b68f5300
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77186505"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme

IoT Edge hizmeti yeni sürümleri yayındıkça, en son özellikler ve güvenlik geliştirmeleri için IoT Edge cihazlarınızı güncellemek isteyeceksiniz. Bu makalede yeni bir sürümü kullanılabilir olduğunda, IOT Edge cihazlarınıza güncelleştirme hakkında bilgi sağlar.

IOT Edge cihazı iki bileşenden yeni bir sürüme taşımak istiyorsanız güncelleştirilmesi gerekir. Birincisi, cihazda çalışan ve cihaz başladığında çalışma zamanı modüllerini Başlatan güvenlik cini ' dir. Şu anda güvenlik arka plan programı CİHAZDAN yalnızca güncelleştirilebilir. İkinci bileşen, IoT Edge hub ve IoT Edge aracı modülleriyle oluşturulan çalışma zamanı. Dağıtımınızı nasıl yapısına bağlı olarak, çalışma zamanı CİHAZDAN veya uzaktan güncelleştirilebilir.

En son Azure IoT Edge sürümünü bulmak için, bkz. [Azure IoT Edge yayınlar](https://github.com/Azure/azure-iotedge/releases).

## <a name="update-the-security-daemon"></a>Güvenlik daemon'ı güncelleştirme

IOT Edge güvenlik arka plan programı, IOT Edge cihazında paket Yöneticisini kullanarak güncelleştirilmesi gereken yerel bir bileşenidir.

Komut `iotedge version`komutunu kullanarak cihazınızda çalışan güvenlik cini sürümünü denetleyin.

### <a name="linux-devices"></a>Linux cihazları

Linux x64 cihazlarda, güvenlik cini en son sürüme güncelleştirmek için apt-get veya uygun paket yöneticisini kullanın.

```bash
apt-get update
apt-get install libiothsm iotedge
```

Güvenlik arka plan programının belirli bir sürümüne güncelleştirmek istiyorsanız, [IoT Edge sürümlerden](https://github.com/Azure/azure-iotedge/releases)hedeflemek istediğiniz sürümü bulun. Bu sürümde, cihazınız için uygun **libiothsm-STD** ve **ıotedge** dosyalarını bulun. Her dosya için dosya bağlantısına sağ tıklayın ve bağlantı adresini kopyalayın. Bu bileşenlerin belirli sürümlerini yüklemek için bağlantı adresini kullanın:

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

Güvenlik arka plan programının belirli bir sürümüne güncelleştirmek istiyorsanız, [IoT Edge sürümlerden](https://github.com/Azure/azure-iotedge/releases)hedeflemek istediğiniz sürümü bulun. Bu sürümde, **Microsoft-Azure-IoTEdge. cab** dosyasını indirin. Sonra, yerel dosya konumuna işaret etmek için `-OfflineInstallationPath` parametresini kullanın. Örneğin:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux> -OfflineInstallationPath <absolute path to directory>
```

>[!NOTE]
>`-OfflineInstallationPath` parametresi, belirtilen dizinde **Microsoft-Azure-IoTEdge. cab** adlı bir dosya arar. IoT Edge Version 1.0.9-RC4 ile başlayarak, biri AMD64 cihaz ve diğeri ARM32 için kullanılabilecek iki. cab dosyası vardır. Cihazınız için doğru dosyayı indirin ve ardından mimari sonekini kaldırmak için dosyayı yeniden adlandırın.

Güncelleştirme seçenekleri hakkında daha fazla bilgi için, komut `Get-Help Update-IoTEdge -full` kullanın veya [tüm yükleme parametrelerine](how-to-install-iot-edge-windows.md#all-installation-parameters)başvurun.

## <a name="update-the-runtime-containers"></a>Çalışma zamanı kapsayıcılarındaki güncelleştir

IoT Edge aracısını ve IoT Edge hub kapsayıcılarını güncelleştirme yöntemi, dağıtımınızda (1,0 gibi) veya belirli Etiketler (örneğin, 1.0.7) kullanmanıza göre değişir.

IoT Edge aracısının sürümünü ve şu anda cihazınızda `iotedge logs edgeAgent` veya `iotedge logs edgeHub`komutlarını kullanarak IoT Edge hub modüllerini denetleyin.

  ![Günlüklerde kapsayıcı sürümü Bul](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>IOT Edge etiketleri anlama

IoT Edge Aracısı ve IoT Edge hub görüntüleri ilişkili oldukları IoT Edge sürümü ile etiketlenir. Çalışma zamanı görüntülerle etiketleri kullanmak için iki farklı yolu vardır:

* **Kayan Etiketler** -bu basamakla eşleşen en son görüntüyü almak için sürüm numarasının yalnızca ilk iki değerini kullanın. Örneğin, en son 1.0.x sürüme işaret edecek şekilde yeni bir sürüm olduğunda 1.0 güncelleştirilir. IOT Edge Cihazınızda kapsayıcı çalışma zamanı görüntü yeniden alınır, çalışma zamanı modülleri en son sürüme güncelleştirildi. Bu yaklaşım, geliştirme amacıyla önerilir. Etiketler alınıyor dağıtımların Azure portal varsayılan.

* **Belirli Etiketler** -görüntü sürümünü açıkça ayarlamak için sürüm numarasının tüm üç değerini kullanın. Örneğin, 1.0.7 ilk sürümünden sonra değişmez. Güncelleştirmeye hazır olduğunuzda dağıtım bildirimi içinde yeni bir sürüm numarası bildirebilirsiniz. Bu yaklaşım, üretim için önerilir.

### <a name="update-a-rolling-tag-image"></a>Sıralı bir etiket görüntüsünü güncelleştir

Dağıtımınızda sıralı Etiketler kullanıyorsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1,0**), cihazdaki kapsayıcı çalışma zamanını görüntünün en son sürümünü çekmek için zorlamanız gerekir.

IOT Edge cihazınızın yerel görüntü sürümü silin. Windows makinelerinde, güvenlik arka plan programının kaldırılması çalışma zamanı görüntülerini da kaldırır, bu nedenle bu adımı tekrar yapmanız gerekmez.

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

Görüntüleri kaldırmak için zorla `-f` bayrağını kullanmanız gerekebilir.

IOT Edge hizmetine en son sürümlerini çalışma zamanı görüntüleri çekin ve otomatik olarak bunları Cihazınızda yeniden başlatın.

### <a name="update-a-specific-tag-image"></a>Belirli bir etiketi görüntüsünü güncelleştir

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

## <a name="update-to-a-release-candidate-version"></a>Sürüm adayı sürümüne güncelleştirme

Azure IoT Edge, IoT Edge hizmetinin yeni sürümlerini düzenli olarak yayınlar. Her kararlı sürümden önce bir veya daha fazla sürüm adayı (RC) sürümü vardır. RC sürümleri, yayın için tüm planlı özellikleri içerir, ancak yine de test ve doğrulama işlemleri yapılır. Yeni bir özelliği erken test etmek istiyorsanız bir RC sürümü yükleyebilir ve GitHub aracılığıyla geri bildirim sağlayabilirsiniz.

Sürüm adayı sürümleri, yayınların aynı numaralandırma kuralını izler, ancak **-RC** ve sonuna artımlı bir sayı eklenir. Sürüm adaylarını kararlı sürümlerle aynı [Azure IoT Edge yayınları](https://github.com/Azure/azure-iotedge/releases) listesinde görebilirsiniz. Örneğin, **1.0.7**'den önce gelen iki sürüm adayı olan **1.0.7-RC1** ve **1.0.7-RC2**bulun. Ayrıca, RC sürümlerinin **yayın öncesi** etiketleriyle işaretlendiğini de görebilirsiniz.

IoT Edge Aracısı ve hub modülleri aynı kurala göre etiketlenmiş RC sürümlerine sahiptir. Örneğin, **MCR.Microsoft.com/azureiotedge-Hub:1.0.7-RC2**.

Önizleme olarak, sürüm adayı sürümleri, normal yükleyicilerin hedefi olan en son sürüm olarak dahil değildir. Bunun yerine, sınamak istediğiniz RC sürümü için varlıkları el ile hedefleyebilirsiniz. Çoğu bölümde, bir RC sürümüne yükleme veya güncelleştirme, Windows cihazlarına yönelik bir farklılık haricinde IoT Edge diğer belirli sürümlerini hedefleyerek aynıdır. 

Bir sürüm adayındaki bir Windows cihazında IoT Edge güvenlik arka plan programını yükleyip yönetmenize olanak tanıyan PowerShell betiği, genel olarak kullanılabilen en yeni sürümden farklı işlevlere sahip olabilir. RC için IoT Edge. cab dosyasını indirmenin yanı sıra, **ıotedgesecuritydaemon. ps1** betiğini de indirin. Geçerli kaynakta indirilen betiği çalıştırmak için [nokta](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7#script-scope-and-dot-sourcing) kaynağını kullanın. Örneğin: 

```powershell
. <path>\IoTEdgeSecurityDaemon.ps1
Update-IoTEdge -OfflineInstallationPath <path>
```

Bir IoT Edge cihazını güvenlik arka plan veya çalışma zamanı modüllerinin belirli bir sürümüne güncelleştirme hakkında bilgi edinmek için bu makaledeki bölümleri kullanın.

Yeni bir makineye IoT Edge yüklüyorsanız, cihaz işletim sisteminize bağlı olarak belirli bir sürümü yükleme hakkında bilgi edinmek için aşağıdaki bağlantıları kullanın:

* [Linux](how-to-install-iot-edge-linux.md#install-a-specific-runtime-version)
* [Windows](how-to-install-iot-edge-windows.md#offline-or-specific-version-installation)

## <a name="next-steps"></a>Sonraki adımlar

En son [Azure IoT Edge sürümlerini](https://github.com/Azure/azure-iotedge/releases)görüntüleyin.

[Nesnelerin interneti blogdaki](https://azure.microsoft.com/blog/topics/internet-of-things/) son güncelleştirmeler ve duyuru ile güncel kalın
