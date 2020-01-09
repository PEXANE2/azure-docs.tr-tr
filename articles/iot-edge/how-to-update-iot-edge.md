---
title: Güncelleştirme IOT Edge cihazları - Azure IOT Edge sürümünde | Microsoft Docs
description: IOT Edge cihazları, güvenlik daemon'ı ve IOT Edge çalışma zamanı en son sürümünü çalıştırma güncelleştirme
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/19/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 1e29420b8e029a7e1723f969753eb2081381a8e8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75434203"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme

IoT Edge hizmeti yeni sürümleri yayındıkça, en son özellikler ve güvenlik geliştirmeleri için IoT Edge cihazlarınızı güncellemek isteyeceksiniz. Bu makalede yeni bir sürümü kullanılabilir olduğunda, IOT Edge cihazlarınıza güncelleştirme hakkında bilgi sağlar.

IOT Edge cihazı iki bileşenden yeni bir sürüme taşımak istiyorsanız güncelleştirilmesi gerekir. Birincisi, cihazda çalışan ve cihaz başladığında çalışma zamanı modüllerini Başlatan güvenlik cini ' dir. Şu anda güvenlik arka plan programı CİHAZDAN yalnızca güncelleştirilebilir. İkinci bileşen, IoT Edge hub ve IoT Edge aracı modülleriyle oluşturulan çalışma zamanı. Dağıtımınızı nasıl yapısına bağlı olarak, çalışma zamanı CİHAZDAN veya uzaktan güncelleştirilebilir.

Azure IOT Edge en son sürümünü bulmak için bkz: [Azure IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases).

## <a name="update-the-security-daemon"></a>Güvenlik daemon'ı güncelleştirme

IOT Edge güvenlik arka plan programı, IOT Edge cihazında paket Yöneticisini kullanarak güncelleştirilmesi gereken yerel bir bileşenidir.

Komutunu kullanarak, Cihazınızda çalışan arka plan programı güvenlik sürümünü denetleme `iotedge version`.

### <a name="linux-devices"></a>Linux cihazları

Linux x64 cihazlarda, güvenlik cini 'nı güncelleştirmek için apt-get veya uygun paket yöneticisini kullanın.

```bash
apt-get update
apt-get install libiothsm iotedge
```

