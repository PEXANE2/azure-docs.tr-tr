---
title: Media Encoder Standard birlikte bir kaplama oluşturma
description: Media Encoder Standard bir kaplama oluşturmayı öğrenin.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 08/31/2020
ms.openlocfilehash: 6c93408bce8da9f8cd0e4a0d0bab615e2bd362dc
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89267335"
---
# <a name="how-to-create-an-overlay-with-media-encoder-standard"></a>Media Encoder Standard birlikte bir kaplama oluşturma

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Media Encoder Standard, görüntünün var olan bir videonun üzerinde yer almasına olanak tanır. Şu anda şu biçimler desteklenir: PNG, jpg, GIF ve BMP.

## <a name="prerequisites"></a>Ön koşullar

* Örnekteki dosya *appsettings.js* yapılandırmak için ihtiyacınız olan hesap bilgilerini toplayın. Bunu nasıl yapadığınıza emin değilseniz, bkz. [hızlı başlangıç: Microsoft Identity platformu ile uygulama kaydetme](../../active-directory/develop/quickstart-register-app.md). Dosyasında *appsettings.js* aşağıdaki değerler beklenmektedir.

    ```json
    {
    "AadClientId": "",
    "AadEndpoint": "https://login.microsoftonline.com",
    "AadSecret": "",
    "AadTenantId": "",
    "AccountName": "",
    "ArmAadAudience": "https://management.core.windows.net/",
    "ArmEndpoint": "https://management.azure.com/",
    "Region": "",
    "ResourceGroup": "",
    "SubscriptionId": ""
    }
    ```

Dönüşümleri henüz bilmiyorsanız, aşağıdaki etkinlikleri gerçekleştirmeniz önerilir:

* [Media Services Ile kodlama videosu ve ses](encoding-concept.md) okuma
* [Özel bir Transform-.net Ile nasıl kodlanacağını](customize-encoder-presets-how-to.md)okuyun. Dönüşümlerle çalışmak için gereken .NET ayarlamak üzere bu makaledeki adımları izleyin ve ardından bir yer paylaşımlı önceden tanımlanmış örneği denemek için buraya geri dönün.
* Bkz. [dönüşümler başvuru belgesi](/rest/api/media/transforms).

Dönüşümleri öğrendiğinizde, kaplamalar örneğini indirin.

## <a name="overlays-preset-sample"></a>Yer paylaşımları önceden ayarlanmış örnek

Yer paylaşımlarını kullanmaya başlamak için [Media-Services-kaplama örneğini](https://github.com/Azure-Samples/media-services-overlays) indirin.

## <a name="next-steps"></a>Sonraki adımlar

* [Media Services .NET ile kodlarken bir videoyu alt kırpın](subclip-video-dotnet-howto.md)