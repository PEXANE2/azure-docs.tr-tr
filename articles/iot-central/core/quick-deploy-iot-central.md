---
title: Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Yeni bir Azure IoT Central uygulaması oluşturun. Ücretsiz fiyatlandırma planı veya standart fiyatlandırma planlarından birini kullanarak uygulamayı oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: 5268f5438c005033f9c6ecf74657dc1a01d3b673
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989423"
---
# <a name="create-an-azure-iot-central-application"></a>Azure IoT Central uygulaması oluşturma

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

_Oluşturucu_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Microsoft Azure IoT Central uygulamanızı tanımlayabilirsiniz. Bu hızlı başlangıçta, örnek bir _cihaz şablonu_Içeren bir Azure IoT Central uygulamasının nasıl oluşturulacağı gösterilmektedir. Oluşturduğunuz uygulama herhangi bir önizleme özelliği kullanmaz.

## <a name="create-an-application"></a>Uygulama oluşturma

[Azure IoT Central Build](https://aka.ms/iotcentral) Web sitesine gidin. Sonra bir Microsoft kişisel, iş veya okul hesabıyla oturum açın.

Önizleme özellikleri etkinken Azure IoT Central uygulaması oluşturmaya başlamak için **Oluştur**' u seçin. Bu bağlantı sizi **IoT uygulamanızı derleme** sayfasına götürür.

![Azure IoT Central derleme sayfası](media/quick-deploy-iot-central/iotcentralcreate.png)

Ardından **özel uygulama**' yı seçin.

Yeni bir Azure IoT Central uygulaması oluşturmak için:

1. Azure IoT Central, seçtiğiniz uygulama şablonuna göre otomatik olarak bir uygulama adı önerir. Bu adı kabul edebilir veya **contoso IoT**gibi kendi kolay uygulama adınızı girebilirsiniz. Azure IoT Central, uygulama adına göre sizin için benzersiz bir URL de oluşturur. İsterseniz bu URL önekini daha kolay bir şekilde değiştirebilirsiniz.

1. Önizleme özelliklerini kullanmayan **eski uygulama** şablonunu seçin.

    | Uygulama şablonu | Açıklama |
    | -------------------- | ----------- |
    | Eski uygulama   | Kendi cihaz şablonlarınız ve cihazlarınızla doldurabileceğiniz boş bir uygulama oluşturur. |

    ![Azure IoT Central yeni uygulama](media/quick-deploy-iot-central/newapplication.png)

    ![Azure IoT Central faturalandırma bilgileri](media/quick-deploy-iot-central/billinginfo.png)

1. 7 günlük ücretsiz deneme fiyatlandırma planını veya standart fiyatlandırma planlarından birini kullanarak bu uygulamayı oluşturmayı seçin:
   - *Ücretsiz* planı kullanarak oluşturduğunuz uygulamalar yedi gün boyunca ücretsizdir ve en fazla beş cihaz destekler. Bunları, süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planı kullanacak şekilde dönüştürebilirsiniz.
   - *Standart* planı kullanarak oluşturduğunuz uygulamalar cihaz başına alınarak faturalandırılır, ilk iki cihazla ücretsiz olarak **Standart 1** veya **Standart 2** fiyatlandırma planı seçebilirsiniz. [Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/)ücretsiz ve standart fiyatlandırma planları hakkında daha fazla bilgi edinin. Standart bir uygulama oluşturursanız, *dizininizi*, *Azure aboneliğinizi*ve *konumunu*seçmeniz gerekir:
        - *Dizin* , uygulamanızı oluşturmak için Azure ACTIVE DIRECTORY (ad). Kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Azure AD yoksa, bir Azure aboneliği oluşturduğunuzda sizin için bir tane oluşturulur.
        - *Azure Aboneliği*, Azure hizmetlerinin örneklerini oluşturmanıza olanak tanır. IoT Central aboneliğinizdeki kaynakları sağlar. Azure aboneliğiniz yoksa, [Azure kayıt sayfasında](https://aka.ms/createazuresubscription) bir abonelik oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra, **uygulama oluşturma** sayfasına geri gidin. Yeni aboneliğiniz **Azure Aboneliği** açılan listesinde gösterilir.
        - *Konum* , uygulamanızı oluşturmak istediğiniz [Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) ' dır. Genellikle en iyi performansı elde etmek için cihazlarınıza fiziksel olarak en yakın konumu seçmeniz gerekir. Azure IoT Central Şu anda **Birleşik Devletler**, **Avustralya**, **Asya Pasifik**veya **Avrupa**'da sunulmaktadır.  Bir konum seçtikten sonra, uygulamanızı daha sonra farklı bir konuma taşıyamazsınız.
        [Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/), fiyatlar hakkında daha fazla bilgi edinin.

1. Daha önce seçtiğiniz ödeme planı için gereken ek bilgileri, 1. adımda girin.

1. Sayfanın alt kısmında **Oluştur** ' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IoT Central uygulaması oluşturdunuz. Önerilen sonraki adım:

> [!div class="nextstepaction"]
> [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](./tutorial-define-device-type.md)
