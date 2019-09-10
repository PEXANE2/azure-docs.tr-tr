---
title: Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Yeni bir Azure IoT Central uygulaması oluşturun. Uygulama şablonu kullanarak bir Deneme veya Kullandıkça Öde uygulaması oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: eb6759d95ab0fb7afd3b6179babf052dfb029ff2
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70873436"
---
# <a name="create-an-azure-iot-central-application"></a>Azure IoT Central uygulaması oluşturma

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

_Oluşturucu_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Microsoft Azure IoT Central uygulamanızı tanımlayabilirsiniz. Bu hızlı başlangıç örnek bir _cihaz şablonu_ ve simülasyon _cihazları_ içeren bir Azure IoT Central uygulaması oluşturma adımlarını göstermektedir.

## <a name="create-an-application"></a>Uygulama oluşturma

[Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) Web sitesine gidin. Microsoft kişisel ya da iş veya okul hesabıyla oturum açmalısınız.

Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni Uygulama**'yı seçin. Bu sizi **Uygulama Oluştur** sayfasına götürür.

![Azure IoT Central Uygulama Oluştur sayfası](media/quick-deploy-iot-central/iotcentralcreate.png)

Yeni bir Azure IoT Central uygulaması oluşturmak için:

1. Ödeme planını seçin:
   - **Deneme** uygulamaları, süresi dolmadan 7 gün ücretsiz kullanılabilir. Süresi dolmadan önce herhangi bir anda Kullandıkça Öde uygulamasına dönüştürülebilir. Bir **deneme** uygulaması oluşturursanız, iletişim bilgilerinizi girmeniz ve Microsoft 'tan bilgi ve ipuçları alıp almamanız gerekir.
   - **Kullandıkça Öde** uygulamaları, ilk 5 cihaz ücretsiz olmak üzere cihaz başına ücretlendirilir. **Kullandıkça Öde** uygulaması oluşturursanız, *dizininizi*, *Azure aboneliğinizi*ve *bölgenizi*seçmeniz gerekir:
      - *Dizin* , uygulamanızı oluşturmak için Azure ACTIVE DIRECTORY (ad). Kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Azure AD yoksa, bir Azure aboneliği oluşturduğunuzda sizin için bir tane oluşturulur.
      - *Azure Aboneliği*, Azure hizmetlerinin örneklerini oluşturmanıza olanak tanır. IoT Central aboneliğinizde kaynakları sağlayacaktır. Azure aboneliğiniz yoksa, [Azure kayıt sayfasında](https://aka.ms/createazuresubscription) bir abonelik oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra, **Uygulama Oluştur** sayfasına geri gidin. Yeni aboneliğiniz **Azure Aboneliği** açılan listesinde gösterilir.
      - *Bölge* , uygulamanızı oluşturmak istediğiniz fiziksel konum veya [Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) . Genellikle en iyi performansı elde etmek için cihazlarınıza fiziksel olarak en yakın bölgeyi seçmeniz gerekir. Azure IoT Central 'ın [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=iot-central) sayfasında kullanılabildiği bölgeleri görebilirsiniz. Bir bölge seçtiğinizde, daha sonra uygulamanızı farklı bir bölgeye taşıyamazsınız.

      [Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/), fiyatlar hakkında daha fazla bilgi edinin.

1. Uygulama şablonunu seçin. Uygulama şablonu, başlamanıza yardımcı olacak cihaz şablonları ve panolar gibi önceden tanımlanmış öğeleri içerebilir.

    | Uygulama şablonu | Açıklama |
    | -------------------- | ----------- |
    | Örnek Contoso       | Bir Soğutmalı Otomat için önceden oluşturulmuş cihaz şablonunu içeren bir uygulama oluşturur. Azure IoT Central'ı incelemeye başlamak için bu şablonu kullanın. |
    | Devkits Örneği       | MXChip veya Raspberry Pi cihazını bağlamak amacıyla sizin için hazırlanmış cihaz şablonlarıyla bir uygulama oluşturur. Bu cihazlardan birini deneyen bir cihaz gelişticisiyseniz bu şablonu kullanın. |
    | Özel uygulama   | Kendi cihaz şablonlarınız ve cihazlarınızla doldurabileceğiniz boş bir uygulama oluşturur. |

1. **Contoso IoT**gibi kolay bir uygulama adı girin. Azure IoT Central sizin için benzersiz bir URL ön eki oluşturur. Bu URL ön ekini daha akılda kalır bir şeyle değiştirebilirsiniz.

1. **Oluştur**'a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IoT Central uygulaması oluşturdunuz. Önerilen sonraki adım:

> [!div class="nextstepaction"]
> [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](./tutorial-define-device-type.md)
