---
title: Kimlik doğrulama belirteçlerini Azure Medya Hizmetleri'ne geçirin | Microsoft Dokümanlar
description: İstemciden Azure Medya Hizmetleri anahtar teslim hizmetine kimlik doğrulama belirteçlerini nasıl göndereceğinizi öğrenin
services: media-services
keywords: içerik koruması, DRM, belirteç kimlik doğrulaması
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 15d4cbc372f5d5ec0d323170189329152ed436e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73684948"
---
# <a name="learn-how-clients-pass-tokens-to-the-azure-media-services-key-delivery-service"></a>Müşterilerin azure medya hizmetleri anahtar teslim hizmetine nasıl belirteçler aktardığını öğrenin
Müşteriler genellikle bir oyuncunun anahtarı alabilmesi için bir oyuncunun doğrulama için Azure Medya Hizmetleri anahtar teslim hizmetine nasıl geçiş yapabildiğini sorar. Medya Hizmetleri basit web belirteci (SWT) ve JSON Web Belirteci (JWT) biçimlerini destekler. Sistemde ortak şifreleme veya Gelişmiş Şifreleme Standardı (AES) zarf şifrelemesi kullanıp kullanmadığınıza bakılmaksızın, belirteç kimlik doğrulaması her tür anahtara uygulanır.

 Hedeflediğiniz oyuncuya ve platforma bağlı olarak, belirtecinizi oyuncunuzla birlikte aşağıdaki yollarla geçirebilirsiniz:

- HTTP Yetkilendirme üstbilgisi aracılığıyla.
    > [!NOTE]
    > "Taşıyıcı" öneki OAuth 2.0 özellikleri başına bekleniyor. Belirteç yapılandırmasına sahip örnek bir oynatıcı, Azure Media Player [demo sayfasında](https://ampdemo.azureedge.net/)barındırılır. Video kaynağını ayarlamak için **AES (JWT Jetonu)** veya **AES (SWT Belirteci)** seçeneğini belirleyin. Belirteç Yetkilendirme üstbilgisi aracılığıyla geçirilir.

- "Token=tokenvalue" içeren bir URL sorgu parametresi eklenmesiyle.  
    > [!NOTE]
    > "Taşıyıcı" öneki beklenmiyor. Belirteç bir URL üzerinden gönderildiğinden, belirteç dizesini zırhla zırhla zırhla bağlamanız gerekir. Burada nasıl yapılacağını gösteren bir C# örnek kodu verem:

    ```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
    ```

- CustomData alanından.
Bu seçenek yalnızca PlayReady lisans edinme için, PlayReady Lisans Edinme Mücadelesi'nin CustomData alanı üzerinden kullanılır. Bu durumda, belirteç burada açıklandığı gibi XML belgeiçinde olmalıdır:

    ```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
    ```
    Kimlik doğrulama belirtinizi Token öğesine koyun.

- Alternatif bir HTTP Live Streaming (HLS) çalma listesi aracılığıyla. iOS/Safari'de AES + HLS oynatma için belirteç kimlik doğrulamasını yapılandırmanız gerekiyorsa, belirteci doğrudan göndermenin bir yolu yoktur. Bu senaryoyu etkinleştirmek için çalma listesinin nasıl alternatif olduğu hakkında daha fazla bilgi için bu [blog gönderisine](https://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]
