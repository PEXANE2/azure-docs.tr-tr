---
title: Media Encoder Standard birlikte bir kaplama oluşturma
description: Media Encoder Standard bir kaplama oluşturmayı öğrenin.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 07/27/2020
ms.openlocfilehash: 0d1a6d5626e081ff50f65b3a4396e223b33f920d
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87433635"
---
# <a name="how-to-create-an-overlay-with-media-encoder-standard"></a>Media Encoder Standard birlikte bir kaplama oluşturma

Media Encoder Standard, görüntünün var olan bir videonun üzerinde yer almasına olanak tanır. Şu anda şu biçimler desteklenir: PNG, jpg, GIF ve BMP.

## <a name="prerequisites"></a>Ön koşullar

* Örnekteki dosya *appsettings.js* yapılandırmak için ihtiyacınız olan hesap bilgilerini toplayın. Bunu nasıl yapadığınıza emin değilseniz, bkz. [hızlı başlangıç: Microsoft Identity platformu ile uygulama kaydetme](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app). Dosyasında *appsettings.js* aşağıdaki değerler beklenmektedir.

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
* Bkz. [dönüşümler başvuru belgesi](https://docs.microsoft.com/rest/api/media/transforms).

Dönüşümleri öğrendiğinizde, kaplamalar örneğini indirin.

## <a name="overlays-preset-sample"></a>Yer paylaşımları önceden ayarlanmış örnek

Yer paylaşımlarını kullanmaya başlamak için [Media-Services-kaplama örneğini](https://github.com/Azure-Samples/media-services-overlays) indirin.

## <a name="next-steps"></a>Sonraki adımlar

* [Media Services .NET ile kodlarken bir videoyu alt kırpın](subclip-video-dotnet-howto.md)