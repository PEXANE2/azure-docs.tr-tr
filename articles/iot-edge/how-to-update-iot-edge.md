---
title: Güncelleştirme IOT Edge cihazları - Azure IOT Edge sürümünde | Microsoft Docs
description: IOT Edge cihazları, güvenlik daemon'ı ve IOT Edge çalışma zamanı en son sürümünü çalıştırma güncelleştirme
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/27/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 58171d2fc8e7e1563bd83689d1cd91c55fea239a
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456553"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme

IoT Edge hizmeti yeni sürümleri yayındıkça, en son özellikler ve güvenlik geliştirmeleri için IoT Edge cihazlarınızı güncellemek isteyeceksiniz. Bu makalede yeni bir sürümü kullanılabilir olduğunda, IOT Edge cihazlarınıza güncelleştirme hakkında bilgi sağlar. 

IOT Edge cihazı iki bileşenden yeni bir sürüme taşımak istiyorsanız güncelleştirilmesi gerekir. Birincisi, cihazda çalışan ve cihaz başladığında çalışma zamanı modüllerini Başlatan güvenlik cini ' dir. Şu anda güvenlik arka plan programı CİHAZDAN yalnızca güncelleştirilebilir. İkinci bileşen, IoT Edge hub ve IoT Edge aracı modülleriyle oluşturulan çalışma zamanı. Dağıtımınızı nasıl yapısına bağlı olarak, çalışma zamanı CİHAZDAN veya uzaktan güncelleştirilebilir. 

En son Azure IoT Edge sürümünü bulmak için, bkz. [Azure IoT Edge yayınlar](https://github.com/Azure/azure-iotedge/releases).

## <a name="update-the-security-daemon"></a>Güvenlik daemon'ı güncelleştirme

IOT Edge güvenlik arka plan programı, IOT Edge cihazında paket Yöneticisini kullanarak güncelleştirilmesi gereken yerel bir bileşenidir. 

Komut `iotedge version`komutunu kullanarak cihazınızda çalışan güvenlik cini sürümünü denetleyin. 

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

Update-ıotedge komutunun çalıştırılması, iki çalışma zamanı kapsayıcı görüntüsü ile birlikte cihazınızdan güvenlik arka plan programını kaldırır. Config. YAML dosyası cihazda, ayrıca Moby kapsayıcı altyapısından (Windows kapsayıcıları kullanıyorsanız) veriler tutulur. Yapılandırma bilgilerinin tutulması, güncelleştirme işlemi sırasında cihazınız için bağlantı dizesini veya cihaz sağlama hizmeti bilgilerini yeniden sağlamanız gerekmediği anlamına gelir. 

Güvenlik arka plan programının belirli bir sürümünü yüklemek istiyorsanız, [IoT Edge sürümlerden](https://github.com/Azure/azure-iotedge/releases)uygun Microsoft-Azure-IoTEdge. cab dosyasını indirin. Sonra, dosya konumuna işaret etmek için `-OfflineInstallationPath` parametresini kullanın. Daha fazla bilgi için bkz. [çevrimdışı yükleme](how-to-install-iot-edge-windows.md#offline-installation).

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

Dağıtımınızda belirli Etiketler kullanırsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1.0.7**), tüm yapmanız gereken, dağıtım bildiriminizde etiketi güncelleştirin ve değişiklikleri cihazınıza uygular. 

Azure portal çalışma zamanı dağıtım görüntüleri, **Gelişmiş uç çalışma zamanı ayarlarını yapılandır** bölümünde belirtilir. 

![Gelişmiş Edge çalışma zamanı ayarlarını yapılandırma](./media/how-to-update-iot-edge/configure-runtime.png)

JSON dağıtım bildiriminde, **Systemmodules** bölümündeki modül görüntülerini güncelleştirin. 

```json
"systemModules": {
  "edgeAgent": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-agent:1.0.7",
      "createOptions": ""
    }
  },
  "edgeHub": {
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0.7",
      "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
    }
  }
},
```

## <a name="update-to-a-release-candidate-version"></a>Sürüm adayı sürümüne güncelleştirme

Azure IoT Edge, IoT Edge hizmetinin yeni sürümlerini düzenli olarak yayınlar. Her kararlı sürümden önce bir veya daha fazla sürüm adayı (RC) sürümü vardır. RC sürümleri yayın için tüm planlı özellikleri içerir, ancak yine de kararlı bir sürüm için gereken test ve doğrulama işlemleriyle devam eder. Yeni bir özelliği erken test etmek istiyorsanız RC sürümünü yükleyebilir ve GitHub aracılığıyla geri bildirim sağlayabilirsiniz. 

Sürüm adayı sürümleri, yayınların aynı numaralandırma kuralını izler, ancak **-RC** ve sonuna artımlı bir sayı eklenir. Sürüm adaylarını kararlı sürümlerle aynı [Azure IoT Edge yayınları](https://github.com/Azure/azure-iotedge/releases) listesinde görebilirsiniz. Örneğin, **1.0.7**'den önce gelen iki sürüm adayı olan **1.0.7-RC1** ve **1.0.7-RC2**bulun. Ayrıca, RC sürümlerinin **yayın öncesi** etiketleriyle işaretlendiğini de görebilirsiniz. 

Önizleme olarak, sürüm adayı sürümleri, normal yükleyicilerin hedefi olan en son sürüm olarak dahil değildir. Bunun yerine, sınamak istediğiniz RC sürümü için varlıkları el ile hedefleyebilirsiniz. IoT Edge cihaz işletim sisteminize bağlı olarak, IoT Edge belirli bir sürüme güncelleştirmek için aşağıdaki bölümleri kullanın:

* [Linux](how-to-install-iot-edge-linux.md#install-a-specific-runtime-version)
* [Windows](how-to-install-iot-edge-windows.md#offline-installation)

## <a name="next-steps"></a>Sonraki adımlar

En son [Azure IoT Edge sürümlerini](https://github.com/Azure/azure-iotedge/releases)görüntüleyin.

[Nesnelerin interneti blogdaki](https://azure.microsoft.com/blog/topics/internet-of-things/) son güncelleştirmeler ve duyuru ile güncel kalın 