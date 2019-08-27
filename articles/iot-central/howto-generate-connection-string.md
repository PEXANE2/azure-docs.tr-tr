---
title: Azure IoT Central için bir cihaz bağlantı dizesi oluşturma | Microsoft Docs
description: Bir cihaz geliştiricisi olarak, bir IoT Central uygulamasına bağlanması gereken bir cihaz için bağlantı dizesi nasıl oluşturabilirim?
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 97c0332656b75c3c8d0cddecb41c7a15ac2f218c
ms.sourcegitcommit: 80dff35a6ded18fa15bba633bf5b768aa2284fa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70019789"
---
# <a name="generate-a-device-connection-string-to-connect-to-an-azure-iot-central-application"></a>Azure IoT Central uygulamasına bağlanmak için bir cihaz bağlantı dizesi oluşturma

Bu makalede bir cihaz geliştiricisi olarak, bir IoT Central uygulamasına bağlanması gereken bir cihaz için bağlantı dizesi oluşturma açıklanır. Bu makalede açıklanan yordam, paylaşılan erişim imzası (SAS) kullanarak tek bir cihazı hızlı bir şekilde nasıl bağlayabileceğinizi gösterir. Bu yaklaşım, IoT Central veya test cihazlarıyla denemeler yaparken faydalıdır. Bir üretim ortamında kullanmak için alternatif yaklaşımlar için bkz. [Azure IoT Central 'Da cihaz bağlantısı](concepts-connectivity.md).

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

- Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
- [Node. js](https://nodejs.org/) sürüm 8.0.0 veya üzeri yüklü bir geliştirme makinesi. Sürümünüzü denetlemek için `node --version` komut satırında çalıştırabilirsiniz. Node.js çeşitli işletim sistemleri için kullanılabilir.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Aşağıdaki adımlarda, bir cihaz için SAS bağlantı dizesi oluşturmak için ihtiyacınız olan bilgilerin nasıl alınacağı açıklanır:

1. **Device Explorer**, uygulamanıza bağlamak istediğiniz gerçek cihazı bulun:

    ![Gerçek bir cihaz seçin](media/howto-generate-connection-string/real-devices.png)

1. **Cihaz** sayfasında, **Bağlan**' ı seçin.

    ![Bağlan 'ı seçin](media/howto-generate-connection-string/connect.png)

1. Aşağıdaki adımlarda kullanılacak bağlantı ayrıntılarını, **kapsam kimliğini**, **cihaz kimliğini**ve **cihaz birincil anahtarını**bir yere göz önünde yapın:

    ![Bağlantı ayrıntıları](media/howto-generate-connection-string/device-connect.png)

    Bu sayfadaki değerleri kaydetmek için kopyalayabilirsiniz.

## <a name="generate-the-connection-string"></a>Bağlantı dizesini oluşturma

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık gerçek bir cihaz için Azure IoT Central uygulamanıza bağlanacak bir bağlantı dizesi oluşturduğunuza göre, önerilen sonraki adımlar aşağıda verilmiştir:

* [Bir DevKit cihazını hazırlama ve bağlama (C)](howto-connect-devkit.md)
* [Raspberry Pi (Python) hazırlama ve bağlama](howto-connect-raspberry-pi-python.md)
* [Raspberry Pi (C#) hazırlama ve bağlama](howto-connect-raspberry-pi-csharp.md)
* [Windows 10 IoT çekirdek cihazını hazırlama ve bağlama (C#)](howto-connect-windowsiotcore.md)
* [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs.md)