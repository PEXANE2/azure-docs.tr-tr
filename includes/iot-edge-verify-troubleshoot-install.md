---
title: Yükleme ve sağlamaktan sonra başarıyı doğrulama ve sorun giderme
description: dosya dahil etme
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 10/06/2020
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 295769e5d4924e529b296dbb0b9d405ee197c1db
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91979886"
---
## <a name="verify-successful-setup"></a>Kurulumun başarılı olduğunu doğrulama

IoT Edge hizmetinin durumunu kontrol edin. Çalışıyor olarak listelenmelidir.  

# <a name="linux"></a>[Linux](#tab/linux)

```bash
systemctl status iotedge
```

# <a name="windows"></a>[Windows](#tab/windows)

```powershell
Get-Service iotedge
```

---

Hizmet günlüklerini inceleyin.

# <a name="linux"></a>[Linux](#tab/linux)

```bash
journalctl -u iotedge --no-pager --no-full
```

# <a name="windows"></a>[Windows](#tab/windows)

IoT Edge çalışma zamanını yüklemeyi tamamladıysanız, çalıştırılan **dağıtım-ıotedge** ve **Initialize-ıotedge**arasındaki zamandan oluşan hataların bir listesini görebilirsiniz. Hizmetin yapılandırmadan önce başlatılmaya çalıştığı için bu hatalar beklenmektedir.

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

---

En yaygın yapılandırma ve ağ hatalarını denetlemek için [sorun giderme aracını](../articles/iot-edge/troubleshoot.md#run-the-check-command) çalıştırın.

```powershell
iotedge check
```

Cihazınızda IoT Edge için ilk modülünüzü dağıtana kadar, **$edgeHub** sistem modülü cihaza dağıtılmayacak. Sonuç olarak, otomatik denetim bağlantı denetimi için bir hata döndürür `Edge Hub can bind to ports on host` . Bu hata, cihaza bir modül dağıttıktan sonra gerçekleşmediği takdirde yoksayılabilir.

Son olarak, çalışan modülleri listeleyin:

```powershell
iotedge list
```

Yeni bir yüklemeden sonra, çalıştırmayı görmeniz gereken tek modül **Edgeagent**' dir.

## <a name="tips-and-troubleshooting"></a>İpuçları ve sorun giderme

Kaynak kısıtlı cihazlarda, [sorun giderme kılavuzunda](../articles/iot-edge/troubleshoot.md)her yönerge Için *Optimizeforperformance* ortam değişkenini *false* olarak ayarlamanız kesinlikle önerilir.

Cihazınız IoT Hub bağlanamaz ve ağınızda bir ara sunucu varsa, [proxy sunucusu üzerinden iletişim kurmak için IoT Edge cihazınızı yapılandırma](../articles/iot-edge/how-to-configure-proxy-support.md)bölümündeki adımları izleyin.

# <a name="linux"></a>[Linux](#tab/linux)

Linux cihazlarda komutları çalıştırmak için yükseltilmiş ayrıcalıklara sahip olmanız gerekir `iotedge` . Çalışma zamanını yükledikten sonra, makinenizde oturumunuzu kapatın ve izinlerinizi otomatik olarak güncelleştirmek için yeniden oturum açın. Bundan sonra, `sudo` komutları yükseltilmiş ayrıcalıklarla çalıştırmak için kullanın.

# <a name="windows"></a>[Windows](#tab/windows)

Windows kapsayıcıları çalıştıran Windows cihazlarında, Moby kapsayıcı altyapısı IoT Edge bir parçası olarak yüklenmiştir. Moby altyapısı, Docker Desktop ile paralel olarak çalışacak şekilde tasarlanmıştır. `docker`Cihazınızdaki kapsayıcılarla doğrudan etkileşim kurmak istiyorsanız komutları kullanabilirsiniz. Ancak, Docker Desktop 'ın cihaza de yüklenmesi durumunda Moby altyapısını özellikle hedeflemelidir.

Örneğin, tüm Docker görüntülerini listelemek için aşağıdaki komutu kullanın:

```powershell
docker images
```

Tüm Moby görüntülerini listelemek için, aynı komutu Moby altyapısına yönelik bir işaretçi ile değiştirin:

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

Altyapı URI 'SI, yükleme komut dosyasının çıktısında listelenir veya bunu config. YAML dosyasının kapsayıcı çalışma zamanı ayarları bölümünde bulabilirsiniz.

![config. YAML içinde moby_runtime URI 'si](./media/iot-edge-verify-troubleshoot-install/moby-runtime-uri.png)

---