Linux ARM32 cihazlarında, güvenlik arka plan programının en son sürümünü yüklemek için [Linux 'ta Azure IoT Edge Runtime for Install (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) bölümündeki adımları kullanın.

### <a name="windows-devices"></a>Windows cihazları

Windows cihazlarında güvenlik cini 'nı güncelleştirmek için PowerShell betiğini kullanın. Komut dosyası otomatik olarak güvenlik arka plan programının en son sürümünü çeker.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

Update-ıotedge komutunun çalıştırılması, iki çalışma zamanı kapsayıcı görüntüsü ile birlikte cihazınızdan güvenlik cini kaldırır ve güncelleştirir. Config. YAML dosyası cihazda, ayrıca Moby kapsayıcı altyapısından (Windows kapsayıcıları kullanıyorsanız) veriler tutulur. Yapılandırma bilgilerinin tutulması, güncelleştirme işlemi sırasında cihazınız için bağlantı dizesini veya cihaz sağlama hizmeti bilgilerini yeniden sağlamanız gerekmediği anlamına gelir.

Güvenlik arka plan programının belirli bir sürümünü yüklemek istiyorsanız, [IoT Edge sürümlerden](https://github.com/Azure/azure-iotedge/releases)uygun Microsoft-Azure-IoTEdge. cab dosyasını indirin. Sonra, dosya konumuna işaret etmek için `-OfflineInstallationPath` parametresini kullanın. Daha fazla bilgi için bkz. [çevrimdışı yükleme](how-to-install-iot-edge-windows.md#offline-installation).

## <a name="update-the-runtime-containers"></a>Çalışma zamanı kapsayıcılarındaki güncelleştir

IoT Edge aracısını ve IoT Edge hub kapsayıcılarını güncelleştirme yöntemi, dağıtımınızda (1,0 gibi) veya belirli Etiketler (örneğin, 1.0.7) kullanmanıza göre değişir.

IoT Edge aracısının sürümünü ve şu anda cihazınızda `iotedge logs edgeAgent` veya `iotedge logs edgeHub`komutlarını kullanarak IoT Edge hub modüllerini denetleyin.

  ![Günlüklerde kapsayıcı sürümü Bul](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>IOT Edge etiketleri anlama

IoT Edge Aracısı ve IoT Edge hub görüntüleri ilişkili oldukları IoT Edge sürümü ile etiketlenir. Çalışma zamanı görüntülerle etiketleri kullanmak için iki farklı yolu vardır:

* **Etiketler alınıyor** -yalnızca ilk iki değeri sürüm numarasının basamaklar eşleşen en son görüntü almak için kullanın. Örneğin, en son 1.0.x sürüme işaret edecek şekilde yeni bir sürüm olduğunda 1.0 güncelleştirilir. IOT Edge Cihazınızda kapsayıcı çalışma zamanı görüntü yeniden alınır, çalışma zamanı modülleri en son sürüme güncelleştirildi. Bu yaklaşım, geliştirme amacıyla önerilir. Etiketler alınıyor dağıtımların Azure portal varsayılan.

* **Belirli etiketlere** -görüntü sürümü açıkça ayarlamak için sürüm numarası, tüm üç değerleri kullanın. Örneğin, 1.0.7 ilk sürümünden sonra değişmez. Güncelleştirmeye hazır olduğunuzda dağıtım bildirimi içinde yeni bir sürüm numarası bildirebilirsiniz. Bu yaklaşım, üretim için önerilir.

### <a name="update-a-rolling-tag-image"></a>Sıralı bir etiket görüntüsünü güncelleştir

Dağıtımınızda çalışırken etiketleri kullanıyorsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1.0**) kapsayıcı çalışma zamanı görüntüsünün son sürümünü çekmek için Cihazınızda zorlama ayarını yapmanız gerekir.

IOT Edge cihazınızın yerel görüntü sürümü silin. Windows makinelerinde, güvenlik arka plan programının kaldırılması çalışma zamanı görüntülerini da kaldırır, bu nedenle bu adımı tekrar yapmanız gerekmez.

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

Zorla kullanmanız gerekebilir `-f` görüntülerini kaldırmak için bayrak.

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

Azure IoT Edge, IoT Edge hizmetinin yeni sürümlerini düzenli olarak yayınlar. Her kararlı sürümden önce bir veya daha fazla sürüm adayı (RC) sürümü vardır. RC sürümleri yayın için tüm planlı özellikleri içerir, ancak yine de kararlı bir sürüm için gereken test ve doğrulama işlemleriyle devam eder. Yeni bir özelliği erken test etmek istiyorsanız RC sürümünü yükleyebilir ve GitHub aracılığıyla geri bildirim sağlayabilirsiniz.

Sürüm adayı sürümleri, yayınların aynı numaralandırma kuralını izler, ancak **-RC** ve sonuna artımlı bir sayı eklenir. Sürüm adaylarını kararlı sürümlerle aynı [Azure IoT Edge yayınları](https://github.com/Azure/azure-iotedge/releases) listesinde görebilirsiniz. Örneğin, **1.0.7**'den önce gelen iki sürüm adayı olan **1.0.7-RC1** ve **1.0.7-RC2**bulun. Ayrıca, RC sürümlerinin **yayın öncesi** etiketleriyle işaretlendiğini de görebilirsiniz.

Önizleme olarak, sürüm adayı sürümleri, normal yükleyicilerin hedefi olan en son sürüm olarak dahil değildir. Bunun yerine, sınamak istediğiniz RC sürümü için varlıkları el ile hedefleyebilirsiniz. IoT Edge cihaz işletim sisteminize bağlı olarak, IoT Edge belirli bir sürüme güncelleştirmek için aşağıdaki bölümleri kullanın:

* [Linux](how-to-install-iot-edge-linux.md#install-a-specific-runtime-version)
* [Windows](how-to-install-iot-edge-windows.md#offline-installation)

## <a name="next-steps"></a>Sonraki adımlar

En son görüntülemek [Azure IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases).

[Nesnelerin interneti blogdaki](https://azure.microsoft.com/blog/topics/internet-of-things/) son güncelleştirmeler ve duyuru ile güncel kalın