---
title: Hızlı başlangıç-Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Hızlı başlangıç-yeni bir Azure IoT Central uygulaması oluşturun. Ücretsiz fiyatlandırma planı veya standart fiyatlandırma planlarından birini kullanarak uygulamayı oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 12/28/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: c5e357824652077acab299e0e27678f528f23b9a
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99833907"
---
# <a name="quickstart---create-an-azure-iot-central-application"></a>Hızlı başlangıç-Azure IoT Central uygulaması oluşturma

Bu hızlı başlangıçta, Azure IoT Central uygulamasının nasıl oluşturulacağı gösterilmektedir.


[!INCLUDE [Warning About Access Required](../../../includes/iot-central-warning-contribitorrequireaccess.md)]

## <a name="create-an-application"></a>Uygulama oluşturma

[Azure IoT Central Build](https://aka.ms/iotcentral) sitesine gidin. Sonra bir Microsoft kişisel, iş veya okul hesabıyla oturum açın.

Sektörün hızlı bir şekilde başlamanıza veya **özel bir uygulamalar** şablonunu kullanarak sıfırdan başlamanıza yardımcı olması için sektör ile ilgili IoT Central şablonlar listesinden yeni bir uygulama oluşturursunuz. Bu hızlı başlangıçta, **özel uygulama** şablonunu kullanırsınız.

**Özel uygulama** şablonundan yeni bir Azure IoT Central uygulaması oluşturmak için:

1. **Derleme** sayfasına gidin:

    :::image type="content" source="media/quick-deploy-iot-central/iotcentralcreate-new-application.png" alt-text="IoT uygulama sayfanızı oluşturma":::

1. **Özel uygulamalar** ' ı seçin ve **özel uygulama** şablonunun seçili olduğundan emin olun.

1. Azure IoT Central, seçtiğiniz uygulama şablonuna göre otomatik olarak bir **uygulama adı** önerir. Bu adı kullanabilir veya kendi kolay uygulamanızın adını girebilirsiniz.

1. Azure IoT Central, uygulama adına bağlı olarak sizin için benzersiz bir **Uygulama URL 'si** ön eki de oluşturur. Uygulamanıza erişmek için bu URL 'YI kullanırsınız. İsterseniz bu URL önekini daha kolay bir şekilde değiştirin.

    :::image type="content" source="media/quick-deploy-iot-central/iotcentralcreate-custom.png" alt-text="Azure IoT Central uygulama sayfası oluşturma":::

    :::image type="content" source="media/quick-deploy-iot-central/iotcentralcreate-billinginfo.png" alt-text="Azure IoT Central faturalandırma bilgileri":::

    > [!Tip]
    > Önceki sayfada **özel uygulama** ' yı seçerseniz, bir **uygulama şablonu** açılır listesini görürsünüz. Açılan menü, kuruluşunuz tarafından size sunulan diğer şablonları gösterebilir.

1. 7 günlük ücretsiz deneme fiyatlandırma planını veya standart fiyatlandırma planlarından birini kullanarak bu uygulamayı oluşturmayı seçin:

    - *Ücretsiz* planı kullanarak oluşturduğunuz uygulamalar yedi gün boyunca ücretsizdir ve en fazla beş cihaz destekler. Bunları, süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planı kullanacak şekilde dönüştürebilirsiniz.
        > [!NOTE]
        > *Ücretsiz* plan kullanılarak oluşturulan uygulamalar, Azure abonelikleri gerektirmez ve bu nedenle, bunları Azure Portal Azure aboneliğinizdeki listede bulamayamayacağız. IoT Central portalından yalnızca ücretsiz uygulamaları görebilir ve yönetebilirsiniz.          
    - *Standart* bir plan kullanarak oluşturduğunuz uygulamalar, cihaz başına temelinde faturalandırılır, ilk iki cihazla serbest bırakılmakta olan **Standart 0**, **Standart 1** veya **Standart 2** fiyatlandırma planı arasından seçim yapabilirsiniz. [Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/)ücretsiz ve standart fiyatlandırma planları hakkında daha fazla bilgi edinin. Standart fiyatlandırma planı kullanarak bir uygulama oluşturursanız, *dizininizi*, *Azure aboneliğinizi* ve *konumunu* seçmeniz gerekir:
        - *Dizin* , uygulamanızı oluşturduğunuz Azure Active Directory. Azure Active Directory Kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Bir Azure Active Directory yoksa, bir Azure aboneliği oluşturduğunuzda sizin için bir tane oluşturulur.
        - *Azure Aboneliği*, Azure hizmetlerinin örneklerini oluşturmanıza olanak tanır. IoT Central aboneliğinizdeki kaynakları sağlar. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)ücretsiz olarak bir tane oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra **Yeni uygulama** sayfasına geri gidin. Yeni aboneliğiniz artık **Azure abonelik** açılır penceresinde görünür.
        - *Konum* , uygulamanızı oluşturmak istediğiniz [Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) ' dır. Genellikle en iyi performansı elde etmek için cihazlarınıza fiziksel olarak en yakın konumu seçmeniz gerekir. Bir konum seçtikten sonra uygulamanızı daha sonra farklı bir konuma taşıyamazsınız.

1. Hüküm ve koşulları gözden geçirin ve sayfanın alt kısmında **Oluştur** ' u seçin. Birkaç dakika sonra, uygulama IoT Central kullanmaya hazırız:

    :::image type="content" source="media/quick-deploy-iot-central/iotcentral-application.png" alt-text="Azure IoT Central uygulaması":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-central-clean-up-resources](../../../includes/iot-central-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IoT Central uygulaması oluşturdunuz. IoT Central hakkında öğrenmeye devam etmek için önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [IoT Central uygulamanıza sanal cihaz ekleme](./quick-create-simulated-device.md)

Bir cihaz geliştiricisiyseniz ve bazı kodları incelemek istiyorsanız, önerilen sonraki adım şunları yapmanız gerekir:
> [!div class="nextstepaction"]
> [Bir istemci uygulamasını oluşturma ve Azure IoT Central uygulamanıza bağlama](./tutorial-connect-device.md)