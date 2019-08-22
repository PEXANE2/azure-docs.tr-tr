---
title: Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Yeni bir Azure IoT Central uygulaması oluşturun. Uygulama şablonu kullanarak bir Deneme veya Kullandıkça Öde uygulaması oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 06/07/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 05b761dbf9f58f6afca6459e28d2fecca9aa7b06
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878309"
---
# <a name="create-an-azure-iot-central-application-preview-features"></a>Azure IoT Central uygulaması oluşturma (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu hızlı başlangıçta IoT Tak ve Kullan gibi Önizleme özelliklerini kullanan bir Azure IoT Central uygulamasının nasıl oluşturulacağı gösterilmektedir.

> [!WARNING]
> Azure IoT Central IoT Tak ve Kullan özellikleri şu anda genel önizlemededir. Üretim iş yükleri için IoT Tak ve Kullan özellikli IoT Central uygulaması kullanmayın. Üretim ortamları için, geçerli, genel olarak kullanılabilir bir uygulama şablonundan oluşturulan bir IoT Merkezi uygulaması kullanın.

## <a name="create-an-application"></a>Uygulama oluşturma

Azure IoT Central [Uygulama Yöneticisi](https://aka.ms/iotcentral) sayfasına gidin. Sonra bir Microsoft kişisel veya iş veya okul hesabıyla oturum açın.

Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni Uygulama**'yı seçin. Bu bağlantı sizi **uygulama oluşturma** sayfasına götürür.

![Azure IoT Central uygulama sayfası oluşturma](media/quick-deploy-iot-central-pnp/iotcentralcreate.png)

IoT Tak ve Kullan gibi Önizleme özelliklerini içeren yeni bir Azure IoT Central uygulaması oluşturmak için:

1. Ödeme planını seçin:
   - **Deneme** uygulamaları, süresi dolmadan yedi gün boyunca ücretsizdir. Süresi dolmadan önce herhangi bir anda Kullandıkça Öde uygulamasına dönüştürülebilir. Bir **deneme** uygulaması oluşturursanız, iletişim bilgilerinizi girmeniz ve Microsoft 'tan bilgi ve ipuçları alıp almamanız gerekir.
   - **Kullandıkça Öde** uygulamaları, ilk beş cihaz ücretsiz olarak cihaz başına ücretlendirilir. **Kullandıkça Öde** uygulaması oluşturursanız, *dizininizi*, *Azure aboneliğinizi*ve *bölgenizi*seçmeniz gerekir:
      - *Dizin* , uygulamanızı oluşturmak için Azure ACTIVE DIRECTORY (ad). Kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Azure AD yoksa, bir Azure aboneliği oluşturduğunuzda sizin için bir tane oluşturulur.
      - *Azure Aboneliği*, Azure hizmetlerinin örneklerini oluşturmanıza olanak tanır. IoT Central aboneliğinizde kaynakları sağlayacaktır. Azure aboneliğiniz yoksa, [Azure kayıt sayfasında](https://aka.ms/createazuresubscription) bir abonelik oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra, **Uygulama Oluştur** sayfasına geri gidin. Yeni aboneliğiniz **Azure Aboneliği** açılan listesinde gösterilir.
      - *Bölge*, uygulamanızı oluşturmak istediğiniz fiziksel konumdur. Genellikle en iyi performansı elde etmek için cihazlarınıza fiziksel olarak en yakın bölgeyi seçersiniz. Azure IoT Central 'ın [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/) sayfasında kullanılabildiği bölgeleri görebilirsiniz. Bir bölge seçtiğinizde, daha sonra uygulamanızı farklı bir bölgeye taşıyamazsınız.

      [Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/), fiyatlar hakkında daha fazla bilgi edinin.

1. **Contoso IoT** gibi kolay bir uygulama adı seçin. Azure IoT Central sizin için benzersiz bir URL ön eki oluşturur. Bu URL ön ekini daha akılda kalır bir şeyle değiştirebilirsiniz.

1. **Önizleme uygulaması** şablonunu seçin. Uygulama şablonu, başlamanıza yardımcı olacak cihaz şablonları ve panolar gibi önceden tanımlanmış öğeleri içerebilir.

1. Sayfanın alt kısmında **Oluştur** ' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Önizleme özelliklerini kullanan bir IoT Central uygulaması oluşturdunuz. Önerilen sonraki adım:

> [!div class="nextstepaction"]
> [IoT Central'da bir tura katılın](overview-iot-central-tour-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
