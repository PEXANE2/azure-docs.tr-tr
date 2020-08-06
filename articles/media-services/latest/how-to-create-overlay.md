---
title: Media Encoder Standard birlikte bir kaplama oluşturma
description: Media Encoder Standard bir kaplama oluşturmayı öğrenin.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 07/27/2020
ms.openlocfilehash: 85b1b6c56221deaa03057a7ccb658b4bf67124eb
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87830589"
---
# <a name="how-to-create-an-overlay-with-media-encoder-standard"></a>Media Encoder Standard birlikte bir kaplama oluşturma

Media Encoder Standard, görüntünün var olan bir videonun üzerinde yer almasına olanak tanır. Şu anda şu biçimler desteklenir: PNG, jpg, GIF ve BMP.

## <a name="prerequisites"></a>Önkoşullar

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