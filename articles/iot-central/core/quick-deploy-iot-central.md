---
title: Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Yeni bir Azure IoT Central uygulaması oluşturun. Ücretsiz fiyatlandırma planı veya standart fiyatlandırma planlarından birini kullanarak uygulamayı oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 02/12/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: d69a761df8066b4a84312c0c3ae8be5a79490960
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169400"
---
# <a name="create-an-azure-iot-central-application"></a>Azure IoT Central uygulaması oluşturma

Bu hızlı başlangıçta, Azure IoT Central uygulamasının nasıl oluşturulacağı gösterilmektedir.

## <a name="create-an-application"></a>Uygulama oluşturma

[Azure IoT Central Build](https://aka.ms/iotcentral) sitesine gidin. Sonra bir Microsoft kişisel, iş veya okul hesabıyla oturum açın.

Sektörün hızlı bir şekilde başlamanıza veya **özel bir uygulamalar** şablonunu kullanarak sıfırdan başlamanıza yardımcı olması için sektör ile ilgili IoT Central şablonlar listesinden yeni bir uygulama oluşturursunuz. Bu hızlı başlangıçta, **özel uygulama** şablonunu kullanırsınız.

**Özel uygulama** şablonundan yeni bir Azure IoT Central uygulaması oluşturmak için:

1. **Derleme** sayfasına gidin:

    ![IoT uygulama sayfanızı oluşturma](media/quick-deploy-iot-central/iotcentralcreate-new-application.png)

1. **Özel uygulamalar** ' ı seçin ve **özel uygulama** şablonunun seçili olduğundan emin olun.

1. Azure IoT Central, seçtiğiniz uygulama şablonuna göre otomatik olarak bir **uygulama adı** önerir. Bu adı kullanabilir veya kendi kolay uygulamanızın adını girebilirsiniz.

1. Azure IoT Central, uygulama adına bağlı olarak sizin için benzersiz bir **Uygulama URL 'si** ön eki de oluşturur. Uygulamanıza erişmek için bu URL 'YI kullanırsınız. İsterseniz bu URL önekini daha kolay bir şekilde değiştirin.

    ![Azure IoT Central uygulama sayfası oluşturma](media/quick-deploy-iot-central/iotcentralcreate-custom.png)

    ![Azure IoT Central faturalandırma bilgileri](media/quick-deploy-iot-central/iotcentralcreate-billinginfo.png)

    > [!NOTE]
    > Önceki sayfada **özel uygulama** ' yı seçerseniz, bir **uygulama şablonu** açılır listesini görürsünüz. Buradan özel ve eski şablonlar arasında geçiş yapabilirsiniz. Ayrıca, kuruluşunuz için kullanılabilir hale getirilen diğer şablonları da görebilirsiniz.

1. 7 günlük ücretsiz deneme fiyatlandırma planını veya standart fiyatlandırma planlarından birini kullanarak bu uygulamayı oluşturmayı seçin:

    - *Ücretsiz* planı kullanarak oluşturduğunuz uygulamalar yedi gün boyunca ücretsizdir ve en fazla beş cihaz destekler. Bunları, süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planı kullanacak şekilde dönüştürebilirsiniz.
    - *Standart* bir plan kullanarak oluşturduğunuz uygulamalar cihaz başına alınarak faturalandırılır, ilk iki cihazla ücretsiz olarak **Standart 1** veya **Standart 2** fiyatlandırma planı seçebilirsiniz. [Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/)ücretsiz ve standart fiyatlandırma planları hakkında daha fazla bilgi edinin. Standart fiyatlandırma planı kullanarak bir uygulama oluşturursanız, *dizininizi*, *Azure aboneliğinizi*ve *konumunu*seçmeniz gerekir:
        - *Dizin* , uygulamanızı oluşturduğunuz Azure Active Directory. Azure Active Directory Kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Bir Azure Active Directory yoksa, bir Azure aboneliği oluşturduğunuzda sizin için bir tane oluşturulur.
        - *Azure Aboneliği*, Azure hizmetlerinin örneklerini oluşturmanıza olanak tanır. IoT Central aboneliğinizdeki kaynakları sağlar. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)ücretsiz olarak bir tane oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra **Yeni uygulama** sayfasına geri gidin. Yeni aboneliğiniz artık **Azure abonelik** açılır penceresinde görünür.
        - *Konum* , uygulamanızı oluşturmak istediğiniz [Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) ' dır. Genellikle en iyi performansı elde etmek için cihazlarınıza fiziksel olarak en yakın konumu seçmeniz gerekir. Bir konum seçtikten sonra uygulamanızı daha sonra farklı bir konuma taşıyamazsınız.

1. Hüküm ve koşulları gözden geçirin ve sayfanın alt kısmında **Oluştur** ' u seçin. Birkaç dakika sonra, uygulama IoT Central kullanmaya hazırız:

    ![Azure IoT Central uygulaması](media/quick-deploy-iot-central/iotcentral-application.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IoT Central uygulaması oluşturdunuz. Önerilen sonraki adım:

> [!div class="nextstepaction"]
> [IoT Central uygulamanıza sanal cihaz ekleme](./quick-create-pnp-device.md)
