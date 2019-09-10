---
title: Azure IoT Central gezginini kullanarak cihaz bağlantısını izleme
description: Cihaz iletilerini izleyin ve IoT Central Explorer CLı aracılığıyla cihaz ikizi değişikliklerini gözlemleyin.
author: viv-liu
ms.author: viviali
ms.date: 06/17/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 38cbe43e9038a47c4e222fd4744f0b844f9ddb4e
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70845693"
---
# <a name="monitor-device-connectivity-using-the-azure-iot-central-explorer"></a>Azure IoT Central gezginini kullanarak cihaz bağlantısını izleme

*Bu konu, oluşturucular ve yöneticiler için geçerlidir.*

Cihazlarınızın IoT Central gönderdiği iletileri görmek ve IoT Hub ikizi değişiklikleri gözlemlemek için IoT Central Explorer CLı kullanın. Bu açık kaynaklı Aracı, cihaz bağlantısı durumu hakkında ayrıntılı bilgi edinmek ve ikizi değişikliklere yanıt vermeyen buluta veya cihazlara ulaşmayan cihaz iletilerinin sorunlarını tanılamak için kullanabilirsiniz.

[GitHub 'da ıotc-Explorer deposunu ziyaret edin.](https://aka.ms/iotciotcexplorercligithub)

## <a name="prerequisites"></a>Önkoşullar

+ Node. js sürüm 8. x veya üzeri- https://nodejs.org
+ Uygulamanızın Yöneticisi, ıotc-Explorer 'da kullanabilmeniz için bir erişim belirteci üretmelidir

## <a name="install-iotc-explorer"></a>İotc-Explorer 'ı yükler

Yüklemek için komut satırınızdan aşağıdaki komutu çalıştırın:

```cmd/sh
npm install -g iotc-explorer
```

> [!NOTE]
> Genellikle Install komutunu `sudo` UNIX benzeri ortamlarda çalıştırmanız gerekir.

## <a name="run-iotc-explorer"></a>İotc-Explorer Çalıştır

Aşağıdaki bölümlerde, çalıştırdığınızda `iotc-explorer`kullanabileceğiniz ortak komutlar ve seçenekler açıklanır. Komutların ve seçeneklerin tam kümesini görüntülemek için alt komutlarına `--help` `iotc-explorer` veya herhangi birine geçiş yapın.

### <a name="login"></a>Oturum aç

Çalışmaya başlamadan önce, kullanmanız için bir erişim belirteci almak üzere IoT Central uygulamanızın yöneticisine sahip olmanız gerekir. Yönetici aşağıdaki adımları gerçekleştirir:

1. **Yönetim** ' e gidin ve **belirteçlere erişin**.
1. **Belirteç oluştur**' u seçin.
    ![Erişim belirteci sayfası ekran görüntüsü](media/howto-use-iotc-explorer/accesstokenspage.png)

1. Bir belirteç adı girin, **İleri**' yi ve ardından **Kopyala**' yı seçin.
    > [!NOTE]
    > Belirteç değeri yalnızca bir kez gösteriliyor, bu nedenle iletişim kutusu kapatılmadan önce kopyalanmalıdır. İletişim kutusunu kapattıktan sonra hiç bir daha gösterilmemiştir.

    ![Erişim belirteci Kopyala iletişim kutusu ekran görüntüsü](media/howto-use-iotc-explorer/copyaccesstoken.png)

Aşağıdaki gibi, CLı 'da oturum açmak için belirtecini kullanabilirsiniz:

```cmd/sh
iotc-explorer login "<Token value>"
```

Kabuk geçmişinizde belirtecin kalıcı olmasını tercih ediyorsanız, belirteci dışarı bırakabilir ve istendiğinde şunları yapabilirsiniz:

```cmd/sh
iotc-explorer login
```

### <a name="monitor-device-messages"></a>Cihaz iletilerini izleme

`monitor-messages` Komutunu kullanarak, belirli bir cihazdan ya da uygulamanızdaki tüm cihazlardan gelen iletileri izleyebilirsiniz. Bu komut, geldikçe yeni iletileri sürekli çıkaran bir izleyici başlatır:

Uygulamanızdaki tüm cihazları izlemek için aşağıdaki komutu çalıştırın:

```cmd/sh
iotc-explorer monitor-messages
```

Çıktı:

![Monitor-messages komut çıktısı](media/howto-use-iotc-explorer/monitormessages.png)

Belirli bir cihazı izlemek için, yalnızca komutun sonuna cihaz kimliği ekleyin:

```cmd/sh
iotc-explorer monitor-messages <your-device-id>
```

Komutuna `--raw` seçeneği ekleyerek daha fazla makineye yönelik bir biçim de yazabilirsiniz:

```cmd/sh
iotc-explorer monitor-messages --raw
```

### <a name="get-device-twin"></a>Cihaz ikizi al

Bir IoT Central cihazının ikizi `get-twin` içeriğini almak için komutunu kullanabilirsiniz. Bunu yapmak için aşağıdaki komutu çalıştırın:

```cmd/sh
iotc-explorer get-twin <your-device-id>
```

Çıktı:

![Get-ikizi komutu çıkışı](media/howto-use-iotc-explorer/getdevicetwin.png)

İle `monitor-messages`olduğu gibi, `--raw` seçeneği geçirerek makineye yönelik daha kolay bir çıkış edinebilirsiniz:

```cmd/sh
iotc-explorer get-twin <your-device-id> --raw
```

## <a name="next-steps"></a>Sonraki adımlar

IoT Central gezgin 'i nasıl kullanacağınızı öğrendiğinize göre, önerilen sonraki adım [cihazları yönetmeyi IoT Central](howto-manage-devices.md)keşfetmeye yönelik olarak sunulmuştur.
