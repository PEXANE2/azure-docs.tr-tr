---
title: Azure IoT Merkezi Gezgini'ni kullanarak aygıt bağlantısını izleme
description: Aygıt iletilerini izleyin ve IoT Central Explorer CLI aracılığıyla aygıt ikiz değişikliklerini gözlemleyin.
author: viv-liu
ms.author: viviali
ms.date: 03/27/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: 09209c21fe1b2b115c1ba6d6e00fcd0ee59a9393
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365416"
---
# <a name="monitor-device-connectivity-using-azure-cli"></a>Azure CLI'yı kullanarak cihaz bağlantısını izleme

*Bu konu oluşturucular ve yöneticiler için geçerlidir.*

Aygıtlarınızın IoT Central'a gönderdiği iletileri görmek ve aygıt ikizindeki değişiklikleri gözlemlemek için Azure CLI IoT uzantısını kullanın. Bu aracı, aygıt bağlantısının hata ayıklanması ve gözlemlemesi ve aygıt iletilerinin buluta ulaşmayan veya iki zıt değişikliklere yanıt veremeyeceği sorunları tanılamak için kullanabilirsiniz.

[Daha fazla ayrıntı için Azure CLI uzantıları başvurusunu ziyaret edin](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/central)

## <a name="prerequisites"></a>Ön koşullar

+ Azure CLI yüklendi ve sürüm 2.0.7 veya üstü. Çalıştırarak Azure CLI sürümünü `az --version`kontrol edin. [Azure CLI dokümanlarından](https://docs.microsoft.com/cli/azure/install-azure-cli) nasıl yükleyip güncelleştiringerektiğini öğrenin
+ Azure'daki bir iş veya okul hesabı, bir IoT Central uygulamasına kullanıcı olarak eklendi.

## <a name="install-the-iot-central-extension"></a>IoT Merkezi uzantısını yükleme

Yüklemek için komut satırınızdan aşağıdaki komutu çalıştırın:

```azurecli
az extension add --name azure-iot
```

Uzantının sürümünü çalıştırarak denetleyin:

```azurecli
az --version
```

Azure-iot uzantısı 0.8.1 veya daha yüksek olduğunu görmelisiniz. Değilse, çalıştırın:

```azurecli
az extension update --name azure-iot
```

## <a name="using-the-extension"></a>Uzantıyı kullanma

Aşağıdaki bölümlerde, çalıştırdığınızda `az iot central`kullanabileceğiniz ortak komutlar ve seçenekler açıklanabilir. Komutların ve seçeneklerin tam kümesini `--help` `az iot central` görüntülemek için, alt komutlarına veya alt komutlarından herhangi birini geçirin.

### <a name="login"></a>Oturum Aç

Azure CLI'de oturum açarak başlayın. 

```azurecli
az login
```

### <a name="get-the-application-id-of-your-iot-central-app"></a>IoT Central uygulamanızın Başvuru Kimliğini Alın
**Yönetim/Uygulama Ayarlarında,** **Uygulama Kimliğini**kopyalayın. Bu değeri sonraki adımlarda kullanırsınız.

### <a name="monitor-messages"></a>İletileri izleme
IoT Central uygulamanıza gönderilen iletileri cihazlarınızdan izleyin. Çıktı, tüm üstbilgi ve ek açıklamaları içerir.

```azurecli
az iot central app monitor-events --app-id <app-id> --properties all
```

### <a name="view-device-properties"></a>Aygıt özelliklerini görüntüleme
Belirli bir aygıtın geçerli okuma ve okuma/yazma aygıt özelliklerini görüntüleyin.

```azurecli
az iot central device-twin show --app-id <app-id> --device-id <device-id>
```

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Nasıl IoT Central Explorer kullanmayı öğrendim, önerilen bir sonraki adım [ioT Merkezi cihazları yönetme](howto-manage-devices.md)keşfetmektir.
