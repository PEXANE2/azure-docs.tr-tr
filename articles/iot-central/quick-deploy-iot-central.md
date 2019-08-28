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
ms.openlocfilehash: 0d5f04d9c0cc6dcd8a784ae7a816d93e2aab30aa
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70099481"
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
      - *Bölge*, uygulamanızı oluşturmak istediğiniz fiziksel konumdur. Normalde, en uygun performansı elde etmek için cihazlarınıza en yakın fiziksel konumu seçmeniz gerekir. Azure IoT Central'ın sağlandığı bölgeleri, [Bölgeye göre sağlanan ürünler](https://azure.microsoft.com/regions/services/) sayfasında görebilirsiniz. Bir bölge seçtiğinizde, daha sonra uygulamanızı farklı bir bölgeye taşıyamazsınız.

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
