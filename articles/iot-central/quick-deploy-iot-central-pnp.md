---
title: Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Yeni bir Azure IoT Central uygulaması oluşturun. Uygulama şablonu kullanarak bir deneme veya kullandıkça öde uygulaması oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 06/07/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 3d5dfe824d95506fa3f83f1fbbdca1e29e02d566
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72001303"
---
# <a name="create-an-azure-iot-central-application-preview-features"></a>Azure IoT Central uygulaması oluşturma (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu hızlı başlangıçta IoT Tak ve Kullan gibi Önizleme özelliklerini kullanan bir Azure IoT Central uygulamasının nasıl oluşturulacağı gösterilmektedir.

> [!WARNING]
> Azure IoT Central IoT Tak ve Kullan özellikleri şu anda genel önizlemededir. Üretim iş yükleri için IoT Tak ve Kullan özellikli IoT Central uygulaması kullanmayın. Üretim ortamları için, geçerli, genel olarak kullanılabilir bir uygulama şablonundan oluşturulan bir IoT Merkezi uygulaması kullanın.

## <a name="create-an-application"></a>Uygulama oluşturma

[Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) Web sitesine gidin. Sonra bir Microsoft kişisel, iş veya okul hesabıyla oturum açın.

Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni uygulama**' yı seçin. Bu bağlantı sizi **uygulama oluşturma** sayfasına götürür.

![Azure IoT Central uygulama sayfası oluşturma](media/quick-deploy-iot-central-pnp/iotcentralcreate-pnp.png)

IoT Tak ve Kullan gibi Önizleme özelliklerini içeren yeni bir Azure IoT Central uygulaması oluşturmak için:

1. Bir ödeme planı seçin:
   - **Deneme** uygulamaları, süresi dolmadan yedi gün boyunca ücretsizdir. Bu kişiler, süresi dolmadan önce herhangi bir zamanda **Kullandıkça Öde** 'ye dönüştürülebilirler. Bir **deneme** uygulaması oluşturursanız, iletişim bilgilerinizi girmeniz ve Microsoft 'tan bilgi ve ipuçları alıp almamanız gerekir.
   - **Kullandıkça Öde** uygulamaları, ilk beş cihaz ücretsiz olarak cihaz başına ücretlendirilir. **Kullandıkça Öde** uygulaması oluşturursanız, *dizininizi*, *Azure aboneliğinizi*ve *bölgenizi*seçmeniz gerekir:
        - *Dizin* , uygulamanızı oluşturmak için Azure ACTIVE DIRECTORY (ad). Kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Azure AD yoksa, bir Azure aboneliği oluşturduğunuzda sizin için bir tane oluşturulur.
        - *Azure aboneliği* , Azure Hizmetleri örnekleri oluşturmanıza olanak sağlar. IoT Central aboneliğinizdeki kaynakları sağlar. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir tane oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra, **uygulama oluşturma** sayfasına geri gidin. Yeni aboneliğiniz **Azure aboneliği** açılır penceresinde görüntülenir.
        - *Bölge* , uygulamanızı oluşturmak istediğiniz fiziksel konumdur. Genellikle en iyi performansı elde etmek için cihazlarınıza fiziksel olarak en yakın bölgeyi seçmeniz gerekir. Genel Önizleme sırasında, bir **Önizleme uygulaması** için kullanılabilen tek bölge **Kuzey Avrupa** ve **Orta ABD**. Bir bölge seçtiğinizde, uygulamanızı daha sonra farklı bir bölgeye taşıyamazsınız.

        [Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/)fiyatlandırma hakkında daha fazla bilgi edinin.

1. **Önizleme uygulaması** şablonunu seçin. Uygulama şablonu, başlamanıza yardımcı olacak cihaz şablonları ve panolar gibi önceden tanımlanmış öğeleri içerebilir.

1. Azure IoT Central, seçtiğiniz uygulama şablonuna göre otomatik olarak bir uygulama adı önerir. Bu adı kabul edebilir veya **contoso IoT**gibi kendi kolay uygulama adınızı girebilirsiniz. Azure IoT Central, uygulama adına bağlı olarak sizin için benzersiz bir URL ön eki de oluşturur. İsterseniz bu URL önekini daha kolay bir şekilde değiştirebilirsiniz.

1. Daha önce seçtiğiniz ödeme planı için gereken ek bilgileri, 1. adımda girin.

1. Sayfanın alt kısmında **Oluştur** ' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Önizleme özelliklerini kullanan bir IoT Central uygulaması oluşturdunuz. Önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](./tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
