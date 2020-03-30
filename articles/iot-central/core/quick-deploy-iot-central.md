---
title: Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Yeni bir Azure IoT Central uygulaması oluşturun. Uygulamayı ücretsiz fiyatlandırma planını veya standart fiyatlandırma planlarından birini kullanarak oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 02/12/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: d69a761df8066b4a84312c0c3ae8be5a79490960
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77169400"
---
# <a name="create-an-azure-iot-central-application"></a>Azure IoT Central uygulaması oluşturma

Bu hızlı başlangıç, azure IoT Merkezi uygulamasını nasıl oluşturabileceğinizi gösterir.

## <a name="create-an-application"></a>Uygulama oluşturma

[Azure IoT Merkezi Yapı](https://aka.ms/iotcentral) sitesine gidin. Ardından bir Microsoft kişisel, iş veya okul hesabıyla oturum açın.

Hızlı bir şekilde başlamanıza yardımcı olmak için endüstriyle alakalı IoT Central şablonları listesinden veya **Özel uygulamalar** şablonu kullanarak sıfırdan başlamak için yeni bir uygulama oluşturursunuz. Bu hızlı başlatmada, **Özel uygulama** şablonu kullanın.

**Özel uygulama** şablonundan yeni bir Azure IoT Merkezi uygulaması oluşturmak için:

1. **Yapı** sayfasına gidin:

    ![IoT uygulama sayfanızı oluşturun](media/quick-deploy-iot-central/iotcentralcreate-new-application.png)

1. **Özel uygulamaları** seçin ve **Özel uygulama** şablonundan seçildiğinden emin olun.

1. Azure IoT Central, seçtiğiniz uygulama şablonuna göre otomatik olarak bir **uygulama adı** önerir. Bu adı kullanabilir veya kendi kolay uygulama adınızı girebilirsiniz.

1. Azure IoT Central, uygulama adını temel alan benzersiz bir **uygulama URL** öneki de oluşturur. Bu URL'yi uygulamanıza erişmek için kullanırsınız. İsterseniz bu URL önekini daha akılda kalıcı bir şeyle değiştirin.

    ![Azure IoT Merkezi Uygulama sayfası oluşturma](media/quick-deploy-iot-central/iotcentralcreate-custom.png)

    ![Azure IoT Merkezi fatura bilgileri](media/quick-deploy-iot-central/iotcentralcreate-billinginfo.png)

    > [!NOTE]
    > Önceki sayfada **Özel uygulamayı** seçtiyseniz, bir Uygulama **şablonu** açılır görmezsiniz. Buradan özel ve eski şablonlar arasında geçiş yapabilirsiniz. Kuruluşunuz için kullanıma sunulan diğer şablonları da görebilirsiniz.

1. Bu uygulamayı 7 günlük ücretsiz deneme fiyatlandırma planını veya standart fiyatlandırma planlarından birini kullanarak oluşturmayı seçin:

    - *Ücretsiz* planı kullanarak oluşturduğunuz uygulamalar yedi gün boyunca ücretsizdir ve beş cihaza kadar destek lenir. Bunları, süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planı kullanmaya dönüştürebilirsiniz.
    - *Standart* bir plan kullanarak oluşturduğunuz uygulamalar cihaz bazında faturalandırılır, ilk iki cihaz ücretsiz olan **Standart 1** veya Standart **2** fiyatlandırma planını seçebilirsiniz. [Azure IoT Merkezi fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/iot-central/)ücretsiz ve standart fiyatlandırma planları hakkında daha fazla bilgi edinin. Standart bir fiyatlandırma planı kullanarak bir uygulama oluşturuyorsanız, *Dizininizi,* *Azure Aboneliğinizi*ve *Konumunuzu*seçmeniz gerekir:
        - *Dizin,* uygulamanızı oluşturduğunuz Azure Etkin Dizini'dir. Azure Etkin Dizini kullanıcı kimlikleri, kimlik bilgileri ve diğer kuruluş bilgilerini içerir. Azure Etkin Dizin'inyoksa, bir Azure aboneliği oluşturduğunuzda sizin için bir tane oluşturulur.
        - *Azure Aboneliği*, Azure hizmetlerinin örneklerini oluşturmanıza olanak tanır. IoT Merkezi, aboneliğinizdeki kaynakları sağlar. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)ücretsiz bir abonelik oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra **Yeni uygulama** sayfasına geri gidin. Yeni aboneliğiniz artık **Azure Aboneliği** açılır açılır yerinde görünür.
        - *Konum,* uygulamanızı oluşturmak istediğiniz [coğrafyadır.](https://azure.microsoft.com/global-infrastructure/geographies/) Genellikle, en iyi performansı elde etmek için fiziksel olarak aygıtlarınıza en yakın konumu seçmeniz gerekir. Bir konum seçtikten sonra, daha sonra uygulamanızı başka bir konuma taşıyamazsınız.

1. Şartlar ve Koşulları gözden geçirin ve sayfanın altındaki **Oluştur'u** seçin. Birkaç dakika sonra, IoT Central uygulaması kullanıma hazırdır:

    ![Azure IoT Merkezi uygulaması](media/quick-deploy-iot-central/iotcentral-application.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IoT Central uygulaması oluşturdunuz. Önerilen sonraki adım:

> [!div class="nextstepaction"]
> [IoT Merkezi uygulamanıza benzetilen bir aygıt ekleme](./quick-create-pnp-device.md)
