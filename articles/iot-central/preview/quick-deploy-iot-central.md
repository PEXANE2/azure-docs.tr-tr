---
title: Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Yeni bir Azure IoT Central uygulaması oluşturun. Uygulama şablonu kullanarak bir deneme veya standart uygulama oluşturun.
author: lmasieri
ms.author: lmasieri
ms.date: 12/18/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: 1b19909f005fa11b842fccc0497cb49960e32a3b
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989746"
---
# <a name="create-an-azure-iot-central-application-preview-features"></a>Azure IoT Central uygulaması oluşturma (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Bu hızlı başlangıçta IoT Tak ve Kullan gibi Önizleme özelliklerini içeren bir Azure IoT Central uygulamasının nasıl oluşturulacağı gösterilmektedir.

> [!WARNING]
> Azure IoT Central IoT Tak ve Kullan özellikleri şu anda genel önizlemededir. Üretim iş yükleri için IoT Tak ve Kullan özellikli IoT Central uygulaması kullanmayın. Üretim ortamları için, geçerli, genel olarak kullanılabilir bir uygulama şablonundan oluşturulan bir IoT Merkezi uygulaması kullanın.

## <a name="create-an-application"></a>Uygulama oluşturma

[Azure IoT Central Build](https://aka.ms/iotcentral) sitesine gidin. Sonra bir Microsoft kişisel, iş veya okul hesabıyla oturum açın.

Sektörden ilgili IoT Central şablonlar listesinden, hızlıca başlamanıza veya **özel uygulama** şablonunu kullanarak sıfırdan başlamanıza yardımcı olması için yeni bir uygulama oluşturursunuz.

![Azure IoT Central uygulama sayfası oluşturma](media/quick-deploy-iot-central/iotcentralcreate-templates-pnp.png)

Yeni bir Azure IoT Central uygulaması oluşturmak için:

1. Bir *sektör şablonundan*yeni bir Azure IoT Central uygulaması oluşturmak için, sektörlerden birinin altındaki kullanılabilir şablonlar listesinden bir uygulama şablonu seçin. Ayrıca, *özel uygulama*' yı seçerek sıfırdan başlayabilirsiniz.
1. Azure IoT Central, seçtiğiniz uygulama şablonuna göre otomatik olarak bir **uygulama adı** önerir. Bu adı kullanabilir veya kendi kolay uygulamanızın adını girebilirsiniz.
1. Azure IoT Central, uygulama adına bağlı olarak sizin için benzersiz bir **Uygulama URL 'si** ön eki de oluşturur. Uygulamanıza erişmek için bu URL 'YI kullanırsınız. İsterseniz bu URL önekini daha kolay bir şekilde değiştirebilirsiniz.

    ![Azure IoT Central uygulama sayfası oluşturma](media/quick-deploy-iot-central/iotcentralcreate-industry-pnp.png)

    ![Azure IoT Central uygulama sayfası oluşturma](media/quick-deploy-iot-central/iotcentralcreate-billinginfo-pnp.png)
    > [!NOTE]
    > Özel uygulama şablonunu kullanıyorsanız, bir **uygulama şablonu** açılır alanı görürsünüz. Buradan önizleme ve genel kullanıma sunulan şablonlar arasında geçiş yapabilirsiniz. Ayrıca, kuruluşunuz için kullanılabilir hale getirilen diğer şablonları da görebilirsiniz.

1. Bu uygulamayı 7 günlük ücretsiz deneme sürümünü kullanarak oluşturmak mı yoksa standart bir abonelik kullanmak mı istediğinizi seçin.
    - **Standart** planı kullanarak oluşturduğunuz uygulamalar cihaz başına alınarak faturalandırılır, ilk iki cihazla ücretsiz olarak **Standart 1** veya **Standart 2** fiyatlandırma planı seçebilirsiniz. [Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/)ücretsiz ve standart fiyatlandırma planları hakkında daha fazla bilgi edinin. Standart bir uygulama oluşturursanız, *dizininizi*, *Azure aboneliğinizi*ve *konumunu*seçmeniz gerekir:
      - *Dizin* , uygulamanızı oluşturacağınız Azure ACTIVE DIRECTORY (AAD). Azure AD, Kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Azure AD yoksa, bir Azure aboneliği oluşturduğunuzda sizin için bir tane oluşturulur.
      - *Azure Aboneliği*, Azure hizmetlerinin örneklerini oluşturmanıza olanak tanır. IoT Central aboneliğinizdeki kaynakları sağlar. Azure aboneliğiniz yoksa, [Azure kayıt sayfasında](https://aka.ms/createazuresubscription) bir abonelik oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra, **uygulama oluşturma** sayfasına geri gidin. Yeni aboneliğiniz **Azure aboneliği** açılır penceresinde görünür.
      - *Konum* , uygulamanızı oluşturmak istediğiniz [Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) ' dır. Genellikle en iyi performansı elde etmek için cihazlarınıza fiziksel olarak en yakın konumu seçmeniz gerekir. Azure IoT Central genel önizlemesi Şu anda **Birleşik Devletler**veya **Avrupa**'da kullanılabilir. Bir konum seçtikten sonra, uygulamanızı daha sonra farklı bir konuma taşıyamazsınız.
        > [!NOTE]
        > Genel Önizleme sırasında, **Önizleme uygulamaları** için yalnızca kullanılabilir konumlar **Avrupa** ve **Birleşik Devletler**.

1. Hüküm ve koşulları gözden geçirin ve sayfanın alt kısmında **Oluştur** ' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IoT Central uygulaması oluşturdunuz. Önerilen sonraki adım:

> [!div class="nextstepaction"]
> [IoT Central uygulamanıza sanal cihaz ekleme](./quick-create-pnp-device.md)
